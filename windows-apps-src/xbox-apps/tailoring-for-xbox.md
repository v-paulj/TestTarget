---
author: payzer
title: "Bewährte Methoden für Xbox"
description: "Hier erfahren Sie, wie Sie Ihre Anwendung für Xbox optimieren."
translationtype: Human Translation
ms.sourcegitcommit: 422ab09117ad25183f352992a7d21fe511a5f3bb
ms.openlocfilehash: 6198abcdde4a30df815ff9f36d062b8db3a37fab

---

# Bewährte Methoden für Xbox
Standardmäßig können UWP-Apps auf Xbox One ohne zusätzlichen Aufwand von Ihrer Seite ausgeführt werden. Wenn Sie Ihre Kunden allerdings mit Ihrer App beeindrucken möchten und mit den besten Apps auf Xbox konkurrieren möchten, sollten Sie die folgenden bewährten Methoden anwenden.
  > [!NOTE]
  > Bevor Sie beginnen, sehen Sie sich die Entwurfsrichtlinien in [Entwerfen für Xbox und Fernsehgeräte](../input-and-devices/designing-for-tv.md) an.   

## So erzielen Sie optimale Ergebnisse für Xbox One

### *Empfohlen:* Deaktivieren des Mausmodus
Xbox-Benutzer lieben ihren Controller. Deaktivieren Sie zur Optimierung der Controller-Eingabe den Mausmodus [Deaktivieren des Mausmodus](how-to-disable-mouse-mode.md) und ermöglichen Sie die direktionale Navigation (auch als [XY-Fokus](../input-and-devices/designing-for-tv.md#xy-focus-navigation-and-interaction) bezeichnet). Achten Sie auf Fokustrapping und auf eine nicht zugängliche Benutzeroberfläche.

### *Empfohlen:* Ziehen Sie ein Fokusrechteck, das für einen Abstand von 3m geeignet ist
Die meisten Xbox-Nutzer sitzen im Wohnzimmer am Fernseher. Denken Sie also daran, dass das Standard-Fokusrechteck aus 3m Entfernung schwer zu erkennen ist. Um sicherzustellen, dass das UI-Element mit dem Eingabefokus jederzeit deutlich für die Nutzer sichtbar ist, befolgen Sie die Richtlinien zur [Fokusanzeige](../input-and-devices/designing-for-tv.md#focus-visual). In XAML erhalten Sie dieses Verhalten automatisch, wenn Ihre App auf Xbox ausgeführt wird, aber für HTML-Apps ist eine benutzerdefinierte CSS-Formatvorlage erforderlich.

### *Empfohlen:* Integration in die SystemMediaTransportControls-Klasse 
Xbox-Benutzer möchten Medien-Apps mit der Xbox-Medienfernbedienung Cortana (insbesondere mit den Sprachbefehlen „Wiedergabe“ und „Anhalten“) und mit Xbox SmartGlass steuern. Diese Features erhalten Sie automatisch, wenn in Ihren Apps die [SystemMediaTransportControls](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.systemmediatransportcontrols.aspx)-Klasse verwendet wird, die automatisch in den Xbox-Mediensteuerelementen enthalten ist. Wenn in Ihrer App benutzerdefinierte Mediensteuerelemente verwendet werden, stellen Sie sicher, dass diese in die **SystemMediaTransportControls**-Klasse integriert werden, damit diese Features für Ihre Nutzer bereitstehen. Wenn Sie eine App mit Hintergrundmusik erstellen, integrieren Sie die **SystemMediaTransportControls**-Klasse, um sicherzustellen, dass die Hintergrundmusik-Steuerelemente in der Xbox-Multitasking-Registerkarte richtig funktionieren.

### *Empfohlen:* Berücksichtigung von angedockten Apps durch Verwendung einer adaptiven Benutzeroberfläche
Eines der einzigartigen Features der Xbox One ist, dass Apps wie Cortana neben einer anderen App angedockt werden können. Ihre App sollte daher ordnungsgemäß reagieren, wenn sie im *Füllmodus* ausgeführt wird. Implementieren Sie eine [adaptive Benutzeroberfläche](../get-started/universal-application-platform-guide.md#design-adaptive-ui-with-adaptive-panels), und testen Sie Ihre App in der Entwicklungsphase, indem Sie eine App neben ihr andocken.

### *Beachten:* Zeichnen bis zum Bildschirmrand
Viele Fernseher schneiden das Bild an den Bildschirmrändern ab. Daher sollte der wichtige Inhalt Ihrer App im [fernsehsicheren Bereich](../input-and-devices/designing-for-tv.md#tv-safe-area) angezeigt werden. UWP hält den Inhalt mit *Overscan* im fernsehsicheren Bereich, aber bei diesem Standardverhalten könnte ein sichtbarer Rahmen um die App gezeichnet werden. Sie erzielen optimale Ergebnisse, wenn Sie das Standardverhalten deaktivieren und die Anweisungen auf [So wird's gemacht - Zeichnen der Benutzeroberfläche bis zum Bildschirmrand](turn-off-overscan.md) befolgen.
> [!IMPORTANT]
  > Wenn Sie Overscan deaktivieren, müssen Sie selbst sicherstellen, dass interaktive Elemente und Text innerhalb des fernsehsicheren Bereichs bleiben. 

### *Beachten:* Verwenden von fernsehsicheren Farben 
Fernsehgeräte gehen mit hohen Farbintensitäten nicht so gut wie Computermonitore um. Vermeiden Sie in Ihren Apps Farben mit hoher Intensität, damit die Benutzer keine merkwürdigen Bandeffekte und keine verwaschenen Bilder sehen. Beachten Sie auch, dass Unterschiede zwischen Fernsehern dazu führen können, dass Farben, die auf *Ihrem* Fernseher gut aussehen, für Ihre Benutzer völlig anders aussehen können. Lesen Sie [TV-Farben](../input-and-devices/designing-for-tv.md#colors), um zu erfahren, wie Ihre App für alle toll aussieht.

### *Denken Sie daran:* Die Skalierung kann deaktiviert werden.
UWP-Apps werden automatisch skaliert, um sicherzustellen, dass Elemente der Benutzeroberfläche wie Steuerelemente und Schriftarten auf allen Geräten lesbar sind. XAML-Apps werden auf 200%, skaliert, während HTML-Apps auf 150% skaliert werden. Wenn Sie eine bessere Kontrolle über das Aussehen Ihrer App auf der Xbox möchten, deaktivieren Sie den Standardskalierungsfaktor und verwenden Sie die tatsächliche Pixelanzahl eines HDTV-Geräts (1920 x 1080). Sehen Sie sich [So wird's gemacht: Deaktivieren der Skalierung](disable-scaling.md) und [Effektive Pixel und Skalierung](../layout/design-and-ui-intro.md#effective-pixels-and-scaling) an, um Informationen zum Anpassen Ihrer App für die Xbox zu erhalten.

## Channel9
Die folgenden Gespräche auf [Channel 9](https://channel9.msdn.com/) sind eine hervorragende Informationsquelle für beeindruckende Apps auf Xbox:

- [Erstellen von großartigen UWP-Apps (Universelle Windows-Plattform) für Xbox](https://channel9.msdn.com/Events/Build/2016/B883)
- [Passen Sie Ihre App für die Xbox One und den Fernseher an](https://channel9.msdn.com/Events/Build/2016/T651-R1)
- [UWP-Entwicklung 1: Erstellen einer adaptiven Benutzeroberfläche](https://channel9.msdn.com/Events/Build/2016/L724-R1)
- [Web-Apps über den Browser hinaus: plattformübergreifende und geräteübergreifende Entwicklung](https://channel9.msdn.com/Events/Build/2016/B888)


## Weitere Informationen
- [UWP auf XboxOne](index.md)




<!--HONumber=Aug16_HO4-->


