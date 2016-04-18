---
Description: Description: Benutzerinteraktionen in der Universellen Windows-Plattform (UWP) sind eine Kombination von Eingabe- und Ausgabequellen (z. B. Maus, Tastatur, Stift, Toucheingabe, Touchpad, Spracherkennung, Cortana, Controller, Gesten, Mimik usw.), neben diversen Modi oder Modifizierern, wodurch erweiterte Funktionen ermöglicht werden (u. a. Mausrad und -tasten, Radierer- und Zeichenstift-Schaltflächen, Bildschirmtastatur und App-Dienste im Hintergrund).
title: title: Einführung in die Interaktion
ms.assetid: 73008F80-FE62-457D-BAEC-412ED6BAB0C8
label: Interaction primer
template: detail.hbs
---

# ms.assetid: 73008F80-FE62-457D-BAEC-412ED6BAB0C8


label: Einführung in die Interaktion template: detail.hbs


![Einführung in die Interaktion](images/input-interactions/icons-inputdevices03.png)

\[ Aktualisiert für UWP-Apps unter Windows 10.

Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \] Windows-Eingabetypen

Benutzerinteraktionen in der Universellen Windows-Plattform (UWP) sind eine Kombination von Eingabe- und Ausgabequellen (z. B. Maus, Tastatur, Stift, Toucheingabe, Touchpad, Spracherkennung, **Cortana**, Controller, Gesten, Mimik usw.), neben diversen Modi oder Modifizierern, wodurch erweiterte Funktionen ermöglicht werden (u. a. Mausrad und -tasten, Radierer- und Zeichenstift-Schaltflächen, Bildschirmtastatur und App-Dienste im Hintergrund). Die UWP verwendet ein intelligentes, kontextbezogenes Interaktionssystem, mit dem in den meisten Fällen die von der App empfangenen eindeutigen Eingabetypen nicht individuell behandelt werden müssen.

## Dazu zählt das Behandeln von Toucheingabe, Touchpad, Maus und Stifteingabe als generischer Zeigertyp, um statische Gesten wie Tippen oder Gedrückthalten und Manipulationsgesten wie Ziehen zum Verschieben oder zum Rendern von Freihandeingabe zu unterstützen.


Machen Sie sich mit einzelnen Typen von Eingabegeräten, dem jeweiligen Verhalten, den Möglichkeiten und Beschränkungen in Verbindung mit gewissen Formfaktoren vertraut.

Dies erleichtert Ihnen die Entscheidung, ob die Steuerelemente und Angebote der Plattform für die App ausreichend sind oder ob Sie angepasste Funktionen für die Benutzerinteraktion bereitstellen müssen.
-   <span id="Cortana"></span><span id="cortana"></span><span id="CORTANA"></span>Cortana
-   In Windows 10 können Sie mit der Erweiterung von **Cortana** Sprachbefehle von einem Benutzer behandeln und die Anwendung zum Ausführen einer einzelnen Aktion starten.
-   Unterstützung von Geräten
-   Smartphones und Phablets
-   Tablet
-   PCs und Laptops
-   Surface Hub

![IoT](images/input-interactions/icons-cortana01.png)

Xbox
HoloLens Cortana Typische Verwendung

Ein Sprachbefehl ist eine einzelne, in einer Sprachbefehldefinitions-Datei (VCD-Datei) definierte Äußerung, die über **Cortana** an eine installierte App weitergeleitet wird. Die App kann im Vordergrund oder im Hintergrund gestartet werden, je nach Ebene und Komplexität der Interaktion. Sprachbefehle, die zusätzlichen Kontext oder Benutzereingaben erfordern, werden beispielsweise am besten im Vordergrund ausgeführt, während grundlegende Befehle im Hintergrund behandelt werden können.

**Cortana** integriert die grundlegenden Funktionen Ihrer App, bietet einen zentralen Einstiegspunkt, über den der Benutzer die meisten Aufgaben ohne Öffnen der App ausführen kann, und wird somit zum Bindeglied zwischen Ihrer App und dem Benutzer.
[In vielen Fällen spart der Benutzer dadurch viel Zeit und Mühe.](https://msdn.microsoft.com/library/windows/apps/dn974233)
 

## Weitere Informationen finden Sie unter [Cortana-Entwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/dn974233).


Weitere Informationen [Cortana-Entwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/dn974233)

<span id="Speech"></span><span id="speech"></span><span id="SPEECH"></span>Spracherkennung Die Spracherkennung ist eine effektive und natürliche Möglichkeit, mit Apps zu kommunizieren. Sie bietet eine unkomplizierte und präzise Möglichkeit der Kommunikation mit Apps; damit können Benutzer in einer Vielzahl von Situationen produktiv arbeiten und auf dem Laufenden bleiben.

Die Spracherkennung kann den Haupteingabetyp je nach Gerät in vielen Fällen ergänzen oder sogar an dessen Stelle treten.

Beispielsweise unterstützen Geräte wie HoloLens und Xbox keine herkömmlichen Eingabetypen (abgesehen von einer Softwaretastatur in bestimmten Szenarios).
-   Stattdessen verwenden sie für die meisten Benutzerinteraktionen Spracheingabe und -ausgabe (häufig zusammen mit anderen modernen Eingabetypen wie Gestik und Mimik).
-   Mit Text-zu-Sprache (auch als TTS oder Sprachsynthese bezeichnet) werden Informationen oder Anweisungen an den Benutzer ausgegeben.
-   Unterstützung von Geräten
-   Smartphones und Phablets
-   Tablet
-   PCs und Laptops
-   Surface Hub

![IoT](images/input-interactions/icons-speech01.png)

Xbox

HoloLens

Spracherkennung  
Typische Verwendung Es gibt drei Modi der Sprachinteraktion: <span id="Natural_language"></span><span id="natural_language"></span><span id="NATURAL_LANGUAGE"></span>Natürliche Sprache

Mit natürlicher Sprache kommunizieren wir laufend mündlich mit anderen Personen.

Unsere Sprache variiert in Abhängigkeit von der jeweiligen Person oder Situation, sie wird jedoch generell verstanden.

Wenn dies nicht der Fall ist, verwenden wir häufig andere Wörter oder eine abweichende Wortreihenfolge, um die gleichen Gedanken zu vermitteln.  
Die Kommunikation über natürliche Sprache mit einer App verläuft ähnlich: Wir sprechen über unser Gerät mit der App wie mit einer Person und erwarten, dass sie uns versteht und entsprechend reagiert.

Die natürliche Sprache ist der fortgeschrittenste Modus der Sprachinteraktion, der über **Cortana** implementiert und verfügbar gemacht wird. <span id="Command_and_control"></span><span id="command_and_control"></span><span id="COMMAND_AND_CONTROL"></span>Befehl und Steuerung

Unter Befehl und Steuerung wird die Verwendung von Sprachbefehlen zum Aktivieren von Steuerelementen und Funktionen verstanden, z. B. zum Klicken auf eine Schaltfläche oder zum Auswählen eines Menüelements.  
Da Befehl und Steuerung entscheidend für eine erfolgreiche Benutzeroberfläche ist, wird ein einzelner Eingabetyp im Allgemeinen nicht empfohlen. Die Spracherkennung ist in der Regel eine von mehreren Eingabeoptionen für einen Benutzer, die von seinen Vorlieben und Hardwarefunktionen abhängen.

<span id="Dictation"></span><span id="dictation"></span><span id="DICTATION"></span>Diktieren

Die einfachste Methode der Spracheingabe.
[Jede Äußerung wird in Text umgewandelt.](https://msdn.microsoft.com/library/windows/apps/dn596121)
 

## Das Diktieren wird in der Regel verwendet, wenn eine App die Bedeutung oder die Absicht nicht verstehen muss.


Weitere Informationen

[Richtlinien für den Sprachentwurf](https://msdn.microsoft.com/library/windows/apps/dn596121)
-   <span id="Pen"></span><span id="pen"></span><span id="PEN"></span>Stift Ein (Eingabe-)Stift kann ähnlich wie eine Maus als pixelgenaues Zeigegerät verwendet werden, und er eignet sich optimal für die Freihandeingabe. **Hinweis**: Es gibt zwei Arten von Stiften: aktive und passive.
-   Passive Stifte enthalten keine Elektronik, und sie emulieren effektiv die Toucheingabe über einen Finger. Sie benötigen eine Basisgerätanzeige, welche die Eingabe basierend auf dem Berührungsdruck erkennt.

 

Da Benutzer beim Schreiben auf der Eingabeoberfläche häufig die Hand ablegen, können Eingabedaten wegen des nicht erfolgreichen Ablehnens der Handfläche verzerrt werden.

Aktive Stifte enthalten Elektronik und können mit komplexen Gerätedisplays zusammenwirken und viel umfassendere Eingabedaten (u. a. Daten bei Daraufzeigen oder Näherung) für das System und die App liefern.
-   Die Handflächenablehnung ist sehr viel robuster.
-   In diesem Text beziehen wir uns auf aktive Stifte, die umfangreiche Eingabedaten liefern und in erster Linie für präzise Freihandeingaben und für zeigebasierte Interaktionen verwendet werden.
-   Unterstützung von Geräten
-   Smartphones und Phablets
-   Tablet

![PCs und Laptops](images/input-interactions/icons-pen01.png)

Surface Hub
IoT Zeichenstift Typische Verwendung

In Kombination mit einem Stift ermöglicht die Windows-Freihandplattform die natürliche Erstellung von handschriftlichen Notizen, Zeichnungen und Anmerkungen.

Die Plattform unterstützt das Erfassen von Freihanddaten aus einem Eingabedigitalisierungsgerät, das Generieren von Freihanddaten, das Ausgeben von Daten und das Rendern von Freihandstrichen auf dem Ausgabegerät sowie das Verwalten von Daten und das Ausführen einer Schrifterkennung. Ihre App kann nicht nur die räumlichen Bewegungen des Stifts beim Schreiben oder Zeichnen erfassen. Sie kann auch Informationen zu Druck, Form, Farbe und Deckkraft sammeln und ermöglicht so eine Arbeitsweise, die der Verwendung eines Stifts, Bleistifts oder Pinsels auf Papier schon sehr nahe kommt.

Die Stift- und die Toucheingabe unterscheiden sich dahingehend, dass bei der Toucheingabe die direkte Manipulation von UI-Elementen auf dem Bildschirm durch physische Gesten für diese Objekte (wie Wischen, Ziehen, Drehen usw.) emuliert werden kann.
[Sie müssen stiftspezifische Benutzeroberflächenbefehle (oder Angebote) zur Unterstützung dieser Interaktionen bereitstellen.](https://msdn.microsoft.com/library/windows/apps/dn456352)
 

## Verwenden Sie beispielsweise Schaltflächen für „Zurück“ und „Weiter“ (oder „+“ und „-“), um dem Benutzer das Blättern durch Seiten bzw. das Drehen, Vergrößern/Verkleinern und Zoomen von Objekten zu ermöglichen.


Weitere Informationen [Designrichtlinien für die Zeichenstifteingabe](https://msdn.microsoft.com/library/windows/apps/dn456352)

<span id="Touch"></span><span id="touch"></span><span id="TOUCH"></span>Toucheingabe
-   Bei der Toucheingabe können mit einem oder mehreren Fingern ausgeführte Gesten verwendet werden, um die direkte Manipulation von UI-Elementen (wie etwa Verschieben, Drehen, Ändern der Größe oder Bewegen) zu emulieren. Außerdem können die Gesten auch als alternative Eingabemethode (ähnlich einer Maus- oder Stifteingabe) oder als ergänzende Eingabemethode (zum Modifizieren anderer Eingaben; also beispielsweise zum Verwischen eines mit einem Stift gezeichneten Freihandstrichs) verwendet werden.
-   Diese berührungsbasierte Bedienung wird von Benutzern unter Umständen als natürlicher wahrgenommen als die symbolbasierte Interaktion auf einem Bildschirm.
-   Unterstützung von Geräten
-   Smartphones und Phablets
-   Tablet

![PCs und Laptops](images/input-interactions/icons-touch01.png)

Surface Hub
IoT

Toucheingabe

Typische Verwendung

Die Unterstützung für die Toucheingabe kann je nach Gerät stark variieren.

Einige Geräte unterstützen keinerlei Toucheingabe, einige unterstützen einen einzelnen Touchkontakt, während wiederum andere Multitouchinteraktionen (zwei oder mehr Kontakte) unterstützen.

-   Die meisten Geräte mit Unterstützung von Multitouchinteraktionen erkennen zehn eindeutige gleichzeitige Kontakte.
-   Surface Hub-Geräte erkennen 100 eindeutige, gleichzeitige Berührungskontakte.
-   Im Allgemeinen weist die Toucheingabe folgende Merkmale auf:

Einzelner Benutzer, es sei denn, sie kommt mit einem Microsoft-Team-Gerät wie Surface-Hub zur Anwendung, bei dem der Schwerpunkt auf der Zusammenarbeit liegt.
[Keine Beschränkung hinsichtlich der Geräteausrichtung.](https://msdn.microsoft.com/library/windows/apps/hh465370)
 

## Wird für alle Interaktionen, u. a. Texteingabe (Bildschirmtastatur) und Freihandeingabe (für die App konfiguriert) verwendet.


Weitere Informationen [Richtlinien für die Toucheingabe](https://msdn.microsoft.com/library/windows/apps/hh465370)

<span id="Touchpad"></span><span id="touchpad"></span><span id="TOUCHPAD"></span>Toucheingabepad
-   Ein Touchpad vereint die indirekte Multitoucheingabe mit der Präzisionseingabe eines Zeigergeräts (beispielsweise eine Maus).
-   Dadurch ist das Touchpad sowohl für eine touchoptimierte Benutzeroberfläche als auch die kleineren Ziele von Produktivitäts-Apps geeignet.

![Unterstützung von Geräten](images/input-interactions/icons-touchpad01.png)

PCs und Laptops
IoT

Touchpad Typische Verwendung

Touchpads unterstützen in der Regel eine Reihe von Touchgesten, die für die direkte Manipulation von Objekten und Benutzeroberflächenelementen eine ähnliche Unterstützung bieten wie die Toucheingabe. Aufgrund dieser Konvergenz bei der von Touchpads unterstützten Interaktion empfiehlt es sich, sich nicht allein auf die Unterstützung der Toucheingabe zu verlassen, sondern auch Benutzeroberflächenbefehle oder Angebote für die Mauseingabe bereitzustellen.

Stellen Sie Touchpad-spezifische Benutzeroberflächenbefehle (oder Angebote) zur Unterstützung dieser Interaktionen bereit.
[Sie müssen mausspezifische Benutzeroberflächenbefehle (oder Angebote) zur Unterstützung dieser Interaktionen bereitstellen.](https://msdn.microsoft.com/library/windows/apps/dn456353)
 

## Verwenden Sie beispielsweise Schaltflächen für „Zurück“ und „Weiter“ (oder „+“ und „-“), um dem Benutzer das Blättern durch Seiten bzw. das Drehen, Vergrößern/Verkleinern und Zoomen von Objekten zu ermöglichen.


Weitere Informationen

[Touchpad-Designrichtlinien](https://msdn.microsoft.com/library/windows/apps/dn456353)

<span id="Keyboard"></span><span id="keyboard"></span><span id="KEYBOARD"></span>Tastatur
-   Eine Tastatur ist das Haupteingabegerät für Text und häufig unentbehrlich für Personen mit bestimmten körperlichen Beeinträchtigungen oder für Benutzer, die die Tastatur als schnellere und effizientere Interaktionsmethode betrachten.
-   Mit [Continuum für Smartphones](http://go.microsoft.com/fwlink/p/?LinkID=699431), einer neuen Funktion für kompatible Windows 10-Mobilgeräte können Benutzer ihre Smartphones mit einer Maus und einer Tastatur verbinden und damit Ihr Gerät wie einen Laptop nutzen.
-   Unterstützung von Geräten
-   Smartphones und Phablets
-   Tablet
-   PCs und Laptops
-   Surface Hub

![IoT](images/input-interactions/icons-keyboard01.png)

Xbox
HoloLens

Tastatur Typische Verwendung Benutzer können mit Universellen Windows-Apps über eine Hardwaretastatur und zwei Softwaretastaturen (Bildschirmtastatur oder Touchtastatur) interagieren.

Bei der Bildschirmtastatur handelt es sich um eine visuelle Softwaretastatur, die Sie anstelle der physischen Tastatur zum Eingeben von Daten per Touch, Maus, (Eingabe-)Stift oder anderen Zeigegeräten verwenden können. Ein Touchscreen ist nicht erforderlich. Die Bildschirmtastatur ist für Systeme ohne physische Tastatur oder für Benutzer vorgesehen, deren Mobilitätseinschränkungen die Verwendung herkömmlicher physischer Eingabegeräte verhindern. Die Bildschirmtastatur emuliert nahezu alle Funktionen der Hardwaretastatur.

Bei der Bildschirmtastatur handelt es sich um eine visuelle Softwaretastatur für die Texteingabe per Touchscreen.

 

Die Touchtastatur ist kein Ersatz für die Bildschirmtastatur. Sie wird nur für die Texteingabe (ohne Emulierung der Hardwaretastatur) verwendet und nur angezeigt, wenn der Fokus auf einem Textfeld oder einem anderen bearbeitbaren Textsteuerelement liegt.

-   Die Bildschirmtastatur unterstützt keine App- oder Systembefehle.
-   **Hinweis** Die Bildschirmtastatur hat Vorrang vor der Touchtastatur. Ist die Bildschirmtastatur vorhanden, wird die Touchtastatur nicht angezeigt.
-   Im Allgemeinen weist eine Tastatur folgende Merkmale auf:
-   Einzelner Benutzer.

Keine Beschränkung hinsichtlich der Geräteausrichtung.
[Wird für Texteingabe, Navigation, Spiele und Eingabehilfen verwendet.](https://msdn.microsoft.com/library/windows/apps/hh972345)
 

## Immer verfügbar, entweder proaktiv oder reaktiv.


Weitere Informationen

[Tastaturentwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/hh972345)
-   <span id="Mouse"></span><span id="mouse"></span><span id="MOUSE"></span>Maus
-   Eine Maus eignet sich am besten für Produktivitäts-Apps und Benutzeroberflächen mit hoher Dichte, bei denen Benutzerinteraktionen beim Zielen und bei der Befehlseingabe Pixelgenauigkeit erfordern.
-   Unterstützung von Geräten
-   Smartphones und Phablets
-   Tablet

![PCs und Laptops](images/input-interactions/icons-mouse01.png)

Surface Hub
IoT Maus Typische Verwendung

Die Mauseingabe kann über verschiedene Tasten der Tastatur (STRG, UMSCHALTTASTE, ALT usw.) geändert werden.

Diese Tasten können mit der linken Maustaste, der rechten Maustaste, der Radtaste und den X-Tasten zu einem erweiterten, mausoptimierten Befehlssatz kombiniert werden. (Einige Microsoft-Mausgeräte verfügen über zwei weitere Schaltflächen, die als X-Tasten bezeichnet werden. Diese dienen gewöhnlich dazu, in Webbrowsern zurück und vorwärts zu navigieren.

Ähnlich wie bei der Stifteingabe unterscheiden sich Maus- und Toucheingabe dahingehend, dass bei der Toucheingabe die direkte Manipulation von UI-Elementen auf dem Bildschirm durch physische Gesten für diese Objekte (z. B. Wischen, Ziehen, Drehen usw.) emuliert werden kann.
[Sie müssen mausspezifische Benutzeroberflächenbefehle (oder Angebote) zur Unterstützung dieser Interaktionen bereitstellen.](https://msdn.microsoft.com/library/windows/apps/dn456351)
 

## Verwenden Sie beispielsweise Schaltflächen für „Zurück“ und „Weiter“ (oder „+“ und „-“), um dem Benutzer das Blättern durch Seiten bzw. das Drehen, Vergrößern/Verkleinern und Zoomen von Objekten zu ermöglichen.


Weitere Informationen [Richtlinien für die Mausinteraktion](https://msdn.microsoft.com/library/windows/apps/dn456351) <span id="Gesture"></span><span id="gesture"></span><span id="GESTURE"></span>Geste

Eine Geste ist jede Art von Benutzerbewegung, die als Steuerungs- oder Interaktionseingabe für eine Anwendung erkannt wird.
-   Es gibt verschiedene Arten von Gesten – von der einfachen Geste, die dazu dient, mit der Hand etwas auf dem Bildschirm zu verwenden, über spezifische, erlernte Bewegungsmuster bis hin zu langen Bewegungsabläufen des gesamten Körpers.
-   Beachten Sie beim Entwerfen benutzerdefinierter Gesten, dass diese in anderen Regionen/Kulturkreisen unter Umständen eine andere Bedeutung haben.
-   Unterstützung von Geräten
-   PCs und Laptops

![IoT](images/input-interactions/icons-gesture01.png)

Xbox
HoloLens

Geste Typische Verwendung

Touchpads unterstützen in der Regel eine Reihe von Touchgesten, die für die direkte Manipulation von Objekten und Benutzeroberflächenelementen eine ähnliche Unterstützung bieten wie die Toucheingabe. Aufgrund dieser Konvergenz bei der von Touchpads unterstützten Interaktion empfiehlt es sich, sich nicht allein auf die Unterstützung der Toucheingabe zu verlassen, sondern auch Benutzeroberflächenbefehle oder Angebote für die Mauseingabe bereitzustellen.

 

## Stellen Sie Touchpad-spezifische Benutzeroberflächenbefehle (oder Angebote) zur Unterstützung dieser Interaktionen bereit.


Sie müssen mausspezifische Benutzeroberflächenbefehle (oder Angebote) zur Unterstützung dieser Interaktionen bereitstellen. Verwenden Sie beispielsweise Schaltflächen für „Zurück“ und „Weiter“ (oder „+“ und „-“), um dem Benutzer das Blättern durch Seiten bzw. das Drehen, Vergrößern/Verkleinern und Zoomen von Objekten zu ermöglichen.

<span id="Gamepad_Controller"></span><span id="gamepad_controller"></span><span id="GAMEPAD_CONTROLLER"></span>Gamepad/Controller
-   Ein Gamepad/Controller ist ein sehr spezielles Gerät und kommt in der Regel bei Spielen zum Einsatz.
-   Es wird jedoch auch zum Emulieren einfacher Tastatureingaben sowie für eine tastaturähnliche UI-Navigation verwendet.
-   Unterstützung von Geräten

![PCs und Laptops](images/input-interactions/icons-controller01.png)

IoT
Xbox

Controller Typische Verwendung

Touchpads unterstützen in der Regel eine Reihe von Touchgesten, die für die direkte Manipulation von Objekten und Benutzeroberflächenelementen eine ähnliche Unterstützung bieten wie die Toucheingabe. Aufgrund dieser Konvergenz bei der von Touchpads unterstützten Interaktion empfiehlt es sich, sich nicht allein auf die Unterstützung der Toucheingabe zu verlassen, sondern auch Benutzeroberflächenbefehle oder Angebote für die Mauseingabe bereitzustellen.

 

## Stellen Sie Touchpad-spezifische Benutzeroberflächenbefehle (oder Angebote) zur Unterstützung dieser Interaktionen bereit.


Sie müssen mausspezifische Benutzeroberflächenbefehle (oder Angebote) zur Unterstützung dieser Interaktionen bereitstellen.

Verwenden Sie beispielsweise Schaltflächen für „Zurück“ und „Weiter“ (oder „+“ und „-“), um dem Benutzer das Blättern durch Seiten bzw. das Drehen, Vergrößern/Verkleinern und Zoomen von Objekten zu ermöglichen.
-   <span id="Multiple_inputs"></span><span id="multiple_inputs"></span><span id="MULTIPLE_INPUTS"></span>Mehrere Eingaben
-   Durch die Berücksichtigung einer möglichst großen Anzahl von Benutzern, Geräten und Eingabearten (Gesten, Spracherkennung, Toucheingabe, Touchpad, Maus und Tastatur) maximieren Sie die Flexibilität, Benutzerfreundlichkeit und Barrierefreiheit Ihrer Apps.
-   Unterstützung von Geräten
-   Smartphones und Phablets
-   Tablet
-   PCs und Laptops
-   Surface Hub

![IoT](images/input-interactions/icons-inputdevices03-vertical.png)

Xbox
HoloLens Mehrere Eingaben



Typische Verwendung Personen kommunizieren untereinander mit einer Mischung aus Sprache und Gesten, und auch bei der Interaktion mit einer App kann sich die Verwendung mehrerer Eingabearten und -modi als hilfreich erweisen.

 

 






<!--HONumber=Apr16_HO3-->


