---
author: jnHs
Description: "Sie können Werbecodes für Apps oder In-App-Produkte (IAP) generieren, die Sie im Windows Store veröffentlicht haben."
title: Generieren von Werbecodes
ms.assetid: 9B632266-64EC-4D62-A4C4-55B6643D8750
ms.sourcegitcommit: df994a3bbda9f6d4df0ee2dd6c2cb646d91a0bfc
ms.openlocfilehash: bfea802fb4a6efcfe34121802ce49f20a9b0305e

---

# Generieren von Werbecodes


Sie können Werbecodes für Apps oder In-App-Produkte (IAP) generieren, die Sie im Windows Store veröffentlicht haben. Werbecodes sind eine einfache Möglichkeit, einflussreichen Benutzern kostenlosen Zugriff auf Ihre App oder Ihr IAP zu ermöglichen. Sie können Werbecodes auch in Kundendienstszenarien verwenden, indem Sie Benutzern kostenlosen Zugriff auf Ihre App oder Ihr IAP gewähren, oder für [Betatests](beta-testing-and-targeted-distribution.md) mit Windows 10.

Jeder Werbecode hat eine entsprechende eindeutige einlösbare URL, die Sie an einen Benutzer verteilen können. Der Benutzer muss einfach auf die URL klicken, um den Code einzulösen und Ihre App oder Ihr IAP über den Windows Store zu installieren.

Auf dem Windows Dev Center-Dashboard können Sie folgende Aktionen ausführen:

-   Bestellen von Werbecodes für Ihre App
-   Herunterladen einer erfüllten Werbecodebestellung
-   Überprüfen der Werbecodenutzung für Ihre Apps, u. a.:
    -   Zusammenfassungen der Werbecodebestellungen für alle Ihre Apps (auf der Seite **Dashboard-Übersicht**) und für jede einzelne App (auf der Seite **App-Übersicht** für jede App).
    -   Eine ausführliche Zusammenfassung der Werbecodebestellungen für jede App (auf der Seite **Werbecodes** für jede App).

> **Hinweis**  Sie können Werbecodes auch dann generieren, wenn Sie auf der Dashboardseite [Preise und Verfügbarkeit](set-app-pricing-and-availability.md) für Ihre App die Option **Diese App ausblenden und den Erwerb verhindern. Kunden mit einem Werbecode können die App auf Windows 10-Geräten weiterhin herunterladen** ausgewählt haben. Ihre App muss die abschließende Veröffentlichungsphase des [App-Zertifizierungsprozesses](the-app-certification-process.md) bestehen, bevor Benutzer einen Werbecode zur Installation einlösen können.

## Richtlinien für Werbecodes


Beachten Sie die folgenden Richtlinien für Werbecodes:

-   Werbecodes können für alle über den Windows Store veröffentlichten Apps oder IAPs generiert werden. Benutzer können die Codes auf allen Versionen von Windows einlösen, die von Ihren Apps oder IAPs unterstützt werden.
-   Werbecodes laufen 6 Monate nach dem Datum der Bestellung ab.
-   Sie können für alle Apps oder IAPs alle 6 Monate bis zu 500 Werbecodes generieren. Der Zeitraum von 6 Monaten beginnt mit der Übermittlung der ersten Werbecodebestellung.
-   Sie müssen die in der [Vereinbarung für App-Entwickler](https://msdn.microsoft.com/library/windows/apps/hh694058) definierten Anforderungen erfüllen, einschließlich Abschnitt **3k. Werbecodes**.

## Bestellen von Werbecodes


So bestellen Sie Werbecodes für Apps oder IAPs, die Sie im Windows Store veröffentlicht haben

1.  Gehen Sie auf dem Windows Dev Center-Dashboard wie folgt vor:
    -   Navigieren Sie auf der Seite **App-Übersicht** für Ihre App zum Abschnitt **Werbecodes**, und klicken Sie auf **Codes bestellen**.
    -   Erweitern Sie auf einer beliebigen Dashboardseite für Ihre App im linken Navigationsmenü **Monetisierung**, und klicken Sie auf **Werbecodes**. Klicken Sie auf der Seite **Werbecodes** auf **Codes bestellen**.

2.  Geben Sie auf der Seite **Neue Werbecodes bestellen** Folgendes ein:
    -   Wählen Sie Apps oder IAPs, für die Sie Codes generieren möchten.
    -   Geben Sie einen Namen für die Bestellung an. Anhand dieses Namens können Sie beim Überprüfen der Werbecode-Nutzungsdaten zwischen verschiedenen Codebestellungen unterscheiden.
    -   Geben Sie die zu bestellenden Codemenge an.
    -   Geben Sie an, wann die Werbecodes aktiv werden sollen. Um ein spezifisches Startdatum und eine spezifische Startuhrzeit zu wählen, entfernen Sie die Markierung für das Kontrollkästchen **Codes werden umgehend aktiv**.
    -   Geben Sie an, wann die Werbecodes ablaufen sollen. Um ein spezifisches Startdatum und eine spezifische Startuhrzeit zu wählen, entfernen Sie die Markierung für das Kontrollkästchen **Codes laufen nach 6 Monaten ab**.

3.  Klicken Sie auf **Codes bestellen**. Die Bestellung wird übermittelt, und das Dashboard leitet Sie zur Seite **Werbecodes** weiter, auf der die neue Bestellung in der Zusammenfassungstabelle der Werbecodebestellungen als **Ausstehend** aufgeführt wird.

Werbecodes stehen in der Regel innerhalb von 60 Minuten nach Übermittlung der Bestellung zum Download zur Verfügung, wobei dies in einigen Fällen länger dauern kann. Wenn Ihre Bestellung erfüllt wurde und die Codes zum Download zur Verfügung stehen, wechselt der Bestellungsstatus zu **Verfügbar**.

## Herunterladen und Verteilen von Werbecodes


So laden Sie erfüllte Werbecodebestellungen herunter und verteilen die Codes an die Benutzer Ihrer App

1.  Kehren Sie auf dem Windows Dev Center-Dashboard zur Seite **Werbecodes** für Ihre App zurück (erweitern Sie **Monetarisierung**, und klicken Sie auf **Werbecodes**).
2.  Vergewissern Sie sich, dass die Bestellung den Status **Verfügbar** aufweist. Klicken Sie auf den Link zum **Herunterladen** Ihrer Bestellung, und speichern Sie die bereitgestellte Datei auf Ihrem Computer. Diese Datei enthält Informationen zu Ihrer Werbecodebestellung im TSV-Format (Tabulatorgetrennte Werte).
3.  Öffnen Sie die TSV-Datei im Editor Ihrer Wahl. Öffnen Sie für ein optimales Ergebnis die TSV-Datei in einer Anwendung, die Daten in tabellarischer Struktur anzeigen kann, z. B. Microsoft Excel. Allerdings können Sie die Datei auch in einem Text-Editor öffnen.

    Die Datei enthält Spalten mit den folgenden Daten für jeden Code:

    -   **Produktname**: Der Name der App oder des IAP, dem der Code zugeordnet ist.
    -   **Bestellungsname**: Der Name der Bestellung, in der dieser Code erfüllt wurde.
    -   **Werbecode**: Der Code selbst. Dies ist eine 5x5-Zeichenfolge von alphanumerischen Zeichen, die durch Bindestriche getrennt sind. Beispiel:

        DM3GY-M2GYM-6YMW6-4QHHT-23W2Z

    -   **Einlösbare URL**: Die URL, mit der ein Benutzer den Code einlösen und die App bzw. das IAP installieren kann. Die URL weist das folgende Format auf:

        https://account.microsoft.com/billing/redeem?mstoken=&lt;promotional_code>

    -   **Startdatum**: Das Datum, an dem dieser Code gültig wird.
    -   **Ablaufdatum**: Das Datum, an dem dieser Code abläuft.
    -   **Code-ID**: Eine eindeutige ID für diesen Code.
    -   **Bestellungs-ID**: Eine eindeutige ID für die Bestellung, in der dieser Code erfüllt wurde.
    -   **Ausgegeben an**: Ein leeres Feld, in das Sie einen Wert eintragen können, der den Benutzer identifiziert, an den Sie den Code verteilt haben.
    -   **Verfügbar**: Die Anzahl der Codes, die noch eingelöst werden können.
    -   **Eingelöst**: Die Anzahl der Codes, die eingelöst wurden.

4.  Sie können die einlösbaren URLs in einem beliebigen, von Ihnen bevorzugten Kommunikationsformat verteilen (z. B. E-Mail, SMS-Nachricht oder gedruckte Karten). Die Kommunikation sollte Folgendes enthalten:
    -   Eine Erklärung, für welche App bzw. welches IAP der Werbecode vorgesehen ist, und optional eine Beschreibung, warum der Benutzer den Code erhält.
    -   Die einlösbare URL für den Code.
    -   Anweisungen zum Aufrufen der einlösbaren URL, Anmelden mit dem Microsoft-Konto und Befolgen der Anweisungen zum Herunterladen und Installieren Ihrer App.

## Benutzererfahrung bei Einlösung des Codes


Wenn Sie eine einlösbare URL verteilt haben, wird in den folgenden Schritten die Benutzererfahrung beim Einlösen der App beschrieben.

1.  Der Benutzer klickt auf die einlösbare URL.

    Der Browser öffnet eine authentifizierte Seite **Code einlösen** unter <https://account.microsoft.com/billing/redeem>. Diese Seite enthält eine Beschreibung der App, für die der Benutzer den Code einlöst.

2.  Der Benutzer klickt auf **Einlösen**.

    Der Browser navigiert zur Seite **Danke** mit dem Link **Abrufen*****&lt;Name Ihrer App&gt;***.

    > **Hinweis**  Benutzern wird bei diesem Schritt eine Fehlermeldung angezeigt, wenn Ihre App noch nicht veröffentlicht wurde.

3.  Der Benutzer klickt auf **Abrufen*****&lt;Name Ihrer App&gt;***.

4.  Wenn sich der Benutzer an einem Computer mit Windows Store für Windows 10 oder Windows 8.1 befindet, wird die Übersichtsseite für die App im Windows Store geöffnet. Der Benutzer kann dann zum kostenlosen Installieren der App auf **Installieren** klicken.

    Wenn sich der Benutzer an einem Computer oder einem Gerät befindet, auf dem Windows Store nicht installiert ist, wird im Browser die Windows Store-Webseite für die App geöffnet. Der Benutzer kann zum kostenlosen Installieren der App auf **Installieren** klicken.

    > **Hinweis**  In einigen Fällen wird auf der App-Seite die Schaltfläche **Kaufen** anstelle der Schaltfläche **Installieren** angezeigt, obwohl die App erfolgreich über den Werbecode eingelöst wurde. Der Benutzer kann dann zum kostenlosen Installieren der App auf **Kaufen** klicken.

## Überprüfen der Werbecodes


Es gibt mehrere Möglichkeiten, die Nutzung von Werbecodes zu überprüfen.

-   Besuchen Sie zum Anzeigen einer Übersicht über die Werbecodebestellungen für alle Apps die Seite **Dashboard-Übersicht**, und navigieren Sie zum Abschnitt **Werbecodes** auf dieser Seite. In diesem Abschnitt werden die verbleibenden Werbecodes für alle Apps, die Gesamtanzahl von eingelösten Werbecodes für alle Apps und die Gesamtanzahl von Werbecodebestellungen angezeigt, die Sie für alle Ihre Apps aufgegeben haben.
-   Navigieren Sie zum Anzeigen einer Übersicht über die Werbecodebestellungen für eine bestimmte App zur Seite **App-Übersicht** für die App, und navigieren Sie zum Abschnitt **Werbecodes** auf dieser Seite. In diesem Abschnitt werden die verbleibenden aktiven Werbecodes für die App, die Gesamtanzahl von eingelösten Werbecodes für die App und die Gesamtanzahl von Werbecodebestellungen angezeigt, die Sie für die App aufgegeben haben.
-   Navigieren Sie zum Anzeigen einer ausführlichen Übersicht über die Werbecodebestellungen für eine bestimmte App zur Seite **Werbecodes** für Ihre App (Erweitern Sie **Monetarisierung**, und klicken Sie auf **Werbecodes**). Sie können die folgenden detaillierten Informationen für alle aktuellen und inaktiven Werbecodes für die App überprüfen:
    -   Bestellungsname
    -   Name der App oder des IAP
    -   Bestellungsdatum
    -   Ablaufdatum
    -   Status

Sie können auch eine aktive Bestellung aus dieser Tabelle herunterladen.

 

 







<!--HONumber=Jun16_HO4-->


