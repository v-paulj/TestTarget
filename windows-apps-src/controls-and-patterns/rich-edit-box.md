---
author: Jwmsft
Description: "Sie können ein RichEditBox-Steuerelement verwenden, um Rich-Text-Dokumente zu bearbeiten, die formatierten Text, Hyperlinks und Bilder enthalten. Sie können den Schreibschutz für ein RichEditBox-Steuerelement aktivieren, indem Sie die IsReadOnly-Eigenschaft des Steuerelements auf „true“ festlegen."
title: RichEditBox
ms.assetid: 4AFC0DFA-3B89-434D-9F86-4309CCFF7839
label: Rich edit box
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: f26bcc596417f607ee348e93009905ec4a3e27c8

---
# RichEditBox

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Sie können ein RichEditBox-Steuerelement verwenden, um Rich-Text-Dokumente zu bearbeiten, die formatierten Text, Hyperlinks und Bilder enthalten. Sie können den Schreibschutz für ein RichEditBox-Steuerelement aktivieren, indem Sie die IsReadOnly-Eigenschaft des Steuerelements auf **true** festlegen.

<div class="important-apis" >
<b>Wichtige APIs</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx"><strong>RichEditBox-Klasse</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.document.aspx"><strong>Document-Eigenschaft</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.isreadonly.aspx"><strong>IsReadOnly-Eigenschaft</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.isspellcheckenabled.aspx"><strong>IsSpellCheckEnabled-Eigenschaft</strong></a></li>
</ul>

</div>
</div>






## Ist dies das richtige Steuerelement?

Verwenden Sie **RichEditBox** zum Anzeigen und Bearbeiten von Textdateien. RichEditBox wird nicht verwendet, um wie bei anderen Standard-Texteingabefeldern Benutzereingaben in der App abzurufen. Es wird vielmehr dazu verwendet, mit Textdateien zu arbeiten, die von Ihrer App getrennt sind. Normalerweise speichern Sie Text, der in einer RichEditBox eingegeben wird, in einer RTF-Datei.
-   Wenn der Hauptzweck des mehrzeiligen Textfelds darin besteht, Dokumente zu erstellen (z. B. Blogeinträge oder die Inhalte einer E-Mail-Nachricht) und diese Dokumente Rich-Text erfordern, verwenden Sie ein Rich-Text-Feld.
-   Wenn die Benutzer in der Lage sein sollen, ihre Texte zu formatieren, verwenden Sie ein Rich-Text-Feld.
-   Wenn Texte erfasst werden, die nur genutzt und nicht für die Benutzer erneut angezeigt werden, verwenden Sie ein Nur-Text-Eingabesteuerelement.
-   Verwenden Sie in allen anderen Szenarien ein Nur-Text-Eingabesteuerelement.

Weitere Informationen zur Auswahl des passenden Textsteuerelements finden Sie im Artikel über [Textsteuerelemente](text-controls.md).

## Beispiele

In diesem Rich-Edit-Feld ist ein Rich-Text-Dokument geöffnet. Die Formatierungs- und Dateischaltflächen sind nicht Teil des Rich-Edit-Felds, Sie sollten aber zumindest grundlegende Formatierungsschaltflächen bereitstellen und ihre Aktionen implementieren.

![Ein Rich-Text-Feld mit einem geöffneten Dokument](images/rich-edit-box.png)

## Erstellen eines Rich-Edit-Felds

RichEditBox unterstützt standardmäßig die Rechtschreibprüfung. Um die Rechtschreibprüfung zu deaktivieren, legen Sie die [IsSpellCheckEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.isspellcheckenabled.aspx)-Eigenschaft auf **false** fest. Weitere Informationen finden Sie im Artikel [Richtlinien für die Rechtschreibprüfung](spell-checking-and-prediction.md).

Mit der [Document](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.document.aspx)-Eigenschaft von RichEditBox können Sie den Inhalt des Steuerelements abrufen. Der Inhalt eines RichEditBox ist anders als das RichTextBlock-Steuerelement ein [Windows.UI.Text.ITextDocument](https://msdn.microsoft.com/library/windows/apps/xaml/bb774052.aspx)-Objekt, das [Windows.UI.Xaml.Documents.Block](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.block.aspx)-Objekte als Inhalte verwendet. Die ITextDocument-Schnittstelle bietet unter anderem folgende Möglichkeiten: Dokumente in einen Datenstrom laden und speichern, Textbereiche abrufen, die aktive Auswahl abrufen, Änderungen rückgängig machen und wiederholen, Standardformatierungsattribute festlegen usw.

Dieses Beispiel zeigt, wie Sie eine RTF-Datei (Rich Text Format) bearbeiten, sie in ein RichEditBox laden und speichern.

```xaml
<RelativePanel Margin="20" HorizontalAlignment="Stretch">
    <RelativePanel.Resources>
        <Style TargetType="AppBarButton">
            <Setter Property="IsCompact" Value="True"/>
        </Style>
    </RelativePanel.Resources>
    <AppBarButton x:Name="openFileButton" Icon="OpenFile"
                  Click="OpenButton_Click" ToolTipService.ToolTip="Open file"/>
    <AppBarButton Icon="Save" Click="SaveButton_Click"
                  ToolTipService.ToolTip="Save file"
                  RelativePanel.RightOf="openFileButton" Margin="8,0,0,0"/>

    <AppBarButton Icon="Bold" Click="BoldButton_Click" ToolTipService.ToolTip="Bold"
                  RelativePanel.LeftOf="italicButton" Margin="0,0,8,0"/>
    <AppBarButton x:Name="italicButton" Icon="Italic" Click="ItalicButton_Click"
                  ToolTipService.ToolTip="Italic" RelativePanel.LeftOf="underlineButton" Margin="0,0,8,0"/>
    <AppBarButton x:Name="underlineButton" Icon="Underline" Click="UnderlineButton_Click"
                  ToolTipService.ToolTip="Underline" RelativePanel.AlignRightWithPanel="True"/>

    <RichEditBox x:Name="editor" Height="200" RelativePanel.Below="openFileButton"
                 RelativePanel.AlignLeftWithPanel="True" RelativePanel.AlignRightWithPanel="True"/>
</RelativePanel>
```


```csharp
private async void OpenButton_Click(object sender, RoutedEventArgs e)
{
    // Open a text file.
    Windows.Storage.Pickers.FileOpenPicker open =
        new Windows.Storage.Pickers.FileOpenPicker();
    open.SuggestedStartLocation =
        Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
    open.FileTypeFilter.Add(".rtf");

    Windows.Storage.StorageFile file = await open.PickSingleFileAsync();

    if (file != null)
    {
        try
        {
            Windows.Storage.Streams.IRandomAccessStream randAccStream =
        await file.OpenAsync(Windows.Storage.FileAccessMode.Read);

            // Load the file into the Document property of the RichEditBox.
            editor.Document.LoadFromStream(Windows.UI.Text.TextSetOptions.FormatRtf, randAccStream);
        }
        catch (Exception)
        {
            ContentDialog errorDialog = new ContentDialog()
            {
                Title = "File open error",
                Content = "Sorry, I couldn't open the file.",
                PrimaryButtonText = "Ok"
            };

            await errorDialog.ShowAsync();
        }
    }
}

private async void SaveButton_Click(object sender, RoutedEventArgs e)
{
    Windows.Storage.Pickers.FileSavePicker savePicker = new Windows.Storage.Pickers.FileSavePicker();
    savePicker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;

    // Dropdown of file types the user can save the file as
    savePicker.FileTypeChoices.Add("Rich Text", new List<string>() { ".rtf" });

    // Default file name if the user does not type one in or select a file to replace
    savePicker.SuggestedFileName = "New Document";

    Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();
    if (file != null)
    {
        // Prevent updates to the remote version of the file until we
        // finish making changes and call CompleteUpdatesAsync.
        Windows.Storage.CachedFileManager.DeferUpdates(file);
        // write to file
        Windows.Storage.Streams.IRandomAccessStream randAccStream =
            await file.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);

        editor.Document.SaveToStream(Windows.UI.Text.TextGetOptions.FormatRtf, randAccStream);

        // Let Windows know that we're finished changing the file so the
        // other app can update the remote version of the file.
        Windows.Storage.Provider.FileUpdateStatus status = await Windows.Storage.CachedFileManager.CompleteUpdatesAsync(file);
        if (status != Windows.Storage.Provider.FileUpdateStatus.Complete)
        {
            Windows.UI.Popups.MessageDialog errorBox =
                new Windows.UI.Popups.MessageDialog("File " + file.Name + " couldn't be saved.");
            await errorBox.ShowAsync();
        }
    }
}

private void BoldButton_Click(object sender, RoutedEventArgs e)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
        charFormatting.Bold = Windows.UI.Text.FormatEffect.Toggle;
        selectedText.CharacterFormat = charFormatting;
    }
}

private void ItalicButton_Click(object sender, RoutedEventArgs e)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
        charFormatting.Italic = Windows.UI.Text.FormatEffect.Toggle;
        selectedText.CharacterFormat = charFormatting;
    }
}

private void UnderlineButton_Click(object sender, RoutedEventArgs e)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
        if (charFormatting.Underline == Windows.UI.Text.UnderlineType.None)
        {
            charFormatting.Underline = Windows.UI.Text.UnderlineType.Single;
        }
        else {
            charFormatting.Underline = Windows.UI.Text.UnderlineType.None;
        }
        selectedText.CharacterFormat = charFormatting;
    }
}
```

## Auswählen der richtigen Tastatur für das Textsteuerelement

Um Benutzern die Eingabe von Daten mit der Bildschirmtastatur oder dem Soft Input Panel (SIP) zu erleichtern, können Sie den Eingabeumfang des Textsteuerelements an die Art der Daten anpassen, die der Benutzer vermutlich eingeben wird. Das Standardtastaturlayout ist in der Regel für die Arbeit mit Rich-Text-Dokumenten geeignet.

Weitere Informationen zur Verwendung von Eingabeumfängen finden Sie unter [Verwenden des Eingabeumfangs zum Ändern der Bildschirmtastatur](https://msdn.microsoft.com/library/windows/apps/mt280229).

## Empfehlungen

-   Wenn Sie ein Rich-Text-Feld erstellen, stellen Sie Stilschaltflächen bereit, und implementieren Sie die zugehörigen Aktionen.
-   Verwenden Sie eine Schriftart, die mit dem Stil der App konsistent ist.
-   Legen Sie die Höhe des Textsteuerelements so fest, dass genügend Platz für typische Einträge vorhanden ist.
-   Die Höhe der Texteingabesteuerelemente sollte sich während der Benutzereingabe nicht verändern.
-   Verwenden Sie kein mehrzeiliges Textfeld, wenn die Benutzer nur eine Zeile benötigen.
-   Verwenden Sie kein Rich-Text-Steuerelement, wenn ein Nur-Text-Steuerelement ausreicht.





## Verwandte Artikel

[Textsteuerelemente](text-controls.md)

**Für Designer**
- [Richtlinien für die Rechtschreibprüfung](spell-checking-and-prediction.md)
- [Hinzufügen von Suchfunktionen](search.md)
- [Richtlinien für die Texteingabe](text-controls.md)

**Für Entwickler (XAML)**
- [**TextBox-Klasse**](https://msdn.microsoft.com/library/windows/apps/br209683)
- [**Windows.UI.Xaml.Controls PasswordBox-Klasse**](https://msdn.microsoft.com/library/windows/apps/br227519)



<!--HONumber=Aug16_HO3-->


