---
author: jnHs
Description: "Im Bericht zu Add-On-Käufen im Windows Dev Center-Dashboard sehen Sie, wie viele Add-Ons Sie verkauft haben, und Sie können demografische und plattformspezifische Details einsehen."
title: "Bericht zu Add-On-Käufen"
ms.assetid: F2DF9188-0A98-4AC3-81C0-3E2C37B15582
translationtype: Human Translation
ms.sourcegitcommit: 0edf45e997f36a82a8bfcb92c1d8fd2c79242461
ms.openlocfilehash: 144a8400acf0333fcd50e698b333c02942081ef3

---

# Bericht zu Add-On-Käufen


Im Bericht **Add-On-Käufe** im Windows Dev Center-Dashboard sehen Sie, wie viele Add-Ons Sie verkauft haben, und Sie können demografische und plattformspezifische Details einsehen. Sie können diese Informationen in Ihrem Dashboard anzeigen oder [den Bericht herunterladen](download-analytic-reports.md), um die Daten offline anzuzeigen. Sie können diese Daten aber auch programmgesteuert mit der [Windows Store-REST-API für Analysen](../monetize/access-analytics-data-using-windows-store-services.md) abrufen.

In diesem Bericht steht „Add-On-Kauf“ für einen Kunden, der ein Add-On von Ihnen erworben hat. Wenn ein Kunde mehrere Käufe desselben konsumierbaren Add-Ons getätigt hat, werden diese als separate Add-On-Käufe aufgeführt.

> **Wichtig**  Im Bericht **Add-On-Käufe** sind keine Daten über Erstattungen, Rückbuchungen, Rückvergütungen usw. enthalten. Informationen zum Schätzen der Erträge aus Ihren Apps finden Sie unter [Auszahlungszusammenfassung](payout-summary.md). Klicken Sie im Abschnitt **Reserviert** auf den Link **Reservierte Transaktionen herunterladen**.

## Anwenden von Filtern


Im oberen Seitenbereich können Sie **Filter anwenden** erweitern, um alle Daten auf dieser Seite nach Datumsbereich und/oder Gerätetyp zu filtern. Sie können auch Daten nach einem bestimmten Add-On filtern.

-   **Datum**: Der Standardfilter lautet **Letzte 30 Tage**, aber er kann bis auf **Letzte 12 Monate** erweitert werden.
-   **Add-On**: Der Standardfilter ist **Alle Add-Ons**. Wenn Sie Daten zu Käufen nur für eines Ihrer Add-Ons anzeigen möchten, können Sie hier ein bestimmtes Add-On angeben.
-   **Gerätetyp**: Die Standardeinstellung ist **Alle Geräte**. Wenn Daten für Add-On-Käufe nur für einen bestimmten Gerätetyp angezeigt werden sollen, können Sie hier einen bestimmten Typ auswählen.

Die Informationen in den unten angezeigten Diagrammen beziehen sich auf den unter **Filter anwenden** ausgewählten Zeitraum.

Die Informationen in allen unten angezeigten Diagrammen beziehen sich auf den im Abschnitt **Filter anwenden** ausgewählten Zeitraum. Standardmäßig gehören dazu Daten für alle Gerätetypen, sofern Sie nicht mithilfe von **Filter anwenden** einen bestimmten Gerätetyp herausgefiltert haben.

## Add-On-Käufe


Das Diagramm **Add-On-Käufe** zeigt, wie oft Ihre Add-Ons im ausgewählten Zeitraum pro Tag oder Woche gekauft wurde. (Wenn Sie die Daten über einen längeren Zeitraum mithilfe von **Filter anwenden** filtern, werden die Daten nach Woche gruppiert.)

Sie können auch anzeigen, wie oft Add-Ons während ihrer gesamten Lebensdauer gekauft wurden. Zeigt den kumulierten Gesamtwert aller Käufe an (ab der ersten Veröffentlichung Ihrer App).

Das Diagramm enthält auch den Preis, den ein Kunde für das Add-On entrichtet hat.

Optional können Sie die Ergebnisse nach Markt und/oder Betriebssystemversion filtern.

## Wichtigste Add-Ons

Das Diagramm **Wichtigste Add-Ons** zeigt die Gesamtanzahl von Käufen für jedes Ihrer Add-Ons im ausgewählten Zeitraum nach Markt an. Standardmäßig wird das Add-On mit den meisten Käufen an oberster Stelle vor den Märkten mit weniger Käufen angezeigt. Sie können die Reihenfolge umkehren, indem Sie auf den Pfeil in der Spalte **Käufe** des Diagramms klicken.

## Märkte

Das Diagramm **Märkte** zeigt die Gesamtzahl von Add-On-Käufen im ausgewählten Zeitraum nach Markt. Standardmäßig wird der Markt mit den meisten Käufen an oberster Stelle vor den Märkten mit weniger Käufen angezeigt. Sie können die Reihenfolge umkehren, indem Sie auf den Pfeil in der Spalte **Käufe** des Diagramms klicken.

## Kundendemografie

Das Diagramm **Kundendemografie** zeigt demografische Informationen zu den Personen, die Ihr Add-On erworben haben. Sie können sehen, wie viele Käufe (im ausgewählten Zeitraum) von Personen einer bestimmten Altersgruppe getätigt wurden und welches Geschlecht die Käufer hatten.

> **Hinweis**  Einige Kunden haben festgelegt, dass sie diese Informationen nicht freigeben möchten. Falls die Altergruppe oder das Geschlecht nicht ermittelt werden konnten, wird der Kauf als **Unbekannt** kategorisiert.

## Betriebssystemversion

Im Diagramm **Betriebssystemversion** wird die Gesamtzahl der Käufe entsprechend dem Betriebssystem des Kunden angezeigt (bzw. über [großvolumige Käufe durch Organisationen](organizational-licensing.md)). In einigen Fällen können diese Informationen jedoch nicht ermittelt werden. Die Betriebssystemversion wird dann als **Unbekannt** angegeben.

 

 



<!--HONumber=Aug16_HO3-->


