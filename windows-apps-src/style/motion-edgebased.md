---
author: mijacobs
Description: Randbasierte Animationen blenden UI-Elemente ein oder aus, die vom Bildschirmrand ausgehen.
title: "Animationen für randbasierte Benutzeroberflächenelemente in UWP-Apps"
ms.assetid: 5A8F73B1-F4F6-424b-9EDF-A9766C5DEAE8
label: Motion--edge-based UI
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: a835e57c3ae778149cda933d57cf74408b74b416

---

# Animationen für randbasierte Benutzeroberflächenelemente




Randbasierte Animationen blenden UI-Elemente ein oder aus, die vom Bildschirmrand ausgehen. Die Aktionen zum Anzeigen und Ausblenden können von Benutzern oder von der App initiiert werden. Die UI-Elemente können die Anwendung überlagern oder Teil der Hauptoberfläche der App sein. Wenn das UI-Element Teil der App-Oberfläche ist, müssen Sie möglicherweise die Größe der restlichen App entsprechend anpassen.

**Wichtige APIs**

-   [**EdgeUIThemeTransition-Klasse**](https://msdn.microsoft.com/library/windows/apps/hh702324)


## Empfohlene und nicht empfohlene Vorgehensweisen


-   Verwenden Sie Animationen für Randbenutzeroberflächen zum Anzeigen oder Ausblenden einer benutzerdefinierten Meldungs- oder Fehlerleiste, die nicht weit in den Bildschirm hineinreicht.
-   Verwenden Sie Panelanimationen, um Benutzeroberflächen anzuzeigen, die weit in den Bildschirm hineinreichen, beispielsweise ein Aufgabenbereich oder eine benutzerdefinierte Bildschirmtastatur.
-   Lassen Sie die UI von demselben Rand hereingleiten, mit dem sie verbunden ist.
-   Lassen Sie die UI an demselben Rand hinausgleiten, von dem sie hereingekommen ist.
-   Wenn der Inhalt der App aufgrund des Herein- oder Hinausgleitens der Benutzeroberfläche in der Größe angepasst werden muss, verwenden Sie Ein- und Ausblendungsanimationen.
    -   Wenn die Benutzeroberfläche hereingleitet, verwenden Sie nach der Animation für Randbenutzeroberflächen oder Panels eine Ein- oder Ausblendungsanimation.
    -   Wenn die Benutzeroberfläche herausgleitet, verwenden Sie gleichzeitig mit der Animation für Randbenutzeroberflächen oder Panels eine Ein- oder Ausblendungsanimation.
-   Wenden Sie diese Animationen nicht auf Benachrichtigungen an. Benachrichtigungen sollten sich nicht auf der randbasierten Benutzeroberfläche befinden.
-   Wenden Sie die Animation für Randbenutzeroberflächen oder Panels nicht auf Benutzeroberflächencontainer oder Steuerelemente an, die sich nicht am Rand des Bildschirms befinden. Diese Animationen werden nur für das Anzeigen und Schließen von Benutzeroberflächen am Rand des Bildschirms sowie zum Ändern ihrer Größe verwendet. Verwenden Sie zum Verschieben anderer Benutzeroberflächenarten die Animationen für das Ändern der Position.

    ![Veranschaulicht, wann Sie Animationen für Randbenutzeroberflächen oder für Panels verwenden sollten und wann Sie Ändern der Position verwenden sollten.](images/edgevsreposition.png)

## Verwandte Artikel


**Für Entwickler (XAML)**
* [Übersicht über Animationen](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animieren von randbasierten Benutzeroberflächen](https://msdn.microsoft.com/library/windows/apps/xaml/jj649428)
* [Schnellstart: Animieren der Benutzeroberfläche mithilfe von Bibliothekanimationen](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**EdgeUIThemeTransition-Klasse**](https://msdn.microsoft.com/library/windows/apps/hh702324)
* [**PaneThemeTransition-Klasse**](https://msdn.microsoft.com/library/windows/apps/hh969160)
* [Animieren von Ein- und Ausblendungen](https://msdn.microsoft.com/library/windows/apps/xaml/jj649429)
* [Animieren von Änderungen der Position](https://msdn.microsoft.com/library/windows/apps/xaml/jj649434)

 

 







<!--HONumber=Aug16_HO3-->


