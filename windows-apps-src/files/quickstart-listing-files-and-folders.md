---
author: TylerMSFT
ms.assetid: 4C59D5AC-58F7-4863-A884-E9E54228A5AD
title: "Aufzählen und Abfragen von Dateien und Ordnern"
description: "Greifen Sie auf Dateien und Ordner zu, die sich in einem Ordner, in einer Bibliothek, auf einem Gerät oder an einer Netzwerkadresse befinden. Sie können auch durch Erstellen von Datei- und Ordnerabfragen Dateien und Ordner an bestimmten Speicherorten abrufen."
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 6ecad1bbd3c08dcd7aa1d3b82739931f20fc4ee2

---
# Aufzählen und Abfragen von Dateien und Ordnern


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Greifen Sie auf Dateien und Ordner zu, die sich in einem Ordner, in einer Bibliothek, auf einem Gerät oder an einer Netzwerkadresse befinden. Sie können auch durch Erstellen von Datei- und Ordnerabfragen Dateien und Ordner an bestimmten Speicherorten abrufen.


            **Hinweis**  Weitere Informationen finden Sie im [Beispiel für Ordnerenumeration](http://go.microsoft.com/fwlink/p/?linkid=619993).

 
## Voraussetzungen

-   **Verstehen der asynchronen Programmierung für UWP-Apps (Universelle Windows-Plattform)**

    Informationen zum Schreiben von asynchronen Apps in C# oder Visual Basic finden Sie unter [Aufrufen asynchroner APIs in C# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Zugriffsberechtigungen für den Speicherort**

    Der Code in diesen Beispielen erfordert beispielsweise den Zugriff auf die **picturesLibrary**-Funktion, während Ihr Speicherort einen anderen Zugriffstyp oder keinen Zugriff voraussetzt. Weitere Informationen finden Sie unter [Berechtigungen für den Dateizugriff](file-access-permissions.md).

## Auflisten der Dateien und Ordner an einem Speicherort

> 
            **Hinweis**  Denken Sie daran, die **picturesLibrary**-Funktion anzugeben.

In diesem Beispiel verwenden wir zunächst die [**StorageFolder.GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br227276)-Methode, um alle Dateien im Stammordner der [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) (nicht in den Unterordnern) abzurufen und die Namen der einzelnen Dateien aufzulisten. Als Nächstes verwenden wir die [**GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br227280)-Methode, um alle Unterordner in der **PicturesLibrary** abzurufen und die Namen der einzelnen Unterordner aufzulisten.

<!--BUGBUG: IAsyncOperation<IVectorView<StorageFolder^>^>^  causes build to flake out-->
> [!div class="tabbedCodeSnippets"] 
> ```cpp
> //#include <ppltasks.h>
> //#include <string>
> //#include <memory>
> using namespace Windows::Storage;
> using namespace Platform::Collections;
> using namespace concurrency;
> using namespace std;
> 
> // Be sure to specify the Pictures Folder capability in the appxmanifext file.
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
> 
> // Use a shared_ptr so that the string stays in memory
> // until the last task is complete.
> auto outputString = make_shared<wstring>();
> *outputString += L"Files:\n";
> 
> // Get a read-only vector of the file objects
> // and pass it to the continuation. 
> create_task(picturesFolder->GetFilesAsync())        
>    // outputString is captured by value, which creates a copy 
>    // of the shared_ptr and increments its reference count.
>    .then ([outputString] (IVectorView\<StorageFile^>^ files)
>    {        
>        for ( unsigned int i = 0 ; i < files->Size; i++)
>        {
>            *outputString += files->GetAt(i)->Name->Data();
>            *outputString += L"\n";
>       }
>    })
>    // We need to explicitly state the return type 
>    // here: -> IAsyncOperation<...>
>    .then([picturesFolder]() -> IAsyncOperation\<IVectorView\<StorageFolder^>^>^ 
>    {
>        return picturesFolder->GetFoldersAsync();
>    })
>    // Capture "this" to access m_OutputTextBlock from within the lambda.
>    .then([this, outputString](IVectorView\<StorageFolder^>^ folders)
>    {        
>        *outputString += L"Folders:\n";
> 
>        for ( unsigned int i = 0; i < folders->Size; i++)
>        {
>           *outputString += folders->GetAt(i)->Name->Data();
>           *outputString += L"\n";
>        }
> 
>        // Assume m_OutputTextBlock is a TextBlock defined in the XAML.
>        m_OutputTextBlock->Text = ref new String((*outputString).c_str());
>     });
> ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> StringBuilder outputText = new StringBuilder();
> 
> IReadOnlyList<StorageFile> fileList = 
>     await picturesFolder.GetFilesAsync();
> 
> outputText.AppendLine("Files:");
> foreach (StorageFile file in fileList)
> {
>     outputText.Append(file.Name + "\n");
> }
> 
> IReadOnlyList<StorageFolder> folderList = 
>     await picturesFolder.GetFoldersAsync();
>            
> outputText.AppendLine("Folders:");
> foreach (StorageFolder folder in folderList)
> {
>     outputText.Append(folder.DisplayName + "\n");
> }
> ```
> ```vb
> Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
> Dim outputText As New StringBuilder
> 
> Dim fileList As IReadOnlyList(Of StorageFile) =
>     Await picturesFolder.GetFilesAsync()
> 
> outputText.AppendLine("Files:")
> For Each file As StorageFile In fileList
> 
>     outputText.Append(file.Name & vbLf)
> 
> Next file
> 
> Dim folderList As IReadOnlyList(Of StorageFolder) =
>     Await picturesFolder.GetFoldersAsync()
> 
> outputText.AppendLine("Folders:")
> For Each folder As StorageFolder In folderList
> 
>     outputText.Append(folder.DisplayName & vbLf)
> 
> Next folder
> ```


> [!div class="tabbedCodeSnippets"]
 


            **Hinweis**  Denken Sie in C# oder Visual Basic daran, das **async**-Schlüsselwort in der Methodendeklaration aller Methoden anzugeben, in denen Sie den **await**-Operator verwenden. Alternativ können Sie die [**GetItemsAsync**](https://msdn.microsoft.com/library/windows/apps/br227286)-Methode verwenden, um alle Elemente (Dateien und Ordner) an einem bestimmten Speicherort abzurufen. Im folgenden Beispiel wird die **GetItemsAsync**-Methode verwendet, um alle Dateien und Unterordner im Stammordner der [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) abzurufen (nicht in den Unterordnern). Anschließend werden die Namen der einzelnen Dateien und Unterordner aufgelistet.

> [!div class="tabbedCodeSnippets"] 
> ```cpp
> // See previous example for comments, namespace and #include info.
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
> auto outputString = make_shared<wstring>();
> 
> create_task(picturesFolder->GetItemsAsync())        
>     .then ([this, outputString] (IVectorView<IStorageItem^>^ items)
> {        
>     for ( unsigned int i = 0 ; i < items->Size; i++)
>     {
>         *outputString += items->GetAt(i)->Name->Data();
>         if(items->GetAt(i)->IsOfType(StorageItemTypes::Folder))
>         {
>             *outputString += L"  folder\n";
>         }
>         else
>         {
>             *outputString += L"\n";
>         }
>         m_OutputTextBlock->Text = ref new String((*outputString).c_str());
>     }
> });
> ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> StringBuilder outputText = new StringBuilder();
> 
> IReadOnlyList<IStorageItem> itemsList = 
>     await picturesFolder.GetItemsAsync();
> 
> foreach (var item in itemsList)
> {
>     if (item is StorageFolder)
>     {
>         outputText.Append(item.Name + " folder\n");
> 
>     }
>     else
>     {
>         outputText.Append(item.Name + "\n");
> 
>     }
> }
> ```
> ```vb
> Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
> Dim outputText As New StringBuilder
> 
> Dim itemsList As IReadOnlyList(Of IStorageItem) =
>     Await picturesFolder.GetItemsAsync()
> 
> For Each item In itemsList
> 
>     If TypeOf item Is StorageFolder Then
> 
>         outputText.Append(item.Name & " folder" & vbLf)
> 
>     Else
> 
>         outputText.Append(item.Name & vbLf)
> 
>     End If
> 
> Next item
> ```

## Wenn das Element ein Unterordner ist, wird dem Namen die Zeichenfolge `"folder"` angefügt.

[!div class="tabbedCodeSnippets"] Abfragen von Dateien aus einem Speicherort und Auflisten der entsprechenden Dateien In diesem Beispiel erfolgt eine Abfrage nach allen Dateien in der [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156), die nach Monat gruppiert werden, wobei das Beispiel dieses Mal auch die Unterordner rekursiv durchsucht.

Zunächst wird [**StorageFolder.CreateFolderQuery**](https://msdn.microsoft.com/library/windows/apps/br227262) aufgerufen und der [**CommonFolderQuery.GroupByMonth**](https://msdn.microsoft.com/library/windows/apps/br207957)-Wert an die Methode übergeben. Dadurch erhalten wir ein [**StorageFolderQueryResult**](https://msdn.microsoft.com/library/windows/apps/br208066)-Objekt.

> [!div class="tabbedCodeSnippets"] 
> ```cpp
> //#include <ppltasks.h>
> //#include <string>
> //#include <memory>
> using namespace Windows::Storage;
> using namespace Windows::Storage::Search;
> using namespace concurrency;
> using namespace Platform::Collections;
> using namespace Windows::Foundation::Collections;
> using namespace std;
> 
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
> 
> StorageFolderQueryResult^ queryResult = 
>     picturesFolder->CreateFolderQuery(CommonFolderQuery::GroupByMonth);
> 
> // Use shared_ptr so that outputString remains in memory
> // until the task completes, which is after the function goes out of scope.
> auto outputString = std::make_shared<wstring>();
> 
> create_task( queryResult->GetFoldersAsync()).then([this, outputString] (IVectorView<StorageFolder^>^ view) 
> {        
>     for ( unsigned int i = 0; i < view->Size; i++)
>     {
>         create_task(view->GetAt(i)->GetFilesAsync()).then([this, i, view, outputString](IVectorView<StorageFile^>^ files)
>         {
>             *outputString += view->GetAt(i)->Name->Data();
>             *outputString += L"(";
>             *outputString += to_wstring(files->Size);
>             *outputString += L")\r\n";
>             for (unsigned int j = 0; j < files->Size; j++)
>             {
>                 *outputString += L"     ";
>                 *outputString += files->GetAt(j)->Name->Data();
>                 *outputString += L"\r\n";
>             }
>         }).then([this, outputString]()
>         {
>             m_OutputTextBlock->Text = ref new String((*outputString).c_str());
>         });
>     }    
> });
> ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> 
> StorageFolderQueryResult queryResult = 
>     picturesFolder.CreateFolderQuery(CommonFolderQuery.GroupByMonth);
>         
> IReadOnlyList<StorageFolder> folderList = 
>     await queryResult.GetFoldersAsync();
> 
> StringBuilder outputText = new StringBuilder();
> 
> foreach (StorageFolder folder in folderList)
> {
>     IReadOnlyList<StorageFile> fileList = await folder.GetFilesAsync();
> 
>     // Print the month and number of files in this group.
>     outputText.AppendLine(folder.Name + " (" + fileList.Count + ")");
> 
>     foreach (StorageFile file in fileList)
>     {
>         // Print the name of the file.
>         outputText.AppendLine("   " + file.Name);
>     }
> }
> ```
> ```vb
> Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
> Dim outputText As New StringBuilder
> 
> Dim queryResult As StorageFolderQueryResult =
>     picturesFolder.CreateFolderQuery(CommonFolderQuery.GroupByMonth)
> 
> Dim folderList As IReadOnlyList(Of StorageFolder) =
>     Await queryResult.GetFoldersAsync()
> 
> For Each folder As StorageFolder In folderList
> 
>     Dim fileList As IReadOnlyList(Of StorageFile) =
>         Await folder.GetFilesAsync()
> 
>     ' Print the month and number of files in this group.
>     outputText.AppendLine(folder.Name & " (" & fileList.Count & ")")
> 
>     For Each file As StorageFile In fileList
> 
>         ' Print the name of the file.
>         outputText.AppendLine("   " & file.Name)
> 
>     Next file
> 
> Next folder
> ```

Als Nächstes wird [**StorageFolderQueryResult.GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br208074) aufgerufen, das [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)-Objekte zurückgibt, die virtuelle Ordner darstellen.

``` syntax
July ‎2015 (2)
   MyImage3.png
   MyImage4.png
‎December ‎2014 (2)
   MyImage1.png
   MyImage2.png
```




<!--HONumber=Jun16_HO4-->


