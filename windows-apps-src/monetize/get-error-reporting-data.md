---
author: mcleanbyron
ms.assetid: 252C44DF-A2B8-4F4F-9D47-33E423F48584
description: Mittels dieser Methode in der Windows Store-Analyse-API können Sie gesammelte Fehlerberichtsdaten für einen bestimmten Zeitraum und andere optionale Filter abrufen.
title: Abrufen von Fehlerberichtsdaten
---

# Abrufen von Fehlerberichtsdaten


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Mittels dieser Methode in der Windows Store-Analyse-API können Sie gesammelte Fehlerberichtsdaten für einen bestimmten Zeitraum und andere optionale Filter abrufen. Diese Methode gibt die Daten im JSON-Format zurück.

## Voraussetzungen


Sie benötigen Folgendes, um diese Methode verwenden zu können:

-   Ordnen Sie die Azure AD-Anwendung, die Sie zum Aufrufen dieser Methode verwenden, Ihrem Dev Center-Konto zu.

-   Rufen Sie ein Azure AD-Zugriffstoken für Ihre Anwendung ab.

Weitere Informationen finden Sie unter [Zugreifen auf Analysedaten mit Windows Store-Diensten](access-analytics-data-using-windows-store-services.md).

## Anforderung


### Anforderungssyntax

| Methode | Anforderungs-URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits |

 

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
<td align="left">Die Produkt-ID der App, für die Fehlerberichtsdaten abgerufen werden sollen. Die Produkt ID ist im Eintragungslink der App eingebettet, die auf der [Seite App-Identität](https://msdn.microsoft.com/library/windows/apps/mt148561) des Dev Center-Dashboards verfügbar ist. Ein Beispiel für eine Produkt-ID ist 9WZDNCRFJ3Q8.</td>
<td align="left">Ja</td>
</tr>
<tr class="even">
<td align="left">startDate</td>
<td align="left">date</td>
<td align="left">Das Startdatum im Datumsbereich der Fehlerberichtsdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum.</td>
<td align="left">Nein</td>
</tr>
<tr class="odd">
<td align="left">endDate</td>
<td align="left">date</td>
<td align="left">Das Enddatum im Datumsbereich der Fehlerberichtsdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum.</td>
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
<td align="left">Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Dies kann eine der folgenden Zeichenfolgen sein: <strong>day</strong>, <strong>week</strong> oder <strong>month</strong>. Wenn keine Angabe erfolgt, lautet der Standardwert <strong>day</strong>. Wenn Sie <strong>week</strong> oder <strong>month</strong> angeben, sind die Werte <em>failureName</em> und <em>failureHash</em> auf 1000 Buckets begrenzt.</td>
<td align="left">Nein</td>
</tr>
<tr class="even">
<td align="left">groupby</td>
<td align="left">string</td>
<td align="left">Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder angeben:
<ul>
<li><strong>failureName</strong></li>
<li><strong>failureHash</strong></li>
<li><strong>symbol</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>eventType</strong></li>
<li><strong>market</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>packageName</strong></li>
<li><strong>packageVersion</strong></li>
</ul>
<p>Die zurückgegebenen Datenzeilen enthalten die Felder, die im Parameter <em>groupby</em> angegeben sind, sowie die folgenden:</p>
<ul>
<li><strong>date</strong></li>
<li><strong>applicationId</strong></li>
<li><strong>applicationName</strong></li>
<li><strong>deviceCount</strong></li>
<li><strong>eventCount</strong></li>
</ul>
<p>Der Parameter <em>groupby</em> kann mit dem Parameter <em>aggregationLevel</em> verwendet werden. Beispiel: <em>&amp;groupby=failureName,market&amp;aggregationLevel=week</em></p></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">orderby</td>
<td align="left">string</td>
<td align="left">Eine Anweisung, die die Ergebnisdatenwerte für die einzelnen Käufe anfordert. Die Syntax ist <em>orderby=field [order],field [order],...</em>. Der Parameter <em>field</em> kann eine der folgenden Zeichenfolgen sein:
<ul>
<li><strong>date</strong></li>
<li><strong>failureName</strong></li>
<li><strong>failureHash</strong></li>
<li><strong>symbol</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>eventType</strong></li>
<li><strong>market</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>packageName</strong></li>
<li><strong>packageVersion</strong></li>
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
<td align="left">failureName</td>
<td align="left">Der Name des Fehlers.</td>
</tr>
<tr class="even">
<td align="left">failureHash</td>
<td align="left">Der eindeutige Bezeichner des Fehlers.</td>
</tr>
<tr class="odd">
<td align="left">symbol</td>
<td align="left">Das diesem Fehler zugewiesene Symbol.</td>
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
<li><strong>Unbekannt</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">eventType</td>
<td align="left">Eine der folgenden Zeichenfolgen:
<ul>
<li><strong>crash</strong></li>
<li><strong>hang</strong></li>
<li><strong>memory</strong></li>
<li><strong>jse</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">market</td>
<td align="left">Eine Zeichenfolge, die den ISO 3166-Ländercode des Gerätemarkts enthält.</td>
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
<td align="left">packageName</td>
<td align="left">Der eindeutige Name des App-Pakets, das mit diesem Fehler verknüpft ist.</td>
</tr>
<tr class="odd">
<td align="left">packageVersion</td>
<td align="left">Die Version des App-Pakets, das mit diesem Fehler verknüpft ist.</td>
</tr>
</tbody>
</table>

 

### Anforderungsbeispiel

Die folgenden Beispiele zeigen verschiedene Anforderungen für das Abrufen von Fehlerberichtsdaten. Ersetzen Sie den Wert *ApplicationId* durch die Produkt-ID Ihrer App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'phone' HTTP/1.1
Authorization: Bearer <your access token>
```

## Antwort


### Antworttext

| Wert      | Typ    | Beschreibung                                                                                                                                                                                                                                                                    |
|------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Value      | array   | Ein Array von Objekten, die gesammelte Fehlerberichtsdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Fehlerwerte](#error-values).                                                                                                          |
| @nextLink  | string  | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10.000 Zeilen mit Fehlern für die Abfrage gibt. |
| TotalCount | inumber | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.                                                                                                                                                                                                                     |

 
### Fehlerwerte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                              |
|-----------------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date            | string  | Das erste Datum im Datumsbereich für die Kaufdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist das erste Datum in diesem Datumsbereich dieser Wert. |
| applicationId   | string  | Die Produkt-ID der App, für die Sie IAP-Kaufdaten abrufen möchten.                                                                                                                                                           |
| applicationName | string  | Der Anzeigename der App.                                                                                                                                                                                                             |
| failureName     | string  | Der Name des Fehlers.                                                                                                                                                                                                                 |
| failureHash     | string  | Der eindeutige Bezeichner des Fehlers.                                                                                                                                                                                                   |
| symbol          | string  | Das diesem Fehler zugewiesene Symbol.                                                                                                                                                                                                       |
| osVersion       | string  | Die Version des Betriebssystems, auf dem der Fehler aufgetreten ist. Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).                                                                                                       |
| eventType       | string  | Der Typ des Fehlerereignisses. Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).                                                                                                                          |
| market          | string  | Die ISO 3166-Ländercode des Gerätemarkts.                                                                                                                                                                                          |
| deviceType      | string  | Der Typ des Geräts, auf dem der Kauf ausgeführt wurde. Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).                                                                                                  |
| packageName     | string  | Der eindeutige Name des App-Pakets, das mit diesem Fehler verknüpft ist.                                                                                                                                                                 |
| packageVersion  | string  | Die Version des App-Pakets, das mit diesem Fehler verknüpft ist.                                                                                                                                                                     |
| eventCount      | inumber | Die Anzahl der Ereignisse, die diesem Fehler für die angegebene Aggregationsebene zugeordnet werden.                                                                                                                                            |
| deviceCount     | inumber | Die Anzahl der eindeutigen Geräte, die diesem Fehler für die angegebene Aggregationsebene entsprechen.                                                                                                                                        |

 

### Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "date": "2015-03-09",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "failureName": "APPLICATION_FAULT_8013150a_StoreWrapper.ni.DLL!70475e55",
      "failureHash": "5a6b2170-1661-ed47-24d7-230fed0077af",
      "symbol": "storewrapper_ni!70475e55",
      "osVersion": "Windows Phone 8",
      "eventType": "crash",
      "market": "US",
      "deviceType": "mobile",
      "packageName": "",
      "packageVersion": "0.0.0.0",
      "deviceCount": 0.0,
      "eventCount": 1.0
    }
  ],
  "@nextLink": "failurehits?applicationId=9NBLGGGZ5QDR&aggregationLevel=week&startDate=2015/03/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 191753
}

```

## Verwandte Themen

* [Zugreifen auf Analysedaten mit Windows Store-Diensten](access-analytics-data-using-windows-store-services.md)
* [Abrufen von App-Käufen](get-app-acquisitions.md)
* [Abrufen von IAP-Käufen](get-in-app-acquisitions.md)
* [Abrufen von App-Bewertungen](get-app-ratings.md)
* [Abrufen von App-Rezensionen](get-app-reviews.md)


<!--HONumber=May16_HO2-->


