---
author: mcleanbyron
ms.assetid: 2fba38c4-11be-4058-bfa3-5f979390791c
description: Hier erfahren Sie, wie Sie mit den Ereignissen der AdControl-Klasse umgehen.
title: AdControl-Ereignisse in C#
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: 969d668c89b40e37245a8168879842159b4f5c14

---

# AdControl-Ereignisse in C\# #  




Die folgenden Beispiele veranschaulichen, wie die Ereignisse der [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)-Klasse behandelt werden. In diesen Beispielen wird davon ausgegangen, dass Sie die Ereignishandler zuvor den **AdControl**-Ereignissen in XAML zugewiesen haben. Weitere Informationen hierzu finden Sie unter [Beispiel für XAML-Eigenschaften](xaml-properties-example.md).

Weitere Informationen zum Behandeln von Ereignissen in C# finden Sie unter [Übersicht über Ereignisse und Routingereignisse (Universelle Windows-Apps mit C#/VB/C++ und XAML)](http://msdn.microsoft.com/library/windows/apps/hh758286).

## Beispiele


``` syntax
private void OnAdError(object sender, AdErrorEventArgs e) {
  // place code here for when there is an error serving an ad
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes
  return;
}

private void OnAdRefresh(object sender, RoutedEventArgs e) {
  // place code here that you wish to execute when the ad refreshes.
 return;
}

private void OnAdEngagedChanged(object sender, RoutedEventArgs e) {
  // place code here for when there is an error serving an ad
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes
  return;
}
```

## Verwandte Themen

* [Anzeigenbeispiele bei GitHub](http://aka.ms/githubads)
* [AdControl-Fehlerbehandlung](adcontrol-error-handling.md)
* [RoutedEventArgs-Klasse](http://msdn.microsoft.com/library/system.windows.routedeventargs.aspx)

 

 



<!--HONumber=Aug16_HO3-->


