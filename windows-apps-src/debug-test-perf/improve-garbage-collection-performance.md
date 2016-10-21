---
author: mcleblanc
ms.assetid: F912161D-3767-4F35-88C0-E1ECDED692A2
title: Verbessern der Leistung bei der Garbage Collection
description: "In C# und VisualBasic geschriebene UWP-Apps profitieren von der automatischen Arbeitsspeicherverwaltung des .NETGarbage Collectors. Dieser Abschnitt bietet einen Überblick über das Verhalten des .NETGarbage Collectors sowie über die bewährten Methoden zur Leistungssteigerung für den Garbage Collector in UWP-Apps."
translationtype: Human Translation
ms.sourcegitcommit: 5f9626c644315884ea8605a4e69e7e580a44010b
ms.openlocfilehash: 32290820d4732f1a8b28fd18682514e3948e0356

---
# Verbessern der Leistung bei der Garbage Collection

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

In C# und VisualBasic geschriebene UWP-Apps profitieren von der automatischen Arbeitsspeicherverwaltung des .NETGarbage Collectors. Dieser Abschnitt bietet einen Überblick über das Verhalten des .NETGarbage Collectors sowie über die bewährten Methoden zur Leistungssteigerung für den Garbage Collector in UWP-Apps. Weitere Informationen zur Funktionsweise des .NETGarbage Collectors sowie zu Debugging- und Leistungsanalysetools für den Garbage Collector finden Sie unter [Garbage Collection](https://msdn.microsoft.com/library/windows/apps/xaml/0xy59wtx.aspx).

**Hinweis**  Die Notwendigkeit, in das Standardverhalten des Garbage Collector eingreifen zu müssen, ist ein starkes Indiz für allgemeine Speicherprobleme mit Ihrer App. Weitere Informationen finden Sie unter [Speichernutzungstool beim Debuggen in Visual Studio 2015](http://blogs.msdn.com/b/visualstudioalm/archive/2014/11/13/memory-usage-tool-while-debugging-in-visual-studio-2015.aspx). Dieses Thema betrifft ausschließlich C# und Visual Basic.

 

Der Garbage Collector bestimmt den Moment der Ausführung, indem er den Speicherverbrauch des verwalteten Heaps mit dem Aufwand für die Garbage Collection vergleicht. Zur Ermittlung dieses Zeitpunkts unterteilt der Garbage Collector unter anderem den Heap in Generationen und erfasst größtenteils nur einen Teil des Heaps. Der verwaltete Heap enthält drei Generationen:

-   Generation0. Diese Generation enthält neu zugeordnete Objekte mit einer Größe von 85 KB oder mehr. Ab dieser Größe gehören die Objekte dem Heap für große Objekte an. Der Heap für große Objekte wird im Rahmen der Generation2 bereinigt. Bereinigungen der Generation0 werden am häufigsten ausgeführt und bereinigen kurzlebige Objekte wie lokale Variablen.
-   Generation1. Diese Generation enthält Objekte, die nach Generation0 noch vorhanden sind. Sie hat die Funktion eines Puffers zwischen den Generationen0 und2. Bereinigungen der Generation1 sind seltener als Bereinigungen der Generation0 und bereinigen temporäre Objekte, die bei vorangegangenen Bereinigungen der Generation0 aktiv waren. Eine Bereinigung der Generation1 bereinigt auch die Generation0.
-   Generation2. Diese Generation enthält langlebige Objekte, die nach den Generationen0 und1 noch vorhanden sind. Collections der Generation2 sind am seltensten und erfassen den gesamten verwalteten Heap – einschließlich des Heaps für große Objekte, der Objekte ab einer Größe von 85 KB enthält.

Zur Ermittlung der Leistung des Garbage Collectors können zwei Aspekte betrachtet werden: die zum Ausführen der Garbage Collection erforderliche Zeit und die Arbeitsspeichernutzung des verwalteten Heaps. Konzentrieren Sie sich bei einer kleinen App mit einer Heapgröße von weniger als 100MB auf die Verringerung der Arbeitsspeichernutzung. Konzentrieren Sie sich nur dann auf die Verringerung der für die Garbage Collection benötigtenZeit, wenn der verwaltete Heap Ihrer App eine Größe von 100MB übersteigt. Hier erfahren Sie, wie der.NET Garbage Collector eine bessere Leistung erzielen kann.

## Verringern der Arbeitsspeichernutzung

### Versionsverweise

Ein Verweis auf ein Objekt in Ihrer App verhindert die Bereinigung dieses Objekts sowie aller Objekte, auf die es verweist. Der .NET-Compiler erkennt sehr zuverlässig, wann eine Variable nicht mehr verwendet wird, sodass Objekte, die von dieser Variablen gehalten werden, der Collection zugeführt werden können. Manchmal ist es jedoch möglicherweise nicht ohne Weiteres ersichtlich, dass Objekte auf andere Objekte verweisen, da sich ein Teil des Objektgraphs unter Umständen im Besitz von Bibliotheken befindet, die Ihre App verwendet. Informationen zu den Tools und Techniken, mit denen Sie ermitteln können, welche Objekte nach einer Garbage Collection noch vorhanden sind, finden Sie unter [Garbage Collection und Leistung](https://msdn.microsoft.com/library/windows/apps/xaml/ee851764.aspx).

### Auslösen einer Garbage Collection bei Bedarf

Lösen Sie eine Garbage Collection nur aus, nachdem Sie die Leistung Ihrer App ermittelt haben und zu dem Schluss gekommen sind, dass sich eine Bereinigung positiv auf das Ergebnis auswirkt.

Rufen Sie zum Auslösen der Garbage Collection einer Generation [**GC.Collect(n)**](https://msdn.microsoft.com/library/windows/apps/xaml/y46kxc5e.aspx) auf, wobei „n“ für die Generation steht, die Sie erfassen möchten (0, 1 oder 2).

**Hinweis**  Wir empfehlen, in Ihrer App keine Garbage Collection zu erzwingen, da der Garbage Collector den besten Zeitpunkt für eine Collection anhand einer Vielzahl heuristischer Daten ermittelt und die Erzwingung einer Collection somit häufig eine unnötige Belastung der CPU darstellt. Falls Ihre App allerdings eine große Anzahl von Objekten enthält, die nicht mehr verwendet werden, und Sie den entsprechenden Arbeitsspeicher wieder für das System freigeben möchten, ist die Erzwingung einer Garbage Collection unter Umständen dennoch angemessen. So können Sie beispielsweise in einem Spiel eine Bereinigung am Ende einer Ladesequenz auslösen, um vor Spielbeginn Arbeitsspeicher freizugeben.
 
Damit Sie nicht versehentlich zu viele Garbage Collections auslösen, können Sie [**GCCollectionMode**](https://msdn.microsoft.com/library/windows/apps/xaml/bb495757.aspx) auf **Optimized** festlegen. Dadurch wird der Garbage Collector angewiesen, nur dann eine Garbage Collection zu starten, wenn eine ausreichend hohe Produktivität dies rechtfertigt.

## Verringern der erforderlichen Zeit für die Garbage Collection

Dieser Abschnitt gilt, wenn Sie Ihre App analysiert und eine hohe Garbage Collection-Dauer festgestellt haben. Die Pausenzeiten für die Garbage Collection werden von folgenden Aspekten beeinflusst: der erforderlichen Zeit zum Ausführen eines einzelnen Garbage Collection-Durchlaufs und der Zeit, die die App insgesamt für Garbage Collections aufwendet. Die Dauer einer Bereinigung hängt davon ab, wie viele Livedaten der Collector analysieren muss. Die Größe der Generationen0 und1 ist zwar begrenzt, die Größe der Generation2 nimmt jedoch mit steigender Anzahl aktiver langlebiger Objekte in Ihrer App immer weiter zu. Das bedeutet, dass auch die Bereinigungsdauer für die Generationen0 und1 begrenzt ist, während Collections der Generation2 ggf. mehr Zeit in Anspruch nehmen. Da eine Garbage Collection Arbeitsspeicher freigibt, um Zuordnungsanforderungen zu befriedigen, hängt die Ausführungshäufigkeit von Garbage Collections in erster Linie davon ab, wie viel Arbeitsspeicher Sie zuordnen.

Der Garbage Collector hält Ihre App von Zeit zu Zeit an, um Arbeiten auszuführen. Dabei wird die App aber nicht zwingend für die gesamte Dauer der Bereinigung angehalten. Die Pausenzeiten sind für den Benutzer Ihrer App üblicherweise nicht wahrnehmbar. Dies gilt besonders bei Bereinigungen der Generationen0 und1. Die [Hintergrund-Garbage Collection](https://msdn.microsoft.com/library/windows/apps/xaml/ee787088.aspx#background-garbage-collection) des .NET Garbage Collectors ermöglicht die parallele Ausführung von Bereinigungen der Generation2 während der App-Ausführung. Die App wird dabei jeweils nur ganz kurz angehalten. Bereinigungen der Generation2 können allerdings nicht immer im Hintergrund ausgeführt werden. In diesem Fall ist die Pause unter Umständen für den Benutzer wahrnehmbar, sofern der Heap entsprechend groß (über 100MB) ist.

Häufig ausgeführte Garbage Collections können eine höhere CPU-Auslastung (und damit einen höheren Energiebedarf) sowie längere Ladezeiten oder geringere Bildfrequenzen in Ihrer Anwendung zur Folge haben. Im Anschluss finden Sie einige Techniken, mit denen Sie die Dauer der Garbage Collection sowie Pausenzeiten für die Garbage Collection einer verwalteten UWP-App verringern können.

### Verringern von Arbeitsspeicherzuordnungen

Wenn Sie überhaupt keine Objekte zuordnen, wird der Garbage Collector nur ausgeführt, wenn im System wenig Arbeitsspeicher vorhanden ist. Eine Verringerung der zugeordneten Arbeitsspeichermenge führt unmittelbar zu einer selteneren Ausführung von Garbage Collections.

Sollten Pausen in bestimmten Bereichen Ihrer App absolut unerwünscht sein, können Sie die benötigten Objekte während einer weniger leistungskritischen Phase bereits vorab zuordnen. So kann ein Spiel beispielsweise alle zum Spielen benötigten Objekte zuordnen, während der Ladebildschirm eines Levels angezeigt wird, damit beim Spielen keine Zuordnungen mehr erforderlich sind. Dadurch werden Pausen während des Spielverlaufs vermieden und ggf. höhere und einheitlichere Bildfrequenzen erzielt.

### Verringern von Bereinigungen der Generation2 durch Vermeidung von Objekten mittlerer Lebensdauer

Generationsbasierte Garbage Collections funktionieren am besten, wenn Ihre App deutlich kurzlebige und/oder deutlich langlebige Objekte enthält. Kurzlebige Objekte werden im Rahmen der weniger aufwendigen Generationen0 und1 bereinigt, während Objekte mit langer Lebensdauer der seltener ausgeführten Bereinigung der Generation2 vorbehalten sind. Langlebige Objekte werden während der gesamten Laufzeit Ihrer App (oder über einen längeren Zeitraum) verwendet – beispielsweise während der Anzeige einer bestimmten Seite oder während eines bestimmten Levels.

Wenn Sie häufig temporäre Objekte erstellen, die lange genug verwendet werden, um der Generation2 zugeordnet zu werden, werden mehr der aufwendigeren Bereinigungen der Generation2 ausgeführt. Die Anzahl von Collections der Generation2 lässt sich gegebenenfalls durch Wiederverwenden vorhandener Objekte oder durch schnelleres Freigeben der Objekte verringern.

Ein gängiges Beispiel für Objekte mit mittlerer Lebensdauer sind Objekte, die zum Anzeigen von Elementen in einer Liste dienen, in der der Benutzer einen Bildlauf ausführt. Wenn Objekte erstellt werden, sobald die Elemente in der Liste in den sichtbaren Bereich rücken, und nicht mehr auf sie verwiesen wird, wenn die Elemente in der Liste den sichtbaren Bereich wieder verlassen, fällt in der App üblicherweise eine große Anzahl von Bereinigungen der Generation2 an. In solchen Situationen können Sie für die Daten, die dem Benutzer aktiv angezeigt werden, einen Satz von Objekten vorab zuordnen und wiederverwenden. Dabei können Sie die Infos unter Verwendung kurzlebiger Objekte laden, wenn die Listenelemente in den sichtbaren Bereich rücken.

### Verringern von Bereinigungen der Generation2 durch Vermeidung großer Objekte mit kurzer Lebensdauer

Wie bereits erwähnt, wird jedes Objekt ab einer Größe von 85 KB dem Heap für große Objekte (Large Object Heap, LOH) zugeordnet und im Rahmen der Generation2 erfasst. Falls Sie über temporäre Variablen (beispielsweise Puffer) mit einer Größe von mehr als 85KB verfügen, werden diese durch eine Bereinigung der Generation2 bereinigt. Indem Sie die Größe der temporären Variablen auf unter 85 KB beschränken, können Sie in Ihrer App die Anzahl von Collections der Generation2 verringern. Eine gängige Technik ist die Erstellung eines Pufferpools und die Wiederverwendung von Objekten aus dem Pool, um umfangreiche temporäre Zuordnungen zu vermeiden.

### Vermeiden von Objekten mit vielen Verweisen

Der Garbage Collector folgt den Verweisen zwischen Objekten, um zu ermitteln, bei welchen Objekten es sich um Liveobjekte handelt. Den Ausgangspunkt bildet dabei jeweils der Stamm in Ihrer App. Weitere Informationen finden Sie unter [Vorgänge während einer Garbage Collection](https://msdn.microsoft.com/library/windows/apps/xaml/ee787088.aspx#what-happens-during-a-garbage-collection). Enthält ein Objekt eine Vielzahl von Verweisen, bedeutet dies einen höheren Aufwand für den Garbage Collector. Eine gängige Technik (besonders bei großen Objekten) ist das Konvertieren von Objekten mit vielen Verweisen in Objekte ohne Verweise (beispielsweise, indem anstelle eines Verweises ein Index gespeichert wird). Diese Technik funktioniert natürlich nur, wenn diese Vorgehensweise logisch möglich ist.

Das Ersetzen von Objektverweisen durch Indizes kann sich für Ihre App als komplizierte Änderung mit Störpotenzial erweisen und ist besonders für große Objekte mit einer Vielzahl von Verweisen gedacht. Führen Sie diesen Schritt nur aus, wenn Sie in Ihrer App eine hohe Garbage Collection-Dauer feststellen, die auf Objekte mit vielen Verweisen zurückzuführen ist.

 

 







<!--HONumber=Aug16_HO3-->


