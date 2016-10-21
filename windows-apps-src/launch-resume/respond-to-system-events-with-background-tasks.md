---
author: TylerMSFT
title: Reagieren auf Systemereignisse mit Hintergrundaufgaben
description: "Hier erfahren Sie, wie Sie eine Hintergrundaufgabe erstellen können, die auf SystemTrigger-Ereignisse reagiert."
ms.assetid: 43C21FEA-28B9-401D-80BE-A61B71F01A89
translationtype: Human Translation
ms.sourcegitcommit: b877ec7a02082cbfeb7cdfd6c66490ec608d9a50
ms.openlocfilehash: 37a11b573267726707ee3743309083b774727886

---

# Reagieren auf Systemereignisse mit Hintergrundaufgaben

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Wichtige APIs**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838)

Hier erfahren Sie, wie Sie eine Hintergrundaufgabe erstellen können, die auf [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839)-Ereignisse reagiert.

Dieses Thema setzt voraus, dass Sie für Ihre App eine Hintergrundaufgabenklasse geschrieben haben und dass diese Aufgabe als Reaktion auf ein vom System ausgelöstes Ereignis ausgeführt werden muss, z.B. wenn sich die Internetverfügbarkeit ändert oder sich der Benutzer anmeldet. Der Schwerpunkt dieses Themas liegt auf der [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839)-Klasse. Weitere Informationen zum Schreiben einer Hintergrundaufgabenklasse finden Sie unter [Erstellen und Registrieren einer Einzelprozess-Hintergrundaufgabe](create-and-register-a-singleprocess-background-task.md) oder
* [Erstellen und Registrieren einer in einem separaten Prozess ausgeführten Hintergrundaufgabe](create-and-register-a-background-task.md).

## Erstellen eines SystemTrigger-Objekts

-   Erstellen Sie in Ihrem App-Code ein neues [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838)-Objekt. *triggerType* ist der erste Parameter und gibt den Typ des Systemereignis-Triggers zur Aktivierung der Hintergrundaufgabe an. Eine Liste mit Ereignistypen finden Sie unter [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839).

    Der zweite Parameter (*OneShot*) gibt an, ob die Hintergrundaufgabe nur einmal ausgeführt wird, wenn das Systemereignis das nächste Mal eintritt, oder ob die Hintergrundaufgabe beim Auslösen des Systemereignisses immer ausgeführt wird, bis die Registrierung der Aufgabe aufgehoben wird.

    Der folgende Code gibt an, dass die Hintergrundaufgabe immer ausgeführt wird, wenn das Internet verfügbar wird:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > SystemTrigger internetTrigger = new SystemTrigger(SystemTriggerType.InternetAvailable, false);
    > ```
    > ```cpp
    > SystemTrigger ^ internetTrigger = ref new SystemTrigger(SystemTriggerType::InternetAvailable, false);
    > ```

## Registrieren der Hintergrundaufgabe

-   Registrieren Sie die Hintergrundaufgabe, indem Sie die Funktion zum Registrieren der Hintergrundaufgabe aufrufen. Weitere Informationen zum Registrieren von Hintergrundaufgaben finden Sie unter [Registrieren einer Hintergrundaufgabe](register-a-background-task.md).

    Der folgende Code registriert die Hintergrundaufgabe für einen Hintergrundprozess, der in einem separaten Prozess ausgeführt wird. Wenn Sie eine Hintergrundaufgabe aufrufen würden, die im gleichen Prozess wie die Host-App ausgeführt wird, würden Sie nicht `entrypoint` festlegen:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > string entryPoint = "Tasks.ExampleBackgroundTaskClass"; // Namespace name, '.', and the name of the class containing the background task
    > string taskName   = "Internet-based background task";
    >
    > BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
    > ```
    > ```cpp
    > String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass"; // don't set for single-process background tasks
    > String ^ taskName   = "Internet-based background task";
    >
    > BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
    > ```

    > **Hinweis:** Universelle Windows-Apps müssen vor der Registrierung von Hintergrundtrigger-Typen [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) aufrufen.

    Rufen Sie [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) und anschließend [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) auf, wenn die App nach der Aktualisierung gestartet wird, um sicherzustellen, dass Ihre universelle Windows-App nach der Veröffentlichung eines Updates weiterhin ordnungsgemäß ausgeführt wird. Weitere Informationen finden Sie unter [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md).

    > **Hinweis:** Parameter für die Registrierung von Hintergrundaufgaben werden zum Zeitpunkt der Registrierung überprüft. Bei ungültigen Registrierungsparametern wird ein Fehler zurückgegeben. Stellen Sie sicher, dass Ihre App Szenarien, in denen die Registrierung von Hintergrundaufgaben einen Fehler verursacht, problemlos verarbeitet.
 
## Anmerkungen

Laden Sie das [Beispiel zu Hintergrundaufgaben](http://go.microsoft.com/fwlink/p/?LinkId=618666) herunter, um die Registrierung der Hintergrundaufgabe in Aktion zu sehen.

Hintergrundaufgaben können als Reaktion auf die Ereignisse [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) und [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) ausgeführt werden. Dennoch ist das [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md) erforderlich. Vor dem Registrieren einer Hintergrundaufgabe müssen Sie außerdem [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) aufrufen.

Apps können Hintergrundaufgaben registrieren, die auf die Ereignisse [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843), [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) und [**NetworkOperatorNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/br224831) reagieren. So können die Apps eine Echtzeitkommunikation mit dem Benutzer bereitstellen, auch wenn die App sich nicht im Vordergrund befindet. Weitere Informationen finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

> **Hinweis:** Dieser Artikel ist für Windows10-Entwickler gedacht, die Apps für die Universelle Windows-Plattform (UWP) schreiben. Informationen zur Entwicklung unter Windows8.x oder Windows Phone8.x finden Sie in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

## Verwandte Themen

****

* [Erstellen und Registrieren einer in einem separaten Prozess ausgeführten Hintergrundaufgabe](create-and-register-a-background-task.md)
* [Erstellen und Registrieren einer Einzelprozess-Hintergrundaufgabe](create-and-register-a-singleprocess-background-task.md)
* [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md)
* [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)
* [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
* [Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe](update-a-live-tile-from-a-background-task.md)
* [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)
* [Ausführen einer Hintergrundaufgabe für einen Timer](run-a-background-task-on-a-timer-.md)
* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)

****

* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [So wird's gemacht: Auslösen von Anhalte-, Fortsetzungs- und Hintergrundereignissen in Windows Store-Apps (beim Debuggen)](http://go.microsoft.com/fwlink/p/?linkid=254345)



<!--HONumber=Aug16_HO3-->


