---
author: normesta
ms.assetid: 3604524F-112A-474F-B0CA-0726DC8DB885
title: "Ermitteln der Verfügbarkeit von Microsoft OneDrive-Dateien"
description: "Ermitteln Sie mithilfe der StorageFile.IsAvailable-Eigenschaft, ob eine Microsoft OneDrive-Datei verfügbar ist."
translationtype: Human Translation
ms.sourcegitcommit: 82edf9c3ee7f7303788b7a1272ecb261d3748c5a
ms.openlocfilehash: 2ed00b525fd2b7af51da00ad0464e37f1cabd889

---
# Ermitteln der Verfügbarkeit von Microsoft OneDrive-Dateien

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


** Wichtige APIs **

-   [**FileIO-Klasse**](https://msdn.microsoft.com/library/windows/apps/Hh701440)
-   [**StorageFile-Klasse**](https://msdn.microsoft.com/library/windows/apps/BR227171)
-   [**StorageFile.IsAvailable-Eigenschaft**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx)

Ermitteln Sie mithilfe der [**StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx)-Eigenschaft, ob eine Microsoft OneDrive-Datei verfügbar ist.

## Voraussetzungen

-   **Verstehen der asynchronen Programmierung für UWP-Apps (Universelle Windows-Plattform)**

    Informationen zum Schreiben von asynchronen Apps in C# oder Visual Basic finden Sie unter [Aufrufen asynchroner APIs in C# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/Mt187337). Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](https://msdn.microsoft.com/library/windows/apps/Mt187334).

-   **Deklaration der App-Funktionen**

    Weitere Informationen finden Sie unter [Berechtigungen für den Dateizugriff](file-access-permissions.md).

## Verwenden der StorageFile.IsAvailable-Eigenschaft

Benutzer können OneDrive-Dateien als „Offline verfügbar“ (Standardeinstellung) oder „Nur online verfügbar“ kennzeichnen. Diese Funktion bietet Benutzern die Möglichkeit, große Dateien (z. B. Bilder und Videos) in ihren OneDrive-Speicher zu verschieben, als nur online verfügbar zu kennzeichnen und so Speicherplatz auf der Festplatte zu sparen (lokal wird nur eine Datei mit Metadaten gespeichert).

[**StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx) wird verwendet, um zu ermitteln, ob eine Datei zurzeit verfügbar ist. Die folgende Tabelle zeigt den Wert der **StorageFile.IsAvailable**-Eigenschaft in verschiedenen Szenarien.

| Dateityp                              | Online | Getaktetes Netzwerk        | Offline |
|-------------------------------------------|--------|------------------------|---------|
| Lokale Datei                                | True   | True                   | True    |
| Als "Offline verfügbar" gekennzeichnete OneDrive-Datei | True   | True                   | True    |
| Als "Nur online verfügbar" gekennzeichnete OneDrive-Datei       | True   | Basierend auf Benutzereinstellungen | False   |
| Netzwerkdatei                              | True   | Basierend auf Benutzereinstellungen | False   |

 

Die folgenden Schritte zeigen, wie festgestellt wird, ob eine Datei momentan verfügbar ist.

1.  Deklarieren Sie eine für die Bibliothek, auf die Sie zugreifen möchten, geeignete Funktion.
2.  Schließen Sie den [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346)-Namespace ein. Dieser Namespace enthält die Typen zum Verwalten von Dateien, Ordnern und Anwendungseinstellungen. Außerdem enthält er den erforderlichen [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171)-Typ.
3.  Beschaffen Sie ein [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171)-Objekt für die gewünschte Datei bzw. die gewünschten Dateien. Wenn Sie eine Bibliothek aufzählen, können Sie zur Durchführung dieses Schritts die [**StorageFolder.CreateFileQuery**](https://msdn.microsoft.com/library/windows/apps/BR227252)-Methode und dann die [**GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br227276.aspx)-Methode des sich ergebenden [**StorageFileQueryResult**](https://msdn.microsoft.com/library/windows/apps/BR208046)-Objekts aufrufen. Die **GetFilesAsync**-Methode gibt eine [IReadOnlyList](http://go.microsoft.com/fwlink/p/?LinkId=324970)-Sammlung mit **StorageFile**-Objekten zurück.
4.  Nachdem Sie den Zugriff auf ein [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171)-Objekt eingerichtet haben, das die gewünschten Dateien darstellt, spiegelt der Wert der [**StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx)-Eigenschaft wider, ob die Datei verfügbar ist.

Die folgende generische Methode veranschaulicht, wie Sie einen beliebigen Ordner aufzählen und die Sammlung mit [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171)-Objekten für diesen Ordner zurückgeben. Die aufrufende Methode durchläuft dann die zurückgegebene Sammlung und verweist für jede Datei auf die [**StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx)-Eigenschaft.

```CSharp
/// <summary>
/// Generic function that retrieves all files from the specified folder.
/// </summary>
/// <param name="folder">The folder to be searched.</param>
/// <returns>An IReadOnlyList collection containing the file objects.</returns>
async Task<System.Collections.Generic.IReadOnlyList<StorageFile>> GetLibraryFilesAsync(StorageFolder folder)
{
    var query = folder.CreateFileQuery();
    return await query.GetFilesAsync();
}

...

private async void CheckAvailabilityOfFilesInPicturesLibrary()
{
    // Determine availability of all files within Pictures library.
    var files = await GetLibraryFilesAsync(KnownFolders.PicturesLibrary);
    for (int i = 0; i < files.Count; i++)
    {
        StorageFile file = files[i];

        StringBuilder fileInfo = new StringBuilder();
        fileInfo.AppendFormat("{0} (on {1}) is {2}",
                    file.Name,
                    file.Provider.DisplayName,
                    file.IsAvailable ? "available" : "not available");
    }
}
```

 

 



<!--HONumber=Aug16_HO3-->


