---
author: TylerMSFT
ms.assetid: 27914C0A-2A02-473F-BDD5-C931E3943AA0
title: Erstellen, Schreiben und Lesen einer Datei
description: Lesen und Schreiben Sie eine Datei mithilfe eines StorageFile-Objekts.
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 067a9fb20c393e6486206a230b882a835264303a

---

# Erstellen, Schreiben und Lesen einer Datei


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**StorageFolder-Klasse**](https://msdn.microsoft.com/library/windows/apps/br227230)
-   [**StorageFile-Klasse**](https://msdn.microsoft.com/library/windows/apps/br227171)
-   [**FileIO-Klasse**](https://msdn.microsoft.com/library/windows/apps/hh701440)

Lesen und Schreiben Sie eine Datei mithilfe eines [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Objekts.

> **Hinweis:**  Siehe auch [Beispiel zum Dateizugriff](http://go.microsoft.com/fwlink/p/?linkid=619995).

## Voraussetzungen

-   **Verstehen der asynchronen Programmierung für UWP-Apps (Universelle Windows-Plattform)**

    Informationen zum Schreiben von asynchronen Apps in C# oder Visual Basic finden Sie unter [Aufrufen asynchroner APIs in C# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Kenntnis, wie die Datei abgerufen wird, aus der Sie lesen und/oder in die Sie schreiben möchten**

    Unter [Öffnen von Dateien und Ordnern mit einer Auswahl](quickstart-using-file-and-folder-pickers.md) erfahren Sie, wie Sie eine Datei mit einer Dateiauswahl abrufen können.

## Erstellen einer Datei

Nachfolgend finden Sie Informationen zum Erstellen einer Datei im lokalen Ordner der App. Wenn sie bereits vorhanden ist, ersetzen Sie sie.
> [!div class="tabbedCodeSnippets"]
```cs
// Create sample file; replace if exists.
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.CreateFileAsync("sample.txt",
        Windows.Storage.CreationCollisionOption.ReplaceExisting);
```
```vb
' Create sample file; replace if exists.
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.CreateFileAsync("sample.txt", CreationCollisionOption.ReplaceExisting)
```

## Schreiben in eine Datei


Im Folgenden finden Sie Informationen zum Schreiben in eine beschreibbare Datei auf dem Datenträger mithilfe der [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Klasse. Der allgemein erste Schritt für die verschiedenen Methoden zum Schreiben in eine Datei ist das Abrufen der Datei mit [**StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272). (Es sei denn, Sie schreiben sofort nach dem Erstellen in die Datei.)
> [!div class="tabbedCodeSnippets"]
```cs
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
```
```vb
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.GetFileAsync("sample.txt")
```

**Schreiben von Text in eine Datei**

Sie schreiben Text in die Datei, indem Sie die [**WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505)-Methode der [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440)-Klasse aufrufen.
> [!div class="tabbedCodeSnippets"]
```cs
await Windows.Storage.FileIO.WriteTextAsync(sampleFile, "Swift as a shadow");
```
```vb
Await Windows.Storage.FileIO.WriteTextAsync(sampleFile, "Swift as a shadow")
```

**Schreiben von Bytes in eine Datei mithilfe eines Puffers (2 Schritte)**

1.  Rufen Sie zuerst [**ConvertStringToBinary**](https://msdn.microsoft.com/library/windows/apps/br241385) auf, um einen Puffer der Bytes (basierend auf einer beliebigen Zeichenfolge) zu erhalten, die Sie in die Datei schreiben möchten.
> [!div class="tabbedCodeSnippets"]
```cs
var buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
        "What fools these mortals be", Windows.Security.Cryptography.BinaryStringEncoding.Utf8);
```
```vb
Dim buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
                    "What fools these mortals be",
                    Windows.Security.Cryptography.BinaryStringEncoding.Utf8)
```

2.  Schreiben Sie dann die Bytes aus dem Puffer in die Datei, indem Sie die [**WriteBufferAsync**](https://msdn.microsoft.com/library/windows/apps/hh701490)-Methode der [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440)-Klasse aufrufen.
> [!div class="tabbedCodeSnippets"]
```cs
await Windows.Storage.FileIO.WriteBufferAsync(sampleFile, buffer);
```
```vb
Await Windows.Storage.FileIO.WriteBufferAsync(sampleFile, buffer)
```

**Schreiben von Text in eine Datei mithilfe eines Datenstroms (4 Schritte)**

1.  Öffnen Sie zunächst die Datei durch Aufrufen der [**StorageFile.OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851)-Methode. Wenn der Vorgang zum Öffnen abgeschlossen ist, wird ein Datenstrom des Dateiinhalts zurückgegeben.
> [!div class="tabbedCodeSnippets"]
```cs
var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
```
```vb
Dim stream = Await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite)
```

2.  Rufen Sie dann einen Ausgabedatenstrom ab, indem Sie im `stream` die [**GetOutputStreamAt**](https://msdn.microsoft.com/library/windows/apps/br241738)-Methode aufrufen. Fügen Sie diesen in eine **using**-Anweisung ein, um die Lebensdauer des Ausgabedatenstroms zu verwalten.
> [!div class="tabbedCodeSnippets"]
```cs
using (var outputStream = stream.GetOutputStreamAt(0))
    {
        // We'll add more code here in the next step.
    }
    stream.Dispose(); // Or use the stream variable (see previous code snippet) with a using statement as well.
```
```vb
Using outputStream = stream.GetOutputStreamAt(0)
  ' We'll add more code here in the next step.
End Using
```

3.  Fügen Sie jetzt diesen Code in die vorhandene **using**-Anweisung ein, um in den Ausgabedatenstrom zu schreiben, indem Sie ein neues [**DataWriter**](https://msdn.microsoft.com/library/windows/apps/br208154)-Objekt erstellen und die [**DataWriter.WriteString**](https://msdn.microsoft.com/library/windows/apps/br241642)-Methode aufrufen.
> [!div class="tabbedCodeSnippets"]
```cs
using (var dataWriter = new Windows.Storage.Streams.DataWriter(outputStream))
    {
        dataWriter.WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.");
    }
```
```vb
    Dim dataWriter As New DataWriter(outputStream)
    
    dataWriter.WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.")
```

4.  Abschließend fügen Sie diesen Code (innerhalb der inneren **using**-Anweisung) mit [**StoreAsync**](https://msdn.microsoft.com/library/windows/apps/br208171) hinzu, um den Text in der Datei zu speichern, und schließen Sie den Datenstrom mit [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/br241729).
> [!div class="tabbedCodeSnippets"]
```cs
    await dataWriter.StoreAsync();
        await outputStream.FlushAsync(); 
```
```vb
    Await dataWriter.StoreAsync()
        Await outputStream.FlushAsync()
```

## Lesen aus einer Datei


Nachfolgend finden Sie Informationen zum Lesen aus einer Datei auf dem Datenträger mithilfe der [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Klasse. Der allgemein erste Schritt für die einzelnen Methoden zum Lesen von Daten aus einer Datei ist das Abrufen der Datei mit [**StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272).
> [!div class="tabbedCodeSnippets"]
```cs
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
```
```vb
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.GetFileAsync("sample.txt")
```

**Lesen von Text aus einer Datei**

Lesen Sie Text aus einer Datei, indem Sie die [**ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482)-Methode der [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440)-Klasse aufrufen.
> [!div class="tabbedCodeSnippets"]
```cs
string text = await Windows.Storage.FileIO.ReadTextAsync(sampleFile);
```
```vb
Dim text As String = Await Windows.Storage.FileIO.ReadTextAsync(sampleFile)
```

**Lesen von Bytes aus einer Datei mithilfe eines Puffers (2 Schritte)**

1.  Lesen Sie zunächst Bytes aus dem Puffer in die Datei, indem Sie die [**ReadBufferAsync**](https://msdn.microsoft.com/library/windows/apps/hh701468)-Methode der [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440)-Klasse aufrufen.
> [!div class="tabbedCodeSnippets"]
```cs
var buffer = await Windows.Storage.FileIO.ReadBufferAsync(sampleFile);
```
```vb
Dim buffer = Await Windows.Storage.FileIO.ReadBufferAsync(sampleFile)
```

2.  Verwenden Sie dann ein [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119)-Objekt, um zunächst die Länge des Puffers und dann dessen Inhalt zu lesen.
> [!div class="tabbedCodeSnippets"]
```cs
using (var dataReader = Windows.Storage.Streams.DataReader.FromBuffer(buffer))
    {
        string text = dataReader.ReadString(buffer.Length);
    }
```
```vb
Dim dataReader As DataReader = Windows.Storage.Streams.DataReader.FromBuffer(buffer)
    Dim text As String = dataReader.ReadString(buffer.Length)
```

**Lesen von Text aus einer Datei mithilfe eines Datenstroms (4 Schritte)**

1.  Öffnen Sie einen Datenstrom für die Datei, indem Sie die [**StorageFile.OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851)-Methode aufrufen. Wenn der Vorgang abgeschlossen ist, wird ein Datenstrom des Dateiinhalts zurückgegeben.
> [!div class="tabbedCodeSnippets"]
```cs
var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
```
```vb
Dim stream = Await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite)
```

2.  Rufen Sie die Größe des Datenstroms zur späteren Verwendung ab.
> [!div class="tabbedCodeSnippets"]
```cs
ulong size = stream.Size;
```
```vb
Dim size = stream.Size
```

3.  Rufen Sie einen Eingabedatenstrom ab, indem Sie die [**GetInputStreamAt**](https://msdn.microsoft.com/library/windows/apps/br241737)-Methode aufrufen. Fügen Sie ihn in eine **using**-Anweisung ein, um die Lebensdauer des Eingabedatenstroms zu verwalten. Geben Sie beim Aufrufen von **GetInputStreamAt** 0 an, um die Position auf den Anfang des Datenstroms festzulegen.
> [!div class="tabbedCodeSnippets"]
```cs
using (var inputStream = stream.GetInputStreamAt(0))
    {
        // We'll add more code here in the next step.
    }
```
```vb
Using inputStream = stream.GetInputStreamAt(0)
    ' We'll add more code here in the next step.
End Using
```

4.  Abschließend fügen Sie diesen Code in die vorhandene **using**-Anweisung zum Abrufen eines [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119)-Objekts im Datenstrom ein. Lesen Sie dann den Text durch den Aufruf von [**DataReader.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/br208135) und [**DataReader.ReadString**](https://msdn.microsoft.com/library/windows/apps/br208147).
> [!div class="tabbedCodeSnippets"]
```cs
using (var dataReader = new Windows.Storage.Streams.DataReader(inputStream))
    {
        uint numBytesLoaded = await dataReader.LoadAsync((uint)size);
        string text = dataReader.ReadString(numBytesLoaded);
    }
```
```vb
Dim dataReader As New DataReader(inputStream)
    Dim numBytesLoaded As UInteger = Await dataReader.LoadAsync(CUInt(size))
    Dim text As String = dataReader.ReadString(numBytesLoaded)
```

 

 







<!--HONumber=Jun16_HO4-->


