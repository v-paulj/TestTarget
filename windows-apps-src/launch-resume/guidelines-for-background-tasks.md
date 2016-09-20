---
author: TylerMSFT
title: "Richtlinien für Hintergrundaufgaben"
description: "Stellen Sie sicher, dass Ihre App die für die Ausführung von Hintergrundaufgaben erforderlichen Anforderungen erfüllt."
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
translationtype: Human Translation
ms.sourcegitcommit: 9ef86dcd4ae3d922b713d585543f1def48fcb645
ms.openlocfilehash: 57f4fd2beeb04db44804689e3e46744b4fc03ce5

---

# Richtlinien für Hintergrundaufgaben

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Stellen Sie sicher, dass Ihre App die für die Ausführung von Hintergrundaufgaben erforderlichen Anforderungen erfüllt.

## Ratschläge zu Hintergrundaufgaben

Beachten Sie beim Entwickeln Ihrer Hintergrundaufgabe und vor dem Veröffentlichen Ihrer App die folgenden Empfehlungen.

Wenn Sie eine Hintergrundaufgabe zur Medienwiedergabe im Hintergrund verwenden, finden Sie unter [Wiedergeben von Medien im Hintergrund](https://msdn.microsoft.com/en-us/windows/uwp/audio-video-camera/background-audio) Informationen zu Verbesserungen in Windows 10, Version 1607, die dies erleichtern.

**Einzelprozess-Hintergrundaufgaben im Vergleich zu in einem separaten Prozess ausgeführte Hintergrundaufgaben:** Mit Windows 10, Version 1607, wurden [Einzelprozess-Hintergrundaufgaben](create-and-register-a-singleprocess-background-task.md) eingeführt. Dadurch kann Hintergrundcode im selben Prozess ausgeführt werden wie die Vordergrund-App. Berücksichtigen Sie die folgenden Faktoren bei der Entscheidung, ob für Ihre Hintergrundaufgaben ein Einzelprozess oder ein separater Prozess verwendet werden soll:

|Überlegung | Auswirkungen |
|--------------|--------|
|Flexibilität   | Wenn der Hintergrundprozess in einem anderen Prozess ausgeführt wird, hat ein Absturz im Hintergrundprozess keine Auswirkungen auf Ihre Vordergrund-Anwendung. Außerdem kann die Hintergrundaktivität beendet werden, sogar innerhalb Ihrer App, wenn Ausführungszeitlimits überschritten werden. Das Trennen der Hintergrundarbeit in eine Aufgabe separat von der Vordergrund-App ist möglicherweise eine bessere Wahl, wenn Vordergrund- und Hintergrundprozess nicht miteinander kommunizieren müssen (denn ein wesentlicher Vorteil von Einzelprozess-Hintergrundaufgaben besteht darin, die Notwendigkeit von Inter-Prozess-Kommunikation zu eliminieren). |
|Einfachheit    | Einzelprozess-Hintergrundaufgaben erfordern keine prozessübergreifende Kommunikation und sind weniger schwierig zu schreiben.  |
|Verfügbare Trigger | Einzelprozess-Hintergrundaufgaben unterstützen nicht die folgenden Trigger: [DeviceUseTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) und **IoTStartupTask** |
|VoIP | Einzelprozess-Hintergrundaufgaben unterstützen nicht die Aktivierung einer VoIP-Hintergrundaufgabe in Ihrer Anwendung. |  

**CPU-Kontingente:** Hintergrundaufgaben sind durch die Gesamtbetrachtungszeit eingeschränkt, die ihnen basierend auf dem Triggertyp zugeteilt wird. Die meisten Trigger sind auf 30Sekunden der Gesamtbetrachtungszeit beschränkt. Einige können jedoch bis zu 10Minuten ausgeführt werden, um rechenintensive Aufgaben abschließen zu können. Hintergrundaufgaben sollten klein bleiben, um den Akku zu schonen und ein besseres Benutzererlebnis für die Apps im Vordergrund zu ermöglichen. Die für Hintergrundaufgaben geltenden Ressourcenbeschränkungen finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

**Verwalten von Hintergrundaufgaben:** Ihre App muss eine Liste mit registrierten Hintergrundaufgaben abrufen, sich für Fortschritts- und Vervollständigungshandler registrieren und diese Ereignisse angemessen behandeln. Ihre Hintergrundaufgabenklassen müssen Fortschritt, Abbruch und Abschluss berichten. Weitere Informationen finden Sie unter [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md) und [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md).

**Verwenden Sie [BackgroundTaskDeferral](https://msdn.microsoft.com/library/windows/apps/hh700499):**: Wenn Ihre Hintergrundaufgabenklasse asynchronen Code ausführt, müssen Sie Verzögerungen verwenden. Andernfalls kann Ihre Hintergrundaufgabe vorzeitig beendet werden, wenn die [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx)-Methode (oder die [OnBackgroundActivated](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx)-Methode bei Einzelprozess-Hintergrundaufgaben) abgeschlossen wird. Weitere Informationen finden Sie unter [Erstellen und Registrieren einer Hintergrundaufgabe](create-and-register-a-background-task.md).

Alternativ können Sie eine Verzögerung bewirken und asynchrone Methodenaufrufe mit **async/await** abschließen. Schließen Sie die Verzögerung nach den **await**-Methodenaufrufen.

**Aktualisieren des App-Manifests:** Deklarieren Sie für Hintergrundaufgaben, die in einem separaten Prozess ausgeführt werden, jede Hintergrundaufgabe im Anwendungsmanifest, zusammen mit dem Triggertyp, mit dem sie verwendet wird. Andernfalls kann Ihre App die Hintergrundaufgaben zur Laufzeit nicht registrieren.

Hintergrundaufgaben, die im gleichen Prozess wie die Vordergrund-App ausgeführt werden, müssen sich im Anwendungsmanifest nicht selbst deklarieren. Weitere Informationen zum Deklarieren von Hintergrundaufgaben, die in einem separaten Prozess im Manifest ausgeführt werden, finden Sie unter [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md).

**Vorbereiten von App-Updates:** Wenn Ihre App aktualisiert werden soll, erstellen und registrieren Sie eine **ServicingComplete**-Hintergrundaufgabe (siehe [SystemTriggerType](https://msdn.microsoft.com/library/windows/apps/br224839)), um die Registrierung von Hintergrundaufgaben für die vorherige Version der App aufzuheben und die Hintergrundaufgaben für die neue Version zu registrieren. Dies ist auch ein geeigneter Zeitpunkt, um App-Updates durchzuführen, die möglicherweise außerhalb des Kontexts der Ausführung im Vordergrund erforderlich sind.

**Anfordern der Ausführung von Hintergrundaufgaben:**

> **Wichtig**  Ab Windows 10 ist es keine Voraussetzung mehr, dass sich Apps auf dem Sperrbildschirm befinden, um Hintergrundaufgaben auszuführen.

UWP (Universelle Windows-Plattform)-Apps können alle unterstützten Aufgabentypen ausführen, ohne auf dem Sperrbildschirm angeheftet zu sein. Apps müssen jedoch vor dem Registrieren einer Hintergrundaufgabe [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) aufrufen. Diese Methode gibt [**BackgroundAccessStatus.Denied**](https://msdn.microsoft.com/library/windows/apps/hh700439) zurück, wenn der Benutzer Berechtigungen für Hintergrundaufgaben für Ihre App in den Geräteeinstellungen explizit verweigert hat.
## Prüfliste für Hintergrundaufgaben

*Gilt sowohl für Einzelprozess- als auch für in einem separaten Prozess ausgeführte Hintergrundaufgaben*

-   Verknüpfen Sie Ihre Hintergrundaufgabe mit dem korrekten Trigger.
-   Fügen Sie Bedingungen hinzu, die für das erfolgreiche Ausführen Ihrer Hintergrundaufgabe sorgen.
-   Behandeln Sie Status, Abschluss und Abbruch von Hintergrundaufgaben.
-   Registrieren Sie Ihre Hintergrundaufgaben während des App-Starts erneut. Dadurch wird sichergestellt, dass sie beim ersten Start der App registriert sind. Zudem können Sie mit dieser Methode feststellen, ob der Benutzer die Ausführung von Hintergrundaufgaben in Ihrer App deaktiviert hat (falls die Registrierung fehlschlägt)
-   Prüfen Sie Ihre App auf Fehler bei der Registrierung von Hintergrundaufgaben. Führen Sie die Registrierung der Hintergrundaufgabe ggf. mit anderen Parameterwerten erneut durch.
-   Für alle Gerätefamilien mit Ausnahme von Desktops können Hintergrundaufgaben beendet werden, wenn der Arbeitsspeicher des Geräts knapp wird. Wenn eine Ausnahme über wenig Arbeitsspeicher nicht angezeigt oder von der App nicht behandelt wird, wird die Hintergrundaufgabe ohne Warnung und ohne Auslösen des OnCanceled-Ereignisses beendet. Dadurch soll die Benutzerfreundlichkeit der App im Vordergrund sichergestellt werden. Entwerfen Sie die Hintergrundaufgabe so, dass dieses Szenario behandelt wird.

*Gilt nur für in einem separaten Prozess ausgeführte Hintergrundaufgaben*

-   Erstellen Sie Ihre Hintergrundaufgabe in einer Komponente für Windows-Runtime.
-   Zeigen Sie keine UI außer Popup-, Kachel- und Signalupdates von der Hintergrundaufgabe an.
-   Fordern Sie in der [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx)-Methode Verzögerungen für jeden asynchronen Methodenaufruf an, und schließen Sie diese, wenn die Methode abgeschlossen wurde. Sie können auch eine Verzögerung mit **async/await** verwenden.
-   Verwenden Sie den dauerhaften Speicher, um Daten für die Hintergrundaufgabe und die App freizugeben.
-   Deklarieren Sie jede Hintergrundaufgabe im Anwendungsmanifest zusammen mit dem Triggertyp, mit dem sie verwendet wird. Überprüfen Sie die Korrektheit von Einstiegspunkten und Triggertypen.
-   Geben Sie im Manifest nur dann ein Executable-Element an, wenn Sie einen Trigger nutzen, der in demselben Kontext wie die App ausgeführt werden soll (z.B. [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)).

*Gilt nur für Einzelprozess-Hintergrundaufgaben*

- Stellen Sie beim Abbrechen einer Aufgabe sicher, dass der `BackgroundActivated`-Ereignishandler vorhanden ist, bevor der Abbruch eintritt, oder der gesamte Prozess wird beendet.
-   Schreiben Sie Hintergrundaufgaben mit kurzer Laufzeit. Hintergrundaufgaben sind auf 30 Sekunden der Gesamtbetrachtungszeit beschränkt.
-   Verlassen Sie sich bei Hintergrundaufgaben nicht auf Benutzerinteraktionen.

## Windows: Prüfliste für Hintergrundaufgaben für sperrbildschirmfähige Apps

Befolgen Sie diese Richtlinien bei der Entwicklung von Hintergrundaufgaben, die auch auf dem Sperrbildschirm verwendet werden könnten. Befolgen Sie die Richtlinien in [Richtlinien und Prüfliste für Kacheln auf dem Sperrbildschirm](https://msdn.microsoft.com/library/windows/apps/hh465403).

-   Überprüfen Sie, ob Ihre App überhaupt auf dem Sperrbildschirm erscheinen muss, bevor Sie sie als sperrbildschirmfähige App entwickeln. Weitere Informationen hierzu finden Sie in der [Übersicht über den Sperrbildschirm](https://msdn.microsoft.com/library/windows/apps/hh779720).

-   Stellen Sie sicher, dass Ihre App auch ohne auf dem Sperrbildschirm zu erscheinen funktioniert.

-   Fügen Sie eine mit [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543), [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) oder [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) registrierte Hintergrundaufgabe ein, und deklarieren Sie sie im App-Manifest. Überprüfen Sie die Korrektheit von Einstiegspunkten und Triggertypen. Dies wird für die Zertifizierung benötigt und damit der Benutzer die App auf dem Sperrbildschirm platzieren kann.

**Hinweis**  
Dieser Artikel ist für Windows10-Entwickler bestimmt, die Apps für die universelle Windows-Plattform (UWP) schreiben. Informationen für die Entwicklung unter Windows8.x oder Windows Phone8.x finden Sie in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

## Verwandte Themen

* [Erstellen und Registrieren einer Einzelprozess-Hintergrundaufgabe](create-and-register-a-singleprocess-background-task.md).
* [Erstellen und Registrieren einer in einem separaten Prozess ausgeführten Hintergrundaufgabe](create-and-register-a-background-task.md)
* [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md)
* [Wiedergeben von Medien im Hintergrund](https://msdn.microsoft.com/en-us/windows/uwp/audio-video-camera/background-audio)
* [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)
* [Überwachen von Status und Durchführung von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
* [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
* [Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe](update-a-live-tile-from-a-background-task.md)
* [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)
* [Ausführen einer Hintergrundaufgabe für einen Timer](run-a-background-task-on-a-timer-.md)
* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [So wird's gemacht: Auslösen von Anhalte-, Fortsetzungs- und Hintergrundereignissen in Windows Store-Apps (beim Debuggen)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Aug16_HO4-->


