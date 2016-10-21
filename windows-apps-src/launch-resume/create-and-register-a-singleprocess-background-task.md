---
author: TylerMSFT
title: Erstellen und Registrieren einer Einzelprozess-Hintergrundaufgabe
description: "Erstellen und registrieren Sie eine Einzelprozessaufgabe, die im gleichen Prozess wie die Vordergrund-App ausgeführt wird."
translationtype: Human Translation
ms.sourcegitcommit: 9e959a8ae6bf9496b658ddfae3abccf4716957a3
ms.openlocfilehash: 5a2461d00114ba71ced7cca64c197f253f33690d

---

# Erstellen und Registrieren einer Einzelprozess-Hintergrundaufgabe

**Wichtige APIs**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

In diesem Thema wird gezeigt, wie Sie eine Hintergrundaufgabe erstellen und registrieren, die im gleichen Prozess wie Ihre App ausgeführt wird.

Einzelprozess-Hintergrundaufgaben sind einfacher zu implementieren als Hintergrundaufgaben, die in einem separaten Prozess ausgeführt werden. Sie sind jedoch weniger stabil. Wenn der im Hintergrund ausgeführte Code abstürzt, wirkt sich dies auf Ihre App aus. Beachten Sie außerdem, dass [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) und **IoTStartupTask** nicht mit dem Einzelprozessmodell verwendet werden können. Zudem ist das Aktivieren einer VoIP-Hintergrundaufgabe innerhalb der Anwendung nicht möglich. Diese Trigger und Aufgaben werden bei Verwendung des Multiprozess-Hintergrundaufgabenmodells weiterhin unterstützt.

Beachten Sie, dass Hintergrundaktivitäten auch bei Ausführung innerhalb des Vordergrundprozesses des App beendet werden können, wenn Ausführungszeitlimits überschritten werden. Für bestimmte Zwecke ist die Resilienz der Trennung von Arbeiten in eine Hintergrundaufgabe, die in einem separaten Prozess ausgeführt wird, weiterhin hilfreich. Die Trennung von Hintergrundarbeiten als Aufgabe von der Vordergrundanwendung kann die beste Option für Arbeiten sein, die keine Kommunikation mit der Vordergrundanwendung erfordern.

## Grundlagen

Das Einzelprozessmodell erweitert den Anwendungslebenszyklus mit verbesserter Benachrichtigungen dafür, wann sich die App im Vordergrund oder im Hintergrund befindet. Zwei neue Ereignisse sind vom Anwendungsobjekt für diese Übergänge verfügbar: [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) und [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground). Diese Ereignisse passen je nach Sichtbarkeitszustand der Anwendung in den Anwendungslebenszyklus. Weitere Informationen zu diesen Ereignissen und deren Auswirkungen auf den Anwendungslebenszyklus finden Sie unter [App-Lebenszyklus](app-lifecycle.md).

Auf oberer Ebene behandeln Sie das **EnteredBackground**-Ereignis zum Ausführen von Code bei der Ausführung der App im Hintergrund und das **LeavingBackground**-Ereignis, um zu wissen, wann Ihre App in den Vordergrund verschoben wurde.

## Registrieren des Triggers für die Hintergrundaufgabe

Einzelprozess-Hintergrundaktivitäten werden ähnlich wie Multiprozess-Hintergrundaktivitäten registriert. Alle Hintergrundtrigger werden mit der Registrierung mit [BackgroundTaskBuilder](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundtaskbuilder.aspx?f=255&MSPPError=-2147217396) gestartet. Der Generator vereinfacht die Registrierung einer Hintergrundaufgabe, indem alle erforderlichen Werte an einem zentralen Ort festlegt werden:

> [!div class="tabbedCodeSnippets"]
> ```cs
> var builder = new BackgroundTaskBuilder();
> builder.Name = "My Background Trigger";
> builder.SetTrigger(new TimeTrigger(15, true));
> // Do not set builder.TaskEntryPoint for single-process background tasks
> // Here we register the task and work will start based on the time trigger.
> BackgroundTaskRegistration task = builder.Register();
> ```

> [!NOTE]
> Universelle Windows-Apps müssen jedoch [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) aufrufen, bevor Hintergrundtrigger-Typen registriert werden.
> Rufen Sie [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) und anschließend [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) auf, wenn die App nach der Aktualisierung gestartet wird, um sicherzustellen, dass Ihre universelle Windows-App nach der Veröffentlichung eines Updates weiterhin ordnungsgemäß ausgeführt wird. Weitere Informationen finden Sie unter [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md).

Einzelprozess-Hintergrundaktivitäten erfolgen ohne das Festlegen von `TaskEntryPoint.` Bleibt dieser Wert leer, wird der Standardeinstiegspunkt aktiviert, eine neue geschützte Methode für das Application-Objekt namens [OnBackgroundActivated()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx).

Sobald ein Trigger registriert ist, wird er basierend auf dem in der [SetTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundtaskbuilder.settrigger.aspx)-Methode festgelegten Triggertyp ausgelöst. Im obigen Beispiel wird ein [TimeTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.timetrigger.aspx) verwendet, der 15Minuten nach dem Registrierungszeitpunkt ausgelöst wird.

## Hinzufügen einer Bedingung, die bestimmt, wann Ihre Aufgabe ausgeführt wird (optional)

Sie können eine Bedingung hinzufügen, die bestimmt, wann Ihre Aufgabe ausgeführt wird, nachdem das Auslöseereignis eintritt. Wenn Sie zum Beispiel möchten, dass die Aufgabe erst ausgeführt wird, wenn der Benutzer anwesend ist, verwenden Sie die Bedingung **UserPresent**. Eine Liste mit möglichen Bedingungen finden Sie in [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

    The following sample code assigns a condition requiring the user to be present:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
    > ```

## Platzieren des Codes der Hintergrundaktivität in OnBackgroundActivated()

Platzieren Sie den Code der Hintergrundaktivität in [OnBackgroundActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx)**, um auf den Hintergrundtrigger zu reagieren, wenn dieser ausgelöst wird. **OnBackgroundActivated** kann genauso wie [IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) behandelt werden. Die Methode verfügt über einen [BackgroundActivatedEventArgs](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.backgroundactivatedeventargs.aspx)-Parameter, der alle Elemente enthält, die die Run-Methode bereitstellt.

## Behandeln des Status und Abschlusses von Hintergrundaufgaben

Der Aufgabenstatus und -abschluss kann genauso wie bei Multiprozess-Hintergrundaufgaben überwacht werden (siehe [Überwachen von Status und Durchführung von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)). Wahrscheinlich stellen Sie jedoch fest, dass die Überwachung mithilfe von Variablen zum Nachverfolgen von Status oder Abschluss in Ihrer App einfacher ist. Dies ist einer der Vorteile, wenn der Code der Hintergrundaktivität im selben Prozess wie Ihre App ausgeführt wird.

## Behandeln des Abbruchs von Hintergrundaufgaben

Einzelprozess-Hintergrundaufgaben werden auf die gleiche Weise abgebrochen wie Multiprozess-Hintergrundaufgaben (siehe [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)). Beachten Sie, dass der **BackgroundActivated**-Ereignishandler vor dem Abbruch beendet werden muss, oder der gesamte Prozess wird beendet. Wenn die Vordergrund-App beim Abbrechen der Hintergrundaufgabe unerwartet geschlossen wird, überprüfen Sie, ob der Handler vor dem Abbruch beendet wurde.

## Das Manifest

Im Gegensatz zu Multiprozess-Hintergrundaufgaben müssen Sie dem Paketmanifest keine Hintergrundaufgabeninformationen hinzufügen, um Einzelprozess-Hintergrundaufgaben auszuführen.

## Zusammenfassung und nächste Schritte

Sie sollten jetzt über die Grundlagen verfügen, um eine Einzelprozess-Hintergrundaufgabe zu schreiben.

Eine API-Referenz, konzeptionelle Richtlinien zu Hintergrundaufgaben und ausführlichere Anweisungen zum Schreiben von Apps, die Hintergrundaufgaben verwenden, finden Sie unter den folgenden verwandten Themen:

## Verwandte Themen

**Ausführliche Themen mit Anweisungen zu Hintergrundaufgaben**

* [Konvertieren einer Multiprozess-Hintergrundaufgabe in eine Einzelprozess-Hintergrundaufgabe](convert-multiple-process-background-task.md)
* [Erstellen und Registrieren einer in einem separaten Prozess ausgeführten Hintergrundaufgabe](create-and-register-a-background-task.md)
* [Wiedergeben von Medien im Hintergrund](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)
* [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
* [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)
* [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)
* [Überwachen von Status und Durchführung von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)
* [Ausführen einer Hintergrundaufgabe für einen Timer](run-a-background-task-on-a-timer-.md)
* [Erstellen und Registrieren einer Einzelprozess-Hintergrundaufgabe](create-and-register-a-singleprocess-background-task.md).  

**Ratschläge zu Hintergrundaufgaben**

* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [So wird's gemacht: Auslösen von Anhalte-, Fortsetzungs- und Hintergrundereignissen in Windows Store-Apps (beim Debuggen)](http://go.microsoft.com/fwlink/p/?linkid=254345)

**Hintergrundaufgabe – API-Referenz**

* [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)



<!--HONumber=Aug16_HO3-->


