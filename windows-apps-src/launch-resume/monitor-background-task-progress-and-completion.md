---
author: TylerMSFT
title: "Überwachen des Status und Abschlusses von Hintergrundaufgaben"
description: Hier erfahren Sie, wie Ihre App den von einer Hintergrundaufgabe gemeldeten Status und Abschluss erkennt.
ms.assetid: 17544FD7-A336-4254-97DC-2BF8994FF9B2
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 07d69b63b272153bc784ed19166e649f80a98297

---

# Überwachen des Status und Abschlusses von Hintergrundaufgaben


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)
-   [**BackgroundTaskProgressEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224785)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

Hier erfahren Sie, wie Ihre App den von einer Hintergrundaufgabe gemeldeten Status und Abschluss erkennt. Hintergrundaufgaben sind zwar von der App entkoppelt und werden getrennt ausgeführt, Status und Abschluss können jedoch vom App-Code überwacht werden. Hierzu abonniert die App Ereignisse der Hintergrundaufgaben, die sie im System registriert hat.

-   In diesem Thema wird vorausgesetzt, dass Sie über eine App verfügen, die Hintergrundaufgaben registriert. Um schnell mit dem Erstellen einer Hintergrundaufgabe zu beginnen, lesen Sie die Infos unter [Erstellen und Registrieren einer Hintergrundaufgabe](create-and-register-a-background-task.md). Ausführlichere Informationen zu Bedingungen und Triggern finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

## Erstellen Sie einen Ereignishandler zum Behandeln abgeschlossener Hintergrundaufgaben.


1.  Erstellen Sie eine Ereignishandlerfunktion zum Behandeln abgeschlossener Hintergrundaufgaben. Dieser Code muss einem bestimmten Profil entsprechen und ein [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224803)-Objekt sowie ein [**BackgroundTaskCompletedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224778)-Objekt enthalten.

    Verwenden Sie das folgende Profil für die Ereignishandlermethode für die OnCompleted-Hintergrundaufgabe:

>  [!div class="tabbedCodeSnippets"]
>  ```cs
>  private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
>  {
>      // TODO: Add code that deals with background task completion.
>  }
>  ```
>  ```cpp
>  auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
>  {
>      // TODO: Add code that deals with background task completion.
>  };
>  ```

2.  Fügen Sie dem Ereignishandler Code zum Behandeln des Abschlusses der Hintergrundaufgabe hinzu.

    Das [Beispiel für eine Hintergrundaufgabe](http://go.microsoft.com/fwlink/p/?LinkId=618666) aktualisiert z. B. die Benutzeroberfläche.

    > [!div class="tabbedCodeSnippets"] ```cs
    >     private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
    >     {
    >         UpdateUI();
    >     }
    >     ```
    >     ```cpp
    >     auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >     {    
    >         UpdateUI();
    >     };
    >     ```

## Erstellen einer Ereignishandlerfunktion zum Behandeln des Hintergrundaufgabenfortschritts


1.  Erstellen Sie eine Ereignishandlerfunktion zum Behandeln abgeschlossener Hintergrundaufgaben. Dieser Code muss einem bestimmten Profil entsprechen und ein [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224803)-Objekt sowie ein [**BackgroundTaskProgressEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224782)-Objekt enthalten:

    Verwenden Sie das folgende Profil für die Ereignishandlermethode für die OnProgress-Hintergrundaufgabe:

    > [!div class="tabbedCodeSnippets"] ```cs
    >     private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
    >     {
    >         // TODO: Add code that deals with background task progress.
    >     }
    >     ```
    >     ```cpp
    >     auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    >     {
    >         // TODO: Add code that deals with background task progress.
    >     };
    >     ```

2.  Fügen Sie dem Ereignishandler Code zum Behandeln des Abschlusses der Hintergrundaufgabe hinzu.

    So aktualisiert beispielsweise das [Beispiel für eine Hintergrundaufgabe](http://go.microsoft.com/fwlink/p/?LinkId=618666) die Benutzeroberfläche mit dem Fortschrittsstatus, der mithilfe des *args*-Parameters übergeben wird:

    > [!div class="tabbedCodeSnippets"]     ```cs     private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)     {         var progress = "Progress: " + args.Progress + "%";         BackgroundTaskSample.SampleBackgroundTaskProgress = progress;
    >
    >         UpdateUI();
    >     }
    >     ```
    >     ```cpp
    >     auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    >     {
    >         auto progress = "Progress: " + args->Progress + "%";
    >         BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    >
    >         UpdateUI();
    >     };
    >     ```

## Registrieren Sie die Ereignishandlerfunktionen mit neuen und vorhandenen Hintergrundaufgaben.


1.  Wenn die App erstmals eine Hintergrundaufgabe registriert, muss sie sich für den Empfang von Status- und Abschlussupdates registrieren, falls die Aufgabe ausgeführt wird, während die App im Vordergrund noch aktiv ist.

    So ruft beispielsweise das [Beispiel für eine Hintergrundaufgabe](http://go.microsoft.com/fwlink/p/?LinkId=618666) für jede Hintergrundaufgabe, die es registriert, die folgende Funktion auf:

    > [!div class="tabbedCodeSnippets"] ```cs
    >     private void AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration task)
    >     {
    >         task.Progress += new BackgroundTaskProgressEventHandler(OnProgress);
    >         task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
    >     }
    >     ```
    >     ```cpp     void SampleBackgroundTask::AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration^ task)     {         auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    >                   {             auto progress = "Progress: " + args->Progress + "%";             BackgroundTaskSample::SampleBackgroundTaskProgress = progress;             UpdateUI();         };
    >
    >         task->Progress += ref new BackgroundTaskProgressEventHandler(progress);
    >         
    >
    >         auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >         {
    >             UpdateUI();
    >         };
    >
    >         task->Completed += ref new BackgroundTaskCompletedEventHandler(completed);
    >     }
    >     ```

2.  Wenn die App startet oder zu einer neuen Seite navigiert, für die der Hintergrundaufgabenstatus relevant ist, muss sie eine Liste mit den derzeit registrierten Hintergrundaufgaben abrufen und diese den Status- und Abschlussereignishandlerfunktionen zuordnen. Die Liste mit den derzeit von der Anwendung registrierten Hintergrundaufgaben ist in der [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786).[**AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787)-Eigenschaft gespeichert.

    So verwendet beispielsweise das [Beispiel für eine Hintergrundaufgabe](http://go.microsoft.com/fwlink/p/?LinkId=618666) den folgenden Code, um Ereignishandler anzufügen, wenn zur Seite „SampleBackgroundTask“ navigiert wird:

    > [!div class="tabbedCodeSnippets"]     ```cs     protected override void OnNavigatedTo(NavigationEventArgs e)     {         foreach (var task in BackgroundTaskRegistration.AllTasks)         {             if (task.Value.Name == BackgroundTaskSample.SampleBackgroundTaskName)             {                 AttachProgressAndCompletedHandlers(task.Value);                 BackgroundTaskSample.UpdateBackgroundTaskStatus(BackgroundTaskSample.SampleBackgroundTaskName, true);             }         }
    >
    >         UpdateUI();
    >     }
    >     ```
    >     ```cpp
    >     void SampleBackgroundTask::OnNavigatedTo(NavigationEventArgs^ e)
    >     {
    >         // A pointer back to the main page.  This is needed if you want to call methods in MainPage such
    >         // as NotifyUser()
    >         rootPage = MainPage::Current;
    >
    >         //
    >         // Attach progress and completed handlers to any existing tasks.
    >         //
    >         auto iter = BackgroundTaskRegistration::AllTasks->First();
    >         auto hascur = iter->HasCurrent;
    >         while (hascur)
    >         {
    >             auto cur = iter->Current->Value;
    >
    >             if (cur->Name == SampleBackgroundTaskName)
    >             {
    >                 AttachProgressAndCompletedHandlers(cur);
    >                 break;
    >             }
    >
    >             hascur = iter->MoveNext();
    >         }
    >
    >         UpdateUI();
    >     }
    >     ```

## Verwandte Themen


****

* [Erstellen und Registrieren einer Hintergrundaufgabe](create-and-register-a-background-task.md)
* [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md)
* [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
* [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
* [Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe](update-a-live-tile-from-a-background-task.md)
* [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)
* [Ausführen einer Hintergrundaufgabe für einen Timer](run-a-background-task-on-a-timer-.md)
* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)

****

* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [So wird's gemacht: Auslösen von Anhalte-, Fortsetzungs- und Hintergrundereignissen in Windows Store-Apps (beim Debuggen)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Jun16_HO4-->


