---
author: TylerMSFT
ms.assetid: 066711E0-D5C4-467E-8683-3CC64EDBCC83
title: Aufrufen asynchroner APIs in C# oder Visual Basic
description: "Die Universelle Windows-Plattform (UWP) enthält viele asynchrone APIs. Diese sorgen dafür, dass Ihre App reaktionsfähig bleibt, wenn sie über einen längeren Zeitraum mit einer Aufgabe beschäftigt ist."
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: da2c6eddcc842e176e31b1a1628c91994efb1969

---
# Aufrufen asynchroner APIs in C# oder Visual Basic

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Die Universelle Windows-Plattform (UWP) enthält viele asynchrone APIs. Diese sorgen dafür, dass Ihre App reaktionsfähig bleibt, wenn sie über einen längeren Zeitraum mit einer Aufgabe beschäftigt ist. In diesem Thema wird die Verwendung asynchroner Methoden aus der Universellen Windows-Plattform (UWP) in C# oder Microsoft Visual Basic erläutert.

Asynchrone APIs verhindern, dass Ihre App auf den Abschluss umfangreicher Vorgänge warten muss, bevor die Ausführung fortgesetzt wird. Beispielsweise muss eine App, die Infos aus dem Internet herunterlädt, vielleicht mehrere Sekunden warten, bis die Infos übermittelt sind. Wenn Sie die Infos mit einer synchronen Methode abrufen, ist die App solange blockiert, bis der Methodenaufruf beendet ist. In diesem Zeitraum reagiert die App nicht auf Benutzerinteraktionen, und da sie nicht zu antworten scheint, ist der Benutzer möglicherweise verärgert. Die UWP stellt asynchrone APIs bereit, damit Ihre App für den Benutzer reaktionsfähig bleibt, wenn sie lange Vorgänge ausführt.

Die meisten asynchronen APIs in UWP haben kein synchrones Gegenstück. Es ist daher wichtig zu verstehen, wie Sie die asynchronen APIs mit C# oder Microsoft Visual Basic in Ihrer UWP-App verwenden können. Hier erfahren Sie, wie Sie die asynchronen APIs der Universellen Windows-Plattform aufrufen können.

## Verwenden asynchroner APIs


Üblicherweise erhalten asynchrone Methoden Namen, die auf „Async“ enden. Asynchrone APIs werden in der Regel als Reaktion auf eine Benutzeraktion aufgerufen – beispielsweise, wenn der Benutzer auf eine Schaltfläche klickt. Der Aufruf einer asynchronen Methode in einem Ereignishandler ist eine der einfachsten Möglichkeiten, asynchrone APIs zu verwenden. Hier verwenden wir den **await**-Operator als Beispiel.

Angenommen, Sie haben eine App, die Titel von Blogbeiträgen von einem bestimmten Ort auflistet. Die App enthält ein [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)-Element, auf das der Benutzer klickt, um die Titel abzurufen. Die Titel werden in einem [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)-Element angezeigt. Wenn der Benutzer auf die Schaltfläche klickt, ist es wichtig, dass die App reaktionsfähig bleibt, während sie auf Infos von der Website des Blogs wartet. Um diese Reaktionsfähigkeit sicherzustellen, stellt die Universelle Windows-Plattform (UWP) eine asynchrone Methode ([**SyndicationClient.RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460)) zum Herunterladen des Feeds bereit.

In diesem Beispiel wird durch Aufrufen der asynchronen Methode [**SyndicationClient.RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460) die Liste mit den Blogbeiträgen abgerufen und auf das Ergebnis gewartet.

> [!div class="tabbedCodeSnippets" data-resources="OutlookServices.Calendar"]
[!code-csharp[Main](./AsyncSnippets/csharp/MainPage.xaml.cs#SnippetDownloadRSS)]
[!code-vb[Main](./AsyncSnippets/vbnet/MainPage.xaml.vb#SnippetDownloadRSS)]

Zu diesem Beispiel gibt es eine Reihe wichtiger Dinge anzumerken. Zunächst verwendet die Zeile `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)` den **await**-Operator mit dem Aufruf der asynchronen Methode [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460). Der **await**-Operator teilt dem Compiler gewissermaßen mit, dass Sie eine asynchrone Methode aufrufen. Der Compiler übernimmt daraufhin einen Teil der Arbeit für Sie. Als Nächstes enthält die Deklaration des Ereignishandlers das Schlüsselwort **async**. Sie müssen dieses Schlüsselwort in der Methodendeklaration aller Methoden angeben, in denen Sie den **await**-Operator verwenden.

Dieses Thema beschäftigt sich nicht detailliert damit, was der Compiler mit dem **await**-Operator ausführt, sondern damit, wie Ihre App asynchron und reaktionsfähig bleibt. Überlegen Sie, was geschieht, wenn Sie synchronen Code verwenden. Angenommen, es gibt eine Methode namens `SyndicationClient.RetrieveFeed`, die synchron ist. (So eine Methode gibt es nicht, aber wir stellen es uns einfach mal vor.) Wenn Ihre App anstelle von `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)` die Zeile `SyndicationFeed feed = client.RetrieveFeed(feedUri)` enthielte, würde die Ausführung der App beendet, bis der Rückgabewert von `RetrieveFeed` verfügbar ist. Während Ihre App auf den Abschluss der Methode wartet, kann sie nicht auf andere Ereignisse reagieren, z.B. auf ein weiteres [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737)-Ereignis. Ihre App wäre also blockiert, bis `RetrieveFeed` zurückgegeben wird.

Rufen Sie aber `client.RetrieveFeedAsync` auf, initiiert die Methode den Abruf und wird sofort zurückgegeben. Wenn Sie **await** mit [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460) verwenden, beendet die App den Ereignishandler vorübergehend. Sie kann dann die anderen Ereignisse verarbeiten, während **RetrieveFeedAsync** asynchron ausgeführt wird. Hierdurch bleibt die App für den Benutzer reaktionsfähig. Wenn **RetrieveFeedAsync** abgeschlossen und [**SyndicationFeed**](https://msdn.microsoft.com/library/windows/apps/BR243485) verfügbar ist, macht die App im Ereignishandler im Grunde an der Stelle weiter, wo sie aufgehört hat, und zwar nach `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)`, und beendet den Rest der Methode.

Der Vorteil des **await**-Operators ist, dass der Code nicht viel anders aussieht, als wenn Sie die imaginäre `RetrieveFeed`-Methode verwendet hätten. Es ist möglich, asynchronen Code in C# oder Visual Basic ohne den **await**-Operator zu schreiben. Der resultierende Code neigt jedoch dazu, die Funktionsweise der asynchronen Ausführung zu betonen. Asynchroner Code ist deshalb kompliziert zu schreiben, zu verstehen und zu verwalten. Durch Verwenden des **await**-Operators können Sie die Vorteile einer asynchronen App nutzen, ohne den Code zu komplex zu gestalten.

## Rückgabetypen und Ergebnisse asynchroner APIs


Wenn Sie dem Link zu [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460) gefolgt sind, haben Sie vielleicht festgestellt, dass der Rückgabetyp von **RetrieveFeedAsync** kein [**SyndicationFeed**](https://msdn.microsoft.com/library/windows/apps/BR243485) ist. Stattdessen lautet der Rückgabetyp `IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress>`. Aus der Perspektive der Rohsyntax gibt eine asynchrone API ein Objekt mit dem darin enthaltenen Ergebnis zurück. Zwar ist es üblich und mitunter sinnvoll, sich eine asynchrone Methode als „awaitable” vorzustellen, der **await**-Operator agiert aber tatsächlich basierend auf dem Rückgabewert und nicht basierend auf der Methode. Wenn Sie den **await**-Operatoren verwenden, erhalten Sie das Ergebnis des **GetResult**-Aufrufs für das Objekt zurück, das von der Methode zurückgegeben wird. In diesem Beispiel ist **SyndicationFeed** das Ergebnis von **RetrieveFeedAsync.GetResult()**.

Wenn Sie eine asynchrone Methode verwenden, können Sie anhand der Signatur feststellen, was Sie nach dem Warten auf den Wert erhalten, der von der Methode zurückgegeben wird. Alle asynchronen APIs in der UWP geben einen der folgenden Typen zurück:

-   [**IAsyncOperation&lt;TResult&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206598)
-   [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206594)
-   [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.aspx)
-   [**IAsyncActionWithProgress&lt;TProgress&gt;**](https://msdn.microsoft.com/library/windows/apps/br206581.aspx)

Der Ergebnistyp einer asynchronen Methode entspricht dem Typ des Parameters `      TResult`. Typen ohne `TResult` haben kein Ergebnis. Sie können sich das Ergebnis als **void** vorstellen. In Visual Basic entspricht eine [Sub](https://msdn.microsoft.com/library/windows/apps/xaml/831f9wka.aspx)-Prozedur einer Methode mit einem **void**-Rückgabetyp.

Die Tabelle hier enthält Beispiele asynchroner Methoden sowie den Rückgabetyp und den Ergebnistyp für jede Methode.

| Asynchrone Methode                                                                           | Rückgabetyp                                                                                                                                        | Ergebnistyp                                       |
|-----------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| [**SyndicationClient.RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460)     | [**IAsyncOperationWithProgress&lt;SyndicationFeed, RetrievalProgress&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206594)                                 | [**SyndicationFeed**](https://msdn.microsoft.com/library/windows/apps/BR243485) |
| [**FileOpenPicker.PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/JJ635275) | [**IAsyncOperation&lt;StorageFile&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206598)                                                                                | [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171)          |
| [**XmlDocument.SaveToFileAsync**](https://msdn.microsoft.com/library/windows/apps/BR206284)                 | [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.aspx)                                                                                                           | **void**                                          |
| [**InkStrokeContainer.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/Hh701757)               | [**IAsyncActionWithProgress&lt;UInt64&gt;**](https://msdn.microsoft.com/library/windows/apps/br206581.aspx)                                                                   | **void**                                          |
| [**DataReader.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/BR208135)                            | [**DataReaderLoadOperation**](https://msdn.microsoft.com/library/windows/apps/BR208120), eine benutzerdefinierte Ergebnisklasse, die **IAsyncOperation&lt;UInt32&gt;** implementiert. | [**UInt32**](https://msdn.microsoft.com/library/windows/apps/br206598.aspx)                     |

 

In [**.NET for UWP apps**](https://msdn.microsoft.com/library/windows/apps/xaml/br230232.aspx) definierte asynchrone Methoden besitzen den Rückgabetyp [**Task**](https://msdn.microsoft.com/library/windows/apps/xaml/system.threading.tasks.task.aspx) oder [**Task&lt;TResult&gt;**](https://msdn.microsoft.com/library/windows/apps/xaml/dd321424.aspx). Methoden, die **Task** zurückgeben, ähneln den asynchronen Methoden der Universellen Windows-Plattform, die [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.aspx) zurückgeben. In jedem Fall ist das Ergebnis der asynchronen Methode **void**. Der Rückgabetyp **Task&lt;TResult&gt;** gleicht [**IAsyncOperation&lt;TResult&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206598) dahingehend, dass es sich beim Ergebnis der asynchronen Methode um den gleichen Typ handelt wie beim `TResult`-Typparameter, wenn die Aufgabe ausgeführt wird. Weitere Informationen zum Verwenden von **.NET for UWP apps** und Aufgaben finden Sie unter [.NET für Windows-Runtime-Apps– Übersicht](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx).

## Behandeln von Fehlern


Wenn Sie Ihre Ergebnisse mit dem **await**-Operator aus einer asynchronen Methode abrufen, können Sie mit einem **try/catch**-Block auf die gleiche Weise wie bei synchronen Methoden Fehler behandeln, die in asynchronen Methoden auftreten. Im vorherigen Beispiel werden die **RetrieveFeedAsync**-Methode und der **await**-Vorgang zur Fehlerbehandlung beim Auslösen einer Ausnahme in einen **try/catch**-Block eingeschlossen.

Werden asynchrone Methoden von anderen asynchronen Methoden aufgerufen, wird jede Methode, die zu einer Ausnahme führt, an die äußeren Methoden weitergegeben. Folglich können Sie die äußerste Methode mit einem **try/catch**-Block versehen, um Fehler für die geschachtelten asynchronen Methoden abzufangen. Auch hier werden Ausnahmen ähnlich wie bei synchronen Methoden erfasst. **await** kann aber nicht im **catch**-Block verwendet werden.

**Tipp:** Ab C# können Sie in Microsoft Visual Studio2005 **await** im **catch**-Block verwenden.

## Zusammenfassung und nächste Schritte

Das hier gezeigte Muster für den Aufruf einer asynchronen Methode ist am einfachsten zu verwenden, wenn Sie asynchrone APIs in einem Ereignishandler aufrufen. Sie können dieses Muster auch verwenden, wenn Sie eine asynchrone Methode in einer überschriebenen Methode aufrufen, die **void** oder **Sub** in Visual Basic zurückgibt.

Beachten Sie bei der Arbeit mit asynchronen Methoden in der Universellen Windows-Plattform Folgendes:

-   Üblicherweise erhalten asynchrone Methoden Namen, die auf „Async“ enden.
-   Methoden, die den **await**-Operator verwenden, müssen mit dem **async**-Schlüsselwort markiert sein.
-   Wenn eine App den **await**-Operator findet, bleibt die App bei einer Benutzerinteraktion reaktionsfähig, während die asynchrone Methode ausgeführt wird.
-   Beim Warten auf den von einer asynchronen Methode zurückgegebenen Wert wird ein Objekt zurückgegeben, das das Ergebnis enthält. In den meisten Fällen ist das im Rückgabewert enthaltene Ergebnis nützlich, nicht der Rückgabewert selbst. Sie können den Typ des Werts, der im Ergebnis enthalten ist, anhand des Rückgabetyps der asynchronen Methode ermitteln.
-   Mit asynchronen APIs und **async**-Strukturen kann häufig die Reaktionsfähigkeit Ihrer App verbessert werden.

Das Beispiel in diesem Thema gibt Text aus, der wie folgt aussieht.

``` syntax
Windows Experience Blog
PC Snapshot: Sony VAIO Y, 8/9/2011 10:26:56 AM -07:00
Tech Tuesday Live Twitter #Chat: Too Much Tech #win7tech, 8/8/2011 12:48:26 PM -07:00
Windows 7 themes: what’s new and what’s popular!, 8/4/2011 11:56:28 AM -07:00
PC Snapshot: Toshiba Satellite A665 3D, 8/2/2011 8:59:15 AM -07:00
Time for new school supplies? Find back-to-school deals on Windows 7 PCs and Office 2010, 8/1/2011 2:14:40 PM -07:00
Best PCs for blogging (or working) on the go, 8/1/2011 10:08:14 AM -07:00
Tech Tuesday – Blogging Tips and Tricks–#win7tech, 8/1/2011 9:35:54 AM -07:00
PC Snapshot: Lenovo IdeaPad U460, 7/29/2011 9:23:05 AM -07:00
GIVEAWAY: Survive BlogHer with a Sony VAIO SA and a Samsung Focus, 7/28/2011 7:27:14 AM -07:00
3 Ways to Stay Cool This Summer, 7/26/2011 4:58:23 PM -07:00
Getting RAW support in Photo Gallery & Windows 7 (…and a contest!), 7/26/2011 10:40:51 AM -07:00
Tech Tuesdays Live Twitter Chats: Photography Tips, Tricks and Essentials, 7/25/2011 12:33:06 PM -07:00
3 Tips to Go Green With Your PC, 7/22/2011 9:19:43 AM -07:00
How to: Buy a Green PC, 7/22/2011 9:13:22 AM -07:00
Windows 7 themes: the distinctive artwork of Cheng Ling, 7/20/2011 9:53:07 AM -07:00
```



<!--HONumber=Aug16_HO3-->


