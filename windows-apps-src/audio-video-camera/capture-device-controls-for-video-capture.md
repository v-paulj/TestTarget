---
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: In diesem Artikel erfahren Sie, wie Sie Steuerelemente eines Videoaufnahmegeräts verwenden, um verbesserte Videoaufnahmeszenarien, einschließlich HDR-Video und Belichtungspriorität, zu ermöglichen.
title: Steuerelemente des Aufnahmegeräts für Videoaufnahmen
---

# Steuerelemente des Aufnahmegeräts für Videoaufnahmen

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


In diesem Artikel erfahren Sie, wie Sie Steuerelemente eines Videoaufnahmegeräts verwenden, um verbesserte Videoaufnahmeszenarien, einschließlich HDR-Video und Belichtungspriorität, zu ermöglichen.

Die in diesem Artikel beschriebenen Steuerelemente des Videoaufnahmegeräts werden alle mithilfe desselben Musters zu Ihrer App hinzugefügt. Überprüfen Sie zunächst, ob das Steuerelement auf dem aktuellen Gerät unterstützt wird, auf dem Ihre App ausgeführt wird. Wenn das Steuerelement unterstützt wird, legen Sie den gewünschten Modus für das Steuerelement fest. Wenn ein bestimmtes Steuerelement auf dem aktuellen Gerät nicht unterstützt wird, sollten Sie das UI-Element, über das der Benutzer das Feature aktivieren kann, deaktivieren oder ausblenden.

Alle in diesem Artikel beschriebenen Gerätesteuerelement-APIs sind Member des [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902)-Namespaces.

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

**Hinweis**  
Dieser Artikel baut auf Konzepten und Code auf, die unter [Aufnehmen von Fotos und Videos mit MediaCapture](capture-photos-and-video-with-mediacapture.md) erläutert werden. Darin werden die Schritte für die Implementierung einer grundlegenden Foto- und Videoaufnahme beschrieben. Es wird empfohlen, dass Sie sich mit dem grundlegenden Muster für die Medienaufnahme in diesem Artikel vertraut machen, bevor Sie in fortgeschrittene Aufnahmeszenarien einsteigen. Bei dem Code in diesem Artikel wird davon ausgegangen, dass Ihre App bereits eine Instanz von MediaCapture aufweist, die ordnungsgemäß initialisiert wurde.

## HDR-Video

Die Videofunktion „High Dynamic Range“ (HDR) bezieht sich auf die HDR-Verarbeitung des Videostreams des Aufnahmegeräts. Ermitteln Sie, ob HDR-Video unterstützt wird, indem Sie die [**HdrVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926682)-Eigenschaft überprüfen.

Das HDR-Videosteuerelement unterstützt drei Modi: ein, aus und automatisch. Dies bedeutet, dass das Gerät dynamisch ermittelt, ob die Medienaufnahme durch die HDR-Videoverarbeitung verbessert würde und HDR-Video aktiviert, wenn dies der Fall ist. Um festzustellen, ob ein bestimmter Modus auf dem aktuellen Gerät unterstützt wird, überprüfen Sie, ob die [**HdrVideoControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926683)-Auflistung den gewünschten Modus enthält.

Aktivieren oder deaktivieren Sie die HDR-Videoverarbeitung, in dem Sie [**HdrVideoControl.Mode**](https://msdn.microsoft.com/library/windows/apps/dn926681) auf den gewünschten Modus festlegen.

[!code-cs[SetHdrVideoMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetHdrVideoMode)]

## Belichtungspriorität

Wenn [**ExposurePriorityVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926644) aktiviert ist, werden die Videoframes des Aufnahmegeräts ausgewertet, um zu bestimmen, ob in dem Video ein Motiv unter schlechten Lichtverhältnissen aufgenommen wird. Wenn dies der Fall ist, verringert das Steuerelement die Bildfrequenz des aufgenommenen Videos, um die Belichtungszeit für jeden Frame zu erhöhen und die visuelle Qualität des aufgenommenen Videos zu verbessern.

Ermitteln Sie, ob das Steuerelement für die Belichtungspriorität auf dem aktuellen Gerät unterstützt wird, indem Sie die [**ExposurePriorityVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926647)-Eigenschaft überprüfen.

Aktivieren oder deaktivieren Sie das Steuerelement für die Belichtungspriorität, indem Sie [**ExposurePriorityVideoControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926646) auf den gewünschten Modus festlegen.

[!code-cs[EnableExposurePriority](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEnableExposurePriority)]

## Verwandte Themen

* [Aufnehmen von Fotos und Videos mit MediaCapture](capture-photos-and-video-with-mediacapture.md)
 

 






<!--HONumber=Mar16_HO1-->


