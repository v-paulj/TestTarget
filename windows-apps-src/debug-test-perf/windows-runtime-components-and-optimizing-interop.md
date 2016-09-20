---
author: mcleblanc
ms.assetid: 9899F6A0-7EDD-4988-A76E-79D7C0C58126
title: "Universelle Windows-Plattform-Komponenten und Optimierung der Interoperabilität"
description: Erstellen Sie UWP (Universelle Windows-Plattform)-Apps, die UWP-Komponenten verwenden, mit systemeigenen und verwalteten Typen zusammenarbeiten und gleichzeitig Probleme mit der Interopleistung vermeiden.
ms.sourcegitcommit: 5c7a49558ed11f82b7afea1ea96271c45c2f9139
ms.openlocfilehash: b9300b3feb1e5229951f3e1ebe454b61ba8065ae

---
# Universelle Windows-Plattform-Komponenten und Interop-Optimierung

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Erstellen Sie UWP (Universelle Windows-Plattform)-Apps, die UWP-Komponenten verwenden, mit systemeigenen und verwalteten Typen zusammenarbeiten und gleichzeitig Probleme mit der Interopleistung vermeiden.

## Bewährte Methoden für die Interoperabilität mit UWP-Komponenten

Wenn Sie nicht vorsichtig sind, kann die Verwendung von UWP-Komponenten die Leistung Ihrer App stark beeinträchtigen. In diesem Abschnitt wird beschrieben, wie Sie eine gute Leistung erzielen, wenn Ihre App UWP-Komponenten verwendet.

### Einführung

Interoperabilität kann große Auswirkungen auf die Leistung haben, und Sie verwenden sie möglicherweise, ohne es zu bemerken. Die UWP übernimmt einen Großteil der Interoperabilität für Sie, damit Sie produktiver sein und in anderen Sprachen geschriebenen Code erneut verwenden können. Sie sollten die Vorteile nutzen, die Ihnen die UWP bietet, müssen aber beachten, dass sie die Leistung beeinträchtigen kann. In diesem Abschnitt werden Dinge erläutert, die Sie tun können, um die Auswirkungen der Interoperabilität auf die Leistung Ihrer Anwendung zu reduzieren.

Die UWP verfügt über eine Bibliothek mit Typen, auf die in allen Sprachen zugegriffen werden kann, in denen eine UWP-App geschrieben werden kann. Sie verwenden die UWP-Typen in C# oder MicrosoftVisual Basic auf die gleiche Weise wie .NET-Objekte. Sie müssen in der Plattform keine Methodenaufrufe vornehmen, um auf die UWP-Komponenten zuzugreifen. Dadurch wird das Schreiben Ihrer Apps einfacher, aber es muss unbedingt beachtet werden, dass möglicherweise mehr Interoperabilität auftritt, als Sie erwarten. Wenn eine UWP-Komponente in einer anderen Sprache als C# oder Visual Basic geschrieben wird, überschreiten Sie beim Verwenden dieser Komponente die Interoperabilitätsgrenzen. Das Überschreiten der Interoperabilitätsgrenzen kann sich auf die Leistung einer App auswirken.

Wenn Sie eine UWP-App in C# oder Visual Basic entwickeln, verwenden Sie als API-Gruppen am häufigsten UWP-APIs und die .NET-APIs für UWP-Apps. In UWP definierte Typen sind normalerweise in Namespaces enthalten, die mit „Windows“ beginnen. .NET-Typen sind in Namespaces enthalten, die mit „System“ beginnen. Es gibt jedoch Ausnahmen. Die Verwendung der Typen in .NET für UWP-Apps erfordert keine Interoperabilität. Wenn Sie in einem Bereich, in dem die UWP verwendet wird, eine schlechte Leistung feststellen, können Sie stattdessen möglicherweise .NET für UWP-Apps verwenden, um eine bessere Leistung zu erzielen.

**Hinweis**  
Die meisten der im Lieferumfang von Windows10 enthaltenen UWP-Komponenten sind in C++ implementiert, sodass Sie Interoperabilitätsgrenzen überschreiten, wenn Sie sie in C# oder Visual Basic verwenden. Stellen Sie daher wie immer sicher, dass Sie Ihre App messen, um festzustellen, ob sich die Verwendung der UWP-Komponenten auf die Leistung Ihrer App auswirkt, bevor Sie in Änderungen an Ihrem Code investieren.

Wenn in diesem Thema von „UWP-Komponenten” die Rede ist, sind Komponenten gemeint, die in einer anderen Sprache als C# oder Visual Basic geschrieben sind.

 

Immer wenn Sie auf eine Eigenschaft zugreifen oder eine Methode für eine UWP-Komponente aufrufen, entsteht ein Interoperabilitätsaufwand. Das Erstellen einer UWP-Komponente ist tatsächlich aufwendiger als das Erstellen eines .NET-Objekts. Gründe dafür sind, dass die UWP Code ausführen muss, der von der Sprache Ihrer App in die Sprache der Komponente übertragen wird. Außerdem müssen die Daten zwischen verwalteten und nicht verwalteten Typen konvertiert werden, wenn Sie Daten an die Komponente übergeben.

### Effiziente Verwendung von UWP-Komponenten

Wenn Sie der Meinung sind, dass Sie eine höhere Leistung benötigen, können Sie sicherstellen, dass Ihr Code die UWP-Komponenten so effizient wie möglich verwendet. In diesem Abschnitt werden einige Tipps zum Verbessern der Leistung bei Verwendung von UWP-Komponenten erläutert.

Es sind viele Aufrufe in einem kurzen Zeitraum erforderlich, damit die Auswirkungen auf die Leistung spürbar werden. Eine gut entwickelte Anwendung, die Aufrufe von UWP-Komponenten aus Geschäftslogiken und anderem verwalteten Code kapselt, sollte keinen hohen Interoperabilitätsaufwand verursachen. Wenn Ihre Tests jedoch zeigen, dass die Verwendung der UWP-Komponenten die Leistung Ihrer App beeinträchtigt, helfen Ihnen die in diesem Abschnitt erläuterten Tipps dabei, die Leistung zu verbessern.

### Erwägen der Verwendung von .NET für UWP-Apps

Es gibt bestimmte Fälle, in denen eine Aufgabe mithilfe der UWP oder .NET für UWP-Apps ausgeführt werden kann. Sie sollten .NET-Typen und UWP-Typen nach Möglichkeit nicht mischen. Verwenden Sie die einen oder die anderen Typen. Beispielweise können Sie einen XML-Datenstrom mithilfe des [**Windows.Data.Xml.Dom.XmlDocument**](https://msdn.microsoft.com/library/windows/apps/BR206173)-Typs (ein UWP-Typ) oder des [**System.Xml.XmlReader**](https://msdn.microsoft.com/library/windows/apps/xaml/system.xml.xmlreader.aspx)-Typs (ein .NET-Typ) analysieren. Verwenden Sie die API, die dieselbe Technologie wie der Datenstrom aufweist. Wenn Sie beispielsweise XML aus einem [**MemoryStream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.memorystream.aspx) lesen, verwenden Sie den **System.Xml.XmlReader**-Typ, da es sich bei beiden Typen um .NET-Typen handelt. Wenn Sie aus einer Datei lesen, verwenden Sie den **Windows.Data.Xml.Dom.XmlDocument**-Typ, da es sich bei den Datei-APIs und **XmlDocument** um UWP-Komponenten handelt.

### Kopieren von Windows-Runtime-Objekten in .NET-Typen

Wenn eine UWP-Komponente ein UWP-Objekt zurückgibt, kann es von Vorteil sein, das zurückgegebene Objekt in ein .NET-Objekt zu kopieren. Zwei Stellen, an denen dies besonders wichtig ist, ist das Verwenden von Sammlungen und Datenströmen.

Wenn Sie eine UWP-API aufrufen, die eine Sammlung zurückgibt, und Sie diese Sammlung dann speichern und mehrfach auf sie zugreifen, kann es von Vorteil sein, die Sammlung in eine .NET-Sammlung zu kopieren und ab diesem Zeitpunkt die .NET-Version zu verwenden.

### Zwischenspeichern der Ergebnisse von Aufrufen von UWP-Komponenten zur späteren Verwendung

Möglicherweise erzielen Sie eine bessere Leistung, indem Sie Werte in lokalen Variablen speichern, anstatt mehrfach auf einen UWP-Typ zuzugreifen. Dies kann besonders hilfreich sein, wenn Sie einen Wert innerhalb einer Schleife verwenden. Messen Sie Ihre App, um festzustellen, ob sich die Leistung Ihrer App durch die Verwendung lokaler Variablen verbessert. Mit zwischengespeicherten Werten kann die Geschwindigkeit Ihrer App erhöht werden, da weniger Zeit für die Interoperabilität aufgewendet wird.

### Kombinieren der Aufrufe von UWP-Komponenten

Versuchen Sie, Aufgaben mit möglichst wenigen Aufrufen von UWP-Objekten auszuführen. Beispielsweise ist es in der Regel besser, eine große Datenmenge aus einem Datenstrom zu lesen, anstatt jeweils nur kleine Mengen zu lesen.

Verwenden Sie APIs, die die Arbeit in so wenig Aufrufen wie möglich bündeln, anstelle von APIs, die weniger Arbeit ausführen und mehr Aufrufe erfordern. Erstellen Sie beispielsweise lieber ein Objekt durch Aufrufen von Konstruktoren, die mehrere Eigenschaften initialisieren, anstatt den Standardkonstruktor aufzurufen und jeweils eine Eigenschaft zuzuweisen.

### Erstellen einer UWP-Komponente

Wenn Sie eine UWP-Komponente schreiben, die von in C++ oder JavaScript geschriebenen Apps verwendet werden kann, stellen Sie sicher, dass Ihre Komponente eine gute Leistung ermöglicht. Alle Vorschläge für das Erzielen einer guten Leistung in Apps gelten auch für das Erzielen einer guten Leistung in Komponenten. Messen Sie Ihre Komponenten, um festzustellen, welche APIs ein hohes Datenverkehrsmuster aufweisen, und stellen Sie für solche Bereiche APIs bereit, die es Ihren Benutzern ermöglichen, die Arbeit mit wenigen Aufrufen zu erledigen.

## Aufrechterhalten der Schnelligkeit Ihrer App bei der Verwendung von Interoperabilität in verwaltetem Code

Die UWP erleichtert die Interoperabilität zwischen systemeigenem und verwaltetem Code, aber wenn Sie nicht vorsichtig sind, können Leistungseinbußen auftreten. Hier zeigen wir Ihnen, wie Sie bei Verwendung der Interoperabilität in verwalteten UWP-Apps eine gute Leistung erzielen.

Mit der UWP können Entwickler Apps mithilfe von XAML-Code in ihrer bevorzugten Sprache schreiben. Dies ist dank der Projektionen der UWP-APIs möglich, die in jeder Sprache verfügbar sind. Das Schreiben einer App in C# oder Visual Basic geht auf Kosten der Interoperabilität, da die UWP-APIs normalerweise im systemeigenen Code implementiert sind und die CLR für jeden UWP-Aufruf aus C# oder Visual Basic von einem verwalteten in einen systemeigenen Stapelrahmen übergehen muss. Zudem müssen Funktionsparameter in Darstellungen gemarshallt werden, auf die der systemeigene Code zugreifen kann. Dieser Aufwand ist für die meisten Apps gering. Wenn Sie jedoch UWP-APIs im kritischen Pfad einer App sehr häufig aufrufen (Hunderttausende bis Millionen Aufrufe), kann dieser Aufwand beträchtlich werden. Im Allgemeinen möchten Sie erreichen, dass die für den Wechsel von Sprachen erforderliche Zeit klein im Vergleich zur Ausführungszeit des restlichen Codes ist. Dies wird im folgenden Diagramm veranschaulicht.

![Interopübergänge sollten die Ausführungszeit des Programms nicht beherrschen.](images/interop-transitions.png)

Die unter [**.NET für Windows-Apps**](https://msdn.microsoft.com/library/windows/apps/xaml/br230232.aspx) aufgelisteten Typen tragen nicht zum Interopaufwand bei, wenn sie von C# oder Visual Basic aus verwendet werden. Als Faustregel kann angenommen werden, dass Typen in Namespaces, die mit „Windows.“ beginnen, zur UWP gehören und Typen in Namespaces, die mit „System.” beginnen, .NET-Typen sind. Denken Sie daran, dass selbst die einfache Verwendung von UWP-Typen, z.B. Speicherzuordnung oder Zugriff auf Eigenschaften, Interoperabilitätskosten nach sich zieht.

Sie sollten Ihre App messen und feststellen, ob die Interoperabilität einen großen Teil der Ausführungszeit Ihrer App verschlingt, bevor Sie daran gehen, die Interoperabilitätskosten zu optimieren. Wenn Sie die Leistung Ihrer App mit Visual Studio analysieren, erhalten Sie leicht eine obere Grenze für die Interoperabilitätskosten, indem Sie in der Ansicht **Funktionen** auf die inklusive Zeit achten, die von Methoden verbraucht wird, die die UWP aufrufen.

Wenn Ihre App aufgrund des Interoperabilitätsaufwands langsam ist, können Sie die Leistung der App verbessern, indem Sie die Anzahl von UWP-API-Aufrufen in Pfaden mit wichtigem Code reduzieren. Beispielsweise kann eine Spielengine, die unzählige physische Berechnungen durchführt und hierzu ständig Position und Dimensionen von [**UIElements**](https://msdn.microsoft.com/library/windows/apps/BR208911) abfragt, sehr viel Zeit sparen, indem die notwendigen Infos aus **UIElements** in lokalen Variablen gespeichert werden. Anschließend werden diese zwischengespeicherte Werte für die Berechnungen verwendet, und das Endergebnis wird nach Abschluss der Berechnungen wieder an **UIElements** zurückgegeben. Ein weiteres Beispiel: Wenn C#- oder Visual Basic-Code sehr häufig auf eine Auflistung zugreift, ist es effizienter, eine Auflistung aus dem [**System.Collections**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.aspx)-Namespace anstatt aus dem [**Windows.Foundation.Collections**](https://msdn.microsoft.com/library/windows/apps/BR206657)-Namespace zu verwenden. Sie können auch erwägen, Aufrufe von UWP-Komponenten zu kombinieren. Ein Fall, in dem dies möglich ist, ist die Verwendung der [**Windows.Storage.BulkAccess**](https://msdn.microsoft.com/library/windows/apps/BR207676)-APIs.

### Erstellen einer UWP-Komponente

Wenn Sie eine UWP-Komponente schreiben, die von Apps verwendet werden soll, die in C++ oder JavaScript geschrieben wurden, stellen Sie sicher, dass Ihre Komponente eine gute Leistung ermöglicht. Ihre API-Oberfläche definiert Ihre Interoperabilitätsgrenze und den Grad, bis zu dem Benutzer über die Richtlinien in diesem Thema nachdenken müssen. Wenn Sie Ihre Komponenten weitergeben, wird dies besonders wichtig.

Alle Vorschläge für das Erzielen einer guten Leistung in Apps gelten auch für das Erzielen einer guten Leistung in Komponenten. Messen Sie Ihre Komponenten, um festzustellen, welche APIs ein hohes Datenverkehrsmuster aufweisen, und stellen Sie für solche Bereiche APIs bereit, die es Ihren Benutzern ermöglichen, die Arbeit mit wenigen Aufrufen zu erledigen. Es wurden große Anstrengungen unternommen, um die UWP so zu gestalten, dass Apps sie ohne häufige Überschreitung der Interoperabilitätsgrenze verwenden können.

 




<!--HONumber=Jun16_HO4-->


