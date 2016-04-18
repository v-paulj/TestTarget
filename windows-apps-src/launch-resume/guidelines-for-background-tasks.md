---
title: Richtlinien für Hintergrundaufgaben
description: Stellen Sie sicher, dass Ihre App die für die Ausführung von Hintergrundaufgaben erforderlichen Anforderungen erfüllt.
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
---

# Richtlinien für Hintergrundaufgaben


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Stellen Sie sicher, dass Ihre App die für die Ausführung von Hintergrundaufgaben erforderlichen Anforderungen erfüllt.

## Ratschläge zu Hintergrundaufgaben


Beachten Sie beim Entwickeln Ihrer Hintergrundaufgabe und vor dem Veröffentlichen Ihrer App die folgenden Ratschläge.

**CPU-Kontingente:** Hintergrundaufgaben sind durch die Gesamtbetrachtungszeit eingeschränkt, die ihnen basierend auf dem Triggertyp zugeteilt wird. Die meisten Trigger sind auf 30 Sekunden der Gesamtbetrachtungszeit beschränkt. Einige können jedoch bis zu 10 Minuten ausgeführt werden, um rechenintensive Aufgaben abschließen zu können. Hintergrundaufgaben sollten klein bleiben, um den Akku zu schonen und ein besseres Benutzererlebnis für die Apps im Vordergrund zu ermöglichen. Die für Hintergrundaufgaben geltenden Ressourcenbeschränkungen finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

**Verwalten von Hintergrundaufgaben:** Ihre App muss eine Liste der registrierten Hintergrundaufgaben abrufen, sich für Fortschritts- und Vervollständigungshandler registrieren und diese Ereignisse angemessen behandeln. Ihre Hintergrundaufgabenklassen müssen Fortschritt, Abbruch und Abschluss berichten. Weitere Informationen finden Sie unter [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md) und [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md).

**Verwenden Sie [**BackgroundTaskDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700499): **Wenn Ihre Hintergrundaufgabenklasse asynchronen Code ausführt, müssen Sie Verzögerungen verwenden. Andernfalls wird Ihre Hintergrundaufgabe möglicherweise vorzeitig beendet, wenn die [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx)-Methode abgeschlossen wird. Weitere Informationen finden Sie unter [Erstellen und Registrieren einer Hintergrundaufgabe](create-and-register-a-background-task.md).

Alternativ können Sie eine Verzögerung bewirken und asynchrone Methodenaufrufe mit **async/await** abschließen. Schließen Sie die Verzögerung nach den **await**-Methodenaufrufen.

**Aktualisieren des App-Manifests:** Deklarieren Sie jede Hintergrundaufgabe im App-Manifest zusammen mit den Triggertypen, mit denen sie verwendet wird. Andernfalls kann Ihre App die Hintergrundaufgaben zur Laufzeit nicht registrieren. Weitere Informationen finden Sie unter [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md).

**Vorbereiten von App-Updates:** Wenn Ihre App aktualisiert wird, erstellen und registrieren Sie eine **ServicingComplete**-Hintergrundaufgabe (siehe [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839)), mit der App-Updates durchgeführt werden können, die möglicherweise außerhalb des im Vordergrund ausgeführten Kontexts erforderlich sind.

**Anfordern der Ausführung von Hintergrundaufgaben:**

> **Wichtig** Ab Windows 10 müssen sich Apps nicht mehr auf dem Sperrbildschirm befinden, um Hintergrundaufgaben auszuführen.

Universelle Windows-Plattform-Apps (UWP) können alle unterstützten Aufgabentypen ausführen, ohne auf dem Sperrbildschirm angeheftet zu sein. Apps müssen jedoch vor dem Registrieren einer Hintergrundaufgabe [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) aufrufen. Diese Methode gibt [**BackgroundAccessStatus.Denied**](https://msdn.microsoft.com/library/windows/apps/hh700439) zurück, wenn der Benutzer Berechtigungen für Hintergrundaufgaben für Ihre App in den Geräteeinstellungen explizit verweigert hat.
## Prüfliste für Hintergrundaufgaben


Die folgende Prüfliste gilt für alle Hintergrundaufgaben.

-   Verknüpfen Sie Ihre Hintergrundaufgabe mit dem korrekten Trigger.
-   Fügen Sie Bedingungen hinzu, die für das erfolgreiche Ausführen Ihrer Hintergrundaufgabe sorgen.
-   Behandeln Sie Status, Abschluss und Abbruch von Hintergrundaufgaben.
-   Zeigen Sie keine UI außer Popup-, Kachel- und Signalupdates von der Hintergrundaufgabe an.
-   Fordern Sie in der [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx)-Methode Verzögerungen für jeden asynchronen Methodenaufruf an, und schließen Sie diese, wenn die Methode abgeschlossen wurde. Sie können auch eine Verzögerung mit **async/await** verwenden.
-   Verwenden Sie den dauerhaften Speicher, um Daten für die Hintergrundaufgabe und die App freizugeben.
-   Deklarieren Sie jede Hintergrundaufgabe im App-Manifest zusammen mit den Triggertypen, mit denen sie verwendet wird. Überprüfen Sie die Korrektheit von Einstiegspunkten und Triggertypen.
-   Schreiben Sie Hintergrundaufgaben mit kurzer Laufzeit. Hintergrundaufgaben sind auf 30 Sekunden der Gesamtbetrachtungszeit beschränkt.
-   Verlassen Sie sich bei Hintergrundaufgaben nicht auf Benutzerinteraktionen.
-   Registrieren Sie Ihre Hintergrundaufgaben während des App-Starts erneut. Dadurch wird sichergestellt, dass sie beim ersten Start der App registriert sind. Zudem können Sie mit dieser Methode feststellen, ob der Benutzer die Ausführung von Hintergrundaufgaben in Ihrer App (falls die Registrierung fehlschlägt) deaktiviert hat.
-   Geben Sie im Manifest nur dann ein Executable-Element an, wenn Sie einen Trigger nutzen, der in demselben Kontext wie die App ausgeführt werden soll (z. B. [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)).
-   Prüfen Sie Ihre App auf Fehler bei der Registrierung von Hintergrundaufgaben. Führen Sie die Registrierung der Hintergrundaufgabe ggf. mit anderen Parameterwerten erneut durch.
-   Für alle Gerätefamilien mit Ausnahme von Desktops können Hintergrundaufgaben beendet werden, wenn der Arbeitsspeicher des Geräts knapp wird. Wenn eine Ausnahme über wenig Arbeitsspeicher nicht angezeigt oder von der App nicht behandelt wird, wird die Hintergrundaufgabe ohne Warnung und ohne Auslösen des OnCanceled-Ereignisses beendet. Dadurch soll die Benutzerfreundlichkeit der App im Vordergrund sichergestellt werden. Entwerfen Sie die Hintergrundaufgabe so, dass dieses Szenario behandelt wird.

## Windows: Prüfliste für Hintergrundaufgaben für sperrbildschirmfähige Apps


Befolgen Sie diese Richtlinien bei der Entwicklung von Hintergrundaufgaben, die auch auf dem Sperrbildschirm verwendet werden könnten. Befolgen Sie die Richtlinien in [Richtlinien und Prüfliste für Kacheln auf dem Sperrbildschirm](https://msdn.microsoft.com/library/windows/apps/hh465403).

-   Überprüfen Sie, ob Ihre App überhaupt auf dem Sperrbildschirm erscheinen muss, bevor Sie sie als sperrbildschirmfähige App entwickeln. Weitere Informationen hierzu finden Sie in der [Übersicht über den Sperrbildschirm](https://msdn.microsoft.com/library/windows/apps/hh779720).

-   Stellen Sie sicher, dass Ihre App auch ohne auf dem Sperrbildschirm zu erscheinen funktioniert.

-   Fügen Sie eine mit [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543), [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) oder [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) registrierte Hintergrundaufgabe ein, und deklarieren Sie sie im App-Manifest. Überprüfen Sie die Korrektheit von Einstiegspunkten und Triggertypen. Dies wird für die Zertifizierung benötigt und damit der Benutzer die App auf dem Sperrbildschirm platzieren kann.

**Hinweis**  
Dieser Artikel ist für Windows 10-Entwickler bestimmt, die Apps für die universelle Windows-Plattform (UWP) schreiben. Wenn Sie für Windows 8.x oder Windows Phone 8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Verwandte Themen

* [Erstellen und Registrieren einer Hintergrundaufgabe](create-and-register-a-background-task.md)
* [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md)
* [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)
* [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
* [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
* [Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe](update-a-live-tile-from-a-background-task.md)
* [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)
* [Ausführen einer Hintergrundaufgabe für einen Timer](run-a-background-task-on-a-timer-.md)
* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [So wird's gemacht: Auslösen von Anhalte-, Fortsetzungs- und Hintergrundereignissen in Windows Store-Apps (beim Debuggen)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 





<!--HONumber=Mar16_HO1-->


