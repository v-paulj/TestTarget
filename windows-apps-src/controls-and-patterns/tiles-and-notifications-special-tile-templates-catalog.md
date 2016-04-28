---
description: Spezielle Kachelvorlagen sind individuelle Vorlagen, die animiert sind mit denen Sie Vorgänge durchführen können, die mit adaptiven Kacheln nicht möglich sind.
title: Spezielle Kachelvorlagen
ms.assetid: 1322C9BA-D5B2-45E2-B813-865884A467FF
label: TBD
template: detail.hbs
---

# Spezielle Kachelvorlagen


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Spezielle Kachelvorlagen sind individuelle Vorlagen, die animiert sind oder mit denen Sie Vorgänge durchführen können, die mit adaptiven Kacheln nicht möglich sind. Jede spezielle Kachelvorlage wurde speziell für Windows 10 erstellt, mit Ausnahme der Iconic-Kachelvorlage, einer klassischen Spezialvorlage, die für Windows 10 aktualisiert wurde. In diesem Artikel werden drei spezielle Kachelvorlagen behandelt: Iconic, Fotos und Kontakte.

## <span id="Iconic_tile_template"></span><span id="iconic_tile_template"></span><span id="ICONIC_TILE_TEMPLATE"></span>Iconic-Kachelvorlage


Mit der Iconic-Vorlage (auch als „IconWithBadge“-Vorlage bezeichnet) können Sie ein kleines Bild in der Mitte der Kachel anzeigen. Windows 10 unterstützt die Vorlage sowohl auf Telefonen als auch auf Tablets/Desktops.

![Kleine und mittelgroße E-Mail-Kacheln](images/iconic-template-mail-2sizes.png)

### <span id="How_to_create_an_iconic_tile"></span><span id="how_to_create_an_iconic_tile"></span><span id="HOW_TO_CREATE_AN_ICONIC_TILE"></span>Erstellen einer Iconic-Kachel

In den folgenden Schritten wird alles erläutert, was Sie zum Erstellen einer Iconic-Kachel für Windows 10 wissen müssen. Auf hoher Ebene benötigen Sie Ihre Iconic-Bildressource. Dann senden Sie mithilfe der Iconic-Vorlage eine Benachrichtigung an die Kachel und senden schließlich eine Signalbenachrichtigung, die die auf der Kachel anzuzeigende Zahl bereitstellt.

![Entwicklerablauf der Iconic-Kachel](images/iconic-template-dev-flow.png)

**Schritt 1: Erstellen Ihrer Bildressourcen im PNG-Format**

Erstellen Sie die Symbolressourcen für die Kachel, und platzieren Sie diese mit den anderen Ressourcen in Ihren Projektressourcen. Erstellen Sie mindestens ein Symbol mit 200 x 200 Pixeln, das für kleine und mittelgroße Kacheln auf dem Telefon und dem Desktop funktioniert. Um die bestmögliche Benutzerfreundlichkeit bereitzustellen, erstellen Sie für jede Größe ein eigenes Symbol. Informationen zu den Größen finden Sie im Bild unten.

Speichern Sie Symbolressourcen im PNG-Format und mit Transparenz. Unter Windows Phone wird jedes nicht transparente Pixel weiß (RGB-Wert 255, 255, 255) angezeigt. Verwenden Sie aus Gründen der Konsistenz und Einfachheit weiß auch für Desktopsymbole.

Windows 10 auf Tablet, Laptop und Desktop unterstützt nur quadratische Symbolressourcen. Windows Phone unterstützt quadratische Ressourcen und Ressourcen, die höher als breit sind, bis zu einem Breite-Höhe-Verhältnis von 2:3, was für Bilder, z. B. ein Telefonsymbol, nützlich ist.

![Symbolgrößen auf kleinen und mittelgroßen Kacheln auf Telefon und Desktop](images/iconic-template-sizing-info.png)

**Schritt 2: Erstellen der Basiskachel**

Sie können die Iconic-Vorlage für primäre und sekundäre Kacheln verwenden. Wenn Sie sie für eine sekundäre Kachel verwenden, müssen Sie zuerst die sekundäre Kachel erstellen oder eine bereits angeheftete sekundäre Kachel verwenden . Primäre Kacheln sind implizit angeheftet, und Sie können jederzeit Benachrichtigungen an sie senden.

**Schritt 3: Senden einer Benachrichtigung an die Kachel**

Obwohl dieser Schritt variieren kann, je nachdem, ob die Benachrichtigung lokal oder per Serverpush gesendet wird, bleibt die gesendete XML-Nutzlast identisch. Um eine lokale Kachelbenachrichtigung zu senden, erstellen Sie einen [**TileUpdater**](https://msdn.microsoft.com/library/windows/apps/br208628) für die Kachel (entweder primäre oder sekundäre Kachel), und senden Sie dann eine Benachrichtigung an die Kachel, die die Iconic-Kachelvorlage verwendet, wie unten dargestellt. Im Idealfall sollten Sie auch Bindungen für breite und große Kachelgrößen mit [adaptiven Kachelvorlagen](tiles-and-notifications-adaptive-tiles-schema.md) einschließen.

Hier sehen Sie einen Beispielcode für die XML-Nutzlast:

```XML
<tile>
  <visual>

    <binding template="TileSquare150x150IconWithBadge">
      <image id="1" src="Iconic.png" alt="alt text"/>
    </binding>
    
    <binding template="TileSquare71x71IconWithBadge">
      <image id="1" src="Iconic.png" alt="alt text"/>
    </binding>

  </visual>
</tile>
```

Die XML-Nutzlast dieser Iconic-Kachelvorlage verwendet ein Bildelement, das auf das Bild verweist, das Sie in Schritt 1 erstellt haben. Jetzt kann die Kachel das Signal neben Ihrem Symbol anzeigen, wenn Signalbenachrichtigungen gesendet werden.

**Schritt 4: Senden einer Signalbenachrichtigung an die Kachel**

Wie Schritt 3 kann dieser Schritt variieren, je nachdem, ob die Benachrichtigung lokal oder per Serverpush gesendet wird, aber die gesendete XML-Nutzlast bleibt identisch. Erstellen Sie zum Senden einer lokalen Signalbenachrichtigung einen [**BadgeUpdater**](https://msdn.microsoft.com/library/windows/apps/br208537) für die Kachel (entweder primäre oder sekundäre Kachel), und senden Sie dann eine Signalbenachrichtigung mit dem gewünschten Wert (oder löschen Sie das Signal).

Hier sehen Sie einen Beispielcode für die XML-Nutzlast:

```XML
<badge value="2"/></code></pre></td>
</tr>
</tbody>
</table>
```

Das Signal der Kachel wird entsprechend aktualisiert.

**Schritt 5: Zusammenfassung**

Die folgende Abbildung zeigt, wie die verschiedenen APIs und Nutzlasten den einzelnen Aspekten der Iconic-Kachelvorlage zugeordnet sind. Ein [Kachelbenachrichtigung](https://msdn.microsoft.com/library/windows/apps/hh779724) (die die &lt;binding&gt;-Elemente enthält) dient zum Angeben der Iconic-Vorlage und der Bildressource; eine [Signalbenachrichtigung](https://msdn.microsoft.com/library/windows/apps/hh779719) gibt den numerischen Wert an; Kacheleigenschaften steuern den Anzeigenamen, die Farbe und weitere Eigenschaften der Kachel.

![APIs und Nutzlasten, die der Iconic-Kachelvorlage zugeordnet sind](images/iconic-template-properties-info.png)

## <span id="Photos_tile_template"></span><span id="photos_tile_template"></span><span id="PHOTOS_TILE_TEMPLATE"></span>Kachelvorlage „Fotos“


Mit der Kachelvorlage „Fotos“ können Sie eine Diashow von Fotos auf der Live-Kachel anzeigen. Die Vorlage wird für alle Kachelgrößen, einschließlich klein, unterstützt und verhält sich für jede Kachelgröße gleich. Das folgende Beispiel zeigt fünf Frames einer mittelgroßen Kachel, die die Vorlage „Fotos“ verwendet. Die Vorlage verfügt über eine Zoom- und Überblendanimation, die ausgewählte Fotos in einer Endlosschleife durchläuft.

![Bilddiashow mit Kachelvorlage „Fotos“](images/photo-tile-template-image01.jpg)

### <span id="How_to_use_the_photos_template"></span><span id="how_to_use_the_photos_template"></span><span id="HOW_TO_USE_THE_PHOTOS_TEMPLATE"></span>Verwenden der Kachelvorlage „Fotos“

Das Verwenden der Kachelvorlage „Fotos“ ist einfach, wenn Sie die [Windows 10-Version von NotificationExtensions](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/08/20/introducing-notificationsextensions-for-windows-10.aspx) installiert haben. Obwohl Sie unformatierte XML-Daten verwenden können, empfehlen wir dringend die Verwendung von NotificationExtensions, damit Sie sich keine Gedanken zum Generieren gültiger XML- oder XML-Escapesequenzinhalte machen müssen.

Windows Phone zeigt bis zu 9 Fotos in einer Diashow an. Auf Tablet, Laptop und Desktop werden bis zu 12 Fotos angezeigt.

Informationen zum Senden der Kachelbenachrichtigung finden Sie im [Artikel zum Senden von Benachrichtigungen](tiles-badges-notifications.md).

<span codelanguage="XML"></span>
```XML
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<!--
 
To use the Photos template...
 
 - On any adaptive tile binding (like TileMedium or TileWide)
   - Set the hint-presentation attribute to "photos"
   - Add up to 12 images as children of the binding.
    
-->
 
<tile>
  <visual>
     
    <binding template="TileMedium" hint-presentation="photos">
       
      <image src="Assets/1.jpg" />
      <image src="ms-appdata:///local/Images/2.jpg"/>
      <image src="http://msn.com/images/3.jpg"/>
       
      <!--TODO: Can have 12 images total-->
       
    </binding>
     
    <!--TODO: Add bindings for other tile sizes-->
     
  </visual>
</tile>
```

```CSharp
/*
 
To use the Photos template...
 
 - On any TileBinding object
   - Set Content property to new instance of TileBindingContentPhotos
   - Add up to 12 images to Images property of TileBindingContentPhotos.
 
*/
 
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentPhotos()
            {
                Images =
                {
                    new TileImageSource("Assets/1.jpg"),
                    new TileImageSource("ms-appdata:///local/Images/2.jpg"),
                    new TileImageSource("http://msn.com/images/3.jpg")
 
                    // TODO: Can have 12 images total
                }
            }
        }
 
        // TODO: Add other tile sizes
    }
};
```

## <span id="People_tile_template"></span><span id="people_tile_template"></span><span id="PEOPLE_TILE_TEMPLATE"></span>Kachelvorlage „Kontakte“


Die Kontakte-App in Windows 10 verwendet eine spezielle Kachelvorlage, die eine Sammlung von Bildern in Kreisen anzeigt, die sich auf der Kachel vertikal oder horizontal verschieben. Diese Kachelvorlage ist seit Windows 10 Build 10572 verfügbar und kann jederzeit in Apps verwendet werden.

Die Kachelvorlage „Kontakte“ funktioniert auf Kacheln folgender Größen:

**Mittelgroße Kachel** (TileMedium)

![Mittelgroße Kachel „Kontakte“](images/people-tile-medium.png)

 

**Breite Kachel** (TileWide)

![Breite Kachel „Kontakte“](images/people-tile-wide.png)

 

**Große Kachel (nur Desktop)** (TileLarge)

![Große Kachel „Kontakte“](images/people-tile-large.png)

 

Bei Verwendung von [NotificationExtensions](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/08/20/introducing-notificationsextensions-for-windows-10.aspx) müssen Sie zum Verwenden der Kachelvorlage „Kontakte“ nur ein neues *TileBindingContentPeople*-Objekt für Ihren *TileBinding*-Inhalt erstellen. Die *TileBindingContentPeople*-Klasse verfügt über eine Bildereigenschaft, mit der Sie Ihre Bilder hinzufügen.

Wenn Sie unformatiertes XML verwenden, legen Sie *hint-presentation* auf „Kontakte“ fest, und fügen Sie die Bilder als untergeordnete Elemente des Bindungselements hinzu.

Das folgende C#-Codebeispiel nimmt an, dass Sie [NotificationExtensions](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/08/20/introducing-notificationsextensions-for-windows-10.aspx) verwenden.

```CSharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentPeople()
            {
                Images =
                {
                    new TileImageSource("Assets/ProfilePics/1.jpg"),
                    new TileImageSource("Assets/ProfilePics/2.jpg"),
                    new TileImageSource("Assets/ProfilePics/3.jpg"),
                    new TileImageSource("Assets/ProfilePics/4.jpg"),
                    new TileImageSource("Assets/ProfilePics/5.jpg"),
                    new TileImageSource("Assets/ProfilePics/6.jpg"),
                    new TileImageSource("Assets/ProfilePics/7.jpg"),
                    new TileImageSource("Assets/ProfilePics/8.jpg"),
                    new TileImageSource("Assets/ProfilePics/9.jpg")
                }
            }
        }
    }
};
```

```XML
<tile>
  <visual>
 
    <binding template=&#39;TileMedium&#39; hint-presentation=&#39;people&#39;>
      <image src=&#39;Assets/ProfilePics/1.jpg&#39;/>
      <image src=&#39;Assets/ProfilePics/2.jpg&#39;/>
      <image src=&#39;Assets/ProfilePics/3.jpg&#39;/>
      <image src=&#39;Assets/ProfilePics/4.jpg&#39;/>
      <image src=&#39;Assets/ProfilePics/5.jpg&#39;/>
      <image src=&#39;Assets/ProfilePics/6.jpg&#39;/>
      <image src=&#39;Assets/ProfilePics/7.jpg&#39;/>
      <image src=&#39;Assets/ProfilePics/8.jpg&#39;/>
      <image src=&#39;Assets/ProfilePics/9.jpg&#39;/>
    </binding>
 
  </visual>
</tile>
```

Für die optimale Benutzerfreundlichkeit sollten Sie die folgende Anzahl Fotos für jede Kachelgröße bereitstellen:

-   Mittelgroße Kachel: 9 Fotos
-   Breite Kachel: 15 Fotos
-   Große Kachel: 20 Fotos

Durch die Anzahl der Fotos werden einige leere Kreise ermöglicht, was bedeutet, dass die Kachel visuell nicht zu ausgelastet ist. Sie können die Anzahl der Fotos anpassen, um das gewünschte Erscheinungsbild zu erhalten.

Informationen zum Senden der Benachrichtigung finden Sie unter [Auswählen einer Methode für die Übermittlung von Benachrichtigungen](tiles-and-notifications-choosing-a-notification-delivery-method.md).

## <span id="related_topics"></span>Verwandte Themen


* [Vollständiges Codebeispiel auf GitHub](https://github.com/WindowsNotifications/quickstart-people-tile-template)
* [NotificationsExtensions auf GitHub](https://github.com/WindowsNotifications/NotificationsExtensions/wiki)
* [Kacheln, Signale und Benachrichtigungen](tiles-badges-notifications.md)
* [Erstellen adaptiver Kacheln](tiles-and-notifications-create-adaptive-tiles.md)
* [Vorlagen für adaptive Kacheln: Schema und Dokumentation](tiles-and-notifications-adaptive-tiles-schema.md)
 

 






<!--HONumber=Mar16_HO1-->


