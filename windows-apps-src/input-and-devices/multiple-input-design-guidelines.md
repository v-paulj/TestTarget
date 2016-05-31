---
author: Karl-Bridge-Microsoft
Description: Ebenso wie Menschen untereinander mit einer Mischung aus Sprache und Gesten kommunizieren, kann sich auch bei der Interaktion mit einer App die Verwendung mehrerer Eingabearten und -modi als hilfreich erweisen.
title: Entwurfsrichtlinien für mehrere Eingaben
ms.assetid: 03EB5388-080F-467C-B272-C92BE00F2C69
label: Multiple inputs
template: detail.hbs
---

# Mehrere Eingaben

Ebenso wie Menschen untereinander mit einer Mischung aus Sprache und Gesten kommunizieren, kann sich auch bei der Interaktion mit einer App die Verwendung mehrerer Eingabearten und -modi als hilfreich erweisen.


Durch die Berücksichtigung einer möglichst großen Anzahl von Benutzern, Geräten und Eingabearten (Gesten, Spracherkennung, Toucheingabe, Touchpad, Maus und Tastatur) maximieren Sie die Flexibilität, Benutzerfreundlichkeit und Barrierefreiheit Ihrer Apps. Dadurch wird die Flexibilität, Benutzerfreundlichkeit und Barrierefreiheit optimiert.

Um zu beginnen, sollten Sie die verschiedenen Szenarien berücksichtigen, in denen Ihre App Eingaben verarbeitet. Versuchen Sie, in der gesamten App konsistent zu sein, und denken Sie daran, dass die Steuerelemente der Plattform integrierte Unterstützung für mehrere Eingabetypen bereitstellen.

-   Können Benutzer über mehrere Eingabegeräte mit der Anwendung interagieren?
-   Werden alle Eingabemethoden jederzeit unterstützt? Mit bestimmten Steuerelementen? Zu bestimmten Zeiten oder Umständen?
-   Hat eine Eingabemethode Priorität?

## <span id="Single__or_exclusive_-mode_interactions_"></span><span id="single__or_exclusive_-mode_interactions_"></span><span id="SINGLE__OR_EXCLUSIVE_-MODE_INTERACTIONS_"></span>Einzel- (oder Exklusiv)-Modusinteraktionen


Mit Einzelmodusinteraktionen werden mehrere Eingabetypen unterstützt, es kann jedoch nur einer pro Aktion verwendet werden. Z. B. Spracherkennung für Befehle und Gesten für die Navigation; oder die Texteingabe per Touch oder Gesten, je nach Näherung.

## <span id="Multimodal_interactions"></span><span id="multimodal_interactions"></span><span id="MULTIMODAL_INTERACTIONS"></span>Kombinierte Interaktionen


Mit kombinierten Interaktionen werden nacheinander mehrere Eingabemethoden verwendet, um eine einzelne Aktion auszuführen.

<span id="Speech___gesture"></span><span id="speech___gesture"></span><span id="SPEECH___GESTURE"></span>Spracherkennung + Geste  
Der Benutzer zeigt auf ein Produkt, und sagt dann „Dem Einkaufswagen hinzufügen“.

<span id="Speech___touch"></span><span id="speech___touch"></span><span id="SPEECH___TOUCH"></span>Spracherkennung + Toucheingabe  
Der Benutzer wählt ein Foto, indem er dieses gedrückt hält, und sagt dann „Foto senden“.





<!--HONumber=May16_HO2-->


