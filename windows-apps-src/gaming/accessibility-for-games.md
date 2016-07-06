---
author: joannaleecy
title: Erstellen barrierefreier Spiele
description: Erfahren Sie, wie Sie barrierefreie Spiele erstellen. Verwenden Sie das inklusive Spielentwurfsprinzip, um ein barrierefreies Spiel zu entwickeln.
ms.assetid: f5ba1e60-0d7c-11e6-91ec-0002a5d5c51b
ms.sourcegitcommit: 2492ff5c8b3ba0331e831234943a1db124f8fa4f
ms.openlocfilehash: 2b78d767a7ac75e27f759c0eb06953e6158fb88b

---
#  Erstellen barrierefreier Spiele

Eingabehilfen können jede Person und jedes Unternehmen auf der Welt dabei unterstützen, mehr zu erreichen. Dies gilt auch für die Verbesserung der Barrierefreiheit von Spielen. Dieser Artikel wurde für Spieleentwickler geschrieben, besonders für Spieledesigner, Hersteller und Manager. Er enthält eine Übersicht über Anleitungen für die Entwicklung barrierefreier Spiele verschiedener Organisationen, die unten im Referenzabschnitt aufgelistet werden, und führt Sie in das inklusive Spielentwurfsprinzip ein, sodass Sie die Barrierefreiheit Ihrer Spiele verbessern können.

##  Warum sollten Sie barrierefreie Spiele entwickeln?

### Größere Zahl von Spielern

Grundsätzlich betrachtet, ist es nicht schwierig, die Entwicklung barrierefreier Spiele geschäftlich zu begründen:

Sie multiplizieren die Zahl der Benutzer, die Ihr Spiel spielen können, mit der Qualität Ihres Spiels und erhalten so die Verkaufszahlen für Ihr Spiel.

Wenn Sie ein beeindruckendes Spiel entwickelt haben, das so kompliziert oder komplex ist, dass es nur von sehr wenigen Menschen gespielt werden kann, begrenzen Sie Ihre Verkaufszahlen. Und wenn Sie ein Spiel entwickeln, das von Menschen mit physischen, sensorischen oder kognitiven Behinderungen nicht gespielt werden kann, entgehen Ihnen ebenfalls mögliche Umsätze. Angesichts der Tatsache, dass [19 % der Bevölkerung der Vereinigten Staaten über eine Art von Behinderung verfügen](http://www.census.gov/newsroom/releases/archives/miscellaneous/cb12-134.html), kann dies potenziell einen großen Einfluss auf den Umsatz haben, den Sie mit Ihrem Spiel erzielen können. 

Weitere geschäftliche Begründungen finden Sie unter [Erstellen barrierefreier Videospiele](https://msdn.microsoft.com/library/windows/desktop/ee415219).

### Bessere Spiele

Wenn Sie die Barrierefreiheit eines Spiels verbessern, kann dies dazu führen, dass das Spiel insgesamt besser wird. 

Ein Beispiel hierfür ist die Verwendung von Untertiteln in Spielen. In der Vergangenheit unterstützten Spiele nur selten Untertitel bzw. Untertitelungen für die Dialoge in Spielen. Heute wird erwartet, dass Spiele Untertitel bzw. Untertitelungen enthalten. Diese Änderung wurde nicht von Spielern mit Behinderungen verursacht. Sie geht auf Spieler zurück, die es einfach bevorzugten, mit Untertiteln zu spielen, da so ihre Spielerfahrung verbessert wurde. Spieler aktivieren Untertitel bzw. Untertitelungen, wenn sie mit zu viel Hintergrundgeräuschen spielen; wenn sie Schwierigkeiten haben, Stimmen zu hören, wenn gleichzeitig mehrere Audioeffekte oder Umgebungsgeräusche abgespielt werden; oder wenn sie einfach nur die Lautstärke niedrig halten möchten, um andere nicht zu stören. Untertitel und Untertitelungen haben Spielern nicht nur geholfen, eine bessere Spielerfahrung zu erzielen, sondern ermöglichen auch Menschen mit beeinträchtigter Hörfähigkeit, Spiele zu spielen.

Die Neuzuordnung von Controllern ist ein weiteres Feature, das aus ähnlichen Gründen allmählich zum Standard in der Spielebranche wird. Spieler genießen es, ihre Spielumgebungen anzupassen. Die meisten Menschen wissen jedoch nicht, dass die Möglichkeit, Tasten auf einem Eingabegerät neu zuzuordnen, tatsächlich ein Barrierefreiheitsfeature ist, das ein Spiel für Menschen mit verschiedenen motorischen Behinderungen spielbar machen soll.

Letzten Endes führt das Nachdenken darüber, wie Sie die Barrierefreiheit Ihres Spiels verbessern können, häufig zu einem besseren Spiel, da Sie auf diese Weise eine Umgebung mit höherer Benutzerfreundlichkeit und besserer Anpassbarkeit für Ihre Spieler entwickeln.

### Soziale Interaktionen

Gaming ist eine Form der Unterhaltung und kann Stunden von Spaß bieten. Für einige ist Gaming jedoch nicht nur eine Form der Unterhaltung, sondern auch eine Möglichkeit, einem Bett im Krankenhaus, chronischen Schmerzen oder lähmenden sozialen Ängsten zu entkommen. Spieler werden in eine Welt transportiert, in denen sie zu den Hauptpersonen im Videospiel werden. Durch das Gaming können Sie einen sozialen Raum für sich selbst erstellen und in diesem agieren, der sie von ihren alltäglichen Problemen aufgrund ihrer Behinderung ablenkt und ihnen eine Möglichkeit bietet, mit Menschen zu kommunizieren, mit denen sie andernfalls möglicherweise nicht interagieren könnten.

##  Ist das Spiel, das Sie heute entwickeln, barrierefrei?

Wenn Sie zum ersten Mal darüber nachdenken, Ihr Spiel barrierefrei zu machen, finden Sie im Folgenden einige Fragen, die Sie sich stellen sollten:

* Können Sie das Spiel mit nur einer Hand spielen? 
* Kann ein durchschnittlicher Benutzer das Spiel auswählen und sofort spielen?
* Können Sie das Spiel auf einem kleinen Bildschirm oder auf einem Fernseher aus einer gewissen Entfernung effektiv spielen?
* Unterstützen Sie mehr als eine Art von Eingabegerät, das während des gesamten Spiels verwendet werden kann?
* Können Sie das Spiel ohne Audio spielen?
* Können Sie das Spiel mit einem auf Schwarzweiß eingestellten Monitor spielen?

Wenn Sie die meisten Fragen mit „nein“ beantwortet haben oder die Antwort auf die meisten Fragen nicht kennen, ist es an der Zeit, die Barrierefreiheit Ihres Spiels zu verbessern.

## Definition von Behinderungen

Behinderungen werden als „fehlende Übereinstimmung zwischen den Bedürfnissen einer Person und dem Service, dem Produkt oder der Umgebung, die angeboten werden“ definiert. ([Video zum Inklusivprinzip](https://www.microsoft.com/design/inclusive), Microsoft.com.) Dies bedeutet, dass jeder Benutzer eine Behinderung erfahren kann und dies ein kurzfristiger oder situationsbedingter Zustand sein kann. Stellen Sie sich die Herausforderungen vor, denen sich Spieler in einer derartigen Lage gegenübersehen, wenn sie Ihr Spiel spielen, und überlegen Sie, wie Sie Ihr Spiel für diese Personen verbessern können. Im Folgenden finden Sie einige Behinderungen, an die Sie denken sollten:

### Sehvermögen

*   Medizinische langfristige Bedingungen wie grüner Star, Katarakte, Farbblindheit, Kurzsichtigkeit und diabetesbedingte Retinopathie
*   Kurzfristige situationsbedingte Zustände wie ein kleiner Monitor oder Bildschirm, ein Bildschirm mit niedriger Auflösung oder ein Bildschirm, der aufgrund von hellen Lichtquellen auf dem Monitor blendet
        
### Hörvermögen

* Medizinische langfristige Bedingungen wie vollständige Taubheit oder teilweiser Hörverlust aufgrund von Krankheiten oder genetischen Ursachen
* Kurzfristige situationsbedingte Zustände wie übermäßige Hintergrundgeräusche oder eingeschränkte Lautstärke, um andere nicht zu stören
        
### Motorische Fähigkeiten

* Medizinische langfristige Bedingungen wie Parkinson's Krankheit, amyotrophe Lateralsklerose (ALS), Arthritis und Muskeldystrophie
* Kurzfristige situationsbedingte Zustände wie eine verletzte Hand, Halten eines Getränks oder Tragen eines Kindes auf einem Arm
  
### Kognitive Fähigkeiten

* Medizinische langfristige Bedingungen wie Dyslexie, Epilepsie, Aufmerksamkeitsdefizitstörung (ADHD), Demenz und Amnesie
* Kurzfristige situationsbedingte Zustände wie Schlafmangel oder temporäre Ablenkungen wie Sirenen eines Einsatzfahrzeugs, das am Haus vorbeifährt

### Sprechvermögen

* Medizinische langfristige Bedingen wie beschädigte Stimmbänder, Dysarthrie und Apraxie
* Kurzfristige situationsbedingte Zustände wie zahnärztliche Behandlungen oder Essen und Trinken


## Wie können Sie die Barrierefreiheit von Spielen verbessern?

### Änderung des Designansatzes: inklusives Spieledesign

Das inklusive Design konzentriert sich auf das Erstellen von Produkten und Diensten, die für ein breiteres Spektrum von Verbrauchern besser zugänglich sind, darunter auch Menschen mit Behinderungen.

Um erfolgreich zu sein, müssen die Spieledesigner von heute über die Entwicklung von unterhaltsamen Spielen für eine kleine, ausgewählte Zielgruppe hinaus denken. Spieledesigner müssen die Auswirkungen ihrer Designentscheidungen auf die allgemeine Barrierefreiheit des Spiels berücksichtigen, d. h. die Spielbarkeit des Spiels für ihre potenzielle Zielgruppe insgesamt, einschließlich Menschen mit Behinderungen.

Es muss daher ein Paradigmenwechsel vom herkömmlichen Spieledesign hin zum inklusiven Konzept für das Design von Spielen stattfinden. Das inklusive Spieledesign geht über das einfache Spieledesign hinaus, das der Zielgruppe Unterhaltung bietet. Es bedeutet, zusätzliche oder geänderte Personas zu entwickeln, um eine breitere Zielgruppe anzusprechen. 

Dieser zusätzliche Schritt hilft Ihnen, Lücken im ursprünglichen Design zu ermitteln. Indem Sie diese Lücken identifizieren, können Sie das ursprüngliche Designkonzept überarbeiten und verbessern. Wenn Sie sich Zeit nehmen und den inklusiven Designansatz für Ihr Spiel berücksichtigen, wird Ihr Spiel letzten Endes besser zugänglich.

### Optionen für Spieler

Barrierefreiheit bedeutet, Optionen bereitzustellen. Geben Sie Spielern Optionen, um ihre Spielumgebung anzupassen. Wenn Sie bereits eine großer Fanbasis haben, gibt es möglicherweise eine erhebliche Zahl von Spielern, die nicht möchten, dass die Umgebung auch nur minimal geändert wird. Das ist in Ordnung. Ermöglichen Sie Ihren Spielern, diese Features zu aktivieren und zu deaktivieren, und ermöglichen Sie die individuelle Konfiguration von Features.

### Innovationen: Seien Sie kreativ

Es gibt viele kreative Möglichkeiten, um die Barrierefreiheit Ihres Spiels zu verbessern. Werden Sie kreativ, und lernen Sie von anderen barrierefreien Spielen auf dem Markt. Wenn Sie bereits ein Spiel entwickelt haben, sollten Sie versuchen, aktuelle Features Ihres Spiels zu identifizieren, die verbessert werden können, ohne das ursprüngliche Design der zentralen Spielmechanismen und der Spielerfahrung zu verändern. Wie bereits erwähnt, geht es bei der Barrierefreiheit von Spielen darum, Spielern Optionen für die Anpassung ihrer Spielerfahrung bereitzustellen.

### Werben Sie: Machen Sie die Barrierefreiheit in Ihrem Spielestudio zu einer Priorität

Für die Spieleentwicklung gibt es stets einen engen Terminplan. Wenn Sie die Barrierefreiheit zur Priorität erklären, wird es einfacher. Eine Möglichkeit besteht darin, das Spiel von Anfang an im Gedanken an Barrierefreiheit zu entwerfen. Teilen Sie Ihr Wissen über Barrierefreiheit mit Ihrem Team, und teilen Sie auch die geschäftlichen Begründungen mit.

### Überprüfen Sie: Bewerten Sie Ihr Spiel laufend

Sie können einen Überprüfungsprozess während der Entwicklung einführen, um sicherzustellen, dass die Barrierefreiheit bei jedem Schritt berücksichtigt wird. Erstellen Sie eine Checkliste wie die unten gezeigte, um Ihrem Team zu helfen, laufend zu überprüfen, ob die entwickelten Features barrierefrei sind.

| Checkliste                                         | Barrierefreiheitsfeatures                                                                                                         |
|---------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Kinoeffekte im Spiel                                | Hat Untertitel und Untertitelungen, ist auf Fotosensibilität getestet                                                                           |
| Grafiken im Allgemeinen (2D- und 3D-Grafiken)              | Für farbenblinde Spieler geeignete Farben und Optionen, keine ausschließliche Abhängigkeit von Farben für die Identifizierung, sondern auch von Formen und Mustern|
| Startbildschirm, Einstellungsmenü und andere Menüs       | Möglichkeit, Optionen vorzulesen, Möglichkeit, Einstellungen zu merken, alternative Eingabemethode für Befehlssteuerung, anpassbarer UI-Schriftgrad  |
| Spielplan                                          | Umfassend anpassbarere Schwierigkeitsgrade, Untertitel und Untertitelungen, gutes visuelles und Audiofeedback für die Spieler                           |
| HUD-Anzeige                                       | Anpassbare Bildschirmposition, anpassbarer Schriftgrad, Einstellungen für farbenblinde Spieler                                                  |        
| Steuerelementeingabe                                     | Möglichkeit, Steuerelemente dem Eingabegerät zuzuordnen, benutzerdefinierte Controller-Unterstützung, Zulässigkeit vereinfachter Eingaben im Spiel                               |        

### Spieletests und Überarbeitungen: Bitten Sie Spieler um Feedback

Laden Sie Spieletester mit Behinderungen, für die bei der Spieleentwicklung berücksichtigt wurden, zu den Spieletests ein. Achten Sie darauf, wie sie spielen, und bitten Sie sie um Feedback. Stellen Sie fest, welche Änderungen vorgenommen werden müssen, um das Spiel zu verbessern.

### Erzählen Sie es jedem: Lassen Sie die Welt wissen, dass Ihr Spiel barrierefrei ist

Verbraucher möchten wissen, ob Ihr Spiel von Spielern mit Behinderungen gespielt werden kann. Geben Sie die Barrierefreiheit des Spiels auf der Website des Spiels und auf der Verpackung an, um sicherzustellen, dass Verbraucher beim Kauf Ihres Spiels wissen, was sie erwarten können. Denken Sie daran, auch die Website und alle Vertriebskanäle für das Spiel barrierefrei zu gestalten. An wichtigsten ist jedoch, dass Sie die Community von Spielern mit Behinderungen über Ihr Spiel informieren.

## Barrierefreiheitsfeatures von Spielen

In diesem Abschnitt werden einige Features beschrieben, mit denen Sie die Barrierefreiheit Ihres Spiels verbessern können. Diese Features sind den [Anleitungen für die Barrierefreiheit von Spielen](http://gameaccessibilityguidelines.com/) entnommen, die die Ergebnisse der Zusammenarbeit einer Gruppe von Studios, Spezialisten und Hochschullehrern darstellen. Weitere Informationen finden Sie unter [Anleitungen für die Barrierefreiheit von Spielen](http://gameaccessibilityguidelines.com/). 

### Für farbenblinde Personen geeignete Grafiken und Benutzeroberflächen

Die Netzhaut des Auges besitzt zwei Arten von lichtempfindlichen Zellen: die Zapfen, um zu erkennen, wo Licht ist, und die Stäbchen, um bei schlechten Lichtbedingungen sehen zu können. Es gibt drei Arten von Zapfen (für Rot, Grün und Blau), um uns zu ermöglichen, Farben korrekt zu erkennen. Farbenblindheit tritt auf, wenn mindestens einer dieser drei Arten von lichtempfindlichen Zapfen nicht wie erwartet funktioniert. Der Grad der Farbenblindheit kann von einer fast normalen Farbwahrnehmung mit reduzierter Empfindlichkeit für rotes, grünes oder blaues Licht bis zur völligen Unfähigkeit, Rot, Grün oder Blau wahrzunehmen, reichen. Da eine reduzierte Empfindlichkeit für blaues Licht seltener ist, werden bei der Entwicklung für farbenblinde Spieler vor allem Personen berücksichtigt, die unempfindlich für rotes oder grünes Licht sind:
 
  + Verwenden Sie Farbkombinationen, die von Benutzern unterschieden werden können, die rotes/grünes Licht nicht wahrnehmen können:
  
    * Ähnliche Farben: alle Abstufungen von Rot und Grün sowie von Braun und Orange
    * Farben, die sich abheben: Blau und Gelb
    
  + Verlassen Sie sich nicht ausschließlich auf Farben, um Spielobjekte zu unterscheiden; verwenden Sie auch Formen und Muster.
  
### Untertitelungen und Untertitel

Ziel des Designs von Untertitelungen und Untertiteln für Ihr Spiel ist die optionale Bereitstellung lesbarer Untertitel, damit Ihr Spiel auch ohne Audio gespielt werden kann. Es sollte möglich sein, Spielkomponenten wie Spieldialoge, Spielaudio und Soundeffekte als Text auf dem Bildschirm anzuzeigen.

Im Folgenden werden einige einfache Anleitungen aufgelistet, die Sie beim Design von Untertitelungen und Untertiteln berücksichtigen sollten:

*   Wählen Sie eine einfache, lesbare Schriftart.
*   Wählen Sie einen ausreichend großen Schriftgrad, oder stellen Sie eine Option für die Anpassung des Schriftgrads bereit, um größere Flexibilität zu bieten. (Der ideale Schriftgrad ist von der Bildschirmgröße, dem Abstand vom Bildschirm usw. abhängig.)
*   Schaffen Sie einen hohen Kontrast zwischen Hintergrund und Schriftfarbe. (Weitere Informationen hierzu finden Sie unter [Informationen zum Kontrastverhältnis](https://msdn.microsoft.com/windows/uwp/accessibility/accessible-text-requirements).)
* Zeigen Sie auf dem Bildschirm kurze Sätze an. (Denken Sie daran, nicht zu viel über das Spiel zu verraten, indem Sie den Text anzeigen, bevor das Ereignis eintritt.)
*   Stellen Sie klar, aus welcher Quelle der Audioeffekt stammt oder wer gerade spricht. (Beispiel: „Daniel: Hallo!“.)
*   Bieten Sie die Möglichkeit, Untertitelungen und Untertitel ein- und auszuschalten. (Zusätzliches Feature: Bieten Sie die Möglichkeit an, anhand der Bedeutung auszuwählen, wie viele Audioinformationen angezeigt werden.)

### Audiofeedback

Audio- oder Soundeffekte bieten dem Spieler Feedback, zusätzlich zum visuellen Feedback. Ein gutes Audiodesign des Spiels kann die Barrierefreiheit für Spieler mit Sehschwächen verbessern. Im Folgenden werden einige Anleitungen aufgeführt, die Sie berücksichtigen sollten:

*   Verwenden Sie 3D-Audiohinweise, um zusätzliche räumliche Informationen bereitzustellen.
* Trennen Sie Musik-, Sprach- und Audio- bzw. Soundeffekte.
*   Gestalten Sie die Sprachhinweise so, dass sie den Spielern nützliche Informationen bieten. (Beispiel: „Die Feinde nähern sich“ im Gegensatz zu „Die Feinde nähern sich von der Hintertür aus“.)
*   Stellen Sie sicher, dass die Sprachhinweise mit einer akzeptablen Geschwindigkeit gesprochen werden, und ermöglichen Sie die Steuerung der Geschwindigkeit, um eine bessere Barrierefreiheit zu erzielen.

### Vollständig zuordbare Steuerelemente

Es gibt Unternehmen und Organisationen wie z. B. [Special Effect](http://www.specialeffect.org.uk/), die benutzerdefinierte Steuergeräte für Spiele entwickeln, die mit verschiedenen Gamingsystemen wie Windows und Xbox One verwendet werden können. Diese Anpassung ermöglicht Benutzern mit unterschiedlichen Arten von Behinderungen, Spiele zu spielen, die sie andernfalls möglicherweise nicht spielen könnten. Weitere Informationen zu Personen, die nun mithilfe angepasster Steuergeräte eigenständig Spiele spielen können, finden Sie unter [Unterstützte Personen](http://www.specialeffect.org.uk/who-we-helped).

Als Spieleentwickler können Sie die Barrierefreiheit Ihres Spiels verbessern, indem Sie die vollständige Zuordbarkeit von Steuerelemente ermöglichen, damit Spieler ihre benutzerdefinierten Steuergeräte anschließen und die Tasten entsprechend ihren Bedürfnissen zuordnen können.

Sowohl die Xbox One-Standardsteuergeräte als auch die Xbox Elite-Steuergeräte ermöglichen die Anpassung der Steuergeräte, um ein präzises Gaming zu bieten. Weitere Informationen hierzu finden Sie unter [Xbox One](http://support.xbox.com/xbox-one/accessories/customize-standard-controller-with-accessories-app) und [Xbox Elite](http://support.xbox.com/xbox-one/accessories/use-accessories-app-configure-elite-controller).

### Breitere Auswahl an Schwierigkeitsgraden

Videospiele bieten Unterhaltung. Die Herausforderung für Spieleentwickler besteht darin, den Schwierigkeitsgrad so anzupassen, dass den Spielern der richtige Grad an Herausforderungen geboten wird. Nicht alle Spieler besitzen die gleiche Geschicklichkeit und die gleichen Fähigkeiten, sodass die Entwicklung einer breiteren Auswahl an Schwierigkeitsoptionen die Chance verbessert, Spielern den richtigen Grad an Herausforderungen zu bieten. Gleichzeitig verbessert diese breitere Auswahl auch die Barrierefreiheit Ihres Videospiels, da so eine potenziell größere Zahl von Menschen mit Behinderungen Ihr Spiel spielen kann. Denken Sie daran, dass Spieler in einem Spiel Herausforderungen überwinden und dafür belohnt werden möchten. Sie möchten kein Spiel spielen, in dem sie nicht gewinnen können.

Die Anpassung der Schwierigkeitsgrade Ihres Spiels ist ein heikler Vorgang. Wenn Ihr Spiel zu einfach ist, sind die Spieler möglicherweise gelangweilt. Wenn es zu schwierig ist, geben die Spieler möglicherweise auf und spielen das Spiel nicht mehr. Der Ausgleich zwischen diesen beiden Extremen ist Kunst und Wissenschaft zugleich. Es gibt viele Möglichkeiten, ein Spiellevel mit dem richtigen Grad an Herausforderungen zu entwickeln. Einige Spiele bieten vereinfachte Eingaben wie Ein-Klick-Optionen, eine Rückspulungs- und Wiederholungsoption, um Fehler im Spiel leichter verzeihbar zu machen, oder bieten weniger und schwächere Feinde an, um es einfacher machen, im Spiel weiterzukommen, wenn mehrere Versuche gescheitert sind.

### Tests auf Lichtempfindlichkeitsepilepsie

Lichtempfindlichkeitsepilepsie (PSE) ist eine Erkrankung, bei der durch visuelle Reize wie aufblitzendes Licht oder bestimmte sich bewegende visuelle Formen und Muster Anfälle ausgelöst werden. Diese Erkrankung tritt bei ungefähr drei Prozent der Menschen auf und ist besonders bei Kindern und Jugendlichen verbreitet.

Es gibt viele Faktoren, die beim Spielen von Videospielen eine lichtempfindliche Reaktion auslösen können, darunter die Spieldauer, die Häufigkeit des aufblitzenden Lichts, die Intensität des Lichts, der Kontrast zwischen Hintergrund und Licht, der Abstand zwischen Bildschirm und Spieler sowie die Wellenlänge des Lichts.

Als Entwickler finden Sie hier einige Tipps für das Entwerfen eines Spiels, das auch von Spielern gespielt werden kann, die zur Lichtempfindlichkeitsepilepsie neigen:

*   Vermeiden Sie Licht, das mit einer Frequenz von 5 bis 30 Blitzen pro Sekunde (Hertz) aufblitzt, da blitzendes Licht in diesem Bereich am wahrscheinlichsten Anfälle auslöst.
*   Verwenden Sie ein automatisiertes System, um das Spiel auf Reize zu überprüfen, die einen Anfall von Lichtempfindlichkeitsepilepsie auslösen können. (Beispiel: [Harding Flash and Pattern Analyzer (FPA) G2](http://www.hardingfpa.com/harding-fpa-for-games/) von Cambridge Research System Ltd und Professor Graham Harding.) 
*   Planen Sie Pausen zwischen Spiellevels ein, damit Spieler nicht ohne Unterbrechung spielen.

## Weitere Ressourcen für barrierefreie Spiele

Im Folgenden finden Sie einige externe Websites, die zusätzliche Informationen in Bezug auf barrierefreie Spiele bereitstellen.

### Anleitungen für die Barrierefreiheit von Spielen
* [Anleitungen für die Barrierefreiheit von Spielen](http://gameaccessibilityguidelines.com/)
* [Anleitungen der AbleGamers Foundation](http://www.includification.com/)
* [Universell zugängliche Spiele (Universally Accessible (UA)-Spiele)](http://www.ics.forth.gr/hci/ua-games/index_main.php?l=e&c=555)

### Benutzerdefinierte Eingabesteuergeräte
* [Special Effect](http://www.specialeffect.org.uk/)
* [Warfighter Engaged](http://www.warfighterengaged.org/)

## Verwendete Quellen
* [Anleitungen für die Barrierefreiheit von Spielen](http://gameaccessibilityguidelines.com/)
* [Anleitungen der AbleGamers Foundation](http://www.includification.com/)
* [Color Blind Awareness, a Community Interest Company](http://www.colourblindawareness.org/colour-blindness/types-of-colour-blindness/)
* [How to do subtitles well (Anleitung für das Erstellen von Untertiteln) – Blogbeitrag auf Gamasutra von Ian Hamilton](http://www.gamasutra.com/blogs/IanHamilton/20150715/248571/How_to_do_subtitles_well__basics_and_good_practices.php)
* [Innovation for All Programme](http://www.inclusivedesign.no/practical-tools/definitions-article56-127.html)

## Verwandte Links
* [Inklusives Design](https://www.microsoft.com/design/inclusive)
* [Microsoft Accessibility Developer Hub](https://developer.microsoft.com/windows/accessible-apps)
* [Entwickeln von barrierefreien UWP-Apps](https://msdn.microsoft.com/windows/uwp/accessibility/accessibility)
* [E-Book: Engineering Software For Accessibility (Entwickeln von barrierefreier Software)](https://www.microsoft.com/download/details.aspx?id=19262)



<!--HONumber=Jun16_HO4-->


