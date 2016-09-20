---
author: drewbatgit
ms.assetid: 
description: "In diesem Artikel wird beschrieben, wie Sie beim Aufnehmen von Fotos und Videos mit einer Hilfsklasse die Geräteausrichtung handhaben."
title: "Handhaben der Geräte- und Bildschirmausrichtung mit „MediaCapture“"
translationtype: Human Translation
ms.sourcegitcommit: 9f1d65d73bdf50697d75b0d57429aed66898e1b5
ms.openlocfilehash: eb6487e7f2c19a8227320c5a7f087e4b3c3c6270

---

# Handhaben der Geräte- und Bildschirmausrichtung mit „MediaCapture“
Wenn Ihre App ein Foto oder Video aufnimmt, das außerhalb der App angezeigt werden soll, z.B. in eine Datei auf dem Gerät des Benutzers gespeichert oder online freigegeben werden soll, ist es wichtig, dass das Bild mit den richtigen Ausrichtungsmetadaten codiert wird. Dadurch wird bei der Anzeige durch eine andere App oder ein anderes Gerät das Bild korrekt ausgerichtet. Die Bestimmung der korrekten Ausrichtungsdaten für eine Mediendatei kann eine komplexe Aufgabe sein, da verschiedene Variablen berücksichtigt werden müssen. Dazu zählen die Ausrichtung des Geräte-Chassis, die Ausrichtung der Anzeige und die Platzierung der Kamera (nach vorne oder nach hinten gerichtete Kamera). 

Um die Handhabung der Ausrichtung zu vereinfachen, empfehlen wir die Verwendung der Hilfsklasse **CameraRotationHelper**, deren vollständige Definition am Ende dieses Artikels bereitgestellt wird. Sie können diese Klasse dem Projekt hinzufügen und dann die Schritte zum Hinzufügen von Ausrichtungsunterstützung zur Kamera-App in diesem Artikel ausführen. Die Hilfsklasse erleichtert auch das Drehen der Steuerelemente in der Kamera-UI, damit sie aus Sicht des Benutzers korrekt gerendert werden.

> [!NOTE] 
> Dieser Artikel baut auf Konzepten und Code auf, die im Artikel [**Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“**](basic-photo-video-and-audio-capture-with-mediacapture.md) erläutert werden. Es wird empfohlen, dass Sie sich mit den grundlegenden Konzepten der Verwendung der **MediaCapture**-Klasse vertraut machen, bevor Sie die Ausrichtungsunterstützung zur App hinzufügen.

## In diesem Artikel verwendete Namespaces
Der Beispielcode in diesem Artikel verwendet APIs aus den folgenden Namespaces, die Sie in den Code einschließen sollten. 

[!code-cs[OrientationUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOrientationUsing)]

Der erste Schritt beim Hinzufügen der Ausrichtungsunterstützung zur App ist das Sperren der Anzeige, sodass diese sich beim Drehen des Geräts nicht automatisch dreht. Die automatische UI-Drehung eignet sich gut für die meisten Arten von Apps. Es ist jedoch für Benutzer nicht intuitiv, wenn sich die Kameravorschau dreht. Sperren Sie die Bildschirmausrichtung, indem Sie die [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayInformation.AutoRotationPreferences)-Eigenschaft auf [**DisplayOrientations.Landscape**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayOrientations) festlegen. 

[!code-cs[AutoRotationPreference](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAutoRotationPreference)]

## Nachverfolgen der Position des Kamerageräts
Um die richtige Ausrichtung für aufgenommene Medien zu berechnen, muss die App die Position des Kamerageräts am Chassis ermitteln. Fügen Sie eine boolesche Membervariable hinzu, um nachzuverfolgen, ob es sich um eine externe Kamera handelt, z.B. eine USB-Webcam. Fügen Sie eine weitere boolesche Variable hinzu, um nachzuverfolgen, ob die Vorschau gespiegelt werden soll. Dies ist bei der Verwendung einer nach vorne gerichteten Kamera der Fall. Fügen Sie außerdem eine Variable zum Speichern eines **DeviceInformation**-Objekts hinzu, das die ausgewählte Kamera darstellt.

[!code-cs[CameraDeviceLocationBools](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCameraDeviceLocationBools)]
[!code-cs[DeclareCameraDevice](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareCameraDevice)]

## Auswählen eines Kamerageräts und Initialisieren des MediaCapture-Objekts
Im Artikel [**Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“**](basic-photo-video-and-audio-capture-with-mediacapture.md) wird beschrieben, wie Sie das **MediaCapture**-Objekt mit nur wenigen Codezeilen initialisieren. Um die Ausrichtung der Kamera zu unterstützen, fügen wir dem Initialisierungsprozess weitere Schritte hinzu.

Rufen Sie zuerst [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation.FindAllAsync) auf, und übergeben Sie die Geräteauswahl [**DeviceClass.VideoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceClass), um eine Liste aller verfügbaren Videoaufzeichnungsgeräte abzurufen. Wählen Sie dann das erste Gerät in der Liste aus, für das die Bereichsposition der Kamera bekannt ist und das mit dem angegebenen Wert übereinstimmt, in diesem Beispiel eine nach vorne gerichtete Kamera. Wird keine Kamera im gewünschten Bereich gefunden, wird die erste oder standardmäßig verfügbare Kamera verwendet.

Wenn ein Kameragerät gefunden wird, wird ein neues [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings)-Objekt erstellt, und die [**VideoDeviceId**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.VideoDeviceId)-Eigenschaft wird auf das ausgewählte Gerät festgelegt. Erstellen Sie als Nächstes das MediaCapture-Objekt, rufen Sie [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.InitializeAsync) auf, und übergeben Sie das Einstellungsobjekt, um dem System mitzuteilen, dass die ausgewählte Kamera verwendet werden soll.

Überprüfen Sie abschließend, ob der ausgewählte Gerätebereich null oder unbekannt ist. In diesem Fall ist die Kamera extern. Dies bedeutet, dass ihre Drehung ohne Bezug auf die Drehung des Geräts ist. Wenn der Bereich bekannt ist und sich an der Vorderseite des Geräte-Chassis befindet, wissen wir, dass die Vorschau gespiegelt werden soll. Die Variable zur Nachverfolgung ist also festgelegt.

[!code-cs[InitMediaCaptureWithOrientation](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitMediaCaptureWithOrientation)]
## Initialisieren der CameraRotationHelper-Klasse

Wir beginnen nun mit der Verwendung der **CameraRotationHelper**-Klasse. Deklarieren Sie eine Klassenmembervariable zum Speichern des Objekts. Rufen Sie den Konstruktor auf, und übergeben Sie die Gehäuseposition der ausgewählten Kamera. Die Hilfsklasse verwendet diese Informationen, um die richtige Ausrichtung für aufgenommene Medien, den Vorschaustream und die Benutzeroberfläche zu berechnen. Registrieren Sie einen Handler für das **OrientationChanged**-Ereignis der Hilfsklasse, das ausgelöst wird, wenn die Ausrichtung der Benutzeroberfläche oder des Vorschaustreams aktualisiert werden muss.

[!code-cs[DeclareRotationHelper](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareRotationHelper)]

[!code-cs[InitRotationHelper](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitRotationHelper)]

## Hinzufügen von Ausrichtungsdaten zum Kameravorschau-Stream
Das Hinzufügen der richtigen Ausrichtung zu den Metadaten des Vorschaustreams hat keinen Einfluss auf die Anzeige der Vorschau für Benutzer, erleichtert jedoch die richtige Codierung von aus dem Vorschaustream erfassten Frames durch das System.

Starten Sie die Kameravorschau durch Aufrufen von [**MediaCapture.StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.StartPreviewAsync). Überprüfen Sie zuvor die Membervariable, um festzustellen, ob die Vorschau gespiegelt werden soll (für eine nach vorne gerichtete Kamera). Wenn dies der Fall ist, legen Sie die [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.FlowDirection)-Eigenschaft von [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.CaptureElement), das in diesem Beispiel den Namen *PreviewControl* hat, auf [**FlowDirection.RightToLeft**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FlowDirection) fest. Rufen Sie nach dem Starten der Vorschau die Hilfsmethode **SetPreviewRotationAsync** auf, um die Drehung der Vorschau festzulegen. Es folgt die Implementierung dieser Methode.

[!code-cs[StartPreviewWithRotationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewWithRotationAsync)]

Wir legen die Vorschaudrehung in einer separaten Methode fest, sodass sie bei Änderung der Telefonausrichtung aktualisiert werden kann, ohne den Vorschaustream zu initialisieren. Wenn es sich um eine externe Kamera handelt, wird keine Aktion ausgeführt. Andernfalls wird die **CameraRotationHelper**-Methode **GetCameraPreviewOrientation** aufgerufen und gibt die richtige Ausrichtung für den Vorschaustream zurück. 

Zum Festlegen der Metadaten werden die Vorschaustreameigenschaften durch Aufrufen von [**VideoDeviceController.GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.VideoDeviceController.GetMediaStreamProperties) abgerufen. Erstellen Sie als Nächstes die GUID, die das Media Foundation Transform (MFT)-Attribut für die Drehung des Videostreams darstellt. In C++ können Sie die Konstante [**MF_MT_VIDEO_ROTATION**](https://msdn.microsoft.com/en-us/library/windows/desktop/hh162880.aspx) verwenden, während Sie in C# den GUID-Wert manuell angeben müssen. 

Fügen Sie dem Datenstromeigenschaftenobjekt einen Eigenschaftswert hinzu, und geben Sie die GUID als Schlüssel und die Vorschaudrehung als Wert an. Diese Eigenschaft erwartet Werte in Einheiten von Grad entgegen dem Uhrzeigersinn, sodass mithilfe der **CameraRotationHelper**-Methode **ConvertSimpleOrientationToClockwiseDegrees** der einfache Ausrichtungswert konvertiert wird. Rufen Sie abschließend [**SetEncodingPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.SetEncodingPropertiesAsync(Windows.Media.Capture.MediaStreamType,Windows.Media.MediaProperties.IMediaEncodingProperties,Windows.Media.MediaProperties.MediaPropertySet)) auf, um die neue Drehungseigenschaft auf den Stream anzuwenden.

[!code-cs[SetPreviewRotationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSetPreviewRotationAsync)]

Fügen Sie als Nächstes den Handler für das **CameraRotationHelper.OrientationChanged**-Ereignis hinzu. Dieses Ereignis übergibt ein Argument, das Ihnen mitteilt, ob der Vorschaustream gedreht werden muss. Wenn die Ausrichtung des Geräts nach oben oder unten geändert wurde, ist dieser Wert „false“. Wenn die Vorschau gedreht werden muss, rufen Sie **SetPreviewRotationAsync** auf, die zuvor definiert wurde.

Aktualisieren Sie als Nächstes im **OrientationChanged**-Ereignishandler bei Bedarf die Benutzeroberfläche. Rufen Sie die aktuelle empfohlene UI-Ausrichtung durch Aufrufen von **GetUIOrientation** aus der Hilfsklasse ab, und konvertieren Sie den Wert in Grad im Uhrzeigersinn für XAML-Transformationen. Erstellen Sie eine [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.RotateTransform) aus dem Ausrichtungswert, und legen Sie die [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.RenderTransform)-Eigenschaft der XAML-Steuerelemente fest. Je nach dem UI-Layout müssen Sie möglicherweise zusätzliche Anpassungen zum einfachen Drehen der Steuerelemente vornehmen. Beachten Sie außerdem, dass alle Aktualisierungen der Benutzeroberfläche im UI-Thread vorgenommen werden müssen. Daher sollten Sie diesen Code innerhalb eines Aufrufs von [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Core.CoreDispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority,Windows.UI.Core.DispatchedHandler)) platzieren.  

[!code-cs[HelperOrientationChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetHelperOrientationChanged)]

## Aufnehmen eines Fotos mit Ausrichtungsdaten
Im Artikel [**Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“**](basic-photo-video-and-audio-capture-with-mediacapture.md) wird erläutert, wie Sie ein Foto in einer Datei aufnehmen, indem Sie zuerst in einen In-Memory-Datenstrom aufnehmen und dann mithilfe eines Decoders die Bilddaten aus dem Datenstrom lesen und mithilfe eines Encoders die Bilddaten in eine Datei transcodieren. Aus der **CameraRotationHelper**-Klasse abgerufene Ausrichtungsdaten können während des Transcodierungsvorgangs der Bilddatei hinzugefügt werden.

Im folgenden Beispiel wird ein Foto in einem [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.Streams.InMemoryRandomAccessStream) mit einem Aufruf von [**CapturePhotoToStreamAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.CapturePhotoToStreamAsync) aufgenommen, und ein [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder) wird aus dem Stream erstellt. Als Nächstes wird eine [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFile) erstellt und geöffnet, um einen [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.Streams.IRandomAccessStream) zum Schreiben in die Datei abzurufen. 

Vor dem Transcodieren der Datei wird die Ausrichtung des Fotos aus der Hilfsklassenmethode **GetCameraCaptureOrientation** abgerufen. Diese Methode gibt ein [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Sensors.SimpleOrientation)-Objekt zurück, das mit der Hilfsmethode **ConvertSimpleOrientationToPhotoOrientation** in ein [**PhotoOrientation**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.FileProperties.PhotoOrientation)-Objekt konvertiert wird. Als Nächstes wird ein neues **BitmapPropertySet**-Objekt erstellt und eine Eigenschaft hinzugefügt, wobei der Schlüssel „System.Photo.Orientation“ und der Wert die Fotoausrichtung ist, ausgedrückt als **BitmapTypedValue**. „System.Photo.Orientation“ ist eine der vielen Windows-Eigenschaften, die als Metadaten einer Bilddatei hinzugefügt werden können. Eine Liste aller fotobezogenen Eigenschaften finden Sie unter [**Windows-Eigenschaften – Foto**](https://msdn.microsoft.com/en-us/library/windows/desktop/ff516600). Weitere Informationen zum Arbeiten mit Metadaten in Bildern finden Sie unter [**Bildmetadaten**](image-metadata.md).

Schließlich wird der Eigenschaftensatz, der die Ausrichtungsdaten enthält, für den Encoder mit einem Aufruf von [**SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/) festgelegt, und das Bild wird mit einem Aufruf von [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.FlushAsync) transcodiert.

[!code-cs[CapturePhotoWithOrientation](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCapturePhotoWithOrientation)]

## Aufnehmen eines Videos mit Ausrichtungsdaten
Die allgemeine Videoaufnahme wird im Artikel [**Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“**](basic-photo-video-and-audio-capture-with-mediacapture.md) beschrieben. Das Hinzufügen von Ausrichtungsdaten zur Codierung des aufgenommenen Videos erfolgt mit dem gleichen Verfahrens wie weiter oben im Abschnitt zum Hinzufügen von Ausrichtungsdaten zum Vorschaustream beschrieben.

Im folgenden Beispiel wird eine Datei erstellt, in die das aufgenommene Video geschrieben wird. Ein MP4-Codierungsprofil wird mithilfe der statischen [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile.CreateMp4)-Methode erstellt. Die richtige Ausrichtung für das Video wird aus der **CameraRotationHelper**-Klasse mit einem Aufruf von **GetCameraCaptureOrientation** abgerufen. Da die Drehungseigenschaft erfordert, dass die Ausrichtung in Grad gegen den Uhrzeigersinn ausgedrückt wird, wird die **ConvertSimpleOrientationToClockwiseDegrees**-Hilfsmethode zum Konvertieren des Ausrichtungswerts aufgerufen. Erstellen Sie als Nächstes die GUID, die das Media Foundation Transform (MFT)-Attribut für die Drehung des Videostreams darstellt. In C++ können Sie die Konstante [**MF_MT_VIDEO_ROTATION**](https://msdn.microsoft.com/en-us/library/windows/desktop/hh162880.aspx) verwenden, während Sie in C# den GUID-Wert manuell angeben müssen. Fügen Sie dem Datenstromeigenschaftenobjekt einen Eigenschaftswert hinzu, und geben Sie die GUID als Schlüssel und die Drehung als Wert an. Rufen Sie schließlich [**StartRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.StartRecordToStorageFileAsync(Windows.Media.MediaProperties.MediaEncodingProfile,Windows.Storage.IStorageFile)) auf, um die mit Ausrichtungsdaten codierte Videoaufnahme zu beginnen.

[!code-cs[StartRecordingWithOrientationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartRecordingWithOrientationAsync)]

## Vollständiger Code für CameraRotationHelper
Der folgende Codeausschnitt führt den vollständigen Code für die **CameraRotationHelper**-Klasse auf, mit der die Hardwareausrichtungssensoren verwaltet, die richtigen Ausrichtungswerte für Fotos und Videos berechnet und Hilfsmethoden zum Konvertieren zwischen den unterschiedlichen Darstellungsformen der Ausrichtung bereitgestellt werden, die von verschiedenen Windows-Features verwendet werden. Wenn Sie die im oben genannten Artikel gezeigten Anleitungen befolgen, können Sie diese Klasse dem Projekt in der vorliegenden Form hinzufügen, ohne Änderungen vornehmen zu müssen. Natürlich können Sie den folgenden Code an die Anforderungen des jeweiligen Szenarios anpassen.

Diese Hilfsklasse verwendet den [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Sensors.SimpleOrientationSensor) des Geräts zum Ermitteln der aktuellen Ausrichtung des Geräte-Chassis und die [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayInformation)-Klasse zum Ermitteln der aktuellen Ausrichtung der Anzeige. Alle diese Klassen bieten Ereignisse, die ausgelöst werden, wenn sich die aktuelle Ausrichtung ändert. Die Seite, auf der sich das Aufnahmegerät befindet – nach vorne gerichtet, nach hinten gerichtet oder extern –, wird verwendet, um festzustellen, ob der Vorschaustream gespiegelt werden soll. Darüber hinaus wird mithilfe der von einigen Geräten unterstützten [**EnclosureLocation.RotationAngleInDegreesClockwise**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.EnclosureLocation.RotationAngleInDegreesClockwise)-Eigenschaft die Ausrichtung ermittelt, mit der die Kamera auf dem Chassis montiert wird.

Mit den folgenden Methoden können empfohlene Ausrichtungswerte für die angegebenen Kamera-App-Aufgaben abgerufen werden:

* **GetUIOrientation** – Gibt die vorgeschlagene Ausrichtung für Kamera-UI-Elemente zurück.
* **GetCameraCaptureOrientation** – Gibt die vorgeschlagene Ausrichtung für die Codierung in Bildmetadaten zurück.
* **GetCameraPreviewOrientation** – Gibt die vorgeschlagene Ausrichtung für den Vorschaustream zum Bereitstellen einer natürlicheren Benutzererfahrung zurück.

[!code-cs[CameraRotationHelperFull](./code/SimpleCameraPreview_Win10/cs/CameraRotationHelper.cs#SnippetCameraRotationHelperFull)]



## Verwandte Themen

* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 







<!--HONumber=Aug16_HO3-->


