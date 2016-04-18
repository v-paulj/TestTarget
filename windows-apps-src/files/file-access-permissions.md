---
ms.assetid: 3A404CC0-A997-45C8-B2E8-44745539759D
Titel: Berechtigungen für den Dateizugriff
Beschreibung: Hier erfahren Sie, wie Apps standardmäßig auf bestimmte Dateisystemspeicherorte zugreifen können. Apps können darüber hinaus mithilfe der Dateiauswahl oder über die Deklaration von Funktionen auf weitere Speicherorte zugreifen.
---
# Berechtigungen für den Dateizugriff

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Apps können standardmäßig auf bestimmte Dateisystemspeicherorte zugreifen. Apps können darüber hinaus mithilfe der Dateiauswahl oder über die Deklaration von Funktionen auf weitere Speicherorte zugreifen.

## Für alle Apps zugängliche Speicherorte

Bei Erstellung einer neuen App können Sie standardmäßig auf folgende Dateisystemspeicherorte zugreifen:

-   **Installationsverzeichnis von Anwendungen**. Der Ordner, in welchem Ihre Anwendung im System des Benutzers installiert ist.

    Es gibt im Wesentlichen zwei Möglichkeiten, auf Dateien und Ordner im Installationsverzeichnis Ihrer App zuzugreifen.

    1.  Sie können auf folgende Weise einen [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) aufrufen, der das Installationsverzeichnis Ihrer App darstellt:
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        Windows.Storage.StorageFolder installedLocation = Windows.ApplicationModel.Package.Current.InstalledLocation;
        ```
        ```javascript
        var installDirectory = Windows.ApplicationModel.Package.current.installedLocation;
        ```

       Sie können anschließend mithilfe von [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)-Methoden auf Dateien und Ordner im Verzeichnis zugreifen. Im Beispiel wird dieser **StorageFolder** in der `installDirectory`-Variablen gespeichert. Sie können das [Informationsbeispiel des Anwendungspakets](http://go.microsoft.com/fwlink/p/?linkid=231526) für Windows 8.1 herunterladen und dessen Quellcode in Ihrer Windows 10-App wiederverwenden, um mehr über die Arbeit mit dem App-Paket und Installationsverzeichnis zu erfahren.

    2.  Sie können eine Datei direkt aus dem Installationsverzeichnis Ihrer Anwendung mithilfe der Anwendungs-URI wie folgt aufrufen:
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;            
        StorageFile file = await StorageFile.GetFileFromApplicationUriAsync("ms-appx:///file.txt");
        ```
        ```javascript
        Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appx:///file.txt").done(
            function(file) {
                // Process file
            }
        );
        ```
        
        Nach vollständiger Ausführung von [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) wird ein [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) zurückgegeben, das die Datei *file.txt* im Installationsverzeichnis der Anwendung darstellt (`file` im Beispiel).

        Das Präfix „ms-appx:///“ in der URI bezieht sich auf das Installationsverzeichnis der Anwendung. Weitere Informationen zur Verwendung von App-URIs finden Sie unter [Verwenden von URIs zum Verweisen auf Inhalte](https://msdn.microsoft.com/library/windows/apps/hh781215).

    Darüber hinaus können Sie im Gegensatz zu anderen Speicherorten auf das Installationsverzeichnis Ihrer App auch direkt mit [Win32 und COM für UWP-Apps (Universelle Windows-Plattform)](https://msdn.microsoft.com/library/windows/apps/br205757) und einigen [Funktionen der C/C++-Standardbibliothek in Microsoft Visual Studio](http://msdn.microsoft.com/library/hh875057.aspx) zugreifen.

    Das Installationsverzeichnis der Anwendung ist ein schreibgeschützter Speicherort. Sie können nicht über die Dateiauswahl auf das Installationsverzeichnis zugreifen.

-   **Speicherorte von Anwendungsdaten.** Die Ordner, in denen Ihre App Daten speichern kann. Diese Ordner (lokal, servergespeichert und temporär) werden nach Installation Ihrer Anwendung erstellt.

    Es gibt im Wesentlichen zwei Möglichkeiten, auf Dateien und Ordner in den Dateispeicherorten Ihrer Anwendung zuzugreifen:

    1.  Verwenden Sie die [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587)-Eigenschaften, um einen Dateiordner abzurufen.

        Sie können zum Beispiel [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587).[**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) verwenden, um einen [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) abzurufen, der den lokalen Ordner Ihrer Anwendung wie folgt darstellt:
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFolder localFolder = ApplicationData.Current.LocalFolder;
        ```
        ```javascript
        var localFolder = Windows.Storage.ApplicationData.current.localFolder;
        ```
 
        Wenn Sie auf den servergespeicherten oder temporären Ordner Ihrer Anwendung zugreifen möchten, verwenden Sie die [**RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623)- oder [**TemporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629)-Eigenschaft.

        Nach dem Aufrufen des [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230), der den Dateispeicherort der Anwendung darstellt, können Sie auf Dateien und Ordner im Verzeichnis mithilfe der **StorageFolder**-Methode zugreifen. Im Beispiel werden diese **StorageFolder**-Objekte in der `localFolder`-Variablen gespeichert. Weitere Informationen zum Verwenden der Speicherorte von App-Daten finden Sie unter [Verwalten von Anwendungsdaten](https://msdn.microsoft.com/library/windows/apps/hh465109). Sie können auch das [Beispiel für Anwendungsdaten](http://go.microsoft.com/fwlink/p/?linkid=231478) für Windows 8.1 herunterladen und dessen Quellcode in Ihrer Windows 10-App wiederverwenden.

    2.  Sie können eine Datei zum Beispiel direkt aus dem lokalen Ordner Ihrer Anwendung mithilfe der Anwendungs-URI wie folgt aufrufen:
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFile file = await StorageFile.GetFileFromApplicationUriAsync("ms-appdata:///local/file.txt");
        ```
        ```javascript
        Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appdata:///local/file.txt").done(
            function(file) {
                // Process file
            }
        );
        ```

        Nach vollständiger Ausführung von [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) wird ein [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) zurückgegeben, das die Datei *file.txt* im lokalen Ordner der Anwendung darstellt (`file` im Beispiel).

        Das Präfix „ms-appdata:///local/“ in der URI bezieht sich auf den lokalen Ordner der Anwendung. Für den Zugriff auf Dateien in servergespeicherten oder temporären Ordnern verwenden Sie „ms-appdata:///roaming/“ oder „ms-appdata:///temporary/“. Weitere Informationen zur Verwendung von Anwendungs-URIs finden Sie in [So wird's gemacht: Laden von Dateiressourcen](https://msdn.microsoft.com/library/windows/apps/hh781229).

    Darüber hinaus können Sie, im Gegensatz zu anderen Speicherorten, auf Dateien in den App-Datenspeicherorten auch mit [Win32 und COM für UWP-Apps (Universelle Windows-Plattform)](https://msdn.microsoft.com/library/windows/apps/br205757) und einigen Funktionen der C/C++-Standardbibliothek in Microsoft Visual Studio zugreifen.

    Sie können nicht über die Dateiauswahl auf lokale, servergespeicherte oder temporäre Ordner zugreifen.

-   **Wechselmedien.** Darüber hinaus kann Ihre App standardmäßig auf einige der Dateien auf verbundenen Geräten zugreifen. Diese Möglichkeit besteht, wenn Ihre App die [Erweiterung für die automatische Wiedergabe](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh464906.aspx#autoplay) verwendet, die automatisch gestartet wird, sobald Benutzer ein Gerät, wie eine Kamera oder einen USB-Speicherstick, an Ihr System anschließen. Die Dateien, auf welche Ihre App zugreifen kann, sind auf bestimmte Dateitypen begrenzt, die über Deklarationen von Dateitypzuordnungen in Ihrem App-Manifest festgelegt sind.

    Sie können natürlich auch auf Dateien und Ordner auf einem Wechselmedium mithilfe der Dateiauswahl zugreifen (unter Verwendung von [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) und [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)) und den Benutzer die Dateien und Ordner auswählen lassen, auf welche Ihre App Zugriff haben soll. Weitere Informationen zur Verwendung der Dateiauswahl finden Sie unter [Öffnen von Dateien und Ordnern mit einer Auswahl](quickstart-using-file-and-folder-pickers.md).

    **Hinweis**  Weitere Informationen zum Zugriff auf eine SD-Karte aus einer mobilen App finden Sie unter [Zugreifen auf die SD-Karte](access-the-sd-card.md).

     

## Speicherorte, auf die Windows Store-Apps zugreifen können

-   **Downloadordner des Benutzers.** Der Ordner, in dem heruntergeladene Dateien standardmäßig gespeichert werden.

    Standardmäßig kann Ihre Anwendung nur auf Dateien und Ordner im Ordner "Downloads" des Benutzers zugreifen, die von Ihrer App erstellt wurden. Sie können jedoch durch Aufrufen der Dateiauswahl auf weitere Dateien und Ordner im Downloadordner des Benutzers zugreifen ([**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) oder [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)), sodass Benutzer zu Dateien oder Ordner navigieren und diese auswählen können und Ihre Anwendung Zugriff auf diese erhält.

    -   Sie können im Downloadordner des Benutzers eine Datei wie folgt erstellen:
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFile newFile = await DownloadsFolder.CreateFileAsync("file.txt");
        ```
        ```javascript
        Windows.Storage.DownloadsFolder.createFileAsync("file.txt").done(
            function(newFile) {
                // Process file
            }
        );
        ```
 
        [**DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh996761) ist überladen, sodass Sie festlegen können, was das System im Falle einer bereits vorhandenen gleichnamigen Datei im Downloadordner des Benutzers tun sollte. Nach vollständiger Ausführung dieser Methoden wird ein [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) zurückgegeben, das die erstellte Datei darstellt. Diese Datei wird im Beispiel `newFile` genannt.

    -   Sie können im Downloadordner des Benutzers wie folgt einen Unterordner erstellen:
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFolder newFolder = await DownloadsFolder.CreateFolderAsync("New Folder");
        ```
        ```javascript
        Windows.Storage.DownloadsFolder.createFolderAsync("New Folder").done(
            function(newFolder) {
                // Process folder
            }
        );
        ```
 
        [**DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFolderAsync**](https://msdn.microsoft.com/library/windows/apps/hh996763) ist überladen, sodass Sie festlegen können, was das System im Falle eines bereits bestehenden gleichnamigen Unterordners im Ordner "Downloads" des Benutzers tun sollte. Nach vollständiger Ausführung dieser Methoden wird ein [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) zurückgegeben, der den erstellten Unterordner darstellt. Diese Datei wird im Beispiel `newFolder` genannt.

    Wenn Sie eine Datei oder einen Ordner im Downloadordner erstellen, empfehlen wir, die Datei oder den Ordner der [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) Ihrer App hinzuzufügen, sodass zukünftig leicht auf dieses Element zugegriffen werden kann.

## Zugriff auf zusätzliche Speicherorte

Zusätzlich zu den Standardspeicherorten kann eine App durch das Deklarieren von Funktionen im App-Manifest (siehe [Deklaration der App-Funktionen](https://msdn.microsoft.com/library/windows/apps/mt270968)) oder durch Aufrufen der Dateiauswahl auf zusätzliche Dateien und Ordner zugreifen, um den Benutzer Dateien und Ordner auswählen zu lassen, auf welche die App Zugriff haben soll (siehe [Öffnen von Dateien und Ordnern mit einer Auswahl](quickstart-using-file-and-folder-pickers.md)).

In der folgenden Tabelle sind weitere Speicherorte aufgeführt, auf die Sie durch Deklarieren von Funktionen und Verwenden der zugehörigen [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346)-APIs zugreifen können:

| Ort | Funktion | Windows.Storage-API |
|----------|------------|---------------------|
| Dokumente | DocumentsLibrary <br><br>Hinweis: Sie müssen Ihrem App-Manifest Dateitypzuordnungen hinzufügen, die bestimmte Dateitypen deklarieren, auf die Ihre App an diesem Speicherort Zugriff hat. <br><br>Verwenden Sie diese Funktion, wenn Ihre App:<br>- Den plattformübergreifenden Offlinezugriff auf bestimmte OneDrive-Inhalte mit gültigen OneDrive-URLs oder Ressourcen-IDs ermöglicht.<br>- Geöffnete Dateien im Offlinezustand automatisch im OneDrive des Benutzers speichert. | [KnownFolders.DocumentsLibrary](https://msdn.microsoft.com/library/windows/apps/br227152) |
| Musik     | MusicLibrary <br>Weitere Informationen finden Sie unter [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.MusicLibrary](https://msdn.microsoft.com/library/windows/apps/br227155) |    
| Bilder  | PicturesLibrary<br> Weitere Informationen finden Sie unter [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.PicturesLibrary](https://msdn.microsoft.com/library/windows/apps/br227156) |  
| Videos    | VideosLibrary<br>Weitere Informationen finden Sie unter [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.VideosLibrary](https://msdn.microsoft.com/library/windows/apps/br227159) |   
| Wechselmedien  | RemovableDevices <br><br>Hinweis: Sie müssen Ihrem App-Manifest Dateitypzuordnungen hinzufügen, die bestimmte Dateitypen deklarieren, auf die Ihre App an diesem Speicherort Zugriff hat. <br><br>Weitere Informationen finden Sie unter [Zugreifen auf die SD-Karte](access-the-sd-card.md). | [KnownFolders.RemovableDevices](https://msdn.microsoft.com/library/windows/apps/br227158) |  
| Bibliotheken der Heimnetzgruppen  | Mindestens eine der folgenden Funktionen ist erforderlich. <br>– MusicLibrary <br>– PicturesLibrary <br>– VideosLibrary | [KnownFolders.HomeGroup](https://msdn.microsoft.com/library/windows/apps/br227153) |      
| Medienservergeräte (DLNA) | Mindestens eine der folgenden Funktionen ist erforderlich. <br>– MusicLibrary <br>– PicturesLibrary <br>– VideosLibrary | [KnownFolders.MediaServerDevices](https://msdn.microsoft.com/library/windows/apps/br227154) | 
| Universal Naming Convention (UNC)-Ordner | Eine Kombination der folgenden Funktionen ist erforderlich. <br><br>Die Funktion für Heim- oder Arbeitsplatznetzwerke: <br>– PrivateNetworkClientServer <br><br>Zudem mindestens eine Funktion zu Internet und öffentlichen Netzwerken: <br>– InternetClient <br>– InternetClientServer <br><br>Und gegebenenfalls die Funktion für Domänenanmeldeinformationen:<br>– EnterpriseAuthentication <br><br>Hinweis: Sie müssen Ihrem App-Manifest Dateitypzuordnungen hinzufügen, die bestimmte Dateitypen deklarieren, auf die Ihre App an diesem Speicherort Zugriff hat. | Abrufen eines Ordners mithilfe von: <br>[StorageFolder.GetFolderFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227278) <br><br>Abrufen einer Datei mithilfe von: <br>[StorageFile.GetFileFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227206) |



<!--HONumber=Mar16_HO1-->


