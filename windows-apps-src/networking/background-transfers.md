---
description: Verwenden Sie die Hintergrundübertragungs-API zum zuverlässigen Kopieren von Dateien im Netzwerk.
title: Hintergrundübertragungen
ms.assetid: 1207B089-BC16-4BF0-BBD4-FD99950C764B
---

# Hintergrundübertragungen

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**Wichtige APIs**

-   [**Windows.Networking.BackgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242)
-   [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998)
-   [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)

Verwenden Sie die Hintergrundübertragungs-API zum zuverlässigen Kopieren von Dateien im Netzwerk. Die Hintergrundübertragungs-API bietet erweiterte Upload- und Downloadfeatures, die bei angehaltener App im Hintergrund ausgeführt werden und auch nach Beendigung der App aktiv bleiben. Die API überwacht den Netzwerkstatus und kann Übertragungen automatisch anhalten und fortsetzen, wenn die Verbindung unterbrochen wird. Übertragungen sind außerdem akkuabhängig – die Downloadaktivität wird also basierend auf dem aktuellen Verbindungs- und Geräteakkustatus angepasst. Die API ist ideal für das Hoch- und Herunterladen von großen Dateien über HTTP(S) geeignet. FTP wird auch unterstützt, allerdings nur für Downloads.

Hintergrundübertragungen werden getrennt von der aufrufenden App ausgeführt und wurden hauptsächlich für lange Übertragungen von Ressourcen wie Videos, Musik und großen Bildern entwickelt. Für diese Szenarien ist die Verwendung der Hintergrundübertragung unverzichtbar, da Downloads im Hintergrund fortgesetzt werden – selbst dann, wenn die App angehalten wurde.

Wenn Sie kleine Ressourcen herunterladen, deren Download in der Regel schnell abgeschlossen ist, sollten Sie anstelle der Hintergrundübertragung [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)-APIs verwenden.

## Verwenden von Windows.Networking.BackgroundTransfer


### Wie funktioniert das Feature für die Hintergrundübertragung?

Wenn eine App die Hintergrundübertragung verwendet, um eine Übertragung zu initiieren, wird die Anforderung mithilfe des Klassenobjekts [**BackgroundDownloader**](https://msdn.microsoft.com/library/windows/apps/br207126) oder [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) konfiguriert und initialisiert. Alle Übertragungsvorgänge werden vom System einzeln und getrennt von der aufrufenden App behandelt. Statusinformationen sind verfügbar, falls Sie den Benutzer in der Benutzeroberfläche Ihrer App über den Status informieren möchten. Zudem kann Ihre App während der Übertragung angehalten, fortgesetzt und abgebrochen werden oder sogar die Daten lesen. Die Behandlung von Übertragungen durch das System unterstützt den intelligenten Stromverbrauch und verhindert Probleme, die entstehen können, wenn bei einer verbundenen App Ereignisse wie das Anhalten der App, das Beenden der App oder plötzliche Änderungen des Netzwerkstatus auftreten.

### Durchführen authentifizierter Dateianforderungen mit Hintergrundübertragung

Das Feature für die Hintergrundübertragung stellt Methoden bereit, die allgemeine Server- und Proxyanmeldeinformationen, Cookies sowie die Verwendung von benutzerdefinierten HTTP-Headern (mithilfe von [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146)) für einzelne Übertragungen unterstützen.

### Wie reagiert dieses Feature bei Netzwerkstatusänderungen und unerwartetem Herunterfahren?

Das Feature für die Hintergrundübertragung bewahrt die Einheitlichkeit der einzelnen Übertragungen, wenn sich der Netzwerkstatus ändert. Hierzu werden die vom Feature für die [Konnektivität](https://msdn.microsoft.com/library/windows/apps/hh452990) bereitgestellten Statusinformationen zur Konnektivität und zum Status des Datentarifs des Netzbetreibers intelligent genutzt. Um das Verhalten für unterschiedliche Netzwerkszenarien zu definieren, legt eine App mithilfe der von [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) definierten Werte eine Kostenrichtlinie für die einzelnen Übertragungen fest.

Beispielsweise kann die für einen Vorgang definierte Kostenrichtlinie vorsehen, dass der Vorgang automatisch angehalten wird, wenn das Gerät in einem getakteten Netzwerk verwendet wird. Die Übertragung wird automatisch fortgesetzt (oder erneut gestartet), wenn eine Verbindung mit einem „uneingeschränkten“ Netzwerk hergestellt wird. Weitere Informationen zur Definition von Netzwerken durch Kosten finden Sie unter [**NetworkCostType**](https://msdn.microsoft.com/library/windows/apps/br207292).

Obwohl das Feature für die Hintergrundübertragung über eigene Mechanismen zur Behandlung von Netzwerkstatusänderungen verfügt, müssen bei mit einem Netzwerk verbundenen Apps einige allgemeine Punkte im Zusammenhang mit der Konnektivität beachtet werden. Weitere Informationen finden Sie unter [Zugreifen auf den Netzwerkverbindungsstatus und Verwalten von Netzwerkkosten (HTML)](https://msdn.microsoft.com/library/windows/apps/hh452983).

> **Hinweis**  Mithilfe von Features für auf Mobilgeräten ausgeführten Apps kann der Benutzer die übertragene Datenmenge basierend auf dem Verbindungstyp, Roamingstatus und Datentarif überwachen und einschränken. Aus diesem Grund können Hintergrundübertragungen auf dem Telefon auch dann angehalten werden, wenn die Übertragung gemäß dem [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138)-Wert fortgesetzt werden sollte.

Die folgende Tabelle zeigt für jeden [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138)-Wert, wann Hintergrundübertragungen basierend auf dem aktuellen Status des Telefons zulässig sind. Sie können den aktuellen Status des Telefons auch mithilfe der [**ConnectionCost**](https://msdn.microsoft.com/library/windows/apps/br207244)-Klasse ermitteln.

| Gerätestatus                                                                                                                      | UnrestrictedOnly | Standard | Always |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------|---------|--------|
| Mit WLAN verbunden                                                                                                                 | Zulassen            | Zulassen   | Zulassen  |
| Getaktete Verbindung, kein Roaming, unter dem Datenlimit, Überschreitung des Limits nicht zu erwarten                                                   | Verweigern             | Zulassen   | Zulassen  |
| Getaktete Verbindung, kein Roaming, unter dem Datenlimit, Überschreitung des Limits zu erwarten                                                       | Verweigern             | Verweigern    | Zulassen  |
| Getaktete Verbindung, Roaming, unter dem Datenlimit                                                                                     | Verweigern             | Verweigern    | Zulassen  |
| Getaktete Verbindung, Roaming, über dem Datenlimit. Dieser Status tritt nur auf, wenn der Benutzer in der Data Sense-Benutzeroberfläche die Option „Datennutzung im Hintergrund einschränken“ aktiviert. | Verweigern             | Verweigern    | Verweigern   |

 

## Hochladen von Dateien


Bei der Hintergrundübertragung erfolgt der Upload als [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224). Dabei wird eine Reihe von Steuerungsmethoden zum Neustarten oder Abbrechen des Vorgangs verfügbar gemacht. App-Ereignisse (z. B. Anhalten oder Beenden) und Konnektivitätsänderungen werden vom System automatisch durch **UploadOperation** behandelt. Uploads werden bei Anhalten oder Unterbrechen einer App und auch nach dem Beenden der App fortgesetzt. Außerdem wird durch Festlegen der [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018)-Eigenschaft angegeben, ob die App Uploads startet, wenn für die Internetkonnektivität ein getaktetes Netzwerk verwendet wird.

In den folgenden Beispielen werden die Erstellung und Initialisierung eines einfachen Uploads sowie das Aufzählen und Fortsetzen von in einer vorherigen App-Sitzung gespeicherten Vorgängen erläutert.

### Hochladen einer Datei

Die Erstellung eines Uploads beginnt mit [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140). Mit dieser Klasse werden die Methoden bereitgestellt, womit die App den Upload konfigurieren kann, bevor die resultierende [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224)-Instanz erstellt wird. Im folgenden Beispiel wird gezeigt, wie dies mit den erforderlichen [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) und [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Objekten gemacht wird.

**Identifizieren der Datei und des Ziels für den Upload**

Vor dem Erstellen einer [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) müssen wir den URI des Speicherorts für den Upload und die hochzuladende Datei bestimmen. Im folgenden Beispiel wird der *uriString*-Wert mit einer Zeichenfolge aus der Benutzeroberflächeneingabe und der *file*-Wert mit dem [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Objekt gefüllt, das von einem [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275)-Vorgang zurückgegeben wird.

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_B "Identifizieren der Datei und des Ziels für den Upload")]

**Erstellen und Initialisieren des Uploadvorgangs**

Im vorherigen Schritt wurden die Werte *uriString* und *file* an eine Instanz von „UploadOp“ aus dem nächsten Beispiel übergeben. Dort werden sie zum Konfigurieren und Starten des neuen Uploadvorgangs verwendet. Zunächst wird *uriString* analysiert, um das erforderliche [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998)-Objekt zu erstellen.

Dann verwendet [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) die Eigenschaften der bereitgestellten [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Klasse (*file*) zum Füllen des Anforderungsheaders und Festlegen der *SourceFile*-Eigenschaft mit dem **StorageFile**-Objekt. Anschließend wird die [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146)-Methode aufgerufen, um den als Zeichenfolge bereitgestellten Dateinamen und die [**StorageFile.Name**](https://msdn.microsoft.com/library/windows/apps/br227220)-Eigenschaft einzufügen.

Schließlich erstellt [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) die [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224)-Klasse (*upload*).

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_A "Erstellen und Initialisieren des mehrteiligen Uploadvorgangs")]

Beachten Sie die asynchronen Methodenaufrufe, die mit JavaScript-Zusagen definiert sind. Sehen Sie sich die folgende Zeile aus dem letzten Beispiel an:

```javascript
promise = upload.startAsync().then(complete, error, progress);
```

    The async method call is followed by a then statement which indicates methods, defined by the app, that are called when a result from the async method call is returned. For more information on this programming pattern, see [Asynchronous programming in JavaScript using promises](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx).

### Hochladen mehrerer Dateien

**Identifizieren der Dateien und des Ziels für den Upload**

    In a scenario involving multiple files transferred with a single [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), the process begins as it usually does by first providing the required destination URI and local file information. Similar to the example in the previous section, the URI is provided as a string by the end-user and [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) can be used to provide the ability to indicate files through the user interface as well. However, in this scenario the app should instead call the [**PickMultipleFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br207851) method to enable the selection of multiple files through the UI.

```javascript
function uploadFiles() {
       var filePicker = new Windows.Storage.Pickers.FileOpenPicker();
       filePicker.fileTypeFilter.replaceAll(["*"]);

       filePicker.pickMultipleFilesAsync().then(function (files) {
          if (files === 0) {
             printLog("No file selected");
                return;
          }

          var upload = new UploadOperation();
          var uriString = document.getElementById("serverAddressField").value;
          upload.startMultipart(uriString, files);

          // Persist the upload operation in the global array.
          uploadOperations.push(upload);
       });
    }
```

**Erstellen von Objekten für die verfügbaren Parameter**

    The next two examples use code contained in a single example method, **startMultipart**, which was called at the end of the last step. For the purpose of instruction the code in the method that creates an array of [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) objects has been split from the code that creates the resultant [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224).

    First, the URI string provided by the user is initialized as a [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998). Next, the array of [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) objects (**files**) passed to this method is iterated through, each object is used to create a new [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) object which is then placed in the **contentParts** array.

```javascript
upload.startMultipart = function (uriString, files) {
        try {
            var uri = new Windows.Foundation.Uri(uriString);
            var uploader = new Windows.Networking.BackgroundTransfer.BackgroundUploader();

            var contentParts = [];
            files.forEach(function (file, index) {
                var part = new Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart("File" + index, file.name);
                part.setFile(file);
                contentParts.push(part);
            });
```

**Erstellen und Initialisieren des mehrteiligen Uploadvorgangs**

    With our contentParts array populated with all of the [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) objects representing each [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) for upload, we are ready to call [**CreateUploadAsync**](https://msdn.microsoft.com/library/windows/apps/hh923973) using the [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) to indicate where the request will be sent.

```javascript
        // Create a new upload operation.
            uploader.createUploadAsync(uri, contentParts).then(function (uploadOperation) {

               // Start the upload and persist the promise to be able to cancel the upload.
               upload = uploadOperation;
               promise = uploadOperation.startAsync().then(complete, error, progress);
            });

         } catch (err) {
             displayError(err);
         }
     };
```

### Neustarten von unterbrochenen Uploadvorgängen

Bei Abschluss oder Abbruch von [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) werden alle zugeordneten Systemressourcen freigegeben. Wenn die App allerdings vor Auftreten eines dieser Ereignisse beendet wird, werden aktive Vorgänge angehalten, und die ihnen zugeordneten Ressourcen bleiben belegt. Wenn diese Vorgänge nicht in der nächsten App-Sitzung aufgezählt und fortgesetzt werden, werden sie nicht abgeschlossen und belegen weiterhin Geräteressourcen.

1.  Bevor Sie die Funktion zum Aufzählen beibehaltener Vorgänge definieren, müssen wir ein Array erstellen, das die zurückzugebenden [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224)-Objekte enthält:

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_C "Neustarten von unterbrochenen Uploadvorgängen")]

2.  Im nächsten Schritt definieren Sie die Funktion, die alle beibehaltenen Vorgänge aufzählt und im Array speichert. Die **load**-Methode, die zum Neuzuweisen der Rückrufe von [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) aufgerufen wird, wenn der Download nach Beendigung der App fortgesetzt wird, befindet sich in der UploadOp-Klasse, die Sie weiter unten in diesem Abschnitt definieren.

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_D "Aufzählen beibehaltener Vorgänge")]

## Herunterladen von Dateien

Bei der Hintergrundübertragung erfolgt jeder Download als [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154). Dabei werden eine Reihe von Steuerungsmethoden zum Anhalten, Fortsetzen, Neustarten und Abbrechen des Vorgangs verfügbar gemacht. App-Ereignisse (z. B. Anhalten oder Beenden) und Konnektivitätsänderungen werden vom System automatisch durch **DownloadOperation** behandelt. Downloads werden bei Anhalten oder Unterbrechen einer App und auch nach dem Beenden der App fortgesetzt. In Szenarien für mobile Netzwerke wird außerdem durch Festlegen der [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018)-Eigenschaft angegeben, ob die App Downloads startet oder fortsetzt, wenn für die Internetkonnektivität ein getaktetes Netzwerk verwendet wird.

Wenn Sie kleine Ressourcen herunterladen, deren Download in der Regel schnell abgeschlossen ist, sollten Sie anstelle der Hintergrundübertragung [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)-APIs verwenden.

In den folgenden Beispielen werden die Erstellung und Initialisierung eines einfachen Downloads sowie das Aufzählen und Fortsetzen von in einer vorherigen App-Sitzung gespeicherten Vorgängen erläutert.

### Konfigurieren und Starten eines Dateidownloads mit Hintergrundübertragung

Das folgende Beispiel veranschaulicht, wie mit Zeichenfolgen, die einen URI und einen Dateinamen darstellen, ein [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998)-Objekt und die [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) erstellt werden können, die die angeforderte Datei enthalten. Im Beispiel wird die neue Datei automatisch an einem vordefinierten Speicherort abgelegt. Alternativ kann Benutzern mithilfe von [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) ermöglicht werden, den Speicherort der Datei auf dem Gerät anzugeben. Die **load**-Methode, die zum Neuzuweisen der Rückrufe von [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) aufgerufen wird, wenn der Download nach Beendigung der App fortgesetzt wird, befindet sich in der „DownloadOp“-Klasse, die Sie weiter unten in diesem Abschnitt definieren.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_A)]

Beachten Sie die asynchronen Methodenaufrufe, die mit JavaScript-Zusagen definiert sind. Aus Zeile 17 des vorherigen Codebeispiels geht Folgendes hervor:

```javascript
promise = download.startAsync().then(complete, error, progress);
```

Nach dem Async-Methodenaufruf folgt eine then-Anweisung, die von der App definierte Methoden angibt, die aufgerufen werden, wenn ein Ergebnis aus dem Async-Methodenaufruf zurückgegeben wird. Weitere Informationen zu diesem Programmierungsmuster finden Sie unter [Asynchrone Programmierung in JavaScript mit Zusagen](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx).

### Hinzufügen weiterer Methoden zur Vorgangssteuerung

Der Grad der Steuerung lässt sich durch Implementieren zusätzlicher [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154)-Methoden erhöhen. Wenn Sie beispielsweise dem obigen Beispiel den folgenden Code hinzufügen, wird das Abbrechen des Downloads ermöglicht.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_B)]

### Aufzählen beibehaltener Vorgänge beim Start

Bei Abschluss oder Abbruch von [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) werden alle zugeordneten Systemressourcen freigegeben. Wenn die App allerdings vor Auftreten eines dieser Ereignisse beendet wird, werden die Downloads angehalten und im Hintergrund beibehalten. In den folgenden Beispielen wird gezeigt, wie Sie beibehaltene Downloads in einer neuen App-Sitzung fortsetzen.

1.  Bevor Sie die Funktion zum Aufzählen beibehaltener Vorgänge definieren, müssen wir ein Array erstellen, das die zurückzugebenden [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154)-Objekte enthält:

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_D)]

2.  Im nächsten Schritt definieren Sie die Funktion, die alle beibehaltenen Vorgänge aufzählt und im Array speichert. Die **load**-Methode, die zum Neuzuweisen von Rückrufen für eine beibehaltene [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) aufgerufen wird, befindet sich im Beispiel für die DownloadOp-Klasse, die weiter unten in diesem Abschnitt definiert wird.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_E)]

3.  Die mit Daten aufgefüllte Liste kann nun zum Neustarten ausstehender Vorgänge verwendet werden.

## Nachverarbeitung

Ein neues Feature in Windows 10 ermöglicht es nach Abschluss einer Hintergrundübertragung, den Anwendungscode auszuführen, selbst wenn die App nicht ausgeführt wird. Ihre App möchte z. B. eine Liste der verfügbaren Filme aktualisieren, nachdem ein Film heruntergeladen wurde, statt bei jedem Start nach neuen Filmen zu suchen. Ihre App möchte ggf. eine fehlerhafte Dateiübertragung behandeln, indem diese unter Verwendung eines anderes Servers oder Ports wiederholt wird. Nachbearbeitung wird für erfolgreiche und nicht erfolgreiche Übertragungen aufgerufen, damit Sie sie zum Implementieren benutzerdefinierter Fehlerbehandlung und Wiederholungslogik verwenden können.

Nachbearbeitung verwendet die vorhandene Hintergrundaufgaben-Infrastruktur. Erstellen Sie eine Hintergrundaufgabe, und ordnen Sie sie Ihren Übertragungen zu, bevor Sie die Übertragung starten. Die Übertragungen werden dann im Hintergrund ausgeführt, und wenn sie abgeschlossen sind, wird die Hintergrundaufgabe aufgerufen, um die Nachbearbeitung auszuführen.

Für die Nachbearbeitung wird als neue Klasse [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209) verwendet. Diese Klasse ist mit der vorhandenen [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030)-Klasse vergleichbar, mit der Sie Hintergrundübertragungen gruppieren können. Die **BackgroundTransferCompletionGroup**-Klasse ermöglicht es jedoch zudem, eine Hintergrundaufgabe anzugeben, die nach Abschluss der Übertragung ausgeführt werden soll.

Initiieren Sie wie folgt eine Hintergrundübertragung mit Nachbearbeitung.

1.  Erstellen Sie ein [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209)-Objekt. Erstellen Sie dann ein [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)-Objekt. Legen Sie die **Trigger**-Eigenschaft des Generator-Objekts auf das Abschlussgruppenobjekt und die **TaskEngtyPoint**-Eigenschaft des Generators auf den Einstiegspunkt der Hintergrundaufgabe fest, die nach Abschluss der Übertragung ausgeführt werden soll. Rufen Sie schließlich die [**BackgroundTaskBuilder.Register**](https://msdn.microsoft.com/library/windows/apps/br224772)-Methode auf, um die Hintergrundaufgabe zu registrieren. Beachten Sie, dass viele Abschlussgruppen einen Einstiegspunkt für die Hintergrundaufgabe gemeinsam verwenden können, Sie jedoch nur über eine Abschlussgruppe pro Registrierung einer Hintergrundaufgabe verfügen dürfen.

   ```csharp
    var completionGroup = new BackgroundTransferCompletionGroup();
    BackgroundTaskBuilder builder = new BackgroundTaskBuilder();

    builder.Name = "MyDownloadProcessingTask";
    builder.SetTrigger(completionGroup.Trigger);
    builder.TaskEntryPoint = "Tasks.BackgroundDownloadProcessingTask";

    BackgroundTaskRegistration downloadProcessingTask = builder.Register();
    ```

2.  Next you associate background transfers with the completion group. Once all transfers are created, enable the completion group.

   ```csharp
    BackgroundDownloader downloader = new BackgroundDownloader(completionGroup);
    DownloadOperation download = downloader.CreateDownload(uri, file);
    Task<DownloadOperation> startTask = download.StartAsync().AsTask();

    // App still sees the normal completion path
    startTask.ContinueWith(ForegroundCompletionHandler);

    // Do not enable the CompletionGroup until after all downloads are created.
    downloader.CompletinGroup.Enable();
    ```

3.  The code in the background task extracts the list of operations from the trigger details, and your code can then inspect the details for each operation and perform appropriate post-processing for each operation.

   ```csharp
    public class BackgroundDownloadProcessingTask : IBackgroundTask
    {
      public async void Run(IBackgroundTaskInstance taskInstance)
      {
        var details = (BackgroundTransferCompletionGroupTriggerDetails)taskInstance.TriggerDetails;
        IReadOnlyList<DownloadOperation> downloads = details.Downloads;

        // Do post-processing on each finished operation in the list of downloads
      }
    }
    ```

The post-processing task is a regular background task. It is part of the pool of all background tasks, and it is subject to the same resource management policy as all background tasks.

Also, note that post-processing does not replace foreground completion handlers. If your app defines a foreground completion handler, and your app is running when the file transfer completes, then both your foreground completion handler and your background completion handler will be called. The order in which foreground and background tasks are called is not guaranteed. If you define both, you should ensure that the two tasks will work properly and not interfere with each other if they are running concurrently.

## Request timeouts

There are two primary connection timeout scenarios to take into consideration:

-   When establishing a new connection for a transfer, the connection request is aborted if it is not established within five minutes.

-   After a connection has been established, an HTTP request message that has not received a response within two minutes is aborted.

> **Note**  In either scenario, assuming there is Internet connectivity, Background Transfer will retry a request up to three times automatically. In the event Internet connectivity is not detected, additional requests will wait until it is.

## Debugging guidance

Stopping a debugging session in Microsoft Visual Studio is comparable to closing your app; PUT uploads are paused and POST uploads are terminated. Even while debugging, your app should enumerate and then restart or cancel any persisted uploads. For example, you can have your app cancel enumerated persisted upload operations at app startup if there is no interest in previous operations for that debug session.

While enumerating downloads/uploads on app startup during a debug session, you can have your app cancel them if there is no interest in previous operations for that debug session. Note that if there are Visual Studio project updates, like changes to the app manifest, and the app is uninstalled and re-deployed, [**GetCurrentUploadsAsync**](https://msdn.microsoft.com/library/windows/apps/hh701149) cannot enumerate operations created using the previous app deployment.

When using Background Transfer during development, you may get into a situation where the internal caches of active and completed transfer operations can get out of sync. This may result in the inability to start new transfer operations or interact with existing operations and [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030) objects. In some cases, attempting to interact with existing operations may trigger a crash. This result can occur if the [**TransferBehavior**](https://msdn.microsoft.com/library/windows/apps/dn279033) property is set to **Parallel**. This issue occurs only in certain scenarios during development and is not applicable to end users of your app.

Four scenarios using Visual Studio can cause this issue.

-   You create a new project with the same app name as an existing project, but a different language (from C++ to C#, for example).
-   You change the target architecture (from x86 to x64, for example) in an existing project.
-   You change the culture (from neutral to en-US, for example) in an existing project.
-   You add or remove a capability in the package manifest (adding **Enterprise Authentication**, for example) in an existing project.

Regular app servicing, including manifest updates which add or remove capabilities, do not trigger this issue on end user deployments of your app.
To work around this issue, completely uninstall all versions of the app and re-deploy with the new language, architecture, culture, or capability. This can be done via the **Start** screen or using PowerShell and the **Remove-AppxPackage** cmdlet.

## Exceptions in Windows.Networking.BackgroundTransfer

An exception is thrown when an invalid string for a the Uniform Resource Identifier (URI) is passed to the constructor for the [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) object.

**.NET:  **The [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) type appears as [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) in C# and VB.

In C# and Visual Basic, this error can be avoided by using the [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) class in the .NET 4.5 and one of the [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) methods to test the string received from the app user before the URI is constructed.

In C++, there is no method to try and parse a string to a URI. If an app gets input from the user for the [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998), the constructor should be in a try/catch block. If an exception is thrown, the app can notify the user and request a new hostname.

The [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) namespace has convenient helper methods and uses enumerations in the [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) namespace for handling errors. This can be useful for handling specific network exceptions differently in your app.

An error encountered on an asynchronous method in the [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) namespace is returned as an **HRESULT** value. The [**BackgroundTransferError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701093) method is used to convert a network error from a background transfer operation to a [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818) enumeration value. Most of the **WebErrorStatus** enumeration values correspond to an error returned by the native HTTP or FTP client operation. An app can filter on specific **WebErrorStatus** enumeration values to modify app behavior depending on the cause of the exception.

For parameter validation errors, an app can also use the **HRESULT** from the exception to learn more detailed information on the error that caused the exception. Possible **HRESULT** values are listed in the *Winerror.h* header file. For most parameter validation errors, the **HRESULT** returned is **E\_INVALIDARG**.



<!--HONumber=Mar16_HO1-->


