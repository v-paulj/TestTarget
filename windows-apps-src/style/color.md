---
author: mijacobs
Description: "Farben tragen dazu bei, dass sich Benutzer intuitiv in den verschiedenen Informationsebenen einer App zurechtfinden, und spielen eine wichtige Rolle für das Interaktionsmodell."
title: Farben
ms.assetid: 3ba7176f-ac47-498c-80ed-4448edade8ad
label: Color
template: detail.hbs
extraBodyClass: style-color
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 878470a7cbf44862c47a1428a1d25d332db32fdc

---

# Farben

Farben tragen dazu bei, dass sich Benutzer intuitiv in den verschiedenen Informationsebenen einer App zurechtfinden, und spielen eine wichtige Rolle für das Interaktionsmodell.

Unter Windows sind Farben auch individuell anpassbar. Benutzer können eine Farbe sowie ein helles oder dunkles Design auswählen, das sich auf der gesamten Benutzeroberfläche widerspiegelt.

## Akzentfarbe

Benutzer können eine einzelne Farbe, die sogenannte Akzentfarbe, auswählen: *Einstellungen > Personalisierung > Farben*. Ihnen stehen dabei 48 speziell zusammengestellte Farbmuster zur Verfügung. Für Xbox beschränkt sich die Farbpalette auf 21 fernsehsichere Farben.

<!-- Alternate version for the dev center. Need to add hex values. -->
![Standard-Akzentfarben](images/accentcolorswatch.png) Standard-Akzentfarben

![Xbox-Akzentfarben](images/accentcolorswatch_xbox.png) Xbox-Akzentfarben



Wenn der Benutzer eine Akzentfarbe auswählt, wird sie Teil des Systemdesigns. Davon sind dann Startseite, Taskleiste, Fensterchrom, bestimmte Interaktionszustände und Links mit [allgemeinen Steuerelementen](https://dev.windows.com/design/controls-patterns) betroffen. Bei Apps besteht zudem die Möglichkeit, die Akzentfarbe in die Typografie, Hintergründe und Interaktionen zu integrieren – oder sie zu ignorieren, um das Branding der jeweiligen App nicht zu verändern.

## Farbe auf Farbe

Nachdem eine Akzentfarbe ausgewählt wurde, werden auf der Grundlage der HSB-Werte für die Leuchtdichte helle und dunkle Schattierungen der Akzentfarbe erstellt. Mithilfe von Schattierungsvarianten können Apps eine visuelle Hierarchie erstellen und Interaktionen darstellen.

Standardmäßig werden Hyperlinks in der Akzentfarbe des Benutzers dargestellt. Zeichnet sich der Hintergrund der Seite durch eine ähnliche Farbe aus, können Sie den Hyperlinks für einen besseren Kontrast einen helleren (oder dunkleren) Akzent-Farbton zuweisen.

<figure class="figure-img" >
    <img src="images/shades.png" alt="A single accent color with its 6 shades"  />
        <figcaption><p>Die unterschiedlichen Hell/Dunkel-Töne der Standard-Akzentfarbe.</p>
</figcaption>
</figure>

<figure class="figure-img" >
    <img src="images/action_center_redline_zoom.png" alt="Redlines for Colored Action Center"  />
        <figcaption><p>Ein Beispiel dafür, wie Farblogik auf eine Designspezifikation angewendet wird.</p>
</figcaption>
</figure>

<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
In XAML wird die primäre Akzentfarbe verfügbar gemacht als eine [Designressource](https://msdn.microsoft.com/library/windows/apps/Mt187274.aspx) mit dem Namen `SystemAccentColor`. Die Farbtöne stehen als `SystemAccentColorLight3`, `SystemAccentColorLight2`, `SystemAccentColorLight1`, `SystemAccentColorDark1`, `SystemAccentColorDark2` und `SystemAccentColorDark3` zur Verfügung. Auch programmgesteuert verfügbar über die Enumeration[UISettings.GetColorValue](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx) und [UIColorType](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx).
    </div>
</aside>

## Farbdesigns

Der Benutzer kann zwischen einem hellen und einem dunklen Design für das System wählen. Einige Apps passen Ihr Design auf der Grundlage der Einstellung des Benutzers an, andere dagegen nicht.

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
Jede Farbe steht als eine XAML-[Designressource](https://msdn.microsoft.com/library/windows/apps/Mt187274.aspx#the_xaml_color_ramp_and_theme-dependent_brushes) zur Verfügung und folgt der `System*Color`-Benennungskonvention (z. B.: `SystemChromeHighColor`). Sie können Ihr App-Design über [Application.RequestedTheme](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.requestedtheme.aspx) oder [FrameworkElement.RequestedTheme](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.requestedtheme.aspx) steuern.
    </div>
</aside>

## Barrierefreiheit

Unsere Palette ist für die Verwendung auf Bildschirmen optimiert. Zur optimalen Lesbarkeit von Text wird ein Mindestkontrastverhältnis gegenüber dem Hintergrund von 4,5:1 empfohlen. Es gibt viele kostenlose Tools, mit denen Sie testen können, ob Ihre Farben geeignet sind, z.B. [Kontrastverhältnis](http://leaverou.github.io/contrast-ratio/).



<!--HONumber=Jun16_HO4-->


