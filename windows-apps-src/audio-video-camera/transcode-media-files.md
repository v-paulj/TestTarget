---
author: drewbatgit
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: "Mit den Windows.Media.Transcoding-APIs können Sie Videodateien von einem Format in ein anderes transcodieren."
title: Transcodieren von Mediendateien
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 06c452291f10acd35dde9659c08a386ea38fa90a

---

# Transcodieren von Mediendateien

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Mit den [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105)-APIs können Sie Videodateien in ein anderes Format transcodieren.

Beim *Transcodieren* wird eine digitale Mediendatei (etwa eine Video- oder Audiodatei) in ein anderes Format konvertiert. Dies geschieht in der Regel durch Decodieren und erneutes Codieren der Datei. Beispiel: Sie konvertieren eine Windows Media-Datei in MP4, um sie auf einem Gerät wiederzugeben, das das MP4-Format unterstützt. Oder Sie können eine Videodatei mit hoher Auflösung in eine niedrigere Auflösung konvertieren. In diesem Fall kann die neu codierte Datei den gleichen Codec wie die Originaldatei verwenden, sie besitzt jedoch ein anderes Codierungsprofil.

## Einrichten des Projekts für die Transcodierung

Zusätzlich zu den Namespaces, auf die von der Standardprojektvorlage verwiesen wird, müssen Sie auf die folgenden Namespaces verweisen, um Mediendateien mit dem Code in diesem Artikel transcodieren zu können:

[!code-cs[Using](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## Auswählen der Quell- und Zieldateien

Die Art, wie Ihre App die Quell- und Zieldateien für die Transcodierung ermittelt, hängt von der Implementierung ab. In diesem Beispiel werden eine [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)-Klasse und eine [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)-Klasse verwendet, um Benutzern die Auswahl einer Quell- und Zieldatei zu ermöglichen.

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## Erstellen eines Mediencodierungsprofils

Das Codierungsprofil enthält die Einstellungen, die die Codierungsart der Zieldatei festlegen. Hier stehen die meisten Optionen zum Transcodieren einer Datei zur Verfügung.

Die [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026)-Klasse stellt statische Methoden zum Erstellen vordefinierter Codierungsprofile bereit:

-   WAV
-   AAC-Audio (M4A)
-   MP3-Audio
-   Windows Media Audio (WMA)
-   AVI
-   MP4-Video (H.264-Video und AAC-Audio)
-   Windows Media Video (WMV)

Die ersten vier Profile in der Liste enthalten nur Audio. Die anderen drei Profile enthalten Video und Audio.

Der folgende Code erstellt ein Profil für MP4-Video:

[!code-cs[TranscodeMediaProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeMediaProfile)]

Die statische [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/hh701078)-Methode erstellt ein MP4-Codierungsprofil. Der Parameter für diese Methode gibt die Zielauflösung für das Video an. In diesem Fall bedeutet [**VideoEncodingQuality.hd720p**](https://msdn.microsoft.com/library/windows/apps/hh701290): 1280 x 720 Pixel mit 30 Bildern pro Sekunde. („720p“ steht für 720 progressive Scanlinien pro Frame.) Dieses Muster gilt auch für die anderen Methoden zum Erstellen vordefinierter Profile.

Alternativ können Sie mit der [**MediaEncodingProfile.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701047)-Methode ein Profil erstellen, das einer vorhandenen Mediendatei entspricht. Wenn Sie die genauen gewünschten Codierungseinstellungen kennen, können Sie auch ein neues [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026)-Objekt erstellen und die Profildetails ausfüllen.

## Transcodieren der Datei

Erstellen Sie zum Transcodieren der Datei ein neues [**MediaTranscoder**](https://msdn.microsoft.com/library/windows/apps/br207080)-Objekt, und rufen Sie die [**MediaTranscoder.PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936)-Methode auf. Übergeben Sie die Quelldatei, die Zieldatei und das Codierungsprofil. Rufen Sie dann die [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946)-Methode für das [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941)-Objekt auf, das mit dem asynchronen Transcodierungsvorgang zurückgegeben wurde.

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## Reaktion auf Transcodierungsfortschritt

Sie können Reaktionsereignisse registrieren, wenn sich der Fortschritt der asynchronen [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946)-Klasse ändert. Diese Ereignisse sind Teil des asynchronen Programmierungsframeworks für Apps für die universelle Windows-Plattform (UWP) und nicht für die Transcodierungs-API spezifisch.

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]

 

 







<!--HONumber=Jun16_HO4-->


