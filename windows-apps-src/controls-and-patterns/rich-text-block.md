---
author: Jwmsft
Description: Verwenden Sie ein RichTextBlock-Element mit RichTextBlockOverflow-Elementen, um erweiterte Textlayouts zu erstellen.
title: RichTextBlock
ms.assetid: E4BE4B1B-418E-4075-88F1-22C09DDF8E45
label: Rich text block
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 28c78b39bad4c66457ec5aba8cf0b4ce0de4f00a

---
# Rich-Text-Block
Rich-Text-Blöcke bieten verschiedene Features für erweitertes Textlayout, die Sie verwenden können, wenn Sie Unterstützung für Absätze, Inline-UI-Elemente oder komplexe Textlayouts benötigen.


<span class="sidebar_heading" style="font-weight: bold;">Wichtige APIs</span>

-   [**RichTextBlock-Klasse**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx)
-   [**RichTextBlockOverflow-Klasse**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx)
-   [**Paragraph-Klasse**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx)
-   [**Typography-Klasse**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx)

## Ist dies das richtige Steuerelement?

Verwenden Sie **RichTextBlock**, wenn Sie Unterstützung für mehrere Absätze, mehrspaltige oder andere komplexe Textlayouts oder Inline-UI-Elemente wie Bilder benötigen.

Mit **TextBlock** können Sie die meisten schreibgeschützten Texte in Ihrer App anzeigen. Sie können es zum Anzeigen von einzeiligem oder mehrzeiligem Text, Inlinelinks und Text mit Formatierung, z. B. fett, kursiv oder unterstrichen, verwenden. TextBlock stellt ein einfacheres Inhaltsmodell bereit. Daher ist er in der Regel einfacher zu verwenden und bietet eine bessere Leistung beim Rendern von Text als RichTextBlock. Er wird für den meisten UI-Text in Apps bevorzugt. Sie können zwar Zeilenumbrüche in den Text einfügen, jedoch ist TextBlock zum Anzeigen eines einzelnen Absatzes vorgesehen und unterstützt keinen Texteinzug.

Weitere Informationen zur Auswahl des passenden Textsteuerelements finden Sie im Artikel über [Textsteuerelemente](text-controls.md).

## Beispiele


## Erstellen eines Rich-Text-Blocks

Die Inhaltseigenschaft von RichTextBlock ist die [Blocks](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.blocks.aspx)-Eigenschaft, die mit dem [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx)-Element absatzbasierten Text unterstützt. Es gibt keine **Text**-Eigenschaft, die Sie für einen bequemen Zugriff auf den Textinhalt des Steuerelements in Ihrer App verwenden können. RichTextBlock bietet jedoch verschiedene einzigartige Features, die TextBlock nicht bereitstellt. 

Von RichTextBlock unterstützte Features:
- Mehrere Absätze. Legen Sie den Einzug für Absätze mit der [TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.textindent.aspx)-Eigenschaft fest.
- Inline-UI-Elemente. Verwenden Sie einen [InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.inlineuicontainer.aspx), um UI-Elemente, z. B. Bilder, inline im Text anzuzeigen.
- Überlaufcontainer. Verwenden Sie [RichTextBlockOverflow](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx)-Elemente, um mehrspaltige Textlayouts zu erstellen.

### Absätze

Sie definieren mit [**Paragraph**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx)-Elementen die Textblöcke, die in einem RichTextBlock Steuerelement angezeigt werden sollen. Jeder RichTextBlock sollte mindestens einen Paragraph enthalten. 

Mit der [RichTextBlock.TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.textindent.aspx)-Eigenschaft können Sie die Größe des Einzugs für alle Absätze in einem RichTextBlock festlegen. Sie können diese Einstellung für bestimmte Absätze in einem RichTextBlock überschreiben, indem Sie die [Paragraph.TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.textindent.aspx)-Eigenschaft auf einen anderen Wert festlegen.

```xaml
<RichTextBlock TextIndent="12">
  <Paragraph TextIndent="24">First paragraph.</Paragraph>
  <Paragraph>Second paragraph.</Paragraph>
  <Paragraph>Third paragraph. <Bold>With an inline.</Bold></Paragraph>
</RichTextBlock>
```

### Inline-UI-Elemente

Mit der [**InlineUIContainer**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.inlineuicontainer.aspx)-Klasse können Sie jedes UIElement inline im Text einbetten. Ein gängiges Szenario ist das Einfügen eines Inline-Elements vom Typ „Image“ in den Text. Sie können aber natürlich auch interaktive Elemente wie „Button“ oder „CheckBox“ verwenden.

Wenn Sie mehrere Elemente inline an der gleichen Position einbetten möchten, können Sie ein Panel als einzelnes untergeordnetes InlineUIContainer-Element verwenden und dann mehrere Elemente in diesem Panel platzieren.

In diesem Beispiel wird veranschaulicht, wie mithilfe eines InlineUIContainer ein Bild in einen RichTextBlock eingefügt wird. 

```xaml
<RichTextBlock>
    <Paragraph>
        <Italic>This is an inline image.</Italic>
        <InlineUIContainer>
            <Image Source="Assets/Square44x44Logo.png" Height="30" Width="30"/>
        </InlineUIContainer>
        Mauris auctor tincidunt auctor.
    </Paragraph>
</RichTextBlock>
```

## Überlaufcontainer

Sie können einen RichTextBlock mit [**RichTextBlockOverflow**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx)-Elementen verwenden, um mehrspaltige Seitenlayouts oder andere erweiterte Seitenlayouts zu erstellen. Der Inhalt für ein RichTextBlockOverflow-Element stammt immer aus einem RichTextBlock-Element. Sie verknüpfen RichTextBlockOverflow-Elemente, indem Sie sie als OverflowContentTarget eines RichTextBlock-Elements oder eines anderen RichTextBlockOverflow-Elements festlegen.

Hier ist ein einfaches Beispiel, in dem ein zweispaltiges Layout erstellt wird. Ein komplexeres Beispiel finden Sie im Abschnitt „Beispiele“.

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <RichTextBlock Grid.Column="0" 
                   OverflowContentTarget="{Binding ElementName=overflowContainer}" >
        <Paragraph>
            Proin ac metus at quam luctus ultricies.
        </Paragraph>
    </RichTextBlock>
    <RichTextBlockOverflow x:Name="overflowContainer" Grid.Column="1"/>
</Grid>
```

## Formatieren von Text

Obwohl der RichTextBlock Nur-Text speichert, können Sie verschiedene Formatierungsoptionen anwenden, um das Rendern des Texts in der App anzupassen. Sie können Standard-Steuerelementeigenschaften, z. B. FontFamily, FontSize, FontStyle, Foreground und CharacterSpacing, festlegen, um das Erscheinungsbild des Texts zu ändern. Sie können den Text auch mit Inlinetextelementen und angefügten Typography-Eigenschaften formatieren. Diese Optionen beeinflussen nur die lokale Anzeige des Texts im RichTextBlock. Wenn Sie den Text kopieren und z. B. in ein Rich-Text-Steuerelement einfügen, wird daher keine Formatierung angewendet.

### Inline-Elemente

Der [Windows.UI.Xaml.Documents](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.aspx)-Namespace bietet eine Vielzahl von Inlinetextelementen, mit denen Sie Text formatieren können, z. B. Bold, Italic, Run, Span und LineBreak. Ein typisches Verfahren zum Formatieren von Textabschnitten ist das Einfügen des Texts in ein Run-Element oder Span-Element und das anschließende Festlegen der Eigenschaften für das Element.

Dies ist ein Paragraph, in dem der erste Ausdruck als fett formatierter blauer Text mit dem Schriftgrad 16 pt angezeigt wird.

```xaml
<Paragraph>
    <Bold><Span Foreground="DarkSlateBlue" FontSize="16">Lorem ipsum dolor sit amet</Span></Bold>
    , consectetur adipiscing elit.
</Paragraph>
```

### Typografie

Die angefügten Eigenschaften der [Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx)-Klasse ermöglichen den Zugriff auf eine Reihe von Microsoft OpenType-Typografieeigenschaften. Sie können diese angefügten Eigenschaften entweder für den RichTextBlock oder für einzelne Inlinetextelemente festlegen, wie hier gezeigt.

```xaml
<RichTextBlock Typography.StylisticSet4="True">
    <Paragraph>
        <Span Typography.Capitals="SmallCaps">Lorem ipsum dolor sit amet</Span>
        , consectetur adipiscing elit.
    </Paragraph>
</RichTextBlock>
```

## Empfehlungen

Siehe „Typografie“ und „Richtlinien für Schriftarten“.



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



<!--HONumber=Jun16_HO4-->


