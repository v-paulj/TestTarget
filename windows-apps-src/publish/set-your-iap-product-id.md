---
author: jnHs
Description: Der erste Schritt beim Übermitteln eines IAPs (In-App-Produkts) wird im Windows Dev Center-Dashboard ausgeführt.
title: Festlegen der IAP-Produkt-ID
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
---

# Festlegen der IAP-Produkt-ID


Der erste Schritt beim Übermitteln eines IAPs (In-App-Produkts) wird im Windows Dev Center-Dashboard ausgeführt. Dies ist derselbe Bezeichner, den Sie für den Verweis im [App-Code zum Aufrufen des IAPs](https://msdn.microsoft.com/library/windows/apps/mt219684) benötigen.

Ein IAP muss einer App zugeordnet sein, die bereits im Dashboard erstellt (aber noch nicht unbedingt eingereicht) wurde. Sie finden die Schaltfläche zum **Erstellen eines neuen IAP** auf der Seite **Übersicht** oder **IAPs** der App.

Nachdem Sie auf die Schaltfläche geklickt haben, wird die Seite **IAP erstellen** angezeigt. Hier geben Sie die eindeutige Produkt-ID ein, die Sie verwenden, um auf das Angebot in Ihrem Code zu verweisen.

Folgende Dinge sollten bei der Wahl einer Produkt-ID beachtet werden:

-   Die Produkt-ID ist für Kunden nicht sichtbar. (Sie können später einen [Titel und eine Beschreibung](create-iap-descriptions.md) eingeben, die für Kunden sichtbar sind.)
-   Sie können die Produkt-ID eines IAPs nach der Veröffentlichung nicht ändern oder löschen.
-   Eine Produkt-ID darf maximal 100 Zeichen umfassen.
-   Folgende Zeichen dürfen nicht in der Produkt-ID enthalten sein: **&lt;&gt; \* % & : \\ ? + ,**
-   Um das IAP auf allen Geräten anzubieten, dürfen nur alphanumerische Zeichen, Punkte und/oder Unterstriche verwendet werden. Bei Verwendung anderer Zeichen kann das IAP von Kunden mit Windows Phone 8.1 oder früheren Versionen nicht erworben werden.
-   Eine Produkt-ID muss zwar nicht im Windows Store, aber für Ihr Entwicklerkonto eindeutig sein.

 

 






<!--HONumber=May16_HO2-->


