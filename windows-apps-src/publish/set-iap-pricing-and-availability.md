---
author: jnHs
Description: "Beim Übermitteln eines IAPs bestimmen die Optionen auf der Seite Preise und Verfügbarkeit, zu welchem Preis und wie das IAP Kunden angeboten werden soll."
title: "Festlegen der Preise und Verfügbarkeit von IAPs"
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.sourcegitcommit: 52816584a9afbd6c8e213a182bae18732f082aef
ms.openlocfilehash: 0e6c58f2d892f213d2de53c3cb9b97b1e8152137

---

# Festlegen der Preise und Verfügbarkeit von IAPs


Beim Übermitteln eines IAPs bestimmen die Optionen auf der Seite **Preise und Verfügbarkeit**, zu welchem Preis und wie das IAP Kunden angeboten werden soll.

## Grundpreis


Sie müssen ein Grundpreis für Ihr IAP auswählen. Diese Preisniveaus sind identisch mit denen für Apps und beginnen bei 0,99 USD. Sie können Ihr IAP auch kostenlos zur Verfügung stellen.

## Märkte und angepasste Preise


Das IAP wird standardmäßig in allen möglichen Märkten, einschließlich zukünftigen Märkten, die später hinzukommen, zum Grundpreis eingetragen.

Allerdings können Sie genauso wie bei einer App bestimmen, in welchen Märkten Ihr IAP angeboten werden soll. In den meisten Fällen werden Sie dieselben Märkte wie für die App auswählen, haben aber die Flexibilität, nach Bedarf Änderungen vorzunehmen. Sie können auch angepasste Preise festlegen, um in verschiedenen Märkten unterschiedliche Preise für das IAP zu verlangen.

Weitere Informationen und eine vollständige Liste der verfügbaren Märkte finden Sie unter [Festlegen des Preises und Auswählen der Märkte](define-pricing-and-market-selection.md).

## Sonderangebotsverkaufspreise


Wenn Sie Ihr IAP zu einem reduzierten Preis für einen begrenzten Zeitraum anbieten möchten, können Sie ein Sonderangebot erstellen und planen. Weitere Informationen finden Sie unter [Anbieten von Apps und IAPs](put-apps-and-iaps-on-sale.md).

## Verteilung und Sichtbarkeit


Sie können festlegen, ob Ihr IAP Kunden zum Kauf angeboten werden soll. Wählen Sie eine der folgenden Optionen:

-   **Zum Kauf erhältlich. Wird u. U. in Ihrer App-Liste angezeigt:** Dies ist die Standardeinstellung. Sie wird empfohlen, sofern der Zugriff auf das IAP nicht eingeschränkt werden soll. Lassen Sie diese Option für IAPs aktiviert, die für alle Kunden verfügbar gemacht werden.
-   **Zum Kauf erhältlich. Wird nicht in Ihrer App-Liste angezeigt:** Bei Auswahl dieser Option können Kunden das IAP innerhalb Ihrer App kaufen, es wird aber im Store-Eintrag Ihrer App nicht aufgeführt. Verwenden Sie diese Option nur, wenn das Angebot nicht allgemein verfügbar ist, z. B. im Anfangszeitraum interner Tests.
-   **Nicht mehr zum Kauf erhältlich. Wird nicht im App-Eintrag angezeigt.** Diese Option bedeutet, dass das IAP nicht im App-Eintrag angezeigt wird und nicht von neuen Kunden erworben werden kann. Allerdings wird **diese Option für Kunden mit Windows 8.1 oder einer früheren Version nicht unterstützt**. Wenn Ihre App für Windows 8.1 oder eine frühere Version verfügbar ist, kann das IAP von diesen Kunden erworben werden. Um das IAP-Angebot für Kunden mit Windows 8.1 oder einer früheren Version einzustellen, müssen Sie die App aktualisieren, indem Sie den Code entfernen, durch den das IAP angeboten wird, und eine neue Übermittlung für die App veröffentlichen. Dieser Schritt wird selbst dann empfohlen, wenn Ihre App keine Unterstützung für Windows 8.1 oder frühere Versionen bietet, da die Kundenerfahrung beeinträchtigt werden könnte, wenn Sie erst ein IAP anbieten, das Sie später zurückziehen.
    
 > **Hinweis:** Das Auswählen dieser Einstellung und/oder Übermitteln eines App-Updates, durch das das IAP aus dem Code der App entfernt wird, wirkt sich nicht auf Kunden aus, die das IAP bereits erworben haben, und zwar unabhängig von ihrem Betriebssystem.


## Veröffentlichungsdatum

Mithilfe der Optionen im Abschnitt **Veröffentlichungsdatum** können Sie angeben, wann das IAP (oder Update) veröffentlicht wird.

-   Wählen Sie **Mein IAP sofort nach erfolgreicher Zertifizierung veröffentlichen** aus, um die Übermittlung so schnell wie möglich im Store verfügbar zu machen.
-   Wählen Sie **Dieses IAP manuell veröffentlichen** aus, wenn Sie den Veröffentlichungszeitpunkt Ihrer Übermittlung selbst angeben möchten. Dazu können Sie auf der Seite mit dem Zertifizierungsstatus auf **Jetzt veröffentlichen** klicken oder ein bestimmtes Datum auswählen, wie unten beschrieben.
-   Wählen Sie **Nicht vor \[Datum\]**, um sicherzustellen, dass die Übermittlung nicht vor einem bestimmten Datum veröffentlicht wird. Mit dieser Option wird Ihre Übermittlung möglichst zum oder nach dem angegebenen Datum veröffentlicht. Das Datum muss mindestens 24 Stunden in der Zukunft liegen. Zusammen mit dem Datum können Sie auch die Uhrzeit angeben, zu der die Veröffentlichung der Übermittlung beginnen soll.

 > **Hinweis:** Aufgrund von Verzögerungen bei der Zertifizierung oder Veröffentlichung kann das gewünschte Veröffentlichungsdatum unter Umständen nicht eingehalten werden. Es kann nicht garantiert werden, dass Ihr IAP (oder Update) an einem bestimmten Datum im Windows Store zur Verfügung steht.
 

 







<!--HONumber=Jun16_HO5-->


