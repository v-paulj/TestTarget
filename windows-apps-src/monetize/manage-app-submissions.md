---
author: mcleanbyron
ms.assetid: C7428551-4B31-4259-93CD-EE229007C4B8
description: "Verwenden Sie diese Methoden in der Windows Store-Übermittlungs-API, um Übermittlungen für Apps zu verwalten, die in Ihrem Windows Dev Center-Konto registriert wurden."
title: "Verwalten von App-Übermittlungen mithilfe der Windows Store-Übermittlungs-API"
translationtype: Human Translation
ms.sourcegitcommit: 178b70db1583790c174d65e060c8bce6e4f69243
ms.openlocfilehash: 448eafbdadb21476da43e7408bb8bad354ba486d

---

# Verwalten von App-Übermittlungen mithilfe der Windows Store-Übermittlungs-API




Verwenden Sie diese Methoden in der Windows Store-Übermittlungs-API, um Übermittlungen für Apps zu verwalten, die in Ihrem Windows Dev Center-Konto registriert wurden. Eine Einführung in die Windows Store-Übermittlungs-API einschließlich der Voraussetzungen für die Verwendung der API finden Sie unter [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md).

>**Hinweis**&nbsp;&nbsp;Diese Methoden können nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert.


| Methode        | URI    | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}``` | Ruft Daten für eine vorhandene App-Übermittlung ab. Weitere Informationen finden Sie unter [Abrufen einer App-Übermittlung](get-an-app-submission.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status``` | Ruft den Status einer vorhandenen App-Übermittlung ab. Weitere Informationen finden Sie unter [Abrufen des Status einer App-Übermittlung](get-status-for-an-app-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions``` | Erstellt eine neue Übermittlung für eine App, die in Ihrem Windows Dev Center-Konto registriert wurde. Weitere Informationen finden Sie unter [Erstellen einer App-Übermittlung](create-an-app-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit``` | Führt ein Commit für eine neue oder aktualisierte App-Übermittlung an Windows Dev Center aus. Weitere Informationen finden Sie unter [Ausführen eines Commit für eine App-Übermittlung](commit-an-app-submission.md). |
| PUT | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}``` | Aktualisiert eine vorhandene App-Übermittlung. Weitere Informationen finden Sie unter [Aktualisieren einer App-Übermittlung](update-an-app-submission.md). |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}``` | Löscht einer App-Übermittlung. Weitere Informationen finden Sie unter [Löschen einer App-Übermittlung](delete-an-app-submission.md). |

<span id="create-an-app-submission">
## Erstellen einer App-Übermittlung

Folgen Sie diesem Prozess, um eine Übermittlung für eine App zu erstellen.

1. Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Windows Store-Übermittlungs-API.

  >**Hinweis**&nbsp;&nbsp;Stellen Sie sicher, dass für die App mindestens eine abgeschlossene Übermittlung mit abgeschlossenen Informationen zu den [Altersfreigaben](https://msdn.microsoft.com/windows/uwp/publish/age-ratings) vorhanden ist.

3. [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Sie müssen dieses Zugriffstoken an die Methoden in der Windows Store-Übermittlungs-API übergeben. Sie können ein abgerufenes Zugriffstoken 60Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

4. Führen Sie die folgende Methode in der Windows Store-Übermittlungs-API aus. Diese Methode erstellt eine neue laufende Übermittlung, die eine Kopie der letzten veröffentlichten Übermittlung ist. Weitere Informationen finden Sie unter [Erstellen einer App-Übermittlung](create-an-app-submission.md).

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions
  ```

  Der Antworttext enthält drei Elemente: die ID der neuen Übermittlung, die Daten für die neue Übermittlung (einschließlich aller Einträge und Preisinformationen) und die Shared Access-Signatur (SAS)-URI, um alle App-Pakete und Eintragsbilder für die Übermittlung hochzuladen. Weitere Informationen zu SAS finden Sie unter [Shared Access-Signaturen, Teil 1: Verstehen des SAS-Modells](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).

3. Wenn Sie neue Pakete oder Bilder für die Übermittlung hinzufügen, [müssen Sie die App-Pakete vorbereiten](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements) und auch [die App-Screenshots und -Bilder vorbereiten](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images). Fügen Sie all diese Dateien einem ZIP-Archiv hinzu.

4. Aktualisieren Sie die Übermittlungsdaten mit alle erforderlichen Änderungen für die neue Übermittlung, und führen Sie die folgende Methode aus, um die Übermittlung zu aktualisieren. Weitere Informationen finden Sie unter [Aktualisieren einer App-Übermittlung](update-an-app-submission.md).

  ```
  PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}
  ```

  >**Hinweis**&nbsp;&nbsp;Wenn Sie neue Pakete oder Bilder für die Übermittlung hinzufügen, müssen Sie die Übermittlungsdaten aktualisieren, damit diese auf den Namen und den relativen Pfad dieser Dateien im ZIP-Archiv verweisen.

4. Wenn Sie neue Pakete oder Bilder für die Übermittlung hinzufügen, müssen Sie das ZIP-Archiv zu der SAS-URI hochladen, die im Antworttext der POST-Methode bereitgestellt wurde, die Sie in Schritt2 aufgerufen haben. Weitere Informationen finden Sie unter [Shared Access-Signaturen, Teil 2: Erstellen und Verwenden einer SAS mit BLOB-Speicher](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

  Der folgende Codeausschnitt zeigt, wie Sie das Archiv mithilfe der [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx)-Klasse in der Azure Storage-Clientbibliothek für .NET hochladen.

  ```csharp
  string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";

  Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
  await blockBob.UploadFromStreamAsync(stream);
  ```

5. Sie führen das Commit für die Übermittlung durch Ausführen der folgenden Methode aus. Hierdurch wird Dev Center darüber benachrichtigt, dass Sie Ihre Übermittlung fertig gestellt haben und die Updates nun auf Ihr Konto jetzt angewendet werden sollen. Weitere Informationen finden Sie unter [Ausführen eines Commit für eine App-Übermittlung](commit-an-app-submission.md).

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit
  ```

6. Sie überprüfen den Status des Commit durch Ausführen der folgenden Methode. Weitere Informationen finden Sie unter [Abrufen des Status einer App-Übermittlung](get-status-for-an-app-submission.md).

    ```
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status
    ```

    Um den Status der Übermittlung zu überprüfen, zeigen Sie den Wert *status* im Antworttext an. Dieser Wert sollte von **CommitStarted** entweder in **PreProcessing** geändert worden sein, wenn die Anforderung erfolgreich war, oder in **CommitFailed**, wenn die Anforderung Fehler enthalten hat. Wenn Fehler aufgetreten sind, enthält das Feld *StatusDetails* Feld weitere Details zu den Fehlern.

7. Nachdem das Commit erfolgreich abgeschlossen wurde, wird die Übermittlung zur Aufnahme an den Store gesendet. Sie können die Übermittlung mithilfe der vorherigen Methode oder durch Aufruf des Dev Center-Dashboards weiter überwachen.

## Ressourcen

Diese Methoden verwenden die folgenden Ressourcen zum Formatieren von Daten.

<span id="app-submission-object" />
### App-Übermittlung

Diese Ressource stellt eine Übermittlung für eine App dar. Das folgende Beispiel veranschaulicht das Format der Ressource.

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "ApiTestApp For Devbox"
      },
      "platformOverrides": {}
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/387a9ea8-a412-43a9-8fb3-a38d03eb483d?sv=2014-02-14&sr=b&sig=sdd12JmoaT6BhvC%2BZUrwRweA%2Fkvj%2BEBCY09C2SZZowg%3D&se=2016-06-17T18:32:26Z&sp=rwl",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "Uploaded",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
    }
  ],
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "friendlyName": "Submission 2"
}
```

Diese Ressource hat die folgenden Werte.

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | string  | Die ID der Übermittlung.  |
| applicationCategory           | string  |   Eine Zeichenfolge, die die [Kategorie und/oder Unterkategorie](https://msdn.microsoft.com/windows/uwp/publish/category-and-subcategory-table) für Ihre App angibt. Kategorien und Unterkategorien werden mit einem Unterstrich „_“ zu einer einzigen Zeichenfolge zusammengefasst, z.B. **BooksAndReference_EReader**.      |  
| pricing           |  object  | Ein Objekt, das Preisinfos für die App enthält. Weitere Informationen finden Sie unten im Abschnitt [Preisressource](#pricing-object).       |   
| visibility           |  string  |  Die Sichtbarkeit der App. Folgende Werte sind möglich: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>       |   
| targetPublishMode           | string  | Der Veröffentlichungsmodus für die Übermittlung. Folgende Werte sind möglich: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Das Veröffentlichungsdatum der Übermittlung im ISO 8601-Format, wenn *TargetPublishMode* den Wert SpecificDate hat.  |  
| listings           |   object  |  Ein Verzeichnis von Schlüssel-Wert-Paaren, wobei ein Schlüssel ein Ländercode und ein Wert ein [Eintragsressourcen](#listing-object)-Objekt ist, das Eintragsinfos für die App enthält.       |   
| hardwarePreferences           |  array  |   Ein Array von Zeichenfolgen, die die [Hardwareeinstellungen](https://msdn.microsoft.com/windows/uwp/publish/enter-app-properties#hardware_preferences) für die App definieren. Folgende Werte sind möglich: <ul><li>Touch</li><li>Keyboard</li><li>Mouse</li><li>Kamera</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| automaticBackupEnabled           |  boolean  |   Gibt an, ob Windows die App-Daten in automatische Sicherungen auf OneDrive aufnehmen kann. Weitere Informationen finden Sie unter [App-Deklarationen](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).   |   
| canInstallOnRemovableMedia           |  boolean  |   Gibt an, ob Kunden die App auf Wechselmedien installieren können. Weitere Informationen finden Sie unter [App-Deklarationen](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| isGameDvrEnabled           |  boolean |   Gibt an, ob game DVR für die App aktiviert ist.    |   
| hasExternalInAppProducts           |     boolean          |   Gibt an, ob die App Benutzern Käufe außerhalb des E-Commerce-Systems des Windows Store gestattet. Weitere Informationen finden Sie unter [App-Deklarationen](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| meetAccessibilityGuidelines           |    boolean           |  Gibt an, ob getestet wurde, ob die App die Richtlinien zur Barrierefreiheit erfüllt. Weitere Informationen finden Sie unter [App-Deklarationen](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).      |   
| notesForCertification           |  string  |   Enthält [Hinweise zur Zertifizierung](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification) für Ihre App.    |    
| status           |   string  |  Der Status der Übermittlung. Folgende Werte sind möglich: <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>      |    
| statusDetails           |   object  |  Enthält zusätzliche Details über den Status der Übermittlung, einschließlich Informationen zu Fehlern. Weitere Informationen finden Sie unten im Abschnitt [Statusdetails](#status-details-object).       |    
| fileUploadUrl           |   string  | Die Shared Access-(SAS)-URI für das Hochladen der Pakete für die Übermittlung. Wenn Sie neue Pakete oder Bilder für die Übermittlung hinzufügen, müssen Sie das ZIP-Archiv, das die Pakete und Bilder enthält, zu dieser URI hochladen. Weitere Informationen finden Sie unter [Erstellen einer App-Übermittlung](#create-an-app-submission).       |    
| applicationPackages           |   array  | Enthält Objekte, die Details über die einzelnen Pakete der Übermittlung bereitstellen. Weitere Informationen finden Sie unten im Abschnitt [Anwendungspaket](#application-package-object). |    
| enterpriseLicensing           |  string  |  Einer der Werte für die [Enterprise-Lizenzierung](#enterprise-licensing), die das Verhalten der Enterprise-Lizenzierung für die App angeben.  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  boolean   |  Gibt an, ob Microsoft [die App für zukünftige Windows10-Gerätefamilien verfügbar machen](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families) darf.    |    
| allowTargetFutureDeviceFamilies           | object   |  Ein Verzeichnis von Schlüssel-Wert-Paaren, wobei jeder Schlüssel eine [Windows10-Gerätefamilie](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families) ist und jeder Wert ein boolescher Wert ist, der angibt, ob Ihre App auf die angegebene Gerätefamilie ausgerichtet werden darf.     |    
| friendlyName           |   string  |  Der Anzeigename der App, der für die Anzeige verwendet wird.       |  


<span id="listing-object" />
### Eintrag

Diese Ressource enthält die Eintragsinformationen für eine App. Diese Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  baseListing               |   object      |  Die Informationen für den [Basiseintrag](#base-listing-object) für die App, die die standardmäßigen Eintragsinformationen für alle Plattformen definiert.   |     
|  platformOverrides               | object |   Ein Verzeichnis von Schlüssel-Wert-Paaren, in denen jeder Schlüssel eine Zeichenfolge ist, die eine Plattform identifiziert, für die die Eintragsinformationen überschrieben werden sollen, und jeder Wert ein [Basiseintrag](#base-listing-object)-Objekt ist (das nur die Werte von Beschreibung bis Titel enthält), das die Eintragsinformationen angibt, die für die angegebene Plattform überschrieben werden sollen. Die Schlüssel können die folgenden Werte haben: <ul><li>Unknown</li><li>Windows80</li><li>Windows81</li><li>WindowsPhone71</li><li>WindowsPhone80</li><li>WindowsPhone81</li></ul>     |      |     

<span id="base-listing-object" />
### Basiseintrag

Diese Ressource enthält die Basiseintragsinformationen für eine App. Diese Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  copyrightAndTrademarkInfo                |   string      |  Optionale [Copyright- und/oder Markeninformationen](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#copyright-and-trademark-info).  |
|  keywords                |  array       |  Ein Array von [keyword](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#keywords), um die Anzeige Ihrer App in Suchergebnissen zu unterstützen.    |
|  licenseTerms                |    string     | Die optionalen [Lizenzbestimmungen](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#additional-license-terms) für Ihre App.     |
|  privacyPolicy                |   string      |   Die URL für die [Datenschutzrichtlinie](https://msdn.microsoft.com/windows/uwp/publish/privacy-policy) für Ihre App.    |
|  supportContact                |   string      |  Die URL oder E-Mail-Adresse für die [Kontaktinformationen für den Support](https://msdn.microsoft.com/windows/uwp/publish/support-contact-info) für Ihre App.     |
|  websiteUrl                |   string      |  Die URL der [Webseite](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#website) für Ihre App.    |    
|  description               |    string     |   Die [Beschreibung](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#description) für den App-Eintrag.   |     
|  features               |    array     |  Ein Array von bis zu 20Zeichenfolgen, die die [Features](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#app-features) für Ihre App auflisten.     |
|  releaseNotes               |  string       |  Die [Versionshinweise](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#release-notes) für Ihre App.    |
|  images               |   array      |  Ein Array von [Bild- und Symbol](#image-object)-Daten für den App-Eintrag.  |
|  recommendedHardware               |   array      |  Ein Array von bis zu 11Zeichenfolgen, die die [empfohlenen Hardwarekonfigurationen](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#recommended-hardware) für Ihre App auflisten.     |
|  title               |     string    |   Der Titel für den App-Eintrag.   |  


<span id="image-object" />
### Bild

Diese Ressource enthält Bild- und Symboldaten für einen App-Eintrag. Weitere Informationen zu Bildern und Symbolen für Einträge finden Sie unter [App-Screenshots und -Bilder](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images). Diese Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  fileName               |    string     |   Der Name der Bilddatei im ZIP-Archiv, das Sie für die Übermittlung hochgeladen haben.    |     
|  fileStatus               |   string      |  Der Status der Bilddatei. Folgende Werte sind möglich: <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |
|  id  |  string  | Die ID für das Bild wie von Dev Center angegeben.  |
|  description  |  string  | Die Beschreibung für das Bild.  |
|  imageType  |  string  | Eine der folgenden Zeichenfolgen, die den Typ des Bilds angibt: <ul><li>Unknown</li><li>Screenshot</li><li>PromotionalArtwork414X180</li><li>PromotionalArtwork846X468</li><li>PromotionalArtwork558X756</li><li>PromotionalArtwork414X468</li><li>PromotionalArtwork558X558</li><li>PromotionalArtwork2400X1200</li><li>Icon</li><li>WideIcon358X173</li><li>BackgroundImage1000X800</li><li>SquareIcon358X358</li><li>MobileScreenshot</li><li>XboxScreenshot</li><li>SurfaceHubScreenshot</li><li>HoloLensScreenshot</li></ul>      |


<span id="pricing-object" />
### Preise

Diese Ressource enthält Preisinformationen für die App. Diese Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  trialPeriod               |    string     |  Eine Zeichenfolge, die den Testzeitraum für die App angibt. Folgende Werte sind möglich: <ul><li>NoFreeTrial</li><li>OneDay</li><li>TrialNeverExpires</li><li>SevenDays</li><li>FifteenDays</li><li>ThirtyDays</li></ul>    |
|  marketSpecificPricings               |    object     |  Ein Verzeichnis von Schlüssel-Wert-Paaren, wobei jeder Schlüssel ein aus zwei Buchstaben bestehender ISO3166-1-Alpha-2-Ländercode ist und jeder Wert ein [Preisniveau](#price-tiers) ist. Diese Elemente stellen die [benutzerdefinierten Preise für Ihre App in bestimmten Märkten](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices) dar. Alle Elemente in diesem Verzeichnis überschreiben für den angegebenen Markt den Basispreis, der durch den Wert *priceId* angegeben wird.      |     
|  sales               |   array      |  Ein Array von Objekten, die Verkaufsinformationen für die App enthalten. Weitere Informationen finden Sie unten im Abschnitt [Verkauf](#sale-object).    |     
|  priceId               |   string      |  Ein [Preisniveau](#price-tier), das den [Basispreis](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#base-price) für die App angibt.   |


<span id="sale-object" />
### Verkauf

Diese Ressource enthält die Verkaufsinformationen für eine App. Diese Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  name               |    string     |   Der Name des Verkaufs.    |     
|  basePriceId               |   string      |  Das [Preisniveau](#price-tiers), das für den Basispreis des Verkaufs verwendet werden soll.    |     
|  startDate               |   string      |   Das Startdatum für den Verkauf im Format ISO8601.  |     
|  endDate               |   string      |  Das Enddatum für den Verkauf im Format ISO8601.      |     
|  marketSpecificPricings               |   object      |   Ein Verzeichnis von Schlüssel-Wert-Paaren, wobei jeder Schlüssel ein aus zwei Buchstaben bestehender ISO3166-1-Alpha-2-Ländercode ist und jeder Wert ein [Preisniveau](#price-tiers) ist. Diese Elemente stellen die [benutzerdefinierten Preise für Ihre App in bestimmten Märkten](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices) dar. Alle Elemente in diesem Verzeichnis überschreiben für den angegebenen Markt den Basispreis, der durch den Wert *basePriceId* angegeben wird.    |


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


<span id="application-package-object" />
### Anwendungspaket

Diese Ressource enthält Details zu einem App-Paket für die Einreichung. Das folgende Beispiel veranschaulicht das Format der Ressource.

```json
{
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "Uploaded",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
    }
  ],
}
```

Diese Ressource hat die folgenden Werte.  

>**Hinweis**&nbsp;&nbsp;Beim Aufruf der Methode [update an app submission](update-an-app-submission.md) sind im Anforderungstext nur die Werte *fileName*, *fileStatus*, *minimumDirectXVersion* und *minimumSystemRam* dieses Objekts erforderlich. Die übrigen Werte werden von Dev Center aufgefüllt.

| Wert           | Typ    | Beschreibung                   |
|-----------------|---------|------|
| fileName   |   string      |  Der Name des Pakets.    |  
| fileStatus    | string    |  Der Status des Pakets. Folgende Werte sind möglich: <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>    |  
| id    |  string   |  Eine ID, die das Paket eindeutig identifiziert. Dieser Wert wird von Dev Center verwendet.   |     
| version    |  string   |  Die Version des App-Pakets. Weitere Informationen finden Sie unter [Paketversionsnummern](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering).   |   
| architecture    |  string   |  Die Architektur des Pakets (z.B. ARM).   |     
| languages    | array    |  Ein Array von Sprachcodes für die Sprachen, die von der App unterstützt werden. Weitere Informationen finden Sie unter [Unterstützte Sprachen](https://msdn.microsoft.com/windows/uwp/publish/supported-languages).    |     
| capabilities    |  array   |  Ein Array von Funktionen, die für das Paket erforderlich sind. Weitere Informationen zu Funktionen finden Sie unter [Deklaration der App-Funktionen](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations).   |     
| minimumDirectXVersion    |  string   |  Die DirectX-Version, die vom App-Paket mindestens unterstützt wird. Dieser Wert kann nur für Apps festgelegt werden, die für Windows8.x bestimmt sind. Im Fall von Apps, die für andere Versionen bestimmt sind, wird er ignoriert. Folgende Werte sind möglich: <ul><li>None</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | string    |  Die Menge an RAM, die für das App-Paket mindestens erforderlich ist. Dieser Wert kann nur für Apps festgelegt werden, die für Windows8.x bestimmt sind. Im Fall von Apps, die für andere Versionen bestimmt sind, wird er ignoriert. Folgende Werte sind möglich: <ul><li>None</li><li>Memory2GB</li></ul>   |       
| targetDeviceFamilies    | array    |  Ein Array von Zeichenfolgen, die die Gerätefamilien darstellen, auf die das Paket ausgerichtet ist. Dieser Wert wird nur für Pakete verwendet, die für Windows10 bestimmt sind. Im Fall von Paketen, die für frühere Versionen bestimmt sind, hat dieser Wert den Wert **None**. Die folgenden Gerätefamilienzeichenfolgen werden zurzeit für Windows10-Pakete unterstützt, wobei *{0}* eine Zeichenfolge für Windows10-Versionen ist, z.B. 10.0.10240.0, 10.0.10586.0 oder 10.0.14393.0: <ul><li>Windows.Universal min version *{0}*</li><li>Windows.Desktop min version *{0}*</li><li>Windows.Mobile min version *{0}*</li><li>Windows.Xbox min version *{0}*</li><li>Windows.Holographic min version *{0}*</li></ul>   |    

<span/>

<span id="certification-report-object" />
### Zertifizierungsbericht

Diese Ressource stellt den Zugriff auf die Zertifizierungsberichtdaten für eine Übermittlung bereit. Diese Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|------|
|     date            |    string     |  Das Datum und die Zeit im Format ISO8601, an dem und zu der der Bericht erstellte wurde.    |
|     reportUrl            |    string     |  Die URL, unter der Sie auf den Bericht zugreifen können.    |



## Enumerationen

Diese Methoden verwenden die folgenden Enumerationen.


<span id="price-tiers" />
### Preisniveaus

Die folgenden Werte stellen die verfügbaren Preisniveaus für eine App-Übermittlung dar.

| Wert           | Beschreibung                                                                                                                                                                                                                          |
|-----------------|------|
|  Base               |   Das Preisniveau ist nicht festgelegt. Verwenden Sie den Basispreis für die App.      |     
|  NotAvailable              |   Die App ist für die angegebene Region nicht verfügbar.    |     
|  Free              |   Die App ist kostenlos.    |    
|  Tier2 through Tier194               |   Tier2 stellt die Preisstufe 0,99 USD dar. Jede zusätzliche Stufe stellt zusätzliche Inkremente dar (1,29USD, 1,49USD, 1,99USD usw.).    |


<span id="enterprise-licensing" />
### Enterprise-Lizenzwerte

Die folgenden Werte stellen das Verhalten der App für die Enterprise-Lizenzierung dar. Weitere Informationen zu diesen Optionen finden Sie unter [Lizenzierungsoptionen für Unternehmen](https://msdn.microsoft.com/windows/uwp/publish/organizational-licensing).

| Wert           |  Beschreibung      |
|-----------------|---------------|
| None            |     Ihre App soll Unternehmen nicht über die Store-verwaltete Volumenlizenzierung (Onlinevolumenlizenzierung) zur Verfügung gestellt werden.         |     
| Online        |     Ihre App soll Unternehmen über die Store-verwaltete Volumenlizenzierung (Onlinevolumenlizenzierung) zur Verfügung gestellt werden.  |
| OnlineAndOffline | Ihre App soll Unternehmen über die Store-verwaltete Volumenlizenzierung (Onlinevolumenlizenzierung) sowie über die Offlinelizenzierung zur Verfügung gestellt werden. |


<span id="submission-status-code" />
### Übermittlungsstatuscode

Die folgenden Werte stellen den Statuscode einer Übermittlung dar.

| Wert           |  Beschreibung      |
|-----------------|---------------|
| None            |     Es wurde kein Code angegeben.         |     
| InvalidArchive        |     Das ZIP-Archiv, das das Paket enthält, ist ungültig oder hat ein unbekanntes Archivformat.  |
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
* [Abrufen von App-Daten mit der Windows Store-Übermittlungs-API](get-app-data.md)
* [App-Übermittlungen im Dev Center-Dashboard](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)



<!--HONumber=Aug16_HO5-->


