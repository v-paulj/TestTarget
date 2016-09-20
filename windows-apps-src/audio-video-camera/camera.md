---
author: drewbatgit
ms.assetid: 
description: "In diesem Artikel werden die für UWP-Apps verfügbaren Kamera-Features aufgeführt, sowie die Links zu den Anleitungen für ihre Verwendung."
title: Kamera
translationtype: Human Translation
ms.sourcegitcommit: f9f85359bd24e0a642bf9cbe3c76f6bfac7866f8
ms.openlocfilehash: 8759a7cdb1d516f9c88f866887861c2f28085b5b

---

# Kamera

Dieser Abschnitt enthält Richtlinien für das Erstellen von UWP (Universelle Windows-Plattform)-Apps, die die Kamera oder das Mikrofon verwenden, um Fotos, Videos oder Audiodateien zu erfassen.

##Verwenden der in Windows integrierten Kamera-UI
| Thema                                                                                             | Beschreibung                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Aufnehmen von Fotos und Videos mit der in Windows integrierten Kamera-UI](capture-photos-and-video-with-cameracaptureui.md) | Zeigt, wie Sie die [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CameraCaptureUI)-Klasse zum Aufnehmen von Fotos oder Videos mit der in Windows integrierten Kamera-UI verwenden. Wenn Sie einfach den Benutzer dazu befähigen möchten, ein Foto oder Video aufzuzeichnen und das Ergebnis an Ihre App zurückzugeben, ist dies die schnellste und einfachste Möglichkeit dafür.  |
##Grundlegende MediaCapture-Funktionen
| Thema                                                                                             | Beschreibung                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Anzeigen der Kameravorschau](simple-camera-preview-access.md) | Beschreibt, wie Sie in einer UWP-App innerhalb einer XAML-Seite den Kameravorschau-Stream schnell anzeigen können. |
| [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md) | Zeigt die einfachste Möglichkeit zum Aufnehmen von Fotos und Videos mit der [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture)-Klasse. Die **MediaCapture**-Klasse stellt einen robusten Satz von APIs bereit, der eine Steuerung der Aufnahme-Pipeline auf unterster Ebene sowie fortgeschrittene Aufnahme-Szenarien ermöglicht. Dieser Artikel soll Ihnen jedoch helfen, Ihrer App die grundlegende Medienaufnahme schnell und problemlos hinzuzufügen. |
| [Kamera-UI-Features für mobile Geräte](camera-ui-features-for-mobile-devices.md) | Beschreibt, wie Sie spezielle Kamera-UI-Features nutzen, die nur auf mobilen Geräten vorhanden sind.  |
                                                                                                               
##Erweiterte MediaCapture-Funktionen   
                                                                                                               
| Thema                                                                                             | Beschreibung                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Handhaben der Geräte- und Bildschirmausrichtung mit „MediaCapture“](handle-device-orientation-with-mediacapture.md) | Beschreibt, wie Sie beim Aufnehmen von Fotos und Videos mit einer Hilfsklasse die Geräteausrichtung handhaben. | 
| [Entdecken und Auswählen von Kamerafunktionen mit Kameraprofilen](camera-profiles.md) | Beschreibt, wie Sie Kameraprofile verwenden, um die Funktionen verschiedener Videoaufzeichnungsgeräte zu ermitteln und zu verwalten. Dazu gehören Aufgaben, wie z. B. das Auswählen von Profilen, die bestimmte Auflösungen oder Bildfrequenzen unterstützen, von Profilen, die gleichzeitigen Zugriff auf mehrere Kameras unterstützen, sowie von Profilen, die HDR unterstützen. |
| [Festlegen von Format, Auflösung und Bildfrequenz für „MediaCapture“](set-media-encoding-properties.md) | Beschreibt, wie Sie die [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011)-Schnittstelle verwenden, um die Auflösung und Bildfrequenz des Kameravorschau-Datenstroms sowie von aufgenommenen Fotos und Videos festzulegen. Außerdem wird beschrieben, wie Sie sicherstellen, dass das Seitenverhältnis des Vorschaudatenstroms mit dem Seitenverhältnis der aufgenommenen Medien übereinstimmt. |
| [Aufnehmen von HDR-Fotos und Fotos bei schlechten Lichtverhältnissen](high-dynamic-range-hdr-photo-capture.md) | Beschreibt, wie Sie die [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture)-Klasse verwenden, um HDR (High Dynamic Range)-Fotos und Fotos bei schlechten Lichtverhältnissen aufzunehmen. |
| [Manuelle Kamerasteuerelemente für Foto- und Videoaufnahmen](capture-device-controls-for-photo-and-video-capture.md) | Beschreibt, wie Sie manuelle Gerätesteuerelemente verwenden, um erweiterte Foto- und Videoaufnahmeszenarien (einschließlich optischer Bildstabilisierung und fließendem Zoom) zu ermöglichen. |
| [Manuelle Kamerasteuerelemente für die Videoaufnahme](capture-device-controls-for-video-capture.md) | Beschreibt, wie Sie manuelle Gerätesteuerelemente verwenden, um erweiterte Videoaufnahmeszenarien, einschließlich HDR-Video und Belichtungspriorität, zu ermöglichen.  |
| [Videostabilisierungseffekt für die Videoaufnahme](effects-for-video-capture.md) | Beschreibt, wie Sie den Videostabilisierungseffekt verwenden.  |
| [Szenenanalyse für „MediaCapture“](scene-analysis-for-media-capture.md) | Beschreibt, wie Sie den [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.SceneAnalysisEffect) und den [**FaceDetectionEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.FaceDetectionEffect) verwenden, um den Inhalt des Vorschaudatenstroms der Medienaufnahme zu analysieren.  |
| [Aufnehmen einer Fotosequenz mit „VariablePhotoSequence“](variable-photo-sequence.md) | Beschreibt, wie Sie eine variable Fotosequenz aufnehmen, um mehrere Frames von Bildern in schneller Folge aufzunehmen und jeden Frame so zu konfigurieren, dass unterschiedliche Einstellungen für Fokus, Blitz, ISO, Belichtung und Belichtungskorrektur verwendet werden.  |
| [Verarbeiten von Medienframes mit „MediaFrameReader“](process-media-frames-with-mediaframereader.md) | Beschreibt, wie Sie [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) mit [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) verwenden, um Medienframes aus einer oder mehreren verfügbaren Quellen abzurufen. Hierzu zählen Farb-, Tiefen- und Infrarotkameras sowie Audiogeräte und sogar benutzerdefinierte Framequellen (etwa für Skeletal-Tracking-Frames). Dieses Feature wurde für die Verwendung von Apps entwickelt, die Medienframes in Echtzeit verarbeiten, wie beispielsweise Augmented-Reality- und Tiefenkamera-Apps.  |
| [Abrufen eines Vorschauframes](get-a-preview-frame.md) | Beschreibt, wie Sie einen einzelnen Vorschauframe aus dem Vorschaustream der Medienaufnahme abrufen.  |                                                                                                   


## UWP-App-Beispiele für Kamera

* [Beispiel für Gesichtserkennung durch Kamera](http://go.microsoft.com/fwlink/p/?LinkID=619486&clcid=0x409)
* [Beispiel für Kamera-Vorschauframe](http://go.microsoft.com/fwlink/p/?LinkID=620516&clcid=0x409)
* [Beispiel für Kamera-HDR](http://go.microsoft.com/fwlink/p/?LinkID=620517&clcid=0x409)
* [Beispiel für manuelle Kamerasteuerelemente](http://go.microsoft.com/fwlink/p/?LinkID=627611&clcid=0x409)
* [Beispiel für Kameraprofil](http://go.microsoft.com/fwlink/p/?LinkID=620518&clcid=0x409)
* [Beispiel für Kameraauflösung](http://go.microsoft.com/fwlink/p/?LinkID=624252&clcid=0x409)
* [Kamera-Starterkit](http://go.microsoft.com/fwlink/p/?LinkID=619479&clcid=0x409)
* [Beispiel für Videostabilisierung für Kamera](http://go.microsoft.com/fwlink/p/?LinkID=620519&clcid=0x409)

## Verwandte Themen

* [Audio, Video und Kamera](index.md)
 

 







<!--HONumber=Aug16_HO3-->


