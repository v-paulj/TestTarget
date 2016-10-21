---
author: drewbatgit
ms.assetid: 05E418B4-5A62-42BD-BF66-A0762216D033
description: In diesem Thema wird das Abrufen eines einzelnen Vorschauframes aus dem Vorschaustream der Medienaufnahme beschrieben.
title: Abrufen eines Vorschauframes
translationtype: Human Translation
ms.sourcegitcommit: e19fa2a574e6824941c89db1db1e7e69f9e38ae9
ms.openlocfilehash: d8d5780672592b1888a9c894dcc3ed58ebc2be36

---

# Abrufen eines Vorschauframes

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \].

In diesem Thema wird das Abrufen eines einzelnen Vorschauframes aus dem Vorschaustream der Medienaufnahme beschrieben.

> [!NOTE] 
> Dieser Artikel baut auf Konzepten und Codebeispielen auf, die unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md) erläutert werden. Dort werden die Schritte für die Implementierung einer grundlegenden Foto- und Videoaufnahme beschrieben. Wir empfehlen Ihnen, sich mit dem grundlegenden Medienaufnahmemuster in diesem Artikel vertraut zu machen, bevor Sie sich komplexeren Aufnahmeszenarien zuwenden. Voraussetzung für den Code in diesem Artikel ist, dass Ihre App bereits über eine Instanz von MediaCapture verfügt, die ordnungsgemäß initialisiert wurde, und Sie ein [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) mit einem aktiven Videovorschaustream besitzen.

Neben den Namespaces, die für eine einfache Medienaufzeichnung erforderlich sind, erfordert das Aufzeichnen eines Vorschauframes den folgenden Namespace.

[!code-cs[PreviewFrameUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewFrameUsing)]

Wenn Sie einen Vorschauframe anfordern, können Sie das Format angeben, in dem Sie den Frame empfangen möchten, indem Sie ein [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917)-Objekt im gewünschten Format erstellen. Dieses Beispiel erstellt einen Videoframe mit der gleichen Auflösung wie der Vorschaustream durch Aufrufen von [**VideoDeviceController.GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211995) und Angeben von [**MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640) zum Anfordern der Eigenschaften für den Vorschaustream. Die Breite und Höhe des Vorschaustreams werden zum Erstellen des neuen Videoframes verwendet.

[!code-cs[CreateFormatFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateFormatFrame)]

Wenn das [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124)-Objekt initialisiert ist, und Sie einen aktiven Vorschaustream besitzen, rufen Sie [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926711) auf, um einen Vorschauframe abzurufen. Übergeben Sie den Videoframe, den Sie im letzten Schritt erstellt haben, um das Format des zurückgegebenen Frames anzugeben.

[!code-cs[GetPreviewFrameAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewFrameAsync)]

Rufen Sie eine [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358)-Darstellung des Vorschauframes durch Zugriff auf die [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn930926)-Eigenschaft des [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917)-Objekts ab. Informationen zum Speichern, Laden und Ändern von Softwarebitmaps finden Sie unter [Imaging](imaging.md).

[!code-cs[GetPreviewBitmap](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewBitmap)]

Sie können auch eine [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505)-Darstellung des Vorschauframes abrufen, wenn Sie das Bild mit Direct3D-APIs verwenden möchten.

[!code-cs[GetPreviewSurface](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewSurface)]

> [!IMPORTANT]
> Die Eigenschaft [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn930926) oder die Eigenschaft [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn930920) des zurückgegebenen **VideoFrame** ist möglicherweise Null, abhängig davon, wie Sie **GetPreviewFrameAsync** aufrufen, und von dem Gerät, auf dem Ihre App ausgeführt wird.

> - Wenn Sie die Überladung von [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926713) aufrufen, die ein **VideoFrame**-Argument akzeptiert, dann ist für den zurückgegebenen **VideoFrame** der **SoftwareBitmap**-Wert nicht Null, und die Eigenschaft **Direct3DSurface** ist Null.
> - Wenn Sie auf einem Gerät, das eine Direct3D-Oberfläche verwendet, um den Frame intern darzustellen, die Überladung von [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926712) aufrufen, das keine Argumente besitzt, ist die Eigenschaft **Direct3DSurface** nicht Null und die Eigenschaft **SoftwareBitmap** ist Null.
> - Wenn Sie auf einem Gerät, das keine Direct3D-Oberfläche verwendet, um den Frame intern darzustellen, die Überladung von [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926712) aufrufen, das keine Argumente besitzt, ist die Eigenschaft **SoftwareBitmap** nicht Null und die Eigenschaft **Direct3DSurface** ist Null.

Ihre App sollte stets nach einem Null-Wert suchen, bevor versucht wird, Vorgänge für Objekte auszuführen, die von den Eigenschaften **SoftwareBitmap** oder **Direct3DSurface** zurückgegeben werden.

Wenn Sie den Vorschauframe nicht mehr benötigen, rufen Sie auf jeden Fall die zugehörige [**Close**](https://msdn.microsoft.com/library/windows/apps/dn930918)-Methode auf (Dispose-Methode in C#), um die vom Frame verwendeten Ressourcen freizugeben. Alternativ können Sie auch das Muster **using** verwenden, durch das das Objekt automatisch entfernt wird.

[!code-cs[CleanUpPreviewFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpPreviewFrame)]

## Verwandte Themen

* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 







<!--HONumber=Aug16_HO3-->


