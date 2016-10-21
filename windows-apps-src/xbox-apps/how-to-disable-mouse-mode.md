---
author: payzer
title: Deaktivieren des Mausmodus
description: Anleitungen zum Deaktivieren des Standardmausmodus.
translationtype: Human Translation
ms.sourcegitcommit: b4df1f944d909640791e4ed7e3bcf8d8bdf7a0d1
ms.openlocfilehash: 91e530a3313d53c4e693b88a64b849f3188a72de

---

# Deaktivieren des Mausmodus
Der Mausmodus ist standardmäßig für alle Anwendungen aktiviert. Das bedeutet, dass alle Anwendungen, für die die Option nicht deaktiviert wurde, einen Mauszeiger erhalten (ähnlich dem Zeiger im Edge-Browser auf der Konsole). Es wird nachdrücklich empfohlen, diese Option zu deaktivieren und die direktionale Navigation über den Controller zu optimieren.   
   
## HTML   
Um die direktionale Navigation über den Controller in einer JavaScript-UWP-App (Universelle Windows-Plattform) zu aktivieren, verwenden Sie die JavaScript-Bibliothek [TVHelpers für die direktionale Navigation](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation). Fügen Sie die JavaScript-Datei für die direktionale Navigation in Ihr App-Paket ein, und fügen Sie einen Verweis auf diese in allen HTML-Seiten ein, die eine direktionale Navigation über den Controller erfordern:

```code
<script src="directionalnavigation-1.0.0.0.js"></script>
```
Weitere Informationen finden Sie im [Wiki für direktionale Navigation](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation).

Wenn Sie stattdessen den Mausmodus deaktivieren und die DOM- oder WinRT-Gamepad-APIs direkt verwenden möchten, führen Sie folgende Schritte für jede Seite aus, für die dies erforderlich ist: 
   
```code
navigator.gamepadInputEmulation = "gamepad";
```   

   Diese Eigenschaft ist standardmäßig auf `mouse` festgelegt, wodurch der Mausmodus aktiviert wird. Wenn Sie diese auf `keyboard` festlegen, wird der Mausmodus deaktiviert, und Gamepad-Eingaben generieren DOM-Tastaturereignisse. Wenn Sie diese auf `gamepad` festlegen, wird der Mausmodus deaktiviert, und es werden keine DOM-Tastaturereignisse generiert. Auf diese Weise können Sie nur die DOM- oder WinRT-Gamepad-APIs verwenden.

## XAML    
Um den Mausmodus zu deaktivieren, fügen Sie dem Konstruktor für Ihre App Folgendes hinzu:   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
}
```

## C++/DirectX   
Wenn Sie eine C++-/DirectX-App schreiben, müssen Sie keine Schritte ausführen. Der Mausmodus gilt nur für HTML- und XAML-Anwendungen.

## Weitere Informationen
- [Bewährte Methoden für Xbox](tailoring-for-xbox.md)
- [UWP auf XboxOne](index.md)




<!--HONumber=Aug16_HO3-->


