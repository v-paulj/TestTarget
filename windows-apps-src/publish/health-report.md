---
author: jnHs
Description: Über den Bericht „Integrität“ im Windows Dev Center-Dashboard können Sie Daten zur Leistung und Qualität Ihrer App einschließlich App-Abstürzen abrufen.
title: Bericht „Integrität“
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
---

# Bericht „Integrität“


Über den Bericht **Integrität** im Windows Dev Center-Dashboard können Sie Daten zur Leistung und Qualität Ihrer App, einschließlich App-Abstürzen, abrufen. Sie können diese Informationen in Ihrem Dashboard anzeigen oder [den Bericht herunterladen](download-analytic-reports.md), um Ihre Daten offline anzuzeigen. Bei Bedarf können Sie Stapelüberwachungen für weitere Debugzwecke anzeigen. Sie können diese Daten jedoch auch programmgesteuert mit der [Windows Store-REST-API für Analysen](../monetize/access-analytics-data-using-windows-store-services.md) abrufen.

> **Hinweis**  Wenn Sie in den früheren Dashboards bereits Apps veröffentlicht und Leistungsdaten angezeigt haben, werden Sie möglicherweise feststellen, dass hier eine höhere Anzahl von Abstürzen und Ereignissen gemeldet wird. Das liegt daran, dass wir in diesem Bericht mehr Daten angeben können, sodass Sie ein vollständiges Bild erhalten.

## Anwenden von Filtern


Im oberen Seitenbereich können Sie **Filter anwenden** erweitern, um alle Daten auf dieser Seite nach Datumsbereich und/oder Paketversion zu filtern.

-   **Datum**: Der Standardfilter lautet **Letzte 72 Stunden**, er kann jedoch bis auf **Letzte 6 Monate** erweitert werden.
-   **Paketversion**: Die Standardeinstellung ist **Alle Versionen**. Wenn Ihre App mehr als eine Paketversion enthält, können Sie eine spezifische Version auswählen.

Die Informationen in allen unten angezeigten Diagrammen beziehen sich auf den im Abschnitt **Filter anwenden** ausgewählten Zeitraum. Standardmäßig gehören dazu Daten für alle Paketversionen, sofern Sie nicht mithilfe von **Filter anwenden** eine Version herausgefiltert haben.

## Gesamtanzahl der Abstürze und Ereignisse


Das Diagramm **Gesamtanzahl der Abstürze und Ereignisse** zeigt die Anzahl von täglichen Abstürzen und Ereignissen, die Kunden bei der Nutzung Ihrer App im ausgewählten Zeitraum festgestellt haben. Jeder Ereignistyp, der in der App aufgetreten ist, wird separat überwacht: Abstürze, Blockaden, JavaScript-Ausnahmen oder Speicherfehler.

Optional können Sie die Ergebnisse nach Markt und/oder Betriebssystemversion filtern.

## Aufschlüsselung der Abstürze und Ereignisse


Das Diagramm **Aufschlüsselung der Abstürze und Ereignisse** bietet Ihnen Einblicke in Diagramme, in denen bestimmte Details der Kundenkonfigurationen im Fall einer abgestürzten oder nicht reagierenden App nachverfolgt werden. Klicken Sie auf die Abschnittsüberschriften, um Details anzuzeigen:

-   Betriebssystemversion
-   Gerätetyp
-   Arbeitsspeicher (MB)
-   Massenspeicher (GB)
-   CPU-Geschwindigkeit (GHz)

Sie können die Ergebnisse optional nach **Absturztyp** filtern: Abstürze, Blockaden, JavaScript-Ausnahmen oder Speicherfehler. (Gemäß Standardeinstellung werden alle Absturztypen angezeigt.)

## Markt


Das Diagramm **Markt** zeigt die Gesamtanzahl von Abstürzen und Ereignissen im ausgewählten Zeitraum nach Markt an. Standardmäßig wird der Markt mit den meisten Käufen an oberster Stelle vor den Märkten mit weniger Käufen angezeigt. Sie können die Reihenfolge umkehren, indem Sie auf den Pfeil in der Spalte **Abstürze** des Diagramms klicken.

## Fehlerprotokoll


Wenn Sie PDB-Symboldateien eingefügt haben, enthält das Diagramm **Fehlerprotokoll** Details zum Vorkommen bestimmter Symbole, darunter die Gesamtanzahl von Abstürzen und die durchschnittlichen täglichen Abstürze pro Symbol.

Diese Informationen basieren auf dem Prozentsatz der Gesamtereignisse. Im oberen Bereich des Diagramms geben wir an, wie viel Prozent der stichprobenartig geprüften Ereignisse zur Bereitstellung dieser Daten verwendet wurden.

 

 


<!--HONumber=May16_HO2-->


