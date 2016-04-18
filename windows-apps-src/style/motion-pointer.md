---
Description: Verwenden Sie Zeigeranimationen, um Benutzern visuelles Feedback zu liefern, wenn diese auf ein Element tippen.
title: Animationen für Zeigerklicks in UWP-Apps
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
label: Motion--Pointer animations
template: detail.hbs
---

# Animationen für Zeigerklicks

Verwenden Sie Zeigeranimationen, um Benutzern visuelles Feedback zu liefern, wenn diese auf ein Element tippen. Bei der Animation für „Zeiger nach unten“ wird das gedrückte Element leicht verkleinert und geneigt. Sie wird wiedergegeben, wenn erstmalig auf ein Element getippt wird. Die Animation für „Zeiger nach oben“, mit der der ursprüngliche Zustand des Elements wiederhergestellt wird, wird beim Loslassen des Zeigers wiedergegeben.

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**PointerUpThemeAnimation-Klasse**](https://msdn.microsoft.com/library/windows/apps/hh969168)
-   [**PointerDownThemeAnimation-Klasse**](https://msdn.microsoft.com/library/windows/apps/hh969164)



## Empfohlene und nicht empfohlene Vorgehensweisen


-   Wenn Sie eine Animation für „Zeiger hoch“ verwenden, sollte die Animation ausgelöst werden, nachdem der Benutzer den Zeiger losgelassen hat. So wird Benutzern sofortiges Feedback geliefert, dass ihre Aktion erkannt wurde. Dies gilt auch, wenn die durch das Tippen ausgelöste Aktion (beispielsweise Navigieren zu einer neuen Seite) langsamer reagiert.

## Verwandte Artikel

**Für Entwickler (XAML)**
* [Übersicht über Animationen](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animieren von Zeigerklicks](https://msdn.microsoft.com/library/windows/apps/xaml/jj649432)
* [Schnellstart: Animieren der Benutzeroberfläche mithilfe von Bibliothekanimationen](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**PointerUpThemeAnimation-Klasse**](https://msdn.microsoft.com/library/windows/apps/hh969168)
* [**PointerDownThemeAnimation-Klasse**](https://msdn.microsoft.com/library/windows/apps/hh969164)

 

 






<!--HONumber=Mar16_HO3-->


