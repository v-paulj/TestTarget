---
author: drewbatgit
ms.assetid: 
description: "In diesem Artikel wird beschrieben, wie Sie mit „MediaFrameReader“ und „MediaCapture“ Medienframes aus einer oder mehreren verfügbaren Quellen abrufen. Hierzu zählen Farb-, Tiefen- und Infrarotkameras sowie Audiogeräte und sogar benutzerdefinierte Framequellen (etwa für Skeletal-Tracking-Frames)."
title: "Verarbeiten von Medienframes mit „MediaFrameReader“"
translationtype: Human Translation
ms.sourcegitcommit: 21433f812915a2b4da6b4d68151bbc922a97a7a7
ms.openlocfilehash: 5c4bb51ea3b1740cdbb5fa43746ce7b3edca6aa1

---

# Verarbeiten von Medienframes mit „MediaFrameReader“

In diesem Artikel wird beschrieben, wie Sie mit [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) und [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) Medienframes aus einer oder mehreren verfügbaren Quellen abrufen. Hierzu zählen Farb-, Tiefen- und Infrarotkameras sowie Audiogeräte und sogar benutzerdefinierte Framequellen (etwa für Skeletal-Tracking-Frames). Dieses Feature wurde für die Verwendung von Apps entworfen, die Medienframes in Echtzeit verarbeiten, wie beispielsweise Augmented-Reality- und Tiefenkamera-Apps.

Wenn Sie normale Videos oder Fotos aufnehmen möchten, wie mit einer typischen Foto-App, sollten Sie möglicherweise eine der anderen Aufnahmemethoden verwenden, die von [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) unterstützt werden. Eine Liste der verfügbaren Methoden zur Medienaufnahme und Artikel zu ihrer Verwendung finden Sie unter [**Kamera**](camera.md).

> [!NOTE] 
> Die in diesem Artikel besprochenen Features sind erst ab Windows10, Version1607, verfügbar.

> [!NOTE] 
> Es gibt ein Beispiel für universelle Windows-Apps, in dem die Verwendung von **MediaFrameReader** zum Anzeigen von Frames aus unterschiedlichen Framequellen demonstriert wird, unter anderem Farb-, Tiefen- und Infrarotkameras. Weitere Informationen finden Sie unter [Beispiel für Kameraframes](http://go.microsoft.com/fwlink/?LinkId=823230).

## Einrichten Ihres Projekts
Wie bei allen Apps, die **MediaCapture** verwenden, müssen Sie deklarieren, dass Ihre App die *Webcam*-Funktion verwendet. Erst dann können Sie auf Kamerageräte zugreifen. Wenn Ihre App von einem Audiogerät aufzeichnet, müssen Sie auch die *microphone*-Gerätefunktion deklarieren. 

**Hinzufügen von Funktionen zum App-Manifest**

1.  Öffnen Sie in MicrosoftVisual Studio im **Projektmappen-Explorer** den Designer für das Anwendungsmanifest, indem Sie auf das Element **package.appxmanifest** doppelklicken.
2.  Wählen Sie die Registerkarte **Funktionen** aus.
3.  Aktivieren Sie die Kontrollkästchen für **Webcam** und **Mikrofon**.
4.  Für den Zugriff auf die Bibliothek „Bilder und Videos“ aktivieren Sie die Kontrollkästchen für **Bildbibliothek** und **Videobibliothek**.

Im Beispielcode in diesem Artikel werden neben den in der Standard-Projektvorlage enthaltenen APIs auch APIs aus den folgenden Namespaces verwendet.

[!code-cs[FramesUsing](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFramesUsing)]

## Auswählen von Framequellen und Framequellgruppen
Viele Apps, die Medienframes verarbeiten, müssen Frames aus mehreren Quellen gleichzeitig abrufen, z.B. die Farb- und Tiefenkameras eines Geräts. Das [**MediaFrameSourceGroup**] (https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup)-Objekt stellt einen Satz von Medienframequellen dar, die gleichzeitig verwendet werden können. Rufen Sie die statische Methode [**MediaFrameSourceGroup.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.FindAllAsync) auf, um eine Liste aller vom aktuellen Gerät unterstützten Gruppen von Framequellen abzurufen.

[!code-cs[FindAllAsync](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFindAllAsync)]

Sie können auch einen [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceWatcher) erstellen, indem Sie mit [**DeviceInformation.CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/br225427) und dem vom [**MediaFrameSourceGroup.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.GetDeviceSelector) zurückgegebenen Wert Benachrichtigungen empfangen, wenn sich die verfügbaren Framequellgruppen für das Gerät ändern, z.B. bei Anschließen einer externen Kamera. Weitere Informationen finden Sie unter [**Auflisten von Geräten**](https://msdn.microsoft.com/windows/uwp/devices-sensors/enumerate-devices).

Eine [**MediaFrameSourceGroup**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup) verfügt über eine Sammlung von [**MediaFrameSourceInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo)-Objekten, die in der Gruppe enthaltene Framequellen beschreiben. Nach dem Abrufen der auf dem Gerät verfügbaren Framequellgruppen können Sie die Gruppe auswählen, die die für Sie relevanten Framequellen verfügbar macht.

Das folgende Beispiel zeigt die einfachste Möglichkeit zum Auswählen einer Framequellgruppe. Vom Code werden dann einfach Schleifen durch alle verfügbaren Gruppen und anschließend durch die einzelnen Elemente in der [**SourceInfos**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.SourceInfos)-Sammlung durchgeführt. Für jede **MediaFrameSourceInfo** wird geprüft, ob sie die gewünschten Funktionen unterstützt. In diesem Fall wird die [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo.MediaStreamType)-Eigenschaft auf den Wert [**VideoPreview**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaStreamType) überprüft. D.h. das Gerät stellt einen Videovorschau-Stream bereit, und die [**SourceKind**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo.SourceKind)-Eigenschaft wird auf den Wert [**Farbe**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceKind) überprüft, um anzuzeigen, dass die Quelle Farbframes bereitstellt.

[!code-cs[SimpleSelect](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSimpleSelect)]

Diese Methode zur Identifizierung der gewünschten Framequellgruppe und Framequellen ist für einfache Fälle geeignet. Wenn Sie jedoch anhand komplexerer Kriterien Framequellen auswählen möchten, wird dieses Verfahren unpraktisch. Eine weitere Methode ist die Auswahl mithilfe von Linq-Syntax und anonymen Objekten. Im folgenden Beispiel wird die Erweiterungsmethode **Select** verwendet, um die **MediaFrameSourceGroup**-Objekte in der *FrameSourceGroups*-Liste in ein anonymes Objekt mit zwei Feldern umzuwandeln:*sourceGroup*, welches die Gruppe selbst darstellt, und *colorSourceInfo*, welches die Farbframequelle in der Gruppe repräsentiert. Für das Feld *colorSourceInfo* wird das Ergebnis der **FirstOrDefault**-Methode festgelegt, die das erste Objekt auswählt, für dessen bereitgestelltes Prädikat die Auflösung „true“ ist. In diesem Fall ist das Prädikat „true“, wenn der Datenstromtyp **VideoPreview** und die Art der Quelle **Color** ist und sich die Kamera an der Vorderseite des Geräts befindet.

Aus der Liste der von der oben beschriebenen Abfrage zurückgegebenen anonymen Objekte werden mit der **Where**-Erweiterungsmethode nur die Objekte ausgewählt, in denen das Feld *colorSourceInfo* nicht NULL ist. Schließlich wird **FirstOrDefault** aufgerufen, um das erste Element in der Liste auszuwählen.

Nun können Sie mithilfe der Felder des ausgewählten Objekts die Verweise auf das ausgewählte **MediaFrameSourceGroup**- und das **MediaFrameSourceInfo**-Objekt abrufen, die die Farbkamera darstellen. Diese werden später verwendet, um das **MediaCapture**-Objekt zu initialisieren und einen **MediaFrameReader** für die ausgewählte Quelle zu erstellen. Abschließend sollten Sie testen, ob Quellgruppe NULL ist. Dies würde bedeuten, dass das aktuelle Gerät nicht über Ihre angeforderten Aufnahmequellen verfügt.

[!code-cs[SelectColor](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSelectColor)]

Im folgenden Beispiel wird mit einer ähnlichen Methode wie der oben beschriebenen eine Quellgruppe mit Farb-, Tiefen- und Infrarotkameras ausgewählt.

[!code-cs[ColorInfraredDepth](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetColorInfraredDepth)]

## Initialisieren des MediaCapture-Objekts zum Verwenden der ausgewählten Framequellgruppe
Im nächsten Schritt wird das **MediaCapture**-Objekt initialisiert, um die im vorherigen Schritt ausgewählte Framequellgruppe zu verwenden.

Das **MediaCapture**-Objekt wird üblicherweise an mehreren Stellen in Ihrer App verwendet. Sie sollten daher eine Klassenmembervariable deklarieren, um es aufzunehmen.

[!code-cs[DeclareMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

Erstellen Sie eine Instanz des **MediaCapture**-Objekts, indem Sie den Konstruktor aufrufen. Erstellen Sie als Nächstes ein [**MediaCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSettings)-Objekt, das zum Initialisieren des **MediaCapture**-Objekts verwendet wird. In diesem Beispiel werden die folgenden Einstellungen verwendet:

* [**SourceGroup**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.SourceGroup) – Damit wird dem System mitgeteilt, welche Quellgruppe zum Abrufen von Frames verwendet wird. Bedenken Sie, dass die Quellgruppe einen Satz von Medienframequellen definiert, die gleichzeitig verwendet werden können.
* [**SharingMode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.SharingMode) – Damit wird dem System mitgeteilt, ob exklusive Steuerung der Aufnahmequellgeräte erforderlich ist. Bei Festlegung auf [**ExclusiveControl**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSharingMode) können Sie die Einstellungen für das Aufnahmegerät, z.B. das Format der von ihm erzeugten Frames, ändern. Wenn jedoch eine andere App bereits über die exklusive Steuerung verfügt, wird der Versuch Ihrer App, das Medienaufnahmegerät zu initialisieren, fehlschlagen. Bei Festlegung auf [**SharedReadOnly**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSharingMode) können Sie Frames von den Framequellen empfangen, auch wenn diese gerade von einer anderen App verwendet werden. Sie können jedoch nicht die Einstellungen der Geräte ändern.
* [**MemoryPreference**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.MemoryPreference) – Wenn Sie [**CPU**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureMemoryPreference) angeben, verwendet das Gerät den CPU-Speicher. Damit wird garantiert, dass eintreffende Frames als [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap)-Objekte verfügbar sind. Wenn Sie [**Auto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureMemoryPreference) angeben, wählt das System dynamisch den optimalen Speicherort zum Speichern der Frames aus. Wenn das System die Verwendung des GPU-Speichers auswählt, werden die Medienframes als [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface)-Objekt und nicht als **SoftwareBitmap**-Objekt übermittelt.
* [**StreamingCaptureMode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.StreamingCaptureMode) – Legen Sie [**Video**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.StreamingCaptureMode) fest, um anzugeben, dass das Streamen von Audio nicht erforderlich ist.

Rufen Sie [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) auf, um **MediaCapture** mit ihren gewünschten Einstellungen zu initialisieren. Achten Sie darauf, den Aufruf in einem *Try*-Block auszuführen, falls die Initialisierung fehlschlägt.

[!code-cs[InitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitMediaCapture)]

## Festlegen des bevorzugten Formats für die Framequelle
Zum Einstellen des bevorzugten Formats einer Framequelle benötigen Sie ein [**MediaFrameSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource)-Objekt, das die Quelle darstellt. Sie erhalten dieses Objekt, indem Sie auf das [**Frames**](https://msdn.microsoft.com/library/windows/apps/Windows.Phone.Media.Capture.CameraCaptureSequence.Frames)-Verzeichnis des initialisierten **MediaCapture** -Objekts zugreifen und dabei den Bezeichner der gewünschten Framequelle angeben. Aus diesem Grund wurde das [**MediaFrameSourceInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo)-Objekt bei der Auswahl einer Framequellgruppe gespeichert.

Die [**MediaFrameSource.SupportedFormats**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource.SupportedFormats)-Eigenschaft enthält eine Liste von [**MediaFrameFormat**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameFormat)-Objekten, die die unterstützten Formate der Framequelle beschreiben. Verwenden Sie die Linq-Erweiterungsmethode **Where**, um basierend auf den gewünschten Eigenschaften ein Format auszuwählen. In diesem Beispiel wird ein Format mit einer Breite von 1.080Pixeln ausgewählt, welches Frames im 32-Bit-RGB-Format bereitstellen kann. Die **FirstOrDefault**-Erweiterungsmethode wählt den ersten Eintrag in der Liste aus. Ist das ausgewählte Format NULL, wird das angeforderte Format nicht von der Framequelle unterstützt. Wenn das Format unterstützt wird, können Sie [**SetFormatAsync**](https://msdn.microsoft.com/library/windows/apps/) aufrufen, um die Verwendung dieses Formats durch die Quelle anzufordern.

[!code-cs[GetPreferredFormat](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetPreferredFormat)]

## Erstellen eines Frame-Readers für die Framequelle
Verwenden Sie zum Empfangen von Frames für die Medienframequelle einen [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader).

[!code-cs[DeclareMediaFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareMediaFrameReader)]

Instanziieren Sie den Frame-Reader, indem Sie auf Ihrem initialisierten **MediaCapture**-Objekt [**CreateFrameReaderAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.CreateFrameReaderAsync) aufrufen. Das erste Argument dieser Methode ist die Framequelle, von der Frames empfangen werden sollen. Sie können für jede gewünschte Framequelle einen separaten Frame-Reader erstellen. Das zweite Argument gibt dem System das Ausgabeformat vor, in dem die Frames übermittelt werden sollen. So entfällt das Konvertieren der übermittelten Frames. Beachten Sie, dass eine Ausnahme ausgelöst wird, wenn Sie ein von der Framequelle nicht unterstütztes Format festlegen. Stellen Sie daher sicher, dass dieser Wert in der [**SupportedFormats**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource.SupportedFormats)-Sammlung enthalten ist.  

Registrieren Sie nach dem Erstellen des Frame-Readers einen Handler für das [**FrameArrived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.FrameArrived)-Ereignis, das immer dann ausgelöst wird, wenn ein neuer Frame von der Quelle verfügbar ist.

Rufen Sie [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.StartAsync) auf, um dem System zu befehlen, mit dem Lesen von Frames aus der Quelle zu beginnen.

[!code-cs[CreateFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCreateFrameReader)]

## Behandeln des „FrameArrived“-Ereignisses
Das [**MediaFrameReader.FrameArrived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.FrameArrived)-Ereignis wird ausgelöst, wenn ein neuer Frame verfügbar ist. Sie können entweder jeden empfangenen Frame verarbeiten oder Frames nur bei Bedarf verwenden. Der Frame-Reader löst das Ereignis in seinem eigenen Thread aus. Daher müssen Sie möglicherweise Synchronisierungslogik implementieren, damit Sie nicht aus mehreren Threads auf dieselben Daten zugreifen. In diesem Abschnitt wird das Synchronisieren von Zeichenfarbframes zu einem Image-Steuerelement in einer XAML-Seite veranschaulicht. Dieses Szenario behandelt die zusätzliche Synchronisierungseinschränkung, die erfordert, dass alle Updates für XAML-Steuerelemente im UI-Thread ausgeführt werden.

Als erster Schritt beim Anzeigen von Frames in XAML wird ein Bild-Steuerelement erstellt. 

[!code-xml[ImageElementXAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetImageElementXAML)]

Deklarieren Sie auf der CodeBehind-Seite eine Klassenmembervariable vom Typ **SoftwareBitmap**. Diese wird als Hintergrundpuffer verwendet, in den alle eingehenden Bilder kopiert werden. Beachten Sie, dass nicht die Bilddaten selbst kopiert werden, sondern nur die Objektverweise. Deklarieren Sie außerdem einen booleschen Wert, um nachzuverfolgen, ob der UI-Vorgang aktuell ausgeführt wird.

[!code-cs[DeclareBackBuffer](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareBackBuffer)]

Da Frames als **SoftwareBitmap**-Objekte übermittelt werden, müssen Sie ein [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource)-Objekt erstellen, mit dem Sie eine **SoftwareBitmap** als Quelle für ein XAML-**Steuerelement** verwenden können. Sie sollten die Bildquelle in Ihrem Code festlegen, bevor Sie den Frame-Reader starten.

[!code-cs[ImageElementSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetImageElementSource)]

Nun wird der **FrameArrived**-Ereignishandler implementiert. Wenn der Handler aufgerufen wird, enthält der *sender*-Parameter einen Verweis auf das **MediaFrameReader**-Objekt, welches das Ereignis ausgelöst hat. Rufen Sie [**TryAcquireLatestFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.TryAcquireLatestFrame) für dieses Objekt auf, um zu versuchen, den aktuellen Frame abzurufen. Wie der Name schon sagt, schlägt das Zurückgeben eines Frames über **TryAcquireLatestFrame** möglicherweise fehl. Testen Sie daher beim Zugreifen auf die VideoMediaFrame- und die SoftwareBitmap-Eigenschaften, ob diese auf NULL festgelegt sind. In diesem Beispiel wird der Operator mit NULL-Bedingung ? verwendet, um auf die **SoftwareBitmap** zurückzugreifen. Anschließend wird geprüft, ob das abgerufene Objekt auf NULL festgelegt ist.

Das **Image**-Steuerelement kann nur Bilder in BRGA8 Format anzeigen, die entweder vormultipliziert sind oder kein Alpha aufweisen. Weist der eingehende Frame nicht dieses Format auf, wird mit der statischen Methode [**Convert**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap.Covert) die Software-Bitmap in das richtige Format konvertiert.

Als Nächstes wird mit der [**Interlocked.Exchange**](https://msdn.microsoft.com/en-us/library/bb337971)-Methode der Verweis auf die eingehende Bitmap mit der Hintergrundpuffer-Bitmap ausgetauscht. Bei dieser Methode werden die Verweise in einer threadsicheren atomischen Operation ausgetauscht. Nach dem Austausch wird das alte Hintergrundpufferbild – jetzt in der *softwareBitmap*-Variablen – gelöscht, um seine Ressourcen zu bereinigen.

Als Nächstes wird mit dem mit dem **Image**-Element verknüpften [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Core.CoreDispatcher) eine Aufgabe erstellt, die durch Aufrufen von [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) im UI-Thread ausgeführt wird. Da die asynchronen Aufgaben in dieser Aufgabe ausgeführt werden, wird der an **RunAsync** weitergegebene Lambda-Ausdruck mit dem *async*- Schlagwort deklariert.

Innerhalb der Aufgabe wird die *_taskRunning*-Variable überprüft, um sicherzustellen, dass zu jedem Zeitpunkt nur eine Instanz der Aufgabe ausgeführt wird. Wird die Aufgabe nicht bereits ausgeführt, wird *_taskRunning* auf „true“ festgelegt, um zu verhindern, dass die Aufgabe erneut ausgeführt wird. **Interlocked.Exchange** wird zum Kopieren aus dem Hintergrundpuffer in eine temporäre **SoftwareBitmap** in einer *while*-Schleife aufgerufen, bis das Hintergrundpufferbild NULL ist. Bei jedem Auffüllen der temporären Bitmap wird die **Source**-Eigenschaft des **Bildes** in eine **SoftwareBitmapSource** umgewandelt. Anschließend wird [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856) aufgerufen, um die Bildquelle festzulegen.

Schließlich wird die *_taskRunning*-Variable wieder auf „false“ festgelegt, damit die Aufgabe beim nächsten Aufrufen des Handlers erneut ausgeführt werden kann.

[!code-cs[FrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFrameArrived)]

## Bereinigung von Ressourcen
Achten Sie darauf, nach dem Lesen der Frames den Medienframe-Reader zu beenden, indem Sie [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.StopAsync) aufrufen, die Registrierung des **FrameArrived**-Handlers aufheben und das **MediaCapture**-Objekt löschen.

[!code-cs[Bereinigen](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCleanup)]

Weitere Informationen zum Bereinigen von Medienaufnahmeobjekten bei angehaltener Anwendung finden Sie unter [**Anzeigen der Kameravorschau**](simple-camera-preview-access.md).

## Die FrameRenderer-Hilfsprogrammklasse
Das Universal Windows-[Beispiel für Kameraframes](http://go.microsoft.com/fwlink/?LinkId=823230) stellt eine Hilfsprogrammklasse zum einfachen Anzeigen der Frames aus Farb-, Infrarot- und Tiefenquellen in Ihrer App bereit. In der Regel sollen Tiefen- und Infrarotdaten nicht nur auf dem Bildschirm angezeigt werden. Dennoch ist diese Hilfsprogrammklasse ein hilfreiches Tool zum Veranschaulichen der Frame-Reader-Funktion und zum Debuggen Ihrer eigenen Frame-Reader-Implementierung.

Die **FrameRenderer**-Hilfsprogrammklasse implementiert die folgenden Methoden.

* **FrameRenderer**-Konstruktor – Der Konstruktor initialisiert die Verwendung des übergebenen XAML-[**Bild**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Image)-Elements durch die Hilfsprogrammklasse zum Anzeigen von Medienframes.
* **ProcessFrame** – Bei dieser Methode wird ein durch eine [**MediaFrameReference**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference) repräsentierter Medienframe in dem in den Konstruktor eingereichten **Bild**-Element angezeigt. Üblicherweise sollten Sie diese Methode aus Ihrem [**FrameArrived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.FrameArrived)-Ereignishandler aufrufen, wenn der von [**TryAcquireLatestFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.TryAcquireLatestFrame) zurückgegebene Frame übergeben wird.
* **ConvertToDisplayableImage** – Bei dieser Methode wird das Format des Medienframes überprüft und ggf. in ein anzeigbares Format umgewandelt. Bei Farbbildern wird also überprüft, ob als Farbformat BGRA8 festgelegt ist und der Bitmap-Alphamodus prämultipliziert wird. Bei Tiefen- oder Infrarotframes wird jede Scanzeile verarbeitet, um die Tiefen- oder Infrarotwerte mithilfe der ebenfalls im Beispiel enthaltenen und unten aufgeführten **PsuedoColorHelper**-Klasse in einen Pseudo-Farbgradienten umzuwandeln

> [!NOTE] 
> Zum Ändern von Pixeln in **SoftwareBitmap**-Bildern müssen Sie auf einen nativen Speicherpuffer zugreifen. Zu diesem Zweck müssen Sie die in der untenstehenden Codeauflistung enthaltene IMemoryBufferByteAccess COM-Schnittstelle verwenden und die Projekteigenschaften aktualisieren, um die Kompilierung von unsicherem Code zuzulassen. Weitere Informationen finden Sie unter [Erstellen, Bearbeiten und Speichern von Bitmapbildern](imaging.md).

[!code-cs[FrameArrived](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetFrameRenderer)]

## Verwandte Themen

* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Beispiel für Kameraframes](http://go.microsoft.com/fwlink/?LinkId=823230)
 

 







<!--HONumber=Aug16_HO3-->


