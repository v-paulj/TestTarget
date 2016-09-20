---
author: mcleblanc
ms.assetid: 3A477380-EAC5-44E7-8E0F-18346CC0C92F
title: Virtualisierung von ListView- und GridView-Daten
description: Verbessern Sie die Leistung und Startzeit von ListView und GridView durch Datenvirtualisierung.
translationtype: Human Translation
ms.sourcegitcommit: e44dd5c2c3c9fb252062af3a6a9f409e1777a878
ms.openlocfilehash: 0a16dc27db6fb1e04e1ab0c575077ca10b97f12d

---
# Virtualisierung von „ListView“- und „GridView“-Daten

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Hinweis**  Weitere Informationen finden Sie unter der „//build/“-Sitzung [Erhebliches Erhöhen der Leistung bei der Interaktion von Benutzern mit großen Mengen von Daten in GridView und ListView](https://channel9.msdn.com/Events/Build/2013/3-158).

Verbessern Sie die Leistung und Startzeit von [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) und [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) durch Datenvirtualisierung. Informationen zu UI-Virtualisierung, Elementreduzierung und die progressive Aktualisierung von Elementen finden Sie unter [Optimieren der ListView- und GridView-Benutzeroberfläche](optimize-gridview-and-listview.md).

Eine Methode zur Datenvirtualisierung ist für einen Datensatz erforderlich, der so groß ist, dass er zu einem bestimmten Zeitpunkt nicht komplett im Arbeitsspeicher gespeichert werden kann oder soll. Sie laden einen ersten Teil (vom lokalen Datenträger, aus dem Netzwerk oder der Cloud) in den Arbeitsspeicher und wenden die UI-Virtualisierung auf dieses Teildataset an. Sie können die Daten später inkrementell oder bei Bedarf von beliebigen Punkten im Master-Dataset (wahlfreier Zugriff) laden. Ob die Datenvirtualisierung für Sie geeignet ist, hängt von vielen Faktoren ab.

-   Die Größe Ihres Datasets
-   Die Größe der einzelnen Elemente
-   Die Quelle des Datasets (lokaler Datenträger, Netzwerk oder Cloud)
-   Die gesamte Arbeitsspeichernutzung Ihrer App

**Hinweis**  Beachten Sie, dass für „ListView“ und „GridView“ standardmäßig ein Feature aktiviert ist, das temporäre Platzhalter für visuelle Elemente anzeigt, wenn der Benutzer den Bildausschnitt schnell verschiebt oder einen schnellen Bildlauf durchführt. Beim Laden von Daten werden diese Platzhalter für visuelle Elemente durch die Elementvorlage ersetzt. Sie können das Feature deaktivieren, indem Sie [**ListViewBase.ShowsScrollingPlaceholders**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders) auf „false“ festlegen. In diesem Fall wird jedoch empfohlen, das „x: Phase“-Attribut zu verwenden, um die Elemente in der Elementvorlage progressiv zu rendern. Siehe [Progressives Aktualisieren von ListView- und GridView-Elementen](optimize-gridview-and-listview.md#update-items-incrementally).

Hier erhalten Sie weitere Informationen über die Verfahren der inkrementellen Datenvirtualisierung und der Datenvirtualisierung mit wahlfreiem Zugriff.

## Inkrementelle Datenvirtualisierung

Bei der inkrementellen Datenvirtualisierung werden Daten sequenziell geladen. Ein [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878), das die inkrementelle Datenvirtualisierung verwendet, kann zum Anzeigen einer Sammlung von einer Million Elementen verwendet werden, wobei aber anfänglich nur 50 Elemente geladen werden. Beim Schwenken/Bildlauf werden die nächsten 50 Elemente geladen. Der Ziehpunkt der Bildlaufleiste wird beim Laden der Elemente kleiner. Für diese Art der Datenvirtualisierung schreiben Sie eine Datenquellenklasse, die diese Schnittstellen implementiert.

-   [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx)
-   [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) (C#/VB) oder [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052) (C++/CX)
-   [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916)

Eine derartige Datenquelle ist eine speicherinterne Liste, die ständig erweitert werden kann. Das Elementsteuerelement fordert die Elemente mithilfe der standardmäßigen „Indexer“- und „Count“-Eigenschaften von [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx) an. Die Anzahl sollte die Anzahl der lokalen Elemente darstellen, nicht die tatsächliche Größe des Datasets.

Wenn sich das Elementsteuerelement dem Ende der vorhandenen Daten nähert, ruft es [**ISupportIncrementalLoading.HasMoreItems**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.isupportincrementalloading.hasmoreitems) auf. Wenn **true** zurückgegeben wird, ruft es [**ISupportIncrementalLoading.LoadMoreItemsAsync**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.isupportincrementalloading.loadmoreitemsasync) auf und übergibt eine empfohlene Anzahl von zu ladenden Elementen. Je nachdem, von wo die Daten (lokaler Datenträger, Netzwerk oder Cloud) geladen werden, können Sie eine andere als die empfohlene Anzahl von Elementen laden. Wenn Ihr Dienst z.B. Stapel von 50 Elementen unterstützt, aber das Elementsteuerelement nur 10 anfordert, dann können Sie 50 Elemente laden. Laden Sie die Daten vom Back-End, fügen Sie sie zur Liste hinzu, und lösen Sie über [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) oder [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052) eine Änderungsbenachrichtigung aus, damit das Elementsteuerelement über die neuen Elemente informiert wird. Geben Sie außerdem die Anzahl der tatsächlich geladenen Elemente zurück. Wenn Sie weniger Artikel als empfohlen laden oder das Elementsteuerelement in der Zwischenzeit noch weiter verschoben/durchlaufen wurde, dann wird die Datenquelle erneut aufgerufen, um weitere Elemente zu laden, und der Zyklus wird anschließend fortgesetzt. Weitere Informationen finden Sie durch Herunterladen des [XAML-Datenbindungsbeispiel](https://code.msdn.microsoft.com/windowsapps/Data-Binding-7b1d67b5) für Windows 8.1 und der Wiederverwendung des Quellcodes in Ihrer App für Windows 10.

## Datenvirtualisierung mit wahlfreiem Zugriff

Die Datenvirtualisierung mit wahlfreiem Zugriff ermöglicht das Laden von Daten, die sich an einem beliebigen Punkt des Datasets befinden. Ein [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878), das die Datenvirtualisierung mit wahlfreiem Zugriff verwendet und zum Anzeigen einer Sammlung von einer Million Elementen verwendet wird, kann die Elemente 100.000 bis 100.050 laden. Wechselt der Benutzer anschließend zum Listenanfang, lädt das Steuerelement die Elemente von 1 bis 50. Der Ziehpunkt der Bildlaufleiste gibt jederzeit an, dass **ListView** eine Million Elemente enthält. Die Position des Ziehpunkts der Bildlaufleiste gibt Aufschluss über die relative Position der sichtbaren Elemente innerhalb des gesamten Datasets der Sammlung. Diese Art der Datenvirtualisierung kann den Speicherbedarf und die Ladezeiten für die Sammlung erheblich reduzieren. Zur Aktivierung müssen Sie eine Datenquellenklasse erstellen, die Daten bei Bedarf abruft, einen lokalen Cache verwaltet und diese Schnittstellen implementiert.

-   [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx)
-   [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) (C#/VB) oder [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052) (C++/CX)
-   (Optional) [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070)
-   (Optional) [**ISelectionInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877074)

[**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070) stellt Informationen dazu bereit, welche Elemente das Steuerelement aktiv verwendet. Das Steuerelement ruft diese Methode bei jeder Änderung der Ansicht auf und bezieht diese beiden Sätze von Bereichen ein.

-   Der Satz von Elementen, die sich im Viewport befinden.
-   Eine Reihe von nicht virtualisierten Elemente, die das Steuerelement verwendet und möglicherweise nicht im Viewport enthalten sind.
    -   Ein um den Viewport angeordneter Puffer von Elementen, der von den Elementen gesteuert wird, damit die Verschiebung per Toucheingabe reibungslos verläuft.
    -   Das Element, das den Fokus hat.
    -   Das erste Element.

Durch die Implementierung von [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070) weiß Ihre Datenquelle, welche Elemente abgerufen und zwischengespeichert werden müssen. Zudem ist ihr bekannt, wann nicht länger im Cache erforderliche Daten gelöscht werden müssen. **IItemsRangeInfo** verwendet [**ItemIndexRange**](https://msdn.microsoft.com/library/windows/apps/Dn877081)-Objekte, um eine Reihe von Elementen auf Basis ihres Indexes in der Sammlung zu beschreiben. Auf diese Weise werden keine Elementzeiger verwendet, die möglicherweise nicht korrekt oder zuverlässig sind. **IItemsRangeInfo** ist dafür vorgesehen, nur von einer einzelnen Instanz eines Elementsteuerelements verwendet zu werden, da es auf Zustandsinformationen für dieses Elementsteuerelement beruht. Wenn mehrere Elementsteuerelemente Zugriff auf dieselben Daten benötigen, ist jeweils eine separate Instanz der Datenquelle erforderlich. Sie können einen gemeinsamen Cache teilen, aber die Logik zum Löschen aus dem Cache ist dann komplizierter.

Hier folgt die grundlegende Strategie für die Datenvirtualisierung mit wahlfreiem Zugriff auf die Datenquelle.

-   Bei der Anforderung eines Elements
    -   Wenn es im Arbeitsspeicher verfügbar ist, geben Sie es zurück.
    -   Wenn es nicht vorhanden ist, geben Sie Null oder ein Platzhalterelement zurück.
    -   Verwenden Sie die Anforderung für ein Element (oder die Bereichsinformationen aus [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070)), um zu wissen, welche Elemente erforderlich sind, und um Daten für Elemente asynchron vom Back-End abzurufen. Lösen Sie nach dem Abrufen der Daten eine Änderungsbenachrichtigung über [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) oder [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052) aus, damit das Elementsteuerelement über das neue Element informiert ist.
-   (Optional) Wenn sich der Viewport des Elementsteuerelements ändert, identifizieren Sie über Ihre Implementierung von [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070), welche Elemente aus der Datenquelle erforderlich sind.

Die Strategie, wann die Datenelemente geladen werden, wie viele Datenelemente geladen werden und welche Elemente im Arbeitsspeicher verbleiben, hängt darüber hinaus von Ihrer Anwendung ab. Einige allgemeine, zu berücksichtigende Überlegungen:

-   Stellen Sie asynchrone Anforderungen für die Daten. Blockieren Sie nicht den UI-Thread.
-   Finden Sie den optimalen Bereich für die Größe der Stapel, in denen Sie Elemente abrufen. Bevorzugen Sie eine kompakte Vorgehensweise. Nicht zu klein, dass zu viele kleine Anforderungen erforderlich sind, aber auch nicht zu groß, dass das Abrufen zu lange dauert.
-   Berücksichtigen Sie, wie viele Anforderungen gleichzeitig ausstehen sollen. Die sequenzielle Ausführung ist einfacher, aber möglicherweise zu langsam, wenn die Verarbeitungszeit hoch ist.
-   Können Sie die Datenanforderungen abbrechen?
-   Ergeben sich Kosten für die einzelnen Transaktionen, wenn Sie einen gehosteten Dienst verwenden?
-   Welche Art von Benachrichtigungen werden vom Dienst bereitgestellt, wenn sich die Ergebnisse einer Abfrage ändern? Können Sie erfahren, ob ein Element bei Index 33 eingefügt wird? Wenn Ihr Dienst Abfragen auf Basis von Schlüssel-plus-Versatz unterstützt, ist dies möglicherweise besser geeignet, anstatt nur einen Index zu verwenden.
-   Wie intelligent soll das Vorabrufen der Elemente verlaufen? Möchten Sie die Richtung und Geschwindigkeit des Bildlaufs testen und nachverfolgen, um vorherzusagen, welche Elemente erforderlich sind?
-   Wie offensiv möchten Sie beim Leeren des Caches vorgehen? Hierbei müssen Arbeitsspeicher und Erfahrung gegeneinander abgewogen werden.







<!--HONumber=Aug16_HO4-->


