---
author: Karl-Bridge-Microsoft
Description: Ebenso wie Menschen untereinander mit einer Mischung aus Sprache und Gesten kommunizieren, kann sich auch bei der Interaktion mit einer App die Verwendung mehrerer Eingabearten und -modi als hilfreich erweisen.
title: "Entwurfsrichtlinien für mehrere Eingaben"
ms.assetid: 03EB5388-080F-467C-B272-C92BE00F2C69
label: Multiple inputs
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: d4238e0d1becee719148b38acf5a91d74230601d

---

# Mehrfacheingaben

Ebenso wie Menschen untereinander mit einer Mischung aus Sprache und Gesten kommunizieren, kann sich auch bei der Interaktion mit einer App die Verwendung mehrerer Eingabearten und -modi als hilfreich erweisen.


Um eine möglichst große Zahl von Benutzern und Geräten zu unterstützen, empfehlen wir, Ihre App für die Kompatibilität mit so vielen Eingabearten wie möglich zu entwickeln(Gesten, Spracherkennung, Toucheingabe, Touchpad, Maus und Tastatur). Dadurch werden Flexibilität, Benutzerfreundlichkeit und Barrierefreiheit optimiert.

Um zu beginnen, sollten Sie die verschiedenen Szenarien berücksichtigen, in denen Ihre App Eingaben verarbeitet. Versuchen Sie, in der gesamten App konsistent zu sein, und denken Sie daran, dass die Steuerelemente der Plattform integrierte Unterstützung für mehrere Eingabetypen bereitstellen.

-   Können Benutzer über mehrere Eingabegeräte mit der Anwendung interagieren?
-   Werden alle Eingabemethoden jederzeit unterstützt? Mit bestimmten Steuerelementen? Zu bestimmten Zeiten oder Umständen?
-   Hat eine Eingabemethode Priorität?

## Einzel- (oder Exklusiv)-Modusinteraktionen


Mit Einzelmodusinteraktionen werden mehrere Eingabetypen unterstützt, es kann jedoch nur einer pro Aktion verwendet werden. Z. B. Spracherkennung für Befehle und Gesten für die Navigation; oder die Texteingabe per Touch oder Gesten, je nach Näherung.

## Kombinierte Interaktionen


Mit kombinierten Interaktionen werden nacheinander mehrere Eingabemethoden verwendet, um eine einzelne Aktion auszuführen.

Spracherkennung + Geste  
Der Benutzer zeigt auf ein Produkt, und sagt dann „Dem Einkaufswagen hinzufügen“.

Spracherkennung + Toucheingabe  
Der Benutzer wählt ein Foto, indem er dieses gedrückt hält, und sagt dann „Foto senden“.






<!--HONumber=Aug16_HO3-->


