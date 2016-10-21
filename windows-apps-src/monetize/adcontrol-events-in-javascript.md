---
author: mcleanbyron
ms.assetid: 2383296e-c3d7-4b49-bcd2-621391228fdb
description: Hier erfahren Sie, wie Sie mit den Ereignissen der AdControl-Klasse umgehen.
title: AdControl-Ereignisse in JavaScript
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: d24030dfae92451924000ba4f1ac19cf6c4d4abe


---

# AdControl-Ereignisse in JavaScript




Die folgenden Beispiele veranschaulichen, wie die Ereignisse der [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)-Klasse behandelt werden. In diesen Beispielen wird davon ausgegangen, dass Sie die Ereignishandler zuvor den **AdControl**-Ereignissen zugewiesen haben. Weitere Informationen hierzu finden Sie unter [Beispiel für HTML-Eigenschaften](html-properties-example.md).

In JavaScript müssen die **AdControl**-Ereignisse von der [MarkSupportedForProcessing](http://msdn.microsoft.com/library/windows/apps/Hh967819.aspx)-Funktion eingeschlossen werden. Weitere Informationen zum Behandeln von Ereignissen in JavaScript finden Sie unter [Codieren einfacher Apps (HTML)](https://msdn.microsoft.com/library/windows/apps/hh780660.aspx#adding-event-handlers).

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
* [RoutedEventArgs-Klasse](http://msdn.microsoft.com/library/system.windows.routedeventargs.aspx)

 

 



<!--HONumber=Aug16_HO3-->


