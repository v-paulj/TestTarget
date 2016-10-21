---
author: mijacobs
Description: "Verwenden Sie Ein- und Ausblendungsanimationen, um Elemente anzuzeigen oder nicht anzuzeigen. Die beiden üblichen Animationen dieser Art sind das Einblenden und das Ausblenden."
title: Ein-und Ausblendungsanimationen in UWP-Apps
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 9999c353bfc7a72f04ba8ae0052212f9f2166703

---

# Ein- und Ausblendungsanimationen

Verwenden Sie Ein- und Ausblendungsanimationen, um Elemente anzuzeigen oder nicht anzuzeigen. Die beiden üblichen Animationen dieser Art sind das Einblenden und das Ausblenden.

**Wichtige APIs**

-   [**FadeInThemeAnimation-Klasse**](https://msdn.microsoft.com/library/windows/apps/br210298)
-   [**FadeOutThemeAnimation-Klasse**](https://msdn.microsoft.com/library/windows/apps/br210302)


## Empfohlene und nicht empfohlene Vorgehensweisen


-   Wenn die App zwischen nicht verbundenen oder textlastigen Elementen wechselt, sollten Sie eine Ausblendungsanimation gefolgt von einer Einblendungsanimation verwenden. Auf diese Weise kann das ausgehende Objekt vollständig ausgeblendet werden, bevor das eingehende Objekt sichtbar wird.
-   Blenden Sie die eingehenden Elemente über den ausgehenden Elementen ein, wenn die Größe der Elemente konstant bleibt und die Benutzer den Eindruck haben sollen, das gleiche Element zu sehen. Sobald die Einblendung abgeschlossen ist, kann das ausgehende Element entfernt werden. Diese Vorgehensweise ist nur dann geeignet, wenn das ausgehende Element vom eingehenden Element vollständig verdeckt wird.
-   Vermeiden Sie es, dass bei Ein- und Ausblendungsanimationen Elemente einer Liste hinzugefügt oder daraus gelöscht werden sollen. Verwenden Sie stattdessen die zu diesem Zweck erstellten Listenanimationen.
-   Verwenden Sie Ein- und Ausblendungsanimationen möglichst nicht, um den gesamten Inhalt einer Seite zu ändern. Verwenden Sie stattdessen die zu diesem Zweck erstellten Seitenübergangsanimationen.
-   Das Ausblenden ist eine gute Möglichkeit, um ein Element ohne großen Aufwand zu entfernen.
## Verwandte Artikel

**Für Entwickler (XAML)**
* [Übersicht über Animationen](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animieren von Ein- und Ausblendungen](https://msdn.microsoft.com/library/windows/apps/xaml/jj649429)
* [Schnellstart: Animieren der Benutzeroberfläche mithilfe von Bibliothekanimationen](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**FadeInThemeAnimation-Klasse**](https://msdn.microsoft.com/library/windows/apps/br210298)
* [**FadeOutThemeAnimation-Klasse**](https://msdn.microsoft.com/library/windows/apps/br210302)

 

 







<!--HONumber=Aug16_HO3-->


