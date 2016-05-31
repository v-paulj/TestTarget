---
author: mijacobs
Description: Dieser Artikel beschreibt die Features, Vorteile und Anforderungen der Plattform für UWP-Apps (Universelle Windows-Plattform) aus Entwurfsperspektive. Hier erfahren Sie, welche Komponenten die Plattform kostenlos zur Verfügung stellt und welche Tools verfügbar sind.
title: Einführung in das UWP-App-Design
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
label: Intro to UWP app design
template: detail.hbs
---

#  Einführung in das UWP-App-Design 

Eine App für die Universelle Windows-Plattform (UWP) kann auf beliebigen Windows-Geräten ausgeführt werden – Smartphones, Tablets oder PCs.

![Windows-Geräte](images/1894834-hig-device-primer-01-500.png)

Der Entwurf einer App, die auf so vielen verschiedenen mobilen Geräten gut aussieht, kann eine große Herausforderung darstellen. Wie also entwerfen Sie eine App, die ein hervorragendes Benutzererlebnis auf Geräten bietet, die über deutlich unterschiedliche Bildschirmgrößen und verschiedene Eingabemethoden verfügen? Glücklicherweise bietet die Universelle Windows-Plattform (UWP) eine Reihe von integrierten Funktionen und universellen Bausteinen, mit denen Sie genau dies tun können. 

![ein Design für eine App, die auf Windows-Telefonen, Tablets und PCs ausgeführt werden kann](images/food-truck-finder/uap-foodtruck--md-detail.png)

Dieser Artikel beschreibt die UI-Features und Vorteile von UWP-Apps und gibt allgemeine Designtipps zum Erstellen Ihrer ersten UWP-App. Lassen Sie uns zunächst einen Blick auf einige der Features werfen, die Sie erhalten, wenn Sie eine UWP-App erstellen. 

## UWP-App-Features

### Effektive Pixel und Skalierung

UWP-Apps passen automatisch die Größe von Steuerelementen, Schriftarten und anderer UI-Elemente an, damit sie auf allen Geräten lesbar sind.

Wenn Ihre App auf einem Gerät ausgeführt wird, verwendet das System einen Algorithmus, um die Art der Anzeige der UI-Elemente auf dem Bildschirm zu normalisieren. Dieser Skalierungsalgorithmus berücksichtigt den Abstand zum Bildschirm und die Bildschirmdichte (Pixel pro Zoll), um die wahrgenommene Größe (anstelle der physischen Größe) zu optimieren. Mit dem Skalierungsalgorithmus wird sichergestellt, dass der Schriftgrad 24 Pixel auf einem 3 Meter entfernten Surface Hub genauso für den Benutzer lesbar ist wie der Schriftgrad 24 Pixel auf einem 5-Zoll-Smartphone, das nur einige Zentimeter entfernt ist.

![Sichtabstände für verschiedene Geräte](images/1910808-hig-uap-toolkit-03.png)

Aufgrund der Funktionsweise des Skalierungssystems beim Entwerfen Ihrer UWP-App, verwenden Sie *effektive Pixeln*, und nicht die tatsächlichen physischen Pixel. Welche Auswirkungen hat dies beim Entwerfen Ihrer App?

-   Sie können die Pixeldichte und die tatsächliche Bildschirmauflösung beim Entwerfen ignorieren. Entwerfen Sie stattdessen für die effektive Auflösung (die Auflösung in effektiven Pixeln) für eine Größenklasse (Größenklassen werden [weiter unten in diesem Artikel](#sizeclasses) definiert).

-   Wenn das System die Benutzeroberfläche skaliert, erfolgt dies durch die Multiplikation mit vier. Um eine scharfe Darstellung zu gewährleisten, docken Sie Ihre Entwürfe an das 4 x 4-Pixelraster an: Stellen Sie Ränder, Größen und Positionen von UI-Elementen (nicht jedoch die Größe, Text kann eine beliebige Größe aufweisen) und die Position des Textes als ein Vielfaches von 4 in effektiven Pixeln dar.

Diese Abbildung zeigt Designelemente, die dem 4 x 4-Pixelraster zugeordnet sind. Das Designelement wird immer scharfe Kanten aufweisen.

![Andocken an das 4x4-Pixelraster](images/rsp-design/epx-4pixelgood.png)

Die nächste Abbildung zeigt Designelemente, die nicht an das 4 x 4-Raster angedockt werden. Auf einigen Geräten werden diese Designelemente verschwommene, weiche Kanten aufweisen.

![Designelemente, die nicht am 4 x 4-Pixelraster ausgerichtet werden](images/rsp-design/offthegridillustration.png)

**Tipp**   Legen Sie beim Erstellen von Bildschirmmodellen in Bildbearbeitungsprogrammen die DPI auf 72 und die Bildgröße auf effektive Auflösung für die Zielgrößenklasse fest. (Eine Liste der Größenklassen und effektiven Auflösungen finden Sie im Abschnitt [Empfehlungen für bestimmte Größenklassen](#sizeclasses) dieses Artikels.)


### Universelle Eingabe und Smart-Interaktionen

Eine andere integrierte Funktion von UWP ist universelle Eingabe über Smart-Interaktionen. Obwohl Sie Ihre Apps für bestimmte Eingabemethoden und Geräte entwerfen können, müssen Sie dies nicht unbedingt tun. Der Grund dafür ist, dass Apps für die universelle Windows-Plattform standardmäßig auf Smart-Interaktionen basieren. Dies bedeutet, dass Sie um eine Klick-Interaktion herum entwerfen können, ohne zu wissen oder festzulegen, ob der Klick von einem echten Mausklick oder einem Fingertipp stammt.

### Universelle Steuerelemente und Stile


Die UWP bietet auch einige nützliche Bausteine, die den Entwurf von Apps für mehrere Gerätefamilien vereinfachen.

-   **Universelle Steuerelemente**

    Die UWP bietet einen Satz von universellen Steuerelementen, die garantiert gut auf allen Windows-Geräten funktionieren. Dieser Satz universeller Steuerelemente enthält allgemeine Formularsteuerelemente wie Optionsschaltflächen und Textfelder bis hin zu komplexen Steuerelementen wie Rasteransicht und Listenansicht, mit denen Listen an Elementen aus einem Datenstrom und einer Vorlage generiert werden können. Diese Steuerelemente sind eingabebewusst und werden mit dem richtigen Satz an Eingabeaufforderungen, Ereigniszuständen und allgemeiner Funktionalität für jede Gerätefamilie bereitgestellt.

    Eine vollständige Liste dieser Steuerelemente und der Muster, die Sie aus ihnen erstellen können, finden Sie im Abschnitt [Steuerelemente und Muster](https://dev.windows.com/design/controls-patterns).

-   **Universelle Stile**

    Ihre UWP-App erhält automatisch eine Reihe von Standardstilen, die diese Features bieten:

    -   Eine Reihe von Stilen, die Ihrer App automatisch ein helles oder dunkles Design (Ihrer Wahl) verleihen und die die bevorzugte Akzentfarbe des Benutzers integrieren können.

        ![helles und dunkles Design](images/1910808-hig-uap-toolkit-01.png)

    -   Eine Segoe-basierter Typenverlauf, der sicherstellt, dass der App-Text auf allen Geräten scharf dargestellt wird.
    -   Standardanimationen für Interaktionen.
    -   Automatische Unterstützung von Modi mit hohem Kontrast. Bei der Entwicklung unserer Stile wurden hohe Kontraste berücksichtigt, sodass Ihre App auch auf einem Gerät im Modus mit hohem Kontrast gut dargestellt wird.
    -   Automatische Unterstützung anderer Sprachen. Die Standardstile wählen automatisch die richtige Schriftart für jede Sprache aus, die Windows unterstützt. Sie können sogar mehrere Sprachen in derselben App verwenden, die dann ebenfalls richtig dargestellt werden.
    -   Integrierte Unterstützung für die Leserichtung von rechts nach links.

    Sie können diese Standardstile anpassen, um Ihrer App eine persönliche Note zu verleihen, oder Sie können sie vollständig durch Ihre eigene visuelle Benutzeroberfläche erstellen. Hier sehen Sie als Beispiel ein Design für eine Wetter-App mit einem einzigartigen visuellen Stil:

    ![eine Wetter-App mit eigenem visuellem Stil](images/weather/uwp-weather-tab-phone-700.png)

Nachdem wir die Bausteine von UWP-Apps beschrieben haben, sehen wir uns jetzt an, wie diese zu einer UI zusammengefügt werden. 
    
## Die Anatomie einer typischen UWP-App


Moderne Benutzeroberflächen sind äußerst komplex und bestehen aus Textelementen, Formen, Farben und Animationen, die sich wiederum aus den einzelnen Pixeln des Bildschirms des verwendeten Geräts zusammensetzen. Wenn Sie eine Benutzeroberfläche entwerfen, kann die reine Anzahl an Möglichkeiten verwirrend sein.

Der Einfachheit halber sehen wir uns den Aufbau einer App aus der Entwurfsperspektive an. Nehmen wir an, dass eine App aus Bildschirmen und Seiten besteht. Jede Seite verfügt über eine Benutzeroberfläche, die sich aus drei Arten von UI-Elementen zusammensetzt: Navigation, Befehle und Inhalt.



<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><img src="images/1895065-hig-anatomyofanapp-02.png" alt="Navigation, command, and content areas of an address book app" /></p>
<p></p></td>
<td align="left"><strong>Navigationselemente</strong>
<p>Mithilfe von Navigationselementen können Benutzer die Inhalte auswählen, die sie anzeigen möchten. Zu den Navigationselementen zählen beispielsweise [Registerkarten und Pivots](../controls-and-patterns/tabs-pivot.md), [Hyperlinks](../controls-and-patterns/hyperlinks.md) und [Navigationsbereiche](../controls-and-patterns/nav-pane.md).</p>
<p>Navigationselemente werden ausführlich im Artikel [Navigationsdesigngrundlagen](navigation-basics.md) behandelt.</p>
<strong>Befehlselemente</strong>
<p>Befehlselemente initiieren Aktionen wie etwa das Bearbeiten, Speichern oder Freigeben von Inhalten. Zu den Befehlselementen zählen beispielsweise [Schaltflächen](../controls-and-patterns/buttons.md) und die [Befehlsleiste](../controls-and-patterns/app-bars.md). Zu den Befehlselementen können auch Tastenkombinationen gehören, die nicht auf dem Bildschirm angezeigt werden.</p>
<p>Befehlselemente werden ausführlich im Artikel [Befehlsdesigngrundlagen](commanding-basics.md) behandelt.</p>
<strong>Inhaltselemente</strong>
<p>Die Inhaltselemente zeigen die Inhalte der App an. Bei einer Zeichen-App kann als Inhalt beispielsweise eine Zeichnung und bei einer Nachrichten-App beispielsweise ein Nachrichtenartikel angezeigt werden.</p>
<p>Inhaltselemente werden ausführlich im Artikel [Inhaltsdesigngrundlagen](content-basics.md) behandelt.</p></td>
</tr>
</tbody>
</table>

 

Eine App verfügt mindestens über einen Begrüßungsbildschirm und eine Startseite, die die Benutzeroberfläche definiert. Eine typische App besitzt mehrere Seiten und Bildschirme, und die Navigations-, Befehls- und Inhaltselemente können je nach Seite variieren.

Bei der Entscheidung für die UI-Elemente für Ihre App sollten Sie die Geräte und Bildschirmgrößen berücksichtigen, auf denen Ihre App ausgeführt werden wird.

## <span id="Why_tailor_your_app_for_specific_device_families_and_screen_sizes_"></span><span id="why_tailor_your_app_for_specific_device_families_and_screen_sizes_"></span><span id="WHY_TAILOR_YOUR_APP_FOR_SPECIFIC_DEVICE_FAMILIES_AND_SCREEN_SIZES_"></span>Anpassen Ihrer App an bestimmte Geräte und Bildschirmgrößen


UWP-Apps verwenden effektive Pixel, um sicherzustellen, dass Ihre Designelemente lesbar sind und auf allen Geräten mit Windows verwendet werden können. Warum sollten also Sie überhaupt die Benutzeroberfläche Ihrer App für eine bestimmte Gerätefamilie anpassen wollen?

**Hinweis**  
Bevor wir fortfahren, möchten wir Sie darauf aufmerksam machen, dass Windows keine Möglichkeit für Ihre App zum Erkennen des Geräts bietet, auf dem sie ausgeführt wird. Sie kann die Gerätefamilie des Geräts (mobil, Desktop usw.), auf dem sie ausgeführt wird, die effektive Auflösung und den für die App verfügbaren Bildschirmbereich (Größe des App-Fensters) erkennen.

 

-   **Effektive Bildschirmbereichnutzung und reduzierte Navigation**

    Wenn Sie eine App für ein Gerät mit einem kleinen Bildschirm entwickeln, z. B. Smartphone, kann die App auf einem PC mit einem viel größeren Bildschirm verwendet werden, eine Menge des Bildschirmbereichs bleibt jedoch vermutlich ungenutzt. Sie können die App anpassen, damit mehr Inhalt angezeigt wird, wenn eine bestimmte Bildschirmgröße überschritten wird. Bei einer Shopping-App beispielsweise wird auf einem Smartphone ggf. eine Kategorie zum bestimmten Zeitpunkt anzeigt, während auf einem PC oder Laptop mehrere Kategorien und Produkte gleichzeitig angezeigt werden.

    Durch das Platzieren von mehr Inhalt auf dem Bildschirm reduzieren Sie die erforderliche Navigation des Benutzers.

-   **Profitieren von Gerätefunktionen**

    Bestimmte Geräte verfügen über bestimmte Gerätefunktionen. Smartphones z. B. verfügen wahrscheinlich über einen Positionssensor und eine Kamera, während ein PC darüber möglicherweise nicht verfügt. Ihre App kann erkennen, welche Funktionen verfügbar sind und Features die Verwendung dieser ermöglichen.

-   **Optimieren für Eingabe**

    Die universelle Steuerelementbibliothek kann mit allen Eingabetypen (Toucheingabe, Stift, Tastatur, Maus) verwendet werden. Sie können jedoch eine Optimierung für bestimmte Eingabetypen erreichen, indem Sie Ihre UI-Elemente neu anordnen. Wenn Sie z. B. Elemente für die Navigation am unteren Bildschirmrand platzieren, ist der Zugriff auf diese für Smartphonebenutzer einfacher; die meisten PC-Benutzer hingegen erwarten, dass Elemente für die Navigation eher am oberen Bildschirmrand angezeigt werden.

## <span id="Responsive_design_techniques"></span><span id="responsive_design_techniques"></span><span id="RESPONSIVE_DESIGN_TECHNIQUES"></span>Reaktionsfähige Designtechniken


Wenn Sie die Benutzeroberfläche Ihrer App für bestimmte Bildschirmbreiten optimieren, spricht man vom Erstellen eines reaktionsfähigen Designs. Im folgenden werden sechs reaktionsfähige Designtechniken aufgeführt, mit denen Sie die Benutzeroberfläche Ihrer App anpassen können:

### <span id="Reposition"></span><span id="reposition"></span><span id="REPOSITION"></span>Ändern der Position

Sie können den Ort und die Position der UI-Elemente der App ändern, um auf jedem Gerät ein optimales Ergebnis zu erzielen. In diesem Beispiel ist für das Hochformat auf Smartphone oder Phablet ein Bildlauf der Benutzeroberfläche erforderlich, da nur ein gesamter Frame zu einem Zeitpunkt sichtbar ist. Wenn die App auf einem Gerät mit zwei vollständigen Frames verwendet wird, egal ob im Hoch- oder im Querformat, kann Frame B den dedizierten Speicherplatz belegen. Wenn Sie ein Raster für die Positionierung verwenden, können Sie das gleiche Raster für eine Neuanordnung der UI-Elemente verwenden.

![Ändern der Position](images/rsp-design/rspd-reposition.png)

In diesem Beispielentwurf einer Foto-App ändert die Foto-App die Position des Inhalts auf größeren Bildschirmen.

![ein Design für eine App, die Inhalte auf größeren Bildschirm neu positioniert](images/rsp-design/rspd-reposition-type1.png)

### <span id="Resize"></span><span id="resize"></span><span id="RESIZE"></span>Ändern der Größe

Sie können die Framegröße optimieren, indem Sie die Ränder und die Größe der UI-Elemente anpassen. Dadurch können Sie, wie im Beispiel dargestellt, die Lesbarkeit auf einem größeren Bildschirm verbessern, indem Sie den Inhaltsframe vergrößern.

![Ändern der Größe von Designelementen](images/rsp-design/rspd-resize.png)

### <span id="Reflow"></span><span id="reflow"></span><span id="REFLOW"></span>Neuanordnen

Durch Änderung der Anordnung von UI-Elementen basierend auf dem Gerät und der Ausrichtung kann Ihre App eine optimale Ansicht der Inhalte bieten. Beim Wechseln zu einem größeren Bildschirm beispielsweise ist es ggf. sinnvoll zu größeren Containern zu wechseln, Spalten hinzuzufügen und Listenelemente auf andere Weise zu generieren.

Dieses Beispiel zeigt, wie eine einzelne Spalte mit Inhalt für vertikalen Bildlauf auf dem Smartphone oder Phablet auf einem größeren Bildschirm zur Anzeige von zwei Textspalten neu angeordnet werden kann.

![Neuanordnen von Designelementen](images/rsp-design/rspd-reflow.png)

### <span id="_____________Reveal___________"></span><span id="_____________reveal___________"></span><span id="_____________REVEAL___________"></span> Einblenden

Das Einblenden von UI-Elementen kann von der Bildschirmfläche sowie davon abhängig gemacht werden, ob das Gerät zusätzliche Funktionen, bestimmte Situationen oder bevorzugte Bildschirmausrichtungen unterstützt.

In diesem Beispiel mit Registerkarten ist die mittlere Registerkarte mit dem Kamerasymbol möglicherweise für die App auf einem Smartphone oder Phablet bestimmt und bei größeren Geräten nicht anwendbar. Deshalb ist sie auf dem Gerät auf der rechten Seite eingeblendet. Ein weiteres häufiges Beispiel für das Einblenden oder Ausblenden der Benutzeroberfläche bezieht sich auf Media Player-Steuerelemente. Hierbei sind die Schaltflächen bei kleineren Geräten minimiert und werden bei Geräten mit größerem Bildschirm vergrößert. Der Media Player auf dem PC kann z. B. wesentlich mehr Funktionen auf dem Bildschirm anzeigen als auf einem Smartphone.

![Ausblenden von Designelementen](images/rsp-design/rspd-revealhide.png)

Die Methode zum Ein- und Ausblenden umfasst die Wahl, wann mehr Metadaten angezeigt werden sollen. Wenn der verfügbare Bildschirmbereich knapp ist, z. B. bei einem Smartphone oder einem Phablet, wird empfohlen, eine minimale Menge von Metadaten anzuzeigen. Mit einem Laptop oder Desktop-PC kann eine wesentlich höhere Menge von Metadaten angezeigt werden. Einige Beispiele zur Verwendung von Ein- oder Ausblenden für Metadaten umfassen:

-   In einer E-Mail-App können Sie den Avatar des Benutzers anzeigen.
-   In einer Musik-App können Sie weitere Informationen zu einem Album oder Interpreten anzeigen.
-   In einer Video-App können Sie weitere Informationen zu einem Film oder einer Show anzeigen, z. B. Details zu Besetzung und Crew.
-   In jeder App können Sie Spalten aufteilen und mehr Details anzeigen.
-   In jeder App können Sie etwas vertikal oder horizontal anordnen. Beim Wechseln von Smartphone oder Phablet auf größere Geräte, können aus gestapelten Listenelementen Zeilen mit Listenelementen und Spalten mit Metadaten werden.

### <span id="Replace"></span><span id="replace"></span><span id="REPLACE"></span>Ersetzen

Mit diesem Verfahren kann die Benutzeroberfläche für eine bestimmte Gerätegrößenklasse oder Ausrichtung geändert werden. In diesem Beispiel ist der Navigationsbereich mit seiner kompakten, vorübergehenden Benutzeroberfläche gut für kleinere Geräte geeignet, auf einem größeren Gerät stellen jedoch Registerkarten u. U. die bessere Wahl dar.

![Ersetzen von Designelementen](images/rsp-design/rspd-replace.png)

### <span id="_____________Re-architect___________"></span><span id="_____________re-architect___________"></span><span id="_____________RE-ARCHITECT___________"></span> Ändern der Architektur

Sie können die Architektur Ihrer App reduzieren oder erweitern, um eine bessere Darstellung für bestimmte Geräte zu erzielen. In diesem Beispiel wird, vom linken Gerät zum rechten, das Verknüpfen von Seiten veranschaulicht.

![ein Beispiel für das erneute Erstellen der Architektur einer Benutzeroberfläche](images/rsp-design/rspd-rearchitect.png)

Hier sehen Sie ein Beispiel für diese Methode, die beim Entwerfen einer Smart Home-App angewendet wird.

![ein Beispiel für einen Entwurf, der die Technik zum erneuten Erstellen des reaktionsfähigen Designs nutzt](images/rsp-design/rspd-rearchitect-type1.png)


## Verwandte Artikel

- [Was ist eine UWP-App?](https://msdn.microsoft.com/library/windows/apps/dn726767.aspx)

 






<!--HONumber=May16_HO2-->


