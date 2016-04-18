---
ms.assetid: 0186EA01-8446-45BA-A109-C5EB4B80F368
description: Die AdvancedPhotoCapture-Klasse ermöglicht Ihnen die Aufnahme von HDR-Fotos (High Dynamic Range).
title: HDR-Fotoaufnahmen (High Dynamic Range)
---

# HDR-Fotoaufnahmen (High Dynamic Range)

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Die [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386)-Klasse ermöglicht Ihnen die Aufnahme von HDR-Fotos (High Dynamic Range). Mithilfe dieser API können Sie außerdem einen Referenzframe aus der HDR-Aufnahme abrufen, bevor die Verarbeitung des endgültigen Bilds abgeschlossen ist.

Die folgenden Artikel beziehen sich ebenfalls auf HDR-Aufnahmen:

-   Sie können den [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/dn948902) verwenden, damit das System den Inhalt des Vorschaudatenstrom der Medienaufnahme bewerten kann, um zu ermitteln, ob das Aufnahmeergebnis durch eine HDR-Verarbeitung verbessert werden kann. Weitere Informationen finden Sie unter [Szenenanalyse für Medienaufnahmen](scene-analysis-for-media-capture.md).

-   Verwenden Sie das [**HdrVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926680) für Videoaufnahmen mit dem integrierten Windows-HDR-Verarbeitungsalgorithmus. Weitere Informationen finden Sie unter [Steuerelemente des Aufnahmegeräts für Videoaufnahmen](capture-device-controls-for-video-capture.md).

-   Mit [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564) können Sie eine Abfolge von Fotos unter Verwendung unterschiedlicher Aufnahmeeinstellungen für jedes Foto aufnehmen und Ihren eigenen HDR- oder einen anderen Verarbeitungsalgorithmus implementieren. Weitere Informationen finden Sie unter [Variable Fotosequenz](variable-photo-sequence.md).

**Hinweis**
-   Das gleichzeitige Aufzeichnen von Video- und Fotoaufnahmen mit **AdvancedPhotoCapture** wird nicht unterstützt.

-   Die Verwendung des Kamerablitzes während der erweiterten Fotoaufnahme wird nicht unterstützt.

**Hinweis** Dieser Artikel baut auf Konzepten und Code auf, die bzw. der unter [Aufnehmen von Fotos und Videos mit MediaCapture](capture-photos-and-video-with-mediacapture.md), erläutert werden. Darin werden die Schritte für die Implementierung einer grundlegenden Foto- und Videoaufnahme beschrieben. Es wird empfohlen, dass Sie sich mit dem grundlegenden Muster für die Medienaufnahme in diesem Artikel vertraut machen, bevor Sie in fortgeschrittene Aufnahmeszenarien einsteigen. Bei dem Code in diesem Artikel wird davon ausgegangen, dass Ihre App bereits eine Instanz von MediaCapture aufweist, die ordnungsgemäß initialisiert wurde.

## Namespaces für die HDR-Fotoaufnahme

Um die HDR-Fotoaufnahme verwenden zu können, muss Ihre App die folgenden Namespaces zusätzlich zu den für die grundlegende Medienaufnahme erforderlichen Namespaces enthalten.

[!code-cs[HDRPhotoUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHDRPhotoUsing)]


## Ermitteln, ob die HDR-Fotoaufnahme auf dem aktuellen Gerät unterstützt wird

Das in diesem Artikel beschriebene HDR-Aufnahmeverfahren wird mithilfe des [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386)-Objekts ausgeführt. Nicht alle Geräte unterstützen die HDR-Aufnahme mit **AdvancedPhotoCapture**. Ermitteln Sie, ob das Gerät, auf dem Ihre App derzeit ausgeführt wird, dieses Verfahren unterstützt, indem Sie zunächst den [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) des **MediaCapture**-Objekts und anschließend die [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840)-Eigenschaft abrufen. Überprüfen Sie die [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844)-Sammlung des Videogerätecontrollers, um festzustellen, ob sie [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845) enthält. Wenn dies der Fall ist, wird die HDR-Aufnahme mit **AdvancedPhotoCapture** unterstützt.

[!code-cs[HdrSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHdrSupported)]

## Konfigurieren und Vorbereiten des AdvancedPhotoCapture-Objekts

Da Sie von mehreren Stellen innerhalb des Codes aus auf die [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386)-Instanz zugreifen müssen, deklarieren Sie eine Membervariable als Container für das Objekt.

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

Erstellen Sie in der App nach der Initialisierung des **MediaCapture**-Objekts ein [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837)-Objekt, und legen Sie den Modus auf [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845) fest. Rufen Sie die [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841)-Methode des [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840)-Objekts auf, und übergeben Sie das erstellte **AdvancedPhotoCaptureSettings**-Objekt.

Rufen Sie die [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403)-Methode des **MediaCapture**-Objekts auf, und übergeben Sie ein [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993)-Objekt, das den von der Aufnahme zu verwendenden Codierungstyp angibt. Die **ImageEncodingProperties**-Klasse enthält statische Methoden zum Erstellen der Bildcodierungen, die vom **MediaCapture**-Objekt unterstützt werden.

**PrepareAdvancedPhotoCaptureAsync** gibt das [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386)-Objekt zurück, mit dem Sie die Fotoaufnahme initiieren. Sie können dieses Objekt zum Registrieren von Handlern für die Ereignisse [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) und [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) verwenden, die später in diesem Artikel erläutert werden.

[!code-cs[CreateAdvancedCaptureAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureAsync)]

## Aufnehmen eines HDR-Fotos

Nehmen Sie ein HDR-Foto auf, indem Sie die [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388)-Methode des [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386)-Objekts aufrufen. Diese Methode gibt ein [**AdvancedCapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/mt181378)-Objekt zurück, das das aufgenommene Foto in seiner [**Frame**](https://msdn.microsoft.com/library/windows/apps/mt181382)-Eigenschaft angibt.

[!code-cs[CaptureHdrPhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureHdrPhotoAsync)]

**ConvertOrientationToPhotoOrientation** und **ReencodeAndSavePhotoAsync** sind Hilfsmethoden, die im Rahmen des grundlegenden Medienaufnahmeszenarios im Artikel [Aufnehmen von Fotos und Videos mit MediaCapture](capture-photos-and-video-with-mediacapture.md) beschrieben werden.

## Abrufen des optionalen Referenzframes

Im HDR-Verfahren werden mehrere Frames aufgenommen und nach Aufnahme aller Frames zu einem einzigen Bild zusammengestellt. Durch Behandeln des [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392)-Ereignisses können Sie auf einen Frame zugreifen, nachdem er aufgenommen wurde, aber bevor das gesamte HDR-Verfahren abgeschlossen ist. Dies ist jedoch nicht erforderlich, wenn Sie lediglich am endgültigen HDR-Fotoergebnis interessiert sind.

**Wichtig** [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) wird nicht auf Geräten ausgelöst, die Hardware HDR unterstützen und daher keine Referenzframes generieren. Ihre App sollte den Fall behandeln, in dem dieses Ereignis nicht ausgelöst wird.

Da der Referenzframe außerhalb des Kontexts des Aufrufs von **CaptureAsync** auftritt, steht ein Mechanismus zur Übergabe von Kontextinformationen an den **OptionalReferencePhotoCaptured**-Handler zur Verfügung. Erstellen Sie zunächst ein Objekt als Container für die Kontextinformationen. Sie können den Namen und den Inhalt dieses Objekts frei wählen. In diesem Beispiel wird ein Objekt definiert, das über Member zum Nachverfolgen des Dateinamens und der Kameraausrichtung der Aufnahme verfügt.

[!code-cs[AdvancedCaptureContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAdvancedCaptureContext)]

Erstellen Sie eine neue Instanz der Kontextobjekts, füllen Sie dessen Member, und übergeben Sie es an die Überladung von [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388), die ein Objekt als Parameter akzeptiert.

[!code-cs[CaptureWithContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureWithContext)]

Wandeln Sie im [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392)-Ereignishandler die [**Context**](https://msdn.microsoft.com/library/windows/apps/mt181405)-Eigenschaft des [**OptionalReferencePhotoCapturedEventArgs**](https://msdn.microsoft.com/library/windows/apps/mt181404)-Objekts in Ihre Kontextobjektklasse um. In diesem Beispiel wird der Dateiname geändert, um den Referenzframe vom endgültigen HDR-Bild zu unterscheiden. Anschließend wird die **ReencodeAndSavePhotoAsync**-Hilfsmethode aufgerufen, um das Bild zu speichern.

[!code-cs[OptionalReferencePhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOptionalReferencePhotoCaptured)]

## Empfangen einer Benachrichtigung nach Aufnahme aller Frames

Die HDR-Fotoaufnahme besteht aus zwei Schritten. Zuerst werden mehrere Frames aufgenommen, dann werden die Frames zum endgültigen HDR-Bild verarbeitet. Während der laufenden Aufnahme der HDR-Quellbilder können Sie keine weitere Aufnahmen starten. Sie können jedoch eine Aufnahme starten, wenn alle Frames aufgenommen wurden, die HDR-Nachverarbeitung aber noch nicht abgeschlossen ist. Das [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387)-Ereignis wird ausgelöst, wenn die HDR-Aufnahmen abgeschlossen sind, um Sie darüber zu informieren, dass Sie eine weitere Aufnahme starten können. In einem typischen Szenario wird die Aufnahmeschaltfläche in der Benutzeroberfläche deaktiviert, wenn die HDR-Aufnahme beginnt, und aktiviert, wenn **AllPhotosCaptured** ausgelöst wird.

[!code-cs[AllPhotosCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAllPhotosCaptured)]

## Bereinigen des AdvancedPhotoCapture-Objekts

Wenn die App alle Fotos aufgenommen hat, fahren Sie vor dem Löschen des **MediaCapture**-Objekts das [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386)-Objekt herunter, indem Sie [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/mt181391) aufrufen und die Membervariable auf NULL festlegen.

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]

## Verwandte Themen

* [Aufnehmen von Fotos und Videos mit MediaCapture](capture-photos-and-video-with-mediacapture.md)

<!--HONumber=Mar16_HO1-->


