---
author: Jwmsft
ms.assetid: 54CC0BD4-1961-44D7-AB40-6E8B58E42D65
title: Zeichnen von Formen
description: "Erfahren Sie, wie Sie Formen wie Ellipsen, Rechtecke, Polygone und Pfade zeichnen. Mithilfe der Klasse Path visualisieren Sie eine ziemlich komplexe vektorbasierte Zeichnungssprache in einer XAML-Benutzeroberfläche. Beispielsweise können Sie auf diese Weise Bézierkurven zeichnen."
ms.sourcegitcommit: 04a3c2dabc4b115faf4b06aa3d3a59c5c38ab95f
ms.openlocfilehash: 42514e5119b646d196e0a1c7d3099ebed2225c69

---
# Zeichnen von Formen

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


** Wichtige APIs **

-   [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355)
-   [**Windows.UI.Xaml.Shapes-Namespace**](https://msdn.microsoft.com/library/windows/apps/BR243401)
-   [**Windows.UI.Xaml.Media-Namespace**](https://msdn.microsoft.com/library/windows/apps/BR243045)

Erfahren Sie, wie Sie Formen wie Ellipsen, Rechtecke, Polygone und Pfade zeichnen. Mithilfe der Klasse [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355) visualisieren Sie eine ziemlich komplexe vektorbasierte Zeichnungssprache in einer XAML-Benutzeroberfläche. Beispielsweise können Sie auf diese Weise Bézierkurven zeichnen.

## Einführung

Zwei Sätze von Klassen definieren einen Bereich in der XAML-Benutzeroberfläche: [**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377)-Klassen und [**Geometry**](https://msdn.microsoft.com/library/windows/apps/BR210041)-Klassen. Der Hauptunterschied zwischen diesen Klassen besteht darin, dass einer **Shape**-Klasse ein Pinsel zugeordnet ist und diese auf dem Bildschirm gerendert werden kann, während eine **Geometry**-Klasse einfach einen Bereich definiert und nur gerendert wird, wenn sie Informationen zu einer anderen UI-Eigenschaft beiträgt. Sie können sich eine **Shape**-Klasse als [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) vorstellen, dessen Umrandung durch eine **Geometry**-Klasse definiert ist. In diesem Thema werden hauptsächlich die **Shape**-Klassen behandelt.

Die [**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377)-Klassen sind [**Line**](https://msdn.microsoft.com/library/windows/apps/BR243345), [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343), [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371), [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359), [**Polyline**](https://msdn.microsoft.com/library/windows/apps/BR243365) und [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355). **Path** ist interessant, da Sie mit dieser Klasse beliebige Geometrien definieren können, und die Klasse [**Geometry**](https://msdn.microsoft.com/library/windows/apps/BR210041) stellt eine Möglichkeit zum Definieren der Teile eines **Path** dar.

## Füllungen und Striche für Formen

Damit eine [**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377)-Klasse auf dem App-Canvas gerendert wird, müssen Sie ihr einen [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076) zuweisen. Legen Sie die [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)-Eigenschaft der **Shape**-Klasse auf den gewünschten **Brush** fest. Weitere Informationen zu Pinseln finden Sie unter [Verwenden von Pinseln](using-brushes.md).

Eine [**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377)-Klasse kann auch einen [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) aufweisen. Dies ist eine Linie, die um den Rand der Form gezeichnet wird. Ein **Stroke** erfordert auch einen [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076). Dieser definiert die Darstellung und sollte einen Wert ungleich Null für [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness) aufweisen. **StrokeThickness** ist eine Eigenschaft, die die Stärke des Rands um die Form definiert. Wenn Sie keinen **Brush**-Wert für **Stroke** angeben oder **StrokeThickness** auf 0 festlegen, wird kein Rahmen um die Form gezeichnet.

## Ellipse

Eine [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343) ist eine Form mit einem kurvenförmigen Rand. Zum Erstellen einer einfachen **Ellipse** geben Sie [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751), [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) und einen [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076) für [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) an.

Im nächsten Beispiel wird eine [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343) mit einer [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) von 200 und einer [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) von 200 erstellt, die die Farbe [**SteelBlue**](https://msdn.microsoft.com/library/windows/apps/Hh748056) für [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) als [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) verwendet.

```xml
<Ellipse Fill="SteelBlue" Height="200" Width="200" />
```

Dies ist die gerenderte [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343).

![Eine gerenderte Ellipse](images/shapes-ellipse.jpg)

In diesem Fall ist die [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343) das, was die meisten Menschen als einen Kreis bezeichnen würden. So werden Kreisformen in XAML definiert: Sie verwenden eine **Ellipse** mit gleicher [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) und [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718).

Wenn eine [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343) in einem UI-Layout positioniert wird, wird davon ausgegangen, dass ihre Größe einem Rechteck dieser [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) und [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) entspricht. Der Bereich außerhalb des Rands weist kein Rendering auf, gehört jedoch weiterhin zur Größe des Layoutschlitzes.

Eine Gruppe von 6 [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343)-Elementen ist Teil der Steuerelementvorlage für das [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/BR227538)-Steuerelement. 2 konzentrische **Ellipse**-Elemente sind Teil eines [**RadioButton**](https://msdn.microsoft.com/library/windows/apps/BR227544).

## <span id="Rectangle"></span><span id="rectangle"></span><span id="RECTANGLE"></span>Rectangle

Ein [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) ist eine vierseitige Form, bei der die gegenüberliegenden Seiten gleich sind. Um ein einfaches **Rectangle** zu erstellen, geben Sie eine [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751), eine [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) und einen Wert für [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) an.

Sie können die Ecken eines [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) abrunden. Um abgerundete Ecken zu erstellen, geben Sie einen Wert für die Eigenschaften [**RadiusX**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusx.aspx) und [**RadiusY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusy) an. Diese Eigenschaften geben die x-Achse und die y-Achse einer Ellipse an, die die Kurven der Ecken angibt. Der zulässige Maximalwert für **RadiusX** ist [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) geteilt durch zwei. Der zulässige Maximalwert für **RadiusY** ist [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) geteilt durch zwei.

Im nächsten Beispiel wird ein [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) mit einer [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) von 200 und einer [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) von 100 erstellt. Es verwendet den Wert [**Blue**](https://msdn.microsoft.com/library/windows/apps/Hh747837) von [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) als [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) und den Wert [**Black**](https://msdn.microsoft.com/library/windows/apps/Hh747833) von **SolidColorBrush** als [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke). Die [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness) wird als 3 festgelegt. Die [**RadiusX**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusx.aspx)-Eigenschaft wird als 50 und die [**RadiusY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusy)-Eigenschaft wird als 10 festgelegt. Damit erhält das **Rectangle** abgerundete Ecken.

```xml
<Rectangle Fill="Blue"
           Width="200"
           Height="100"
           Stroke="Black"
           StrokeThickness="3"
           RadiusX="50"
           RadiusY="10" />
           ```

Here's the rendered [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371).

![A rendered Rectangle.](images/shapes-rectangle.jpg)

**Tip**  There are some scenarios for UI definitions where instead of using a [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371), a [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209250) might be more appropriate. If your intention is to create a rectangle shape around other content, it might be better to use **Border** because it can have child content and will automatically size around that content, rather than using the fixed dimensions for height and width like **Rectangle** does. A **Border** also has the option of having rounded corners if you set the [**CornerRadius**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.border.cornerradius) property.

 

On the other hand, a [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) is probably a better choice for control composition. A **Rectangle** shape is seen in many control templates because it's used as a "FocusVisual" part for focusable controls. Whenever the control is in a "Focused" visual state, this rectangle is made visible, in other states it's hidden.

## Polygon

A [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359) is a shape with a boundary defined by an arbitrary number of points. The boundary is created by connecting a line from one point to the next, with the last point connected to the first point. The [**Points**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.shapes.polygon.points.aspx) property defines the collection of points that make up the boundary. In XAML, you define the points with a comma-separated list. In code-behind you use a [**PointCollection**](https://msdn.microsoft.com/library/windows/apps/BR210220) to define the points and you add each individual point as a [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) value to the collection.

You don't need to explicitly declare the points such that the start point and end point are both specified as the same [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) value. The rendering logic for a [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359) assumes that you are defining a closed shape and will connect the end point to the start point implicitly.

The next example creates a [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359) with 4 points set to `(10,200)`, `(60,140)`, `(130,140)`, and `(180,200)`. It uses a [**LightBlue**](https://msdn.microsoft.com/library/windows/apps/Hh747960) value of [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) for its [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill), and has no value for [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) so it has no perimeter outline.

```xml
<Polygon Fill="LightBlue"
         Points="10,200,60,140,130,140,180,200" />
```

Dies ist das gerenderte [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359).

![Ein gerendertes Polygon](images/shapes-polygon.jpg)

**Tipp**  Ein [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870)-Wert wird in XAML oft als Typ für andere Szenarien als das Deklarieren der Scheitelpunkte von Formen verwendet. Ein **Point** ist beispielsweise Teil der Ereignisdaten für Fingereingabeereignisse, damit Sie ermitteln können, wo genau in Koordinatenbereich die Fingereingabeaktion stattgefunden hat. Weitere Informationen zu **Point** und dessen Verwendung in XAML oder Code finden Sie in der API-Referenz für [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870).

 

## Linie

Eine [**Line**](https://msdn.microsoft.com/library/windows/apps/BR243345) ist eine einfache Verbindung zweier Punkte in einem Koordinatenbereich. Eine **Line** ignoriert alle Werte für [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill), da sie keinen Innenbereich hat. Geben Sie bei einer **Line** Werte für die Eigenschaften [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) und [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness) an, da die **Line** andernfalls nicht gerendert werden kann.

Sie verwenden keine [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870)-Werte, um eine [**Line**](https://msdn.microsoft.com/library/windows/apps/BR243345)-Form anzugeben, sondern diskrete [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)-Werte für [**X1**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.shapes.line.x1.aspx), [**Y1**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.shapes.line.y1.aspx), [**X2**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.shapes.line.x2.aspx) und [**Y2**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.shapes.line.y2.aspx). Dadurch wird das Markup für horizontale oder vertikale Linien minimiert. Beispielsweise definiert `<Line Stroke="Red" X2="400"/>` eine horizontale Linie mit einer Länge von 400 Pixeln. Die anderen X,Y-Eigenschaften sind standardmäßig 0. Mit diesem XAML wird daher bei Punkten eine Linie von `(0,0)` bis `(400,0)` gezeichnet. Anschließend können Sie [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) zum Verschieben der gesamten **Line** verwenden, wenn sie an einem anderen Punkt als (0,0) beginnen soll.

## <span id="_Polyline"></span><span id="_polyline"></span><span id="_POLYLINE"></span> Polyline

Eine [**Polyline**](https://msdn.microsoft.com/library/windows/apps/BR243365) ähnelt einem [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359), da bei beiden die Grenze der Form durch eine Reihe von Punkten definiert wird. Bei einer **Polyline** wird jedoch der letzte Punkt nicht mit dem ersten verbunden.

**Hinweis**   Sie könnten zwar in den [**Points**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx) explizit einen identischen Anfangs- und Endpunkt für [**Polyline**](https://msdn.microsoft.com/library/windows/apps/BR243365) festlegen. In diesem Fall sollten Sie jedoch wahrscheinlich eher ein [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359) verwenden.

 

Wenn Sie die [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) einer [**Polyline**](https://msdn.microsoft.com/library/windows/apps/BR243365) angeben, füllt **Fill** den Innenraum der Form. Dies gilt auch, wenn der Anfangs- und Endpunkt der [**Points**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx), die für **Polyline** festgelegt wurden, keine Überschneidungen aufweisen. Wenn Sie keine **Fill** angeben, ähnelt die **Polyline** dem Rendering für mehrere einzelne [**Line**](https://msdn.microsoft.com/library/windows/apps/BR243345)-Elemente, bei denen sich die Anfangs- und Endpunkte aufeinander folgender Linien überschneiden.

Wie bei einem [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359) definiert die [**Points**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx)-Eigenschaft die Sammlung von Punkten, die zusammen die Grenze ergeben. In XAML definieren Sie die Punkte mit einer durch Komma getrennten Liste. Im CodeBehind verwenden Sie eine [**PointCollection**](https://msdn.microsoft.com/library/windows/apps/BR210220), um die Punkte zu definieren, und fügen der Sammlung jeden einzelnen Punkt als eine [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870)-Struktur hinzu.

Im nächsten Beispiel wird eine [**Polyline**](https://msdn.microsoft.com/library/windows/apps/BR243365) mit vier Punkten bei `(10,200)`, `(60,140)`, `(130,140)` und `(180,200)` erstellt. Es ist ein [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) definiert, jedoch keine [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill).

```xml
<Polyline Stroke="Black"
        StrokeThickness="4"
        Points="10,200,60,140,130,140,180,200" />
```

Dies ist die gerenderte [**Polyline**](https://msdn.microsoft.com/library/windows/apps/BR243365). Beachten Sie, dass der erste und letzte Punkt nicht durch die [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke)-Umrandung miteinander verbunden sind, anders als bei einem [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359).

![Eine gerenderte Polylinie](images/shapes-polyline.jpg)

## Path

Ein [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355) ist die vielseitigste [**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377)-Klasse, da Sie mit dieser beliebige Geometrien definieren können. Diese Vielseitigkeit führt jedoch auch zu Komplexität. Betrachten wir nun, wie ein einfacher **Path** in XAML erstellt wird.

Sie definieren die Geometrie des Pfads mit der [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data)-Eigenschaft. Es gibt zwei Techniken zum Festlegen von **Data**:

-   Sie können einen Zeichenfolgenwert für [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) in XAML festlegen. In diesem Format verwendet der **Path.Data**-Wert ein Serialisierungsformat für Grafiken. Nachdem er einmal festgelegt wurde, wird dieser Wert normalerweise nicht mehr als Zeichenfolge bearbeitet. Stattdessen arbeiten Sie mit Entwicklungstools in einer Entwurfs- oder Zeichnungsmetapher auf einer Oberfläche. Anschließend speichern oder exportieren Sie die Ausgabe und erhalten eine XAML-Datei oder ein XAML-Zeichenfolgenfragment mit **Path.Data**-Informationen.
-   Sie können die [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data)-Eigenschaft für ein einzelnes [**Geometry**](https://msdn.microsoft.com/library/windows/apps/BR210041)-Objekt festlegen. Die kann im Code oder in XAML erfolgen. Diese einzelne **Geometry** ist typischerweise eine [**GeometryGroup**](https://msdn.microsoft.com/library/windows/apps/BR210041group), die als Container dient und mehrere Geometriedefinitionen für das Objektmodell in einem Objekt zusammenfassen kann. In aller Regel wird diese Vorgehensweise gewählt, um mindestens eine der Kurven und komplexen Formen, die als [**Segments**](https://msdn.microsoft.com/library/windows/apps/BR210164)-Werte definiert werden können, für ein [**PathFigure**](https://msdn.microsoft.com/library/windows/apps/BR210143) zu verwenden, beispielsweise [**BezierSegment**](https://msdn.microsoft.com/library/windows/apps/BR228068).

Dieses Beispiel zeigt einen [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355), der möglicherweise daraus entstanden ist, dass Blend für Visual Studio zur Erzeugung einiger Vektorformen verwendet wurde und das Ergebnis als XAML gespeichert wurde. Der vollständige **Path** besteht aus einem Bézierkurven- und einem Liniensegment. Dieses Beispiel soll in erster Linie die Elemente im [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data)-Serialisierungsformat zeigen und verstehen helfen, wofür die Zahlen stehen.

Diese [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) beginnen mit dem move-Befehl, der durch „M“ gekennzeichnet ist und einen Anfangspunkt für den Pfad erstellt.

Das erste Segment ist eine kubische Bézierkurve, die bei `(100,200)` beginnt und bei `(400,175)` endet. Sie wird mit zwei Kontrollpunkten bei `(100,25)` und `(400,350)` gezeichnet. Dieses Segment wird durch den „C“-Befehl in der Zeichenfolge des [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data)-Attributs angegeben.

Das zweite Segment beginnt mit dem Befehl „H“ für eine absolute horizontale Linie. Dieser gibt an, dass eine Linie vom bisherigen Endpunkt des Pfads, `(400,175)`, zum neuen Endpunkt, `(280,175)`, gezeichnet werden soll. Da es sich um einen Befehl für eine horizontale Linie handelt, ist der angegebene Wert eine x-Koordinate.

```xml
<Path Stroke="DarkGoldenRod" 
      StrokeThickness="3"
      Data="M 100,200 C 100,25 400,350 400,175 H 280" />
      ```

Here's the rendered [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355).

![A rendered Path.](images/shapes-path.jpg)

The next example shows a usage of the other technique we discussed: a [**GeometryGroup**](https://msdn.microsoft.com/library/windows/apps/BR210041group) with a [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/BR210168). This example exercises some of the contributing geometry types that can be used as part of a **PathGeometry**: [**PathFigure**](https://msdn.microsoft.com/library/windows/apps/BR210143) and the various elements that can be a segment in [**PathFigure.Segments**](https://msdn.microsoft.com/library/windows/apps/BR210164).

```xml
<Path Stroke="Black" StrokeThickness="1" Fill="#CCCCFF">
            <Path.Data>
              <GeometryGroup>
                  <RectangleGeometry Rect="50,5 100,10" />
                  <RectangleGeometry Rect="5,5 95,180" />
                  <EllipseGeometry Center="100, 100" RadiusX="20" RadiusY="30"/>
                  <RectangleGeometry Rect="50,175 100,10" />
                  <PathGeometry>
                    <PathGeometry.Figures>
                      <PathFigureCollection>
                        <PathFigure IsClosed="true" StartPoint="50,50">
                          <PathFigure.Segments>
                            <PathSegmentCollection>
                              <BezierSegment Point1="75,300" Point2="125,100" Point3="150,50"/>
                              <BezierSegment Point1="125,300" Point2="75,100"  Point3="50,50"/>
                            </PathSegmentCollection>
                          </PathFigure.Segments>
                        </PathFigure>
                      </PathFigureCollection>
                    </PathGeometry.Figures>
                  </PathGeometry>               
              </GeometryGroup>
            </Path.Data>
          </Path>
```

Durch die Verwendung von [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/BR210168) wird das Ergebnis lesbarer als durch das Ausfüllen einer [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data)-Zeichenfolge. Andererseits verwendet [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) eine Syntax, die mit Imagepfaddefinitionen für skalierbare Vektorgrafiken (SVG) kompatibel ist. Daher kann es nützlich sein, Grafiken aus SVG oder als Ausgabe von einem Tool wie etwa Blend zu portieren.

 

 







<!--HONumber=Jun16_HO3-->


