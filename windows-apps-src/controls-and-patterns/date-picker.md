---
author: Jwmsft
Description: "Mit der Datumsauswahl verfügen Sie über eine standardisierte Methode, Benutzern die Möglichkeit zu bieten, einen lokalisierten Datumswert per Touch-, Maus- oder Tastatureingabe auszuwählen."
title: Datumsauswahl
ms.assetid: d4a01425-4dee-4de3-9a05-3e85c3fc03cb
isNew: true
label: Date picker
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 2253172ff20ae46b0ada556551adfeac62136398

---
# Datumsauswahl

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Mit der Datumsauswahl verfügen Sie über eine standardisierte Methode, Benutzern die Möglichkeit zu bieten, einen lokalisierten Datumswert per Touch-, Maus- oder Tastatureingabe auszuwählen. 

<div class="important-apis" >
<b>Wichtige APIs</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx"><strong>DatePicker-Klasse</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.date.aspx"><strong>Date-Eigenschaft</strong></a></li>
</ul>

</div>
</div>






## Ist dies das richtige Steuerelement?
Verwenden Sie eine Datumsauswahl, damit Benutzer ein bekanntes Datum wie etwa einen Geburtstag auswählen können, bei dem der Kalenderkontext unwichtig ist.

Weitere Informationen zur Auswahl des richtigen Datumssteuerelements finden Sie im Artikel über [Datums- und Uhrzeitsteuerelemente](date-and-time.md).

## Beispiele

Der Einstiegspunkt zeigt das ausgewählte Datum an, und wenn der Benutzer den Einstiegspunkt auswählt, wird eine Auswahloberfläche von der Bildschirmmitte aus vertikal erweitert, damit eine Auswahl getroffen werden kann. Die Datumsauswahl überlagert andere Elemente der Benutzeroberfläche; die anderen Elemente der Benutzeroberfläche werden jedoch nicht „beiseitegeschoben“.

![Beispiel für die Erweiterung der Datumsauswahl](images/controls_datepicker_expand.png)

## Erstellen einer Datumsauswahl

Dieses Beispiel zeigt, wie Sie eine einfache Datumsauswahl mit einer Kopfzeile erstellen.

```xaml
<DatePicker x:Name=birthDatePicker Header="Date of birth"/>
```

```csharp
DatePicker birthDatePicker = new DatePicker();
birthDatePicker.Header = "Date of birth";
```

Die fertige Datumsauswahl sieht wie folgt aus:

![Beispiel für eine Datumsauswahl](images/date-picker-closed.png)

> **Hinweis**&nbsp;&nbsp;Wichtige Informationen zu Datumswerten finden Sie im Artikel zu Datums- und Uhrzeitsteuerelementen unter [DateTime- und Calendar-Werte](date-and-time.md#datetime-and-calendar-values).



## Verwandte Artikel

- [Datums- und Uhrzeitsteuerelemente](date-and-time.md)
- [Kalenderdatumsauswahl](calendar-date-picker.md)
- [Kalenderansicht](calendar-view.md)
- [Uhrzeitauswahl](time-picker.md)



<!--HONumber=Aug16_HO3-->


