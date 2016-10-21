---
author: normesta
ms.assetid: AC96F645-1BDE-4316-85E0-2FBDE0A0A62A
title: Abrufen von Dateieigenschaften
description: "Es werden Eigenschaften&\\#8212;oberste Ebene, grundlegend und erweitert&\\#8212;für eine Datei abgerufen, die durch ein StorageFile-Objekt dargestellt wird."
translationtype: Human Translation
ms.sourcegitcommit: de0b23cfd8f6323d3618c3424a27a7d0ce5e1374
ms.openlocfilehash: 78fdc200b134525fc0445af64e73b184b49ef2a3

---
# Abrufen von Dateieigenschaften

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


** Wichtige APIs **

-   [**StorageFile.GetBasicPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh701737)
-   [**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225)
-   [**StorageItemContentProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh770652)

Es werden Eigenschaften – oberste Ebene, grundlegend und erweitert – für eine Datei abgerufen, die durch ein [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Objekt dargestellt wird.

**Hinweis**  Weitere Informationen finden Sie im [Beispiel zum Dateizugriff](http://go.microsoft.com/fwlink/p/?linkid=619995).

 


## Voraussetzungen

-   **Verstehen der asynchronen Programmierung für UWP-Apps (Universelle Windows-Plattform)**

    Informationen zum Schreiben von asynchronen Apps in C# oder Visual Basic finden Sie unter [Aufrufen asynchroner APIs in C# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Zugriffsberechtigungen für den Speicherort**

    Der Code in diesen Beispielen erfordert beispielsweise den Zugriff auf die **picturesLibrary**-Funktion, während Ihr Speicherort einen anderen Zugriffstyp oder keinen Zugriff voraussetzt. Weitere Informationen finden Sie unter [Berechtigungen für den Dateizugriff](file-access-permissions.md).

## Abrufen von Eigenschaften der obersten Ebene einer Datei

Auf viele Dateieigenschaften der obersten Ebene kann in Form von Membern der [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Klasse zugegriffen werden. Diese Eigenschaften enthalten für eine Datei Attribute, Inhaltstyp, Erstellungsdatum, Anzeigename, Dateityp usw.

**Hinweis**  Denken Sie daran, die **picturesLibrary**-Funktion anzugeben.

 

In diesem Beispiel werden alle Dateien der Bildbibliothek aufgezählt, wobei für jede Datei auf einige Eigenschaften der obersten Ebene zugegriffen wird.

```csharp
// Enumerate all files in the Pictures library.
var folder = Windows.Storage.KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Get top-level file properties.
    fileProperties.AppendLine("File name: " + file.Name);
    fileProperties.AppendLine("File type: " + file.FileType);
}
```

## Abrufen von grundlegenden Eigenschaften einer Datei

Viele grundlegende Dateieigenschaften werden abgerufen, indem zuerst die [**StorageFile.GetBasicPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh701737)-Methode aufgerufen wird. Diese Methode gibt ein [**BasicProperties**](https://msdn.microsoft.com/library/windows/apps/br212113)-Objekt zurück, das Eigenschaften für die Größe des Elements (Datei oder Ordner) und das letzte Änderungsdatum des Elements definiert.

In diesem Beispiel werden alle Dateien der Bildbibliothek aufgezählt, wobei für jede Datei auf einige grundlegende Eigenschaften zugegriffen wird.

```csharp
// Enumerate all files in the Pictures library.
var folder = Windows.Storage.KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Get file's basic properties.
    Windows.Storage.FileProperties.BasicProperties basicProperties =
        await file.GetBasicPropertiesAsync();
    string fileSize = string.Format("{0:n0}", basicProperties.Size);
    fileProperties.AppendLine("File size: " + fileSize + " bytes");
    fileProperties.AppendLine("Date modified: " + basicProperties.DateModified);
}
 ```

## Abrufen von erweiterten Eigenschaften einer Datei

Neben den Eigenschaften der obersten Ebene und den grundlegenden Eigenschaften sind dem Inhalt der Datei viele Eigenschaften zugeordnet. Auf diese erweiterten Eigenschaften wird zugegriffen, indem die [**BasicProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br212124)-Methode aufgerufen wird. (Ein [**BasicProperties**](https://msdn.microsoft.com/library/windows/apps/br212113)-Objekt wird durch den Aufruf einer [**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225)-Eigenschaft abgerufen.) Während auf Dateieigenschaften der obersten Ebene und grundlegende Dateieigenschaften in Form von Eigenschaften einer Klasse zugegriffen werden kann – [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) und **BasicProperties**, werden erweiterte Eigenschaften abgerufen, indem eine [IEnumerable](http://go.microsoft.com/fwlink/p/?LinkID=313091) -Sammlung von [String](http://go.microsoft.com/fwlink/p/?LinkID=325032)-Objekten mit den Namen der abzurufenden Eigenschaften an die **BasicProperties.RetrievePropertiesAsync**-Methode übergeben wird. Diese Methode gibt dann eine [IDictionary](http://go.microsoft.com/fwlink/p/?LinkId=325238) -Sammlung zurück. Anschließend werden die einzelnen erweiterten Eigenschaften anhand des Namens oder Indexes aus der Sammlung abgerufen.

In diesem Beispiel werden alle Dateien der Bildbibliothek aufgezählt und die Namen der gewünschten Eigenschaften (**DataAccessed** und **FileOwner**) in einem [List](http://go.microsoft.com/fwlink/p/?LinkID=325246)-Objekt angegeben. Dieses [List](http://go.microsoft.com/fwlink/p/?LinkID=325246)-Objekt wird an [**BasicProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br212124) übergeben, um die Eigenschaften abzurufen. Diese Eigenschaften werden dann anhand des Namens aus dem zurückgegebenen [IDictionary](http://go.microsoft.com/fwlink/p/?LinkId=325238)-Objekt abgerufen.

```csharp
const string dateAccessedProperty = "System.DateAccessed";
const string fileOwnerProperty = "System.FileOwner";

// Enumerate all files in the Pictures library.
var folder = KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Define property names to be retrieved.
    var propertyNames = new List<string>();
    propertyNames.Add(dateAccessedProperty);
    propertyNames.Add(fileOwnerProperty);

    // Get extended properties.
    IDictionary<string, object> extraProperties =
        await file.Properties.RetrievePropertiesAsync(propertyNames);

    // Get date-accessed property.
    var propValue = extraProperties[dateAccessedProperty];
    if (propValue != null)
    {
        fileProperties.AppendLine("Date accessed: " + propValue);
    }

    // Get file-owner property.
    propValue = extraProperties[fileOwnerProperty];
    if (propValue != null)
    {
        fileProperties.AppendLine("File owner: " + propValue);
    }
}
```

 

 



<!--HONumber=Aug16_HO3-->


