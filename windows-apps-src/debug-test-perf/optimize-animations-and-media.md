---
ms.assetid: DE5B084C-DAC1-430B-A15B-5B3D5FB698F7
title: Optimieren von Animationen, Medien und Bildern
description: Erstellen Sie UWP-Apps (Universelle Windows-Plattform) mit flüssigen Animationen, hoher Bildfrequenz und leistungsstarker Medienaufzeichnung und -wiedergabe.
---
# Optimieren von Animationen, Medien und Bildern

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Erstellen Sie UWP-Apps (Universelle Windows-Plattform) mit flüssigen Animationen, hoher Bildfrequenz und leistungsstarker Medienaufzeichnung und -wiedergabe.

## Reibungsloses Einbinden von Animationen

Ein wesentlicher Aspekt der UWP-Apps sind reibungslose Interaktionen. Dazu zählen Fingereingabemanipulationen, die sozusagen am Finger „kleben“, flüssige Übergänge und Animationen sowie kleine Bewegungen, die Eingabefeedback liefern. Das XAML-Framework verfügt über einen so genannten Kompositionsthread, der für die Zusammenstellung und Animation der visuellen Elemente einer App vorgesehen ist. Da der Kompositionsthread vom UI-Thread (dem Thread, der den Framework- und den Entwicklercode ausführt) getrennt ist, können Apps unabhängig von komplizierten Layoutdurchläufen oder erweiterten Berechnungen eine gleichmäßige Bildfrequenz und flüssige Animationen erzielen. In diesem Abschnitt erfahren Sie, wie Sie mithilfe des Kompositionsthreads butterweiche App-Animationen auf den Bildschirm zaubern. Weitere Informationen zu Animationen finden Sie unter [Übersicht über Animationen](https://msdn.microsoft.com/library/windows/apps/Mt187350). Informationen zum Verbessern der Reaktionsfähigkeit bei rechenintensiven Vorgängen finden Sie unter [Sicherstellen der Reaktionsfähigkeit des UI-Threads](keep-the-ui-thread-responsive.md).

### Verwenden unabhängiger Animationen anstelle von abhängigen Animationen

Unabhängige Animationen können zum Zeitpunkt der Erstellung von Anfang bis Ende berechnet werden, da sich Änderungen an der animierten Eigenschaft nicht auf die restlichen Objekte einer Szene auswirken. Unabhängige Animationen können daher im Kompositionsthread und nicht im UI-Thread ausgeführt werden. Dadurch ist gewährleistet, dass die Animationen flüssig verlaufen, denn der Kompositionsthread wird in einem gleichmäßigen Rhythmus aktualisiert.

Alle diese Animationsarten sind garantiert unabhängig:

-   Objektanimationen mit Keyframes
-   Animationen ohne Dauer
-   Animationen der Eigenschaften [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/Hh759771) und [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/Hh759772)
-   Animationen der [**UIElement.Opacity**](https://msdn.microsoft.com/library/windows/apps/BR208962)-Eigenschaft
-   Animationen von Eigenschaften vom Typ [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076), wenn auf die untergeordnete Eigenschaft [**SolidColorBrush.Color**](https://msdn.microsoft.com/library/windows/apps/BR242963) abgezielt wird
-   Animationen der folgenden [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911)-Eigenschaften, wenn auf untergeordneten Eigenschaften dieser Rückgabewerttypen abgezielt wird:

    -   [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208911-rendertransform)
    -   [**Projection**](https://msdn.microsoft.com/library/windows/apps/BR208911-projection)
    -   [**Clip**](https://msdn.microsoft.com/library/windows/apps/BR208911-clip)

Abhängige Animationen wirken sich auf das Layout aus und können nicht ohne zusätzliche Angaben aus dem UI-Thread berechnet werden. Abhängige Animationen umfassen Änderungen an Eigenschaften wie [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) und [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718). Abhängige Animationen werden standardmäßig nicht ausgeführt und erfordern die Zustimmung des App-Entwicklers. Werden sie aktiviert, laufen sie flüssig ab, solange der UI-Thread nicht blockiert wird. Ist das Framework oder die App dagegen stark mit anderen Vorgängen des UI-Threads ausgelastet, beginnen die Animationen zu ruckeln.

Nahezu alle Animationen im XAML-Framework sind standardmäßig unabhängig. Sie können diese Optimierung jedoch mithilfe einiger Schritte deaktivieren. Vorsicht ist besonders bei folgenden Szenarien geboten:

-   Beim Festlegen der [**EnableDependentAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210356)-Eigenschaft, um die Ausführung einer abhängigen Animation im UI-Thread zu ermöglichen. Beim Konvertieren dieser Animationen in eine unabhängige Version. Animieren Sie beispielsweise [**ScaleTransform.ScaleX**](https://msdn.microsoft.com/library/windows/apps/BR242946) und [**ScaleTransform.ScaleY**](https://msdn.microsoft.com/library/windows/apps/BR242948) anstelle der Eigenschaften [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) und [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) eines Objekts. Objekte wie Bilder und Text können ohne Weiteres skaliert werden. Das Framework wendet die bilineare Skalierung nur während der Animierung von [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940) an. Bilder/Texte werden in der endgültigen Größe neu gerastert, um zu gewährleisten, dass sie stets gut erkennbar sind.
-   Bei Verwendung framespezifischer Updates. Hierbei handelt es sich de facto um abhängige Animationen. Ein Beispiel hierfür ist das Anwenden von Transformationen im Handler des [**CompositonTarget.Rendering**](https://msdn.microsoft.com/library/windows/apps/BR228127)-Ereignisses.
-   Beim Ausführen einer beliebigen, als unabhängig geltenden Animation in einem Element, bei dem die [**CacheMode**](https://msdn.microsoft.com/library/windows/apps/BR208911-cachemode)-Eigenschaft auf **BitmapCache** festgelegt ist. Dies wird als abhängige Animation betrachtet, da der Cache für jeden Frame neu gerastert werden muss.

### Animieren Sie keine Webansichten und keine Medienelemente.

Webinhalte innerhalb eines [**WebView**](https://msdn.microsoft.com/library/windows/apps/BR227702)-Steuerelements werden nicht direkt vom XAML-Framework gerendert. Zudem ist das Zusammenfügen mit dem Rest der Szene mit zusätzlichem Aufwand verbunden. Dieser Zusatzaufwand erhöht sich, wenn das animierte Steuerelement über den Bildschirm bewegt wird. Dies kann unter Umständen zu Synchronisierungsproblemen führen (der HTML-Inhalt könnte z. B. nicht synchron mit dem restlichen XAML-Inhalt der Seite sein). Falls Sie ein **WebView**-Steuerelement animieren möchten, ersetzen Sie es für die Dauer der Animation durch ein [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227702brush)-Objekt.

Auch vom Animieren eines [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/BR242926)-Objekts wird abgeraten. Abgesehen von den Leistungseinbußen kann dies im wiedergegebenen Videoinhalt zu Bildstörungen und anderen Artefakten führen.

### Setzen Sie Endlosanimationen sparsam ein.

Die meisten Animationen werden für einen bestimmten Zeitraum ausgeführt. Falls Sie die [**Timeline.Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207)-Eigenschaft dagegen auf „Forever“ festlegen, kann die Animation ohne zeitliche Begrenzung ausgeführt werden. Wir empfehlen, Endlosanimationen möglichst sparsam einzusetzen, da solche Animationen fortlaufend CPU-Ressourcen verbrauchen. Dadurch kann die CPU unter Umständen nicht in den Stromsparmodus oder in den Leerlauf wechseln, was wiederum einen höheren Energieverbrauch zur Folge hat.

Das Hinzufügen eines Handlers für [**CompositionTarget.Rendering**](https://msdn.microsoft.com/library/windows/apps/BR228127) ist vergleichbar mit dem Ausführen eines Endlosanimation. Normalerweise ist der UI-Thread nur aktiv, wenn es etwas für ihn zu tun gibt. Fügen Sie jedoch einen Handler für dieses Ereignis hinzu, muss er in jedem Frame ausgeführt werden. Entfernen Sie den Handler, wenn es nichts zu tun gibt, und registrieren Sie ihn erneut, wenn er wieder benötigt wird.

### Nutzen Sie die Animationsbibliothek.

Der [**Windows.UI.Xaml.Media.Animation**](https://msdn.microsoft.com/library/windows/apps/BR243232)-Namespace umfasst eine Bibliothek mit schnellen Hochleistungsanimationen, deren Erscheinungsbild mit anderen Windows-Animation konsistent ist. Die relevanten Klassen enthalten „Design“ in ihrem Namen und werden in [Übersicht über Animationen](https://msdn.microsoft.com/library/windows/apps/Mt187350) beschrieben. Diese Bibliothek unterstützt viele gängige Animationsszenarien, z. B. die Animation der ersten Ansicht einer App oder die Erstellung von Zustands- und Inhaltsübergängen. Wir empfehlen, die Animationsbibliothek so oft wie möglich zu nutzen, um die Leistung und Konsistenz für UWP-UI zu verbessern.

> **Hinweis**   Von der Animationsbibliothek können nicht alle möglichen Eigenschaften animiert werden. Informationen zu XAML-Szenarien, bei denen die Animationsbibliothek nicht verwendet werden kann, finden Sie unter [Storyboardanimationen](https://msdn.microsoft.com/library/windows/apps/Mt187354).


### Eigenständiges Animieren von CompositeTransform3D-Eigenschaften

Sie können jede [**CompositeTransform3D**](https://msdn.microsoft.com/library/windows/apps/Dn914714)-Eigenschaft eigenständig animieren. Wenden Sie daher nur die Animationen an, die Sie benötigen. Beispiele und weitere Informationen finden Sie unter [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/BR208911-transform3d). Weitere Informationen zum Animieren von Transformationen finden Sie unter [Storyboardanimationen](https://msdn.microsoft.com/library/windows/apps/Mt187354) und [Keyframeanimationen und Animationen für Beschleunigungsfunktionen](https://msdn.microsoft.com/library/windows/apps/Mt187352).

## Optimieren von Medienressourcen

Audio, Video und Bilder sind verlockende Arten von Inhalten, die in den meisten Apps zum Einsatz kommen. Da immer mehr Medieninhalte aufgezeichnet und anstelle von SD-Qualität immer häufiger HD-Qualität verwendet wird, steigt der Ressourcenbedarf für die Speicherung, Decodierung und Wiedergabe dieser Inhalte. Das XAML-Framework nutzt die neuesten Features der UWP-Medienmodule, sodass Apps kostenlos von diesen Verbesserungen profitieren. Hier werden einige zusätzliche Tricks erläutert, mit denen Sie Medien in Ihrer UWP-App optimal nutzen können.

### Freigeben von Mediendatenströmen

Mediendateien sind die gängigsten und aufwendigsten Ressourcen, die von Apps genutzt werden. Da Mediendateien den Speicherbedarf Ihrer App beträchtlich steigern können, ist es wichtig, das Handle für die Medien unverzüglich freizugeben, sobald die App sie nicht mehr verwendet.

Wenn Ihre App also beispielsweise ein [**RandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/BR241747)- oder [**IInputStream**](https://msdn.microsoft.com/library/windows/apps/BR241718)-Objekt verwendet, müssen Sie die close-Methode für das Objekt aufrufen, sobald die App es nicht mehr verwendet. Dadurch wird das zugrundeliegende Objekt freigegeben.

### Anzeigen der Videowiedergabe im Vollbildmodus (sofern möglich)

Verwenden Sie in UWP-Apps immer die [**IsFullWindow**](https://msdn.microsoft.com/library/windows/apps/BR242926-isfullwindow)-Eigenschaft für das [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/BR242926), um das vollständige Rendern des Fensters zu aktivieren bzw. zu deaktivieren. Dies stellt sicher, dass während der Medienwiedergabe Optimierungen auf Systemebene genutzt werden.

Das XAML-Framework kann die Darstellung von Videoinhalten optimieren, wenn nur das Video gerendert wird. Dies senkt den Energieverbrauch und erhöht die Bildfrequenz. Um eine möglichst effiziente Medienwiedergabe zu erhalten, legen Sie die Größe eines [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/BR242926)-Objekts auf die Breite und Höhe des Bildschirms fest. Zeigen Sie außerdem keine anderen XAML-Elemente an.

Es gibt durchaus berechtigte Gründe für die Überlagerung von XAML-Elementen auf einem [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/BR242926)-Objekt, das die gesamte Höhe und Breite des Bildschirms einnimmt – beispielsweise für Untertitel oder kurzzeitig angezeigte Transportsteuerelemente. Durch Ausblenden dieser Elemente (beispielsweise durch Festlegen von „Visibility='Collapsed'“), wenn sie nicht benötigt werden, kehrt die Medienwiedergabe direkt in den effizientesten Zustand zurück.

### Deaktivieren des Displays und Einsparen von Energie

Um ein Deaktivieren des Displays zu verhindern, wenn keine Benutzeraktion mehr festgestellt werden kann (z. B. bei der Wiedergabe eines Videos in einer App), können Sie [**DisplayRequest.RequestActive**](https://msdn.microsoft.com/library/windows/apps/BR241818) aufrufen.

Um Energie zu sparen und den Akku zu schonen, wird empfohlen, [**DisplayRequest.RequestRelease**](https://msdn.microsoft.com/library/windows/apps/BR241819) aufzurufen und die Displayanforderung freizugeben, sobald diese nicht mehr benötigt wird.

Unten sind einige Situationen aufgeführt, in denen Sie die Displayanforderung freigeben sollten:

-   Die Videowiedergabe wird angehalten, z. B. per Benutzeraktion, wird gepuffert oder aufgrund begrenzter Bandbreite angepasst.
-   Die Wiedergabe wird gestoppt. Beispielsweise ist die Wiedergabe des Videos beendet oder die Darstellung vorüber.
-   Ein Wiedergabefehler ist aufgetreten. Es können beispielsweise Probleme mit der Netzwerkverbindung bestehen, oder eine Datei kann beschädigt sein.

### Platzieren anderer Elemente neben dem eingebetteten Video

Apps bieten häufig eine eingebettete Ansicht, bei der das Video innerhalb einer Seite wiedergegeben wird. Die Vollbildoptimierung greift hier nicht, da das [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/BR242926) nicht die gesamte Seitengröße einnimmt und außerdem auch andere XAML-Objekte gezeichnet werden. Achten Sie darauf, nicht versehentlich in diesen Modus zu wechseln, indem Sie einen Rahmen um ein **MediaElement**-Objekt zeichnen.

Zeichnen Sie keine XAML-Elemente im Vordergrund des Videos, wenn sich dieses im eingebetteten Modus befindet. Andernfalls bedeutet dies einen Zusatzaufwand beim Erstellen der Szene. Ein gutes Optimierungsbeispiel für diese Situation ist das Platzieren von Transportsteuerelementen unterhalb eines eingebetteten Medienelements (anstatt im Vordergrund des Videos). In diesem Bild steht der rote Balken für eine Gruppe von Transportsteuerelementen (Wiedergabe, Pause, Stopp usw.).

![Medienelement mit überlagerten Elementen](images/videowithoverlay.png)
Platzieren Sie diese Steuerelemente nicht im Vordergrund eines Mediums, das sich nicht im Vollbildmodus befindet. Positionieren Sie die Transportsteuerelemente stattdessen irgendwo außerhalb des Bereichs, in dem das Medium gerendert wird. Im nächsten Bild befinden sich die Steuerelemente unterhalb des Mediums.

![Medienelement mit benachbarten Elementen](images/videowithneighbors.png)

### Verzögern des Festlegens der Quelle für ein Medienelement

Medienmodule sind aufwendige Objekte, und das XAML-Framework verzögert das Laden von DLLs sowie das Erstellen großer Objekte so lange wie möglich. Das [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/BR242926)-Objekt ist gezwungen, diese Schritte auszuführen, nachdem die Quelle mithilfe der [**Source**](https://msdn.microsoft.com/library/windows/apps/BR242926-source)-Eigenschaft oder der [**SetSource**](https://msdn.microsoft.com/library/windows/apps/BR242926-setsource)-Methode festgelegt wurde. Indem Sie diese Informationen erst dann festlegen, wenn der Benutzer wirklich ein Medium wiedergeben möchte, verlagern Sie den Großteil des Aufwands für das **MediaElement**-Objekt so weit wie möglich nach hinten.

### Festlegen von „MediaElement.PosterSource“

Durch Festlegen von [**MediaElement.PosterSource**](https://msdn.microsoft.com/library/windows/apps/BR242926-postersource) kann XAML einige GPU-Ressourcen freigeben, die ansonsten verwendet würden. Diese API ermöglicht es einer App, so wenig Arbeitsspeicher wie möglich zu beanspruchen.

### Verbessern des Medien-Scrubbings

Bei Medienplattformen ist es immer eine besondere Herausforderung, dafür zu sorgen, dass das Scrubbing möglichst gut reagiert. Üblicherweise wird hierzu der Wert eines Schiebereglers geändert. Hier finden Sie ein paar Tipps für eine möglichst effiziente Verwendung:

-   Binden Sie entweder den Wert eines [**Slider**](https://msdn.microsoft.com/library/windows/apps/BR209614)-Objekts an [**MediaElement.Position**](https://msdn.microsoft.com/library/windows/apps/BR242926-position), oder aktualisieren Sie ihn auf der Grundlage eines Timers. Verwenden Sie nicht beides. Legen Sie bei Verwendung der zweiten Lösung eine geeignete Aktualisierungsfrequenz für den Timer fest. Das XAML-Framework aktualisiert **MediaElement.Position** während der Wiedergabe nur alle 250 Millisekunden.
-   Die Größe der Schrittfrequenz für den Schieberegler muss gemäß der Videolänge skaliert werden.
-   Abonnieren Sie die Ereignisse [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/BR208911-pointerpressed), [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/BR208911-pointermoved) und [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/BR208911-pointerreleased) für den Schieberegler, um die [**MediaElement.PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/BR242926-playbackrate)-Eigenschaft auf „0“ festzulegen, wenn der Benutzer den Ziehpunkt des Schiebereglers bewegt.
-   Legen Sie die Medienposition im [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/BR208911-pointerreleased)-Ereignishandler manuell auf den Wert der Schiebereglerposition fest, um beim Scrubbing eine optimale Ziehpunktausrichtung zu gewährleisten.

### Anpassen der Videoauflösung an die Geräteauflösung

Die Videodecodierung benötigt sehr viel Arbeitsspeicher sowie sehr viele GPU-Zyklen. Entscheiden Sie sich daher für ein Videoformat, das der Auflösung, mit der das Video wiedergegeben wird, am nächsten kommt. Es bringt nichts, die Ressourcen zur Decodierung eines 1080-Videos zu verwenden, wenn es ohnehin auf eine deutlich geringere Größe skaliert wird. Bei vielen Apps liegt ein Video nicht in mehreren Auflösungen vor. Ist dies jedoch der Fall, verwenden Sie möglichst eine Codierung, die der Anzeigeauflösung am nächsten kommt.

### Wählen empfohlener Formate

Die Wahl des Medienformats kann ein sensibles Thema sein und ist häufig abhängig von geschäftspolitischen Entscheidungen. Im Hinblick auf die Leistung von UWP empfehlen wir H.264-Video als primäres Videoformat sowie AAC und MP3 als bevorzugte Audioformate. Bei der lokalen Dateiwiedergabe ist MP4 der bevorzugte Dateicontainer für Videoinhalte. Die Decodierung von H.264 wird von den meisten aktuellen Grafikkarten beschleunigt. Zudem gilt es zu berücksichtigen, dass die Beschleunigung bei vielen der auf dem Markt erhältlichen Grafikkarten häufig auf eine partielle Beschleunigung (oder auf die IDCT-Ebene) beschränkt ist und keine Hardwareauslagerung für die gesamte Datenstromebene (VLD-Modus) stattfindet, obwohl die Hardwarebeschleunigung für die VC-1-Decodierung weitgehend verfügbar ist.

Wenn die Kontrolle über die Videoinhaltsgenerierung vollständig bei Ihnen liegt, müssen Sie ein möglichst ausgeglichenes Verhältnis zwischen Komprimierungseffizienz und GOP-Struktur finden. Eine relativ geringe GOP-Größe bei B-Bildern kann die Leistung bei Such- oder Trickmodi verbessern.

Verwenden Sie für kurze Audioeffekte mit geringer Latenz (beispielsweise in Spielen) WAV-Dateien mit nicht komprimierten PCM-Daten, um den für komprimierte Audioformate typischen Verarbeitungsmehraufwand zu reduzieren.

### Hardwareaudioauslagerung

Wenn die Hardwareaudioauslagerung automatisch verwendet werden soll, muss [**MediaElement.AudioCategory**](https://msdn.microsoft.com/library/windows/apps/BR242926-audiocategory) auf **ForegroundOnlyMedia** oder **BackgroundCapableMedia** festgelegt sein. Hardwareaudioauslagerung optimiert das Audiorendering, wodurch die Funktionalität und Akkulaufzeit verbessert werden können.

## Optimieren von Bildressourcen

### Skalieren von Bildern auf eine angemessene Größe

Bilder werden in sehr hohen Auflösungen erfasst, was dazu führen kann, dass Apps mehr CPU beim Decodieren der Bilddaten und mehr Arbeitsspeicher nach dem Laden vom Datenträger belegen. Es ist jedoch nicht sehr sinnvoll, ein Bild in hoher Auflösung zu decodieren und im Arbeitsspeicher zu speichern, nur um es dann kleiner als in seiner ursprünglichen Größe anzuzeigen. Erstellen Sie stattdessen eine Version des Bilds in der genauen Größe, in der es auf dem Bildschirm gezeichnet wird. Verwenden Sie hierzu die Eigenschaften [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) und [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241).

Nicht empfohlen:

```xaml
<Image Source="ms-appx:///Assets/highresCar.jpg" 
       Width="300" Height="200"/>    <!-- BAD CODE DO NOT USE.-->
```

Empfohlene Vorgehensweise:

```xaml
<Image>
    <Image.Source>
    <BitmapImage UriSource="ms-appx:///Assets/highresCar.jpg" 
                 DecodePixelWidth="300" DecodePixelHeight="200"/>
    </Image.Source>
</Image>
```

Als Einheiten für [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) und [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) werden standardmäßig physische Pixel verwendet. Mit der [**DecodePixelType**](https://msdn.microsoft.com/library/windows/apps/Dn298545)-Eigenschaft kann dieses Verhalten geändert werden: Durch Festlegen von **DecodePixelType** auf **Logical** wird bei der Decodierungsgröße automatisch der aktuelle Skalierungsfaktor des Systems ähnlich wie andere XAML-Inhalte berücksichtigt. Daher ist es in der Regel sinnvoll, **DecodePixelType** auf **Logical** festzulegen, wenn beispielsweise **DecodePixelWidth** und **DecodePixelHeight** den Eigenschaften für Höhe und Breite des Bildsteuerelements entsprechen sollen, in dem das Bild angezeigt wird. Beim Standardverhalten mit Verwendung von physischen Pixeln müssen Sie selbst den aktuellen Skalierungsfaktor des Systems berücksichtigen; für den Fall, dass der Benutzer seine Anzeigeeinstellungen ändert, sollten Sie auf Benachrichtigungen über Skalierungsänderungen achten.

Wenn „DecodePixelWidth/Height“ explizit größer als die Bildanzeige auf dem Bildschirm festgelegt sind, belegt die App unnötigen zusätzlichen Arbeitsspeicher (bis zu 4 Byte pro Pixel), was bei großen Bildern deutlich ins Gewicht fällt. Zudem wird das Bild mit bilinearer Skalierung verkleinert und kann bei großen Skalierungsfaktoren unscharf dargestellt werden.

Wenn DecodePixelWidth/DecodePixelHeight explizit kleiner als die Bilddarstellung auf dem Bildschirm festgelegt werden, wird das Bild vergrößert und kann pixelig aussehen.

In einigen Fällen, in denen es nicht möglich ist, die passende Decodierungsgröße im Voraus zu bestimmen, sollten Sie die automatische Decodierung von XAML auf die richtige Größe nutzen. Dabei wird nach Möglichkeit versucht, das Bild in der passenden Größe zu decodieren, wenn keine explizite DecodePixelWidth/DecodePixelHeight angegeben ist.

Es wird empfohlen, eine explizite Decodierungsgröße festzulegen, wenn Sie die Größe des Bildinhalts bereits vorab kennen. Legen Sie gleichzeitig [**DecodePixelType**](https://msdn.microsoft.com/library/windows/apps/Dn298545) auf **Logical** fest, wenn die angegebene Decodierungsgröße in Bezug auf andere XAML-Elementgrößen relativ ist. Wenn Sie z. B. die Inhaltsgröße explizit mit Image.Width und Image.Height festlegen, legen Sie DecodePixelType auf DecodePixelType.Logical fest, um die gleichen logischen Pixeldimensionen wie ein Bildsteuerelement zu verwenden, und verwenden sie dann explizit BitmapImage.DecodePixelWidth und/oder BitmapImage.DecodePixelHeight, um die Größe des Bilds zu steuern und potenziell mehr Arbeitsspeicher einzusparen.

Beachten Sie, dass Image.Stretch beim Bestimmen der Größe des decodierten Inhalts berücksichtigt werden sollte.

### Richtige Größe der Decodierung

Wenn Sie keine explizite Decodierungsgröße festlegen, versucht XAML, möglich viel Arbeitsspeicher zu sparen. Dabei wird das Bild auf die genaue Größe decodiert, in der es gemäß dem anfänglichen Layout der Seite angezeigt wird. Es wird empfohlen, die Anwendung so zu schreiben, dass dieses Feature nach Möglichkeit genutzt werden kann. Dieses Feature wird deaktiviert, wenn eine der folgenden Bedingungen erfüllt ist.

-   Das [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/BR243235)-Objekt wird mit der Live-XAML-Struktur verbunden, nachdem der Inhalt mit [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522) oder [**UriSource**](https://msdn.microsoft.com/library/windows/apps/BR243235-urisource) festgelegt wurde.
-   Das Bild wird mit synchroner Decodierung wie [**SetSource**](https://msdn.microsoft.com/library/windows/apps/BR243255) decodiert.
-   Das Bild wird durch Festlegung von [**Opacity**](https://msdn.microsoft.com/library/windows/apps/BR208962) auf 0 oder [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208911-visibility) auf **Collapsed** für das Hostbildelement, den Pinsel oder ein beliebiges übergeordnetes Element ausgeblendet.
-   Das Bildsteuerelement bzw. der Pinsel verwendet für die [**Stretch**](https://msdn.microsoft.com/library/windows/apps/BR242968)-Aufzählung den Wert **None**.
-   Das Bild wird als [**NineGrid**](https://msdn.microsoft.com/library/windows/apps/BR242756) verwendet.
-   `CacheMode="BitmapCache"` wird auf das Bildelement oder ein beliebiges übergeordnetes Element festgelegt.
-   Der Bildpinsel ist nicht rechteckig (z. B. bei Anwendung auf eine Form oder auf Text).

In den oben genannten Szenarien ist das Festlegen einer expliziten Decodierungsgröße die einzige Möglichkeit, um Arbeitsspeichereinsparungen zu erzielen.

Fügen Sie stets ein [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/BR243235)-Objekt an die Live-Struktur an, bevor Sie die Quelle festlegen. Dies erfolgt jedes Mal automatisch, wenn ein Bildelement oder ein Pinsel in Markup angegeben wird. Beispiele finden Sie weiter unten im Abschnitt „Live-Struktur-Beispiele“. Vermeiden Sie beim Festlegen einer Datenstromquelle stets die Verwendung von [**SetSource**](https://msdn.microsoft.com/library/windows/apps/BR243255), und verwenden Sie stattdessen [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522). Zudem ist es sinnvoll, das Ausblenden von Bildinhalten (entweder durch eine Deckkraft von null oder die Sichtbarkeitseinstellung „collapsed“) zu vermeiden, während Sie darauf warten, dass das [**ImageOpened**](https://msdn.microsoft.com/library/windows/apps/BR243235-imageopened)-Ereignis ausgelöst wird. Wägen Sie Ihre Entscheidung ab. Sie können in diesem Fall die Vorteile der automatischen Decodierung in der richtigen Größe nicht nutzen. Wenn Ihre App Bilder anfänglich ausblenden muss, sollte nach Möglichkeit auch die Decodierungsgröße explizit festgelegt werden.

**Live-Struktur-Beispiele**

Beispiel 1 (gut): URI (Uniform Resource Identifier) in Markup angegeben.

```xaml
<Image x:Name="myImage" UriSource="Assets/cool-image.png"/>
```

Beispiel 2 – Markup: URI in CodeBehind angegeben.

```xaml
<Image x:Name="myImage"/>
```

Beispiel 2 – CodeBehind (gut): Verbinden des BitmapImage mit der Struktur, bevor dessen UriSource festgelegt wird.

```vb
var bitmapImage = new BitmapImage();
myImage.Source = bitmapImage;
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
```

Beispiel 2 – CodeBehind (schlecht): Festlegen der UriSource von BitmapImage, bevor es mit der Struktur verbunden wird.

```vb
var bitmapImage = new BitmapImage();
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
myImage.Source = bitmapImage;
```

### Optimierungen der Zwischenspeicherung

Optimierungen beim Zwischenspeichern gelten für Bilder, die [**UriSource**](https://msdn.microsoft.com/library/windows/apps/BR243235-urisource) zum Laden von Inhalten aus einem App-Paket oder aus dem Internet verwenden. Der URI wird verwendet, um den zugrunde liegenden Inhalt eindeutig zu identifizieren. Intern sorgt er dafür, dass das XAML-Framework Inhalte nicht mehrmals herunterlädt oder decodiert. Stattdessen werden die zwischengespeicherten Software- oder Hardwareressourcen verwendet, um Inhalte mehrmals anzuzeigen.

Eine Ausnahme von dieser Optimierung besteht, wenn das Bild mehrmals in unterschiedlichen Auflösungen angezeigt wird (was explizit oder über automatische Decodierung in der richtigen Größe angegeben werden kann). Jeder Eintrag im Zwischenspeicher speichert auch die Auflösung des Bilds, und wenn XAML kein Bild mit einem Quell-URI finden kann, der der erforderlichen Auflösung entspricht, wird eine neue Version in dieser Größe decodiert. Die codierten Bilddaten werden jedoch nicht erneut heruntergeladen.

Aus diesem Grund sollten Sie [**UriSource**](https://msdn.microsoft.com/library/windows/apps/BR243235-urisource) verwenden, wenn Sie Bilder aus einem App-Paket laden. Vermeiden Sie zudem die Verwendung eines Dateidatenstroms und von [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522), wenn diese nicht erforderlich sind.

### Bilder in virtualisierten Panels (z. B. ListView)

Wenn ein Bild aus der Struktur entfernt wird (weil es von der App explizit entfernt wurde, oder weil es sich in einem modernen virtualisierten Panel befindet und implizit beim Bildlauf aus der Ansicht entfernt wurde), optimiert XAML die Speichernutzung, indem es die nicht mehr für das Bild benötigten Hardwareressourcen freigibt. Der Arbeitsspeicher wird nicht sofort freigegeben, sondern erst bei der Frameaktualisierung, die eine Sekunde nach Entfernung des Bildelements aus der Struktur stattfindet.

Folglich sollten Sie nach Möglichkeit moderne virtualisierte Panels zum Hosten von Listen mit Bildinhalten nutzen.

### Softwaregerasterte Bilder

Wenn ein Bild für einen nicht rechteckigen Pinsel oder ein [**NineGrid**](https://msdn.microsoft.com/library/windows/apps/BR242756) verwendet wird, verwendet das Bild einen Softwarerasterungspfad, bei dem Bilder gar nicht skaliert werden. Zudem muss eine Kopie des Bilds sowohl im Arbeitsspeicher der Software als auch der Hardware gespeichert werden. Wenn z. B. ein Bild als Pinsel für eine Ellipse verwendet wird, wird das potenziell große Vollbild zweimal intern gespeichert. Bei Verwendung von **NineGrid** oder eines nicht rechteckigen Pinsels sollte die App die Bilder vorab auf die ungefähre Größe skalieren, in der sie gerendert werden.

### Laden von Bildern in einem Hintergrundthread

XAML verfügt über eine interne Optimierung, mit der der Inhalt eines Bildes asynchron auf eine Oberfläche im Hardwarespeicher decodiert werden kann, ohne dass eine Zwischenoberfläche im Softwarespeicher benötigt wird. Dies reduziert Spitzen bei der Speicherauslastung und Renderinglatenz. Dieses Feature wird deaktiviert, wenn eine der folgenden Bedingungen erfüllt ist.

-   Das Bild wird als [**NineGrid**](https://msdn.microsoft.com/library/windows/apps/BR242756) verwendet.
-   `CacheMode="BitmapCache"` wird auf das Bildelement oder ein beliebiges übergeordnetes Element festgelegt.
-   Der Bildpinsel ist nicht rechteckig (z. B. bei Anwendung auf eine Form oder auf Text).

### SoftwareBitmapSource

Die [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Dn997854)-Klasse tauscht interoperable, nicht komprimierte Bilder zwischen unterschiedlichen WinRT-Namespaces aus, z. B. [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/BR226176), Kamera-APIs und XAML. Diese Klasse vermeidet die zusätzliche Kopie, die in der Regel mit [**WriteableBitmap**](https://msdn.microsoft.com/library/windows/apps/BR243259) erforderlich wäre. Dadurch reduzieren sich Spitzenlasten im Speicher sowie die Quelle-zu-Bildschirm-Latenz.

Die [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Dn887358)-Klasse, die Quellinformationen bereitstellt, kann auch für ein benutzerdefiniertes [**IWICBitmap**](https://msdn.microsoft.com/library/windows/desktop/Ee719675)-Objekt konfiguriert werden. Dadurch wird ein erneut ladbarer Sicherungsspeicher bereitgestellt, mit dem die App Arbeitsspeicher nach Bedarf neu zuordnen kann. Dies ist ein erweiterter C++-Anwendungsfall.

Ihre App sollte [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Dn887358) und [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Dn997854) für die Interoperabilität mit anderen WinRT-APIs verwenden, die Bilder produzieren und nutzen. Zudem sollte die App beim Laden von nicht komprimierten Bilddaten anstelle von [**WriteableBitmap**](https://msdn.microsoft.com/library/windows/apps/BR243259) die **SoftwareBitmapSource**-Klasse verwenden.

### Verwenden von „GetThumbnailAsync“ für Miniaturansichten

Ein Anwendungsfall für die Bildskalierung ist die Erstellung von Miniaturansichten. Sie könnten kleine Bildversionen zwar auch mit [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) und [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) bereitstellen, aber die Universelle Windows-Plattform (UWP) bietet noch effizientere APIs zum Abrufen von Miniaturansichten. [**GetThumbnailAsync**](https://msdn.microsoft.com/library/windows/apps/BR227210) stellt die Miniaturansichten für Bilder bereit, bei denen das Dateisystem bereits zwischengespeichert wurde. Dadurch erhalten Sie eine noch bessere Leistung als bei den XAML-APIs, da das Bild weder geöffnet noch decodiert werden muss.

> [!div class="tabbedCodeSnippets"]
```csharp
FileOpenPicker picker = new FileOpenPicker();
picker.FileTypeFilter.Add(".bmp");
picker.FileTypeFilter.Add(".jpg");
picker.FileTypeFilter.Add(".jpeg");
picker.FileTypeFilter.Add(".png");
picker.SuggestedStartLocation = PickerLocationId.PicturesLibrary;

StorageFile file = await picker.PickSingleFileAsync();

StorageItemThumbnail fileThumbnail = await file.GetThumbnailAsync(ThumbnailMode.SingleItem, 64);

BitmapImage bmp = new BitmapImage();
bmp.SetSource(fileThumbnail);

Image img = new Image();
img.Source = bmp;
```
```vb
Dim picker As New FileOpenPicker()
picker.FileTypeFilter.Add(".bmp")
picker.FileTypeFilter.Add(".jpg")
picker.FileTypeFilter.Add(".jpeg")
picker.FileTypeFilter.Add(".png")
picker.SuggestedStartLocation = PickerLocationId.PicturesLibrary

Dim file As StorageFile = Await picker.PickSingleFileAsync()

Dim fileThumbnail As StorageItemThumbnail = Await file.GetThumbnailAsync(ThumbnailMode.SingleItem, 64)

Dim bmp As New BitmapImage()
bmp.SetSource(fileThumbnail)

Dim img As New Image()
img.Source = bmp
```

### Einmaliges Decodieren von Bildern

Damit Bilder nicht mehrmals decodiert werden, weisen Sie die [**Image.Source**](https://msdn.microsoft.com/library/windows/apps/BR242760)-Eigenschaft von einem URI aus zu, anstatt Speicherstreams zu verwenden. Das XAML-Framework kann den gleichen URI an mehreren Orten einem einzelnen decodierten Bild zuordnen. Für mehrere Speicherstreams mit den gleichen Daten ist dies allerdings nicht möglich. Hier erstellt das Framework für jeden Speicherstream ein eigenes decodiertes Bild.



<!--HONumber=Mar16_HO1-->


