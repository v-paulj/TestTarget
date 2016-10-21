---
author: jnHs
Description: "Beim Erstellen eines neuen Add-Ons im WindowsDevCenter-Dashboard müssen Sie einen Produkttyp angeben und eine Produkt-ID zuweisen."
title: "Festlegen von Produkt-ID und Produkttyp für das Add-On"
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
translationtype: Human Translation
ms.sourcegitcommit: e59324aca65cf8baacb085da22a20d952fdb8c9a
ms.openlocfilehash: 2a469506c8b440e1aa8555ac57b88f2026ae4d8e

---

# Festlegen von Produkt-ID und Produkttyp für das Add-On

Ein Add-On muss einer App zugeordnet sein, die bereits im Dashboard erstellt (aber noch nicht unbedingt übermittelt) wurde. Die Schaltfläche zum **Erstellen eines neuen Add-Ons** finden Sie auf der App-Seite **Übersicht** oder **Add-Ons**.

Nachdem Sie auf die Schaltfläche geklickt haben, wird die Seite **Neues Add-On erstellen** angezeigt. Hier müssen Sie einen Produkttyp angeben und eine Produkt-ID zuweisen.

## Produkttyp

Zunächst müssen Sie angeben, welche Art von Add-On Sie anbieten. Diese Auswahloption bezieht sich darauf, wie das Add-On vom Kunden genutzt werden kann.

> **Hinweis:** Nachdem Sie diese Seite gespeichert haben, um das Add-On zu erstellen, kann der Produkttyp nicht mehr geändert werden. Sollten Sie den falschen Produkttyp ausgewählt haben, können Sie die in Bearbeitung befindliche Add-On-Übermittlung jederzeit löschen und ein neues Add-On erstellen.

Wenn das Produkt erworben werden kann, verwendet (verbraucht) und dann erneut gekauft wird, sollten Sie einen der **konsumierbaren** Produkttypen wählen. Konsumierbare Add-Ons bzw. Endverbraucher-Add-Ons werden häufig für Dinge wie spielinterne Währungen (Gold, Münzen usw.) verwendet, die in bestimmten Mengen erworben und vom Kunden aufgebraucht werden. Weitere Informationen zum Hinzufügen von konsumierbaren Add-Ons in Ihrer App finden Sie unter [Unterstützen von Endverbraucher-Add-On-Käufen](../monetize/enable-consumable-add-on-purchases.md).

Es gibt zwei Arten von konsumierbaren Add-Ons, die Sie auswählen können:

- **Von Entwicklern verwaltetes Endverbraucher-Add-On**: Unterstützt unter allen Betriebssystemversionen. Saldo und Erfüllung müssen in der App verwaltet werden. 
- **Vom Store verwaltetes Endverbraucher-Add-On:** Der Saldo wird von Microsoft für alle Geräte des Kunden verfolgt, auf denen Windows10 (Version 1607 oder höher) ausgeführt wird; nicht unterstützt unter früheren Betriebssystemversionen. Um diese Option zu verwenden, muss das übergeordnete Produkt mit Windows10 SDK Version14393 oder höher kompiliert werden. Beachten Sie außerdem, dass Sie erst dann vom Store verwaltete Endverbraucher-Add-Ons zum Store übermitteln können, wenn das übergeordnete Produkt veröffentlicht wurde (es ist jedoch möglich, jederzeit die Übermittlung in Ihrem Dashboard zu erstellen und damit bereits zu arbeiten). Sie müssen die Menge für Ihr vom Store verwaltetes Endverbraucher-Add-on auf der **Eigenschaften**-Seite eingeben.

Sie müssen **Dauerhaft** auswählen, wenn das Produkt nur einmal erworben werden kann. Dauerhafte Add-Ons werden häufig verwendet, um zusätzliche Funktionen in einer App freizuschalten. Dauerhafte Add-Ons werden nicht aufgebraucht, Sie können jedoch eine **Produktlebenszeit** festlegen, sodass sie nach einer festgelegten Zeitspanne (von 1 bis 365Tagen) ablaufen. Die standardmäßige **Produktlebenszeit** dauerhafter Add-Ons ist **Unbegrenzt**. Das Add-On läuft also niemals ab. Diese Einstellung kann im Schritt [Add-On-Eigenschaften](enter-add-on-properties.md) der Add-On-Übermittlung geändert werden.

## Produkt-ID

Geben Sie eine eindeutige Produkt-ID für das Add-On ein. Dies ist der gleiche Bezeichner, den Sie für den Verweis im [App-Code zum Aufrufen des Add-Ons](https://msdn.microsoft.com/library/windows/apps/mt219684) benötigen.

Folgende Dinge sollten bei der Wahl einer Produkt-ID beachtet werden:

-   Die Produkt-ID ist für Kunden nicht sichtbar. (Sie können später [einen Titel und eine Beschreibung](create-add-on-descriptions.md) eingeben, die für Kunden sichtbar sind.)
-   Sie können die Produkt-ID eines Add-Ons nach der Veröffentlichung nicht ändern oder löschen.
-   Eine Produkt-ID darf maximal 100 Zeichen umfassen.
-   Folgende Zeichen dürfen nicht in der Produkt-ID enthalten sein: **&lt; &gt; \* % & : \\ ? + ,**
-   Um das Add-On auf allen Geräten anzubieten, dürfen nur alphanumerische Zeichen, Punkte und/oder Unterstriche verwendet werden. Bei Verwendung anderer Zeichen kann das Add-On von Kunden mit Windows Phone8.1 oder früheren Versionen nicht erworben werden.
-   Eine Produkt-ID muss zwar nicht im Windows Store, aber für Ihr Entwicklerkonto eindeutig sein.
 







<!--HONumber=Aug16_HO5-->


