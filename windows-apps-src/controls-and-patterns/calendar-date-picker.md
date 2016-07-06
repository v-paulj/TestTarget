---
author: Jwmsft
Description: "Die Kalenderdatumsauswahl ist ein Dropdownsteuerelement, das für die Auswahl eines einzelnen Datums in einer Kalenderansicht optimiert ist, in der kontextbezogene Informationen wie der Wochentag oder die Belegung des Kalenders von Bedeutung sind."
title: Kalenderdatumsauswahl
ms.assetid: 9e0213e0-046a-4906-ba86-0b49be51ca99
label: Calendar date picker
template: detail.hbs
ms.sourcegitcommit: 7d438080e2e8533f1148c07e27143d4d1fcacf5d
ms.openlocfilehash: 7ae444d9e096834daa2eef71c691b9bb8b86d7c4

---

# Kalenderdatumsauswahl

Die Kalenderdatumsauswahl ist ein Dropdownsteuerelement, das für die Auswahl eines einzelnen Datums in einer Kalenderansicht optimiert ist, in der kontextbezogene Informationen wie der Wochentag oder die Belegung des Kalenders von Bedeutung sind. Sie können den Kalender bearbeiten, um Kontext hinzuzufügen oder verfügbare Tage einzugrenzen.



-   [**CalendarDatePicker-Klasse**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.aspx)
-   [**Date-Eigenschaft**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.date.aspx)
-   [**DateChanged-Ereignis**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.datechanged.aspx)

## Ist dies das richtige Steuerelement?
Benutzer können in einer **Kalenderdatumsauswahl** in einer kontextbezogenen Kalenderansicht ein einzelnes Datum auswählen. Dies eignet sich beispielsweise, um Termine oder Abflugzeiten einzutragen.

Damit Benutzer ein bekanntes Datum wie etwa einen Geburtstag eintragen können, bei dem der Kalenderkontext unwichtig ist, könnte eine [**Datumsauswahl**](date-picker.md) von Vorteil sein.

Weitere Informationen zur Auswahl des passenden Steuerelements finden Sie im Artikel über [Datums- und Uhrzeitsteuerelemente](date-and-time.md).

## Beispiele

Der Einstiegspunkt zeigt Platzhaltertext an, falls kein Datum festgelegt wurde. Andernfalls wird das ausgewählte Datum angezeigt. Wenn Benutzer den Einstiegspunkt auswählen, wird eine Kalenderansicht eingeblendet, damit sie ein Datum auswählen können. Die Kalenderansicht überlagert andere Elemente der Benutzeroberfläche. Die anderen Elemente der Benutzeroberfläche werden dadurch jedoch nicht „verschoben“.

![Beispiel für Kalenderdatumsauswahl](images/calendar-date-picker-2-views.png)

## Erstellen einer Datumsauswahl

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date"/>
```

```csharp
CalendarDatePicker arrivalCalendarDatePicker = new CalendarDatePicker();
arrivalCalendarDatePicker.Header = "Arrival date";
```

Die fertige Kalenderdatumsauswahl sieht wie folgt aus:

![Beispiel für Kalenderdatumsauswahl](images/calendar-date-picker-closed.png)

Die Kalenderdatumsauswahl verfügt über eine interne [**CalendarView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.aspx)-Klasse, um ein Datum auszuwählen. In CalendarDatePicker ist eine Teilmenge von CalendarView-Eigenschaften, wie [**IsTodayHighlighted**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.istodayhighlighted.aspx) und [**FirstDayOfWeek**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.firstdayofweek.aspx) vorhanden. Diese werden an die interne „CalendarView“-Klasse weitergeleitet, damit Sie sie ändern können. 

Die [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectionmode.aspx)-Eigenschaft der internen CalendarView-Klasse können Sie jedoch nicht ändern und somit keine Mehrfachauswahl zulassen. Falls Benutzer mehrere Tage auswählen sollen oder permanent ein Kalender sichtbar sein muss, bietet sich möglicherweise anstelle der Kalenderdatumsauswahl eine Kalenderansicht an. Weitere Informationen zum Ändern der Kalenderanzeige finden Sie im Artikel zur [Kalenderansicht](calendar-view.md).

### Auswählen von Tagen

Verwenden Sie die [**Date**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.date.aspx)-Eigenschaft, um das ausgewählte Datum abzurufen oder festzulegen. Der Standardwert der „Date“-Eigenschaft lautet **null**. Wenn Benutzer ein Datum in der Kalenderansicht auswählen, wird diese Eigenschaft aktualisiert. Benutzer können die Auswahl eines Datums aufheben, indem sie in der Kalenderansicht auf das Datum klicken. 

Sie können das Datum im Code wie folgt festlegen.

```csharp
myCalendarDatePicker.Date = new DateTime(1977, 1, 5);
```

Wenn SIe das Datum in Code festlegen, wird der Wert durch die Eigenschaften [**MinDate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.mindate.aspx) und [**MaxDate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.maxdate.aspx) beschränkt.
- Wenn **Date** vor **MinDate** liegt, wird der Wert auf **MinDate** gesetzt.
- Wenn **Date** nach **MaxDate** liegt, wird der Wert auf **MaxDate** gesetzt.

Mithilfe des [**DateChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.datechanged.aspx)-Ereignisses können Sie sich bei Änderungen am „Date“-Wert benachrichtigen lassen.

> **Hinweis**
            &nbsp;&nbsp;Wichtige Informationen zu Datumswerten finden Sie im Artikel zu Datums- und Uhrzeitsteuerelementen unter [DateTime- und Calendar-Werte](date-and-time.md#datetime-and-calendar-values).

### Festlegen von Kopfzeilen- und Platzhaltertext

Sie können der Kalenderdatumsauswahl eine [**Header**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.header.aspx)-Eigenschaft (oder eine Beschriftung) und eine [**PlaceholderText**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.placeholdertext.aspx)-Eigenschaft (oder ein Wasserzeichen) hinzufügen, um Benutzern einen Hinweis bezüglich des Verwendungszwecks zu geben. Um das Erscheinungsbild der Kopfzeile anzupassen, können Sie statt der „Header“ Eigenschaft die [**HeaderTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.headertemplate.aspx)-Eigenschaft festlegen.

Als Standardtext für den Platzhalter wird „Datum auswählen“ angezeigt. Sie können diesen Text entfernen, indem Sie die PlaceholderText-Eigenschaft auf eine leere Zeichenfolge festlegen. Alternativ können Sie wie hier gezeigt einen benutzerdefinierten Text eingeben.

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date" 
                    PlaceholderText="Choose your arrival date"/>
```


## Verwandte Artikel

- [Datums- und Uhrzeitsteuerelemente](date-and-time.md)
- [Kalenderansicht](calendar-view.md)
- [Datumsauswahl](date-picker.md)
- [Uhrzeitauswahl](time-picker.md)



<!--HONumber=Jun16_HO4-->


