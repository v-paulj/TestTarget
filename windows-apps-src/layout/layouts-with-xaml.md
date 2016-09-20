---
author: Jwmsft
Description: "XAML bietet Ihnen ein flexibles Layoutsystem, um eine reaktionsfähige Benutzeroberfläche zu erstellen."
title: Definieren von Layouts mit XAML
ms.assetid: 8D4E4162-1C9C-48F4-8A94-34976FB17079
label: Page layouts with XAML
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 681023be35b01eac84272a73d1ae3e7a459351db
ms.openlocfilehash: 8ca8852df2747410b9d139b55a4f24366cc0eefc

---
# Definieren von Seitenlayouts mit XAML

XAML bietet Ihnen ein flexibles Layoutsystem, mit dem Sie die automatische Größenanpassung, Layoutpanel, visuelle Zustände und sogar getrennte UI-Definitionen verwenden können, um eine reaktionsfähige Benutzeroberfläche zu erstellen. Mit einem flexiblen Design können Sie für Ihre App ein großartiges Erscheinungsbild auf Bildschirmen mit unterschiedlichen Größen, Auflösungen, Pixeldichten und Ausrichtungen von App-Fenstern erstellen.

Hier erörtern wir die Verwendung von XAML-Eigenschaften und Layoutpanels zur Erstellung einer reaktionsfähigen und adaptiven App. Wir bauen auf wichtigen Informationen zu Techniken des reaktionsfähigen UI-Designs unter [Einführung in das UWP-App-Design](../layout/design-and-ui-intro.md). Sie sollten wissen, was effektive Pixel sind, und die einzelnen Techniken für das reaktionsfähige Design verstehen: Ändern der Position, Ändern der Größe, Neuanordnen, Einblenden, Ersetzen und Ändern der Architektur.

> **Hinweis**&nbsp;&nbsp;Das App-Layout beginnt mit dem von Ihnen ausgewählten Navigationsmodell, z. B., ob Sie ein [**Pivot**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx) mit dem Modell [„Registerkarten und Pivot“](../controls-and-patterns/tabs-pivot.md) oder [**SplitView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.splitview.aspx) mit dem Modell [„Navigationsbereich“](../controls-and-patterns/nav-pane.md) verwenden. Weitere Informationen hierzu finden Sie unter [Navigationsdesigngrundlagen für UWP-Apps](../layout/navigation-basics.md). Hier sprechen wir über Techniken, wie das Layout einer einzelnen Seite oder Gruppe von Elementen reaktionsfähig gestaltet werden kann. Diese Informationen gelten unabhängig davon, welches Navigationsmodell Sie für Ihre App auswählen.

Das XAML-Framework bietet mehrere Ebenen der Optimierung, die Sie bei der Erstellung einer reaktionsfähigen Benutzeroberfläche verwenden können.
- **Dynamisches Layout**
    Verwenden Sie Layouteigenschaften und -panels, um Ihre Standard-UI dynamisch zu gestalten.

    Die Grundlage für ein dynamisches Layout ist die richtige Verwendung von Layouteigenschaften und -panels, um die Position und Größe von Inhalten zu ändern und Inhalte neu anzuordnen. Sie können eine feste Größe für ein Element festlegen oder eine automatische Größenanpassung verwenden, damit das übergeordnete Layoutpanel die Größe anpassen kann. Die verschiedenen [**Panel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.panel.aspx)-Klassen, z.B. [**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx), [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx), [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx) und [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx), bieten verschiedene Möglichkeiten zur Größenanpassung und Positionierung ihrer untergeordneten Elemente.

- **Adaptives Layout**
    Verwenden Sie visuelle Zustände, um basierend auf der Fenstergröße oder andere Änderungen größere Änderungen an Ihrer UI vorzunehmen.

    Wenn sich die Größe Ihres App-Fensters über einen bestimmten Punkt vergrößert oder verkleinert, können Sie Layouteigenschaften ändern, um die Größe und Position von Abschnitten Ihrer UI zu ändern oder diese neu anzuordnen, einzublenden oder zu ersetzen. Sie können verschiedene visuelle Zustände für Ihre Benutzeroberfläche definieren und diese anwenden, wenn die Fensterbreite oder Fensterhöhe einen bestimmten Schwellenwert überschreitet. Ein [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.adaptivetrigger.aspx) bietet eine einfache Möglichkeit, um einen Schwellenwert (auch „Haltepunkt“ genannt) festzulegen, wenn ein Zustand angewendet wird.

- **Maßgeschneidertes Layout**
    Ein maßgeschneidertes Layout ist für eine bestimmte Gerätefamilie oder verschiedene Bildschirmgrößen optimiert. Das Layout sollte innerhalb der Gerätefamilie im Rahmen der unterstützten Fenstergrößen weiterhin reaktionsfähig bleiben und sich an Änderungen anpassen können.
    > **Hinweis**&nbsp;&nbsp; Mit [Continuum für Smartphones](http://go.microsoft.com/fwlink/p/?LinkID=699431) können Benutzer ihre Smartphones mit einem Bildschirm, einer Maus und einer Tastatur verbinden. Mithilfe dieser Funktion werden die Zeilen zwischen Smartphone- und Desktop-Gerätefamilien weichgezeichnet.

    Zu den Methoden für die Anpassung gehören
    - Erstellen eines benutzerdefinierten Auslösers

    Sie können einen Gerätefamilien-Auslöser erstellen und seine Setter bearbeiten, beispielsweise für adaptive Auslöser.

    - Verwenden Sie separate XAML-Dateien, um unterschiedliche Ansichten für jede Gerätefamilie zu definieren.

    Sie können separate XAML-Dateien mit derselben Codedatei verwenden, um Ansichten der Benutzeroberfläche pro Gerätefamilie zu definieren.

    - Verwenden Sie separate XAML und separaten Code, um unterschiedliche Implementierungen für jede Gerätefamilie bereitzustellen.

    Sie können unterschiedliche Implementierungen einer Seite (XAML und Code) bereitstellen und dann basierend auf der Gerätefamilie, Bildschirmgröße oder anderen Faktoren zu einer bestimmten Implementierung navigieren.

## Layouteigenschaften und -panels

Als Layout wird das Anpassen und Positionieren von Objekten auf der Benutzeroberfläche bezeichnet. Sie müssen visuelle Objekte in einem Panel oder einem anderen Containerobjekt platzieren, um sie zu positionieren. Das XAML-Framework bietet verschiedene Panel-Klassen, z. B. [**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx), [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx), [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx) und [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx), die als Container dienen und Ihnen das Platzieren und Anordnen der darin enthaltenen UI-Elemente ermöglichen.

Das XAML-Layoutsystem unterstützt sowohl statische als auch dynamische Layouts. In einem statischen Layout weisen Sie Steuerelementen feste Größen in Pixel und Positionen zu. Wenn der Benutzer die Auflösung oder Ausrichtung seines Geräts ändert, bleibt die UI unverändert. Statische Layouts können auf bestimmte Formfaktoren und Anzeigegrößen beschnitten werden.

Dynamische Layouts passen sich durch Vergrößerung, Verkleinerung und Umbrüche an den auf einem Gerät verfügbaren Platz für die Anzeige an. Um ein dynamisches Layout zu erstellen, verwenden Sie die automatische oder proportionale Größenanpassung für Elemente, Ausrichtungen, Ränder und Abstände, und lassen Sie die Layoutpanels die Position für ihre untergeordneten Elemente wie erforderlich positionieren. Sie ordnen untergeordnete Elemente an, indem Sie ihre Platzierung in Beziehung zueinander und ihre Größe relativ zu ihrem Inhalt und/oder übergeordneten Element angeben.

In der Praxis verwenden Sie eine Kombination aus statischen und dynamischen Elemente zum Erstellen der Benutzeroberfläche. Statische Elemente und Werte kommen an bestimmten Stellen immer noch vor, es sollte jedoch darauf geachtet werden, dass die Gesamt-UI reaktionsfähig ist und sich an verschiedene Auflösungen, Layouts und Ansichten anpasst.

### Layouteigenschaften

Zum Bestimmen der Größe und Position eines Elements legen Sie seine Layouteigenschaften fest. Im Folgenden werden einige gängige Layouteigenschaften und ihre Auswirkungen beschrieben.

**Höhe und Breite**

Legen Sie die Eigenschaften [**Height**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx) und [**Width**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.width.aspx) fest, um die Größe eines Elements anzugeben. Sie können feste Werte verwenden, die in effektiven Pixeln gemessen werden, oder Sie können eine automatische oder proportionale Größenanpassung verwenden. Verwenden Sie zum Abrufen der Größe eines Elements zur Laufzeit die Eigenschaften [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.actualheight.aspx) und [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.actualwidth.aspx) anstelle von „Height“ und „Width“.

Verwenden Sie die automatische Größenanpassung, damit die Größe von UI-Elementen entsprechend ihren Inhalten oder der Größe des übergeordneten Containers geändert wird. Sie können die automatische Größenanpassung auch für die Zeilen und Spalten eines Rasters verwenden. Um die automatische Größenanpassung zu verwenden, legen Sie für „Height“ und/oder „Width“ von UI-Elementen **Auto** fest.

> **Hinweis**&nbsp;&nbsp;Ob die Größe eines Elements an seinen Inhalt oder seinen Container angepasst wird, ist vom Wert der Eigenschaften [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.horizontalalignment.aspx) und [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.verticalalignment.aspx) abhängig und davon, wie der übergeordnete Container die Größenanpassung seiner untergeordneten Elemente behandelt. Weitere Informationen finden Sie unter [Ausrichtung]() und [Layoutpanels]() weiter unten in diesem Artikel.

Die proportionale Größenanpassung, die auch als *Größenanpassung mit Sternvariable* bezeichnet wird, wird zum gleichmäßigen Aufteilen des verfügbaren Platzes auf die Zeilen und Spalten eines Rasters verwendet. In XAML werden Sternwerte als \* ausgedrückt (bzw. *n*\* für gleichmäßige Größenanpassung mit Sternvariable). Möchten Sie also z.B. angeben, dass eine Spalte fünfmal breiter als die zweite Spalte eines zweispaltigen Layouts ist, verwenden Sie "5\*" und "\*" für die [**Width**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.columndefinition.width.aspx)-Eigenschaften der [**ColumnDefinition**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.columndefinition.aspx)-Elemente.

In diesem Beispiel wird die feste, automatische und proportionale Größenanpassung in einem [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx) mit 4 Spalten kombiniert.

Spalte|Breite|Hinweise
------|------|------
Column_1 | **Auto** | Die Breite der Spalte wird entsprechend ihres Inhalts angepasst.
Column_2 | * | Nach dem Berechnen der Spalten mit automatischer Breite erhält diese Spalte einen Teil der verbleibenden Breite. Column_2 ist nur halb so breit wie Column_4.
Column_3 | **44** | Die Spalte ist 44 Pixel breit.
Column_4 | **2**\* | Nach dem Berechnen der Spalten mit automatischer Breite erhält diese Spalte einen Teil der verbleibenden Breite. Column_4 ist doppelt so breit wie Column_2.

Die standardmäßige Spaltenbreite beträgt "*", daher Sie müssen diesen Wert für die zweite Spalte nicht explizit festlegen.

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="44"/>
        <ColumnDefinition Width="2*"/>
    </Grid.ColumnDefinitions>
    <TextBlock Text="Column 1 sizes to its conent." FontSize="24"/>
</Grid>
```

Im Visual Studio-XAML-Designer sieht das Ergebnis wie folgt aus.

![Ein Raster mit vier Spalten im Visual Studio-Designer](images/xaml-layout-grid-in-designer.png)

**Größenbeschränkungen**

Wenn Sie die automatische Größenanpassung in Ihrer Benutzeroberfläche verwenden, müssen Sie möglicherweise dennoch Einschränkungen für die Größe eines Elements festlegen. Sie können die Eigenschaften [**MinWidth**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.minwidth.aspx)/[**MaxWidth**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.maxwidth.aspx) und [**MinHeight**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.minheight.aspx)/[**MaxHeight**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.maxheight.aspx) festlegen, um Werte anzugeben, die die Größe eines Elements begrenzen, während weiterhin eine dynamische Größenanpassung möglich ist.

In einem „Grid“ kann „MinWidth/MaxWidth“ auch mit Spaltendefinitionen verwendet werden, und „MinHeight/MaxHeight“ kann mit Zeilendefinitionen verwendet werden.

**Ausrichtung**

Verwenden Sie die Eigenschaften [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.horizontalalignment.aspx) und [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.verticalalignment.aspx), um anzugeben, wie ein Element innerhalb seines übergeordneten Containers positioniert werden soll.
- Die Werte für **HorizontalAlignment** sind **Left**, **Center**, **Right** und **Stretch**.
- Die Werte für **VerticalAlignment** sind **Top**, **Center**, **Bottom** und **Stretch**.

Bei der **Stretch**-Ausrichtung füllen die Elemente den Bereich, der für sie im übergeordneten Container bereitgestellt wird. „Stretch“ ist der Standardwert für beide Ausrichtungseigenschaften. Einige Steuerelemente, z.B. [**Button**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.button.aspx) überschreiben diesen Wert jedoch in ihrem Standardstil.
Alle Elemente, die untergeordnete Elemente haben können, können den Wert „Stretch“ für die Eigenschaften „HorizontalAlignment“ und „VerticalAlignment“ eindeutig behandeln. Angenommen, ein Element, das die standardmäßigen „Stretch“-Werte verwendet und in einem „Grid“ platziert ist, wird so gestreckt, dass es die Zelle füllt, die es enthält. Dasselbe Element in einem Canvas-Panel wird an die Größe seines Inhalts angepasst. Weitere Informationen dazu, wie die einzelnen Panels den „Stretch“-Wert behandeln, finden Sie unter im Artikel [Layoutpanels](layout-panels.md).

Weitere Informationen finden Sie im Artikel [Ausrichtung, Rand und Abstand](alignment-margin-padding.md) und auf den Referenzseiten [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.horizontalalignment.aspx) und [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.verticalalignment.aspx).

Steuerelemente verfügen auch über die Eigenschaften [**HorizontalContentAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.horizontalcontentalignment.aspx) und [**VerticalContentAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.verticalcontentalignment.aspx), mit denen Sie angeben, wie der Inhalt positioniert wird. Nicht alle Steuerelemente verwenden diese Eigenschaften. Sie betreffen nur das Layoutverhalten für ein Steuerelement, wenn seine Vorlage die Eigenschaften als Quelle eines „HorizontalAlignment“/„VerticalAlignment“-Werts für Presenter oder Inhaltsbereiche verwendet.

Verwenden Sie für [TextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx), [TextBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx) und [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx) die **TextAlignment**-Eigenschaft, um die Ausrichtung von Text im Steuerelement zu steuern.

**Ränder und Abstände**

Legen Sie die [**Margin**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.margin.aspx)-Eigenschaft fest, um die Größe des leeren Bereichs um ein Element zu steuern. „Margin“ fügt „ActualHeight“ und „ActualWidth“ keine Pixel hinzu und wird für Treffertests und Eingabeereignisse der Quellenentnahme auch nicht als Teil des Elements betrachtet.

Legen Sie die [**Padding**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.padding.aspx)-Eigenschaft fest, um die Größe des Bereichs zwischen dem inneren Rand eines Elements und seinem Inhalt zu steuern. Ein positiver Padding-Wert verkleinert den Inhaltsbereich des Elements.

Diese Abbildung zeigt, wie Rand und Abstand auf ein Element angewendet werden.

![Rand und Abstand](images/xaml-layout-margins-padding.png)

Die Werte für links, rechts, oben und unten für „Margin“ und „Padding“ müssen nicht symmetrisch sein und können auf negative Werte festgelegt werden. Weitere Informationen finden Sie unter [Ausrichtung, Rand und Abstand](alignment-margin-padding.md) sowie auf den Referenzseiten [**Margin**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.margin.aspx) oder [**Padding**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.padding.aspx).

Sehen wir uns nun die Effekte „Margin“ und „Padding“ auf echte Steuerelemente an. Hier sehen Sie ein „TextBox“ innerhalb eines „Grid“ mit dem Standardwert 0 für „Margin“ und „Padding“.

![TextBox mit Rand und Abstand von 0](images/xaml-layout-textbox-no-margins-padding.png)

Hier sehen Sie das gleiche „“TextBox und „Grid“ mit „Margin“- und „Padding“-Werten für das „TextBox“, wie in diesem XAML dargestellt.

```xaml
<Grid BorderBrush="Blue" BorderThickness="4" Width="200">
    <TextBox Text="This is text in a TextBox." Margin="20" Padding="24,16"/>
</Grid>
```

![TextBox mit positiven Rand- und Abstandswerten](images/xaml-layout-textbox-with-margins-padding.png)

**Sichtbarkeit**

Sie können ein Element ein- oder ausblenden, indem Sie seine [**Visibility**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.visibility.aspx)-Eigenschaft auf einen der [**Visibility**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visibility.aspx)-Enumerationswerte festlegen: **Visible** oder **Collapsed**. Wenn ein Element „Collapsed“ ist, verbraucht es keinen Platz im Benutzeroberflächenlayout.

Sie können die Visibility-Eigenschaft eines Elements im Code oder in einem visuellen Zustand ändern. Wenn die Visibility eines Elements geändert wird, werden auch alle untergeordneten Elemente geändert. Sie können Abschnitte der Benutzeroberfläche ersetzen, indem Sie ein Panel einblenden, während Sie ein anderes reduzieren.

> **Tipp**&nbsp;&nbsp;Wenn die Benutzeroberfläche Elemente enthält, die standardmäßig **Collapsed** sind, werden die Objekte beim Start dennoch erstellt, obwohl sie nicht sichtbar sind. Sie können das Laden dieser Elemente bis zu ihrer Anzeige verzögern, indem Sie das **x:DeferLoadStrategy attribute** auf „Lazy“ festlegen. Dies kann die Leistung beim Starten verbessern. Weitere Informationen finden Sie unter [x: DeferLoadStrategy-Attribut](../xaml-platform/x-deferloadstrategy-attribute.md).

### Stilressourcen

Sie müssen nicht jeden Eigenschaftswert einzeln für ein Steuerelement festlegen. Es ist in der Regel effizienter, Eigenschaftswerte in einer [**Style**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.aspx)-Ressource zu gruppieren und den Stil auf ein Steuerelement anzuwenden. Dies gilt insbesondere dann, wenn Sie die gleichen Eigenschaftswerte auf viele Steuerelemente anwenden müssen. Weitere Informationen zum Verwenden von Stilen finden Sie unter [Formatieren von Steuerelementen](../controls-and-patterns/styling-controls.md).

### Layoutpanels

Die meisten App-Inhalte können in Gruppen oder Hierarchien gegliedert werden. Layoutpanels dienen zum Gruppieren und Anordnen von UI-Elementen in Ihrer App. Die Hauptaspekte, die es bei der Wahl eines Layoutpanels zu berücksichtigen gilt, sind die Positionierung und Anpassung der Größe von untergeordneten Elementen durch das Panel. Gegebenenfalls müssen Sie auch die Schichtung untergeordneter Elemente berücksichtigen, die sich überschneiden.

Es folgt ein Vergleich der Hauptfunktionen der Panelsteuerelemente, die im XAML-Framework bereitgestellt werden.

Panelsteuerelement | Beschreibung
--------------|------------
[**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx) | **Canvas** unterstützt keine dynamische Benutzeroberfläche; Sie steuern alle Aspekte der Positionierung und der Anpassung der Größe von untergeordneten Elementen. Sie verwenden dieses Panel in der Regel für spezielle Fälle wie das Erstellen von Grafiken oder das Definieren kleiner statischer Bereiche einer größeren adaptiven Benutzeroberfläche. Sie können Code oder visuelle Zustände verwenden, um Elemente zur Laufzeit neu zu positionieren.<ul><li>Die Elemente werden mithilfe der angefügten Eigenschaften Canvas.Top und Canvas.Left absolut positioniert.</li><li>Die Schichtung kann mithilfe der angefügten Eigenschaft Canvas.ZIndex explizit angegeben werden.</li><li>Streckungswerte für „HorizontalAlignment“/„VerticalAlignment“ werden ignoriert. Wenn die Größe des Elements nicht explizit festgelegt wird, wird die Größe an den Inhalt angepasst.</li><li>Untergeordnete Inhalte werden nicht visuell abgeschnitten, wenn sie größer sind als das Panel. </li><li>Untergeordnete Inhalte werden nicht durch die Grenzen des Panels beschränkt.</li></ul>
[**Raster**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx) | **Grid** unterstützt das dynamische Ändern der Größe von untergeordneten Elementen. Sie können Code oder visuelle Zustände verwenden, um die Position von Elementen zu ändern und sie dynamisch zu umbrechen.<ul><li>Mithilfe der angefügten Eigenschaften Grid.Row und Grid.Column können Elemente in Zeilen und Spalten angeordnet werden.</li><li>Mithilfe der angefügten Eigenschaften „Grid.RowSpan“ und „Grid.ColumnSpan“ können sich die Elemente über mehrere Spalten und Zeilen erstrecken.</li><li>Streckungswerte für „HorizontalAlignment“/„VerticalAlignment“ werden berücksichtigt. Wird die Größe eines Elements nicht explizit festgelegt, wird das Element gestreckt, sodass es den zur Verfügung stehenden Platz in der Rasterzelle ausfüllt.</li><li>Untergeordnete Inhalte werden visuell abgeschnitten, wenn sie größer sind als das Panel.</li><li>Die Größe von Inhalten wird durch die Grenzen des Panels beschränkt, für bildlauffähige Inhalte werden daher bei Bedarf Bildlaufleisten angezeigt.</li></ul>
[**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx) | <ul><li>Elemente werden in Bezug auf den Rand oder die Mitte des Panels und im Verhältnis zueinander angeordnet.</li><li>Elemente werden unter Verwendung verschiedener angefügter Eigenschaften positioniert, die die Ausrichtung des Panels sowie von gleichgeordneten Elementen und die Position gleichgeordneter Elemente steuern. </li><li>Streckungswerte für „HorizontalAlignment“/„VerticalAlignment“ werden ignoriert, es sei denn, die angefügten RelativePanel-Eigenschaften für die Ausrichtung verursachen eine Streckung (ein Element wird zum Beispiel sowohl am rechten als auch am linken Rand des Panels ausgerichtet). Wenn die Größe eines Elements nicht explizit festgelegt und es nicht gestreckt wird, wird die Größe an den Inhalt angepasst.</li><li>Untergeordnete Inhalte werden visuell abgeschnitten, wenn sie größer sind als das Panel.</li><li>Die Größe von Inhalten wird durch die Grenzen des Panels beschränkt, für bildlauffähige Inhalte werden daher bei Bedarf Bildlaufleisten angezeigt.</li></ul>
[**StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx) |<ul><li>Die Elemente werden in einer Linie gestapelt – entweder vertikal oder horizontal.</li><li>Streckungswerte für „HorizontalAlignment“/„VerticalAlignment“ werden in der entgegengesetzten Richtung der „Orientation“-Eigenschaft berücksichtigt. Wird die Größe eines Elements nicht explizit festgelegt, wird das Element gestreckt, sodass es die zur Verfügung stehende Breite (oder Höhe, falls die Ausrichtung auf „Horizontal“ festgelegt ist) ausfüllt. In der von der „Orientation“-Eigenschaft angegebenen Richtung wird ein Element an seinen Inhalt angepasst.</li><li>Untergeordnete Inhalte werden visuell abgeschnitten, wenn sie größer sind als das Panel.</li><li>Die Größe des Inhalts wird nicht durch die Grenzen des Panels in der von der „Orientation“-Eigenschaft angegebenen Richtung beschränkt; bildlauffähige Inhalte werden daher über die Panelgrenzen hinaus gestreckt und weisen keine Bildlaufleisten auf. Sie müssen die Höhe (oder Breite) des untergeordneten Inhalts explizit auf die Bildlaufleisten beschränken, damit die Bildlaufleisten angezeigt werden.</li></ul>
[**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.aspx) |<ul><li>Die Elemente werden in Zeilen oder Spalten angeordnet, die automatisch auf eine neue Zeile oder Spalte umgebrochen werden, wenn der „MaximumRowsOrColumns“-Wert erreicht ist.</li><li>Ob die Elemente in Zeilen oder Spalten angeordnet werden, hängt von der „Orientation“-Eigenschaft ab.</li><li>Mithilfe der angefügten Eigenschaften „VariableSizedWrapGrid.RowSpan“ und „VariableSizedWrapGrid.ColumnSpan“ können sich die Elemente über mehrere Spalten und Zeilen erstrecken.</li><li>Streckungswerte für „HorizontalAlignment“/„VerticalAlignment“ werden ignoriert. Die Größe der Elemente wird gemäß den Eigenschaften „ItemHeight“ und „ItemWidth“ angepasst. Wenn diese Eigenschaften nicht festgelegt werden, wird das Element in der ersten Zelle an seinen Inhalt angepasst, und alle anderen Zellen erben diese Größe.</li><li>Untergeordnete Inhalte werden visuell abgeschnitten, wenn sie größer sind als das Panel.</li><li>Die Größe von Inhalten wird durch die Grenzen des Panels beschränkt, für bildlauffähige Inhalte werden daher bei Bedarf Bildlaufleisten angezeigt.</li></ul>

Ausführliche Informationen und Beispiele für diese Panels finden Sie unter [Layoutpanels](layout-panels.md). Weitere Informationen finden Sie auch im [Beispiel für reaktionsfähige Designtechniken](http://go.microsoft.com/fwlink/p/?LinkId=620024).

Mit Layoutpanels können Sie die Benutzeroberfläche als logische Steuerelementgruppen strukturieren. Wenn Sie diese mit den entsprechenden Eigenschaftseinstellungen verwenden, erhalten Sie Unterstützung für die automatische Größenanpassung, die Neupositionierung und die Neuformatierung von UI-Elementen. Die meisten Benutzeroberflächenlayouts müssen jedoch weiter bearbeitet werden, wenn erhebliche Änderungen an der Fenstergröße vorgenommen werden. Hierzu können Sie visuelle Zustände verwenden.

## Visuelle Zustände und Zustandsauslöser

Verwenden Sie visuelle Zustände, um Abschnitte Ihrer Benutzeroberfläche basierend auf der Bildschirmgröße oder anderen Faktoren neu zu positionieren, neu zu formatieren, einzublenden oder zu ersetzen. Ein [**VisualState**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstate.aspx) definiert Eigenschaftswerte, die auf ein Element angewendet werden, wenn es sich in einem bestimmten Zustand befindet. Gruppieren Sie visuelle Zustände in einem [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstatemanager.aspx), der den entsprechenden „VisualState“ anwendet, wenn die angegebenen Bedingungen erfüllt werden.

### Festlegen von visuellen Zuständen im Code

Um einen visuellen Zustand aus Code anzuwenden, rufen Sie die [**VisualStateManager.GoToState**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstatemanager.gotostate.aspx)-Methode auf. Um beispielsweise einen Zustand anzuwenden, wenn das App-Fenster eine bestimmte Größe erreicht, behandeln Sie das [**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.window.sizechanged.aspx)-Ereignis, und rufen Sie **GoToState** auf, um den entsprechenden Zustand anzuwenden.

Hier enthält eine [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstategroup.aspx) zwei VisualState-Definitionen. Die erste Definition, `DefaultState`, ist leer. Wenn sie angewendet wird, werden die in der XAML-Seite definierten Werte angewendet. Die zweite Definition, `WideState`, ändert die [**DisplayMode**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.splitview.displaymode.aspx)-Eigenschaft der [**SplitView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.splitview.aspx) in **Inline** und öffnet den Bereich. Dieser Zustand wird im „SizeChanged“-Ereignishandler angewendet, wenn die Fensterbreite mindestens 720effektive Pixel beträgt.

```xaml
<Page ...>
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="DefaultState">
                        <Storyboard>
                        </Storyboard>
                    </VisualState>

                <VisualState x:Name="WideState">
                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames
                            Storyboard.TargetProperty="SplitView.DisplayMode"
                            Storyboard.TargetName="mySplitView">
                            <DiscreteObjectKeyFrame KeyTime="0">
                                <DiscreteObjectKeyFrame.Value>
                                    <SplitViewDisplayMode>Inline</SplitViewDisplayMode>
                                </DiscreteObjectKeyFrame.Value>
                            </DiscreteObjectKeyFrame>
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames
                            Storyboard.TargetProperty="SplitView.IsPaneOpen"
                            Storyboard.TargetName="mySplitView">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="True"/>
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <SplitView x:Name="mySplitView" DisplayMode="CompactInline"
                   IsPaneOpen="False" CompactPaneLength="20">
            <!-- SplitView content -->

            <SplitView.Pane>
                <!-- Pane content -->
            </SplitView.Pane>
        </SplitView>
    </Grid>
</Page>
```

```csharp
private void CurrentWindow_SizeChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
{
    if (e.Size.Width >= 720)
        VisualStateManager.GoToState(this, "WideState", false);
    else
        VisualStateManager.GoToState(this, "DefaultState", false);
}
```

### Festlegen von visuellen Zuständen im XAML-Markup

Vor Windows 10 erforderten „VisualState“-Definitionen [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.storyboard.aspx)-Objekte für Eigenschaftsänderungen, und Sie mussten **GoToState** im Code aufrufen, um den Zustand anzuwenden. Dies wird im vorherigen Beispiel veranschaulicht. Sie werden immer noch viele Beispiele finden, in denen diese Syntax verwendet wird, oder Sie verfügen möglicherweise über vorhandenen Code, der sie verwendet.

Ab Windows10 können Sie die hier dargestellte vereinfachte [**Setter**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.setter.aspx)-Syntax verwenden, und Sie können [**StateTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.statetrigger.aspx)-Elemente im XAML-Markup verwenden, um den Zustand anzuwenden. Sie verwenden „StateTrigger“-Elemente, um einfache Regeln zu erstellen, die automatisch Änderungen des visuellen Zustands als Reaktion auf ein App-Ereignis auslösen.

In diesem Beispiel wird die gleiche Aufgabe wie im vorherigen Beispiel ausgeführt, es wird jedoch die vereinfachte **Setter**-Syntax anstelle eines Storyboard zum Definieren von Eigenschaftsänderungen verwendet. Anstatt „GoToState“ aufzurufen, wird der integrierte [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.adaptivetrigger.aspx)-Zustandsauslöser verwendet, um den Zustand anzuwenden. Bei Verwendung von Zustandsauslösern müssen Sie keinen leeren `DefaultState` definieren. Die Standardeinstellungen werden automatisch erneut angewendet, wenn die Bedingungen des Zustandsauslösers nicht mehr erfüllt sind.

```xaml
<Page ...>
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <!-- VisualState to be triggered when the
                             window width is >=720 effective pixels. -->
                        <AdaptiveTrigger MinWindowWidth="720" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="mySplitView.DisplayMode" Value="Inline"/>
                        <Setter Target="mySplitView.IsPaneOpen" Value="True"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <SplitView x:Name="mySplitView" DisplayMode="CompactInline"
                   IsPaneOpen="False" CompactPaneLength="20">
            <!-- SplitView content -->

            <SplitView.Pane>
                <!-- Pane content -->
            </SplitView.Pane>
        </SplitView>
    </Grid>
</Page>
```

> **Wichtig**&nbsp;&nbsp;Im vorherigen Beispiel wird die angefügte „VisualStateManager.VisualStateGroups“-Eigenschaft im **Grid**-Element festgelegt. Bei Verwendung von „StateTrigger“-Elementen müssen Sie immer sicherstellen, dass „VisualStateGroups“ an das erste untergeordnete Element des Stamms angefügt wird, damit die Auslöser automatisch wirksam werden. (Hier ist **Grid** das erste untergeordnete Element des Stammelements **Page**.)

### Syntax von angefügten Eigenschaften

In einem „VisualState“ wird in der Regel ein Wert für eine Steuerelementeigenschaft oder für eine der angefügten Eigenschaften des Panels festgelegt, das das Steuerelement enthält. Wenn Sie eine angefügte Eigenschaft festlegen, verwenden Sie Klammern um den Namen der angefügten Eigenschaft.

In diesem Beispiel wird veranschaulicht, wie die angefügte [**„RelativePanel.AlignHorizontalCenterWithPanel“-Eigenschaft für ein **](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithpanel.aspx)TextBox`myTextBox` mit dem Namen festgelegt wird. Das erste XAML-Markup verwendet [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.objectanimationusingkeyframes.aspx)-Syntax und das zweite die **Setter**-Syntax.

```xaml
<!-- Set an attached property using ObjectAnimationUsingKeyFrames. -->
<ObjectAnimationUsingKeyFrames
    Storyboard.TargetProperty="(RelativePanel.AlignHorizontalCenterWithPanel)"
    Storyboard.TargetName="myTextBox">
    <DiscreteObjectKeyFrame KeyTime="0" Value="True"/>
</ObjectAnimationUsingKeyFrames>

<!-- Set an attached property using Setter. -->
<Setter Target="myTextBox.(RelativePanel.AlignHorizontalCenterWithPanel)" Value="True"/>
```

### Benutzerdefinierte Zustandsauslöser

Sie können die [**StateTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.statetrigger.aspx)-Klasse erweitern, um benutzerdefinierte Auslöser für eine Vielzahl von Szenarien zu erstellen. Sie können z.B. ein „StateTrigger“-Element erstellen, um die verschiedenen Zustände basierend auf dem Eingabetyp auszulösen, und dann die Ränder um ein Steuerelement herum vergrößern, wenn der Eingabetyp „Toucheingabe“ ist. Alternativ können Sie ein „StateTrigger“-Element erstellen, um unterschiedliche Zustände auf der Grundlage der Gerätefamilie anzuwenden, in der die App ausgeführt wird. Beispiele zum Erstellen von benutzerdefinierten Auslösern und zum Verwenden der Auslöser, um optimale UI-Ergebnisse in einer einzelnen XAML-Ansicht zu erzielen, finden Sie im [Beispiel für Zustandsauslöser](http://go.microsoft.com/fwlink/p/?LinkId=620025).

### Visuelle Zustände und Stile

Sie können Stilressourcen in visuellen Zuständen verwenden, um eine Reihe von Eigenschaftsänderungen auf mehrere Steuerelemente anzuwenden. Weitere Informationen zum Verwenden von Stilen finden Sie unter [Formatieren von Steuerelementen](../controls-and-patterns/styling-controls.md).

In diesem vereinfachten XAML-Markup aus dem Beispiel für Zustandsauslöser wird eine Stilressource auf ein Button-Element angewendet, um die Größe und die Ränder für die Maus- oder Toucheingabe anzupassen. Den vollständigen Code und die Definition des benutzerdefinierten Zustandsauslösers finden Sie im [Beispiel für Zustandsauslöser](http://go.microsoft.com/fwlink/p/?LinkId=620025).

```xaml
<Page ... >
    <Page.Resources>
        <!-- Styles to be used for mouse vs. touch/pen hit targets -->
        <Style x:Key="MouseStyle" TargetType="Rectangle">
            <Setter Property="Margin" Value="5" />
            <Setter Property="Height" Value="20" />
            <Setter Property="Width" Value="20" />
        </Style>
        <Style x:Key="TouchPenStyle" TargetType="Rectangle">
            <Setter Property="Margin" Value="15" />
            <Setter Property="Height" Value="40" />
            <Setter Property="Width" Value="40" />
        </Style>
    </Page.Resources>

    <RelativePanel>
        <!-- ... -->
        <Button Content="Color Palette Button" x:Name="MenuButton">
            <Button.Flyout>
                <Flyout Placement="Bottom">
                    <RelativePanel>
                        <Rectangle Name="BlueRect" Fill="Blue"/>
                        <Rectangle Name="GreenRect" Fill="Green" RelativePanel.RightOf="BlueRect" />
                        <!-- ... -->
                    </RelativePanel>
                </Flyout>
            </Button.Flyout>
        </Button>
        <!-- ... -->
    </RelativePanel>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="InputTypeStates">
            <!-- Second set of VisualStates for building responsive UI optimized for input type.
                 Take a look at InputTypeTrigger.cs class in CustomTriggers folder to see how this is implemented. -->
            <VisualState>
                <VisualState.StateTriggers>
                    <!-- This trigger indicates that this VisualState is to be applied when MenuButton is invoked using a mouse. -->
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Mouse" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="BlueRect.Style" Value="{StaticResource MouseStyle}" />
                    <Setter Target="GreenRect.Style" Value="{StaticResource MouseStyle}" />
                    <!-- ... -->
                </VisualState.Setters>
            </VisualState>
            <VisualState>
                <VisualState.StateTriggers>
                    <!-- Multiple trigger statements can be declared in the following way to imply OR usage.
                         For example, the following statements indicate that this VisualState is to be applied when MenuButton is invoked using Touch OR Pen.-->
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Touch" />
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Pen" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="BlueRect.Style" Value="{StaticResource TouchPenStyle}" />
                    <Setter Target="GreenRect.Style" Value="{StaticResource TouchPenStyle}" />
                    <!-- ... -->
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Page>
```

## Maßgeschneiderte Layouts

Wenn Sie wesentliche Änderungen am Layout der Benutzeroberfläche für verschiedene Geräte vornehmen, empfiehlt es sich unter Umständen, eine separate UI-Datei mit einem maßgeschneiderten Layout für das Gerät zu definieren, anstatt eine einzelne Benutzeroberfläche anpassen. Wenn die Funktionalität auf allen Geräten gleich ist, können Sie separate XAML-Ansichten definieren, die die gleiche Codedatei gemeinsam nutzen. Wenn sich die Ansicht und die Funktionalität auf allen Geräten deutlich unterscheiden, können Sie separate „Page“-Elemente definieren und auswählen, zu welcher „Page“ navigiert werden soll, wenn die App geladen wird.

### Separate XAML-Ansichten pro Gerätefamilie

Verwenden Sie XAML-Ansichten, um unterschiedliche UI-Definitionen zu erstellen, die den gleichen CodeBehind verwenden. Sie können eine eindeutige UI-Definition für jede Gerätefamilie bereitstellen. Führen Sie die folgenden Schritte aus, um eine XAML-Ansicht zu Ihrer App hinzuzufügen.

**So fügen Sie eine XAML-Ansicht zu einer App hinzu**
1. Klicken Sie auf „Projekt“ > „Neues Element hinzufügen“. Das Dialogfeld „Neues Element hinzufügen“ wird geöffnet.
    > **Tipp**&nbsp;&nbsp;Stellen Sie sicher, dass im Projektmappen-Explorer nicht die Projektmappe, sondern ein Ordner oder das Projekt ausgewählt ist.
2. Wählen Sie im linken Bereich unter Visual C# oder Visual Basic den Vorlagentyp XAML aus.
3. Wählen Sie im mittleren Bereich XAML-Ansicht aus.
4. Geben Sie den Namen für die Ansicht ein. Die Ansicht muss korrekt benannt werden. Weitere Informationen zur Benennung finden Sie weiter unten in diesem Abschnitt.
5. Klicken Sie auf „Hinzufügen“. Die Datei wird dem Projekt hinzugefügt.

Mit den vorherigen Schritten wird nur eine XAML-Datei erstellt, aber keine zugehörige CodeBehind-Datei. Stattdessen wird die XAML-Ansicht mithilfe eines „DeviceName“-Qualifizierers, der Teil des Datei- oder Ordnernamens ist, einer vorhandenen „CodeBehind“-Datei zugeordnet. Der Name dieses Qualifizierers kann einem Zeichenfolgenwert zugeordnet werden, der die Gerätefamilie des Geräts darstellt, auf dem die App derzeit ausgeführt wird, z.B. „Desktop“, „Mobil“ und die Namen der anderen Gerätefamilien (siehe [**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues.aspx)).

Sie können den Qualifizierer dem Dateinamen hinzufügen, oder Sie können die Datei einem Ordner hinzufügen, der den Qualifizierernamen aufweist.

**Verwenden des Dateinamens**

Um den Qualifizierernamen mit der Datei zu verwenden, verwenden Sie das folgende Format: *[pageName]*.DeviceFamily-*[qualifierString]*.xaml.

Sehen wir uns nun ein Beispiel für eine Datei mit dem Namen „MainPage.xaml“ an. Um eine Ansicht für mobile Geräte zu erstellen, geben Sie der XAML-Ansicht den Namen „MainPage.DeviceFamily-Mobile.xaml“. Um eine Ansicht für PC-Geräte zu erstellen, geben Sie der Ansicht den Namen „MainPage.DeviceFamily-Desktop.xaml“. Nachfolgend sehen Sie, wie die Projektmappe in Microsoft Visual Studio aussieht.

![XAML-Ansichten mit qualifizierten Dateinamen](images/xaml-layout-view-ex-1.png)

**Verwenden des Ordnernamens**

Um die Ansichten in Ihrem Visual Studio-Projekt mithilfe von Ordnern zu organisieren, können Sie den Qualifizierernamen mit dem Ordner verwenden. Benennen Sie den Ordner hierfür folgendermaßen: DeviceFamily-*[qualifierString]*. In diesem Fall hat jede XAML-Ansichtsdatei den gleichen Namen. Schließen Sie den Qualifizierer nicht in den Dateinamen ein.

Hier sehen Sie ein Beispiel, bei dem die Datei wieder den Namen „MainPage.xaml“ trägt. Um eine Ansicht für mobile Geräte zu erstellen, erstellen Sie einen Ordner mit dem Namen „DeviceFamily-Mobile“, und platzieren Sie eine XAML-Ansicht namens „MainPage.xaml“ im Ordner. Um eine Ansicht für PC-Geräte zu erstellen, erstellen Sie einen Ordner mit dem Namen „DeviceFamily-Desktop“, und platzieren Sie eine weitere XAML-Ansicht namens „MainPage.xaml“ darin. Nachfolgend sehen Sie, wie die Projektmappe in Visual Studio aussieht.

![XAML-Ansichten in Ordnern](images/xaml-layout-view-ex-2.png)

In beiden Fällen wird eine eindeutige Ansicht für mobile und PC-Geräte verwendet. Die standardmäßige „MainPage.xaml“-Datei wird verwendet, wenn das Gerät, auf dem sie ausgeführt wird, nicht mit einer der speziellen Ansichten für Gerätefamilien übereinstimmt.

### Separate XAML-Seiten pro Gerätefamilie

Um eindeutige Ansichten und Funktionen bereitzustellen, können Sie separate „Page“-Dateien (XAML und Code) erstellen und dann zu der entsprechenden Seite navigieren, wenn die Seite benötigt wird.

**So fügen Sie einer App eine XAML-Seite hinzu**
1. Klicken Sie auf „Projekt“ > „Neues Element hinzufügen“. Das Dialogfeld „Neues Element hinzufügen“ wird geöffnet.
    > **Tipp**&nbsp;&nbsp;Stellen Sie sicher, dass im Projektmappen-Explorer nicht die Projektmappe, sondern das Projekt ausgewählt ist.
2. Wählen Sie im linken Bereich unter Visual C# oder Visual Basic den Vorlagentyp XAML aus.
3. Wählen Sie im mittleren Bereich „Leere Seite“ aus.
4. Geben Sie den Namen für die Seite ein. Beispielsweise „MainPage_Mobile“. Es wird sowohl eine „“MainPage_Mobile.xaml“- als auch eine „MainPage_Mobile.xaml.cs/vb/cpp“-Codedatei erstellt.
5. Klicken Sie auf „Hinzufügen“. Die Datei wird dem Projekt hinzugefügt.

Überprüfen Sie zur Laufzeit die Gerätefamilie, auf der die App ausgeführt wird, und navigieren Sie zu der korrekten Seite.

```csharp
if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
{
    rootFrame.Navigate(typeof(MainPage_Mobile), e.Arguments);
}
else
{
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
}
```

Sie können auch unterschiedliche Kriterien verwenden, um zu bestimmen, zur welcher Seite navigiert werden soll. Weitere Beispiele finden Sie im Beispiel [Mehrere Ansichten für maßgeschneiderte Inhalte](http://go.microsoft.com/fwlink/p/?LinkId=620636), in dem die [**GetIntegratedDisplaySize**](https://msdn.microsoft.com/library/windows/apps/xaml/dn904185.aspx)-Funktion verwendet wird, um die physische Größe einer integrierten Anzeige zu überprüfen.



<!--HONumber=Aug16_HO3-->


