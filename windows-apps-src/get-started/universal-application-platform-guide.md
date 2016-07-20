---
author: TylerMSFT
title: "Anleitung für Apps für die Universelle Windows-Plattform (UWP)"
description: "In dieser Anleitung erfahren Sie mehr über Apps für die Universelle Windows-Plattform (UWP), die auf einer Vielzahl von Geräten ausgeführt werden können."
ms.assetid: 59849197-B5C7-493C-8581-ADD6F5F8800B
translationtype: Human Translation
ms.sourcegitcommit: 4ad8dc5883b7edafa2c2579d3733eafba0b9cc1f
ms.openlocfilehash: 8f4e906c9f1c685a5f6aeebd5fe0ebcc96ff9a7c

---

# Anleitung für Apps für die Universelle Windows-Plattform (UWP)


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

In dieser Anleitung wird Folgendes behandelt:

-   Informationen zu *Gerätefamilien* und dazu, wie Sie sich für eine Gerätefamilie entscheiden
-   Neue UI-Steuerelemente und Bereiche, mit denen Sie Ihre Benutzeroberfläche für verschiedenene Geräte-Formfaktoren anpassen können.
-   Informationen zum Verständnis und zur Steuerung der API-Oberfläche, die für Ihre App verfügbar ist.

Mit Windows8 wurde die Windows-Runtime (WinRT) eingeführt – eine Weiterentwicklung des Windows-App-Modells. Sie war als allgemeine Anwendungsarchitektur vorgesehen.

Als Windows Phone 8.1 verfügbar wurde, wurde die Windows-Runtime zwischen Windows Phone 8.1 und Windows ausgerichtet. Dadurch konnten Entwickler *universelle Windows8-Apps* erstellen, die dank einer gemeinsamen Codebasis sowohl für Windows als auch Windows Phone geeignet sind.

Die mit Windows10 eingeführte universelle Windows-Plattform (UWP) führt die Weiterentwicklung des Windows-Runtime-Modells fort und integriert es in den einheitlichen Windows10-Kern. Als Teil des Kerns bietet die UWP nun eine gemeinsame App-Plattform, die auf jedem Gerät mit Windows10 verfügbar ist. Mit dieser Entwicklung können für die UWP bestimmte Apps nicht nur die WinRT-APIs aufrufen, die auf allen Geräten verfügbar sind, sondern auch APIs (einschließlich Win32- und .NET-APIs), die für die jeweilige Gerätefamilie spezifisch sind, auf der die App ausgeführt wird. Die UWP bietet eine garantierte Kern-API-Ebene auf allen Geräten. Dies bedeutet, dass Sie ein einzelnes App-Paket erstellen können, das auf einer Vielzahl von Geräten installiert werden kann. Und mit diesem einzelnen App-Paket bietet der Windows Store einen einheitlichen Vertriebskanal, um alle Gerätetypen zu erreichen, auf denen Ihre App ausgeführt werden kann.

![Apps für die Universelle Windows-Plattform können auf einer Vielzahl von Geräten ausgeführt werden, unterstützen adaptive Benutzeroberflächen, natürliche Benutzereingaben, einen Store, ein Dev Center und Clouddienste.](images/universalapps-overview.png)

Da Ihre UWP-App auf einer Vielzahl von Geräten mit verschiedenen Formfaktoren und Eingabemodalitäten ausgeführt werden kann, soll sie für alle Geräte geeignet sein und in der Lage sein, die einzigartigen Funktionen auf jedem Gerät zu entsperren. Geräte fügen ihren eigenen eindeutigen APIs zur garantierten API-Ebene hinzu. Sie können Code schreiben, um auf diese eindeutigen APIs bedingt zuzugreifen, damit Ihre App Features hervorhebt, die speziell für einen Gerätetyp bestimmt sind, und auf anderen Geräten anders dargestellt werden. Adaptive UI-Steuerelemente und neue Layoutbereiche unterstützen Sie bei der Gestaltung Ihrer UI für eine breite Palette von Bildschirmauflösungen.

## Gerätefamilien

Windows8.1- und WindowsPhone8.1-Apps sind auf ein bestimmtes Betriebssystem (Windows oder Windows Phone) ausgerichtet. Mit Windows10 entwickeln Sie eine App nicht mehr für ein Betriebssystem. Stattdessen ist Ihre App für einzelne oder mehrere Gerätefamilien bestimmt. Eine Gerätefamilie identifiziert die APIs, Systemmerkmale und Verhaltensweisen, die Sie auf allen Geräten innerhalb der Gerätefamilie erwarten können. Sie bestimmt außerdem die Gerätegruppe, auf der Ihre App aus dem Store installiert werden kann. Hier finden Sie die Gerätefamilienhierarchie.

![Gerätefamilien](images/devicefamilytree.png)

Eine Gerätefamilie ist eine mit Name und Versionsnummer versehene API-Sammlung. Eine Gerätefamilie ist die Grundlage eines Betriebssystems. PCs führen das Desktop-Betriebssystem aus, es basiert auf der Desktopgerätefamilie. Smartphones, Tablets usw. führen das mobile Betriebssystem aus, das auf der Mobilgerätefamilie basiert. Und so weiter.

Die universelle Gerätefamilie ist ein Sonderfall. Es ist nicht direkt die Grundlage für ein Betriebssystem. Stattdessen wird der Satz von APIs in der universellen Gerätefamilie von den untergeordneten Gerätefamilien geerbt. Die APIs der universellen Gerätefamilie sind daher garantiert in jedem Betriebssystem und auf jedem Gerät vorhanden.

Jede untergeordnete Gerätefamilie fügt den geerbten APIs eigene APIs hinzu. Die resultierende Gesamtmenge der APIs in einer untergeordneten Gerätefamilie ist garantiert im Betriebssystem basierend auf der Gerätefamilie und auf jedem Gerät vorhanden, auf dem dieses Betriebssystem ausgeführt wird.

Ein Vorteil von Gerätefamilien besteht darin, dass Ihre App auf beliebigen bzw. sogar allen Geräten aus einer Vielzahl von Geräten wie Smartphones, Tablets, Desktopcomputern, Surface Hubs, Xbox-Konsolen und HoloLens ausgeführt werden kann. Ihre App kann auch adaptiven Code nutzen, um Features eines Geräts dynamisch zu erkennen und zu verwenden, das außerhalb der universellen Gerätefamilie liegt.

Die Entscheidung darüber, für welche Gerätefamilie (oder Familien) Ihrer App bestimmt ist, liegt bei Ihnen. Und diese Entscheidung wirkt sich folgendermaßen auf Ihre App aus. Sie bestimmt:

-   Den Satz von APIs, bei dem Ihre App davon ausgehen kann, dass sie bei ihrer Ausführung vorhanden sind (und deshalb frei aufgerufen werden können).
-   Den Satz von API-Aufrufen, die nur innerhalb der bedingten Anweisungen sicher sind.
-   Den Satz von Geräten, auf denen Ihre App aus dem Store installiert werden kann (und daher die Formfaktoren, die Sie berücksichtigen müssen).

Es gibt zwei zentrale Konsequenzen einer Entscheidung für eine Gerätefamilie: die API-Oberfläche, die jederzeit von der App aufgerufen werden kann und die Anzahl der Geräte, die von der App erreicht werden können. Diese beiden Faktoren umfassen Nachteile und sind umgekehrt miteinander verknüpft. Eine UWP-App ist beispielsweise eine App, die speziell auf die universelle Gerätefamilie ausgerichtet ist und daher auf allen Geräten verfügbar ist. Eine App für die universelle Gerätefamilie kann das Vorhandensein von nur den APIs in der universellen Gerätefamilie vorausgesetzt werden (da sie darauf ausgerichtet ist). Andere APIs müssen bedingt aufgerufen werden. Außerdem muss eine entsprechende App eine hochgradig adaptive UI und umfassende Funktionen zur Eingabe enthalten, da sie auf einer Vielzahl von Geräten ausgeführt werden kann. Eine mobile Windows-App ist eine App, die speziell auf die Mobilgerätefamilie ausgerichtet ist, und für Geräte verfügbar ist, deren Betriebssystem auf der Mobilgerätefamilie basiert (einschließlich Smartphones, Tablets und ähnlichen Geräten). Eine App für die Mobilgerätefamilie kann von dem Vorhandensein aller APIS in der Mobilgerätefamilie ausgehen und die UI muss angemessen adaptiv sein. Eine App, die auf die IoT-Gerätefamilie ausgerichtet ist, kann nur auf IoT-Geräten installiert werden und es kann vom Vorhandensein aller APIs in der IoT-Gerätefamilie ausgegangen werden. Diese App kann hinsichtlich der Benutzeroberfläche und den Eingabefunktionen sehr speziell sein, da Sie wissen, dass Sie nur auf einem bestimmten Gerätetyp ausgeführt werden kann.

Hier sind einige Überlegungen, die Sie bei der Entscheidung hinsichtlich der Ausrichtung der Gerätefamilie unterstützen:

**Maximieren der Reichweite Ihrer App**

Um die maximale Reichweite von Geräten mit Ihrer App zu erzielen, und damit sie auf so vielen Geräten wie möglich ausgeführt werden kann, wird Ihre App auf die universelle Gerätefamilie ausgerichtet. Auf diese Weise ist die App automatisch auf jede Gerätefamilie ausgerichtet, die auf universell basiert (im Diagramm alle untergeordneten Elemente der universellen). Das bedeutet, dass die App auf Grundlage der Gerätefamilien auf jedem Betriebssystem ausgeführt werden kann, und auf allen Geräten, auf denen diese Betriebssysteme ausgeführt werden. Die einzigen APIs, die garantiert auf all diesen Geräten verfügbar sind, ist die von der bestimmten Version der universellen Gerätefamilie definierte Gruppe, auf die Ihre App ausgerichtet ist. (In dieser Version ist das immer die Version10.0.x.0.) Weitere Informationen dazu, wie eine App APIs außerhalb der Gerätefamilien-Zielversion aufrufen kann, finden Sie weiter unten in diesem Thema unter „Schreiben von Code“.

**Begrenzen der App auf einen Gerätetyp**

Möglicherweise möchten Sie nicht, dass Ihre App auf einer Vielzahl von Geräten ausgeführt werden kann; möglicherweise ist sie speziell auf einen Desktop-PC oder eine Xbox-Konsole ausgerichtet. In diesem Fall können Sie festlegen, dass Ihre App auf eine der untergeordneten Gerätefamilien ausgerichtet ist. Wenn Sie die App beispielsweise auf die Desktop-Gerätefamilie ausrichten, gehören zu den garantiert verfügbaren APIs für Ihre App die APIs, die von der universellen Gerätefamilie vererbt werden und zusätzlich die APIs, die speziell auf die Desktopgerätefamilie ausgerichtet sind.

**Begrenzen der App auf eine Teilmenge aller möglichen Geräte**

Statt die App auf die universelle Gerätefamilie oder auf eine der untergeordneten Gerätefamilie auszurichten, können Sie sie auf zwei (oder mehr) untergeordnete Gerätefamilien ausrichten. Die Ausrichtung auf Desktopcomputer und Mobilgeräte ist für Ihre App möglicherweise sinnvoll. Oder auf Desktopcomputer und HoloLens. Oder Desktopcomputer, Xbox-Konsolen und Surface Hub usw.

**Ausschließen der Unterstützung für eine bestimmte Version einer Gerätefamilie**

In seltenen Fällen möchten Sie vielleicht, dass Ihre App auf allen Geräten außer auf Geräten mit einer bestimmten Version einer bestimmten Gerätefamilie ausgeführt werden kann. Nehmen wir beispielsweise an, Ihre App ist auf Version 10.0.x.0 der universellen Gerätefamilie ausgerichtet. Wenn sich die Betriebssystemversion in Zukunft ändert, z.B. zu 10.0.x.2, können Sie zu diesem Zeitpunkt angeben, dass Ihre App auf allen Versionen außer 10.0.x.1 der Xbox-Konsole ausgeführt werden soll, indem Sie Ihre App in Bezug auf Universal auf Version 10.0.x.0 und in Bezug auf Xbox auf 10.0.x.1 ausrichten. Ihre App ist dann für die Gruppe von Gerätefamilienversionen innerhalb von Xbox10.0.x.1 (einschließlich) und älteren Versionen nicht mehr verfügbar.

Microsoft Visual Studio gibt in der Manifestdatei des App-Pakets standardmäßig **Windows.Universal** als Zielgerätefamilie an. Um im Store die Gerätefamilie(n) anzugeben, für die Ihre App angeboten wird, konfigurieren Sie das [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903)-Element in der Datei „Package.appxmanifest“ manuell.

## UI und universelle Eingabe

Eine UWP-App kann auf vielen verschiedenen Gerätetypen ausgeführt werden, die verschiedene Arten von Eingaben, Auflösungen, DPI-Dichte und andere eindeutige Merkmale aufweisen. Windows10 bietet neue universelle Steuerelemente, Layoutbereiche und Tools, mit denen Sie die Benutzeroberfläche für die Geräte anpassen können, auf denen Ihre App wahrscheinlich ausgeführt wird. Sie können beispielsweise die UI so anpassen, dass sie den Vorteil durch den Unterschied bei der Bildschirmauflösung nutzt, wenn Ihre App im Vergleich zu einem mobilen Gerät auf einem Desktopcomputer ausgeführt wird.

Einige Aspekte der App-UI Ihrer App werden automatisch auf allen Geräten angepasst. Steuerelemente wie Schaltflächen und Schieberegler passen sich automatisch bei allen Gerätefamilien und Eingabemethoden an. Das Design der Benutzererfahrung Ihrer App muss jedoch möglicherweise angepasst werden, je nachdem, auf welchem Gerät die App ausgeführt wird. Eine Foto-App sollte die UI beispielsweise anpassen, wenn sie auf einem kleinen, tragbaren Gerät ausgeführt wird, um sicherzustellen, dass sie optimal mit einer Hand bedient werden kann. Wenn die Foto-App auf einem Desktopcomputer ausgeführt wird, sollte sich die Benutzeroberfläche anpassen, um die zusätzliche Bildschirmfläche zu nutzen.

Windows unterstützt Sie mit folgenden Features, Ihre UI auf mehrere Geräte auszurichten:

-   Universelle Steuerelemente und Layoutbereiche können Sie bei der Optimierung Ihrer UI für die Bildschirmauflösung des Geräts unterstützen
-   Mit der allgemeinen Eingabebehandlung können Sie Benutzereingaben per Toucheingabe, Stift, Maus, Tastatur oder Controller (Microsoft Xbox-Controller oder Ähnliches) erhalten.
-   Tools können Sie bei dem Entwerfen der UI unterstützen, sodass sie sich an verschiedene Bildschirmauflösungen anpasst
-   Die adaptive Skalierung passt die Auflösung und DPI-Unterschiede auf allen Geräten an

### Universelle Steuerelemente und Layoutbereiche

Windows10 umfasst neue Steuerelemente – beispielsweise den Kalender und die geteilte Ansicht. Das Pivot-Steuerelement, das zuvor nur für Windows Phone verfügbar war, ist jetzt auch für die universelle Gerätefamilie verfügbar.

Steuerelemente wurden aktualisiert, damit sie auf größeren Bildschirmen gut funktionieren, sich auf Grundlage der verfügbaren Bildschirmpixel auf dem Gerät selbst anpassen und gut mit mehreren Arten von Eingaben, wie z. B. Tastatur, Maus, Toucheingabe, Stift und Controller, z. B. Xbox-Controller gut funktionieren.

Möglicherweise stellen Sie fest, dass Sie Ihr gesamtes UI-Layout auf Grundlage der Bildschirmauflösung des Geräts anpassen müssen, auf dem Ihre App ausgeführt wird. Eine auf einem Desktopcomputer ausgeführte Kommunikations-App kann beispielsweise eine Bild-im-Bild-Ansicht des Anrufers sowie Steuerelemente enthalten, die ideal für die Mauseingabe geeignet sind:

![Desktopbenutzeroberfläche einer Kommunikations-App](images/adaptiveux-desktop.png)

Wenn die App jedoch auf einem Smartphone ausgeführt wird, wird die Bild-im-Bild-Ansicht möglicherweise entfernt, da weniger Bildschirmfläche verfügbar ist. Dafür wird die Anrufschaltfläche vergrößert, um die Bedienung mit einer Hand zu erleichtern:

![Smartphone-Benutzeroberfläche einer Kommunikations-App](images/adaptiveux-phone.png)

Damit Sie das Layout der gesamten Benutzeroberfläche abhängig vom verfügbaren Platz auf dem Bildschirm leichter anpassen können, führt Windows10 adaptive Bereiche und Entwurfszustände ein.

### Entwerfen einer adaptiver UI mit adaptiven Bereichen

Layoutbereiche steuern Größe und Position untergeordneter Elemente abhängig vom verfügbaren Platz. [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635) ordnet seine untergeordneten Elemente beispielsweise nacheinander an (horizontal oder vertikal). 
              [
              **Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) ist mit einem CSS-Raster vergleichbar, das untergeordnete Elemente in Zellen platziert.

Das neue [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/dn879546)-Element implementiert einen Layoutstil, der durch die Beziehungen zwischen den untergeordneten Elementen definiert ist. Er dient zum Erstellen von App-Layouts, die sich andere Bildschirmauflösungen anpassen können. Das **RelativePanel**-Element vereinfacht den Prozess der Neuanordnung von Elementen, indem es die Beziehungen zwischen Elementen definiert, sodass Sie eine dynamischere Benutzeroberfläche ohne geschachtelte Layouts entwickeln können.

Im folgenden Beispiel wird **blueButton** unabhängig von Änderungen bei Ausrichtung oder Layout rechts von **textBox1** angezeigt. **orangeButton** erscheint direkt darunter und wird an **blueButton** ausgerichtet. Dies ist selbst dann der Fall, wenn sich bei der Eingabe von Text die Breite von **textBox1** ändert. Zuvor waren dafür Zeilen und Spalten in einem **Grid**-Element erforderlich. Nun kann dies mit deutlich geringerem Aufwand erreicht werden.

![Beispiel für „RelativePanel“](images/relativepane-standalone.png)

```XML
<RelativePanel>
    <TextBox x:Name="textBox1" Text="textbox" Margin="5"/>
    <Button x:Name="blueButton" Margin="5" Background="LightBlue" Content="ButtonRight" RelativePanel.RightOf="textBox1"/>
    <Button x:Name="orangeButton" Margin="5" Background="Orange" Content="ButtonBelow" RelativePanel.RightOf="textBox1" RelativePanel.Below="blueButton"/>
</RelativePanel>
```

### Verwenden visueller Zustandsauslöser für eine UI, die sich an die verfügbare Bildschirmfläche anpasst

Die Benutzeroberfläche muss ggf. an Änderungen der Fenstergröße angepasst werden. Mit adaptiven visuellen Zuständen können Sie den visuellen Zustand als Reaktion auf Größenänderungen des Fensters ändern.

Zustandsauslöser definieren einen Schwellenwert, ab dem ein visueller Zustand aktiviert wird. Anschließend werden die passenden Layouteigenschaften für die Fenstergröße festgelegt, die die Zustandsänderung ausgelöst hat.

Wenn die Breite des Fensters wie im folgenden Beispiel 720Pixel oder mehr beträgt, wird der visuelle Zustand **wideView** ausgelöst. Dadurch wird der Bereich **Best-rated games** rechts neben dem Bereich **Top free games** angezeigt und am oberen Rand ausgerichtet.

![Beispiel für visuellen Zustandsauslöser. Breite Ansicht](images/relativepanel-wideview.png)

Wenn das Fenster kleiner als 720Pixel ist, wird der visuelle Zustand **narrowView** ausgelöst, da die Voraussetzung für den **wideView**-Auslöser nicht mehr gegeben und er daher nicht mehr aktiv ist. Der visuelle Zustand **narrowView** positioniert den Bereich **Best-rated games** unten und richtet ihn linksbündig am Bereich **Top paid games** aus:

![Beispiel für visuellen Zustandsauslöser. Schmale Ansicht](images/relativepanel-narrowview.png)

Im Anschluss sehen Sie den XAML-Code für die oben beschriebenen visuellen Zustandsauslöser. Die Definition des Bereichs, auf den durch „`...`“ weiter unten verwiesen wird, wurde aus Platzgründen entfernt.

```XML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="wideView">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="720" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="best.(RelativePanel.RightOf)" Value="free"/>
                    <Setter Target="best.(RelativePanel.AlignTopWidth)" Value="free"/>
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="narrowView">
                <VisualState.Setters>
                    <Setter Target="best.(RelativePanel.Below)" Value="paid"/>
                    <Setter Target="best.(RelativePanel.AlignLeftWithPanel)" Value="true"/>
                </VisualState.Setters>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="0" />
                </VisualState.StateTriggers>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
    ...
</Grid>
```

### Tools

Standardmäßig möchten Sie die App wahrscheinlich auf die größtmögliche Gerätefamilie ausrichten. Wenn Sie bereit sind festzustellen, wie Ihre App und das Layout auf einem bestimmten Gerät aussieht, verwenden Sie die Geräte-Vorschauleiste in Visual Studio, um die UI auf einem kleinen oder mittleren Mobilgerät, einem PC oder einem großen Fernsehbildschirm in der Vorschau anzuzeigen. Auf diese Weise können Sie die adaptiven visuellen Zustände anpassen und testen:

![Geräte-Vorschauleiste in Visual Studio2015](images/vs2015-device-preview-toolbar.png)

Sie müssen nicht schon im Vorfeld im Detail entscheiden, welche Gerätetypen Sie unterstützen möchten. Sie können Ihrem Projekt auch später noch eine zusätzliche Gerätegröße hinzufügen.

### Adaptive Skalierung

Mit Windows10 wird eine Weiterentwicklung des vorhandenen Skalierungsmodells eingeführt. Neben der Skalierung von Vektorinhalten gibt es einen einheitlichen Satz von Skalierungsfaktoren, der eine einheitliche Größe für UI-Elemente für eine Vielzahl von Bildschirmgrößen und -auflösungen bietet. Die Skalierungsfaktoren sind auch mit den Skalierungsfaktoren anderer Betriebssysteme, wie iOS und Android, kompatibel. Dies erleichtert die gemeinsame Nutzung von Ressourcen zwischen diesen Plattformen.

Die Auswahl der aus dem Store herunterzuladenden Ressourcen erfolgt zum Teil auf Grundlage des DPI-Werts eines Geräts. Nur die Ressourcen, die dem Gerät am besten entsprechen, werden heruntergeladen.

### Allgemeine Eingabebehandlung

Sie können eine universelle Windows-App mit universellen Steuerelementen erstellen, die verschiedenen Eingaben verarbeiten können, wie z.B. über die Maus, die Tastatur, Fingereingaben, über einen Stift und einen Controller (wie z.B. dem Xbox-Controller). Mit Freihandeingabe war bislang in der Regel die Eingabe mit einem Stift gemeint. Unter Windows10 sind Freihandeingaben auf einigen Geräten per Toucheingabe oder mit anderen Zeigegeräten möglich. Die Freihandeingabe wird auf vielen Geräten unterstützt (einschließlich mobilen Geräten), und sie kann ganz einfach mit einigen wenigen Codezeilen eingebunden werden.

Die folgenden APIs bieten Zugriff auf Eingaben:

-   
              [
              **CoreIndependentInputSource**](https://msdn.microsoft.com/library/windows/apps/dn298460) ist eine neue API, mit der Sie Rohdateneingaben im Hauptthread oder in einem Hintergrundthread nutzen können.
-   
              [
              **PointerPoint**](https://msdn.microsoft.com/library/windows/apps/br242038) fasst unformatierte Touch-, Maus- und Stifteingaben zu einem einzelnen einheitlichen Satz von Schnittstellen und Ereignissen zusammen, die mit **CoreInput** im Hauptthread oder Hintergrundthread genutzt werden können.
-   
              [
              **PointerDevice**](https://msdn.microsoft.com/library/windows/apps/br225633) ist eine Geräte-API, die die Abfrage von Gerätefunktionen unterstützt, sodass Sie ermitteln können, welche Eingabemodalitäten auf dem Gerät verfügbar sind.
-   Das neue [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-XAML-Steuerelement und die [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011)-Windows-Runtime-APIs ermöglichen Ihnen den Zugriff auf Strichdaten.

## Schreiben von Code


Als Programmiersprachen für Ihr [Windows10-Projekt in Visual Studio](https://msdn.microsoft.com/library/windows/apps/dn609832.aspx#target_win10) können Sie Visual C++, C#, Visual Basic und JavaScript verwenden. Bei Verwendung von Visual C++, C# und Visual Basic können Sie XAML für eine qualitativ hochwertige, systemeigene Benutzeroberfläche verwenden. Bei Verwendung von Visual C++ können Sie statt XAML, oder auch zusätzlich dazu, das Zeichnen mit DirectX wählen. Bei der Verwendung von JavaScript ist die Darstellungsschicht HTML, wobei es sich um einen plattformübergreifenden Webstandard handelt. Ein Großteil Ihres Codes und der Benutzeroberfläche ist universell und kann überall auf die gleiche Weise ausgeführt werden. Wenn Code jedoch speziell auf bestimmte Gerätefamilien und die Benutzeroberfläche auf bestimmte Formfaktoren zugeschnitten ist, können Sie adaptiven Code und adaptive UI verwenden. Betrachten wir nun diese verschiedenen Fälle.

**Aufrufen einer API, die von Ihrer Zielgerätefamilie implementiert wird**

Wenn Sie eine API aufrufen möchten, müssen Sie wissen, ob diese API auch von der Gerätefamilie implementiert wird, auf die sie ausgerichtet ist. Im Zweifelsfall können Sie dies in der API-Referenzdokumentation nachschlagen. Wenn Sie das entsprechende Thema öffnen und sich den Abschnitt mit den Anforderungen ansehen, können Sie ermitteln, welche Gerätefamilie die implementierende Gerätefamilie ist. Nehmen wir einmal an, dass die App auf Version10.0.x.0 der universellen Gerätefamilie ausgelegt ist und Sie Member der [**Windows.UI.Core.SystemNavigationManager**](https://msdn.microsoft.com/library/windows/apps/dn893595)-Klasse aufrufen möchten. In diesem Beispiel ist die Gerätefamilie „Universal“. Es wird empfohlen, sich auch zu vergewissern, dass die Klassenmember, die Sie aufrufen möchten, ebenfalls innerhalb des Zielbereichs liegen. Dies ist hier der Fall. Daher wissen Sie in diesem Beispiel nun, dass die APIs garantiert auf jedem Gerät vorhanden sind, auf dem Ihre App installiert werden kann. Sie können die APIs daher in Ihrem Code wie gewohnt aufrufen.

```csharp
    Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += TestView_BackRequested;
```

Stellen Sie sich als ein weiteres Beispiel vor, dass die App auf die Version 10.0.x.0 der Xbox-Gerätefamilie ausgerichtet ist, und das Referenzthema für eine API, die Sie anrufen möchten, besagt, dass die API in Version 10.0.x.0 der Xbox-Gerätefamilie eingeführt wurde. In diesem Fall ist ebenfalls garantiert, dass die API auf jedem Gerät vorhanden ist, auf dem Ihre App installiert werden kann. Daher können Sie API in Ihrem Code auf die übliche Weise aufrufen.

Beachten Sie, dass die IntelliSense-Funktion von Visual Studio APIs nur dann erkennen kann, wenn sie von der Gerätefamilie, auf die Ihre App ausgerichtet ist, oder von Erweiterungs-SDKs, auf die Sie verweisen, implementiert werden. Wenn Sie nicht auf Erweiterungs-SDKs verweisen, können Sie daher sicher sein, dass die in IntelliSense aufgeführten APIs in der Zielgerätefamilie vorhanden sein müssen, und dass Sie sie uneingeschränkt aufrufen können.

**Aufrufen einer API, die von Ihrer Zielgerätefamilie NICHT implementiert wird**

Es werden Fälle auftreten, in denen Sie eine API aufrufen möchten, die nicht in der Dokumentation zu Ihrer Zielgerätefamilie aufgeführt ist. In diesem Fall können Sie optional adaptiven Code schreiben, um die API aufzurufen.

**Schreiben von adaptivem Code mit der ApiInformation-Klasse**

Das Schreiben von adaptivem Code umfasst zwei Schritte. Als ersten Schritt müssen Sie die APIs, auf die Sie zugreifen möchten, für Ihr Projekt verfügbar machen. Fügen Sie hierzu einen Verweis auf das Erweiterungs-SDK hinzu, das die Gerätefamilie mit den APIs darstellt, die Sie bedingt aufrufen möchten. Weitere Informationen finden Sie unter [Erweiterungs-SDKs](../porting/w8x-to-uwp-porting-to-a-uwp-project.md#extension-sdks).

Anschließend müssen Sie die [**Windows.Foundation.Metadata.ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001)-Klasse in einer Bedingung in Ihrem Code verwenden, um zu testen, ob die aufzurufende API vorhanden ist. Diese Bedingung wird immer dort ausgewertet, wo Ihre App ausgeführt wird. Das Ergebnis ist aber nur dann „True“, wenn sie auf Geräten ausgeführt wird, auf denen die API vorhanden ist und aufgerufen werden kann.

Wenn Sie nur einige wenige APIs aufrufen möchten, können Sie die [**ApiInformation.IsTypePresent**](https://msdn.microsoft.com/library/windows/apps/dn949016)-Methode auf diese Weise verwenden.

```csharp
    // Note: Cache the value instead of querying it more than once.
    bool isHardwareButtonsAPIPresent =
        Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.Phone.UI.Input.HardwareButtons");

    if (isHardwareButtonsAPIPresent)
    {
        Windows.Phone.UI.Input.HardwareButtons.CameraPressed +=
            HardwareButtons_CameraPressed;
    }
```

In diesem Fall können wir davon ausgehen, dass das Vorhandensein der [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557)-Klasse das Vorhandensein des [**CameraPressed**](https://msdn.microsoft.com/library/windows/apps/dn653805)-Ereignisses impliziert, da die Klasse und der Member über die gleichen Anforderungsinformationen verfügen. Mit der Zeit werden jedoch neue Member zu den bereits eingeführten Klassen hinzugefügt, und diese Member haben dann Versionsnummern mit dem Zusatz „eingeführt in“. In diesen Fällen können Sie mithilfe von Methoden wie **IsEventPresent**, **IsMethodPresent**, **IsPropertyPresent** und ähnlichen Methoden testen, ob die einzelnen Member vorhanden sind, anstatt **IsTypePresent** zu verwenden. Im Folgenden finden Sie ein Beispiel hierfür.

```csharp
    bool isHardwareButtons_CameraPressedAPIPresent =
        Windows.Foundation.Metadata.ApiInformation.IsEventPresent
            ("Windows.Phone.UI.Input.HardwareButtons", "CameraPressed");
```

Der Satz von APIs in einer Gerätefamilie wird in weitere Untereinheiten aufgeschlüsselt, die als „API-Verträge“ bezeichnet werden. Sie können die **ApiInformation.IsApiContractPresent**-Methode verwenden, um zu testen, ob ein API-Vertrag vorhanden ist. Dies ist hilfreich, wenn Sie das Vorhandensein einer großen Anzahl von APIs testen möchten, die alle in der gleichen Version eines API-Vertrags vorhanden sind.

```csharp
    bool isWindows_Devices_Scanners_ScannerDeviceContract_1_0Present =
        Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent
            ("Windows.Devices.Scanners.ScannerDeviceContract", 1, 0);
```

**Win32-APIs auf der UWP**

Eine UWP-App oder Komponente für Windows-Runtime, die in C++/CX geschrieben wurde, hat Zugriff auf die Win32 APIs, die Teil der universellen Windows-Plattform (UWP) sind. Diese Win32-APIs werden von allen Windows10-Gerätefamilien implementiert. Verknüpfen Sie Ihre App mit „Windowsapp.lib“. „Windowsapp.lib“ ist eine übergeordnete Bibliothek, die Exporte für die UWP-APIs bereitstellt. Die Verknüpfung mit „Windowsapp.lib“ fügt Ihrer App DLL-Abhängigkeiten hinzu, die für alle Windows10-Gerätefamilien vorhanden sind.

Eine vollständige Liste von Win32-APIs für UWP-Apps finden Sie unter [API-Sätze für UWP-Apps](https://msdn.microsoft.com/library/windows/desktop/mt186421) und [DLLs für UWP-Apps](https://msdn.microsoft.com/library/windows/desktop/mt186422).

## Benutzerfreundlichkeit

Eine universelle Windows-App ermöglicht Ihnen, die einzigartigen Funktionen des Geräts, auf dem die App ausgeführt wird, zu nutzen. Ihre App kann die gesamten Möglichkeiten von Desktopgeräten, natürliche Interaktionen mittels direkter Manipulationen auf Tablets (einschließlich Touch- und Stifteingabe), die Mobilität und Benutzerfreundlichkeit mobiler Geräte, die Funktionen zur Zusammenarbeit von [Surface Hub](http://go.microsoft.com/fwlink/?LinkId=526365) und weiterer Geräte nutzen, die UWP-Apps unterstützen.

Zu einem guten [Design](http://go.microsoft.com/fwlink/?LinkId=258848) gehören nicht nur das gute Aussehen und die Funktionen einer App, sondern auch die Entscheidung darüber, wie Benutzer mit der App interagieren. Die Benutzerfreundlichkeit spielt eine große Rolle bei der Beurteilung, wie gerne Benutzer Ihre App verwenden. Sparen Sie daher nicht an diesem Schritt. 
              [Designgrundlagen](https://dev.windows.com/design) bieten eine Einführung in das Design von Apps für die Universelle Windows-Plattform. Unter [Einführung in universelle Windows-Plattform-Apps (UWP) für Designer](https://msdn.microsoft.com/library/windows/apps/dn958439) finden Sie Informationen zum Entwerfen von UWP-Apps, die Benutzer begeistern. Bevor Sie mit dem Schreiben von Code beginnen, lesen Sie die Informationen unter [Einführung der Geräte](../input-and-devices/device-primer.md). Diese helfen Ihnen dabei, die Interaktionsmöglichkeiten Ihrer App für alle in Frage kommenden Formfaktoren zu durchdenken.

![Windows-Geräte](images/1894834-hig-device-primer-01-500.png)

Zusätzlich zur Interaktion auf verschiedenen Geräten sollten Sie [Ihre App planen](https://msdn.microsoft.com/library/windows/apps/hh465427), um die Vorteile verschiedener Geräte optimal zu nutzen. Beispiel:

-   Verwenden Sie [Clouddienste](http://go.microsoft.com/fwlink/?LinkId=526377) für die Synchronisierung auf allen Geräten. Erfahren Sie, wie Sie eine [Verbindung mit Webdiensten](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504) zur Unterstützung der App-Benutzerumgebung herstellen.

-   Überlegen Sie, wie Sie Benutzer unterstützen können, die von einem Gerät auf ein anderes wechseln, sodass sie nahtlos mit ihrer Arbeit fortfahren können. Beziehen Sie [Benachrichtigungen](https://msdn.microsoft.com/library/windows/apps/mt187203) und [In-App-Einkäufe](https://msdn.microsoft.com/library/windows/apps/mt219684) in Ihre Planung mit ein. Diese Features sollten auf allen Geräten funktionieren.

-   Entwerfen Sie Ihren Workflow anhand von [Navigationsdesigngrundlagen für UWP-Apps](https://msdn.microsoft.com/library/windows/apps/dn958438), damit die App auf dem Bildschirm eines mobilen Geräts genauso gut funktioniert wie auf Geräten mit mittelgroßem und großem Bildschirm. 
              [Erstellen Sie das Layout der Benutzeroberfläche](https://msdn.microsoft.com/library/windows/apps/dn958435), um unterschiedliche Bildschirmgrößen und Auflösungen zu berücksichtigen.

-   Überlegen Sie, ob Ihre App über Features verfügt, die für einen kleinen Bildschirm nicht sinnvoll sind. So kann es im Gegensatz dazu auch Bereiche geben, die auf einem Desktopcomputer nicht sinnvoll sind und ihr volles Potenzial erst auf einem mobilen Gerät entfalten. Hierzu zählen zum Beispiel Szenarien, bei denen es um den [Standort](https://msdn.microsoft.com/library/windows/apps/mt219698) geht, und die ein mobiles Gerät voraussetzen.

-   Achten Sie darauf, wie Sie verschiedene Eingabemodalitäten umsetzen können. In den [Richtlinien für Interaktionen](https://msdn.microsoft.com/library/windows/apps/dn611861) finden Sie Informationen darüber, wie Benutzer anhand von Features wie [Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233), [Sprache](https://msdn.microsoft.com/library/windows/apps/dn596121), [Toucheingaben](https://msdn.microsoft.com/library/windows/apps/hh465370), der [Touch-Bildschirmtastatur](https://msdn.microsoft.com/library/windows/apps/hh972345) und vielem mehr mit Ihrer App interagieren.

    In den [Richtlinien für Text und Texteingabe](https://msdn.microsoft.com/library/windows/apps/dn611864) finden Sie Informationen für eine herkömmlichere Benutzerumgebung.

## Übermitteln einer universellen Windows-App über das Dashboard


Im neuen einheitlichen WindowsDevCenter-Dashboard können Sie all Ihre Apps für Windows-Geräte zentral verwalten und einreichen. Neue Features vereinfachen Prozesse und geben Ihnen mehr Kontrolle. Sie finden dort auch detaillierte [Analyseberichte](https://msdn.microsoft.com/library/windows/apps/mt148522) in Kombination mit [Auszahlungsdetails](https://msdn.microsoft.com/library/windows/apps/dn986925), Möglichkeiten, [Ihre App zu bewerben und Kunden zu erreichen](https://msdn.microsoft.com/library/windows/apps/mt148526) und vieles mehr.

Unter [Verwenden des einheitlichen Windows Dev Center-Dashboards](../publish/using-the-windows-dev-center-dashboard.md) erfahren Sie, wie Sie Ihre Apps zur Veröffentlichung im WindowsStore übermitteln.

 

 



<!--HONumber=Jul16_HO2-->


