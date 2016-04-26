---
label: Grid
template: detail.hbs
extraBodyClass: layout-grid
brief: With its rigorous hierarchies and geometry, the grid orients us. It tells us what’s important and what can wait. As people become comfortable with reductive, flat design, the grid can be more abstract, with fewer cues and signposts. The explicit grid starts to fade, leaving behind the elegant relationships between its elements.
---

# Basiseinheit

Das Raster für Windows wird aus Einheiten von 4 × 4 px konstruiert. Dies sind **effektive Pixel**, keine physischen Pixel. Elemente in Layouts sollten immer in Schritten von 4 px skaliert werden. Dadurch entsteht ein vertrauter Rhythmus, der für Ausgewogenheit und Geschlossenheit sorgt.

![Zeigt das 4-px-Raster](assets/grid/grid.png)

# Effektive Pixel

Effektive Pixel sind eine virtuelle Maßeinheit. Unabhängig von der Pixeldichte (auch bekannt als Dots per Inch oder DPI) eines Bildschirms werden damit Layoutdimensionen und -abstände ausgedrückt. Effektive Pixel ermöglichen es, den Fokus auf die tatsächlich wahrgenommene Größe von Benutzeroberflächenelementen zu legen, ohne sich Gedanken über die Pixeldichte des Geräts machen zu müssen, das zufällig genutzt wird.

In den meisten Fällen kürzen wir effektive Pixel einfach mit px ab, da wir keine Layouts mit physischen Pixeln erstellen.

<video class="video-responsive" controls>
    <source src="assets/grid/epx.mp4" type="video/mp4" />
    Oops! Your browser doesn't seem to support this video. Sorry about that.
</video>

<aside class="aside-dev">
    <div class="aside-dev-title">
        Developer Notes
    </div>
    <div class="aside-dev-content">
            Although XAML doesn't specify units for its widths, heights, margins, and padding, they are all implicitly measured in effective pixels.
    </div>
</aside>

# Touch-Ziele

48 × 48 px ist die optimale Größe für ein Element, das berührt werden muss. Allerdings lässt sich diese noch weiter auf 44 × 44 px reduzieren, falls Größenbeschränkungen bestehen und das Element womöglich weniger häufig genutzt wird. Wenn eine geringere Höhe erforderlich ist, ist eine Größe von 32 × 120 px eine weitere Möglichkeit, die nur für Desktops und 2-in-1-PCs genutzt werden sollte.

![Zeigt das Touch-Ziel von 48 × 48 px](assets/grid/touch-target.png)


<!--HONumber=Mar16_HO4-->


