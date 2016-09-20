---
author: TylerMSFT
title: Registrieren einer Hintergrundaufgabe
description: Hier erfahren Sie, wie eine Funktion erstellt wird, die zum sicheren Registrieren der meisten Hintergrundaufgaben wiederverwendet werden kann.
ms.assetid: 8B1CADC5-F630-48B8-B3CE-5AB62E3DFB0D
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: acee438ae29b568bec20ff1225e8e801934e6c50

---

# Registrieren einer Hintergrundaufgabe


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**BackgroundTaskRegistration-Klasse**](https://msdn.microsoft.com/library/windows/apps/br224786)
-   [**BackgroundTaskBuilder-Klasse**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**SystemCondition-Klasse**](https://msdn.microsoft.com/library/windows/apps/br224834)

Hier erfahren Sie, wie eine Funktion erstellt wird, die zum sicheren Registrieren der meisten Hintergrundaufgaben wiederverwendet werden kann.

In diesem Thema wird davon ausgegangen, dass Sie bereits über eine Hintergrundaufgabe verfügen, die registriert werden muss. (Informationen zum Schreiben von Hintergrundaufgaben finden Sie unter [Erstellen und Registrieren einer Hintergrundaufgabe](create-and-register-a-background-task.md).

Dieses Thema behandelt schrittweise eine Hilfsfunktion, die Hintergrundaufgaben registriert. Diese Hilfsfunktion sucht zuerst nach vorhandenen Registrierungen, um Probleme mit mehrfachen Registrierungen zu vermeiden. Sie kann außerdem eine Systembedingung auf die Hintergrundaufgabe anwenden. Die exemplarische Vorgehensweise enthält ein vollständiges lauffähiges Beispiel für diese Hilfsfunktion.

**Hinweis**  

Universelle Windows-Apps müssen jedoch [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) aufrufen, bevor Hintergrund-Triggertypen registriert werden.

Rufen Sie [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) und anschließend [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) auf, wenn die App nach der Aktualisierung gestartet wird, um sicherzustellen, dass Ihre universelle Windows-App nach der Veröffentlichung eines Updates weiterhin ordnungsgemäß ausgeführt wird. Weitere Informationen finden Sie unter [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md).

## Definieren von Methodensignatur und Rückgabetyp


Diese Methode übernimmt den Einstiegspunkt der Aufgabe, den Namen der Aufgabe, einen vorgefertigten Trigger für die Hintergrundaufgabe und (optional) eine [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) für die Hintergrundaufgabe. Diese Methode gibt ein [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)-Objekt zurück.

> [!div class="tabbedCodeSnippets"]
> ```cs
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint,
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     
>     // We'll add code to this function in subsequent steps.
>
> }
> ```
> ```cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
>     
>     // We'll add code to this function in subsequent steps.
>
> }
> ```

## [!div class="tabbedCodeSnippets"]


Überprüfung von vorhandenen Registrierungen Überprüfen Sie, ob die Aufgabe bereits registriert ist.

Diese Überprüfung ist wichtig, denn eine mehrfach registrierte Hintergrundaufgabe wird mehrfach aufgerufen, wenn sie ausgelöst wird. Dies kann die CPU überlasten und zu unerwartetem Verhalten führen. Sie können die vorhandenen Registrierungen durch Abfrage der [**BackgroundTaskRegistration.AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787)-Eigenschaft und Iteration über das Ergebnis überprüfen.

> Überprüfen Sie die Namen aller Instanzen: wenn ein Name mit dem Namen der Aufgabe übereinstimmt, die registriert werden soll, verlassen Sie die Schleife, und legen Sie eine Flag-Variable fest, damit Ihr Code im nächsten Schritt einen anderen Pfad wählen kann. 
            **Hinweis**  Verwenden Sie innerhalb Ihrer App eindeutige Namen für Hintergrundaufgaben.

Stellen Sie sicher, dass jede Hintergrundaufgabe einen eindeutigen Namen hat.

> [!div class="tabbedCodeSnippets"]
> ```cs
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint,
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
>
>         if (cur.Value.Name == name)
>         {
>             //
>             // The task is already registered.
>             //
>
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>     
>     // We'll register the task in the next step.
> }
> ```
> ```cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>     
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>     
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>         
>         if(cur->Name == name)
>         {
>             //
>             // The task is registered.
>             //
>             
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>         
>         hascur = iter->MoveNext();
>     }
>     
>     // We'll register the task in the next step.
> }
> ```

## Der folgende Code registriert eine Hintergrundaufgabe mit dem im vorigen Schritt erstellten [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838):


[!div class="tabbedCodeSnippets"] Registrieren der Hintergrundaufgabe (oder Rückgabe der bereits vorhandenen Registrierung)

Überprüfen Sie, ob sich die Aufgabe in der Liste der bereits registrierten Hintergrundaufgaben befindet. Geben Sie in diesem Fall diese Instanz der Aufgabe zurück. Registrieren Sie die Aufgabe dann mithilfe eines neuen [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)-Objekts.

> Der Code sollte überprüfen, ob der Parameter für die Bedingung null ist, und andernfalls die Bedingung an das Registrierungsobjekt anfügen. Geben Sie die von der [**BackgroundTaskBuilder.Register**](https://msdn.microsoft.com/library/windows/apps/br224772)-Methode zurückgegebene [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) zurück. 
            **Hinweis**  Parameter für die Registrierung von Hintergrundaufgaben werden zum Zeitpunkt der Registrierung überprüft.

Bei ungültigen Registrierungsparametern wird ein Fehler zurückgegeben.

> [!div class="tabbedCodeSnippets"]
> ```cs
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint,
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
>
>         if (cur.Value.Name == taskName)
>         {
>             //
>             // The task is already registered.
>             //
>
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>     
>     //
>     // Register the background task.
>     //
>
>     var builder = new BackgroundTaskBuilder();
>
>     builder.Name = name;
>     builder.TaskEntryPoint = taskEntryPoint;
>     builder.SetTrigger(trigger);
>
>     if (condition != null)
>     {
>
>         builder.AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration task = builder.Register();
>
>     return task;
> }
> ```
> ```cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
>
>     //
>     // Check for existing registrations of this background task.
>     //
>     
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>     
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>         
>         if(cur->Name == name)
>         {
>             //
>             // The task is registered.
>             //
>             
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>         
>         hascur = iter->MoveNext();
>     }
>     
>     //
>     // Register the background task.
>     //
>
>     auto builder = ref new BackgroundTaskBuilder();
>
>     builder->Name = name;
>     builder->TaskEntryPoint = taskEntryPoint;
>     builder->SetTrigger(trigger);
>
>     if (condition != nullptr) {
>         
>         builder->AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration ^ task = builder->Register();
>
>     return task;
> }
> ```

## Stellen Sie sicher, dass Ihre App Szenarien, in denen die Registrierung von Hintergrundaufgaben einen Fehler verursacht, problemlos verarbeitet.


Im folgenden Beispiel wird entweder die vorhandene Aufgabe zurückgegeben, oder es wird Code hinzugefügt, mit dem die Hintergrundaufgabe registriert wird (ggf. einschließlich der Systembedingung): [!div class="tabbedCodeSnippets"]

> [!div class="tabbedCodeSnippets"]
> ```cs
> //
> // Register a background task with the specified taskEntryPoint, name, trigger,
> // and condition (optional).
> //
> // taskEntryPoint: Task entry point for the background task.
> // taskName: A name for the background task.
> // trigger: The trigger for the background task.
> // condition: Optional parameter. A conditional event that must be true for the task to fire.
> //
> public static BackgroundTaskRegistration RegisterBackgroundTask(string taskEntryPoint,
>                                                                 string taskName,
>                                                                 IBackgroundTrigger trigger,
>                                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
>
>         if (cur.Value.Name == taskName)
>         {
>             //
>             // The task is already registered.
>             //
>
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>     
>     //
>     // Register the background task.
>     //
>
>     var builder = new BackgroundTaskBuilder();
>
>     builder.Name = taskName;
>     builder.TaskEntryPoint = taskEntryPoint;
>     builder.SetTrigger(trigger);
>
>     if (condition != null)
>     {
>
>         builder.AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration task = builder.Register();
>
>     return task;
> }
> ```
> ```cpp
> //
> // Register a background task with the specified taskEntryPoint, name, trigger,
> // and condition (optional).
> //
> // taskEntryPoint: Task entry point for the background task.
> // taskName: A name for the background task.
> // trigger: The trigger for the background task.
> // condition: Optional parameter. A conditional event that must be true for the task to fire.
> //
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(Platform::String ^ taskEntryPoint,
>                                                              Platform::String ^ taskName,
>                                                              IBackgroundTrigger ^ trigger,
>                                                              IBackgroundCondition ^ condition)
> {
>
>     //
>     // Check for existing registrations of this background task.
>     //
>     
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>     
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>         
>         if(cur->Name == name)
>         {
>             //
>             // The task is registered.
>             //
>             
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>         
>         hascur = iter->MoveNext();
>     }
>
>
>     //
>     // Register the background task.
>     //
>
>     auto builder = ref new BackgroundTaskBuilder();
>
>     builder->Name = name;
>     builder->TaskEntryPoint = taskEntryPoint;
>     builder->SetTrigger(trigger);
>
>     if (condition != nullptr) {
>         
>         builder->AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration ^ task = builder->Register();
>
>     return task;
> }
> ```

> Hilfsfunktion zur Registrierung der abgeschlossenen Hintergrundaufgabe Dieses Beispiel zeigt die Hilfsfunktion zur Registrierung der abgeschlossenen Hintergrundaufgabe.

 
## Diese Funktion kann zum Registrieren der meisten Hintergrundaufgaben mit Ausnahme von Hintergrundaufgaben im Netzwerk verwendet werden.


****

* [[!div class="tabbedCodeSnippets"]](create-and-register-a-background-task.md)
* [
            **Hinweis**  Dieser Artikel ist für Windows 10-Entwickler gedacht, die Apps für die Universelle Windows-Plattform (UWP) schreiben.](declare-background-tasks-in-the-application-manifest.md)
* [Wenn Sie für Windows8.x oder Windows Phone8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).](handle-a-cancelled-background-task.md)
* [Verwandte Themen](monitor-background-task-progress-and-completion.md)
* [Erstellen und Registrieren einer Hintergrundaufgabe](respond-to-system-events-with-background-tasks.md)
* [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](set-conditions-for-running-a-background-task.md)
* [Behandeln einer abgebrochenen Hintergrundaufgabe](update-a-live-tile-from-a-background-task.md)
* [Überwachen des Status und Abschlusses von Hintergrundaufgaben](use-a-maintenance-trigger.md)
* [Reagieren auf Systemereignisse mit Hintergrundaufgaben](run-a-background-task-on-a-timer-.md)
* [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](guidelines-for-background-tasks.md)

****

* [Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe](debug-a-background-task.md)
* [Verwenden eines Wartungsauslösers](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Jun16_HO5-->


