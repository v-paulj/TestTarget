---
author: mijacobs
Description: "Ressourcen für App-Symbole, die in einer Vielzahl von Formen innerhalb des Windows 10-Betriebssystems vorkommen, sind die Aushängeschilder für Ihre App für die Universelle Windows-Plattform (UWP)."
title: "Ressourcen für Kacheln und Symbole"
ms.assetid: D6CE21E5-2CFA-404F-8679-36AA522206C7
label: Tile and icon assets
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 98eebc4fbf18aa2fbe4586958f666b41580cf6d9

---

# Richtlinien für die Ressourcen für Kacheln und Symbole





Ressourcen für App-Symbole, die in einer Vielzahl von Formen innerhalb des Windows 10-Betriebssystems vorkommen, sind die Aushängeschilder für Ihre App für die Universelle Windows-Plattform (UWP). In diesen Richtlinien wird beschrieben, wo Ressourcen für App-Symbole im System angezeigt werden, und Sie erhalten ausführliche Designtipps zum Erstellen ansprechender Symbole.

![Starten von Windows 10 und Kacheln](images/assetguidance01.jpg)

## <span id="Adaptive_scaling"></span><span id="adaptive_scaling"></span><span id="ADAPTIVE_SCALING"></span>Adaptive Skalierung


Zunächst erhalten Sie einen kurzen Überblick über die adaptive Skalierung, um die Funktion der Skalierung mit Ressourcen verstehen zu können. Mit Windows 10 wird eine Weiterentwicklung des vorhandenen Skalierungsmodells eingeführt. Neben der Skalierung von Vektorinhalten gibt es einen einheitlichen Satz von Skalierungsfaktoren, der eine einheitliche Größe für UI-Elemente für eine Vielzahl von Bildschirmgrößen und -auflösungen bietet. Die Skalierungsfaktoren sind auch mit den Skalierungsfaktoren anderer Betriebssysteme wie iOS und Android kompatibel, sodass die Ressourcen einfacher für alle diese Plattformen verwendet werden können.

Die Auswahl der aus dem Store herunterzuladenden Ressourcen erfolgt zum Teil auf Grundlage des DPI-Werts eines Geräts. Nur die Ressourcen, die dem Gerät am besten entsprechen, werden heruntergeladen.

## <span id="Tile_elements"></span><span id="tile_elements"></span><span id="TILE_ELEMENTS"></span>Kachelelemente


Die grundlegenden Komponenten einer Startkachel sind eine Rückwand, ein Symbol, eine Brandingleiste, Ränder und ein App-title:

![Aufschlüsselung der Kachelelemente](images/assetguidance02.png)

Auf der Brandingleiste am unteren Rand einer Kachel werden der App-Name, die Signalgebung und der Zähler (falls verwendet) angezeigt:

![Brandingleiste in der Kachel](images/assetguidance03.png)

Die Höhe der Brandingleiste basiert auf dem Skalierungsfaktor des Geräts, auf dem sie angezeigt wird:

| Skalierungsfaktor | Pixel |
|--------------|--------|
| 100 %         | 32     |
| 125 %         | 40     |
| 150 %         | 48     |
| 200 %         | 64     |
| 400%         | 128    |

 

Die Ränder der Kachel werden vom System festgelegt und können nicht geändert werden. Die meisten Inhalte werden innerhalb der Ränder angezeigt, wie in diesem Beispiel:

![Kachelfassaden](images/assetguidance04.png)

Die Randbreite basiert auf dem Skalierungsfaktor des Geräts, auf dem sie angezeigt wird:

| Skalierungsfaktor | Pixel |
|--------------|--------|
| 100 %         | 8      |
| 125 %         | 10     |
| 150 %         | 12     |
| 200 %         | 16     |
| 400%         | 32     |

 

## <span id="Tile_assets"></span><span id="tile_assets"></span><span id="TILE_ASSETS"></span>Kachelressourcen


Jede Kachelressource hat die gleiche Größe wie die Kachel, auf der sie sich befindet. Sie können die App-Kacheln mit zwei unterschiedlichen Darstellungsformen einer Ressource kennzeichnen:

1. Ein zentriertes Symbol oder Logo mit Abstand. Auf diese Weise scheint die Farbe der Rückwand durch:

![Kachel und Rückwand](images/assetguidance05.png)

2. Eine randlose Brandingkachel ohne Abstand:

![Randlose Kachel](images/assetguidance06.png)

Zum Zweck der Einheitlichkeit auf allen Geräten hat jede Kachelgröße (klein, mittel, breit und groß) eine eigene Größenbeziehung. Um eine einheitliche Symbolanordnung auf allen Kacheln zu erreichen, empfehlen wir einige grundlegende Abstandsrichtlinien für die folgenden Kachelgrößen. Der Bereich, in dem zwei violette Überlagerungen schneiden, ist die ideale Fläche für ein Symbol. Symbole passen zwar nicht immer genau in die Fläche, das visuelle Volumen eines Symbols sollte aber ungefähr den dargestellten Beispielen entsprechen.

Kleine Kachelgröße:

![Beispiel für kleine Kachelgröße](images/assetguidance07a.png)

Mittlere Kachelgröße:

![Beispiel für mittlere Kachelgröße](images/assetguidance07b.png)

Breite Kachelgröße:

![Beispiel für breite Kachelgröße](images/assetguidance07c.png)

Große Kachelgröße:

![Beispiel für große Kachelgröße](images/assetguidance07d.png)

In diesem Beispiel ist das Symbol für die Kachel zu groß:

![Symbol zu groß für die Kachel](images/assetguidance08a.png)

In diesem Beispiel ist das Symbol für die Kachel zu klein:

![Symbol zu klein für die Kachel](images/assetguidance08b.png)

Die folgenden Abstandsverhältnisse sind optimal für horizontal oder vertikal ausgerichtete Symbole.

Beschränken Sie bei kleinen Kacheln die Breite und Höhe auf 66 % der Kachelgröße:

![Verhältnisse bei kleinen Kachelgrößen](images/assetguidance09.png)

Beschränken Sie bei mittelgroßen Kacheln die Symbolbreite auf 66 % und die Höhe auf 50 % der Kachelgröße. Dadurch wird verhindert, dass Elemente in der Brandingleiste überlappen:

![Verhältnisse bei mittelgroßen Kacheln](images/assetguidance10.png)

Beschränken Sie bei breiten Kacheln die Symbolbreite auf 66 % und die Höhe auf 50 % der Kachelgröße. Dadurch wird verhindert, dass Elemente in der Brandingleiste überlappen:

![Verhältnisse bei breiten Kacheln](images/assetguidance11.png)

Beschränken Sie bei großen Kacheln die Breite und Höhe auf 50 % der Kachelgröße:

![Verhältnisse bei großen Kacheln](images/assetguidance12.png)

Einige Symbole sind so konzipiert, dass sie horizontal oder vertikal ausgerichtet sind, andere Symbole hingegen weisen komplexere Formen auf, wodurch verhindert wird, dass sie direkt in die Zielabmessungen passen. Symbole, die zentriert erscheinen, können auf eine Seite geneigt sein. In diesem Fall können Teile eines Symbols aus der empfohlenen Fläche heraushängen, vorausgesetzt, das Symbol belegt dasselbe visuelle Gewicht wie ein genau passendes Symbol:

![Drei zentrierte Symbole](images/assetguidance13.png)

Berücksichtigen Sie bei randlosen Ressourcen Elemente, die innerhalb der Ränder und Kanten der Kacheln interagieren. Behalten Sie Ränder von mindestens 16 % der Höhe oder Breite der Kachel bei. Dieser Prozentsatz stellt die doppelte Breite der Ränder bei den kleinsten Kachelgrößen dar:

![Randlose Kachel mit Rändern](images/assetguidance14.png)

In diesem Beispiel sind die Ränder zu eng:

![Randlose Kachel mit zu engen Rändern](images/assetguidance15.png)

## <span id="Tile_assets_in_list_views"></span><span id="tile_assets_in_list_views"></span><span id="TILE_ASSETS_IN_LIST_VIEWS"></span>Kachelressourcen in Listenansichten


Kacheln können auch in einer Listenansicht angezeigt werden. Größenrichtlinien für Kachelressourcen, die in Listenansichten angezeigt werden, unterscheiden sich leicht von den zuvor aufgeführten Kachelressourcen. In diesem Abschnitt werden diese Einzelheiten bei der Größe erläutert.

![Kachelressourcen in einer Listenansicht](images/assetguidance16.png)

Beschränken Sie die Breite und Höhe von Symbolen auf 75 % der Kachelgröße:

![Größe eines Symbols einer Listenansichtkachel](images/assetguidance17.png)

Beschränken Sie bei vertikalen und horizontalen Symbolformaten die Breite und Höhe auf 75 % der Kachelgröße:

![Größe eines Symbols einer Listenansichtkachel](images/assetguidance18.png)

Behalten Sie bei randlosen Grafiken wichtiger Brandingelemente Ränder von 12,5 % bei:

![Randlose Grafiken in Listenansichtkachel](images/assetguidance19.png)

In diesem Beispiel ist das Symbol in der Kachel zu groß:

![Symbol für die Kachel zu groß](images/assetguidance20a.png)

In diesem Beispiel ist das Symbol in der Kachel zu klein:

![Symbol für die Kachel zu klein](images/assetguidance20b.png)

## <span id="Target-based_assets"></span><span id="target-based_assets"></span><span id="TARGET-BASED_ASSETS"></span>Zielbasierte Ressourcen


Zielbasierte Ressourcen gelten für Symbole und Kacheln, die in der Windows-Taskleiste, in der Aufgabenansicht, über ALT+TAB, in der Andockhilfe und in der unteren rechten Ecke von Startkacheln angezeigt werden. Sie müssen für diese Ressourcen keine Abstände hinzufügen, diese werden bei Bedarf von Windows hinzugefügt. Bei diesen Ressourcen sollte eine minimale Fläche von 16 Pixeln vorgesehen werden. Unten sehen Sie ein Beispiel dafür, wie diese Ressourcen als Symbole in der Windows-Taskleiste angezeigt werden:

![Ressourcen in Windows-Taskleiste](images/assetguidance21.png)

Diese Benutzeroberfläche verwendet zwar neben einer farbigen Rückwand standardmäßig eine zielbasierte Ressource, Sie können aber auch eine zielbasierte Ressource ohne Anpassung verwenden. Ressourcen ohne Anpassung sollten so erstellt werden, dass es möglich ist, sie in unterschiedlichen Hintergrundfarben anzuzeigen.

![Ressourcen mit und ohne Anpassung](images/assetguidance22.png)

Nachfolgend finden Sie die Größenempfehlungen für zielbasierte Ressourcen mit einer Skalierung von 100 %:

![Zielbasierte Ressourcengröße bei einer Skalierung von 100 %](images/assetguidance23.png)

**App-Ressourcen für Iconic-Vorlage**

Mit der Iconic-Vorlage (auch als „IconWithBadge“-Vorlage bezeichnet) können Sie ein kleines Bild in der Mitte der Kachel anzeigen. Windows 10 unterstützt die Vorlage auf Telefonen sowie auf Tablets/Desktops. (Weitere Informationen zum Erstellen von Iconic-Kacheln finden Sie im [Artikel zu speziellen Kachelvorlagen](tiles-and-notifications-special-tile-templates-catalog.md).)

Apps, die die Iconic-Vorlage verwenden, z. B. Nachrichten, Telefon und Store, weisen zielbasierte Elemente auf, die über ein Signal (mit dem Live-Zähler) verfügen können. Wie bei anderen zielbasierten Ressourcen ist kein Abstand erforderlich. Iconic-Ressourcen sind nicht Teil des App-Manifests, gehören aber zur Nutzlast einer Live-Kachel. Ressourcen sind so skaliert, dass sie in einen Container mit 3:2-Verhältnis passen und zentriert sind.

![Größen für Ressourcen mit und ohne Signal](images/assetguidance24.png)

Für quadratische Ressourcen erfolgt eine automatische Zentrierung innerhalb des Containers:

![Größe einer quadratischen Ressource, mit und ohne Signal](images/assetguidance25.png)

Bei nicht quadratischen Ressourcen erfolgt eine automatische horizontale/vertikale Zentrierung und ein Andocken an die Breite bzw. Höhe des Containers:

![Größe einer nicht quadratischen Ressource, mit und ohne Signal](images/assetguidance26a.png)

![Größe einer nicht quadratischen Ressource, mit und ohne Signal](images/assetguidance26b.png)

## <span id="Splash_screen_assets"></span><span id="splash_screen_assets"></span><span id="SPLASH_SCREEN_ASSETS"></span>Ressourcen für den Begrüßungsbildschirm


Das Bild für den Begrüßungsbildschirm kann als direkter Pfad zu einer Bilddatei oder als Ressource angegeben werden. Mithilfe eines Ressourcenverweises können Sie Bilder mit verschiedenen Skalierungen bereitstellen, damit Windows die optimale Größe für das jeweilige Gerät und die Bildschirmauflösung auswählen kann. Sie können auch Bilder mit hohem Kontrast für die Barrierefreiheit sowie lokalisierte Bilder für verschiedene Benutzeroberflächensprachen bereitstellen.

Wenn Sie „Package.appxmanifest“ in einem Text-Editor öffnen, wird das [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br211467)-Element als untergeordnetes Element des [**VisualElements**](https://msdn.microsoft.com/library/windows/apps/br211471)-Elements angezeigt. Das standardmäßige Begrüßungsbildschirm-Markup in der Manifestdatei sieht in einem Text-Editor wie folgt aus:

```XML
<uap:SplashScreen Image="Assets\SplashScreen.png" /></code></pre></td>
</tr>
</tbody>
</table>
```

Die Ressource für den Begrüßungsbildschirm wird von jedem Gerät zentriert, auf dem er angezeigt wird:

![Größe der Ressource für den Begrüßungsbildschirm](images/assetguidance27.png)

## <span id="High-contrast_assets"></span><span id="high-contrast_assets"></span><span id="HIGH-CONTRAST_ASSETS"></span>Ressourcen mit hohem Kontrast


Der Modus mit hohem Kontrast verwendet separate Sätze von Ressourcen für kontrastreiches Weiß (weißer Hintergrund mit schwarzem Text) und kontrastreiches Schwarz (schwarzer Hintergrund mit weißem Text). Wenn Sie keine Ressourcen mit hohem Kontrast für Ihre App angeben, werden die Standardressourcen verwendet.

Wenn die Standardressourcen für Ihre App eine akzeptable Anzeigeumgebung bei einem schwarzweißen Hintergrund bieten, sollte Ihre App im Modus mit hohem Kontrast zumindest zufriedenstellend aussehen. Wenn Ihre die Standardressourcen keine akzeptable Anzeigeumgebung bei einem schwarzweißen Hintergrund liefern, sollten Sie in Betracht ziehen, Ressourcen mit hohem Kontrast zu verwenden. Die folgenden Beispiele zeigen die zwei Arten von Ressourcen mit hohem Kontrast:

![Beispiele für hohes Kontrastverhältnis](images/assetguidance28.png)

Wenn Sie Ressourcen mit hohem Kontrast bereitstellen möchten, müssen Sie beide Sätze – weiß auf schwarz und schwarz auf weiß – verwenden. Wenn Sie diese Ressourcen in Ihr Paket aufnehmen, können Sie einen Ordner "Kontrast Schwarz" für weiß auf schwarz, und einen Ordner "Kontrast Weiß" für schwarz auf weiß erstellen.

## <span id="Asset_size_tables"></span><span id="asset_size_tables"></span><span id="ASSET_SIZE_TABLES"></span>Größentabellen


Es wird dringend empfohlen, dass Sie mindestens Ressourcen für die Skalierungsfaktoren 100, 200 und 400 Skalierungsfaktoren bereitstellen. Wenn Sie Ressourcen für alle Skalierungsfaktoren bereitstellen, liefert dies eine optimale Benutzererfahrung.

**Skalierungsbasierte Ressourcen**

| Kategorie             | Elementname      | Bei einer Skalierung von 100 % | Bei einer Skalierung von 125 % | Bei einer Skalierung von 150 % | Bei einer Skalierung von 200 % | Bei einer Skalierung von 400 % |
|----------------------|-------------------|---------------|---------------|---------------|---------------|---------------|
| Klein                | Square71x71Logo   | 71 x 71         | 89 x 89         | 107 x 107       | 142 x 142       | 284 x 284       |
| Mittel               | Square150x150Logo | 150 x 150       | 188 x 188       | 225 x 225       | 300 x 300       | 600 x 600       |
| Breit                 | Square310x150Logo | 310 x 150       | 388 x 188       | 465 x 225       | 620 x 300       | 1240 x 600      |
| Groß (nur Desktop) | Square310x310Logo | 310 x 310       | 388 x 388       | 465 x 465       | 620 x 620       | 1240 x 1240     |
| App-Liste (Symbol)      | Square44x44Logo   | 44 x 44         | 55 x 55         | 66 x 66         | 88 x 88         | 176 x 176       |

 

**Dateinamenbeispiele für skalierungsbasierte Ressourcen**

| Kategorie             | Elementname      | Bei einer Skalierung von 100 %                  | Bei einer Skalierung von 125 %                  | Bei einer Skalierung von 150 %                  |
|----------------------|-------------------|--------------------------------|--------------------------------|--------------------------------|
| Klein                | Square71x71Logo   | AppNameSmallTile.scale-100.png | AppNameSmallTile.scale-125.png | AppNameSmallTile.scale-150.png |
| Medium               | Square150x150Logo | AppNameMedTile.scale-100.png   | AppNameMedTile.scale-125.png   | AppNameMedTile.scale-150.png   |
| Breit                 | Square310x150Logo | AppNameWideTile.scale-100.png  | AppNameWideTile.scale-125.png  | AppNameWideTile.scale-150.png  |
| Groß (nur Desktop) | Square310x310Logo | AppNameLargeTile.scale-100.png | AppNameLargeTile.scale-125.png | AppNameLargeTile.scale-150.png |
| App-Liste (Symbol)      | Square44x44Logo   | AppNameLargeTile.scale-100.png | AppNameLargeTile.scale-125.png | AppNameLargeTile.scale-150.png |

 

| Kategorie             | Elementname      | Bei einer Skalierung von 200 %                  | Bei einer Skalierung von 400 %                  |
|----------------------|-------------------|--------------------------------|--------------------------------|
| Klein                | Square71x71Logo   | AppNameSmallTile.scale-200.png | AppNameSmallTile.scale-400.png |
| Medium               | Square150x150Logo | AppNameMedTile.scale-200.png   | AppNameMedTile.scale-400.png   |
| Breit                 | Square310x150Logo | AppNameWideTile.scale-200.png  | AppNameWideTile.scale-400.png  |
| Groß (nur Desktop) | Square310x310Logo | AppNameLargeTile.scale-200.png | AppNameLargeTile.scale-400.png |
| App-Liste (Symbol)      | Square44x44Logo   | AppNameLargeTile.scale-200.png | AppNameLargeTile.scale-400.png |

 

**Zielbasierte Ressourcen**

Zielbasierte Ressourcen werden über mehrere Skalierungsfaktoren hinweg verwendet. Der Elementname für zielbasierte Ressourcen ist **Square44x44Logo**. Es wird dringend empfohlen, mindestens die folgenden Ressourcen zu übermitteln:

16 x 16, 24 x 24, 32 x 32, 48 x 48, 256 x 256

Die folgende Tabelle enthält alle zielbasierten Ressourcengrößen und die entsprechenden Dateinamenbeispiele:

| Ressourcengröße | Dateinamenbeispiel                 |
|------------|-----------------------------------|
| 16 x 16\*    | AppNameAppList.targetsize-16.png  |
| 24 x 24\*    | AppNameAppList.targetsize-24.png  |
| 32 x 32\*    | AppNameAppList.targetsize-32.png  |
| 48 x 48\*    | AppNameAppList.targetsize-48.png  |
| 256 x 256\*  | AppNameAppList.targetsize-256.png |
| 20 x 20      | AppNameAppList.targetsize-20.png  |
| 30 x 30      | AppNameAppList.targetsize-30.png  |
| 36 x 36      | AppNameAppList.targetsize-36.png  |
| 40 x 40      | AppNameAppList.targetsize-40.png  |
| 60 x 60      | AppNameAppList.targetsize-60.png  |
| 64 x 64      | AppNameAppList.targetsize-64.png  |
| 72 x 72      | AppNameAppList.targetsize-72.png  |
| 80 x 80      | AppNameAppList.targetsize-80.png  |
| 96 x 96      | AppNameAppList.targetsize-96.png  |

 

\* Übermitteln Sie diese Ressourcengrößen als Basislinie

## <span id="Asset_types"></span><span id="asset_types"></span><span id="ASSET_TYPES"></span>Ressourcentypen


Nachfolgend sind alle Ressourcentypen, ihre Anwendungsmöglichkeiten sowie die empfohlenen Dateinamen aufgeführt.

**Kachelressourcen**

-   Zentrierte Ressourcen werden in der Regel auf der Startseite zum Präsentieren Ihrer App verwendet.
-   Dateinamenformat: \*Tile.scale-\*.PNG
-   Betroffene Apps: Jede UWP-App
-   Anwendungsmöglichkeiten:
    -   Standard-Startkacheln (Desktop und mobil)
    -   Info-Center (Desktop und mobil)
    -   Aufgabenumschaltfunktion (mobil)
    -   Freigabeauswahl (mobil)
    -   Datumsauswahl (mobil)
    -   Store

**Skalierbare Listeressourcen mit Anpassung**

-   Diese Ressourcen werden auf Flächen verwendet, die Skalierungsfaktoren erfordern. Diese Ressourcen werden vom System angepasst oder weisen ihre eigene Hintergrundfarbe auf, wenn die App diese enthält.
-   Dateinamenformat: \*AppList.scale-\*.PNG
-   Betroffene Apps: Jede UWP-App
-   Anwendungsmöglichkeiten:
    -   Starten der Liste aller Apps (Desktop)
    -   Liste zum Starten der am häufigsten verwendeten Apps (Desktop)
    -   Task-Manager (Desktop)
    -   Cortana-Suchergebnisse
    -   Liste zum Starten aller Apps (mobil)
    -   Einstellungen

**Zielgröße-Listenressourcen mit Anpassung**

-   Dies sind feste Größen, die nicht skaliert werden. Sie werden meistens für ältere Umgebungen verwendet. Ressourcen werden vom System überprüft.
-   Dateinamenformat: \*AppList.targetsize-\*.PNG
-   Betroffene Apps: Jede UWP-App
-   Anwendungsmöglichkeiten:
    -   Starten der Sprungliste (Desktop)
    -   Starten der unteren Ecke der Kachel (Desktop)
    -   Tastenkombinationen (Desktop)
    -   Systemsteuerung (Desktop)

**Zielgröße-Listenressourcen ohne Anpassung**

-   Dies sind Objekte, die nicht vom System angepasst oder skaliert werden.
-   Dateinamenformat: \*AppList.targetsize-\*\_altform-unplated.PNG
-   Betroffene Apps: Jede UWP-App
-   Anwendungsmöglichkeiten:
    -   Taskleiste und Miniaturansicht der Taskleiste (Desktop)
    -   Taskleisten-Sprungliste
    -   Aufgabenansicht
    -   ALT + TAB

**Dateierweiterungsressourcen**

-   Hierbei handelt es sich um spezielle Ressourcen für Dateierweiterungen. Sie werden neben Win32-Dateizuordnungssymbolen im Datei-Explorer angezeigt und müssen designunabhängig sein. Die Größe ist auf Desktops und mobilen Plattformen unterschiedlich.
-   Dateinamenformat: \*LogoExtensions.targetsize-\*.PNG
-   Betroffene Apps: Musik, Video, Fotos, Microsoft Edge, Microsoft Office
-   Anwendungsmöglichkeiten:
    -   Datei-Explorer
    -   Cortana
    -   Verschiedene Benutzeroberflächen (Desktop)

**Begrüßungsbildschirm**

-   Die Ressource, die auf dem Begrüßungsbildschirm Ihrer App angezeigt wird. Automatische Skalierung auf Desktops und mobilen Plattformen.
-   Dateinamenformat: \*SplashScreen.screen-100.PNG
-   Betroffene Apps: Jede UWP-App
-   Anwendungsmöglichkeiten:
    -   Begrüßungsbildschirm der App

**Ressourcen für Iconic-Kachel**

-   Dies sind Ressourcen für Apps, die die Iconic-Vorlage verwenden.
-   Dateinamenformat: nicht zutreffend
-   Betroffene Apps: Nachrichten, Telefon, Store usw.
-   Anwendungsmöglichkeiten:
    -   Ikonische Kachel



## <span id="related_topics"></span>Verwandte Themen



* [Spezielle Kachelvorlagen](tiles-and-notifications-special-tile-templates-catalog.md)
 

 







<!--HONumber=Jun16_HO4-->


