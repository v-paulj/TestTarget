---
author: mcleanbyron
ms.assetid: d074e9d5-b3e0-4f16-b1e4-02b32ac99b2c
description: Erfahren Sie, wie ** AdControl **-Eigenschaften Werten zugewiesen werden.
title: Beispiel für XAML-Eigenschaften

---

# Beispiel für XAML-Eigenschaften


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Im folgenden XAML-Beispiel wird veranschaulicht, wie [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)-Eigenschaften Werten zugewiesen werden. Wenn eine Eigenschaft nicht festgelegt ist, verwendet **AdControl** Standardwerte, um eine Anzeige zu erstellen, die mit der Benutzeroberfläche der App konsistent ist.

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

 


<!--HONumber=May16_HO2-->


