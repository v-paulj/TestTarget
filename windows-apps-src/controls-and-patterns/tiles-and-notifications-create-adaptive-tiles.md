---
author: mijacobs
Description: Vorlagen für adaptive Kacheln sind ein neues Feature in Windows 10 und ermöglichen den Entwurf eigener Inhalte für Kachelbenachrichtigungen mithilfe einer einfachen, flexiblen Markupsprache, die sich an unterschiedliche Bildschirmdichten anpasst.
title: Erstellen adaptiver Kacheln
ms.assetid: 1246B58E-D6E3-48C7-AD7F-475D113600F9
label: Create adaptive tiles
template: detail.hbs
---

# Erstellen adaptiver Kacheln





Vorlagen für adaptive Kacheln sind ein neues Feature in Windows 10 und ermöglichen den Entwurf eigener Inhalte für Kachelbenachrichtigungen mithilfe einer einfachen, flexiblen Markupsprache, die sich an unterschiedliche Bildschirmdichten anpasst. Dieser Artikel beschreibt, wie Sie adaptive Live-Kacheln für Ihre UWP-App (Universelle Windows-Plattform) erstellen. Die vollständige Liste adaptiver Elemente und Attribute finden Sie unter [Adaptives Kachelschema](tiles-and-notifications-adaptive-tiles-schema.md).

(Falls gewünscht, können Sie weiterhin die voreingestellten Vorlagen aus dem [Windows 8-Kachelvorlagenkatalog](https://msdn.microsoft.com/library/windows/apps/hh761491) beim Entwerfen von Benachrichtigungen für Windows 10 verwenden.)

## <span id="Getting_started"></span><span id="getting_started"></span><span id="GETTING_STARTED"></span>Erste Schritte


**Installieren von NotificationsExtensions.** Wenn Sie lieber C# anstatt XML zum Generieren von Benachrichtigungen verwenden möchten, installieren Sie das NuGet-Paket [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki). Für die in diesem Artikel bereitgestellten C#-Beispiele werden NotificationsExtensions verwendet.

**Installieren von Notifications Visualizer.** Diese kostenlose UWP-App erleichtert das Entwerfen adaptiver Live-Kacheln, indem Ihre Kachel während der Bearbeitung in einer sofortigen visuellen Vorschau dargestellt wird, die mit dem XAML-Editor bzw. der Entwurfsansicht in Visual Studio vergleichbar ist. Weitere Informationen finden Sie in [diesem Blogbeitrag](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/09/22/introducing-notifications-visualizer-for-windows-10.aspx). Der Download von Notifications Visualizer steht [hier](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) bereit.

## <span id="Usage_guidance"></span><span id="usage_guidance"></span><span id="USAGE_GUIDANCE"></span>Informationen zur Verwendung


Adaptive Vorlagen sind für die Unterstützung verschiedener Formfaktoren und Benachrichtigungstypen konzipiert. Elemente wie Gruppen und Untergruppen verknüpfen Inhalte miteinander und implizieren selbst kein bestimmtes visuelles Verhalten. Die endgültige Darstellung einer Benachrichtigung sollte auf dem spezifischen Gerät basieren, auf dem sie angezeigt wird, beispielsweise einem Smartphone, Tablet, Desktop oder anderen Gerät.

Hinweise sind optionale Attribute, die Elementen hinzugefügt werden können, um ein bestimmtes visuelles Verhalten zu erzielen. Hinweise können spezifisch für Geräte oder Benachrichtigungen sein.

## <span id="A_basic_example"></span><span id="a_basic_example"></span><span id="A_BASIC_EXAMPLE"></span>Ein einfaches Beispiel


Dieses Beispiel veranschaulicht, was Vorlagen für adaptive Kacheln leisten können.

```XML
<tile>
  <visual>
  
    <binding template="TileMedium">
      ...
    </binding>
  
    <binding template="TileWide">
      <text hint-style="subtitle">Jennifer Parker</text>
      <text hint-style="captionSubtle">Photos from our trip</text>
      <text hint-style="captionSubtle">Check out these awesome photos I took while in New Zealand!</text>
    </binding>
  
    <binding template="TileLarge">
      ...
    </binding>
  
  </visual>
</tile>
```

```CSharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = ...
  
        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new TileText()
                    {
                        Text = "Jennifer Parker",
                        Style = TileTextStyle.Subtitle
                    },
  
                    new TileText()
                    {
                        Text = "Photos from our trip",
                        Style = TileTextStyle.CaptionSubtle
                    },
  
                    new TileText()
                    {
                        Text = "Check out these awesome photos I took while in New Zealand!",
                        Style = TileTextStyle.CaptionSubtle
                    }
                }
            }
        },
  
        TileLarge = ...
    }
};
```

**Ergebnis:**

![Kurzes Beispiel für eine Kachel](images/adaptive-tiles-quicksample.png)

## <span id="Tile_sizes"></span><span id="tile_sizes"></span><span id="TILE_SIZES"></span>Kachelgrößen


Der Inhalt für jede Kachelgröße wird einzeln in getrennten [&lt;binding&gt;](tiles-and-notifications-adaptive-tiles-schema.md)-Elementen innerhalb der XML-Nutzlast angegeben. Wählen Sie die Zielgröße aus, indem Sie das template-Attribut auf einen der folgenden Werte festlegen:

-   TileSmall
-   TileMedium
-   TileWide
-   TileLarge (nur für Desktop)

Geben Sie für die XML-Nutzlast einer einzelnen Kachelbenachrichtigung &lt;binding&gt;-Elemente für jede zu unterstützende Kachelgröße an, wie im folgenden Beispiel dargestellt:

```XML
<tile>
  <visual>
  
    <binding template="TileSmall">
      <text>Small</text>
    </binding>
  
    <binding template="TileMedium">
      <text>Medium</text>
    </binding>
  
    <binding template="TileWide">
      <text>Wide</text>
    </binding>
  
    <binding template="TileLarge">
      <text>Large</text>
    </binding>
  
  </visual>
</tile>
```

```CSharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileSmall = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new TileText() { Text = "Small" }
                }
            }
        },
  
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new TileText() { Text = "Medium" }
                }
            }
        },
  
        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new TileText() { Text = "Wide" }
                }
            }
        },
  
        TileLarge = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new TileText() { Text = "Large" }
                }
            }
        }
    }
};
```

**Ergebnis:**

![Größen für adaptive Kacheln: klein, mittel, breit und groß](images/adaptive-tiles-sizes.png)

## <span id="Branding"></span><span id="branding"></span><span id="BRANDING"></span>Branding


Sie können das Branding am unteren Rand einer Live-Kachel (den Anzeigenamen und das Cornerlogo) mit dem branding-Attribut in der Benachrichtigungsnutzlast steuern. Mit „none“ wird nichts angezeigt, mit „name“ nur der Name, mit „logo“ nur das Logo, und mit „nameAndLogo“ werden Name und Logo angezeigt.

**Hinweis**  Da Windows Phone kein Cornerlogo unterstützt, wird auf einem Smartphone anstelle von „logo“ und „nameAndLogo“ standardmäßig „name” verwendet.

 

```XML
<visual branding="logo">
  ...
</visual>
```

```CSharp
new TileVisual()
{
    Branding = TileBranding.Logo,
    ...
}

new TileVisual()
{
    Branding = TileBranding.Logo,
    ...
}
```

**Ergebnis:**

![Adaptive Kacheln, Name und Logo](images/adaptive-tiles-namelogo.png)

Das Branding kann für bestimmte Kachelgrößen auf zwei Weisen angewendet werden:

1. Durch Anwenden des Attributs auf das [&lt;binding&gt;](tiles-and-notifications-adaptive-tiles-schema.md)-Element
2. Durch Anwenden des Attributs auf das [&lt;visual&gt;](tiles-and-notifications-adaptive-tiles-schema.md) -Element, was sich auf die gesamte Benachrichtigungsnutzlast auswirkt. Wenn Sie für eine Bindung kein Branding angeben, wird das Branding verwendet, das auf dem visual-Element bereitgestellt wird.

```XML
<tile>
  <visual branding="nameAndLogo">
 
    <binding template="TileMedium" branding="logo">
      ...
    </binding>
 
    <!--Inherits branding from visual-->
    <binding template="TileWide">
      ...
    </binding>
 
  </visual>
</tile>
```

```CSharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        Branding = TileBranding.NameAndLogo,
 
        TileMedium = new TileBinding()
        {
            Branding = TileBranding.Logo,
            ...
        },
 
        // Inherits branding from Visual
        TileWide = new TileBinding()
        {
            ...
        }
    }
};
```

**Ergebnis bei Standardbranding:**

![Standardbranding für Kacheln](images/adaptive-tiles-defaultbranding.png)

Wenn Sie in der Benachrichtigungsnutzlast kein Branding angeben, wird das Branding durch die Eigenschaften der Basiskachel bestimmt. Wenn auf der Basiskachel der Anzeigename dargestellt ist, wird für das Branding standardmäßig „name“ verwendet. Wenn kein Anzeigename vorhanden ist, wird für das Branding standardmäßig „none“ verwendet.

**Hinweis**  Dies ist eine Änderung gegenüber Windows 8.x, wo das Standardbranding „logo“ lautete.

 

## <span id="Display_name"></span><span id="display_name"></span><span id="DISPLAY_NAME"></span>Anzeigename


Sie können den Anzeigenamen einer Benachrichtigung überschreiben, indem Sie für das **displayName**-Attribut die gewünschte Textzeichenfolge eingeben. Wie beim Branding können Sie dies für das [&lt;visual&gt;](tiles-and-notifications-adaptive-tiles-schema.md)-Element angeben, was sich auf die gesamte Benachrichtigungsnutzlast auswirkt, oder für das [&lt;binding&gt;](tiles-and-notifications-adaptive-tiles-schema.md)-Element, was nur einzelne Kacheln betrifft.

```XML
<tile>
  <visual branding="nameAndLogo" displayName="Wednesday 22">
 
    <binding template="TileMedium" displayName="Wed. 22">
      ...
    </binding>
 
    <!--Inherits displayName from visual-->
    <binding template="TileWide">
      ...
    </binding>
 
  </visual>
</tile>
```

```CSharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        Branding = TileBranding.NameAndLogo,
        DisplayName = "Wednesday 22",
 
        TileMedium = new TileBinding()
        {
            DisplayName = "Wed. 22",
            ...
        },
 
        // Inherits DisplayName from Visual
        TileWide = new TileBinding()
        {
            ...
        }
    }
};
```

**Ergebnis:**

![Anzeigename adaptiver Kacheln](images/adaptive-tiles-displayname.png)

## <span id="Text"></span><span id="text"></span><span id="TEXT"></span>Text


Das [&lt;text&gt;](tiles-and-notifications-adaptive-tiles-schema.md)-Element wird zum Anzeigen von Text verwendet. Mithilfe von Hinweisen können Sie die Darstellung von Text anpassen.

```XML
<text>This is a line of text</text></code></pre></td>
</tr>
</tbody>
</table>
```

<span codelanguage="CSharp"></span>
```CSharp
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
new TileText()
{
    Text = "This is a line of text"
};
```

**Ergebnis:**

![Text adaptiver Kacheln](images/adaptive-tiles-text.png)

## <span id="Text_wrapping"></span><span id="text_wrapping"></span><span id="TEXT_WRAPPING"></span>Textumbruch


Text wird standardmäßig nicht umbrochen und verläuft über den Kachelrand hinaus. Verwenden Sie das **hint-wrap**-Attribut, um den Textumbruch für ein text-Element festzulegen. Mit **hint-minLines** und **hint-maxLines**, die beide positive ganze Zahlen akzeptieren, können Sie auch die minimale und maximale Zeilenanzahl steuern.

```XML
<text hint-wrap="true">This is a line of wrapping text</text></code></pre></td>
</tr>
</tbody>
</table>
```

<span codelanguage="CSharp"></span>
```CSharp
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
new TileText()
{
    Text = "This is a line of wrapping text",
    Wrap = true
};
```

**Ergebnis:**

![Adaptive Kachel mit Textumbruch](images/adaptive-tiles-textwrapping.png)

## <span id="Text_styles"></span><span id="text_styles"></span><span id="TEXT_STYLES"></span>Textstile


Stile steuern den Schriftgrad, die Schriftfarbe und Schriftbreite von text-Elementen. Es sind mehrere Stile verfügbar. Zusätzlich gibt es leichte („subtle“) Variationen jedes Stils, durch die die Deckkraft auf 60 % festgelegt und die Textfarbe normalerweise in einem hellgrauen Farbton angezeigt wird.

```XML
<text hint-style="base">Header content</text>
<text hint-style="captionSubtle">Subheader content</text>
```

```CSharp
new TileText()
{
    Text = "Header content",
    Style = TileTextStyle.Base
},
 
new TileText()
{
    Text = "Subheader content",
    Style = TileTextStyle.CaptionSubtle
}
```

**Ergebnis:**

![Textstile adaptiver Kacheln](images/adaptive-tiles-textstyles.png)

**Hinweis**  Wenn „hint-style“ nicht angegeben ist, wird für den Stil standardmäßig „caption“ verwendet.

 

**Allgemeine Textstile**

|                                |                           |             |
|--------------------------------|---------------------------|-------------|
| &lt;text hint-style="\*" /&gt; | Zeichenhöhe               | Schriftbreite |
| caption                        | 12 effektive Pixel (epx) | Regular     |
| body                           | 15 Epx                    | Regular     |
| base                           | 15 Epx                    | Semibold    |
| subtitle                       | 20 Epx                    | Regular     |
| title                          | 24 Epx                    | Semilight   |
| subheader                      | 34 Epx                    | Light       |
| header                         | 46 Epx                    | Light       |

 

**Numerische Variationen des Textstils**

Durch diese Variationen wird die Zeilenhöhe verringert, sodass der Abstand zu Inhalten über und unter der Zeile deutlich kleiner wird.

|                  |
|------------------|
| titleNumeral     |
| subheaderNumeral |
| headerNumeral    |

 

**Leichte Variationen des Textstils**

Jeder Stil weist eine leichte Variation auf, durch die der Text eine 60 %-ige Deckkraft erhält und normalerweise in einem hellgrauen Farbton angezeigt wird.

|                        |
|------------------------|
| captionSubtle          |
| bodySubtle             |
| baseSubtle             |
| subtitleSubtle         |
| titleSubtle            |
| titleNumeralSubtle     |
| subheaderSubtle        |
| subheaderNumeralSubtle |
| headerSubtle           |
| headerNumeralSubtle    |

 

## <span id="Text_alignment"></span><span id="text_alignment"></span><span id="TEXT_ALIGNMENT"></span>Textausrichtung


Text kann horizontal, linksbündig, zentriert oder rechtsbündig ausgerichtet sein. Bei einer von links nach rechts gelesenen Sprache, wie Deutsch, ist Text standardmäßig linksbündig ausgerichtet. Bei einer von rechts nach links gelesenen Sprache, wie Arabisch, ist Text standardmäßig rechtsbündig ausgerichtet. Mit dem **hint-align**-Attribut können Sie die Ausrichtung für Elemente manuell festlegen.

```XML
<text hint-align="center">Hello</text></code></pre></td>
</tr>
</tbody>
</table>
```

<span codelanguage="CSharp"></span>
```CSharp
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
new TileText()
{
    Text = "Hello",
    Align = TileTextAlign.Center
};
```

**Ergebnis:**

![Textausrichtung adaptiver Kacheln](images/adaptive-tiles-textalignment.png)

## <span id="Groups_and_subgroups"></span><span id="groups_and_subgroups"></span><span id="GROUPS_AND_SUBGROUPS"></span>Gruppen und Untergruppen


Mit Gruppen können Sie semantisch deklarieren, dass sich Inhalte in der Gruppe aufeinander beziehen und vollständig angezeigt werden müssen, damit sie Sinn ergeben. Beispielsweise können Sie über zwei text-Elemente in Form einer Überschrift und einer Unterüberschrift verfügen, und es würde keinen Sinn ergeben, wenn nur die Überschrift angezeigt würde. Durch die Gruppierung dieser Elemente innerhalb einer Untergruppe werden entweder alle Elemente angezeigt (sofern sie in den Anzeigebereich passen) oder keine Elemente angezeigt (wenn nicht genügend Platz vorhanden ist).

Um optimale Ergebnisse auf unterschiedlichen Geräten und Bildschirmen zu erzielen, sollten Sie mehrere Gruppen bereitstellen. Mit mehreren Gruppen kann sich die Kachel an größere Bildschirme anpassen.

**Hinweis**  Das einzige gültige untergeordnete Element einer Gruppe ist eine Untergruppe.

 

```XML
...
<binding template="TileWide" branding="nameAndLogo">
  <group>
    <subgroup>
      <text hint-style="subtitle">Jennifer Parker</text>
      <text hint-style="captionSubtle">Photos from our trip</text>
      <text hint-style="captionSubtle">Check out these awesome photos I took while in New Zealand!</text>
    </subgroup>
  </group>
 
  <text />
 
  <group>
    <subgroup>
      <text hint-style="subtitle">Steve Bosniak</text>
      <text hint-style="captionSubtle">Build 2015 Dinner</text>
      <text hint-style="captionSubtle">Want to go out for dinner after Build tonight?</text>
    </subgroup>
  </group>
</binding>
...
```

```CSharp
...
 
TileWide = new TileBinding()
{
    Branding = TileBranding.NameAndLogo,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            CreateGroup(
                from: "Jennifer Parker",
                subject: "Photos from our trip",
                body: "Check out these awesome photos I took while in New Zealand!"),
 
            // For spacing
            new TileText(),
 
            CreateGroup(
                from: "Steve Bosniak",
                subject: "Build 2015 Dinner",
                body: "Want to go out for dinner after Build tonight?")
        }
    }
}
 
...
 
 
private static TileGroup CreateGroup(string from, string subject, string body)
{
    return new TileGroup()
    {
        Children =
        {
            new TileSubgroup()
            {
                Children =
                {
                    new TileText()
                    {
                        Text = from,
                        Style = TileTextStyle.Subtitle
                    },
 
                    new TileText()
                    {
                        Text = subject,
                        Style = TileTextStyle.CaptionSubtle
                    },
 
                    new TileText()
                    {
                        Text = body,
                        Style = TileTextStyle.CaptionSubtle
                    }
                }
            }
        }
    };
}
```

**Ergebnis:**

![Gruppen und Untergruppen adaptiver Kacheln](images/adaptive-tiles-groups-subgroups.png)

## <span id="Subgroups__columns_"></span><span id="subgroups__columns_"></span><span id="SUBGROUPS__COLUMNS_"></span>Untergruppen (Spalten)


Mithilfe von Untergruppen können Sie Daten außerdem in semantische Abschnitte innerhalb einer Gruppe unterteilen. Bei Live-Kacheln werden Untergruppen visuell als Spalten dargestellt.

Mit dem **hint-weight**-Attribut wird die Breite von Spalten gesteuert. Der **hint-weight**-Wert wird als gewichteter Anteil am verfügbaren Platz ausgedrückt, was mit dem **GridUnitType.Star**-Verhalten übereinstimmt. Weisen Sie jeder Gewichtung 1 zu, um Spalten mit gleicher Breite zu erhalten.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">hint-weight</td>
<td align="left">Prozentuale Breite</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">25 %</td>
</tr>
<tr class="odd">
<td align="left">1</td>
<td align="left">25 %</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">25 %</td>
</tr>
<tr class="odd">
<td align="left">1</td>
<td align="left">25 %</td>
</tr>
<tr class="even">
<td align="left">Gesamtgewichtung: 4</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![Untergruppen, Spalten mit gleicher Breite](images/adaptive-tiles-subgroups01.png)

Um eine Spalte doppelt so groß wie eine andere Spalte darzustellen, weisen Sie der kleineren Spalte die Gewichtung 1 und der größeren Spalte die Gewichtung 2 zu.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">hint-weight</td>
<td align="left">Prozentuale Breite</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">33,3 %</td>
</tr>
<tr class="odd">
<td align="left">2</td>
<td align="left">66,7 %</td>
</tr>
<tr class="even">
<td align="left">Gesamtgewichtung: 3</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![Untergruppen, eine Spalte ist doppelt so groß wie die andere](images/adaptive-tiles-subgroups02.png)

Wenn Ihre erste Spalte 20 % und die zweite Spalte 80 % der gesamten Breite einnehmen soll, weisen Sie der ersten Gewichtung 20 und der zweiten Gewichtung 80 zu. Wenn die Gewichtungen insgesamt 100 ergeben, werden sie prozentual ausgedrückt.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">hint-weight</td>
<td align="left">Prozentuale Breite</td>
</tr>
<tr class="even">
<td align="left">20</td>
<td align="left">20 %</td>
</tr>
<tr class="odd">
<td align="left">80</td>
<td align="left">80 %</td>
</tr>
<tr class="even">
<td align="left">Gesamtgewichtung: 100</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![Untergruppen mit einer Gesamtgewichtung von 100](images/adaptive-tiles-subgroups03.png)

**Hinweis**  Zwischen Spalten wird automatisch ein Rand von 8 Pixeln eingefügt.

 

Wenn Sie über mehr als zwei Untergruppen verfügen, geben Sie **hint-weight** an (akzeptiert nur positive ganze Zahlen). Wenn Sie für die erste Untergruppe „hint-weight“ nicht angeben, wird ihr die Gewichtung 50 zugewiesen. Der nächsten Untergruppe, für die „hint-weight“ nicht angegeben wurde, wird die Gewichtung 100 abzüglich der Summe der vorherigen Gewichtungen oder 1 zugewiesen, wenn das Ergebnis 0 (null) ist. Den übrigen Untergruppen, für die „hint-weight“ nicht angegeben wurde, wird die Gewichtung 1 zugewiesen.

Im Folgenden sehen Sie den Beispielcode für eine Wetter-Kachel, die zeigt, wie Sie eine Kachel mit fünf gleich breiten Spalten erhalten:

```XML
...
<binding template="TileWide" displayName="Seattle" branding="name">
  <group>
    <subgroup hint-weight="1">
      <text hint-align="center">Mon</text>
      <image src="Assets\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Tue</text>
      <image src="Assets\Weather\Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-align="center" hint-style="captionsubtle">38°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Wed</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">59°</text>
      <text hint-align="center" hint-style="captionsubtle">43°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Thu</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">62°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Fri</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">71°</text>
      <text hint-align="center" hint-style="captionsubtle">66°</text>
    </subgroup>
  </group>
</binding>
...
```

```CSharp
...
TileWide = new TileBinding()
{
    DisplayName = "Seattle",
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new TileGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°"),
 
                    CreateSubgroup("Tue", "Cloudy.png", "57°", "38°"),
 
                    CreateSubgroup("Wed", "Sunny.png", "59°", "43°"),
 
                    CreateSubgroup("Thu", "Sunny.png", "62°", "42°"),
 
                    CreateSubgroup("Fri", "Sunny.png", "71°", "66°")
                }
            }
        }
    }
}
...
 
 
private static TileSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new TileSubgroup()
    {
        Weight = 1,
 
        Children =
        {
            new TileText()
            {
                Text = day,
                Align = TileTextAlign.Center
            },
 
            new TileImage()
            {
                Source = new TileImageSource("Assets/Weather/" + image),
                RemoveMargin = true
            },
 
            new TileText()
            {
                Text = highTemp,
                Align = TileTextAlign.Center
            },
 
            new TileText()
            {
                Text = lowTemp,
                Align = TileTextAlign.Center,
                Style = TileTextStyle.CaptionSubtle
            }
        }
    };
}
```

**Ergebnis:**

![Beispiel für eine Wetter-Kachel](images/adaptive-tiles-weathertile.png)

## <span id="Images"></span><span id="images"></span><span id="IMAGES"></span>Bilder


Mithilfe des &lt;image&gt;-Elements werden Bilder auf der Kachelbenachrichtigung angezeigt. Bilder können als Inlinebilder im Kachelinhalt (Standard), als Hintergrundbild hinter dem Inhalt oder als animiertes Vorschaubild, das von oben in die Benachrichtigung hineingleitet, konfiguriert werden.

**Hinweis**  Für die [Dateigröße und Abmessungen von Bildern](https://msdn.microsoft.com/library/windows/apps/hh781198) gelten Einschränkungen.

 

Wenn kein zusätzliches Verhalten angegeben wird, verkleinern bzw. vergrößern sich Bilder gleichmäßig in Anpassung an die verfügbare Breite. Das folgende Beispiel zeigt eine Kachel mit zwei Spalten und Inlinebildern. Die Inlinebilder werden gestreckt, um die Spaltenbreite auszufüllen.

```XML
...
<binding template="TileMedium" displayName="Seattle" branding="name">
  <group>
    <subgroup>
      <text hint-align="center">Mon</text>
      <image src="Assets\Apps\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-style="captionsubtle" hint-align="center">42°</text>
    </subgroup>
    <subgroup>
      <text hint-align="center">Tue</text>
      <image src="Assets\Apps\Weather\Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-style="captionSubtle" hint-align="center">38°</text>
    </subgroup>
  </group>
</binding>
...
```

```CSharp
...
TileMedium = new TileBinding()
{
    DisplayName = "Seattle",
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new TileGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°"),
 
                    CreateSubgroup("Tue", "Cloudy.png", "57°", "38°")
                }
            }
        }
    }
}
...
 
 
private static TileSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new TileSubgroup()
    {
        Children =
        {
            new TileText()
            {
                Text = day,
                Align = TileTextAlign.Center
            },
 
            new TileImage()
            {
                Source = new TileImageSource("Assets/Weather/" + image),
                RemoveMargin = true
            },
 
            new TileText()
            {
                Text = highTemp,
                Align = TileTextAlign.Center
            },
 
            new TileText()
            {
                Text = lowTemp,
                Align = TileTextAlign.Center,
                Style = TileTextStyle.CaptionSubtle
            }
        }
    };
}
```

**Ergebnis:**

![Bildbeispiel](images/adaptive-tiles-images01.png)

Bilder, die im &lt;binding&gt;-Stammelement oder in der ersten Gruppe enthalten sind, werden ebenfalls gestreckt, um die verfügbare Höhe auszunutzen.

### <span id="Image_alignment"></span><span id="image_alignment"></span><span id="IMAGE_ALIGNMENT"></span>Bildausrichtung

Bilder können mit dem **hint-align**-Attribut linksbündig, zentriert oder rechtsbündig ausgerichtet werden. Dies bewirkt auch, dass Bilder in ihrer systemeigenen Auflösung angezeigt und nicht gestreckt werden, um die gesamte Breite auszufüllen.

```XML
...
<binding template="TileLarge">
  <image src="Assets/fable.jpg" hint-align="center"/>
</binding>
...
```

```CSharp
...
TileLarge = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new TileImage()
            {
                Source = new TileImageSource("Assets/fable.jpg"),
                Align = TileImageAlign.Center
            }
        }
    }
}
...
```

**Ergebnis:**

![Beispiel für die Bildausrichtung (linksbündig, zentriert, rechtsbündig)](images/adaptive-tiles-imagealignment.png)

### <span id="Image_margins"></span><span id="image_margins"></span><span id="IMAGE_MARGINS"></span>Bildränder

Zwischen Inlinebildern und darüber oder darunter angeordneten Inhalten befindet sich standardmäßig ein Rand von 8 Pixeln. Dieser Rand kann entfernt werden, indem Sie das **hint-removeMargin**-Attribut für das Bild verwenden. Für Bilder wird allerdings immer der 8-Pixel-Rand vom Rand der Kachel und für Untergruppen (Spalten) immer der 8-Pixel-Abstand zwischen Spalten beibehalten.

```XML
...
<binding template="TileMedium" branding="none">
  <group>
    <subgroup>
      <text hint-align="center">Mon</text>
      <image src="Assets\Numbers\4.jpg" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-style="captionsubtle" hint-align="center">42°</text>
    </subgroup>
    <subgroup>
      <text hint-align="center">Tue</text>
      <image src="Assets\Numbers\3.jpg" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-style="captionsubtle" hint-align="center">38°</text>
    </subgroup>
  </group>
</binding>
...
```

```CSharp
...
 
TileMedium = new TileBinding()
{
    Branding = TileBranding.None,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new TileGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "4.jpg", "63°", "42°"),
 
                    CreateSubgroup("Tue", "3.jpg", "57°", "38°")
                }
            }
        }
    }
}
 
...
 
 
private static TileSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new TileSubgroup()
    {
        Weight = 1,
 
        Children =
        {
            new TileText()
            {
                Text = day,
                Align = TileTextAlign.Center
            },
 
            new TileImage()
            {
                Source = new TileImageSource("Assets/Numbers/" + image),
                RemoveMargin = true
            },
 
            new TileText()
            {
                Text = highTemp,
                Align = TileTextAlign.Center
            },
 
            new TileText()
            {
                Text = lowTemp,
                Align = TileTextAlign.Center,
                Style = TileTextStyle.CaptionSubtle
            }
        }
    };
}
```

![Beispiel für „hint-removeMargin“](images/adaptive-tiles-removemargin.png)

### <span id="Image_cropping"></span><span id="image_cropping"></span><span id="IMAGE_CROPPING"></span>Zuschneiden von Bildern

Bilder können mit dem **hint-crop**-Attribut, das derzeit nur die Werte „none“ (Standard) oder „circle“ unterstützt, kreisförmig zugeschnitten werden.

```XML
...
<binding template="TileLarge" hint-textStacking="center">
  <group>
    <subgroup hint-weight="1"/>
    <subgroup hint-weight="2">
      <image src="Assets/Apps/Hipstame/hipster.jpg" hint-crop="circle"/>
    </subgroup>
    <subgroup hint-weight="1"/>
  </group>
 
  <text hint-style="title" hint-align="center">Hi,</text>
  <text hint-style="subtitleSubtle" hint-align="center">MasterHip</text>
</binding>
...
```

```CSharp
...
TileLarge = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        TextStacking = TileTextStacking.Center,
 
        Children =
        {
            new TileGroup()
            {
                Children =
                {
                    new TileSubgroup() { Weight = 1 },
 
                    new TileSubgroup()
                    {
                        Weight = 2,
                        Children =
                        {
                            new TileImage()
                            {
                                Source = new TileImageSource("Assets/Apps/Hipstame/hipster.jpg"),
                                Crop = TileImageCrop.Circle
                            }
                        }
                    },
 
                    new TileSubgroup() { Weight = 1 }
                }
            },
 
 
            new TileText()
            {
                Text = "Hi,",
                Style = TileTextStyle.Title,
                Align = TileTextAlign.Center
            },
 
            new TileText()
            {
                Text = "MasterHip",
                Style = TileTextStyle.SubtitleSubtle,
                Align = TileTextAlign.Center
            }
        }
    }
}
...
```

**Ergebnis:**

![Beispiel für das Zuschneiden eines Bilds](images/adaptive-tiles-imagecropping.png)

### <span id="Background_image"></span><span id="background_image"></span><span id="BACKGROUND_IMAGE"></span>Hintergrundbild

Um ein Hintergrundbild festzulegen, platzieren Sie ein image-Element im Stamm von &lt;binding&gt; und legen das placement-Attribut auf „background“ fest.

```XML
...
<binding template="TileWide">
  <image src="Assets\Mostly Cloudy-Background.jpg" placement="background"/>
  <group>
    <subgroup hint-weight="1">
      <text hint-align="center">Mon</text>
      <image src="Assets\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    ...
  </group>
</binding>
...
```

```CSharp
...
TileWide = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        BackgroundImage = new TileBackgroundImage()
        {
            Source = new TileImageSource("Assets/Mostly Cloudy-Background.jpg")
        },
 
        Children =
        {
            new TileGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°")
                    ...
                }
            }
        }
    }
}
...
 
 
private static TileSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new TileSubgroup()
    {
        Weight = 1,
 
        Children =
        {
            new TileText()
            {
                Text = day,
                Align = TileTextAlign.Center
            },
 
            new TileImage()
            {
                Source = new TileImageSource("Assets/Weather/" + image),
                RemoveMargin = true
            },
 
            new TileText()
            {
                Text = highTemp,
                Align = TileTextAlign.Center
            },
 
            new TileText()
            {
                Text = lowTemp,
                Align = TileTextAlign.Center,
                Style = TileTextStyle.CaptionSubtle
            }
        }
    };
}
```

**Ergebnis:**

![Beispiel für ein Hintergrundbild](images/adaptive-tiles-backgroundimage.png)

Zusätzlich können Sie mit **hint-overlay** eine schwarze Überlagerung für das Hintergrundbild festlegen. Das Attribut akzeptiert ganze Zahlen von 0 bis 100, wobei 0 keine Überlagerung und 100 eine vollständige schwarze Überlagerung angibt. Der Standardwert ist 20.

```XML
...
<binding template="TileWide" hint-overlay="60">
  <image src="Assets\Mostly Cloudy-Background.jpg" placement="background"/>
  ...
</binding>
...
```

```CSharp
...
 
TileWide = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        BackgroundImage = new TileBackgroundImage()
        {
            Source = new TileImageSource("Assets/Mostly Cloudy-Background.jpg"),
            Overlay = 60
        },
 
        ...
    }
}
 
...
```

**Ergebnis von „hint-overlay“:**

![Beispiel für ein Bild mit angewendetem „hint-overlay“](images/adaptive-tiles-image-hintoverlay.png)

### <span id="Peek_image"></span><span id="peek_image"></span><span id="PEEK_IMAGE"></span>Vorschaubild

Sie können ein Bild angeben, das von oben in die Kachel hineingleitet. Das Vorschaubild gleitet mithilfe einer Animation von oben in die Kachel hinein, ist kurz auf der Kachel zu sehen und gleitet danach wieder nach oben heraus, um den Blick auf den Hauptinhalt der Kachel freizugeben. Um ein Vorschaubild festzulegen, platzieren Sie ein image-Element im Stamm von &lt;binding&gt; und legen das placement-Attribut auf „peek“ fest.

```XML
...
<binding template="TileMedium" branding="name">
  <image placement="peek" src="Assets/Apps/Hipstame/hipster.jpg"/>
  <text>New Message</text>
  <text hint-style="captionsubtle" hint-wrap="true">Hey, have you tried Windows 10 yet?</text>
</binding>
...
```

```CSharp
...
 
TileWide = new TileBinding()
{
    Branding = TileBranding.Name,
 
    Content = new TileBindingContentAdaptive()
    {
        PeekImage = new TilePeekImage()
        {
            Source = new TileImageSource("Assets/Apps/Hipstame/hipster.jpg")
        },
 
        Children =
        {
            new TileText()
            {
                Text = "New Message"
            },
 
            new TileText()
            {
                Text = "Hey, have you tried Windows 10 yet?",
                Style = TileTextStyle.CaptionSubtle,
                Wrap = true
            }
        }
    }
}
 
...
```

![Beispiele für Vorschaubilder](images/adaptive-tiles-imagepeeking.png)

**Kreisförmiges Zuschneiden für Vorschau- und Hintergrundbilder**

Wenden Sie das folgende Attribut auf Vorschau- und Hintergrundbilder für einen kreisförmigen Zuschnitt an:

hint-crop="circle"

Das Ergebnis sieht wie folgt aus:

![Kreisförmiges Zuschneiden für Vorschau- und Hintergrundbilder](images/circlecrop-image.png)

**Verwenden eines Vorschau- und eines Hintergrundbilds**

Zur Verwendung eines Vorschau- und eines Hintergrundbilds auf einer Kachelbenachrichtigung geben Sie in der Benachrichtigungsnutzlast sowohl ein Vorschau- als auch ein Hintergrundbild an.

Das Ergebnis sieht wie folgt aus:

![Gleichzeitige Verwendung von Vorschau- und Hintergrundbild](images/peekandbackground.png)

**Verwendung von „hint-overlay“ auf einem Vorschaubild**

Sie können **hint-overlay** auf einem Vorschaubild für mehr Deckkraft und bessere Lesbarkeit des Anzeigenamens der Kachel verwenden. Wenn Sie **hint-overlay** für das &lt;binding&gt;-Element angeben, wird die Überlagerung sowohl auf das Hintergrundbild als auch das Vorschaubild angewendet.

Sie können **hint-overlay** auch auf ein &lt;image&gt;-Element mit dem placement-Wert „peek“ oder „background“ anwenden, um für jedes dieser Bilder eine unterschiedliche Deckkraftstufe zu verwenden. Wenn Sie keine Überlagerung angeben, ist der Standardwert 20 % für das Hintergrundbild und 0 % für das Vorschaubild.

Dieses Beispiel zeigt ein Hintergrundbild mit 20 % Deckkraft (links) und 0 % Deckkraft (rechts):

![„hint-overlay“ auf einem Vorschaubild](images/hintoverlay.png)

## <span id="Vertical_alignment__text_stacking_"></span><span id="vertical_alignment__text_stacking_"></span><span id="VERTICAL_ALIGNMENT__TEXT_STACKING_"></span>Vertikale Ausrichtung (hint-textStacking)


Sie können die vertikale Ausrichtung der Inhalte auf einer Kachel steuern, indem Sie das **hint-textStacking**-Attribut sowohl für das [&lt;binding&gt;](tiles-and-notifications-adaptive-tiles-schema.md)-Element als auch das [&lt;subgroup&gt;](tiles-and-notifications-adaptive-tiles-schema.md)-Element verwenden. Der gesamte Inhalt wird standardmäßig vertikal am oberen Rand ausgerichtet, kann aber auch in der Mitte oder am unteren Rand ausgerichtet werden.

### <span id="Text_stacking_on_binding_element"></span><span id="text_stacking_on_binding_element"></span><span id="TEXT_STACKING_ON_BINDING_ELEMENT"></span>Gestapelter Text für binding-Element

Bei Anwendung auf die [&lt;binding&gt;](tiles-and-notifications-adaptive-tiles-schema.md)-Ebene wird durch Textstapelung die vertikale Ausrichtung des Benachrichtigungsinhalts als Ganzes festgelegt und im verfügbaren vertikalen Bereich über dem Branding-/Signalbereich ausgerichtet.

```XML
...
<binding template="TileMedium" hint-textStacking="center" branding="logo">
  <text hint-style="base" hint-align="center">Hi,</text>
  <text hint-style="captionSubtle" hint-align="center">MasterHip</text>
</binding>
...
```

```CSharp
...
 
TileMedium = new TileBinding()
{
    Branding = TileBranding.Logo,
 
    Content = new TileBindingContentAdaptive()
    {
        TextStacking = TileTextStacking.Center,
 
        Children =
        {
            new TileText()
            {
                Text = "Hi,",
                Style = TileTextStyle.Base,
                Align = TileTextAlign.Center
            },
 
            new TileText()
            {
                Text = "MasterHip",
                Style = TileTextStyle.CaptionSubtle,
                Align = TileTextAlign.Center
            }
        }
    }
}
 
...
```

![Gestapelter Text für binding-Element](images/adaptive-tiles-textstack-bindingelement.png)

### <span id="Text_stacking_on_subgroup_element"></span><span id="text_stacking_on_subgroup_element"></span><span id="TEXT_STACKING_ON_SUBGROUP_ELEMENT"></span>Gestapelter Text für subgroup-Element

Bei Anwendung auf die [&lt;subgroup&gt;](tiles-and-notifications-adaptive-tiles-schema.md)-Ebene wird die vertikale Ausrichtung des Inhalts der Untergruppe (Spalte) durch gestapelten Text festgelegt und im verfügbaren vertikalen Bereich innerhalb der gesamten Gruppe ausgerichtet.

```XML
...
<binding template="TileWide" branding="nameAndLogo">
  <group>
    <subgroup hint-weight="33">
      <image src="Assets/Apps/Hipstame/hipster.jpg" hint-crop="circle"/>
    </subgroup>
    <subgroup hint-textStacking="center">
      <text hint-style="subtitle">Hi,</text>
      <text hint-style="bodySubtle">MasterHip</text>
    </subgroup>
  </group>
</binding>
...
```

```CSharp
...
 
TileWide = new TileBinding()
{
    Branding = TileBranding.NameAndLogo,
 
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new TileGroup()
            {
                Children =
                {
                    // Image column
                    new TileSubgroup()
                    {
                        Weight = 33,
                        Children =
                        {
                            new TileImage()
                            {
                                Source = new TileImageSource("Assets/Apps/Hipstame/hipster.jpg"),
                                Crop = TileImageCrop.Circle
                            }
                        }
                    },
 
                    // Text column
                    new TileSubgroup()
                    {
                        // Vertical align its contents
                        TextStacking = TileTextStacking.Center,
 
                        Children =
                        {
                            new TileText()
                            {
                                Text = "Hi,",
                                Style = TileTextStyle.Subtitle
                            },
 
                            new TileText()
                            {
                                Text = "MasterHip",
                                Style = TileTextStyle.BodySubtle
                            }
                        }
                    }
                }
            }
        }
    }
}
 
...
```

## <span id="related_topics"></span>Verwandte Themen


* [Adaptives Kachelschema](tiles-and-notifications-adaptive-tiles-schema.md)
* [NotificationsExtensions auf GitHub](https://github.com/WindowsNotifications/NotificationsExtensions/wiki)
* [Katalog für spezielle Kachelvorlagen](tiles-and-notifications-special-tile-templates-catalog.md)
 

 






<!--HONumber=May16_HO2-->


