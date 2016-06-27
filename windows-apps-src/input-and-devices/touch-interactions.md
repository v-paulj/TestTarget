---
author: Karl-Bridge-Microsoft
Description: "Erstellen Sie Apps für die universelle Windows-Plattform (UWP) mit intuitiven und unverwechselbaren Umgebungen für Benutzerinteraktionen, die für die Toucheingabe optimiert sind und deren Funktionalität über alle Eingabegeräte hinweg konsistent ist."
title: Toucheingabe-Interaktionen
ms.assetid: DA6EBC88-EB18-4418-A98A-457EA1DEA88A
label: Touch interactions
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 23eac55de26563c68b401d8912264aebb86d0380

---

# Toucheingabe-Interaktionen


Gehen Sie beim Entwerfen Ihrer App davon aus, dass die Benutzer in erster Linie die Toucheingabe als Eingabemethode verwenden werden. Wenn Sie UWP-Steuerelemente verwenden, ist keine zusätzliche Programmierung für die Unterstützung von Touchpad, Maus und Zeichen-/Eingabestift erforderlich, da sie in UWP-Apps standardmäßig bereitgestellt wird.

Bedenken Sie dabei jedoch, dass eine für Toucheingaben optimierte Benutzeroberfläche einer herkömmlichen Benutzeroberfläche nicht in jedem Fall überlegen ist. Beide haben Vor- und Nachteile, die je nach Technologie oder Anwendung unterschiedlich sein können. Sie müssen beim Übergang zu einer vorrangig auf Toucheingabe ausgelegten UI die wichtigsten Unterschiede zwischen Toucheingabe (einschließlich des Touchpads) und Eingabe über Maus, Zeichen- oder Eingabestift und Tastatur verstehen.

**Wichtige APIs**

-   [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)
-   [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)
-   [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)



Viele Geräte verfügen über Multitouch-Bildschirme, die eine Eingabe mit einem oder mehreren Fingern (oder Berührungskontakten) unterstützen. Die Berührungskontakte und ihre Bewegung werden als Touchbewegungen und Manipulationen interpretiert, um verschiedene Benutzerinteraktionen zu unterstützen.

Die universelle Windows-Plattform (UWP) bietet eine Reihe unterschiedlicher Mechanismen zur Behandlung von Toucheingaben, mit deren Hilfe Sie eine immersive, benutzerfreundliche Umgebung schaffen können. Im Folgenden behandeln wir die Grundlagen der Toucheingabe in einer UWP-App.

Interaktionen per Toucheingabe erfordern drei Dinge:

-   Einen berührungsempfindlichen Bildschirm.
-   Direkter Kontakt von einem oder mehreren Fingern auf dem Bildschirm (oder in der Nähe des Bildschirms, wenn er über Näherungssensoren verfügt und die Erkennung des Daraufzeigens unterstützt).
-   Bewegung der Berührungskontakte (oder das Fehlen der Bewegung basierend auf einem Zeitschwellenwert).

Mögliche vom Berührungssensor bereitgestellte Eingabedaten:

-   Interpretation als physische Geste für die direkte Manipulation von einem oder mehreren UI-Elementen (z. B. Schwenken, Drehen, Vergrößern/Verkleinern oder Verschieben). Die Interaktion mit einem Element über das zugehörige Eigenschaftenfenster, Dialogfeld oder ein anderes UI-Angebot gilt dagegen als indirekte Manipulation.
-   Erkennung als alternative Eingabemethode, z. B. Maus oder Stift.
-   Wird zum Ergänzen oder Ändern von Aspekten anderer Eingabemethoden verwendet, z. B. zum Verwischen eines mit einem Stift gezeichneten Freihandstrichs.

In der Regel beinhaltet die Toucheingabe die direkte Manipulation eines Elements auf dem Bildschirm. Das Element reagiert sofort auf alle Berührungen in seinem Treffertestbereich und reagiert entsprechend auf alle nachfolgenden Bewegungen des Touchkontakts, einschließlich dessen Entfernung.

Benutzerdefinierte Touchgesten und Interaktionen sollten sorgfältig entworfen werden. Sie sollten intuitiv, reaktionsfähig und leicht auffindbar sein und eine optimale Benutzererfahrung mit Ihrer App sicherstellen.

Stellen Sie sicher, dass die App-Funktionen konsistent auf allen unterstützten Eingabegerätetypen verfügbar sind. Verwenden Sie bei Bedarf eine Form von indirektem Eingabemodus, z. B. die Texteingabe für Tastaturinteraktionen oder UI-Angebote für Maus und Stift.

Bedenken Sie, dass herkömmliche Eingabegeräte (wie Maus und Tastatur) für viele Benutzer vertraut sind und gerne verwendet werden. Sie bieten häufig Faktoren wie Geschwindigkeit, Genauigkeit und klares Feedback, die bei der Toucheingabe unter Umständen nicht gegeben sind.

Mit den einzigartigen und speziellen Interaktionserfahrungen für alle Eingabegeräte unterstützen Sie eine breite Palette an Funktionen und Einstellungen und wenden sich an eine möglichst breite Zielgruppe. Auf diese Weise gewinnen Sie mehr Kunden für Ihre App.

## Vergleich der Anforderungen für Toucheingabe-Interaktionen

In der folgenden Tabelle sind einige der Unterschiede zwischen den Eingabegeräten aufgeführt, die Sie berücksichtigen sollten, wenn Sie für die Toucheingabe optimierte UWP-Apps entwerfen.

<table>
<tbody><tr><th>Merkmal</th><th>Toucheingabe-Interaktionen</th><th>Interaktionen über Maus, Tastatur oder Zeichen- bzw. Eingabestift</th><th>Touchpad</th></tr>
<tr><td rowspan="3">Präzision</td><td>Der Kontaktbereich einer Fingerspitze ist größer als eine einzige xy-Koordinate. Dadurch erhöht sich die Wahrscheinlichkeit, dass Befehle unbeabsichtigt aktiviert werden.</td><td>Maus und Zeichen- oder Eingabestift liefern eine präzise xy-Koordinate.</td><td>Identisch mit der Maus.</td></tr>
<tr><td>Die Form des Kontaktbereichs ändert sich während der Bewegung.  </td><td>Mausbewegungen und Striche mit Zeichen- oder Eingabestift liefern eine präzise xy-Koordinate. Die Tastatur hat einen expliziten Fokus.</td><td>Identisch mit der Maus.</td></tr>
<tr><td>Es gibt keinen Mauscursor, der die Zielbestimmung unterstützt.</td><td>Der Fokus von Mauscursor, Zeichen- oder Eingabestiftcursor und Tastatur unterstützt die Zielbestimmung.</td><td>Identisch mit der Maus.</td></tr>
<tr><td rowspan="3">Menschliche Anatomie</td><td>Bewegungen mit den Fingerspitzen sind ungenau, da es schwierig ist, mit den Fingern eine lineare Bewegung auszuführen. Dies liegt an der Krümmung der Handgelenke und der Anzahl der an der Bewegung beteiligten Gelenke.</td><td>Es ist leichter, mit der Maus oder mit einem Zeichen- oder Eingabestift eine Bewegung in gerader Linie auszuführen, da die Hand, die diese Geräte steuert, eine kürzere physische Strecke zurücklegen muss als der Cursor auf dem Bildschirm.</td><td>Identisch mit der Maus.</td></tr>
<tr><td>Manche Bereiche der für die Fingereingabe vorgesehenen Oberfläche eines Anzeigegeräts sind aufgrund der Fingerhaltung und der Tatsache, dass der Benutzer das Gerät halten muss, möglicherweise schwer zu erreichen.</td><td>Maus und Zeichen- oder Eingabestift können alle Bereiche des Bildschirms erreichen, während über die Aktivierreihenfolge der Zugriff auf alle Steuerelemente möglich sein sollte. </td><td>Haltung und Druck der Finger spielen dabei eventuell auch eine Rolle.</td></tr>
<tr><td>Objekte werden möglicherweise durch mindestens eine Fingerspitze oder die Hand des Benutzers verdeckt. Dies wird als Okklusion bezeichnet.</td><td>Indirekte Eingabegeräte verursachen keine Okklusion.</td><td>Identisch mit der Maus.</td></tr>
<tr><td>Objektzustand</td><td>Bei der Fingereingabe wird ein Modell mit zwei Zuständen verwendet: Die für die Fingereingabe vorgesehene Oberfläche eines Anzeigegeräts wird entweder berührt (ein) oder nicht berührt (aus). Es gibt keinen Zeigezustand, der zusätzliches visuelles Feedback auslösen kann.</td><td>
<p>Maus, Zeichen- oder Eingabestift und Tastatur machen ein Modell mit drei Zuständen verfügbar: oben (aus), unten (ein) und Zeigen (Fokus).</p>
<p>Mit Zeigen können Benutzer QuickInfos zu UI-Elementen erforschen und kennenlernen. Zeigen und Fokus können vermitteln, welche Objekte interaktiv sind, und außerdem die Zielbestimmung erleichtern. 
</p>
</td><td>Identisch mit der Maus.</td></tr>
<tr><td rowspan="2">Umfangreiche Interaktionen</td><td>Unterstützt die Mehrfingereingabe: mehrere Eingabepunkte (Fingerspitzen) auf einer für die Fingereingabe vorgesehenen Oberfläche.</td><td>Unterstützt einen einzigen Eingabepunkt.</td><td>Identisch mit der Fingereingabe.</td></tr>
<tr><td>Unterstützt die direkte Manipulation von Objekten durch Bewegungen wie beispielsweise Tippen, Ziehen, Zusammendrücken und Drehen.</td><td>Keine Unterstützung für die direkte Manipulation, da Maus, Zeichen- oder Eingabestift und Tastatur indirekte Eingabegeräte sind.</td><td>Identisch mit der Maus.</td></tr>
</tbody></table>



**Hinweis**  
Die indirekte Eingabe hat den Vorteil, dass sie über 25 Jahre optimiert wurde. Features wie durch Zeigen ausgelöste QuickInfos wurden als Lösung für die Erforschung der UI speziell für die Eingabe über Touchpad, Maus, Zeichen- oder Eingabestift und Tastatur entworfen. Solche UI-Funktionen wurden neu entworfen, um der umfassenden Toucheingabefunktion gerecht zu werden, ohne die Benutzererfahrung auf den anderen Geräten zu beeinträchtigen.

 

## Verwenden von Feedback für die Fingereingabe

Durch entsprechendes visuelles Feedback bei Interaktionen mit der App helfen Sie den Benutzern, zu erkennen und zu lernen, wie ihre Interaktionen von der App und von Windows 8 interpretiert werden, und sich daran anzupassen. Visuelles Feedback kann auf erfolgreiche Interaktionen hinweisen, über den Systemstatus informieren, das Gefühl der Kontrolle verstärken, Fehler verringern, Benutzern das Verständnis des Systems und des Eingabegeräts erleichtern und zu Interaktionen ermutigen.

Visuelles Feedback ist wichtig, wenn der Benutzer die Fingereingabe für Aktivitäten verwendet, bei denen Positionsgenauigkeit gefragt ist. Zeigen Sie immer Feedback an, wenn Toucheingabe erkannt wird, damit der Benutzer die von der App und den Steuerelementen definierten angepassten Zielbestimmungsregeln versteht.


## Zielbestimmung

Die Zielbestimmung wird durch Folgendes optimiert:

-   Größe der Toucheingabeziele

    Klare Richtlinien für die Größe gewährleisten, dass Anwendungen eine angenehme UI bereitstellen, die Objekte und Steuerelemente enthält, die als Ziele einfach und sicher zu erreichen sind.

-   Kontaktgeometrie

    Der gesamte Kontaktbereich des Fingers wird verwendet, um das wahrscheinlichste Zielobjekt zu ermitteln.

-   Scrubbing

    Elemente in einer Gruppe können leicht erneut als Ziele bestimmt werden, indem der Finger zwischen ihnen gezogen wird (beispielsweise Optionsfelder). Das aktuelle Element wird aktiviert, wenn der Finger gehoben wird.

-   Wackeln

    Eng angeordnete Elemente (beispielsweise Hyperlinks) können leicht erneut als Ziel bestimmt werden, indem Sie mit dem Finger auf das Element drücken und ohne Ziehen mit dem Finger leicht über den Elementen hin- und herwackeln. Aufgrund von Okklusion wird das aktuelle Element durch eine QuickInfo oder die Statusleiste identifiziert und aktiviert, wenn der Finger gehoben wird.

## Genauigkeit

Berücksichtigen Sie beim Design ungenaue Interaktionen, indem Sie Folgendes verwenden:

-   Andockpunkte, die bei Interaktionen der Benutzer mit dem Inhalt das Anhalten an der gewünschten Position erleichtern.
-   Führungsschienen für die Richtung, die beim vertikalen oder horizontalen Verschieben unterstützen, auch wenn die Hand in einem leichten Bogen bewegt wird. Weitere Informationen finden Sie unter [Richtlinien für Verschiebung](guidelines-for-panning.md).

## Okklusion

Okklusion durch Finger und Hand können Sie durch Folgendes vermeiden:

-   Größe und Positionierung der UI

    Machen Sie die UI-Elemente groß genug, damit sie nicht vollständig von einem Fingerspitzen-Kontaktbereich verdeckt werden können.

    Positionieren Sie Menüs und Popups möglichst immer über dem Kontaktbereich.

-   QuickInfos

    Wenn der Benutzer den Finger auf einem Objekt liegen lässt, sollten QuickInfos angezeigt werden. Dies dient der Beschreibung der Objektfunktionen. Der Benutzer kann verhindern, dass die QickInfo angezeigt wird, wenn er die Fingerspitze von Objekt herunterzieht.

    Versetzen Sie bei kleinen Objekten die QuickInfos, damit sie nicht vom Fingerspitzen-Kontaktbereich verdeckt werden. Dies ist hilfreich für die Zielbestimmung.

-   Präzision durch Ziehpunkte

    Wenn Präzision erforderlich ist (beispielsweise bei der Textauswahl), stellen Sie versetzte Auswahlziehpunkte bereit, um die Genauigkeit zu verbessern. Weitere Informationen finden Sie unter [Richtlinien für die Text- und Bildauswahl (Windows-Runtime-Apps)](guidelines-for-textselection.md).

## Zeitliche Steuerung

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


## <span id="App_views"></span><span id="app_views"></span><span id="APP_VIEWS"></span>App-Ansichten


Mithilfe der Einstellungen für Bewegungen/Bildläufe und Zoomstufen können Sie die Interaktionsmöglichkeiten für Benutzer Ihrer App-Ansichten optimieren. Die App-Ansicht bestimmt, wie ein Benutzer auf Ihre App und deren Inhalte zugreift und diese manipuliert. Ansichten stellen außerdem bestimmte Verhaltensweisen bereit, beispielsweise das Trägheitsverhalten, das „Springen“ an Inhaltsgrenzen und die Andockpunkte.

Die Einstellungen für Verschieben und Scrollen des [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527)-Steuerelements legen fest, wie Benutzer in einer Ansicht navigieren können, wenn der Inhalt der Ansicht über den Viewport hinausgeht. Eine Einzelansicht kann beispielsweise eine Seite in einem Magazin oder Buch, die Ordnerstruktur eines Computers, eine Dokumentbibliothek oder ein Fotoalbum sein.

Zoomeinstellungen gelten sowohl für den optischen Zoom (unterstützt vom [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527)-Steuerelement) als auch für das [**Semantic Zoom**](https://msdn.microsoft.com/library/windows/apps/hh702601)-Steuerelement. Der semantische Zoom ist eine für Toucheingaben optimierte Technik, um große Bestände verwandter Daten oder Inhalte in einer einzelnen Ansicht darzustellen und in diesen zu navigieren. Dabei werden zwei unterschiedliche Klassifizierungsmodi (oder Zoomstufen) verwendet. Dies ist mit dem Verschieben und dem Durchführen von Bildläufen innerhalb einer einzelnen Ansicht vergleichbar. Verschieben und Bildläufe können in Verbindung mit dem semantischen Zoom verwendet werden.

Verwenden Sie App-Ansichten und -Ereignisse zum Ändern des Verhaltens für Schwenken/Bildlauf und Zoom. Dies kann für eine flüssigere Interaktion sorgen, als dies mit der Behandlung von Zeiger- und Gestikereignissen möglich ist.

Weitere Informationen zu App-Ansichten finden Sie unter [Steuerelemente, Layouts und Text](https://msdn.microsoft.com/library/windows/apps/mt228348).

## <span id="intro_to_touch_input"></span><span id="INTRO_TO_TOUCH_INPUT"></span>Benutzerdefinierte Touchinteraktionen


Wenn Sie eine eigene Interaktionsunterstützung implementieren, sollten Sie daran denken, dass die Benutzer eine intuitive Umgebung erwarten, die die direkte Interaktion mit den UI-Elementen der App beinhaltet. Es empfiehlt sich, die benutzerdefinierten Interaktionen auf der Basis der Plattformsteuerelementbibliotheken zu modellieren, um auf diese Weise für eine konsistente und intuitive Benutzerumgebung zu sorgen. Die Steuerelemente in diesen Bibliotheken bieten umfassende Funktionen für Benutzerinteraktionen wie Standardinteraktionen, animierte Bewegungseffekte, visuelles Feedback und Barrierefreiheit. Erstellen Sie benutzerdefinierte Interaktionen nur dann, wenn ein eindeutiger, klar umrissener Bedarf besteht und es keine Basisinteraktion gibt, die das gewünschte Szenario unterstützt.

Zum Bereitstellen von angepasster Toucheingabeunterstützung können Sie verschiedene [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)-Ereignisse behandeln. Diese Ereignisse sind in drei verschiedene Abstraktionsschichten gruppiert.

-   Statische Gestikereignisse werden ausgelöst, wenn eine Interaktion abgeschlossen ist. Zu den Gestikereignissen zählen [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985), [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/br208922), [**RightTapped**](https://msdn.microsoft.com/library/windows/apps/br208984) und [**Holding**](https://msdn.microsoft.com/library/windows/apps/br208928).

    Sie können Gestikereignisse für bestimmte Elemente deaktivieren, indem Sie [**IsTapEnabled**](https://msdn.microsoft.com/library/windows/apps/br208939), [**IsDoubleTapEnabled**](https://msdn.microsoft.com/library/windows/apps/br208931), [**IsRightTapEnabled**](https://msdn.microsoft.com/library/windows/apps/br208937) und [**IsHoldingEnabled**](https://msdn.microsoft.com/library/windows/apps/br208935) auf **false** festlegen.

-   Zeigerereignisse wie [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) und [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970) bieten Details auf unterer Ebene für jeden Touchkontakt, einschließlich Zeigerbewegung, und die Möglichkeit, Ereignisse des Drückens und Loslassens zu unterscheiden.

    Bei einem Zeiger handelt es sich um eine generische Eingabeart mit einem vereinheitlichten Ereignismechanismus. Er stellt grundlegende Informationen (wie die Bildschirmposition) für die aktive Eingabequelle (Touch, Touchpad, Maus oder Stift) zur Verfügung.

-   Manipulationsgestikereignisse wie [**ManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/br208950) weisen auf eine fortlaufende Interaktion hin. Sie werden ausgelöst, wenn der Benutzer ein Element berührt, und bleiben so lange aktiv, bis der Benutzer den bzw. die Finger vom Element hebt oder die Manipulation abgebrochen wird.

    Manipulationsereignisse umfassen Multitouchinteraktionen wie Zoomen, Schwenken und Drehen sowie Interaktionen, die Trägheits- und Geschwindigkeitsdaten nutzen (z. B. Ziehen). Die von den Bearbeitungsereignissen bereitgestellten Informationen identifizieren nicht die Form der ausgeführten Interaktion. Stattdessen enthalten sie Daten, z. B. zu Position, Übersetzungsdelta und Geschwindigkeit. Mit diesen Touchdaten können Sie die Art der auszuführenden Interaktion ermitteln.

Hier sehen Sie den grundlegenden Satz von Touchgesten, die von der UWP unterstützt werden.

| Name           | Typ                 | Beschreibung                                                                            |
|----------------|----------------------|----------------------------------------------------------------------------------------|
| Tippen            | Statische Geste       | Der Bildschirm wird mit einem Finger berührt, und der Finger wird wieder angehoben.                                            |
| Gedrückt halten | Statische Geste       | Ein Finger berührt den Bildschirm und bleibt auf dem Bildschirm.                                      |
| Ziehen          | Manipulationsgeste | Mindestens ein Finger berührt den Bildschirm und bewegt sich in die gleiche Richtung.                   |
| Streifen          | Manipulationsgeste | Mindestens ein Finger berührt den Bildschirm und bewegt sich um eine kurze Distanz in die gleiche Richtung.  |
| Drehen           | Manipulationsgeste | Mindestens zwei Finger berühren den Bildschirm und führen eine Kreisbewegung im oder gegen den Uhrzeigersinn aus. |
| Zusammendrücken          | Manipulationsgeste | Mindestens zwei Finger berühren den Bildschirm und bewegen sich dichter zusammen.                         |
| Aufziehen        | Manipulationsgeste | Mindestens zwei Finger berühren den Bildschirm und bewegen sich weiter auseinander.                           |

 

<!-- mijacobs: Removing for now. We don't have a real page to link to yet. 
For more info about gestures, manipulations, and interactions, see [Custom user interactions](custom-user-input-portal.md).
-->

## <span id="gestures"></span><span id="GESTURES"></span>Gestikereignisse


In der [Steuerelementliste](https://msdn.microsoft.com/library/windows/apps/mt185406) finden Sie ausführliche Informationen zu einzelnen Steuerelementen.

## <span id="using_pointer_events"></span><span id="USING_POINTER_EVENTS"></span>Zeigerereignisse


Zeigerereignisse werden durch eine Vielzahl von aktiven Eingabequellen ausgelöst, darunter Toucheingabe, Touchpad, Stift und Maus (sie ersetzen herkömmliche Mausereignisse.)

Zeigerereignisse basieren auf einen einzigen Eingabepunkt (Finger, Stiftspitze, Mauszeiger) und unterstützen keine geschwindigkeitsbasierten Interaktionen.

Nachfolgend finden Sie eine Liste der Zeigerereignisse und der jeweiligen Ereignisargumente.

| Ereignis oder Klasse                                                       | Beschreibung                                                   |
|----------------------------------------------------------------------|---------------------------------------------------------------|
| [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971)             | Tritt auf, wenn ein Finger den Bildschirm berührt.               |
| [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972)           | Tritt auf, wenn dieser Berührungskontakt gelöst wird.                |
| [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970)                 | Tritt auf, wenn der Mauszeiger über den Bildschirm gezogen wird.         |
| [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968)             | Tritt auf, wenn der Zeiger in den Treffertestbereich eines Elements eintritt. |
| [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969)               | Tritt auf, wenn der Zeiger aus dem Treffertestbereich eines Elements austritt.  |
| [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964)           | Tritt auf, wenn ein Touchkontakt nicht normal verloren geht.               |
| [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965)     | Tritt auf, wenn ein Zeiger durch ein anderes Element erfasst wird.    |
| [**PointerWheelChanged**](https://msdn.microsoft.com/library/windows/apps/br208973)   | Tritt auf, wenn sich der Deltawert eines Mausrads ändert.         |
| [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) | Stellt Daten für alle Zeigerereignisse bereit.                         |

 

Das folgende Beispiel zeigt, wie die Ereignisse [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971), [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) und [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) verwendet werden, um eine Tippinteraktion für ein [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371)-Objekt zu behandeln ist.

Zuerst wird ein [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371) mit dem Namen `touchRectangle` in Extensible Application Markup Language (XAML) erstellt.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
           Height="100" Width="200" Fill="Blue" />
</Grid>
```

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
               Height="100" Width="200" Fill="Blue" />
</Grid>
```

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
           Height="100" Width="200" Fill="Blue" />
</Grid>
```

Als Nächstes werden Listener für die Ereignisse [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971), [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) und [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) festgelegt.

```ManagedCPlusPlus
MainPage::MainPage()
{
    InitializeComponent();

    // Pointer event listeners.
    touchRectangle->PointerPressed += ref new PointerEventHandler(this, &amp;MainPage::touchRectangle_PointerPressed);
    touchRectangle->PointerReleased += ref new PointerEventHandler(this, &amp;MainPage::touchRectangle_PointerReleased);
    touchRectangle->PointerExited += ref new PointerEventHandler(this, &amp;MainPage::touchRectangle_PointerExited);
}
```

```CSharp
public MainPage()
{
    this.InitializeComponent();

    // Pointer event listeners.
    touchRectangle.PointerPressed += touchRectangle_PointerPressed;
    touchRectangle.PointerReleased += touchRectangle_PointerReleased;
    touchRectangle.PointerExited += touchRectangle_PointerExited;
}
```

```VisualBasic
Public Sub New()

    &#39; This call is required by the designer.
    InitializeComponent()

    &#39; Pointer event listeners.
    AddHandler touchRectangle.PointerPressed, AddressOf touchRectangle_PointerPressed
    AddHandler touchRectangle.PointerReleased, AddressOf Me.touchRectangle_PointerReleased
    AddHandler touchRectangle.PointerExited, AddressOf touchRectangle_PointerExited

End Sub
```

Schließlich vergrößert der [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971)-Ereignishandler die [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) und [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) des [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371), während die [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) und [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969)-Ereignishandler die **Höhe** und die **Breite** wieder auf ihre Anfangswerte zurücksetzen.

```ManagedCPlusPlus
// Handler for pointer exited event.
void MainPage::touchRectangle_PointerExited(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Pointer moved outside Rectangle hit test area.
    // Reset the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 200;
        rect->Height = 100;
    }
}

// Handler for pointer released event.
void MainPage::touchRectangle_PointerReleased(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Reset the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 200;
        rect->Height = 100;
    }
}

// Handler for pointer pressed event.
void MainPage::touchRectangle_PointerPressed(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Change the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 250;
        rect->Height = 150;
    }
}
```

```CSharp
// Handler for pointer exited event.
private void touchRectangle_PointerExited(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Pointer moved outside Rectangle hit test area.
    // Reset the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 200;
        rect.Height = 100;
    }
}
// Handler for pointer released event.
private void touchRectangle_PointerReleased(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Reset the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 200;
        rect.Height = 100;
    }
}

// Handler for pointer pressed event.
private void touchRectangle_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Change the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 250;
        rect.Height = 150;
    }
}
```

```VisualBasic
&#39; Handler for pointer exited event.
Private Sub touchRectangle_PointerExited(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    &#39; Pointer moved outside Rectangle hit test area.
    &#39; Reset the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 200
        rect.Height = 100
    End If
End Sub

&#39; Handler for pointer released event.
Private Sub touchRectangle_PointerReleased(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    &#39; Reset the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 200
        rect.Height = 100
    End If
End Sub

&#39; Handler for pointer pressed event.
Private Sub touchRectangle_PointerPressed(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    &#39; Change the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 250
        rect.Height = 150
    End If
End Sub
```

## <span id="using_manipulation_events"></span><span id="USING_MANIPULATION_EVENTS"></span>Manipulationsereignisse


Verwenden Sie Manipulationsereignisse, wenn Sie in Ihrer App Mehrfingereingabe-Interaktionen oder Interaktionen unterstützen müssen, die Geschwindigkeitsdaten erfordern.

Mithilfe von Manipulationsereignissen können Sie Interaktionen wie Ziehen, Zoomen und Halten erkennen.

Nachfolgend finden Sie eine Liste der Manipulationsereignisse und der jeweiligen Ereignisargumente.

| Ereignis oder Klasse                                                                                               | Beschreibung                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| [**ManipulationStarting-Ereignis**](https://msdn.microsoft.com/library/windows/apps/br208951)                                   | Tritt auf, wenn der Bearbeitungsprozessor zuerst erstellt wird.                                                                                  |
| [**ManipulationStarted-Ereignis**](https://msdn.microsoft.com/library/windows/apps/br208950)                                     | Tritt auf, wenn ein Eingabegerät mit der Bearbeitung für das [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) beginnt.                                            |
| [**ManipulationDelta-Ereignis**](https://msdn.microsoft.com/library/windows/apps/br208946)                                         | Tritt auf, wenn ein Eingabegerät bei der Bearbeitung die Position ändert.                                                                      |
| [**ManipulationInertiaStarting-Ereignis**](https://msdn.microsoft.com/library/windows/apps/hh702425)                | Tritt auf, wenn das Eingabegerät während einer Bearbeitung Kontakt mit dem [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)-Objekt verliert und die Trägheit beginnt. |
| [**ManipulationCompleted-Ereignis**](https://msdn.microsoft.com/library/windows/apps/br208945)                                 | Tritt auf, wenn Bearbeitung und Trägheit für das [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) abgeschlossen sind.                                          |
| [**ManipulationStartingRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702132)               | Stelle Daten für das [**ManipulationStarting**](https://msdn.microsoft.com/library/windows/apps/br208951)-Ereignis bereit.                                         |
| [**ManipulationStartedRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702101)                 | Stellt Daten für das [**ManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/br208950)-Ereignis bereit.                                           |
| [**ManipulationDeltaRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702051)                     | Stellt Daten für das [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946)-Ereignis bereit.                                               |
| [**ManipulationInertiaStartingRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702074) | Stellt Daten für das [**ManipulationInertiaStarting**](https://msdn.microsoft.com/library/windows/apps/br208947)-Ereignis bereit.                           |
| [**ManipulationVelocities**](https://msdn.microsoft.com/library/windows/apps/br242032)                                              | Beschreibt die Geschwindigkeit, mit der die Bearbeitung stattfindet.                                                                                         |
| [**ManipulationCompletedRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702035)             | Stellt Daten für das [**ManipulationCompleted**](https://msdn.microsoft.com/library/windows/apps/br208945)-Ereignis bereit.                                       |

 

Eine Bewegung setzt sich aus einer Reihe von Bearbeitungsereignissen zusammen. Jede Geste beginnt mit einem [**ManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/br208950) (z. B., wenn ein Benutzer den Bildschirm berührt).

Anschließend wird mindestens ein [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946)-Ereignis ausgelöst. Beispielsweise, wenn Sie den Bildschirm berühren und dann den Finger über den Bildschirm ziehen. Schließlich wird ein [**ManipulationCompleted**](https://msdn.microsoft.com/library/windows/apps/br208945)-Ereignis ausgelöst, wenn die Interaktion beendet ist.

**Hinweis**  Wenn Sie keinen Touchscreen besitzen, können Sie Ihren Manipulationsereigniscode in der Simulation testen, indem Sie eine Maus- und Mausradschnittstelle verwenden.

 

Das folgende Beispiel zeigt, wie die [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946)-Ereignisse verwendet werden, um eine Ziehen-Interaktion für ein [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371) zu behandeln und es über den Bildschirm zu bewegen.

Zuerst wird ein [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371) mit dem Namen `touchRectangle` mit einer [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) und [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) von 200 in XAML erstellt.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
               Width="200" Height="200" Fill="Blue" 
               ManipulationMode="All"/>
</Grid>
```

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
               Width="200" Height="200" Fill="Blue" 
               ManipulationMode="All"/>
</Grid>
```

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
           Width="200" Height="200" Fill="Blue" 
           ManipulationMode="All"/>
</Grid>
```

Anschließend wird ein globales [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/br243027)-Element mit dem Namen `dragTranslation` erstellt, mit dem das [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371)-Element übersetzt wird. Ein [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946)-Ereignislistener wird für das **Rechteck** angegeben, und `dragTranslation` wird zum [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/br208980)-Element des **Rechtecks** hinzugefügt.

```ManagedCPlusPlus
// Global translation transform used for changing the position of 
// the Rectangle based on input data from the touch contact.
Windows::UI::Xaml::Media::TranslateTransform^ dragTranslation;
```

```CSharp
// Global translation transform used for changing the position of 
// the Rectangle based on input data from the touch contact.
private TranslateTransform dragTranslation;
```

```VisualBasic
&#39; Global translation transform used for changing the position of 
&#39; the Rectangle based on input data from the touch contact.
Private dragTranslation As TranslateTransform
```

```ManagedCPlusPlus
MainPage::MainPage()
{
    InitializeComponent();

    // Listener for the ManipulationDelta event.
    touchRectangle->ManipulationDelta += 
        ref new ManipulationDeltaEventHandler(
            this, 
            &amp;MainPage::touchRectangle_ManipulationDelta);
    // New translation transform populated in 
    // the ManipulationDelta handler.
    dragTranslation = ref new TranslateTransform();
    // Apply the translation to the Rectangle.
    touchRectangle->RenderTransform = dragTranslation;
}
```

```CSharp
public MainPage()
{
    this.InitializeComponent();

    // Listener for the ManipulationDelta event.
    touchRectangle.ManipulationDelta += touchRectangle_ManipulationDelta;
    // New translation transform populated in 
    // the ManipulationDelta handler.
    dragTranslation = new TranslateTransform();
    // Apply the translation to the Rectangle.
    touchRectangle.RenderTransform = this.dragTranslation;
}
```

```VisualBasic
Public Sub New()

    &#39; This call is required by the designer.
    InitializeComponent()

    &#39; Listener for the ManipulationDelta event.
    AddHandler touchRectangle.ManipulationDelta,
        AddressOf testRectangle_ManipulationDelta
    &#39; New translation transform populated in 
    &#39; the ManipulationDelta handler.
    dragTranslation = New TranslateTransform()
    &#39; Apply the translation to the Rectangle.
    touchRectangle.RenderTransform = dragTranslation

End Sub
```

Schließlich wird im [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946)-Ereignishandler die Position des [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371) mit der [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/br243027)-Eigenschaft für die [**Delta**](https://msdn.microsoft.com/library/windows/apps/hh702058)-Eigenschaft aktualisiert.

```ManagedCPlusPlus
// Handler for the ManipulationDelta event.
// ManipulationDelta data is loaded into the
// translation transform and applied to the Rectangle.
void MainPage::touchRectangle_ManipulationDelta(Object^ sender,
    ManipulationDeltaRoutedEventArgs^ e)
{
    // Move the rectangle.
    dragTranslation->X += e->Delta.Translation.X;
    dragTranslation->Y += e->Delta.Translation.Y;
    
}
```

```CSharp
// Handler for the ManipulationDelta event.
// ManipulationDelta data is loaded into the
// translation transform and applied to the Rectangle.
void touchRectangle_ManipulationDelta(object sender,
    ManipulationDeltaRoutedEventArgs e)
{
    // Move the rectangle.
    dragTranslation.X += e.Delta.Translation.X;
    dragTranslation.Y += e.Delta.Translation.Y;
}
```

```VisualBasic
&#39; Handler for the ManipulationDelta event.
&#39; ManipulationDelta data Is loaded into the
&#39; translation transform And applied to the Rectangle.
Private Sub testRectangle_ManipulationDelta(
    sender As Object,
    e As ManipulationDeltaRoutedEventArgs)

    &#39; Move the rectangle.
    dragTranslation.X = (dragTranslation.X + e.Delta.Translation.X)
    dragTranslation.Y = (dragTranslation.Y + e.Delta.Translation.Y)

End Sub
```

## <span id="Routed_events"></span><span id="routed_events"></span><span id="ROUTED_EVENTS"></span>Routingereignisse


Alle hier erwähnten Zeiger-, Gestik- und Manipulationsereignisse werden als *Routingereignisse* implementiert. Folglich kann das Ereignis potenziell auch von Objekten behandelt werden, bei denen es sich nicht um das Objekt handelt, von dem das Ereignis ursprünglich ausgelöst wurde. Von aufeinander folgenden übergeordneten Elementen in einer Objektstruktur (wie etwa den übergeordneten Containern eines [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)-Elements oder dem [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503)-Stammelement Ihrer App) können diese Ereignisse auch dann behandelt werden, wenn sie vom ursprünglichen Element nicht behandelt werden. Umgekehrt gilt: Ein Objekt, von dem das Ereignis nicht behandelt wird, kann das Ereignis als behandelt markieren, damit es keine übergeordneten Elemente mehr erreicht. Weitere Informationen zum Konzept der Routingereignisse sowie zu den Auswirkungen auf die Erstellung von Handlern für Routingereignisse finden Sie unter [Übersicht über Ereignisse und Routingereignisse](https://msdn.microsoft.com/library/windows/apps/hh758286).

## <span id="Dos_and_don_ts"></span><span id="dos_and_don_ts"></span><span id="DOS_AND_DON_TS"></span>Empfohlene und nicht empfohlene Vorgehensweisen


-   Entwerfen Sie Apps mit Toucheingabe als primär erwartete Eingabemethode.
-   Bieten Sie visuelles Feedback für alle Arten von Interaktionen (Berühren, Zeichenstift, Eingabestift, Maus usw.).
-   Optimieren Sie die Zielbestimmung, indem Sie die Größe des Toucheingabeziels, die Kontaktgeometrie, das Scrubbing und das Wackeln anpassen.
-   Optimieren Sie die Genauigkeit durch Verwendung von Andockpunkten und Führungsschienen.
-   Stellen Sie QuickInfos und Handles bereit, um die Genauigkeit der Toucheingabe für eng aneinanderliegende UI-Elemente zu verbessern.
-   Verwenden Sie nach Möglichkeit keine zeitlich festgelegten Interaktionen (Beispiel für eine richtige Verwendung: Berühren und Halten).
-   Verwenden Sie nach Möglichkeit nicht die Anzahl der zu verwendenden Finger, um zwischen den Manipulationen zu unterscheiden.


## <span id="related_topics"></span>Verwandte Artikel

* [Behandeln von Zeigereingaben](handle-pointer-input.md)
* [Identifizieren von Eingabegeräten](identify-input-devices.md)
            
          
            **Beispiele**
* [Einfaches Eingabebeispiel](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Beispiel für Eingabe mit niedriger Latenz](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Beispiel für den Benutzerinteraktionsmodus](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Beispiel für Focus-Visuals](http://go.microsoft.com/fwlink/p/?LinkID=619895)
            
          
            **Archivbeispiele**
* [Eingabe: Beispiel für Gerätefunktionen](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Eingabe: Beispiel für XAML-Benutzereingabeereignisse](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Beispiel für XAML-Bildlauf, -Verschiebung und -Zoom](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Eingabe: Gesten und Manipulationen mit GestureRecognizer](http://go.microsoft.com/fwlink/p/?LinkID=231605)
 

 







<!--HONumber=Jun16_HO3-->


