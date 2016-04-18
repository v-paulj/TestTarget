---
Description: Verwenden Sie die Windows.Globalization.DateTimeFormatting-API mit benutzerdefinierten Mustern, um Datums- und Uhrzeitwerte im gewünschten Format anzuzeigen.
title: Verwenden von Mustern zum Formatieren von Datums- und Uhrzeitwerten
ms.assetid: 012028B3-9DA2-4E72-8C0E-3E06BEC3B3FE
label: Verwenden von Mustern zum Formatieren von Datums- und Uhrzeitwerten
template: detail.hbs
---

# Verwenden von Mustern zum Formatieren von Datums- und Uhrzeitwerten


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**Wichtige APIs**

-   [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
-   [**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828)
-   [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576)

Verwenden Sie die [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)-API mit benutzerdefinierten Mustern, um Datums- und Uhrzeitwerte im gewünschten Format anzuzeigen.

## <span id="Introduction"></span><span id="introduction"></span><span id="INTRODUCTION"></span>Einführung


[**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859) bietet mehrere Möglichkeiten zur ordnungsgemäßen Formatierung von Datums- und Uhrzeitwerten für Sprachen und Regionen auf der ganzen Welt. Sie können Standardformate für Jahr, Monat, Tag usw. oder auch Standard-Zeichenfolgenvorlagen wie „longdate“ oder „month day“ verwenden.

Wenn Sie allerdings mehr Kontrolle über Reihenfolge und Format der Bestandteile der anzuzeigenden [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576)-Zeichenfolge haben möchten, können Sie für den Zeichenfolgenvorlagen-Parameter eine spezielle Syntax (ein so genanntes Muster) verwenden. Mit der Mustersyntax können Sie einzelne Bestandteile eines **DateTime**-Objekts (beispielsweise nur den Monatsnamen oder nur den Jahreswert) abrufen und in einem beliebigen benutzerdefinierten Format anzeigen. Darüber hinaus kann das Muster mittels Lokalisierung für andere Sprachen und Regionen angepasst werden.

**Hinweis:** Hier finden Sie eine Übersicht über Formatmuster. Eine ausführlichere Besprechung der Formatvorlagen und Formatmuster finden Sie in der Dokumentation zur [**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828)-Klasse im Abschnitt „Anmerkungen“.

 

## <span id="What_you_need_to_know"></span><span id="what_you_need_to_know"></span><span id="WHAT_YOU_NEED_TO_KNOW"></span>Wissenswertes


Beachten Sie, dass Sie beim Verwenden von Mustern ein benutzerdefiniertes Format erstellen, das nicht unbedingt kulturübergreifend gültig ist. Betrachten wir als Beispiel die Vorlage „month day“:

**C#**
```CSharp
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```
**JavaScript**
```JavaScript
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```

Hierdurch wird ein Formatierer erstellt, der auf dem Sprach- und Regionswert des aktuellen Kontexts basiert. Monat und Tag werden somit immer gemeinsam in einem geeigneten globalen Format dargestellt. Beispiel: „January 1“ für Englisch (USA), „1 janvier“ für Französisch (Frankreich) und „1月1日“ für Japanisch. Der Grund hierfür ist, dass die Vorlage auf einer kulturspezifischen Musterzeichenfolge basiert, auf die über die Mustereigenschaft zugegriffen werden kann:

**C#**
```CSharp
var monthdaypattern = datefmt.Patterns;
```
**JavaScript**
```JavaScript
var monthdaypattern = datefmt.patterns;
```

Somit ist das Ergebnis je nach Sprache und Region des Formatierers unterschiedlich. Beachten Sie, dass für unterschiedliche Regionen unterschiedliche Bestandteile in unterschiedlicher Reihenfolge und mit oder ohne zusätzliche Zeichen und Leerzeichen verwendet werden können:

``` syntax
En-US: "{month.full} {day.integer}"
Fr-FR: "{day.integer} {month.full}"
Ja-JP: "{month.integer}月{day.integer}日"
```

Mit Mustern können Sie ein benutzerdefiniertes [**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828)-Element erstellen. Hier sehen Sie ein auf dem Muster für Englisch (USA) basierendes Beispiel:

**C#**
```CSharp
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```
**JavaScript**
```JavaScript
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```

Windows gibt in den geschweiften Klammern ({}) kulturspezifische Werte für die einzelnen Bestandteile zurück. Bei der Mustersyntax ist die Reihenfolge der Bestandteile allerdings unveränderlich. Sie erhalten also stets genau das, was Sie anfordern, und das ist unter Umständen nicht immer für die Kultur geeignet:

``` syntax
En-US: January 1
Fr-FR: janvier 1 (inappropriate for France; non-standard order)
Ja-JP: 1月1 (inappropriate for Japan; the day symbol is missing)
```

Darüber hinaus können sich Muster im Lauf der Zeit ändern. Länder und Regionen können Änderungen an ihren Kalendersystemen vornehmen, die eine Anpassung einer Formatvorlage zur Folge haben. Windows aktualisiert die Ausgabe der Formatierer, um solchen Änderungen Rechnung zu tragen. Daher sollten Sie die Mustersyntax nur unter folgenden Bedingungen zum Formatieren von [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) verwenden:

-   Sie benötigen keine bestimmte Ausgabe für ein Format.
-   Das Format muss keinen bestimmten kulturspezifischen Standard erfüllen.
-   Das Muster soll bewusst für alle Kulturen unveränderlich sein.
-   Sie beabsichtigen, das Muster zu lokalisieren.

Zusammenfassung der Unterschiede zwischen Standard-Zeichenfolgenvorlagen und nicht standardmäßigen Zeichenfolgenvorlagen:

**Zeichenfolgenvorlagen, z. B. „month day“:**

-   Eine abstrahierte Darstellung des [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576)-Formats, die Werte für den Tag und den Monat in einer bestimmten Reihenfolge enthält.
-   Gibt für alle von Windows unterstützten Sprach- und Regionswerte stets ein gültiges Standardformat zurück.
-   Liefert stets eine entsprechend der Kultur formatierte Zeichenfolge für die angegebene Sprache und Region.
-   Nicht alle Kombinationen der Bestandteile sind gültig. Beispielsweise gibt es keine Zeichenfolgenvorlage für „dayofweek day“.

**Zeichenfolgenmuster, z. B. „{month.full} {day.integer}“:**

-   Eine Zeichenfolge mit explizit festgelegter Reihenfolge, die den vollständigen Monatsnamen, gefolgt von einem Leerzeichen, auf das der als ganze Zahl dargestellte Tag folgt, angibt (und zwar in dieser Reihenfolge).
-   Entspricht möglicherweise nicht dem gültigen Standardformat für jedes Sprach-Region-Paar.
-   Ist nicht in jedem Fall der Kultur angemessen.
-   Es kann eine beliebige Kombination von Bestandteilen in einer beliebigen Reihenfolge angegeben werden.

## <span id="Tasks"></span><span id="tasks"></span><span id="TASKS"></span>Aufgaben


Angenommen, Sie möchten den aktuellen Monat und den aktuellen Tag zusammen mit der aktuellen Uhrzeit in einen bestimmten Format anzeigen. Sie möchten also z. B., dass für US-Benutzer Folgendes angezeigt wird:

``` syntax
June 25 | 1:38 PM
```

Die Datumskomponente entspricht der Vorlage „month day“, die Uhrzeitkomponente der Vorlage „hour minute“. Sie können also ein benutzerdefiniertes Format erstellen, das die Muster verknüpft, aus denen sich diese Vorlagen zusammensetzen.

Erstellen Sie zunächst die Formatierer der relevanten Vorlagen für Datum und Uhrzeit und dann die Muster dieser Vorlagen:

**C#**
```CSharp
// Get formatters for the date part and the time part.
var mydate = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var mytime = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

// Get the patterns from these formatters.
var mydatepattern = mydate.Patterns[0];
var mytimepattern = mytime.Patterns[0];
```
**JavaScript**
```JavaScript
// Get formatters for the date part and the time part.
var dtf = Windows.Globalization.DateTimeFormatting;
var mydate = dtf.DateTimeFormatter("month day");
var mytime = dtf.DateTimeFormatter("hour minute");

// Get the patterns from these formatters.
var mydatepattern = mydate.patterns[0];
var mytimepattern = mytime.patterns[0];
```

Ihr benutzerdefiniertes Format muss als lokalisierbare Ressourcenzeichenfolge gespeichert werden. Die Zeichenfolge für Englisch (USA) lautet beispielsweise „{date} | {time}“. Lokalisierer können diese Zeichenfolge wie gewünscht anpassen. Sie können beispielsweise die Reihenfolge der Bestandteile ändern, falls in bestimmten Sprachen oder Regionen die Uhrzeit vor dem Datum stehen soll. Oder Sie können „|“ durch ein anderes Trennzeichen ersetzen. Zur Laufzeit werden die Bestandteile „{date}“ und „{time}“ der Zeichenfolge durch das entsprechende Muster ersetzt:

**C#**
```CSharp
// Assemble the custom pattern. This string comes from a resource, and should be localizable. 
var resourceLoader = new Windows.ApplicationModel.Resources.ResourceLoader();
var mydateplustime = resourceLoader.GetString("date_plus_time");
mydateplustime = mydateplustime.replace("{date}", mydatepattern);
mydateplustime = mydateplustime.replace("{time}", mytimepattern);
```
**JavaScript**
```JavaScript
// Assemble the custom pattern. This string comes from a resource, and should be localizable. 
var mydateplustime = WinJS.Resources.getString("date_plus_time");
mydateplustime = mydateplustime.replace("{date}", mydatepattern);
mydateplustime = mydateplustime.replace("{time}", mytimepattern);
```

Anschließend können Sie einen neuen Formatierer auf der Grundlage des benutzerdefinierten Musters erstellen:

**C#**
```CSharp
// Get the custom formatter.
var mydateplustimefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(mydateplustime);
```
**JavaScript**
```JavaScript
// Get the custom formatter.
var mydateplustimefmt = new dtf.DateTimeFormatter(mydateplustime);
```

## <span id="related_topics"></span>Verwandte Themen


* [Beispiel für Datums- und Uhrzeitformatierung](http://go.microsoft.com/fwlink/p/?LinkId=231618)
* [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
* [**Windows.Foundation.DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576)
 

 





<!--HONumber=Mar16_HO1-->


