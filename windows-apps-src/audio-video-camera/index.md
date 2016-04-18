---
ms.assetid: 0fc12d26-f1cf-4da7-b5a7-735a5074b74a
description: Dieser Abschnitt enthält Informationen zum Erstellen von universellen Windows-Apps, mit denen Fotos, Videos oder Audiodateien aufgenommen, wiedergegeben oder bearbeitet werden können.
title: Audio, Video und Kamera
---

# Audio, Video und Kamera

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Dieser Abschnitt enthält Informationen zum Erstellen von universellen Windows-Apps, mit denen Fotos, Videos oder Audiodateien aufgenommen, wiedergegeben oder bearbeitet werden können.
 
| Thema                                                                                             | Beschreibung                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Aufnehmen von Fotos und Videos mit „CameraCaptureUI“](capture-photos-and-video-with-cameracaptureui.md) | Dieser Artikel beschreibt, wie Sie die [CameraCaptureUI](capture-photos-and-video-with-cameracaptureui.md)-Klasse zum Aufnehmen von Fotos oder Videos mit der in Windows integrierten Kamera-UI verwenden.                                                                                                            |
| [Aufnehmen von Fotos und Videos mit „MediaCapture“](capture-photos-and-video-with-mediacapture.md)       | Dieser Artikel beschreibt die Schritte zum Aufnehmen von Fotos und Videos mit der [MediaCapture](https://msdn.microsoft.com/library/windows/apps/br241124)-API sowie das Initialisieren und Herunterfahren von „MediaCapture“ und das Behandeln von Änderungen in der Geräteausrichtung.                                  |
| [Erkennen von Gesichtern in Bildern oder Videos](detect-and-track-faces-in-an-image.md)                         | In diesem Thema wird erläutert, wie sie die [FaceTracker](https://msdn.microsoft.com/library/windows/apps/dn974150)-Klasse verwenden, die für die Verfolgung von Gesichtern im zeitlichen Verlauf einer Sequenz von Videoframes optimiert ist.                                                                                                               |
| [Medienkompositionen und -bearbeitung](media-compositions-and-editing.md)                               | Die APIs in der [Microsoft Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197)-API.                                                                                                                                                                                 |
| [Bildverarbeitung](imaging.md)                                                                             | In diesem Artikel wird das Laden und Speichern von Bilddateien mit dem [SoftwareBitmap](https://msdn.microsoft.com/library/windows/apps/dn887358)-Objekt zum Darstellen von Bitmapbildern erläutert.                                                                                                                     |
| [Transcodieren von Mediendateien](transcode-media-files.md)                                                 | Mit den [Windows.Media.Transcoding](https://msdn.microsoft.com/library/windows/apps/br207105)-APIs können Sie Videodateien von einem Format in ein anderes transcodieren.                                                                                                                                |
| [Verarbeiten von Mediendateien im Hintergrund](process-media-files-in-the-background.md)                 | In diesem Artikel wird beschrieben, wie Sie den [MediaProcessingTrigger](https://msdn.microsoft.com/library/windows/apps/dn806005) und eine Hintergrundaufgabe verwenden, um Mediendateien im Hintergrund zu verarbeiten.                                                                                                       |
| [Medienwiedergabe mit „MediaSource“](media-playback-with-mediasource.md)                             | Die [MediaSource](https://msdn.microsoft.com/library/windows/apps/dn930905)-Klasse wird allgemein zum Verweisen auf Medien aus verschiedenen Quellen (etwa lokale Dateien oder Remotedateien) sowie zum Wiedergeben dieser Medien verwendet und macht ein gemeinsames Modell für den Mediendatenzugriff verfügbar – unabhängig vom zugrunde liegenden Medienformat.  |
| [Adaptives Streaming](adaptive-streaming.md)                                                       | In diesem Artikel wird beschrieben, wie Sie die Wiedergabe von Multimediainhalten für adaptives Streaming zu Apps für die universelle Windows-Plattform (UWP) hinzufügen können. Dieses Feature unterstützt derzeit die Wiedergabe von HTTP Live Streaming (HLS)- und Dynamic Streaming over HTTP (DASH)-Inhalten.                                          |
| [Hintergrundaudio](background-audio.md)                                                           | In diesem Artikel wird beschrieben, wie UWP-Apps erstellt werden, die Audio im Hintergrund wiedergeben.                                                                                                                                                                                                               |
| [Steuerelemente für den Systemmedientransport](system-media-transport-controls.md)                             | Mit der [SystemMediaTransportControls](https://msdn.microsoft.com/library/windows/apps/dn278677)-Klasse kann Ihre App die Steuerelemente für den Systemmedientransport verwenden, die in Windows integriert sind und die Metadaten aktualisieren, die die Steuerelemente zu den von der App aktuell wiedergegebenen Medien anzeigen. |
| [Medienumwandlung](media-casting.md)                                                                 | In diesem Artikel wird beschrieben, wie Sie Medien von einer universellen Windows-App für Remotegeräte umwandeln.                                                                                                                                                                                                       |
| [Audiodiagramme](audio-graphs.md)                                                                   | In diesem Artikel wird gezeigt, wie die APIs im [Windows.Media.Audio](https://msdn.microsoft.com/library/windows/apps/dn914341)-Namespace zum Erstellen von Audiodiagrammen für Audiorouting sowie Misch- und Verarbeitungsszenarien verwendet werden.                                                                            |
| [MIDI](midi.md)                                                                                   | In diesem Artikel wird beschrieben, wie Sie MIDI-Geräte (Musical Instrument Digital Interface) aufzählen und MIDI-Nachrichten in einer universellen Windows-App senden und empfangen.                                                                                                                                   |
| [Kameraunabhängige Taschenlampe](camera-independent-flashlight.md)                                 | In diesem Artikel wird beschrieben, wie Sie auf die Taschenlampe eines Geräts zugreifen und diese verwenden, sofern vorhanden. Die Taschenlampenfunktion wird unabhängig von der Kamera des Geräts und der Blitzfunktion der Kamera verwaltet.                                                                                                                 |
| [Unterstützte Codecs](supported-codecs.md)                                                           | In diesem Artikel wird aufgeführt, welche Audio- und Videocodecs und welche Formate für UWP-Apps unterstützt werden.                                                                                                                                                                                                                  |
| [PlayReady DRM](playready-client-sdk.md)                                                          | In diesem Abschnitt wird beschrieben, wie Sie Ihrer UWP-App (Universelle Windows-Plattform) PlayReady-geschützte Medieninhalte hinzufügen.                                                                                                                                                                                |
| [Verschlüsselte Medienerweiterung von PlayReady](playready-encrypted-media-extension.md)                     | In diesem Abschnitt wird beschrieben, wie Sie Ihre Web-App mit PlayReady ändern, um die Änderungen zu unterstützen, die gegenüber der Windows 8.1-Version in der Version für Windows 10 vorgenommen wurden.                                                                                                                                       |

 

 

 






<!--HONumber=Mar16_HO1-->


