---
author: jnHs
Description: "Der Bericht „Nutzung“ im Windows Dev Center-Dashboard gibt Aufschluss darüber, wie Kunden Ihre App verwenden."
title: "Bericht „Nutzung“"
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
translationtype: Human Translation
ms.sourcegitcommit: 056642044953bab02f78912c7611ddcf5d6d48e6
ms.openlocfilehash: 476e7ee0c9c7ea7dce7f5e3a0389091ede9132c4

---

# Bericht „Nutzung“


Im Bericht **Nutzung** im Windows Dev Center-Dashboard erfahren Sie, wie Kunden Ihre App verwenden, und erhalten Informationen zu den von Ihnen definierten benutzerdefinierten Ereignissen. Sie können diese Daten in Ihrem Dashboard anzeigen oder den [Bericht herunterladen](download-analytic-reports.md), um ihn offline anzuzeigen.

> **Hinweis** Zuvor wurden vom Bericht **Nutzung** nur Daten bereitgestellt, wenn Sie das Visual Studio Application Insights SDK in Ihrer App aktiviert hatten. Beim aktualisierten Bericht **Nutzung** ist dies nicht mehr erforderlich.

## Anwenden von Filtern


Im oberen Seitenbereich können Sie **Filter anwenden** erweitern, um alle Daten auf dieser Seite nach Datumsbereich und/oder Produktgruppe (zugehörigen Betriebssystemversionen) zu filtern.

-   **Datum**: Der Standardfilter lautet **Letzte 30 Tage**, er kann jedoch bis auf **Letzte 12 Monate** erweitert werden.
-   **Paketversion**: Die Standardeinstellung ist **Alle**. Wenn Ihre App mehr als ein Paket enthält, können Sie hier ein bestimmtes auswählen.
-   **Gerätetyp**: Die Standardeinstellung ist **Alle**, Sie können jedoch festlegen, dass nur Daten für einen bestimmten Gerätetyp angezeigt werden.

Die Informationen in allen unten angezeigten Diagrammen beziehen sich auf den unter **Filter anwenden** ausgewählten Zeitraum. Standardmäßig gehören dazu Daten für alle Paketversionen und unterstützten Gerätetypen, sofern Sie nicht im Abschnitt **Filter anwenden** nach einer einzelnen Gruppe gefiltert haben.

## Gesamtanzahl der Benutzersitzungen

Das Diagramm **Gesamtanzahl der Benutzersitzungen** zeigt die Anzahl täglicher Benutzersitzungen für die App über den ausgewählten Zeitraum.

Jede Benutzersitzung stellt einen bestimmten Zeitraum dar, in dem ein Kunde mit der App interagiert hat. Da davon ausgegangen wird, dass jede Benutzersitzung nach einer Zeit der Inaktivität endet, kann ein einzelner Kunde am selben Tag mehrere Benutzersitzungen führen. Beachten Sie, dass in diesem Diagramm keine einzelnen App-Benutzer nachverfolgt werden.

## Aktive Benutzer

Das Diagramm **Aktive Benutzer** zeigt die Anzahl der Kunden, die Ihre App im ausgewählten Zeitraum an einem bestimmten Tag verwendet haben.

Jeder aktive Benutzer steht für einen Kunden, der Ihre App an diesem Tag verwendet hat. In diesem Diagramm werden Benutzersitzungen nicht einzeln nachverfolgt (d. h., ein Kunde wird in diesem Diagramm unabhängig davon dargestellt, ob er Ihre App an diesem Tag nur einmal oder mehrmals verwendet hat).

## Benutzerdefinierte Ereignisse

Das Diagramm **Benutzerdefinierte Ereignisse** zeigt, wie häufig die für Ihre App definierten benutzerdefinierten Ereignisse insgesamt aufgetreten sind. Dazu können mehrere Vorkommen für denselben Kunden gehören.

Benutzerdefinierte Ereignisse werden unter Verwendung der [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx)-Methode im [Microsoft Store Engagement and Monetization SDK](../monetize/monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md) implementiert.



 







<!--HONumber=Jun16_HO4-->


