---
author: Jwmsft
Description: Media Player wird zum Anzeigen und Wiedergeben von Videos, Audiodateien und Bildern verwendet.
title: Media Player
ms.assetid: 9AABB5DE-1D81-4791-AB47-7F058F64C491
dev.assetid: AF2F2008-9B53-430C-BBC3-8888F631B0B0
label: Media player
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 6945708f0d81cd9f2edd363ed193c76601ec5652

---
# Media Player

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Der Media Player wird verwendet, um Videos und Audio anzuzeigen und zu hören. Die Medienwiedergabe kann inline (eingebettet auf einer Seite oder mit einer Gruppe von anderen Steuerelementen) oder in einer dedizierten Vollbildansicht erfolgen. Sie können die Schaltflächen des Players ändern, den Hintergrund der Steuerelementleiste ändern und Layouts wie gewünscht anordnen. Beachten Sie jedoch, dass Benutzer eine Reihe grundlegender Steuerelemente (Wiedergabe/Pause, Zurückspringen, Vorwärts springen) erwarten.

![Media Player-Element mit Transportsteuerelementen](images/controls/mtc_double_video_inprod.png)

<div class="important-apis" >
<b>Wichtige APIs</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx"><strong>MediaPlayerElement-Klasse</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediatransportcontrols"><strong>MediaTransportControls-Klasse</strong></a></li>
</ul>

</div>
</div>



> **Hinweis**&nbsp;&nbsp; **MediaPlayerElement** steht erst ab Windows10, Version1607, zur Verfügung. Für das Entwickeln von Apps für niedrigere Windows10-Versionen muss stattdessen [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) verwendet werden. Alle Empfehlungen auf dieser Seite gelten auch für MediaElement.

## Ist dies das richtige Steuerelement?

Verwenden Sie einen Mediaplayer, wenn Sie Audio- oder Videodateien in Ihrer App wiedergeben möchten. Verwenden Sie zum Anzeigen einer Sammlung von Bildern die [Flip-Ansicht](flipview.md).

## Beispiele

Ein Media Player in der Windows10-Erste Schritte-App.

![Ein Medienelement in der Windows 10-Erste Schritte-App.](images/control-examples/mtc_getstarted_example.png)

## Erstellen eines Media Players
Sie fügen Ihrer App Medien hinzu, indem Sie ein [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)-Objekt in XAML erstellen und die [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) auf eine [**MediaSource**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.core.mediasource.aspx) festlegen, die auf eine Audio- oder Videodatei verweist.

Mit diesem XAML-Code wird ein [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) erstellt, dessen [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx)-Eigenschaft auf den URI einer Videodatei festgelegt wird, bei der es sich um eine lokale Datei der App handelt. Das **MediaPlayerElement** beginnt mit der Wiedergabe, wenn die Seite geladen wird. Um zu verhindern, dass die Medienwiedergabe sofort beginnt, können Sie die [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.autoplay.aspx)-Eigenschaft auf **false** festlegen.

```xaml
<MediaPlayerElement x:Name="mediaSimple"
                    Source="Videos/video1.mp4"
                    Width="400" AutoPlay="True"/>
```

Mit diesem XAML-Code wird ein [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)-Objekt mit aktivierten integrierten Transportsteuerelementen und mit auf **false** festgelegter [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.autoplay.aspx)-Eigenschaft erstellt.


```xaml
<MediaPlayerElement x:Name="mediaPlayer"
                    Source="Videos/video1.mp4"
                    Width="400"
                    AutoPlay="False"
                    AreTransportControlsEnabled="True"/>
```

### Steuerelemente für den Medientransport
[**MediaPlayerElement**]((https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) besitzt integrierte Transportsteuerelemente, die Wiedergabe, Beenden, Anhalten, Lautstärke, Stummschalten, Suche/Status, Untertitel und Audiospurauswahl behandeln. Um diese Steuerelemente zu aktivieren, legen Sie [**AreTransportControlsEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.AreTransportControlsEnabled.aspx) auf **true** fest. Um sie zu deaktivieren, legen Sie **AreTransportControlsEnabled** auf **false** fest. Die Transportsteuerelemente werden durch die [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn831962)-Klasse dargestellt. Sie können die Transportsteuerelemente unverändert verwenden oder auf verschiedene Weise anpassen. Weitere Informationen finden Sie in der Referenz zur [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn831962)-Klasse und unter [Erstellen von benutzerdefinierten Transportsteuerelementen](custom-transport-controls.md).

Die Transportsteuerelemente unterstützen Layouts mit einer Zeile und mit Doppelzeile. Im ersten Beispiel sehen Sie ein einzeiliges-Layout mit der Wiedergabe/Pause-Schaltfläche links von der Medienzeitachse. Dieses Layout wird am besten für die Wiedergabe von Inlinemedien und kompakte Bildschirme reserviert.

![Beispiel für MTC-Steuerelemente, einzelne Zeile](images/controls/mtc_single_inprod_02.png)

Das Layout mit doppelzeiligen Steuerelementen (siehe unten) wird für die meisten Nutzungsszenarien empfohlen, insbesondere für größere Bildschirme. Dieses Layout bietet mehr Platz für Steuerelemente und erleichtert dem Benutzer die Bedienung der Zeitachse.

![Beispiel für MTC-Steuerelemente, Doppelzeile](images/controls/mtc_double_inprod.png)

**Steuerelemente für den Systemmedientransport**

[
              Das **MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) ist automatisch in die Medientransportsteuerelemente des Systems integriert. Die Medientransportsteuerelemente des Systems sind die Steuerelemente, die angezeigt werden, wenn Hardwaretasten für Medien betätigt werden, z.B. die Medientasten auf Tastaturen. Weitere Informationen finden Sie unter [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677).

> **Hinweis**&nbsp;&nbsp; [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) wird nicht automatisch in die Medientransportsteuerelemente des Systems integriert. Sie müssen die Verbindung daher selbst herstellen. Weitere Informationen finden Sie unter [Steuerelemente für den Systemmedientransport](https://msdn.microsoft.com/library/windows/apps/mt228338).


### Festlegen der Medienquelle
Um Dateien im Netzwerk oder in die App eingebettete Dateien wiederzugeben, legen Sie die [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx)-Eigenschaft auf eine [**MediaSource**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.core.mediasource.aspx) mit dem Pfad der Datei fest.

**Tipp**  Zum Öffnen von Dateien aus dem Internet müssen Sie die **Internet (Client)**-Funktion im App-Manifest (Package.appxmanifest) deklarieren. Weitere Informationen zum Deklarieren von Funktionen finden Sie unter [Deklaration der App-Funktionen](https://msdn.microsoft.com/library/windows/apps/mt270968).

 

Dieser Code versucht, die [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx)-Eigenschaft des im XAML-Code definierten [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) auf den Pfad einer Datei festzulegen, der in ein [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)-Element eingegeben wurde.

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
        mediaPlayer.Source = MediaSource.CreateFromUri(pathUri);
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

Um die Medienquelle auf eine in die App eingebettete Mediendatei festzulegen, initialisieren Sie einen [**Uri**](https://msdn.microsoft.com/library/windows/apps/br226017) mit dem Pfadpräfix **ms-appx:///**, erstellen anschließend eine [**MediaSource**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.core.mediasource.aspx) mit dem URI und legen anschließend die [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) auf den URI fest. Für eine Datei mit dem Namen **video1.mp4**, die sich in dem Unterordner **Videos** befindet, würde der Pfad beispielsweise wie folgt aussehen: **ms-appx:///Videos/video1.mp4**.

Mit diesem Code wird die [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx)-Eigenschaft des [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)-Objekts, das zuvor in XAML definiert wurde, auf **ms-appx:///Videos/video1.mp4** festgelegt.

```csharp
private void LoadEmbeddedAppFile()
{
    try
    {
        Uri pathUri = new Uri("ms-appx:///Videos/video1.mp4");
        mediaPlayer.Source = MediaSource.CreateFromUri(pathUri);
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
Zum Öffnen von Dateien im lokalen System oder aus OneDrive können Sie das [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)-Element verwenden, um die Datei abzurufen, und [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx), um die Medienquelle festzulegen; alternativ können Sie programmgesteuert auf die Benutzermedienordner zugreifen.

Falls für Ihre App jedoch der Zugriff auf die **Musik**- oder **Videoordner** ohne Benutzerinteraktion erforderlich ist (wenn Sie beispielsweise sämtliche Musik- oder Videodateien einer Sammlung des Benutzers aufzählen und in Ihrer App anzeigen), müssen Sie die Funktionen der **Musik-** und **Videobibliothek** deklarieren. Weitere Informationen finden Sie unter [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](https://msdn.microsoft.com/library/windows/apps/mt188703).

Das [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)-Element benötigt keine besonderen Funktionen für den Zugriff auf Dateien im lokalen Dateisystem (etwa die Ordner **Musik** oder **Video** des Benutzers), da Benutzer die vollständige Kontrolle darüber haben, auf welche Datei zugegriffen wird. Aus Sicherheits- und Datenschutzgründen ist es am sinnvollsten, die Anzahl der von der App verwendeten Funktionen möglichst gering zu halten.

**So öffnen Sie lokale Medien mit FileOpenPicker**

1.  Rufen Sie [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) auf, um dem Benutzer die Auswahl einer Mediendatei zu ermöglichen.

    Verwenden Sie die [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)-Klasse, um eine Mediendatei auszuwählen. Legen Sie den [**FileTypeFilter**](https://msdn.microsoft.com/library/windows/apps/br207850) fest, um anzugeben, welche Dateitypen der **FileOpenPicker** anzeigt. Rufen Sie [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) auf, um die Dateiauswahl zu starten und die Datei abzurufen.

2.  Verwenden Sie eine [**MediaSource**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.core.mediasource.aspx), um die ausgewählte Mediendatei als [**MediaPlayerElement.Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) festzulegen.

    Um die [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) zu verwenden, die vom [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) zurückgegeben wurde, müssen Sie die [**CreateFromStorageFile**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.core.mediasource.createfromstoragefile.aspx)-Methode für [**MediaSource**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.core.mediasource.aspx) aufrufen und diese als [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) von [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) festlegen. Rufen Sie dann [**Play**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplayer.play.aspx) für das [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx) auf, um die Medien zu starten.


In diesem Beispiel wird veranschaulicht, wie Sie die [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)-Klasse verwenden, um eine Datei auszuwählen, und die Datei als [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) eines [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) festlegen.

```xaml
<MediaPlayerElement x:Name="mediaPlayer"/>
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

    // mediaPlayer is a MediaPlayerElement defined in XAML
    if (file != null)
    {
        mediaPlayer.Source = MediaSource.CreateFromStorageFile(file);

        mediaPlayer.MediaPlayer.Play();
    }
}
```

### Festlegen der Posterquelle
Mit der [**PosterSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.PosterSource.aspx)-Eigenschaft können Sie eine visuelle Darstellung für Ihr [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) bereitstellen, bevor die Medien geladen werden. Eine **PosterSource** ist ein Bild, z.B. ein Screenshot oder Filmplakat, das anstelle der Medien angezeigt wird. Die **PosterSource** wird in folgenden Fällen angezeigt:

-   Wenn keine gültige Quelle festgelegt ist. Beispiel: [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) ist nicht festgelegt, **Source** wurde auf **Null** festgelegt, oder die Quelle ist ungültig (z.B. wenn ein [**MediaFailed**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplayer.mediafailed.aspx)-Ereignis eintritt).
-   Während Medien geladen werden. Beispiel: Eine gültige Quelle ist festgelegt, aber das [**MediaOpened**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplayer.mediaopened.aspx)-Ereignis ist noch nicht eingetreten.
-   Beim Streamen von Medien auf ein anderes Gerät.
-   Wenn die Medien nur Audio enthalten.

Dies ist ein [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx), dessen [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) auf einen Albumtitel und dessen [**PosterSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.PosterSource.aspx) auf ein Bild des Albumcovers festgelegt ist.

```xaml
<MediaPlayerElement Source="Media/Track1.mp4" PosterSource="Media/AlbumCover.png"/>
```

### Abblenden/Ausschalten des Gerätebildschirms verhindern
Normalerweise wird bei einem Gerät das Display abgeblendet (und schließlich ausgeschaltet), um bei Abwesenheit des Benutzers den Akku zu schonen. Bei Video-Apps muss der Bildschirm jedoch eingeschaltet bleiben, damit der Benutzer das Video sehen kann. Um ein Deaktivieren der Anzeige zu verhindern, wenn keine Benutzeraktion mehr festgestellt werden kann (z.B. bei der Wiedergabe eines Videos in einer App), können Sie [**DisplayRequest.RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818) aufrufen. Mithilfe der [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816)-Klasse können Sie Windows anweisen, das Display eingeschaltet zu lassen, damit der Benutzer das Video sehen kann.

Um Energie zu sparen und den Akku zu schonen, wird empfohlen, [**DisplayRequest.RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819) aufzurufen und die Displayanforderung freizugeben, wenn diese nicht mehr benötigt wird. Windows deaktiviert automatisch die aktiven Displayanforderungen der App, wenn die App vom Bildschirm entfernt wird, und aktiviert die Displayanforderungen wieder, wenn Ihre App wieder in den Vordergrund gesetzt wird.

Im Anschluss sind einige Situationen aufgeführt, in denen Sie die Displayanforderung freigeben sollten:

-   Die Videowiedergabe wird angehalten (beispielsweise durch eine Benutzeraktion, zum Puffern oder für eine Anpassung aufgrund von begrenzter Bandbreite).
-   Die Wiedergabe wird gestoppt. Beispielsweise ist die Wiedergabe des Videos beendet oder die Darstellung vorüber.
-   Ein Wiedergabefehler ist aufgetreten. Es können beispielsweise Probleme mit der Netzwerkverbindung bestehen, oder eine Datei kann beschädigt sein.

> **Hinweis**&nbsp;&nbsp; Wenn [**MediaPlayerElement.IsFullWindow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.IsFullWindow.aspx) auf „true“ festgelegt ist und Medien wiedergegeben werden, wird die Deaktivierung der Anzeige automatisch verhindert.

**So verhindern Sie das Abblenden/Ausschalten des Bildschirms**

1.  Erstellen Sie eine globale [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816)-Variable. Initialisieren Sie sie mit dem Wert NULL.
```csharp
// Create this variable at a global scope. Set it to null.
private DisplayRequest appDisplayRequest = null;
```

2.  Rufen Sie [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818) auf, um Windows mitzuteilen, dass für die App das Display eingeschaltet bleiben muss.

3.  Rufen Sie [**RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819) auf, um die Displayanforderung freizugeben, wenn die Videowiedergabe beendet, angehalten oder durch einen Wiedergabefehler unterbrochen wird. Falls für die App keine aktiven Displayanforderungen mehr vorhanden sind, schont Windows den Akku, indem das Display abgeblendet (und schließlich ausgeschaltet) wird, wenn das Gerät nicht verwendet wird.

    Jeder [**MediaPlayerElement.MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx) hat eine [**PlaybackSession**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplayer.playbacksession.aspx) vom Typ [**MediaPlaybackSession**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplaybacksession.aspx), die verschiedene Aspekte der Medienwiedergabe steuert, wie [**PlaybackRate**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplaybacksession.playbackrate.aspx), [**PlaybackState**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplaybacksession.playbackstate.aspx) und [**Position**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplaybacksession.position.aspx). Hier verwenden Sie das [**PlaybackStateChanged**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplaybacksession.playbackstatechanged.aspx)-Ereignis auf [**MediaPlayer.PlaybackSession**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplayer.playbacksession.aspx), um Situationen zu erkennen, in denen Sie die Displayanforderung freigeben sollten. Verwenden Sie dann die [**NaturalVideoHeight**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplaybacksession.naturalvideoheight.aspx)-Eigenschaft, um festzustellen, ob eine Audio- oder Videodatei wiedergegeben wird, und lassen Sie den Bildschirm nur eingeschaltet, wenn eine Videodatei wiedergegeben wird.
    ```xaml
<MediaPlayerElement x:Name="mpe" Source="Media/video1.mp4"/>
    ```

    ```csharp
    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        mpe.MediaPlayer.PlaybackSession.PlaybackStateChanged += MediaPlayerElement_CurrentStateChanged;
        base.OnNavigatedTo(e);
    }

    private void MediaPlayerElement_CurrentStateChanged(object sender, RoutedEventArgs e)
    {
        MediaPlaybackSession playbackSession = sender as MediaPlaybackSession;
        if (playbackSession != null && playbackSession.NaturalVideoHeight != 0)
        {
            if(playbackSession.PlaybackState == MediaPlaybackState.Playing)
            {
                if(appDisplayRequest == null)
                {
                    // This call creates an instance of the DisplayRequest object
                    appDisplayRequest = new DisplayRequest();
                    appDisplayRequest.RequestActive();
                }
            }
            else // PlaybackState is Buffering, None, Opening or Paused
            {
                if(appDisplayRequest != null)
                {
                      // Deactivate the displayr request and set the var to null
                      appDisplayRequest.RequestRelease();
                      appDisplayRequest = null;
                }
            }
        }

    }
    ```

### Programmgesteuertes Steuern der Medienwiedergabe
[**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) stellt zahlreiche Eigenschaften, Methoden und Ereignisse zum Steuern der Audio- und Videowiedergabe über die [**MediaPlayerElement.MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx)-Eigenschaft bereit. Eine vollständige Liste der Eigenschaften, Methoden und Ereignisse finden Sie auf der Referenzseite zu [**MediaPlayer**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplayer.aspx).

### Erweiterte Medienwiedergabeszenarien
Für komplexere Medienwiedergabeszenarien, z.B. die Wiedergabe einer Playlist, Umschalten zwischen Audiosprachen oder Erstellen benutzerdefinierter Metadatentracks, legen Sie [**MediaPlayerElement.Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) auf eine [**MediaPlaybackItem**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplaybackitem.aspx) oder eine [**MediaPlaybackList**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplaybacklist.aspx) fest. Weitere Informationen dazu, wie Sie verschiedene erweiterte Medienfunktionen aktivieren, finden Sie auf der Seite [**Medienwiedergabe**](https://msdn.microsoft.com/en-us/windows/uwp/audio-video-camera/media-playback-with-mediasource) im Dev Center.

### Aktivieren des Videorenderings im Vollfenstermodus

Legen Sie die [**IsFullWindow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.isfullwindow.aspx)-Eigenschaft fest, um das Rendering im Vollfenstermodus zu aktivieren und zu deaktivieren. Wenn Sie das Rendering im Vollfenstermodus in Ihrer App programmgesteuert festlegen, sollten Sie immer die **IsFullWindow**-Eigenschaft verwenden, anstatt diese Einstellung manuell vorzunehmen. **IsFullWindow** stellt sicher, dass Optimierungen auf Systemebene zum Verbessern der Leistung und Akkulaufzeit durchgeführt werden. Wenn das Rendering im Vollfenstermodus nicht korrekt eingerichtet wird, werden diese Optimierungen möglicherweise nicht angewendet.

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

Verwenden Sie die [**Stretch**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.stretch.aspx)-Eigenschaft, um zu ändern, wie der Videoinhalt und/oder die [**PosterSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.postersource.aspx) den Container ausfüllt, in dem er sich befindet. Das Video wird dabei entsprechend dem [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br242968)-Wert vergrößert bzw. verkleinert oder gestreckt. Die **Stretch**-Zustände sind mit den Bildformateinstellungen bei vielen Fernsehern vergleichbar. Sie können dieses Verhalten mit einer Schaltfläche verknüpfen und dem Benutzer die Auswahl der gewünschten Einstellung ermöglichen.

-   [**None**](https://msdn.microsoft.com/library/windows/apps/br242968) zeigt den Inhalt mit der systemeigenen Auflösung in seiner Originalgröße an.
-   [**Uniform**](https://msdn.microsoft.com/library/windows/apps/br242968) füllt unter Beibehaltung des Seitenverhältnisses und des Bildinhalts den größtmöglichen Platz aus. Dies kann zu horizontalen oder vertikalen schwarzen Balken an den Rändern des Videos führen. Dieser Zustand ist mit Breitbildmodi vergleichbar.
-   [**UniformToFill**](https://msdn.microsoft.com/library/windows/apps/br242968) füllt den gesamten Platz unter Beibehaltung des Seitenverhältnisses aus. Dies kann dazu führen, dass ein Teil des Bilds abgeschnitten wird. Dieser Zustand ist mit Vollbildmodi vergleichbar.
-   [**Fill**](https://msdn.microsoft.com/library/windows/apps/br242968) füllt den gesamten Platz aus, ohne das Seitenverhältnis beizubehalten. Das Bild wird nicht zugeschnitten, kann aber gestreckt werden. Dieser Zustand ist mit Streckmodi vergleichbar.

![Stretch-Enumerationswerte](images/Image_Stretch.jpg)

In diesem Beispiel werden mit einem [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244)-Element die [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br242968)-Optionen durchlaufen. Eine **switch**-Anweisung überprüft den aktuellen Zustand der [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br227422)-Eigenschaft und legt sie auf den nächsten Wert in der **Stretch**-Enumeration fest. So kann der Benutzer zwischen verschiedenen Streckungszuständen wechseln.

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

Legen Sie die [**RealTimePlayback**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplayer.realtimeplayback.aspx)-Eigenschaft für ein [**MediaPlayerElement.MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx) auf **true** fest, damit das Media Player-Element die anfängliche Wartezeit für die Wiedergabe reduzieren kann. Dies ist von entscheidender Bedeutung für Apps mit bidirektionaler Kommunikation und kann für einige Spieleszenarien erforderlich sein. Beachten Sie, dass dieser Modus viele Ressourcen und eine höhere Leistung benötigt.

Das folgende Beispiel erstellt ein [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) und legt [**RealTimePlayback**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplayer.realtimeplayback.aspx) auf **true** fest.


```csharp
MediaPlayerElement mp = new MediaPlayerElement();
mp.MediaPlayer.RealTimePlayback = true;
```

## Empfehlungen

MediaPlayer unterstützt helle und dunkle Designs. Dunkle Designs bieten für die Mehrzahl der Unterhaltungsszenarien jedoch eine bessere Umgebung. Der dunkle Hintergrund bietet einen besseren Kontrast, insbesondere bei schwierigen Lichtbedingungen, und verhindert Beeinträchtigungen der Steuerleiste auf der Benutzeroberfläche.

Unterstützen Sie beim Abspielen von Videoinhalten eine dedizierte Anzeigeumgebung, indem Sie den Vollbildmodus gegenüber dem Inlinemodus bevorzugen. Die Vollbildansicht ist optimal. Die Optionen sind im Inlinemodus eingeschränkt.

Wenn Sie die nötige Bildschirmfläche haben oder für 10 Fuß-Umgebungen entwerfen, sollten Sie sich für das Layout mit Doppelzeile entscheiden. Es bietet mehr Platz für Steuerelemente als das kompakte einzeilige Layout, und die Navigation mit dem Gamepad in 10-Fuß-Szenarien ist einfacher.

> **Hinweis**&nbsp;&nbsp; Im Artikel [Designing for Xbox and TV](../input-and-devices/designing-for-tv.md) finden Sie weitere Informationen zur Optimierung Ihrer Anwendung für 10-Fuß-Umgebungen.

Die Standardsteuerelemente wurden für die Medienwiedergabe optimiert. Sie können jedoch benutzerdefinierte Optionen hinzufügen, die Sie für den Media Player benötigen, um für Ihre App eine optimale Umgebung bereitzustellen. Weitere Informationen zum Hinzufügen von benutzerdefinierten Steuerelementen finden Sie unter [Erstellen benutzerdefinierter Transportsteuerelemente](custom-transport-controls.md).

## Verwandte Artikel

- [Befehlsdesigngrundlagen für UWP-Apps](https://msdn.microsoft.com/library/windows/apps/dn958433)
- [Grundlagen des Inhaltsdesigns für UWP-Apps](https://msdn.microsoft.com/library/windows/apps/dn958434)



<!--HONumber=Aug16_HO3-->


