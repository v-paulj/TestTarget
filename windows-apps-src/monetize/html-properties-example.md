---
author: mcleanbyron
ms.assetid: 5fa16a27-fdc0-43b2-84cd-8547fd4915de
description: Erfahren Sie, wie **AdControl**-Eigenschaften in HTML zugewiesen werden.
title: "Beispiel für HTML-Eigenschaften"
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 53b9afdcdc697e3ed508ef6e5ed5d684bdf5aa69


---

# Beispiel für HTML-Eigenschaften


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Das folgende Beispiel zeigt, wie Sie [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)-Eigenschaften in HTML zuweisen. **ApplicationId** und **AdUnitId** sind erforderliche Eigenschaften. Die anderen Eigenschaften und Ereignisse sind optional. Wenn Sie keine Werte für optionale Eigenschaften angeben, verwendet das Steuerelement Standardwerte, die eine Anzeige erstellen, die mit der Benutzeroberfläche der App konsistent ist.

Die letzten fünf Zeilen sind ein Beispiel für die Registrierung von Funktionen für **AdControl**-Ereignisse. Sie können nur Funktionen registrieren, die Sie in Ihrem JavaScript-Code definiert haben.

Diese Werte dienen als Beispiele. In Ihrem Code legen Sie die Werte dieser Funktionen und Eigenschaften entsprechend Ihrer App fest.

``` syntax
data-win-control="MicrosoftNSJS.Advertising.AdControl"
data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1',
                    adUnitId: '10865270',
                    isAutoRefreshEnabled: false,
                    onAdRefreshed: myAdRefreshed,
                    onErrorOccurred: myAdError,
                    onEngagedChanged: myAdEngagedChanged,
                    onPointerDown: myPointerDown,
                    onPointerUp: myPointerUp }"
```

## Verwandte Themen

* [Anzeigenbeispiele auf GitHub](http://aka.ms/githubads)

 



<!--HONumber=Jun16_HO4-->


