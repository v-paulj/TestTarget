---
author: drewbatgit
ms.assetid: 84729E44-10E9-4D7D-8575-6A9D97467ECD
description: "In diesem Thema erfahren Sie, wie mit dem FaceDetector Gesichter in einem Bild erkannt werden. Der FaceTracker ist für die Verfolgung von Gesichtern im zeitlichen Verlauf einer Sequenz von Videoframes optimiert."
title: Erkennen von Gesichtern in Bildern oder Videos
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 66730fcbaad2e3e059f2972475625d278d235002

---

# Erkennen von Gesichtern in Bildern oder Videos

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


\[Einige Informationen beziehen sich auf die Vorabversion, die vor der kommerziellen Freigabe möglicherweise wesentlichen Änderungen unterliegt. Microsoft übernimmt keine Garantie, weder ausdrücklicher noch impliziter Art, für die hier bereitgestellten Informationen.\]

In diesem Thema erfahren Sie, wie mit dem [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) Gesichter in einem Bild erkannt werden. Der [**FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150) ist für die Verfolgung von Gesichtern im zeitlichen Verlauf einer Sequenz von Videoframes optimiert.

Ein alternatives Verfahren der Gesichtsverfolgung mit dem [**FaceDetectionEffect**](https://msdn.microsoft.com/library/windows/apps/dn948776) finden Sie unter [Szenenanalyse für die Medienaufnahme](scene-analysis-for-media-capture.md).

Der Code in diesem Artikel wurde aus den Beispielen [Basic Face Detection](http://go.microsoft.com/fwlink/p/?LinkId=620512&clcid=0x409) und [Basic Face Tracking](http://go.microsoft.com/fwlink/p/?LinkId=620513&clcid=0x409) übernommen und angepasst. Sie können diese Beispiele herunterladen, um den verwendeten Code im Kontext anzuzeigen oder das Beispiel als Ausgangspunkt für Ihre eigene App zu verwenden.

## Erkennen von Gesichtern in einem einzelnen Bild

Die [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129)-Klasse ermöglicht das Erkennen von einem oder mehreren Gesichtern in einem Standbild.

In diesem Beispiel werden APIs aus den folgenden Namespaces verwendet.

[!code-cs[FaceDetectionUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceDetectionUsing)]

Deklarieren Sie eine Klassenmembervariable für das [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129)-Objekt und für die Liste der [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123)-Objekte, die im Bild erkannt werden.

[!code-cs[ClassVariables1](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables1)]

Die Gesichtserkennung arbeitet mit einem [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358)-Objekt, das auf vielfältige Weise erstellt werden kann. In diesem Beispiel wird ein [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) verwendet, um dem Benutzer das Auswählen einer Bilddatei zu ermöglichen, in der Gesichter erkannt werden. Weitere Informationen zum Arbeiten mit Softwarebitmaps finden Sie unter [Bildverarbeitung](imaging.md).

[!code-cs[Dateiauswahl](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetPicker)]

Verwenden Sie die [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176)-Klasse, um die Bilddatei als **SoftwareBitmap** zu decodieren. Mit einem kleineren Bild erfolgt die Gesichtserkennung schneller. Daher empfiehlt es sich, das Quellbild auf eine geringere Größe zu skalieren. Dies kann während der Decodierung erfolgen, indem ein [**BitmapTransform**](https://msdn.microsoft.com/library/windows/apps/br226254)-Objekt erstellt wird, die Eigenschaften [**ScaledWidth**](https://msdn.microsoft.com/library/windows/apps/br226261) und [**ScaledHeight**](https://msdn.microsoft.com/library/windows/apps/br226260) festgelegt werden und das Objekt an den Aufruf von [**GetSoftwareBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn887332) übergeben wird, der die decodierte und skalierte **SoftwareBitmap** zurückgibt.

[!code-cs[Decodieren](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDecode)]

In der aktuellen Version unterstützt die **FaceDetector**-Klasse nur Bilder im Pixelformat Gray8 oder Nv12. Die **SoftwareBitmap**-Klasse stellt die [**Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362)-Methode bereit, die eine Bitmap in ein anderes Format umwandelt. In diesem Beispiel wird das Quellbild in das Pixelformat Gray8 umgewandelt, sofern es nicht bereits dieses Format aufweist. Sie können ggf. mithilfe der Methoden [**GetSupportedBitmapPixelFormats**](https://msdn.microsoft.com/library/windows/apps/dn974140) und [**IsBitmapPixelFormatSupported**](https://msdn.microsoft.com/library/windows/apps/dn974142) zur Laufzeit bestimmen, ob ein Pixelformat unterstützt wird, für den Fall, dass der Satz der unterstützten Formate in zukünftigen Versionen erweitert wird.

[!code-cs[Format](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFormat)]

Instanziieren Sie das **FaceDetector**-Objekt, indem Sie [**CreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn974132) und dann [**DetectFacesAsync**](https://msdn.microsoft.com/library/windows/apps/dn974134) aufrufen, und übergeben Sie die Bitmap, die auf eine geeignete Größe skaliert und in ein unterstütztes Pixelformat konvertiert wurde. Diese Methode gibt eine Liste von [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123)-Objekten zurück. 
            **ShowDetectedFaces** ist eine Hilfsmethode (unten dargestellt), die Quadrate um die Gesichter im Bild zeichnet.

[!code-cs[Erkennen](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDetect)]

Stellen Sie sicher, dass die Objekte gelöscht werden, die während der Gesichtserkennung erstellt wurden.

[!code-cs[Löschen](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDispose)]

Um das Bild anzuzeigen und Rahmen um die erkannten Gesichter zu zeichnen, fügen Sie der XAML-Seite ein [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)-Element hinzu.

[!code-xml[Canvas](./code/FaceDetection_Win10/cs/MainPage.xaml#SnippetCanvas)]

Definieren Sie Membervariablen, um die zu zeichnenden Quadrate zu formatieren.

[!code-cs[ClassVariables2](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables2)]

In der **ShowDetectedFaces**-Hilfsmethode wird ein neuer [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101) erstellt, und die Quelle wird auf eine [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) festgelegt, die aus der das Quellbild darstellenden **SoftwareBitmap** erstellt wird. Der Hintergrund des XAML **Canvas**-Steuerelements wird auf den Bildpinsel festgelegt.

Wenn die Liste der an die Hilfsmethode übergebenen Gesichter nicht leer ist, durchlaufen Sie die einzelnen Gesichter in der Liste, und bestimmen Sie mithilfe der [**FaceBox**](https://msdn.microsoft.com/library/windows/apps/dn974126)-Eigenschaft der [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123)-Klasse die Position und Größe des Rechtecks im Bild, das das Gesicht enthält. Da die Größe des **Canvas**-Steuerelements sehr wahrscheinlich nicht mit der Größe des Quellbilds übereinstimmt, sollten Sie die X- und Y-Koordinate sowie die Breite und Höhe der **FaceBox** mit einem Skalierungswert multiplizieren, der das Verhältnis der Quellbildgröße zur tatsächlichen Größe des **Canvas**-Steuerelements angibt.

[!code-cs[ShowDetectedFaces](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetShowDetectedFaces)]

## Verfolgen von Gesichtern in einer Framesequenz

Für die Gesichtserkennung in einem Video ist es effizienter, die [**FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150)-Klasse anstelle der [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129)-Klasse zu verwenden. Die Implementierungsschritte sind jedoch sehr ähnlich. Der **FaceTracker** optimiert den Erkennungsprozess anhand von Informationen über bereits verarbeitete Frames.

[!code-cs[FaceTrackingUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceTrackingUsing)]

Deklarieren Sie eine Klassenvariable für das **FaceTracker**-Objekt. In diesem Beispiel wird ein [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/br230587) verwendet, um die Gesichtsverfolgung für ein definiertes Intervall zu initiieren. Mit einem [SemaphoreSlim](https://msdn.microsoft.com/library/system.threading.semaphoreslim.aspx) wird sichergestellt, dass immer nur ein Gesichtsverfolgungsvorgang ausgeführt wird.

[!code-cs[ClassVariables3](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables3)]

Erstellen Sie zum Initialisieren des Gesichtsverfolgungsvorgangs ein neues **FaceTracker**-Objekt, indem Sie [**CreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn974151) aufrufen. Initialisieren Sie das gewünschte Timerintervall, und erstellen Sie dann den Timer. Immer wenn das angegebene Intervall verstrichen ist, wird die **ProcessCurrentVideoFrame**-Hilfsmethode aufgerufen.

[!code-cs[TrackingInit](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetTrackingInit)]

Die **ProcessCurrentVideoFrame**-Hilfsmethode wird vom Timer asynchron aufgerufen. Deshalb ruft die Methode zunächst die **Wait**-Methode des Semaphors auf, um festzustellen, ob derzeit ein Verfolgungsvorgang ausgeführt wird. Wenn dies der Fall ist, wird die Methode beendet, ohne dass versucht wird, Gesichter zu erkennen. Am Ende dieser Methode wird die **Release**-Methode des Semaphors aufgerufen, sodass mit dem nachfolgenden Aufruf von **ProcessCurrentVideoFrame** fortgefahren werden kann.

Die [**FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150) Klasse arbeitet mit [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917)-Objekten. Es gibt mehrere Möglichkeiten, ein **VideoFrame** zu erhalten, z. B. durch Aufzeichnen eines Vorschauframes aus einem derzeit ausgeführten [MediaCapture](capture-photos-and-video-with-mediacapture.md)-Objekt oder durch Implementieren der [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764784)-Methode von [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788). In diesem Beispiel wird die nicht definierte Hilfsmethode **GetLatestFrame**, die einen Videoframe zurückgibt, als Platzhalter für diesen Vorgang verwendet. Informationen zum Abrufen von Videoframes aus dem Vorschaudatenstrom eines derzeit ausgeführten Medienaufnahmegeräts finden Sie unter [Abrufen eines Vorschauframes](get-a-preview-frame.md).

**FaceDetector** unterstützt wie **FaceTracker** eine begrenzte Anzahl von Pixelformaten. In diesem Beispiel wird die Gesichtserkennung abgebrochen, wenn der bereitgestellte Frame nicht das Format Nv12 aufweist.

Rufen Sie [**ProcessNextFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn974157) auf, um eine Liste von [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123)-Objekten, die die Gesichter im Frame darstellen, abzurufen. Sobald Sie über die Liste der Gesichter verfügen, können Sie sie auf die gleiche Weise wie weiter oben für die Gesichtserkennung beschrieben anzeigen. Beachten Sie, dass alle UI-Aktualisierungen in einem Aufruf von [**CoredDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) erfolgen müssen, da die Hilfsmethode für die Gesichtsverfolgung nicht im UI-Thread aufgerufen wird.

[!code-cs[ProcessCurrentVideoFrame](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetProcessCurrentVideoFrame)]

## Verwandte Themen

* [Szenenanalyse für die Medienaufnahme](scene-analysis-for-media-capture.md)
* [Beispiel „Basic Face Detection“](http://go.microsoft.com/fwlink/p/?LinkId=620512&clcid=0x409)
* [Beispiel „Basic Face Tracking“](http://go.microsoft.com/fwlink/p/?LinkId=620513&clcid=0x409)
* [Aufnehmen von Fotos und Videos mit MediaCapture](capture-photos-and-video-with-mediacapture.md)



<!--HONumber=Jun16_HO4-->


