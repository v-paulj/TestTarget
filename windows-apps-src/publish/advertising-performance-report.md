---
author: jnHs
Description: "Performance-Daten für die Anzeigeneinheiten in Ihren Apps können Sie mithilfe der Berichte zur Anzeigen-Performance auf App- und Kontoebene im Windows Dev Center-Dashboard anzeigen."
title: Bericht zur Anzeigen-Performance
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
translationtype: Human Translation
ms.sourcegitcommit: 93e12837aec151b0cd1fa711c9e04081d74a3962
ms.openlocfilehash: 1617005cff264a89eb66e326e2bedf9f6641a3da

---

# Bericht zur Anzeigen-Performance


Zum Anzeigen von Performance-Daten für die Anzeigeneinheiten in Ihren Apps können Sie die folgenden Berichte im Windows Dev Center-Dashboard verwenden:

-   [Bericht zur Anzeigenleistung auf App-Ebene](advertising-performance-report.md#app-level-advertising-performance-report). Dieser Bericht enthält die Performance-Daten für die Microsoft-Anzeigeneinheiten in der aktuell im Dashboard ausgewählten App.
-   [Bericht zur Anzeigenleistung auf Kontoebene](advertising-performance-report.md#account-level-advertising-performance-report). Dieser Bericht enthält die detaillierten Performance-Daten für Microsoft-Anzeigeneinheiten und Community-Anzeigen für alle Apps, die in Ihrem Entwicklerkonto registriert sind.
-   [Bericht zur Anzeigenleistung auf Dashboardebene](advertising-performance-report.md#dashboard-level-advertising-performance-report). Dieser Bericht befindet sich auf der Seite **Dashboardübersicht** und enthält eine Zusammenfassung der Performance-Daten für Microsoft-Anzeigeneinheiten in allen Apps, die in Ihrem Entwicklerkonto registriert sind.

Standardmäßig werden die Berichte nach der Performance in den letzten 30 Tagen auf allen Geräten gefiltert. Klicken Sie zum Ändern dieser Filter auf **Seitenfilter**, und wählen Sie einen anderen Zeitrahmen oder einen individuellen Gerätetyp aus. 

> **Hinweis** Es gibt möglicherweise Unterschiede zwischen Leistungsberichten in Dev Center und pubCenter. Im Gegensatz zur Aggregation von pubCenter-Berichten erfolgt die Aggregation von Daten zur Anzeigenleistung in Dev Center auf der Grundlage von UTC und nicht Ihrer jeweiligen Zeitzone.

Die folgenden Abschnitte enthalten weitere Details zu diesen Berichten.

## Bericht zur Anzeigen-Performance auf App-Ebene

Diese Seite enthält Performance-Daten in Diagramm-, Weltkarten- und Tabellenform für die Microsoft-Anzeigeneinheiten in der aktuell im Dashboard ausgewählten App. Wählen Sie zum Anzeigen dieses Berichts eine Ihrer Apps im Dashboard aus, und klicken Sie im Navigationsbereich auf **Analyse**&gt;**Anzeigen-Performance**.

Die Daten werden aus den folgenden Leistungsmetriken abgerufen, die für die Anzeigen in Ihrer App nachverfolgt werden:

-   **Geschätzter Umsatz**: Die geschätzten Einnahmen, die Sie mit den Anzeigen in Ihrer App erzielen.
-   **eCPM**: Die effektiven Kosten pro tausend Anzeigenaufrufen.
-   **Anforderungen**: Die Anzahl der von Ihrer App gesendeten Anzeigenanforderungen.
-   **Aufrufe**: Gibt an, wie häufig eine Anzeige in Ihrer App angezeigt wurde.
-   **Füllrate**: Gibt den Prozentsatz der von Ihrer App gesendeten Anzeigenanforderungen an, bei denen eine Anzeige angezeigt wurde.
-   **Klicks**: Gibt an, wie häufig in Ihrer App auf eine Anzeige geklickt wurde.
-   **CTR** (Click-Through Rate): Gibt an, wie häufig auf eine Anzeige geklickt wurde, geteilt durch die Anzahl von Anzeigenaufrufen.

Weitere Informationen zu diesen Leistungsmetriken für die Anzeigeneinheiten in Ihrer App finden Sie in der Tabelle unter der Diagramm- und Kartenansicht.

Um die Daten für eine dieser Metriken in einer Diagramm- oder Weltkartenansicht zu analysieren, klicken Sie auf **Diagramm** oder **Karte**. Klicken Sie auf die Kopfzeilen oberhalb des Diagramms oder der Karte, um zwischen den verschiedenen Metriken zu wechseln. Standardmäßig werden in den Diagramm- und Kartenansichten Leistungsdaten für alle Anzeigeneinheiten in Ihrer App angezeigt. Sie können aber auf **Anzeigeneinheiten auswählen** klicken, um bis zu sechs einzelne Anzeigeneinheiten auszuwählen, die verglichen werden sollen.

In der Kartenansicht stellen dunklere Schattierungen höhere Werte und hellere Schattierungen niedrigere Werte dar. Sie können mit der Maus auf ein bestimmtes Land oder eine Region auf der Karte zeigen, um den Wert der ausgewählten Metrik zu analysieren. Sie können auch einen beliebigen Bereich der Karte vergrößern, um Daten für kleinere Länder anzuzeigen.

## Bericht zur Anzeigen-Performance auf Kontoebene

Diese Seite enthält die Performance-Daten für Microsoft-Anzeigeneinheiten und Community-Anzeigen in Apps, die in Ihrem Entwicklerkonto registriert sind. Navigieren Sie zum Anzeigen dieses Berichts zur Dashboardübersicht, und klicken Sie im Navigationsbereich auf **Anzeigen-Performance**.

Diese Seite umfasst folgende Abschnitte:

### Microsoft Advertising

Dieser Bericht enthält Performance-Daten für alle Microsoft-Anzeigeneinheiten, die in Ihren Apps verwendet werden. Darüber hinaus enthält er Performance-Daten für pubCenter-Anzeigeneinheiten ohne erfolgreiche Zuordnung zu Ihren Dev Center-Apps.

Dieser Bericht enthält die gleichen sieben Leistungsmetriken und Ansichten (Diagramm, Weltkarte und Tabelle) wie der oben beschriebene Bericht zur Anzeigen-Performance auf App-Ebene. Sie können auf diesen Bericht die folgenden Filter anwenden:

-   **Alle Anzeigeeinheiten**. Wenn Sie diesen Filter auswählen, können Sie Daten aus allen Anzeigeeinheiten oder bis zu sechs bestimmten Anzeigeeinheiten anzeigen.
-   **Alle Apps**. Wenn Sie diesen Filter auswählen, können Sie Daten aus allen Apps oder bis zu sechs bestimmten Apps anzeigen.
-   **Einzelne App**. Wenn Sie eine App auswählen, können Sie Daten aus allen von der App verwendeten Anzeigeneinheiten oder aus bis zu sechs bestimmten Anzeigeneinheiten anzeigen, die von der App verwendet werden.

Wenn Sie Anzeigeneinheiten für eine App mit Microsoft pubCenter erstellt haben, kann es vorkommen, dass nicht alle erfolgreich Ihren Apps in Dev Center zugeordnet wurden. In diesem Bericht werden diese Anzeigeeinheiten App-Namen zugeordnet, die Sie in pubCenter angegeben haben, wobei die Zeichenfolge **(pubCenter)** an den App-Namen angehängt wird.

Wenn Sie Anzeigeneinheiten für eine App mit pubCenter erstellt haben und auf keiner Berichtsseite Daten hierzu angezeigt werden, können Sie auf **pubCenter-Konto verknüpfen** klicken, um dieses Konto mit Ihrem Dev Center-Konto zu verknüpfen. Geben Sie die E-Mail-Adresse, die Sie diesem pubCenter-Konto zugeordnet haben, und Ihren Code für die Kontoverknüpfung ein. Sie erhalten den Code für die Kontoverknüpfung, indem Sie sich an diesem pubCenter-Konto anmelden und zur Seite **Meine Informationen** wechseln.

Weitere Informationen zum Migrieren von pubCenter-Konten zu Dev Center finden Sie unter [pubCenter-DevCenter-Integration](pubcenter-dev-center-integration.md).

### Microsoft Community-Anzeigen

Dieser Abschnitt enthält Performance-Daten in Diagramm- und Weltkartenform für Community-Anzeigen in der aktuell im Dashboard ausgewählten App. Weitere Informationen zu Community-Anzeigen finden Sie unter [Über Community-Anzeigen](about-community-ads.md).

Die Daten werden aus den folgenden Leistungsmetriken abgerufen, die für die Anzeigen in Ihrer App nachverfolgt werden:

-   **Anforderungen**: Die Anzahl der von Ihrer App gesendeten Community-Anzeigenanforderungen.
-   **Füllrate**: Gibt den Prozentsatz der von Ihrer App gesendeten Community-Anzeigenanforderungen an, bei denen eine Anzeige angezeigt wurde.
-   **Klicks**: Gibt an, wie häufig in Ihrer App auf eine Community-Anzeige geklickt wurde.
-   **CTR** (Click-Through-Rate): Gibt an, wie oft auf eine Community-Anzeige geklickt wurde (geteilt durch die Anzahl von Anzeigenaufrufen).
-   **Erworbenes Guthaben**: Gibt das für diese App erworbene Community-Anzeigenguthaben an. Ausführlichere Informationen zum Erwerb von Guthaben finden Sie unter [Über Community Anzeigen](about-community-ads.md).
-   **Beanspruchtes Guthaben**: Gibt das für diese App beanspruchte Community-Anzeigenguthaben an. Ausführlichere Informationen zur Beanspruchung von Guthaben finden Sie unter [Über Community Anzeigen](about-community-ads.md).

Klicken Sie auf **Diagramm** oder **Karte**, um die Daten für eine dieser Metriken in einer Diagramm- oder Weltkartenansicht zu analysieren. Klicken Sie auf die Kopfzeilen oberhalb des Diagramms oder der Karte, um zwischen den verschiedenen Metriken zu wechseln. In der Kartenansicht stellen dunklere Schattierungen höhere Werte und hellere Schattierungen niedrigere Werte dar. Sie können mit der Maus auf ein bestimmtes Land oder eine Region auf der Karte zeigen, um den Wert der ausgewählten Metrik zu analysieren. Sie können auch einen beliebigen Bereich der Karte vergrößern, um Daten für kleinere Länder anzuzeigen.

## Bericht zur Anzeigen-Performance auf Dashboardebene

Der Abschnitt **Anzeigen-Performance** auf der Seite **Dashboard-Übersicht** enthält eine Zusammenfassung der Performance-Daten für alle Microsoft-Anzeigeneinheiten, die in Ihren Apps verwendet werden. Dieser Bericht ist vergleichbar mit dem Abschnitt **Microsoft Advertising** des Berichts zur Anzeigen-Performance auf Kontoebene – allerdings mit folgenden Unterschieden:

-   Die Daten werden nur in Diagrammform bereitgestellt. Verwenden Sie den Bericht zur Anzeigenleistung auf Kontoebene, um die Daten in Tabellenform anzuzeigen.
-   Die bereitgestellten Filter sind für alle oder einzelne Apps. Verwenden Sie zum Filtern nach Anzeigeeinheiten den Bericht zur Anzeigen-Performance auf Kontoebene.


 

 



<!--HONumber=Jun16_HO4-->


