---
author: mcleanbyron
ms.assetid: DD4F6BC4-67CD-4AEF-9444-F184353B0072
description: "Mittels dieser Methode in der Windows Store-Analyse-API können Sie gesammelte Bewertungsdaten für einen bestimmten Zeitraum und andere optionale Filter abrufen."
title: Abrufen von App-Bewertungen
translationtype: Human Translation
ms.sourcegitcommit: 6d0fa3d3b57bcc01234aac7d6856416fcf9f4419
ms.openlocfilehash: 8ec588ceb0a7c8bd6a75f72bf0a2d48c697a8e6a

---

# Abrufen von App-Bewertungen




Mittels dieser Methode in der Windows Store-Analyse-API können Sie gesammelte Bewertungsdaten für einen bestimmten Zeitraum und andere optionale Filter abrufen. Diese Methode gibt die Daten im JSON-Format zurück.

## Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Windows Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60Minuten Zeit, das Token zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.


## Anforderung


### Anforderungssyntax

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings``` |

 

### Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |

<span/> 

### Anforderungsparameter

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameter</th>
<th align="left">Typ</th>
<th align="left">Beschreibung</th>
<th align="left">Erforderlich</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">applicationId</td>
<td align="left">string</td>
<td align="left">Die Store-ID der App, für die Sie Bewertungsdaten abrufen möchten. Die Store-ID ist auf der [Seite mit der App-Identität](../publish/view-app-identity-details.md) des DevCenter-Dashboards verfügbar. Beispiel für eine Store-ID: 9WZDNCRFJ3Q8.</td>
<td align="left">Ja</td>
</tr>
<tr class="even">
<td align="left">startDate</td>
<td align="left">date</td>
<td align="left">Das Startdatum im Datumsbereich der Bewertungsdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum.</td>
<td align="left">Nein</td>
</tr>
<tr class="odd">
<td align="left">endDate</td>
<td align="left">date</td>
<td align="left">Das Enddatum im Datumsbereich der Bewertungsdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum.</td>
<td align="left">Nein</td>
</tr>
<tr class="even">
<td align="left">top</td>
<td align="left">int</td>
<td align="left">Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Wenn die Abfrage keine weiteren Zeilen enthält, entält der Antworttext den Link „Weiter“, den Sie verwenden können, um die nächste Seite mit Daten anzufordern.</td>
<td align="left">Nein</td>
</tr>
<tr class="odd">
<td align="left">skip</td>
<td align="left">int</td>
<td align="left">Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000Datenzeilen usw.</td>
<td align="left">Nein</td>
</tr>
<tr class="even">
<td align="left">filter</td>
<td align="left">string</td>
<td align="left">Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Weitere Informationen finden Sie unten im Abschnitt [Filterfelder](#filter-fields).</td>
<td align="left">Nein</td>
</tr>
<tr class="odd">
<td align="left">aggregationLevel</td>
<td align="left">string</td>
<td align="left">Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Dies kann eine der folgenden Zeichenfolgen sein: <strong>day</strong>, <strong>week</strong> oder <strong>month</strong>. Wenn keine Angabe erfolgt, lautet der Standardwert <strong>day</strong>.</td>
<td align="left">Nein</td>
</tr>
<tr class="even">
<td align="left">orderby</td>
<td align="left">string</td>
<td align="left">Eine Anweisung, die die Ergebnisdatenwerte für die einzelnen Bewertungen anfordert. Die Syntax ist <em>orderby=field [order],field [order],...</em>. Der Parameter <em>field</em> kann eine der folgenden Zeichenfolgen sein:
<ul>
<li><strong>date</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>market</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>isRevised</strong></li>
</ul>
<p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standard ist <strong>asc</strong>.</p>
<p>Dies ist eine Beispielzeichenfolge für <em>orderby</em>: <em>orderby=date,market</em></p></td>
<td align="left">Nein</td>
</tr>
</tbody>
</table>

<span/>
 
### Filterfelder

Der Parameter *Filter* der Anforderung enthält mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält ein Feld und einen Wert, das/der mit den Operatoren **eq** oder **ne** verknüpft ist. Anweisungen können mit **and** oder **or** kombiniert werden.

Dies ist eine Beispielzeichenfolge für *Filter*: *filter=market eq 'US' and deviceType eq 'phone' and isRevised eq true"*

Die Liste der unterstützten Felder finden Sie in der folgenden Tabelle. Zeichenfolgenwerte im Parameter *Filter* müssen von einfachen Anführungszeichen eingeschlossen werden.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Felder</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">market</td>
<td align="left">Eine Zeichenfolge, die den ISO 3166-Ländercode des Gerätemarkts enthält.</td>
</tr>
<tr class="even">
<td align="left">osVersion</td>
<td align="left">Eine der folgenden Zeichenfolgen:
<ul>
<li><strong>Windows Phone 7.5</strong></li>
<li><strong>Windows Phone 8</strong></li>
<li><strong>Windows Phone 8.1</strong></li>
<li><strong>Windows Phone 10</strong></li>
<li><strong>Windows8</strong></li>
<li><strong>Windows 8.1</strong></li>
<li><strong>Windows10</strong></li>
<li><strong>Unknown</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">deviceType</td>
<td align="left">Eine der folgenden Zeichenfolgen:
<ul>
<li><strong>PC</strong></li>
<li><strong>Tablet</strong></li>
<li><strong>Smartphone</strong></li>
<li><strong>IoT</strong></li>
<li><strong>Wearable</strong></li>
<li><strong>Server</strong></li>
<li><strong>Collaborative</strong></li>
<li><strong>Other</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">isRevised</td>
<td align="left">Geben Sie <strong>true</strong> an, um nach Bewertungen zu filtern, die überprüft wurden. Geben Sie andernfalls <strong>false</strong> an.</td>
</tr>
</tbody>
</table>

<span/> 

### Anforderungsbeispiel

Die folgenden Beispiele zeigen verschiedene Anforderungen für das Abrufen von Bewertungsdaten. Ersetzen Sie den Wert *ApplicationId* durch die Store-ID Ihrer App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'phone' HTTP/1.1
Authorization: Bearer <your access token>
```

## Antwort


### Antworttext

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wert      | array  | Ein Array von Objekten, die gesammelte Bewertungsdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Bewertungswerte](#rating-values).                                                                                                                           |
| @nextLink  | string | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10.000 Zeilen mit Kaufdaten für die Abfrage gibt. |
| TotalCount | int    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.                                                                                                                                                                                                                             |

<span/>

### Bewertungswerte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date            | string  | Das erste Datum im Datumsbereich für die Bewertungsdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| applicationId   | string  | Die Store-ID der App, für die Sie Bewertungsdaten abrufen.                                                                                                                                                                 |
| applicationName | string  | Der Anzeigename der App.                                                                                                                                                                                                         |
| market          | string  | Die ISO 3166-Ländervorwahl für den Markt, in dem die Bewertung übermittelt wurde.                                                                                                                                                              |
| osVersion       | string  | Die Version des Betriebssystems, auf dem die Bewertung übermittelt wurde. Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).                                                                                               |
| deviceType      | string  | Der Typ des Geräts, auf dem die Bewertung übermittelt wurde. Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).                                                                                           |
| isRevised       | Boolescher Wert | Der Wert **true** gibt an, dass die Bewertung überprüft wurde; andernfalls **false**.                                                                                                                                                       |
| oneStar         | number  | Die Anzahl von Bewertungen mit einem Stern.                                                                                                                                                                                                      |
| twoStars        | number  | Die Anzahl von Bewertungen mit zwei Sternen.                                                                                                                                                                                                      |
| threeStars      | number  | Die Anzahl von Bewertungen mit drei Sternen.                                                                                                                                                                                                    |
| fourStars       | number  | Die Anzahl von Bewertungen mit vier Sternen.                                                                                                                                                                                                     |
| fiveStars       | number  | Die Anzahl von Bewertungen mit fünf Sternen.                                                                                                                                                                                                     |
 
<span/>

### Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "date": "2015-02-13",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso demo",
      "market": "CN",
      "osVersion": "8.0.10517.0",
      "deviceType": "Phone",
      "isRevised": false,
      "oneStar": 0,
      "twoStars": 0,
      "threeStars": 0,
      "fourStars": 0,
      "fiveStars": 2
    }
  ],
  "@nextLink": "ratings?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 15242
}

```

## Verwandte Themen

* [Zugreifen auf Analysedaten mit WindowsStore-Diensten](access-analytics-data-using-windows-store-services.md)
* [Abrufen von App-Käufen](get-app-acquisitions.md)
* [Abrufen von Add-On-Käufen](get-in-app-acquisitions.md)
* [Abrufen von Fehlerberichtsdaten](get-error-reporting-data.md)
* [Abrufen von App-Rezensionen](get-app-reviews.md)



<!--HONumber=Aug16_HO5-->


