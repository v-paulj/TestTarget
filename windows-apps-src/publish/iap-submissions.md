---
author: jnHs
Description: "IAPs werden über das Windows Dev Center-Dashboard veröffentlicht."
title: "IAP-Übermittlungen"
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.sourcegitcommit: 97f4aee47cab9064ac053e7a6e16441d6960d41f
ms.openlocfilehash: 4a1764dfb8f94409aba973a28ba2999854179196

---

# IAP-Übermittlungen


Ein IAP (In-App-Produkt) ist ein zusätzlicher Artikel für Ihre App, der von Kunden erworben werden kann. Ein IAP kann ein lustiges neues Add-On-Feature, ein neues Gamelevel oder etwas anderes sein, von dem Sie denken, dass Benutzer Spaß daran haben. IAPs bieten nicht nur eine gute Möglichkeit, Geld zu verdienen, sondern fördern die Kundeninteraktion und -bindung.

IAPs werden über das Windows Dev Center-Dashboard veröffentlicht. Sie müssen auch [die IAPs](../monetize/enable-in-app-product-purchases.md) im Code Ihrer App aktivieren.

Der erste Schritt bei der IAP-Übermittlung besteht darin, die IAP im Dashboard zu erstellen, indem Sie den [Produkt-Typ und die Produkt-ID definieren](set-your-iap-product-id.md). Danach können Sie eine Übermittlung erstellen, damit Ihr IAP über den Windows Store erworben werden kann. Sie können ein IAP gleichzeitig mit [Ihrer App einreichen](app-submissions.md), oder unabhängig vorgehen. Außerdem können Sie [Updates](#updating-an-iap-after-submission) für IAPs ausführen, nachdem die App im Store eingetragen wurde, ohne dass die App erneut eingereicht werden muss.

## Prüfliste für die Übermittlung eines IAPs

Hier finden Sie eine Liste der Informationen, die Sie beim Erstellen Ihrer IAP-Übermittlung angeben können. Die erforderlichen Elemente sind im Folgenden aufgeführt. Einige Elemente sind optional oder verfügen bereits über Standardwerte, die Sie nach Bedarf ändern können.

### Erstellen einer neuen IAP-Seite
| Feldname                    | Hinweise                            | 
|-------------------------------|----------------------------------|
| [**Produkttyp**](set-your-iap-product-id.md#product-type)      | Erforderlich. Bei **Gebrauchsgut** muss **Produktlebenszeit** angegeben werden. |  
| [**Produkt-ID**](set-your-iap-product-id.md#product-id)          | Erforderlich |        

### Seite „Eigenschaften“
| Feldname                    | Hinweise                              |   
|-------------------------------|------------------------------------|
| [**Produktlebensdauer**](enter-iap-properties.md#product-lifetime)  | Erforderlich, wenn der Produkttyp **Gebrauchsgut** lautet. Nicht zutreffend, wenn der Produkttyp **Verbrauchsartikel** lautet. | 
| [**Inhaltstyp**](enter-iap-properties.md#content-type)          | Erforderlich       |               
| [**Schlüsselwörter**](enter-iap-properties.md#keywords)                  | Optional (bis zu zehn Schlüsselwörter mit jeweils max. 30 Zeichen) | 
| [**Tag**](enter-iap-properties.md#tag)                               | Optional (max. 3.000 Zeichen)             | 

### Seite „Preise und Verfügbarkeit“ 
| Feldname                    | Hinweise                                       | 
|-------------------------------|---------------------------------------------|
| [**Grundpreis**](set-iap-pricing-and-availability.md#base-price)                | Erforderlich                                    | 
| [**Märkte und angepasste Preise**](set-iap-pricing-and-availability.md#markets-and-custom-prices)  | Standard: in allen möglichen Märkten verfügbar | 
| [**Verkaufspreise**](put-apps-and-iaps-on-sale.md)               | Optional                             |
| [**Verteilung und Sichtbarkeit**](set-iap-pricing-and-availability.md#distribution-and-visibility)   | Standard: Das IAP kann von Kunden beim Browsen oder Suchen im Store gefunden werden. | 
| [**Veröffentlichungsdatum**](set-iap-pricing-and-availability.md#publish-date)                | Standard: Das IAP wird direkt nach der Zertifizierung veröffentlicht. |

### Seite „Beschreibungen“
Eine Beschreibung ist erforderlich. Es wird empfohlen, für jede von der App unterstützte [Sprache](create-iap-descriptions.md#languages) Beschreibungen anzugeben.

| Feldname                    | Hinweise                                       | 
|-------------------------------|---------------------------------------------|
| [**Titel**](create-iap-descriptions.md#title)                    | Erforderlich (max. 100 Zeichen)              |
| [**Beschreibung**](create-iap-descriptions.md#description)       | Optional (max. 200 Zeichen)              |
| [**Symbol**](create-iap-descriptions.md#icon)                    | Optional (PNG, 300x300 Pixel)             | 

Nachdem Sie diese Informationen eingegeben haben, klicken Sie auf **An Store einreichen**. In den meisten Fällen dauert der Zertifizierungsprozess etwa eine Stunde. Danach wird Ihr IAP im Store veröffentlicht und steht für Kunden zum Kauf bereit.

**Hinweis** Das IAP muss außerdem im Code Ihrer App implementiert werden. Weitere Informationen finden Sie unter [Ermöglichen von In-App-Produktkäufen](../monetize/enable-in-app-product-purchases.md).


## Aktualisieren eines IAPs nach der Veröffentlichung

Sie können ein veröffentlichtes IAP jederzeit ändern. IAP-Änderungen werden unabhängig von Ihrer App eingereicht und veröffentlicht. Sie müssen daher in der Regel nicht die gesamte App aktualisieren, um Änderungen an einem IAP wie das Aktualisieren des Preises oder der Beschreibung vorzunehmen.

> **Wichtig**  Wenn die App für Kunden unter Windows 8.x verfügbar ist, müssen Sie eine neue App-Übermittlung erstellen und veröffentlichen, um die IAP-Updates für diese Kunden sichtbar zu machen. Auch wenn Sie neue IAPs einer App für Windows 8.x hinzufügen, nachdem die App veröffentlicht wurde, müssen Sie den App-Code aktualisieren, um auf diese IAPs zu verweisen, und dann die App erneut einreichen. Andernfalls sind die neue IAPs nicht für Kunden unter Windows 8.x sichtbar.

Wechseln Sie zum Einreichen von Updates im Dashboard zur Seite des IAPs, und klicken Sie auf **Aktualisieren**. Dadurch wird eine neue Einreichung für das IAP erstellt, wobei die Informationen aus der vorherigen Einreichung als Ausgangspunkt verwendet werden. Ändern Sie die gewünschten Informationen, und klicken Sie dann auf **An Store einreichen**.

Wenn Sie zuvor angebotene IAPs entfernen möchten, können Sie dies tun, indem Sie eine neue Übermittlung erstellen und die Option [Verteilung und Sichtbarkeit](set-iap-pricing-and-availability.md) in **Nicht mehr zum Kauf erhältlich. Wird nicht im App-Eintrag angezeigt.** ändern. Achten Sie darauf, den App-Code entsprechend zu aktualisieren, um auch Verweise auf das IAP zu entfernen.




<!--HONumber=Jun16_HO4-->


