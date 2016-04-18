---
Description: Entwickeln Sie eine für den globalen Markt geeignete App mit geeigneter Formatierung für Datumsangaben, Uhrzeiten, Zahlen und Währungen.
title: Verwenden weltweit einsetzbarer Formate
ms.assetid: 6ECE8BA4-9A7D-49A6-81EE-AB2BE7F0254F
label: Verwenden weltweit einsetzbarer Formate
template: detail.hbs
---

# <span id="dev_globalizing.use_global-ready_formats"></span>Verwenden weltweit einsetzbarer Formate


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**Wichtige APIs**

-   [**Windows.Globalization.Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724)
-   [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
-   [**Windows.Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)

Entwickeln Sie eine für den globalen Markt geeignete App mit geeigneter Formatierung für Datumsangaben, Uhrzeiten, Zahlen und Währungen. Dadurch können Sie die App später zur weltweiten Vermarktung für weitere Kulturkreise, Regionen und Sprachen anpassen.

## <span id="Introduction"></span><span id="introduction"></span><span id="INTRODUCTION"></span>Einführung


Viele App-Entwickler denken beim Erstellen ihrer Apps naturgemäß nur an ihre eigene Sprache und Kultur. Wenn die App in andere Märkte Einzug hält, kann die Anpassung der App an neue Sprachen und Regionen unerwartet schwierig sein. Datumsangaben, Uhrzeiten, Zahlen, Kalender, Währungen, Telefonnummern, Maßeinheiten und Papierformate sind Beispiele für Elemente, die in den verschiedenen Kulturen oder Sprachen anders angezeigt werden können.

Zur Vereinfachung der Anpassung an neue Märkte können Sie bereits bei der App-Entwicklung einige Dinge berücksichtigen.

## <span id="Prerequisites"></span><span id="prerequisites"></span><span id="PREREQUISITES"></span>Voraussetzungen


[Planen für einen globalen Markt](https://msdn.microsoft.com/library/windows/apps/hh465405)
## <span id="Tasks"></span><span id="tasks"></span><span id="TASKS"></span>Aufgaben


1.  **Formatieren Sie Datumsangaben und Uhrzeiten entsprechend.**

    Es gibt viele verschiedene Möglichkeiten, um Datumsangaben und Uhrzeiten korrekt anzuzeigen. In den verschiedenen Regionen und Kulturen gelten unterschiedliche Konventionen für die Reihenfolge von Tag und Monat im Datum, für die Trennung von Stunden und Minuten und sogar für das zu verwendende Trennzeichen. Zudem kann das Datum in verschiedenen langen Formaten (Mittwoch, 28. März 2012) oder kurzen Formaten (28.03.12) angezeigt werden, die je nach Kultur variieren können. Und natürlich sind die Namen und Kurzformen für die Wochentage und Monate in jeder Sprache anders.

    Wenn Sie Benutzern erlauben möchten, ein Datum oder eine Uhrzeit auszuwählen, verwenden Sie die Standardsteuerelemente zur [Datums- und Uhrzeitauswahl](https://msdn.microsoft.com/library/windows/apps/hh465466). Diese verwenden automatisch die Datums- und Uhrzeitformate für die bevorzugte Sprache und Region des Benutzers.

    Wenn Sie selbst Datumsangaben oder Uhrzeiten anzeigen müssen, verwenden Sie die Formatierer [**Date/Time**](https://msdn.microsoft.com/library/windows/apps/br206859) und [**Number**](https://msdn.microsoft.com/library/windows/apps/br226136), um automatisch das vom Benutzer bevorzugte Format für Datumsangaben, Uhrzeiten und Zahlen anzuzeigen. Der folgende Code formatiert eine bestimmte Datum/Uhrzeit-Angabe gemäß der bevorzugten Sprache und Region. Wenn das aktuelle Datum z. B. der 3. Juni 2012 ist, zeigt der Formatierer 6/3/2012 an, wenn die bevorzugte Sprache des Benutzers „Englisch (USA)“ ist, aber 03.06.2012, wenn die bevorzugte Sprache des Benutzers „Deutsch (Deutschland)“ ist:

    **C#**
    ```    CSharp
    // Use the Windows.Globalization.DateTimeFormatting.DateTimeFormatter class
    // to display dates and times using basic formatters.

    // Formatters for dates and times, using shortdate format.
    var sdatefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate");
    var stimefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shorttime");

    // Obtain the date that will be formatted.
    var dateToFormat = DateTime.Now;

    // Perform the actual formatting.
    var sdate = sdatefmt.Format(dateToFormat);
    var stime = stimefmt.Format(dateToFormat);

    // Results for display.
    var results = "Short Date: " + sdate + "\n" +
                  "Short Time: " + stime;
    ```
    **JavaScript**
    ```    JavaScript
    // Use the Windows.Globalization.DateTimeFormatting.DateTimeFormatter class
    // to display dates and times using basic formatters.

    // Formatters for dates and times, using shortdate format.
    var sdatefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate");
    var stimefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shorttime");

    // Obtain the date that will be formatted.
    var dateToFormat = new Date();

    // Perform the actual formatting.
    var sdate = sdatefmt.format(dateToFormat);
    var stime = stimefmt.format(dateToFormat);

    // Results for display.
    var results = "Short Date: " + sdate + "\n" +
                  "Short Time: " + stime;
    ```

2.  **Formatieren Sie Zahlen und Währungen entsprechend.**

    In verschiedenen Kulturen werden Zahlen unterschiedlich formatiert. Formatunterschiede können sich auf Folgendes beziehen: wie viele Dezimalziffern angezeigt werden, welche Zeichen als Dezimaltrennzeichen verwendet werden und welches Währungssymbol verwendet wird. Verwenden Sie [**NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136), um Dezimal-, Prozent- oder Promillezahlen und Währungen anzuzeigen. In den meisten Fällen zeigen Sie Zahlen oder Währungen einfach gemäß den aktuellen Einstellungen des Benutzers an. Sie können aber auch die Formatierer verwenden, um eine Währung für eine bestimmte Region oder ein bestimmtes Format anzuzeigen.

    Der folgende Code veranschaulicht, wie Währungen gemäß der vom Benutzer bevorzugten Sprache oder Region oder für ein bestimmtes Währungssystem angezeigt werden:

    **C#**
    ```    CSharp
    // This scenario uses the Windows.Globalization.NumberFormatting.CurrencyFormatter class
    // to format a number as a currency.

    // Determine the current user's default currency.
    var userCurrency = Windows.System.UserProfile.GlobalizationPreferences.Currencies[0];

    // Number to be formatted.
    var fractionalNumber = 12345.67;

    // Currency formatter using the current user's preference settings for number formatting.
    var userCurrencyFormat = new Windows.Globalization.NumberFormatting.CurrencyFormatter(userCurrency);
    var currencyDefault = userCurrencyFormat.Format(fractionalNumber);

    // Create a formatter initialized to a specific currency,
    // in this case US Dollar (specified as an ISO 4217 code) 
    // but with the default number formatting for the current user.
    var currencyFormatUSD = new Windows.Globalization.NumberFormatting.CurrencyFormatter("USD"); 
    var currencyUSD = currencyFormatUSD.Format(fractionalNumber);

    // Create a formatter initialized to a specific currency.
    // In this case it's the Euro with the default number formatting for France.
    var currencyFormatEuroFR = new Windows.Globalization.NumberFormatting.CurrencyFormatter("EUR", new[] { "fr-FR" }, "FR");
    var currencyEuroFR = currencyFormatEuroFR.Format(fractionalNumber);

    // Results for display.
    var results = "Fixed number (" + fractionalNumber + ")\n" +
                  "With user&#39;s default currency: " + currencyDefault + "\n" +
                  "Formatted US Dollar: " + currencyUSD + "\n" +
                  "Formatted Euro (fr-FR defaults): " + currencyEuroFR;
    ```
    **JavaScript**
    ```    JavaScript
    // This scenario uses the Windows.Globalization.NumberFormatting.CurrencyFormatter class
    // to format a number as a currency.

    // Determine the current user's default currency.
    var userCurrency = Windows.System.UserProfile.GlobalizationPreferences.currencies;

    // Number to be formatted.
    var fractionalNumber = 12345.67;

    // Currency formatter using the current user's preference settings for number formatting.
    var userCurrencyFormat = new Windows.Globalization.NumberFormatting.CurrencyFormatter(userCurrency);
    var currencyDefault = userCurrencyFormat.format(fractionalNumber);

    // Create a formatter initialized to a specific currency,
    // in this case US Dollar (specified as an ISO 4217 code) 
    // but with the default number formatting for the current user.
    var currencyFormatUSD = new Windows.Globalization.NumberFormatting.CurrencyFormatter("USD"); 
    var currencyUSD = currencyFormatUSD.format(fractionalNumber);

    // Create a formatter initialized to a specific currency.
    // In this case it's the Euro with the default number formatting for France.
    var currencyFormatEuroFR = new Windows.Globalization.NumberFormatting.CurrencyFormatter("EUR", ["fr-FR"], "FR");
    var currencyEuroFR = currencyFormatEuroFR.format(fractionalNumber);

    // Results for display.
    var results = "Fixed number (" + fractionalNumber + ")\n" +
                  "With user&#39;s default currency: " + currencyDefault + "\n" +
                  "Formatted US Dollar: " + currencyUSD + "\n" +
                  "Formatted Euro (fr-FR defaults): " + currencyEuroFR;
    ```

3.  **Verwenden Sie einen kulturspezifischen Kalender.**

    Der Kalender ist für verschiedene Regionen und Sprachen unterschiedlich. Der gregorianische Kalender ist nicht der Standardkalender für alle Regionen. In einigen Regionen wählen Benutzer möglicherweise alternative Kalender aus, z. B. den japanischen Ära-Kalender oder den arabischen Mondkalender. In Datumsangaben und Uhrzeiten im Kalender werden auch verschiedene Zeitzonen und Sommerzeiten berücksichtigt.

    Verwenden Sie die Standardsteuerelemente zur [Datums- und Uhrzeitauswahl](https://msdn.microsoft.com/library/windows/apps/hh465466), um Benutzern die Wahl eines Datums zu ermöglichen und die Verwendung des bevorzugten Kalenderformats zu gewährleisten. Bei komplexeren Szenarien, in denen direkt mit Vorgängen in Bezug auf das Kalenderdatum gearbeitet werden muss, stellt „Windows.Globalization“ eine [**Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724)-Klasse bereit, die eine passende Kalenderdarstellung für die jeweilige Kultur, Region und den Kalendertyp ermöglicht.

4.  **Respektieren Sie die Sprach- und Kultureinstellungen der Benutzer.**

    Für Szenarien, in denen Sie basierend auf den Sprach-, Regions- und Kultureinstellungen des Benutzers unterschiedliche Funktionen bereitstellen, bietet Windows die Möglichkeit, über [**Windows.System.UserProfile.GlobalizationPreferences**](https://msdn.microsoft.com/library/windows/apps/br241825) auf diese Einstellungen zuzugreifen. Verwenden Sie bei Bedarf die **GlobalizationPreferences**-Klasse, um den Wert der aktuellen Region des Benutzers sowie die bevorzugten Sprachen, Währungen usw. zu ermitteln.

## <span id="related_topics"></span>Verwandte Themen


* [Planen für einen globalen Markt](https://msdn.microsoft.com/library/windows/apps/hh465405)
* [Richtlinien für Datums- und Uhrzeitsteuerelemente](https://msdn.microsoft.com/library/windows/apps/hh465466)

**Referenz**
* [**Windows.Globalization.Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724)
* [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
* [**Windows.Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)
* [**Windows.System.UserProfile.GlobalizationPreferences**](https://msdn.microsoft.com/library/windows/apps/br241825)

**Beispiele**
* [Kalenderdetails und Mathematikbeispiel](http://go.microsoft.com/fwlink/p/?linkid=231636)
* [Beispiel für Datums- und Uhrzeitformatierung](http://go.microsoft.com/fwlink/p/?linkid=231618)
* [Beispiel für Globalisierungseinstellungen](http://go.microsoft.com/fwlink/p/?linkid=231608)
* [Beispiel für Zahlenformatierung und Analyse](http://go.microsoft.com/fwlink/p/?linkid=231620)
 

 





<!--HONumber=Mar16_HO1-->


