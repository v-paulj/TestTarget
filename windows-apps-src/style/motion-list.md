---
author: mijacobs
Description: Mit Listenanimationen können Sie einzelne oder mehrere Elemente in einer Sammlung wie z. B. einem Fotoalbum oder einer Liste mit Suchergebnissen einfügen oder entfernen.
title: Hinzufügen und Löschen von Animationen in UWP-Apps
ms.assetid: A85006AE-4992-457a-B514-500B8BEF5DC8
label: Motion--add and delete animations
template: detail.hbs
---

# Hinzufügen und Löschen von Animationen




Mit Listenanimationen können Sie einzelne oder mehrere Elemente in einer Sammlung wie z. B. einem Fotoalbum oder einer Liste mit Suchergebnissen einfügen oder entfernen.

**Wichtige APIs**

-   [**AddDeleteThemeTransition-Klasse**](https://msdn.microsoft.com/library/windows/apps/br243048)


## Empfohlene und nicht empfohlene Vorgehensweisen


-   Verwenden Sie Listenanimationen, um einer bestehenden Menge von Elementen ein einzelnes neues Element hinzuzufügen. Verwenden Sie diese Animationen beispielsweise, wenn eine neue E-Mail eingeht oder ein neues Foto in eine vorhandene Serie importiert wird.
-   Verwenden Sie Listenanimationen, um einer bestehenden Menge von Elementen mehrere Elemente gleichzeitig hinzuzufügen. Verwenden Sie diese Animationen beispielsweise, wenn Sie eine neue Fotoserie in eine vorhandene Sammlung importieren. Wenn mehrere Elemente hinzugefügt oder gelöscht werden, sollte der Vorgang für alle Elemente gleichzeitig erfolgen, ohne dass es zwischen den Aktionen für die einzelnen Objekte zu Verzögerungen kommt.
-   Verwenden Sie Listenanimationen für das Hinzufügen und Löschen als Paar. Verwenden Sie immer, wenn Sie eine dieser Animationen verwenden, die zugehörige Animation für die entgegengesetzte Aktion.
-   Verwenden Sie Listenanimationen mit einer Liste von Elementen, in der Sie ein Element oder eine Gruppe von Elementen gleichzeitig hinzufügen oder löschen können.
-   Verwenden Sie Listenanimationen nicht, um einen Container anzuzeigen oder zu entfernen. Diese Animationen sind für Elemente einer bereits angezeigten Auflistung oder Menge vorgesehen. Verwenden Sie Popupanimationen, um einen kurzlebigen Container auf der App-Oberfläche ein- oder auszublenden. Verwenden Sie Inhaltsübergangsanimationen, um einen Container, der Teil der App-Oberfläche ist, anzuzeigen oder zu ersetzen.
-   Verwenden Sie Listenanimationen nicht für eine vollständige Menge von Elementen. Verwenden Sie die Inhaltsübergangsanimationen, um eine vollständige Auflistung innerhalb des Containers hinzuzufügen oder zu entfernen.



## Verwandte Artikel


**Für Entwickler (XAML)**
* [Übersicht über Animationen](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animieren von Hinzufügungen und Löschungen in Listen](https://msdn.microsoft.com/library/windows/apps/xaml/jj649430)
* [Schnellstart: Animieren der Benutzeroberfläche mithilfe von Bibliothekanimationen](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**AddDeleteThemeTransition-Klasse**](https://msdn.microsoft.com/library/windows/apps/br243048)

 

 






<!--HONumber=May16_HO2-->


