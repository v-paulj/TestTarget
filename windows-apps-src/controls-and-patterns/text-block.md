---
author: Jwmsft
ms.assetid: DA562509-D893-425A-AAE6-B2AE9E9F8A19
label: Text block
template: detail.hbs
---
# Textblock
 Der Textblock ist das wichtigste Steuerelement zum Anzeigen von schreibgeschütztem Text in Apps. Sie können es zum Anzeigen von einzeiligem oder mehrzeiligem Text, Inlinelinks und Text mit Formatierung, z. B. fett, kursiv oder unterstrichen, verwenden.

<span class="sidebar_heading" style="font-weight: bold;">Wichtige APIs</span>

-   [**TextBlock-Klasse**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx)
-   [**Text-Eigenschaft**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx)
-   [**Inlines-Eigenschaft**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.inlines.aspx)

## Ist dies das richtige Steuerelement?

Ein Textblock ist in der Regel einfacher zu verwenden und bietet eine bessere Leistung beim Rendern von Text als ein Rich-Text-Block. Daher wird er in der Regel für App-UI-Text bevorzugt. Sie können über einen Textblock in Ihrer App ganz einfach auf den Text zugreifen und ihn verwenden, indem Sie den Wert der [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx)-Eigenschaft abrufen. Er enthält außerdem viele der gleichen Formatierungsoptionen zum Anpassen des Renderns von Text. 

Sie können zwar Zeilenumbrüche in den Text einfügen, jedoch ist der Textblock zum Anzeigen eines einzelnen Absatzes vorgesehen und unterstützt keinen Texteinzug. Verwenden Sie **RichTextBlock**, wenn Sie Unterstützung für mehrere Absätze, mehrspaltigen Text, andere komplexe Textlayouts oder Inline-UI-Elemente, z. B. Bilder, benötigen.

Weitere Informationen zur Auswahl des passenden Textsteuerelements finden Sie im Artikel [Textsteuerelemente](text-controls.md).

## Beispiele


## Erstellen eines Textblocks

Im Folgenden wird veranschaulicht, wie Sie ein einfaches TextBlock-Steuerelement definieren und seine Text-Eigenschaft auf eine Zeichenfolge festlegen.

```xaml
<TextBlock Text="Hello, world!" />
```

```csharp
TextBlock textBlock1 = new TextBlock();
textBlock1.Text = "Hello, world!";
```

    <TextBlock Text="Hello, world!" />

    TextBlock textBlock1 = new TextBlock();
    textBlock1.Text = "Hello, world!";

### Inhaltsmodell

Es gibt zwei Eigenschaften, die Sie verwenden können, um einem TextBlock Inhalt hinzuzufügen: [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx) und [Inlines](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.inlines.aspx).

Das häufigste Verfahren zum Anzeigen von Text ist das Festlegen der Text-Eigenschaft auf einen Zeichenfolgenwert, wie im vorherigen Beispiel gezeigt.

Sie können auch Inhalte hinzufügen, indem Sie wie hier gezeigt Elemente mit fließendem Inlineinhalt in die TextBox.Inlines-Eigenschaft einfügen.
```xaml
<TextBlock><Run>Text can be <Bold>bold</Bold>, <Italic>italic</Italic>, or <Bold><Italic>both</Italic></Bold>.</Run></TextBlock>
```

Von der Inline-Klasse abgeleitete Elemente, z. B. Bold, Italic, Run, Span und LineBreak, ermöglichen unterschiedliche Formatierungen für unterschiedliche Teile des Texts. Weitere Informationen finden Sie im Abschnitt [Formatieren von Text](). Mit dem Inline-Hyperlink-Element können Sie dem Text einen Link hinzufügen. Durch die Verwendung von Inlines wird jedoch auch das Rendern von Text im schnellen Pfad deaktiviert, wie im nächsten Abschnitt erläutert.


## Leistungsaspekte

XAML verwendet, wenn möglich, einen effizienteren Codepfad für Layouttext. Dieser schnelle Pfad verringert die gesamte Arbeitsspeicherauslastung und reduziert erheblich die CPU-Zeit für die Abmessung und Anordnung von Text. Dieser schnelle Pfad gilt nur für TextBlock und sollte deshalb nach Möglichkeit RichtTextBlock gegenüber bevorzugt werden.

Bestimmte Bedingungen erfordern TextBlock, um auf einen prozessorintensiven Codepfad mit zahlreichen Funktionen zum Rendern von Text zurückzugreifen. Damit das Rendern von Text im schnellen Pfad weiterhin ausgeführt wird, beachten Sie beim Festlegen der Eigenschaften unbedingt die im Folgenden aufgeführten Richtlinien.
- [
              **Text**
            ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx): Die wichtigste Bedingung ist, dass der schnelle Pfad nur verwendet wird, wenn Sie Text durch explizites Definieren der Text-Eigenschaft festlegen, entweder in XAML oder im Code (wie in den vorherigen Beispielen dargestellt). Beim Festlegen des Textes über die Inlines-Sammlung von TextBlock (z. B. `<TextBlock>Inline text</TextBlock>` wird der schnelle Pfad aufgrund der potenziellen Komplexität mehrerer Formate deaktiviert.
- [
              **CharacterSpacing**
            ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.characterspacing.aspx): Nur der Standardwert 0 ist ein schneller Pfad.
- [
              **Typography**
            ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx): Nur die Standardwerte für die verschiedenen Typography -Eigenschaften sind schnelle Pfade.
- [
              **TextTrimming**
            ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.texttrimming.aspx): Nur die Werte **None**, **CharacterEllipsis** und **WordEllipsis** sind schnelle Pfade. Der **Clip**-Wert deaktiviert den schnellen Pfad.
- [
              **LineStackingStrategy**
            ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.linestackingstrategy.aspx): Wenn [LineHeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.lineheight.aspx) nicht 0 ist, deaktivieren die **BaselineToBaseline**- und **MaxHeight**-Werte den schnellen Pfad.
- [
              **IsTextSelectionEnabled**
            ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.istextselectionenabled.aspx): Nur **false** ist ein schneller Pfad. Wenn diese Eigenschaft auf **true** festgelegt wird, wird der schnelle Pfad deaktiviert.

Sie können die [DebugSettings.IsTextPerformanceVisualizationEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.debugsettings.istextperformancevisualizationenabled.aspx)-Eigenschaft während des Debuggens auf **true** festlegen, um festzustellen, ob das Rendern von Text im schnellen Pfad erfolgt. Wenn diese Eigenschaft auf „true“ festgelegt ist, wird der Text im schnellen Pfad in Hellgrün angezeigt. 

>**Tipp**
            &nbsp;&nbsp;Dieses Feature wird in der folgenden Sitzung aus Build 2015 ausführlich erläutert: [XAML-Leistung – Techniken zum Optimieren der Umgebung einer Universellen Windows-App, die mit XAML erstellt wurde](https://channel9.msdn.com/Events/Build/2015/3-698).

 

Debugeinstellungen legen Sie in der Regel wie folgt in der [OnLaunched](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.application.onlaunched.aspx)-Methodenüberschreibung in der CodeBehind-Seite für „App.xaml” fest.
```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
#if DEBUG
    if (System.Diagnostics.Debugger.IsAttached)
    {
        this.DebugSettings.IsTextPerformanceVisualizationEnabled = true;
    }
#endif

// ...

}
```

In diesem Beispiel wird die erste TextBlock-Klasse unter Verwendung des schnellen Pfads gerendert, während dies bei der zweiten nicht der Fall ist.
```xaml
<StackPanel>
    <TextBlock Text="This text is on the fast path."/>
    <TextBlock>This text is NOT on the fast path.</TextBlock>
<StackPanel/>
```

Wenn Sie diesen XAML-Code im Debugmodus mit der auf „true“ festgelegten IsTextPerformanceVisualizationEnabled-Eigenschaft ausführen, sieht das Ergebnis wie folgt aus.

![Im Debugmodus gerenderter Text](images/text-block-rendering-performance.png)

>**Achtung**
            &nbsp;&nbsp;Die Farbe von Text, der nicht im schnellen Pfad enthalten ist, wird nicht geändert. Wenn die Farbe von Text in der App als Hellgrün angegeben ist, wird der Text auch noch in Hellgrün angezeigt, wenn er sich im langsameren Renderingpfad befindet. Verwechseln Sie Text, der in der App auf Grün festgelegt ist, nicht mit Text im schnellen Pfad, der aufgrund der Debugeinstellungen grün ist.

## Formatieren von Text

Obwohl die Text-Eigenschaft Nur-Text speichert, können Sie verschiedene Formatierungsoptionen auf das TextBlock-Steuerelement anwenden, um das Rendern des Texts in der App anzupassen. Sie können Standard-Steuerelementeigenschaften, z. B. FontFamily, FontSize, FontStyle, Foreground und CharacterSpacing festlegen, um das Erscheinungsbild des Texts zu ändern. Sie können den Text auch mit Inlinetextelementen und angefügten Typografie-Eigenschaften formatieren. Diese Optionen beeinflussen nur die lokale Anzeige des Texts im TextBlock. Wenn Sie den Text kopieren und z. B. in ein Rich-Text-Steuerelement einfügen, wird daher keine Formatierung angewendet.

>**Hinweis**
            &nbsp;&nbsp;Beachten Sie, dass Inlinetextelemente und nicht standardmäßige Typografiewerte nicht im schnellen Pfad gerendert werden (dies wurde im vorherigen Abschnitt erläutert).
 

### Inline-Elemente

Der [Windows.UI.Xaml.Documents](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.aspx)-Namespace bietet eine Vielzahl von Inlinetextelementen, mit denen Sie Text formatieren können, z. B. Bold, Italic, Run, Span und LineBreak. 

Sie können in einem TextBlock auch mehrere Zeichenfolgen anzeigen und jede Zeichenfolge anders formatieren. Dazu verwenden Sie ein Run-Element für die Anzeige der einzelnen Zeichenfolgen mit der zugehörigen Formatierung. Trennen Sie die einzelnen Run-Elemente dann mit einem LineBreak-Element.

So definieren Sie unterschiedlich formatierte Textzeichenfolgen in einem TextBlock mit Run-Objekten, die durch einen LineBreak voneinander getrennt sind.
```xaml
<TextBlock FontFamily="Arial" Width="400" Text="Sample text formatting runs">
    <LineBreak/>
    <Run Foreground="Gray" FontFamily="Courier New" FontSize="24"> 
        Courier New 24 
    </Run>
    <LineBreak/>
    <Run Foreground="Teal" FontFamily="Times New Roman" FontSize="18" FontStyle="Italic"> 
        Times New Roman Italic 18 
    </Run>
    <LineBreak/>
    <Run Foreground="SteelBlue" FontFamily="Verdana" FontSize="14" FontWeight="Bold"> 
        Verdana Bold 14 
    </Run>
</TextBlock>
```

Dies ist das Ergebnis.

![Mit Run-Elementen formatierter Text](images/text-block-run-examples.png)

### Typografie

Die angefügten Eigenschaften der [Typografie](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx)-Klasse ermöglichen den Zugriff auf eine Reihe von Microsoft OpenType-Typografieeigenschaften. Sie können diese angefügten Eigenschaften entweder für das TextBlock-Element oder für einzelne Inlinetextelemente festlegen. In diesen Beispielen wird beides gezeigt.
```xaml
<TextBlock Text="Hello, world!"
           Typography.Capitals="SmallCaps"
           Typography.StylisticSet4="True"/>
```

```csharp
TextBlock textBlock1 = new TextBlock();
textBlock1.Text = "Hello, world!";
Windows.UI.Xaml.Documents.Typography.SetCapitals(textBlock1, FontCapitals.SmallCaps);
Windows.UI.Xaml.Documents.Typography.SetStylisticSet4(textBlock1, true);
```

```xaml
<TextBlock>12 x <Run Typography.Fraction="Slashed">1/3</Run> = 4.</TextBlock>
```


## Verwandte Artikel

[Textsteuerelemente](text-controls.md)

**Für Designer**
- [Richtlinien für die Rechtschreibprüfung](spell-checking-and-prediction.md)
- [Hinzufügen von Suchfunktionen](https://msdn.microsoft.com/library/windows/apps/hh465231)
- [Richtlinien für die Texteingabe](text-controls.md)

**Für Entwickler (XAML)**
- [**TextBox-Klasse**](https://msdn.microsoft.com/library/windows/apps/br209683)
- [**Windows.UI.Xaml.Controls PasswordBox-Klasse**](https://msdn.microsoft.com/library/windows/apps/br227519)


**Für Entwickler (Sonstige)**
- [StringLength-Eigenschaft](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)


<!--HONumber=May16_HO2-->


