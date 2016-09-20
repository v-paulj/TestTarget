---
author: Jwmsft
ms.assetid: 90F07341-01F4-4205-8161-92DD2EB49860
title: "3D-Perspektiveneffekte für XAML-UI"
description: "Mithilfe der perspektivischen Transformation können Sie 3D-Effekte auf Inhalte in Ihren Windows-Runtime-Apps anwenden. Sie können z. B. wie hier gezeigt die Illusion schaffen, dass sich ein Objekt auf Sie zu oder von Ihnen wegbewegt."
translationtype: Human Translation
ms.sourcegitcommit: 54bcd19419f31563f910b705fce8128bca33825b
ms.openlocfilehash: 543dfb60b1fa70e2fceebbdd03da8a301eb9d08f

---
# 3D-Perspektiveneffekte für XAML-UI

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Mithilfe der perspektivischen Transformation können Sie 3D-Effekte auf Inhalte in Ihren Windows-Runtime-Apps anwenden. Sie können z.B. wie hier gezeigt die Illusion schaffen, dass sich ein Objekt auf Sie zu oder von Ihnen wegbewegt.

![Bild mit perspektivischer Transformation](images/3dsimple.png)

Eine weitere häufige Anwendung von perspektivischen Transformationen besteht in der Anordnung von Objekten relativ zueinander, wodurch wie hier ein 3D-Effekt erzeugt wird.

![Stapeln von Objekten zum Erzeugen eines 3D-Effekts](images/3dstacking.png)

Neben dem Erzeugen von statischen 3D-Effekten können Sie die Eigenschaften der perspektivischen Transformation animieren, um 3D-Bewegungseffekte zu erzielen. [Führen Sie dieses Beispiel aus](http://go.microsoft.com/fwlink/p/?linkid=236111), um dies zu veranschaulichen.

Sie haben sich soeben damit vertraut gemacht, wie perspektivische Transformationen auf Bilder angewendet werden. Sie können diese Effekte jedoch auf jedes [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) anwenden, einschließlich von Steuerelementen. Sie können beispielsweise wie folgt einen 3D-Effekt auf einen ganzen Container von Steuerelementen anwenden:

![Auf einen Container von Elementen angewendeter 3D-Effekt](images/skewedstackpanel.png)

Dieses Beispiel wurde mit folgendem XAML-Code erstellt:

```xml
<StackPanel Margin="35" Background="Gray">    
    <StackPanel.Projection>        
        <PlaneProjection RotationX="-35" RotationY="-35" RotationZ="15"  />    
    </StackPanel.Projection>    
    <TextBlock Margin="10">Type Something Below</TextBlock>    
    <TextBox Margin="10"></TextBox>    
    <Button Margin="10" Content="Click" Width="100" />
</StackPanel>
```

Wenden Sie sich nun den Eigenschaften von [**PlaneProjection**](https://msdn.microsoft.com/library/windows/apps/BR210192) zu. Damit werden Objekte im 3D-Raum gedreht und bewegt. Im folgenden Beispiel können Sie mit den Eigenschaften experimentieren und feststellen, wie sie sich auf ein Objekt auswirken.

[Dieses Beispiel ausführen](http://go.microsoft.com/fwlink/p/?linkid=236112)

## PlaneProjection-Klasse

Sie können 3D-Effekte auf jedes [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) anwenden. Dazu legen Sie die [**Projection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.projection)-Eigenschaft für das UIElement mit einer [**PlaneProjection**](https://msdn.microsoft.com/library/windows/apps/BR210192) fest. Die **PlaneProjection** definiert, wie die Transformation im Raum gerendert wird. Im nächsten Beispiel wird ein einfacher Fall veranschaulicht.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35"   />
    </Image.Projection>
</Image>
```

Die Abbildung zeigt, wie das Bild gerendert wird. Die X-Achse, die Y-Achse und die Z-Achse sind als rote Linien dargestellt. Das Bild wird mit der [**RotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationx)-Eigenschaft um 35 Grad um die X-Achse rückwärts gedreht.

![RotateX minus 35 Grad](images/3drotatexminus35.png)

Die [**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy)-Eigenschaft führt eine Drehung um die Y-Achse des Drehmittelpunkts aus.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35"   />
    </Image.Projection>
</Image>
```

![RotateY minus 35 Grad](images/3drotateyminus35.png)

Die [**RotationZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationz)-Eigenschaft führt eine Drehung um die Z-Achse des Drehmittelpunkts aus (eine Linie, die eine Senkrechte zur Objektfläche darstellt).

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationZ="-45"/>
    </Image.Projection>
</Image>
```

![RotateZ minus 45Grad](images/3drotatezminus35.png)

Für die Drehungseigenschaften kann ein positiver oder negativer Wert für die Drehung in beiden Richtungen angegeben werden. Die absolute Zahl kann größer als 360 sein, wodurch das Objekt um mehr als eine volle Drehung gedreht wird. [Führen Sie dieses Beispiel aus](http://go.microsoft.com/fwlink/p/?linkid=236112), um mit verschiedenen Werten für die Eigenschaften [**RotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationx), [**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy) und [**RotationZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationz) zu experimentieren und die entsprechenden Auswirkungen zu veranschaulichen.

Sie können den Drehmittelpunkt mithilfe der Eigenschaften [**CenterOfRotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationx), [**CenterOfRotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationy) und [**CenterOfRotationZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationz) verschieben. Standardmäßig verläuft die Drehachse direkt durch den Objektmittelpunkt, wodurch das Objekt um seinen Mittelpunkt gedreht wird. Wenn Sie jedoch den Drehmittelpunkt an den Rand des Objekts verschieben, wird das Objekt um den betreffenden Rand gedreht. Der Standardwert für **CenterOfRotationX** und **CenterOfRotationY** ist 0,5, und der Standardwert für **CenterOfRotationZ** ist 0. Für **CenterOfRotationX** und **CenterOfRotationY** wird der Drehpunkt mit Werten zwischen 0 und 1 auf eine bestimmte Position auf dem Objekt festgelegt. Durch den Wert "0" wird ein Rand des Objekts angegeben, während mit dem Wert "1" der gegenüberliegende Rand angegeben wird. Werte außerhalb dieses Bereichs sind zulässig, und der Drehmittelpunkt wird entsprechend verschoben. Da die Z-Achse des Drehmittelpunkts durch die Fläche des Objekts gezeichnet wird, können Sie den Drehmittelpunkt mit einer negativen Zahl hinter das Objekt und mit einer positiven Zahl vor das Objekt (auf den Betrachter zu) verschieben.

[**CenterOfRotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationx) verschiebt den Drehmittelpunkt entlang der X-Achse parallel zum Objekt, während [**CenterOfRotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationy) den Drehmittelpunkt entlang der Y-Achse des Objekts verschiebt. In der folgenden Abbildung wird die Auswirkung von verschiedenen Werten für **CenterOfRotationY** veranschaulicht.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35" CenterOfRotationY="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationY = "0,5" (Standardeinstellung)**

![CenterOfRotationY ist "0,5"](images/3drotatexminus35.png)
```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35" CenterOfRotationY="0.1"/>
    </Image.Projection>
</Image>
```

**CenterOfRotationY = "0,1"**

![CenterOfRotationY ist "0,1"](images/3dcenterofrotationy0point1.png)

Beachten Sie, wie das Bild um den Mittelpunkt gedreht wird, wenn die [**CenterOfRotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationy)-Eigenschaft auf den Standardwert "0,5" festgelegt ist. Es wird nahe seinem oberen Rand gedreht, wenn die Eigenschaft auf "0,1" festgelegt ist. Ein ähnliches Verhalten ist zu beobachten, wenn die [**CenterOfRotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationx)-Eigenschaft geändert wird, um eine Verschiebung zu bewirken, wobei das Objekt durch die [**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy)-Eigenschaft gedreht wird.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35" CenterOfRotationX="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationX = "0,5" (Standardeinstellung)**

![CenterOfRotationX ist "0,5"](images/3drotateyminus35.png)
```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35" CenterOfRotationX="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationX = "0,9" (rechter Rand)**

![CenterOfRotationX ist "0,9"](images/3dcenterofrotationx0point9.png)

Platzieren Sie den Drehmittelpunkt mithilfe von [**CenterOfRotationZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationz) über bzw. unter der Fläche des Objekts. Auf diese Weise können Sie das Objekt wie einen Planeten auf seiner Umlaufbahn um einen Stern um den betreffenden Punkt drehen.

[Führen Sie dieses Beispiel mit Schiebereglern aus](http://go.microsoft.com/fwlink/p/?linkid=236112), um mit Drehungen des Objekts um verschiedene Drehmittelpunktpositionen zu experimentieren.

## Positionieren eines Objekts

Sie haben nun erfahren, wie Sie ein Objekt im Raum drehen können. Mit den folgenden Eigenschaften können Sie diese gedrehten Objekte relativ zueinander im Raum positionieren:

-   [**LocalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetx) verschiebt ein Objekt entlang der X-Achse der Fläche eines gedrehten Objekts.
-   [**LocalOffsetY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsety) verschiebt ein Objekt entlang der Y-Achse der Fläche eines gedrehten Objekts.
-   [**LocalOffsetZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetz) verschiebt ein Objekt entlang der Z-Achse der Fläche eines gedrehten Objekts.
-   [**GlobalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsetx) verschiebt ein Objekt entlang der am Bildschirm ausgerichteten X-Achse.
-   [**GlobalOffsetY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsety) verschiebt ein Objekt entlang der am Bildschirm ausgerichteten Y-Achse.
-   [**GlobalOffsetZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsetz) verschiebt ein Objekt entlang der am Bildschirm ausgerichteten Z-Achse.

**Lokaler Versatz**

Die Eigenschaften [**LocalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetx), [**LocalOffsetY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsety) und [**LocalOffsetZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetz) versetzen ein Objekt entlang der entsprechenden Achse der Fläche des Objekts, nachdem dieses gedreht wurde. Daher bestimmt die Drehung des Objekts die Richtung, in die das Objekt versetzt wird. Zum Verdeutlichen dieses Konzepts wird im folgenden Beispiel **LocalOffsetX** von 0 bis 400 Grad animiert, und [**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy) wird von 0 bis 65 Grad animiert.

[Dieses Beispiel ausführen](http://go.microsoft.com/fwlink/p/?linkid=236209)

Im vorigen Beispiel ist zu beobachten, dass das Objekt entlang seiner eigenen X-Achse verschoben wird. Das Objekt wird gleich zu Beginn der Animation, wenn der Wert von [**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy) nahe null liegt (d. h. parallel zum Bildschirm), entlang des Bildschirms in der X-Richtung verschoben. Während das Objekt jedoch in Richtung des Betrachters gedreht wird, erfolgt eine Verschiebung entlang der X-Achse der Objektfläche in Richtung Betrachter. Wenn Sie die **RotationY**-Eigenschaft jedoch mit einem Wert von -65 Grad animiert haben, wird das Objekt in einer Kurvenbewegung vom Betrachter weg gedreht.

[**LocalOffsetY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsety) funktioniert ähnlich wie [**LocalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetx), die Bewegung erfolgt jedoch entlang der vertikalen Achse. Eine Änderung von [**RotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationx) wirkt sich daher auf die Richtung aus, in der **LocalOffsetY** das Objekt verschiebt. Im nächsten Beispiel wird **LocalOffsetY** von 0 bis 400 Grad animiert, und **RotationX** wird von 0 bis 65 Grad animiert.

[Dieses Beispiel ausführen](http://go.microsoft.com/fwlink/p/?linkid=236210)

[**LocalOffsetZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetz) versetzt das Objekt senkrecht zur Fläche des Objekts, als ob durch den Mittelpunkt an der Objektrückseite ein Vektor direkt in Richtung des Betrachters gezeichnet würde. Zum Veranschaulichen der Funktionsweise von **LocalOffsetZ** wird im folgenden Beispiel **LocalOffsetZ** von 0 bis 400 Grad animiert, und [**RotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationx) wird von 0 bis 65 Grad animiert.

[Dieses Beispiel ausführen](http://go.microsoft.com/fwlink/p/?linkid=236211)

Das Objekt bewegt sich zu Beginn der Animation, wenn der Wert von [**RotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationx) nahe null liegt (d. h. parallel zum Bildschirm), direkt in Richtung des Betrachters, mit der Abwärtsdrehung des Objekts wird dieses jedoch nach unten verschoben.

**Globaler Versatz**

Die Eigenschaften [**GlobalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsetx), [**GlobalOffsetY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsety) und [**GlobalOffsetZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsetz) versetzen das Objekt entlang der Achsen relativ zum Bildschirm. Anders als bei den Eigenschaften für den lokalen Versatz ist daher die Achse, entlang der das Objekt verschoben wird, unabhängig von der auf das Objekt angewendeten Drehung. Diese Eigenschaften sind hilfreich, wenn Sie das Objekt lediglich entlang der X-, Y- oder Z-Achse des Bildschirms verschieben möchten und eine Drehung des Objekts keine Rolle spielt.

Im nächsten Beispiel wird [**GlobalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsetx) von 0 bis 400 Grad animiert, und [**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy) wird von 0 bis 65 Grad animiert.

[Dieses Beispiel ausführen](http://go.microsoft.com/fwlink/p/?linkid=236213)

Sie stellen fest, dass in diesem Beispiel die Richtung des Objekts während seiner Drehung nicht geändert wird. Dies liegt daran, dass das Objekt ungeachtet seiner Drehung entlang der X-Achse des Bildschirms gedreht wird.

## Positionieren eines Objekts

Sie können den [**Matrix3DProjection**](https://msdn.microsoft.com/library/windows/apps/BR210128)-Typ und den [**Matrix3D**](https://msdn.microsoft.com/library/windows/apps/BR243266)-Typ für Semi-3D-Szenarien nutzen, die komplexer als die von [**PlaneProjection**](https://msdn.microsoft.com/library/windows/apps/BR210192) unterstützten sind. **Matrix3DProjection** bietet Ihnen eine komplette 3D-Transformationsmatrix, die auf ein beliebiges [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) angewendet werden kann. Somit können Sie Transformationsmatrizen und Perspektivmatrizen beliebiger Modelle auf Elemente anwenden. Beachten Sie, dass diese APIs nur im Minimalzustand vorliegen. Wenn Sie sie also nutzen möchten, müssen Sie den Code schreiben, mit dem die 3D-Transformationsmatrizen ordnungsgemäß erstellt werden. Daher ist es einfacher, für einfache 3D-Szenarien **PlaneProjection** zu verwenden.



<!--HONumber=Aug16_HO3-->


