---
author: Xansky
Description: Hier erfahren Sie, wie Sie barrierefreie UWP-Apps für Windows 10 entwickeln, die Tastaturnavigation, Farb- und Kontrasteinstellungen und Unterstützung für Hilfstechnologien enthalten.
ms.assetid: 9311D23A-B340-42F0-BEFE-9261442AF108
title: Entwickeln von barrierefreien Windows 10-Apps
label: Developing inclusive Windows 10 apps
template: detail.hbs
---

# Entwickeln von barrierefreien Windows-Apps  

Hier erfahren Sie, wie Sie barrierefreie UWP-Apps für Windows 10 entwickeln, die Tastaturnavigation, Farb- und Kontrasteinstellungen und Unterstützung für Hilfstechnologien enthalten.

In diesem Artikel wird beschrieben, wie barrierefreie UWP-Apps (Universelle Windows-Plattform) entwickelt werden können. Insbesondere wird vorausgesetzt, dass Sie wissen, wie Sie die logische Hierarchie für Ihre App entwerfen können.  

Wenn Sie dies noch nicht getan haben, beginnen Sie mit dem Lesen [Entwerfen inklusiver Software](designing-inclusive-software.md).

Sie sollten drei Dinge tun, um sicherzustellen, dass Ihre App barrierefrei ist:
1. Machen Sie Ihre UI-Elemente auf [programmgesteuerten Zugriff](#programmatic-access) verfügbar.
2. Stellen Sie sicher, dass Ihre App [die Tastaturnavigation](#keyboard-navigation) für Personen unterstützt, die keine Maus oder keinen Touchscreen verwenden können.
3. Stellen Sie sicher, dass Ihre App barrierefreie [Farbe und Kontrast](#color-and-contrast)-Einstellungen unterstützt.

## Programmgesteuerter Zugriff  
Programmgesteuerter Zugriff ist wichtig für die Barrierefreiheit in Apps. Dies erfolgt durch die Festlegung des Namens (erforderlich) und der Beschreibung (optional) zur Verwendung durch Bildschirmleseprogramme für den Inhalt und die interaktiven UI-Elemente Ihrer App. Dadurch wird sichergestellt, dass die UI-Steuerelemente für unterstützende Technologie (AT) (z. B. Sprachausgabe) oder alternative Audioausgabegeräte (z. B. Braille-Anzeigen) verfügbar gemacht wird. Ohne den programmgesteuerten Zugriff können die APIs für Eingabehilfen Informationen nicht korrekt interpretieren. Dadurch können Benutzer Produkte nicht ausreichend verwenden oder es wird erzwungen, dass die AT undokumentierte Programmierschnittstellen oder Techniken verwenden, die nie als barrierefreie Benutzeroberfläche verwendet werden sollten. Wenn UI-Steuerelemente für Hilfstechnologien verfügbar gemacht werden, kann die AT feststellen, welche Aktionen und Optionen für den Benutzer verfügbar sind.  

Weitere Informationen zur Bereitstellung der UI-Elemente Ihrer App für Hilfstechnologien (AT) finden Sie unter [Grundlegende Informationen zur Barrierefreiheit verfügbar machen](basic-accessibility-information.md).

## Navigation mithilfe der Tastatur  
Für Benutzer, die blind sind oder deren Beweglichkeit eingeschränkt ist, ist die Navigation auf der Benutzeroberfläche mit einer Tastatur äußerst wichtig. Allerdings sollten nur UI-Steuerelemente einen Tastaturfokus erhalten, die eine Benutzerinteraktion erfordern. Komponenten, die keine Aktion erfordern, z. B. statische Bilder, erfordern keinen Tastaturfokus.  

Es ist wichtig, zu beachten, dass im Gegensatz zur Navigation mit einer Maus oder per Fingereingabe die Tastaturnavigation linear ist. Bei der Tastaturnavigation sollten Sie überlegen, wie der Benutzer mit Ihrem Produkt interagiert und was die logische Navigation wird. In westeuropäischen Kulturkreisen liest man von links nach rechts und von oben nach unten. Es ist daher üblich, dieses Muster für die Navigation per Tastatur zu verwenden.  

Untersuchen Sie beim Entwerfen der Tastaturnavigation Ihre Benutzeroberfläche und denken Sie über diese Fragen nach:
* Wie sind die Steuerelemente angeordnet oder in der Benutzeroberfläche gruppiert?
* Gibt es ein paar erhebliche Steuerelementgruppen?
    * Falls ja, enthalten diese Gruppen eine andere Ebene von Gruppen?
*   Sollte die Navigation zwischen Peer-Steuerelementen durch die TAB-Taste oder über spezielle Navigation (z. B. Pfeiltasten) erfolgen, oder beides?

Dem Benutzer soll dadurch ein Verständnis darüber vermittelt werden, wie die Benutzeroberfläche angeordnet ist und wie die behandelten Steuerelemente ermittelt werden. Wenn Sie erkennen, dass zu viele Tabstopps vorhanden sind, bevor der Benutzer die Navigationsschleife abgeschlossen hat, können Sie in Betracht ziehen, verwandte Steuerelemente gemeinsam zu gruppieren. Einige verknüpfte Steuerelemente, z. B. ein Hybrid-Steuerelement, müssen eventuell in diesem frühen Stadium behandelt werden. Nach begonnener Produktentwicklung ist es schwierig, die Tastaturnavigation zu überarbeiten. Planen Sie daher sorgfältig und frühzeitig!  

Weitere Informationen zur Navigation zwischen Benutzeroberflächenelementen mithilfe der Tastatur finden Sie unter [Tastaturzugriff](keyboard-accessibility.md).  

Darüber hinaus enthält das E-Book [Engineering-Software für Eingabehilfen](https://www.microsoft.com/en-us/download/details.aspx?id=19262) ein hervorragendes Kapitel zu diesem Thema mit dem Titel _Designing the Logical Hierarchy_ (Entwerfen der logischen Hierarchie).

## Farbe und Kontrast  
Eine integrierte Eingabehilfe in Windows ist der Modus mit hohem Kontrast, der den Farbkontrast von Texten und Bildern auf dem Bildschirm erhöht. Bei manchen Personen führt ein erhöhter Farbkontrast zu einer Reduzierung der Belastung für die Augen und einer besseren Lesbarkeit. Wenn Sie Ihre Benutzeroberfläche mit hohem Kontrast überprüfen, dann stellen Sie sicher, dass die Steuerelemente konsistent und mit Systemfarben (nicht mit hartcodierten Farben) codiert wurden, um zu gewährleisten, dass alle Steuerelemente auf dem Bildschirm angezeigt werden, die ein Benutzer sehen würde, der den hohen Kontrast nicht verwendet.  

XAML
```xml
<Button Background="{ThemeResource ButtonBackgroundThemeBrush}">OK</Button>
```
Weitere Informationen zur Verwendung von Systemfarben und Ressourcen finden Sie unter [XAML-Designressourcen](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/xaml-theme-resources).

Sofern Sie Systemfarben nicht außer Kraft gesetzt haben, unterstützt eine UWP-App Designs mit hohem Kontrast standardmäßig. Wenn sich ein Benutzer für die Verwendung eines Designs mit hohem Kontrast aus den Systemeinstellungen oder Tools für die Barrierefreiheit entscheidet, werden vom Framework automatisch Farben und Stileinstellungen verwendet, mit denen für Steuerelemente und Komponenten auf der Benutzeroberfläche ein Layout und Rendering mit hohem Kontrast entsteht.   

Weitere Informationen finden Sie unter [Designs mit hohem Kontrast](high-contrast-themes.md).  

Beachten Sie diese Richtlinien, wenn Sie sich entschieden haben, Ihr eigenes Farbdesign anstelle von Systemfarben zu verwenden:  

**Farbkontrastverhältnis** – Der aktualisierte Abschnitt 508 des Gesetzes für Amerikaner mit Behinderung (Americans with Disability Act) sowie andere Rechtsvorschriften erfordern, dass der standardmäßige Farbkontrast zwischen Text und Hintergrund das Verhältnis 5:1 aufweisen muss. Für großen Text (Schriftgröße 18 oder 14 fett markiert) beträgt das erforderliche Kontrastverhältnis 3:1.  

**Farbkombinationen** – Etwa 7 Prozent des Männer (und weniger als 1 Prozent der Frauen) weisen ein Farbdefizit auf. Benutzer mit Farbenblindheit haben Probleme bei der Unterscheidung zwischen bestimmten Farben. Es ist daher wichtig, dass niemals nur Farben verwendet werden, um Status oder Bedeutung in einer Anwendung zu vermitteln. Für dekorative Bilder (wie Symbole oder Hintergründe) sollten Farbkombinationen so gewählt werden, dass die Wahrnehmung des Bilds durch den farbenblinden Benutzer maximiert wird.  

## Prüfliste für die Barrierefreiheit  
Es folgt nun eine gekürzte Version der Prüfliste für die Barrierefreiheit:  
1. Legen Sie den Namen (erforderlich) und die Beschreibung (optional) zur Verwendung durch Bildschirmleseprogramme für den Inhalt und die interaktiven UI-Elemente Ihrer App fest.
2. Implementieren Sie Barrierefreiheit für den Tastaturzugriff.
3. Schauen Sie sich die Benutzeroberfläche an, um sicherzustellen, dass der Textkontrast ausreicht, Elemente in Designs mit hohem Kontrast richtig dargestellt werden und Farben korrekt verwendet werden.
4. Führen Sie Tools zum Testen der Barrierefreiheit aus. Behandeln Sie gemeldete Probleme und überprüfen Sie die Qualität der Sprachausgabe. (Mehr über Testen der Barrierefreiheit erfahren)
5. Stellen Sie sicher, dass die App-Manifesteinstellungen den Richtlinien für Barrierefreiheit entsprechen.
6. Deklarieren Sie Ihre App im Windows Store als barrierefrei. (Siehe das Thema [Eingabehilfen im Store](accessibility-in-the-store.md))

Weitere Details finden Sie im vollständigen Thema [Prüfliste für Barrierefreiheit](accessibility-checklist.md).

## Verwandte Themen  
* [Entwerfen von inklusiver Software](designing-inclusive-software.md)  
* [Inklusives Design](http://design.microsoft.com/inclusive)
* [Nicht empfehlenswerte Praktiken für die Barrierefreiheit](practices-to-avoid.md)
* [Entwickeln von barrierefreier Software](https://www.microsoft.com/en-us/download/details.aspx?id=19262)
* [Microsoft-Hub für die barrierefreie Entwicklung](https://msdn.microsoft.com/enable)


<!--HONumber=May16_HO2-->


