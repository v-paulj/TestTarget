---
author: mcleblanc
title: App-Lebenszyklus
description: In diesem Thema wird der Lebenszyklus einer UWP-App (Universelle Windows-Plattform) von ihrer Aktivierung bis zum Schließen beschrieben.
ms.assetid: 6C469E77-F1E3-4859-A27B-C326F9616D10
---

# App-Lebenszyklus


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**Windows.UI.Xaml.Application-Klasse**](https://msdn.microsoft.com/library/windows/apps/br242324)
-   [**Windows.ApplicationModel.Activation-Namespace**](https://msdn.microsoft.com/library/windows/apps/br224766)

In diesem Thema wird der Lebenszyklus einer UWP-App (Universelle Windows-Plattform) von ihrer Aktivierung bis zum Schließen beschrieben. Viele Benutzer verteilen ihre Arbeit und verwenden dabei mehrere Geräte und Apps. Benutzer erwarten jetzt von Ihrer App, dass sie den entsprechenden Status beim Multitasking auf dem Gerät speichert. Sie erwarten z. B., dass auf der Seite an dieselbe Position navigiert wird und sich alle Steuerelemente in demselben Zustand wie zuvor befinden. Wenn Sie den Lebenszyklus der App in Bezug auf das Starten, Anhalten und Fortsetzen verstehen, können Sie diese Art von reibungslosem Verhalten bereitstellen.

## App-Ausführungsstatus


In dieser Abbildung sind die Übergänge zwischen den App-Ausführungsstatus dargestellt. In den nächsten Abschnitten werden diese Status und Ereignisse beschrieben. Weitere Informationen zu den einzelnen Statusübergängen und dazu, wie die App darauf reagieren sollte, finden Sie in der Referenz zur [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694)-Enumeration.

![Statusdiagramm mit Übergängen zwischen App-Ausführungsstatus](images/state-diagram.png)

## Bereitstellung


Damit eine App aktiviert werden kann, muss sie zunächst bereitgestellt werden. Ihre App wird bereitgestellt, wenn ein Benutzer Ihre App installiert oder wenn Sie Visual Studio zum Erstellen und Ausführen der App während der Entwicklungs- und Testphase verwenden. Weitere Informationen hierzu und zu erweiterten Bereitstellungsszenarien finden Sie unter [App-Pakete und Bereitstellung](https://msdn.microsoft.com/library/windows/apps/hh464929).

## Starten einer App


Eine App wird gestartet, wenn sie sich im Zustand **NotRunning** befindet und der Benutzer auf dem Startbildschirm oder in der Anwendungsliste auf die App-Kachel tippt. Häufig verwendete Apps können auch vorab gestartet werden, um die Reaktionsfähigkeit zu optimieren (siehe [Behandeln des Vorabstarts von Apps](handle-app-prelaunch.md)). Eine App kann den Status **NotRunning** aufweisen, weil sie noch nie gestartet wurde, weil sie ausgeführt wurde und dann abgestürzt ist oder weil sie angehalten, dann aber nicht im Arbeitsspeicher vorgehalten werden konnte und vom System beendet wurde. Starten und Aktivierung sind unterschiedliche Vorgänge. Bei der Aktivierung wird Ihre App über einen Vertrag oder eine Erweiterung aktiviert, z. B. über den Vertrag für „Suche“.

Die [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)-Methode wird aufgerufen, wenn eine App gestartet wird. Dies ist auch dann der Fall, wenn die App derzeit im Arbeitsspeicher angehalten ist. Der [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731)-Parameter enthält den vorherigen Status der App sowie die Aktivierungsargumente.

Wenn der Benutzer zur beendeten App wechselt, sendet das System das [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731)-Argumente, wobei [**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) auf **Launch** und [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) auf **Terminated** oder **ClosedByUser** festgelegt ist. Von der App werden die gespeicherten Anwendungsdaten geladen, und der angezeigte Inhalt wird aktualisiert.

Wenn für [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) der Wert **NotRunning** festgelegt ist, sollte die App von vorne beginnen, als ob sie erstmalig gestartet wurde.

Beim Start einer App zeigt Windows einen Begrüßungsbildschirm für die App an. Informationen zum Konfigurieren des Begrüßungsbildschirms finden Sie unter [Hinzufügen eines Begrüßungsbildschirms](https://msdn.microsoft.com/library/windows/apps/xaml/hh465331).

Während der Begrüßungsbildschirm angezeigt wird, sollte die Benutzeroberfläche Ihrer App bereits vorbereitet sein. Die Hauptaufgaben der App sind die Registrierung von Ereignishandlern und die Einrichtung einer angepassten Benutzeroberfläche, die die App zum Laden der ersten Seite benötigt. Diese Aufgaben sollten nur einige Sekunden dauern. Wenn eine App Daten aus dem Netzwerk anfordern oder große Datenmengen von einem Datenträger abrufen muss, sollten diese Aktivitäten außerhalb der Aktivierung abgeschlossen werden. Eine App kann ihre eigene angepasste Ladebenutzeroberfläche oder einen erweiterten Begrüßungsbildschirm verwenden, während auf den Abschluss dieser Vorgänge mit langer Ausführungszeit gewartet wird. Weitere Infos finden Sie unter [Längere Anzeige des Begrüßungsbildschirms](create-a-customized-splash-screen.md) und im [Begrüßungsbildschirmbeispiel](http://go.microsoft.com/fwlink/p/?linkid=234889). Nach Abschluss der App-Aktivierung wechselt die App in den Status **Running**, und der Begrüßungsbildschirm sowie alle zugehörigen Ressourcen und Objekte werden ausgeblendet.

## Aktivieren einer App


Eine App kann vom Benutzer durch verschiedene Erweiterungen und Verträge wie dem Freigabe-Vertrag aktiviert werden. Eine Liste der Möglichkeiten zum Aktivieren Ihrer App finden Sie unter [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693).

Die [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324)-Klasse definiert Methoden, die außer Kraft gesetzt werden können, um die verschiedenen Aktivierungstypen zu behandeln. Mehrere der Aktivierungstypen verfügen über eine bestimmte Methode, die Sie außer Kraft setzen können, z. B. [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331), [**OnSearchActivated**](https://msdn.microsoft.com/library/windows/apps/br242336) usw. Für die anderen Aktivierungstypen setzen Sie die [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330)-Methode außer Kraft.

Der Aktivierungscode der App kann testen, warum die App aktiviert wurde und ob sie bereits den Status **Running** aufweist.

Die App kann zuvor gespeicherte Daten während der Aktivierung wiederherzustellen, falls die App vom Betriebssystem beendet und vom Benutzer anschließend neu gestartet wurde. Die App kann von Windows aus verschiedenen Gründen beendet werden, nachdem sie angehalten wurde. Der Benutzer kann die App manuell schließen oder sich abmelden, oder die Systemressourcen werden knapp. Wenn der Benutzer die App startet, nachdem sie von Windows beendet wurde, empfängt die App einen [**Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330)-Rückruf, und dem Benutzer wird der Begrüßungsbildschirm der App angezeigt, bis die App aktiviert wurde. Mit diesem Ereignis können Sie bestimmen, ob die App ihre Daten wiederherstellen muss, die beim letzten Anhalten der App gespeichert wurden, oder ob Sie die Standarddaten der App laden müssen. Da der Begrüßungsbildschirm bereits geladen ist, kann der App-Code Verarbeitungsleistung investieren, um diese Aufgabe ohne bemerkbare Verzögerung für den Benutzer zu erledigen. Allerdings gelten die zuvor geäußerten Bedenken zu Vorgängen mit langer Ausführungszeit auch beim Neustarten oder Fortsetzen der Ausführung.

Die [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330)-Ereignisdaten enthalten eine [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729)-Eigenschaft, an der Sie erkennen, in welchem Status sich die App vor der Aktivierung befunden hat. Diese Eigenschaft entspricht einem der Werte aus der [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694)-Enumeration:

| Grund für Beendigung                                                        | Wert der [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729)-Eigenschaft | Auszuführende Aktion          |
|-------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------|
| Beendet vom System (beispielsweise aufgrund von Ressourceneinschränkungen)       | **Terminated**                                                                                          | Sitzungsdaten wiederherstellen    |
| Von Benutzer geschlossen oder durch Benutzerprozess beendet                             | **ClosedByUser**                                                                                        | Mit Standarddaten starten |
| Unerwartet beendet, oder App wurde während der *aktuellen Benutzersitzung* nicht ausgeführt | **NotRunning**                                                                                          | Mit Standarddaten starten |

 

**Hinweis**
            
          
            *Aktuelle Benutzersitzung* basiert auf Windows-Anmeldung. Solange sich der aktuelle Benutzer nicht explizit abgemeldet oder das System heruntergefahren hat oder Windows nicht aus einem anderen Grund neu gestartet wurde, bleibt die aktuelle Benutzersitzung unabhängig von bestimmten Ereignissen – wie Sperrbildschirmauthentifizierung, Benutzerwechsel usw. – bestehen.

 

[
              **PreviousExecutionState**
            ](https://msdn.microsoft.com/library/windows/apps/br224729) könnte auch den Wert **Running** oder **Suspended** besitzen. In diesen Fällen wurde die App zuvor jedoch nicht beendet, und Sie müssen keine Daten wiederherstellen, da sich bereits alle Daten im Arbeitsspeicher befinden.

**Hinweis**  

Wenn Sie sich mit dem Administratorkonto des Computers anmelden, können Sie keine UWP-Apps aktivieren.

Weitere Informationen finden Sie unter [App-Erweiterungen](https://msdn.microsoft.com/library/windows/apps/hh464906).

### **OnActivated** im Vergleich zu bestimmten Aktivierungen

Die [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330)-Methode stellt das Instrument zur Behandlung aller möglichen Aktivierungstypen dar. Üblicherweise werden die meistverwendeten Aktivierungstypen jedoch mit unterschiedlichen Methoden behandelt, und **OnActivated** wird nur als Fallbackmethode für weniger verbreitete Aktivierungstypen verwendet. Beispielsweise verfügt [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) über eine [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)-Methode, die als Rückruf aufgerufen wird, sobald [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693) gleich **Launch** ist. Dies ist die typische Aktivierung für die meisten Apps. Es gibt 6 weitere **On\***-Methoden für spezifische Aktivierungen: [**OnCachedFileUpdaterActivated**](https://msdn.microsoft.com/library/windows/apps/hh701797), [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331), [**OnFileOpenPickerActivated**](https://msdn.microsoft.com/library/windows/apps/hh701799), [**OnFileSavePickerActivated**](https://msdn.microsoft.com/library/windows/apps/hh701801), [**OnSearchActivated**](https://msdn.microsoft.com/library/windows/apps/br242336), [**OnShareTargetActivated**](https://msdn.microsoft.com/library/windows/apps/hh701806). Startvorlagen für eine XAML-App verfügen über eine Implementierung für **OnLaunched** und einen Handler für [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341).

## Anhalten einer App


Das System hält Ihre App an, wenn der Benutzer zu einer anderen App oder zum Desktop bzw. Startbildschirm wechselt. Ihre App kann auch angehalten werden, wenn das Gerät in einen Energiesparmodus wechselt. Wenn der Benutzer wieder zu Ihrer App wechselt, wird die App vom System fortgesetzt. Beim Fortsetzen der App haben die Variablen und Datenstrukturen den gleichen Inhalt wie vor der Unterbrechung. Das System stellt die App exakt so wieder her, wie sie unterbrochen wurde. Dadurch entsteht für den Benutzer der Eindruck, die App wäre im Hintergrund weiter ausgeführt worden.

Wenn der Benutzer eine App in den Hintergrund verlagert, wird von Windows einige Sekunden abgewartet, ob der Benutzer sofort wieder zurück zur App wechselt, damit der Übergang in diesem Fall sehr schnell erfolgt. Wenn der Benutzer nicht innerhalb dieses Zeitfensters zurückkehrt, wird die App von Windows angehalten.

Wenn Sie asynchrone Arbeiten erledigen müssen, während die App angehalten wird, müssen Sie den Abschluss des Anhaltens bis zur Fertigstellung Ihrer Arbeit verzögern. Mithilfe der [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690)-Methode im [**SuspendingOperation**](https://msdn.microsoft.com/library/windows/apps/br224688)-Objekt (verfügbar über die Ereignisargumente) können Sie den Abschluss des Anhaltens verzögern, bis Sie die [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685)-Methode für das zurückgegebene [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684)-Objekt aufgerufen haben.

Das System versucht, die App und ihre Daten im Speicher zu behalten, während sie angehalten ist. Wenn das System aber nicht über die notwendigen Ressourcen verfügt, um die App im Arbeitsspeicher zu behalten, beendet es die App. Apps erhalten keine Benachrichtigung, dass sie beendet werden. App-Daten können also ausschließlich beim Anhalten einer App gespeichert werden. Wenn eine App feststellt, dass sie nach dem Beenden aktiviert wurde, sollte sie die beim Anhalten gespeicherten Anwendungsdaten laden, damit sie denselben Status wie vor dem Anhalten aufweist. Wenn der Benutzer wieder zu einer angehaltenen App wechselt, die beendet wurde, sollte die App ihre Anwendungsdaten mithilfe der [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)-Methode wiederherstellen. Das System benachrichtigt eine App nicht, wenn sie beendet wird. Wenn Ihre App angehalten wird, muss sie daher die App-Daten speichern und die exklusiven Ressourcen und Dateihandles freigeben, damit diese beim erneuten Aktivieren der App nach der Beendigung wiederhergestellt werden können.

Wenn eine App einen Ereignishandler für das [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341)-Ereignis registriert hat, wird dieser Code direkt vor dem Anhalten der App aufgerufen. Sie können App- und Benutzerdaten mithilfe des Ereignishandlers speichern. Es wird empfohlen, dafür die Anwendungsdaten-APIs zu verwenden, da diese garantiert abgeschlossen werden, bevor die App den Status **Suspended** annimmt. Weitere Informationen finden Sie unter [Speichern und Abrufen von Einstellungen und anderen App-Daten](https://msdn.microsoft.com/library/windows/apps/mt299098).

Sie sollten auch exklusive Ressourcen und Dateihandles freigeben, damit andere Apps darauf zugreifen können, während sie von Ihrer App nicht verwendet werden. Beispiele für exklusive Ressourcen sind Kameras, E/A-Geräte, externe Geräte und Netzwerkressourcen. Durch die explizite Freigabe von Ressourcen und Dateihandles kann sichergestellt werden, dass andere Apps auf die Ressourcen und Dateihandles zugreifen können, wenn Ihre App sie nicht verwendet. Wenn die App nach der Beendigung aktiviert ist, sollte sie ihre exklusiven Ressourcen und Dateihandles erneut öffnen.

Im Allgemeinen sollte die App beim Behandeln des Anhalteereignisses sofort ihren Status speichern und ihre Ressourcen und Dateihandles freigeben. Außerdem sollte die Ausführung des Codes maximal eine Sekunde dauern. Wenn eine App nicht innerhalb weniger Sekunden nach dem Anhalteereignis zurückkehrt, geht Windows davon aus, dass die App nicht mehr reagiert, und beendet sie.

In einigen Situationen muss eine App weiter ausgeführt werden, damit Hintergrundaufgaben abgeschlossen werden. Beispielsweise kann Ihre App weiterhin Sound im Hintergrund wiedergeben. Weitere Informationen finden Sie unter [Hintergrundaudio](https://msdn.microsoft.com/library/windows/apps/mt282140). Hintergrundübertragungen werden auch dann fortgesetzt, wenn die App angehalten oder beendet wurde. Weitere Informationen finden Sie unter [So wird's gemacht: Herunterladen einer Datei](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj152726.aspx#downloading_a_file_using_background_transfer).

Entsprechende Richtlinien finden Sie unter [Richtlinien für das Anhalten und Fortsetzen von Apps](https://msdn.microsoft.com/library/windows/apps/hh465088).

**Hinweis zum Debuggen mit Visual Studio:  **Visual Studio verhindert, dass in Windows eine an den Debugger angeschlossene App angehalten wird. Dies hat den Zweck, dem Benutzer das Anzeigen der Debugging-Benutzeroberfläche von Visual Studio zu ermöglichen, während die App ausgeführt wird. Beim Debuggen einer App können Sie mit Visual Studio ein Anhalteereignis an die App senden. Stellen Sie sicher, dass die Symbolleiste **Debugspeicherort** angezeigt wird, und klicken Sie dann auf das Symbol **Anhalten**.

## Sichtbarkeit einer App


Wenn der Benutzer von Ihrer App zu einer anderen App wechselt, ist Ihre App nicht mehr sichtbar, verbleibt aber weiterhin im Status **Running**, bis sie von Windows angehalten wird. Wenn der Benutzer Ihre App verlässt, Ihre App jedoch aktiviert oder zu ihr zurückkehrt, bevor sie angehalten werden konnte, wird der Ausführungsstatus **Running** nicht beendet.

Die App empfängt kein Aktivierungsereignis, wenn sich ihre Sichtbarkeit ändert, da die App noch ausgeführt wird. Windows wechselt je nach Bedarf einfach zwischen Apps. Falls eine App eine Aktion ausführen muss, wenn der Benutzer die App verlässt und zurückkehrt, kann sie dafür das [**Window.VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458)-Ereignis behandeln.

Verlassen Sie sich nicht auf eine bestimmte Anordnung dieser Ereignisse.

## Fortsetzen einer App


Eine angehaltene App wird fortgesetzt, wenn der Benutzer zu ihr wechselt oder es sich um die aktive App handelt und der Energiesparmodus für das Gerät beendet wird.

Eine Aufzählung der Status, die die App aufweisen kann, wenn sie fortgesetzt wird, finden Sie unter [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694). Wenn eine App aus dem Zustand **Suspended** fortgesetzt wird, wechselt sie in den Zustand **Running** und wird an dem Punkt fortgesetzt, an dem sie zuvor angehalten wurde. Keine im Arbeitsspeicher gespeicherten App-Daten gehen verloren. Daher müssen die meisten Apps keine Aktionen ausführen, wenn sie fortgesetzt werden. Möglicherweise hat sich die App jedoch mehrere Stunden oder sogar Tage im angehaltenen Status befunden. Wenn Inhalte oder Netzwerkverbindungen der App inzwischen veraltet sind, sollten diese bei der Fortsetzung aktualisiert werden. Wenn eine App einen Ereignishandler für das [**Application.Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339)-Ereignis registriert hat, wird dieser beim Fortsetzen der App aus dem Status **Suspended** aufgerufen. Sie können die App-Inhalte und -Daten mithilfe dieses Ereignishandlers aktualisieren.

Wenn eine angehaltene App für die Teilnahme an einem App-Vertrag oder einer Erweiterung aktiviert wird, empfängt sie zunächst das **Resuming**-Ereignis und dann das **Activated**-Ereignis.

Angehaltene Apps empfangen keine Netzwerkereignisse, für deren Empfang sie registriert sind. Diese Netzwerkereignisse werden nicht in die Warteschlange gestellt, sondern einfach verpasst. Apps sollten beim Fortsetzen daher den Netzwerkstatus prüfen.

**Hinweis**  Da das [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339)-Ereignis nicht vom UI-Thread ausgelöst wird, muss ein Dispatcher verwendet werden, wenn der Code im Resume-Handler mit der Benutzeroberfläche kommuniziert.

 

Entsprechende Richtlinien finden Sie unter [Richtlinien für das Anhalten und Fortsetzen von Apps](https://msdn.microsoft.com/library/windows/apps/hh465088).

## Schließen einer App


Im Allgemeinen müssen Benutzer Apps nicht schließen, sondern können deren Verwaltung Windows überlassen. Unter Windows können Benutzer Apps jedoch mit der Geste zum Schließen oder durch Drücken von ALT+F4 schließen. Unter Windows Phone werden Apps mithilfe der Aufgabenumschaltfunktion geschlossen.

Es gibt kein spezielles Ereignis zum Angeben, dass der Benutzer die App geschlossen hat.

Nachdem eine App durch den Benutzer geschlossen wurde, wird sie erst angehalten, dann beendet und anschließend in den Status **NotRunning** versetzt.

In Windows 8.1 und höher wird eine App nach dem Schließen durch den Benutzer vom Bildschirm und aus der Umschaltliste entfernt, aber nicht explizit beendet.

Wenn eine App einen Ereignishandler für das **Suspending**-Ereignis registriert hat, wird dieses aufgerufen, sobald die App angehalten wird. Sie können relevante Anwendungs- und Benutzerdaten mithilfe dieses Ereignishandlers im beständigen Speicher speichern.

**Verhalten nach dem Schließen:  **Wenn sich die App nach dem Schließen durch den Benutzer anders verhalten soll als nach dem Schließen durch Windows, können Sie mithilfe des Aktivierungsereignishandlers ermitteln, ob die App durch den Benutzer oder Windows beendet wurde. Beschreibungen zu den Status **ClosedByUser** und **Terminated** finden Sie in der Referenz für die [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694)-Enumeration.

Wir raten dazu, dass Apps sich selbst nur dann programmgesteuert schließen sollten, wenn dies absolut erforderlich ist. Wenn eine App beispielsweise einen Speicherverlust erkennt, kann sie sich selbst schließen, um die Sicherheit der persönlichen Daten des Benutzers zu wahren. Wenn Sie eine App programmgesteuert schließen, wird dies vom System wie ein Absturz der App behandelt.

## App-Absturz


Die Systemabsturzerfahrung ist dafür vorgesehen, dass die Benutzer ihre vorherigen Aktivitäten so schnell wie möglich wieder aufnehmen können. Sie sollten kein Warnungsdialogfeld oder eine andere Benachrichtigung bereitstellen, da dies für den Benutzer eine Verzögerung verursacht.

Wenn die App abstürzt, nicht mehr reagiert oder eine Ausnahme generiert, wird über die [Feedback- und Diagnoseeinstellungen](http://go.microsoft.com/fwlink/p/?LinkID=614828) des Benutzers ein Problembericht an Microsoft gesendet. Microsoft stellt Ihnen einige der Fehlerdaten im Problembericht bereit, mit denen Sie Ihre App verbessern können. Sie können diese Daten auf der Seite „Qualität“ Ihrer App im Dashboard einsehen.

Wenn der Benutzer eine App nach einem Absturz aktiviert, empfängt ihr Ereignishandler einen [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694)-Wert **NotRunning**, und es sollten einfach die ursprüngliche Benutzeroberfläche und die ursprünglichen Daten der App angezeigt werden. Nach einem Absturz sollten Sie die App-Daten, die Sie für **Resuming** verwendet hätten, nicht routinemäßig mit **Suspended** verwenden, da diese Daten beschädigt sein können. Weitere Informationen finden Sie unter [Richtlinien für das Anhalten und Fortsetzen von Apps](https://msdn.microsoft.com/library/windows/apps/hh465088).

## Entfernen einer App


Löscht ein Benutzer eine App, wird die App mit allen zugehörigen lokalen Daten entfernt. Das Entfernen von Apps hat keine Folgen für die Benutzerdaten, die an allgemeinen Speicherorten, z. B. in den Dokument- oder Bildbibliotheken, gespeichert wurden.

## App-Lebenszyklus und Visual Studio-Projektvorlagen


Der grundlegende Code, der für den Lebenszyklus der App relevant ist, wird in den Visual Studio-Startprojektvorlagen bereitgestellt. Die einfache App behandelt die Startaktivierung, stellt einen Speicherort zum Wiederherstellen Ihrer App-Daten bereit und zeigt die primäre Benutzeroberfläche an, bevor Sie eigenen Code hinzugefügt haben. Weitere Informationen finden Sie unter [C#-, VB- und C++-Projektvorlagen für Apps](https://msdn.microsoft.com/library/windows/apps/hh768232).

## Wichtige APIs für den Anwendungslebenszyklus


-   [
              **Windows.ApplicationModel**
            ](https://msdn.microsoft.com/library/windows/apps/br224691)-Namespace
-   [
              **Windows.ApplicationModel.Activation**
            ](https://msdn.microsoft.com/library/windows/apps/br224766)-Namespace
-   [
              **Windows.ApplicationModel.Core**
            ](https://msdn.microsoft.com/library/windows/apps/br205865)-Namespace
-   [
              **Windows.UI.Xaml.Application**
            ](https://msdn.microsoft.com/library/windows/apps/br242324)-Klasse (XAML)
-   [
              **Windows.UI.Xaml.Window**
            ](https://msdn.microsoft.com/library/windows/apps/br209041)-Klasse (XAML)

**Hinweis**  
Dieser Artikel ist für Windows 10-Entwickler bestimmt, die Apps für die universelle Windows-Plattform (UWP) schreiben. Informationen für die Entwicklung unter Windows 8.x oder Windows Phone 8.x finden Sie in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Verwandte Themen


* [Richtlinien für das Anhalten und Fortsetzen von Apps](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [Behandeln des Vorabstarts von Apps](handle-app-prelaunch.md)
* [Behandeln der App-Aktivierung](activate-an-app.md)
* [Behandeln des Anhaltens von Apps](suspend-an-app.md)
* [Behandeln der App-Fortsetzung](resume-an-app.md)

 

 





<!--HONumber=May16_HO2-->


