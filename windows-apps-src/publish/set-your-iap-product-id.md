---
author: jnHs
Description: "Beim Erstellen eines neuen IAP (In-App-Produkt) im Windows Dev Center-Dashboard müssen Sie einen Produkttyp angeben eine Produkt-ID zuweisen."
title: "Festlegen von Produkttyp und Produkt-ID für das IAP"
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
ms.sourcegitcommit: ae4727974af632a275c102a6328734597cee3e9b
ms.openlocfilehash: 9faee009cd907cd8ccdeded019e23713cbc058f0

---

# Festlegen von Produkttyp und Produkt-ID für das IAP

Ein IAP muss einer App zugeordnet sein, die bereits im Dashboard erstellt (aber noch nicht unbedingt übermittelt) wurde. Die Schaltfläche zum **Erstellen eines neuen IAP** finden Sie auf der App-Seite **Übersicht** oder **IAPs**.

Nachdem Sie auf die Schaltfläche geklickt haben, wird die Seite **Neues IAP erstellen** angezeigt. Hier müssen Sie einen Produkttyp angeben und eine Produkt-ID zuweisen.

## Produkttyp

Zunächst müssen Sie angeben, welche Art von IAP Sie anbieten. Diese Auswahloption bezieht sich darauf, wie das IAP vom Kunden genutzt werden kann.

> **Hinweis:** Nachdem Sie diese Seite gespeichert haben, um das IAP zu erstellen, kann der Produkttyp nicht mehr geändert werden. Sollten Sie den falschen Produkttyp ausgewählt haben, können Sie die in Bearbeitung befindliche IAP-Übermittlung jederzeit löschen und ein neues IAP erstellen.

Wählen Sie den geeigneten Produkttyp für Ihr IAP aus:

- Verbrauchsartikel: Ein Produkt, das erworben, verwendet (verbraucht) und anschließend erneut gekauft werden kann. Konsumierbare IAPs werden häufig für Dinge wie spielinterne Währungen (Gold, Münzen usw.) verwendet, die in bestimmten Mengen erworben und vom Kunden aufgebraucht werden.
- Gebrauchsgut: Ein Produkt, das vom Käufer erworben wird und sich eine bestimmte Zeit in seinem Besitz befindet. Dauerhafte IAPs werden häufig verwendet, um zusätzliche Funktionen in einer App freizuschalten. Dauerhafte IAPs werden nicht aufgebraucht, Sie können jedoch eine **Produktlebenszeit** festlegen, sodass die IAPs nach einer festgelegten Zeitspanne (von 1 bis 365 Tagen) ablaufen. Die standardmäßige **Produktlebenszeit** dauerhafter IAPs ist **Unbegrenzt**. Das IAP läuft also niemals ab. Diese Einstellung kann im Schritt [IAP-Eigenschaften](enter-iap-properties.md) der IAP-Übermittlung geändert werden.

## Produkt-ID

Geben Sie eine eindeutige Produkt-ID für Ihr IAP ein. Dies ist der gleiche Bezeichner, den Sie für den Verweis im [App-Code zum Aufrufen des IAPs](https://msdn.microsoft.com/library/windows/apps/mt219684) benötigen.

Folgende Dinge sollten bei der Wahl einer Produkt-ID beachtet werden:

-   Die Produkt-ID ist für Kunden nicht sichtbar. (Sie können später einen [Titel und eine Beschreibung](create-iap-descriptions.md) eingeben, die für Kunden sichtbar sind.)
-   Sie können die Produkt-ID eines IAPs nach der Veröffentlichung nicht ändern oder löschen.
-   Eine Produkt-ID darf maximal 100 Zeichen umfassen.
-   Folgende Zeichen dürfen nicht in der Produkt-ID enthalten sein: **&lt;&gt; \* % & : \\ ? + ,**
-   Um das IAP auf allen Geräten anzubieten, dürfen nur alphanumerische Zeichen, Punkte und/oder Unterstriche verwendet werden. Bei Verwendung anderer Zeichen kann das IAP von Kunden mit Windows Phone 8.1 oder früheren Versionen nicht erworben werden.
-   Eine Produkt-ID muss zwar nicht im Windows Store, aber für Ihr Entwicklerkonto eindeutig sein.
 







<!--HONumber=Jun16_HO4-->


