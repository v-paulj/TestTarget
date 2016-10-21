---
author: mcleanbyron
ms.assetid: 32572890-26E3-4FBB-985B-47D61FF7F387
description: "Erfahren Sie, wie Sie In-App-Käufe und Testversionen in UWP-Apps aktivieren, die für Versionen vor Windows10, Version 1607 bestimmt sind."
title: "In-App-Käufe und Testversionen, die den Windows.ApplicationModel.Store-Namespace verwenden"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 649d082cddcf301fe602a5ab99637ad7bea67d49

---

# In-App-Käufe und Testversionen, die den Windows.ApplicationModel.Store-Namespace verwenden

Der Windows SDK stellt Mitglieder im [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)-Namespace bereit, den Sie verwenden können, um Ihrer App für die universelle Windows-Plattform (UWP) In-App-Käufe und Testfunktionalität hinzuzufügen, um die Monetarisierung Ihrer App zu unterstützen und Ihrer App neue Funktionalität hinzuzufügen. Diese APIs bieten auch Zugriff auf die Lizenzinformationen für Ihre App.

>**Hinweis**&nbsp;&nbsp;Wenn Ihre App auf Windows10, Version1607 oder höher ausgerichtet ist, wird empfohlen, den [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)-Namespace statt des **Windows.ApplicationModel.Store**-Namespace zu verwenden. Der **Windows.Services.Store**-Namespace unterstützt die neuesten Add-On-Typen, wie Store-verwaltete konsumierbare-Add-Ons, und ist so gestaltet, dass er mit zukünftigen Arten von Produkten und Features kompatibel ist, die von Windows Dev Center und dem Store unterstützt werden. Der **Windows.Services.Store**-Namespace wurde darüber hinaus für eine bessere Leistung entworfen. Weitere Informationen finden Sie unter [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md).

Die Artikel in diesem Abschnitt enthalten ausführliche Anleitungen und Codebeispiele für die Verwendung von Mitgliedern im **Windows.ApplicationModel.Store**-Namespace für verschiedene gängige Szenarien. Eine Übersicht über die Konzepte im Zusammenhang mit In-App-Käufen in UWP-Apps finden Sie unter [In app-Käufe und Testversionen](in-app-purchases-and-trials.md).

Ein vollständiges Beispiel, das zeigt, wie Sie Testversionen und In-App-Käufe mithilfe des **Windows.ApplicationModel.Store**-Namespace implementieren, finden Sie im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store).

## Inhalt dieses Abschnitts


| Thema                                                                                                       | Beschreibung                 |
|-------------------------------------------------------------------------------------------------------------|-----------------------------|
| [Unterstützen von Käufen konsumierbarer In-App-Produkte](enable-in-app-product-purchases.md)      |  Sie können unabhängig davon, ob Ihre App kostenlos oder kostenpflichtig ist, Inhalte, andere Apps oder neue App-Funktionen (wie das Freischalten des nächsten Levels eines Spiels) direkt in der App verkaufen. Hier zeigen wir Ihnen, wie Sie diese Produkte in Ihrer App aktivieren können.  |
| [Unterstützen von Käufen konsumierbarer In-App-Produkte](enable-consumable-in-app-product-purchases.md)      | Sie können In-App-Käufe von konsumierbaren Produkten – Artikel, die gekauft, verwendet und wieder gekauft werden können – über die Store-Handelsplattform anbieten, um Kunden beim Kauf Stabilität und Zuverlässigkeit zu bieten. Dies ist besonders nützlich für Dinge wie spielinterne Währungen (Gold, Münzen usw.), die gekauft und dann für den Erwerb bestimmter Power-Ups verwendet werden können. |
| [Ausschließen oder Einschränken von Features in einer Testversion](exclude-or-limit-features-in-a-trial-version-of-your-app.md) | Wenn Sie Kunden ermöglichen, Ihre App während eines Testzeitraums kostenlos zu verwenden, können Sie durch die Deaktivierung oder Einschränkung einiger Features während des Testzeitraums Ihre Kunden dazu bringen, ein Upgrade auf die Vollversion Ihrer App auszuführen. |
| [Verwalten eines großen Katalogs mit In-App-Produkten](manage-a-large-catalog-of-in-app-products.md)      |   Wenn Ihre App einen großen In-App-Produktkatalog enthält, können Sie optional das in diesem Thema beschriebene Verfahren zum Verwalten des Katalogs ausführen.    |
| [Überprüfen von Produktkäufen anhand von Belegen](use-receipts-to-verify-product-purchases.md)      |   Jede Windows Store-Transaktion, die zu einem erfolgreichen Produktkauf führt, kann optional einen Transaktionsbeleg zurückgeben, der dem Kunden Informationen zum aufgelisteten Produkt und zu den Kosten bereitstellt. Der Zugriff auf diese Informationen unterstützt Szenarien, in denen Ihre App überprüfen muss, ob ein Benutzer Ihre App erworben oder In-App-Produktkäufe im WindowsStore getätigt hat. |



<!--HONumber=Aug16_HO5-->


