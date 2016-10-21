---
author: normesta
ms.assetid: 1AE29512-7A7D-4179-ADAC-F02819AC2C39
title: Dateien und Ordner in den Musik-, Bild- und Videobibliotheken
description: "Fügen Sie vorhandene Musik-, Bilder- oder Video-Ordner den entsprechenden Bibliotheken hinzu. Sie können auch Ordner aus Bibliotheken entfernen, die Liste der Ordner in einer Bibliothek abrufen und gespeicherte Fotos, Musik und Videos untersuchen."
translationtype: Human Translation
ms.sourcegitcommit: affe6002e22bd10e714dc4782a60ef528c31a407
ms.openlocfilehash: def1c5c8d9d062a81731744e1e1465472225494a

---

# Dateien und Ordner in den Musik-, Bild- und Videobibliotheken


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Fügen Sie vorhandene Musik-, Bilder- oder Video-Ordner den entsprechenden Bibliotheken hinzu. Sie können auch Ordner aus Bibliotheken entfernen, die Liste der Ordner in einer Bibliothek abrufen und gespeicherte Fotos, Musik und Videos untersuchen.

Eine Bibliothek ist eine virtuelle Sammlung von Ordnern, die standardmäßig einen bekannten Ordner sowie alle anderen Ordner enthält, die der Benutzer mithilfe Ihrer App oder einer der integrierten Apps zur Bibliothek hinzugefügt hat. Die Bildbibliothek enthält z.B. standardmäßig den bekannten Ordner „Bilder“. Der Benutzer kann mithilfe Ihrer App oder der integrierten Fotos-App Ordner zur Bildbibliothek hinzufügen oder aus ihr entfernen.

## Voraussetzungen


-   **Verstehen der asynchronen Programmierung für UWP-Apps (Universelle Windows-Plattform)**

    Informationen zum Schreiben von asynchronen Apps in C# oder Visual Basic finden Sie unter [Aufrufen asynchroner APIs in C# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Zugriffsberechtigungen für den Speicherort**

    Öffnen Sie die App-Manifestdatei im Manifest-Designer in Visual Studio. Wählen Sie auf der Seite **Funktionen** die Bibliotheken aus, die Ihre App verwaltet.

    -   **Musikbibliothek**
    -   **Bildbibliothek**
    -   **Videobibliothek**

    Weitere Informationen finden Sie unter [Berechtigungen für den Dateizugriff](file-access-permissions.md).

## Abrufen eines Verweises auf eine Bibliothek


**Hinweis**  Denken Sie daran, die entsprechende Funktion zu deklarieren.
 

Rufen Sie die [**StorageLibrary.GetLibraryAsync**](https://msdn.microsoft.com/library/windows/apps/dn251725)-Methode auf, um einen Verweis auf die Musik-, Bild- oder Videobibliothek des Benutzers zu erhalten. Geben Sie den entsprechenden Wert der [**KnownLibraryId**](https://msdn.microsoft.com/library/windows/apps/dn298399)-Enumeration ein.

-   [**KnownLibraryId.Music**](https://msdn.microsoft.com/library/windows/apps/br227155)
-   [**KnownLibraryId.Pictures**](https://msdn.microsoft.com/library/windows/apps/br227156)
-   [**KnownLibraryId.Videos**](https://msdn.microsoft.com/library/windows/apps/br227159)

```CSharp
    var myPictures = await Windows.Storage.StorageLibrary.GetLibraryAsync
        (Windows.Storage.KnownLibraryId.Pictures);
```

## Abrufen der Liste der Ordner in einer Bibliothek


Um die Liste der Ordner in einer Bibliothek abzurufen, rufen den Wert der [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724)-Eigenschaft ab.

```CSharp
    using Windows.Foundation.Collections;

    // ...

    IObservableVector<Windows.Storage.StorageFolder> myPictureFolders = myPictures.Folders;
```

## Abrufen des Ordners in einer Bibliothek, in den neue Dateien standardmäßig gespeichert werden


Um den Ordner in einer Bibliothek abzurufen, in den neue Dateien standardmäßig gespeichert werden, rufen Sie den Wert der [**StorageLibrary.SaveFolder**](https://msdn.microsoft.com/library/windows/apps/dn251728)-Eigenschaft ab.

```CSharp
    Windows.Storage.StorageFolder savePicturesFolder = myPictures.SaveFolder;
```

## Hinzufügen eines vorhandenen Ordners zu einer Bibliothek


Um einen Ordner zu einer Bibliothek hinzuzufügen, rufen Sie [**StorageLibrary.RequestAddFolderAsync**](https://msdn.microsoft.com/library/windows/apps/dn251726) auf. Das Aufrufen dieser Methode führt beispielsweise bei der Bildbibliothek zur Anzeige einer Ordnerauswahl für den Benutzer, die die Schaltfläche zum **Hinzufügen dieses Ordners zu Bildern** umfasst. Wenn der Benutzer einen Ordner auswählt, dann verbleibt er am ursprünglichen Speicherort auf dem Datenträger und wird zu einem Element in der [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724)-Eigenschaft (und in der integrierten Fotos-App). Der Ordner wird aber nicht im Datei-Explorer als untergeordnetes Element des Ordners „Bilder“ angezeigt.


```CSharp
    Windows.Storage.StorageFolder newFolder = await myPictures.RequestAddFolderAsync();
```

## Entfernen eines Ordners aus einer Bibliothek


Rufen Sie zum Entfernen eines Ordners aus einer Bibliothek die [**StorageLibrary.RequestRemoveFolderAsync**](https://msdn.microsoft.com/library/windows/apps/dn251727)-Methode auf, und geben Sie den zu entfernenden Ordner an. Sie können [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724) und ein [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878)-Steuerelement (oder ähnliches) verwenden, damit der Benutzer einen zu entfernenden Ordner auswählen kann.

Wenn Sie [**StorageLibrary.RequestRemoveFolderAsync**](https://msdn.microsoft.com/library/windows/apps/dn251727) aufrufen, wird dem Benutzer ein Bestätigungsdialogfeld angezeigt, das darüber informiert, dass der Ordner nicht länger unter „Bilder“ angezeigt, aber auch nicht gelöscht wird. Dies bedeutet, dass der Ordner am ursprünglichen Speicherort auf dem Datenträger verbleibt, aus der [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724)-Eigenschaft entfernt wird und nicht länger in der integrierten Fotos-App enthalten ist.

Im folgenden Beispiel wird davon ausgegangen, dass der Benutzer den zu entfernenden Ordner aus einem [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878)-Steuerelement namens **lvPictureFolders** ausgewählt hat.


```CSharp
    bool result = await myPictures.RequestRemoveFolderAsync(folder);
```

## Empfangen von Benachrichtigungen über Änderungen an der Liste der Ordner in einer Bibliothek


Um über Änderungen an der Liste der Ordner in einer Bibliothek benachrichtigt zu werden, registrieren Sie einen Handler für das [**StorageLibrary.DefinitionChanged**](https://msdn.microsoft.com/library/windows/apps/dn251723)-Ereignis der Bibliothek.


```CSharp
    myPictures.DefinitionChanged += MyPictures_DefinitionChanged;
    // ...

void HandleDefinitionChanged(Windows.Storage.StorageLibrary sender, object args)
{
    // ...
}
```

## Medienbibliothekordner


Ein Gerät bietet fünf vordefinierte Speicherorte, an denen Benutzer und Apps Mediendateien speichern können. Vorinstallierte Apps speichern an diesen Speicherorten sowohl von Benutzern erstellte Medien als auch heruntergeladene Medien.

Die Speicherorte lauten:

-   Ordner **Bilder**. Enthält Bilder.

    -   Ordner **Eigene Aufnahmen**. Enthält Fotos und Videos, die mit der integrierten Kamera aufgenommen wurden.

    -   Ordner **Gespeicherte Bilder**. Enthält Bilder, die der Benutzer aus anderen Apps gespeichert hat.

-   Ordner **Musik**. Enthält Songs, Podcasts und Hörbücher.

-   Ordner **Video**. Enthält Videos.

Mediendateien können von Benutzern und Apps auch außerhalb der Medienbibliothekordner auf der SD-Karte gespeichert werden. Durchsuchen Sie den Inhalt der SD-Karte, um eine Mediendatei auf der SD-Karte auf zuverlässige Weise zu finden, oder bitten Sie den Benutzer, mithilfe einer Dateiauswahl auf die Datei zuzugreifen. Weitere Informationen finden Sie unter [Zugreifen auf die SD-Karte](access-the-sd-card.md).

## Abfragen der Medienbibliotheken

Um eine Sammlung von Dateien zu erhalten, geben Sie die Bibliothek und den Typ der gewünschten Dateien an.

```cs
...
using Windows.Storage;
using Windows.Storage.Search;
...

private async void getSongs()
{
    QueryOptions queryOption = new QueryOptions
        (CommonFileQuery.OrderByTitle, new string[] { ".mp3", ".mp4", ".wma" });

    queryOption.FolderDepth = FolderDepth.Deep

    Queue<IStorageFolder> folders = new Queue<IStorageFolder>();

    var files = await KnownFolders.MusicLibrary.CreateFileQueryWithOptions
      (queryOption).GetFilesAsync();

    foreach (var file in files)
    {
        // do something with the music files.
    }

}
```

### Abfrageergebnisse umfassen sowohl internen Speicher als auch Wechselmedien

Benutzer können angeben, dass Dateien standardmäßig auf der optionalen SD-Karte gespeichert werden sollen. Für Apps kann festgelegt werden, dass Dateien nicht auf der SD-Karte gespeichert werden sollen. Daher können die Medienbibliotheken auf den internen Speicher und die SD-Karte des Geräts aufgeteilt werden.

Sie müssen keinen zusätzlichen Code schreiben, um diese Möglichkeit bereitzustellen. In den Methoden im [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346)-Namespace, mit denen bekannte Ordner abgefragt werden, sind die Abfrageergebnisse beider Speicherorte transparent kombiniert. Sie müssen die **removableStorage**-Funktion in der App-Manifestdatei nicht angeben, um diese kombinierten Ergebnisse zu erhalten.

Beachten Sie den Status des Gerätespeichers, der in der folgenden Abbildung dargestellt ist:

![Bilder auf dem Smartphone und der SD-Karte](images/phone-media-locations.png)

Wenn Sie den Inhalt der Bildbibliothek abfragen, indem Sie `await KnownFolders.PicturesLibrary.GetFilesAsync()` aufrufen, ist sowohl „internalPic.jpg“ als auch „SDPic.jpg“ in den Ergebnissen enthalten.


## Verwenden von Fotos


Auf Geräten, auf denen jedes Bild von der Kamera sowohl in niedriger als auch in hoher Auflösung gespeichert wird, wird bei tiefen Abfragen nur das Bild mit niedriger Auflösung zurückgegeben.

Für die Ordner „Eigene Aufnahmen“ und „Gespeicherte Bilder“ werden keine tiefen Abfragen unterstützt.

**Öffnen eines Fotos in der App, mit der es aufgenommen wurde**

Wenn Sie es Benutzern ermöglichen möchten, ein Foto später erneut in der App zu öffnen, mit der es aufgenommen wurde, können Sie die **CreatorAppId** mit den Metadaten des Fotos speichern. Verwenden Sie hierzu ähnlichen Code wie im folgenden Beispiel. In diesem Beispiel ist **testPhoto** eine [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171).

```CSharp
  IDictionary<string, object> propertiesToSave = new Dictionary<string, object>();

  propertiesToSave.Add("System.CreatorOpenWithUIOptions", 1);
  propertiesToSave.Add("System.CreatorAppId", appId);

  testPhoto.Properties.SavePropertiesAsync(propertiesToSave).AsyncWait();   
```

## Verwenden von Datenstrommethoden zum Hinzufügen einer Datei zur Medienbibliothek


Wenn Sie auf eine Medienbibliothek zugreifen, indem Sie einen bekannten Ordner wie **KnownFolders.PictureLibrary** verwenden, und der Medienbibliothek mithilfe von Datenstrommethoden eine Datei hinzufügen, müssen Sie darauf achten, alle von Ihrem Code geöffneten Datenströme wieder zu schließen. Andernfalls wird die Datei der Medienbibliothek mit diesen Methoden nicht wie erwartet hinzugefügt, weil mindestens ein Stream weiterhin über einen Handle für die Datei verfügt.

Wenn Sie beispielsweise den folgenden Code ausführen, wird die Datei nicht der Medienbibliothek hinzugefügt. In der Codezeile `using (var destinationStream = (await destinationFile.OpenAsync(FileAccessMode.ReadWrite)).GetOutputStreamAt(0))` wird sowohl mit der **OpenAsync**-Methode als auch mit der **GetOutputStreamAt**-Methode ein Datenstrom geöffnet. Es wird aber nur der mit der **GetOutputStreamAt**-Methode geöffnete Datenstrom aufgrund der **using**-Anweisung wieder geschlossen. Der andere Datenstrom bleibt geöffnet und verhindert das Speichern der Datei.

```CSharp
StorageFolder testFolder = await StorageFolder.GetFolderFromPathAsync(@"C:\test");
StorageFile sourceFile = await testFolder.GetFileAsync("TestImage.jpg");
StorageFile destinationFile = await KnownFolders.CameraRoll.CreateFileAsync("MyTestImage.jpg");
using (var sourceStream = (await sourceFile.OpenReadAsync()).GetInputStreamAt(0))
{
    using (var destinationStream = (await destinationFile.OpenAsync(FileAccessMode.ReadWrite)).GetOutputStreamAt(0))
    {
        await RandomAccessStream.CopyAndCloseAsync(sourceStream, destinationStream);
    }
}

```

Um Streammethoden erfolgreich zum Hinzufügen einer Datei zur Medienbibliothek zu verwenden, müssen Sie alle Datenströme schließen, die von Ihrem Code geöffnet werden. Dies ist im folgenden Beispiel dargestellt.

```CSharp
StorageFolder testFolder = await StorageFolder.GetFolderFromPathAsync(@"C:\test");
StorageFile sourceFile = await testFolder.GetFileAsync("TestImage.jpg");
StorageFile destinationFile = await KnownFolders.CameraRoll.CreateFileAsync("MyTestImage.jpg");

using (var sourceStream = await sourceFile.OpenReadAsync())
{
    using (var sourceInputStream = sourceStream.GetInputStreamAt(0))
    {
        using (var destinationStream = await destinationFile.OpenAsync(FileAccessMode.ReadWrite))
        {
            using (var destinationOutputStream = destinationStream.GetOutputStreamAt(0))
            {
                await RandomAccessStream.CopyAndCloseAsync(sourceInputStream, destinationStream);
            }
        }
    }
}
```

 

 



<!--HONumber=Aug16_HO3-->


