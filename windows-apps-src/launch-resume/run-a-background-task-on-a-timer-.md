---
author: TylerMSFT
title: "Ausführen einer Hintergrundaufgabe für einen Timer"
description: "Hier erfahren Sie, wie Sie eine einmalige Hintergrundaufgabe planen oder eine regelmäßige Hintergrundaufgabe ausführen."
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 3fc1e3efa742ff8ab24f78856872fe322703f152

---

# Ausführen einer Hintergrundaufgabe für einen Timer


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)
-   [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494)

Hier erfahren Sie, wie Sie eine einmalige Hintergrundaufgabe planen oder eine regelmäßige Hintergrundaufgabe ausführen.

-   In diesem Beispiel wird davon ausgegangen, dass eine Hintergrundaufgabe regelmäßig oder zu einer bestimmten Uhrzeit ausgeführt werden muss, um die App zu unterstützen. Eine Hintergrundaufgabe wird nur mithilfe eines [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) ausgeführt, wenn Sie [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) aufgerufen haben.
-   Dieses Thema setzt voraus, dass Sie bereits eine Hintergrundaufgabenklasse erstellt haben, einschließlich der Run-Methode, die für die Hintergrundaufgabe als Einstiegspunkt verwendet wird. Um schnell mit dem Erstellen einer Hintergrundaufgabe zu beginnen, lesen Sie die Infos unter [Erstellen und Registrieren einer Hintergrundaufgabe](create-and-register-a-background-task.md). Ausführlichere Informationen zu Bedingungen und Triggern finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

## Erstellen eines Zeitauslösers


-   Erstellen Sie einen neuen Zeitauslöser ([**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)). Der zweite Parameter (*OneShot*) gibt an, ob die Hintergrundaufgabe einmalig oder regelmäßig ausgeführt wird. Wenn *OneShot* auf „true“ festgelegt wird, gibt der erste Parameter (*FreshnessTime*) die Anzahl der Minuten an, die gewartet werden soll, bevor eine Hintergrundaufgabe geplant wird. Wenn *OneShot* auf „false“ festgelegt wird, gibt *FreshnessTime* die Häufigkeit an, mit der die Hintergrundaufgabe ausgeführt wird.

    Der integrierte Timer für auf Desktop- oder Mobilgeräte ausgerichtete UWP-Apps führt Hintergrundaufgaben in einem 15-Minuten-Intervall aus.

    -   Wenn *FreshnessTime* auf 15 Minuten und *OneShot* auf „true“ festgelegt ist, wird die Aufgabe einmalig innerhalb der ersten 15 Minuten nach dem Registrierungszeitpunkt ausgeführt.

    -   Wenn *FreshnessTime* auf 15 Minuten und *OneShot* auf „false“ festgelegt ist, wird die Aufgabe in den ersten 15 Minuten nach dem Registrierungszeitpunkt und dann regelmäßig alle 15 Minuten ausgeführt.

    **Hinweis**  Wenn *FreshnessTime* auf weniger als 15 Minuten festgelegt ist, wird bei dem Versuch, die Hintergrundaufgabe zu registrieren, eine Ausnahme ausgelöst.

     

    Dieser Auslöser veranlasst beispielsweise, dass eine Hintergrundaufgabe einmal pro Stunde ausgeführt wird:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
    > ```
    > ```cpp
    > TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
    > ```

## (Optional) Hinzufügen einer Bedingung


-   Erstellen Sie bei Bedarf eine Bedingung für die Hintergrundaufgabe, um zu steuern, wann die Aufgabe ausgeführt wird. Eine Bedingung sorgt dafür, dass die Hintergrundaufgabe erst ausgeführt wird, wenn die Bedingung erfüllt ist. Weitere Informationen finden Sie unter [Festlegen von Bedingungen für die Ausführung einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md).

    In diesem Beispiel ist die Bedingung auf **UserPresent** festgelegt, damit die ausgelöste Aufgabe nur ausgeführt wird, wenn der Benutzer aktiv ist. Eine Liste mit möglichen Bedingungen finden Sie in [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > SystemCondition userCondition = new SystemCondition(SystemConditionType.UserPresent);
    > ```
    > ```cpp
    > SystemCondition ^ userCondition = ref new SystemCondition(SystemConditionType::UserPresent)
    > ```

##  Aufrufen von RequestAccessAsync()


-   Bevor Sie versuchen, die [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)-Hintergrundaufgabe zu registrieren, rufen Sie [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494) auf.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > BackgroundExecutionManager.RequestAccessAsync();
    > ```
    > ```cpp
    > BackgroundExecutionManager::RequestAccessAsync();
    > ```

## Registrieren der Hintergrundaufgabe


-   Registrieren Sie die Hintergrundaufgabe, indem Sie die Funktion zum Registrieren der Hintergrundaufgabe aufrufen. Weitere Informationen zum Registrieren von Hintergrundaufgaben finden Sie unter [Registrieren einer Hintergrundaufgabe](register-a-background-task.md).

    Der folgende Code registriert die Hintergrundaufgabe:

    > > [!div class="tabbedCodeSnippets"]
    > ```cs
    > string entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > string taskName   = "Example hourly background task";
    >
    > BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
    > ```
    > ```cpp
    > String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > String ^ taskName   = "Example hourly background task";
    >
    > BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
    > ```

    > **Hinweis**  Parameter für die Registrierung von Hintergrundaufgaben werden zum Zeitpunkt der Registrierung überprüft. Bei ungültigen Registrierungsparametern wird ein Fehler zurückgegeben. Stellen Sie sicher, dass Ihre App Szenarien, in denen die Registrierung von Hintergrundaufgaben einen Fehler verursacht, problemlos verarbeitet.


## Anmerkungen

> **Hinweis**  Ab Windows 10 muss der Benutzer Ihre App nicht mehr zum Sperrbildschirm hinzufügen, um Hintergrundaufgaben zu nutzen. Die verschiedenen Arten von Auslösern für Hintergrundaufgaben werden unter [Unterstützen der App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md)erläutert.

> **Hinweis**  Dieser Artikel ist für Windows 10-Entwickler gedacht, die Apps für die Universelle Windows-Plattform(UWP) schreiben. Wenn Sie für Windows 8.x oder Windows Phone 8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).


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
* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)

* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [So wird's gemacht: Auslösen von Anhalte-, Fortsetzungs- und Hintergrundereignissen in Windows Store-Apps (beim Debuggen)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Jun16_HO4-->


