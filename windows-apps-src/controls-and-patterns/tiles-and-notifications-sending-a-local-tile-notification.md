---
description: Hier wird beschrieben, wie Sie eine lokale Benachrichtigung an eine primäre Kachel und an eine sekundäre Kachel mit adaptiven Kachelvorlagen senden.
title: Senden einer lokalen Kachelbenachrichtigung
ms.assetid: D34B0514-AEC6-4C41-B318-F0985B51AF8A
label: TBD
template: detail.hbs
---

# Senden einer lokalen Kachelbenachrichtigung


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Primäre App-Kacheln in Windows 10 werden im App-Manifest definiert, sekundäre Kacheln werden dagegen programmgesteuert erstellt und vom App-Code definiert. In diesem Artikel wird beschrieben, wie Sie eine lokale Benachrichtigung an eine primäre Kachel und an eine sekundäre Kachel mit adaptiven Kachelvorlagen senden. (Eine lokale Benachrichtigung wird vom App-Code gesendet, im Gegensatz zu Benachrichtigungen, die ein Werbserver per Push oder Pull sendet.)

![Standardkachel und Kachel mit Benachrichtigung](images/sending-local-tile-01.png)

**Hinweis**  Hier erhalten Sie weitere Informationen über das [Erstellen von adaptiven Kacheln](tiles-and-notifications-create-adaptive-tiles.md) und das [Vorlageschema für adaptive Kacheln](tiles-and-notifications-adaptive-tiles-schema.md).

 

## <span id="Install_the_NuGet_package"></span><span id="install_the_nuget_package"></span><span id="INSTALL_THE_NUGET_PACKAGE"></span>Installieren des NuGet-Pakets


Wir empfehlen die Installation des [NuGet-Pakets „NotificationsExtensions”](https://www.nuget.org/packages/NotificationsExtensions.Win10/), das Kachelnutzlasten mit Objekten anstelle von unformatiertem XML generiert und dadurch vieles erleichtert.

Die Inlinecodebeispiele in diesem Artikel betreffen C# mit dem installierten NuGet-Paket [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki). (Wenn Sie eigenen XML-Code erstellen möchten, finden Sie Codebeispiele ohne [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) am Ende des Artikels.)

## <span id="Add_namespace_declarations"></span><span id="add_namespace_declarations"></span><span id="ADD_NAMESPACE_DECLARATIONS"></span>Hinzufügen von Namespacedeklarationen


Um auf die Kachel-APIs zuzugreifen, beziehen Sie den [**Windows.UI.Notifications**](https://msdn.microsoft.com/library/windows/apps/br208661)-Namespace ein. Sie sollten auch den **NotificationsExtensions.Tiles**-Namespace einbeziehen, damit Sie die Kachel-Hilfs-APIs nutzen können (Sie müssen das NuGet-Paket [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) installieren, um auf diese APIs zugreifen zu können).

```
using Windows.UI.Notifications;
using NotificationsExtensions.Tiles; // NotificationsExtensions.Win10
```

## <span id="Create_the_notification_content"></span><span id="create_the_notification_content"></span><span id="CREATE_THE_NOTIFICATION_CONTENT"></span>Erstellen des Benachrichtigungsinhalts


In Windows 10 werden Kachelnutzlasten mit adaptiven Kachelvorlagen definiert, mit denen Sie benutzerdefinierte visuelle Layouts für Ihre Benachrichtigungen erstellen können. (Informationen darüber, was mit adaptiven Kacheln möglich ist, finden Sie in den Artikeln [Erstellen adaptiver Kacheln](tiles-and-notifications-create-adaptive-tiles.md) und [Vorlagen für adaptive Kacheln](tiles-and-notifications-adaptive-tiles-schema.md).)

Dieses Codebeispiel erstellt adaptive Kachelinhalte für mittelgroße und breite Kacheln.

```
// In a real app, these would be initialized with actual data
string from = "Jennifer Parker";
string subject = "Photos from our trip";
string body = "Check out these awesome photos I took while in New Zealand!";
 
 
// Construct the tile content
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new TileText()
                    {
                        Text = from
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
        },
 
        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
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
    }
};
```

Auf einer mittelgroßen Kachel wird der Inhalt der Benachrichtigung wie folgt angezeigt:

![Inhalt der Benachrichtigung auf einer mittelgroßen Kachel](images/sending-local-tile-02.png)

## <span id="Create_the_notification"></span><span id="create_the_notification"></span><span id="CREATE_THE_NOTIFICATION"></span>Erstellen der Benachrichtigung


Wenn Sie den Inhalt der Benachrichtigung erstellt haben, müssen Sie eine neue [**TileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616)-Klasse erstellen. Der **TileNotification**-Konstruktor akzeptiert ein Windows-Runtime-[**XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br208620)-Objekt, das Sie aus der **TileContent.GetXml**-Methode ermitteln können, wenn Sie [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) verwenden.

Mit diesem Codebeispiel wird eine Benachrichtigung für eine neue Kachel erstellt.

```
// Create the tile notification
var notification = new TileNotification(content.GetXml());
```

## <span id="Set_an_expiration_time_for_the_notification__optional_"></span><span id="set_an_expiration_time_for_the_notification__optional_"></span><span id="SET_AN_EXPIRATION_TIME_FOR_THE_NOTIFICATION__OPTIONAL_"></span>Festlegen einer Ablaufzeit für die Benachrichtigung (optional)


Standardmäßig laufen lokale Kachel- und Signalbenachrichtigungen nicht ab, während Pushbenachrichtigungen sowie regelmäßige und geplante Benachrichtigungen nach drei Tagen ablaufen. Weil Kachelinhalt nur so lange wie notwendig beibehalten werden soll, sollten Sie, insbesondere für lokale Kachel- und Signalbenachrichtigungen, eine Gültigkeitsdauer festlegen, die für Ihre App sinnvoll ist.

In diesem Codebeispiel wird eine Benachrichtigung erstellt, die nach zehn Minuten abläuft und von der Kachel entfernt wird.

```
tileNotification.ExpirationTime = DateTimeOffset.UtcNow.AddMinutes(10);</code></pre></td>
</tr>
</tbody>
</table>
```

## <span id="Send_the_notification"></span><span id="send_the_notification"></span><span id="SEND_THE_NOTIFICATION"></span>Senden der Benachrichtigung


Das lokale Senden einer Kachelbenachrichtigung ist einfach, das Senden der Benachrichtigung an eine primäre oder sekundäre Kachel weicht aber etwas davon ab.

**Primäre Kachel**

Verwenden Sie zum Senden einer Benachrichtigung an eine primäre Kachel den [**TileUpdateManager**](https://msdn.microsoft.com/library/windows/apps/br208622), um für die primäre Kachel eine Kachelaktualisierung zu erstellen und die Benachrichtigung durch Aufrufen von „Update” zu senden. Die primäre Kachel Ihrer App ist immer vorhanden, selbst wenn sie nicht sichtbar ist. Daher können Sie Benachrichtigungen an die Kachel senden, auch wenn sie nicht angeheftet ist. Wenn der Benutzer später die primäre Kachel anheftet, werden die Benachrichtigungen, die Sie gesendet haben, angezeigt.

Mit diesem Codebeispiel wird eine Benachrichtigung an eine primäre Kachel gesendet.

<span codelanguage=""></span>
```
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
// And send the notification
TileUpdateManager.CreateTileUpdaterForApplication().Update(notification);
```

**Sekundäre Kachel**

Um eine Benachrichtigung an eine sekundäre Kachel zu senden, müssen Sie zuerst sicherstellen Sie, dass die sekundäre Kachel vorhanden ist. Wenn Sie versuchen, eine Kachelaktualisierung für eine sekundäre Kachel zu erstellen, die nicht vorhanden ist (z. B. wenn der Benutzer die sekundäre Kachel gelöst hat), wird eine Ausnahme ausgelöst. Sie können mit [**SecondaryTile.Exists**](https://msdn.microsoft.com/library/windows/apps/br242205)(tileId) ermitteln, ob die sekundäre Kachel angeheftet ist, und dann eine Kachelaktualisierung für eine sekundäre Kachel erstellen und die Benachrichtigung senden.

Mit diesem Codebeispiel wird eine Benachrichtigung an eine sekundäre Kachel gesendet.

```
// If the secondary tile is pinned
if (SecondaryTile.Exists("MySecondaryTile"))
{
    // Get its updater
    var updater = TileUpdateManager.CreateTileUpdaterForSecondaryTile("MySecondaryTile");
 
    // And send the notification
    updater.Update(notification);
}
```

![Standardkachel und Kachel mit Benachrichtigung](images/sending-local-tile-01.png)

## <span id="Clear_notifications_on_the_tile__optional_"></span><span id="clear_notifications_on_the_tile__optional_"></span><span id="CLEAR_NOTIFICATIONS_ON_THE_TILE__OPTIONAL_"></span>Löschen von Benachrichtigungen auf der Kachel (optional)


In den meisten Fällen sollten Sie eine Benachrichtigung löschen, sobald der Benutzer mit diesem Inhalt interagiert hat. Zum Beispiel sollten Sie alle Benachrichtigungen auf der Kachel löschen, wenn der Benutzer die App startet. Falls die Benachrichtigungen zeitabhängig sind, sollten Sie eine Ablaufzeit für die Benachrichtigung festlegen, anstatt sie explizit zu löschen.

Mit diesem Codebeispiel wird die Benachrichtigung auf der Kachel gelöscht.

```
TileUpdateManager.CreateTileUpdaterForApplication().Clear();</code></pre></td>
</tr>
</tbody>
</table>
```

Bei einer Kachel mit aktivierter Benachrichtigungswarteschlange und Benachrichtigungen in der Warteschlange wird durch Aufrufen der Clear-Methode die Warteschlange gelöscht. Es ist aber nicht möglich ist, eine Benachrichtigung über den Server Ihrer App zu löschen; Benachrichtigungen können nur durch lokalen App-Code gelöscht werden.

Durch regelmäßige oder Push-Benachrichtigungen können nur neue Benachrichtigungen hinzugefügt oder vorhandene Benachrichtigungen ersetzt werden. Mit einem lokalen Aufruf der Clear-Methode wird die Kachel gelöscht, unabhängig davon, ob die Benachrichtigungen selbst per Push, regelmäßig oder lokal gesendet wurden. Geplante Benachrichtigungen, die noch nicht angezeigt wurden, werden durch diese Methode nicht gelöscht.

![Kachel mit Benachrichtigung und Kachel nach dem Löschen](images/sending-local-tile-03.png)

## <span id="Next_steps"></span><span id="next_steps"></span><span id="NEXT_STEPS"></span>Nächste Schritte


**Verwenden der Benachrichtigungswarteschlange**

Nachdem Sie Ihre erste Kachelaktualisierung ausgeführt haben, können Sie die Funktionalität der Kachel erweitern, indem Sie eine [Benachrichtigungswarteschlange](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234) aktivieren.

**Andere Methoden für die Benachrichtigungsübermittlung**

In diesem Artikel wird erläutert, wie die Kachelaktualisierung als Benachrichtigung gesendet werden kann. Informationen zu anderen Methoden der Benachrichtigungsübermittlung, einschließlich von geplanten und regelmäßigen und Push-Benachrichtigungen, finden Sie unter [Zustellen von Benachrichtigungen](tiles-and-notifications-choosing-a-notification-delivery-method.md).

**XmlEncode-Übermittlungsmethode**

Wenn Sie [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) nicht verwenden, bietet diese Methode für die Benachrichtigungsübermittlung eine weitere Alternative.

<span codelanguage=""></span>
```
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
public string XmlEncode(string text)
{
    StringBuilder builder = new StringBuilder();
    using (var writer = XmlWriter.Create(builder))
    {
        writer.WriteString(text);
    }

    return builder.ToString();
}
```

## <span id="Code_examples_without_NotificationsExtensions"></span><span id="code_examples_without_notificationsextensions"></span><span id="CODE_EXAMPLES_WITHOUT_NOTIFICATIONSEXTENSIONS"></span>Codebeispiele ohne NotificationsExtensions


Wenn Sie lieber mit unformatiertem XML anstatt mit dem NuGet-Paket [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) arbeiten, verwenden Sie diese alternativen Codebeispiele für die ersten drei Beispiele in diesem Artikel. Die restlichen Codebeispiele können entweder mit [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) oder mit unformatiertem XML verwendet werden.

Hinzufügen von Namespacedeklarationen

```
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
```

Erstellen des Benachrichtigungsinhalts

```
// In a real app, these would be initialized with actual data
string from = "Jennifer Parker";
string subject = "Photos from our trip";
string body = "Check out these awesome photos I took while in New Zealand!";
 
 
// TODO - all values need to be XML escaped
 
 
// Construct the tile content as a string
string content = $@"
<tile>
    <visual>
 
        <binding template=&#39;TileMedium&#39;>
            <text>{from}</text>
            <text hint-style=&#39;captionSubtle&#39;>{subject}</text>
            <text hint-style=&#39;captionSubtle&#39;>{body}</text>
        </binding>
 
        <binding template=&#39;TileWide&#39;>
            <text hint-style=&#39;subtitle&#39;>{from}</text>
            <text hint-style=&#39;captionSubtle&#39;>{subject}</text>
            <text hint-style=&#39;captionSubtle&#39;>{body}</text>
        </binding>
 
    </visual>
</tile>";
```

Erstellen der Benachrichtigung

```
// Load the string into an XmlDocument
XmlDocument doc = new XmlDocument();
doc.LoadXml(content);
 
// Then create the tile notification
var notification = new TileNotification(doc);
```

## <span id="related_topics"></span>Verwandte Themen


* [Erstellen adaptiver Kacheln](tiles-and-notifications-create-adaptive-tiles.md)
* [Vorlagen für adaptive Kacheln: Schema und Dokumentation](tiles-and-notifications-adaptive-tiles-schema.md)
* [NotificationsExtensions.Win10 (NuGet-Paket)](https://www.nuget.org/packages/NotificationsExtensions.Win10/)
* [NotificationsExtensions auf GitHub](https://github.com/WindowsNotifications/NotificationsExtensions/wiki)
* [Vollständiges Codebeispiel auf GitHub](https://github.com/WindowsNotifications/quickstart-sending-local-tile-win10)
* [**Windows.UI.Notifications-Namespace**](https://msdn.microsoft.com/library/windows/apps/br208661)
* [So wird’s gemacht: Verwenden der Benachrichtigungswarteschlange (XAML)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234)
* [Zustellen von Benachrichtigungen](tiles-and-notifications-choosing-a-notification-delivery-method.md)
 

 






<!--HONumber=Mar16_HO1-->


