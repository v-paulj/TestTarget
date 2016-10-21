---
author: Jwmsft
Description: "Ein Texteingabefeld, das während der Benutzereingabe Vorschläge anzeigt."
title: "Richtlinien für Felder mit automatischen Vorschlägen"
ms.assetid: 1F608477-F795-4F33-92FA-F200CC243B6B
dev.assetid: 54F8DB8A-120A-4D79-8B5A-9315A3764C2F
label: Auto-suggest box
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 9406f9b826dfb7d2603a0812f209dfb38cf639ae

---
# Feld mit automatischen Vorschlägen
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Verwenden Sie ein AutoSuggestBox-Element, um eine Liste mit Vorschlägen bereitzustellen, aus der Benutzer bei der Eingabe auswählen können.

![Ein Feld mit automatischen Vorschlägen](images/controls/auto-suggest-box-open.png)

<div class="important-apis" >
<b>Wichtige APIs</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.aspx"><strong>AutoSuggestBox-Klasse</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.textchanged.aspx"><strong>TextChanged-Ereignis</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.suggestionchosen.aspx"><strong>SuggestionChose-Ereignis</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.querysubmitted.aspx"><strong>QuerySubmitted-Ereignis</strong></a></li>
</ul>

</div>
</div>





## Ist dies das richtige Steuerelement?

Wenn Sie ein einfaches, anpassbares Steuerelement verwenden möchten, das die Textsuche mit einer Liste von Vorschlägen ermöglicht, verwenden Sie ein Feld mit automatischen Vorschlägen.

Weitere Informationen zum Auswählen des passenden Textsteuerelements finden Sie im Artikel über [Textsteuerelemente](text-controls.md).

## Beispiele

Ein Feld mit automatischen Vorschlägen in der Groove-Musik-App.

![Ein Feld mit automatischen Vorschlägen in der Groove-Musik-App](images/control-examples/auto-suggest-box-groove.png)

## Aufbau
Der Einstiegspunkt für das Feld mit automatischen Vorschlägen besteht aus einem optionalen Header und einem Feld mit optionalem Hinweistext:

![Beispiel für den Einstiegspunkt für das Steuerelement für automatische Vorschläge](images/controls_autosuggest_entrypoint.png)

Die Ergebnisliste für automatische Vorschläge wird automatisch ausgefüllt, sobald der Benutzer mit der Texteingabe beginnt. Die Ergebnisliste kann oberhalb oder unterhalb des Texteingabefelds angezeigt werden. Eine Schaltfläche „Alles löschen” wird angezeigt:

![Beispiel für das erweiterte Steuerelement für automatische Vorschläge](images/controls_autosuggest_expanded01.png)

## Erstellen eines Felds mit automatischen Vorschlägen

Zum Verwenden eines AutoSuggestBox-Elements müssen Sie auf drei Benutzeraktionen reagieren.

- Text geändert: Aktualisieren Sie die Vorschlagsliste, wenn der Benutzer Text eingibt.
- Vorschlag ausgewählt: Aktualisieren Sie das Textfeld, wenn der Benutzer in der Vorschlagsliste einen Vorschlag auswählt.
- Abfrage gesendet: Zeigen Sie die Abfrageergebnisse an, wenn der Benutzer eine Abfrage sendet.

### Text geändert

Das [**TextChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.textchanged.aspx)-Ereignis tritt ein, wenn der Inhalt des Textfelds aktualisiert wird. Verwenden Sie die [Reason](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestboxtextchangedeventargs.reason.aspx)-Eigenschaft für die Ereignisargumente, um zu ermitteln, ob die Änderung aufgrund einer Benutzereingabe erfolgt ist. Wenn der Grund für die Änderung **UserInput** lautet, filtern Sie Ihre Daten basierend auf der Eingabe. Legen Sie die gefilterten Daten anschließend als [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) des AutoSuggestBox-Elements fest, um die Vorschlagsliste zu aktualisieren.

Um zu steuern, wie Elemente in der Vorschlagsliste angezeigt werden, können Sie [DisplayMemberPath](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.displaymemberpath.aspx) oder [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) verwenden.

- Wenn Sie den Text einer einzelnen Eigenschaft des Datenelements anzeigen möchten, legen Sie die DisplayMemberPath-Eigenschaft fest, um auszuwählen, welche Eigenschaft Ihres Objekts in der Vorschlagsliste angezeigt werden soll.
- Verwenden Sie zum Definieren einer benutzerdefinierten Darstellung für jedes Element in der Liste die ItemTemplate-Eigenschaft.

### Vorschlag ausgewählt

Wenn ein Benutzer mit der Tastatur durch die Vorschlagsliste navigiert, müssen Sie den Text im Textfeld entsprechend aktualisieren.

Sie können die [TextMemberPath](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.textmemberpath.aspx)-Eigenschaft festlegen, um auszuwählen, welche Eigenschaft des Datenobjekts im Textfeld angezeigt werden soll. Wenn Sie ein TextMemberPath-Element angeben, wird das Textfeld automatisch aktualisiert. Sie sollten in der Regel den gleichen Wert für DisplayMemberPath and TextMemberPath angeben, damit der Text in der Vorschlagsliste und im Textfeld identisch ist.

Wenn Sie mehr als eine einfache Eigenschaft anzeigen müssen, behandeln Sie das [SuggestionChosen](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.suggestionchosen.aspx)-Ereignis, um das Textfeld mit benutzerdefiniertem Text basierend auf dem ausgewählten Element zu füllen.

### Abfrage gesendet

Behandeln Sie das [QuerySubmitted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.querysubmitted.aspx) -Ereignis, um eine passende Abfrageaktion für die App durchzuführen und das Ergebnis dem Benutzer anzuzeigen.

Das QuerySubmitted-Ereignis tritt ein, wenn ein Benutzer eine Abfragezeichenfolge absendet. Der Benutzer kann diesen Schritt wie folgt ausführen:
- Mit dem Fokus auf dem Textfeld EINGABETASTE drücken oder auf das Abfragesymbol klicken. Die [ChosenSuggestion](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestboxquerysubmittedeventargs.chosensuggestion.aspx)-Eigenschaft für die Ereignisargumente lautet **null**.
- Mit dem Fokus auf der Vorschlagsliste EINGABETASTE drücken oder auf ein Element klicken oder tippen. Die ChosenSuggestion-Eigenschaft für die Ereignisargumente enthält das Element, das in der Liste ausgewählt wurde.

In allen Fällen enthält die [QueryText](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestboxquerysubmittedeventargs.querytext.aspx)-Eigenschaft für die Ereignisargumente den Text aus dem Textfeld.

## Verwenden von AutoSuggestBox für die Suche

Verwenden Sie ein AutoSuggestBox-Element, um eine Liste mit Vorschlägen bereitzustellen, aus der Benutzer während der Eingabe auswählen können.

Standardmäßig wird für das Texteingabefeld keine Abfrageschaltfläche angezeigt. Sie können die [QueryIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.queryicon.aspx) -Eigenschaft festlegen, um eine Schaltfläche mit dem angegebenen Symbol rechts vom Textfeld hinzuzufügen. Wenn das AutoSuggestBox-Element wie ein typisches Suchfeld aussehen soll, fügen Sie beispielsweise wie folgt ein Suchsymbol hinzu.

```xaml
<AutoSuggestBox QueryIcon="Find"/>
```

Hier ist ein AutoSuggestBox-Element mit einem Suchsymbol dargestellt.

![Beispiel für den Einstiegspunkt für das Steuerelement für automatische Vorschläge](images/controls_autosuggest_entrypoint.png)

## Beispiele

Ein vollständiges, funktionierendes Beispiel für AutoSuggestBox finden Sie unter [Beispiel für AutoSuggestBox-Migration](http://go.microsoft.com/fwlink/p/?LinkId=619996) und [Beispiel für XAML-UI-Grundlagen](http://go.microsoft.com/fwlink/p/?LinkId=619992).

Dies ist ein einfaches AutoSuggestBox-Element mit den erforderlichen Ereignishandlern.

```xaml
<AutoSuggestBox PlaceholderText="Search" QueryIcon="Find" Width="200"
                TextChanged="AutoSuggestBox_TextChanged"
                QuerySubmitted="AutoSuggestBox_QuerySubmitted"
                SuggestionChosen="AutoSuggestBox_SuggestionChosen"/>
```

```csharp
private void AutoSuggestBox_TextChanged(AutoSuggestBox sender, AutoSuggestBoxTextChangedEventArgs args)
{
    // Only get results when it was a user typing,
    // otherwise assume the value got filled in by TextMemberPath
    // or the handler for SuggestionChosen.
    if (args.Reason == AutoSuggestionBoxTextChangeReason.UserInput)
    {
        //Set the ItemsSource to be your filtered dataset
        //sender.ItemsSource = dataset;
    }
}


private void AutoSuggestBox_SuggestionChosen(AutoSuggestBox sender, AutoSuggestBoxSuggestionChosenEventArgs args)
{
    // Set sender.Text. You can use args.SelectedItem to build your text string.
}


private void AutoSuggestBox_QuerySubmitted(AutoSuggestBox sender, AutoSuggestBoxQuerySubmittedEventArgs args)
{
    if (args.ChosenSuggestion != null)
    {
        // User selected an item from the suggestion list, take an action on it here.
    }
    else
    {
        // Use args.QueryText to determine what to do.
    }
}
```

## Empfohlene und nicht empfohlene Vorgehensweisen

-   Zeigen Sie, wenn Sie das Feld mit automatischen Vorschlägen zum Durchführen von Suchen verwenden und keine Suchergebnisse für den eingegebenen Text vorhanden sind, die einzeilige Meldung „Keine Ergebnisse” an, damit Benutzer wissen, dass ihre Suchanfrage ausgeführt wurde:

    ![Beispiel für ein Feld mit automatischen Vorschlägen ohne Suchergebnisse](images/controls_autosuggest_noresults.png)


## Verwandte Artikel

- [Textsteuerelemente](text-controls.md)
- [Rechtschreibprüfung](spell-checking-and-prediction.md)
- [Suche](search.md)
- [**TextBox-Klasse**](https://msdn.microsoft.com/library/windows/apps/br209683)
- [**Windows.UI.Xaml.Controls PasswordBox-Klasse**](https://msdn.microsoft.com/library/windows/apps/br227519)
- [StringLength-Eigenschaft](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)



<!--HONumber=Aug16_HO3-->


