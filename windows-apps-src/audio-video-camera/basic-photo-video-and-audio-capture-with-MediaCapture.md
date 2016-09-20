---
author: drewbatgit
ms.assetid: 
description: In diesem Artikel wird beschrieben, wie Sie Fotos und Videos mit der MediaCapture-Klasse am einfachsten aufnehmen.
title: "Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“"
translationtype: Human Translation
ms.sourcegitcommit: 0c7355d442cd650f110042d5e01f51becbb51d0e
ms.openlocfilehash: 19a546d4778d0edfbc5ca2e3acf0ded084445958

---

# Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \].

In diesem Artikel wird beschrieben, wie Sie Fotos und Videos mit der [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture)-Klasse am einfachsten aufnehmen. Die **MediaCapture**-Klasse stellt einen leistungsfähigen Satz von APIs bereit, der eine Steuerung der Aufnahmepipeline auf unterster Ebene sowie fortgeschrittene Aufnahmeszenarien ermöglicht. Dieser Artikel soll Ihnen jedoch helfen, Ihre App schnell und einfach durch grundlegende Medienaufnahmefunktionen zu erweitern. Informationen zu den von **MediaCapture** bereitgestellten Features finden Sie unter [**Kamera**](camera.md).

Wenn Sie einfach ein Foto oder Video aufnehmen möchten, ohne weitere Features für die Medienaufnahme hinzuzufügen, oder wenn Sie keine eigene Kamera-UI erstellen möchten, können Sie die [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CameraCaptureUI)-Klasse verwenden. So können Sie einfach die in Windows integrierte Kamera-App starten, um die bereits aufgenommene Foto- oder Videodatei zu empfangen. Weitere Informationen finden Sie unter [**Aufnehmen von Fotos und Videos mit der in Windows integrierten Kamera-UI**](capture-photos-and-video-with-cameracaptureui.md)


## Hinzufügen von Funktionsdeklarationen zum App-Manifest

Damit Ihre App auf die Kamera eines Geräts zugreifen kann, müssen Sie deklarieren, dass die App *webcam*- und *microphone*-Gerätefunktionen verwendet. Wenn Sie aufgenommene Fotos und Videos in der Bild- oder Videobibliothek des Benutzers speichern möchten, müssen Sie auch die Funktionen *picturesLibrary* und *videosLibrary* deklarieren.

**So fügen Sie dem App-Manifest Funktionen hinzu**

1.  Öffnen Sie in MicrosoftVisual Studio im **Projektmappen-Explorer** den Designer für das Anwendungsmanifest, indem Sie auf das Element **package.appxmanifest** doppelklicken.
2.  Wählen Sie die Registerkarte **Funktionen** aus.
3.  Aktivieren Sie die Kontrollkästchen für **Webcam** und **Mikrofon**.
4.  Für den Zugriff auf die Bibliothek „Bilder und Videos“ aktivieren Sie die Kontrollkästchen für **Bildbibliothek** und **Videobibliothek**.


## Initialisieren des MediaCapture-Objekts
Alle in diesem Artikel beschriebenen Aufnahmemethoden erfordern als ersten Schritt die Initialisierung des [ **MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture)-Objekts. Dazu wird erst der Konstruktor und dann [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.InitializeAsync) aufgerufen. Da der Zugriff auf das **MediaCapture**-Objekt von mehreren Stellen in der App erfolgt, deklarieren Sie eine Klassenvariable als Container für das Objekt.  Implementieren Sie einen Handler für das [**Failed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.Failed)-Ereignis des **MediaCapture**-Objekts, damit Sie bei einem fehlerhaften Aufnahmevorgang benachrichtigt werden.

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

[!code-cs[InitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitMediaCapture)]

## Einrichten der Kameravorschau
Fotos, Videos und Audiodateien können auch mit **MediaCapture** aufgezeichnet werden, ohne die Kameravorschau anzuzeigen. Normalerweise ist der Vorschaudatenstrom jedoch erwünscht, damit der Benutzer sehen kann, was er aufzeichnet. Zum Aktivieren einiger **MediaCapture**-Features wie Autofokus, automatische Belichtung und automatischer Weißabgleich ist es außerdem erforderlich, dass der Vorschaudatenstrom aktiv ist. Informationen zum Einrichten der Kameravorschau finden Sie unter [**Anzeigen der Kameravorschau**](simple-camera-preview-access.md).

## Aufnehmen eines Fotos in „SoftwareBitmap“
Die [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap)-Klasse wurde in Windows 10 eingeführt, um über mehrere Features hinweg eine einheitliche Bilddarstellung zu gewährleisten. Wenn Sie ein Foto aufzeichnen und dieses dann direkt in Ihrer App verwenden (z. B. in XAML anzeigen) möchten, sollte Sie es in **SoftwareBitmap** statt in einer Datei aufzeichnen. Sie können das Bild später immer noch auf einem Datenträger speichern.

Nachdem Sie das **MediaCapture**-Objekt initialisiert haben, können Sie ein Foto mithilfe der [**LowLagPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture)-Klasse in **SoftwareBitmap** aufzeichnen. Rufen Sie eine Instanz dieser Klasse ab, indem Sie [**PrepareLowLagPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagPhotoCaptureAsync) aufrufen und ein [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties)-Objekt übergeben, das das zu verwendende Bildformat angibt. [Mit **CreateUncompressed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties.CreateUncompressed) wird eine nicht komprimierte Codierung mit dem angegebenen Pixelformat erstellt. Nehmen Sie ein Foto durch Aufrufen von [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture.CaptureAsync) auf. Dadurch wird ein [**CapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedPhoto)-Objekt zurückgegeben. Rufen Sie **SoftwareBitmap** ab, indem Sie erst auf die [**Frame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedPhoto.Frame)-Eigenschaft und dann auf die [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame.SoftwareBitmap)-Eigenschaft zugreifen.

Sie können auch mehrere Fotos erfassen, indem Sie **CaptureAsync** wiederholt aufrufen. Nachdem die Aufzeichnung abgeschlossen ist, rufen Sie [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.FinishAsync) auf, um die **LowLagPhotoCapture**-Sitzung zu schließen und die zugeordneten Ressourcen freizugeben. Wenn Sie nach dem Aufrufen von **FinishAsync** mit dem Aufzeichnen von Fotos beginnen möchten, müssen Sie [**PrepareLowLagPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagPhotoCaptureAsync) erneut aufrufen, um die Aufzeichnungssitzung erneut zu initialisieren, bevor Sie [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture.CaptureAsync) aufrufen.

[!code-cs[CaptureToSoftwareBitmap](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToSoftwareBitmap)]

Weitere Informationen zum Arbeiten mit dem **SoftwareBitmap**-Objekt, z. B. wie Sie ein Objekt in einer XAML-Seite anzeigen, finden Sie unter [**Erstellen, Bearbeiten und Speichern von Bitmapbildern**](imaging.md).

## Aufnehmen eines Fotos in einer Datei
Bei einer normalen Foto-App wird ein aufgenommenes Foto auf einem Datenträger oder in der Cloud gespeichert. Dabei müssen der Datei Metadaten hinzugefügt werden, z. B. die Ausrichtung des Fotos. Das folgende Beispiel zeigt, wie Sie ein Foto in einer Datei aufnehmen. Sie können später immer noch ein **SoftwareBitmap**-Objekt aus der Bilddatei erstellen. 

Bei dem in diesem Beispiel dargestellten Verfahren wird das Foto in einen In-Memory-Datenstrom aufgenommen und dann vom Datenstrom in eine Datei auf einem Datenträger codiert. Im Beispiel wird [**GetLibraryAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageLibrary.GetLibraryAsync) verwendet, um die Bildbibliothek des Benutzers abzurufen, und dann die [**SaveFolder**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageLibrary.SaveFolder)-Eigenschaft, um einen Referenz-Standardspeicherordner abzurufen. Denken Sie daran, Ihrem App-Manifest die Funktion **Bildbibliothek** hinzuzufügen, um auf diesen Ordner zugreifen zu können. [Mit **CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFolder.CreateFileAsync) wird eine neue [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFile) erstellt, in der das Foto gespeichert wird.

Erstellen Sie eine [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.Streams.InMemoryRandomAccessStream)-Klasse, und rufen Sie [**CapturePhotoToStreamAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.CapturePhotoToStreamAsync) auf, um ein Foto in den Datenstrom aufzunehmen. Dabei werden der Datenstrom und ein [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties)-Objekt übergeben, in dem das zu verwendende Bildformat angegeben ist. Sie können benutzerdefinierte Codierungseigenschaften erstellen, indem Sie das Objekt selbst initialisieren. Die Klasse stellt jedoch statische Methoden wie [**ImageEncodingProperties.CreateJpeg**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties.CreateJpeg) für allgemeine Codierungsformate bereit. Als Nächstes erstellen Sie einen Dateidatenstrom zur Ausgabedatei, indem Sie [ **OpenAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFile.OpenAsync) aufrufen. Erstellen Sie [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder), um das Bild aus dem In-Memory-Datenstrom zu decodieren, und erstellen Sie dann [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder), um das Bild durch Aufrufen von [**CreateForTranscodingAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.CreateForTranscodingAsync) in eine Datei zu codieren.

Optional können Sie ein [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapPropertySet)-Objekt erstellen und dann [**SetPropertiesAsync**](https://msdn.microsoft.com/en-us/library/windows/apps/br226252.aspx) für den Bildcodierer aufrufen, um die Fotometadaten in die Bilddatei aufzunehmen. Weitere Informationen über Codierungseigenschaften finden Sie unter [**Bildmetadaten**](image-metadata.md). Bei den meisten Foto-Apps kommt es darauf an, dass die Geräteausrichtung richtig behandelt wird. Weitere Informationen finden Sie unter [**Handhaben der Geräte- und Bildschirmausrichtung mit „MediaCapture“**](handle-device-orientation-with-mediacapture.md).

Rufen Sie zuletzt [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.FlushAsync) für das Codiererobjekt auf, um das Foto vom In-Memory-Datenstrom in die Datei zu transcodieren.

[!code-cs[CaptureToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToFile)]

Weitere Informationen zum Arbeiten mit Dateien und Ordnern finden Sie unter [**Dateien, Ordner und Bibliotheken**](https://msdn.microsoft.com/windows/uwp/files/index).

## Aufnehmen eines Videos
Mithilfe der [**LowLagMediaRecording**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording)-Klasse fügen Sie Ihrer App schnell eine Videoaufnahme hinzu. Deklarieren Sie zuerst eine Klassenvariable für das Objekt.

[!code-cs[LowLagMediaRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetLowLagMediaRecording)]

Erstellen Sie als Nächstes ein **StorageFile**-Objekt, in dem das Video gespeichert wird. Um die Videobibliothek des Benutzers wie im Beispiel gezeigt zu speichern, müssen Sie Ihrem App-Manifest die Funktion **Videobibliothek** hinzufügen. Rufen Sie [**PrepareLowLagRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagRecordToStorageFileAsync) auf, um die Medienaufzeichnung zu initialisieren. Dabei übergeben Sie die Speicherdatei und ein [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile)-Objekt, in dem die Codierung für das Video angegeben wird. Von der Klasse werden statische Methoden wie [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile.CreateMp4) zum Erstellen allgemeiner Videocodierungsprofile bereitgestellt.

Rufen Sie zuletzt [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StartAsync) auf, um mit der Videoaufnahme zu beginnen.

[!code-cs[StartVideoCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartVideoCapture)]

Zum Beenden der Videoaufnahme rufen Sie [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopAsync) auf.

[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

Sie können **StartAsync** und **StopAsync** weiterhin aufrufen, um zusätzliche Videos aufzunehmen. Nachdem die Videoaufnahme abgeschlossen ist, rufen Sie [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.FinishAsync) auf, um die Aufzeichnungssitzung zu löschen und die zugehörigen Ressourcen zu bereinigen. Nach diesem Aufruf müssen Sie **PrepareLowLagRecordToStorageFileAsync** erneut aufrufen, um die Aufzeichnungssitzung vor dem Aufrufen von **StartAsync** erneut zu initialisieren.

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]

Bei der Videoaufnahme sollten Sie einen Handler für das [**RecordLimitationExceeded**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.RecordLimitationExceeded)-Ereignis des **MediaCapture**-Objekts registrieren. Dieses Ereignis wird vom Betriebssystem ausgelöst, wenn Sie den Grenzwert für eine einzelne Aufnahme (derzeit drei Stunden) überschreiten. Schließen Sie die Aufnahme im Ereignishandler ab, indem Sie [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopAsync) aufrufen.

[!code-cs[RecordLimitationExceeded](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceeded)]

[!code-cs[RecordLimitationExceededHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceededHandler)]

## Anhalten und Fortsetzen der Videoaufnahme
Sie können eine Videoaufnahme anhalten und dann ohne Erstellung einer separaten Ausgabedatei fortsetzen, indem Sie [**PauseAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.PauseAsync) und dann [**ResumeAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.ResumeAsync) aufrufen.

[!code-cs[PauseRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseRecordingSimple)]

[!code-cs[ResumeRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeRecordingSimple)]

Ab Windows 10, Version 1607, können Sie eine Videoaufnahme anhalten und den letzten Frame empfangen, der vor dem Anhalten der Aufnahme erfasst wurde. Anschließend können Sie diesen Frame in der Kameravorschau überlagern, damit der Benutzer die Kamera auf den angehaltenen Frame ausrichten kann, bevor die Aufnahme fortgesetzt wird. Durch Aufruf von [**PauseWithResultAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.PauseWithResultAsync) wird ein [**MediaCapturePauseResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapturePauseResult)-Objekt zurückgegeben. Die [**LastFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapturePauseResult.LastFrame)-Eigenschaft ist ein [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.VideoFrame)-Objekt, das den letzten Frame darstellt. Um den Frame in XAML anzuzeigen, rufen Sie die **SoftwareBitmap**-Darstellung des Videoframes ab. Derzeit werden nur Bilder im BGRA8-Format mit prämultipliziertem oder leerem Alphakanal unterstützt. Zum Abrufen des richtigen Formats müssen Sie deshalb [**Convert**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap.Covert) aufrufen.  Erstellen Sie ein neues [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource)-Objekt, und rufen Sie [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource.SetBitmapAsync) auf, um es zu initialisieren. Legen Sie zuletzt die **Source**-Eigenschaft des XAML-Steuerelements [**Image**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Image) fest, um das Bild anzuzeigen. Damit dies funktioniert, muss Ihr Bild auf das **CaptureElement**-Steuerelement ausgerichtet sein und einen Transparenzwert kleiner als 1 aufweisen. Da Sie die Benutzeroberfläche nur im UI-Thread ändern können, müssen Sie den Aufruf in [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Core.CoreDispatcher.RunAsync) ausführen.

Durch **PauseWithResultAsync** wird auch die Dauer der Videoaufnahme im vorangehenden Segment zurückgegeben, falls Sie die Gesamtaufnahmezeit nachverfolgen müssen.

[!code-cs[PauseCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseCaptureWithResult)]

Wenn Sie die Aufnahme fortsetzen, können Sie die Quelle des Bilds auf NULL festlegen, um es auszublenden.

[!code-cs[ResumeCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeCaptureWithResult)]

Sie können beim Beenden des Videos auch einen Ergebnisframe abrufen, indem Sie [**StopWithResultAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopWithResultAsync) aufrufen.


## Audioaufnahme 
Sie können Ihrer App schnell eine Audioaufnahme hinzufügen, indem Sie die oben bei der Videoaufnahme beschriebene Vorgehensweise verwenden. Im folgenden Beispiel wird eine **StorageFile** im Anwendungsdatenordner erstellt. Rufen Sie [**PrepareLowLagRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagRecordToStorageFileAsync) auf, um die Aufzeichnungssitzung zu initialisieren. Dabei werden die Datei und eine [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile)-Klasse übergeben, die im Beispiel von der statischen Methode [**CreateMp3**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile.CreateMp3) generiert wird. Um mit der Aufnahme zu beginnen, rufen Sie [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StartAsync) auf.

[!code-cs[StartAudioCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartAudioCapture)]


Rufen Sie [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoSequenceCapture.StopAsync) auf, um die Audioaufnahme zu beenden.

[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

Sie können **StartAsync** und **StopAsync** mehrfach aufrufen, um mehrere Audiodateien aufzuzeichnen. Nachdem die Audioaufnahme abgeschlossen ist, rufen Sie [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.FinishAsync) auf, um die Aufzeichnungssitzung zu löschen und die zugehörigen Ressourcen zu bereinigen. Nach diesem Aufruf müssen Sie **PrepareLowLagRecordToStorageFileAsync** erneut aufrufen, um die Aufzeichnungssitzung vor dem Aufrufen von **StartAsync** erneut zu initialisieren.

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]

## Verwandte Themen

* [Kamera](camera.md)
* [Aufnehmen von Fotos und Videos mit der in Windows integrierten Kamera-UI](capture-photos-and-video-with-cameracaptureui.md)
* [Handhaben der Geräte- und Bildschirmausrichtung mit „MediaCapture“](handle-device-orientation-with-mediacapture.md)
* [Erstellen, Bearbeiten und Speichern von Bitmapbildern](imaging.md)
* [Dateien, Ordner und Bibliotheken](https://msdn.microsoft.com/windows/uwp/files/index)




<!--HONumber=Aug16_HO3-->


