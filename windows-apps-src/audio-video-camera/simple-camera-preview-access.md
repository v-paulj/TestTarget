---
ms.assetid: 9BA3F85A-970F-411C-ACB1-B65768B8548A
description: Dieser Artikel beschreibt, wie Sie in einer UWP (Universelle Windows-Plattform)-App innerhalb einer XAML-Seite schnell den Kameravorschau-Stream anzeigen.
title: Einfacher Zugriff auf die Kameravorschau
---

# Einfacher Zugriff auf die Kameravorschau

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Dieser Artikel beschreibt, wie Sie in einer UWP (Universelle Windows-Plattform)-App innerhalb einer XAML-Seite schnell den Kameravorschau-Stream anzeigen. Zum Erstellen einer App, die Fotos und Videos mit der Kamera erfasst, müssen Sie Aufgaben wie das Behandeln der Geräte- und Kameraausrichtung oder das Festlegen von Codierungsoptionen für die erfasste Datei durchführen. Für einige App-Szenarien möchten Sie vielleicht einfach nur den Vorschaustream von der Kamera anzeigen, ohne sich Gedanken über diese anderen Überlegungen machen zu müssen. Dieser Artikel zeigt, wie dies mit einem Minimum an Code möglich ist. Hinweis: Sie sollten den Vorschaustream immer ordnungsgemäß beenden, wenn Sie damit fertig sind; führen Sie dazu die folgenden Schritte aus.

Informationen zum Schreiben einer Kamera-App, die Fotos oder Videos erfasst, finden Sie unter [Aufnehmen von Fotos und Videos mit MediaCapture](capture-photos-and-video-with-mediacapture.md).

## Hinzufügen von Funktionsdeklarationen zum App-Manifest

Damit Ihre App auf die Kamera eines Geräts zugreifen kann, müssen Sie deklarieren, dass die App *webcam*- und *microphone*-Gerätefunktionen verwendet. Wenn Sie aufgenommene Fotos und Videos in der Bild- oder Videobibliothek des Benutzers speichern möchten, müssen Sie auch die Funktionen *picturesLibrary* und *videosLibrary* deklarieren.

**Hinzufügen von Funktionen zum App-Manifest**

1.  Öffnen Sie in Microsoft Visual Studio im **Projektmappen-Explorer** den Designer für das Anwendungsmanifest, indem Sie auf das Element **package.appxmanifest** doppelklicken.
2.  Wählen Sie die Registerkarte **Funktionen** aus.
3.  Aktivieren Sie die Kontrollkästchen für **Webcam** und **Mikrofon**.
4.  Für den Zugriff auf die Bibliothek „Bilder und Videos“ aktivieren Sie die Kontrollkästchen für **Bildbibliothek** und **Videobibliothek**.

## Hinzufügen eines CaptureElement zu einer Seite

Verwenden Sie ein [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) zum Anzeigen des Vorschaustreams innerhalb der XAML-Seite.

[!code-xml[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]

## Verwenden von MediaCapture zum Starten des Vorschaustreams

Das [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124)-Objekt ist die Schnittstelle Ihrer App mit der Kamera des Geräts. Diese Klasse ist ein Mitglied des Namespace „Windows.Media.Capture“. Im Beispiel in diesem Artikel werden neben den in der Standard-Projektvorlage enthaltenen APIs auch APIs aus den Namespaces [**Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691) und [System.Threading.Tasks](https://msdn.microsoft.com/library/windows/apps/xaml/system.threading.tasks.aspx) verwendet.

[!code-cs[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]

Fügen Sie eine "using"-Direktive hinzu, um die folgenden Namespaces in die cs-Datei Ihrer Seite einzubeziehen.

[!code-cs[SimpleCameraPreviewUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSimpleCameraPreviewUsing)]

Deklarieren Sie eine Klassenvariable für das **MediaCapture**-Objekt.

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

Erstellen Sie eine neue Instanz der **MediaCapture**-Klasse, und rufen Sie [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) auf, um das Aufnahmegerät zu initialisieren. Diese Methode schlägt u. U. fehl, beispielsweise auf Geräten ohne Kamera, daher sollte der Aufruf aus einem **try**-Block erfolgen. Beim Versuch, die Kamera zu initialisieren, wird eine **UnauthorizedAccessException** ausgelöst, wenn der Benutzer in den Datenschutzeinstellungen des Geräts den Kamerazugriff deaktiviert hat. Diese Ausnahme tritt auch während der Entwicklung auf, wenn Sie Ihrem App-Manifest nicht die richtigen Funktionen hinzugefügt haben.

**Wichtig** Bei einigen Gerätefamilien wird dem Benutzer eine Aufforderung zur Zustimmung des Benutzers angezeigt, bevor Ihrer App der Zugriff auf die Kamera des Geräts gewährt wird. Aus diesem Grund müssen Sie nur [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) aus dem Hauptthread der Benutzeroberfläche aufrufen. Der Versuch, die Kamera von einem anderen Thread aus zu initialisieren, kann zum einem Initialisierungsfehler führen.

Verbinden Sie das **MediaCapture**-Objekt mit dem **CaptureElement** durch Festlegen der [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280)-Eigenschaft. Starten Sie schließlich die Vorschau durch Aufrufen von [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613).

[!code-cs[StartPreviewAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]


## Beenden des Vorschaustreams

Wenn Sie mit der Verwendung des Vorschaustreams fertig sind, sollten Sie den Stream immer beenden und die zugehörigen Ressourcen ordnungsgemäß löschen, um sicherzustellen, dass die Kamera für andere Apps auf dem Gerät verfügbar ist. Folgende Schritte sind zum Beenden des Vorschaustreams erforderlich:

-   Rufen Sie [**StopPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226622) auf, um den Vorschaustream zu beenden.
-   Setzen Sie die [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280)-Eigenschaft von **CaptureElement** auf null.
-   Rufen Sie die [**Dispose**](https://msdn.microsoft.com/library/windows/apps/dn278858)-Methode des **MediaCapture**-Objekts auf, um es freizugeben.
-   Legen Sie die **-MediaCapture**-Membervariable auf null fest.

[!code-cs[CleanupCameraAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

Sie sollten den Vorschaustream durch Außerkraftsetzen der [**OnNavigatedFrom**](https://msdn.microsoft.com/library/windows/apps/br227507)-Methode beenden, wenn der Benutzer Ihre Seite verlässt.

[!code-cs[OnNavigatedFrom](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

Sie sollten den Vorschaustream auch beenden, wenn die App angehalten wird. Registrieren Sie dazu im Konstruktor der Seite einen Handler für das [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860)-Ereignis.

[!code-cs[RegisterSuspending](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterSuspending)]

Stellen Sie im **Suspending**-Ereignishandler zunächst sicher, dass die Seite im [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) der Anwendung angezeigt wird, indem Sie den Seitentyp mit der [**CurrentSourcePageType**](https://msdn.microsoft.com/library/windows/apps/hh702390)-Eigenschaft vergleichen. Wird die Seite derzeit nicht angezeigt, sollten das **OnNavigatedFrom**-Ereignis bereits ausgelöst und der Vorschaustream geschlossen worden sein. Wird die Seite angezeigt, rufen Sie ein [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684)-Objekt aus den an den Handler übergebenen Ereignisargumenten ab, um sicherzustellen, dass das System Ihre App nicht anhält, bis der Vorschaustream beendet wurde. Rufen Sie nach dem Beenden des Streams die [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685)-Methode der Verzögerung auf, damit das System die App weiterhin anhalten kann.

[!code-cs[SuspendingHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSuspendingHandler)]

## Erfassen eines Standbilds aus dem Vorschaustream

Das Aufzeichnen eines Standbilds im Vorschaustream der Medienaufnahme in Form eines [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358)-Elements ist einfach. Weitere Informationen finden Sie unter [Abrufen eines Vorschauframes](get-a-preview-frame.md).

## Verwandte Themen

* [Aufnehmen von Fotos und Videos mit "MediaCapture"](capture-photos-and-video-with-mediacapture.md)
* [Abrufen eines Vorschauframes](get-a-preview-frame.md)


<!--HONumber=Mar16_HO5-->


