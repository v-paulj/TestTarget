---
author: mcleblanc
ms.assetid: 88e16ec8-deff-4a60-bda6-97c5dabc30b8
description: "Dieses Thema enthält eine Fallstudie zum Portieren eines funktionierenden Peer-zu-Peer-Quizspiels (WinRT8.1-Beispiel-App) zu einer UWP (Universelle Windows-Plattform)-App für Windows 10."
title: "Windows-Runtime 8.x zu universeller Windows-Plattform (UWP) – Fallstudie, QuizGame-Beispiel-App (Peer-zu-Peer)"
ms.sourcegitcommit: 98b9bca2528c041d2fdfc6a0adead321737932b4
ms.openlocfilehash: cd05c3edbc254cceb00c55caba698d21998f5594

---

# Windows-Runtime 8.x zu universeller Windows-Plattform (UWP) – Fallstudie: QuizGame-Beispiel-App (Peer-zu-Peer)


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Dieses Thema enthält eine Fallstudie zum Portieren eines funktionierenden Peer-zu-Peer-Quizspiels (WinRT8.1-Beispiel-App) zu einer UWP (Universelle Windows-Plattform)-App für Windows 10.

Bei einer universellen 8.1-App werden zwei Versionen derselben App erstellt: ein App-Paket für Windows 8.1 und ein anderes App-Paket für Windows Phone 8.1. Für die WinRT8.1-Version von QuizGame wird eine Anordnung mit einem Projekt für eine universelle Windows-App verwendet. Dabei wird aber ein anderer Ansatz gewählt und für die beiden Plattformen jeweils eine App mit unterschiedlicher Funktionalität erstellt. Das Windows 8.1-App-Paket dient als Host für eine Sitzung des Quizspiels, während das Paket mit der Windows Phone 8.1-App die Rolle des Clients für den Host übernimmt. Die beiden Hälften der Quizspielsitzung kommunizieren per Peer-zu-Peer-Netzwerkverbindung.

Die individuelle Anpassung der beiden Hälften für PC bzw. Telefone ist sinnvoll. Aber wäre es nicht sogar noch besser, wenn Sie sowohl den Host als auch den Client auf nahezu jedem beliebigen Gerät ausführen könnten? In dieser Fallstudie werden beide Apps zu Windows 10 portiert. Es wird jeweils ein einzelnes App-Paket erstellt, das Benutzer auf vielen verschiedenen Geräten installieren können.

Für die App werden Muster verwendet, bei denen Ansichten und Ansichtsmodelle genutzt werden. Sie werden sehen, dass der Portiervorgang für diese App aufgrund dieser klaren Trennung sehr einfach ist.


            **Hinweis:**  Bei diesem Beispiel wird vorausgesetzt, dass Ihr Netzwerk für das Senden und Empfangen von benutzerdefinierten UDP-Gruppenmulticastpaketen konfiguriert ist (dies ist bei den meisten Heimnetzwerken der Fall, gilt aber nicht für alle Netzwerke). Im Beispiel werden auch TCP-Pakete gesendet und empfangen.

 


            **Hinweis:**   Wenn beim Öffnen von QuizGame10 in Visual Studio die Meldung „Visual Studio-Update erforderlich“ angezeigt wird, führen Sie die Schritte unter [TargetPlatformVersion](w8x-to-uwp-troubleshooting.md#targetplatformversion) aus.

 

## Downloads


            [Laden Sie die universelle 8.1-App „QuizGame“ herunter](http://go.microsoft.com/fwlink/?linkid=532953). Dies ist der ursprünglichen Zustand der App vor dem Portieren. 


            [Laden Sie die Windows 10-App „QuizGame10“ herunter](http://go.microsoft.com/fwlink/?linkid=532954). Dies ist der Zustand der App direkt nach dem Portieren. 


            [Die aktuelle Version dieses Beispiels finden Sie auf GitHub](https://github.com/Microsoft/Windows-appsample-quizgame).

## WinRT8.1-Projektmappe


So sieht QuizGame aus. Dies ist die App, die wir portieren werden.

![QuizGame-Host-App – Ausführung unter Windows](images/w8x-to-uwp-case-studies/c04-01-win81-how-the-host-app-looks.png)

QuizGame-Host-App – Ausführung unter Windows

 

![QuizGame-Client-App – Ausführung unter Windows Phone](images/w8x-to-uwp-case-studies/c04-02-wp81-how-the-client-app-looks.png)

QuizGame-Client-App – Ausführung unter Windows Phone

## Exemplarische Vorgehensweise für die Nutzung von QuizGame

Dies ist eine kurze hypothetische Beschreibung der App-Nutzung. Sie enthält aber nützliche Informationen, falls Sie die App selbst per WLAN ausprobieren möchten.

In einer Bar wird ein lustiges Quizspiel veranstaltet. Es gibt in der Bar einen großen Fernseher, der für alle Gäste sichtbar ist. Der Quizmaster nutzt einen PC, dessen Ausgabe auf dem Fernseher zu sehen ist. Auf diesem PC wird die „Host-App“ ausgeführt. Alle Personen, die am Quiz teilnehmen möchten, müssen lediglich die „Client-App“ auf Ihrem Telefon oder Surface-Gerät installieren.

Die Host-App befindet sich im Lobbymodus, und auf dem großen Fernseher wird angezeigt, dass sie bereit für die Verbindung mit Client-App ist. Joan startet die Client-App auf ihrem Mobilgerät. Sie gibt ihren Namen in das Textfeld **Spielername** ein und tippt dann auf **Spiel beitreten**. Die Host-App bestätigt, dass Joan beigetreten ist. Ihr Name wird angezeigt, und in der Client-App von Joan wird angegeben, dass auf den Beginn des Spiels gewartet wird. Als Nächstes führt Maxwell auf seinem mobilen Gerät die gleichen Schritte aus.

Der Quizmaster klickt auf **Spiel starten**, und in der Host-App werden eine Frage und die möglichen Antworten angezeigt (sowie eine Liste mit den verbundenen Spielern in normaler Schriftbreite und grauer Farbe). Gleichzeitig werden die Antworten auf den Clientgeräten als Schaltflächen angezeigt. Joan tippt auf die Schaltfläche mit der Antwort „1975“. Daraufhin werden alle Schaltflächen deaktiviert. In der Host-App wird Joans Name grün (und fett) angezeigt, um zu bestätigen, dass ihre Antwort eingegangen ist. Maxwell antwortet ebenfalls. Der Quizmaster sieht, dass die Namen aller Spieler grün sind, und klickt auf **Nächste Frage**.

Nach diesem Muster werden weitere Fragen gestellt und beantwortet. Nachdem die letzte Frage in der Host-App angezeigt wurde, lautet der Text der Schaltfläche nicht mehr **Nächste Frage**, sondern **Ergebnisse anzeigen**. Nach dem Klicken auf **Ergebnisse anzeigen** werden die Ergebnisse angezeigt. Wenn auf **Zurück zur Lobby** geklickt wird, beginnt der Spielzyklus von vorne. Spieler, die bereits beigetreten sind, sind jetzt aber schon angegeben. Nach dem Zurücksetzen auf den Lobbymodus können neue Spieler beitreten. Dies ist ein guter Zeitpunkt für Spieler, das Spiel zu verlassen (obwohl dies jederzeit durch Tippen auf **Spiel verlassen** möglich ist).

## Lokaler Testmodus

Wenn Sie die App und ihre Interaktionen auf einem einzelnen PC ausprobieren möchten, anstatt verteilt über mehrere Geräte, können Sie die Host-App im lokalen Testmodus erstellen. In diesem Modus entfällt die Nutzung der Netzwerkverbindung vollständig. Stattdessen zeigt die Benutzeroberfläche der Host-App den Hostteil links im Fenster an, und auf der rechten Seite sind zwei Kopien der Client-App-UI vertikal übereinander zu sehen (beachten Sie, dass in dieser Version die UI des lokalen Testmodus für eine PC-Anzeige festgelegt ist, sie passt sich nicht an kleine Geräte an). Diese UI-Segmente, die alle Teil derselben App sind, kommunizieren untereinander über einen so genannten Client Communicator. Damit werden die Interaktionen simuliert, die sonst über das Netzwerk erfolgen würden.

Legen Sie zum Aktivieren des lokalen Testmodus den Eintrag **LOCALTESTMODEON** (in den Projekteigenschaften) als Symbol für die bedingte Kompilierung fest, und führen Sie eine Neuerstellung durch.

## Portieren auf ein Windows 10-Projekt

QuizGame verfügt über die folgenden Bestandteile:

-   P2PHelper Dies ist eine portable Klassenbibliothek, in der die Peer-zu-Peer-Netzwerklogik enthalten ist.
-   QuizGame.Windows Dies ist das Projekt, mit dem das App-Paket für die auf Windows 8.1 ausgerichtete Host-App erstellt wird.
-   QuizGame.WindowsPhone Dies ist das Projekt, mit dem das App-Paket für die Client-App für Windows Phone8.1 erstellt wird.
-   QuizGame.Shared Dies ist das Projekt, das den Quellcode, die Markupdateien und andere Assets und Ressourcen enthält, die von den beiden anderen Projekten verwendet werden.

Für diese Fallstudie verwenden wir die üblichen Optionen, die unter [Bei einer Universal8.1-App](w8x-to-uwp-root.md#if-you-have-an-81-universal-windows-app) in Bezug auf die zu unterstützenden Geräte beschrieben sind.

Basierend auf diesen Optionen portieren wir „QuizGame.Windows” zu einem neuen Windows 10-Projekt mit dem Namen „QuizGameHost”. Und wir portieren „QuizGame.WindowsPhone” zu einem neuen Windows 10-Projekt mit dem Namen „QuizGameClient”. Diese Projekte sind für die universelle Gerätefamilie bestimmt, damit sie auf jedem Gerät ausgeführt werden können. Wir belassen die QuizGame.Shared-Quelldateien usw. in ihrem eigenen Ordner und verknüpfen diese freigegebenen Dateien mit den beiden neuen Projekten. Wir verwenden wieder eine einzelne Projektmappe und nennen sie QuizGame10.

**QuizGame10-Projektmappe**

-   Erstellen Sie eine neue Projektmappe (**Neues Projekt**&gt;**Andere Projekttypen**&gt;**Visual Studio-Projektmappen**) und geben Sie ihr den Namen QuizGame10.

**P2PHelper**

-   Erstellen Sie in der Projektmappe über (**Neues Projekt**&gt;**Windows Universal**&gt;**Class Library (Windows Universal)**) ein neues Windows10-Klassenbibliotheksprojekt und geben Sie für das Projekt den Namen „P2PHelper” ein.
-   Löschen Sie „Class1.cs“ aus dem neuen Projekt.
-   Kopieren Sie „P2PSession.cs“, „P2PSessionClient.cs“ und „P2PSessionHost.cs“ in den Ordner des neuen Projekts, und fügen Sie die kopierten Dateien in das neue Projekt ein.
-   Das Projekt kann dann ohne weitere Änderungen erstellt werden.

**Freigegebene Dateien**

-   Kopieren Sie die Ordner „Common“, „Model“, „View“ und „ViewModel“ aus „\\QuizGame.Shared\\“ in „\\QuizGame10\\“.
-   „Common“, „Model“, „View“ und „ViewModel“ sind die Ordner, die gemeint sind, wenn es um die freigegebenen Ordner auf dem Datenträger geht.

**QuizGameHost**

-   Erstellen Sie über (**Hinzufügen**&gt;**Neues Projekt**&gt;**Windows Universal**&gt;**Leere Anwendung (Windows Universal)**) ein neues Windows10-App-Projekt und geben Sie für das Projekt den Namen „QuizGameHost” ein.
-   Fügen Sie einen Verweis auf P2PHelper hinzu (**Verweis hinzufügen**&gt;**Projekte**&gt;**Projektmappe**&gt;**P2PHelper**).
-   Erstellen Sie im **Projektmappen-Explorer** für jeden freigegebenen Ordner auf dem Datenträger einen neuen Ordner. Klicken Sie nacheinander mit der rechten Maustaste auf die einzelnen Ordner, die Sie gerade erstellt haben, und klicken Sie dann auf **Hinzufügen**&gt;**Vorhandenes Element**. Wechseln Sie anschließend eine Ordnerebene nach oben. Öffnen Sie den entsprechenden freigegebenen Ordner, wählen Sie alle Dateien aus, und klicken Sie anschließend auf **Link hinzufügen**.
-   Kopieren Sie „MainPage.xaml“ aus „\\QuizGame.Windows\\“ in „\\QuizGameHost\\“, und ändern Sie den Namespace in „QuizGameHost“.
-   Kopieren Sie „App.xaml“ aus „\\QuizGame.Shared\\“ in „\\QuizGameHost\\“, und ändern Sie den Namespace in „QuizGameHost“.
-   Anstatt „app.xaml.cs“ zu überschreiben, behalten wir die Version im neuen Projekt bei und nehmen nur eine gezielte Änderung vor, um den lokalen Testmodus zu unterstützen. Ersetzen Sie in „app.xaml.cs“ die folgende Codezeile:

```CSharp
rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

durch diese Codezeile:

```CSharp
#if LOCALTESTMODEON
    rootFrame.Navigate(typeof(TestView), e.Arguments);
#else
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
#endif
```

-   Fügen Sie unter **Eigenschaften**&gt;**Erstellen**&gt;**Bedingte Kompilierungssymbole** den Text „LOCALTESTMODEON“ hinzu.
-   Jetzt können Sie wieder auf den Code zurückgreifen, den Sie der Datei „app.xaml.cs“ hinzugefügt haben, und den TestView-Typ auflösen.
-   Ändern Sie in „package.appxmanifest“ den capability-Namen von „internetClient“ in „internetClientServer“.

**QuizGameClient**

-   Erstellen Sie über (**Hinzufügen**&gt;**Neues Projekt**&gt;**Windows Universal**&gt;**Leere Anwendung (Windows Universal)**) ein neues Windows10-App-Projekt und geben Sie für das Projekt den Namen „QuizGameClient” ein.
-   Fügen Sie einen Verweis auf P2PHelper hinzu (**Verweis hinzufügen**&gt;**Projekte**&gt;**Projektmappe**&gt;**P2PHelper**).
-   Erstellen Sie im **Projektmappen-Explorer** für jeden freigegebenen Ordner auf dem Datenträger einen neuen Ordner. Klicken Sie nacheinander mit der rechten Maustaste auf die einzelnen Ordner, die Sie gerade erstellt haben, und klicken Sie dann auf **Hinzufügen**&gt;**Vorhandenes Element**. Wechseln Sie anschließend eine Ordnerebene nach oben. Öffnen Sie den entsprechenden freigegebenen Ordner, wählen Sie alle Dateien aus, und klicken Sie anschließend auf **Link hinzufügen**.
-   Kopieren Sie „MainPage.xaml“ aus „\\QuizGame.WindowsPhone\\“ in „\\QuizGameClient\\“, und ändern Sie den Namespace in „QuizGameClient“.
-   Kopieren Sie „App.xaml“ aus „\\QuizGame.Shared\\“ in „\\QuizGameClient\\“, und ändern Sie den Namespace in „QuizGameClient“.
-   Ändern Sie in „package.appxmanifest“ den capability-Namen von „internetClient“ in „internetClientServer“.

Nun können Sie die Erstellung durchführen und die App ausführen.

## Adaptive UI

Die Windows 10-App „QuizGameHost” sieht gut aus, wenn sie in einem breiten Fenster ausgeführt wird (nur auf einem Gerät mit großem Bildschirm möglich). Aber wenn das Fenster der App schmal ist (auf kleinen und großen Geräten möglich), wird die UI so gestaucht, dass die Lesbarkeit nicht mehr gewährleistet ist.

Wir können das adaptive Visual State-Manager-Feature nutzen, um dies zu beheben. Dies ist unter [Fallstudie: Bookstore2](w8x-to-uwp-case-study-bookstore2.md) beschrieben. Zuerst legen wir die Eigenschaften für visuelle Elemente so fest, dass für die UI standardmäßig das schmale Layout gewählt wird. Alle diese Änderungen werden in „\\View\\HostView.xaml“ vorgenommen.

-   Ändern Sie im **Grid**-Hauptelement den **Height**-Wert des **RowDefinition**-Elements von „140“ in „Auto“.
-   Legen Sie im **Grid**-Element, in dem das **TextBlock**-Element mit dem Namen `pageTitle` enthalten ist, `x:Name="pageTitleGrid"` und `Height="60"` fest. Diese ersten beiden Schritte dienen dazu, dass wir die Höhe des **RowDefinition**-Elements über einen Setter in einem visuellen Zustand effektiv steuern können.
-   Geben Sie unter `pageTitle` den Code `Margin="-30,0,0,0"` an.
-   Legen Sie für das **Grid**-Element, das mit dem Kommentar `<!-- Content -->` angegeben wird, `x:Name="contentGrid"` und `Margin="-18,12,0,0"` fest.
-   Geben Sie für das **TextBlock**-Element direkt oberhalb des Kommentars `<!-- Options -->` den Code `Margin="0,0,0,24"` an.
-   Ändern Sie im standardmäßigen **TextBlock**-Stil (erste Ressource in der Datei) den Wert des **FontSize**-Setters in „15“.
-   Ändern Sie unter `OptionContentControlStyle` den Wert des **FontSize**-Setters in „20“. Durch diesen und den vorherigen Schritt erhalten wir eine gute Typhierarchie, die auf allen Geräten einwandfrei funktioniert. Dies sind deutlich flexiblere Größen als beim Wert „30“, den wir für die Windows 8.1-App verwendet haben.
-   Fügen Sie dem **Grid**-Stammelement schließlich den geeigneten Visual State-Manager-Markupcode hinzu.

```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState x:Name="WideState">
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="548"/>
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="pageTitleGrid.Height" Value="140"/>
                <Setter Target="pageTitle.Margin" Value="0,0,30,40"/>
                <Setter Target="contentGrid.Margin" Value="40,40,0,0"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

## Universelle Formatierung


Sie werden feststellen, dass die Schaltflächen in Windows 10 in der Vorlage nicht über die gleichen Abstände für das Toucheingabeziel verfügen. Dies lässt sich mit zwei kleinen Änderungen beheben. Fügen Sie diesen Markupcode zuerst in QuizGameHost und QuizGameClient jeweils der Datei „app.xaml“ hinzu.

```xml
<Style TargetType="Button">
    <Setter Property="Margin" Value="12"/>
</Style>
```

Fügen Sie diesen Setter anschließend dem `OptionButtonStyle`-Element in „\\View\\ClientView.xaml“ hinzu.

```xml
<Setter Property="Margin" Value="6"/>
```

Dank dieser letzten Optimierung verhält sich die App genauso wie vor dem Portieren und sieht auch genauso aus. Der entscheidende Vorteil ist aber, dass sie jetzt auf allen Geräten ausgeführt werden kann.

## Fazit

Die App, die wir im Rahmen dieser Fallstudie portiert haben, war eine relativ komplexe App mit mehrere Projekten, einer Klassenbibliothek und einer größeren Menge an Code und UI-Elementen. Trotzdem war der Portiervorgang einfach. Ein Grund für das einfache Portieren ist die große Ähnlichkeit zwischen der Windows10-Entwicklerplattform und den Windows8.1- und Windows Phone8.1-Plattformen. Ein anderer Grund ist der Entwurf der ursprünglichen App, bei dem darauf geachtet wurde, dass die Modelle, Ansichtsmodelle und Ansichten separat gehalten wurden.



<!--HONumber=Jun16_HO4-->


