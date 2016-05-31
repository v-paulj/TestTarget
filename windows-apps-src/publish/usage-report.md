---
author: jnHs
Description: Der Bericht Nutzung im Windows Dev Center-Dashboard gibt Aufschluss darüber, wie Kunden Ihre App verwenden.
title: Bericht „Nutzung“
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
---

# Bericht „Nutzung“


> **Wichtig**  Der Bericht **Nutzung** enthält nur dann Daten, wenn Sie das [Visual Studio Application Insights SDK](http://go.microsoft.com/fwlink/?LinkId=615086) in Ihrer App (oder beim Erstellen Ihres Pakets das Kontrollkästchen „Telemetrie im Windows Dev Center anzeigen“) aktiviert haben. Bevor Daten in diesem Bericht angezeigt werden, müssen Sie die App außerdem im einheitlichen Dev Center-Dashboard einreichen und die App-Nutzungstelemetrie in Ihren Kontoeinstellungen aktivieren.

Der Bericht **Nutzung** im Windows Dev Center-Dashboard gibt Aufschluss darüber, wie Kunden Ihre App verwenden. Sie können diese Informationen in Ihrem Dashboard anzeigen oder [den Bericht herunterladen](download-analytic-reports.md), um die Daten offline anzuzeigen.

Dieser Bericht enthält allgemeine Informationen über die Nutzung Ihrer App. Wenn Sie dem Visual Studio Application Insights SDK für Ihre App ein Azure-Abonnement zugeordnet haben, erhalten Sie im Azure-Portal ausführlichere Telemetrieinformationen zur App-Nutzung. Im oberen Berichtsbereich finden Sie Links zu den App-Berichten im Azure-Portal.

Beachten Sie, dass das mit Ihrem Entwicklerkonto verknüpfte Microsoft-Konto auch mit dem für Application Insights verwendeten Azure-Abonnement verknüpft sein muss. Wenn Sie nicht an beiden Stellen das gleiche Microsoft-Konto verwenden, müssen Sie sich beim Azure-Verwaltungsportal anmelden und dann das mit Ihrem Entwicklerkonto verknüpfte Microsoft-Konto als Dienstadministrator, Co-Administrator oder Besitzer für dieses Abonnement hinzufügen.

## Anwenden von Filtern


Im oberen Seitenbereich können Sie das **Filter anwenden** erweitern, um alle Daten auf dieser Seite nach Datumsbereich und/oder Produktgruppe (zugehörigen Betriebssystemversionen) zu filtern.

-   **Datum**: Der Standardfilter lautet **Letzte 72 Stunden**, er kann jedoch bis auf **Letzte 12 Monate** erweitert werden.
-   **Produktgruppen**: Die Standardeinstellung lautet **Alle**. Wenn Ihre App mehr als eine Produktgruppe enthält, können Sie hier eine bestimmte auswählen.

Die Informationen in allen unten angezeigten Diagrammen beziehen sich auf den unter **Filter anwenden** ausgewählten Zeitraum. Standardmäßig gehören dazu Daten für alle unterstützten Produktgruppen, sofern Sie nicht mithilfe des Abschnitts **Filter anwenden** nach einer einzelnen Gruppe gefiltert haben.

## Gesamtanzahl der Benutzersitzungen


Das Diagramm **Gesamtanzahl der Benutzersitzungen** zeigt die Anzahl täglicher Benutzersitzungen für die App über den ausgewählten Zeitraum.

Jede Benutzersitzung steht für einen Kunden, der Ihre App startet und über einen Zeitraum verwendet. In diesem Diagramm werden Benutzer nicht einzeln nachverfolgt (d. h., mehrere hier angezeigte Benutzersitzungen können sich auf denselben Kunden beziehen).

## Aktive Benutzer


Das Diagramm **Aktive Benutzer** zeigt die Anzahl der Kunden, die Ihre App im ausgewählten Zeitraum an einem bestimmten Tag verwendet haben.

Jeder aktive Benutzer steht für einen Kunden, der Ihre App an diesem Tag verwendet hat. In diesem Diagramm werden Benutzersitzungen nicht einzeln nachverfolgt (d. h., ein Kunde wird in diesem Diagramm unabhängig davon dargestellt, ob er Ihre App an diesem Tag nur einmal oder mehrmals verwendet hat).

## Durchschnittliche Dauer der Benutzersitzung in Sekunden


Das Diagramm **Durchschnittliche Dauer der Benutzersitzung in Sekunden** zeigt, wie lange ein Kunde Ihre App im ausgewählten Zeitraum an einem bestimmten Tag durchschnittlich verwendet hat.

Sie können auch auf **Durchschnittliche Zeit zwischen Sitzungen in Sekunden** klicken. In diesem Fall wird im Diagramm die durchschnittliche Zeit zwischen getrennten Sitzungen, in denen Ihre App im ausgewählten Zeitraum verwendet wird, angezeigt.

## Benutzerdefinierte Ereignisse der letzten 30 Tage


Das Diagramm **Benutzerdefinierte Ereignisse der letzten 30 Tage** zeigt, wie häufig die für Ihre App definierten benutzerdefinierten Ereignisse insgesamt aufgetreten sind. Dazu können mehrere Vorkommen für denselben Kunden gehören.

## Seitenaufrufe in den letzten 30 Tagen


Das Diagramm **Seitenaufrufe in den letzten 30 Tagen** zeigt die Gesamtanzahl der Aufrufe bestimmter Seiten in Ihrer App. Dazu können mehrere Aufrufe desselben Kunden gehören.

 

 






<!--HONumber=May16_HO2-->


