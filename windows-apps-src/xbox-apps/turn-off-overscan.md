---
author: payzer
title: "So wird&quot;s gemacht - Zeichnen der Benutzeroberfläche bis zum Bildschirmrand"
description: 
translationtype: Human Translation
ms.sourcegitcommit: b5961d3266a031ab09a9da63319e9883cf050789
ms.openlocfilehash: cddde27a17e897ab8a68bbed099e532a8cd48f07

---

# So wird's gemacht - Zeichnen der Benutzeroberfläche bis zum Bildschirmrand   
Standardmäßig befinden sich die Begrenzungen von Anwendungen an den Rändern des Viewports, um den fernsehsicheren Bereich zu berücksichtigen (Weitere Informationen finden Sie unter [Entwerfen für Xbox und Fernsehgeräte](../input-and-devices/designing-for-tv.md#tv-safe-area)). 

Wir empfehlen, diese Option zu deaktivieren und bis zum Bildschirmrand zu zeichnen. Wenn Sie bis zum Bildschirmrand zeichnen möchten, fügen Sie den folgenden Code hinzu, wenn die Anwendung gestartet wird:
   
```
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```
   
> [!NOTE]
> Für C++-/DirectX-Anwendungen ist dies nicht relevant. Das System rendert die Anwendung immer bis zum Bildschirmrand.

## Weitere Informationen
- [Bewährte Methoden für Xbox](tailoring-for-xbox.md)
- [UWP auf XboxOne](index.md)



<!--HONumber=Aug16_HO3-->


