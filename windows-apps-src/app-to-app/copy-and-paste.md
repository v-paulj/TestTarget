---
description: In diesem Artikel wird erläutert, wie in Apps für die universelle Windows-Plattform (UWP) das Kopieren und Einfügen über die Zwischenablage unterstützt wird.
title: Kopieren und Einfügen
ms.assetid: E882DC15-E12D-4420-B49D-F495BB484BEE
author: awkoren
---
#Kopieren und Einfügen

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


In diesem Artikel wird erläutert, wie das Kopieren und Einfügen mit der Zwischenablage in Apps der universellen Windows-Plattform unterstützt wird. Kopieren und Einfügen ist die klassische Methode zum Austausch von Daten zwischen Apps oder in einer App, und nahezu jede App kann Zwischenablageaktionen bis zu einem gewissen Grad unterstützen.

## Überprüfen der integrierten Unterstützung für die Zwischenablage


In vielen Fällen müssen Sie keinen Code für die Unterstützung von Zwischenablageaktionen schreiben. Viele der Standard-XAML-Steuerelemente, die Sie beim Erstellen Ihrer Apps verwenden können, unterstützen bereits Zwischenablageaktionen. Weitere Informationen zu den verfügbaren Steuerelementen finden Sie in der [Steuerelementliste][ControlsList].

## Vorbereitung

Schließen Sie zunächst den [**Windows.ApplicationModel.DataTransfer**][DataTransfer]-Namespace in Ihre App ein. Fügen Sie dann eine Instanz des [**DataPackage**][DataPackage]-Objekts hinzu. Dieses Objekt enthält sowohl die vom Benutzer kopierten Daten als auch alle Eigenschaften (z. B. eine Beschreibung), die Sie einfügen möchten.

<!-- For some reason, the snippets in this file are all inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
DataPackage dataPackage = new DataPackage();
```

## Kopieren und Ausschneiden

Kopieren und Ausschneiden (auch als Verschieben bezeichnet) funktionieren nahezu identisch. Wählen Sie mit der [**DataPackage.RequestedOperation**][RequestedOperation]-Eigenschaft den gewünschten Vorgang aus.

```cs
// copy 
dataPackage.RequestedOperation = DataPackageOperation.Copy;
// or cut
dataPackage.RequestedOperation = DataPackageOperation.Move;
```

Anschließend können Sie die vom Benutzer ausgewählten Daten in das [**DataPackage**][DataPackage]-Objekt einfügen. Wenn die Daten von der **DataPackage**-Klasse unterstützt werden, können Sie die entsprechenden Methoden aus dem **DataPackage**-Objekt verwenden. Gehen Sie zum Hinzufügen von Text wie folgt vor:

```cs
dataPackage.SetText("Hello World!");
```

Im letzten Schritt wird das [**DataPackage**][DataPackage]-Objekt durch Aufrufen der statischen [**Clipboard.SetContent**][SetContent]-Methode zur Zwischenablage hinzugefügt.

```cs
Clipboard.SetContent(dataPackage);
```
## Einfügen

Rufen Sie zum Abrufen des Inhalts der Zwischenablage die statische [**Clipboard.GetContent**[GetContent]-Methode auf. Die Methode gibt eine [**DataPackageView**][DataPackageView] mit dem Inhalt zurück. Dieses Objekt ist mit dem [**DataPackage**][DataPackage]-Objekt nahezu identisch, der Inhalt ist allerdings schreibgeschützt. Anhand dieses Objekts können Sie dann entweder mit der [**AvailableFormats**][AvailableFormats]-Methode oder mit der [**Contains**][Contains]-Methode ermitteln, welche Formate verfügbar sind. Rufen Sie anschließend die entsprechende **DataPackageView**-Methode auf, um die Daten abzurufen.

```cs
DataPackageView dataPackageView = Clipboard.GetContent();
if (dataPackageView.Contains(StandardDataFormats.Text))
{
    string text = await dataPackageView.GetTextAsync();
    // To output the text from this example, you need a TextBlock control
    TextOutput.Text = "Clipboard now contains: " + text;
}
```

## Nachverfolgen von Änderungen an der Zwischenablage

Zusätzlich zu den Befehlen zum Kopieren und Einfügen empfiehlt es sich unter Umständen, auch Änderungen an der Zwischenablage nachzuverfolgen. Behandeln Sie dafür das [**Clipboard.ContentChanged**][ContentChanged]-Ereignis der Zwischenablage.

```cs
Clipboard.ContentChanged += (s, e) => 
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        string text = await dataPackageView.GetTextAsync();
        // To output the text from this example, you need a TextBlock control
        TextOutput.Text = "Clipboard now contains: " + text;
    }
}
```

<!-- LINKS --> 
[DataTransfer]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.aspx 
[DataPackage]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx 
[DataPackageView]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.aspx
[DataPackagePropertySet]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx 
[DataRequest]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.aspx 
[DataRequested]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx 
[FailWithDisplayText]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext.aspx
[ShowShareUi]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.showshareui.aspx
[RequestedOperation]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.requestedoperation.aspx 
[ControlsList]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/mt185406.aspx 
[SetContent]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.setcontent.aspx 
[GetContent]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.getcontent.aspx
[AvailableFormats]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.availableformats.aspx 
[Contains]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.contains.aspx
[ContentChanged]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.contentchanged.aspx 



<!--HONumber=Mar16_HO5-->


