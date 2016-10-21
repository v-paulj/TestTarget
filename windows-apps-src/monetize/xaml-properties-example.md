---
author: mcleanbyron
ms.assetid: d074e9d5-b3e0-4f16-b1e4-02b32ac99b2c
description: Erfahren Sie, wie Werten ** AdControl **-Eigenschaften zugewiesen werden.
title: "Beispiel für XAML-Eigenschaften"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: fb0533aa0ea760bca686276f886f0afcb21bf6f7


---

# Beispiel für XAML-Eigenschaften




Im folgenden XAML-Beispiel wird veranschaulicht, wie Werten [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)-Eigenschaften zugewiesen werden. Wenn eine Eigenschaft nicht festgelegt ist, verwendet **AdControl** Standardwerte, um eine Anzeige zu erstellen, die mit der Benutzeroberfläche der App konsistent ist.

Die Werte dienen als Beispiele. In Ihrem Code legen Sie die Werte dieser Funktionen und Eigenschaften entsprechend für Ihre App fest.

``` syntax
Width="300",
Height="250",
AdUnitId="10865270",
ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1",
IsAutoRefreshEnabled="false",
AdRefreshed="OnAdRefresh",
ErrorOcurred="OnAdError",
IsEngagedChanged="OnAdEngagedChanged"
```

## Verwandte Themen

* [Anzeigenbeispiele bei GitHub](http://aka.ms/githubads)

 



<!--HONumber=Aug16_HO3-->


