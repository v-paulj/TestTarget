---
author: mcleanbyron
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: "Erfahren Sie, wie Sie In-App-Einkäufe und Testversionen in UWP-Apps aktivieren."
title: "In-App-Einkäufe und Testversionen"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 99143d48a5f2155b0a47008574d0a78243dea925

---

# In-App-Einkäufe und Testversionen

Mit dem Windows SDK werden APIs bereitgestellt, mit denen Sie In-App-Einkäufe und Testfunktionen der Universal Windows Platform (UWP)-App hinzufügen können, um die App gewinnbringend zu nutzen und neue Funktionen hinzufügen. Diese APIs bieten auch Zugriff auf die Lizenzinformationen für Ihre App.

Für diese Szenarien bietet Windows10 zwei unterschiedliche APIs:

* Alle Versionen von Windows10 unterstützen eine API für In-App-Einkäufe und Lizenzinformationen im [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)-Namespace.

* Ab Windows10, Version1607 gibt es eine alternative API für In-App-Einkäufe und Lizenzinformationen im [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)-Namespace.  

Obwohl die APIs in diesen Namespaces den gleichen Ziele dienen, sind sie unterschiedlich gestaltet, und der Code ist zwischen den beiden APIs nicht kompatibel. Wenn Ihre App auf Windows10, Version1607 oder höher ausgerichtet ist, wird empfohlen, den **Windows.Services.Store**-Namespace zu verwenden. Dieser Namespace unterstützt die neuesten Add-On-Typen wie vom Store verwaltete Endverbraucher-Add-Ons und ist so gestaltet, dass er mit von Windows Dev Center und dem Store unterstützten zukünftigen Arten von Produkten und Features kompatibel ist. Der **Windows.Services.Store**-Namespace verfügt auch über eine höhere Leistung.

Dieser Artikel bietet eine Einführung in In-App-Einkäufe für UWP-Apps und enthält eine Übersicht über den **Windows.Services.Store**-Namespace, der ab Windows10, Version1607 zur Verfügung steht. Informationen zur Verwendung der Member im **Windows.ApplicationModel.Store**-Namespace finden Sie unter [In-App-Einkäufe und Testversionen mit dem Windows.ApplicationModel.Store-Namespace](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).


## Übersicht über In-App-Einkäufe in UWP-Apps

In diesem Abschnitt werden die Kernkonzepte zur Funktionsweise von In-App-Einkäufen und Testversionen mit UWP-Apps im Store beschrieben. Die meisten dieser Konzepte gelten sowohl für den **Windows.Services.Store**-Namespace als auch den **Windows.ApplicationModel.Store**-Namespace.

Jedes im Store angebotene Element wird im Allgemeinen als *Produkt* bezeichnet. Die meisten Entwickler arbeiten mit den folgenden Arten von Produkten: *Apps* und *Add-Ons* (auch bekannt als In-App-Produkte oder IAPs). Ein Add-On bezieht sich auf ein Produkt oder Feature, das Sie Ihren Kunden im Kontext Ihrer App zur Verfügung stellen. Ein Add-On kann alle Funktionen darstellen, die Ihre App Kunden bietet: z.B. in einer App oder einem Spiel zu verwendende Währung, neue Karten oder Waffen für ein Spiel, die Möglichkeit zur Verwendung der App ohne Werbung oder digitale Inhalte wie Musik oder Videos für Apps, die diese Art von Inhalten anbieten können.

Jede App und jedes Add-On verfügt über eine zugehörige Lizenz, die angibt, ob der Benutzer zur Verwendung dieser App oder des Add-Ons berechtigt ist. Wenn der Benutzer berechtigt ist, die App bzw. das Add-On als Testversion zu verwenden, enthält die Lizenz auch zusätzliche Informationen zur Testversion.

Um Kunden ein Add-On in Ihrer App anzubieten, beginnen Sie mit dem [Definieren der Add-Ons für Ihre App im Dev Center-Dashboard](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions). Schreiben Sie dann den Code in Ihrer App, um zu ermitteln, ob der Benutzer über eine Lizenz für die Verwendung des durch das Add-On dargestellten Features verfügt. Bieten Sie das Add-On zum Verkauf an den Benutzer als In-App-Einkauf an, wenn er noch keine Lizenz hat. Beispiele für verwandte Aufgaben mit dem **Windows.Services.Store**-Namespace in Apps, die auf Windows10, Version1607 oder höher ausgerichtet sind, finden Sie in den folgenden Artikeln:

* [Abrufen von Produktinformationen zu Apps und Add-Ons](get-product-info-for-apps-and-add-ons.md)
* [Abrufen von Lizenzinformationen zu Apps und Add-Ons](get-license-info-for-apps-and-add-ons.md)
* [Aktivieren von In-App-Einkäufen von Apps und Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Unterstützen von Endverbraucher-Add-On-Käufen](enable-consumable-add-on-purchases.md)
* [Implementieren einer Testversion der App](implement-a-trial-version-of-your-app.md)

Beispiele für verwandte Aufgaben mit dem **Windows.ApplicationModel.Store**-Namespace finden Sie unter [In-App-Einkäufe und Testversionen mit dem Windows.ApplicationModel.Store-Namespace](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

Alle Entwickler können die folgenden Arten von Add-Ons erstellen.

| Add-On-Typ |  Beschreibung  |
|---------|-------------------|
| Gebrauchsgut  |  Ein Add-On, das für die Lebensdauer beibehalten wird, die Sie im [Windows Dev Center-Dashboard](https://msdn.microsoft.com/windows/uwp/publish/enter-iap-properties) angeben. <p/><p/>Standardmäßig laufen Gebrauchsgut-Add-Ons nie ab, in diesem Fall können sie nur einmal gekauft werden. Wenn Sie eine bestimmte Dauer für das Add-On angeben, kann der Benutzer das Add-On erneut kaufen, nachdem es abgelaufen ist.  |
| Von Entwicklern verwaltetes Endverbraucher-Add-On  |  Ein Add-On, das gekauft, verwendet und erneut gekauft werden kann. Diese Art von Add-On wird in der Regel für In-App-Währung verwendet. <p/><p/>Bei dieser Art von Verbrauchsartikel sind Sie dafür verantwortlich, das Guthaben des Benutzers an Elementen zu verfolgen, die das Add-On darstellt, und den Kauf des Add-Ons dem Store als erfüllt zu melden, nachdem der Benutzer alle Elemente genutzt hat. Der Benutzer kann das Add-On erst dann erneut kaufen, nachdem Ihre App den vorherigen Kauf des Add-Ons als erfüllt gemeldet hat. <p/><p/>Wenn beispielsweise das Add-On 100 Münzen in einem Spiel darstellt und der Benutzer 10 Münzen nutzt, muss die App oder der Dienst den neuen Restbetrag von 90 Münzen für den Benutzer verwalten. Nachdem der Benutzer alle 100 Münzen genutzt hat, muss die App das Add-On als erfüllt melden, und danach kann der Benutzer das Add-On für 100 Münzen erneut kaufen.    |
| Vom Store verwaltetes Endverbraucher-Add-On  |  Ein Add-On, das gekauft, verwendet und erneut gekauft werden kann. Diese Art von Add-On wird in der Regel für In-App-Währung verwendet.<p/><p/>Bei dieser Art von Verbrauchsartikel verfolgt der Store das Guthaben des Benutzers an Elementen, die das Add-On darstellt. Wenn der Benutzer Elemente nutzt, sind Sie verantwortlich dafür, diese Elemente dem Store als erfüllt zu melden, und der Store aktualisiert das Guthaben des Benutzers. Ihre App kann das aktuelle Guthaben für den Benutzer jederzeit abfragen. Nachdem der Benutzer alle Elemente genutzt hat, kann er das Add-On erneut kaufen.  <p/><p/> Wenn das Add-On beispielsweise eine anfängliche Menge von 100 Münzen in einem Spiel darstellt und der Benutzer 10 Münzen nutzt, meldet die App dem Store, dass 10 Einheiten des Add-Ons erfüllt wurden, und der Store aktualisiert den Restbetrag. Nachdem der Benutzer alle 100 Münzen genutzt hat, kann er das Add-On für 100 Münzen erneut kaufen. <p/><p/> **Vom Store verwaltete Endverbraucher-Add-Ons sind ab Windows10, Version1607 verfügbar. Die Möglichkeit zum Erstellen eines vom Store verwalteten Endverbraucher-Add-Ons im Windows Dev Center-Dashboard ist in Kürze verfügbar.**  |

<span />

>**Hinweis**&nbsp;&nbsp;Andere Arten von Add-Ons wie Gebrauchsgut-Add-Ons mit Paketen (auch bekannt als Inhalte zum Herunterladen oder DLC) stehen nur einer eingeschränkten Gruppe von Entwicklern zur Verfügung und werden in dieser Dokumentation nicht behandelt.

<span id="api_intro" />
## Einführung in den Windows.Services.Store-Namespace

Der Haupteinstiegspunkt in den **Windows.Services.Store**-Namespace ist die [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Klasse. Diese Klasse stellt Methoden bereit, mit denen Sie Informationen für die aktuelle App und die verfügbaren Add-Ons abrufen, eine App oder ein Add-On für den aktuellen Benutzer kaufen, Lizenzinformationen für die aktuelle App oder die Add-Ons abrufen und weitere Aufgaben durchführen können. Gehen Sie zum Abrufen eines [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Objekts wie folgt vor:

* Rufen Sie in einer Einzelbenutzer-App (einer App, die nur im Kontext des Benutzers ausgeführt wird, der die App gestartet hat) mit der [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx)-Methode ein **StoreContext**-Objekt ab, mit dem Sie auf Windows Store-bezogene Daten für den Benutzer zugreifen und diese verwalten können. Die meisten Universal Windows Platform (UWP)-Apps sind Einzelbenutzer-Apps.

  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* Rufen Sie in einer Mehrbenutzer-App mit der [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx)-Methode ein **StoreContext**-Objekt ab, mit dem Sie auf Windows Store-bezogene Daten für einen bestimmten Benutzer, der beim Verwenden der App mit seinem Microsoft-Konto angemeldet ist, zugreifen und diese verwalten können. Weitere Informationen zu Mehrbenutzer-Apps finden Sie unter [Einführung in Anwendungen mit mehreren Benutzern](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications). Im folgenden Beispiel wird ein **StoreContext**-Objekt für den ersten verfügbaren Benutzer abgerufen.

  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

Nachdem Sie über einen [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) verfügen, können Sie mit dem Aufrufen von Methoden zum Kaufen einer App oder eines Add-Ons für den aktuellen Benutzer und für andere Aufgaben beginnen. Weitere Informationen finden Sie in den folgenden Artikeln:

* [Abrufen von Produktinformationen zu Apps und Add-Ons](get-product-info-for-apps-and-add-ons.md)
* [Abrufen von Lizenzinformationen zu Apps und Add-Ons](get-license-info-for-apps-and-add-ons.md)
* [Aktivieren von In-App-Einkäufen von Apps und Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Unterstützen von Endverbraucher-Add-On-Käufen](enable-consumable-add-on-purchases.md)
* [Implementieren einer Testversion der App](implement-a-trial-version-of-your-app.md)

Ein vollständiges Beispiel, das veranschaulicht, wie Sie Testversionen und In-App-Einkäufe mit dem **Windows.Services.Store**-Namespace implementieren, finden Sie im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

<span />
### Objektmodell für Produkte, SKUs und Verfügbarkeiten

Jedes Produkt im Store verfügt über mindestens eine *SKU*, und jede SKU verfügt über mindestens eine *Verfügbarkeit*. Diese Konzepte werden im Windows Dev Center-Dashboard für die meisten Entwickler herausgenommen, und die meisten Entwickler definieren nie SKUs oder Verfügbarkeiten für ihre Apps oder Add-Ons. Da jedoch das Objektmodell für Store-Produkte im **Windows.Services.Store**-Namespace SKUs und Verfügbarkeiten umfasst, können Grundkenntnisse zu diesen Konzepten hilfreich sein.

| Objekttyp |  Beschreibung  |
|---------|-------------------|
| Produkt  |  Die [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)-Klasse stellt alle im Store verfügbaren Produkttypen dar, einschließlich Apps und Add-Ons. Diese Klasse stellt Eigenschaften für den Zugriff auf Daten bereit, z.B. die Store-ID des Produkts, die Bilder und Videos für den Store-Eintrag sowie Preisinformationen. Sie stellt außerdem Methoden bereit, mit denen Sie das Produkt kaufen können. |
| SKU |  Die [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx)-Klasse stellt eine *SKU* für ein Produkt dar. Eine SKU ist eine bestimmte Version eines Produkts mit eigener Beschreibung, Preis und anderen eindeutigen Produktdetails. Jede App und jedes Add-On verfügt über eine Standard-SKU. Die meisten Entwickler verfügen nur dann über mehrere SKUs für eine App, wenn sie eine Vollversion der App und eine Testversion veröffentlichen (im Store-Katalog ist jede dieser Versionen eine andere SKU derselben App). <p/><p/> Einige Herausgeber können ihre eigenen SKUs definieren. Ein großer Spieleherausgeber kann z.B. ein Spiel mit einer SKU, die grünes Blut zeigt, in Märkten veröffentlichen, in denen kein rotes Blut zulässig ist, und eine andere SKU für rotes Blut in allen anderen Märkten. Alternativ kann ein Herausgeber, der digitale Videoinhalte verkauft, zwei SKUs für ein Video veröffentlichen, eine SKU für eine HD-Version und eine andere SKU für die SD-Version. <p/><p/> Jedes Produkt verfügt über eine [Skus](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.skus.aspx)-Eigenschaft, mit der Sie auf die SKUs zugreifen können. |
| Verfügbarkeit  |  Die [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx)-Klasse stellt eine *Verfügbarkeit* für eine SKU dar. Eine Verfügbarkeit ist eine bestimmte Version einer SKU mit eigenen eindeutigen Preisinformationen. Jede SKU verfügt über eine Standardverfügbarkeit. Einige Herausgeber können eigene Verfügbarkeiten zum Vorstellen unterschiedlicher Preisoptionen für eine bestimmte SKU definieren. <p/><p/> Jede SKU verfügt über eine [Availabilities](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.availabilities.aspx)-Eigenschaft, mit der Sie auf die Verfügbarkeiten zugreifen können. Für die meisten Entwickler verfügt jede SKU über eine einzelne Standardverfügbarkeit.  |

<span id="store_ids" />
### Store-IDs

Jede App und jedes Add-On im Store weist eine zugeordnete **Store-ID** auf. Viele der APIs im **Windows.Services.Store**-Namespace erfordern die Store-ID, um einen Vorgang für eine App oder ein Add-On auszuführen. Produkte, SKUs und Verfügbarkeiten weisen unterschiedliche Store-ID-Formate auf.

| Objekttyp |  Store-ID-Format  |
|---------|-------------------|
| Produkt  |  Die Store-ID eines Produkts im Store ist eine zwölfstellige alphanumerische Zeichenfolge, z.B. ```9NBLGGH4R315```. Diese Store-ID ist auf der Windows Dev Center-Dashboardseite für die App oder das Add-On verfügbar und wird von der [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.storeid.aspx)-Eigenschaft des [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)-Objekts zurückgegeben. Diese ID wird mitunter als *Produkt-Store-ID* bezeichnet. |
| SKU |  Für eine SKU hat die Store-ID das Format ```<product Store ID>/xxxx```, wobei ```xxxx``` eine vierstellige alphanumerische Zeichenfolge ist, die eine SKU für das Produkt identifiziert. Beispiel: ```9NBLGGH4R315/000N```. Diese ID wird von der [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.storeid.aspx)-Eigenschaft eines [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx)-Objekts zurückgegeben und mitunter als die *SKU-Store-ID* bezeichnet. |
| Verfügbarkeit  |  Für eine Verfügbarkeit hat die Store-ID das Format ```<product Store ID>/xxxx/yyyyyyyyyyyy```, wobei ```xxxx``` eine vierstellige alphanumerische Zeichenfolge ist, die eine SKU für das Produkt identifiziert, und ```yyyyyyyyyyyy``` eine zwölfstellige alphanumerische Zeichenfolge, die eine Verfügbarkeit für die SKU identifiziert. Beispiel: ```9NBLGGH4R315/000N/4KW6QZD2VN6X```. Diese ID wird von der [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.storeid.aspx)-Eigenschaft eines [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx)-Objekts zurückgegeben und mitunter als die *Verfügbarkeits-Store-ID* bezeichnet.  |

<span id="testing" />
### Testen von Apps, die den Windows.Services.Store-Namespace verwenden

Der **Windows.Services.Store**-Namespace stellt keine Klasse bereit, mit der Sie während der Tests Lizenzinformationen simulieren können. Stattdessen müssen Sie eine App im Store veröffentlichen und diese App auf Ihr Gerät für die Entwicklung herunterladen, um die Lizenz zum Testen zu verwenden. Dies ist eine andere Erfahrung als bei Apps, die den **Windows.ApplicationModel.Store**-Namespace verwenden, da diese Apps mithilfe der [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766)-Klasse Lizenzinformationen während der Tests simulieren können.

Wenn Ihre App APIs im **Windows.Services.Store**-Namespace zum Zugriff auf Informationen für die App und ihre Add-Ons verwendet, gehen Sie zum Testen des Codes wie folgt vor:

1. Wenn Ihre App im Store bereits veröffentlicht und verfügbar ist und Sie diese App für die Verwendung von APIs im **Windows.Services.Store**-Namespace aktualisieren möchten, können Sie direkt loslegen. Wenn Sie Add-Ons für die App anbieten möchten, führen Sie die Aktion zum [Definieren der Add-Ons für Ihre App](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions) im Dev Center-Dashboard aus.

  Wenn Sie noch nicht über eine im Store veröffentlichte und verfügbare App verfügen, erstellen Sie eine einfache App, die die Mindestanforderungen gemäß dem [Zertifizierungskit für Windows-Apps](https://developer.microsoft.com/windows/develop/app-certification-kit) erfüllt, und [übermitteln Sie diese App](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) an das Windows Dev Center-Dashboard. Wenn Sie Add-Ons für die App anbieten möchten, müssen Sie [die Add-Ons für Ihre App definieren](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions). Optional können Sie während der Tests [die App im Store ausblenden](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability).

2. Schreiben Sie Code in Ihrer App, der eine der [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Methoden im **Windows.Services.Store**-Namespace verwendet, um Aufgaben wie [Abrufen der für die aktuelle App verfügbaren Add-Ons](get-product-info-for-apps-and-add-ons.md), [Kauf einer App oder eines Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md) oder [Abrufen von Lizenzinformationen für Ihre App](get-license-info-for-apps-and-add-ons.md) auszuführen. Weitere Beispiele finden Sie unten im Abschnitt mit den verwandten Themen.

3. Klicken Sie in Visual Studio auf das **Menü „Projekt“**, zeigen Sie auf **Store**, und klicken Sie dann auf **App mit Store verknüpfen**. Befolgen Sie die Anweisungen des Assistenten, um das App-Projekt mit der App im Windows Dev Center-Konto zu verknüpfen, die Sie zum Testen verwenden möchten.

  >**Hinweis**&nbsp;&nbsp;Wenn Sie das Projekt nicht mit einer App im Store verknüpfen, legen die [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Methoden die **ExtendedError**-Eigenschaften ihrer Rückgabewerte auf den Fehlercodewert 0x803F6107 fest. Dieser Wert gibt an, dass die App im Store nicht bekannt ist.

4. Falls noch nicht geschehen, installieren Sie die App aus dem im vorherigen Schritt angegebenen Store, führen Sie die App einmal aus, und schließen Sie dann diese App. Damit wird sichergestellt, dass auf dem Gerät für die Entwicklung eine gültige Lizenz für die App installiert wird.

5. Starten Sie in Visual Studio die Ausführung oder das Debuggen des Projekts. Der Code sollte App- und Add-On-Daten aus der Store-App abrufen, die Sie mit dem lokalen Projekt verknüpft haben. Wenn Sie zur Neuinstallation der App aufgefordert werden, folgen Sie den Anweisungen, und führen Sie dann das Projekt aus oder debuggen Sie es.

## Verwandte Themen

* [Abrufen von Produktinformationen zu Apps und Add-Ons](get-product-info-for-apps-and-add-ons.md)
* [Abrufen von Lizenzinformationen zu Apps und Add-Ons](get-license-info-for-apps-and-add-ons.md)
* [Aktivieren von In-App-Einkäufen von Apps und Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Unterstützen von Endverbraucher-Add-On-Käufen](enable-consumable-add-on-purchases.md)
* [Implementieren einer Testversion der App](implement-a-trial-version-of-your-app.md)
* [In-App-Einkäufe und Testversionen mit dem Windows.ApplicationModel.Store-Namespace](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)



<!--HONumber=Aug16_HO5-->


