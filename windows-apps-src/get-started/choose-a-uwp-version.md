---
author: QuinnRadich
title: "Auswählen einer UWP-Version"
description: "Beim Schreiben einer UWP-App in Microsoft Visual Studio können Sie wählen, für welche Version die App bestimmt ist. Sie erhalten Informationen zum Unterschied zwischen unterschiedlichen UWP-Versionen und zur Konfiguration Ihrer Auswahl in neuen und vorhandenen Projekten."
redirect_url: ../updates-and-versions/choose-a-uwp-version/
translationtype: Human Translation
ms.sourcegitcommit: a86002c944841536d37735bb8c4b657905582144
ms.openlocfilehash: d6d2be6c91ddf5fb85cdec759c753db1561f066f

---

# Auswählen einer UWP-Version

**Diese Seite wurde nach „../updates-and-versions/choose-a-uwp-version/“ verschoben.**

Beim Schreiben einer UWP-App in Microsoft Visual Studio können Sie wählen, für welche Version die App bestimmt ist. Sie erhalten Informationen zum Unterschied zwischen unterschiedlichen UWP-Versionen und zur Konfiguration Ihrer Auswahl in neuen und vorhandenen Projekten.

## Wodurch unterscheiden sich die einzelnen UWP-Versionen?

Neue und geänderte APIs für UWP sind in jeder neuen Version von Windows10 verfügbar. Ausführlichere Informationen dazu, welche Funktionen in welcher Version hinzugefügt wurden, finden Sie unter [Neuigkeiten für Entwickler in Windows10](../whats-new/windows-10-version-1607.md).

Referenzthemen, in denen alle Gerätefamilien mit ihren Versionen und alle API-Verträge mit ihren Versionen aufgeführt sind, finden Sie unter [Gerätefamilien](https://msdn.microsoft.com/library/windows/apps/dn706137.aspx) und [API-Verträge](https://msdn.microsoft.com/library/windows/apps/dn706135.aspx).

## Auswählen der Version für die App

In Visual Studio können Sie im Dialogfeld **Neues universelles Windows-Projekt** eine Version für **Zielversion** und **Mindestens erforderliche Version** auswählen.

* **Zielversion**: Legt die Einstellung *TargetPlatformVersion* in Ihrer Projektdatei fest. Außerdem wird der Wert des Attributs *TargetDeviceFamily@MaxVersionTested* im App-Paketmanifest bestimmt. Mit dem von Ihnen gewählten Wert wird die Version der UWP-Plattform angegeben, für die Ihr Projekt bestimmt ist, und somit auch der in der App verfügbare API-Satz. Wir empfehlen Ihnen also, möglichst die aktuelle Version zu wählen. Weitere Informationen zum App-Paketmanifest und einige Richtlinien zur manuellen Konfiguration von TargetDeviceFamily finden Sie unter [TargetDeviceFamily](https://msdn.microsoft.com/library/windows/apps/dn986903).
* **Mindestens erforderliche Version**: Legt die Einstellung *TargetPlatformMinVersion* in Ihrer Projektdatei fest. Außerdem wird der Wert des Attributs *TargetDeviceFamily@MinVersion* im App-Paketmanifest bestimmt. Mit dem von Ihnen ausgewählten Wert wird die Mindestversion der UWP-Plattform angegeben, die von Ihrem Projekt verwendet werden kann.

Seien Sie sich hierbei bewusst, dass Sie Folgendes deklarieren: Ihre App funktioniert mit jeder Windows-Version, die zwischen **Mindestens erforderliche Version** und **Zielversion** liegt. Falls es sich um dieselbe Version handelt, müssen Sie nichts Besonderes unternehmen. Wenn es unterschiedliche Versionen sind, sollten Sie die hier angegebenen Hinweise beachten.

* Im Code können Sie frei (also ohne Bedingungsprüfungen) alle APIs aufrufen, die in der unter **Mindestens erforderliche Version** angegebenen API vorhanden sind.
* Der Wert von **Zielversion** wird zum Identifizieren aller Verweise (Vertrags-WinMds) genutzt, die zum Kompilieren des Projekts verwendet werden. Mit diesen Verweisen können Sie Ihren Code aber mit Aufrufen von APIs kompilieren, die nicht unbedingt auf Geräten vorhanden sind, für die Sie die Unterstützung deklariert haben (mit **Mindestens erforderliche Version**). Daher müssen alle APIs, die nach **Mindestens erforderliche Version** eingeführt wurden, mit adaptivem Code aufgerufen werden. Weitere Informationen zu adaptivem Code finden Sie unter [Anleitung für Apps für die Universelle Windows-Plattform (UWP)](universal-application-platform-guide.md).


<!--HONumber=Aug16_HO5-->


