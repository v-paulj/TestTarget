---
author: mcleanbyron
ms.assetid: C1E42E8B-B97D-4B09-9326-25E968680A0F
description: Verwenden Sie diese Methode in der Windows Store-Analyse-API, um die aggregierten Kaufdaten für eine Anwendung während eines bestimmten Zeitraums und andere optionale Filter abzurufen.
title: Abrufen von App-Käufen
---

# Abrufen von App-Käufen


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Verwenden Sie diese Methode in der Windows Store-Analyse-API, um die aggregierten Kaufdaten für eine Anwendung während eines bestimmten Zeitraums und andere optionale Filter abzurufen. Diese Methode gibt die Daten im JSON-Format zurück.

## Voraussetzungen


Sie benötigen Folgendes, um diese Methode verwenden zu können:

-   Ordnen Sie die Azure AD-Anwendung, die Sie zum Aufrufen dieser Methode verwenden, Ihrem Dev Center-Konto zu.

-   Rufen Sie ein Azure AD-Zugriffstoken für Ihre Anwendung ab.

Weitere Informationen finden Sie unter [Zugreifen auf Analysedaten mit Windows Store-Diensten](access-analytics-data-using-windows-store-services.md).

## Anforderung


### Anforderungssyntax

| Methode | Anforderungs-URI                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions |

 

### Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer**&lt;*token*&gt;. |

 

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
<td align="left">Die Produkt-ID der App, für die Sie Kaufdaten abrufen möchten. Die Produkt ID ist im Eintragungslink der App eingebettet, die auf der [Seite App-Identität](https://msdn.microsoft.com/library/windows/apps/mt148561) des Dev Center-Dashboards verfügbar ist. Ein Beispiel für eine Produkt-ID ist 9WZDNCRFJ3Q8.</td>
<td align="left">Ja</td>
</tr>
<tr class="even">
<td align="left">startDate</td>
<td align="left">date</td>
<td align="left">Das Startdatum im Datumsbereich der Kaufdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum.</td>
<td align="left">Nein</td>
</tr>
<tr class="odd">
<td align="left">endDate</td>
<td align="left">date</td>
<td align="left">Das Enddatum im Datumsbereich der Kaufdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum.</td>
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
<td align="left">Eine Anweisung, die die Ergebnisdatenwerte für die einzelnen Käufe anfordert. Die Syntax ist <em>orderby=field [order],field [order],...</em>. Der Parameter <em>field</em> kann eine der folgenden Zeichenfolgen sein:
<ul>
<li><strong>date</strong></li>
<li><strong>acquisitionType</strong></li>
<li><strong>ageGroup</strong></li>
<li><strong>storeClient</strong></li>
<li><strong>gender</strong></li>
<li><strong>market</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>orderName</strong></li>
</ul>
<p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standard ist <strong>asc</strong>.</p>
<p>Dies ist eine Beispielzeichenfolge für <em>orderby</em> : <em>orderby=date,market</em></p></td>
<td align="left">Nein</td>
</tr>
</tbody>
</table>

 
### Filterfelder

Der Parameter *Filter* des Anforderungstexts enthält mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält ein Feld und einen Wert, das/der mit den Operatoren **eq** oder **ne** verknüpft ist. Anweisungen können mit **and** oder **or** kombiniert werden. Dies sind einige Beispielparameter für *Filter*:

-   *filter=market eq 'US' and gender eq 'm'*
-   *filter=(market ne 'US') and (gender ne 'Unknown') and (gender ne 'm') and (market ne 'NO') and (ageGroup ne 'greater than 55' or ageGroup ne ‘less than 13’)*

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
<td align="left">acquisitionType</td>
<td align="left">Eine der folgenden Zeichenfolgen:
<ul>
<li><strong>free</strong></li>
<li><strong>trial</strong></li>
<li><strong>paid</strong></li>
<li><strong>promotional code</strong></li>
<li><strong>iap</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">ageGroup</td>
<td align="left">Eine der folgenden Zeichenfolgen:
<ul>
<li><strong>less than 13</strong></li>
<li><strong>13-17</strong></li>
<li><strong>18-24</strong></li>
<li><strong>25-34</strong></li>
<li><strong>35-44</strong></li>
<li><strong>44-55</strong></li>
<li><strong>greater than 55</strong></li>
<li><strong>Unknown</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">storeClient</td>
<td align="left">Eine der folgenden Zeichenfolgen:
<ul>
<li><strong>Windows Phone Store (client)</strong></li>
<li><strong>Windows Store (client)</strong></li>
<li><strong>Windows Store (web)</strong></li>
<li><strong>Volume purchase by organizations</strong></li>
<li><strong>Other</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">gender</td>
<td align="left">Eine der folgenden Zeichenfolgen:
<ul>
<li><strong>m</strong></li>
<li><strong>f</strong></li>
<li><strong>Unknown</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">market</td>
<td align="left">Eine Zeichenfolge, die den ISO 3166-Ländercode des Markts enthält, in dem der Kauf erfolgte.</td>
</tr>
<tr class="even">
<td align="left">osVersion</td>
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
<td align="left">orderName</td>
<td align="left">Eine Zeichenfolge, die den Namen der Bestellung für den Werbecode angibt, der für den kauf der App verwendet wurde. (Dies gilt nur, wenn der Benutzer die App durch Einlösen eines Werbecodes erworben hat.)</td>
</tr>
</tbody>
</table>

 

### Anforderungsbeispiel

Das folgende Beispiel zeigt verschiedene Anforderungen für den Abruf von Kaufdaten für Apps. Ersetzen Sie den Wert *ApplicationId* durch die Produkt-ID Ihrer App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US'; and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## Antwort


### Antworttext

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wert      | array  | Ein Array von Objekten, die gesammelte Bewertungsdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Kaufwerte](#acquisition-values).                                                                                                                      |
| @nextLink  | string | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10.000 Zeilen mit Kaufdaten für die Abfrage gibt. |
| TotalCount | int    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.                                                                                                                                                                                                                             |

 
### Kaufwerte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | Typ   | Beschreibung                                                                                                                                                                                                                              |
|---------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date                | string | Das erste Datum im Datumsbereich für die Kaufdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist das erste Datum in diesem Datumsbereich dieser Wert. |
| applicationId       | string | Die Produkt-ID der App, für die Sie Kaufdaten abrufen.                                                                                                                                                                 |
| applicationName     | string | Der Anzeigename der App.                                                                                                                                                                                                             |
| deviceType          | string | Der Typ des Geräts, auf dem der Kauf ausgeführt wurde. Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).                                                                                                  |
| orderName           | string | Der Name der Bestellung.                                                                                                                                                                                                                   |
| storeClient         | string | Die Version des Store, in dem der Kauf erfolgte. Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).                                                                                            |
| osVersion           | string | Die Version des Betriebssystems, auf dem der Kauf ausgeführt wurde. Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).                                                                                                   |
| market              | string | Die ISO 3166-Ländercode des Markts, in dem der Kauf erfolgte.                                                                                                                                                                  |
| gender              | string | Das Geschlecht des Benutzers, der den Kauf ausgeführt hat. Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).                                                                                                    |
| ageGroup            | string | Die Altersgruppe des Benutzers, der den Kauf ausgeführt hat. Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).                                                                                                 |
| acquisitionType     | string | Der Typ des Kaufs (kostenlos, kostenpflichtig usw.). Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).                                                                                                    |
| acquisitionQuantity | number | Die Anzahl der Käufe, die während der angegebenen Aggregationsebene erfolgten.                                                                                                                                                         |

 

### Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "date": "2016-02-01",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "deviceType": "Phone",
      "orderName": "",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows Phone 8.1",
      "market": "IT",
      "gender": "m",
      "ageGroup": "0-17",
      "acquisitionType": "Free",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "appacquisitions?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1&orderby=date desc",
  "TotalCount": 466766
}
```

## Verwandte Themen

* [Zugreifen auf Analysedaten mit Windows Store-Diensten](access-analytics-data-using-windows-store-services.md)
* [Abrufen von IAP-Käufen](get-in-app-acquisitions.md)
* [Abrufen von Fehlerberichtsdaten](get-error-reporting-data.md)
* [Abrufen von App-Bewertungen](get-app-ratings.md)
* [Abrufen von App-Rezensionen](get-app-reviews.md)




<!--HONumber=May16_HO2-->


