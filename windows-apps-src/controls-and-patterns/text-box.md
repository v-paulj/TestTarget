---
ms.assetid: CC1BF51D-3DAC-4198-ADCB-1770B901C2FC
label: Textfeld
template: detail.hbs
---
# Textfeld
Mit dem TextBox-Steuerelement können Benutzer Text in eine App eingeben. Es wird üblicherweise verwendet, um eine einzelne Textzeile einzugeben, kann jedoch auch so konfiguriert werden, dass eine mehrzeilige Texteingabe möglich ist. Der Text wird auf dem Bildschirm in einem einfachen, einheitlichen Klartext angezeigt.

TextBox bietet eine Reihe von Features, mit denen die Texteingabe vereinfacht werden kann. Es enthält ein bekanntes integriertes Kontextmenü, das das Kopieren und Einfügen von Text unterstützt. Über die Schaltfläche „Alles löschen” können Benutzer den gesamten eingegebenen Text schnell löschen. Es bietet außerdem eine integrierte Rechtschreibprüfung, die standardmäßig aktiviert ist.

<span class="sidebar_heading" style="font-weight: bold;">Wichtige APIs</span>

-   [**TextBox-Klasse**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx)
-   [**Text-Eigenschaft**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.text.aspx)


## Ist dies das richtige Steuerelement?

Ein **TextBox**-Steuerelement ermöglicht es Benutzern, unformatierten Text einzugeben und zu bearbeiten, z. B. in einem Formular. Mit der [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.text.aspx)-Eigenschaft können Sie den Text in einem TextBox-Steuerelement abrufen und festlegen.

Sie können das TextBox-Element als schreibgeschützt festlegen, dies sollte aber nur ein temporärer, bedingter Zustand sein. Wenn der Text nie bearbeitbar sein soll, ziehen Sie stattdessen die Verwendung eines [**TextBlock**](text-block.md)-Elements in Erwägung.

Verwenden Sie ein [**PasswordBox**](password-box.md)-Steuerelement, um Kennwörter oder andere private Daten wie Sozialversicherungsnummern zu erfassen. Ein Kennwortfeld sieht wie ein Texteingabefeld aus. Die einzige Ausnahme besteht darin, dass anstelle des eingegebenen Texts Aufzählungszeichen erscheinen.

Verwenden Sie ein [**AutoSuggestBox**](auto-suggest-box.md)-Steuerelement, um Benutzern die Eingabe von Suchbegriffen zu ermöglichen oder eine Liste mit Vorschlägen bereitzustellen, aus der Benutzer während der Eingabe auswählen können.

Verwenden Sie ein [**RichEditBox**](rich-edit-box.md)-Element, um RTF-Dateien anzuzeigen und zu bearbeiten.

Weitere Informationen zur Auswahl des passenden Textsteuerelements finden Sie im Artikel [Textsteuerelemente](text-controls.md).

## Beispiele

![Ein Textfeld](images/text-box.png)

## Erstellen eines Textfelds

Dies ist der XAML-Code für ein einfaches Textfeld mit einer Überschrift und Platzhaltertext.

```xaml
<TextBox Width="500" Header="Notes" PlaceholderText="Type your notes here"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.Width = 500;
textBox.Header = "Notes";
textBox.PlaceholderText = "Type your notes here";
// Add the TextBox to the visual tree.
rootGrid.Children.Add(textBox);
```

Dieser XAML-Code führt zu folgendem Textfeld.

![Ein einfaches Textfeld](images/text-box-ex1.png)

### Verwenden eines Textfelds für die Dateneingabe in ein Formular

Üblicherweise wird ein Textfeld verwendet, um die Dateneingabe in ein Formular zu ermöglichen, wobei die [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.text.aspx)-Eigenschaft dazu dient, die vollständige Textzeichenfolge aus dem Textfeld abzurufen. Zum Zugriff auf die Text-Eigenschaft nutzen Sie in der Regel ein Ereignis wie etwa eine Schaltfläche, Sie können jedoch auch ein [TextChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.textchanged.aspx)- oder [TextChanging](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.textchanging.aspx)-Ereignis verwenden, wenn bei einer Textänderung eine Aktion ausgeführt werden muss. 

Sie können dem Textfeld eine [Header](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.header.aspx)-Eigenschaft (oder eine Beschriftung) und eine [PlaceholderText](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.placeholdertext.aspx)-Eigenschaft (oder ein Wasserzeichen) hinzufügen, um Benutzern einen Hinweis bezüglich des Verwendungszwecks zu geben. Um das Erscheinungsbild der Überschrift anzupassen, können Sie anstelle der Header-Eigenschaft die [HeaderTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.headertemplate.aspx)-Eigenschaft festlegen. *Informationen zum Design finden Sie unter „Richtlinien für Beschriftungen”*.

Sie können die Anzahl der Zeichen, die der Benutzer eingeben darf, beschränken, indem Sie die [MaxLength](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.maxlength.aspx)-Eigenschaft festlegen. MaxLength beschränkt jedoch nicht die Länge des eingefügten Texts. Verwenden Sie das [Paste](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.paste.aspx)-Ereignis, um eingefügten Text zu bearbeiten, wenn dies für Ihre App von Bedeutung ist.

Das Textfeld besitzt eine „Alles löschen”-Schaltfläche („X”), die angezeigt wird, sobald Text in das Feld eingegeben wird. Wenn ein Benutzer auf das „X” klickt, wird der Text im Textfeld gelöscht. Das sieht ungefähr wie folgt aus:

![Ein Textfeld mit einer „Alles löschen”-Schaltfläche](images/text-box-clear-all.png)

Die „Alles löschen”-Schaltfläche wird nur für bearbeitbare, einzeilige Textfelder angezeigt, die Text enthalten und den Fokus besitzen. 

In den folgenden Fällen wird die „Alles löschen”-Schaltfläche nicht angezeigt:
- **IsReadOnly** ist **true**.
- **AcceptsReturn** ist **true**.
- **TextWrap** hat einen anderen Wert als **NoWrap**.

### Festlegen eines Textfelds auf schreibgeschützt

Sie können ein Textfeld auf schreibgeschützt festlegen, indem Sie die [IsReadOnly](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.isreadonly.aspx)-Eigenschaft auf **true** festlegen. Diese Eigenschaft werden Sie in Ihrem App-Code üblicherweise basierend auf den Bedingungen in Ihrer App umschalten. Wenn Text immer schreibgeschützt sein soll, ziehen Sie stattdessen die Verwendung eines TextBlock-Elements in Erwägung.

Sie können ein TextBox-Element auf schreibgeschützt festlegen, indem Sie die IsReadOnly-Eigenschaft auf true festlegen. Sie können z. B. ein Textfeld bereitstellen, in das Benutzer Kommentare eingeben können und das nur unter bestimmten Bedingungen aktiviert wird. Sie können das Textfeld als schreibgeschützt festlegen, bis die Bedingungen erfüllt sind. Wenn Text nur angezeigt werden soll, ziehen Sie stattdessen die Verwendung eines TextBlock- oder RichtTextBlock-Elements in Erwägung.

Ein schreibgeschütztes Textfeld sieht genauso aus wie ein Textfeld zum Lesen/Schreiben, was auf Benutzer verwirrend wirken könnte. 
Ein Benutzer kann Text auswählen und kopieren.
IsEnabled


### Aktivieren der mehrzeiligen Texteingabe

Es gibt zwei Eigenschaften, mit denen Sie steuern können, ob das TextBox-Element Text auf mehr als einer Zeile anzeigt. Sie legen üblicherweise beide Eigenschaften fest, um ein mehrzeiliges Textfeld zu erzeugen.
- Damit das Textfeld Zeilenwechsel- oder Zeilenumbruchzeichen zulässt und anzeigt, legen Sie die [**AcceptsReturn**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.acceptsreturn.aspx)-Eigenschaft auf **true** fest.
- Um Textumbrüche zu ermöglichen, legen Sie die [**TextWrapping**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.textwrapping.aspx)-Eigenschaft auf **Wrap** fest. Dies bewirkt, dass der Text umbrochen wird, sobald der Rand des Textfelds erreicht ist – unabhängig von Zeilentrennzeichen.

> **Hinweis**&nbsp;&nbsp;TextBox und RichEditBox unterstützen den **WrapWholeWords**-Wert für ihre TextWrapping-Eigenschaften nicht. Wenn Sie versuchen, WrapWholeWords als Wert für TextBox.TextWrapping oder RichEditBox.TextWrapping zu verwenden, wird eine Ausnahme für ein ungültiges Argument ausgelöst.

Ein mehrzeiliges TextBox vergrößert sich während der Texteingabe weiterhin vertikal, sofern es nicht durch seine [Height](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx)- oder [MaxHeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.maxheight.aspx)-Eigenschaft oder durch einen übergeordneten Container begrenzt wird. Sie sollten testen, ob sich ein mehrzeiliges Textfeld über seinen sichtbaren Bereich hinaus vergrößert, und gegebenenfalls seine Höhe begrenzen. Es wird empfohlen, für ein mehrzeiliges Textfeld immer eine angemessene Höhe festzulegen, die sich während der Texteingabe durch den Benutzer nicht verändert. 

Der Bildlauf mit einem Mausrad oder per Toucheingabe wird bei Bedarf automatisch aktiviert. Die vertikalen Bildlaufleisten werden jedoch nicht standardmäßig angezeigt. Sie können die vertikalen Bildlaufleisten anzeigen, indem Sie [ScrollViewer.VerticalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility.aspx) im eingebetteten ScrollViewer wie hier dargestellt auf **Auto** festlegen. 

```xaml
<TextBox AcceptsReturn="True" TextWrapping="Wrap" 
         MaxHeight="172" Width="300" Header="Description"
         ScrollViewer.VerticalScrollBarVisibility="Auto"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.AcceptsReturn = true;
textBox.TextWrapping = TextWrapping.Wrap;
textBox.MaxHeight = 172;
textBox.Width = 300;
textBox.Header = "Description";
ScrollViewer.SetVerticalScrollBarVisibility(textBox, ScrollBarVisibility.Auto);
```

Das Textfeld sieht nach dem Hinzufügen von Text folgendermaßen aus.

![Ein mehrzeiliges Textfeld](images/text-box-multi-line.png)

### Formatieren der Textanzeige

Verwenden Sie die [TextAlignment]()-Eigenschaft, um Text innerhalb eines Textfelds auszurichten. Verwenden Sie zum Ausrichten des Textfelds innerhalb des Seitenlayouts die Eigenschaften [HorizontalAlignment](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.horizontalalignment.aspx) und [VerticalAlignment](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.verticalalignment.aspx).

Obwohl die TextBox-Eigenschaft nur unformatierten Text unterstützt, können zur Anpassung an das Branding Ihrer App festlegen, wie der Text im Textfeld angezeigt wird. Sie können Standardeigenschaften von [Steuerelementen](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.aspx), z. B. [FontFamily](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.fontfamily.aspx), [FontSize](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.fontsize.aspx), [FontStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.fontstyle.aspx), [Background](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.background.aspx), [Foreground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.foreground.aspx) und [CharacterSpacing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.characterspacing.aspx), festlegen, um das Erscheinungsbild des Texts zu ändern. Diese Eigenschaften beeinflussen nur die lokale Anzeige des Texts im Textfeld. Wenn Sie den Text kopieren und z. B. in ein Rich-Text-Steuerelement einfügen, wird daher keine Formatierung angewendet.

In diesem Beispiel wird ein schreibgeschütztes TextBox-Element mit mehreren festgelegten Eigenschaften zur Anpassung des Erscheinungsbilds des Textes dargestellt.

```xaml
<TextBox Text="Sample Text" IsReadOnly="True" 
         FontFamily="Verdana" FontSize="24"
         FontWeight="Bold" FontStyle="Italic" 
         CharacterSpacing="200" Width="300"
         Foreground="Blue" Background="Beige"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.Text = "Sample Text";
textBox.IsReadOnly = true;
textBox.FontFamily = new FontFamily("Verdana");
textBox.FontSize = 24;
textBox.FontWeight = Windows.UI.Text.FontWeights.Bold;
textBox.FontStyle = Windows.UI.Text.FontStyle.Italic;
textBox.CharacterSpacing = 200;
textBox.Width = 300;
textBox.Background = new SolidColorBrush(Windows.UI.Colors.Beige);
textBox.Foreground = new SolidColorBrush(Windows.UI.Colors.Blue);
// Add the TextBox to the visual tree.
rootGrid.Children.Add(textBox);
```

Das resultierende Textfeld sieht wie folgt aus.

![Ein formatiertes Textfeld](images/text-box-formatted.png)

### Ändern des Kontextmenüs

Standardmäßig hängen die im Kontextmenü des Textfelds angezeigten Befehle vom Zustand des Textfelds ab. Die folgenden Befehle können zum Beispiel angezeigt werden, wenn das Textfeld bearbeitbar ist.

Befehl | Angezeigt, wenn ...
------- | -------------
Kopieren | Text ausgewählt ist. 
Ausschneiden | Text ausgewählt ist. 
Einfügen | die Zwischenablage Text enthält. 
Alle auswählen | das Textfeld Text enthält. 
Rückgängig machen | Text geändert wurde. 

Um die im Kontextmenü angezeigten Befehle zu ändern, müssen Sie das [ContextMenuOpening](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.contextmenuopening.aspx)-Ereignis behandeln. Ein Beispiel hierfür finden Sie im Szenario 2 in [Kontextmenübeispiel](http://go.microsoft.com/fwlink/p/?linkid=234891). Informationen zum Design finden Sie in den Richtlinien für Kontextmenüs.

### Auswählen, Kopieren und Einfügen

Sie können markierten Text in einem TextBox-Element [SelectedText](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectedtext.aspx)-Eigenschaft abrufen und festlegen. Verwenden Sie die Eigenschaften [SelectionStart](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionstart.aspx) und [SelectionLength](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionlength.aspx) sowie die Methoden [Select](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.select.aspx) und [SelectAll](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectall.aspx), um die Textauswahl zu bearbeiten. Behandeln Sie das [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionchanged.aspx)-Ereignis, um eine Aktion auszuführen, wenn der Benutzer Text markiert oder die Markierung aufhebt. Durch Festlegen der [SelectionHighlightColor](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionhighlightcolor.aspx)-Eigenschaft können Sie die zum Hervorheben von ausgewähltem Text verwendete Farbe ändern.

TextBox unterstützt standardmäßig das Kopieren und Einfügen. Sie können eine benutzerdefinierte Behandlung des [Paste](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.paste.aspx)-Ereignisses für Steuerelemente für editierbaren Text Ihrer App ermöglichen. Sie könnten z. B. die Zeilenumbrüche aus einer mehrzeiligen Adresse entfernen, die in ein einzeiliges Suchfeld eingegeben wird. Oder Sie überprüfen die Länge des eingefügten Texts und warnen den Benutzer, wenn die maximale Textlänge, die in einer Datenbank gespeichert werden kann, überschritten ist. Weitere Informationen und Beispiele finden Sie im [Paste](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.paste.aspx)-Ereignis.

Hier sehen Sie ein Beispiel für diese verwendeten Eigenschaften und Methoden. Wenn Sie im ersten Textfeld Text markieren, wird dieser im zweiten Textfeld, welches schreibgeschützt ist, angezeigt. Die Werte der Eigenschaften [SelectionLength](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionlength.aspx) und [SelectionStart](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionstart.aspx) werden in zwei Textblöcken angezeigt. Dies erfolgt durch Verwendung des [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionchanged.aspx)-Ereignisses.

```xaml
<StackPanel>
   <TextBox x:Name="textBox1" Height="75" Width="300" Margin="10"
         Text="The text that is selected in this TextBox will show up in the read only TextBox below." 
         TextWrapping="Wrap" AcceptsReturn="True"
         SelectionChanged="TextBox1_SelectionChanged" />
   <TextBox x:Name="textBox2" Height="75" Width="300" Margin="5" 
         TextWrapping="Wrap" AcceptsReturn="True" IsReadOnly="True"/>
   <TextBlock x:Name="label1" HorizontalAlignment="Center"/>
   <TextBlock x:Name="label2" HorizontalAlignment="Center"/>
</StackPanel>
```

```csharp
private void TextBox1_SelectionChanged(object sender, RoutedEventArgs e)
{
    textBox2.Text = textBox1.SelectedText;
    label1.Text = "Selection length is " + textBox1.SelectionLength.ToString();
    label2.Text = "Selection starts at " + textBox1.SelectionStart.ToString();
}
```

Dies ist das Ergebnis des Codes.

![Markierter Text in einem Textfeld](images/text-box-selection.png)

## Auswählen der richtigen Tastatur für Ihr Textsteuerelement

Um Benutzern die Eingabe von Daten mit der Bildschirmtastatur oder dem Soft Input Panel (SIP) zu erleichtern, können Sie den Eingabeumfang des Textsteuerelements an die Art der Daten anpassen, die der Benutzer vermutlich eingeben wird.

Die Bildschirmtastatur kann für die Texteingabe verwendet werden, wenn Ihre App auf einem Gerät mit Touchscreen ausgeführt wird. Die Bildschirmtastatur wird aufgerufen, wenn der Benutzer auf ein bearbeitbares Eingabefeld wie etwa ein TextBox oder RichEditBox tippt. Benutzer können Daten in Ihre App schneller und einfacher eingeben, wenn Sie den Eingabeumfang des Textsteuerelements an die Art der Daten anpassen, die der Benutzer vermutlich eingeben wird. Der Eingabeumfang bietet dem System einen Hinweis auf die Art von Text, die vermutlich über das Steuerelement eingegeben wird. Auf diese Weise kann das System ein spezielles Bildschirmtastaturlayout für den Eingabetyp bereitstellen.

Wird ein Textfeld beispielsweise nur verwendet, um eine vierstellige PIN einzugeben, legen Sie die [InputScope](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.inputscope.aspx)-Eigenschaft auf **Number** fest. Dies weist das System an, das Layout der Zehnertastatur anzuzeigen, das dem Benutzer die Eingabe der PIN erleichtert.

> **Wichtig**&nbsp;&nbsp;Durch diesen Eingabeumfang wird keine Eingabeüberprüfung durchgeführt, und der Benutzer kann Eingaben über eine Hardwaretastatur oder ein anderes Eingabegerät vornehmen. Bei Bedarf müssen Sie dennoch die Benutzereingabe in Ihrem Code überprüfen.

Weitere Eigenschaften, die sich auf die Bildschirmtastatur beziehen, sind [IsSpellCheckEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.isspellcheckenabled.aspx), [IsTextPredictionEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.istextpredictionenabled.aspx) und [PreventKeyboardDisplayOnProgrammaticFocus](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.preventkeyboarddisplayonprogrammaticfocus.aspx). (IsSpellCheckEnabled wirkt sich auch auf TextBox aus, wenn eine Hardware-Tastaur verwendet wird.) 

Weitere Informationen und Beispiele finden Sie unter [Verwenden des Eingabeumfangs zum Ändern der Bildschirmtastatur]() und in der Eigenschaftendokumentation.

## Empfehlungen

-   Verwenden Sie eine Beschriftung oder Platzhaltertext, wenn der Zweck des Textfelds nicht eindeutig ist. Eine Beschriftung ist sichtbar, unabhängig davon, ob das Texteingabefeld über einen Wert verfügt. Platzhaltertext wird im Texteingabefeld angezeigt und wird erst ausgeblendet, wenn der Benutzer einen Wert eingibt.
-   Verleihen Sie dem Textfeld eine entsprechende Breite für den Bereich der Werte, die eingegeben werden können. Die Wortlänge variiert zwischen den einzelnen Sprachen. Sie sollten also die Lokalisierung berücksichtigen, wenn Ihre App global verwendet werden soll.
-   Ein Texteingabefeld ist normalerweise einzeilig (`TextWrap = "NoWrap"`). Wenn Benutzer eine lange Zeichenfolge eingeben oder bearbeiten müssen, sollten Sie ein mehrzeiliges Texteingabefeld festlegen (`TextWrap = "Wrap"`).
-   Im Allgemeinen wird ein Texteingabefeld für bearbeitbaren Text verwendet. Sie können ein Texteingabefeld jedoch auch als schreibgeschützt festlegen, sodass der Feldinhalt gelesen, ausgewählt und kopiert, jedoch nicht bearbeitet werden kann.
-   Um eine unübersichtliche Darstellung in einer Ansicht zu vermeiden, können Sie festlegen, dass ein Teil der Texteingabefelder nur angezeigt wird, wenn ein entsprechendes Kontrollkästchen aktiviert wird. Sie können den Aktivierungszustand eines Texteingabefelds auch an ein Steuerelement binden, z. B. an ein Kontrollkästchen.
-   Legen Sie das gewünschte Verhalten eines Texteingabefelds fest, wenn dieses einen Wert enthält, und ein Benutzer auf das Feld tippt. Als Standardverhalten empfiehlt es sich, dass der Wert bearbeitet werden kann und nicht ersetzt wird. Der Einfügepunkt wird zwischen den Wörtern platziert, und es wird keine Auswahl getroffen. Wenn „Ersetzen“ der am häufigsten verwendete Anwendungsfall für ein bestimmtes Texteingabefeld ist, können Sie den gesamten Text im Feld auswählen, wenn das Steuerelement den Fokus erhält. Durch die Benutzereingabe wird die Auswahl entfernt.

**Einzeilige Eingabefelder**

-   Verwenden Sie mehrere einzeilige Textfelder, um viele kleine Textinformationsteile zu erfassen. Wenn die Textfelder ähnliche Merkmale aufweisen, gruppieren Sie diese.

-   Legen Sie die Größe der einzeiligen Textfelder etwas breiter fest als die längste erwartete Eingabe. Dadurch wird das Steuerelement zu breit, und es wird in zwei Steuerelemente aufgeteilt. Beispielsweise können Sie eine einzelne Adresseingebe in „Adresszeile 1” und „Adresszeile 2” aufteilen.
-   Legen Sie eine maximale Länge für die Zeichen fest, die eingegeben werden können. Wenn die zugrunde liegende Datenquelle keine lange Eingabezeichenfolge zulässt, beschränken Sie die Eingabe, und verwenden Sie ein Popupfenster zur Bestätigung, um die Benutzer bei Erreichen der Beschränkung zu benachrichtigen.
-   Verwenden Sie einzeilige Texteingabesteuerelemente, um kleinere Textmengen der Benutzer zu erfassen.
 
    Das folgende Beispiel zeigt ein einzeiliges Textfeld zur Erfassung der Antwort auf eine Sicherheitsfrage. Da eine kurze Antwort erwartet wird, ist an dieser Stelle ein einzeiliges Textfeld angemessen.

    ![Grundlegende Dateneingabe](images/guidelines_and_checklist_for_singleline_text_input_type_text.png)
    
-   Verwenden Sie eine Gruppe von kurzen einzeiligen Texteingabesteuerelementen fester Größe, um Daten mit einem bestimmten Format einzugeben.

    ![Formatierte Dateneingabe](images/textinput_example_productkey.png)

-   Verwenden Sie ein einzeiliges, nicht eingeschränktes Texteingabesteuerelement, um Zeichenfolgen einzugeben oder zu bearbeiten, in Kombination mit einer Befehlsschaltfläche, über die die Benutzer gültige Werte auswählen können.

    ![Unterstützte Dateneingabe](images/textinput_example_assisted.png)


**Mehrzeilige Texteingabesteuerelemente**

-   Wenn Sie ein Rich-Text-Feld erstellen, stellen Sie Stilschaltflächen bereit, und implementieren Sie die zugehörigen Aktionen.
-   Verwenden Sie eine Schriftart, die mit dem Stil der App konsistent ist.
-   Legen Sie die Höhe des Textsteuerelements so fest, dass genügend Platz für typische Einträge vorhanden ist.
-   Verwenden Sie für die Erfassung von langen Textabschnitten mit einer maximalen Zeichen- oder Wörteranzahl ein Nur-Text-Feld, und stellen Sie einen Live-Zähler bereit, der dem Benutzer anzeigt, wie viele Zeichen bzw. Wörter bis zum Erreichen der Grenze verbleiben. Den Zähler müssen Sie selbst erstellen. Platzieren Sie ihn unter dem Textfeld, und aktualisieren Sie ihn dynamisch, während der Benutzer die einzelnen Zeichen oder Wörter eingibt.

    ![Langer Textabschnitt](images/guidelines_and_checklist_for_multiline_text_input_text_limits.png)

-   Die Höhe der Texteingabesteuerelemente sollte sich während der Benutzereingabe nicht verändern.
-   Verwenden Sie kein mehrzeiliges Textfeld, wenn die Benutzer nur eine Zeile benötigen.
-   Verwenden Sie kein Rich-Text-Steuerelement, wenn ein Nur-Text-Steuerelement angemessen ist.

\[Dieser Artikel enthält spezielle Informationen zu Apps für die universelle Windows-Plattform (UWP) und Windows 10. Laden Sie für Windows 8.1 die [PDF-Datei mit Windows 8.1-Richtlinien](https://go.microsoft.com/fwlink/p/?linkid=258743) herunter.\]

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


<!--HONumber=Mar16_HO1-->


