---
author: jnHs
Description: "Im Bericht IAP-Käufe im Windows Dev Center-Dashboard können Sie sehen, wie viele IAPs Sie verkauft haben. Außerdem können Sie demografische und plattformspezifische Details anzeigen."
title: "Bericht zu IAP-Käufen"
ms.assetid: F2DF9188-0A98-4AC3-81C0-3E2C37B15582
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 3191df12ca5b59f545cf250c3e669b30192af6bc

---

# Bericht zu IAP-Käufen


Im Bericht **IAP-Käufe** im Windows Dev Center-Dashboard können Sie sehen, wie viele IAPs Sie verkauft haben. Außerdem können Sie demografische und plattformspezifische Details anzeigen. Sie können diese Informationen in Ihrem Dashboard anzeigen oder [den Bericht herunterladen](download-analytic-reports.md), um die Daten offline anzuzeigen. Sie können diese Daten jedoch auch programmgesteuert mit der [Windows Store-REST-API für Analysen](../monetize/access-analytics-data-using-windows-store-services.md) abrufen.

In diesem Bericht steht „IAP-Kauf“ für einen Kunden, der ein IAP von Ihnen erworben hat. Wenn ein Kunde mehrere Käufe desselben konsumierbaren IAPs getätigt hat, werden diese als separate IAP-Käufe aufgeführt.

> **Wichtig**  Im Bericht **IAP-Käufe** sind keine Daten über Erstattungen, Rückbuchungen, Rückvergütungen usw. enthalten. Um die Erträge aus Ihren Apps zu schätzen, besuchen Sie [Auszahlungszusammenfassung](payout-summary.md). Klicken Sie im Abschnitt **Reserviert** auf den Link **Reservierte Transaktionen herunterladen**.

## Anwenden von Filtern


Im oberen Seitenbereich können Sie **Filter anwenden** erweitern, um alle Daten auf dieser Seite nach Datumsbereich und/oder Gerätetyp zu filtern. Sie können auch Daten nach einem bestimmten IAP filtern.

-   **Datum**: Der Standardfilter lautet **Letzte 30 Tage**, er kann jedoch bis auf **Letzte 12 Monate** erweitert werden.
-   **IAP**: Der Standardfilter ist **Alle IAPs**. Wenn Sie Daten zu Käufen nur für einen Ihrer IAPs anzeigen möchten, können Sie hier einen bestimmten angeben.
-   **Gerätetyp**: Die Standardeinstellung ist **Alle Geräte**. Wenn Daten für IAP-Käufe nur für einen bestimmten Gerätetyp angezeigt werden sollen, können Sie hier einen bestimmten auswählen.

Die Informationen in den unten angezeigten Diagrammen beziehen sich auf den unter **Filter anwenden** ausgewählten Zeitraum.

Die Informationen in allen unten angezeigten Diagrammen beziehen sich auf den im Abschnitt **Filter anwenden** ausgewählten Zeitraum. Standardmäßig gehören dazu Daten für alle Gerätetypen, sofern Sie nicht mithilfe von **Filter anwenden** einen bestimmten Gerätetyp herausgefiltert haben.

## IAP-Käufe


Das Diagramm **IAP-Käufe** zeigt, wie oft Ihre IAPs im ausgewählten Zeitraum pro Tag oder Woche gekauft wurden. (Wenn Sie die Daten über einen längeren Zeitraum mithilfe von **Filter anwenden** filtern, werden die Daten nach Woche gruppiert.)

Sie können auch anzeigen, wie oft Ihre IAPs während ihrer gesamten Lebensdauer gekauft wurden. Zeigt den kumulierten Gesamtwert aller Käufe an (ab der ersten Veröffentlichung Ihrer App).

Das Diagramm enthält auch den Preis, den ein Kunde für das IAP entrichtet hat.

Optional können Sie die Ergebnisse nach Markt und/oder Betriebssystemversion filtern.

## Wichtigste IAPs


Das Diagramm **Wichtigste IAPs** zeigt die Gesamtanzahl von Käufen für jedes Ihrer IAPs im ausgewählten Zeitraum nach Markt an. Standardmäßig wird das IAP mit den meisten Käufen an oberster Stelle vor den IAPs mit weniger Käufen angezeigt. Sie können die Reihenfolge umkehren, indem Sie auf den Pfeil in der Spalte **Käufe** des Diagramms klicken.

## Märkte


Das Diagramm **Märkte** zeigt die Gesamtanzahl von IAP-Käufen im ausgewählten Zeitraum nach Markt. Standardmäßig wird der Markt mit den meisten Käufen an oberster Stelle vor den Märkten mit weniger Käufen angezeigt. Sie können die Reihenfolge umkehren, indem Sie auf den Pfeil in der Spalte **Käufe** des Diagramms klicken.

## Kundendemografie


Das Diagramm **Kundendemografie** zeigt demografische Informationen zu den Personen, die Ihre App erworben haben. Sie können sehen, wie viele Käufe (im ausgewählten Zeitraum) von Personen einer bestimmten Altersgruppe getätigt wurden und welches Geschlecht die Käufer hatten.

> **Hinweis** Einige Kunden haben festgelegt, dass sie diese Informationen nicht freigeben möchten. Falls die Altergruppe oder das Geschlecht nicht ermittelt werden konnten, wird der Kauf als **Unbekannt** kategorisiert.

## Betriebssystemversion


Im Diagramm **Betriebssystemversion** wird die Gesamtzahl der Käufe entsprechend dem Betriebssystem des Kunden angezeigt (bzw. über [großvolumige Käufe durch Organisationen](organizational-licensing.md)). In einigen Fällen können diese Informationen jedoch nicht ermittelt werden. Die Betriebssystemversion wird dann als **Unbekannt** angegeben.

 

 



<!--HONumber=Jun16_HO4-->


