---
author: mcleanbyron
ms.assetid: 2A454057-FF14-40D2-8ED2-CEB5F27E0226
description: "Verwenden Sie diese Methoden in der Windows Store-Übermittlungs-API, um Übermittlungen von Flight-Paketen für Apps zu verwalten, die in Ihrem Windows Dev Center-Konto registriert wurden."
title: "Verwalten von Flight-Paket-Übermittlungen mit der Windows Store-Übermittlungs-API"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 18d28495b80101cf5cfe53869b0f5cd3d61b50c9

---

# Verwalten von Flight-Paket-Übermittlungen mit der Windows Store-Übermittlungs-API




Verwenden Sie die folgenden Methoden in der Windows Store-Übermittlungs-API, um Übermittlungen von Flight-Paketen für Apps zu verwalten, die in Ihrem Windows Dev Center-Konto registriert wurden. Eine Einführung in die Windows Store-Übermittlungs-API einschließlich der Voraussetzungen für die Verwendung der API finden Sie unter [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md).

>**Hinweis**&nbsp;&nbsp;Diese Methoden können nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert. Um die folgenden Methoden für das Erstellen oder Verwalten von Flight-Paket-Übermittlungen verwenden zu können, muss das Flight-Paket bereits in Ihrem Dev Center-Konto vorhanden sein. Sie können Flight-Pakete [über das Dev Center-Dashboard](https://msdn.microsoft.com/windows/uwp/publish/package-flights) oder mithilfe der Methoden für die Windows Store-Übermittlungs-API erstellen, die in [Verwalten von Flight-Paketen](manage-flights.md) beschrieben werden.

| Methode        | URI    | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` | Ruft Daten für eine vorhandene Flight-Paket-Übermittlung ab. Weitere Informationen finden Sie unter [Abrufen einer Flight-Paket-Übermittlung](get-a-flight-submission.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status``` | Ruft den Status einer vorhandenen Flight-Paket-Übermittlung ab. Weitere Informationen finden Sie unter [Abrufen des Status einer Flight-Paket-Übermittlung](get-status-for-a-flight-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/applications/{applicationId}/flights/{flightId}/submissions``` | Erstellt eine neue Flight-Paket-Übermittlung für eine App, die in Ihrem Windows Dev Center-Konto registriert wurde. Weitere Informationen finden Sie unter [Erstellen einer Flight-Paket-Übermittlung](create-a-flight-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit``` | Führt ein Commit für eine neue oder aktualisierte Flight-Paket-Übermittlung an Windows Dev Center aus. Weitere Informationen finden Sie unter [Ausführen eines Commit für eine Flight-Paketübermittlung](commit-a-flight-submission.md). |
| PUT | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` | Aktualisiert eine vorhandene Flight-Paket-Übermittlung. Weitere Informationen finden Sie unter [Aktualisieren einer Flight-Paket-Übermittlung](update-a-flight-submission.md). |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` | Löscht eine Flight-Paket-Übermittlung. Weitere Informationen finden Sie unter [Löschen einer Flight-Paket-Übermittlung](delete-a-flight-submission.md). |

<span id="create-a-package-flight-submission">
## Erstellen einer Flight-Paket-Übermittlung

Um eine Übermittlung für ein Flight-Paket zu erstellen, führen Sie diesen Prozess aus.

1. Wenn noch nicht erfolgt, erfüllen Sie die Voraussetzungen, wie in [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md) beschrieben, einschließlich des Verknüpfens einer Azure AD-Anwendung mit Ihrem Windows Dev Center-Konto und des Abrufens von Client-ID und Schlüssel. Sie müssen dies nur einmal durchführen. nachdem Sie Client-ID und Schlüssel erhalten haben, können Sie diese jedes Mal wiederverwenden, wenn Sie ein neues Azure AD-Token erstellen müssen.  

2. [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Sie müssen dieses Zugriffstoken an die Methoden in der Windows Store-Übermittlungs-API übergeben. Sie können ein abgerufenes Zugriffstoken 60Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

3. Führen Sie die folgende Methode in der Windows Store-Übermittlungs-API aus. Diese Methode erstellt eine neue laufende Übermittlung, die eine Kopie der letzten veröffentlichten Übermittlung ist. Weitere Informationen finden Sie unter [Erstellen einer Flight-Paket-Übermittlung](create-a-flight-submission.md).

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications{applicationId}/flights/{flightId}/submissions
  ```

  Der Antworttext enthält drei Elemente: die ID der neuen Übermittlung, die Daten für die neue Übermittlung (einschließlich aller Einträge und Preisinformationen) und die Shared Access-Signatur (SAS)-URI, um Pakete für die Übermittlung hochzuladen. Weitere Informationen zu SAS finden Sie unter [Shared Access-Signaturen, Teil 1: Verstehen des SAS-Modells](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).

4. Wenn Sie neue Pakete für die Übermittlung hinzufügen, müssen Sie [die Pakete vorbereiten](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements) und einem ZIP-Archiv hinzufügen.

5. Aktualisieren Sie die Übermittlungsdaten mit alle erforderlichen Änderungen für die neue Übermittlung, und führen Sie die folgende Methode aus, um die Übermittlung zu aktualisieren. Weitere Informationen finden Sie unter [Aktualisieren einer Flight-Paket-Übermittlung](update-a-flight-submission.md).

  ```
  PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}
  ```

  >**Hinweis**&nbsp;&nbsp;Wenn Sie neue Pakete für die Übermittlung hinzufügen, müssen Sie die Übermittlungsdaten aktualisieren, damit diese auf den Namen und den relativen Pfad dieser Dateien im ZIP-Archiv verweisen.

4. Wenn Sie neue Pakete für die Übermittlung hinzufügen, müssen Sie das ZIP-Archiv zu der SAS-URI hochladen, die im Antworttext der POST-Methode bereitgestellt wurde, die Sie in Schritt2 aufgerufen haben. Weitere Informationen finden Sie unter [Shared Access-Signaturen, Teil 2: Erstellen und Verwenden einer SAS mit BLOB-Speicher](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

  Der folgende Codeausschnitt zeigt, wie Sie das Archiv mithilfe der [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx)-Klasse in der Azure Storage-Clientbibliothek für.NET hochladen.

  ```csharp
  string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";

  Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
  await blockBob.UploadFromStreamAsync(stream);
  ```

5. Sie führen das Commit für die Übermittlung durch Ausführen der folgenden Methode aus. Hierdurch wird Dev Center darüber benachrichtigt, dass Sie Ihre Übermittlung fertig gestellt haben und die Updates nun auf Ihr Konto jetzt angewendet werden sollen. Weitere Informationen finden Sie unter [Ausführen eines Commit für eine Flight-Paketübermittlung](commit-a-flight-submission.md).

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit
  ```

6. Sie überprüfen den Status des Commit durch Ausführen der folgenden Methode. Weitere Informationen finden Sie unter [Abrufen des Status einer Flight-Paket-Übermittlung](get-status-for-a-flight-submission.md).

  ```
  GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status
  ```

  Um den Status der Übermittlung zu überprüfen, zeigen Sie den Wert *status* im Antworttext an. Dieser Wert sollte von **CommitStarted** entweder in **PreProcessing** geändert worden sein, wenn die Anforderung erfolgreich war, oder in **CommitFailed**, wenn die Anforderung Fehler enthalten hat. Wenn Fehler aufgetreten sind, enthält das Feld *StatusDetails* Feld weitere Details zu den Fehlern.

7. Nachdem das Commit erfolgreich abgeschlossen wurde, wird die Übermittlung zur Aufnahme an den Store gesendet. Sie können die Übermittlung mithilfe der vorherigen Methode oder durch Aufruf des Dev Center-Dashboards weiter überwachen.

## Ressourcen

Diese Methoden verwenden die folgenden Datenressourcen.

<span id="flight-submission-object" />
### Flight-Paket-Übermittlung

Diese Ressource stellt eine Übermittlung für ein Flight-Paket dar. Das folgende Beispiel veranschaulicht das Format der Ressource.

```json
{
  "id": "1152921504621243649",
  "flightId": "cd2e368a-0da5-4026-9f34-0e7934bc6f23",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

Diese Ressource hat die folgenden Werte.

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | string  | Die ID für die Übermittlung.  |
| flightId           | string  |  Die ID des Flight-Pakets, dem die Übermittlung zugeordnet ist.  |  
| status           | string  | Der Status der Übermittlung. Folgende Werte sind möglich: <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  Enthält zusätzliche Details über den Status der Übermittlung, einschließlich Informationen zu Fehlern. Weitere Informationen finden Sie unten im Abschnitt [Statusdetails](#status-details-object). |
| flightPackages           | array  | Enthält Objekte, die Details über die einzelnen Pakete der Übermittlung bereitstellen. Weitere Informationen finden Sie unten im Abschnitt [Flight-Paket](#flight-package-object).  |
| fileUploadUrl           | string  | Die Shared Access-(SAS)-URI für das Hochladen der Pakete für die Übermittlung. Wenn Sie neue Pakete oder Bilder für die Übermittlung hinzufügen, müssen Sie das ZIP-Archiv, das die Pakete enthält, zu dieser URI hochladen. Weitere Informationen finden Sie unter [Erstellen einer Flight-Paket-Übermittlung](#create-a-package-flight-submission).  |
| targetPublishMode           | string  | Der Veröffentlichungsmodus für die Übermittlung. Folgende Werte sind möglich: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Das Veröffentlichungsdatum der Übermittlung im ISO 8601-Format, wenn *TargetPublishMode* den Wert SpecificDate hat.  |
| notesForCertification           | string  |  Enthält zusätzliche Informationen für Zertifizierungstester wie Anmeldeinformationen für Testkonten und Schritte zum Zugriff auf und zur Überprüfung von Features. Weitere Informationen finden Sie unter [Hinweise zur Zertifizierung](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification). |

<span id="status-details-object" />
### Statusdetails

Diese Ressource enthält weitere Informationen über den Status einer Übermittlung. Diese Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  errors               |    object     |   Ein Array von Objekten, die Fehlerdetails für die Übermittlung enthalten. Weitere Informationen finden Sie unten im Abschnitt [Statusdetails](#status-detail-object).   |     
|  warnings               |   object      | Ein Array von Objekten, die Warnungsdetails für die Übermittlung enthalten. Weitere Informationen finden Sie unten im Abschnitt [Statusdetails](#status-detail-object).     |
|  certificationReports               |     object    |   Ein Array von Objekten, die den Zugriff auf die Zertifizierungsberichtdaten für die Übermittlung bereitstellen. Sie können diese Berichte auf weitere Informationen überprüfen, wenn die Zertifizierung nicht erfolgreich ist. Weitere Informationen finden Sie unten im Abschnitt [Zertifizierungsbericht](#certification-report-object).   |  


<span id="status-detail-object" />
### Statusdetails

Diese Ressource enthält weitere Informationen zu Fehlern oder Warnungen für eine Übermittlung. Diese Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  code               |    string     |   Eine Zeichenfolge, die den Typ des Fehlers oder der Warnung beschreibt. Weitere Informationen finden Sie unten im Abschnitt [Übermittlungsstatuscode](#submission-status-code).   |     
|  details               |     string    |  Eine Meldung mit weiteren Details zum Problem.     |


<span id="certification-report-object" />
### Zertifizierungsbericht

Diese Ressource stellt den Zugriff auf die Zertifizierungsberichtdaten für eine Übermittlung bereit. Diese Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|------|
|     date            |    string     |  Das Datum und die Zeit im Format ISO8601, an dem und zu der der Bericht erstellte wurde.    |
|     reportUrl            |    string     |  Die URL, unter der Sie auf den Bericht zugreifen können.    |


<span id="flight-package-object" />
### Flight-Paket

Diese Ressource enthält Details zu einem Paket in einer Übermittlung. Das folgende Beispiel veranschaulicht das Format der Ressource.

```json
{
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
}
```

Diese Ressource hat die folgenden Werte.

>**Hinweis**&nbsp;&nbsp;Beim Aufruf der Methode für das [Aktualisieren eines Flight-Pakets](update-a-flight-submission.md) sind im Anforderungstext nur die Werte *fileName*, *fileStatus*, *minimumDirectXVersion* und *minimumSystemRam* dieses Objekts erforderlich. Die übrigen Werte werden von Dev Center aufgefüllt.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|------|
| fileName   |   string      |  Der Name des Pakets.    |  
| fileStatus    | string    |  Der Status des Pakets. Folgende Werte sind möglich: <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>    |  
| id    |  string   |  Eine ID, die das Paket eindeutig identifiziert. Dieser Wert wird von Dev Center verwendet.   |     
| version    |  string   |  Die Version des App-Pakets. Weitere Informationen finden Sie unter [Paketversionsnummern](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering).   |   
| architecture    |  string   |  Die Architektur des App-Pakets (z.B. ARM).   |     
| languages    | array    |  Ein Array von Sprachcodes für die Sprachen, die von der App unterstützt werden. Weitere Informationen finden Sie unter [Unterstützte Sprachen](https://msdn.microsoft.com/windows/uwp/publish/supported-languages).    |     
| capabilities    |  array   |  Ein Array von Funktionen, die für das Paket erforderlich sind. Weitere Informationen zu Funktionen finden Sie unter [Deklaration der App-Funktionen](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations).   |     
| minimumDirectXVersion    |  string   |  Die DirectX-Version, die vom App-Paket mindestens unterstützt wird. Dieser Wert kann nur für Apps festgelegt werden, die für Windows8.x bestimmt sind. Im Fall von Apps, die für andere Versionen bestimmt sind, wird er ignoriert. Folgende Werte sind möglich: <ul><li>None</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | string    |  Die Menge an RAM, die für das App-Paket mindestens erforderlich ist. Dieser Wert kann nur für Apps festgelegt werden, die für Windows8.x bestimmt sind. Im Fall von Apps, die für andere Versionen bestimmt sind, wird er ignoriert. Folgende Werte sind möglich: <ul><li>None</li><li>Memory2GB</li></ul>   |    

<span/>

## Enumerationen

Diese Methoden verwenden die folgenden Enumerationen.

<span id="submission-status-code" />
### Übermittlungsstatuscode

Die folgenden Codes stellen den Status einer Übermittlung dar.

| Code           |  Beschreibung      |
|-----------------|---------------|
|  None            |     Es wurde kein Code angegeben.         |     
|      InvalidArchive        |     Das ZIP-Archiv, das das Paket enthält, ist ungültig oder hat ein unbekanntes Archivformat.  |
| MissingFiles | Das ZIP-Archiv enthält nicht alle Dateien, die in den Übermittlungsdaten aufgeführt sind, oder sie befinden sich am falschen Speicherort im Archiv. |
| PackageValidationFailed | Mindestens ein Paket in der Übermittlung konnte nicht überprüft werden. |
| InvalidParameterValue | Einer der Parameter im Anforderungstext ist ungültig. |
| InvalidOperation | Der von Ihnen versuchte Vorgang ist ungültig. |
| InvalidState | Der von Ihnen versuchte Vorgang ist für den aktuellen Zustand des Flight-Pakets ungültig. |
| ResourceNotFound | Das angegebene Flight-Paket konnte nicht gefunden werden. |
| ServiceError | Ein interner Dienstfehler hat verhindert, dass die Anforderung erfolgreich ausgeführt wurde. Führen Sie die Anforderung erneut aus. |
| ListingOptOutWarning | Der Entwickler hat einen Eintrag aus einer vorherigen Übermittlung entfernt oder Eintragsinformationen nicht hinzugefügt, die vom Paket unterstützt werden. |
| ListingOptInWarning  | Der Entwickler hat einen Eintrag hinzugefügt. |
| UpdateOnlyWarning | Der Entwickler versucht, etwas einzufügen, für das nur Aktualisierungsunterstützung verfügbar ist. |
| Other  | Die Übermittlung befindet sich in einem nicht erkannten oder nicht kategorisierten Zustand. |
| PackageValidationWarning | Der Paketüberprüfungsvorgang hat zu einer Warnung geführt. |

<span/>

## Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Flight-Paketen mithilfe der Windows Store-Übermittlungs-API](manage-flights.md)
* [Abrufen einer Flight-Paket-Übermittlung](get-a-flight-submission.md)
* [Erstellen einer Flight-Paket-Übermittlung](create-a-flight-submission.md)
* [Aktualisieren einer Flight-Paket-Übermittlung](update-a-flight-submission.md)
* [Ausführen eines Commit für eine Flight-Paket-Übermittlung](commit-a-flight-submission.md)
* [Löschen einer Flight-Paket-Übermittlung](delete-a-flight-submission.md)
* [Abrufen des Status einer Flight-Paket-Übermittlung](get-status-for-a-flight-submission.md)



<!--HONumber=Aug16_HO5-->


