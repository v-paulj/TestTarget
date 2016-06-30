---
author: Karl-Bridge-Microsoft
Description: "Benutzerinteraktionen in der Universellen Windows-Plattform (UWP) stellen eine Kombination von Eingabe- und Ausgabequellen dar (z. B. Maus, Tastatur, Stift, Toucheingabe, Touchpad, Spracherkennung, Cortana, Controller, Gesten, Mimik usw.), neben verschiedenen Modi oder Modifizierern, die erweiterte Funktionen ermöglichen (u. a. Mausrad und -tasten, Radierer- und Zeichenstift-Schaltflächen, Bildschirmtastatur sowie App-Dienste im Hintergrund)."
title: "Einführung in die Interaktion"
ms.assetid: 73008F80-FE62-457D-BAEC-412ED6BAB0C8
label: Interaction primer
template: detail.hbs
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: 068bb7d0c7af4b55ab10a955c563988f5123e5eb

---

# Einführung in die Interaktion


![Windows-Eingabetypen](images/input-interactions/icons-inputdevices03.png)

Benutzerinteraktionen in der Universellen Windows-Plattform (UWP) stellen eine Kombination von Eingabe- und Ausgabequellen dar (z. B. Maus, Tastatur, Stift, Toucheingabe, Touchpad, Spracherkennung, **Cortana**, Controller, Gesten, Mimik usw.), neben verschiedenen Modi oder Modifizierern, die erweiterte Funktionen ermöglichen (u. a. Mausrad und -tasten, Radierer- und Zeichenstift-Schaltflächen, Bildschirmtastatur sowie App-Dienste im Hintergrund).

Die UWP verwendet ein intelligentes, kontextbezogenes Interaktionssystem, mit dem in den meisten Fällen die von der App empfangenen eindeutigen Eingabetypen nicht individuell behandelt werden müssen. Dazu zählt das Behandeln von Toucheingabe, Touchpad, Maus und Stifteingabe als generischer Zeigertyp, um statische Gesten wie Tippen oder Gedrückthalten und Manipulationsgesten wie Ziehen zum Verschieben oder zum Rendern von Freihandeingabe zu unterstützen.

Machen Sie sich mit einzelnen Typen von Eingabegeräten, dem jeweiligen Verhalten, den Möglichkeiten und Beschränkungen in Verbindung mit gewissen Formfaktoren vertraut. Dies erleichtert Ihnen die Entscheidung, ob die Steuerelemente und Angebote der Plattform für die App ausreichend sind oder ob Sie angepasste Funktionen für die Benutzerinteraktion bereitstellen müssen.

## Cortana


In Windows 10 können Sie mit der Erweiterung von **Cortana** Sprachbefehle von einem Benutzer behandeln und die Anwendung zum Ausführen einer einzelnen Aktion starten.

Unterstützung von Geräten
-   Smartphones und Phablets
-   Tablet
-   PCs und Laptops
-   Surface Hub
-   IoT
-   Xbox
-   HoloLens

![Cortana](images/input-interactions/icons-cortana01.png)

Typische Nutzung Ein Sprachbefehl ist eine einzelne, in einer Sprachbefehldefinitions-Datei (Voice Command Definition (VCD)-Datei) definierte Äußerung, die über **Cortana** an eine installierte App weitergeleitet wird. Die App kann im Vordergrund oder im Hintergrund gestartet werden, je nach Ebene und Komplexität der Interaktion. Sprachbefehle, die zusätzlichen Kontext oder Benutzereingaben erfordern, werden beispielsweise am besten im Vordergrund ausgeführt, während grundlegende Befehle im Hintergrund behandelt werden können.

**Cortana** integriert die grundlegenden Funktionen Ihrer App, bietet einen zentralen Einstiegspunkt, über den der Benutzer die meisten Aufgaben ohne Öffnen der App ausführen kann, und wird somit zum Bindeglied zwischen Ihrer App und dem Benutzer. In vielen Fällen spart der Benutzer dadurch viel Zeit und Mühe. Weitere Informationen finden Sie unter [Cortana-Entwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/dn974233).

Weitere Informationen finden Sie unter [Cortana-Entwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/dn974233).
 

## Spracherkennung


Die Spracherkennung ist eine effektive und natürliche Möglichkeit, mit Apps zu kommunizieren. Sie bietet eine unkomplizierte und präzise Möglichkeit der Kommunikation mit Apps; damit können Benutzer in einer Vielzahl von Situationen produktiv arbeiten und auf dem Laufenden bleiben.

Die Spracherkennung kann den Haupteingabetyp je nach Gerät in vielen Fällen ergänzen oder sogar an dessen Stelle treten. Beispielsweise unterstützen Geräte wie HoloLens und Xbox keine herkömmlichen Eingabetypen (abgesehen von einer Softwaretastatur in bestimmten Szenarios). Stattdessen verwenden sie für die meisten Benutzerinteraktionen Spracheingabe und -ausgabe (häufig zusammen mit anderen modernen Eingabetypen wie Gestik und Mimik).

Mit Text-zu-Sprache (auch als TTS oder Sprachsynthese bezeichnet) werden Informationen oder Anweisungen an den Benutzer ausgegeben.

Unterstützung von Geräten
-   Smartphones und Phablets
-   Tablet
-   PCs und Laptops
-   Surface Hub
-   IoT
-   Xbox
-   HoloLens

![Spracherkennung](images/input-interactions/icons-speech01.png)

Typische Verwendung

Es gibt drei Modi der Sprachinteraktion:

Natürliche Sprache  
Mit natürlicher Sprache kommunizieren wir laufend mündlich mit anderen Personen. Unsere Sprache variiert in Abhängigkeit von der jeweiligen Person oder Situation, sie wird jedoch generell verstanden. Wenn dies nicht der Fall ist, verwenden wir häufig andere Wörter oder eine abweichende Wortreihenfolge, um die gleichen Gedanken zu vermitteln.

Die Kommunikation über natürliche Sprache mit einer App verläuft ähnlich: Wir sprechen über unser Gerät mit der App wie mit einer Person und erwarten, dass sie uns versteht und entsprechend reagiert.

Die natürliche Sprache ist der fortgeschrittenste Modus der Sprachinteraktion, der über **Cortana** implementiert und verfügbar gemacht wird.

Befehl und Steuerung  
Unter Befehl und Steuerung wird die Verwendung von Sprachbefehlen zum Aktivieren von Steuerelementen und Funktionen verstanden, z. B. zum Klicken auf eine Schaltfläche oder zum Auswählen eines Menüelements.

Da Befehl und Steuerung entscheidend für eine erfolgreiche Benutzeroberfläche ist, wird ein einzelner Eingabetyp im Allgemeinen nicht empfohlen. Die Spracherkennung ist in der Regel eine von mehreren Eingabeoptionen für einen Benutzer, die von seinen Vorlieben und Hardwarefunktionen abhängen.

Diktieren  
Die einfachste Methode der Spracheingabe. Jede Äußerung wird in Text umgewandelt.

Das Diktieren wird in der Regel verwendet, wenn eine App die Bedeutung oder die Absicht nicht verstehen muss.

Weitere Informationen finden Sie unter [Entwurfsrichtlinien für die Spracherkennung](https://msdn.microsoft.com/library/windows/apps/dn596121).
 

## Stift


Ein (Eingabe-)Stift kann ähnlich wie eine Maus als pixelgenaues Zeigegerät verwendet werden, und er eignet sich optimal für die Freihandeingabe.

**Hinweis**  Es gibt zwei Arten von Stiften: aktive und passive.
-   Passive Stifte enthalten keine Elektronik, und sie emulieren effektiv die Toucheingabe über einen Finger. Sie benötigen eine Basisgerätanzeige, welche die Eingabe basierend auf dem Berührungsdruck erkennt. Da Benutzer beim Schreiben auf der Eingabeoberfläche häufig die Hand ablegen, können Eingabedaten wegen des nicht erfolgreichen Ablehnens der Handfläche verzerrt werden.
-   Aktive Stifte enthalten Elektronik und können mit komplexen Gerätedisplays zusammenwirken und viel umfassendere Eingabedaten (u. a. Daten bei Daraufzeigen oder Näherung) für das System und die App liefern. Die Handflächenablehnung ist sehr viel robuster.

 

In diesem Text beziehen wir uns auf aktive Stifte, die umfangreiche Eingabedaten liefern und in erster Linie für präzise Freihandeingaben und für zeigebasierte Interaktionen verwendet werden.

Unterstützung von Geräten
-   Smartphones und Phablets
-   Tablet
-   PCs und Laptops
-   Surface Hub
-   IoT

![Zeichenstift](images/input-interactions/icons-pen01.png)

Typische Nutzung In Kombination mit einem Stift ermöglicht die Windows-Freihandplattform die natürliche Erstellung handschriftlicher Notizen, Zeichnungen und Anmerkungen. Die Plattform unterstützt das Erfassen von Freihanddaten aus einem Eingabedigitalisierungsgerät, das Generieren von Freihanddaten, das Ausgeben von Daten und das Rendern von Freihandstrichen auf dem Ausgabegerät sowie das Verwalten von Daten und das Ausführen einer Schrifterkennung. Ihre App kann nicht nur die räumlichen Bewegungen des Stifts beim Schreiben oder Zeichnen erfassen. Sie kann auch Informationen zu Druck, Form, Farbe und Deckkraft sammeln und ermöglicht so eine Arbeitsweise, die der Verwendung eines Stifts, Bleistifts oder Pinsels auf Papier schon sehr nahe kommt.

Die Stift- und die Toucheingabe unterscheiden sich dahingehend, dass bei der Toucheingabe die direkte Manipulation von UI-Elementen auf dem Bildschirm durch physische Gesten für diese Objekte (wie Wischen, Ziehen, Drehen usw.) emuliert werden kann.

Sie müssen stiftspezifische Benutzeroberflächenbefehle (oder Angebote) zur Unterstützung dieser Interaktionen bereitstellen. Verwenden Sie beispielsweise Schaltflächen für „Zurück“ und „Weiter“ (oder „+“ und „-“), um dem Benutzer das Blättern durch Seiten bzw. das Drehen, Vergrößern/Verkleinern und Zoomen von Objekten zu ermöglichen.

Weitere Informationen finden Sie unter [Entwurfsrichtlinien für Stifte](https://msdn.microsoft.com/library/windows/apps/dn456352).
 

## Toucheingabe


Bei der Toucheingabe können mit einem oder mehreren Fingern ausgeführte Gesten verwendet werden, um die direkte Manipulation von UI-Elementen (wie etwa Verschieben, Drehen, Ändern der Größe oder Bewegen) zu emulieren. Außerdem können die Gesten auch als alternative Eingabemethode (ähnlich einer Maus- oder Stifteingabe) oder als ergänzende Eingabemethode (zum Modifizieren anderer Eingaben; also beispielsweise zum Verwischen eines mit einem Stift gezeichneten Freihandstrichs) verwendet werden. Diese berührungsbasierte Bedienung wird von Benutzern unter Umständen als natürlicher wahrgenommen als die symbolbasierte Interaktion auf einem Bildschirm.

Unterstützung von Geräten
-   Smartphones und Phablets
-   Tablet
-   PCs und Laptops
-   Surface Hub
-   IoT

![Toucheingabe](images/input-interactions/icons-touch01.png)

Typische Nutzung Die Unterstützung für die Toucheingabe ist je nach Gerät sehr unterschiedlich.

Einige Geräte unterstützen keinerlei Toucheingabe, einige unterstützen einen einzelnen Touchkontakt, während wiederum andere Multitouchinteraktionen (zwei oder mehr Kontakte) unterstützen.

Die meisten Geräte mit Unterstützung von Multitouchinteraktionen erkennen zehn eindeutige gleichzeitige Kontakte.

Surface Hub-Geräte erkennen 100 eindeutige, gleichzeitige Berührungskontakte.

Im Allgemeinen weist die Toucheingabe folgende Merkmale auf:

-   Einzelner Benutzer, es sei denn, sie kommt mit einem Microsoft-Team-Gerät wie Surface-Hub zur Anwendung, bei dem der Schwerpunkt auf der Zusammenarbeit liegt.
-   Keine Beschränkung hinsichtlich der Geräteausrichtung.
-   Wird für alle Interaktionen, u. a. Texteingabe (Bildschirmtastatur) und Freihandeingabe (für die App konfiguriert) verwendet.

Weitere Informationen finden Sie unter [Designrichtlinien für die Toucheingabe](https://msdn.microsoft.com/library/windows/apps/hh465370).
 

## Touchpad


Ein Touchpad vereint die indirekte Multitoucheingabe mit der Präzisionseingabe eines Zeigergeräts (beispielsweise eine Maus). Dadurch ist das Touchpad sowohl für eine touchoptimierte Benutzeroberfläche als auch die kleineren Ziele von Produktivitäts-Apps geeignet.

Unterstützung von Geräten
-   PCs und Laptops
-   IoT

![Touchpad](images/input-interactions/icons-touchpad01.png)

Typische Nutzung Touchpads unterstützen in der Regel eine Reihe von Touchgesten, die für die direkte Manipulation von Objekten und Benutzeroberflächenelementen eine ähnliche Unterstützung bieten wie die Toucheingabe.

Aufgrund dieser Konvergenz bei der von Touchpads unterstützten Interaktion empfiehlt es sich, sich nicht allein auf die Unterstützung der Toucheingabe zu verlassen, sondern auch Benutzeroberflächenbefehle oder Angebote für die Mauseingabe bereitzustellen. Stellen Sie Touchpad-spezifische Benutzeroberflächenbefehle (oder Angebote) zur Unterstützung dieser Interaktionen bereit.

Sie müssen mausspezifische Benutzeroberflächenbefehle (oder Angebote) zur Unterstützung dieser Interaktionen bereitstellen. Verwenden Sie beispielsweise Schaltflächen für „Zurück“ und „Weiter“ (oder „+“ und „-“), um dem Benutzer das Blättern durch Seiten bzw. das Drehen, Vergrößern/Verkleinern und Zoomen von Objekten zu ermöglichen.

Weitere Informationen finden Sie unter [Designrichtlinien für Touchpads](https://msdn.microsoft.com/library/windows/apps/dn456353).
 

## Tastatur


Eine Tastatur ist das Haupteingabegerät für Text und häufig unentbehrlich für Personen mit bestimmten körperlichen Beeinträchtigungen oder für Benutzer, die die Tastatur als schnellere und effizientere Interaktionsmethode betrachten.

Mit [Continuum für Smartphones](http://go.microsoft.com/fwlink/p/?LinkID=699431), einer neuen Funktion für kompatible Windows 10-Mobilgeräte können Benutzer ihre Smartphones mit einer Maus und einer Tastatur verbinden und damit Ihr Gerät wie einen Laptop nutzen.

Unterstützung von Geräten
-   Smartphones und Phablets
-   Tablet
-   PCs und Laptops
-   Surface Hub
-   IoT
-   Xbox
-   HoloLens

![Tastatur](images/input-interactions/icons-keyboard01.png)

Typische Nutzung Benutzer können mit Universellen Windows-Apps über eine Hardwaretastatur und zwei Softwaretastaturen (Bildschirmtastatur oder Touchtastatur) interagieren.

Bei der Bildschirmtastatur handelt es sich um eine visuelle Softwaretastatur, die Sie anstelle der physischen Tastatur zum Eingeben von Daten per Touch, Maus, (Eingabe-)Stift oder anderen Zeigegeräten verwenden können. Ein Touchscreen ist nicht erforderlich. Die Bildschirmtastatur ist für Systeme ohne physische Tastatur oder für Benutzer vorgesehen, deren Mobilitätseinschränkungen die Verwendung herkömmlicher physischer Eingabegeräte verhindern. Die Bildschirmtastatur emuliert nahezu alle Funktionen der Hardwaretastatur.

Bei der Bildschirmtastatur handelt es sich um eine visuelle Softwaretastatur für die Texteingabe per Touchscreen. Die Touchtastatur ist kein Ersatz für die Bildschirmtastatur. Sie wird nur für die Texteingabe (ohne Emulierung der Hardwaretastatur) verwendet und nur angezeigt, wenn der Fokus auf einem Textfeld oder einem anderen bearbeitbaren Textsteuerelement liegt. Die Bildschirmtastatur unterstützt keine App- oder Systembefehle.

**Hinweis**  Die Bildschirmtastatur hat Vorrang vor der Touchtastatur. Ist die Bildschirmtastatur vorhanden, wird die Touchtastatur nicht angezeigt.

 

Im Allgemeinen weist eine Tastatur folgende Merkmale auf:

-   Einzelner Benutzer.
-   Keine Beschränkung hinsichtlich der Geräteausrichtung.
-   Wird für Texteingabe, Navigation, Spiele und Eingabehilfen verwendet.
-   Immer verfügbar, entweder proaktiv oder reaktiv.

Weitere Informationen finden Sie unter [Designrichtlinien für Tastaturen](https://msdn.microsoft.com/library/windows/apps/hh972345).
 

## Maus


Eine Maus eignet sich am besten für Produktivitäts-Apps und Benutzeroberflächen mit hoher Dichte, bei denen Benutzerinteraktionen beim Zielen und bei der Befehlseingabe Pixelgenauigkeit erfordern.

Unterstützung von Geräten
-   Smartphones und Phablets
-   Tablet
-   PCs und Laptops
-   Surface Hub
-   IoT

![Maus](images/input-interactions/icons-mouse01.png)

Typische Nutzung Die Mauseingabe kann durch die Hinzufügung verschiedener Tasten der Tastatur (STRG, UMSCHALTTASTE, ALT usw.) geändert werden. Diese Tasten können mit der linken Maustaste, der rechten Maustaste, der Radtaste und den X-Tasten zu einem erweiterten, mausoptimierten Befehlssatz kombiniert werden. (Einige Microsoft-Mausgeräte verfügen über zwei weitere Schaltflächen, die als X-Tasten bezeichnet werden. Diese dienen gewöhnlich dazu, in Webbrowsern zurück und vorwärts zu navigieren.

Ähnlich wie bei der Stifteingabe unterscheiden sich Maus- und Toucheingabe dahingehend, dass bei der Toucheingabe die direkte Manipulation von UI-Elementen auf dem Bildschirm durch physische Gesten für diese Objekte (z. B. Wischen, Ziehen, Drehen usw.) emuliert werden kann.

Sie müssen mausspezifische Benutzeroberflächenbefehle (oder Angebote) zur Unterstützung dieser Interaktionen bereitstellen. Verwenden Sie beispielsweise Schaltflächen für „Zurück“ und „Weiter“ (oder „+“ und „-“), um dem Benutzer das Blättern durch Seiten bzw. das Drehen, Vergrößern/Verkleinern und Zoomen von Objekten zu ermöglichen.

Weitere Informationen finden Sie unter [Designrichtlinien für die Maus](https://msdn.microsoft.com/library/windows/apps/dn456351).
 

## Geste


Eine Geste ist jede Art von Benutzerbewegung, die als Steuerungs- oder Interaktionseingabe für eine Anwendung erkannt wird. Es gibt verschiedene Arten von Gesten – von der einfachen Geste, die dazu dient, mit der Hand etwas auf dem Bildschirm zu verwenden, über spezifische, erlernte Bewegungsmuster bis hin zu langen Bewegungsabläufen des gesamten Körpers. Beachten Sie beim Entwerfen benutzerdefinierter Gesten, dass diese in anderen Regionen/Kulturkreisen unter Umständen eine andere Bedeutung haben.

Unterstützung von Geräten
-   PCs und Laptops
-   IoT
-   Xbox
-   HoloLens

![Geste](images/input-interactions/icons-gesture01.png)

Typische Nutzung Touchpads unterstützen in der Regel eine Reihe von Touchgesten, die für die direkte Manipulation von Objekten und Benutzeroberflächenelementen eine ähnliche Unterstützung bieten wie die Toucheingabe.

Aufgrund dieser Konvergenz bei der von Touchpads unterstützten Interaktion empfiehlt es sich, sich nicht allein auf die Unterstützung der Toucheingabe zu verlassen, sondern auch Benutzeroberflächenbefehle oder Angebote für die Mauseingabe bereitzustellen. Stellen Sie Touchpad-spezifische Benutzeroberflächenbefehle (oder Angebote) zur Unterstützung dieser Interaktionen bereit.

Sie müssen mausspezifische Benutzeroberflächenbefehle (oder Angebote) zur Unterstützung dieser Interaktionen bereitstellen. Verwenden Sie beispielsweise Schaltflächen für „Zurück“ und „Weiter“ (oder „+“ und „-“), um dem Benutzer das Blättern durch Seiten bzw. das Drehen, Vergrößern/Verkleinern und Zoomen von Objekten zu ermöglichen.

 

## Gamepad/Controller


Ein Gamepad/Controller ist ein sehr spezielles Gerät und kommt in der Regel bei Spielen zum Einsatz. Es wird jedoch auch zum Emulieren einfacher Tastatureingaben sowie für eine tastaturähnliche UI-Navigation verwendet.

Unterstützung von Geräten
-   PCs und Laptops
-   IoT
-   Xbox

![Controller](images/input-interactions/icons-controller01.png)

Typische Nutzung Touchpads unterstützen in der Regel eine Reihe von Touchgesten, die für die direkte Manipulation von Objekten und Benutzeroberflächenelementen eine ähnliche Unterstützung bieten wie die Toucheingabe.

Aufgrund dieser Konvergenz bei der von Touchpads unterstützten Interaktion empfiehlt es sich, sich nicht allein auf die Unterstützung der Toucheingabe zu verlassen, sondern auch Benutzeroberflächenbefehle oder Angebote für die Mauseingabe bereitzustellen. Stellen Sie Touchpad-spezifische Benutzeroberflächenbefehle (oder Angebote) zur Unterstützung dieser Interaktionen bereit.

Sie müssen mausspezifische Benutzeroberflächenbefehle (oder Angebote) zur Unterstützung dieser Interaktionen bereitstellen. Verwenden Sie beispielsweise Schaltflächen für „Zurück“ und „Weiter“ (oder „+“ und „-“), um dem Benutzer das Blättern durch Seiten bzw. das Drehen, Vergrößern/Verkleinern und Zoomen von Objekten zu ermöglichen.

 

## Mehrere Eingaben


Durch die Berücksichtigung einer möglichst großen Anzahl von Benutzern, Geräten und Eingabearten (Gesten, Spracherkennung, Toucheingabe, Touchpad, Maus und Tastatur) maximieren Sie die Flexibilität, Benutzerfreundlichkeit und Barrierefreiheit Ihrer Apps.

Unterstützung von Geräten
-   Smartphones und Phablets
-   Tablet
-   PCs und Laptops
-   Surface Hub
-   IoT
-   Xbox
-   HoloLens

![Mehrere Eingaben](images/input-interactions/icons-inputdevices03-vertical.png)

Typische Nutzung Genauso, wie Menschen durch eine Mischung aus Sprache und Gesten kommunizieren, können verschiedene Eingabearten und -modi bei der Interaktion mit einer App nützlich sein. Diese kombinierten Interaktionen müssen jedoch möglichst intuitiv und natürlich gestaltet sein, da sie andernfalls zu Verwirrung führen können.





 

 







<!--HONumber=Jun16_HO4-->


