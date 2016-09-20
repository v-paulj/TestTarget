---
author: Jwmsft
Description: "Mittels Verschiebung und Bildlauf können Benutzer Inhalte erreichen, die sich jenseits der Bildschirmgrenzen befinden."
title: "Richtlinien für Bildlaufleisten"
ms.assetid: 1BFF0E81-BF9C-43F7-95F6-EFC6BDD5EC31
label: Scroll bars
template: detail.hbs
ms.sourcegitcommit: c183f7390c5b4f99cf0f31426c1431066e1bc96d
ms.openlocfilehash: b390f8a2cbabf243bd4d73c16122648e3d4a0586

---
# Bildlaufleisten

<span class="sidebar_heading" style="font-weight: bold;">Wichtige APIs</span>

-   [**ScrollViewer-Klasse**](https://msdn.microsoft.com/library/windows/apps/br209527)
-   [**ZoomMode-Eigenschaft**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.zoommode.aspx)

Mittels Verschiebung und Bildlauf können Benutzer Inhalte erreichen, die sich jenseits der Bildschirmgrenzen befinden.

Ein Bildlaufanzeigesteuerelement umfasst so viele Inhalte, wie in den Viewport passen, und entweder eine oder zwei Bildlaufleisten. Fingerbewegungen können zum Verschieben und Zoomen verwendet werden (die Bildlaufleisten werden nur während der Bearbeitung eingeblendet), während der Zeiger für den Bildlauf genutzt werden kann. Bei einer Streichbewegung erfolgt die Verschiebung mit Trägheit.


            **Hinweis**  Windows: Es gibt, je nach ermitteltem Eingabegerät, zwei Anzeigemodi für das Verschieben – Verschiebungsindikatoren für Fingereingabe und Bildlaufleisten für andere Eingabegeräte, darunter Maus, Touchpad, Tastatur und Eingabestift.

![Beispiel für standardmäßige Bildlaufleisten und Verschiebungsindikatoren-Steuerelemente](images/SCROLLBAR.png)

## Beispiele

Ein [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.aspx) ermöglicht es, Inhalte in einem kleineren Bereich als der tatsächlichen Größe anzuzeigen. Wenn der Inhalt der Bildlaufanzeige nicht vollständig sichtbar ist, zeigt die Bildlaufanzeige Bildlaufleisten an, mit denen der Benutzer den sichtbaren Inhaltsbereich verschieben kann. Der Bereich, der den gesamten Inhalt der Bildlaufanzeige enthält, ist der *Umfang*. Der sichtbare Bereich des Inhalts ist der *Viewport*.

![Screenshot mit einem standardmäßigen Bildlaufleistensteuerelement](images/ScrollBar_Standard.jpg)

## Erstellen einer Bildlaufanzeige

Dieser XAML-Code veranschaulicht das Einfügen eines Bilds in einer Bildlaufanzeige und das Aktivieren des Zooms.

```xaml
<ScrollViewer ZoomMode="Enabled" MaxZoomFactor="10" 
              HorizontalScrollMode="Enabled" HorizontalScrollBarVisibility="Visible"
              Height="200" Width="200">
    <Image Source="Assets/Logo.png" Height="400" Width="400"/>
</ScrollViewer>
```

## ScrollViewer in einer Steuerelementvorlage

Normalerweise ist das ScrollViewer-Steuerelement Teil von anderen Steuerelementen. Eine ScrollViewer-Komponente zeigt zusammen mit der [**ScrollContentPresenter**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollcontentpresenter.aspx)-Klasse zur Unterstützung nur dann einen Viewport sowie Bildlaufleisten an, wenn der Layoutbereich des Hoststeuerelements einschränkt wird und kleiner als die Größe des erweiterten Inhalts ist. Dies ist häufig bei Listen der Fall, daher enthalten [**ListView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listview.aspx)- und [**GridView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridview.aspx)-Vorlagen immer ScrollViewer. 
            [
              **TextBox**
            ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx) und [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx) umfassen ebenfalls ScrollViewer in ihren Vorlagen.

Wenn eine **ScrollViewer**-Komponente in einem Steuerelement vorhanden ist, ist im Hoststeuerelement häufig die Ereignisbehandlung für bestimmte Eingabeereignisse und Bearbeitungen integriert, mit denen ein Bildlauf für den Inhalt durchgeführt werden kann. GridView interpretiert z. B. eine Wischbewegung, wodurch für den Inhalt ein horizontaler Bildlauf durchgeführt wird. Die Eingabeereignisse und Manipulationen von Rohdaten, die das Hoststeuerelement empfängt, werden als durch das Steuerelement behandelt betrachtet, und untergeordnete Ereignisse wie [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.pointerpressed.aspx) werden nicht ausgelöst oder per Bubbling an übergeordnete Container weitergeleitet. Sie können das integrierte Verhalten des Steuerelements teilweise ändern, indem Sie eine Steuerelementklasse und die virtuellen **On***-Methoden für Ereignisse überschreiben oder eine neue Vorlage für das Steuerelement verwenden. In beiden Fällen ist es allerdings nicht unkompliziert, das ursprüngliche Standardverhalten zu reproduzieren, das in der Regel vorhanden ist, damit das Steuerelement wie erwartet auf Ereignisse und Eingabeaktionen und -gesten des Benutzers reagiert. Sie sollten daher genau überlegen, ob das Eingabeereignis wirklich ausgelöst werden soll. Sie sollten überprüfen, ob andere Eingabeereignisse oder Gesten vorhanden sind, die nicht von dem Steuerelement behandelt werden, und diese im Entwurf für die App oder die Steuerelementinteraktion verwenden.

Damit Steuerelemente, die einen ScrollViewer enthalten, einige Verhaltensweisen und Eigenschaften innerhalb der ScrollViewer-Komponente steuern können, definiert ScrollViewer eine Reihe von angefügten XAML-Eigenschaften, die in Stilen festgelegt und in Vorlagenbindungen verwendet werden können. Weitere Informationen zu angefügten Eigenschaften finden Sie unter [Übersicht über angefügte Eigenschaften](../xaml-platform/attached-properties-overview.md).

**Angefügte XAML-Eigenschaften für ScrollViewer**

ScrollViewer definiert die folgenden angefügten XAML-Eigenschaften:
- [ScrollViewer.BringIntoViewOnFocusChange](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.bringintoviewonfocuschange.aspx) 
- [ScrollViewer.HorizontalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility.aspx) 
- [ScrollViewer.HorizontalScrollMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode.aspx)
- [ScrollViewer.IsDeferredScrollingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isdeferredscrollingenabled.aspx) 
- [ScrollViewer.IsHorizontalRailEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.ishorizontalrailenabled.aspx)
- [ScrollViewer.IsHorizontalScrollChainingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.ishorizontalscrollchainingenabled.aspx) 
- [ScrollViewer.IsScrollInertiaEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isscrollinertiaenabled.aspx)
- [ScrollViewer.IsVerticalRailEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isverticalrailenabled.aspx)
- [ScrollViewer.IsVerticalScrollChainingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isverticalscrollchainingenabled.aspx) 
- [ScrollViewer.IsZoomChainingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.iszoominertiaenabled.aspx)
- [ScrollViewer.IsZoomInertiaEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.iszoominertiaenabled.aspx)
- [ScrollViewer.VerticalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibilityproperty.aspx) 
- [ScrollViewer.VerticalScrollMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.verticalscrollmode.aspx)
- [ScrollViewer.ZoomMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.zoommode.aspx)

Diese angefügten XAML-Eigenschaften sind für Fälle vorgesehen, in denen ScrollViewer implizit ist, z. B. wenn der ScrollViewer in der Standardvorlage für ListView oder GridView vorhanden ist und Sie das Bildlaufverhalten des Steuerelements ohne Zugriff auf Vorlagenelemente steuern möchten.

Im Folgenden wird z. B. erläutert, wie Sie die vertikalen Bildlaufleisten für die integrierte Bildlaufanzeige einer ListView immer sichtbar machen.
```xaml
<ListView ScrollViewer.VerticalScrollBarVisibility="Visible"/> 
```

In Fällen, in denen ein ScrollViewer explizit in Ihrem XAML-Code vorhanden ist, wie im folgenden Beispielcode dargestellt, müssen Sie keine Syntax mit angefügten Eigenschaften verwenden. Verwenden Sie einfach eine Attributsyntax, z. B. `<ScrollViewer VerticalScrollBarVisibility="Visible"/>`.


## Empfehlungen

-   Verwenden Sie die Verschiebung entlang einer Achse für Inhaltsbereiche, die über eine Viewportgrenze (vertikal oder horizontal) hinausgehen. Verwenden Sie die Verschiebung entlang zweier Achsen für Inhaltsbereiche, die über beide Viewportgrenzen (vertikal und horizontal) hinausgehen.
-   Verwenden Sie im Listenfeld, in der Dropdownliste, im Texteingabefeld, in der Rasteransicht, der Listenansicht und für Hubsteuerelemente die integrierte Bildlauffunktionalität. Wenn die Anzahl Elemente so groß ist, dass sie nicht alle gleichzeitig angezeigt werden können, hat der Benutzer mit diesen Steuerelementen die Möglichkeit, einen vertikalen oder horizontalen Bildlauf durch die Elementliste durchzuführen.
-   Wenn der Benutzer die Verschiebung in beide Richtungen um einen größeren Bereich herum ausführen und möglicherweise auch zoomen soll (wenn Sie dem Benutzer beispielsweise das Verschieben und Zoomen über ein Bild in voller Größe ermöglichen möchten, anstatt ein Bild mit an den Bildschirm angepasster Größe zu verwenden), positionieren Sie das Bild in einer Bildlaufanzeige.
-   Wenn der Benutzer in einer langen Textpassage einen Bildlauf ausführen wird, konfigurieren Sie die Bildlaufanzeige ausschließlich für den vertikalen Bildlauf.
-   Bei Verwendung einer Bildlaufanzeige darf diese nur ein Objekt umfassen. Beachten Sie, dass es sich bei dem einen Objekt um einen Layoutbereich handeln kann, der wiederum eine beliebige Anzahl eigener Objekte enthält.

## Verwandte Themen

**Für Entwickler (XAML)**
* [**ScrollViewer-Klasse**](https://msdn.microsoft.com/library/windows/apps/br209527)



<!--HONumber=Jun16_HO4-->


