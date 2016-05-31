---
author: Jwmsft
Description: Media Player wird zum Anzeigen und Wiedergeben von Videos, Audiodateien und Bildern verwendet.
title: Media Player
ms.assetid: 9AABB5DE-1D81-4791-AB47-7F058F64C491
dev.assetid: AF2F2008-9B53-430C-BBC3-8888F631B0B0
label: Media player
template: detail.hbs
---
# Media Player

Media Player wird zum Anzeigen und Wiedergeben von Videos, Audiodateien und Bildern verwendet. Die Medienwiedergabe kann inline (eingebettet auf einer Seite oder mit einer Gruppe von anderen Steuerelementen) oder in einer dedizierten Vollbild-Ansicht erfolgen. Sie können die Schaltflächen des Players ändern, den Hintergrund der Steuerelementleiste ändern und Layouts wie gewünscht anordnen. Beachten Sie jedoch, dass die Benutzer eine Reihe grundlegender Steuerelemente (Wiedergabe/Pause, Zurückspringen, Vorwärts springen) erwarten.

![Medienelement mit Transportsteuerelementen](images/controls/media-transport-controls.png)

<span class="sidebar_heading" style="font-weight: bold;">Wichtige APIs</span>

-   [**MediaElement-Klasse**](https://msdn.microsoft.com/library/windows/apps/br242926)
-   [**MediaTransportControls-Klasse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediatransportcontrols)

## Ist dies das richtige Steuerelement?

Verwenden Sie einen Mediaplayer, wenn Sie Audio- oder Videodateien in Ihrer App wiedergeben möchten. Verwenden Sie zum Anzeigen einer Sammlung von Bildern die [Flip-Ansicht](flipview.md).

## Beispiele

Ein Medienelement in der Windows 10-App für erste Schritte.

![Ein Medienelement in der Windows 10-App für erste Schritte.](images/control-examples/media-element-getstarted.png)

## Erstellen eines Media Players
Erstellen Sie zum Hinzufügen von Medien zu Ihrer App ein [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)-Objekt in XAML, und legen Sie für [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) einen Uniform Resource Identifier (URI) fest, der auf eine Audio- oder Videodatei verweist.

Mit diesem XAML-Code wird ein [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) erstellt, dessen [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419)-Eigenschaft auf den URI einer Videodatei festgelegt wird, bei der es sich um eine lokale Datei der App handelt. Die Wiedergabe des **MediaElement**-Objekts beginnt, wenn die Seite geladen wird. Um zu verhindern, dass die Medienwiedergabe sofort beginnt, können Sie die [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/br227360)-Eigenschaft auf **false** festlegen.

```xaml
<MediaElement x:Name="mediaSimple" 
              Source="Videos/video1.mp4" 
              Width="400" AutoPlay="False"/>
```

Mit diesem XAML-Code wird ein [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)-Objekt mit aktivierten integrierten Transportsteuerelementen und mit auf **false** festgelegter [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/br227360)-Eigenschaft erstellt.


```csharp
<MediaElement x:Name="mediaPlayer" 
              Source="Videos/video1.mp4" 
              Width="400" 
              AutoPlay="False"
              AreTransportControlsEnabled="True"/>
```

### Steuerelemente für den Medientransport
MediaElement verfügt über integrierte Transportsteuerelemente für Wiedergabe, Beenden, Anhalten, Lautstärke, Stummschaltung, Suche/Status und Audiotitelauswahl. Um diese Steuerelemente zu aktivieren, legen Sie [**AreTransportControlsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn298977) auf **true** fest. Um sie zu deaktivieren, legen Sie **AreTransportControlsEnabled** auf **false** fest. Die Transportsteuerelemente werden durch die [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn831962)-Klasse dargestellt. Sie können die Transportsteuerelemente unverändert verwenden oder auf verschiedene Weise anpassen. Weitere Informationen finden Sie in der Referenz zur [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn831962)-Klasse und unter [Erstellen von benutzerdefinierten Transportsteuerelementen](custom-transport-controls.md).

Mit den Transportsteuerelementen können Benutzer die meisten Aspekte des [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) steuern, aber das **MediaElement** bietet auch zahlreiche Eigenschaften und Methoden, die Sie zum Steuern der Audio- und Videowiedergabe verwenden können. Weitere Informationen finden Sie im Abschnitt [Programmgesteuertes Steuern von MediaElement](#control_mediaelement_programmatically) weiter unten in diesem Artikel.

Die Transportsteuerelemente unterstützen Layouts mit einer Zeile und mit Doppelzeile. Im ersten Beispiel sehen Sie ein einzeiliges-Layout mit der Wiedergabe/Pause-Schaltfläche links von der Medienzeitachse. Dieses Layout wird am besten für kompakte Bildschirme reserviert. 

![Beispiel für MTC-Steuerelemente in einer einzelnen Zeile](images/controls_mtc_singlerow_phone.png)

Das Layout mit doppelzeiligen Steuerelementen (siehe unten) wird für die meisten Nutzungsszenarien empfohlen, insbesondere für größere Bildschirme. Dieses Layout bietet mehr Platz für Steuerelemente und erleichtert dem Benutzer die Bedienung der Zeitachse.

![Beispiel für MTC-Steuerelemente in einer Doppelzeile](images/controls_mtc_doublerow_phone.png)

**Steuerelemente für den Systemmedientransport**

Sie können das [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) auch in die Steuerelemente für den Systemmedientransport integrieren. Die Systemtransportsteuerelemente sind die Steuerelemente, die angezeigt werden, wenn Hardwaretasten für Medien betätigt werden, z. B. die Medientasten auf Tastaturen). Wenn der Benutzer die PAUSE-Taste auf einer Tastatur drückt und Ihre App die [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) unterstützt, wird die App benachrichtigt, und Sie können die entsprechende Aktion durchführen. Weitere Informationen finden Sie unter [Steuerelemente für den Systemmedientransport](https://msdn.microsoft.com/library/windows/apps/mt228338).

### Festlegen der Medienquelle
Legen Sie die [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419)-Eigenschaft auf den Pfad der gewünschten Datei fest, um Dateien aus dem Netzwerk oder in die App eingebettete Dateien wiederzugeben.

**Tipp**  Zum Öffnen von Dateien aus dem Internet müssen Sie die **Internet (Client)**-Funktion im App-Manifest (Package.appxmanifest) deklarieren. Weitere Informationen zum Deklarieren von Funktionen finden Sie unter [Deklaration der App-Funktionen](https://msdn.microsoft.com/library/windows/apps/mt270968).

 

Dieser Code versucht, die [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419)-Eigenschaft des im XAML-Code definierten [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)-Objekts auf den Pfad einer Datei festzulegen, der in ein [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)-Element eingegeben wurde.

```xaml
<TextBox x:Name="txtFilePath" Width="400" 
         FontSize="20"
         KeyUp="TxtFilePath_KeyUp"
         Header="File path"
         PlaceholderText="Enter file path"/>
```

```csharp
private void TxtFilePath_KeyUp(object sender, KeyRoutedEventArgs e)
{
    if (e.Key == Windows.System.VirtualKey.Enter)
    {
        TextBox tbPath = sender as TextBox;

        if (tbPath != null)
        {
            LoadMediaFromString(tbPath.Text);
        }
    }
}

private void LoadMediaFromString(string path)
{
    try
    {
        Uri pathUri = new Uri(path);
        mediaPlayer.Source = pathUri;
    }
    catch (Exception ex)
    {
        if (ex is FormatException)
        {
            // handle exception. 
            // For example: Log error or notify user problem with file
        }
    }
}
```

Um die Medienquelle auf eine in die App eingebettete Mediendatei festzulegen, erstellen Sie einen [**Uri**](https://msdn.microsoft.com/library/windows/apps/br226017) mit dem Pfadpräfix **ms-appx:///**, und legen Sie anschließend die [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419)-Eigenschaft auf den URI fest. Für eine Datei mit dem Namen **video1.mp4** , die sich in dem Unterordner **Videos** befindet, würde der Pfad beispielsweise wie folgt aussehen: **ms-appx:///Videos/video1.mp4**

Mit diesem Code wird die [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419)-Eigenschaft des [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)-Objekts, das zuvor in XAML definiert wurde, auf **ms-appx:///Videos/video1.mp4** festgelegt.

```csharp
private void LoadEmbeddedAppFile()
{
    try
    {
        Uri pathUri = new Uri("ms-appx:///Videos/video1.mp4");
        mediaPlayer.Source = pathUri;
    }
    catch (Exception ex)
    {
        if (ex is FormatException)
        {
            // handle exception. 
            // For example: Log error or notify user problem with file
        }
    }
}
```

### Öffnen von lokalen Mediendateien
Zum Öffnen von Dateien im lokalen System oder aus OneDrive können Sie das [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)-Element verwenden, um die Datei abzurufen, und [**SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338), um die Medienquelle festzulegen; alternativ können Sie programmgesteuert auf die Benutzermedienordner zugreifen.

Falls für Ihre App jedoch der Zugriff auf die **Musik**- oder **Videoordner** ohne Benutzerinteraktion erforderlich ist (wenn Sie beispielsweise sämtliche Musik- oder Videodateien einer Sammlung des Benutzers aufzählen und in Ihrer App anzeigen), müssen Sie die Funktionen der **Musik-** und **Videobibliothek** deklarieren. Weitere Informationen finden Sie unter [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](https://msdn.microsoft.com/library/windows/apps/mt188703).

Das [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)-Element benötigt keine besonderen Funktionen für den Zugriff auf Dateien im lokalen Dateisystem (etwa die Ordner **Musik** oder **Video** des Benutzers), da Benutzer die vollständige Kontrolle darüber haben, auf welche Datei zugegriffen wird. Aus Sicherheits- und Datenschutzgründen ist es am sinnvollsten, die Anzahl der von der App verwendeten Funktionen möglichst gering zu halten.

**So öffnen Sie lokale Medien mit FileOpenPicker**

1.  Rufen Sie [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) auf, um dem Benutzer die Auswahl einer Mediendatei zu ermöglichen.

    Verwenden Sie die [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)-Klasse, um eine Mediendatei auszuwählen. Legen Sie den [**FileTypeFilter**](https://msdn.microsoft.com/library/windows/apps/br207850) fest, um anzugeben, welche Dateitypen der **FileOpenPicker** anzeigt. Rufen Sie [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) auf, um die Dateiauswahl zu starten und die Datei abzurufen.

2.  Rufen Sie [**SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338) auf, um die ausgewählte Mediendatei als [**MediaElement.Source**](https://msdn.microsoft.com/library/windows/apps/br227419) festzulegen.

    Sie müssen einen Datenstrom öffnen, um die [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) von [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) auf die [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) festzulegen, die von [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) zurückgegeben wurde. Rufen Sie die [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851)-Methode für die **StorageFile** auf; hierdurch wird ein Datenstrom zurückgegeben, den Sie an die [**MediaElement.SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338)-Methode übergeben können. Rufen Sie dann [**Play**](https://msdn.microsoft.com/library/windows/apps/br227402) für das **MediaElement** auf, um die Medien zu starten.

In diesem Beispiel wird veranschaulicht, wie Sie die [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)-Klasse verwenden, um eine Datei auszuwählen, und die Datei als [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) eines [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) festlegen.

```xaml
<MediaElement x:Name="mediaPlayer"/>
...
<Button Content="Choose file" Click="Button_Click"/>
```

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    await SetLocalMedia();
}

async private System.Threading.Tasks.Task SetLocalMedia()
{
    var openPicker = new Windows.Storage.Pickers.FileOpenPicker();

    openPicker.FileTypeFilter.Add(".wmv");
    openPicker.FileTypeFilter.Add(".mp4");
    openPicker.FileTypeFilter.Add(".wma");
    openPicker.FileTypeFilter.Add(".mp3");

    var file = await openPicker.PickSingleFileAsync();
    
    // mediaPlayer is a MediaElement defined in XAML
    if (file != null)
    {
        var stream = await file.OpenAsync(Windows.Storage.FileAccessMode.Read);
        mediaPlayer.SetSource(stream, file.ContentType);

        mediaPlayer.Play();
    }
}
```

### Festlegen der Posterquelle
Mit der [**PosterSource**](https://msdn.microsoft.com/library/windows/apps/br227409)-Eigenschaft können Sie eine visuelle Darstellung für Ihr [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) bereitstellen, bevor die Medien geladen werden. Eine **PosterSource** ist ein Bild, z. B. ein Screenshot oder Filmplakat, das anstelle der Medien angezeigt wird. Die **PosterSource** wird in folgenden Fällen angezeigt:

-   Wenn keine gültige Quelle festgelegt ist. Beispiel: [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) ist nicht festgelegt, **Source** wurde auf **Null** festgelegt, oder die Quelle ist ungültig (z. B. wenn ein [**MediaFailed**](https://msdn.microsoft.com/library/windows/apps/br227393)-Ereignis eintritt).
-   Während Medien geladen werden. Beispiel: Eine gültige Quelle ist festgelegt, aber das [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/br227394)-Ereignis ist noch nicht eingetreten.
-   Beim Streamen von Medien auf ein anderes Gerät.
-   Wenn die Medien nur Audio enthalten.

Hier sehen Sie ein [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926), dessen [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) auf einen Albumtitel und dessen [**PosterSource**](https://msdn.microsoft.com/library/windows/apps/br227409) auf ein Bild des Albumcovers festgelegt ist.

```xaml
<MediaElement Source="Media/Track1.mp4" PosterSource="Media/AlbumCover.png"/> 
```

### Abblenden/Ausschalten des Gerätebildschirms verhindern
Normalerweise wird bei einem Gerät das Display abgeblendet (und schließlich ausgeschaltet), um bei Abwesenheit des Benutzers den Akku zu schonen. Bei Video-Apps muss der Bildschirm jedoch eingeschaltet bleiben, damit der Benutzer das Video sehen kann. Um zu verhindern, dass das Display deaktiviert wird, wenn keine Benutzeraktion mehr festgestellt werden kann, beispielsweise beim Wiedergeben eines Videos in einer App im Vollbild, können Sie [**DisplayRequest.RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818) aufrufen. Mithilfe der [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816)-Klasse können Sie Windows anweisen, das Display eingeschaltet zu lassen, damit der Benutzer das Video sehen kann.

Um Energie zu sparen und den Akku zu schonen, wird empfohlen, [**DisplayRequest.RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819) aufzurufen und die Displayanforderung freizugeben, wenn diese nicht mehr benötigt wird. Windows deaktiviert automatisch die aktiven Displayanforderungen der App, wenn die App vom Bildschirm entfernt wird, und aktiviert die Displayanforderungen wieder, wenn Ihre App wieder in den Vordergrund gesetzt wird.

Im Anschluss sind einige Situationen aufgeführt, in denen Sie die Displayanforderung freigeben sollten:

-   Die Videowiedergabe wird angehalten (beispielsweise durch eine Benutzeraktion, zum Puffern oder für eine Anpassung aufgrund von begrenzter Bandbreite).
-   Die Wiedergabe wird gestoppt. Beispielsweise ist die Wiedergabe des Videos beendet oder die Darstellung vorüber.
-   Ein Wiedergabefehler ist aufgetreten. Es können beispielsweise Probleme mit der Netzwerkverbindung bestehen, oder eine Datei kann beschädigt sein.

**So verhindern Sie das Abblenden/Ausschalten des Bildschirms**

1.  Erstellen Sie eine globale [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816)-Variable. Initialisieren Sie sie mit dem Wert NULL.
```csharp
// Create this variable at a global scope. Set it to null.
private DisplayRequest appDisplayRequest = null;
```

2.  Rufen Sie [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818) auf, um Windows mitzuteilen, dass für die App das Display eingeschaltet bleiben muss.

3.  Rufen Sie [**RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819) auf, um die Displayanforderung freizugeben, wenn die Videowiedergabe beendet, angehalten oder durch einen Wiedergabefehler unterbrochen wird. Falls für die App keine aktiven Displayanforderungen mehr vorhanden sind, schont Windows den Akku, indem das Display abgeblendet (und schließlich ausgeschaltet) wird, wenn das Gerät nicht verwendet wird.

    Hier verwenden Sie das [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375)-Ereignis, um diese Situationen zu erkennen. Verwenden Sie dann die [**IsAudioOnly**](https://msdn.microsoft.com/library/windows/apps/hh965334)-Eigenschaft, um festzustellen, ob eine Audio- oder Videodatei wiedergegeben wird, und lassen Sie den Bildschirm nur eingeschaltet, wenn eine Videodatei wiedergegeben wird.
    ```xaml
<MediaElement Source="Media/video1.mp4"
              CurrentStateChanged="MediaElement_CurrentStateChanged"/>
    ```
 
    ```csharp
private void MediaElement_CurrentStateChanged(object sender, RoutedEventArgs e)
{
    MediaElement mediaElement = sender as MediaElement;
    if (mediaElement != null && mediaElement.IsAudioOnly == false)
    {
        if (mediaElement.CurrentState == Windows.UI.Xaml.Media.MediaElementState.Playing)
        {                
            if (appDisplayRequest == null)
            {
                // This call creates an instance of the DisplayRequest object. 
                appDisplayRequest = new DisplayRequest();
                appDisplayRequest.RequestActive();
            }
        }
        else // CurrentState is Buffering, Closed, Opening, Paused, or Stopped. 
        {
            if (appDisplayRequest != null)
            {
                // Deactivate the display request and set the var to null.
                appDisplayRequest.RequestRelease();
                appDisplayRequest = null;
            }
        }            
    }
} 
    ```

### Programmgesteuertes Steuern der Medienwiedergabe
[
              **MediaElement**
            ](https://msdn.microsoft.com/library/windows/apps/br242926) stellt zahlreiche Eigenschaften, Methoden und Ereignisse zum Steuern der Audio- und Videowiedergabe bereit. Eine vollständige Liste der Eigenschaften, Methoden und Ereignisse finden Sie auf der Referenzseite zu [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926).
    

### Auswählen von Audiotiteln in verschiedenen Sprachen

Verwenden Sie die [**AudioStreamIndex**](https://msdn.microsoft.com/library/windows/apps/br227358)-Eigenschaft und die [**GetAudioStreamLanguage**](https://msdn.microsoft.com/library/windows/apps/br227384)-Methode, um die Audiotitel eines Videos auf eine andere Sprache festzulegen. Videos können außerdem mehrere Audiotitel in derselben Sprache enthalten, beispielsweise einen Kommentar des Regisseurs zu einem Film. In diesem Beispiel wird gezielt dargestellt, wie Sie zwischen verschiedenen Sprachen wechseln. Sie können diesen Code jedoch ändern, um zwischen beliebigen Audiotiteln zu wechseln.

**So wählen Sie Audiotitel in verschiedenen Sprachen aus**

1.  Rufen Sie die Audiotitel ab.

    Suchen Sie nach einem Titel in einer bestimmten Sprache, indem Sie die einzelnen Audiotitel im Video durchlaufen. Verwenden Sie [**AudioStreamCount**](https://msdn.microsoft.com/library/windows/apps/br227356) als Maximalwert für eine **for**-Schleife.

2.  Rufen Sie die Sprache des Audiotitels ab.

    Rufen Sie die Sprache des Titels mit der [**GetAudioStreamLanguage**](https://msdn.microsoft.com/library/windows/apps/br227384)-Methode ab. Die Sprache des Titels wird durch einen [Sprachcode](http://msdn.microsoft.com/library/ms533052(vs.85).aspx) identifiziert, z. B. **"en"** für Englisch oder **"ja"** für Japanisch.

3.  Legen Sie den aktiven Audiotitel fest.

    Wenn Sie den Titel mit der gewünschten Sprache gefunden haben, legen Sie [**AudioStreamIndex**](https://msdn.microsoft.com/library/windows/apps/br227358) auf den Index des Titels fest. Durch Festlegen von **AudioStreamIndex** auf **null** wird der Standardaudiotitel ausgewählt, der durch den Inhalt definiert ist.

Im Folgenden sehen Sie Code, mit dem der Audiotitel auf die angegebene Sprache festgelegt wird. Es durchläuft die Audiospuren eines [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)-Objekts und ruft mit [**GetAudioStreamLanguage**](https://msdn.microsoft.com/library/windows/apps/br227384) die Sprache der einzelnen Spuren ab. Ist der gewünschte Sprachtitel vorhanden, wird [**AudioStreamIndex**](https://msdn.microsoft.com/library/windows/apps/br227358) auf den Index dieses Titels festgelegt.

```csharp
/// <summary>
/// Attemps to set the audio track of a video to a specific language
/// </summary>
/// <param name="lcid">The id of the language. For example, "en" or "ja"</param>
/// <returns>true if the track was set; otherwise, false.</returns>
private bool SetAudioLanguage(string lcid, MediaElement media)
{
    bool wasLanguageSet = false;

    for (int index = 0; index < media.AudioStreamCount; index++)
    {
        if (media.GetAudioStreamLanguage(index) == lcid)
        {
            media.AudioStreamIndex = index;
            wasLanguageSet = true;
        }
    }

    return wasLanguageSet;
}
```

### Aktivieren des Videorenderings im Vollfenstermodus

Legen Sie die [**IsFullWindow**](https://msdn.microsoft.com/library/windows/apps/dn298980)-Eigenschaft fest, um das Rendering im Vollfenstermodus zu aktivieren und zu deaktivieren. Wenn Sie das Rendering im Vollfenstermodus in Ihrer App programmgesteuert festlegen, sollten Sie immer die **IsFullWindow**-Eigenschaft verwenden, anstatt diese Einstellung manuell vorzunehmen. **IsFullWindow** stellt sicher, dass Optimierungen auf Systemebene zum Verbessern der Leistung und Akkulaufzeit durchgeführt werden. Wenn das Rendering im Vollfenstermodus nicht korrekt eingerichtet wird, werden diese Optimierungen möglicherweise nicht angewendet.

Der folgende Code erstellt ein [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244)-Steuerelement zum Umschalten des Renderings im Vollfenstermodus.

```xaml
<AppBarButton Icon="FullScreen" 
              Label="Full Window"
              Click="FullWindow_Click"/>>
```

```csharp
private void FullWindow_Click(object sender, object e)
{
    mediaPlayer.IsFullWindow = !media.IsFullWindow;
}
```

### Ändern der Größe und Strecken von Videos

Verwenden Sie die [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br227422)-Eigenschaft, um zu ändern, wie der Videoinhalt den Container ausfüllt, in dem er sich befindet. Das Video wird dabei entsprechend dem [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br242968)-Wert vergrößert bzw. verkleinert oder gestreckt. Die **Stretch**-Zustände sind mit den Bildformateinstellungen bei vielen Fernsehern vergleichbar. Sie können dieses Verhalten mit einer Schaltfläche verknüpfen und dem Benutzer die Auswahl der gewünschten Einstellung ermöglichen.

-   [
              **None**
            ](https://msdn.microsoft.com/library/windows/apps/br242968) zeigt den Inhalt mit der systemeigenen Auflösung in seiner Originalgröße an.
-   [
              **Uniform**
            ](https://msdn.microsoft.com/library/windows/apps/br242968) füllt unter Beibehaltung des Seitenverhältnisses und des Bildinhalts den größtmöglichen Platz aus. Dies kann zu horizontalen oder vertikalen schwarzen Balken an den Rändern des Videos führen. Dieser Zustand ist mit Breitbildmodi vergleichbar.
-   [
              **UniformToFill**
            ](https://msdn.microsoft.com/library/windows/apps/br242968) füllt den gesamten Platz unter Beibehaltung des Seitenverhältnisses aus. Dies kann dazu führen, dass ein Teil des Bilds abgeschnitten wird. Dieser Zustand ist mit Vollbildmodi vergleichbar.
-   [
              **Fill**
            ](https://msdn.microsoft.com/library/windows/apps/br242968) füllt den gesamten Platz aus, ohne das Seitenverhältnis beizubehalten. Das Bild wird nicht zugeschnitten, kann aber gestreckt werden. Dieser Zustand ist mit Streckmodi vergleichbar.

![Stretch-Enumerationswerte](images/Image_Stretch.jpg) In diesem Beispiel werden mit einem [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244)-Element die [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br242968)-Optionen durchlaufen. Eine **switch**-Anweisung überprüft den aktuellen Zustand der [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br227422)-Eigenschaft und legt sie auf den nächsten Wert in der **Stretch**-Enumeration fest. So kann der Benutzer zwischen verschiedenen Streckungszuständen wechseln.

```xaml
<AppBarButton Icon="Switch" 
              Label="Resize Video"
              Click="PictureSize_Click" />
```

```csharp
private void PictureSize_Click(object sender, RoutedEventArgs e)
{
    switch (mediaPlayer.Stretch)
    {
        case Stretch.Fill:
            mediaPlayer.Stretch = Stretch.None;
            break;
        case Stretch.None:
            mediaPlayer.Stretch = Stretch.Uniform;
            break;
        case Stretch.Uniform:
            mediaPlayer.Stretch = Stretch.UniformToFill;
            break;
        case Stretch.UniformToFill:
            mediaPlayer.Stretch = Stretch.Fill;
            break;
        default:
            break;
    }
}
```

### Aktivieren der Wiedergabe mit geringer Wartezeit

Legen Sie die [**RealTimePlayback**](https://msdn.microsoft.com/library/windows/apps/br227414)-Eigenschaft für ein [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) auf **true** fest, damit das Medienelement die anfängliche Wartezeit für die Wiedergabe reduzieren kann. Dies ist von entscheidender Bedeutung für Apps mit bidirektionaler Kommunikation und kann für einige Spieleszenarien erforderlich sein. Beachten Sie, dass dieser Modus viele Ressourcen und eine höhere Leistung benötigt.

Das folgende Beispiel erstellt ein [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) und legt [**RealTimePlayback**](https://msdn.microsoft.com/library/windows/apps/br227414) auf **true** fest.

```xaml
<MediaElement x:Name="mediaPlayer" RealTimePlayback="True"/>
```

```csharp
MediaElement mediaPlayer = new MediaElement();
mediaPlayer.RealTimePlayback = true;
```
    
## Empfehlungen 

Der Mediaplayer wird in einem dunklen Design und einem hellen Design bereitgestellt. Sie sollten sich aber in den meisten Fällen für das dunkle Design entscheiden. Der dunkle Hintergrund bietet einen besseren Kontrast, insbesondere bei schwierigen Lichtbedingungen, und verhindert Beeinträchtigungen der Steuerleiste auf der Benutzeroberfläche.

Fördern Sie eine dedizierte Anzeigeumgebung, indem Sie den Vollbildmodus gegenüber dem Inlinemodus bevorzugen. Die Vollbildansicht ist optimal. Die Optionen sind im Inlinemodus eingeschränkt.

Wenn Sie Platz auf dem Bildschirm haben, verwenden Sie das doppelzeilige Layout. Es bietet mehr Platz für Steuerelemente als das kompakte einzeilige Layout.

Fügen Sie beliebige benutzerdefinierte Optionen in den Mediaplayer ein, um die beste Benutzererfahrung für Ihre App bereitzustellen, aber bedenken Sie Folgendes:

-   Beschränken Sie die Anpassung der Standardsteuerelemente, die für die Medienwiedergabe optimiert wurden.
-   Auf Handys und anderen mobilen Geräten bleibt das Geräte-Chrom schwarz, auf Laptops und Desktops wird für das Geräte-Chrom die Designfarbe des Benutzers übernommen.
-   Versuchen Sie nicht, die Steuerleiste mit zu vielen Optionen zu überladen.
-   Minimieren Sie die Medienzeitachse nicht unter ihre minimale Standardgröße, weil dadurch ihre Effektivität stark verringert wird.

## Verwandte Artikel

- [Befehlsdesigngrundlagen für UWP-Apps](https://msdn.microsoft.com/library/windows/apps/dn958433)
- [Inhaltsdesigngrundlagen für UWP-Apps](https://msdn.microsoft.com/library/windows/apps/dn958434)


<!--HONumber=May16_HO2-->


