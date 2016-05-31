---
author: drewbatgit
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: Dieser Artikel beschreibt, wie Sie die „CameraCaptureUI“-Klasse zum Aufnehmen von Fotos oder Videos mit der in Windows integrierten Kamera-UI verwenden.
title: Aufnehmen von Fotos und Videos mit „CameraCaptureUI“
---

# Aufnehmen von Fotos und Videos mit „CameraCaptureUI“

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Dieser Artikel beschreibt, wie Sie die „CameraCaptureUI“-Klasse zum Aufnehmen von Fotos oder Videos mit der in Windows integrierten Kamera-UI verwenden. Dieses Feature ist benutzerfreundlich und ermöglicht, dass die App ein vom Benutzer aufgenommenes Fotos oder Video mit nur wenigen Codezeilen abruft.

Wenn Ihr Szenario eine zuverlässigere Steuerung des Aufnahmevorgangs auf unterster Ebene erfordert, sollten Sie das [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124)-Objekt verwenden und Ihre eigene Aufzeichnungsoberfläche implementieren. Weitere Informationen finden Sie unter [Aufnehmen von Fotos und Videos mit MediaCapture](capture-photos-and-video-with-mediacapture.md).

## Aufnehmen eines Fotos mit CameraCaptureUI

Um die Kameraaufnahme-UI zu verwenden, schließen Sie den [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738)-Namespace in Ihr Projekt ein. Um Dateivorgänge mit der zurückgegebenen Bilddatei auszuführen, schließen Sie [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346) ein.

[!code-cs[UsingCaptureUI](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingCaptureUI)]

Um ein Foto aufzunehmen, erstellen Sie ein neues [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030)-Objekt. Durch Verwendung der [**PhotoSettings**](https://msdn.microsoft.com/library/windows/apps/br241058)-Eigenschaft des Objekts können Sie Eigenschaften für das zurückgegebene Foto angeben, z. B. das Bildformat des Fotos. Standardmäßig kann der Benutzer über die Kameraaufnahme-UI das Foto zuschneiden, bevor es zurückgegeben wird. Dies kann aber mit der [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042)-Eigenschaft deaktiviert werden. In diesem Beispiel wird [**CroppedSizeInPixels**](https://msdn.microsoft.com/library/windows/apps/br241044) festgelegt, um anzufordern, dass das zurückgegebene Bild 200 x 200 Pixel hat.

**Hinweis**  Das Zuschneiden von Bildern im „CameraCaptureUI“-Objekt wird für Geräte in der Mobilgerätefamilie nicht unterstützt. Der Wert der [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042)-Eigenschaft wird ignoriert, wenn Ihre App auf diesen Geräten ausgeführt wird.

Rufen Sie [**CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057) auf, und geben Sie [**CameraCaptureUIMode.Photo**](https://msdn.microsoft.com/library/windows/apps/br241040) an, um festzulegen, dass ein Foto aufgenommen werden soll. Die Methode gibt eine [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Instanz mit dem Bild zurück, wenn die Aufnahme erfolgreich ist. Wenn der Benutzer die Aufnahme abbricht, ist das zurückgegebene Objekt „null“.

[!code-cs[CapturePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCapturePhoto)]

Nachdem Sie die **StorageFile**-Instanz mit dem aufgenommenen Foto erstellt haben, können Sie ein [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358)-Objekt erstellen, das mit mehreren unterschiedlichen Features der universellen Windows-App verwendet werden kann.

Sie sollten zunächst den [**Windows.Graphics.Imaging**](https://msdn.microsoft.com/library/windows/apps/br226400)-Namespace in Ihr Projekt einschließen.

[!code-cs[UsingSoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmap)]

Rufen Sie [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227116) auf, um einen Stream aus der Bilddatei abzurufen. Rufen Sie [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/br226182) auf, um einen Bitmap-Decoder für den Stream abzurufen. Rufen Sie dann [**GetSoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887332) auf, um eine **SoftwareBitmap**-Darstellung des Bilds abzurufen.

[!code-cs[SoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmap)]

Um das Bild in der Benutzeroberfläche anzuzeigen, deklarieren Sie ein [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752)-Steuerelement auf der XAML-Seite.

[!code-xml[ImageControl](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetImageControl)]

Um die Software-Bitmap in der XAML-Seite zu verwenden, schließen Sie den [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258)-Namespace in Ihr Projekt ein.

[!code-cs[UsingSoftwareBitmapSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmapSource)]

Das **Image**-Steuerelement erfordert, dass die Bildquelle das BGRA8-Format mit vormultipliziertem oder gar keinem Alphaformat aufweist. Rufen Sie daher die statische Methode [**SoftwareBitmap.Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362) auf, um eine neue Software-Bitmap mit dem gewünschten Format zu erstellen. Erstellen Sie als Nächstes ein neues [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854)-Objekt, und rufen Sie [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856) auf, um die Software-Bitmap der Quelle zuzuweisen. Legen Sie abschließend die [**Source**](https://msdn.microsoft.com/library/windows/apps/br242760)-Eigenschaft des **Image**-Steuerelements fest, um das aufgenommene Foto in der Benutzeroberfläche anzuzeigen.

[!code-cs[SetImageSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetImageSource)]

## Aufnehmen eines Videos mit CameraCaptureUI

Um ein Video aufzunehmen, erstellen Sie ein neues [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030)-Objekt. Durch Verwendung der [**VideoSettings**](https://msdn.microsoft.com/library/windows/apps/br241059)-Eigenschaft des Objekts können Sie Eigenschaften für das zurückgegebene Video angeben, z. B. das Format des Videos.

Rufen Sie [**CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057) auf, und geben Sie [**Video**](https://msdn.microsoft.com/library/windows/apps/br241059) an, um festzulegen, dass ein Video aufgenommen werden soll. Die Methode gibt eine [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Instanz mit dem Video zurück, wenn die Aufnahme erfolgreich ist. Wenn der Benutzer die Aufnahme abbricht, ist das zurückgegebene Objekt „null“.

[!code-cs[CaptureVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCaptureVideo)]

Wofür Sie die aufgenommene Videodatei verwenden, ist von dem Szenario für Ihre App abhängig. Im weiteren Verlauf dieses Artikels wird gezeigt, wie Sie schnell eine Medienkomposition aus einem oder mehreren aufgenommenen Videos erstellen und diese in der Benutzeroberfläche anzeigen.

Fügen Sie zunächst der XAML-Seite ein [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)-Steuerelement hinzu, in dem die Videokomposition angezeigt werden soll.

[!code-xml[MediaElement](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetMediaElement)]

Fügen Sie dem Projekt die Namespaces [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565) und [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) hinzu.


[!code-cs[UsingMediaComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingMediaComposition)]

Deklarieren Sie Membervariablen für ein [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646)-Objekt und eine [**MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn282716)-Klasse, die für die Lebensdauer der Seite im Bereich beibehalten werden soll.

[!code-cs[DeclareMediaComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetDeclareMediaComposition)]

Bevor Sie Videos aufnehmen, sollten Sie eine neue Instanz der **MediaComposition**-Klasse erstellen.

[!code-cs[InitComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetInitComposition)]

Erstellen Sie mit der von der Kameraaufnahme-UI zurückgegebenen Videodatei einen neuen [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596), indem Sie [**MediaClip.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652607) aufrufen. Fügen Sie den Medienclip der [**Clips**](https://msdn.microsoft.com/library/windows/apps/dn652648)-Sammlung der Komposition hinzu.

Rufen Sie [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674) auf, um das **MediaStreamSource**-Objekt aus der Komposition zu erstellen.

[!code-cs[AddToComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetAddToComposition)]

Zum Schluss legen Sie die Streamquelle so fest, dass die [**SetMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn299029)-Methode des Medienelements verwendet wird, um die Komposition in der Benutzeroberfläche anzuzeigen.

[!code-cs[SetMediaElementSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetMediaElementSource)]

Sie können weiter Videoclips aufzeichnen und diese der Komposition hinzufügen. Weitere Informationen zu Medienkompositionen finden Sie unter [Medienkompositionen und -bearbeitung](media-compositions-and-editing.md).

**Hinweis**  
Dieser Artikel ist für Windows 10-Entwickler bestimmt, die Apps für die universelle Windows-Plattform (UWP) schreiben. Wenn Sie für Windows 8.x oder Windows Phone 8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Verwandte Themen

* [Aufnehmen von Fotos und Videos mit MediaCapture](capture-photos-and-video-with-mediacapture.md)
* [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030)
 

 






<!--HONumber=May16_HO2-->


