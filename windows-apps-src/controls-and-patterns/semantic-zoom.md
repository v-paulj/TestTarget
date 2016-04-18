---
Description: Ein semantisches Zoomsteuerelement ermöglicht Benutzern, zwischen zwei verschiedenen semantischen Ansichten des gleichen Datensatzes zu zoomen.
title: Semantischer Zoom
ms.assetid: B5C21FE7-BA83-4940-9CC1-96F6A2DC28C7
label: Semantischer Zoom
template: detail.hbs
---

# Semantischer Zoom

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Mit dem Steuerelement für den semantischen Zoom können Benutzer zwischen zwei unterschiedlichen Ansichten desselben Inhalts zoomen, um schnell durch große Datenmengen zu navigieren. Die vergrößerte Ansicht ist die Hauptansicht des Inhalts. In dieser Ansicht werden die gesamten Daten angezeigt. Die verkleinerte Ansicht zeigt den gleichen Inhalt auf höherer Ebene. In dieser Ansicht werden normalerweise die Gruppenköpfe für gruppierte Daten angezeigt. Bei der Anzeige eines Adressbuchs kann der Benutzer beispielsweise einen Buchstaben vergrößern und die zu dem betreffenden Buchstaben gehörenden Namen einsehen. 

**Wichtige APIs**

-   [**SemanticZoom-Klasse**](https://msdn.microsoft.com/library/windows/apps/hh702601)

**Features**:

-   Die Größe der verkleinerten Ansicht ist durch die Grenzen des Steuerelements für semantischen Zoom beschränkt.
-   Durch Tippen auf einen Gruppenkopf wird zwischen Ansichten gewechselt. Auch Zusammendrücken kann als Möglichkeit zum Wechseln zwischen den Ansichten aktiviert werden.
-   Aktive Kopfzeilen wechseln zwischen Ansichten.

## Beispiele

In der Fotos-App verwendeter semantischer Zoom.

![In der Fotos-App verwendeter semantischer Zoom](images/control-examples/semantic-zoom-photos.png)

Ein Adressbuch ist ein Beispiel für einen Datensatz, der sich mit einem Steuerelement für semantisches Zoomen sehr viel einfacher navigieren lässt. Eine Ansicht enthält den vollständigen, alphanumerischen Überblick über die Kontakte im Adressbuch (linkes Bild), während die vergrößerte Ansicht die sortierten Daten mit mehr Details anzeigt (rechtes Bild).

![Beispiel für semantischen Zoom in einer Kontaktliste](images/semanticzoom-win10.png)

## Empfehlungen

-   Stellen Sie bei Verwendung des semantischen Zooms in Ihrer App sicher, dass sich das Elementlayout und die Verschieberichtung nicht basierend auf der Zoomstufe ändern. Layouts und Verschiebeinteraktionen sollten auf allen Zoomstufen konsistent und vorhersehbar sein.
-   Der semantische Zoom ermöglicht Benutzern das schnelle Wechseln zu Inhalten; beschränken Sie daher die Anzahl der Seiten oder Bildschirme in der verkleinerten Ansicht auf drei. Zu viel Verschiebung beeinträchtigt die praktische Nutzung des semantischen Zooms.
-   Vermeiden Sie das Verwenden des semantischen Zooms, um den Umfang der Inhalte zu ändern. So sollte beispielsweise ein Fotoalbum nicht zu einer Ordneransicht im Datei-Explorer wechseln.
-   Verwenden Sie eine Struktur und Semantik, die für die Ansicht wichtig sind.
-   Verwenden Sie Gruppennamen für Elemente in einer gruppierten Auflistung.
-   Verwenden Sie die Sortierreihenfolge für eine nicht gruppierte, aber sortierte Sammlung; sortieren Sie beispielsweise Datumsangaben chronologisch und Namenslisten alphabetisch.

\[Dieser Artikel enthält spezielle Informationen zu UWP-Apps (Universal Windows Platform) und Windows 10. Laden Sie für Windows 8.1 die [PDF-Datei mit Windows 8.1-Richtlinien](https://go.microsoft.com/fwlink/p/?linkid=258743) herunter.\]

## Verwandte Artikel

* [Richtlinien für häufige Benutzerinteraktionen](https://dev.windows.com/design/inputs-devices)


**Beispiele (XAML)**
* [Eingabe: Beispiel für XAML-Benutzereingabeereignisse](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Beispiel für XAML-Bildlauf, -Verschiebung und -Zoom](http://go.microsoft.com/fwlink/p/?linkid=251717)

**Beispiele (DirectX)**
* [Beispiel für die DirectX-Fingereingabe](http://go.microsoft.com/fwlink/p/?LinkID=231627)
* [Eingabe: Beispiel für Manipulationen und Gesten (C++)](http://go.microsoft.com/fwlink/p/?linkid=231605)
 

 






<!--HONumber=Mar16_HO1-->


