---
author: jnHs
Description: "Das Massenverwalten Ihrer IAPs ermöglicht Ihnen, Änderungen für mehrere IAPs auf einmal durchzuführen, anstatt jedes Update einzeln übermitteln zu müssen."
title: IAPs massenverwalten
translationtype: Human Translation
ms.sourcegitcommit: 475371dd55aa111f3743c03dc1600e8cfdbeb5b0
ms.openlocfilehash: ae4d4ed33b9bd10a2b01b336c942ad3212de6533


---

# IAPs massenverwalten

> 
            **Wichtig** Dieses Feature ist zurzeit nur für Entwicklerkonten verfügbar, die Mitglied des [Dev Center-Windows-Insider-Programms](dev-center-insider-program.md) sind. Die Implementierung dieses Features wird möglicherweise geändert, bevor es für alle Entwickler verfügbar ist. Diese vorläufige Dokumentation enthält einige grundlegende Informationen zur Funktionsweise des Features.

Das Massenverwalten Ihrer IAPs ermöglicht Ihnen, Änderungen für mehrere IAPs auf einmal durchzuführen, anstatt jedes Update einzeln übermitteln zu müssen. Sie können auf der Übersichtsseite Ihrer App auf diese Funktionalität zugreifen, indem Sie auf **IAPs massenverwalten** klicken.

## Aktuelle IAP-Informationen exportieren

Zunächst müssen Sie eine Vorlagendatei im CSV-Format herunterladen, um die ersten Schritte zu machen. Wenn Sie bereits IAPs erstellt haben, enthält diese Datei Informationen über diese. Wenn dies nicht der Fall ist, ist die Datei leer, und Sie können diese verwenden, um Informationen für neue IAPs einzugeben. 

Um diese Vorlagendatei zu generieren und herunterzuladen, klicken Sie auf **IAPs exportieren** und speichern die CSV-Datei auf Ihrem Computer.

Die CSV-Datei enthält die folgenden Spalten. 

| Name der Spalte               | Beschreibung                            | Erforderlich?      |
|---------------------------|----------------------------------|----------------------|
| Produkt-ID    |  Die eindeutige [Produkt-ID](set-your-iap-product-id.md#product-id) des IAP.  | Ja. Kann nach der Veröffentlichung des IAP nicht geändert werden. |
| Aktion |Die Aktion, die Sie anwenden möchten, wenn Sie die Vorlage importieren. Unterstützte Werte sind **Submit** (um ein neues IAP zu übermitteln oder ein zuvor veröffentlichtes IAP zu aktualisieren) und **CreateDraft** (um die Änderungen zu speichern, ohne sie an den Store zu übermitteln). |    Ja |
| Produkttyp  | Der [Produkttyp](set-your-iap-product-id.md#product-type) des IAP. Unterstützte Werte sind **Konsumierbarer Artikel** oder **Gebrauchsgut**. | Ja. Kann nach der Veröffentlichung des IAP nicht geändert werden. |
| Produktlebensdauer  | Für ein langlebiges IAP ist dies entweder **Unbegrenzt** (für ein Produkt, das niemals abläuft) oder eine festgelegte Dauer. Zulässige Werte für die Dauer sind: **1day, 3days, 5days, 7days, 14days, 30days, 60days, 90days, 180days, 365days**.   | Ja (wenn der Produkttyp „Gebrauchsgut“ ist) |
| Inhaltstyp  | Der [Inhaltstyp](enter-iap-properties.md#content-type) des IAP. Für die Mehrzahl der IAPs sollte dies **ElectronicSoftwareDownload** sein. Weitere zulässige Werte sind: **ElectronicBooks, ElectronicMagazineSingleIssue, ElectronicNewspaperSingleIssue, MusicDownload, MusicStreaming, OnlineDataStorageServices, VideoDownload, VideoStreaming, SoftwareAsAService**. | Ja |
| Tag   | Optionale [Tag](enter-iap-properties.md#tag)informationen, die in der Implementierung Ihrer App verwendet werden. | Nein |
| Grundpreis    | Das [Preisniveau](set-iap-pricing-and-availability.md#base-price), auf dem Sie das IAP anbieten möchten. Dieses muss entweder **Kostenlos** oder ein gültiges Preisniveau im Format **0,99USD** sein. |   Ja |
| Datum der Veröffentlichung  | Das Datum, an dem Sie das IAP veröffentlichen möchten. Zulässige Werte sind **Direkt**, **Manuell** oder eine Datumszeichenfolge, die dem [ISO8601-Standard](http://go.microsoft.com/fwlink/p/?LinkId=817237) entspricht. | Ja |
| Titel    | Der Name des IAP, der Kunden angezeigt wird und dem der Sprachcode und ein Semikolon vorausgehen. Um beispielsweise den Titel „Beispieltitel“ in deutscher Sprache zu verwenden, würden Sie *de-de;Beispieltitel* eingeben. Zusätzliche Titel für weitere Sprachen können jeweils durch ein Semikolon getrennt werden. Jeder Titel darf maximal 100Zeichen enthalten.     | Ja |
|Beschreibungen   | Optionale zusätzliche Informationen, die Kunden angezeigt werden und denen der Gebietsschemacode der Sprache und ein Semikolon vorausgehen. Um beispielsweise die Beschreibung „Dies ist ein Beispiel.“ in deutscher Sprache zu verwenden, würden Sie *de-de;Dies ist ein Beispiel.* eingeben. Zusätzliche Titel für weitere Sprachen können jeweils durch ein Semikolon getrennt werden. Jede Beschreibung darf maximal 200Zeichen enthalten.    | Nein |
| Märkte | Ein oder mehrere [Märkte](define-pricing-and-market-selection.md#windows-store-consumer-markets), in dem Sie das IAP anbieten möchten. Trennen Sie die einzelnen Märkte jeweils durch ein Semikolon. | Ja |
|Schlüsselwörter | Optionale [Schlüsselwörter](enter-iap-properties.md#keywords), die in der Implementierung Ihrer App verwendet werden. | Nein |

## IAPs importieren

Bevor Sie Änderungen importieren können, müssen Sie die heruntergeladene CSV-Datei mit den Änderungen aktualisieren, die Sie durchführen möchten.

Um IAPs zu ändern, die Sie bereits veröffentlicht haben, aktualisieren Sie die gewünschten Werte in Ihrer Kopie der Tabelle. Sie können Zeilen für IAPs, die Sie nicht aktualisieren möchten, entfernen oder in der Tabelle belassen. Beachten Sie, dass Sie keine Änderungen mittels der CSV-Datei durchführen können, wenn bereits eine Übermittlung für das betreffende IAP bearbeitet wird.

> 
            **Wichtig** Wenn Sie Aktualisierungen für IAPs übermitteln, die Sie bereits veröffentlicht haben, können Sie die Felder **Produkt-ID** und **Produkttyp** nicht ändern.

Um ein neues IAP hinzuzufügen, fügen Sie eine neue Zeile ein und geben die Informationen für Ihr neues IAP ein. Achten Sie darauf, dass Sie alle erforderlichen Informationen eingeben. 

Wenn Sie alle Änderungen durchgeführt haben, speichern Sie die CSV-Datei (mit dem gleichen Dateinamen) und laden anschließend die Datei hoch, indem Sie sie auf das angegebene Feld ziehen (oder auf **Dateien durchsuchen** klicken). Anschließend wird eine Übersicht über die Änderungen zusammen mit allen Fehlern angezeigt, die vor der Übermittlung behoben werden müssen. Nachdem Sie überprüft haben, ob die Informationen korrekt sind, klicken Sie auf **An Store übermitteln**. Jedes IAP durchläuft den Übermittlungsvorgang auf der Basis der von Ihnen bereitgestellten Informationen.




<!--HONumber=Jul16_HO1-->


