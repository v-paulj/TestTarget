---
author: TylerMSFT
title: "Verwenden eines Wartungsauslösers"
description: "Hier erfahren Sie, wie Sie die MaintenanceTrigger-Klasse zum Ausführen von einfachem Code im Hintergrund verwenden, während das Gerät eingesteckt ist."
ms.assetid: 727D9D84-6C1D-4DF3-B3B0-2204EA4D76DD
translationtype: Human Translation
ms.sourcegitcommit: b877ec7a02082cbfeb7cdfd6c66490ec608d9a50
ms.openlocfilehash: 1181605c097f876af49e8055e245a2c445fc30d3

---

# Verwenden eines Wartungsauslösers

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

**Wichtige APIs**

-   [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)

Hier erfahren Sie, wie Sie die [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517)-Klasse zum Ausführen von einfachem Code im Hintergrund verwenden, während das Gerät eingesteckt ist.

## Erstellen eines MaintenanceTrigger-Objekts

In diesem Beispiel wird davon ausgegangen, dass Sie über einfachen Code verfügen, den Sie im Hintergrund ausführen können, um Ihre App zu optimieren, wenn das Gerät eingesteckt ist. Der Schwerpunkt dieses Themas liegt auf der [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517)-Klasse, die der [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839)-Klasse ähnelt.

Weitere Informationen zum Schreiben einer Hintergrundaufgabenklasse finden Sie in [Erstellen und Registrieren einer Einzelprozess-Hintergrundaufgabe](create-and-register-a-singleprocess-background-task.md) oder [Erstellen und Registrieren einer in einem separaten Prozess ausgeführten Hintergrundaufgabe](create-and-register-a-background-task.md).

Erstellen Sie ein neues [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)-Objekt. Der zweite Parameter *OneShot* gibt an, ob die Wartungsaufgabe nur ein Mal oder regelmäßig ausgeführt wird. Wenn *OneShot* auf „true“ festgelegt ist, gibt der erste Parameter (*FreshnessTime*) an, wie lange mit der Planung der Hintergrundaufgabe gewartet werden soll (in Minuten). Wenn *OneShot* auf „false“ festgelegt ist, gibt *FreshnessTime* an, wie oft die Hintergrundaufgabe ausgeführt wird.

> **Hinweis**  Wenn *FreshnessTime* auf weniger als 15 Minuten festgelegt ist, wird bei dem Versuch, die Hintergrundaufgabe zu registrieren, eine Ausnahme ausgelöst.

Dieser Beispielcode erstellt einen Auslöser, der einmal pro Stunde ausgeführt wird:

> [!div class="tabbedCodeSnippets"]
> ```cs
> uint waitIntervalMinutes = 60;
>
> MaintenanceTrigger taskTrigger = new MaintenanceTrigger(waitIntervalMinutes, false);
> ```
> ```cpp
> unsigned int waitIntervalMinutes = 60;
>
> MaintenanceTrigger ^ taskTrigger = ref new MaintenanceTrigger(waitIntervalMinutes, false);
> ```

## (Optional) Hinzufügen einer Bedingung

-   Erstellen Sie bei Bedarf eine Hintergrundaufgabenbedingung, um zu steuern, wann die Aufgabe ausgeführt wird. Eine Bedingung sorgt dafür, dass die Hintergrundaufgabe erst ausgeführt wird, wenn die Bedingung erfüllt ist. Weitere Informationen finden Sie unter [Festlegen von Bedingungen für die Ausführung einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md).

In diesem Beispiel wird die Bedingung auf **InternetAvailable** festgelegt, damit die Wartung ausgeführt wird, wenn eine Internetverbindung verfügbar ist (oder wird). Eine Liste mit möglichen Hintergrundaufgabenbedingungen finden Sie unter [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

Der folgende Code fügt dem Hintergrundaufgaben-Generator eine Bedingung hinzu:

> [!div class="tabbedCodeSnippets"]
> ```cs
> SystemCondition exampleCondition = new SystemCondition(SystemConditionType.InternetAvailable);
> ```
> ```cpp
> SystemCondition ^ exampleCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
> ```

## Registrieren der Hintergrundaufgabe

-   Registrieren Sie die Hintergrundaufgabe, indem Sie die Funktion zum Registrieren der Hintergrundaufgabe aufrufen. Weitere Informationen zum Registrieren von Hintergrundaufgaben finden Sie unter [Registrieren einer Hintergrundaufgabe](register-a-background-task.md).

    Der folgende Code registriert die Wartungsaufgabe. Beachten Sie, dass davon ausgegangen wird, dass die Hintergrundaufgaben in einem von Ihrer App getrennten Prozess ausgeführt wird, da `entryPoint` angegeben wird. Wenn die Hintergrundaufgabe in demselben Prozess wie Ihre App ausgeführt wird, wird `entryPoint` nicht angegeben.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > string entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > string taskName   = "Maintenance background task example";
    >
    > BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
    > ```
    > ```cpp
    > String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > String ^ taskName   = "Maintenance background task example";
    >
    > BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
    > ```

    > **Hinweis:**  Mit Ausnahme von Desktopgeräten können Hintergrundaufgaben bei allen Gerätefamilien beendet werden, wenn der Arbeitsspeicher des Geräts knapp wird. Wenn eine Ausnahme über wenig Arbeitsspeicher nicht angezeigt oder von der App nicht behandelt wird, wird die Hintergrundaufgabe ohne Warnung und ohne Auslösen des OnCanceled-Ereignisses beendet. Dadurch soll die Benutzerfreundlichkeit der App im Vordergrund sichergestellt werden. Entwerfen Sie die Hintergrundaufgabe so, dass dieses Szenario behandelt wird.

    > **Hinweis**  Universelle Windows-Apps müssen vor der Registrierung von Hintergrundtrigger-Typen [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) aufrufen.

    Rufen Sie [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) und anschließend [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) auf, wenn die App nach der Aktualisierung gestartet wird, um sicherzustellen, dass Ihre universelle Windows-App nach der Veröffentlichung eines Updates der App weiterhin ordnungsgemäß ausgeführt wird. Weitere Informationen finden Sie unter [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md).

    > **Hinweis**  Parameter für die Registrierung von Hintergrundaufgaben werden zum Zeitpunkt der Registrierung überprüft. Bei ungültigen Registrierungsparametern wird ein Fehler zurückgegeben. Stellen Sie sicher, dass Ihre App problemlos mit Szenarien ohne erfolgreiche Registrierung von Hintergrundaufgaben zurechtkommt. Andernfalls stürzt die App unter Umständen ab, wenn sie so konzipiert ist, dass nach dem Versuch, eine Aufgabe zu registrieren, ein gültiges Registrierungsobjekt vorhanden sein muss.


> **Hinweis**  Dieser Artikel ist für Windows 10-Entwickler gedacht, die Apps für die Universelle Windows-Plattform (UWP) schreiben. Informationen für die Entwicklung unter Windows8.x oder Windows Phone8.x finden Sie in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

## Verwandte Themen

****

* [Erstellen und Registrieren einer Einzelprozess-Hintergrundaufgabe](create-and-register-a-singleprocess-background-task.md).
* [Erstellen und Registrieren einer in einem separaten Prozess ausgeführten Hintergrundaufgabe](create-and-register-a-background-task.md)
* [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md)
* [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)
* [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
* [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
* [Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe](update-a-live-tile-from-a-background-task.md)
* [Ausführen einer Hintergrundaufgabe für einen Timer](run-a-background-task-on-a-timer-.md)
* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)

****

* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [So wird's gemacht: Auslösen von Anhalte-, Fortsetzungs- und Hintergrundereignissen in Windows Store-Apps (beim Debuggen)](http://go.microsoft.com/fwlink/p/?linkid=254345)



<!--HONumber=Aug16_HO3-->


