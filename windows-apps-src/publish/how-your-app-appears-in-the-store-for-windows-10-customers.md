---
author: jnHs
Description: "Wenn Sie im Store bereits Apps für Windows oder WindowsPhone veröffentlicht haben, werden diese Apps für Kunden auch auf Windows 10-Geräten verfügbar gemacht."
title: "Darstellung Ihrer App im Store für Windows10-Kunden"
ms.assetid: 487BB57E-F547-4764-99B1-84FE396E6D3A
translationtype: Human Translation
ms.sourcegitcommit: fc7b0d7dd85e661e1688f4df3f67280c16eb6742
ms.openlocfilehash: a3ecb8a4a1aebc719f8a8a4eb140776685f53b4e

---

# Darstellung Ihrer App im Store für Windows10-Kunden


Wenn Sie im Store bereits Apps für Windows oder WindowsPhone veröffentlicht haben, werden diese Apps für Kunden auch auf Windows 10-Geräten verfügbar gemacht. Bei der Darstellung und Kategorisierung von Apps im Store für Kunden mit Windows 10 gab es einige Änderungen. In diesem Thema finden Sie Informationen dazu.

**Hinweis**  Wenn Sie diese Details ändern möchten, [erstellen Sie eine neue Übermittlung](app-submissions.md), nehmen die Änderungen vor und übermitteln das Update an den Store.

 

## Überlegungen in Bezug auf Apps mit einer gemeinsamen Identität im WindowsStore und im WindowsPhone Store


Wenn Sie denselben reservierten Namen für eine in beiden Stores veröffentlichte App verwendet haben (auch bekannt als Teilen der Identität Ihrer App), wird diese jetzt als eine App betrachtet, nicht als zwei. Im Dashboard wird sie als einzelne App mit Paketen für Windows- und WindowsPhone angezeigt.

Die meisten Entwickler legen in jedem Store die gleichen Preise und andere Eigenschaften für die App und für Add-Ons fest. Unterscheiden sich jedoch einige dieser Werte, ist es wichtig zu wissen, welche Ihren Windows10-Kunden angezeigt werden.

### Preise
Wenn Sie für Ihre App (oder Ihr Add-On) in jedem Store unterschiedliche Basispreise ausgewählt haben, wird der Basispreis aus dem WindowsStore verwendet.

**Hinweis**  Wenn Sie im Windows Phone Store marktspezifische Preise festgelegt haben, werden benutzerdefinierte Preise auch für Ihre Windows 10-Kunden angezeigt.

### Kostenlose Testversionen
In den beiden früheren Stores waren unterschiedliche Optionen für Testversionen vorhanden, daher ist es möglich, dass Sie für jeden Store verschiedene Optionen ausgewählt haben. Die für Ihre Windows10-Kunden verfügbare Testversionsoption wird anhand der folgenden Tabelle bestimmt.

| Windows8-App       | Windows Phone-App   | Testversionseinstellung für Windows 10                                                  |
|---------------------|---------------------|-------------------------------------------------------------------------------|
| Keine kostenlose Testversion       | Keine kostenlose Testversion       | Keine kostenlose Testversion                                                                 |
| Keine kostenlose Testversion       | Testversion läuft nie ab | Keine kostenlose Testversion                                                                 |
| Testversion läuft nie ab | Testversion läuft nie ab | Testversion läuft nie ab                                                           |
| Testversion läuft nie ab | Keine kostenlose Testversion       | Keine kostenlose Testversion                                                                 |
| Zeitlich begrenzte Testversion  | Testversion läuft nie ab | Keine kostenlose Testversion unter Windows Phone 8.1 und früheren Versionen; andernfalls zeitlich begrenzte Testversion |
| Zeitlich begrenzte Testversion  | Keine kostenlose Testversion       | Keine kostenlose Testversion unter Windows Phone 8.1 und früheren Versionen; andernfalls zeitlich begrenzte Testversion |

### Märkte
Ihre App ist für Windows10-Kunden in allen Märkten verfügbar, in denen Sie die App heute veröffentlicht haben. Dies gilt auch dann, wenn Sie für jeden Store eine andere Marktauswahl getroffen haben.

### Kategorien
Wenn Ihre App in den beiden Stores in verschiedenen Kategorien angezeigt wurde, verwenden wir die Kategorie aus dem WindowsStore, um die neue Kategorie festzulegen. Beachten Sie, dass einige Kategorien im Store für Windows10-Kunden unterschiedlich sind. Überprüfen Sie daher die folgende [table](#cat).

### Altersfreigabe
Wenn Sie verschiedene Altersfreigaben angegeben haben, wird die strengere Freigabe (höheres Alter) verwendet.

### Datenschutzrichtlinie
Wenn für Ihre App eine Datenschutzrichtlinie vorhanden ist, wird Ihren Windows10-Kunden ebenfalls die Version angezeigt, die Sie beim Einreichen Ihrer Windows8-App bereitgestellt haben.

### Screenshots
Wir prüfen alle von Ihnen übermittelten Screenshots und verwenden basierend auf dem Typ des jeweils verwendeten Geräts die geeignete Version zur Anzeige für Windows10-Kunden. Im seltenen Fall, dass sich Ihre unterstützten Sprachen für jeden Store unterscheiden, wird für einige Kunden u.U. ein Screenshot angezeigt, der am besten die Oberfläche darstellt, die sie beim Kauf der App erhalten würden.

### Store-Einträge
Wir versuchen, basierend auf der Sprache den jeweils am besten geeigneten Store-Eintrag für Ihre Windows10-Kunden anzuzeigen. Wenn Store-Einträge in der gleichen Sprache aus mehreren Quellen verfügbar sind, wird Ihren Windows10-Kunden der Eintrag aus Ihrer WindowsStore-App angezeigt. In dem seltenen Fall, dass sich Ihre unterstützten Sprachen für die einzelnen Stores unterscheiden, wird Kunden möglicherweise ein Store-Eintrag aus Ihrer WindowsPhone-App angezeigt, falls dies der einzige in dieser Sprache bereitgestellte Store-Eintrag ist.

Wenn Sie den Ihren Windows10-Kunden angezeigten Store-Eintrag aktualisieren möchten, um sie über Funktionen zu informieren, die auf mehreren Geräten funktionieren, aktualisieren Sie dazu [die Beschreibung Ihrer App](create-app-store-listings.md) in den vorhandenen Stores. Kunden wird unter Windows 10 die standardmäßige Beschreibung Ihrer App angezeigt, aber Sie können auch [plattformspezifische Store-Einträge erstellen](create-platform-specific-store-listings.md), wenn für Kunden mit unterschiedlichen Betriebssystemversionen verschiedene Store-Einträge angezeigt werden sollen.

## Kategorieänderungen


In vielen Fällen sind neue [Kategorien und Unterkategorien](category-and-subcategory-table.md) für Apps und Spiele die gleichen wie im Store für vorherige Betriebssystemversionen. Es wurden jedoch einige Änderungen vorgenommen. Überprüfen Sie anhand der folgenden Tabelle, wie Ihre App im Store für Windows10-Kunden basierend auf ihrer vorherigen Kategorie kategorisiert wird.

**Hinweis**  Die neue Kategorie wird Ihnen beim Anzeigen der [App-Kategorie](category-and-subcategory-table.md) auf der Seite [App-Eigenschaften](enter-app-properties.md) einer Übermittlung angezeigt. Kunden, die den Store auf Windows 10-Geräten anzeigen, wird die App in der neuen Kategorie angezeigt. Bei Kunden, die den Store unter einem früheren Betriebssystem anzeigen, wird die App jedoch weiterhin in seiner ursprünglichen Kategorie angezeigt.


**Kategorieänderungen für WindowsPhone-Apps:**

| Vorherige Kategorie                       | Neue Kategorie                  |
|-----------------------------------------|-------------------------------|
| Behörden + Politik &gt; Stellungnahmen   | Behörden + Politik         |
| Behörden + Politik &gt; Rechtliche Fragen | Behörden + Politik         |
| Behörden + Politik &gt; Politik     | Behörden + Politik         |
| Behörden + Politik &gt; Ressourcen    | Behörden + Politik         |
| Gesundheit + Fitness &gt; Ernährung  | Gesundheit + Fitness              |
| Gesundheit + Fitness &gt; Fitness           | Gesundheit + Fitness              |
| Gesundheit + Fitness &gt; Gesundheit            | Gesundheit + Fitness              |
| Lifestyle &gt; Kunst + Unterhaltung      | Lifestyle                     |
| Lifestyle &gt; Unterwegs              | Lifestyle                     |
| Lifestyle &gt; Kulinarisches            | Essen + Trinken                 |
| Lifestyle &gt; Shopping                 | Shopping                      |
| News + Wetter &gt; International       | News + Wetter                |
| News + Wetter &gt; Lokal + national    | News + Wetter                |
| Dienstprogramme + Produktivität                | Dienstprogramme + Tools             |
| Reise + Navigation                     | Reisen                        |
| Reise + Navigation &gt; Planen       | Reisen                        |
| Reise + Navigation &gt; Tools          | Reisen                        |
| Reise + Navigation &gt; Mit Kindern      | Kinder + Familie &gt; Reisen     |
| Reise + Navigation &gt; Sprache       | Bildung &gt; Sprache       |
| Reise + Navigation &gt; Zuordnen        | Navigation + Karten             |
| Reise + Navigation &gt; Navigation     | Navigation + Karten             |
| Spiele &gt; Klassiker                     | Spiele &gt; Action + Adventure |
| Spiele &gt; Familie                       | Spiele &gt; Familie und Kinder      |
| Spiele &gt; Sport + Erholung          | Spiele &gt; Sport             |
| Spiele &gt; Strategie + Simulation        | Spiele &gt; Strategie           |

 

**Kategorieänderungen für Windows8-Apps:**

| Vorherige Kategorie           | Neue Kategorie                         |
|-----------------------------|--------------------------------------|
| Bücher + Nachschlagewerke &gt; Kinder | Kinder + Familie &gt; Bücher + Nachschlagewerke |
| Musik + Videos &gt; Video   | Fotos + Videos                        |
| Musik + Videos &gt; Musik   | Musik                                |
| Behörden                  | Behörden + Politik                |
| Finanzen                     | Persönliche Finanzen                     |
| Spiele &gt; Action           | Spiele &gt; Action + Adventure        |
| Spiele &gt; Adventure        | Spiele &gt; Action + Adventure        |
| Games &gt; Arcade           | Spiele &gt; Action + Adventure        |
| Spiele &gt; Karten             | Spiele &gt; Karten + Brett              |
| Spiele &gt; Kinder             | Spiele &gt; Familie und Kinder             |
| Spiele &gt; Familie           | Spiele &gt; Familie und Kinder             |
| Spiele &gt; Rätsel           | Spiele &gt; Rätsel + Trivia           |
| Spiele &gt; Rennsport           | Spiele &gt; Rennsport + Fliegen           |



<!--HONumber=Aug16_HO5-->


