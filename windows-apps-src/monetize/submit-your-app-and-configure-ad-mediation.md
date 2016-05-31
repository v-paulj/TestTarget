---
author: mcleanbyron
ms.assetid: 69E05E56-B5F0-4D4C-A1FF-B6EAFF5D0E28
description: Während der Übermittlung können Sie das gewünschte Verhalten der Anzeigenvermittlung konfigurieren. Dieses können Sie später anpassen, ohne dazu Codeänderungen vorzunehmen oder neue Pakete zu übermitteln.
title: Übermitteln der App und Konfigurieren der Anzeigenvermittlung
---

# Übermitteln der App und Konfigurieren der Anzeigenvermittlung


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Sobald Sie Ihre App mit allen Anzeigennetzwerken, die Sie verwenden möchten, erstellt und ihre korrekte Funktion getestet haben, ist die App zur Übermittlung bereit. Während der Übermittlung können Sie das gewünschte Verhalten der Anzeigenvermittlung konfigurieren. Dieses können Sie später anpassen, ohne dazu Codeänderungen vorzunehmen oder neue Pakete zu übermitteln.

## Erstellen einer Basiskonfiguration für ein App-Paket


Wenn Sie Ihre Pakete hochladen, erkennt Dev Center automatisch, dass Sie die Anzeigenvermittlung verwenden, und identifiziert die von Ihnen verwendeten Anzeigennetzwerke. Auf der Seite **Pakete** sehen Sie einen Link **Anzeigenvermittlung konfigurieren**. Über diesen Link rufen Sie die Seite [Monetisierung durch Werbeanzeigen](https://msdn.microsoft.com/library/windows/apps/mt170658) auf, auf der Sie im Abschnitt **Windows-Anzeigenvermittlung** die Vermittlungslogik konfigurieren. Weitere Informationen zu den anderen Abschnitten auf dieser Seite finden Sie unter Monetisierung durch Werbeanzeigen.

Beim erstmaligen Konfigurieren der Anzeigenvermittlungseinstellungen für Ihre App erstellen Sie eine Basiskonfiguration. Nach dem Einrichten dieser Konfiguration können Sie marktspezifische Konfigurationen hinzufügen, um die Vorteile bestimmter Anzeigennetzwerke in verschiedenen Märkten zu nutzen. Wenn Sie die Einstellungen für die Anzeigenvermittlung nicht vor dem Übermitteln Ihrer App konfigurieren, wendet Dev Center automatisch [Standardeinstellungen](#default-ad-mediation-settings) an, und Sie können [diese Einstellungen später ändern](#optimize-ad-mediation-after-the-app-has-been-published).

Die folgenden Schritte beschreiben, wie Sie im Abschnitt **Windows-Anzeigenvermittlung** der Seite **Monetisierung durch Werbeanzeigen** eine Basiskonfiguration erstellen.

1.  Stellen Sie sicher, dass unter **Konfigurieren der Vermittlung für** das richtige App-Paket ausgewählt ist.
2.  Stellen Sie sicher, dass unter **Ziel** die Option **Baseline** ausgewählt ist.
3.  Wählen Sie unter **Aktualisierungsrate** die Länge des Vermittlungszyklus aus (die Häufigkeit, mit der neue Anzeigen eingeblendet werden). Die Dauer muss zwischen 30 und 120 Sekunden liegen.
  > **Hinweis**  Wenn Sie in einem Ihrer Anzeigennetzwerkportale bereits eine Aktualisierungsrate konfiguriert haben, achten Sie darauf, hier die gleiche Aktualisierungsrate festzulegen.

4.  Als Nächstes enthält der Abschnitt **Windows-Anzeigenvermittlung** alle von der App verwendeten Anzeigennetzwerke. Es werden zwei verschiedene Methoden bereitgestellt, um anzugeben, wie oft die einzelnen Anzeigennetzwerke von der App verwendet werden sollen. Wählen Sie eine der folgenden Optionen aus der Dropdownliste **Vermittlungstyp** aus:

    -   **Nach Gewicht sortieren**. Wählen Sie diese Option aus, um Prozentwerte auf die einzelnen Anzeigennetzwerke anzuwenden. Diese geben an, wie oft jedes Anzeigennetzwerk von der App verwendet werden soll. Die für alle Anzeigennetzwerke festgelegten Prozentsätze müssen insgesamt genau 100 % ergeben. Weitere Informationen finden Sie unter [Sortieren von Anzeigennetzwerken nach Gewicht](#order-ad-networks-by-weight).
    -   **Nach Rang sortieren**. Wählen Sie diese Option aus, um eine Rangfolge von 1 bis *n* auf die einzelnen Anzeigennetzwerke anzuwenden. Diese gibt an, wie oft jedes Anzeigennetzwerk von der App verwendet werden soll. Weitere Informationen finden Sie unter [Sortieren von Anzeigennetzwerken nach Rang](#order-ad-networkds-by-rank).

    Dev Center wendet automatisch [Standardeinstellungen](#default-ad-mediation-settings) an, die festlegen, wie oft die einzelnen Anzeigennetzwerke von der App verwendet werden sollen.

5.  Klicken Sie in der Liste der von der App verwendeten Anzeigennetzwerke auf den Dropdownpfeil jedes Anzeigennetzwerkanbieters, um die erforderlichen Parameter für die einzelnen Netzwerke anzuzeigen, und geben Sie die erforderlichen Parameter ein. Eine Liste der Parameter finden Sie unter [Auswählen und Verwalten der Anzeigennetzwerke](select-and-manage-your-ad-networks.md).

    In der erweiterten Parameterliste jedes Netzwerks können Sie optional das Feld **Zeitüberschreitung** verwenden, um die Anzahl von Sekunden (von 2 bis 120) anzugeben, die die Anzeigenvermittlung nach Anforderung einer Anzeige von diesem Anzeigennetzwerk warten soll, bevor die Anfrage abgebrochen und eine Anfrage bei einem anderen Netzwerk gestellt wird. Wenn dieser [Wert bereits im Code angegeben ist](add-and-use-the-ad-mediator-control.md#set-timeouts), wird der hier angegebene Wert durch den Wert im Code überschrieben.

    Bei Verwendung von Microsoft Advertising ist Folgendes zu beachten:

    -   Die Parameter für **Microsoft Advertising** werden basierend auf dem Inhalt des App-Pakets für Sie ausgefüllt. Für **Anwendungs-ID** und **ID der Anzeigeeinheit** können Sie optional eigene Werte für **Microsoft Advertising** angeben.
    -   Zusätzlich zu **Microsoft Advertising** gibt es eine Zeile für **Microsoft Advertising house ads**. Eigenwerbung ermöglicht das Erstellen kostenloser Anzeigen, die eine Ihrer Apps in einer Ihrer anderen Apps bewerben. Obwohl Eigenwerbung kostenlos ist, wird für die Zuteilung von Eigenwerbung derselbe Pool verwendet wie für andere Anzeigennetzwerke. Wenn Sie **Microsoft Advertising house ads** Anzeigenanforderungen zuordnen, nutzen Sie somit diesen Prozentsatz Ihres gesamten Anzeigenvorrats für Eigenwerbung. Wenn Sie Anzeigenanforderungen an Eigenwerbung zuordnen, müssen Sie auch eine Eigenwerbungskampagne für mindestens eine andere App erstellen, die unter Ihrem Entwicklerkonto registriert ist. Weitere Informationen finden Sie unter [Über Eigenwerbung](https://msdn.microsoft.com/library/windows/apps/mt148566).

6.  Klicken Sie auf **Speichern**, um die Basiskonfiguration zu speichern.

### Sortieren von Anzeigennetzwerken nach Gewicht

Wählen Sie eine dieser Optionen in der Dropdownliste **Vermittlungstyp** aus, um Prozentwerte anzugeben, die festlegen, wie oft jedes Anzeigennetzwerk von der App verwendet werden soll. Die für alle Anzeigennetzwerke festgelegten Prozentsätze müssen insgesamt genau 100 % ergeben.

Um Anforderungen automatisch gleichmäßig auf alle Netzwerke zu verteilen, klicken Sie auf **Anzeigenanforderungen gleichmäßig über alle aktiven Anzeigennetzwerke verteilen**. Wenn Sie Anforderungen ungleichmäßig verteilen möchten, legen Sie mithilfe der Schieberegler-Steuerelemente fest, wie oft jedes Anzeigennetzwerk von Ihrer App verwendet werden soll. Die für alle Anzeigennetzwerke festgelegten Prozentsätze müssen insgesamt genau 100 % ergeben. Bei der Verwendung der Schieberegler-Steuerelemente haben Sie verschiedene Optionen:

-   Wenn Sie den Schieberegler auf eine Zahl einstellen, gibt die Zahl den Prozentsatz der Zeit an, in der dieses Anzeigennetzwerk als erste Wahl der App in einem Vermittlungszyklus aufgerufen wird.
-   Wenn Sie den Schieberegler auf **Sicherung** einstellen, wird das Anzeigennetzwerk nur aufgerufen, wenn keines der Anzeigennetzwerke, für die ein Prozentsatz angegeben ist, eine Anzeige bereitstellen kann. Dies entspricht dem Festlegen eines Prozentsatzes von 0 %.
-   Wenn Sie den Schieberegler auf **Inaktiv** einstellen, wird das Anzeigennetzwerk nie aufgerufen. Die Assemblys für das Anzeigennetzwerk bleiben zwar im Paket, der Vermittler versucht aber nicht, sie aufzurufen. Sie können diese Option in marktspezifischen Konfigurationen verwenden, um Anzeigennetzwerke auszuschließen, die in bestimmten Märkten bekanntermaßen schlechte Leistung bieten oder diese nicht unterstützen.
-   Wenn Sie den Prozentsatz für ein Anzeigennetzwerk anpassen, werden alle anderen Schieberegler-Steuerelemente, mit denen Sie einen anderen Wert als **Sicherung** ausgewählt haben, automatisch so angepasst, dass die Gesamtverteilung 100 % beträgt. Wenn Sie das Kontrollkästchen **Sperren** für ein Anzeigennetzwerk aktivieren, wird der derzeit ausgewählte Wert für dieses Anzeigennetzwerk beibehalten. Sie können die Werte für die anderen Anzeigennetzwerke dann anpassen, ohne dass der Wert für das gesperrte Anzeigennetzwerk dadurch geändert wird.

### Sortieren von Anzeigennetzwerken nach Rang

Wählen Sie diese Option in der Dropdownliste **Vermittlungstyp** aus, um eine Rangfolge von 1 bis *n* auf die einzelnen Anzeigennetzwerke anzuwenden. Diese gibt an, wie oft jedes Anzeigennetzwerk von der App verwendet werden soll.

Wenn Sie das Kontrollkästchen **Aktiv** für ein Anzeigennetzwerk deaktivieren, wird das Anzeigennetzwerk nie aufgerufen. Die Assemblys für das Anzeigennetzwerk verbleiben zwar im Paket, aber der Vermittler versucht nicht, sie aufzurufen. Sie können diese Option in marktspezifischen Konfigurationen verwenden, um Anzeigennetzwerke auszuschließen, die in bestimmten Märkten bekanntermaßen schlechte Leistung erbringen oder von einem bestimmten Markt nicht unterstützt werden.

### Standardeinstellungen für die Anzeigenvermittlung

Dev Center wendet auf der Seite **Monetisierung durch Werbeanzeigen** standardmäßig die folgenden Anzeigenvermittlungseinstellungen auf Ihre App an. Diese Standardwerte werden auch angewendet, wenn Sie die Anzeigenvermittlung nicht vor dem Übermitteln der App konfigurieren.

-   Wenn die App Microsoft Advertising verwendet, ist **Microsoft Advertising** auf 100 (wenn Sie **Nach Gewicht sortieren** auswählen) oder auf 1 (wenn Sie **Nach Rang sortieren** auswählen) festgelegt, und alle anderen Anzeigennetzwerke sind deaktiviert.
-   Wenn die App Microsoft Advertising nicht verwendet, sind alle Anzeigennetzwerke deaktiviert.

## Erstellen einer neuen Zielkonfiguration


Nachdem Sie eine Basiskonfiguration für Ihre App erstellt haben, können Sie zusätzliche Konfigurationen für bestimmte Märkte erstellen. Diese neuen Konfigurationen erben ihre anfänglichen Einstellungen von der Basiskonfiguration, Sie können aber Details für die spezifischen Märkte anpassen, die Sie für Ihre App unterstützen.

So erstellen Sie eine neue Zielkonfiguration

1.  Wählen Sie unter **Ziel** den Markt aus, für den Sie die neue Konfiguration erstellen möchten.
2.  Ändern Sie die Aktualisierungsrate und die Informationen zur Verteilung der Anzeigenanforderung wie gewünscht.
3.  Klicken Sie auf **Speichern**, um die neue Konfiguration zu speichern.

## Optimieren der Anzeigenvermittlung nach der Veröffentlichung der App


Wenn Sie Ihre Anzeigenvermittlung für eine bestimmte App anpassen möchten, ist dies jederzeit möglich, ohne die App erneut zu übermitteln. Dies ist hilfreich, wenn Sie Ihrer App bereits Anzeigennetzwerke hinzugefügt haben, für die Sie noch keine Konten einrichtet hatten, oder wenn Sie feststellen, dass ein Anzeigennetzwerk in bestimmten Märkten Anzeigen nicht zuverlässig bereitstellen kann. Sie können Ihre Basiskonfiguration sowie marktspezifische Konfigurationen ändern. Sie können bei Bedarf auch marktspezifische Konfigurationen hinzufügen oder entfernen.

Führen Sie einen der folgenden Schritte aus, um zu den Anzeigenvermittlungseinstellungen für Ihre App zurückzukehren:

-   Klicken Sie im Dashboard auf Ihrer Kontoübersichtsseite im Abschnitt **Anzeigenvermittlung** auf Ihre App.
-   Erweitern Sie im Dashboard auf der Übersichtsseite für Ihre App **Monetisierung**, und klicken Sie dann auf **Monetisierung durch Werbeanzeigen**.

## Einstellungen für die Anzeigenvermittlung und App-Updates


Sofern die folgenden Bedingungen erfüllt sind, werden die vorhandenen Konfigurationsinformationen für die Anzeigenvermittlung Ihrer App automatisch auf das aktualisierte Paket angewendet, wenn Sie ein App-Update übermitteln:

-   Die Anzahl von **AdMediatorControl**-Steuerelementen ist mit der Anzahl von Steuerelementen im vorherigen App-Paket identisch, und alle Steuerelemente verfügen über die gleichen IDs wie im vorherigen App-Paket.
-   Die Liste der Anzeigennetzwerkanbieter ist mit der im vorherigen App-Paket identisch.

Wenn eine dieser Bedingungen nicht erfüllt ist, müssen Sie die Basiskonfiguration und alle marktspezifischen Zielkonfigurationen für Ihre App neu erstellen.

> **Hinweis**  Die ID eines **AdMediatorControl**-Steuerelements wird generiert, wenn Sie es auf eine Entwurfsoberfläche in Ihrer App ziehen. Diese ID wird nur geändert, wenn Sie das Steuerelement löschen und anschließend ersetzen, indem Sie ein neues Steuerelement auf die gleiche Oberfläche ziehen.

 

## Überprüfen von Anzeigenvermittlungsberichten im einheitlichen Dev Center-Dashboard


Wechseln Sie zum Überprüfen von Anzeigenvermittlungsberichten zum Abschnitt **Analyse** in Dev Center, und klicken Sie auf **Anzeigenvermittlung**. Weitere Informationen zu diesem Bericht finden Sie unter [Bericht „Anzeigenvermittlung“](https://msdn.microsoft.com/library/windows/apps/mt148521).

## Beispielszenarien


Sie können Ihre Anzeigennetzwerke auf unterschiedliche Arten konfigurieren, um verschiedene Ziele und Szenarien zu unterstützen. Die folgenden Beispiele zeigen, wie Sie die Anzeigenvermittlung mit **Nach Gewicht sortieren** konfigurieren können, wenn Sie drei Anzeigennetzwerke eingefügt haben. Wir verwenden Microsoft Advertising, Inneractive und AdDuplex als Beispielnetzwerke.

### Beispiel 1

Sie möchten die Füllrate und die Kosten pro tausend Ansichten (eCPM) für alle Netzwerke ermitteln, bevor Sie mit der Optimierung beginnen.

Legen Sie zunächst für jedes Netzwerk eine gleichmäßige Verteilung von Vermittlungsdatenverkehr fest. Sie können die Berichte der einzelnen Anzeigennetzwerk später überprüfen, um die Anzeigennetzwerke mit der besten Leistung für die einzelnen Märkten zu bestimmen.

Nach einigen Tagen oder Wochen sollten Sie die Füllrate und eCPM in allen Anzeigennetzwerk-Portalen überprüfen. So können Sie die besten Anzeigennetzwerke für jeden Markt ermitteln. Anschließend können Sie Anpassungen für bestimmte Märkte (oder insgesamt) vornehmen, ohne neue Pakete zu übermitteln.

### Beispiel 2

Sie möchten Microsoft Advertising nach Möglichkeit zuerst verwenden. Wenn Microsoft Advertising keine Anzeige bereitstellen kann, möchten Sie gerne eines Ihrer anderen Anzeigennetzwerke als Sicherung verwenden, wobei keines einem anderen vorgezogen wird.

| Anzeigennetzwerk            | Konfiguration |
|-----------------------|---------------|
| Microsoft             | 100 %          |
| Inneractive           | Sicherung        |
| AdDuplex              | Sicherung        |

### Beispiel 3

Im Allgemeinen möchten Sie Microsoft Advertising als Erstes verwenden. Wenn Microsoft Advertising keine Anzeige bereitstellen kann, sollten Sie sicherstellen, dass Inneractive vor AdDuplex aufgerufen wird.

| Anzeigennetzwerk            | Konfiguration |
|-----------------------|---------------|
| Microsoft             | 90 %           |
| Inneractive           | 10 %           |
| AdDuplex              | Sicherung        |

### Beispiel 4

Sie möchten Microsoft Advertising und Inneractive gleichermaßen verwenden, sodass Ihre App eine größere Auswahl von Anzeigen enthält, als dies bei nur einem Netzwerk der Fall wäre, das immer zuerst aufgerufen wird. Wenn keines dieser Anzeigennetzwerke verfügbar ist, verwenden Sie stattdessen AdDuplex.

| Anzeigennetzwerk            | Konfiguration |
|-----------------------|---------------|
| Microsoft             | 50 %           |
| Inneractive           | 50 %           |
| AdDuplex              | Sicherung        |


## Verwandte Themen

* [Auswählen und Verwalten der Anzeigennetzwerke](select-and-manage-your-ad-networks.md)
* [Hinzufügen und Verwenden des Steuerelements für die Anzeigenvermittlung](add-and-use-the-ad-mediator-control.md)
* [Testen der Implementierung der Anzeigenvermittlung](test-your-ad-mediation-implementation.md)
* [Problembehandlung bei der Anzeigenvermittlung](troubleshoot-ad-mediation.md)
 

 


<!--HONumber=May16_HO2-->


