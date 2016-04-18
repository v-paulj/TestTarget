---
Description: Mit der Zeitauswahl verfügen Sie über eine standardmäßige Methode, mit der Benutzer einen Zeitwert per Touch-, Maus- oder Tastatureingabe auswählen können.
tite: Zeitauswahl
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Zeitauswahl
template: detail.hbs
---

# Zeitauswahl

Mit der Zeitauswahl verfügen Sie über eine standardmäßige Methode, mit der die Benutzer einen Zeitwert per Touch-, Maus- oder Tastatureingabe auswählen können. 

<span class="sidebar_heading" style="font-weight: bold;">Wichtige APIs</span>

-   [**TimePicker-Klasse**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx)
-   [**Time-Eigenschaft**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.time.aspx)

## Ist dies das richtige Steuerelement?
Verwenden Sie die Zeitauswahl, um Benutzern die Auswahl eines einzelnen Zeitwerts zu ermöglichen.

Weitere Informationen zur Auswahl des passenden Steuerelements finden Sie im Artikel über [Datums- und Uhrzeitsteuerelemente](date-and-time.md).

## Beispiele

Der Einstiegspunkt zeigt die ausgewählte Uhrzeit an, und wenn der Benutzer den Einstiegspunkt auswählt, wird eine Auswahloberfläche von der Bildschirmmitte aus vertikal erweitert, damit er eine Auswahl treffen kann. Die Zeitauswahl überlagert andere Elemente der Benutzeroberfläche. Die anderen Elemente der Benutzeroberfläche werden dadurch jedoch nicht „verschoben“.

![Beispiel für die Erweiterung der Zeitauswahl](images/controls_timepicker_expand.png)

## Erstellen einer Zeitauswahl

Dieses Beispiel zeigt, wie Sie ein einfaches Zeitauswahl-Steuerelement mit einer Kopfzeile erstellen.

```xaml
<TimePicker x:Name=arrivalTimePicker Header="Arrival time"/>
```

```csharp
TimePicker arrivalTimePicker = new TimePicker();
arrivalTimePicker.Header = "Arrival time";
```

Die fertige Zeitauswahl sieht wie folgt aus:

![Beispiel für Zeitauswahl](images/time-picker-closed.png)

> **Hinweis**&nbsp;&nbsp;Wichtige Informationen zu *Datums- und Zeitwerten* finden Sie im Artikel [Datums- und Uhrzeitsteuerelemente](date-and-time.md#datetime-and-calendar-values).

\[Dieser Artikel enthält spezielle Informationen zu Apps für die universelle Windows-Plattform (UWP) und Windows 10. Laden Sie für Windows 8.1 die [PDF-Datei mit Windows 8.1-Richtlinien](https://go.microsoft.com/fwlink/p/?linkid=258743) herunter.\]

## Verwandte Themen

- [Datums- und Uhrzeitsteuerelemente](date-and-time.md)
- [Kalenderdatumsauswahl](calendar-date-picker.md)
- [Kalenderansicht](calendar-view.md)
- [Datumsauswahl](date-picker.md)


<!--HONumber=Mar16_HO1-->


