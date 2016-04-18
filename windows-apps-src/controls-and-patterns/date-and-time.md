---
Description: Mit Datums- und Uhrzeitsteuerelementen können Sie das Datum und die Uhrzeit anzeigen und festlegen. Dieser Artikel enthält Entwurfsrichtlinien und hilft Ihnen beim Auswählen des richtigen Steuerelements.
title: Richtlinien für Datums- und Uhrzeitsteuerelemente
ms.assetid: 4641FFBB-8D82-4290-94C1-D87617997F61
label: Calendar, date, and time controls
template: detail.hbs
---

# Kalender-, Datums- und Uhrzeitsteuerelemente

Datums- und Uhrzeitsteuerelemente bieten Ihnen standardmäßige lokalisierte Methoden, den Benutzern die Anzeige oder Festlegung von Datums-und Uhrzeitwerten in Ihrer App zu ermöglichen. Dieser Artikel enthält Entwurfsrichtlinien und hilft Ihnen beim Auswählen des richtigen Steuerelements.

<span class="sidebar_heading" style="font-weight: bold;">Wichtige APIs</span>

-   [**CalendarView-Klasse**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.aspx)
-   [**CalendarDatePicker-Klasse**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.aspx)
-   [**DatePicker-Klasse**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx)
-   [**TimePicker-Klasse**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx)

## Welches Datums- bzw. Uhrzeitsteuerelement sollten Sie verwenden?

Es stehen vier Datums- und Uhrzeitsteuerelemente zur Auswahl. Welches Steuerelement Sie verwenden, hängt vom jeweiligen Szenario ab. Die Informationen in diesem Abschnitt sollen Ihnen dabei helfen, das richtige Steuerelement für Ihre App auszuwählen.

&nbsp;|&nbsp;|&nbsp;                                                                                                                      
--------------------|-------|-------------------------------------------------------------------------------------------------------------------------------
Kalenderansicht       |![Beispiel für eine Kalenderansicht](images/controls_calendar_monthview_small.png)|Verwenden Sie diese Ansicht, um ein einzelnes Datum oder einen Datumsbereich aus einem immer sichtbaren Kalender auszuwählen.                   
Kalenderdatumsauswahl|![Beispiel für eine Kalenderdatumsauswahl](images/calendar-date-picker-closed.png)|Verwenden Sie diese Ansicht, um ein einzelnes Datum aus einem kontextbezogenen Kalender auszuwählen. 
Datumsauswahl         |![Beispiel für eine Datumsauswahl](images/date-picker-closed.png)|Verwenden Sie diese Ansicht, um ein einzelnes bekanntes Datum auszuwählen, wenn kontextbezogene Informationen nicht wichtig sind.
Uhrzeitauswahl         |![Beispiel für eine Zeitauswahl](images/time-picker-closed.png)|Verwenden Sie diese Ansicht zum Auswählen eines einzelnen Uhrzeitwertes.                                        

<!-- This table seems redundant, not sure it's needed.-->

### Kalenderansicht

Mit **CalendarView** können Benutzer einen Kalender anzeigen und damit interagieren. Als Ansichten sind Monat, Jahr und zehn Jahre möglich. Benutzer können ein einzelnes Datum oder einen Datumsbereich auswählen. Es gibt keine Auswahloberfläche, und der Kalender ist immer sichtbar.

Die Kalenderansicht besteht aus drei Ansichten: Monat, Jahr und Jahrzehnt. Standardmäßig ist in der Kalenderansicht zunächst die Monatsansicht geöffnet, Sie können jedoch jede Ansicht als Startansicht angeben.

![Beispiel für eine Kalenderdatumsauswahl](images/calendar-view-3-views.png)

- Wenn es wichtig ist, das Benutzer gleichzeitig mehrere Tage auswählen können, müssen Sie eine **CalendarView** verwenden.
- Wenn der Benutzern jeweils nur ein Datum auswählen soll und der Kalender nicht permanent sichtbar sein muss, kann möglicherweise ein Steuerelement **CalendarDatePicker** bzw. **DatePicker** verwendet werden.

### Kalenderdatumsauswahl

**CalendarDatePicker** ist ein Dropdownsteuerelement, das für die Auswahl eines einzelnen Datums aus einer Kalenderansicht optimiert ist, in der kontextbezogene Informationen wie der Wochentag oder die Belegung des Kalenders von Bedeutung sind. Sie können den Kalender bearbeiten, um Kontext hinzuzufügen oder verfügbare Tage einzugrenzen.

Der Einstiegspunkt zeigt Platzhaltertext an, falls kein Datum festgelegt wurde. Andernfalls wird das ausgewählte Datum angezeigt. Wenn Benutzer den Einstiegspunkt auswählen, wird eine Kalenderansicht eingeblendet, damit sie ein Datum auswählen können. Die Kalenderansicht überlagert andere Elemente der Benutzeroberfläche. Die anderen Elemente der Benutzeroberfläche werden dadurch jedoch nicht „beiseitegeschoben“.

![Beispiel für eine Kalenderdatumsauswahl](images/calendar-date-picker-2-views.png)

- Verwenden Sie eine Kalenderdatumsauswahl, um beispielsweise einen Termin oder ein Abreisedatum auszuwählen. 

### Datumsauswahl

Das **DatePicker**-Steuerelement bietet eine standardisierte Methode zum Auswählen eines bestimmten Datums. 

Der Einstiegspunkt zeigt das ausgewählte Datum an, und wenn der Benutzer den Einstiegspunkt auswählt, wird eine Auswahloberfläche von der Bildschirmmitte aus vertikal erweitert, damit eine Auswahl getroffen werden kann. Die Datumsauswahl überlagert andere Elemente der Benutzeroberfläche; die anderen Elemente der Benutzeroberfläche werden jedoch nicht „beiseitegeschoben“.

![Beispiel für die Erweiterung der Datumsauswahl](images/controls_datepicker_expand.png)

- Verwenden Sie eine Datumsauswahl, damit Benutzer ein bekanntes Datum wie etwa einen Geburtstag auswählen können, bei dem der Kalenderkontext unwichtig ist.

### Uhrzeitauswahl

**TimePicker** wird zur Auswahl eines einzelnen Uhrzeitwertes für Termine oder Abreisezeiten verwendet. Es handelt sich um eine statische Anzeige, die vom Benutzer oder im Code festgelegt wird, jedoch nicht aktualisiert wird, um die aktuelle Uhrzeit anzuzeigen. 

Der Einstiegspunkt zeigt die ausgewählte Uhrzeit an, und wenn der Benutzer den Einstiegspunkt auswählt, wird eine Auswahloberfläche von der Bildschirmmitte aus vertikal erweitert, damit eine Auswahl getroffen werden kann. Die Zeitauswahl überlagert andere Elemente der Benutzeroberfläche. Die anderen Elemente der Benutzeroberfläche werden dadurch jedoch nicht „beiseitegeschoben“.

![Beispiel für die Erweiterung der Zeitauswahl](images/controls_timepicker_expand.png)

- Verwenden Sie die Zeitauswahl, um Benutzern die Auswahl eines einzelnen Zeitwerts zu ermöglichen.

## Erstellen eines Datums oder Uhrzeitsteuerelements

Informationen und Beispiele zu den einzelnen Datums- und Textsteuerelementen finden Sie in den folgenden Artikeln.

- [**Kalenderansicht**](calendar-view.md)
- [**Kalenderdatumsauswahl**](calendar-date-picker.md)
- [**Datumsauswahl**](date-picker.md)
- [**Uhrzeitauswahl**](time-picker.md)

### Globalisierung

Die XAML-Datumssteuerelemente unterstützen jedes der von Windows unterstützten Kalendersysteme. Diese Kalender werden in der [**Windows.Globalization.CalendarIdentifiers**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.calendaridentifiers.aspx)-Klasse angegeben. Jedes Steuerelement verwendet den richtigen Kalender für die standardmäßige Sprache Ihrer App. Alternativ können Sie die **CalendarIdentifier**-Eigenschaft festlegen, um ein bestimmtes Kalendersystem zu verwenden.

Das Zeitauswahl-Steuerelement unterstützt jedes der Uhrsysteme, die in der [ **Windows.Globalization.ClockIdentifiers** ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.clockidentifiers.aspx)-Klasse angegeben sind. Sie können für die [ **ClockIdentifier** ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.clockidentifier.aspx)-Eigenschaft festlegen, dass das 12-Stunden- oder 24-Stunden-Uhrzeitformat verwendet werden soll. Die Art der Eigenschaft ist „String“ (Zeichenfolge), Sie müssen jedoch Werte verwenden, die den statischen Zeichenfolgeneigenschaften der ClockIdentifiers-Klasse entsprechen. Dies lauten: TwelveHour (Zeichenfolge „12HourClock“) und TwentyFourHour (Zeichenfolge „24HourClock“). Der Standardwert lautet „12HourClock“


### Werte für Datum/Uhrzeit und Kalender

Die in den XAML-Datums- und Uhrzeitsteuerelementen verwendeten Datumsobjekte weisen je nach Programmiersprache eine unterschiedliche Darstellung auf. 
- C# und Visual Basic verwenden die [ **System.DateTimeOffset** ](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx)-Struktur, die Teil von .NET ist. 
- C++ / CX verwendet die [ **Windows::Foundation::DateTime** ](https://msdn.microsoft.com/library/windows/apps/xaml/br205770.aspx)-Struktur. 

Ein verwandtes Konzept ist die Calendar-Klasse, die beeinflusst, wie die Daten im Kontext interpretiert werden. Alle Windows-Runtime-Apps können die [ **Windows.Globalization.Calendar** ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.calendar.aspx)-Klasse verwenden. C# und Visual Basic-Apps können auch die [ **System.Globalization.Calendar** ](https://msdn.microsoft.com/library/windows/apps/xaml/system.globalization.calendar.aspx)-Klasse verwenden. Diese weist eine sehr ähnliche Funktionalität auf. (Windows-Runtime-Apps können die .NET-Basisklasse Calendar verwenden, nicht jedoch die spezifischen Implementierungen wie z. B. GregorianCalendar.)

.NET unterstützt auch einen Typ mit der Bezeichnung [ **DateTime**](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetime.aspx), der implizit in [ **DateTimeOffset**](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx) konvertierbar ist. Somit könnte ein Typ „DateTime“ in .NET-Code verwendet werden, der zum Festlegen von Werten verwendet wird, bei denen es sich eigentlich um DateTimeOffset handelt. Weitere Informationen zum Unterschied zwischen DateTime und DateTimeOffset finden Sie in den Bemerkungen in der [ **DateTimeOffset** ](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx)-Klasse.

> **Hinweis:**&nbsp;&nbsp;Eigenschaften, die Datumsobjekte verwenden, können nicht als XAML-Attributzeichenfolge festgelegt werden, da der Windows-Runtime-XAML-Parser nicht über eine Konvertierungslogik zum Konvertieren von Zeichenfolgen in Datumsangaben als DateTime/DateTimeOffset Objekte verfügt. In der Regel legen Sie diese Werte im Code fest. Eine andere Möglichkeit besteht darin, ein Datum zu definieren, das als Datenobjekt oder im Datenkontext verfügbar ist, und anschließend die Eigenschaft als XAML-Attribut festzulegen, das auf einen [\{Binding\}-Markuperweiterung](../xaml-platform/binding-markup-extension.md)-Ausdruck verweist, der auf das Datum als Daten zugreifen kann.


## Verwandte Themen

**Für Entwickler (XAML)**
- [**CalendarView-Klasse**](https://msdn.microsoft.com/library/windows/apps/dn890052)
- [**CalendarDatePicker-Klasse**](https://msdn.microsoft.com/library/windows/apps/dn950083)
- [**DatePicker-Klasse**](https://msdn.microsoft.com/library/windows/apps/dn298584)
- [**TimePicker-Klasse**](https://msdn.microsoft.com/library/windows/apps/dn299280)


<!--HONumber=Mar16_HO4-->


