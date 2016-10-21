---
author: mcleanbyron
Description: "Sie können unabhängig davon, ob Ihre App kostenlos oder kostenpflichtig ist, Inhalte, andere Apps oder neue App-Funktionen (wie das Freischalten des nächsten Levels eines Spiels) direkt in der App verkaufen. Hier zeigen wir Ihnen, wie Sie diese Produkte in Ihrer App aktivieren können."
title: "Unterstützen von Käufen von In-App-Produkten"
ms.assetid: D158E9EB-1907-4173-9889-66507957BD6B
keywords: In-App-Angebot, Codebeispiel
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 531b5c5a5c70461e98b5809246fdce7215805a25

---

# Unterstützen von Käufen von In-App-Produkten



>**Hinweis**&nbsp;&nbsp;In diesem Artikel wird die Verwendung von Mitgliedern des [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)-Namespace erläutert. Wenn Ihre App für Windows10, Version 1607 oder höher, vorgesehen ist, empfehlen wir die Verwendung von Mitgliedern des [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)-Namespace zum Verwalten von Add-Ons (auch als In-App-Produkte oder IAPs bezeichnet), anstelle des **Windows.ApplicationModel.Store**-Namespace. Weitere Informationen finden Sie unter [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md).

Sie können unabhängig davon, ob Ihre App kostenlos oder kostenpflichtig ist, Inhalte, andere Apps oder neue App-Funktionen (wie das Freischalten des nächsten Levels eines Spiels) direkt in der App verkaufen. Hier zeigen wir Ihnen, wie Sie diese Produkte in Ihrer App aktivieren können.

> **Hinweis**  In-App-Produkte können nicht in einer Testversion einer App angeboten werden. Kunden, die eine Testversion Ihrer App verwenden, können nur dann In-App-Produkte kaufen, wenn sie eine Vollversion der App kaufen.

## Voraussetzungen

-   Eine Windows-App zum Hinzufügen von Features zum Kauf für Kunden.
-   Wenn Sie Code für neue In-App-Produkte erstmalig schreiben und testen, müssen Sie anstelle des [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)-Objekts das [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)-Objekt verwenden. Auf diese Weise können Sie überprüfen, ob die Lizenzlogik simulierte Aufrufe an den Lizenzserver und nicht an den Liveserver verwendet. Dazu müssen Sie die Datei mit dem Namen „WindowsStoreProxy.xml“ in %userprofile%\\AppData\\local\\packages\\&lt;package name&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData anpassen. Diese Datei wird vom Simulator in Microsoft Visual Studio erstellt, wenn Sie Ihre App erstmalig ausführen. Sie können jedoch auch eine benutzerdefinierte Version dieser Datei zur Laufzeit laden. Weitere Informationen finden Sie unter [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766).
-   In diesem Thema wird auch auf Codebeispiele verwiesen, die im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store) zu finden sind. Dieses Beispiel bietet eine hervorragende Möglichkeit, die verschiedenen Monetarisierungsoptionen zu testen, die für Universelle Windows-Plattform (UWP)-Apps verfügbar sind.

## Schritt 1: Initialisieren der Lizenzinfos für Ihre App

Rufen Sie bei der Initialisierung Ihrer App das [**LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/br225157)-Objekt für Ihre App ab, indem Sie [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) oder [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) initialisieren, um Einkäufe von In-App-Produkten zu aktivieren.

```CSharp
void AppInit()
{
    // some app initialization functions

    // Get the license info
    // The next line is commented out for testing.
    // licenseInformation = CurrentApp.LicenseInformation;

    // The next line is commented out for production/release.       
    licenseInformation = CurrentAppSimulator.LicenseInformation;

    // other app initialization functions
}
```

## Schritt 2: Hinzufügen von In-App-Produktangeboten zu Ihrer App

Erstellen Sie für jedes Feature, das über ein In-App-Produkt zur Verfügung stehen soll, ein Angebot in der App, und fügen Sie es Ihrer App hinzu.

> **Wichtig**  Sie müssen Ihrer App alle In-App-Produkte hinzufügen, die Sie Ihren Kunden zur Verfügung stellen möchten, bevor Sie diese an den Store übermitteln. Wenn Sie zu einem späteren Zeitpunkt neue In-App-Produkte hinzufügen möchten, müssen Sie die App aktualisieren und eine neue Version übermitteln.

1.  **Erstellen Sie ein Token für In-App-Angebote**

    Sie identifizieren die einzelnen In-App-Produkte Ihrer App durch Token. Bei diesem Token handelt es sich um eine Zeichenfolge, die Sie festlegen und in Ihrer App und im Store verwenden, um ein bestimmtes In-App-Produkt zu identifizieren. Geben Sie ihm einen (für Ihre App) eindeutigen und aussagekräftigen Namen, sodass Sie beim Schreiben des Codes schnell das richtige Feature ermitteln können, für das es steht. Im Folgenden finden Sie einige Beispiele für Namen:

    -   "SpaceMissionLevel4"

    -   "ContosoCloudSave"

    -   "RainbowThemePack"

2.  **Schreiben Sie den Code für das Feature in einem Bedingungsblock.**

    Sie müssen den Code für jedes Feature, das mit einem In-App-Produkt verknüpft ist, in einen Bedingungsblock aufnehmen. Dieser Bedingungsblock überprüft, ob ein Kunde eine Lizenz für die Verwendung dieses Features besitzt.

    Im folgenden Beispiel wird gezeigt, wie Sie Code für das Produkt-Feature **featureName** in einem lizenzspezifischen Bedingungsblock kodieren. Die Zeichenfolge **featureName** ist das Token, das dieses Produkt eindeutig in der App und im Store identifiziert.

    ```    CSharp
    if (licenseInformation.ProductLicenses["featureName"].IsActive)
    {
        // the customer can access this feature
    }
    else
    {
        // the customer can' t access this feature
    }
    ```

3.  **Fügen Sie die Kauf-UI für dieses Feature hinzu.**

    Ihre App muss den Kunden außerdem die Möglichkeit bieten, das über das In-App-Produkt angebotene Produkt oder Feature zu kaufen. Das Feature oder Produkt kann nicht auf die gleiche Weise wie die gesamte App im Store erworben werden.

    Hier finden Sie ein Beispiel dafür, wie Sie testen, ob der Kunde bereits ein In-App-Produkt besitzt. Es veranschaulicht außerdem, wie das Kaufdialogfeld angezeigt wird, sodass der Kunde es ggf. erwerben kann. Ersetzen Sie den Kommentar „show the purchase dialog“ durch den benutzerdefinierten Code für das Kaufdialogfeld (z.B. ein Fenster mit der Schaltfläche „Diese App kaufen“) .

    ```    CSharp
    void BuyFeature1()
    {
        if (!licenseInformation.ProductLicenses["featureName"].IsActive)
        {
            try
            {
                // The customer doesn't own this feature, so
                // show the purchase dialog.
                await CurrentAppSimulator.RequestProductPurchaseAsync("featureName", false);

                //Check the license state to determine if the in-app purchase was successful.
            }
            catch (Exception)
            {
                // The in-app purchase was not completed because
                // an error occurred.
            }
        }
        else
        {
            // The customer already owns this feature.
        }
    }
    ```

## Schritt 3: Ändern Sie den Testcode für die endgültigen Aufrufe.

Dies ist ein einfacher Schritt: Ändern Sie im Code Ihrer App alle Verweise auf [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) in [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765). Sie müssen die Datei „WindowsStoreProxy.xml“ nicht mehr bereitstellen. Entfernen Sie diese daher aus dem Pfad Ihrer App. Sie können sie jedoch zu späteren Referenzzwecken speichern, wenn Sie im nächsten Schritt das Angebot in der App konfigurieren.

## Schritt 4: Konfigurieren des In-App-Produktangebots im Store

Definieren Sie im Dev Center-Dashboard die Produkt-ID, den Typ, den Preis und andere Eigenschaften für das In-App-Produkt. Die Konfiguration muss genau mit der Konfiguration in der Datei WindowsStoreProxy.xml übereinstimmen, die Sie beim Testen festlegen. Weitere Informationen finden Sie unter [IAP-Einreichungen](https://msdn.microsoft.com/library/windows/apps/mt148551).

## Hinweise

Wenn Sie Ihren Kunden konsumierbare In-App-Produktoptionen (Elemente, die gekauft, verwendet und erneut gekauft werden können, wenn gewünscht) bereitstellen möchten, wechseln Sie zum Thema [Unterstützen von Käufen konsumierbarer In-App-Produkte](enable-consumable-in-app-product-purchases.md).

Wenn Sie anhand von Belegen überprüfen möchten, ob ein Kunde einen In-App-Einkauf getätigt hat, lesen Sie den Artikel [Überprüfen von Produktkäufen anhand von Belegen](use-receipts-to-verify-product-purchases.md).

## Verwandte Themen


* [Käufe von konsumierbaren In-App-Produkten aktivieren](enable-consumable-in-app-product-purchases.md)
* [Verwalten eines großen Katalogs mit In-App-Produkten](manage-a-large-catalog-of-in-app-products.md)
* [Überprüfen von Produktkäufen anhand von Belegen](use-receipts-to-verify-product-purchases.md)
* [Store-Beispiel (zeigt Testversionen und In-App-Einkäufe)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)



<!--HONumber=Aug16_HO5-->


