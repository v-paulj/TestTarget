---
title: Behandeln einer abgebrochenen Hintergrundaufgabe
description: Hier erfahren Sie, wie Sie eine Hintergrundaufgabe erstellen, die Abbruchanforderungen erkennt, die Ausführung beendet und den Abbruch mithilfe des beständigen Speichers an die App meldet.
ms.assetid: B7E23072-F7B0-4567-985B-737DD2A8728E
---

# Behandeln einer abgebrochenen Hintergrundaufgabe

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

**Wichtige APIs**

-   [**BackgroundTaskCanceledEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224775)
-   [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797)
-   [**ApplicationData.Current**](https://msdn.microsoft.com/library/windows/apps/br241619)

Hier erfahren Sie, wie Sie eine Hintergrundaufgabe erstellen, die Abbruchanforderungen erkennt, die Ausführung beendet und den Abbruch mithilfe des beständigen Speichers an die App meldet.

> **Hinweis**  Für alle Gerätefamilien mit Ausnahme von Desktops können Hintergrundaufgaben beendet werden, wenn der Arbeitsspeicher des Geräts knapp wird. Wenn eine Ausnahme über wenig Arbeitsspeicher nicht angezeigt oder von der App nicht behandelt wird, wird die Hintergrundaufgabe ohne Warnung und ohne Auslösen des OnCanceled-Ereignisses beendet. Dadurch soll die Benutzerfreundlichkeit der App im Vordergrund sichergestellt werden. Entwerfen Sie die Hintergrundaufgabe so, dass dieses Szenario behandelt wird.

Dieses Thema setzt voraus, dass Sie bereits eine Hintergrundaufgabenklasse erstellt haben, einschließlich der Run-Methode, die für die Hintergrundaufgabe als Einstiegspunkt verwendet wird. Um schnell mit dem Erstellen einer Hintergrundaufgabe zu beginnen, lesen Sie die Infos unter [Erstellen und Registrieren einer Hintergrundaufgabe](create-and-register-a-background-task.md). Ausführlichere Informationen zu Bedingungen und Triggern finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

## Verwenden der Methode "OnCanceled" zum Erkennen von Abbruchanforderungen

Schreiben Sie eine Methode für die Behandlung des Abbruchereignisses.

Erstellen Sie eine Methode mit dem Namen "OnCanceled" und der folgenden Syntax. Diese Methode ist der Einstiegspunkt, der von der Windows-Runtime aufgerufen wird, wenn eine Abbruchanforderung für die Hintergrundaufgabe erfolgt.

Die OnCanceled-Methode muss wie folgt aussehen:

> [!div class="tabbedCodeSnippets"]
> ```cs
>    private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
>    {
>        // TODO: Add code to notify the background task that it is cancelled.
>    }
> ```
> ```cpp
>    void ExampleBackgroundTask::OnCanceled(IBackgroundTaskInstance^ taskInstance, BackgroundTaskCancellationReason reason)
>    {
>        // TODO: Add code to notify the background task that it is cancelled.
>    }
> ```

Fügen Sie der Hintergrundaufgabenklasse eine Kennzeichenvariable mit dem Namen **\_CancelRequested** hinzu. Diese Variable wird verwendet, um anzuzeigen, ob eine Abbruchanforderung erfolgt ist.

> [!div class="tabbedCodeSnippets"]
> ```cs
>   volatile bool _CancelRequested = false;
> ```
> ```cpp
>   private:
>     volatile bool CancelRequested;
> ```

Legen Sie in der OnCanceled-Methode, die Sie in Schritt 1 erstellt haben, die Kennzeichenvariable **\_CancelRequested** auf **true** fest.

Die vollständige OnCanceled-Methode des [Beispiels zur Hintergrundaufgabe]( http://go.microsoft.com/fwlink/p/?linkid=227509) legt **\_CancelRequested** auf **true** fest und gibt eine möglicherweise hilfreiche Debugmeldung aus:

> [!div class="tabbedCodeSnippets"]
> ```cs
>     private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
>     {
>         //
>         // Indicate that the background task is canceled.
>         //
> 
>         _cancelRequested = true;
> 
>         Debug.WriteLine("Background " + sender.Task.Name + " Cancel Requested...");
>     }
> ```
> ```cpp
>     void SampleBackgroundTask::OnCanceled(IBackgroundTaskInstance^ taskInstance, BackgroundTaskCancellationReason reason)
>     {
>         //
>         // Indicate that the background task is canceled.
>         //
> 
>         CancelRequested = true;
>     }
> ```

Registrieren Sie in der Run-Methode der Hintergrundaufgabe die OnCanceled-Ereignishandlermethode, bevor Sie mit der Arbeit beginnen. Verwenden Sie z. B. folgende Codezeile:

> [!div class="tabbedCodeSnippets"]
> ```cs
>     taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);
> ```
> ```cpp
>     taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &SampleBackgroundTask::OnCanceled);
> ```

## Behandeln des Abbruchs durch Verlassen der Run-Methode


Wenn eine Abbruchanforderung empfangen wird, muss die Run-Methode angehalten und verlassen werden, indem erkannt wird, dass **\_cancelRequested** auf **true** festgelegt wurde.

Ändern Sie den Code der Hintergrundaufgabenklasse, um die Kennzeichenvariable zu überprüfen, während die Hintergrundaufgabe ausgeführt wird. Wenn **\_cancelRequested** auf „true“ festgelegt ist, halten Sie die Fortsetzung der Arbeit an.

Das [Beispiel zur Hintergrundaufgabe](http://go.microsoft.com/fwlink/p/?LinkId=618666) enthält eine Überprüfung, die den regelmäßigen Zeitgeberrückruf anhält, wenn die Hintergrundaufgabe abgebrochen wird:

> [!div class="tabbedCodeSnippets"]
> ```cs
>     if ((_cancelRequested == false) &amp;&amp; (_progress &lt; 100))
>     {
>         _progress += 10;
>         _taskInstance.Progress = _progress;
>     }
>     else
>     {
>         _periodicTimer.Cancel();
> 
>         // TODO: Record whether the task completed or was cancelled.
>     }
> ```
> ```cpp
>     if ((CancelRequested == false) &amp;&amp; (Progress &lt; 100))
>     {
>         Progress += 10;
>         TaskInstance->Progress = Progress;
>     }
>     else
>     {
>         PeriodicTimer->Cancel();
> 
>         // TODO: Record whether the task completed or was cancelled.
>     }
> ```

> **Hinweis**  Das oben dargestellte Codebeispiel verwendet die Eigenschaft [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797).[**Progress**](https://msdn.microsoft.com/library/windows/apps/br224800), mit der der Fortschritt der Hintergrundaufgabe aufgezeichnet wird. Der Fortschritt wird der App mithilfe der [**BackgroundTaskProgressEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224782)-Klasse gemeldet.

Ändern Sie die Run-Methode, damit sie nach dem Anhalten der Arbeit aufzeichnet, ob die Aufgabe ausgeführt oder abgebrochen wurde.

Das [Beispiel zur Hintergrundaufgabe](http://go.microsoft.com/fwlink/p/?LinkId=618666) zeichnet den Status in „LocalSettings“ auf:

> [!div class="tabbedCodeSnippets"]
> ```cs
>     if ((_cancelRequested == false) &amp;&amp; (_progress &lt; 100))
>     {
>         _progress += 10;
>         _taskInstance.Progress = _progress;
>     }
>     else
>     {
>         _periodicTimer.Cancel();
> 
>         var settings = ApplicationData.Current.LocalSettings;
>         var key = _taskInstance.Task.TaskId.ToString();
> 
>         //
>         // Write to LocalSettings to indicate that this background task ran.
>         //
> 
>         if (_cancelRequested)
>         {
>             settings.Values[key] = "Canceled";
>         }
>         else
>         {
>             settings.Values[key] = "Completed";
>         }
>         
>         Debug.WriteLine("Background " + _taskInstance.Task.Name + (_cancelRequested ? " Canceled" : " Completed"));
>         
>         //
>         // Indicate that the background task has completed.
>         //
> 
>         _deferral.Complete();
>     }
> ```
> ```cpp
>     if ((CancelRequested == false) &amp;&amp; (Progress &lt; 100))
>     {
>         Progress += 10;
>         TaskInstance->Progress = Progress;
>     }
>     else
>     {
>         PeriodicTimer->Cancel();
>         
>         //
>         // Write to LocalSettings to indicate that this background task ran.
>         //
>         
>         auto settings = ApplicationData::Current->LocalSettings;
>         auto key = TaskInstance->Task->Name;
>         settings->Values->Insert(key, (Progress &lt; 100) ? "Canceled" : "Completed");
>         
>         //
>         // Indicate that the background task has completed.
>         //
>         
>         Deferral->Complete();
>     }
> ```

## Anmerkungen

Sie können das [Beispiel zur Hintergrundaufgabe](http://go.microsoft.com/fwlink/p/?LinkId=618666) herunterladen, um diese Codebeispiele im Kontext von Methoden anzuzeigen.

Zu Demonstrationszwecken enthält der Beispielcode nur Teile der Run-Methode (und des Rückruftimers) aus dem [Beispiel zur Hintergrundaufgabe](http://go.microsoft.com/fwlink/p/?LinkId=618666).

## Beispiel der Run-Methode

Im Folgenden sind die vollständige Run-Methode und der Timerrückruf-Code aus dem [Beispiel zur Hintergrundaufgabe](http://go.microsoft.com/fwlink/p/?LinkId=618666) aufgeführt:

> [!div class="tabbedCodeSnippets"]
> ```cs
> //
> // The Run method is the entry point of a background task.
> //
> public void Run(IBackgroundTaskInstance taskInstance)
> {
>     Debug.WriteLine("Background " + taskInstance.Task.Name + " Starting...");
> 
>     //
>     // Query BackgroundWorkCost
>     // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
>     // of work in the background task and return immediately.
>     //
>     var cost = BackgroundWorkCost.CurrentBackgroundWorkCost;
>     var settings = ApplicationData.Current.LocalSettings;
>     settings.Values["BackgroundWorkCost"] = cost.ToString();
> 
>     //
>     // Associate a cancellation handler with the background task.
>     //
>     taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);
> 
>     //
>     // Get the deferral object from the task instance, and take a reference to the taskInstance;
>     //
>     _deferral = taskInstance.GetDeferral();
>     _taskInstance = taskInstance;
> 
>     _periodicTimer = ThreadPoolTimer.CreatePeriodicTimer(new TimerElapsedHandler(PeriodicTimerCallback), TimeSpan.FromSeconds(1));
> }
> 
> //
> // Simulate the background task activity.
> //
> private void PeriodicTimerCallback(ThreadPoolTimer timer)
> {
>     if ((_cancelRequested == false) &amp;&amp; (_progress < 100))
>     {
>         _progress += 10;
>         _taskInstance.Progress = _progress;
>     }
>     else
>     {
>         _periodicTimer.Cancel();
> 
>         var settings = ApplicationData.Current.LocalSettings;
>         var key = _taskInstance.Task.Name;
> 
>         //
>         // Write to LocalSettings to indicate that this background task ran.
>         //
>         settings.Values[key] = (_progress < 100) ? "Canceled with reason: " + _cancelReason.ToString() : "Completed";
>         Debug.WriteLine("Background " + _taskInstance.Task.Name + settings.Values[key]);
> 
>         //
>         // Indicate that the background task has completed.
>         //
>         _deferral.Complete();
>     }
> }
> ```
> ```cpp
> void SampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
> {
>     //
>     // Query BackgroundWorkCost
>     // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
>     // of work in the background task and return immediately.
>     //
>     auto cost = BackgroundWorkCost::CurrentBackgroundWorkCost;
>     auto settings = ApplicationData::Current->LocalSettings;
>     settings->Values->Insert("BackgroundWorkCost", cost.ToString());
> 
>     //
>     // Associate a cancellation handler with the background task.
>     //
>     taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &amp;SampleBackgroundTask::OnCanceled);
> 
>     //
>     // Get the deferral object from the task instance, and take a reference to the taskInstance.
>     //
>     TaskDeferral = taskInstance->GetDeferral();
>     TaskInstance = taskInstance;
> 
>     auto timerDelegate = [this](ThreadPoolTimer^ timer)
>     {
>         if ((CancelRequested == false) &amp;&amp;
>             (Progress < 100))
>         {
>             Progress += 10;
>             TaskInstance->Progress = Progress;
>         }
>         else
>         {
>             PeriodicTimer->Cancel();
> 
>             //
>             // Write to LocalSettings to indicate that this background task ran.
>             //
>             auto settings = ApplicationData::Current->LocalSettings;
>             auto key = TaskInstance->Task->Name;
>             settings->Values->Insert(key, (Progress < 100) ? "Canceled with reason: " + CancelReason.ToString() : "Completed");
> 
>             //
>             // Indicate that the background task has completed.
>             //
>             TaskDeferral->Complete();
>         }
>     };
> 
>     TimeSpan period;
>     period.Duration = 1000 * 10000; // 1 second
>     PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(ref new TimerElapsedHandler(timerDelegate), period);
> }
> ```

> **Hinweis**  Dieser Artikel ist für Windows 10-Entwickler bestimmt, die UWP-Apps (Universelle Windows-Plattform) schreiben. Wenn Sie für Windows 8.x oder Windows Phone 8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

## Verwandte Themen

* [Erstellen und Registrieren einer Hintergrundaufgabe](create-and-register-a-background-task.md)
* [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md)
* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
* [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
* [Ausführen einer Hintergrundaufgabe für einen Timer](run-a-background-task-on-a-timer-.md)
* [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
* [Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe](update-a-live-tile-from-a-background-task.md)
* [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)

* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [So wird's gemacht: Auslösen von Anhalte-, Fortsetzungs- und Hintergrundereignissen in Windows Store-Apps (beim Debuggen)](http://go.microsoft.com/fwlink/p/?linkid=254345)





<!--HONumber=Mar16_HO1-->

