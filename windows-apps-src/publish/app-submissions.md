---
author: jnHs
Description: "Nachdem Sie Ihre App durch die Reservierung eines Namens erstellt haben, können Sie mit der Veröffentlichung beginnen. Der erste Schritt besteht darin, eine Übermittlung zu erstellen."
title: "App-Übermittlungen"
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: "Prüfliste"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: ce9858da15cac0e34a2bb2c68c25ba63ec79af4c

---

# App-Übermittlungen


Nachdem Sie Ihre [App durch die Reservierung eines Namens erstellt haben](create-your-app-by-reserving-a-name.md), können Sie mit der Veröffentlichung beginnen. Der erste Schritt besteht darin, eine ***Übermittlung** zu erstellen.

Sie beginnen mit der Einreichung, sobald Ihre App fertig und bereit für die Veröffentlichung ist. Sie können mit der Eingabe von Infos beginnen, noch bevor Sie eine einzige Codezeile programmiert haben. Die Einreichung wird in Ihrem Dashboard gespeichert, sodass Sie zu einem beliebigen Zeitpunkt damit arbeiten können.

Nach der Veröffentlichung Ihrer App können Sie eine aktualisierte Version veröffentlichen, indem Sie eine weitere Einreichung im Dashboard erstellen. Durch die Erstellung einer neuen Einreichung können Sie alle erforderlichen Änderungen vornehmen und veröffentlichen – z. B. neue Pakete hochladen oder Preisdetails oder App-Kategorie ändern. Um eine neue Übermittlung für eine App zu erstellen, klicken Sie neben der aktuellen Übermittlung, die auf der App-Übersichtsseite angezeigt wird, auf **Aktualisieren**.

> **Hinweis**&nbsp;&nbsp;In diesem Abschnitt der Dokumentation wird das Erstellen einer App-Übermittlung über das Dev Center-Dashboard beschrieben. Alternativ dazu können Sie auch die [Windows Store-Übermittlungs-API](../monetize/create-and-manage-submissions-using-windows-store-services.md) verwenden, um App-Übermittlungen zu automatisieren.

## Prüfliste für die App-Übermittlung


Hier finden Sie eine Liste mit den Informationen, die Sie beim Erstellen Ihrer App-Übermittlung angeben können, sowie Links zu weiteren Informationen.

Die erforderlichen Elemente sind im Folgenden aufgeführt. Einige Bereiche sind optional oder verfügen über Standardwerte, die Sie nach Bedarf ändern können.

### Seite „Preise und Verfügbarkeit“
| Feldname                    | Hinweise                                       | Weitere Informationen                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Grundpreis**                | Erforderlich                                    | [Grundpreis](set-app-pricing-and-availability.md#base-price)              |
| **Kostenlose Testversion**                | Standard: Keine kostenlose Testversion                      | [Hinzufügen von Testversionen und In-App-Einkäufen](https://msdn.microsoft.com/library/windows/apps/jj193599)  |
| **Märkte und angepasste Preise** | Standard: alle möglichen Märkte, keine angepassten Preise | [Festlegen des Preises und Auswählen der Märkte](define-pricing-and-market-selection.md)              |
| **Sonderangebotsverkaufspreise**              | Optional                                    | [Anbieten vergünstigter Apps und Add-Ons](put-apps-and-add-ons-on-sale.md)                                       |
| **Verteilung und Sichtbarkeit** | Standard: Diese App im Store verfügbar machen | [Verteilung und Sichtbarkeit](set-app-pricing-and-availability.md#distribution-and-visibility) |
| **Organisationslizenzierung**    | Standard: Großvolumige Käufe durch Organisationen sind erlaubt. | [Optionen für die Organisationslizenzierung](organizational-licensing.md)                        |
| **Veröffentlichungsdatum**                | Standard: so schnell wie möglich veröffentlichen      | [Veröffentlichungsdatum](set-app-pricing-and-availability.md#publish-date)          |

<span/>

### Seite „App-Eigenschaften“

| Feldname                    | Hinweise                                       | Weitere Informationen                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Kategorie und Unterkategorie**  | Erforderlich                                    | [Kategorie- und Unterkategorietabelle](category-and-subcategory-table.md)       |
| **Systemanforderungen**      | Optional                                    | [Systemanforderungen](enter-app-properties.md#system-requirements)      |
| **App-Deklarationen**          | Standard: Kunden können diese App auf alternativen Laufwerken oder Wechselmedien installieren. Windows kann die Daten dieser App in automatische Sicherungen auf OneDrive einschließen. | [App-Deklarationen](app-declarations.md) |

<span/>

### Seite „Altersfreigaben“

| Feldname                    | Hinweise                                       | Weitere Informationen                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Altersfreigaben**               | Erforderlich                                    | [Altersfreigaben](age-ratings.md)          |

<span/>

### Seite „Pakete“

| Feldname                    | Hinweise                                  | Weitere Informationen                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Package upload control (Paketuploadsteuerung)**    | Erforderlich (mindestens ein Paket)        | [Hochladen von App-Paketen](upload-app-packages.md) |
| **Verfügbarkeit von Gerätefamilien** | Standard: basierend auf Ihren Paketen       | [Verfügbarkeit von Gerätefamilien](upload-app-packages.md#device-family-availability) |
| **Schrittweises Paketrollout**   | Optional (nur für Updates)            | [Schrittweises Paketrollout](gradual-package-rollout.md) |
| **Erforderliches Update**          | Optional (nur für Updates)            | [Erforderliches Update](upload-app-packages.md#mandatory-update)

<span/>

### Store-Einträge

Sie benötigen alle erforderlichen Informationen für mindestens eine der von Ihrer App unterstützten Sprachen. Wir empfehlen Ihnen, [Store-Einträge](create-app-store-listings.md) in allen Sprachen anzugeben, die von der App unterstützt werden. Außerdem können Sie [Store-Einträge in weiteren Sprachen angeben](create-app-store-listings.md#store-listing-languages).

| Feldname                    | Hinweise                                       | Weitere Informationen                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Beschreibung**               | Erforderlich                                    | [Erstellen einer interessanten App-Beschreibung](write-a-great-app-description.md) |
| **Versionshinweise**             | Optional                                    | [Versionshinweise](create-app-store-listings.md#release-notes)         |
| **Screenshots**               | Erforderlich (mindestens ein Screenshot)          | [App-Screenshots und -Bilder](app-screenshots-and-images.md)       |
| **Symbol für App-Kachel**             | Optional, jedoch dringend empfohlen für Windows Phone8.1 und frühere Versionen | [Symbol für App-Kachel](create-app-store-listings.md#app-tile-icon) |
| **Werbebilder**       | Optional                                    | [App-Screenshots und -Bilder](app-screenshots-and-images.md)       |
| **App-Features**              | Optional                                    | [Features](create-app-store-listings.md#app-features)               |
| **Weitere Systemanforderungen**      | Optional                                    | [Weitere Systemanforderungen](create-app-store-listings.md#additional-system-requirements) |
| **Schlüsselwörter**                  | Optional                                    | [Schlüsselwörter](create-app-store-listings.md#keywords)                   |
| **Urheberrecht- und Markeninformationen** | Optional                                 | [Urheberrecht- und Markeninformationen](create-app-store-listings.md#copyright-and-trademark-info) |
| **Zusätzliche Lizenzbedingungen**  | Optional                                    | [Additional license terms (Zusätzliche Lizenzbedingungen)](create-app-store-listings.md#additional-license-terms) |
| **Website**                   | Optional                                    | [Website](create-app-store-listings.md#website)                     |
| **Support – Kontaktinfos**      | Optional                                    | [Support – Kontaktinfos](create-app-store-listings.md)                |
| **Privacy policy (Datenschutzrichtlinie)**            | Für einige Apps erforderlich. Weitere Informationen finden Sie in der [Vereinbarung für App-Entwickler](https://msdn.microsoft.com/library/windows/apps/hh694058) und den [WindowsStore-Richtlinien](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_5_1). | [Datenschutzrichtlinie](create-app-store-listings.md#privacy-policy) |
| **Plattformspezifische Store-Einträge** | Optional                               | [Erstellen plattformspezifischer Store-Einträge](create-platform-specific-store-listings.md) |

<span/>

### Seite „Hinweise für Zertifizierung“

| Feldname                    | Hinweise                                       | Weitere Informationen                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Hinweise**                     | Optional                                    | [Hinweise für Zertifizierung](notes-for-certification.md)             |

<span/>

**Hinweis**&nbsp;&nbsp;Informationen zum Veröffentlichen von branchenspezifischen Apps direkt für Unternehmen finden Sie unter [Verteilen von branchenspezifischen Apps an Unternehmen](distribute-lob-apps-to-enterprises.md).



<!--HONumber=Aug16_HO5-->


