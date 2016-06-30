---
author: mcleanbyron
ms.assetid: 2383296e-c3d7-4b49-bcd2-621391228fdb
description: Hier erfahren Sie, wie Sie mit den Ereignissen der AdControl-Klasse umgehen.
title: AdControl-Ereignisse in JavaScript
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 5a47915b0dd2792ed50cc5d556b1181ee2c259e1


---

# AdControl-Ereignisse in JavaScript


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Die folgenden Beispiele veranschaulichen, wie die Ereignisse der [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)-Klasse behandelt werden. In diesen Beispielen wird davon ausgegangen, dass Sie die Ereignishandler zuvor den **AdControl**-Ereignissen zugewiesen haben. Weitere Informationen hierzu finden Sie unter [Beispiel für HTML-Eigenschaften](html-properties-example.md).

In JavaScript müssen die **AdControl**-Ereignisse von der [MarkSupportedForProcessing](http://msdn.microsoft.com/en-us/library/windows/apps/Hh967819.aspx)-Funktion eingeschlossen werden. Weitere Informationen zum Behandeln von Ereignissen in JavaScript finden Sie unter [Codieren einfacher Apps (HTML)](https://msdn.microsoft.com/en-us/library/windows/apps/hh780660.aspx#adding-event-handlers).

## Beispiele

``` syntax
WinJS.Utilities.markSupportedForProcessing(
window.myAdError = function (sender, msg) {
  // place code here for when there is an error serving an ad.
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes.
});

WinJS.Utilities.markSupportedForProcessing(
window.myAdRefreshed = function (sender) {
  // place code here that you wish to execute when the ad refreshes.
});

WinJS.Utilities.markSupportedForProcessing(
window.myAdEngagedChanged = function (sender) {
  if (true == sender.isEngaged) {
    // code here for when user engaged with ad, e.g. if a game, pause it.
  }
  else {
    // user no longer engaged with ad, include code to unpause.
  }
});
```

## Verwandte Themen

* [Anzeigenbeispiele bei GitHub](http://aka.ms/githubads)
* [AdControl-Fehlerbehandlung](adcontrol-error-handling.md)
* [RoutedEventArgs-Klasse](http://msdn.microsoft.com/en-us/library/system.windows.routedeventargs.aspx)

 

 



<!--HONumber=Jun16_HO4-->


