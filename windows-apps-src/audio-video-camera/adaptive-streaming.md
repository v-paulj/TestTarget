---
author: drewbatgit
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: "In diesem Artikel wird beschrieben, wie Sie die Wiedergabe von Multimediainhalten für adaptives Streaming einer App für die Universelle Windows-Plattform (UWP) hinzufügen können. Dieses Feature unterstützt derzeit die Wiedergabe von Inhalten über HTTP Live Streaming(HLS) und Dynamic Streaming over HTTP(DASH)."
title: Adaptives Streaming
translationtype: Human Translation
ms.sourcegitcommit: d0941887ebc17f3665302fae6c7b0a124dfb5a0b
ms.openlocfilehash: 431fa345c0135a08c1da68904a8d58d969490a8d

---

# Adaptives Streaming

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \].

In diesem Artikel wird beschrieben, wie Sie die Wiedergabe von Multimediainhalten für adaptives Streaming einer App für die Universelle Windows-Plattform (UWP) hinzufügen können. Dieses Feature unterstützt derzeit die Wiedergabe von Inhalten über HTTP Live Streaming(HLS) und Dynamic Streaming over HTTP(DASH).

Eine Liste mit unterstützten HLS-Protokolltags finden Sie unter [Unterstützung von HLS-Tags](hls-tag-support.md). 

> [!NOTE] 
> Der Code in diesem Artikel wurde aus dem [Beispiel für adaptives Streaming](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming) für UWP übernommen und angepasst.

## Einfaches adaptives Streaming mit MediaPlayer und MediaPlayerElement

Erstellen Sie ein **Uri**-Objekt, das auf eine DASH- oder HLS-Manifestdatei verweist, um Medien für adaptives Streaming in einer UWP-App wiederzugeben. Erstellen Sie eine Instanz der [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer)-Klasse. Rufen Sie [**MediaSource.CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912) auf, um ein neues **MediaSource**-Objekt zu erstellen, und legen Sie es dann auf die [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source)-Eigenschaft von **MediaPlayer** fest. Rufen Sie [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play) auf, um die Wiedergabe des Medieninhalts zu starten.

[!code-cs[DeclareMediaPlayer](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-cs[ManifestSourceNoUI](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSourceNoUI)]

Im obigen Beispiel werden die Audiodaten des Medieninhalts wiedergegeben, aber der Inhalt wird nicht automatisch auf Ihrer Benutzeroberfläche gerendert. Für die meisten Apps, mit denen Videoinhalte wiedergegeben werden, wird der Inhalt auf einer XAML-Seite gerendert.  Fügen Sie der XAML-Seite hierzu ein [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement)-Steuerelement hinzu.

[!code-xml[MediaPlayerElementXAML](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

Rufen Sie [**MediaSource.CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912) auf, um ein **MediaSource**-Element über den URI einer DASH- oder HLS-Manifestdatei zu erstellen. Legen Sie anschließend die [**Source**](https://msdn.microsoft.com/library/windows/apps/br227420)-Eigenschaft von **MediaPlayerElement** fest. **MediaPlayerElement** erstellt automatisch eine neues **MediaPlayer**-Objekt für den Inhalt. Sie können **Play** für das **MediaPlayer**-Objekt aufrufen, um die Wiedergabe des Inhalts zu starten.

[!code-cs[ManifestSource](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSource)]

> [!NOTE] 
> Ab Windows10, Version1607, wird die Verwendung der **MediaPlayer**-Klasse zum Wiedergeben von Medienelementen empfohlen. **MediaPlayerElement** ist ein einfaches XAML-Steuerelement, das zum Rendern des Inhalts eines **MediaPlayer**-Objekts auf einer XAML-Seite verwendet wird. Das **MediaElement**-Steuerelement wird aus Gründen der Abwärtskompatibilität weiterhin unterstützt. Weitere Informationen zur Verwendung von **MediaPlayer** und **MediaPlayerElement** zum Wiedergeben von Medieninhalten finden Sie unter [Wiedergeben von Audio- und Videoinhalten mit „MediaPlayer“](play-audio-and-video-with-mediaplayer.md). Informationen zur Verwendung von **MediaSource** und dazugehörigen APIs für die Arbeit mit Medieninhalten finden Sie unter [Medienelemente, Wiedergabelisten und Titel](media-playback-with-mediasource.md).

## Adaptives Streaming mit AdaptiveMediaSource

Verwenden Sie das [**AdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn946912)-Objekt, wenn Ihre App erweiterte adaptive Streamingfunktionen erfordert, z.B. das Bereitstellen von benutzerdefinierten HTTP-Headern, die Überwachung der aktuellen Download- und Wiedergabe-Bitraten oder das Anpassen der Verhältnisse, die bestimmen, wann das System Bitraten des adaptiven Datenstroms wechselt.

Die adaptiven Streaming-APIs sind im [**Windows.Media.Streaming.Adaptive**](https://msdn.microsoft.com/library/windows/apps/dn931279)-Namespace zu finden.

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

Initialisieren Sie **AdaptiveMediaSource** mit dem URI einer adaptiven Streaming-Manifestdatei durch Aufrufen von [**CreateFromUriAsync**](https://msdn.microsoft.com/library/windows/apps/dn931261). Der von dieser Methode zurückgegebene [**AdaptiveMediaSourceCreationStatus**](https://msdn.microsoft.com/library/windows/apps/dn946917)-Wert teilt Ihnen mit, ob die Medienquelle erfolgreich erstellt wurde. In diesem Fall können Sie das Objekt als Datenstromquelle für **MediaPlayer** durch Aufrufen von [**SetMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn652653) festlegen. In diesem Beispiel wird die [**AvailableBitrates**](https://msdn.microsoft.com/library/windows/apps/dn931257)-Eigenschaft abgefragt, um die maximal unterstützte Bitrate für diesen Datenstrom zu ermitteln. Anschließend wird dieser Wert als ursprüngliche Bitrate festgelegt. In diesem Beispiel werden auch Handler für die Ereignisse [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272), [**DownloadBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931269) und [**PlaybackBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931278) registriert, die später in diesem Artikel erläutert werden.

[!code-cs[InitializeAmmo.](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

Wenn Sie benutzerdefinierte HTTP-Header für das Abrufen der Manifestdatei festlegen müssen, können Sie ein [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)-Objekt erstellen, die gewünschten Header festlegen und das Objekt an die Überladung von **CreateFromUriAsync** übergeben.

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

Das [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272)-Ereignis wird ausgelöst, wenn das System eine Ressource vom Server abruft. Die an den Ereignishandler übergebene [**AdaptiveMediaSourceDownloadRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn946935) macht Eigenschaften verfügbar, die Informationen über die angeforderte Ressource wie Typ und URI der Ressource bereitstellen.

Sie können mit dem **DownloadRequested**-Ereignishandler die Ressourcenanforderung durch Aktualisieren der Eigenschaften des von den Ereignisargumenten bereitgestellten [**AdaptiveMediaSourceDownloadResult**](https://msdn.microsoft.com/library/windows/apps/dn946942)-Objekts ändern. Im folgenden Beispiel wird der URI, aus dem die Ressource abgerufen wird, durch die Aktualisierung der [**ResourceUri**](https://msdn.microsoft.com/library/windows/apps/dn931250)-Eigenschaften des Ergebnisobjekts geändert.

Sie können den Inhalt der angeforderten Ressource durch Festlegen der [**Buffer**](https://msdn.microsoft.com/library/windows/apps/dn946943)- oder [**InputStream**](https://msdn.microsoft.com/library/windows/apps/dn931249)-Eigenschaften des Ergebnisobjekts überschreiben. Im folgenden Beispiel wird der Inhalt der Manifestressource durch Festlegen der **Buffer**-Eigenschaft ersetzt. Wenn Sie die Ressourcenanforderung mit Daten aktualisieren, die asynchron abgerufen werden, z.B. durch Abrufen von Daten von einem Remoteserver oder asynchrone Benutzerauthentifizierung, müssen Sie [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/dn946936) aufrufen, um eine Verzögerung abzurufen. Anschließend bei Abschluss des Vorgangs rufen Sie dann [**Complete**](https://msdn.microsoft.com/library/windows/apps/dn946934) auf, um dem System zu signalisieren, dass der Downloadanforderungsvorgang fortgesetzt werden kann.

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

Das **AdaptiveMediaSource**-Objekt stellt Ereignisse bereit, mit denen Sie reagieren können, wenn sich die Download- oder Wiedergabe-Bitraten ändern. In diesem Beispiel werden die aktuellen Bitraten einfach auf der Benutzeroberfläche aktualisiert. Sie können die Verhältnisse ändern, die bestimmen, wann das System Bitraten des adaptiven Datenstroms wechselt. Weitere Informationen finden Sie unter der [**AdvancedSettings**](https://msdn.microsoft.com/library/windows/apps/mt628697)-Eigenschaft.

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

## Verwandte Themen
* [Medienwiedergabe](media-playback.md)
* [Unterstützung von HLS-Tags](hls-tag-support.md) 
* [Wiedergeben von Audio- und Videoinhalten mit „MediaPlayer“](play-audio-and-video-with-mediaplayer.md)
* [Wiedergeben von Medien im Hintergrund](background-audio.md) 








<!--HONumber=Aug16_HO3-->


