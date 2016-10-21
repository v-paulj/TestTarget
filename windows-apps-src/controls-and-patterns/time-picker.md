---
author: Jwmsft
Description: "Mit der Zeitauswahl verfügen Sie über eine standardmäßige Methode, mit der die Benutzer einen Zeitwert per Touch-, Maus- oder Tastatureingabe auswählen können."
title: Zeitauswahl
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Time picker
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 69f682b0edddbcf88515af537c33b3d8297f91f0

---
# Zeitauswahl
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Mit der Zeitauswahl verfügen Sie über eine standardmäßige Methode, mit der die Benutzer einen Zeitwert per Touch-, Maus- oder Tastatureingabe auswählen können. 

<div class="important-apis" >
<b>Wichtige APIs</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx"><strong>TimePicker-Klasse</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.time.aspx"><strong>Time-Eigenschaft</strong></a></li>
</ul>

</div>
</div>




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

> **Hinweis**&nbsp;&nbsp;Wichtige Informationen zu Uhrzeit- und Datumswerten finden Sie unter [DateTime- und Calendar-Werte](date-and-time.md#datetime-and-calendar-values) im Artikel *Steuerelemente für Datum und Uhrzeit*.



## Verwandte Themen

- [Steuerelemente für Datum und Uhrzeit](date-and-time.md)
- [Kalenderdatumsauswahl](calendar-date-picker.md)
- [Kalenderansicht](calendar-view.md)
- [Datumsauswahl](date-picker.md)



<!--HONumber=Aug16_HO3-->


