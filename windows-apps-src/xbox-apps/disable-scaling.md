---
author: payzer
title: So wird's gemacht - Deaktivieren der Skalierung
description: 
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 192de32bf3afd11cd375655ad92d194ccb09dae1
ms.openlocfilehash: 307606bc290e9c5268fc5a37b72770d6b1ada4da

---

# So wird's gemacht - Deaktivieren der Skalierung   
Standardmäßig werden Anwendungen für XAML-Apps auf 200Prozent und für HTML-Apps auf 150Prozent skaliert. Der Standardskalierungsfaktor kann deaktiviert werden. Infolgedessen verwendet die Anwendung die tatsächlichen Pixelabmessungen des Geräts (1910 x 1080 Pixel).   
   
## HTML   
Sie können den Skalierungsfaktor deaktivieren, indem Sie den folgenden Codeausschnitt verwenden: 
   
`var result = Windows.UI.ViewManagement.ApplicationViewScaling.trySetDisableLayoutScaling(true);` 

Oder Sie können eine für das Web geeignete Methode verwenden:   

```   
@media (max-height: 1080px) {   
    @-ms-viewport {   
        height: 1080px;   
    }   
}   
```

## XAML
Sie können den Skalierungsfaktor deaktivieren, indem Sie den folgenden Codeausschnitt verwenden:   
   
`bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);`   
   
## DirectX/C++   
DirectX-/C++-Anwendungen werden nicht skaliert. Die automatische Skalierung gilt nur für HTML- und XAML-Anwendungen.   



<!--HONumber=Jul16_HO1-->


