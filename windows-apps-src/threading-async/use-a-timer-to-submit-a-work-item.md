---
author: TylerMSFT
ms.assetid: AAE467F9-B3C7-4366-99A2-8A880E5692BE
title: Senden einer Arbeitsaufgabe mithilfe eines Timers
description: "Hier erfahren Sie, wie Sie eine Arbeitsaufgabe erstellen, die nach dem Ablaufen eines Timers ausgeführt wird."
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: 033669a781aa85cc2c90fa11816e385ffefa997d

---
# Übermitteln einer Arbeitsaufgabe mithilfe eines Timers

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** Wichtige APIs **

-   [**Windows.UI.Core-Namespace**](https://msdn.microsoft.com/library/windows/apps/BR208383)
-   [**Windows.System.Threading-Namespace**](https://msdn.microsoft.com/library/windows/apps/BR229642)

Hier erfahren Sie, wie Sie eine Arbeitsaufgabe erstellen, die nach dem Ablaufen eines Timers ausgeführt wird.

## Erstellen eines einmaligen Timers

Verwenden Sie die [**CreateTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967921)-Methode, um einen Timer für die Arbeitsaufgabe zu erstellen. Stellen Sie eine Lambda-Funktion zum Ausführen der Arbeit bereit, und geben Sie mit dem *delay*-Parameter an, wie lange der Threadpool warten soll, bevor er die Arbeitsaufgabe einem verfügbaren Thread zuweist. Die Verzögerung wird mithilfe einer [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/BR225996)-Struktur angegeben.

> 
            **Hinweis:**  Sie können [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317) verwenden, um auf die Benutzeroberfläche zuzugreifen und den Status der Arbeitsaufgabe anzuzeigen.

Das folgende Beispiel erstellt eine Arbeitsaufgabe, die in drei Minuten ausgeführt wird:

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> TimeSpan delay = TimeSpan.FromMinutes(3);
>             
> ThreadPoolTimer DelayTimer = ThreadPoolTimer.CreateTimer(
>     (source) =>
>     {
>         // 
>         // TODO: Work
>         // 
>         
>         // 
>         // Update the UI thread by using the UI core dispatcher.
>         // 
>         Dispatcher.RunAsync(
>             CoreDispatcherPriority.High,
>             () =>
>             {
>                 // 
>                 // UI components can be accessed within this scope.
>                 // 
> 
>             });
> 
>     }, delay);
> ```
> ``` cpp
> TimeSpan delay;
> delay.Duration = 3 * 60 * 10000000; // 10,000,000 ticks per second
> 
> ThreadPoolTimer ^ DelayTimer = ThreadPoolTimer::CreateTimer(
>         ref new TimerElapsedHandler([this](ThreadPoolTimer^ source)
>         {
>             // 
>             // TODO: Work
>             // 
>             
>             // 
>             // Update the UI thread by using the UI core dispatcher.
>             // 
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([this]()
>                 {
>                     // 
>                     // UI components can be accessed within this scope.
>                     // 
> 
>                     ExampleUIUpdateMethod("Timer completed.");
> 
>                 }));
> 
>         }), delay);
> ```

## [!div class="tabbedCodeSnippets"]

Bereitstellen eines Abschlusshandlers Behandeln Sie den Abbruch und Abschluss der Arbeitsaufgabe ggf. mit einem [**TimerDestroyedHandler**](https://msdn.microsoft.com/library/windows/apps/Hh967926)-Element. Stellen Sie mithilfe der [**CreateTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967921)-Überladung eine zusätzliche Lambda-Funktion bereit.

Diese Funktion wird ausgeführt, wenn der Timer abgebrochen oder die Arbeitsaufgabe abgeschlossen wird.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> TimeSpan delay = TimeSpan.FromMinutes(3);
>             
> bool completed = false;
> 
> ThreadPoolTimer DelayTimer = ThreadPoolTimer.CreateTimer(
>     (source) =>
>     {
>         // 
>         // TODO: Work
>         // 
> 
>         // 
>         // Update the UI thread by using the UI core dispatcher.
>         // 
>         Dispatcher.RunAsync(
>                 CoreDispatcherPriority.High,
>                 () =>
>                 {
>                     // 
>                     // UI components can be accessed within this scope.
>                     // 
> 
>                 });
> 
>         completed = true;
>     },
>     delay,
>     (source) =>
>     {
>         // 
>         // TODO: Handle work cancellation/completion.
>         // 
> 
> 
>         // 
>         // Update the UI thread by using the UI core dispatcher.
>         // 
>         Dispatcher.RunAsync(
>             CoreDispatcherPriority.High,
>             () =>
>             {
>                 // 
>                 // UI components can be accessed within this scope.
>                 // 
> 
>                 if (completed)
>                 {
>                     // Timer completed.
>                 }
>                 else
>                 {
>                     // Timer cancelled.
>                 }
> 
>             });
>     });
> ```
> ``` cpp
> TimeSpan delay;
> delay.Duration = 3 * 60 * 10000000; // 10,000,000 ticks per second
> 
> completed = false;
> 
> ThreadPoolTimer ^ DelayTimer = ThreadPoolTimer::CreateTimer(
>         ref new TimerElapsedHandler([&](ThreadPoolTimer ^ source)
>         {
>             // 
>             // TODO: Work
>             // 
> 
>             // 
>             // Update the UI thread by using the UI core dispatcher.
>             // 
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     // 
>                     // UI components can be accessed within this scope.
>                     // 
> 
>                 }));
> 
>             completed = true;
> 
>         }),
>         delay,
>         ref new TimerDestroyedHandler([&](ThreadPoolTimer ^ source)
>         {
>             // 
>             // TODO: Handle work cancellation/completion.
>             // 
> 
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     // 
>                     // Update the UI thread by using the UI core dispatcher.
>                     // 
> 
>                     if (completed)
>                     {
>                         // Timer completed.
>                     }
>                     else
>                     {
>                         // Timer cancelled.
>                     }
> 
>                 }));
>         }));
> ```

## Das folgende Beispiel erstellt einen Timer, der die Arbeitsaufgabe übermittelt, und ruft eine Methode auf, wenn die Arbeitsaufgabe abgeschlossen oder der Timer abgebrochen wird:

[!div class="tabbedCodeSnippets"] Abbrechen des Timers

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> DelayTimer.Cancel();
> ```
> ``` cpp
> DelayTimer->Cancel();
> ```

## Wenn der Timer weiter läuft, die Arbeitsaufgabe aber nicht mehr benötigt wird, rufen Sie [**Cancel**](https://msdn.microsoft.com/library/windows/apps/BR230588) auf.

Der Timer wird abgebrochen, und die Arbeitsaufgabe wird nicht an den Threadpool übermittelt. [!div class="tabbedCodeSnippets"]

Hinweise UWP-Apps können **Thread.Sleep** nicht verwenden, da dies den UI-Thread blockieren kann.

Verwenden Sie zum Erstellen einer Arbeitsaufgabe stattdessen einen [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/BR230587). Dieser Timer verzögert die von der Arbeitsaufgabe ausgeführte Aufgabe, ohne den UI-Thread zu blockieren.

## Ein vollständiges Codebeispiel für Arbeitsaufgaben, Arbeitsaufgaben mit Zeitgeber und regelmäßige Arbeitsaufgaben finden Sie im [Beispiel für den Threadpool](http://go.microsoft.com/fwlink/p/?linkid=255387).

* [Das Codebeispiel wurde ursprünglich für Windows8.1 geschrieben, der Code kann jedoch für Windows10 wiederverwendet werden.](submit-a-work-item-to-the-thread-pool.md)
* [Informationen zu Wiederholungstimern finden Sie unter [Erstellen einer regelmäßigen Arbeitsaufgabe](create-a-periodic-work-item.md).](best-practices-for-using-the-thread-pool.md)
* [Verwandte Themen](use-a-timer-to-submit-a-work-item.md)
 

 




<!--HONumber=Jun16_HO4-->


