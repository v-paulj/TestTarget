---
author: payzer
title: Deaktivieren des Mausmodus
description: 
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 6f4719c98d490cdcac8c799c4c68af55b217cbc5
ms.openlocfilehash: d1ee946693b9f9714b8d570b8ae3718469d2c10d

---

# Deaktivieren des Mausmodus
Der Mausmodus ist standardmäßig für alle Apps aktiviert. Das bedeutet, dass alle Anwendungen, für die die Option nicht deaktiviert wurde, einen Mauszeiger erhalten (ähnlich dem Zeiger im Edge-Browser auf der Konsole). Es wird nachdrücklich empfohlen, diese Option zu deaktivieren und die direktionale Navigation über den Controller zu optimieren.   
   
## HTML   
Um die direktionale Navigation über den Controller in einer JavaScript-UWP-App zu aktivieren, verwenden Sie die JavaScript-Bibliothek [TVHelpers für die direktionale Navigation](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation). Fügen Sie die JavaScript-Datei für die direktionale Navigation in Ihr App-Paket ein, und fügen Sie einen Verweis auf diese in allen HTML-Seiten ein, die eine direktionale Navigation über den Controller erfordern:
```code
<script src="directionalnavigation-1.0.0.0.js"></script>
```
Weitere Informationen finden Sie im [Wiki für direktionale Navigation](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation).

Wenn Sie stattdessen den Mausmodus deaktivieren und die DOM- oder WinRT-Gamepad-APIs direkt verwenden möchten, führen Sie Folgendes für jede Seite aus, die diesen erfordert: 
   
```code
navigator.gamepadInputEmulation = "gamepad";
```   

Diese Eigenschaft ist standardmäßig ```'mouse'```, wodurch der Mausmodus aktiviert wird. Wenn Sie diese auf ```'keyboard'``` festlegen, wird der Mausmodus deaktiviert, und Gamepad-Eingaben generieren DOM-Tastaturereignisse. Wenn Sie diese auf ```'gamepad'``` festlegen, wird der Mausmodus deaktiviert und es werden keine DOM-Tastaturereignisse generiert. Auf diese Weise können Sie nur die DOM- oder WinRT-Gamepad-APIs verwenden.

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
Wenn Sie eine C++-/DirectX-App schreiben, müssen Sie nichts tun. Der Mausmodus gilt nur für HTML- und XAML-Anwendungen.



<!--HONumber=Jul16_HO1-->


