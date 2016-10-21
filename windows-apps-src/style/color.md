---
author: mijacobs
description: "Farben tragen dazu bei, dass sich Benutzer intuitiv in den verschiedenen Informationsebenen einer App zurechtfinden, und spielen eine wichtige Rolle für das Interaktionsmodell."
title: Farben
ms.assetid: 3ba7176f-ac47-498c-80ed-4448edade8ad
template: detail.hbs
extraBodyClass: style-color
translationtype: Human Translation
ms.sourcegitcommit: d7236006f2c620a4ff0de4e0f413f32a2eaf5687
ms.openlocfilehash: 8e253c93f932e04b825478cf0801e4c8c0d43b9d

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

## Farbpaletten-Bausteine

Nachdem eine Akzentfarbe ausgewählt wurde, werden auf der Grundlage der HSB-Werte für die Leuchtdichte helle und dunkle Schattierungen der Akzentfarbe erstellt. Mithilfe von Schattierungsvarianten können Apps eine visuelle Hierarchie erstellen und Interaktionen darstellen.

Standardmäßig werden Hyperlinks in der Akzentfarbe des Benutzers dargestellt. Zeichnet sich der Hintergrund der Seite durch eine ähnliche Farbe aus, können Sie den Hyperlinks für einen besseren Kontrast einen helleren (oder dunkleren) Akzent-Farbton zuweisen.

![Eine einzelne Akzentfarbe mit sechs Schattierungen](images/shades.png) Die unterschiedlichen Hell/Dunkel-Töne der Standard-Akzentfarbe.

![Redlines für ein farbiges Info-Center](images/action_center_redline_zoom.png) Ein Beispiel dafür, wie Farblogik auf eine Designspezifikation angewendet wird.

**Hinweis**&nbsp;&nbsp;In XAML wird die primäre Akzentfarbe verfügbar gemacht als eine [Designressource](https://msdn.microsoft.com/library/windows/apps/Mt187274.aspx) mit dem Namen `SystemAccentColor`. Die Farbtöne stehen als `SystemAccentColorLight3`, `SystemAccentColorLight2`, `SystemAccentColorLight1`, `SystemAccentColorDark1`, `SystemAccentColorDark2` und `SystemAccentColorDark3` zur Verfügung. Auch programmgesteuert verfügbar über die Enumeration [UISettings.GetColorValue](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx) und [UIColorType](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx).

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


## Ändern des Designs

Sie können Designs einfach ändern, indem Sie die **RequestedTheme**-Eigenschaft in „App.xaml“ ändern:

```XAML
<Application
    x:Class="App9.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App9"
    RequestedTheme="Dark">

</Application>
```

Das Entfernen von **RequestedTheme** bedeutet, dass Ihre Anwendung die App-Moduseinstellungen des Benutzers berücksichtigt. Dieser kann festlegen, dass die App im dunklen oder hellen Design angezeigt wird. 

Berücksichtigen Sie beim Erstellen Ihrer App unbedingt das Design, da dieses einen großen Einfluss auf das Erscheinungsbild des App hat.

## Barrierefreiheit

Unsere Palette ist für die Verwendung auf Bildschirmen optimiert. Zur optimalen Lesbarkeit von Text wird ein Mindestkontrastverhältnis gegenüber dem Hintergrund von 4,5:1 empfohlen. Es gibt viele kostenlose Tools, mit denen Sie testen können, ob Ihre Farben geeignet sind, z.B. [Contrast Ratio](http://leaverou.github.io/contrast-ratio/).

## Verwandte Artikel

* [XAML-Formatvorlagen](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/xaml-theme-resources)
* [XAML-Designressourcen](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/xaml-theme-resources)



<!--HONumber=Aug16_HO3-->


