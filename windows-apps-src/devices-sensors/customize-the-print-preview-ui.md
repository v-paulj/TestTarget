---
author: DBirtolo
ms.assetid: 88132B6F-FB50-4B03-BC21-233988746230
title: "Anpassen der Benutzeroberfläche für die Druckvorschau"
description: "In diesem Abschnitt wird beschrieben, wie die Druckoptionen und -einstellungen in der Benutzeroberfläche für die Druckvorschau angepasst werden."
translationtype: Human Translation
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: dd64266c2015e1bb640cf159b0836b9819cf7845

---
# Anpassen der Benutzeroberfläche für die Druckvorschau

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


** Wichtige APIs **

-   [**Windows.Graphics.Printing**](https://msdn.microsoft.com/library/windows/apps/BR226489)
-   [**Windows.UI.Xaml.Printing**](https://msdn.microsoft.com/library/windows/apps/BR243325)
-   [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426)

In diesem Abschnitt wird beschrieben, wie die Druckoptionen und -einstellungen in der Benutzeroberfläche für die Druckvorschau angepasst werden. Weitere Informationen zur Druckfunktion finden Sie unter [Drucken in Apps](print-from-your-app.md).

**Tipp** Die Mehrzahl der Beispiele in diesem Thema basiert auf dem Druckbeispiel. Laden Sie das [Druckbeispiel für die universelle Windows-Plattform (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619984) aus dem Repository [Beispiele für Universelle Windows-Plattform](http://go.microsoft.com/fwlink/p/?LinkId=619979) auf GitHub herunter, um den vollständigen Code anzuzeigen.

 

## Anpassen der Druckoptionen

Standardmäßig werden in der Benutzeroberfläche für die Druckvorschau die Druckoptionen [**ColorMode**](https://msdn.microsoft.com/library/windows/apps/BR226478), [**Copies**](https://msdn.microsoft.com/library/windows/apps/BR226479) und [**Orientation**](https://msdn.microsoft.com/library/windows/apps/BR226486) angezeigt. Neben diesen Optionen sind weitere allgemeine Druckeroptionen verfügbar, die Sie der Benutzeroberfläche für die Druckvorschau hinzufügen können:

-   [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR226476)
-   [**Collation**](https://msdn.microsoft.com/library/windows/apps/BR226477)
-   [**Duplex**](https://msdn.microsoft.com/library/windows/apps/BR226480)
-   [**HolePunch**](https://msdn.microsoft.com/library/windows/apps/BR226481)
-   [**InputBin**](https://msdn.microsoft.com/library/windows/apps/BR226482)
-   [**MediaSize**](https://msdn.microsoft.com/library/windows/apps/BR226483)
-   [**MediaType**](https://msdn.microsoft.com/library/windows/apps/BR226484)
-   [**NUp**](https://msdn.microsoft.com/library/windows/apps/BR226485)
-   [**PrintQuality**](https://msdn.microsoft.com/library/windows/apps/BR226487)
-   [**Staple**](https://msdn.microsoft.com/library/windows/apps/BR226488)

Diese Optionen werden in der [**StandardPrintTaskOptions**](https://msdn.microsoft.com/library/windows/apps/BR226475)-Klasse definiert. Sie können in der Optionsliste, die in der Druckvorschau-Benutzeroberfläche angezeigt wird, Optionen hinzufügen oder entfernen. Sie können auch die Reihenfolge, in der die Optionen angezeigt werden, und die für den Benutzer angezeigten Standardeinstellungen ändern.

Die Änderungen, die Sie auf diese Weise vornehmen, betreffen allerdings nur die Druckvorschau-Benutzeroberfläche. Der Benutzer kann stets auf alle vom Drucker unterstützten Optionen zugreifen, indem er in der Druckvorschau-Benutzeroberfläche auf **Weitere Einstellungen** tippt.

**Hinweis** Obwohl Ihre App jede beliebige Druckoption zur Anzeige angeben kann, werden in der Benutzeroberfläche für die Druckvorschau nur die vom ausgewählten Drucker unterstützten Optionen angezeigt. In der Druckbenutzeroberfläche werden keine Optionen angezeigt, die der ausgewählte Drucker nicht unterstützt.

 

### Definieren der anzuzeigenden Optionen

Wenn der Bildschirm der App geladen wird, wird die App für den Vertrag für „Drucken“ registriert. Im Rahmen dieser Registrierung wird der [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597)-Ereignishandler definiert. Der Code zum Anpassen der in der Druckvorschau-Benutzeroberfläche angezeigten Optionen wird dem **PrintTaskRequested**-Ereignishandler hinzugefügt.

Ändern Sie den [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597)-Ereignishandler, um die [**printTask.options**](https://msdn.microsoft.com/library/windows/apps/BR226469)-Anweisungen einzubeziehen, mit denen die Druckeinstellungen konfiguriert werden, die Sie auf der Benutzeroberfläche für die Druckvorschau anzeigen möchten. Überschreiben Sie für den Bildschirm Ihrer App, in dem Sie eine benutzerdefinierte Liste von Druckoptionen anzeigen möchten, den **PrintTaskRequested**-Ereignishandler in der Basisklasse, um Code hinzuzufügen, der die Optionen angibt, die bei der Ausgabe des Bildschirms angezeigt werden sollen.

``` csharp
protected override void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequestedArgs =>
   {
         IList<string> displayedOptions = printTask.Options.DisplayedOptions;

         // Choose the printer options to be shown.
         // The order in which the options are appended determines the order in which they appear in the UI
         displayedOptions.Clear();
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.MediaSize);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Collation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Duplex);

         // Preset the default value of the printer option
         printTask.Options.MediaSize = PrintMediaSize.NorthAmericaLegal;

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

         sourceRequestedArgs.SetSource(printDocumentSource);
   });
}
```

**Wichtig** Durch das Aufrufen von [**displayedOptions.clear**](https://msdn.microsoft.com/library/windows/apps/BR226453)() werden alle Druckoptionen aus der Druckvorschau-Benutzeroberfläche entfernt, einschließlich des Links **Weitere Einstellungen**. Fügen Sie alle Optionen an, die in der Druckvorschau-Benutzeroberfläche angezeigt werden sollen.

### Festlegen der Standardoptionen

Sie können auch die Standardwerte der Optionen in der Druckvorschau-Benutzeroberfläche festlegen. Die folgende Codezeile aus dem letzten Beispiel legt den Standardwert der Option [**MediaSize**](https://msdn.microsoft.com/library/windows/apps/BR226483) fest.

``` csharp
         // Preset the default value of the printer option
         printTask.Options.MediaSize = PrintMediaSize.NorthAmericaLegal;
```         

## Hinzufügen neuer Druckoptionen

In diesem Abschnitt werden die Erstellung einer neuen Druckoption, die Definition einer Liste von Werten, die von dieser Option unterstützt werden, und das Hinzufügen der Option zur Druckvorschau gezeigt. Fügen Sie die Druckoption wie im vorherigen Abschnitt gezeigt im [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597)-Ereignishandler hinzu.

Rufen Sie zunächst ein [**PrintTaskOptionDetails**](https://msdn.microsoft.com/library/windows/apps/Hh701256)-Objekt ab. Dies wird verwendet, um die neue Druckoption zur Benutzeroberfläche für die Druckvorschau hinzuzufügen. Löschen Sie dann die Liste der Optionen, die in der Druckvorschau-Benutzeroberfläche angezeigt werden, und fügen Sie die Optionen hinzu, die angezeigt werden sollen, wenn der Benutzer in der App druckt. Anschließend erstellen Sie die neue Druckoption und initialisieren die Liste der Optionswerte. Abschließend fügen Sie die neue Option hinzu und weisen dem **OptionChanged**-Ereignis einen Handler zu.

``` csharp
protected override void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequestedArgs =>
   {
         PrintTaskOptionDetails printDetailedOptions = PrintTaskOptionDetails.GetFromPrintTaskOptions(printTask.Options);
         IList<string> displayedOptions = printDetailedOptions.DisplayedOptions;

         // Choose the printer options to be shown.
         // The order in which the options are appended determines the order in which they appear in the UI
         displayedOptions.Clear();

         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.ColorMode);

         // Create a new list option
         PrintCustomItemListOptionDetails pageFormat = printDetailedOptions.CreateItemListOption("PageContent", "Pictures");
         pageFormat.AddItem("PicturesText", "Pictures and text");
         pageFormat.AddItem("PicturesOnly", "Pictures only");
         pageFormat.AddItem("TextOnly", "Text only");

         // Add the custom option to the option list
         displayedOptions.Add("PageContent");

         printDetailedOptions.OptionChanged += printDetailedOptions_OptionChanged;

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

         sourceRequestedArgs.SetSource(printDocumentSource);
   });
}
```

Die Optionen werden in der Druckvorschau-Benutzeroberfläche in der Reihenfolge angezeigt, in der sie hinzugefügt werden, wobei die erste Option oben im Fenster erscheint. In diesem Beispiel wird die benutzerdefinierte Option als Letztes hinzugefügt, sodass sie am Ende der Optionsliste erscheint. Sie könnten sie jedoch an einer beliebigen Stelle der Liste platzieren. Benutzerdefinierte Druckoptionen müssen nicht zuletzt hinzugefügt werden.

Wenn der Benutzer die ausgewählte Option in Ihrer benutzerdefinierten Druckoption ändert, aktualisieren Sie das Druckvorschaubild. Rufen Sie die [**InvalidatePreview**](https://msdn.microsoft.com/library/windows/apps/Hh702146)-Methode auf, um das Bild in der Druckvorschau-Benutzeroberfläche neu zu zeichnen, wie unten gezeigt.

``` csharp
async void printDetailedOptions_OptionChanged(PrintTaskOptionDetails sender, PrintTaskOptionChangedEventArgs args)
{
   // Listen for PageContent changes
   string optionId = args.OptionId as string;
   if (string.IsNullOrEmpty(optionId))
   {
         return;
   }

   if (optionId == "PageContent")
   {
         await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
         {
            printDocument.InvalidatePreview();
         });
   }
}
```

## Verwandte Themen

* [Gestaltungsrichtlinien für Druckvorgänge](https://msdn.microsoft.com/library/windows/apps/Hh868178)
* [//Build2015-Video: Entwickeln von druckfähigen Apps in Windows10](https://channel9.msdn.com/Events/Build/2015/2-94)
* [UWP-Druckbeispiel](http://go.microsoft.com/fwlink/p/?LinkId=619984)




<!--HONumber=Aug16_HO3-->


