---
author: jnHs
Description: "Bei der Arbeit mit einer App im Windows Dev Center-Dashboard können Sie Details zur eindeutigen Identität der App anzeigen, die ihr im Windows Store zugewiesen wurde, und einen Link zum Eintrag Ihrer App im Store abrufen."
title: "Anzeigen von Details zur App-Identität"
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
translationtype: Human Translation
ms.sourcegitcommit: a25d87556bb85718f818af5b586f54e6985aaaa4
ms.openlocfilehash: dc61971865a05e1de17cdcf55ab495fee4917b74

---

# Anzeigen von Details zur App-Identität


Bei der Arbeit mit einer App im Windows Dev Center-Dashboard können Sie Details zur eindeutigen Identität der App anzeigen, die ihr im Windows Store zugewiesen wurde, und einen Link zum Eintrag Ihrer App im Store abrufen.

Um diese Informationen zu suchen, navigieren Sie zu einer Ihrer Apps und erweitern im linken Navigationsmenü **App-Verwaltung**. Klicken Sie auf **App-Identität**, um diese Details anzuzeigen.

> **Hinweis:**  Sie benötigen einen [reservierten Namen](create-your-app-by-reserving-a-name.md) für Ihre App, um die meisten Identitätsdetails einzusehen.

## In das appx-Manifest einzuschließende Werte


Die folgenden Werte müssen im appx-Manifest enthalten sein. Wenn Sie Ihre Pakete mit Microsoft Visual Studio erstellen und mit demselben Microsoft-Konto angemeldet sind, das Sie mit Ihrem Entwicklerkonto verknüpft haben, werden diese Details automatisch eingefügt. Wenn Sie Ihr Paket manuell erstellen, müssen Sie diese selbst hinzufügen.

-   **Paket/Identität/Name**
-   **Paket/Identität/Herausgeber**

Weitere Informationen finden Sie unter [**Identity**](https://msdn.microsoft.com/library/windows/apps/br211441) in der [Paketmanifestschema-Referenz](https://msdn.microsoft.com/library/windows/apps/br211473).

Zusammen werden durch diese Elemente die Identität Ihrer App deklariert und die „Paketfamilie“ gebildet, der alle zugehörigen Pakete angehören. Einzelne Pakete verfügen über zusätzliche Details, z.B. die Architektur und Version.

## Zusätzliche Werte für die Paketfamilie


Die folgenden zusätzlichen Werte beziehen sich auf die Paketfamilie der App, werden aber nicht in Ihr Manifest eingeschlossen.

-   **Paketfamilienname (PFN)**: Dieser Wert wird bei bestimmten Windows-APIs verwendet.
-   **Paket-SID**: Sie benötigen diesen Wert, um WNS-Benachrichtigungen an Ihre App zu senden. Weitere Informationen finden Sie unter [Übersicht über den Windows-Pushbenachrichtigungsdienst (Windows Push Notification Service, WNS)](https://msdn.microsoft.com/library/windows/apps/mt187203).

## Erstellen eines Links zum Eintrag Ihrer App

Der Link zu Ihrer App-Seite kann geteilt werden, um Kunden das Auffinden der App im Store zu erleichtern. Dieser Link hat das Format **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

> **Hinweis:**  Diese URL funktioniert auf jeder Version des Betriebssystems, auf dem die App verfügbar ist. Es werden möglicherweise auch zusätzliche Links für Windows8.1 und frühere Versionen bzw. Windows Phone8.1 und frühere Versionen angezeigt, die nur auf den angegebenen Betriebssystemversionen funktionieren.

Wenn ein Kunde auf diesen Link klickt, wird die webbasierte Eintragsseite für Ihre App geöffnet. Wenn Ihre App für das Windows-Gerät des Kunden verfügbar ist, wird die Store-App gestartet und der App-Eintrag angezeigt.

Die **Store-ID** Ihrer App wird in diesem Abschnitt auch angezeigt. Diese Store-ID kann dazu verwendet werden, [Store-Badges zu generieren](http://go.microsoft.com/fwlink/p/?LinkId=534236) oder Ihre App anderweitig zu identifizieren.

 

 







<!--HONumber=Aug16_HO3-->


