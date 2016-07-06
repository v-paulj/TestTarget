---
author: mcleblanc
ms.assetid: FA25562A-FE62-4DFC-9084-6BD6EAD73636
title: "Aufrechterhalten der Reaktionsfähigkeit des UI-Threads"
description: "Benutzer erwarten, dass eine App beim Durchführen einer Berechnung reaktionsfähig bleibt, unabhängig vom jeweiligen Computertyp."
translationtype: Human Translation
ms.sourcegitcommit: 165105c141405cd752f876c822f76a5002d38678
ms.openlocfilehash: 6144b5b60a0092efd1056dd5de166a64733356ec

---
# Aufrechterhalten der Reaktionsfähigkeit des UI-Threads

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Benutzer erwarten, dass eine App beim Durchführen einer Berechnung reaktionsfähig bleibt, unabhängig vom jeweiligen Computertyp. Das bedeutet für jede App etwas anderes. Für einige Apps bedeutet dies z. B. Folgendes: realistischere physische Effekte bereitstellen, Daten vom Datenträger oder aus dem Internet schneller laden, komplexe Szenen schnell darstellen, schnell zwischen Seiten navigieren, Anweisungen im Nu finden oder schnelles Verarbeiten von Daten. Unabhängig von der Art der Berechnung möchten Benutzer, dass die App auf ihre Eingabe reagiert. Es stört sie, wenn die App scheinbar nicht reagiert, während sie "denkt".

Ihre App ist ereignisgesteuert, was bedeutet, dass Ihr Code in Reaktion auf ein Ereignis eine Funktion erfüllt und anschließend befindet sich die App im Leerlauf und wartet auf das nächste Ereignis. Plattformcode für die Benutzeroberfläche (Layout, Eingabe, Auslösen von Ereignissen usw.) und Ihr App-Code für die Benutzeroberfläche werden alle im gleichen UI-Thread ausgeführt. Nur eine Anweisung kann jeweils für diesen Thread ausgeführt werden, wenn Ihr App-Code also zu lange für das Verarbeiten eines Ereignisses benötigt, kann das Framework das Layout nicht ausführen oder neue Ereignisse auslösen, die Interaktionen des Benutzers darstellen. Das heißt, die Reaktionsfähigkeit Ihrer App hängt direkt davon ab, ob der UI-Thread zum Durchführen von Arbeit verfügbar ist.

Sie müssen den UI-Thread verwenden, um fast alle Änderungen am UI-Thread vorzunehmen, einschließlich der Erstellung von UI-Typen und des Zugriffs auf ihre Member. Sie können die UI nicht aus einem Hintergrundthread aktualisieren, können jedoch mit [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317) eine Nachricht posten, damit der Code dort ausgeführt wird.

> **Hinweis**  Die einzige Ausnahme besteht darin, dass es einen separaten Renderthread gibt, der UI-Änderungen ohne Auswirkungen auf die Eingabeverarbeitung oder auf das Basislayout anwenden kann. Viele Animationen und Übergänge, die sich nicht auf das Layout auswirken, können z. B. auf diesem Renderthread ausgeführt werden.

## Verzögern der Element-Instanziierung

Einige der langsamsten Phasen in einer App können der Start und das Wechseln zwischen Ansichten sein. Machen Sie sich nicht mehr Arbeit als nötig, um die Benutzeroberfläche anzuzeigen, die dem Benutzer zuerst angezeigt wird. Erstellen Sie beispielsweise nicht die Benutzeroberfläche für die schrittweise offengelegte Benutzeroberfläche und den Inhalt von Popups.

-   Verwenden Sie [x:DeferLoadStrategy](https://msdn.microsoft.com/library/windows/apps/Mt204785) zum Verzögern instanziierter Elemente.
-   Fügen Sie programmgesteuerte Elemente nach Bedarf in der Struktur ein.

[
              **CoreDispatcher.RunIdleAsync**
            ](https://msdn.microsoft.com/library/windows/apps/Hh967918)-Warteschlangen eignen sich für die Verarbeitung des UI-Threads, wenn er nicht aktiv ist.

## Verwenden von asynchronen APIs

Zur Verbesserung der Reaktionsfähigkeit der App stellt die Plattform asynchrone Versionen vieler APIs bereit. Eine asynchrone API stellt sicher, dass Ihr aktiver Ausführungsthread nie für längere Zeit blockiert wird. Wenn Sie ein API-Element aus dem UI-Thread heraus aufrufen, verwenden Sie die asynchrone Version, sofern diese verfügbar ist. Weitere Informationen zum Programmieren mit **async**-Strukturen finden Sie unter [Asynchrone Programmierung](https://msdn.microsoft.com/library/windows/apps/Mt187335) oder unter [Aufrufen asynchroner APIs in C# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/Mt187337).

## Auslagern der Arbeit an Hintergrundthreads

Schreiben Sie Ereignishandler, die schnell zurückgegeben werden. In Fällen, in denen umfassende Arbeiten vorgenommen werden müssen, sollten Sie sie in einem Hintergrundthread planen und zurückgeben.

Mit dem Operator **await** in C#, dem Operator **Await** in Visual Basic oder mit Delegaten in C++ können Sie Arbeit auch asynchron planen. Dies garantiert jedoch nicht, dass die geplante Arbeit in einem Hintergrundthread ausgeführt wird. Viele der UWP-APIs planen für Sie die Arbeit im Hintergrundthread, wenn Sie Ihren App-Code jedoch nur mit **await** oder einem Delegaten aufrufen, führen Sie diesen Delegaten oder diese Methode in einem UI-Thread aus. Wenn Sie Ihren App-Code in einem Hintergrundthread ausführen möchten, müssen Sie dies explizit angeben. In C# und Visual Basic können Sie dies erreichen, indem Sie Code an [**Task.Run**](https://msdn.microsoft.com/library/windows/apps/xaml/system.threading.tasks.task.run.aspx) übergeben.

Denken Sie daran, dass nur aus dem UI-Thread auf UI-Elemente zugegriffen werden kann. Verwenden Sie den UI-Thread für den Zugriff auf UI-Elemente vor dem Starten der Hintergrundverarbeitung und/oder der Verwendung von [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317) oder [**CoreDispatcher.RunIdleAsync**](https://msdn.microsoft.com/library/windows/apps/Hh967918) im Hintergrundthread.

Ein Beispiel für Arbeit, die in einem Hintergrundthread ausgeführt werden kann, ist die Berechnung der Computer-KI in einem Spiel. Die Ausführung des Codes zur Berechnung der nächsten Aktion des Computers nimmt viel Zeit in Anspruch.

```csharp
public class AsyncExample
{
    private async void NextMove-Click(object sender, RoutedEventArgs e)
    {
        // The await causes the handler to return immediately.
        await System.Threading.Tasks.Task.Run(() => ComputeNextMove());
        // Now update the UI with the results.
        // ...
    }

    private async System.Threading.Tasks.Task ComputeNextMove()
    {
        // Perform background work here.
        // Don't directly access UI elements from this method.
    }
}
```

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public class Example
> {
>     // ...
>     private async void NextMove-Click(object sender, RoutedEventArgs e)
>     {
>         await Task.Run(() => ComputeNextMove());
>         // Update the UI with results
>     }
> 
>     private async Task ComputeNextMove()
>     {
>         // ...
>     }
>     // ...
> }
> ```
> ```vb
> Public Class Example
>     ' ...
>     Private Async Sub NextMove-Click(ByVal sender As Object, ByVal e As RoutedEventArgs)
>         Await Task.Run(Function() ComputeNextMove())
>         ' update the UI with results
>     End Sub
> 
>     Private Async Function ComputeNextMove() As Task
>         ' ...
>     End Function
>     ' ...
> End Class
> ```

In diesem Beispiel wird der `NextMove-Click`-Handler bei **await** zurückgegeben, damit der UI-Thread reaktionsfähig bleibt. Die Ausführung wird jedoch in diesem Handler fortgesetzt, nachdem `ComputeNextMove` abgeschlossen ist (in einem Hintergrundthread ausgeführt). Der restliche Code im Handler aktualisiert die UI mit den Ergebnissen.

> **Hinweis**  Es gibt darüber hinaus eine [**ThreadPool**](https://msdn.microsoft.com/library/windows/apps/BR229621)- und eine [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/windows.system.threading.threadpooltimer.aspx)-API für die UWP, die für ähnliche Szenarien verwendet werden können. Weitere Informationen finden Sie unter [Threading und asynchrone Programmierung](https://msdn.microsoft.com/library/windows/apps/Mt187340).

## Verwandte Themen

* [Benutzerdefinierte Benutzerinteraktionen](https://msdn.microsoft.com/library/windows/apps/Mt185599)




<!--HONumber=Jun16_HO4-->


