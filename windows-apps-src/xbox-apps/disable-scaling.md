---
author: payzer
title: So deaktivieren Sie die Skalierung
description: Anleitung zum Deaktivieren des Standard-Skalierungsfaktors.
translationtype: Human Translation
ms.sourcegitcommit: 582f5677c15f7cd62c398103b48743ba4bea6c5b
ms.openlocfilehash: 8079be9685558277565766fa8d0ebbfd4a555904

---

# So deaktivieren Sie die Skalierung   
Standardmäßig werden Anwendungen für XAML-Apps auf 200Prozent und für HTML-Apps auf 150Prozent skaliert. Der Standardskalierungsfaktor kann deaktiviert werden. Infolgedessen verwendet die Anwendung die tatsächlichen Pixelabmessungen des Geräts (1910 x 1080Pixel).   
   
## HTML   
Sie können den Skalierungsfaktor deaktivieren, indem Sie den folgenden Codeausschnitt verwenden: 
   
```
var result = Windows.UI.ViewManagement.ApplicationViewScaling.trySetDisableLayoutScaling(true);
```

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
   
```
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```
   
## DirectX/C++   
DirectX-/C++-Anwendungen werden nicht skaliert. Die automatische Skalierung gilt nur für HTML- und XAML-Anwendungen.  

## Weitere Informationen
- [Bewährte Methoden für Xbox](tailoring-for-xbox.md)
- [UWP auf XboxOne](index.md)



<!--HONumber=Aug16_HO3-->


