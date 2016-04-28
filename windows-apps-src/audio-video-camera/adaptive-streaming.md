---
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: Hier wird beschrieben, wie Sie die Wiedergabe von Multimediainhalten für adaptives Streaming zu Apps für die UWP hinzufügen können. Dieses Feature unterstützt derzeit die Wiedergabe von HTTP Live Streaming (HLS)- und Dynamic Streaming over HTTP (DASH)-Inhalten.
title: Adaptives Streaming
---

# Adaptives Streaming

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

In diesem Artikel wird beschrieben, wie Sie die Wiedergabe von Multimediainhalten für adaptives Streaming zu Apps für die universelle Windows-Plattform (UWP) hinzufügen können. Dieses Feature unterstützt derzeit die Wiedergabe von HTTP Live Streaming (HLS)- und Dynamic Streaming over HTTP (DASH)-Inhalten.

## Einfache adaptives Streaming mit MediaElement

Um adaptive Streaming-Multimediainhalte in einer XAML-basierten App anzuzeigen, fügen Sie der Seite ein [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)-Steuerelement hinzu.

[!code-xml[MediaElementXAML](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml#SnippetMediaElementXAML)]

Legen Sie die [**Source**](https://msdn.microsoft.com/library/windows/apps/br227420)-Eigenschaft von **MediaElement** auf den URI einer DASH- oder HLS-Manifestdatei fest.

[!code-cs[ManifestSource](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetManifestSource)]

## Adaptives Streaming mit AdaptiveMediaSource

Verwenden Sie das [**AdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn946912)-Objekt, wenn Ihre App erweiterte adaptive Streamingfunktionen erfordert, z. B. das Bereitstellen von benutzerdefinierten HTTP-Headern, die Überwachung der aktuellen Download- und Wiedergabe-Bitraten oder das Anpassen der Verhältnisse, die bestimmen, wann das System Bitraten des adaptiven Datenstroms wechselt.

Die adaptiven Streaming-APIs sind im [**Windows.Media.Streaming.Adaptive**](https://msdn.microsoft.com/library/windows/apps/dn931279)-Namespace zu finden.

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

Initialisieren Sie **AdaptiveMediaSource** mit dem URI einer adaptiven Streaming-Manifestdatei durch Aufrufen von [**CreateFromUriAsync**](https://msdn.microsoft.com/library/windows/apps/dn931261). Der von dieser Methode zurückgegebene [**AdaptiveMediaSourceCreationStatus**](https://msdn.microsoft.com/library/windows/apps/dn946917)-Wert teilt Ihnen mit, ob die Medienquelle erfolgreich erstellt wurde. In diesem Fall können Sie das Objekt als Datenstromquelle für **MediaElement** durch Aufrufen von [**SetMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn299029) festlegen. In diesem Beispiel wird die [**AvailableBitrates**](https://msdn.microsoft.com/library/windows/apps/dn931257)-Eigenschaft abgefragt, um die maximal unterstützte Bitrate für diesen Datenstrom zu ermitteln. Anschließend wird dieser Wert als ursprüngliche Bitrate festgelegt. In diesem Beispiel werden auch Handler für die Ereignisse [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272), [**DownloadBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931269) und [**PlaybackBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931278) registriert, die später in diesem Artikel erläutert werden.

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

Wenn Sie benutzerdefinierte HTTP-Header für das Abrufen der Manifestdatei festlegen müssen, können Sie ein [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)-Objekt erstellen, die gewünschten Header festlegen und das Objekt an die Überladung von **CreateFromUriAsync** übergeben.

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

Das [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272)-Ereignis wird ausgelöst, wenn das System eine Ressource vom Server abruft. Die an den Ereignishandler übergebene [**AdaptiveMediaSourceDownloadRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn946935) macht Eigenschaften verfügbar, die Informationen über die angeforderte Ressource wie Typ und URI der Ressource bereitstellen.

Sie können mit dem **DownloadRequested**-Ereignishandler die Ressourcenanforderung durch Aktualisieren der Eigenschaften des von den Ereignisargumenten bereitgestellten [**AdaptiveMediaSourceDownloadResult**](https://msdn.microsoft.com/library/windows/apps/dn946942)-Objekts ändern. Im folgenden Beispiel wird der URI, aus dem die Ressource abgerufen wird, durch die Aktualisierung der [**ResourceUri**](https://msdn.microsoft.com/library/windows/apps/dn931250)-Eigenschaften des Ergebnisobjekts geändert.

Sie können den Inhalt der angeforderten Ressource durch Festlegen der [**Buffer**](https://msdn.microsoft.com/library/windows/apps/dn946943)- oder [**InputStream**](https://msdn.microsoft.com/library/windows/apps/dn931249)-Eigenschaften des Ergebnisobjekts überschreiben. Im folgenden Beispiel wird der Inhalt der Manifestressource durch Festlegen der **Buffer**-Eigenschaft ersetzt. Wenn Sie die Ressourcenanforderung mit Daten aktualisieren, die asynchron abgerufen werden, z. B. durch Abrufen von Daten von einem Remoteserver oder asynchrone Benutzerauthentifizierung, müssen Sie [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/dn946936) aufrufen, um eine Verzögerung abzurufen, und anschließend bei Abschluss des Vorgangs [**Complete**](https://msdn.microsoft.com/library/windows/apps/dn946934) aufrufen, um dem System zu signalisieren, dass der Downloadanforderungsvorgang fortgesetzt werden kann.

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

Das **AdaptiveMediaSource**-Objekt stellt Ereignisse bereit, mit denen Sie reagieren können, wenn sich die Download- oder Wiedergabe-Bitraten ändern. In diesem Beispiel werden die aktuellen Bitraten einfach auf der Benutzeroberfläche aktualisiert. Sie können die Verhältnisse ändern, die bestimmen, wann das System Bitraten des adaptiven Datenstroms wechselt. Weitere Informationen finden Sie in der [**AdvancedSettings**](https://msdn.microsoft.com/library/windows/apps/mt628697)-Eigenschaft.

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

 

 






<!--HONumber=Mar16_HO1-->


