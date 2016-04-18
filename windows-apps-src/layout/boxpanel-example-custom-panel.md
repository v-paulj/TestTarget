---
Description: 'Hier erfahren Sie, wie Sie Code für eine benutzerdefinierte Panel-Klasse schreiben. Dabei implementieren Sie die Methoden „ArrangeOverride“ und „MeasureOverride“ und verwenden die Children-Eigenschaft.'
MS-HAID: 'dev\_ctrl\_layout\_txt.boxpanel\_example\_custom\_panel'
MSHAttr: 'PreferredLib:/library/windows/apps'
Search.Product: eADQiWindows 10XVcnh
title: 'BoxPanel, ein Beispiel für benutzerdefinierte Panels'
ms.assetid: 981999DB-81B1-4B9C-A786-3025B62B74D6
label: BoxPanel, an example custom panel
template: detail.hbs
---

# BoxPanel, ein Beispiel für benutzerdefinierte Panels


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**Wichtige APIs**

-   [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511)
-   [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)
-   [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)

Hier erfahren Sie, wie Sie Code für eine benutzerdefinierte [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511)-Klasse schreiben. Dabei implementieren Sie die Methoden [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) und [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) und verwenden die [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514)-Eigenschaft. Der Beispielcode zeigt eine benutzerdefinierte Panelimplementierung. Wir gehen jedoch nicht detailliert auf die Erklärung der Layoutkonzepte ein, die Einfluss darauf haben, wie Sie ein Panel für verschiedene Layoutszenarien anpassen können. Wenn Sie weitere Informationen zu diesen Layoutkonzepten und der Anwendbarkeit auf Ihr jeweiliges Layoutszenario benötigen, lesen Sie [Übersicht über benutzerdefinierte XAML-Panels](custom-panels-overview.md).

Ein *Panel* ist ein Objekt, das ein Layoutverhalten für die darin enthaltenen untergeordneten Elemente bereitstellt, wenn das XAML-Layoutsystem ausgeführt und die Benutzeroberfläche Ihrer App dargestellt wird. Sie können für ein XAML-Layout benutzerdefinierte Panels definieren, indem Sie eine benutzerdefinierte Klasse aus der [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511)-Klasse ableiten. Das Verhalten für das Panel wird bereitgestellt, indem Sie die Methoden [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) und [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) außer Kraft setzen und somit eine Logik liefern, die die untergeordneten Elemente misst und anordnet. Dieses Beispiel wurde von **Panel** abgeleitet. Wenn Sie mit einem **Panel** beginnen, haben die Methoden **ArrangeOverride** und **MeasureOverride** kein Startverhalten. Ihr Code stellt das Gateway bereit, mit dessen Hilfe die untergeordneten Elemente dem XAML-Layoutsystem mitgeteilt und auf der Benutzeroberfläche gerendert werden. Daher ist es wirklich wichtig, dass der Code alle untergeordneten Elemente berücksichtigt und dem vom Layoutsystem erwarteten Muster folgt.

## Ihr Layoutszenario


Beim Definieren eines benutzerdefinierten Panels definieren Sie ein Layoutszenario.

Ein Layoutszenario wird durch folgende Aspekte ausgedrückt:

-   Was geschieht, wenn das Panel untergeordnete Elemente enthält?
-   Gelten für den eigenen Bereich des Panels Einschränkungen?
-   Wie bestimmt die Logik des Panels alle Messwerte, Platzierungspositionen und Größenanpassungen, die letztlich zu einem gerenderten UI-Layout der untergeordneten Elemente führen?

Unter Berücksichtigung dieser Aspekte ist das hier gezeigte `BoxPanel` für ein bestimmtes Szenario geeignet. Da in diesem Beispiel der Code im Vordergrund stehen soll, werden wir das Szenario noch nicht detailliert erläutern, sondern uns stattdessen auf die notwendigen Schritte und die Codierungsmuster konzentrieren. Wenn Sie sich zunächst detaillierter über das Szenario informieren möchten, lesen Sie unter [Szenario für `BoxPanel`](#scenario) weiter, und kehren Sie später zum Code zurück.
## Erster Schritt: Ableiten von der **Panel**-Klasse


Leiten Sie zunächst aus der [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511)-Klasse eine benutzerdefinierte Klasse ab. Die einfachste Methode dafür ist wahrscheinlich, mithilfe der Kontextmenüoptionen **Hinzufügen** | **Neues Element** | **Klasse** für ein Projekt aus dem **Projektmappen-Explorer** in Microsoft Visual Studio eine separate Codedatei für diese Klasse zu definieren. Benennen Sie die Klasse (und Datei) mit `BoxPanel`.

Die Vorlagendatei für eine Klasse beginnt nicht mit vielen **using**-Anweisungen, weil sie nicht ausschließlich für UWP-Apps (Universelle Windows-Plattform) bestimmt ist. Fügen Sie daher zuerst **using**-Anweisungen hinzu. Die Vorlagendatei beginnt außerdem mit einigen **using**-Anweisungen, die Sie vielleicht nicht benötigen, und die gelöscht werden können. Hier ist eine Liste mit Vorschlägen für **using**-Anweisungen zur Auflösung von Typen, die Sie für einen typischen Code eines benutzerdefinierten Panels benötigen:

```CSharp
using System;
using System.Collections.Generic; //if you need to cast IEnumerable for iteration, or define your own collection properties
using Windows.Foundation; //Point Size and Rect
using Windows.UI.Xaml; //DependencyObject UIElement and FrameworkElement
using Windows.UI.Xaml.Controls; //Panel
using Windows.UI.Xaml.Media; //if you need Brushes or other utilities
```

Da Sie nun die [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511)-Klasse auflösen können, legen Sie diese als Basisklasse für `BoxPanel` fest. Machen Sie `BoxPanel` zudem öffentlich:

```CSharp
public class BoxPanel : Panel
{
}
```

Definieren Sie auf Klassenebene einige **int**- und **double**-Werte, die von mehreren Logikfunktionen gemeinsam verwendet werden, aber nicht als öffentliche API verfügbar gemacht werden müssen. In dem Beispiel sind diese wie folgt benannt: `maxrc`, `rowcount`, `colcount`, `cellwidth`, `cellheight`, `maxcellheight`, `aspectratio`.

Nachdem Sie dies getan haben, sieht die vollständige Codedatei wie folgt aus (Kommentare zu **using** werden entfernt, da Sie nun wissen, wozu sie dienen):

```CSharp
using System;
using System.Collections.Generic;
using Windows.Foundation;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

public class BoxPanel : Panel 
{
    int maxrc, rowcount, colcount;
    double cellwidth, cellheight, maxcellheight, aspectratio;
}
```

Ab hier zeigen wir Ihnen jeweils eine Memberdefinition. Dabei kann es sich um eine Methodenüberschreibung oder eine Unterstützung wie eine Abhängigkeitseigenschaft handeln. Sie können diese dem oben gezeigten Skelett in beliebiger Reihenfolge hinzufügen. Wir zeigen die **using**-Anweisungen oder die Definition des Klassenbereichs erst wieder in den Codeausschnitten, wenn wir den finalen Code präsentieren.

## **MeasureOverride**


```CSharp
protected override Size MeasureOverride(Size availableSize)
{
    Size returnSize;
    // Determine the square that can contain this number of items.
    maxrc = (int)Math.Ceiling(Math.Sqrt(Children.Count));
    // Get an aspect ratio from availableSize, decides whether to trim row or column.
    aspectratio = availableSize.Width / availableSize.Height;

    // Now trim this square down to a rect, many times an entire row or column can be omitted.
    if (aspectratio > 1)
    {
        rowcount = maxrc;
        colcount = (maxrc > 2 &amp;&amp; Children.Count < maxrc * (maxrc - 1)) ? maxrc - 1 : maxrc;
    } 
    else 
    {
        rowcount = (maxrc > 2 &amp;&amp; Children.Count < maxrc * (maxrc - 1)) ? maxrc - 1 : maxrc;
        colcount = maxrc;
    }

    // Now that we have a column count, divide available horizontal, that&#39;s our cell width.
    cellwidth = (int)Math.Floor(availableSize.Width / colcount);
    // Next get a cell height, same logic of dividing available vertical by rowcount.
    cellheight = Double.IsInfinity(availableSize.Height) ? Double.PositiveInfinity : availableSize.Height / rowcount;
           
    foreach (UIElement child in Children)
    {
        child.Measure(new Size(cellwidth, cellheight));
        maxcellheight = (child.DesiredSize.Height > maxcellheight) ? child.DesiredSize.Height : maxcellheight;
    }
    return LimitUnboundedSize(availableSize);
}
```

Das notwendige Muster einer [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)-Implementierung ist die Schleife durch die einzelnen Elemente in [**Panel.Children**](https://msdn.microsoft.com/library/windows/apps/br227514). Rufen Sie für jedes dieser Elemente immer die [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952)-Methode auf. **Measure** hat einen Parameter vom Typ [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995). An dieser Stelle übergeben Sie die Größe, die dem Panel für das jeweilige untergeordnete Element zur Verfügung stehen soll. Bevor Sie also den Schleife ausführen und mit dem **Measure**-Aufruf beginnen können, müssen Sie wissen, wie viel Platz die einzelnen Zellen belegen können. Aus der **MeasureOverride**-Methode selbst stammt der *availableSize*-Wert. Dies ist die Größe, die vom übergeordneten Panel beim **Measure**-Aufruf verwendet wurde. Dies war in erster Linie der Auslöser für diesen **MeasureOverride**-Aufruf. Eine typische Logik besteht also darin, ein Schema zu entwerfen, in dem jedes untergeordnete Element den Raum des gesamten *availableSize*-Werts für das Panel teilt. Anschließend übergeben Sie die einzelnen Größenteilungen für jedes untergeordnete Element an **Measure**.

Die Größe wird vom `BoxPanel` ziemlich einfach aufgeteilt: Es teilt den Platz in mehrere Felder, deren Anzahl größtenteils durch die Anzahl Elemente bestimmt wird. Die Größenanpassung der Felder basiert auf der Anzahl der Zeilen und Spalten und auf der verfügbaren Größe. Mitunter wird eine Zeile oder Spalte eines Quadrats nicht benötigt. Demzufolge wird es verworfen, und der Bereich ist bezüglich des Zeilen-/Spaltenverhältnisses kein Quadrat mehr, sondern wird zu einem Rechteck. Wenn Sie weitere Informationen dazu erhalten möchten, wie diese Logik erstellt wurde, lesen Sie unter [„Das Szenario für `BoxPanel`“](#scenario) weiter.

Was geschieht also während des Messdurchlaufs? Dabei wird für jedes Element, bei dem [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) aufgerufen wurde, ein Wert für die schreibgeschützte [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921)-Eigenschaft festgelegt. Das Vorhandensein eines **DesiredSize**-Werts kann beim Erreichen des Anordnungsdurchlaufs von Bedeutung sein, weil **DesiredSize** Aufschluss darüber gibt, wie die Größe bei der Anordnung und in der finalen Darstellung lauten kann oder soll. Selbst wenn Sie **DesiredSize** in Ihrer eigenen Logik nicht verwenden, wird der Wert dennoch vom System benötigt.

Dieses Panel kann verwendet werden, wenn die Höhenkomponenten von *availableSize* unbegrenzt ist. Wenn dies wahr ist, verfügt das Panel über keine bekannte Höhe zum Teilen. In diesem Fall informiert die Logik für den Messdurchlauf jedes untergeordnete Element darüber, dass es noch keine begrenzte Höhe aufweist. Dazu wird ein [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)-Wert an den [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952)-Aufruf für untergeordnete Elemente übergeben, wobei [**Size.Height**](https://msdn.microsoft.com/library/windows/apps/hh763910) endlos ist. Dies ist zulässig. Beim Aufruf von **Measure** wird der [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921)-Wert laut Logik als Minimalwert von Folgendem festgelegt: an **Measure** übergebene Werte oder die natürliche Größe des jeweiligen Elements aus Faktoren wie den explizit festgelegten Werten [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) und [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751).

**Hinweis**  Die interne [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)-Logik weist zudem dieses Verhalten auf: **StackPanel** übergibt einen endlosen Dimensionswert an [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) für untergeordnete Elemente. Dies deutet darauf hin, dass für untergeordnete Elemente in der Ausrichtungsdimension keine Beschränkung vorliegt. **StackPanel** passt seine Größe normalerweise dynamisch an, sodass alle untergeordneten Elemente in einem Stapel Platz haben, der in dieser Dimension zunimmt.

??

Das Panel selbst kann aber kein [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)-Element mit einem endlosen Wert aus [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) zurückgeben, weil dadurch im Layout eine Ausnahme ausgelöst wird. Teil dieser Logik ist es also, die von einem beliebigen untergeordneten Element angeforderte Maximalhöhe herauszufinden und diese Höhe als Zellenhöhe zu verwenden, wenn sie nicht bereits aus den eigenen Größenbeschränkungen des Panels stammt. Dies ist die Hilfsfunktion `LimitUnboundedSize`, auf die im vorherigen Code verwiesen wurde, der diese Zellenmaximalhöhe verwendet, um dem Panel eine begrenzte Höhe zuzuweisen. Zudem wird davon ausgegangen, dass `cellheight` eine finite Zahl ist, bevor der Anordnungsdurchlauf initiiert wird:

```CSharp
// This method is called only if one of the availableSize dimensions of measure is infinite.
// That can happen to height if the panel is close to the root of main app window.
// In this case, base the height of a cell on the max height from desired size
// and base the height of the panel on that number times the #rows.

Size LimitUnboundedSize(Size input)
{
    if (Double.IsInfinity(input.Height))
    {
        input.Height = maxcellheight * colcount;
        cellheight = maxcellheight;
    }
    return input;
}
```

## **ArrangeOverride**


```CSharp
protected override Size ArrangeOverride(Size finalSize)
{
     int count = 1
     double x, y;
     foreach (UIElement child in Children)
     {
          x = (count - 1) % colcount * cellwidth;
          y = ((int)(count - 1) / colcount) * cellheight;
          Point anchorPoint = new Point(x, y);
          child.Arrange(new Rect(anchorPoint, child.DesiredSize));
          count++;
     }
     return finalSize;
}
```

Das notwendige Muster einer [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)-Implementierung ist die Schleife durch die einzelnen Elemente in [**Panel.Children**](https://msdn.microsoft.com/library/windows/apps/br227514). Rufen Sie für jedes dieser Elemente immer die [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914)-Methode auf.

Beachten Sie, dass hier nicht so viele Berechnungen erfolgen wie in [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) – das ist typisch. Die Größe der untergeordneten Elemente ist bereits aus der eigenen **MeasureOverride**-Logik des Panels oder aus dem [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921)-Wert der einzelnen untergeordneten Elemente bekannt, der während des Messdurchlaufs festgelegt wird. Dennoch müssen wir die Position innerhalb des Panels festlegen, an der die einzelnen untergeordneten Elemente angezeigt werden. In einem typischen Panel sollte jedes untergeordnete Element an einer anderen Position gerendert werden. Ein Panel, das überlappende Elemente erzeugt, ist für gewöhnliche Szenarien nicht wünschenswert (obwohl Sie durchaus Panels mit zweckmäßigen Überlappungen erstellen können, wenn dies wirklich das beabsichtigte Szenario ist).

Dieses Panel wird nach dem Konzept von Zeilen und Spalten angeordnet. Die Anzahl Zeilen und Spalten wurde bereits berechnet (dies war für die Messung nötig). Die Form der Zeilen und Spalten sowie die bekannten Größen der einzelnen Zellen sind also Teil der Definitionslogik einer Darstellungsposition (`anchorPoint`) für jedes Element, das in diesem Panel enthalten ist. Dieser [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)-Wert und der bereits aus der Messung bekannte [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)-Wert werden zusammen als die beiden Komponenten verwendet, aus denen eine [**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994)-Struktur konstruiert wird. **Rect** ist der Eingabetyp für [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914).

Panels müssen die zugehörigen Inhalte mitunter beschneiden. Wenn dies der Fall ist, entspricht die beschnittene Größe der in [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) vorhandenen Größe, weil sie die [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952)-Logik als Mindestwert, der an **Measure** übergeben wurde, oder als andere natürliche Größenfaktoren festlegt. Normalerweise müssen Sie die Beschneidung in der [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914)-Phase also nicht überprüfen. Die Beschneidung erfolgt einfach basierend auf der Übergabe der **DesiredSize** an die einzelnen **Arrange**-Aufrufe.

Beim Durchlaufen der Schleife benötigen Sie nicht immer eine Zählung, wenn alle zum Definieren der Renderingposition benötigten Informationen anderweitig bekannt sind. Zum Beispiel ist in der [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)-Layoutlogik die Position in der [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514)-Sammlung unerheblich. Alle zum Positionieren der einzelnen Elemente in einer **Canvas** benötigten Infos sind durch Lesen der Werte [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) und [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772) der untergeordneten Elemente im Rahmen der Anordnungslogik bekannt. Die `BoxPanel`-Logik benötigt eine Zählung für den Vergleich mit dem *colcount*-Wert, damit ersichtlich ist, wann eine neue Zeile begonnen und der *y*-Wert versetzt wird.

Normalerweise sind der eingegebene Wert für *finalSize* und der Wert für [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995), der aus einer [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)-Implementierung zurückgegeben wird, identisch. Weitere Informationen zu den entsprechenden Gründen finden Sie unter [Übersicht über benutzerdefinierte XAML-Panels](custom-panels-overview.md) im Abschnitt **ArrangeOverride**.

## Verfeinerung: Steuern der Anzahl der Zeilen und Spalten


Sie könnten diesen Bereich nun in diesem Zustand kompilieren und verwenden. Wir fügen jedoch noch eine Verfeinerung hinzu. In dem gerade gezeigten Code positioniert die Logik die Extrazeile oder -spalte auf der Seite, die im Seitenverhältnis die längste ist. Aus Gründen der besseren Kontrolle über die Formen der Zellen kann es jedoch wünschenswert sein, eine 4-x-3-Zellenmenge anstatt 3 x 4 auszuwählen. Dies ist auch dann der Fall, wenn das eigene Seitenverhältnis des Panels „Hochformat“ lautet. Daher fügen wir eine optionale Abhängigkeitseigenschaft hinzu, die der Benutzer des Panels zum Steuern dieses Verhaltens festlegen kann. Hier finden Sie die sehr einfache Definition der Abhängigkeitseigenschaft:

```CSharp
public static readonly DependencyProperty UseOppositeRCRatioProperty =
   DependencyProperty.Register("UseOppositeRCRatio", typeof(bool), typeof(BoxPanel), null);

public bool UseSquareCells
{
    get { return (bool)GetValue(UseOppositeRCRatioProperty); }
    set { SetValue(UseOppositeRCRatioProperty, value); }
}
```

Und so wirkt sich die Verwendung von `UseOppositeRCRatio` auf die Messlogik aus. Dabei werden tatsächlich lediglich die Ableitungen von `rowcount` und `colcount` von `maxrc` und das tatsächliche Seitenverhältnis geändert. Daraus ergeben sich für jede Zelle entsprechende Größenunterschiede. Wenn `UseOppositeRCRatio` **true** ist, wird der Wert des tatsächlichen Seitenverhältnisses vor dessen Verwendung für die Zählung der Zeilen und Spalten invertiert.

```CSharp
if (UseOppositeRCRatio) { aspectratio = 1 / aspectratio;}
```

## Szenario für `BoxPanel`


Das besondere Szenario für `BoxPanel` sieht so aus, dass es sich um einen Bereich handelt, in dem sich eine der Hauptdeterminante zur Aufteilung der Fläche aus der Kenntnis der Anzahl untergeordneter Elemente ergibt und in dem die bekannte verfügbare Fläche für den Bereich geteilt wird. Bereiche sind immanente Rechtecksformen. Viele Panels funktionieren so, dass die rechteckige Fläche in weitere Rechtecke geteilt wird. Genau diese Funktion übernimmt die [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)-Klasse für die zugehörigen Zellen. Im Fall von **Grid** wird die Größe der Zellen durch [**ColumnDefinition**](https://msdn.microsoft.com/library/windows/apps/br209324)- und [**RowDefinition**](https://msdn.microsoft.com/library/windows/apps/br227606)-Werte festgelegt, und Elemente deklarieren die exakte Zelle, in die sie über die angehängten Eigenschaften [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795) und [**Grid.Column**](https://msdn.microsoft.com/library/windows/apps/hh759774) einfließen. Zum Abrufen eines guten Layouts aus einem **Grid** muss zuvor normalerweise die Anzahl der untergeordneten Elemente bekannt sein, sodass genügend Zellen vorhanden sind und jedes untergeordnete Element seine angehängten Eigenschaften so festlegt, dass es in seine eigene Zelle passt.

Wie verhält es sich jedoch, wenn die Anzahl untergeordneter Elemente dynamisch ist? Das ist durchaus möglich. Der Code Ihrer App kann Sammlungen Elemente hinzufügen. Dies geschieht in Reaktion auf dynamische Laufzeitzustände, die Sie für wichtig genug erachten, um die Benutzeroberfläche zu aktualisieren. Wenn Sie eine Datenbindung zum Sichern von Sammlungen/Geschäftsobjekten verwenden, erfolgen der Abruf solcher Updates und die Aktualisierung der Benutzeroberfläche automatisch. Daher ist dies oftmals die bevorzugte Technik (siehe [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946)).

Aber nicht alle App-Szenarien eignen sich für die Datenbindung. Manchmal müssen Sie zur Laufzeit neue UI-Elemente erstellen und sichtbar machen. `BoxPanel` ist für dieses Szenario gedacht. Eine Änderung der Anzahl der untergeordneten Elemente ist kein Problem für `BoxPanel`, weil diese Anzahl in Berechnungen verwendet wird und sowohl die vorhandenen als auch die neuen untergeordneten Elemente in einem neuen Layout angepasst werden, sodass sie alle passen.

Ein erweitertes Szenario für das Erweitern von `BoxPanel` (hier nicht gezeigt) könnte sowohl dynamische untergeordnete Elemente umfassen als auch den [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921)-Wert eines untergeordneten Elements als stärkeren Faktor für die Größenanpassung einzelner Zellen verwenden. In diesem Szenario könnten auch die Zeilen- oder Spaltengrößen oder Nichtrasterformen variieren, sodass weniger Platz „verschwendet“ wird. Dies erfordert eine Strategie für die Einpassung von mehreren Rechtecken mit verschiedenen Größen und Seitenverhältnissen in ein einschließendes Rechteck, um ästhetische Aspekte ebenso wie die kleinste Größe zu berücksichtigen. `BoxPanel` verwendet solch eine Strategie nicht. Stattdessen wird der Platz mithilfe einer einfacheren Technik aufgeteilt. Die `BoxPanel`-Technik besteht darin, die kleinste Quadratzahl zu bestimmen, die größer als die Anzahl der untergeordneten Elemente ist. Beispielsweise würden neun Elemente in ein Quadrat mit den Abmessungen 3 × 3 passen. Für zehn Elemente wird ein Quadrat mit den Abmessungen 4 × 4 benötigt. Oftmals können Sie aber Elemente zuordnen und trotzdem eine Zeile oder Spalte aus dem Ausgangsquadrat entfernen, um Platz zu sparen. Im Beispiel mit der Anzahl 10 wäre auch ein Rechteck mit den Abmessungen 4 × 3 oder 3 × 4 passend.

Möglicherweise fragen Sie sich, warum das Panel nicht stattdessen die Abmessung 5 x 2 für zehn Elemente auswählt, da die Elementanzahl dann genau passen würde. In der Praxis wird die Größe von Panels aber als Rechteck festgelegt, das nur selten ein fest ausgerichtetes Seitenverhältnis aufweist. Die Technik mit den kleinsten Quadraten ist eine Methode, um die Größenfestlegungslogik so zu beeinflussen, dass sie ordnungsgemäß mit den typischen Layoutformen funktioniert und um zu verhindern, dass durch die Größenfestlegung eigenartige Seitenverhältnisse der Zellenformen auftreten.

**Hinweis**
Dieser Artikel ist für Windows 10-Entwickler bestimmt, die Apps für die universelle Windows-Plattform (UWP) schreiben. Wenn Sie für Windows 8.x oder Windows Phone 8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

??

## Verwandte Themen


**Referenz**
[**FrameworkElement.ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)

[**FrameworkElement.MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)

[**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511)

**Konzepte**
[Ausrichtung, Rand und Abstand](alignment-margin-padding.md)

??

??





<!--HONumber=Mar16_HO4-->


