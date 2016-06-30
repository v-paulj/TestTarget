---
author: Xansky
Description: "Listet die Praktiken auf, die Sie vermeiden sollten, wenn Sie eine barrierefreie UWP-App (Universelle Windows-Plattform) erstellen möchten."
ms.assetid: 024A9B70-9821-45BB-93F1-61C0B2ECF53E
title: "Nicht empfehlenswerte Praktiken für die Barrierefreiheit"
label: Accessibility practices to avoid
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: b5f5f220d5ff70d70dd797c0bf26a355bd447563

---
# Nicht empfehlenswerte Praktiken für die Barrierefreiheit



Listet die Praktiken auf, die Sie vermeiden sollten, wenn Sie eine barrierefreie UWP-App (Universelle Windows-Plattform) erstellen möchten.

* Vermeiden Sie die Erstellung benutzerdefinierter UI-Elemente, wenn Sie die Windows-Standardsteuerelemente oder Steuerelemente verwenden können, die bereits die Microsoft-Benutzeroberflächenautomatisierung unterstützen. Windows-Standardsteuerelemente sind standardmäßig barrierefrei. Es müssen lediglich einige Barrierefreiheitsattribute hinzugefügt werden, die für die App spezifisch sind. Im Gegensatz dazu ist das Implementieren der [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185)-Unterstützung für ein echtes benutzerdefiniertes Steuerelement etwas komplizierter (siehe [Benutzerdefinierte Automatisierungspeers](custom-automation-peers.md)).
* Verwenden Sie keinen statischen Text oder andere nicht interaktive Elemente in der Aktivierreihenfolge (beispielsweise durch das Festlegen einer [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) -Eigenschaft für ein Element, das nicht interaktiv ist). Nicht interaktive Elemente in der Aktivierreihenfolge verstoßen gegen die Barrierefreiheitsrichtlinien für Tastaturen, da sich durch diese die Effizienz bei der Tastaturnavigation für die Benutzer verringert. In vielen Hilfstechnologien werden die Aktivierreihenfolge und die Möglichkeit, den Fokus auf ein Element zu legen, als Teil der Logik für die Darstellung der App für Benutzer von Hilfstechnologien verwendet. Außerdem können dadurch Benutzer verwirrt werden, die ausschließlich interaktive Elemente in der Aktivierreihenfolge erwarten (Schaltflächen, Kontrollkästchen, Texteingabefelder, Kombinationsfelder, Listen usw.).
* Vermeiden Sie die absolute Positionierung von UI-Elementen (z. B. in einem [**Canvas**](https://msdn.microsoft.com/library/windows/apps/BR209267)-Element), da die Darstellungsreihenfolge häufig von der Deklarierungsreihenfolge der untergeordneten Elemente (der tatsächlichen logischen Reihenfolge) abweicht. Ordnen Sie, wenn möglich, UI-Elemente in Dokumentreihenfolge oder logischer Reihenfolge an, um sicherzustellen, dass Bildschirmleseprogramme diese Elemente in der richtigen Reihenfolge lesen können. Wenn die sichtbare Reihenfolge von UI-Elementen von der Dokumentreihenfolge oder der logischen Reihenfolge abweichen kann, verwenden Sie explizite Werte für die Aktivierreihenfolge (legen Sie [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461)fest), um die richtige Lesereihenfolge zu definieren.
* Vermeiden Sie es, Informationen nur durch den Einsatz von Farben zu transportieren. Benutzer, die farbenblind sind, können Informationen, die ausschließlich durch Farben vermittelt werden, beispielsweise in einer farbbasierten Statusanzeige, nicht wahrnehmen. Schließen Sie andere visuelle Hinweise (vorzugsweise Text) ein, um dafür zu sorgen, dass diese Informationen der Barrierefreiheit entsprechen.
* Aktualisieren Sie nicht die gesamte App-Oberfläche, sofern es nicht ausdrücklich für die App-Funktionalität notwendig ist. Wenn Sie Seiteninhalte automatisch aktualisieren müssen, aktualisieren Sie nur bestimmte Bereiche der Seite. Assistive technologies generally must assume that a refreshed app canvas is a totally new structure, even if the effective changes were minimal. Das bedeutet für Benutzer von Hilfstechnologie, dass jede Dokumentansicht oder Beschreibung der aktualisierten App jetzt neu erstellt und erneut für den Benutzer dargestellt werden muss.

> [!NOTE]
> Wenn Sie Inhalte in einem Bereich aktualisieren, können Sie die Barrierefreiheitseigenschaft [**AccessibilityProperties.LiveSetting**](https://msdn.microsoft.com/library/windows/apps/JJ191516) für dieses Element auf eine der nicht standardmäßigen Einstellungen **Polite** oder **Assertive** festlegen. Von einigen Hilfstechnologien kann diese Einstellung dem Accessible Rich Internet Applications (ARIA)-Konzept von Live-Regionen zugeordnet werden. Dadurch kann der Benutzer darüber informiert werden, dass sich ein Inhaltsbereich geändert hat.

* Eine bewusst vom Benutzer initiierte Seitennavigation ist ein berechtigter Fall zum Aktualisieren der App-Struktur. Das UI-Element, das die Navigation initiiert, muss aber korrekt identifiziert oder benannt sein, um darauf hinweisen zu können, dass ein Aufruf des UI-Elements zu einer Kontextänderung und einem Neuladen der Seite führt.
* Verwenden Sie keine UI-Elemente, die mehr als dreimal pro Sekunden blinken. Blinkende Elemente können bei einigen Menschen Anfälle auslösen. Am besten ist es, blinkende UI-Elemente vollständig zu vermeiden.
* Vermeiden Sie den automatischen Wechsel des Benutzerkontexts oder die automatische Aktivierung von Funktionalität. Kontext- oder Aktivierungsänderungen sollten nur erfolgen, wenn der Benutzer eine direkte Aktion mit einem Benutzeroberflächenelement ausführt, das den Fokus hat. Zu Änderungen des Benutzerkontexts zählen die Verlagerung des Fokus, die Anzeige neuer Inhalte und die Navigation zu einer anderen Seite. Kontextwechsel ohne Beteiligung des Benutzers können für Benutzer mit Behinderungen verwirrend sein. Ausnahmen von dieser Vorgabe sind das Anzeigen von Untermenüs, die Überprüfung von Formularen, das Anzeigen von Hilfetext in einem anderen Steuerelement sowie Kontextwechsel als Reaktion auf ein asynchrones Ereignis.

<span id="related_topics"/>
## Verwandte Themen  
* [Barrierefreiheit](accessibility.md)
* [Barrierefreiheit im Store](accessibility-in-the-store.md)
* [Prüfliste für die Barrierefreiheit](accessibility-checklist.md)



<!--HONumber=Jun16_HO4-->


