---
ms.assetid: F46D5E18-10A3-4F7B-AD67-76437C77E4BC
title: Transformationen – Übersicht
description: Hier erfahren Sie, wie Sie Transformationen in der Windows-Runtime-API durch Ändern des relativen Koordinatensystems der UI-Elemente verwenden.
---

# Transformationen – Übersicht

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Hier erfahren Sie, wie Sie Transformationen in der Windows-Runtime-API durch Ändern des relativen Koordinatensystems der UI-Elemente verwenden. Hiermit kann die Darstellung individueller XAML-Elemente angepasst werden, z. B. durch Skalieren, Drehen oder Transformieren der Position im zweidimensionalen Raum (XY).

## <span id="What_is_a_transform_"></span><span id="what_is_a_transform_"></span><span id="WHAT_IS_A_TRANSFORM_"></span>Was ist eine Transformation?

Eine *Transformation* definiert, wie Punkte aus einem Koordinatenbereich einem anderen Koordinatenbereich zugeordnet (transformiert) werden. Beim Anwenden von Transformationen auf UI-Elemente wird die Darstellung des UI-Elements auf dem Bildschirm geändert.

Transformationen können in vier breite Bereiche klassifiziert werden: Übersetzung, Drehung, Skalierung und Neigung (oder Scherung). Zum Ändern der Darstellung von UI-Elementen anhand von Grafik-APIs erstellen Sie am besten Transformationen, die nur jeweils einen Vorgang definieren. Auf diese Weise definiert die Windows-Runtime eine diskrete Klasse für jede der Transformationsklassifikationen:

-   [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027): Übersetzt ein Element im X-Y-Raum durch Festlegen der Werte für [**X**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.translatetransform.x.aspx) und [**Y**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.translatetransform.y).
-   [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940): Skaliert die Transformation auf der Grundlage eines Mittelpunkts durch Festlegen von Werten für [**CenterX**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.scaletransform.centerx.aspx), [**CenterY**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.scaletransform.centery.aspx), [**ScaleX**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.scaletransform.scalex.aspx) und [**ScaleY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.scaleyproperty).
-   [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932): Zur Drehung im X-Y-Raum durch Festlegen von Werten für [**Angle**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.rotatetransform.angle.aspx), [**CenterX**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.rotatetransform.centerx.aspx) und [**CenterY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.rotatetransform.centery).
-   [**SkewTransform**](https://msdn.microsoft.com/library/windows/apps/BR242950): Zur Neigung bzw. Scherung im X-Y-Raum durch Festlegen von Werten für [**AngleX**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.skewtransform.anglex.aspx), [**AngleY**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.skewtransform.angley.aspx), [**CenterX**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.skewtransform.centerx.aspx) und [**CenterY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.centeryproperty).

Von diesen Werten werden Sie für UI-Szenarien höchstwahrscheinlich am häufigsten [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) und [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940) verwenden.

Zwei Windows-Runtime-Klassen unterstützen das Kombinieren von Transformationen: [**CompositeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228105) und [**TransformGroup**](https://msdn.microsoft.com/library/windows/apps/BR243022). In einem **CompositeTransform**-Element werden Transformationen in folgender Reihenfolge angewendet: Skalieren, Neigen, Drehen, Übersetzen. Verwenden Sie **TransformGroup** anstelle von **CompositeTransform**, wenn die Transformationen in einer anderen Reihenfolge angewendet werden sollen. Weitere Informationen finden Sie unter [**CompositeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228105).

## <span id="Transforms_and_layout"></span><span id="transforms_and_layout"></span><span id="TRANSFORMS_AND_LAYOUT"></span>Transformationen und Layout

Im XAML-Layout werden Transformationen nach Abschluss des Layoutdurchlaufs angewendet. Berechnungen zum verfügbaren Raum und andere Layout-Entscheidungen werden also vor der Anwendung der Transformationen durchgeführt. Da das Layout an erster Stelle steht, werden Sie mitunter unerwartete Ergebnisse erhalten, wenn Sie Elemente transformieren, die sich in einer [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)-Zelle oder in einem ähnlichen Layoutcontainer befinden, der Raum im Layout zuordnet. Das transformierte Element wird möglicherweise abgeschnitten oder verdeckt dargestellt, da in einem Bereich gezeichnet wird, für den bei der Raumeinteilung innerhalb des übergeordneten Containers die Abmessungen nach der Transformation nicht berechnet wurden. Experimentieren Sie ggf. mit den Transformationsergebnissen und passen Sie die Einstellungen an. Anstatt beispielsweise auf ein adaptives Layout und Größenanpassung mit Sternvariabler zu vertrauen, müssen Sie möglicherweise die **Center**-Eigenschaften ändern oder die Pixelmaße für den Layoutraum deklarieren, um sicherzustellen, dass das übergeordnete Element genügend Platz zuweist.

**Migrationshinweis: **In Windows Presentation Foundation (WPF) stand eine **LayoutTransform**-Eigenschaft zur Verfügung, die Transformationen vor dem Layoutdurchlauf anwendet. Windows-Runtime-XAML unterstützt jedoch keine **LayoutTransform**-Eigenschaft. (In Microsoft Silverlight war diese Eigenschaft auch nicht vorhanden.)

## <span id="Applying_a_transform_to_a_UI_element"></span><span id="applying_a_transform_to_a_ui_element"></span><span id="APPLYING_A_TRANSFORM_TO_A_UI_ELEMENT"></span>Anwenden einer Transformation auf ein UI-Element

Wenn Sie eine Transformation auf ein Objekt anwenden, legen Sie damit normalerweise die [**UIElement.RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980)-Eigenschaft fest. Durch Festlegen dieser Eigenschaft wird das Objekt nicht wirklich Pixel für Pixel geändert. Tatsächlich wendet die Eigenschaft die Transformation innerhalb des lokalen Koordinatenraums an, in dem das Objekt vorhanden ist. Die Renderlogik und der Vorgang (Post-Layout) rendern die kombinierten Koordinatenbereiche. Hierdurch entsteht der Eindruck, als hätte das Objekt sein Erscheinungsbild und möglicherweise auch die Layoutposition geändert (wenn [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) angewendet wurde).

Standardmäßig befindet sich der Mittelpunkt jeder Rendertransformation am Ursprung des lokalen Koordinatensystems des Zielobjekts (0,0). Die einzige Ausnahme ist eine [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027), für die keine Mittelpunkteigenschaften festgelegt werden können, da der Übersetzungseffekt unabhängig vom Mittelpunkt derselbe ist. Alle anderen Transformationen verfügen über Eigenschaften, die **CenterX**- und **CenterY**-Werte festlegen.

Wenn Sie Transformationen mit [**UIElement.RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980)-Elementen verwenden, beachten Sie, dass eine weitere Eigenschaft für [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) vorhanden ist, die sich auf das Verhalten der Transformation auswirkt: [**RenderTransformOrigin**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.rendertransformorigin.aspx). Die **RenderTransformOrigin**-Eigenschaft deklariert, ob sich die gesamte Transformation auf den Standardpunkt (0,0) eines Elements oder einen anderen Ursprungspunkt innerhalb des relativen Koordinatenraums des Elements beziehen soll. Normalerweise wird mit (0,0) die Transformation in der oberen linken Ecke platziert. Je nachdem, welcher Effekt erzielt werden soll, empfiehlt sich die Änderung des **RenderTransformOrigin**-Elements anstelle der Anpassung der **CenterX**- und **CenterY**-Werte für die Transformationen. Beachten Sie: Wenn Sie sowohl **RenderTransformOrigin** als auch **CenterX**-/**CenterY**-Werte anwenden, kann dies zu verwirrenden Ergebnissen führen. Dies gilt insbesondere, wenn Sie die Werte animieren.

Bei Treffertests reagiert ein Objekt, auf das eine Transformation angewendet wird, weiterhin erwartungsgemäß und im Einklang mit der visuellen Darstellung im zweidimensionalen Raum (XY) auf Eingaben. Haben Sie beispielsweise ein [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)-Element verwendet, um ein [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371)-Element auf der Benutzeroberfläche seitwärts um 400 Pixel zu verschieben, reagiert das **Rectangle**-Element auf [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.pointerpressed.aspx)-Ereignisse, sobald der Benutzer den Punkt berührt, an dem das **Rectangle**-Element dargestellt wird. Wenn der Benutzer den Bereich berührt, in dem sich das **Rectangle**-Element vor der Übersetzung befand, erhalten Sie keine falschen Ereignisse. Im Hinblick auf Z-Index-Überlegungen, die sich auf Treffertests auswirken, macht das Anwenden von Transformationen keinen Unterschied. Der Z-Index, der bestimmt, von welchem Element Eingabeereignisse für einen Punkt im X-Y-Raum gehandhabt werden, wird immer noch anhand der untergeordneten Reihenfolge ausgewertet, die im Container deklariert ist. Diese Reihenfolge entspricht normalerweise der Reihenfolge, in der die Elemente im XAML-Code deklariert werden, obwohl für untergeordnete Elemente eines [**Canvas**](https://msdn.microsoft.com/library/windows/apps/BR209267)-Objekts die Reihenfolge durch Anwenden der angehängten [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.zindex.aspx)-Eigenschaft auf untergeordnete Elemente angepasst werden kann.

## <span id="Other_transform_properties"></span><span id="other_transform_properties"></span><span id="OTHER_TRANSFORM_PROPERTIES"></span>Andere Transformationseigenschaften

-   [**Brush.Transform**](https://msdn.microsoft.com/library/windows/apps/BR228082), [**Brush.RelativeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228080): Beeinflusst, wie ein [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076)-Element den Koordinatenraum innerhalb des Bereichs verwendet, auf den das **Brush**-Element angewendet wird, um visuelle Eigenschaften (etwa Vorder- und Hintergründe) festzulegen. Diese Transformationen sind für die meisten gängigen Brush-Elemente (mit denen normalerweise Volltonfarben mit [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) festgelegt werden) zwar nicht relevant, sie können jedoch beim Zeichnen in Bereichen mit einem [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101)- oder [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108)-Element hilfreich sein.
-   [**Geometry.Transform**](https://msdn.microsoft.com/library/windows/apps/BR210066): Sie können diese Eigenschaft zum Anwenden einer Transformation auf ein geometrisches Objekt anwenden, bevor das geometrische Objekt für einen [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/BR243356) -Eigenschaftswert verwendet wird.

## <span id="Animating_a_transform"></span><span id="animating_a_transform"></span><span id="ANIMATING_A_TRANSFORM"></span>Animieren einer Transformation

[**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006)-Objekte können animiert werden. Um ein **Transform**-Element zu animieren, wenden Sie eine kompatible Animation auf die entsprechende Eigenschaft an. Normalerweise bedeutet das, dass Sie [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136)- oder [**DoubleAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR243136usingkeyframes)-Objekte zum Definieren der Animation verwenden, da alle Transformationseigenschaften vom Typ [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) sind. Animationen, die sich auf eine für einen [**UIElement.RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980)-Wert verwendete Transformation auswirken, werden selbst dann nicht als abhängige Animationen betrachtet, wenn Ihre Dauer nicht Null ist. Weitere Informationen zu abhängigen Animationen finden Sie unter [Storyboardanimationen](storyboarded-animations.md).

Wenn Sie Eigenschaften animieren, um einen ähnlichen Darstellungseffekt zu erzielen wie bei einer Transformation (etwa, wenn Sie die [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) und [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718)-Eigenschaften eines [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706)-Elements animieren, anstatt ein [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)-Element anzuwenden), werden diese nahezu immer als abhängige Animationen behandelt. Die Animationen müssen aktiviert werden, und bei dieser Animation können beträchtliche Leistungsprobleme auftreten, insbesondere wenn während der Animation des Objekts Benutzerinteraktionen unterstützt werden sollen. Aus diesem Grund empfiehlt sich die Verwendung einer Transformation und deren Animation anstelle der Animation einer anderen Eigenschaft, bei der die Animation als abhängige Animation behandelt werden würde.

Zur Ausrichtung auf die Transformation muss ein [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006)-Element als Wert für das [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980)-Element vorhanden sein. Normalerweise wird ein Element für den entsprechenden Transformationstyp im ursprünglichen XAML-Code platziert. Manchmal sind keine Eigenschaften für diese Transformation festgelegt.

Typischerweise wird eine indirekte Zieltechnik verwendet, um die Animationen auf die Eigenschaften einer Transformation anzuwenden. Weitere Informationen zur Syntax für indirektes Zielen finden Sie unter [Storyboardanimationen](storyboarded-animations.md) und [Eigenschaftspfadsyntax](https://msdn.microsoft.com/library/windows/apps/Mt185586).

Standardformate für Steuerelemente definieren manchmal Animationen von Transformationen als Teil des Verhaltens des visuellen Zustands. Beispiel: Die visuellen Zustände für das [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/BR227538)-Element verwenden animierte [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932)-Werte, um die Punkte im Ring zu drehen.

Im Anschluss folgt ein einfaches Beispiel für die Animation einer Transformation. In diesem Fall wird das [**Angle**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.rotatetransform.angle.aspx)-Element eines [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932)-Elements animiert, um ein [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371)-Element um den visuellen Mittelpunkt zu drehen. In diesem Beispiel wird das **RotateTransform**-Element benannt, daher ist kein indirektes Animationsziel erforderlich. Alternativ kann die Transformation unbenannt bleiben, und Sie können das Element benennen, auf das die Transformation angewendet wird und eine indirekte Ausrichtung verwenden (etwa `(UIElement.RenderTransform).(RotateTransform.Angle)`).

```xml
<StackPanel Margin="15">
  <StackPanel.Resources>
    <Storyboard x:Name="myStoryboard">
      <DoubleAnimation
       Storyboard.TargetName="myTransform"
       Storyboard.TargetProperty="Angle"
       From="0" To="360" Duration="0:0:5" 
       RepeatBehavior="Forever" />
    </Storyboard>
  </StackPanel.Resources>
  <Rectangle Width="50" Height="50" Fill="RoyalBlue"
   PointerPressed="StartAnimation">
    <Rectangle.RenderTransform>
      <RotateTransform x:Name="myTransform" Angle="45" CenterX="25" CenterY="25" />
    </Rectangle.RenderTransform>
  </Rectangle>
</StackPanel>
```

```xml
void StartAnimation (object sender, RoutedEventArgs e) {
    myStoryboard.Begin();
}
```

## <span id="Accounting_for_coordinate_frames_of_reference_at_run_time"></span><span id="accounting_for_coordinate_frames_of_reference_at_run_time"></span><span id="ACCOUNTING_FOR_COORDINATE_FRAMES_OF_REFERENCE_AT_RUN_TIME"></span>Angeben von Referenz-Koordinatennetzen zur Laufzeit

[**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) verfügt über eine Methode mit der Bezeichnung [**TransformToVisual**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.transformtovisual.aspx), die ein [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006)-Element generieren kann, das die Referenz-Koordinatennetze für zwei UI-Elemente abgleicht. So können Sie Elemente mit dem standardmäßigen Referenz-Koordinatennetz der App vergleichen, wenn der Stamm der visuellen Struktur als erster Parameter übergeben wird. Dies kann hilfreich sein, wenn ein Eingabeereignis von einem anderen Element erfasst wurde, oder wenn Layoutverhalten ohne das Anfordern einer Layoutübergabe prognostiziert werden soll.

Von Zeigerereignissen abgerufene Daten bieten Zugriff auf eine [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/BR212141)-Methode, wobei ein *relativeTo*-Parameter zum Ändern des Referenz-Koordinatennetzes in ein spezifisches Element anstelle des App-Standards angegeben werden kann. Mit diesem Ansatz wird schlicht eine Übersetzungstransformation intern angewendet, und die X-Y-Koordinatendaten werden beim Erstellen des zurückgegebenen [**PointerPoint**](https://msdn.microsoft.com/library/windows/apps/BR242038)-Elements transformiert.

## <span id="Describing_a_transform_mathematically"></span><span id="describing_a_transform_mathematically"></span><span id="DESCRIBING_A_TRANSFORM_MATHEMATICALLY"></span>Mathematische Beschreibung einer Transformation

Eine Transformation kann als Transformationsmatrix beschrieben werden. Mit einer 3x3-Matrix werden die Transformationen in einer zweidimensionalen X-Y-Ebene beschrieben. Affine Transformationsmatrizes können multipliziert werden und so eine Reihe linearer Transformationen ergeben, z. B. Drehen und Neigen (Scheren), gefolgt von Übersetzen. Die finale Spalte einer affinen Transformationsmatrix ist gleich (0, 0, 1). Demzufolge müssen Sie in der mathematischen Beschreibung lediglich die Member der ersten beiden Spalten angeben.

Die mathematische Beschreibung einer Transformation ist ggf. hilfreich, wenn Sie über mathematisches Fachwissen verfügen oder mit Grafikprogrammierungstechniken vertraut sind, die auch Matrizen zum Beschreiben von Transformationen von Koordinatenraum verwenden. Es gibt eine von [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006) abgeleitete Klasse, mit der Sie eine Transformation direkt als 3x3-Matrix beschreiben können: [**MatrixTransform**](https://msdn.microsoft.com/library/windows/apps/BR210137). **MatrixTransform** besitzt eine [**Matrix**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.matrixtransform.matrix.aspx)-Eigenschaft, die eine Struktur mit sechs Eigenschaften enthält: [**M11**](https://msdn.microsoft.com/library/windows/apps/Hh673847), [**M12**](https://msdn.microsoft.com/library/windows/apps/Hh673853), [**M21**](https://msdn.microsoft.com/library/windows/apps/Hh673851), [**M22**](https://msdn.microsoft.com/library/windows/apps/Hh673849), [**OffsetX**](https://msdn.microsoft.com/library/windows/apps/Hh673810) und [**OffsetY**](https://msdn.microsoft.com/library/windows/apps/Hh673816). Jede [**Matrix**](https://msdn.microsoft.com/library/windows/apps/BR210127)-Eigenschaft verwendet einen **Double**-Wert und entspricht den sechs relevanten Werten (Spalten 1 und 2) einer affinen Transformationsmatrix.

|                                             |                                             |     |
|---------------------------------------------|---------------------------------------------|-----|
| [**M11**](https://msdn.microsoft.com/library/windows/apps/Hh673847)         | [**M21**](https://msdn.microsoft.com/library/windows/apps/Hh673851)         | 0   |
| [**M12**](https://msdn.microsoft.com/library/windows/apps/Hh673853)         | [**M22**](https://msdn.microsoft.com/library/windows/apps/Hh673849)         | 0   |
| [**OffsetX**](https://msdn.microsoft.com/library/windows/apps/Hh673810) | [**OffsetY**](https://msdn.microsoft.com/library/windows/apps/Hh673816) | 1   |

 

Alle mit einem [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)-, [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940)-, [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932)- oder [**SkewTransform**](https://msdn.microsoft.com/library/windows/apps/BR242950)-Objekt beschreibbaren Transformationen können ebenfalls mit einem [**MatrixTransform**](https://msdn.microsoft.com/library/windows/apps/BR210137)-Element mit einem [**Matrix**](https://msdn.microsoft.com/library/windows/apps/BR210127)-Wert beschrieben werden. Normalerweise werden allerdings das **TranslateTransform**-Element und die anderen Elemente verwendet, da die Eigenschaften für diese Transformationsklassen einfacher in Konzepte zu fassen sind als das Festlegen der Vektorkomponenten in einem **Matrix**-Element. Es ist auch einfacher, die diskreten Eigenschaften von Transformationen zu animieren. Bei einem **Matrix**-Element handelt es sich eigentlich um eine Struktur und nicht um ein [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/BR242356)-Element. Daher können keine individuellen animierten Werte unterstützt werden.

Einige XAML-Designtools, die Ihnen das Anwenden von Transformationsvorgängen ermöglichen, serialisieren die Ergebnisse als [**MatrixTransform**](https://msdn.microsoft.com/library/windows/apps/BR210137)-Element. In diesem Fall empfiehlt es sich, erneut das gleiche Designtool zu verwenden, um den Transformationseffekt zu ändern und den XAML-Code erneut zu serialisieren, anstatt die [**Matrix**](https://msdn.microsoft.com/library/windows/apps/BR210127)-Werte direkt im XAML-Code zu ändern.

## <span id="3-D_transforms"></span><span id="3-d_transforms"></span><span id="3-D_TRANSFORMS"></span>3D-Transformationen

Mithilfe *perspektivischer Transformationen* können Sie 3D-Effekte auf beliebige [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911)-Elemente anwenden. So können Sie beispielsweise wie hier gezeigt den Eindruck erwecken, dass sich ein Objekt auf einer perspektivischen Ebene in Ihre Richtung (oder von Ihnen weg) dreht. Hierzu legen Sie die [**Projection**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.projection.aspx)-Eigenschaft des **UIElement**-Objekts als [**PlaneProjection**](https://msdn.microsoft.com/library/windows/apps/BR210192)-Wert fest. Die **PlaneProjection**-Klasse definiert, wie die Transformation in einem simulierten 3D-Raum gerendert wird. Diese Art der Transformation wird im Thema [3D-Perspektiven-Effekte für XAML-UI](3-d-perspective-effects.md) ausführlicher beschrieben.

**Hinweis:** Es ist technisch möglich, die gleichen Ergebnisse mit [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006)-Klassen zu erzielen. Allerdings ist die [**PlaneProjection**](https://msdn.microsoft.com/library/windows/apps/BR210192)-Technik mit Bildern als Brush-Elemente für gewöhnlich optisch ansprechender, und die Eigenschaftswerte lassen sich sehr viel einfacher korrekt festlegen.

 

## <span id="related_topics"></span>Verwandte Themen

* [Zeichnen von Formen](drawing-shapes.md)
* [3D-Perspektiveneffekte für XAML-UI](3-d-perspective-effects.md)
* [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006)
 

 






<!--HONumber=Mar16_HO1-->


