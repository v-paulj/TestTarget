---
ms.assetid: E0728EB0-DFC3-4203-A367-8997B16E2328
description: "In diesem Abschnitt wird erläutert, wie Sie Daten für UWP-Apps (Universelle Windows-Plattform) freigeben. Dabei geht es unter anderem um den Freigabe-Vertrag, das Kopieren und Einfügen und Drag & Drop."
title: App-zu-App-Kommunikation
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: dcd542257761083f3ec04eb0da2b13d5d68a19e2
ms.openlocfilehash: 63550064b6f31b85cd3b6fa2a09bac4b7cfcf895

---

# App-zu-App-Kommunikation

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

In diesem Abschnitt wird erläutert, wie Sie Daten für UWP-Apps (Universelle Windows-Plattform) freigeben. Dabei geht es unter anderem um den Freigabe-Vertrag, das Kopieren und Einfügen und Drag & Drop.

Der Freigabe-Vertrag ermöglicht Benutzern das schnelle Austauschen von Daten zwischen Apps. Ein Benutzer möchte beispielsweise mit einer App für ein soziales Netzwerk eine Webseite mit seinen Freunden teilen, oder er möchte in einer Notiz-App einen Link zum späteren Aufrufen speichern. Wenn Ihre App Inhalte in Szenarien empfängt, die ein Benutzer in einer anderen App schnell abschließen kann, sollten Sie einen Freigabe-Vertrag erwägen.

Eine App kann das Feature „Freigeben“ auf zwei unterschiedliche Arten unterstützen. Zunächst kann sie eine Quell-App sein, die vom Benutzer freizugebende Inhalte bereitstellt. Außerdem kann es eine Ziel-App geben, die vom Benutzer als Ziel für die freigegebenen Inhalte ausgewählt wird. Eine App kann sowohl Quell- als auch Ziel-App sein. Wenn Ihre App als Quell-App zum Teilen von Inhalten fungieren soll, müssen Sie festlegen, welche Datenformate die App bereitstellen kann.

Zusätzlich zum Freigabe-Vertrag können in Apps auch herkömmliche Verfahren zum Übertragen von Daten integriert sein, z. B. Drag & Drop und Kopieren und Einfügen. Neben der Kommunikation zwischen UWP-Apps unterstützen diese Methoden auch die Freigabe für Desktopanwendungen.

## Inhalt dieses Abschnitts

| Thema | Beschreibung |
|-------|-------------|
| [Freigeben von Daten](share-data.md) | In diesem Artikel wird erläutert, wie der Freigabe-Vertrag in einer UWP-App unterstützt wird. Der Freigabe-Vertrag ist eine einfache Möglichkeit, Daten wie z. B. Text, Links, Fotos und Videos schnell für andere Apps freizugeben. Ein Benutzer möchte beispielsweise mit einer App für ein soziales Netzwerk eine Webseite mit seinen Freunden teilen, oder er möchte in einer Notiz-App einen Link zum späteren Aufrufen speichern. |
| [Empfangen von Daten](receive-data.md) | In diesem Artikel wird erläutert, wie Sie in Ihrer UWP-App Inhalte empfangen, die in einer anderen App mithilfe des Freigabe-Vertrags freigegeben wurden. Mit diesem Freigabe-Vertrag kann Ihre App als Option angezeigt werden, wenn der Benutzer „Freigeben“ aufruft. |
| [Kopieren und Einfügen](copy-and-paste.md) | In diesem Artikel wird erläutert, wie das Kopieren und Einfügen mit der Zwischenablage in UWP-Apps unterstützt wird. Kopieren und Einfügen ist die klassische Methode zum Austausch von Daten zwischen Apps oder in einer App, und nahezu jede App kann Zwischenablageaktionen bis zu einem gewissen Grad unterstützen. |
| [Drag & Drop](drag-and-drop.md) | In diesem Artikel erfahren Sie, wie Sie Ihrer UWP-App Drag & Drop hinzufügen. Drag & Drop ist ein klassisches, natürliches Interaktionsmodell für Inhalte wie Bilder und Dateien. Nach der Implementierung stehen Drag & Drop-Vorgänge für sämtliche Richtungen (App zu App, App zu Desktop und Desktop zu App) zur Verfügung. |
| [Schützen von zwischen Apps übertragenen Unternehmensdaten mithilfe von EDP](use-edp-to-protect-enterprise-data-transferred-between-apps.md) | Dieses Thema enthält Beispiele für Programmieraufgaben, die in einigen der gängigsten EDP-Szenarien mit Datenübertragung durchgeführt werden müssen. |



<!--HONumber=Jun16_HO4-->


