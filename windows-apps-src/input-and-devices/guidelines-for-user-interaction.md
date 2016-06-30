---
author: Karl-Bridge-Microsoft
Description: "Gestalten Sie die Benutzerinteraktion mit Ihren UWP-Apps (Universelle Windows-Plattform) intuitiv und unverwechselbar, und optimieren Sie diese für die Toucheingabe. Dabei sollte die Funktionalität jedoch für alle Eingabegeräte einheitlich sein."
title: "Richtlinien für die Toucheingabe"
ms.assetid: 3250F729-4FDD-4AD4-B856-B8BA575C3375
label: Touch design guidelines
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 67b851ce854c803934c2b97dbe7519e2916383a3

---

# Richtlinien für die Toucheingabe





Gestalten Sie die Benutzerinteraktion mit Ihren UWP-Apps (Universelle Windows-Plattform) intuitiv und unverwechselbar, und optimieren Sie diese für die Toucheingabe. Dabei sollte die Funktionalität jedoch für alle Eingabegeräte einheitlich sein.

## <span id="Dos_and_don_ts"></span><span id="dos_and_don_ts"></span><span id="DOS_AND_DON_TS"></span>Empfohlene und nicht empfohlene Vorgehensweisen


-   Entwerfen Sie Apps mit Toucheingabe als primär erwartete Eingabemethode.
-   Bieten Sie visuelles Feedback für alle Arten von Interaktionen (Berühren, Zeichenstift, Eingabestift, Maus usw.).
-   Optimieren Sie die Zielbestimmung, indem Sie die Größe des Toucheingabeziels, die Kontaktgeometrie, das Scrubbing und das Wackeln anpassen.
-   Optimieren Sie die Genauigkeit durch Verwendung von Andockpunkten und Führungsschienen.
-   Stellen Sie QuickInfos und Handles bereit, um die Genauigkeit der Toucheingabe für eng aneinanderliegende UI-Elemente zu verbessern.
-   Verwenden Sie nach Möglichkeit keine zeitlich festgelegten Interaktionen (Beispiel für eine richtige Verwendung: Berühren und Halten).
-   Verwenden Sie nach Möglichkeit nicht die Anzahl der zu verwendenden Finger, um zwischen den Manipulationen zu unterscheiden.

## <span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>Weitere Hinweise zur Verwendung


Am wichtigsten ist, dass Sie beim Entwerfen Ihrer App davon ausgehen, dass die Benutzer in erster Linie die Fingereingabe als Eingabemethode verwenden. Wenn Sie die Steuerelemente der Plattform verwenden, ist keine zusätzliche Programmierung für die Unterstützung von Touchpad, Maus und Zeichen-/Eingabestift erforderlich, da sie in Windows 8 standardmäßig bereitgestellt wird.

Bedenken Sie dabei jedoch, dass eine für Fingereingaben optimierte Benutzeroberfläche einer herkömmlichen Benutzeroberfläche nicht in jedem Fall überlegen ist. Beide haben Vor- und Nachteile, die je nach Technologie oder Anwendung unterschiedlich sein können. Sie müssen beim Übergang zu einer vorrangig auf Fingereingabe ausgelegten UI die wichtigsten Unterschiede zwischen Fingereingabe (einschließlich des Touchpads) und Eingabe über Maus, Zeichen- oder Eingabestift und Tastatur verstehen. Betrachten Sie die Eigenschaften vertrauter Eingabegeräte nicht als selbstverständlich, da die Fingereingabe in Windows 8 nicht einfach nur diese Funktionalität emuliert.

In diesen Richtlinien wird immer wieder erkennbar, dass die Fingereingabe einen anderen Ansatz für das UI-Design erfordert.

**Vergleich der Anforderungen für Fingereingabeinteraktionen**

In der folgenden Tabelle sind einige der Unterschiede zwischen den Eingabegeräten aufgeführt, die Sie berücksichtigen sollten, wenn Sie für die Fingereingabe optimierte Windows Store-Apps entwerfen.

Faktor Touchinteraktionen Interaktionen mit Maus, Tastatur, Zeichen-/Eingabestift Touchpadgenauigkeit Der Kontaktbereich einer Fingerspitze ist größer als eine einzelne XY-Koordinate. Hierdurch wird die Wahrscheinlichkeit größer, dass nicht beabsichtigte Befehlsaktivierungen erfolgen.
Maus und Zeichen- oder Eingabestift liefern eine präzise xy-Koordinate.
Identisch mit der Maus.
Die Form des Kontaktbereichs ändert sich während der Bewegung.
Mausbewegungen und Striche mit Zeichen- oder Eingabestift liefern eine präzise xy-Koordinate. Die Tastatur hat einen expliziten Fokus.
Identisch mit der Maus.
Es gibt keinen Mauscursor, der die Zielbestimmung unterstützt.
Der Fokus von Mauscursor, Zeichen- oder Eingabestiftcursor und Tastatur unterstützt die Zielbestimmung.
Identisch mit der Maus.
Menschliche Anatomie Bewegungen mit den Fingerspitzen sind ungenau, da es schwierig ist, mit einem oder mehreren Fingern eine lineare Bewegung auszuführen. Dies liegt an der Krümmung der Handgelenke und der Anzahl der an der Bewegung beteiligten Gelenke.
Es ist leichter, mit der Maus oder mit einem Zeichen- oder Eingabestift eine Bewegung in gerader Linie auszuführen, da die Hand, die diese Geräte steuert, eine kürzere physische Strecke zurücklegen muss als der Cursor auf dem Bildschirm.
Identisch mit der Maus.
Manche Bereiche der für die Fingereingabe vorgesehenen Oberfläche eines Anzeigegeräts sind aufgrund der Fingerhaltung und der Tatsache, dass der Benutzer das Gerät halten muss, möglicherweise schwer zu erreichen.
Maus und Zeichen- oder Eingabestift können alle Bereiche des Bildschirms erreichen, während über die Aktivierreihenfolge der Zugriff auf alle Steuerelemente möglich sein sollte.
Haltung und Druck der Finger spielen dabei eventuell auch eine Rolle.
Objekte werden möglicherweise durch mindestens eine Fingerspitze oder die Hand des Benutzers verdeckt. Dies wird als Okklusion bezeichnet.
Indirekte Eingabegeräte verursachen keine Okklusion.
Identisch mit der Maus.
Objektstatus Die Toucheingabe verwendet ein Modell mit zwei Zuständen: Die für die Toucheingabe vorgesehene Oberfläche eines Anzeigegeräts wird entweder berührt (ein) oder nicht berührt (aus). Es gibt keinen Zeigezustand, der zusätzliches visuelles Feedback auslösen kann.
Maus, Zeichen- oder Eingabestift und Tastatur machen ein Modell mit drei Zuständen verfügbar: oben (aus), unten (ein) und Zeigen (Fokus).

Mit Zeigen können Benutzer QuickInfos zu UI-Elementen erforschen und kennenlernen. Zeigen und Fokus können vermitteln, welche Objekte interaktiv sind, und außerdem die Zielbestimmung erleichtern.

Identisch mit der Maus.
Umfangreiche Interaktionen Unterstützt Multi-Touch: mehrere Eingabepunkte (Fingerspitzen) auf einer für die Toucheingabe vorgesehenen Oberfläche.
Unterstützt einen einzigen Eingabepunkt.
Identisch mit der Fingereingabe.
Unterstützt die direkte Manipulation von Objekten durch Bewegungen wie beispielsweise Tippen, Ziehen, Zusammendrücken und Drehen.
Keine Unterstützung für die direkte Manipulation, da Maus, Zeichen- oder Eingabestift und Tastatur indirekte Eingabegeräte sind.
Identisch mit der Maus.
 

**Hinweis**  
Die indirekte Eingabe hat den Vorteil, dass sie über 25 Jahre optimiert wurde. Features wie durch Zeigen ausgelöste QuickInfos wurden als Lösung für die Erforschung der UI speziell für die Eingabe über Touchpad, Maus, Zeichen- oder Eingabestift und Tastatur entworfen. Solche UI-Funktionen wurden neu entworfen, um der umfassenden Toucheingabefunktion gerecht zu werden, ohne die Benutzererfahrung auf den anderen Geräten zu beeinträchtigen.

 

**Verwenden von Feedback für die Fingereingabe**

Durch entsprechendes visuelles Feedback bei Interaktionen mit der App helfen Sie den Benutzern, zu erkennen und zu lernen, wie ihre Interaktionen von der App und von Windows 8 interpretiert werden, und sich daran anzupassen. Visuelles Feedback kann auf erfolgreiche Interaktionen hinweisen, über den Systemstatus informieren, das Gefühl der Kontrolle verstärken, Fehler verringern, Benutzern das Verständnis des Systems und des Eingabegeräts erleichtern und zu Interaktionen ermutigen.

Visuelles Feedback ist wichtig, wenn der Benutzer die Fingereingabe für Aktivitäten verwendet, bei denen Positionsgenauigkeit gefragt ist. Zeigen Sie immer Feedback an, wenn Toucheingabe erkannt wird, damit der Benutzer die von der App und den Steuerelementen definierten angepassten Zielbestimmungsregeln versteht.

**Erstellen immersiver Interaktionen**

Mit den folgenden Techniken können Sie die immersive Wahrnehmung von Windows Store-Apps verbessern.

**Zielbestimmung**

Die Zielbestimmung wird durch Folgendes optimiert:

-   Größe der Fingereingabeziele

    Klare Richtlinien für die Größe gewährleisten, dass Anwendungen eine angenehme UI bereitstellen, die Objekte und Steuerelemente enthält, die als Ziele einfach und sicher zu erreichen sind.

-   Kontaktgeometrie

    Der gesamte Kontaktbereich des Fingers wird verwendet, um das wahrscheinlichste Zielobjekt zu ermitteln.

-   Scrubbing

    Elemente in einer Gruppe können leicht erneut als Ziele bestimmt werden, indem der Finger zwischen ihnen gezogen wird (beispielsweise Optionsfelder). Das aktuelle Element wird aktiviert, wenn der Finger gehoben wird.

-   Wackeln

    Eng angeordnete Elemente (beispielsweise Hyperlinks) können leicht erneut als Ziel bestimmt werden, indem Sie mit dem Finger auf das Element drücken und ohne Ziehen mit dem Finger leicht über den Elementen hin- und herwackeln. Aufgrund von Okklusion wird das aktuelle Element durch eine QuickInfo oder die Statusleiste identifiziert und aktiviert, wenn der Finger gehoben wird.

**Genauigkeit**

Berücksichtigen Sie beim Design ungenaue Interaktionen, indem Sie Folgendes verwenden:

-   Andockpunkte, die bei Interaktionen der Benutzer mit dem Inhalt das Anhalten an der gewünschten Position erleichtern.
-   Führungsschienen für die Richtung, die beim vertikalen oder horizontalen Verschieben unterstützen, auch wenn die Hand in einem leichten Bogen bewegt wird. Weitere Informationen finden Sie unter [Richtlinien für Verschiebung](guidelines-for-panning.md).

**Okklusion**

Okklusion durch Finger und Hand können Sie durch Folgendes vermeiden:

-   Größe und Positionierung der UI

    Machen Sie die UI-Elemente groß genug, damit sie nicht vollständig von einem Fingerspitzen-Kontaktbereich verdeckt werden können.

    Positionieren Sie Menüs und Popups möglichst immer über dem Kontaktbereich.

-   QuickInfos

    Wenn der Benutzer den Finger auf einem Objekt liegen lässt, sollten QuickInfos angezeigt werden. Dies dient der Beschreibung der Objektfunktionen. Der Benutzer kann verhindern, dass die QickInfo angezeigt wird, wenn er die Fingerspitze von Objekt herunterzieht.

    Versetzen Sie bei kleinen Objekten die QuickInfos, damit sie nicht vom Fingerspitzen-Kontaktbereich verdeckt werden. Dies ist hilfreich für die Zielbestimmung.

-   Präzision durch Ziehpunkte

    Wenn Präzision erforderlich ist (beispielsweise bei der Textauswahl), stellen Sie versetzte Auswahlziehpunkte bereit, um die Genauigkeit zu verbessern. Weitere Informationen finden Sie unter [Richtlinien für die Text- und Bildauswahl (Windows-Runtime-Apps)](guidelines-for-textselection.md).

**Zeitliche Steuerung**

Vermeiden Sie zeitlich festgelegte Modusänderungen und verwenden Sie stattdessen direkte Manipulation. Bei der direkten Manipulation wird die direkte physische Handhabung eines Objekts in Echtzeit simuliert. Das Objekt reagiert, wenn die Finger bewegt werden.

Eine zeitlich festgelegte Interaktion dagegen tritt nach einer Fingereingabeinteraktion auf. Zeitlich festgelegte Interaktionen bestimmen normalerweise abhängig von unsichtbaren Schwellenwerten wie Zeit, Entfernung oder Geschwindigkeit, welcher Befehl ausgeführt werden soll. Bei zeitlich festgelegten Interaktionen erfolgt visuelles Feedback erst, wenn das System die Aktion ausführt.

Die direkte Manipulation bietet gegenüber zeitlich festgelegten Interaktionen eine Reihe von Vorteilen:

-   Sofortiges visuelles Feedback bei Interaktionen weckt die Aufmerksamkeit der Benutzer und vermittelt ihnen ein Gefühl von Sicherheit und Kontrolle.
-   Direkte Manipulationen sorgen für mehr Sicherheit beim Erforschen eines Systems, da sie umkehrbar sind, das heißt, Benutzer können ihre Aktionen leicht auf logische und intuitive Weise Schritt für Schritt rückgängig machen.
-   Interaktionen, die direkte Auswirkungen auf Objekte haben und reale Interaktionen nachahmen, sind intuitiver und leichter zu finden und zu merken. Sie sind nicht auf schwer verständliche oder abstrakte Interaktionen angewiesen.
-   Zeitlich festgelegte Interaktionen können schwer auszuführen sein, da die Benutzer willkürliche und unsichtbare Schwellenwerte erreichen müssen.

Darüber hinaus wird Folgendes dringend empfohlen:

-   Manipulationen sollten nicht anhand der Anzahl der verwendeten Finger unterschieden werden.
-   Die Interaktionen sollten zusammengesetzte Manipulationen unterstützen. Beispiel: Zoomen durch Zusammendrücken und gleichzeitiges Ziehen der Finger, um etwas zu verschieben.
-   Die Interaktionen sollten nicht anhand der Zeit unterschieden werden. Eine Interaktion sollte unabhängig von der Ausführungsdauer immer zum gleichen Ergebnis führen. Zeitbasierte Aktivierungen führen zu obligatorischen Verzögerungen für Benutzer und beeinträchtigen die immersive Natur der direkten Manipulation und die Wahrnehmung der Reaktion des Systems.

    **Hinweis**  Eine Ausnahme ist die Verwendung bestimmter zeitlich festgelegter Interaktionen zur Unterstützung beim Lernen und Erkunden (z. B. Drücken und Halten).

     

-   Entsprechende Beschreibungen und visuelle Hinweise haben einen großen Einfluss auf die Verwendung erweiterter Interaktionen.

## <span id="related_topics"></span>Verwandte Artikel

**Für Entwickler (XAML)**
* [Interaktionen per Toucheingabe](https://msdn.microsoft.com/library/windows/apps/mt185617)
* [Benutzerdefinierte Benutzerinteraktionen](https://msdn.microsoft.com/library/windows/apps/mt185599)
 

 







<!--HONumber=Jun16_HO4-->


