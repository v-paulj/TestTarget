---
Description: Farben tragen dazu bei, dass sich Benutzer intuitiv in den verschiedenen Informationsebenen einer App zurechtfinden, und spielen eine wichtige Rolle für das Interaktionsmodell.
title: Farben
ms.assetid: 3ba7176f-ac47-498c-80ed-4448edade8ad
label: Color
template: detail.hbs
extraBodyClass: style-color
brief: Color provides intuitive wayfinding through an app's various levels of information and serves as a crucial tool for reinforcing the interaction model.<br /><br />In Windows, color is also personal. Users can choose a color and a light or dark theme to be reflected throughout their experience.
---

# Farben für UWP-Apps
Farben tragen dazu bei, dass sich Benutzer intuitiv in den verschiedenen Informationsebenen einer App zurechtfinden, und spielen eine wichtige Rolle für das Interaktionsmodell.

## Akzentfarbe

Der Benutzer kann eine einzelne Farbe (ein so genannter Akzent) auswählen. Zur Auswahl stehen 48 speziell zusammengestellte Farbmuster.


<!-- Alternate version for the dev center. Need to add hex values. -->
<figure>
![Accent colors](images/accentcolorswatch.png)
<figcaption>Faustregel: Wenn die ursprüngliche Akzentfarbe als Hintergrund verwendet wird, muss im Vordergrund immer weißer Text angezeigt werden.</figcaption>
</figure>

Wenn der Benutzer eine Akzentfarbe auswählt, wird sie Teil des Systemdesigns. Davon sind dann Startseite, Taskleiste, Fensterchrom, bestimmte Interaktionszustände und Links mit [allgemeinen Steuerelementen](https://dev.windows.com/design/controls-patterns) betroffen. Bei Apps besteht zudem die Möglichkeit, die Akzentfarbe in die Typografie, Hintergründe und Interaktionen zu integrieren – oder sie zu ignorieren, um das Branding der jeweiligen App nicht zu verändern.

## Farbauswahl

Nachdem eine Akzentfarbe ausgewählt wurde, werden auf der Grundlage der HCL Werte für die Leuchtdichte helle und dunkle Schattierungen der Akzentfarbe erstellt. Mithilfe von Schattierungsvarianten können Apps eine visuelle Hierarchie erstellen und Interaktionen darstellen.

![Eine einzelne Akzentfarbe mit sechs Schattierungen](images/shades.png)

<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
            In XAML, the accent color is exposed as a [theme resource](https://msdn.microsoft.com/en-us/library/windows/apps/Mt187274.aspx) named `SystemAccentColor`. It's also available programmatically from [UISettings.GetColorValue](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx). You can programmatically access the different shades from [UISettings.GetColorValue](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx), see the [UIColorType](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx) enum.
    </div>
</aside>

## Farbdesigns

Der Benutzer kann sich auch für ein helles oder dunkles Systemdesign entscheiden. (Gilt nur für Smartphones. Bei Tablet- und Desktopgeräten steht diese Option noch nicht zur Verfügung. Hier kann stattdessen eine App-interne Einstellung verwendet werden.) Einige Apps passen Ihr Design auf der Grundlage der Einstellung des Benutzers an, andere dagegen nicht.

Apps mit hellem Design sind für Produktivitätsszenarien konzipiert. Ein Beispiel wäre etwa die App-Suite von Microsoft Office. Das helle Design ermöglicht komfortables Lesen umfangreicher Texte sowie längeres konzentriertes Arbeiten.

Das dunkle Design verbessert den Kontrast bei Inhalten von Medien-Apps oder bei Szenarien, in denen den Benutzern zahlreiche Videos oder Bilder angezeigt werden. In diesen Szenarien steht nicht unbedingt das Lesen von Texten im Vordergrund, sondern vielleicht die Wiedergabe eines Films bei schwacher Umgebungsbeleuchtung.

Falls keine dieser Beschreibungen zu Ihrer App passt, empfiehlt es sich unter Umständen, sich am Systemdesign zu orientieren und den Benutzer selbst entscheiden zu lassen.

Zur Vereinfachung der Designentwicklung bietet Windows eine zusätzliche Farbpalette, die sich automatisch an das Design anpasst.


<!-- OP version -->
### Helles Design
#### Basis
![Das helle Basisdesign](images/themes-light-base.png)
#### Alternativ
![Das helle Alternativdesign](images/themes-light-alt.png)
#### Liste
![Das helle Listendesign](images/themes-light-list.png)
#### Chrom
![Das helle Chromdesign](images/themes-light-chrome.png)
### Dunkles Design
#### Basis
![Das dunkle Basisdesign](images/themes-dark-base.png)
#### Alternativ
![Das dunkle Alternativdesign](images/themes-dark-alt.png)
#### Liste
![Das dunkle Listendesign](images/themes-dark-list.png)
#### Chrom
![Das dunkle Chromdesign](images/themes-dark-chrome.png)


<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
            Each color is available as a XAML [theme resource](https://msdn.microsoft.com/en-us/library/windows/apps/Mt187274.aspx#the_xaml_color_ramp_and_theme-dependent_brushes) that follows the `System*Color` naming convention (ex: `SystemChromeHighColor`). You can control your app's theme through either [Application.RequestedTheme](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.requestedtheme.aspx) or [FrameworkElement.RequestedTheme](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.requestedtheme.aspx).
    </div>
</aside>

## Barrierefreiheit

Unsere Palette ist für die Verwendung auf Bildschirmen optimiert. Zur optimalen Lesbarkeit von Text wird ein Mindestkontrastverhältnis von 4,5: 1 empfohlen.


<!--HONumber=Mar16_HO5-->


