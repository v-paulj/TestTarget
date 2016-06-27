---
author: Jwmsft
Description: "Erfahren Sie, wie Sie Bilder in Ihre App integrieren. Dazu gehört auch die Verwendung der APIs der beiden XAML-Hauptklassen, Image und ImageBrush."
title: Bilder und Bildpinsel
ms.assetid: CEA8780C-71A3-4168-A6E8-6361CDFB2FAF
label: Images and image brushes
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 972480aabb6f0db3b5091bd55323f9d1946086e6

---
# Bilder und Bildpinsel

Sie können zum Anzeigen von Bildern das **Image**-Objekt oder das **ImageBrush**-Objekt verwenden. Ein Image-Objekt rendert ein Bild, und ein ImageBrush-Objekt zeichnet ein anderes Objekt mit einem Bild. 

<span class="sidebar_heading" style="font-weight: bold;">Wichtige APIs</span>

-   [**Image-Klasse**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx)
-   [**Source-Eigenschaft**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.source.aspx)
-   [**ImageBrush-Klasse**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.aspx)
-   [**ImageSource-Eigenschaft**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.imagesource.aspx)

## Sind dies die richtigen Elemente?
Verwenden Sie ein **Image**-Element, um ein eigenständiges Bild in Ihrer App anzuzeigen.

Verwenden Sie **ImageBrush**, um ein Image auf ein anderes Objekt anzuwenden. „ImageBrush“ kann u. a. für dekorative Effekte für Text oder unterteilte Hintergründe für Steuerelemente oder Layoutcontainer verwendet werden. Sie können steuern, wie das Bild gestreckt, ausgerichtet und unterteilt wird, um Muster und andere Effekte zu erzeugen. 

## Beispiele



## Erstellen eines Bilds

### Image
In diesem Beispiel wird veranschaulicht, wie ein Bild mit dem [**Image**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx)-Objekt erstellt wird.


```XAML
<Image Width="200" Source="licorice.jpg" />
```

Hier ist das gerenderte Image-Objekt.

![Beispiel für ein Image-Element](images/Image_Licorice.jpg)

Die [**Source**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.source.aspx)-Eigenschaft in diesem Beispiel gibt den Speicherort des Bilds an, das Sie anzeigen möchten. Sie können die Quelle festlegen, indem Sie die absolute URL (z. B. „http://contoso.com/myPicture.jpg“) oder eine URL relativ zu Ihrer App-Verpackungsstruktur angeben. In unserem Beispiel legen wir die Bilddatei „licorice.jpg“ im Stammverzeichnis unseres Projekts ab und deklarieren Projekteinstellungen, die die Bilddatei als Inhalt einbeziehen.

### ImageBrush

Mit dem [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.aspx)-Objekt können Sie ein Bild verwenden, um einen Bereich zu zeichnen, der ein [**Brush**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.aspx)-Objekt annimmt. So können Sie ein ImageBrush-Objekt für den Wert der [**Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx)-Eigenschaft einer [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.ellipse.aspx)-Klasse oder die [**Background**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.background.aspx)-Eigenschaft einer [**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx)-Klasse verwenden.

Im nächsten Beispiel ist dargestellt, wie „ImageBrush“ zum Zeichnen eines Ellipse verwendet wird.

```XAML
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="licorice.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

Hier ist die Ellipse, die von „ImageBrush“ gezeichnet wurde.

![Eine Ellipse, die mit „ImageBrush“ gezeichnet wurde](images/Image_ImageBrush_Ellipse.jpg)

### Strecken von Bildern

Wenn Sie den [**Width**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.width.aspx)-Wert oder [**Height**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx)-Wert eines **Image**-Objekts nicht festlegen, wird es mit den von **Source** angegebenen Abmessungen des Bilds angezeigt. Durch das Festlegen von **Width** und **Height** wird ein rechteckiger Bereich erstellt, in dem das Bild angezeigt wird. Sie können festlegen, wie das Bild den Bereich auffüllt, indem Sie die [**Stretch**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.stretch.aspx)-Eigenschaft verwenden. Die Stretch-Eigenschaft akzeptiert die folgenden Werte, die durch die [**Stretch**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.stretch.aspx)-Enumeration definiert werden:

-   **None**: Das Bild wird nicht gestreckt, um den Ausgabebereich auszufüllen. Beachten Sie bei dieser Stretch-Einstellung Folgendes: Wenn das Quellbild größer als der enthaltende Bereich ist, wird das Bild abgeschnitten. Dies sollte hier normalerweise jedoch vermieden werden, da nicht gesteuert werden kann, welcher Ausschnitt angezeigt wird, wie dies bei einer absichtlichen [**Clip**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.clip.aspx)-Anwendung der Fall ist.
-   **Uniform**: Die Bildgröße wird angepasst, sodass das Bild in die Abmessungen der Ausgabe passt. Das Seitenverhältnis des Inhalts bleibt jedoch erhalten. Dies ist der Standardwert.
-   **UniformToFill**: Das Bild wird skaliert, sodass es den Ausgabebereich vollständig ausfüllt, das ursprüngliche Seitenverhältnis jedoch beibehalten wird.
-   **Fill**: Die Bildgröße wird angepasst, sodass das Bild in die Abmessungen der Ausgabe passt. Da Höhe und Breite des Inhalts unabhängig voneinander dimensioniert werden, wird das ursprüngliche Seitenverhältnis möglicherweise nicht beibehalten. Mit anderen Worten, das Bild wird eventuell verzerrt, um den Ausgabebereich vollständig auszufüllen

![Ein Beispiel für Streckeinstellungen](images/Image_Stretch.jpg)

### Zuschneiden von Bildern

Mit der [**Clip**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.clip.aspx)-Eigenschaft können Sie einen Bildausgabebereich beschneiden. Die Clip-Eigenschaft wird für eine [**Geometry**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.geometry.aspx)-Klasse festgelegt. Das Beschneiden wird derzeit nur für Rechtecke unterstützt.

Im nächsten Beispiel erfahren Sie, wie Sie eine [**RectangleGeometry**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.rectanglegeometry.aspx)-Klasse als Zuschneidebereich für ein Bild verwenden. In diesem Beispiel definieren wir ein **Image**-Objekt mit einer Höhe von 200. Eine **RectangleGeometry**-Klasse definiert ein Rechteck für den Bereich des Bilds, der angezeigt wird. Die [**Rect**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.rectanglegeometry.rect.aspx)-Eigenschaft ist auf „25,25,100,150“ festgelegt, wodurch ein Rechteck definiert ist, das bei Position 25,25 mit einer Breite von 100 und einer Höhe von 150 startet. Nur der Teil des Bilds, der sich innerhalb des Rechteckbereichs befindet, wird angezeigt.

```xaml
<Image Source="licorice.jpg" Height="200">
    <Image.Clip>
        <RectangleGeometry Rect="25,25,100,150" />
    </Image.Clip>
</Image>
```

Hier sehen Sie das zugeschnittene Bild auf einem schwarzen Hintergrund.

![Ein Image-Objekt, das mit einer RectangleGeometry-Klasse zugeschnitten wurde](images/Image_Cropped.jpg)

### Anwenden von Transparenz

Sie können [**Opacity**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.opacity.aspx) auf ein Bild anwenden, sodass das Bild teilweise durchsichtig gerendert wird. Die Transparenzwerte reichen von 0,0 bis 1,0, wobei 1,0 vollständig deckend und 0,0 vollständig durchsichtig bedeutet. Im Beispiel wird dargestellt, wie eine Transparenz von 0,5 auf ein Bild angewendet wird.

```xaml
<Image Height="200" Source="licorice.jpg" Opacity="0.5" />
```

Dies ist das gerenderte Bild mit einer Transparenz von 0,5 und einem schwarzen Hintergrund, der durch das teilweise durchlässige Bild zu sehen ist.

![Ein Image-Objekt mit einer Transparenz von 0,5.](images/Image_Opacity.jpg)

### Bilddateiformate

**Image** und **ImageBrush** können die folgenden Bilddateiformate anzeigen:

-   JPEG (Joint Photographic Experts Group)
-   PNG (Portable Network Graphics)
-   BMP (Bitmap)
-   GIF (Graphics Interchange Format)
-   TIFF (Tagged Image File Format)
-   JPEG XR
-   ICO (Symbole)

Die APIs für [**Image**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx), [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapimage.aspx) und [**BitmapSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapsource.aspx) enthalten keine dedizierten Methoden für das Codieren und Decodieren von Medienformaten. Sämtliche Codier- und Decodiervorgänge sind integriert. Aspekte dieser Vorgänge sind auf der Oberfläche allenfalls als Bestandteil von Ereignisdaten für Load-Ereignisse sichtbar. Falls Sie gezielt mit der Codierung und Decodierung von Bildern arbeiten möchten, weil Ihre App beispielsweise Bildkonvertierungen oder Bildbearbeitungsfunktionen ausführt, sollten Sie die im [**Windows.Graphics.Imaging**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.graphics.imaging.aspx)-Namespace verfügbaren APIs verwenden. Die APIs werden außerdem von der Windows-Bilderstellungskomponente (Windows Imaging Component, WIC) unterstützt.

Weitere Informationen zu App-Ressourcen und zum Packen von Bildquellen in einer App finden Sie unter [Definieren von App-Ressourcen](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321).

### WriteableBitmap

[
            **WriteableBitmap**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.writeablebitmap.aspx) stellt eine [**BitmapSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapsource.aspx) bereit, die geändert werden kann und nicht die grundlegende dateibasierte Decodierung aus der WIC verwendet. Sie können Bilder dynamisch bearbeiten und das aktualisierte Bild erneut rendern. Verwenden Sie zum Definieren des Pufferinhalts eines **WriteableBitmap**-Elements die [**PixelBuffer**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.writeablebitmap.pixelbuffer.aspx)-Eigenschaft, um auf den Puffer zuzugreifen, und einen Datenstrom oder sprachspezifischen Puffertyp, um ihn zu füllen. Beispielcode finden Sie unter [**WriteableBitmap**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.writeablebitmap.aspx).

### RenderTargetBitmap

Die [**RenderTargetBitmap**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.rendertargetbitmap.aspx)-Klasse kann die XAML-Benutzeroberflächenstruktur aus einer ausgeführten App erfassen und dann eine Bitmapbildquelle darstellen. Nach der Erfassung kann diese Bildquelle auf andere Teile der App angewendet, vom Benutzer als Ressourcen- oder App-Daten gespeichert oder für andere Szenarien verwendet werden. Ein besonders hilfreiches Szenario ist die Erstellung eines Laufzeitminiaturbilds einer XAML-Seite für ein Navigationsschema. Dies kann beispielsweise die Bereitstellung eines Bildlinks über ein [**Hub**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.hub.aspx)-Steuerelement sein. **RenderTargetBitmap** besitzt einige Einschränkungen hinsichtlich des Inhalts, der im erfassten Bild angezeigt wird. Weitere Informationen finden Sie im API-Referenzthema für [**RenderTargetBitmap**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.rendertargetbitmap.aspx).

### Bildquellen und Skalierung

Erstellen Sie Ihre Bildquellen in mehreren empfohlenen Größen, damit die App immer gut aussieht, wenn Windows sie skaliert. Beim Angeben von **Source** für **Image** können Sie eine Benennungskonvention verwenden, die automatisch auf die richtige Ressource für die aktuelle Skalierung verweist. Einzelheiten zur Benennungskonvention sowie weiterführende Informationen finden Sie unter [Schnellstart: Verwenden von Datei- oder Bildressourcen](https://msdn.microsoft.com/library/windows/apps/xaml/hh965325).

Weitere Informationen zur Berücksichtigung der Skalierung in Ihrem App-Design finden Sie unter [UX-Richtlinien für Layout und Skalierung](https://msdn.microsoft.com/library/windows/apps/dn611863).

### „Image“ und „ImageBrush“ im Code

In der Regel werden das Image- und das ImageBrush-Element mit XAML und nicht mit Code angegeben. Das liegt daran, dass diese Elemente häufig von Entwicklungstools als Teil einer XAML-UI-Definition ausgegeben werden.

Wenn Sie „Image“ oder „ImageBrush“ mit Code definieren, verwenden Sie die Standardkonstruktoren, und legen Sie dann die relevanten Eigenschaften ([**Image.Source**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.source.aspx) oder [**ImageBrush.ImageSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.imagesource.aspx)) fest. Die Quelleigenschaften erfordern ein [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapimage.aspx)-Objekt (keinen URI), wenn Sie sie mithilfe von Code festlegen. Falls es sich bei Ihrer Quelle um einen Datenstrom handelt, initialisieren Sie den Wert mit der [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync.aspx)-Methode. Ist Ihre Quelle ein (URI), der Ihrer App Inhalt mit dem **ms-appx**- oder dem **ms-resource**-Schema hinzufügt, verwenden Sie den [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/xaml/br243238.aspx)-Konstruktor, für den ein URI angegeben wird. Wenn beim Abrufen oder Decodieren der Bildquelle Probleme mit der Zeitsteuerung auftreten und Sie alternativen Inhalt anzeigen müssen, bis die Bildquelle verfügbar ist, können Sie auch das [**ImageOpened**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.imageopened.aspx)-Ereignis behandeln. Beispielcode finden Sie unter [Beispiel für XAML-Bilder](http://go.microsoft.com/fwlink/p/?linkid=238575).

> **Hinweis**
            &nbsp;&nbsp;Wenn Sie Bilder mithilfe von Code festlegen, können Sie die automatische Behandlung für den Zugriff auf nicht qualifizierte Ressourcen mit den aktuellen Skalierungs- und Kulturqualifizierern verwenden. Alternativ können Sie auch [**ResourceManager**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.resources.core.resourcemanager.aspx) und [**ResourceMap**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.resources.core.resourcemap.aspx) mit Qualifizierern für Kultur und Skalierung verwenden, um die Ressourcen direkt abzurufen. Weitere Informationen finden Sie unter [Ressourcenverwaltungssystem](https://msdn.microsoft.com/library/windows/apps/xaml/jj552947.aspx).




<!--HONumber=Jun16_HO3-->


