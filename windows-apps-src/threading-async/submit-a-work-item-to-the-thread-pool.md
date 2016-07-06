---
author: TylerMSFT
ms.assetid: E2A1200C-9583-40FA-AE4D-C9E6F6C32BCF
title: Senden einer Arbeitsaufgabe an den Threadpool
description: "Hier erfahren Sie, wie Sie Arbeit in einem separaten Thread erledigen können, indem Sie eine Arbeitsaufgabe an den Threadpool übermitteln."
translationtype: Human Translation
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: afb6d8b1b1ee5eeb99ba68e8b842436bd58619d0

---
# Senden einer Arbeitsaufgabe an den Threadpool

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** Wichtige APIs **

-   [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/BR230593)
-   [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/BR206580)

Hier erfahren Sie, wie Sie Arbeit in einem separaten Thread erledigen können, indem Sie eine Arbeitsaufgabe an den Threadpool übermitteln. Somit sorgen Sie dafür, dass die Benutzeroberfläche bei der Erledigung von Arbeit, die viel Zeit in Anspruch nimmt, reaktionsfähig bleibt, und Sie können mehrere Aufgaben parallel bearbeiten.

## Erstellen und Senden der Arbeitsaufgabe

Erstellen Sie eine Arbeitsaufgabe, indem Sie [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/BR230593) aufrufen. Stellen Sie einen Delegaten zur Durchführung der Arbeit bereit (Sie können eine Lambda-Funktion oder Delegatfunktion verwenden). **RunAsync** gibt ein [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/BR206580)-Objekt zurück. Speichern Sie dieses Objekt, da es im nächsten Schritt verwendet wird.

Drei Versionen von [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/BR230593) sind verfügbar. Damit können Sie optional die Priorität der Arbeitsaufgabe angeben und steuern, ob sie gleichzeitig mit anderen Arbeitsaufgaben ausgeführt wird.

**Hinweis**  Verwenden Sie [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317), um auf den Benutzeroberflächenthread zuzugreifen und den Fortschritt der Arbeitsaufgabe anzuzeigen.

Im folgenden Beispiel werden eine Arbeitsaufgabe erstellt und ein Lambda für die Verarbeitung angegeben:

> [!div class="tabbedCodeSnippets"]
``` cpp
// The nth prime number to find.
const unsigned int n = 9999;

// A shared pointer to the result.
// We use a shared pointer to keep the result alive until the 
// thread is done.
std::shared_ptr<unsigned long> nthPrime = make_shared<unsigned long int>(0);

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.
auto workItem = ref new WorkItemHandler(
    \[this, n, nthPrime](IAsyncAction^ workItem)
{
    unsigned int progress = 0; // For progress reporting.
    unsigned int primes = 0;   // Number of primes found so far.
    unsigned long int i = 2;   // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        *nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem->Status == AsyncStatus::Canceled)
        {
            break;
        }

        // Go to the next number.
        i++;

        // Check for prime.
        bool prime = true;
        for (unsigned int j = 2; j < i; ++j)
        {
            if ((i % j) == 0)
            {
                prime = false;
                break;
            }
        };

        if (prime)
        {
            // Found another prime number.
            primes++;

            // Report progress at every 10 percent.
            unsigned int temp = progress;
            progress = static_cast<unsigned int>(10.f*primes / n);

            if (progress != temp)
            {
                String^ updateString;
                updateString = "Progress to " + n + "th prime: "
                    + (10 * progress).ToString() + "%\n";

                // Update the UI thread with the CoreDispatcher.
                CoreApplication::MainView->CoreWindow->Dispatcher->RunAsync(
                    CoreDispatcherPriority::High,
                    ref new DispatchedHandler([this, updateString]()
                {
                    UpdateUI(updateString);
                }));
            }
        }
    }

    // Return the nth prime number.
    *nthPrime = i;
});

auto asyncAction = ThreadPool::RunAsync(workItem);

// A reference to the work item is cached so that we can trigger a 
// cancellation when the user presses the Cancel button.
m_workItem = asyncAction;
```
``` csharp
// The nth prime number to find.
const uint n = 9999;

// A shared pointer to the result.
// We use a shared pointer to keep the result alive until the 
// thread is done.
ulong nthPrime = 0;

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.
IAsyncAction asyncAction = Windows.System.Threading.ThreadPool.RunAsync(
    (workItem) =>
{
    uint  progress = 0; // For progress reporting.
    uint  primes = 0;   // Number of primes found so far.
    ulong i = 2;        // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem.Status == AsyncStatus.Canceled)
        {
            break;
        }

        // Go to the next number.
        i++;

        // Check for prime.
        bool prime = true;
        for (uint j = 2; j < i; ++j)
        {
            if ((i % j) == 0)
            {
                prime = false;
                break;
            }
        };

        if (prime)
        {
            // Found another prime number.
            primes++;

            // Report progress at every 10 percent.
            uint temp = progress;
            progress = (uint)(10.0*primes/n);

            if (progress != temp)
            {
                String updateString;
                updateString = "Progress to " + n + "th prime: "
                    + (10 * progress) + "%\n";

                // Update the UI thread with the CoreDispatcher.
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
                    CoreDispatcherPriority.High,
                    new DispatchedHandler(() =>
                {
                    UpdateUI(updateString);
                }));
            }
        }
    }

    // Return the nth prime number.
    nthPrime = i;
});

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = asyncAction;
```

Nach dem Aufruf von [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/BR230593) wird die Arbeitsaufgabe vom Threadpool in eine Warteschlange eingereiht und ausgeführt, wenn ein Thread verfügbar wird. Arbeitsaufgaben im Threadpool werden asynchron und in einer beliebigen Reihenfolge ausgeführt. Stellen Sie daher sicher, dass Ihre Arbeitsaufgaben über eine unabhängige Funktionsweise funktionieren.

Beachten Sie, dass von der Arbeitsaufgabe die [**IAsyncInfo.Status**](https://msdn.microsoft.com/library/windows/apps/BR206593)-Eigenschaft überprüft und bei einem Abbruch der Arbeitsaufgabe beendet wird.

## Behandeln der Vervollständigung der Arbeitsaufgabe

Stellen Sie einen Vervollständigungshandler zur Verfügung, indem Sie die [**IAsyncAction.Completed**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.foundation.iasyncaction.completed.aspx)-Eigenschaft der Arbeitsaufgabe festlegen. Geben Sie einen Delegaten an (Sie können eine Lambda-Funktion oder Delegatfunktion nutzen), mit dem die Arbeitsaufgabe durchgeführt wird. Verwenden Sie z. B. [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317), um auf den Benutzeroberflächenthread zuzugreifen und das Ergebnis anzuzeigen.

Im folgenden Beispiel wird die Benutzeroberfläche mit dem Ergebnis der in Schritt 1 übermittelten Arbeitsaufgabe aktualisiert:

> [!div class="tabbedCodeSnippets"]
``` cpp
asyncAction->Completed = ref new AsyncActionCompletedHandler(
    \[this, n, nthPrime](IAsyncAction^ asyncInfo, AsyncStatus asyncStatus)
{
    if (asyncStatus == AsyncStatus::Canceled)
    {
        return;
    }
    
    String^ updateString;
    updateString = "\n" + "The " + n + "th prime number is " 
        + (*nthPrime).ToString() + ".\n";

    // Update the UI thread with the CoreDispatcher.
    CoreApplication::MainView->CoreWindow->Dispatcher->RunAsync(
        CoreDispatcherPriority::High,
        ref new DispatchedHandler([this, updateString]()
    {
        UpdateUI(updateString);
    }));
});
```
``` csharp
asyncAction.Completed = new AsyncActionCompletedHandler(
    (IAsyncAction asyncInfo, AsyncStatus asyncStatus) =>
{
    if (asyncStatus == AsyncStatus.Canceled)
    {
        return;
    }

    String updateString;
    updateString = "\n" + "The " + n + "th prime number is " 
        + nthPrime + ".\n";

    // Update the UI thread with the CoreDispatcher.
    CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
        CoreDispatcherPriority.High,
        new DispatchedHandler(()=>
    {
        UpdateUI(updateString);
    }));
});
```

Beachten Sie, dass vom Vervollständigungshandler überprüft wird, ob die Arbeitsaufgabe abgebrochen wurde, bevor eine Aktualisierung der Benutzeroberfläche bereitgestellt wird.

## Zusammenfassung und nächste Schritte

Weitere Informationen erhalten Sie durch das Herunterladen des Codes in dieser Schnellstartanleitung unter [Beispiel für das Erstellen einer Threadpool-Arbeitsaufgabe](http://go.microsoft.com/fwlink/p/?LinkID=328569) für Windows 8.1 und die erneute Verwendung des Quellcodes in einer win\_unap Windows 10-App.

## Verwandte Themen

* [Senden einer Arbeitsaufgabe an den Threadpool](submit-a-work-item-to-the-thread-pool.md)
* [Bewährte Methoden zum Verwenden des Threadpools](best-practices-for-using-the-thread-pool.md)
* [Senden einer Arbeitsaufgabe mithilfe eines Timers](use-a-timer-to-submit-a-work-item.md)
 




<!--HONumber=Jun16_HO4-->


