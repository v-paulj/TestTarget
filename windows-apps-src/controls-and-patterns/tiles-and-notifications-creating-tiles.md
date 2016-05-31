---
author: mijacobs
Description: Eine Kachel ist die Darstellung einer App im Startmenü. Jede App verfügt über eine Kachel. Wenn Sie ein neues App-Projekt für die Universelle Windows-Plattform (UWP) in Microsoft Visual Studio erstellen, enthält es eine Standardkachel, die den Namen und das Logo der App anzeigt.
title: Kacheln
ms.assetid: 09C7E1B1-F78D-4659-8086-2E428E797653
label: Tiles
template: detail.hbs
---

# Kacheln für UWP-Apps





Eine *Kachel* ist die Darstellung einer App im Startmenü. Jede App verfügt über eine Kachel. Wenn Sie ein neues UWP-App-Projekt in Microsoft Visual Studio erstellen, enthält es eine Standardkachel, die den Namen und das Logo Ihrer App anzeigt. Windows zeigt diese Kachel bei der erstmaligen Installation Ihrer App an. Nachdem Ihre App installiert wurde, können Sie den Inhalt der Kachel mithilfe von Benachrichtigungen ändern. Sie können die Kachel zum Beispiel so ändern, dass dem Benutzer neue Informationen angezeigt werden, wie etwa neue Schlagzeilen oder der Betreff der letzten ungelesenen Nachricht.

## <span id="Configure_the_default_tile"></span><span id="configure_the_default_tile"></span><span id="CONFIGURE_THE_DEFAULT_TILE"></span>Konfigurieren der Standardkachel


Wenn Sie ein neues Projekt in Visual Studio erstellen, wird eine einfache Standardkachel erstellt, die den Namen und das Logo Ihrer App anzeigt.

```XML
  <Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="ExampleApp.App">
      <uap:VisualElements
        DisplayName="ExampleApp"
        Square150x150Logo="Assets\Logo.png"
        Square44x44Logo="Assets\SmallLogo.png"
        Description="ExampleApp"
        BackgroundColor="#464646">
        <uap:SplashScreen Image="Assets\SplashScreen.png" />
      </uap:VisualElements>
    </Application>
  </Applications>
```

Aktualisieren Sie die folgenden Elemente:

-   DisplayName: Ersetzen Sie diesen Wert mit dem Namen, der auf der Kachel angezeigt werden soll.
-   ShortName: Da der Platz für Ihren Anzeigenamen auf Kacheln begrenzt ist, empfehlen wir, auch einen ShortName (Kurznamen) anzugeben, um sicherzustellen, dass der Name Ihrer App nicht abgeschnitten wird.
-   Logobilder:

    Ersetzen Sie diese Bilder durch eigene. Sie können Bilder für verschiedene visuelle Skalierungen bereitstellen, Sie müssen aber nicht alle bereitstellen. Um sicherzustellen, dass die App auf einer Vielzahl von Geräten gut aussieht, wird empfohlen, skalierte Versionen der Bilder mit jeweils 100 %, 200 % und 400 % bereitzustellen.

    Skalierte Bilder haben die folgende Benennungskonvention:
    
    *
              &lt;Bildname&gt;*.scale-*&lt;Skalierungsfaktor&gt;*.*&lt;Bilddateierweiterung&gt;* 


     

    Beispiel: SmallLogo.scale-100.png

    Wenn Sie auf das Bild verweisen, verweisen Sie auf *&lt;Bildname&gt;*.*&lt;Bilddateierweiterung&gt;* („SmallLogo.png“ in diesem Beispiel). Das System wählt aus den bereitgestellten Bildern automatisch das entsprechend skalierte Bild für das Gerät aus.

-   Es wird ausdrücklich empfohlen, Logos für breite und große Kacheln bereitzustellen, damit der Benutzer die Größe der Kachel Ihrer App entsprechend anpassen kann. Erstellen Sie zum Bereitstellen dieser zusätzlichen Bilder ein `DefaultTile`-Element, und verwenden Sie die `Wide310x150Logo`- und `Square310x310Logo`-Attribute, um zusätzliche Bilder anzugeben:
```    XML
  <Applications>
        <Application Id="App"
          Executable="$targetnametoken$.exe"
          EntryPoint="ExampleApp.App">
          <uap:VisualElements
            DisplayName="ExampleApp"
            Square150x150Logo="Assets\Logo.png"
            Square44x44Logo="Assets\SmallLogo.png"
            Description="ExampleApp"
            BackgroundColor="#464646">
            <uap:DefaultTile
              Wide310x150Logo="Assets\WideLogo.png"
              Square310x310Logo="Assets\LargeLogo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
      </Applications>
```

## <span id="Use_notifications_to_customize_your_tile"></span><span id="use_notifications_to_customize_your_tile"></span><span id="USE_NOTIFICATIONS_TO_CUSTOMIZE_YOUR_TILE"></span>Verwenden von Benachrichtigungen zum Anpassen der Kachel


Nachdem Ihre App installiert wurde, können Sie die Kachel mit Benachrichtigungen anpassen. Dies kann entweder beim ersten Start der App oder als Reaktion auf ein bestimmtes Ereignis wie eine Pushbenachrichtigung geschehen.

1.  Erstellen Sie eine XML-Nutzlast (in Form von [**Windows.Data.Xml.Dom.XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br206173)), die die Kachel beschreibt.

    -   In Windows 10 wird ein neues adaptives Kachelschema eingeführt, das Sie verwenden können. Eine Anleitung hierzu finden Sie unter [Adaptive Kacheln](tiles-and-notifications-create-adaptive-tiles.md). Informationen zum Schema finden Sie unter [Adaptives Kachelschema](tiles-and-notifications-adaptive-tiles-schema.md). 

    -   Mit den Windows 8.1-Kachelvorlagen können Sie Ihre Kachel definieren. Weitere Informationen finden Sie unter [Erstellen von Kacheln und Signalen (Windows 8.1)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868260).

2.  Erstellen Sie ein Kachelbenachrichtigungsobjekt, und übergeben Sie es an das erstellte [**XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br206173) Es gibt verschiedene Arten von Benachrichtigungsobjekten:
    -   Ein [**Windows.UI.NotificationsTileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616)-Objekt zum sofortigen Aktualisieren der Kachel
    -   Ein [**Windows.UI.Notifications.ScheduledTileNotification**](https://msdn.microsoft.com/library/windows/apps/hh701637)-Objekt zum Aktualisieren der Kachel zu einem bestimmten Zeitpunkt

3.  Verwenden Sie [**Windows.UI.Notifications.TileUpdateManager.CreateTileUpdaterForApplication**](https://msdn.microsoft.com/library/windows/apps/br208623), um ein [**TileUpdater**](https://msdn.microsoft.com/library/windows/apps/br208628)-Objekt zu erstellen.
4.  Rufen Sie die [**TileUpdater.Update**](https://msdn.microsoft.com/library/windows/apps/br208632)-Methode auf, und übergeben Sie sie an das in Schritt 2 erstellte Kachelbenachrichtigungsobjekt.

 

 






<!--HONumber=May16_HO2-->


