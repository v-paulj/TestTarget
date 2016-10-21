---
author: mcleanbyron
ms.assetid: 66400066-24BF-4AF2-B52A-577F5C3CA474
description: "Verwenden Sie diese Methoden in der Windows Store-Übermittlungs-API, um Übermittlungen von Add-Ons für Apps zu verwalten, die in Ihrem Windows Dev Center-Konto registriert wurden."
title: "Verwalten von Add-On-Übermittlungen mithilfe der Windows Store-Übermittlungs-API"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 52e589c90a8d78905a9617dc2802d76a2f0f0360

---

# Verwalten von Add-On-Übermittlungen mithilfe der Windows Store-Übermittlungs-API



Verwenden Sie die folgenden Methoden in der Windows Store-Übermittlungs-API, um Add-On-Übermittlungen (auch bekannt als In-App-Produkt- oder IAP-Übermittlungen) zu verwalten, die in Ihrem Windows Dev Center-Konto registriert wurden. Eine Einführung in die Windows Store-Übermittlungs-API einschließlich der Voraussetzungen für die Verwendung der API finden Sie unter [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md).

>**Hinweis**&nbsp;&nbsp;Diese Methoden können nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert. Um die folgenden Methoden für das Erstellen oder Verwalten von Add-Ons verwenden zu können, muss das Add-On bereits in Ihrem Dev Center-Konto vorhanden sein. Sie können Add-Ons [über das Dev Center-Dashboard](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions) oder mithilfe der Methoden der Windows Store-Übermittlungs-API erstellen, die in [Verwalten von Add-Ons](manage-add-ons.md) beschrieben werden.

| Methode        | URI    | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}``` | Ruft Daten für eine vorhandene Add-On-Übermittlung ab. Weitere Informationen finden Sie unter [Abrufen einer Add-On-Übermittlung](get-an-add-on-submission.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status``` | Ruft den Status einer vorhandenen Add-On-Übermittlung ab. Weitere Informationen finden Sie unter [Abrufen des Status einer Add-On-Übermittlung](get-status-for-an-add-on-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions``` | Erstellt eine neue Add-On-Übermittlung für eine App, die in Ihrem Windows Dev Center-Konto registriert wurde. Weitere Informationen finden Sie unter [Erstellen einer Add-On-Übermittlung](create-an-add-on-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit``` | Führt ein Commit für eine neue oder aktualisierte Add-On-Übermittlung an Windows Dev Center aus. Weitere Informationen finden Sie unter [Ausführen eines Commits einer Add-On-Übermittlung](commit-an-add-on-submission.md). |
| PUT | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}``` | Aktualisiert eine vorhandene Add-On-Übermittlung. Weitere Informationen finden Sie unter [Aktualisieren einer Add-On-Übermittlung](update-an-add-on-submission.md). |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}``` | Löscht eine Add-On-Übermittlung. Weitere Informationen finden Sie unter [Löschen einer Add-On-Übermittlung](delete-an-add-on-submission.md). |

<span id="create-an-add-on-submission">
## Erstellen einer Add-On-Übermittlung

Folgen Sie diesem Prozess, um eine Übermittlung für ein Add-On zu erstellen.

1. Wenn noch nicht erfolgt, erfüllen Sie die Voraussetzungen, wie in [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md) beschrieben, einschließlich des Verknüpfens einer Azure AD-Anwendung mit Ihrem Windows Dev Center-Konto und des Abrufens von Client-ID und Schlüssel. Sie müssen dies nur einmal durchführen. nachdem Sie Client-ID und Schlüssel erhalten haben, können Sie diese jedes Mal wiederverwenden, wenn Sie ein neues Azure AD-Token erstellen müssen.  

2. [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Sie müssen dieses Zugriffstoken an die Methoden in der Windows Store-Übermittlungs-API übergeben. Sie können ein abgerufenes Zugriffstoken 60Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

3. Führen Sie die folgende Methode in der Windows Store-Übermittlungs-API aus. Diese Methode erstellt eine neue laufende Übermittlung, die eine Kopie der letzten veröffentlichten Übermittlung ist. Weitere Informationen finden Sie unter [Erstellen einer Add-On-Übermittlung](create-an-add-on-submission.md).

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions
  ```

  Der Antworttext enthält drei Elemente: die ID der neuen Übermittlung, die Daten für die neue Übermittlung (einschließlich aller Einträge und Preisinformationen) und die Shared Access-Signatur (SAS)-URI, um Add-On-Symbole für die Übermittlung hochzuladen. Weitere Informationen zu SAS finden Sie unter [Shared Access-Signaturen, Teil 1: Verstehen des SAS-Modells](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).

4. Wenn Sie neue Symbole für die Übermittlung hinzufügen, müssen Sie [die Symbole vorbereiten](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon) und einem ZIP-Archiv hinzufügen.

5. Aktualisieren Sie die Übermittlungsdaten mit alle erforderlichen Änderungen für die neue Übermittlung, und führen Sie die folgende Methode aus, um die Übermittlung zu aktualisieren. Weitere Informationen finden Sie unter [Aktualisieren einer Add-On-Übermittlung](update-an-add-on-submission.md).

  ```
  PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}
  ```

  >**Hinweis**&nbsp;&nbsp;Wenn Sie neue Symbole für die Übermittlung hinzufügen, müssen Sie die Übermittlungsdaten aktualisieren, damit diese auf den Namen und den relativen Pfad dieser Dateien im ZIP-Archiv verweisen.

4. Wenn Sie neue Symbole für die Übermittlung hinzufügen, müssen Sie das ZIP-Archiv zu der SAS-URI hochladen, die im Antworttext der POST-Methode bereitgestellt wurde, die Sie in Schritt2 aufgerufen haben. Weitere Informationen finden Sie unter [Shared Access-Signaturen, Teil 2: Erstellen und Verwenden einer SAS mit BLOB-Speicher](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

  Der folgende Codeausschnitt zeigt, wie Sie das Archiv mithilfe der [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx)-Klasse in der Azure Storage-Clientbibliothek für.NET hochladen.

  ```csharp
  string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";

  Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
  await blockBob.UploadFromStreamAsync(stream);
  ```

5. Sie führen das Commit für die Übermittlung durch Ausführen der folgenden Methode aus. Hierdurch wird Dev Center darüber benachrichtigt, dass Sie Ihre Übermittlung fertig gestellt haben und die Updates nun auf Ihr Konto jetzt angewendet werden sollen. Weitere Informationen finden Sie unter [Ausführen eines Commits einer Add-On-Übermittlung](commit-an-add-on-submission.md).

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit
  ```

6. Sie überprüfen den Status des Commit durch Ausführen der folgenden Methode. Weitere Informationen finden Sie unter [Abrufen des Status einer Add-On-Übermittlung](get-status-for-an-add-on-submission.md).

  ```
  GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status
  ```

  Um den Status der Übermittlung zu überprüfen, zeigen Sie den Wert *status* im Antworttext an. Dieser Wert sollte von **CommitStarted** entweder in **PreProcessing** geändert worden sein, wenn die Anforderung erfolgreich war, oder in **CommitFailed**, wenn die Anforderung Fehler enthalten hat. Wenn Fehler aufgetreten sind, enthält das Feld *StatusDetails* Feld weitere Details zu den Fehlern.

7. Nachdem das Commit erfolgreich abgeschlossen wurde, wird die Übermittlung zur Aufnahme an den Store gesendet. Sie können die Übermittlung mithilfe der vorherigen Methode oder durch Aufruf des Dev Center-Dashboards weiter überwachen.

## Ressourcen

Diese Methoden verwenden die folgenden Ressourcen zum Formatieren von Daten.

<span id="add-on-submission-object" />
### Add-On-Übermittlung

Diese Ressource stellt eine Übermittlung für ein Add-On dar. Das folgende Beispiel veranschaulicht das Format der Ressource.

```json
{
  "id": "1152921504621243680",
  "contentType": "EMagazine",
  "keywords": [
    "books"
  ],
  "lifetime": "FiveDays",
  "listings": {
    "en": {
      "description": "English add-on description",
      "icon": {
        "fileName": "add-on-en-us-listing2.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (English)"
    },
    "ru": {
      "description": "Russian add-on description",
      "icon": {
        "fileName": "add-on-ru-listing.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (Russian)"
    }
  },
  "pricing": {
    "marketSpecificPricings": {
      "RU": "Tier3",
      "US": "Tier4",
    },
    "sales": [
      {
         "name": "Sale1",
         "basePriceId": "Free",
         "startDate": "2016-05-21T18:40:11.7369008Z",
         "endDate": "2016-05-22T18:40:11.7369008Z",
         "marketSpecificPricings": {
            "RU": "NotAvailable"
         }
      }
    ],
    "priceId": "Free"
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [
      {
        "code": "None",
        "details": "string"
      }
    ],
    "warnings": [
      {
        "code": "ListingOptOutWarning",
        "details": "You have removed listing language(s): []"
      }
    ],
    "certificationReports": [
      {
      }
    ]
  },

    "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl",
    "friendlyName": "Submission 2"
}
```

Diese Ressource hat die folgenden Werte.

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | string  | Die ID der Übermittlung.  |
| contentType           | string  |  Der [Inhaltstyp](https://msdn.microsoft.com/windows/uwp/publish/enter-iap-properties#content-type), der im Add-On bereitgestellt wird. Folgende Werte sind möglich: <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>VideoDownload</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| keywords           | array  | Ein Array von Zeichenfolgen, das bis zu 10 [Schlüsselwörter](../publish/enter-iap-properties.md#keywords) für das Add-On enthalten kann. Die App kann mit diesen Schlüsselwörter Add-Ons abfragen.   |
| lifetime           | string  |  Die Lebensdauer des Add-Ons. Folgende Werte sind möglich: <ul><li>Forever</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| listings           | object  |  Ein Verzeichnis von Schlüssel-Wert-Paaren, wobei jeder Schlüssel ein aus zwei Buchstaben bestehender ISO3166-1-Alpha-2-Ländercode ist und jeder Wert ein [Eintragsressourcen](#listing-object)-Objekt ist, das Eintragsinformationen für das Add-On enthält.  |
| pricing           | object  | Ein Objekt, das Eintragsinformationen für das Add-On enthält. Weitere Informationen finden Sie unten im Abschnitt [Preisressource](#pricing-object).  |
| targetPublishMode           | string  | Der Veröffentlichungsmodus für die Übermittlung. Folgende Werte sind möglich: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Das Veröffentlichungsdatum der Übermittlung im ISO 8601-Format, wenn *TargetPublishMode* den Wert SpecificDate hat.  |
| tag           | string  |  Das [Tag](../publish/enter-iap-properties.md#tag) für das Add-On.   |
| visibility  | string  |  Die Sichtbarkeit des Add-Ons. Folgende Werte sind möglich: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>  |
| status  | string  |  Der Status der Übermittlung. Folgende Werte sind möglich: <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  Enthält zusätzliche Details über den Status der Übermittlung, einschließlich Informationen zu Fehlern. Weitere Informationen finden Sie unten im Abschnitt [Statusdetails](#status-details-object). |
| fileUploadUrl           | string  | Die Shared Access-(SAS)-URI für das Hochladen der Pakete für die Übermittlung. Wenn Sie neue Pakete oder Bilder für die Übermittlung hinzufügen, müssen Sie das ZIP-Archiv, das die Pakete enthält, zu dieser URI hochladen. Weitere Informationen finden Sie unter [Erstellen einer Add-On-Übermittlung](#create-an-add-on-submission).  |
| friendlyName  | string  |  Der Anzeigename des Add-Ons, der für die Anzeige verwendet wird.  |

<span id="listing-object" />
### Eintrag

Diese Ressource enthält die Eintragsinformationen für ein Add-On. Diese Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  description               |    string     |   Die Beschreibung für den Add-On-Eintrag.   |     
|  icon               |   object      |  Enthält Daten für das Symbol für den Add-On-Eintrag. Weitere Informationen finden Sie unten im Abschnitt [Symbol](#icon-object).   |
|  title               |     string    |   Der Titel für den Add-On-Eintrag.   |  

<span id="icon-object" />
### Icon

Diese Ressource enthält Symboldaten für einen Add-On-Eintrag. Diese Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  fileName               |    string     |   Der Name der Symboldatei im ZIP-Archiv, das Sie für die Übermittlung hochgeladen haben.    |     
|  fileStatus               |   string      |  Der Status der Symboldatei. Folgende Werte sind möglich: <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |

<span id="pricing-object" />
### Preise

Diese Ressource enthält Preisinformationen für das Add-On. Diese Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  marketSpecificPricings               |    object     |  Ein Verzeichnis von Schlüssel-Wert-Paaren, wobei jeder Schlüssel ein aus zwei Buchstaben bestehender ISO3166-1-Alpha-2-Ländercode ist und jeder Wert ein [Preisniveau](#price-tiers) ist. Diese Elemente stellen die [benutzerdefinierten Preise für Ihr Add-On in bestimmten Märkten](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-prices) dar. Alle Elemente in diesem Verzeichnis überschreiben für den angegebenen Markt den Basispreis, der durch den Wert *priceId* angegeben wird.     |     
|  sales               |   array      |  Ein Array von Objekten, die Verkaufsinformationen für das Add-On enthalten. Weitere Informationen finden Sie unten im Abschnitt [Verkauf](#sale-object).    |     
|  priceId               |   string      |  Ein [Preisniveau](#price-tier), das den [Basispreis](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#base-price) für das Add-On angibt.    |


<span id="sale-object" />
### Verkauf

Diese Ressource enthält die Verkaufsinformationen für ein Add-On. Diese Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  name               |    string     |   Der Name des Verkaufs.    |     
|  basePriceId               |   string      |  Das [Preisniveau](#price-tiers), das für den Basispreis des Verkaufs verwendet werden soll.    |     
|  startDate               |   string      |   Das Startdatum für den Verkauf im Format ISO8601.  |     
|  endDate               |   string      |  Das Enddatum für den Verkauf im Format ISO8601.      |     
|  marketSpecificPricings               |   object      |   Ein Verzeichnis von Schlüssel-Wert-Paaren, wobei jeder Schlüssel ein aus zwei Buchstaben bestehender ISO3166-1-Alpha-2-Ländercode ist und jeder Wert ein [Preisniveau](#price-tiers) ist. Diese Elemente stellen die [benutzerdefinierten Preise für Ihr Add-On in bestimmten Märkten](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-pricess) dar. Alle Elemente in diesem Verzeichnis überschreiben für den angegebenen Markt den Basispreis, der durch den Wert *basePriceId* angegeben wird.    |



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
|  Details               |     string    |  Eine Meldung mit weiteren Details zum Problem.     |


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

Die folgenden Werte stellen die verfügbaren Preisniveaus für eine Add-On-Übermittlung dar.

| Wert           | Beschreibung                                                                                                                                                                                                                          |
|-----------------|------|
|  Base               |   Das Preisniveau ist nicht festgelegt. Verwenden Sie den Basispreis für das Add-On.      |     
|  NotAvailable              |   Das Add-On ist für die angegebene Region nicht verfügbar.    |     
|  Free              |   Das Add-On ist kostenlos.    |    
|  Tier2 through Tier194               |   Tier2 stellt die Preisstufe 0,99 USD dar. Jede zusätzliche Stufe stellt zusätzliche Inkremente dar (1,29USD, 1,49USD, 1,99USD usw.).    |


<span id="submission-status-code" />
### Übermittlungsstatuscode

Die folgenden Werte stellen den Statuscode einer Übermittlung dar.

| Wert           |  Beschreibung      |
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
* [Verwalten von Add-Ons mithilfe der Windows Store-Übermittlungs-API](manage-add-ons.md)
* [Add-On-Übermittlungen im Dev Center-Dashboard](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)



<!--HONumber=Aug16_HO5-->


