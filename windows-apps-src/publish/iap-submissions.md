---
author: jnHs
Description: IAPs werden über das Windows Dev Center-Dashboard veröffentlicht.
title: IAP-Übermittlungen
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
---

# IAP-Übermittlungen


Ein IAP (In-App-Produkt) ist ein zusätzlicher Artikel für Ihre App, der von Kunden erworben werden kann. Ein IAP kann ein lustiges neues Add-On-Feature, ein neues Gamelevel oder etwas anderes sein, von dem Sie denken, dass Benutzer Spaß daran haben. IAPs bieten nicht nur eine gute Möglichkeit, Geld zu verdienen, sondern fördern die Kundeninteraktion und -bindung.

IAPs werden über das Windows Dev Center-Dashboard veröffentlicht. Sie müssen auch [die IAPs](https://msdn.microsoft.com/library/windows/apps/mt219684) im Code Ihrer App aktivieren.

Der erste Schritt bei der IAP-Einreichung besteht darin, die IAP im Dashboard zu erstellen, indem Sie dessen [Produkt-ID definieren](set-your-iap-product-id.md). Danach können Sie eine Einreichung erstellen, damit Ihr IAP über den Windows Store erworben werden kann. Sie können ein IAP gleichzeitig mit [Ihrer App einreichen](app-submissions.md), oder unabhängig vorgehen. Außerdem können Sie [Updates](#updating-an-iap-after-submission) für IAPs ausführen, nachdem die App im Store eingetragen ist, ohne dass die App erneut eingereicht werden muss.

## Prüfliste für die Einreichung eines IAPs


Hier finden Sie die Informationen, die Sie beim Erstellen Ihrer IAP-Übermittlung angeben können. Die erforderlichen Elemente sind im Folgenden aufgeführt. Einige Elemente sind optional oder verfügen bereits über Standardwerte, die Sie nach Bedarf ändern können.

### Seite „Eigenschaften“
| Feldname                    | Hinweise                                       | Weitere Informationen                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Produkttyp**              | Erforderlich. Bei **Gebrauchsgut** muss **Produktlebenszeit** angegeben werden. | [Eingeben von IAP-Eigenschaften](enter-iap-properties.md)         |
| **Inhaltstyp**              | Erforderlich                                    | [Eingeben von IAP-Eigenschaften](enter-iap-properties.md)                           | 
| **Schlüsselwörter**                  | Optional (bis zu 10 Schlüsselwörter mit jeweils max. 30 Zeichen) | [Eingeben von IAP-Eigenschaften](enter-iap-properties.md)                 |
| **Tag**                       | Optional (max. 3.000 Zeichen)             | [Eingeben von IAP-Eigenschaften](enter-iap-properties.md)                           |

### Seite „Preise und Verfügbarkeit“ 
| Feldname                    | Hinweise                                       | Weitere Informationen                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Grundpreis**                | Erforderlich                                    | [Festlegen der Preise und Verfügbarkeit von IAPs](set-iap-pricing-and-availability.md)   |
| **Märkte und angepasste Preise** | Standard: in allen möglichen Märkten verfügbar | [Festlegen der Preise und Verfügbarkeit von IAPs](set-iap-pricing-and-availability.md)   |
| **Verkaufspreise**              | Optional                                    | [Festlegen der Preise und Verfügbarkeit von IAPs](set-iap-pricing-and-availability.md)   |
| **Verteilung und Sichtbarkeit** | Standard: Das IAP kann von Kunden beim Browsen oder Suchen im Store gefunden werden. | [Festlegen der Preise und Verfügbarkeit von IAPs](set-iap-pricing-and-availability.md) |
| **Veröffentlichungsdatum**              | Standard: Das IAP wird direkt nach der Zertifizierung veröffentlicht. | [Festlegen der Preise und Verfügbarkeit von IAPs](set-iap-pricing-and-availability.md)   |

### Seite „Beschreibungen“
Eine Beschreibung ist erforderlich. Es wird empfohlen, für jede von der App unterstützte Sprache Beschreibungen anzugeben.

| Feldname                    | Hinweise                                       | Weitere Informationen       |
|-------------------------------|---------------------------------------------|---------------------|
| **Titel**                     | Erforderlich (max. 100 Zeichen)              | [Erstellen von IAP-Beschreibungen](create-iap-descriptions.md)                     |
| **Beschreibung**               | Optional (max. 200 Zeichen)              | [Erstellen von IAP-Beschreibungen](create-iap-descriptions.md)                     |
| **Symbol**                      | Optional (PNG, 300x300 Pixel)             | [Erstellen von IAP-Beschreibungen](create-iap-descriptions.md)                     |

Nachdem Sie diese Informationen eingegeben haben, klicken Sie auf **An Store übermitteln**. In den meisten Fällen dauert der Zertifizierungsprozess etwa eine Stunde. Danach wird Ihr IAP im Store veröffentlicht und steht für Kunden zum Kauf bereit.

**Hinweis**  Der IAP muss außerdem im Code Ihrer App implementiert werden. Weitere Informationen finden Sie unter [Ermöglichen von In-App-Produktkäufen](https://msdn.microsoft.com/library/windows/apps/mt219684).


## Aktualisieren eines IAPs nach der Veröffentlichung


Sie können ein veröffentlichtes IAP jederzeit ändern. IAP-Änderungen werden unabhängig von Ihrer App eingereicht und veröffentlicht. Sie müssen daher in der Regel nicht die gesamte App aktualisieren, um Änderungen an einem IAP wie das Aktualisieren des Preises oder der Beschreibung vorzunehmen.

> **Wichtig**  Wenn die App für Kunden unter Windows 8.x verfügbar ist, müssen Sie eine neue App-Übermittlung erstellen und veröffentlichen, um die IAP-Updates für diese Kunden sichtbar zu machen. Auch wenn Sie neue IAPs einer App für Windows 8.x hinzufügen, nachdem die App veröffentlicht wurde, müssen Sie den App-Code aktualisieren, um auf diese IAPs zu verweisen, und dann die App erneut einreichen. Andernfalls sind die neue IAPs nicht für Kunden unter Windows 8.x sichtbar.

Wechseln Sie zum Einreichen von Updates im Dashboard zur Seite des IAPs, und klicken Sie auf **Aktualisieren**. Dadurch wird eine neue Einreichung für das IAP erstellt, wobei die Informationen aus der vorherigen Einreichung als Ausgangspunkt verwendet werden. Ändern Sie die gewünschten Informationen, und klicken Sie dann auf **An Store einreichen**.

Wenn Sie zuvor angebotene IAPs entfernen möchten, können Sie dies tun, indem Sie eine neue Übermittlung erstellen und die Option [Verteilung und Sichtbarkeit](set-iap-pricing-and-availability.md) in **Nicht mehr zum Kauf erhältlich. Wird nicht im App-Eintrag angezeigt.** ändern. Achten Sie darauf, den App-Code entsprechend zu aktualisieren, um auch Verweise auf das IAP zu entfernen.



<!--HONumber=May16_HO2-->


