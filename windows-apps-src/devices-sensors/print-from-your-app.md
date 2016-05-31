---
author: DBirtolo
ms.assetid: 9A0F1852-A76B-4F43-ACFC-2CC56AAD1C03
title: Drucken in Apps
description: Hier erfahren Sie, wie Sie Dokumente in einer Universellen Windows-App drucken. In diesem Thema wird zudem gezeigt, wie bestimmte Seiten gedruckt werden.
---
# Drucken in Apps

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


** Wichtige APIs **

-   [**Windows.Graphics.Printing**](https://msdn.microsoft.com/library/windows/apps/BR226489)
-   [**Windows.UI.Xaml.Printing**](https://msdn.microsoft.com/library/windows/apps/BR243325)
-   [**PrintDocument**](https://msdn.microsoft.com/library/windows/apps/BR243314)

Hier erfahren Sie, wie Sie Dokumente in einer universellen Windows-App drucken. In diesem Thema wird zudem gezeigt, wie bestimmte Seiten gedruckt werden. Informationen zu erweiterten Änderungen an der Benutzeroberfläche für die Druckvorschau finden Sie unter [Anpassen der Benutzeroberfläche für die Druckvorschau](customize-the-print-preview-ui.md).

**Tipp**  Die Mehrzahl der Beispiele in diesem Thema basiert auf dem Druckbeispiel. Laden Sie das [Druckbeispiel für die universelle Windows-Plattform (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619984) aus dem Repository [Beispiele für Universelle Windows-Plattform](http://go.microsoft.com/fwlink/p/?LinkId=619979) auf GitHub herunter, um den vollständigen Code anzuzeigen.

## Registrieren für Druckfunktionen

Um Ihrer App Druckfunktionen hinzuzufügen, müssen Sie sie als Erstes für den Vertrag für "Drucken" registrieren. Die Registrierung ist für jeden Bildschirm erforderlich, auf dem der Benutzer drucken können soll. Es kann jeweils nur der Bildschirm, der gerade angezeigt wird, für das Drucken registriert werden. Wenn ein Bildschirm Ihrer App für das Drucken registriert wurde, muss die Registrierung beim Schließen des Bildschirms aufgehoben werden. Wird an seiner Stelle ein anderer Bildschirm angezeigt, muss dieser nächste Bildschirm beim Öffnen für einen neuen Vertrag für „Drucken“ registriert werden.

**Tipp**  Falls das Drucken mehrerer Seiten in Ihrer App unterstützt werden soll, können Sie diesen Druckcode in eine allgemeine Hilfsprogrammklasse einfügen und von den App-Seiten wiederverwenden lassen. Ein entsprechendes Beispiel finden Sie in der `PrintHelper`-Klasse im [UWP-Druckbeispiel](http://go.microsoft.com/fwlink/p/?LinkId=619984).

Geben Sie zuerst die Klassen [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426) und [**PrintDocument**](https://msdn.microsoft.com/library/windows/apps/BR243314) an. Der **PrintManager**-Typ und Typen zur Unterstützung anderer Windows-Druckfunktionen befinden sich im [**Windows.Graphics.Printing**](https://msdn.microsoft.com/library/windows/apps/BR226489)-Namespace. Der **PrintDocument**-Typ und andere Typen zur Unterstützung der Vorbereitung von XAML-Inhalten für das Drucken sind im [**Windows.UI.Xaml.Printing**](https://msdn.microsoft.com/library/windows/apps/BR243325)-Namespace enthalten. Sie können das Schreiben eines eigenen Druckcodes vereinfachen, indem Sie der Seite die folgenden **using**- oder **Imports**-Anweisungen hinzufügen.

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
```

Die [**PrintDocument**](https://msdn.microsoft.com/library/windows/apps/BR243314)-Klasse wird verwendet, um den Großteil der Interaktion zwischen der App und dem [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426) zu behandeln. Sie macht jedoch auch verschiedene eigene Rückrufe verfügbar. Erstellen Sie während der Registrierung Instanzen der Klassen **PrintManager** und **PrintDocument**, und registrieren Sie Handler für deren Druckereignisse.

Im [UWP-Druckbeispiel](http://go.microsoft.com/fwlink/p/?LinkId=619984) wird die Registrierung von der `RegisterForPrinting`-Methode durchgeführt.

```csharp
public virtual void RegisterForPrinting()
{
   printDocument = new PrintDocument();
   printDocumentSource = printDocument.DocumentSource;
   printDocument.Paginate += CreatePrintPreviewPages;
   printDocument.GetPreviewPage += GetPrintPreviewPage;
   printDocument.AddPages += AddPrintPages;

   PrintManager printMan = PrintManager.GetForCurrentView();
   printMan.PrintTaskRequested += PrintTaskRequested;
}
```

Wenn der Benutzer zu einer Seite wechselt, die dies unterstützt, wird die Registrierung innerhalb der `OnNavigatedTo`-Methode initiiert.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
   // Initalize common helper class and register for printing
   printHelper = new PrintHelper(this);
   printHelper.RegisterForPrinting();

   // Initialize print content for this scenario
   printHelper.PreparePrintContent(new PageToPrint());

   // Tell the user how to print
   MainPage.Current.NotifyUser("Print contract registered with customization, use the Print button to print.", NotifyType.StatusMessage);
}
```

Wenn der Benutzer die Seite verlässt, trennen Sie die Verbindung mit den Druckereignishandlern. Falls Sie bei einer App mit mehreren Seiten die Verbindung nicht trennen, wird eine Ausnahme ausgelöst, wenn der Benutzer die Seite verlässt und sie dann erneut aufruft.

```csharp
protected override void OnNavigatedFrom(NavigationEventArgs e)
{
   if (printHelper != null)
   {
         printHelper.UnregisterForPrinting();
   }
}
```
## Erstellen einer Druckschaltfläche

Fügen Sie eine Druckschaltfläche an der gewünschten Stelle auf dem App-Bildschirm hinzu. Stellen Sie sicher, dass sie den zu druckenden Inhalt nicht verdeckt oder anderweitig stört.

```xml
<Button x:Name="InvokePrintingButton" Content="Print" Click="OnPrintButtonClick"/>
```

Als Nächstes fügen Sie einen Ereignishandler zum Behandeln des Click-Ereignisses zum Code Ihrer App hinzu. Verwenden Sie die [**ShowPrintUIAsync**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing.printmanager.showprintuiasync)-Methode, um das Drucken über Ihre App zu starten. **ShowPrintUIAsync** ist eine asynchrone Methode, die das entsprechende Druckfenster anzeigt. Wenn das Drucken zu diesem Zeitpunkt nicht möglich ist, löst die Methode eine Ausnahme aus. Es wird empfohlen, die Ausnahmen abzufangen und dem Benutzer mitzuteilen, wenn der Druckvorgang nicht fortgesetzt werden kann, wie hier gezeigt.

```csharp
async private void OnPrintButtonClick(object sender, RoutedEventArgs e)
{
    try
    {
        // Show print UI
        await Windows.Graphics.Printing.PrintManager.ShowPrintUIAsync();

    }
    catch
    {
        // Printing cannot proceed at this time
        ContentDialog noPrintingDialog = new ContentDialog()
        {
            Title = "Printing error",
            Content = "\nSorry, printing can' t proceed at this time.", PrimaryButtonText = "OK"
        };
        await noPrintingDialog.ShowAsync();
    }
}
```

In diesem Beispiel wird ein Druckfenster im Ereignishandler für das Klicken auf eine Schaltfläche angezeigt. Wenn die Methode eine Ausnahme auslöst (da das Drucken zu diesem Zeitpunkt nicht möglich ist), informiert ein [**ContentDialog**](https://msdn.microsoft.com/library/windows/apps/Dn633972)-Steuerelement den Benutzer über die Situation.

## Formatieren der App-Inhalte

Wenn **ShowPrintUIAsync** aufgerufen wird, wird das [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597)-Ereignis ausgelöst. Der in diesem Schritt gezeigte **PrintTaskRequested**-Ereignishandler erstellt eine [**PrintTask**](https://msdn.microsoft.com/library/windows/apps/BR226436)-Klasse, indem er die [**PrintTaskRequest.CreatePrintTask**](https://msdn.microsoft.com/library/windows/apps/BR226436request_createprinttask)-Methode aufruft und den Titel für die zu druckende Seite sowie den Namen eines [**PrintTaskSourceRequestedHandler**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing.printtask.source) -Delegaten übergibt. Beachten Sie, dass **PrintTaskSourceRequestedHandler** in diesem Beispiel inline definiert wird. **PrintTaskSourceRequestedHandler** stellt den formatierten Inhalt für das Drucken bereit und wird an späterer Stelle beschrieben.

In diesem Beispiel wurde außerdem ein Abschlusshandler definiert, um Fehler aufzufangen. Es empfiehlt sich, Abschlussereignisse zu behandeln, da die App den Benutzer dann über aufgetretene Fehler und mögliche Lösungen informieren kann. Die App kann das Abschlussereignis auch verwenden, um nachfolgende Schritte anzugeben, die der Benutzer nach einem erfolgreichen Druckauftrag ausführen kann.

```csharp
protected virtual void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequested =>
   {
         // Print Task event handler is invoked when the print job is completed.
         printTask.Completed += async (s, args) =>
         {
            // Notify the user when the print operation fails.
            if (args.Completion == PrintTaskCompletion.Failed)
            {
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     MainPage.Current.NotifyUser("Failed to print.", NotifyType.ErrorMessage);
               });
            }
         };

         sourceRequested.SetSource(printDocumentSource);
   });
}
```

Nachdem die Druckaufgabe erstellt wurde, löst der [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426) das [**Paginate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.paginate)-Ereignis aus, um eine Sammlung zu druckender Seiten anzufordern, die auf der Druckvorschau-Benutzeroberfläche angezeigt werden. Dies entspricht der **Paginate**-Methode der **IPrintPreviewPageCollection**-Schnittstelle. Der Ereignishandler, den Sie bei der Registrierung erstellt haben, wird zu diesem Zeitpunkt aufgerufen.

**Wichtig**  Wenn der Benutzer die Druckeinstellungen ändert, wird der Ereignishandler für die Aufteilung auf Seiten erneut aufgerufen, damit Sie den Inhalt neu umbrechen können. Für die bestmögliche Benutzerfreundlichkeit wird empfohlen, die Einstellungen zu überprüfen, bevor Sie den Inhalt umbrechen und die erneute Initialisierung der auf Seiten aufgeteilten Inhalte vermeiden, wenn dies nicht erforderlich ist.

Im [**Paginate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.paginate)-Ereignishandler (der `CreatePrintPreviewPages`-Methode im [UWP-Druckbeispiel](http://go.microsoft.com/fwlink/p/?LinkId=619984)) erstellen Sie die Seiten, die auf der Druckvorschau-Benutzeroberfläche angezeigt und an den Drucker gesendet werden. Der Code zum Vorbereiten der App-Inhalte für den Druck muss speziell an Ihre App und die gedruckten Inhalte angepasst werden. Im Quellcode für das [UWP-Druckbeispiel](http://go.microsoft.com/fwlink/p/?LinkId=619984) können Sie sehen, wie der Inhalt für das Drucken formatiert wird.

```csharp
protected virtual void CreatePrintPreviewPages(object sender, PaginateEventArgs e)
{
   // Clear the cache of preview pages
   printPreviewPages.Clear();

   // Clear the print canvas of preview pages
   PrintCanvas.Children.Clear();

   // This variable keeps track of the last RichTextBlockOverflow element that was added to a page which will be printed
   RichTextBlockOverflow lastRTBOOnPage;

   // Get the PrintTaskOptions
   PrintTaskOptions printingOptions = ((PrintTaskOptions)e.PrintTaskOptions);

   // Get the page description to deterimine how big the page is
   PrintPageDescription pageDescription = printingOptions.GetPageDescription(0);

   // We know there is at least one page to be printed. passing null as the first parameter to
   // AddOnePrintPreviewPage tells the function to add the first page.
   lastRTBOOnPage = AddOnePrintPreviewPage(null, pageDescription);

   // We know there are more pages to be added as long as the last RichTextBoxOverflow added to a print preview
   // page has extra content
   while (lastRTBOOnPage.HasOverflowContent && lastRTBOOnPage.Visibility == Windows.UI.Xaml.Visibility.Visible)
   {
         lastRTBOOnPage = AddOnePrintPreviewPage(lastRTBOOnPage, pageDescription);
   }

   if (PreviewPagesCreated != null)
   {
         PreviewPagesCreated.Invoke(printPreviewPages, null);
   }

   PrintDocument printDoc = (PrintDocument)sender;

   // Report the number of preview pages created
   printDoc.SetPreviewPageCount(printPreviewPages.Count, PreviewPageCountType.Intermediate);
}
```

Wenn eine bestimmte Seite in der Druckvorschau angezeigt werden soll, löst [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426) das [**GetPreviewPage**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.getpreviewpage)-Ereignis aus. Dies entspricht der **MakePage**-Methode der **IPrintPreviewPageCollection**-Schnittstelle. Der Ereignishandler, den Sie bei der Registrierung erstellt haben, wird zu diesem Zeitpunkt aufgerufen.

Legen Sie im [**GetPreviewPage**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.getpreviewpage)-Ereignishandler (der `GetPrintPreviewPage`-Methode im [UWP-Druckbeispiel](http://go.microsoft.com/fwlink/p/?LinkId=619984)) die entsprechende Seite des Druckdokuments fest.

```csharp
protected virtual void GetPrintPreviewPage(object sender, GetPreviewPageEventArgs e)
{
   PrintDocument printDoc = (PrintDocument)sender;
   printDoc.SetPreviewPage(e.PageNumber, printPreviewPages[e.PageNumber - 1]);
}
```

Nachdem der Benutzer auf die Druckschaltfläche geklickt hat, fordert [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426) die abschließende Sammlung der an den Drucker zu sendenden Seiten an, indem die **MakeDocument**-Methode der **IDocumentPageSource**-Schnittstelle aufgerufen wird. In XAML löst dies das [**AddPages**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.addpages)-Ereignis aus. Der Ereignishandler, den Sie bei der Registrierung erstellt haben, wird zu diesem Zeitpunkt aufgerufen.

Im [**AddPages**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.addpages)-Ereignishandler (der `AddPrintPages`-Methode im [UWP-Druckbeispiel](http://go.microsoft.com/fwlink/p/?LinkId=619984)) fügen Sie dem [**PrintDocument**](https://msdn.microsoft.com/library/windows/apps/BR243314)-Objekt, das an den Drucker gesendet werden soll, Seiten aus der Seitensammlung hinzu. Wenn ein Benutzer bestimmte Seiten oder einen Seitenbereich zum Drucken angibt, verwenden Sie diese Informationen, um nur die Seiten hinzuzufügen, die tatsächlich an den Drucker gesendet werden.

```csharp
protected virtual void AddPrintPages(object sender, AddPagesEventArgs e)
{
   // Loop over all of the preview pages and add each one to  add each page to be printied
   for (int i = 0; i < printPreviewPages.Count; i++)
   {
         // We should have all pages ready at this point...
         printDocument.AddPage(printPreviewPages[i]);
   }

   PrintDocument printDoc = (PrintDocument)sender;

   // Indicate that all of the print pages have been provided
   printDoc.AddPagesComplete();
}
```

## Vorbereiten von Druckoptionen

Als Nächstes bereiten Sie Druckoptionen vor. Dieser Abschnitt beschreibt als Beispiel, wie die Seitenbereichsoption festgelegt wird, um das Drucken bestimmter Seiten zu gestatten. Weitere Optionen finden Sie unter [Anpassen der Benutzeroberfläche für die Druckvorschau](customize-the-print-preview-ui.md).

In diesem Schritt wird eine neue Druckoption erstellt und eine Liste von Werten definiert, die die Option unterstützt. Dann wird die Option der Druckvorschau-UI hinzugefügt. Die Seitenbereichsoption hat folgende Einstellungen:

| Optionsname          | Aktion | 
|----------------------|--------|
| **Print all**        | Druckt alle Seiten im Dokument. |
| **Print Selection**  | Druckt nur den vom Benutzer ausgewählten Inhalt. | 
| **Print Range**      | Zeigt ein Bearbeitungssteuerelement an, in das der Benutzer die zu druckenden Seiten eingeben kann. |
 
Ändern Sie zunächst den [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597)-Ereignishandler, um den Code zum Abrufen eines [**PrintTaskOptionDetails**](https://msdn.microsoft.com/library/windows/apps/Hh701256)-Objekts hinzuzufügen.

```csharp
PrintTaskOptionDetails printDetailedOptions = PrintTaskOptionDetails.GetFromPrintTaskOptions(printTask.Options);
```

Löschen Sie die Liste der Optionen, die in der Druckvorschau-Benutzeroberfläche angezeigt werden, und fügen Sie die Optionen hinzu, die angezeigt werden sollen, wenn der Benutzer in der App druckt.

**Hinweis**  Die Optionen werden in der Druckvorschau-Benutzeroberfläche in der Reihenfolge angezeigt, in der sie hinzugefügt werden, wobei die erste Option oben im Fenster erscheint.

```csharp
IList<string> displayedOptions = printDetailedOptions.DisplayedOptions;

displayedOptions.Clear();
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.ColorMode);
```

Erstellen Sie die neue Druckoption, und initialisieren Sie die Liste der Optionswerte.

```csharp
// Create a new list option
PrintCustomItemListOptionDetails pageFormat = printDetailedOptions.CreateItemListOption("PageRange", "Page Range");
pageFormat.AddItem("PrintAll", "Print all");
pageFormat.AddItem("PrintSelection", "Print Selection");
pageFormat.AddItem("PrintRange", "Print Range");
```

Fügen Sie Ihre benutzerdefinierte Druckoption hinzu, und weisen Sie den Ereignishandler zu. Die benutzerdefinierte Option wird als Letztes hinzugefügt, sodass sie am Ende der Optionsliste erscheint. Sie können sie aber an einer beliebigen Stelle der Liste platzieren. Benutzerdefinierte Druckoptionen müssen nicht zuletzt hinzugefügt werden.

```csharp
// Add the custom option to the option list
displayedOptions.Add("PageRange");

// Create new edit option
PrintCustomTextOptionDetails pageRangeEdit = printDetailedOptions.CreateTextOption("PageRangeEdit", "Range");

// Register the handler for the option change event
printDetailedOptions.OptionChanged += printDetailedOptions_OptionChanged;
```

Die [**CreateTextOption**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing.optiondetails.printtaskoptiondetails.createtextoption)-Methode erstellt das Textfeld **Bereich**. Hier gibt der Benutzer die zu druckenden Seiten ein, wenn die Option **Druckbereich** ausgewählt wurde.

## Behandeln von Druckoptionsänderungen

Der **OptionChanged**-Ereignishandler ist im Wesentlichen für zwei Dinge zuständig. Erstens blendet er das Texteingabefeld für den Seitenbereich je nach der vom Benutzer ausgewählten Seitenbereichsoption ein und aus. Zweitens testet er den Text, den der Benutzer in das Seitenbereich-Textfeld eingibt, um sicherzustellen, dass es sich um einen gültigen Seitenbereich für das Dokument handelt.

Dieses Beispiel zeigt, wie das [UWP-Druckbeispiel](http://go.microsoft.com/fwlink/p/?LinkId=619984) Änderungsereignisse behandelt.

```csharp
async void printDetailedOptions_OptionChanged(PrintTaskOptionDetails sender, PrintTaskOptionChangedEventArgs args)
{
   if (args.OptionId == null)
   {
         return;
   }

   string optionId = args.OptionId.ToString();

   // Handle change in Page Range Option
   if (optionId == "PageRange")
   {
         IPrintOptionDetails pageRange = sender.Options[optionId];
         string pageRangeValue = pageRange.Value.ToString();

         selectionMode = false;

         switch (pageRangeValue)
         {
            case "PrintRange":
               // Add PageRangeEdit custom option to the option list
               sender.DisplayedOptions.Add("PageRangeEdit");
               pageRangeEditVisible = true;
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     ShowContent(null);
               });
               break;
            case "PrintSelection":
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     Scenario4PageRange page = (Scenario4PageRange)scenarioPage;
                     PageToPrint pageContent = (PageToPrint)page.PrintFrame.Content;
                     ShowContent(pageContent.TextContentBlock.SelectedText);
               });
               RemovePageRangeEdit(sender);
               break;
            default:
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     ShowContent(null);
               });
               RemovePageRangeEdit(sender);
               break;
         }

         Refresh();
   }
   else if (optionId == "PageRangeEdit")
   {
         IPrintOptionDetails pageRange = sender.Options[optionId];
         // Expected range format (p1,p2...)*, (p3-p9)* ...
         if (!Regex.IsMatch(pageRange.Value.ToString(), @"^\s*\d+\s*(\-\s*\d+\s*)?(\,\s*\d+\s*(\-\s*\d+\s*)?)*$"))
         {
            pageRange.ErrorText = "Invalid Page Range (eg: 1-3, 5)";
         }
         else
         {
            pageRange.ErrorText = string.Empty;
            try
            {
               GetPagesInRange(pageRange.Value.ToString());
               Refresh();
            }
            catch (InvalidPageException ipex)
            {
               pageRange.ErrorText = ipex.Message;
            }
         }
   }
}
```

**Tipp**  Weitere Informationen zum Analysieren des vom Benutzer in das Textfeld für den Bereich eingegebenen Werts finden Sie in der `GetPagesInRange`-Methode im [UWP-Druckbeispiel](http://go.microsoft.com/fwlink/p/?LinkId=619984).

## Vorschau auf ausgewählte Seiten

Die Formatierung des Inhalts Ihrer App zum Drucken hängt von der Art der App und deren Inhalt ab. Das [UWP-Druckbeispiel](http://go.microsoft.com/fwlink/p/?LinkId=619984) verwendet eine Druckhilfsprogrammklasse, um seinen Inhalt für den Druckvorgang zu formatieren.

Wenn nur ein Teil der Seiten gedruckt wird, gibt es mehrere Möglichkeiten, den Inhalt in der Druckvorschau anzuzeigen. Unabhängig davon, welche Methode Sie zum Anzeigen des Seitenbereichs in der Druckvorschau ausgewählt haben, darf der Ausdruck nur die ausgewählten Seiten enthalten.

-   Alle Seiten in der Druckvorschau anzeigen, unabhängig davon, ob ein Seitenbereich festgelegt wurde. Der Benutzer muss selbst wissen, welche Seiten ausgedruckt werden.
-   Nur den vom Benutzer ausgewählten Seitenbereich in der Druckvorschau anzeigen und die Anzeige aktualisieren, wenn der Benutzer den Seitenbereich ändert.
-   Alle Seiten in der Druckvorschau anzeigen, dabei aber alle Seiten ausgrauen, die nicht in dem vom Benutzer ausgewählten Seitenbereich liegen.

## Verwandte Themen

* [Gestaltungsrichtlinien für Druckvorgänge](https://msdn.microsoft.com/library/windows/apps/Hh868178)
* [//Build 2015-Video: Entwickeln von druckfähigen Apps in Windows 10](https://channel9.msdn.com/Events/Build/2015/2-94)
* [UWP-Druckbeispiel](http://go.microsoft.com/fwlink/p/?LinkId=619984)



<!--HONumber=May16_HO2-->


