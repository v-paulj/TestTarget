---
author: jnHs
Description: "Auf der Seite App-Eigenschaften des App-Übermittlungsprozesses können Sie die Kategorie Ihrer App festlegen sowie Hardwareeinstellungen und weitere Deklarationen angeben."
title: Eingeben von App-Eigenschaften
ms.assetid: CDE4AF96-95A0-4635-9D07-A27B810CAE26
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 8861c13478adbe2010a164126c56f555375e0472

---

# Eingeben von App-Eigenschaften

Auf der Seite **App-Eigenschaften** des [App-Übermittlungsprozesses](app-submissions.md) können Sie die Kategorie Ihrer App festlegen sowie Hardwareeinstellungen und weitere Deklarationen angeben. Wir stellen Ihnen hier die auf dieser Seite verfügbaren Optionen vor und informieren Sie darüber, was Sie bei der Eingabe dieser Informationen beachten sollten.

> **Hinweis**  Altersfreigaben erhalten jetzt eine separate Seite im Übermittlungsprozess. Weitere Informationen finden Sie unter [Altersfreigaben](age-ratings.md).

## Kategorie und Unterkategorie

In diesem Abschnitt geben Sie die Kategorie (und ggf. eine Unterkategorie) an, die im Store zur Kategorisierung der App verwendet werden soll. Die Angabe einer Kategorie ist für die Einreichung Ihrer App erforderlich.

Weitere Informationen finden Sie unter [Kategorie- und Unterkategorietabelle](category-and-subcategory-table.md).

## Hardware-Einstellungen


In diesem Abschnitt können Sie angeben, ob bestimmte Hardwarefeatures erforderlich sind, um Ihre App ordnungsgemäß auszuführen und mit ihr zu interagieren.

Ist dies der Fall, wird im Windows Store versucht festzustellen, ob das Gerät eines Kunden das ausgewählte Hardwarefeature unterstützt. Wenn festgestellt wird, dass dies nicht der Fall ist, wird dem Kunden eine Warnung angezeigt, dass er eine App herunterladen möchte, für die eine bevorzugte Hardware deklariert ist. Kunden mit Windows 10-Geräten werden darüber hinaus die ausgewählten Features im Abschnitt **Hardwareanforderungen** des Store-Eintrags für Ihre App angezeigt.

Dies hindert Kunden nicht daran, Ihre App auf Geräte ohne geeignete Hardware herunterzuladen. Allerdings können sie Ihre App auf diesen Geräten nicht bewerten oder eine Rezension für diese verfassen.

> **Wichtig**  Mit Ausnahme von **Touchscreen** werden diese Warnungen nur Kunden mit Windows 10-Geräten angezeigt, die die ausgewählten Features nicht besitzen.

Zusätzlich zu der hier zutreffenden Auswahl wird empfohlen, in die App Laufzeitprüfungen auf die angegebene Hardware aufzunehmen, da im Store nicht immer erkannt werden kann, dass ein Kundengerät das ausgewählte Feature nicht bietet, und Kunden die App trotz der Warnung herunterladen können.

> **Tipp**  Wenn Sie verhindern möchten, dass Ihre UWP-App auf ein Gerät heruntergeladen wird, das die Mindestanforderungen für die Arbeitsspeicherkapazität oder DirectX-Ebene nicht erfüllt, können Sie die Mindestanforderungen in der Datei „StoreManifest.xml“ festlegen. Weitere Informationen finden Sie unter [StoreManifest-Schema (Windows 10)](https://msdn.microsoft.com/library/windows/apps/mt617335).

## App-Deklarationen


Über die Kontrollkästchen in diesem Abschnitt geben Sie an, ob Deklarationen auf Ihre App zutreffen. Dies kann sich darauf auswirken, wie Ihre App angezeigt wird, ob sie bestimmten Kunden angeboten wird und wie sie von Kunden genutzt werden kann.

Weitere Informationen finden Sie unter [App-Deklarationen](app-declarations.md).



<!--HONumber=Jun16_HO4-->


