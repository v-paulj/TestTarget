---
ms.assetid: 2967C757-9D8A-4B37-8AA4-A325F7A060C5
description: Mittels dieser Methode in der Windows Store-Analyse-API können Sie Rezensionsdaten für einen bestimmten Zeitraum und andere optionale Filter abrufen.
title: Abrufen von App-Rezensionen
---

# Abrufen von App-Rezensionen


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Mittels dieser Methode in der Windows Store-Analyse-API können Sie Rezensionsdaten für einen bestimmten Zeitraum und andere optionale Filter abrufen. Diese Methode gibt die Daten im JSON-Format zurück.

## Voraussetzungen


Sie benötigen Folgendes, um diese Methode verwenden zu können:

-   Ordnen Sie die Azure AD-Anwendung, die Sie zum Aufrufen dieser Methode verwenden, Ihrem Dev Center-Konto zu.

-   Rufen Sie ein Azure AD-Zugriffstoken für Ihre Anwendung ab.

Weitere Informationen finden Sie unter [Zugreifen auf Analysedaten mit Windows Store-Diensten](access-analytics-data-using-windows-store-services.md).

## Anforderung


### Anforderungssyntax

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews |

 

### Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |

 

### Anforderungstext

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
<td align="left">Die Produkt-ID der App, für die Sie Rezensionsdaten abrufen möchten. Die Produkt ID ist im Eintragungslink der App eingebettet, die auf der [App identity page](https://msdn.microsoft.com/library/windows/apps/mt148561) des Dev Center-Dashboards verfügbar ist. Ein Beispiel für eine Produkt-ID ist 9WZDNCRFJ3Q8.</td>
<td align="left">Ja</td>
</tr>
<tr class="even">
<td align="left">startDate</td>
<td align="left">date</td>
<td align="left">Das Startdatum im Datumsbereich der Rezensionsdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum.</td>
<td align="left">Nein</td>
</tr>
<tr class="odd">
<td align="left">endDate</td>
<td align="left">date</td>
<td align="left">Das Enddatum im Datumsbereich der Rezensionsdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum.</td>
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
<td align="left">Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw.</td>
<td align="left">Nein</td>
</tr>
<tr class="even">
<td align="left">filter</td>
<td align="left">string</td>
<td align="left">Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Weitere Informationen finden Sie unten im Abschnitt [filter fields](#filter-fields).</td>
<td align="left">Nein</td>
</tr>
<tr class="odd">
<td align="left">orderby</td>
<td align="left">string</td>
<td align="left">Eine Anweisung, die die Ergebnisdatenwerte für die einzelnen Bewertungen anfordert. Die Syntax ist <em>orderby=field [order],field [order],...</em>. Der Parameter <em>field</em> kann eine der folgenden Zeichenfolgen sein:
<ul>
<li><strong>date</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>market</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>isRevised</strong></li>
<li><strong>packageVersion</strong></li>
<li><strong>deviceModel</strong></li>
<li><strong>productFamily</strong></li>
<li><strong>deviceScreenResolution</strong></li>
<li><strong>isTouchEnabled</strong></li>
<li><strong>reviewerName</strong></li>
<li><strong>reviewTitle</strong></li>
<li><strong>reviewText</strong></li>
<li><strong>helpfulCount</strong></li>
<li><strong>notHelpfulCount</strong></li>
<li><strong>responseDate</strong></li>
<li><strong>responseText</strong></li>
<li><strong>deviceRAM</strong></li>
<li><strong>deviceStorageCapacity</strong></li>
<li><strong>rating</strong></li>
</ul>
<p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standard ist <strong>asc</strong>.</p>
<p>Dies ist eine Beispielzeichenfolge für <em>orderby</em> : <em>orderby=date,market</em></p></td>
<td align="left">Nein</td>
</tr>
</tbody>
</table>

 
### Filterfelder

Der Parameter *Filter* des Anforderungstexts enthält mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält ein Feld und einen Wert, das/der mit den Operatoren **eq** oder **ne** verknüpft ist. Einige Felder unterstützen darüber hinaus die Operatoren **contains**, **gt**, **lt**, **ge** und **le**. Anweisungen können mittels **and** oder **or** kombiniert werden.

Dies ist eine Beispielzeichenfolge für *filter*: *filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US'*

Eine Liste der unterstützten Felder und Operatoren für die einzelnen Felder finden Sie in der folgenden Tabelle. Zeichenfolgenwerte im Parameter *Filter* müssen von einfachen Anführungszeichen eingeschlossen werden.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Felder</th>
<th align="left">Unterstützte Operatoren</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">market</td>
<td align="left">eq, ne</td>
<td align="left">Eine Zeichenfolge, die den ISO 3166-Ländercode des Gerätemarkts enthält.</td>
</tr>
<tr class="even">
<td align="left">osVersion</td>
<td align="left">eq, ne</td>
<td align="left">Eine der folgenden Zeichenfolgen:
<ul>
<li><strong>Windows Phone 7.5</strong></li>
<li><strong>Windows Phone 8</strong></li>
<li><strong>Windows Phone 8.1</strong></li>
<li><strong>Windows Phone 10</strong></li>
<li><strong>Windows 8</strong></li>
<li><strong>Windows 8.1</strong></li>
<li><strong>Windows 10</strong></li>
<li><strong>Unknown</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">deviceType</td>
<td align="left">eq, ne</td>
<td align="left">Eine der folgenden Zeichenfolgen:
<ul>
<li><strong>PC</strong></li>
<li><strong>Tablet</strong></li>
<li><strong>Phone</strong></li>
<li><strong>IoT</strong></li>
<li><strong>Wearable</strong></li>
<li><strong>Server</strong></li>
<li><strong>Collaborative</strong></li>
<li><strong>Other</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">isRevised</td>
<td align="left">eq, ne</td>
<td align="left">Geben Sie <strong>true</strong> an, um nach Rezensionen zu filtern, die überprüft wurden. Geben Sie andernfalls <strong>false</strong> an.</td>
</tr>
<tr class="odd">
<td align="left">packageVersion</td>
<td align="left">eq, ne</td>
<td align="left">Die Version des App-Pakets, das überprüft wurde.</td>
</tr>
<tr class="even">
<td align="left">deviceModel</td>
<td align="left">eq, ne</td>
<td align="left">Der Typ des Geräts, auf dem die App überprüft wurde.</td>
</tr>
<tr class="odd">
<td align="left">productFamily</td>
<td align="left">eq, ne</td>
<td align="left">Eine der folgenden Zeichenfolgen:
<ul>
<li><strong>PC</strong></li>
<li><strong>Tablet</strong></li>
<li><strong>Phone</strong></li>
<li><strong>Wearable</strong></li>
<li><strong>Server</strong></li>
<li><strong>Collaborative</strong></li>
<li><strong>Other</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">deviceScreenResolution</td>
<td align="left">eq, ne</td>
<td align="left">Die Auflösung des Gerätebildschirms im Format &quot;<em>Breite</em> x <em>Höhe</em>&quot;.</td>
</tr>
<tr class="odd">
<td align="left">isTouchEnabled</td>
<td align="left">eq, ne</td>
<td align="left">Geben Sie <strong>true</strong> an, um nach für die Fingereingabe aktivierten Geräten zu filtern; andernfalls <strong>false</strong>.</td>
</tr>
<tr class="even">
<td align="left">reviewerName</td>
<td align="left">eq, ne</td>
<td align="left">Der Name der Person, die die App geprüft hat.</td>
</tr>
<tr class="odd">
<td align="left">helpfulCount</td>
<td align="left">eq, ne</td>
<td align="left">Die Anzahl der Rezensionen, die als nützlich markiert wurden.</td>
</tr>
<tr class="even">
<td align="left">notHelpfulCount</td>
<td align="left">eq, ne</td>
<td align="left">Die Anzahl der Rezensionen, die als nicht nützlich markiert wurden.</td>
</tr>
<tr class="odd">
<td align="left">reviewTitle</td>
<td align="left">eq, ne, contains</td>
<td align="left">Der Titel der Rezension.</td>
</tr>
<tr class="even">
<td align="left">reviewText</td>
<td align="left">eq, ne, contains</td>
<td align="left">Der Textinhalt der Rezension.</td>
</tr>
<tr class="odd">
<td align="left">responseText</td>
<td align="left">eq, ne, contains</td>
<td align="left">Der Textinhalt der Antwort.</td>
</tr>
<tr class="even">
<td align="left">responseDate</td>
<td align="left">eq, ne</td>
<td align="left">Das Datum, an dem die Antwort übermittelt wurde.</td>
</tr>
<tr class="odd">
<td align="left">deviceRAM</td>
<td align="left">eq, ne, gt, lt, ge, le</td>
<td align="left">Der physische Arbeitsspeicher in MB.</td>
</tr>
<tr class="even">
<td align="left">deviceStorageCapacity</td>
<td align="left">eq, ne, gt, lt, ge, le</td>
<td align="left">Die Kapazität des primären Datenspeichers in GB.</td>
</tr>
<tr class="odd">
<td align="left">rating</td>
<td align="left">eq, ne, gt, lt, ge, le</td>
<td align="left">Die App-Bewertung in Sternen.</td>
</tr>
</tbody>
</table>

 

### Anforderungsbeispiel

Die folgenden Beispiele zeigen verschiedene Anforderungen für das Abrufen von Rezensionsdaten. Ersetzen Sie den Wert *ApplicationId* durch die Produkt-ID Ihrer App.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US' HTTP/1.1
Authorization: Bearer <your access token>
```

## Antwort


### Antworttext

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wert      | array  | Ein Array von Objekten, die Rezensionsdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Rezensionswerte](#review-values).                                                                                                                                      |
| @nextLink  | string | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10.000 Zeilen mit Kaufdaten für die Abfrage gibt. |
| TotalCount | int    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.                                                                                                                                                                                                                             |

 
### Rezensionswerte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert                  | Typ    | Beschreibung                                                                                                                                                                                                                          |
|------------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date                   | string  | Das erste Datum im Datumsbereich für die Bewertungsdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist das erste Datum in diesem Datumsbereich dieser Wert. |
| applicationId          | string  | Die Produkt-ID der App, für die Sie Bewertungsdaten abrufen.                                                                                                                                                                 |
| applicationName        | string  | Der Anzeigename der App.                                                                                                                                                                                                         |
| market                 | string  | Die ISO 3166-Ländervorwahl für den Markt, in dem die Bewertung übermittelt wurde.                                                                                                                                                              |
| osVersion              | string  | Die Version des Betriebssystems, auf dem die Bewertung übermittelt wurde. Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).                                                                                               |
| deviceType             | string  | Der Typ des Geräts, auf dem die Bewertung übermittelt wurde. Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).                                                                                           |
| isRevised              | Boolescher Wert | Der Wert **true** gibt an, dass die Rezension überprüft wurde; andernfalls **false**.                                                                                                                                                       |
| packageVersion         | string  | Die Version des App-Pakets, das überprüft wurde.                                                                                                                                                                                    |
| deviceModel            | string  | Der Typ des Geräts, auf dem die App überprüft wurde.                                                                                                                                                                                    |
| productFamily          | string  | Der Name der Gerätefamilie. Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).                                                                                                                         |
| deviceScreenResolution | string  | Die Auflösung des Gerätebildschirms im Format „*Breite* x *Höhe*“.                                                                                                                                                                     |
| isTouchEnabled         | Boolescher Wert | Der Wert **true** gibt an, dass die Fingereingabe aktiviert ist; andernfalls **false**.                                                                                                                                                             |
| reviewerName           | string  | Der Name der Person, die die App geprüft hat.                                                                                                                                                                                                                   |
| helpfulCount           | number  | Die Anzahl der Rezensionen, die als nützlich markiert wurden.                                                                                                                                                                                   |
| notHelpfulCount        | number  | Die Anzahl der Rezensionen, die als nicht nützlich markiert wurden.                                                                                                                                                                               |
| reviewTitle            | string  | Der Titel der Rezension.                                                                                                                                                                                                             |
| reviewText             | string  | Der Textinhalt der Rezension.                                                                                                                                                                                                     |
| responseText           | string  | Der Textinhalt der Antwort.                                                                                                                                                                                                   |
| responseDate           | string  | Das Datum, an dem eine Antwort übermittelt wurde.                                                                                                                                                                                                   |
| deviceRAM              | number  | Der physische Arbeitsspeicher in MB.                                                                                                                                                                                                             |
| deviceStorageCapacity  | number  | Die Kapazität des primären Datenspeichers in GB.                                                                                                                                                                                     |
| rating                 | number  | Die App-Bewertung in Sternen.                                                                                                                                                                                                            |

 

### Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "date": "2015-07-29",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso demo",
      "market": "US",
      "osVersion": "10.0.10240.16410",
      "deviceType": "PC",
      "isRevised": true,
      "packageVersion": "",
      "deviceModel": "Microsoft Corporation-Virtual Machine",
      "productFamily": "PC",
      "deviceRAM": -1,
      "deviceScreenResolution": "1024 x 768",
      "deviceStorageCapacity": 51200,
      "isTouchEnabled": false,
      "reviewerName": "ALeksandra",
      "rating": 5,
      "reviewTitle": "Love it",
      "reviewText": "Great app for demos and great dev response.",
      "helpfulCount": 0,
      "notHelpfulCount": 0,
      "responseDate": "2015-08-07T01:50:22.9874488Z",
      "responseText": "1"
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## Verwandte Themen

* [Zugreifen auf Analysedaten mit Windows Store-Diensten](access-analytics-data-using-windows-store-services.md)
* [Abrufen von App-Käufen](get-app-acquisitions.md)
* [Abrufen von IAP-Käufen](get-in-app-acquisitions.md)
* [Abrufen von Fehlerberichtsdaten](get-error-reporting-data.md)
* [Abrufen von App-Bewertungen](get-app-ratings.md)



<!--HONumber=Mar16_HO2-->


