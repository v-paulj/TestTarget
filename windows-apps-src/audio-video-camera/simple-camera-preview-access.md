---
author: drewbatgit
ms.assetid: 9BA3F85A-970F-411C-ACB1-B65768B8548A
description: Dieser Artikel beschreibt, wie Sie in einer UWP (Universelle Windows-Plattform)-App innerhalb einer XAML-Seite schnell den Datenstrom der Kameravorschau anzeigen.
title: Anzeigen der Kameravorschau
translationtype: Human Translation
ms.sourcegitcommit: 599e7dd52145d695247b12427c1ebdddbfc4ffe1
ms.openlocfilehash: 8330ee43089207faab5f6f72e2ac7b32aada72ce

---

# Anzeigen der Kameravorschau

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \].

Dieser Artikel beschreibt, wie Sie in einer UWP (Universelle Windows-Plattform)-App innerhalb einer XAML-Seite schnell den Datenstrom der Kameravorschau anzeigen. Zum Erstellen einer App, die Fotos und Videos mit der Kamera erfasst, müssen Sie Aufgaben wie das Behandeln der Geräte- und Kameraausrichtung oder das Festlegen von Codierungsoptionen für die erfasste Datei durchführen. Für einige App-Szenarien möchten Sie vielleicht einfach nur den Vorschaudatenstrom von der Kamera anzeigen, ohne sich Gedanken über diese anderen Überlegungen machen zu müssen. Dieser Artikel zeigt, wie dies mit einem Minimum an Code möglich ist. Hinweis: Sie sollten den Vorschaudatenstrom immer ordnungsgemäß beenden, wenn Sie damit fertig sind; führen Sie dazu die folgenden Schritte aus.

Informationen zum Schreiben einer Kamera-App, die Fotos oder Videos aufnimmt, finden Sie unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md).

## Hinzufügen von Funktionsdeklarationen zum App-Manifest

Damit Ihre App auf die Kamera eines Geräts zugreifen kann, müssen Sie deklarieren, dass die App *webcam*- und *microphone*-Gerätefunktionen verwendet. 

**Hinzufügen von Funktionen zum App-Manifest**

1.  Öffnen Sie in MicrosoftVisual Studio im **Projektmappen-Explorer** den Designer für das Anwendungsmanifest, indem Sie auf das Element **package.appxmanifest** doppelklicken.
2.  Wählen Sie die Registerkarte **Funktionen** aus.
3.  Aktivieren Sie die Kontrollkästchen für **Webcam** und **Mikrofon**.

## Hinzufügen eines CaptureElement zu einer Seite

Mithilfe eines [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) können Sie den Vorschaudatenstrom innerhalb der XAML-Seite anzeigen.

[!code-xml[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]



## Verwenden von MediaCapture zum Starten des Vorschaudatenstroms

Das [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124)-Objekt ist die Schnittstelle Ihrer App mit der Kamera des Geräts. Diese Klasse ist ein Mitglied des Namespace „Windows.Media.Capture“. Im Beispiel in diesem Artikel werden neben den in der Standard-Projektvorlage enthaltenen APIs auch APIs aus den Namespaces [**Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691) und [System.Threading.Tasks](https://msdn.microsoft.com/library/windows/apps/xaml/system.threading.tasks.aspx) verwendet.

Fügen Sie using-Direktiven hinzu, um die folgenden Namespaces in die CS-Datei Ihrer Seite einzubeziehen.

[!code-cs[SimpleCameraPreviewUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSimpleCameraPreviewUsing)]

Deklarieren Sie eine Klassenmembervariable für das **MediaCapture**-Objekt. Fügen Sie einen booleschen Member hinzu, um nachzuverfolgen, ob die Vorschau der Kamera gerade aktiv ist. 

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

Deklarieren Sie eine Variable vom Typ [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/Windows.System.Display.DisplayRequest), um sicherzustellen, dass die Anzeige während der Ausführung der Vorschau nicht deaktiviert wird.

[!code-cs[DeclareDisplayRequest](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareDisplayRequest)]

Erstellen Sie eine neue Instanz der **MediaCapture**-Klasse, und rufen Sie [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) auf, um das Aufnahmegerät zu initialisieren. Diese Methode schlägt u.U. fehl, beispielsweise auf Geräten ohne Kamera, daher sollte der Aufruf aus einem **try**-Block erfolgen. Beim Versuch, die Kamera zu initialisieren, wird eine **UnauthorizedAccessException** ausgelöst, wenn der Benutzer in den Datenschutzeinstellungen des Geräts den Kamerazugriff deaktiviert hat. Diese Ausnahme tritt auch während der Entwicklung auf, wenn Sie Ihrem App-Manifest nicht die richtigen Funktionen hinzugefügt haben.

**Wichtig:** Bei einigen Gerätefamilien wird dem Benutzer eine Aufforderung zur Zustimmung des Benutzers angezeigt, bevor Ihrer App der Zugriff auf die Kamera des Geräts gewährt wird. Aus diesem Grund müssen Sie nur [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) aus dem Hauptthread der Benutzeroberfläche aufrufen. Der Versuch, die Kamera von einem anderen Thread aus zu initialisieren, kann zum einem Initialisierungsfehler führen.

Verbinden Sie das **MediaCapture**-Objekt mit der **CaptureElement**-Klasse, indem Sie die [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280)-Eigenschaft festlegen. Starten Sie die Vorschau durch Aufrufen von [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613). Rufen Sie [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/Windows.System.Display.DisplayRequest.RequestActive) auf, um sicherzustellen, dass das Gerät während der Ausführung der Vorschau nicht in den Standbymodus wechselt. Legen Sie abschließend die [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayInformation.AutoRotationPreferences)-Eigenschaft auf [**Landscape**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayOrientations) fest, um zu verhindern, dass die sich Benutzeroberfläche und das **CaptureElement** drehen, wenn der Benutzer die Ausrichtung des Geräts ändert. Weitere Informationen zum Behandeln von Änderungen an der Geräteausrichtung finden Sie unter [**Handhaben der Geräte- und Bildschirmausrichtung mit „MediaCapture“**](handle-device-orientation-with-mediacapture.md).  

[!code-cs[StartPreviewAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]


## Beenden des Vorschaudatenstroms

Wenn Sie den Vorschaudatenstrom nicht mehr benötigen, sollten Sie ihn stets beenden und die dazugehörigen Ressourcen ordnungsgemäß löschen, um sicherzustellen, dass die Kamera für andere Apps auf dem Gerät verfügbar ist. Folgende Schritte sind zum Beenden des Vorschaudatenstroms erforderlich:

-   Wenn die Vorschau der Kamera gerade aktiv ist, rufen Sie [**StopPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226622) auf, um den Vorschaudatenstrom zu beenden. Wenn Sie **StopPreviewAsync** aufrufen, während die Vorschau nicht ausgeführt, wird eine Ausnahme ausgelöst.
-   Legen Sie die [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280)-Eigenschaft von **CaptureElement** auf NULL fest. Stellen Sie mithilfe von [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Core.CoreDispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority,Windows.UI.Core.DispatchedHandler) sicher, dass dieser Aufruf im UI-Thread ausgeführt wird.
-   Rufen Sie die [**Dispose**](https://msdn.microsoft.com/library/windows/apps/dn278858)-Methode des **MediaCapture**-Objekts auf, um es freizugeben. Verwenden Sie erneut [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Core.CoreDispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority,Windows.UI.Core.DispatchedHandler), um sicherzustellen, dass dieser Aufruf im UI-Thread ausgeführt wird.
-   Legen Sie die **MediaCapture**-Membervariable auf NULL fest.
-   Rufen Sie [**RequestRelease**](https://msdn.microsoft.com/library/windows/apps/Windows.System.Display.DisplayRequest.RequestRelease) auf, wenn der Bildschirm bei Inaktivität ausgeschaltet werden soll.

[!code-cs[CleanupCameraAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

Sie sollten den Vorschaudatenstrom durch Außerkraftsetzen der [**OnNavigatedFrom**](https://msdn.microsoft.com/library/windows/apps/br227507)-Methode beenden, wenn der Benutzer Ihre Seite verlässt.

[!code-cs[OnNavigatedFrom](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

Sie sollten den Vorschaudatenstrom auch ordnungsgemäß beenden, wenn die App angehalten wird. Registrieren Sie dazu im Konstruktor der Seite einen Handler für das [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860)-Ereignis.

[!code-cs[RegisterSuspending](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterSuspending)]

Stellen Sie im **Suspending**-Ereignishandler zunächst sicher, dass die Seite im [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) der Anwendung angezeigt wird, indem Sie den Seitentyp mit der [**CurrentSourcePageType**](https://msdn.microsoft.com/library/windows/apps/hh702390)-Eigenschaft vergleichen. Wird die Seite derzeit nicht angezeigt, sollten das **OnNavigatedFrom**-Ereignis bereits ausgelöst und der Vorschaudatenstrom geschlossen worden sein. Wird die Seite angezeigt, rufen Sie ein [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684)-Objekt aus den an den Handler übergebenen Ereignisargumenten ab, um sicherzustellen, dass das System Ihre App nicht anhält, bis der Vorschaudatenstrom beendet wurde. Rufen Sie nach dem Beenden des Datenstroms die [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685)-Methode der Verzögerung auf, damit das System die App weiterhin anhalten kann.

[!code-cs[SuspendingHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSuspendingHandler)]


## Verwandte Themen

* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Abrufen eines Vorschauframes](get-a-preview-frame.md)



<!--HONumber=Aug16_HO3-->


