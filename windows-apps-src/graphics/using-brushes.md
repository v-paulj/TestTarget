---
author: Jwmsft
ms.assetid: 02141F86-355E-4046-86EA-2A89D615B7DB
title: Verwenden von Pinseln
description: "Mit Brush-Objekten werden Innenbereiche oder Ränder von Formen, Text und Teilen von Steuerelementen gezeichnet, damit das gezeichnete Objekt auf einer Benutzeroberfläche sichtbar ist."
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 38999529dda7f5e21ef7aee4a99b2420cb37bfa6

---
# Verwenden von Pinseln

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


** Wichtige APIs **

-   [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076)


              [
              **Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076)-Objekte werden verwendet, um Innenbereiche oder Konturen von Formen, Texten und Teilen von Steuerelementen zu zeichnen, damit das gezeichnete Objekt auf einer Benutzeroberfläche sichtbar ist. Machen Sie sich im Folgenden mit den verfügbaren Pinseln und deren Verwendung vertraut.

## Einführung in Pinsel

Verwenden Sie für das Zeichnen von Objekten wie einer [**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377) oder Teilen eines [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390), das auf der App-Canvas angezeigt wird, einen [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076). Sie können z. B. die [**Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx)-Eigenschaft der **Shape** oder die [**Background**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.background.aspx)-Eigenschaft und die [**Foreground**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.foreground.aspx)-Eigenschaft eines **Control** auf einen **Brush**-Wert festlegen. Dieser **Brush** bestimmt, wie das UI-Element in der UI gezeichnet oder gerendert wird. Es gibt folgende Pinseltypen: [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962), [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108), [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) und [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703).

## Einfarbige Pinsel

Ein [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) zeichnet einen Bereich mit einer einzelnen [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723), z.B. Rot oder Blau. Dies ist der einfachste Pinsel. Es gibt drei Möglichkeiten, in XAML einen **SolidColorBrush** und die zugehörige Volltonfarbe zu definieren: vordefinierte Farbnamen, hexadezimale Farbwerte und die Syntax für Eigenschaftselemente.

### Vordefinierte Farbnamen

Sie können einen vordefinierten Farbnamen wie [**Yellow**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.yellow.aspx) oder [**Magenta**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.magenta.aspx) verwenden. Es stehen 256benannte Farben zur Verfügung. Der XAML-Parser wandelt den Farbnamen in eine [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723)-Struktur mit den richtigen Farbkanälen um. Die 256 benannten Farben basieren auf den *X11*-Farbnamen der CSS3-Spezifikation (Cascading Style Sheets, Level3). Möglicherweise kennen Sie diese Liste benannter Farben also bereits, wenn Sie über Vorkenntnisse in Webentwicklung oder -design verfügen.

In diesem Beispiel wird die [**Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx)-Eigenschaft eines [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) auf die vordefinierte Farbe [**Red**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.red.aspx) festgelegt.

```xml
<Rectangle Width="100" Height="100" Fill="Red" />
```

Die folgende Abbildung zeigt den [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962), der auf das [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) angewendet wurde.

![Ein gerenderter SolidColorBrush](images/brushes-solidcolorbrush.jpg)

Wenn Sie einen [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) mit Code anstatt mit XAML definieren, sind die einzelnen benannten Farben als statische Eigenschaftswerte der [**Colors**](https://msdn.microsoft.com/library/windows/apps/windows.ui.colors)-Klasse verfügbar. Wenn Sie z.B. einen [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.solidcolorbrush.color.aspx)-Wert eines **SolidColorBrush** zum Darstellen der benannten Farbe „Orchid“ deklarieren möchten, legen Sie den **Color**-Wert auf den statischen Wert [**Colors.Orchid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.orchid.aspx) fest.

### Hexadezimale Farbwerte

Sie können eine Zeichenfolge im Hexadezimalformat verwenden, um präzise 24-Bit-Farbwerte mit 8-Bit-Alphakanal für einen [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) zu deklarieren. Zwei Zeichen im Bereich 0 bis F definieren die einzelnen Komponentenwerte. Die Reihenfolge der Komponentenwerte für die Hexadezimalzeichenfolge lautet wie folgt: Alphakanal (Deckkraft), roter Kanal, grüner Kanal und blauer Kanal (**ARGB**). Der Hexadezimalwert „#FFFF0000“ definiert z.B. ein vollständig deckendes Rot (Alpha=FF, Rot=FF, Grün=00 und Blau=00).

In diesem XAML-Beispiel wird die [**Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx)-Eigenschaft eines [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) auf den Hexadezimalwert „\#FFFF0000“ festgelegt. Das Ergebnis ist das gleiche wie bei der Verwendung der benannten Farbe [**Colors.Red**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.red.aspx).

```xml
<StackPanel>
  <Rectangle Width="100" Height="100" Fill="#FFFF0000" />
</StackPanel>
```

### <span id="Property_element_syntax__"></span><span id="property_element_syntax__"></span><span id="PROPERTY_ELEMENT_SYNTAX__"></span>Syntax für Eigenschaftselemente

Mithilfe der Syntax für Eigenschaftselemente können Sie einen [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) definieren. Diese Syntax ist ausführlicher als die zuvor gezeigten Methoden, und Sie können zusätzliche Eigenschaftswerte für ein Element angeben, wie z.B. die [**Opacity**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.opacity.aspx) des Pinsels. Weitere Informationen zur XAML-Syntax, einschließlich der Syntax für Eigenschaftselemente, finden Sie unter [XAML-Übersicht](https://msdn.microsoft.com/library/windows/apps/Mt185595) und [Anleitung zur XAML-Syntax](https://msdn.microsoft.com/library/windows/apps/Mt185596).

In den vorhergehenden Beispielen kam die Zeichenfolge „SolidColorBrush“ in der Syntax nicht vor. Der erstellte Pinsel wird implizit und automatisch erstellt und ist Teil einer durchdachten Kurzschrift der XAML-Sprache, mit der die UI-Definition für die am häufigsten Anwendungsfälle unkompliziert gehalten wird. Im nächsten Beispiel werden ein [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) und dann explizit der [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) als Elementwert für eine [**Rectangle.Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx)-Eigenschaft erstellt. Die [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.solidcolorbrush.color.aspx) von **SolidColorBrush** wird auf [**Blue**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.blue.aspx) und die [**Opacity**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.opacity.aspx) auf 0,5 festgelegt.

```xml
<Rectangle Width="200" Height="150">
    <Rectangle.Fill>
        <SolidColorBrush Color="Blue" Opacity="0.5" />
    </Rectangle.Fill>
</Rectangle>
```

## <span id="Linear_gradient_brushes_"></span><span id="linear_gradient_brushes_"></span><span id="LINEAR_GRADIENT_BRUSHES_"></span>Lineare Farbverlaufspinsel

Ein [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108) zeichnet einen Bereich mit einem Farbverlauf entlang einer Linie. Diese Linie wird als *Farbverlaufsachse* bezeichnet. Sie geben die Farben für den Farbverlauf und ihre Position an der Farbverlaufsachse mithilfe von [**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078)-Objekten an. Standardmäßig verläuft die Farbverlaufsachse von der oberen linken Ecke zur unteren rechte Ecke des Bereichs, den der Pinsel zeichnet. Dies führt zu einer diagonalen Schattierung.

Der [**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078) bildet die Grundlage für einen Farbverlaufspinsel. Ein Farbverlaufsstopp gibt die [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.color.aspx) des Pinsels an einem [**Offset**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.offset.aspx) entlang der Farbverlaufsachse an, wenn der Pinsel auf den zu zeichnenden Bereich angewendet wird.

Die [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.color.aspx)-Eigenschaft des Farbverlaufsstopps gibt die Farbe am Farbverlaufsstopp an. Sie können diese Farbe mit einem vordefinierten Farbnamen oder mit hexadezimalen **ARGB**-Werten angeben.

Die [**Offset**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.offset.aspx)-Eigenschaft eines [**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078) gibt die Position für jeden **GradientStop** entlang der Farbverlaufsachse an. Der **Offset** ist ein **double**-Wert zwischen 0 und1. Mit einem**Offset** von 0 wird der **GradientStop** am Anfang der Farbverlaufsachse platziert, also in der Nähe des [**StartPoint**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.lineargradientbrush.startpoint.aspx). Ein **Offset** von 1 platziert den **GradientStop** am [**EndPoint**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.lineargradientbrush.endpoint.aspx). Ein sinnvoller [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108) sollte mindestens über zwei **GradientStop**-Werte verfügen. Dabei sollte jeder **GradientStop** eine andere [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.color.aspx) und einen anderen **Offset** zwischen 0 und 1 aufweisen.

Dieses Beispiel erstellt einen linearen Farbverlauf mit vier Farben, der zum Zeichnen eines [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) verwendet wird.

```xml
<!-- This rectangle is painted with a diagonal linear gradient. -->
<Rectangle Width="200" Height="100">
    <Rectangle.Fill>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="Yellow" Offset="0.0" x:Name="GradientStop1"/>
            <GradientStop Color="Red" Offset="0.25" x:Name="GradientStop2"/>
            <GradientStop Color="Blue" Offset="0.75" x:Name="GradientStop3"/>
            <GradientStop Color="LimeGreen" Offset="1.0" x:Name="GradientStop4"/>
        </LinearGradientBrush>
    </Rectangle.Fill>
</Rectangle>
```

Die Farbe der einzelnen Punkte zwischen den Farbverlaufsstopps wird linear als eine Kombination der von den beiden umgebenden Farbverlaufsstopps angegebenen Farben interpoliert. In der Abbildung sind die Farbverlaufsstopps aus dem vorherigen Beispiel hervorgehoben. Die Kreise kennzeichnen die Position der Farbverlaufsstopps, und die gestrichelten Linie zeigt die Farbverlaufsachse.

![Farbverlaufsstopps](images/linear-gradients-stops.png) Sie können die Linie ändern, an der die Farbverlaufsstopps liegen, indem Sie die [**StartPoint**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.lineargradientbrush.startpoint.aspx)-Eigenschaft und die [**EndPoint**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.lineargradientbrush.endpoint.aspx)-Eigenschaft auf andere Werte als die Standardausgangswerte `(0,0)` und `(1,1)` festlegen. Durch Ändern der Koordinatenwerte für **StartPoint** und **EndPoint** können Sie horizontale oder vertikale Farbverläufe erstellen, die Richtung des Farbverlaufs umkehren oder die Farbverlaufsausdehnung verkleinern und so auf einen kleineren Bereich als den gesamten gezeichneten Bereich anwenden. Zum Verkleinern des Farbverlaufs legen Sie die Werte für **StartPoint** und/oder **EndPoint** auf einen Wert zwischen 0 und 1 fest. Angenommen, Sie möchten einen horizontalen Farbverlauf erstellen, bei dem der Farbverlauf nur in der linken Hälfte des Pinsels erfolgt und die rechte Hälfte nur die letzte [**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078)-Farbe hat. Hierfür geben Sie einen **StartPoint** von `(0,0)` und einen **EndPoint** von `(0.5,0)` an.

### <span id="Use_tools_to_make_gradients"></span><span id="use_tools_to_make_gradients"></span><span id="USE_TOOLS_TO_MAKE_GRADIENTS"></span>Verwenden von Tools zum Erstellen von Farbverläufen

Nachdem Sie nun wissen, wie lineare Farbverläufe funktionieren, können Sie Visual Studio oder Blend nutzen, um diese Farbverläufe einfacher erstellen zu können. Um einen Farbverlauf zu erstellen, wählen Sie in der Entwurfsoberfläche oder in der XAML-Ansicht das Objekt aus, auf das ein Farbverlauf angewendet werden soll. Erweitern Sie **Pinsel**, und wählen Sie die Registerkarte **Linearer Farbverlauf** aus (siehe nächstes Bildschirmfoto).

![Erstellen Sie einen linearen Farbverlauf mithilfe von Visual Studio.](images/tool-gradient-brush-1.png)

Jetzt können Sie die Farben der Farbverlaufsstopps ändern und ihre Positionen unter Verwendung der Leiste am unteren Rand verschieben. Sie können auch neue Farbverlaufsstopps hinzufügen, indem Sie auf die Leiste klicken, und Farbverlaufsstopps entfernen, indem Sie sie von der Leiste herunter ziehen (siehe nächstes Bildschirmfoto).

![Die Leiste am unteren Rand des Eigenschaftenfensters, die zur Steuerung der Farbverlaufsstopps dient.](images/tool-gradient-brush-2.png)

## <span id="Image_brushes"></span><span id="image_brushes"></span><span id="IMAGE_BRUSHES"></span>Bildpinsel

Mit einem [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) wird in einem Bereich ein Bild gezeichnet. Dabei stammt das zu zeichnende Bild aus einer Bilddateiquelle. Legen Sie die [**ImageSource**](https://msdn.microsoft.com/library/windows/apps/BR210107)-Eigenschaft auf den Pfad des Bilds fest, das Sie laden möchten. Normalerweise stammt die Bildquelle aus einem **Content**-Element, das Teil der App-Ressourcen ist.

Ein [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) streckt das Bild standardmäßig so weit, dass es den gezeichneten Bereich ausfüllt. Dadurch wird möglicherweise das Bild verzerrt, wenn der gezeichnete Bereich ein anderes Seitenverhältnis als das Bild aufweist. Sie können dieses Verhalten ändern, indem Sie die [**Stretch**](https://msdn.microsoft.com/library/windows/apps/BR242975)-Eigenschaft vom Standardwert **Fill** in **None**, **Uniform** oder **UniformToFill** ändern.

Im nächsten Beispiel wird ein [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) erstellt und [**ImageSource**](https://msdn.microsoft.com/library/windows/apps/BR210107) auf das Bild „licorice.jpg“ festgelegt, das Sie als Ressource in die App aufnehmen müssen. Der **ImageBrush** zeichnet dann den von einer [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343)-Form definierten Bereich.

```xml
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="licorice.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

So sieht der gerenderte [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) aus.

![Ein gerenderter ImageBrush](images/brushes-imagebrush.jpg)


              [
              **ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) und [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) verweisen beide mit einem URI (Uniform Resource Identifier) auf eine Bildquelldatei. Die Bildquelldatei liegt dabei in verschiedenen Formaten vor. Diese Bildquelldateien werden als URIs angegeben. Weitere Informationen zum Angeben von Bildquellen, zu den verwendbaren Bildformaten und zum Packen dieser Dateien in einer App finden Sie unter [„Image“ und „ImageBrush“](https://msdn.microsoft.com/library/windows/apps/Mt280382).

## Pinsel und Text

Sie können mit Pinseln auch Renderingmerkmale auf Textelemente anwenden. Die [**Foreground**](https://msdn.microsoft.com/library/windows/apps/BR209665)-Eigenschaft von [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) akzeptiert z.B. einen [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076). Sie können alle hier beschriebenen Pinsel auf Text anwenden. Seien Sie jedoch beim Anwenden von Pinseln auf Text vorsichtig, da der Text unlesbar werden kann. Dies ist z. B. der Fall, wenn die verwendeten Pinsel mit dem Hintergrund verlaufen, über dem der Text gerendert wird, oder von den Konturen der Textzeichen ablenken. Verwenden Sie den [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962), der i.d.R. keine negativen Auswirkungen auf die Lesbarkeit hat. Es sei denn, Sie möchten das Textelement besonders dekorativ gestalten.

Achten Sie auch bei Volltonfarben immer darauf, dass die ausgewählte Textfarbe einen ausreichenden Kontrast zur Hintergrundfarbe des Layoutcontainers für den Text aufweist. Die Stärke des Kontrasts zwischen Textvordergrund und Textcontainer sollte im Hinblick auf die Barrierefreiheit festgelegt werden.

## WebViewBrush

Bei einem [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703) handelt es sich um einen besonderen Pinseltyp, mit dem auf die Inhalte zugegriffen werden kann, die normalerweise in einem [**WebView**](https://msdn.microsoft.com/library/windows/apps/BR227702)-Steuerelement angezeigt werden. Anstatt den Inhalt im rechteckigen **WebView**-Steuerelementbereich zu rendern, zeichnet **WebViewBrush** diesen Inhalt in ein anderes Element mit einer [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076)-Typeigenschaft für eine Renderingoberfläche. 
              **WebViewBrush** ist nicht für jedes Pinselszenario geeignet, kann aber für Übergänge einer **WebView** nützlich sein. Weitere Informationen finden Sie unter **WebViewBrush**.

## Pinsel als XAML-Ressourcen

Sie können jeden Pinsel als XAML-Ressource mit Schlüssel in einem XAML-Ressourcenwörterbuch deklarieren. So können die gleichen Pinselwerte bequem dupliziert und auf eine Vielzahl von Elementen in einer UI angewendet werden. Die Pinselwerte werden dann freigegeben und in allen Fällen angewendet, in denen Sie auf die Pinselressource mit einer [{StaticResource}](https://msdn.microsoft.com/library/windows/apps/Mt185588)-Verwendung im XAML verweisen. Hierzu gehören auch Fälle, in denen mit einer XAML-Steuerelementvorlage auf den freigegebenen Pinsel verwiesen wird und es sich bei der Steuerelementvorlage selbst um eine XAML-Ressource mit Schlüssel handelt.

## Pinsel in Code

Pinsel werden in der Regel mit XAML und nicht mit Code angegeben. Das liegt daran, dass Pinsel normalerweise als XAML-Ressourcen definiert und Pinselwerte häufig von Entwicklungstools ausgegeben werden oder Teil einer XAML-UI-Definition sind. Für die Gelegenheiten, bei denen Sie einen Pinsel mithilfe von Code definieren möchten, stehen weiterhin alle [**Pinsel**](https://msdn.microsoft.com/library/windows/apps/BR228076)-Typen für die Codeinstanziierung zur Verfügung.

Verwenden Sie zum Erstellen eines [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)-Elements in Code den Konstruktor, der einen [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723)-Parameter akzeptiert. Übergeben Sie einen Wert, bei dem es sich um eine statische Eigenschaft der [**Colors**](https://msdn.microsoft.com/library/windows/apps/windows.ui.colors)-Klasse handelt:

```cs
SolidColorBrush blueBrush = new SolidColorBrush(Windows.UI.Colors.Blue);
```

```vb
Dim blueBrush as SolidColorBrush = New SolidColorBrush(Windows.UI.Colors.Blue)
```

```cpp
blueBrush = ref new SolidColorBrush(Windows::UI::Colors::Blue);
```

Verwenden Sie für [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703) und [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) den Standardkonstruktor, und rufen Sie dann andere APIs auf, bevor Sie diesen Pinsel für eine UI-Eigenschaft verwenden.

-   
              [
              **ImageSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imagebrush.imagesourceproperty.aspx) erfordert ein [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/BR243235) (keinen URI), wenn Sie einen [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) mithilfe von Code definieren. Falls es sich bei Ihrer Quelle um einen Datenstrom handelt, initialisieren Sie den Wert mit der [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522)-Methode. Ist Ihre Quelle ein (URI), der Ihrer App Inhalt mit dem **ms-appx**- oder dem **ms-resource**-Schema hinzufügt, verwenden Sie den [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/br243238.aspx)-Konstruktor, für den ein URI angegeben wird. Falls beim Abrufen oder Decodieren der Bildquelle Probleme mit der Zeitsteuerung auftreten und Sie alternativen Inhalt anzeigen müssen, bis die Bildquelle verfügbar ist, können Sie auch das [**ImageOpened**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.imageopened.aspx)-Ereignis behandeln.
-   Für [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703) müssen Sie möglicherweise [**Redraw**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewbrush.redraw.aspx) aufrufen, wenn Sie kürzlich die [**SourceName**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewbrush.sourcename.aspx)-Eigenschaft zurückgesetzt haben oder der Inhalt von [**WebView**](https://msdn.microsoft.com/library/windows/apps/BR227702) ebenfalls mittels Code geändert wird.

Codebeispiele finden Sie auf den Referenzseiten für [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703) und [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101).
 

 







<!--HONumber=Jul16_HO2-->


