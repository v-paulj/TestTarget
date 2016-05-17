---
author: drewbatgit
ms.assetid: 1361E82A-202F-40F7-9239-56F00DFCA54B
description: Dieser Artikel beschreibt die Schritte zum Aufnehmen von Fotos und Videos mit der MediaCapture-API sowie das Initialisieren und Herunterfahren von „MediaCapture“ und das Behandeln von Geräteausrichtungsänderungen.
title: Aufnehmen von Fotos und Videos mit „MediaCapture“
---

# Aufnehmen von Fotos und Videos mit „MediaCapture“

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Dieser Artikel beschreibt die Schritte zum Aufnehmen von Fotos und Videos mit der [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124)-API sowie das Initialisieren und Herunterfahren von **MediaCapture** und das Behandeln von Änderungen in der Geräteausrichtung.

**MediaCapture** wird als Unterstützung für Apps bereitgestellt, deren Medienaufnahmeprozess auf unterster Ebene gesteuert werden muss und die Szenarien mit erweiterten Aufnahmefunktionen implementieren. Für die Verwendung von **MediaCapture** müssen Sie auch Ihre eigene Aufnahme-UI bereitstellen. Wenn Ihre App nur ein Foto oder Video aufnehmen muss und fortschrittlichere Aufnahmetechniken keine Rolle spielen, können Sie mit [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) mit nur wenigen Codezeilen ganz einfach ein Foto oder Video aufnehmen. Weitere Informationen finden Sie unter [Aufnehmen von Fotos und Videos mit CameraCaptureUI](capture-photos-and-video-with-cameracaptureui.md).

Der Code in diesem Artikel wurde aus dem [CameraStarterKit-Beispiel](http://go.microsoft.com/fwlink/?LinkId=619479) übernommen und angepasst. Sie können das Beispiel herunterladen, um den verwendeten Code im Kontext anzuzeigen oder das Beispiel als Ausgangspunkt für Ihre eigene App zu verwenden.

## Konfigurieren des Projekts

### Hinzufügen von Funktionsdeklarationen zum App-Manifest

Damit Ihre App auf die Kamera eines Geräts zugreifen kann, müssen Sie deklarieren, dass die App *webcam*- und *microphone*-Gerätefunktionen verwendet. Wenn Sie aufgenommene Fotos und Videos in der Bild- oder Videobibliothek des Benutzers speichern möchten, müssen Sie auch die Funktionen *picturesLibrary* und *videosLibrary* deklarieren.

**Hinzufügen von Funktionen zum App-Manifest**

1.  Öffnen Sie in Microsoft Visual Studio im **Projektmappen-Explorer** den Designer für das Anwendungsmanifest, indem Sie auf das Element **package.appxmanifest** doppelklicken.
2.  Wählen Sie die Registerkarte **Funktionen** aus.
3.  Aktivieren Sie die Kontrollkästchen für **Webcam** und **Mikrofon**.
4.  Für den Zugriff auf die Bibliothek „Bilder und Videos“ aktivieren Sie die Kontrollkästchen für **Bildbibliothek** und **Videobibliothek**.

### Hinzufügen von using-Direktiven für APIs im Zusammenhang mit der Medienaufnahme

Im folgenden Codebeispiel sind die Namespaces dargestellt, auf die vom Beispielcode in diesem Artikel verwiesen wird. Zudem wird beschrieben, welche Funktion jeder Namespace bereitstellt.

[!code-cs[Using](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## Initialisieren des MediaCapture-Objekts

Die [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124)-Klasse im [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738)-Namespace ist die grundlegende Schnittstelle für alle Medienaufnahmevorgänge. Apps deklarieren in der Regel eine Variable dieses Typs, die auf eine einzelne Seite beschränkt ist. Die App muss den aktuellen Zustand von **MediaCapture** aufzeichnen. Daher sollten boolesche Variablen für die Initialisierung, die Vorschau und den Aufzeichnungszustand des Objekts deklariert werden.

[!code-cs[MediaCaptureVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMediaCaptureVariables)]

Um das Vorschauvideo korrekt auszurichten, erstellen Sie Membervariablen, um nachzuverfolgen, ob die Kamera extern ist und ob die App momentan den Vorschaustream wiedergibt. Die App sollte den Vorschaustream wiedergeben, wenn Sie der Meinung sind, dass der Videofeed den Benutzer aufgrund der natürlicheren Benutzererfahrung stärker anspricht.

[!code-cs[PreviewVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewVariables)]

Die folgende Beispielmethode initialisiert das Medienaufnahmeobjekt. Der Code sucht zunächst nach einem Videoaufzeichnungsgerät, das für die Medienaufnahme verwendet werden kann. Nachdem ein Gerät gefunden wurde, wird das **MediaCapture**-Objekt initialisiert, und es werden Handler für seine Ereignisse registriert. Als Nächstes wird ein [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/desktop/hh802710)-Objekt mit der ID des Videoaufzeichnungsgeräts erstellt. Die **MediaCapture**-Klasse wird anschließend mit einem [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598)-Aufruf initialisiert.

[!code-cs[InitializeCameraAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitializeCameraAsync)]

-   Die [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432)-Methode kann verwendet werden, um alle Geräte eines bestimmten Typs zu suchen. In diesem Beispiel wird der **DeviceClass.VideoCapture**-Aufzählungswert übergeben, um anzugeben, dass nur Videoaufzeichnungsgeräte zurückgegeben werden sollen. Beachten Sie, dass ein Videoaufzeichnungsgerät sowohl für die Aufnahme von Fotos als auch von Videos verwendet wird.

-   **FindAllAsync** gibt ein [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395)-Objekt zurück, das ein [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393)-Objekt für jedes gefundene Gerät des angeforderten Typs enthält. Die **FirstOrDefault**-Erweiterungsmethode aus dem **System.Linq**-Namespace bietet eine einfache Syntax zum Auswählen eines Elements aus einer Liste basierend auf den angegebenen Bedingungen. Beim ersten Aufruf wird versucht, das erste **DeviceInformation**-Objekt in der Liste auszuwählen, das für [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906) den Wert **Panel.Back** aufweist. Dadurch wird angegeben, dass sich die Kamera auf der Rückseite des Gerätegehäuses befindet. Wenn das Gerät keine Kamera auf der Rückseite aufweist, wird die erste verfügbare Kamera verwendet.

-   Wenn Sie bei der Initialisierung der [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573)-Klasse keine Geräte-ID angeben, wählt das System das erste Gerät aus seiner internen Liste von Geräten aus.

-   Der [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598)-Aufruf initialisiert das Objekt so, dass das angegebene Aufnahmegerät verwendet wird. Dieser Aufruf erfolgt innerhalb eines **try**-Blocks, da eine **UnauthorizedAccessException** ausgelöst wird, wenn der Benutzer der aufrufenden App den Zugriff auf die Kamera verweigert hat. Wenn der Aufruf erfolgreich ist, wird die **\_isInitialized**-Variable auf „true“ festgelegt, damit nachfolgende Methodenaufrufe ermitteln können, ob das Aufnahmegerät initialisiert wurde.

- **Wichtig** Bei einigen Gerätefamilien wird dem Benutzer eine Aufforderung zur Zustimmung des Benutzers angezeigt, bevor Ihrer App der Zugriff auf die Kamera des Geräts gewährt wird. Aus diesem Grund müssen Sie nur [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) aus dem Hauptthread der Benutzeroberfläche aufrufen. Der Versuch, die Kamera von einem anderen Thread aus zu initialisieren, kann zum einem Initialisierungsfehler führen.

-   Wenn die Initialisierung des Aufnahmegeräts erfolgreich ist, wird durch Festlegen von Variablen angegeben, ob es sich um ein externes Aufnahmegerät handelt oder ob es sich auf der Vorderseite des Geräts befindet. Diese Werte werden zur korrekten Ausrichtung der Aufnahmevorschau für den Benutzer verwendet. Schließlich wird die Benutzeroberfläche so aktualisiert, dass ersichtlich wird, dass die Aufnahme verfügbar ist und dass der Vorschaustream von dem Aufnahmegerät gestartet wurde. Alle diese Aufgaben werden in Hilfsmethoden ausgeführt, die später in diesem Artikel beschrieben werden.

## Starten der Aufnahmevorschau

Damit der Benutzer sehen kann, was aufgezeichnet wird, stellen Sie auf der Benutzeroberfläche eine Vorschau der Anzeige im Videoaufzeichnungsgerät bereit.

**Wichtig** Sie müssen die Aufnahmevorschau starten, damit das Aufnahmegerät den Autofokus, die automatische Belichtung und den automatischen Weißabgleich aktiviert.

Das [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278)-Steuerelement wird bereitgestellt, um die Aufnahmevorschau zu aktivieren. Nachfolgend sehen Sie XAML-Beispielcode, der das Capture-Element definiert.

[!code-xml[CaptureElement](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetCaptureElement)]

Benutzer erwarten, dass der Bildschirm während der Anzeige des Videoaufzeichnungsbildschirms in der Vorschau aktiv bleibt und nicht aufgrund von Inaktivität ausgeschaltet wird. Erstellen Sie zu diesem Zweck ein [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816)-Objekt. Deklarieren Sie diese Variable mit einem Seitenbereich, damit dieser während der gesamten Aufzeichnungssitzung erhalten bleibt.

[!code-cs[DisplayRequest](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayRequest)]

Die folgende Methode startet die Vorschau der Medienaufzeichnung. Zuerst wird durch Aufrufen der [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818)-Methode in der [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816)-Klasse angefordert, dass die Anzeige aktiv bleibt. Als Nächstes wird die Vorschau durch Aufrufen der [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613)-Methode gestartet.

[!code-cs[StartPreviewAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]

-   Die [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818)-Methode wird für das **DisplayRequest**-Objekt aufgerufen, um anzufordern, dass das System den Bildschirm angeschaltet lässt.

-   Die [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419)-Eigenschaft der **CaptureElement**-Klasse wird auf das **MediaCapture**-Objekt der App festgelegt, um die Quelle der Vorschau zu definieren.

-   Die [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716)-Eigenschaft wird vom XAML-Framework angegeben, um bidirektionale Benutzeroberflächen zu unterstützen. Durch Festlegen der Flussrichtung der **CaptureElement** -Klasse auf [**FlowDirection.RightToLeft**](https://msdn.microsoft.com/library/windows/apps/br242397) wird die Videovorschau horizontal gekippt. Dies wird verwendet, wenn sich das Aufnahmegerät auf der Vorderseite des Geräts befindet, damit die Vorschau aus Sicht des Benutzers in der richtigen Richtung angezeigt wird.

-   Die [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613)-Methode startet die Anzeige des Vorschaustreams innerhalb der **CaptureElement**-Klasse. Bei einem erfolgreichen Start der Vorschau wird die **\_isPreviewing**-Variable so festgelegt, dass andere Teile der App wissen, dass die App derzeit in der Vorschau angezeigt wird. Zudem wird die Hilfsmethode zum Einstellen der Drehung der Vorschau aufgerufen. Diese Methode wird im nächsten Abschnitt definiert.

## Erkennen von Bildschirm- und Geräteausrichtung

Es gibt mehrere Bereiche einer Medienaufnahme-App, die von der aktuellen Ausrichtung des Geräts betroffen sind, wenn die App auf einem mobilen Gerät wie einem Telefon oder einem Tablet ausgeführt wird. Zu diesen Bereichen gehört das korrekte Drehen des Vorschaustreams von der Kamera und die korrekte Codierung von aufgenommenen Fotos und Videos, sodass diese korrekt ausgerichtet sind, wenn der Benutzer diese anzeigt.

Der Begriff „Anzeigeausrichtung“ bezieht sich auf die Art und Weise, wie das System die XAML-Seite auf dem Gerät dreht, damit diese für den Benutzer in einer aufrechten Position bleibt. „Geräteausrichtung“ bezieht sich auf die Ausrichtung des Geräts im Raum der Welt und somit die Ausrichtung des physischen Geräts im Raum der Welt. Beide Arten von Ausrichtung sind für eine Medienaufnahme-App von Bedeutung. Um die Anzeigeausrichtung zu bearbeiten, deklarieren und initialisieren Sie eine auf eine Seite begrenzte Variable für die [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/dn264258)-Klasse. Deklarieren Sie eine weitere Variable vom Typ [**DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/br226142), um die aktuelle Ausrichtung der Anzeige zu verfolgen. Deklarieren Sie eine [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400)-Variable und eine [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399)-Variable, um die Geräteausrichtung zu verfolgen.

[!code-cs[DisplayInformationAndOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayInformationAndOrientation)]

Mit den folgenden Hilfsmethoden werden Ereignishandler für die Ereignisse [**DisplayInformation.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) und [**SimpleOrientationSensor.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/br206407) registriert bzw. deren Registrierung wird aufgehoben. Zudem werden die Verfolgungsvariablen für die aktuelle Ausrichtung initialisiert. Beachten Sie, dass nicht alle Geräte eine [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400)-Klasse besitzen. Überprüfen Sie dies daher, bevor Sie den Handler registrieren oder versuchen, die aktuelle Ausrichtung abzurufen.

[!code-cs[RegisterOrientationEventHandlers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterOrientationEventHandlers)]

[!code-cs[UnregisterOrientationEventHandlers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterOrientationEventHandlers)]

Aktualisieren Sie im Ereignishandler für das **SimpleOrientationSensor.OrientationChanged**-Ereignis die Geräteausrichtungsvariable mit der aktuellen Ausrichtung. Sie sollten die Ausrichtung nicht aktualisieren, wenn das Gerät nach oben oder unten gerichtet ist.

[!code-cs[SimpleOrientationChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSimpleOrientationChanged)]

Aktualisieren Sie im Ereignishandler für das **DisplayInformation.OrientationChanged**-Ereignis die Anzeigeausrichtungsvariable mit der aktuellen Ausrichtung. Wenn die Videovorschau des Aufnahmegeräts gerade angezeigt wird, aktualisieren Sie die Drehung des Vorschauvideostreams. Die **SetPreviewRotationAsync**-Hilfsmethode wird im folgenden Abschnitt beschrieben.

[!code-cs[DisplayOrientationChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayOrientationChanged)]

## Festlegen der Drehung der Medienaufnahmevorschau

Benutzer erwarten, dass UI-Steuerelemente gedreht werden, wenn sich die Ausrichtung ihres mobilen Geräts ändert, sodass der Text in der Benutzeroberfläche vertikal ausgerichtet und lesbar ist. Bei Verwendung des **CaptureElement**-Steuerelements wird es jedoch in der Regel bevorzugt, dass sich die Ausrichtung der Videovorschau nicht mit dem Gerät dreht. Um die Erwartungen des Benutzers zu erfüllen, sollten Sie den Vorschaustream so drehen, dass er mit der Ausrichtung des Geräts übereinstimmt.

Die Ausrichtung des Vorschaustreams muss in Grad angegeben werden. Die folgende Hilfsmethode konvertiert die [**DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/br226142)-Aufzählungswerte in Grad.

[!code-cs[ConvertDisplayOrientationToDegrees](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertDisplayOrientationToDegrees)]

Diese Hilfsmethode konvertiert einen [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399)-Aufzählungswert, der von der [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400)-Klasse verwendet wird, um die Gerätedrehung in Grad auszudrücken.

[!code-cs[ConvertDeviceOrientationToDegrees](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertDeviceOrientationToDegrees)]

Auf unterer Ebene wird die Drehung eines Streams durch das Microsoft Media Foundation-Framework ausgeführt Die Drehung wird mithilfe des [MF\_MT\_VIDEO\_ROTATION](https://msdn.microsoft.com/library/windows/desktop/hh162880)-Attributs angegeben. Da es sich um eine Windows-Runtime-App handelt, wird die Drehung mithilfe der GUID für das Attribut und nicht mit dem Attributnamen angegeben. Definieren Sie die folgende GUID, um das Attribut für die Videodrehung zu identifizieren.

[!code-cs[RotationKey](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRotationKey)]

Die folgende Methode legt die Drehung des Vorschaustreams fest. Die [**GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211995)-Methode der [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825)-Klasse der Medienaufzeichnung gibt einen Eigenschaftensatz zurück, der aus Schlüssel-Wert-Paaren besteht. [
              **MediaStreamType.VideoPreview**
            ](https://msdn.microsoft.com/library/windows/apps/br226640) legt fest, dass wir die Eigenschaften des Videovorschaustreams abrufen möchten und nicht die des Videoaufzeichnungsstreams oder des Audiostreams. Der Eigenschaftensatz ist eine allgemeine Schnittstelle zum Festlegen von Streameigenschaften. Für diese Aufgabe wird jedoch die oben definierte Videodrehungs-GUID als Eigenschaftenschlüssel hinzugefügt. Die gewünschte Ausrichtung des Videostreams wird als Grad-Wert angegeben. [
              **SetEncodingPropertiesAsync**
            ](https://msdn.microsoft.com/library/windows/apps/dn297781) aktualisiert die Codierungseigenschaften mit den neuen Werten. Auch hier wird mit **MediaStreamType.VideoPreview** angegeben, dass die festgelegten Eigenschaften für den Videovorschaustream gelten.

[!code-cs[SetPreviewRotation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetPreviewRotation)]

-   Für Geräte mit externen Kameras erwartet der Benutzer nicht, dass sich der Kamerastream mit dem Gerät dreht.

-   Wenn die Vorschau für eine Kamera auf der Vorderseite gespiegelt wird, muss die Drehrichtung umgekehrt werden, damit sie mit der Drehung des Geräts übereinstimmt.

-   Einige Geräte, in der Regel Telefone, unterstützen das Festlegen von [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/dn264259)-Eigenschaften auf einen Ausrichtungswert wie [**DisplayOrientations.Landscape**](https://msdn.microsoft.com/library/windows/apps/br226142), um zu erzwingen, dass sich die Anzeige mit dem Gerät dreht. Legen Sie diesen Wert fest, da er ein hohes Maß an Benutzerfreundlichkeit auf Geräten bietet, die dies unterstützen. Sie sollten jedoch trotzdem das Muster oben in Ihre App implementieren, um Geräte zu unterstützen, die keine Einstellungen für die automatische Drehung unterstützen.

-   In früheren Versionen war die [**SetPreviewRotation**](https://msdn.microsoft.com/library/windows/apps/br226611)-Methode die einzige Möglichkeit, den Vorschaustream zu drehen. Diese Methode ist auf der API-Oberfläche weiterhin zur Unterstützung bestehender Apps vorhanden. Aufgrund ihrer Ineffizienz sollte sie jedoch nicht für neue Apps verwendet werden.

## Aufnehmen eines Fotos

Die folgende Methode nimmt ein Foto mithilfe der [**CapturePhotoToStreamAsync**](https://msdn.microsoft.com/library/windows/apps/hh700840)-Methode auf. Dabei werden die angeforderten Codierungseigenschaften sowie ein [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241720)-Objekt übergeben, das die Ausgabe des Aufnahmevorgangs enthalten soll. Die [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993)-Klasse stellt Hilfsmethoden wie [**CreateJpeg**](https://msdn.microsoft.com/library/windows/apps/hh700994) bereit, um die Codierungseigenschaften für die von der Medienaufzeichnung unterstützten Dateitypen zu generieren.

[!code-cs[TakePhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetTakePhotoAsync)]

Ermitteln Sie vor dem Speichern des Fotos in einer Datei die richtige Ausrichtung. Das **MediaCapture**-Objekt kennt die Ausrichtung des Geräts nicht und codiert die aufgenommenen Fotodaten so, als befände sich das Aufnahmegerät in seiner Standardausrichtung. Dies kann zu einer negativen Benutzererfahrung führen, wenn der Benutzer das aufgenommene Foto anzeigt, da das Foto falsch ausgerichtet ist. Die folgenden Hilfsmethoden bestimmen die korrekte Fotoausrichtung und speichern die Datei dann mit der richtigen Ausrichtung.

Die **GetCameraOrientation**-Hilfsmethode beginnt mit der aktuellen Geräteausrichtung und dreht dann diesen Wert in Abhängigkeit von der systemeigenen Ausrichtung des Geräts und der Position der Kamera auf dem Gerät. Wenn sich die Kamera auf der Vorderseite des Geräts befindet, wie in diesem Beispiel durch die **\_mirroringPreview**-Variable angegeben, sollte die Ausrichtung der Kamera umgekehrt werden.

[!code-cs[GetCameraOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetCameraOrientation)]

Die folgende Hilfsmethode konvertiert einfach Werte aus den [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399)-Aufzählungswerten, die vom Ausrichtungssensor verwendet werden, in den entsprechenden [**PhotoOrientation**](https://msdn.microsoft.com/library/windows/apps/hh965476)-Wert, der vom Bitmap-Encoder zum Speichern der Datei verwendet wird.

[!code-cs[ConvertOrientationToPhotoOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertOrientationToPhotoOrientation)]

Das aufgenommene Foto kann schließlich codiert und gespeichert werden. Erstellen Sie ein [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176)-Objekt aus dem Eingangsstream, der die Daten des aufgenommenen Fotos enthält. Erstellen Sie eine neue [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Klasse, und öffnen Sie sie zum Lesen und Schreiben. Erstellen Sie ein [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206)-Objekt, und übergeben Sie die Ausgabedatei und den Decoder, der die Bilddaten enthält. Erstellen Sie eine neue [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/hh974338)-Klasse, und legen Sie eine neue Eigenschaft fest. Der Schlüssel der „System.Photo.Orientation“-Eigenschaft gibt an, dass die Eigenschaft die Ausrichtung des Fotos darstellt. Der Wert ist der zuvor berechnete [**PhotoOrientation**](https://msdn.microsoft.com/library/windows/apps/hh965476)-Wert. Rufen Sie [**SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252) auf, um die Eigenschaften für den Encoder zu aktualisieren. Schreiben Sie das Foto anschließend durch Aufrufen von [**FlushAsync**](https://msdn.microsoft.com/library/dn237883) in die Speicherdatei.

[!code-cs[ReencodeAndSavePhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetReencodeAndSavePhotoAsync)]

-   Durch Festlegen der „System.Photo.Orientation“-Bitmapeigenschaft wird die Ausrichtung des Fotos in den Metadaten der Datei codiert. Die eigentlichen Bilddaten werden dadurch nicht anders codiert. Weitere Informationen zum Einbetten von Metadaten in Bilddateien finden Sie unter [Bildmetadaten](image-metadata.md).

-   Weitere Informationen zum Arbeiten, Codieren und Decodieren von Bildern finden Sie unter [Bildverarbeitung](imaging.md).

## Aufnehmen eines Videos

Um mit der Aufnahme eines Videos zu beginnen, erstellen Sie zuerst eine Speicherdatei, in der das Video aufgezeichnet werden soll. Erstellen Sie als Nächstes [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026)-Klasse, mit der die **MediaCapture**-Klasse das Video in die Datei codiert. Die **MediaEncodingProfile**-Klasse stellt Methoden wie [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/hh701078) bereit, die Codierungsprofile für unterstützte Videoformate erstellen. Verwenden Sie die zuvor erläuterten Hilfsmethoden, um die richtige Drehung für das Video in Grad zu erhalten. Im Gegensatz zum Fotoszenario werden die Drehungsinformationen für Videos von der **MediaCapture**-Klasse im Stream codiert. Ergänzen Sie das Codierungsprofil mit den Drehungsinformationen, indem Sie diese der [**VideoEncodingProperties.Properties**](https://msdn.microsoft.com/library/windows/apps/hh701254)-Auflistung hinzufügen. Die zuvor definierte GUID für die Videodrehung wird als Schlüssel verwendet. Die Drehung in Grad stellt den Wert dar. Rufen Sie schließlich [**MediaCapture.StartRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh700863) auf, und geben Sie die Codierungseigenschaften und die Ausgabedatei an, um mit der Aufzeichnung zu beginnen.

[!code-cs[StartRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartRecordingAsync)]

Um die Aufzeichnung zu beenden, rufen Sie einfach [**MediaCapture.StopRecordAsync**](https://msdn.microsoft.com/library/windows/apps/br226623) auf.

[!code-cs[StopRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStopRecordingAsync)]

Ein Handler für das [**MediaCapture.RecordLimitationExceeded**](https://msdn.microsoft.com/library/windows/apps/hh973312)-Ereignis wurde bei der Initialisierung der **MediaCapture**-Klasse registriert. Rufen Sie im Handler die **StopRecordingAsync**-Methode auf, um die Videoaufzeichnung zu beenden.

[!code-cs[RecordLimitationExceeded](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceeded)]

## Anhalten und Fortsetzen der Videoaufzeichnung

In einigen Fällen möchten Sie eine laufende Videoaufzeichnung möglicherweise anhalten und fortsetzen, anstatt die Aufzeichnung zu beenden und eine neue Aufzeichnung zu starten. Um die Aufzeichnung anzuhalten, rufen Sie [**PauseRecordAsync**](https://msdn.microsoft.com/library/windows/apps/dn858102) auf. Wenn Sie [**MediaCapturePauseBehavior.RetainHardwareResources**](https://msdn.microsoft.com/library/windows/apps/dn926686) angeben, kann die App auf Geräten, die keine gleichzeitige Aufnahme von Videos und Fotos unterstützen, bei angehaltener Videoaufzeichnung keine Fotos aufnehmen. Informationen dazu, wie Sie ermitteln, ob ein Gerät die gleichzeitige Aufnahme von Fotos und Videos unterstützt, finden Sie unter [Kameraprofile](camera-profiles.md).

[!code-cs[PauseRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPauseRecordingAsync)]

Um eine zuvor angehaltene Videoaufzeichnung fortzusetzen, rufen Sie [**ResumeRecordAsync**](https://msdn.microsoft.com/library/windows/apps/dn858103) auf.

[!code-cs[ResumeRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetResumeRecordingAsync)]

## Bereinigen der Aufnahmeressourcen

Es ist äußerst wichtig, dass Sie die Medienaufnahmeressourcen korrekt beenden und freigeben. Wenn Sie dies nicht tun, kann dies zu einem unerwarteten Verhalten der Kamera führen, nachdem die App geschlossen wurde. Dies führt zu einer negativen Benutzererfahrung für die App. In den folgenden Abschnitten werden Sie durch die verschiedenen Schritte zum Beenden der Kamera geführt.

### Beenden der Aufnahmevorschau

Um die Vorschau für die Aufnahme zu beenden, rufen Sie zunächst [**MediaCapture.StopPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226622) auf. Setzen Sie die [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419)-Eigenschaft der [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)-Klasse auf Null. Rufen Sie dann die [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816)-Variable [**RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819) auf, damit das System die Anzeige bei Bedarf ausschalten kann.

[!code-cs[StopPreviewAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStopPreviewAsync)]

-   Sie können nicht aus einem Nicht-UI-Thread auf die Benutzeroberfläche zugreifen. Das Festlegen der **MediaElement.Source**-Eigenschaft und das Aufrufen von **RequestRelease** muss daher mithilfe der [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317)-Methode erfolgen, sodass die Aufrufe im UI-Thread ausgeführt werden.

### Beenden und Löschen des MediaCapture-Objekts

Beenden Sie vor dem Löschen des **MediaCapture**-Objekts alle laufenden Aufzeichnungen sowie den Vorschaustream, indem Sie die zuvor definierten Hilfsmethoden aufrufen. Entfernen Sie anschließend alle für die **MediaCapture**-Klasse registrierten Ereignishandler. Rufen Sie dann [**Dispose**](https://msdn.microsoft.com/library/windows/apps/dn278858) auf, um alle Ressourcen freizugeben, die mit dem Objekt verknüpft sind, und setzen Sie die **MediaCapture**-Variable auf NULL.

[!code-cs[CleanupCameraAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

Rufen Sie diese Methode auf, um das **MediaCapture**-Objekt aus dem Handler für das [**MediaCapture.Failed**](https://msdn.microsoft.com/library/windows/apps/br226593)-Ereignis zu beenden und zu löschen.

[!code-cs[CaptureFailed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureFailed)]

### Verarbeiten der Ereignisse für die App-Lebensdauer und die Seitennavigation

Mithilfe der App-Lebensdauerereignisse kann die App Ressourcen initialisieren und freigeben. Dies ist besonders für das **Application.Suspending**-Ereignis wichtig, bei dem es entscheidend ist, dass die App die Medienaufnahmeressourcen korrekt löscht.

Sie können Handler für die Anwendungslebensdauerereignisse im Konstruktor der Seite registrieren.

[!code-cs[RegisterAppLifetimeEvents](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterAppLifetimeEvents)]

Im Handler für das **Application.Suspending**-Ereignis sollten Sie die Registrierung der Handler für die Anzeige- und Geräteausrichungsereignisse aufheben und das **MediaCapture**-Objekt beenden. Das [**SystemMediaTransportControls.PropertyChanged**](https://msdn.microsoft.com/library/windows/apps/dn278720)-Ereignis, dessen Registrierung hier aufgehoben wird, wird für eine andere Aufgabe im Zusammenhang mit dem Anwendungslebenszyklus benötigt. Sie wird später in diesem Artikel beschrieben.

Achtung Sie müssen das Anhalten einer Verzögerung anfordern, indem Sie [**SuspendingOperation.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) zu Beginn des Handlers für das Anhalteereignis aufrufen. Dadurch wird angefordert, dass das System vor dem Beenden der App auf Ihr Signal wartet, dass der Vorgang abgeschlossen wurde. Dies ist notwendig, da die **MediaCapture**-Vorgänge zum Beenden asynchron sind. Der **Application.Suspending**-Ereignishandler wurde also möglicherweise abgeschlossen, bevor die Kamera ordnungsgemäß beendet wurde.

[!code-cs[Nachdem die erwarteten asynchronen Aufrufe abgeschlossen wurden, sollten Sie die Verzögerung freigeben, indem Sie [**SuspendingDeferral.Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) aufrufen.](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSuspending)]

Anhalten

[!code-cs[Registrieren Sie im Handler für das **Application.Resuming**-Ereignis Handler für die Anzeige- und Geräteausrichtungsereignisse, registrieren Sie das **SystemMediaTransportControls.PropertyChanged**-Ereignis, und initialisieren Sie das **MediaCapture**-Objekt.](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetResuming)]

Fortsetzen

[!code-cs[Mit dem [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508)-Ereignis können Sie anfänglich Handler für die Ereignisse der Anzeige- und Geräteausrichtung registrieren und das **MediaCapture**-Objekt initialisieren.](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnNavigatedTo)]

OnNavigatedTo

[!code-cs[Wenn die App über mehrere Seiten verfügt, sollten Sie die Medienaufnahmeobjekte im [**OnNavigatingFrom**](https://msdn.microsoft.com/library/windows/apps/br227509)-Ereignishandler bereinigen.](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnNavigatingFrom)]

OnNavigatingFrom Damit die App auf Geräten, die mehrere gleichzeitige Fenster unterstützen, korrekt ausgeführt wird, müssen Sie reagieren, wenn die App minimiert oder wiederhergestellt wird. Behandeln Sie hierfür das [**SystemMediaTransportControls.PropertyChanged**](https://msdn.microsoft.com/library/windows/apps/dn278720)-Ereignis.

[!code-cs[Initialisieren Sie eine Membervariable, um einen Verweises auf das [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)-Objekt für Ihre App zu speichern.](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareSystemMediaTransportControls)]

DeclareSystemMediaTransportControls Der Code zum Registrieren und Aufheben der Registrierung des **PropertyChanged**-Ereignisses sollte den App-Lebenszyklusereignissen hinzugefügt werden, wie in den Beispielen oben dargestellt. Überprüfen Sie im Handler für das Ereignis, ob es sich bei der Eigenschaftenänderung, die das Ereignis ausgelöst hat, um die [**SystemMediaTransportControlsProperty.SoundLevel**](https://msdn.microsoft.com/library/windows/apps/dn278721)-Eigenschaft handelt. Wenn diese Eigenschaft geändert wurde, überprüfen Sie den Wert der Eigenschaft. Wenn der Wert [**SoundLevel.Muted**](https://msdn.microsoft.com/library/windows/apps/hh700852) lautet, wurde die App minimiert. In diesem Fall sollten Sie die Medienaufnahmeressourcen entsprechend bereinigen. Andernfalls signalisiert das Ereignis, dass das App-Fenster wiederhergestellt wurde und Sie die Medienaufnahmeressourcen erneut initialisieren sollten.

[!code-cs[Auf die **SoundLevel**-Eigenschaft muss im UI-Thread zugegriffen werden. Der Code in dieser Methode ist daher in einen Aufruf von [**Dispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) eingeschlossen.](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSystemMediaControlsPropertyChanged)]

## SystemMediaControlsPropertyChanged

### Weitere Überlegungen zur Benutzeroberfläche für die Medienaufzeichnung

Festlegen der Einstellungen für die automatische Drehung Wie im vorherigen Abschnitt zum Drehen des Vorschaustreams erwähnt, unterstützen einige Geräte das Festlegen von [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/dn264259), um zu verhindern, dass die Seite mit der [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278)-Klasse zum Anzeigen der Vorschau mit dem Gerät gedreht wird. Dies bietet eine hohes Maß an Benutzerfreundlichkeit auf Geräten, die dies unterstützen. Legen Sie diesen Wert fest, wenn Ihre App gestartet wird oder wenn Sie mit dem Anzeigen der Vorschau beginnen.

[!code-cs[Sie sollten dennoch eine Verarbeitung der Vorschaudrehung für Geräte implementieren, die keine Einstellungen für die automatische Drehung unterstützen.](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetAutoRotationPreferences)]

### SetAutoRotationPreferences

Behandeln der Ausrichtung von UI-Elementen Benutzer erwarten in der Regel, dass sich die UI-Elemente in einer Kamera-App, wie etwa die Schaltflächen für die Initiierung der Foto- oder Videoaufnahme, zusammen mit der Videovorschau drehen. Die folgende Methode veranschaulicht die Verwendung der zuvor definierten Hilfsmethoden für die Ausrichtung, damit die Schaltflächen in der Kamera-UI korrekt ausgerichtet werden. Rufen Sie diese Methode aus den Ereignishandlern [**DisplayInformation.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) und [**SimpleOrientationSensor.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/br206407) sowie beim ersten Starten der App auf.

[!code-cs[Ihre Implementierung kann je nach der Benutzeroberfläche der App variieren.](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateButtonOrientation)]

UpdateButtonOrientation

[!code-cs[Wenn die App beendet wird oder Sie zu einer Seite navigieren, die nicht im Zusammenhang mit der Medienaufzeichnung steht, setzen Sie die Einstellung für die automatische Drehung auf [**None**](https://msdn.microsoft.com/library/windows/apps/br226142), damit die Benutzeroberfläche normal gedreht wird.](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRevertAutoRotationPreferences)]

### RevertAutoRotationPreferences

Unterstützen der gleichzeitigen Aufnahme von Fotos und Videos Mit der [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738)-API können Sie gleichzeitig Fotos und Videos auf Geräten aufnehmen, die dies unterstützen. Der Kürze halber wird in diesem Beispiel mithilfe der [**ConcurrentRecordAndPhotoSupported**](https://msdn.microsoft.com/library/windows/apps/dn278843)-Eigenschaft ermittelt, ob die gleichzeitige Aufnahme von Videos und Fotos unterstützt wird. Eine robustere und empfohlene Methode hierfür ist jedoch die Verwendung von Kameraprofilen.

Weitere Informationen finden Sie unter [Kameraprofile](camera-profiles.md). Die folgende Hilfsmethode aktualisiert die App-Steuerelemente so, dass sie dem aktuellen Aufnahmezustand der App entsprechen.

[!code-cs[Rufen Sie diese Methode immer dann auf, wenn sich der Aufnahmezustand der App ändert, z. B. wenn die Videoaufzeichnung gestartet oder beendet wird.](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateCaptureControls)]

### UpdateCaptureControls

Unterstützung von UI-Features für mobile Geräte Der gesamte in diesem Artikel dargestellte Code funktioniert in einer Universellen Windows-App. Mit ein paar zusätzlichen Codezeilen können Sie spezielle UI-Features nutzen, die nur auf mobilen Geräten vorhanden sind.

**Um diese Features zu verwenden, müssen Sie einen Verweis auf das Microsoft Mobile Extension SDK für die Universelle App-Plattform zu Ihrem Projekt hinzufügen.**

1.  So fügen Sie einen Verweis auf das mobile Erweiterungs-SDK für die Unterstützung einer Hardwaretaste an der Kamera hinzu

2.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen...** aus.

3.  Erweitern Sie den Knoten für die **Universelle Windows-App**, und wählen Sie **Erweiterungen**aus.

Aktivieren Sie das Kontrollkästchen neben **Microsoft Mobile Extension SDK für Universelle App-Plattform**. Mobile Geräte verfügen über ein [**StatusBar**](https://msdn.microsoft.com/library/windows/apps/dn633864)-Steuerelement, das dem Benutzer Statusinformationen zum Gerät liefert. Dieses Steuerelement benötigt Speicherplatz auf dem Bildschirm, der die Medienaufnahme-UI beeinträchtigen kann. Sie können die Statusleiste ausblenden, indem Sie [**HideAsync**](https://msdn.microsoft.com/library/windows/apps/dn610339) aufrufen. Dieser Aufruf muss jedoch innerhalb eines Bedingungsblocks ausgeführt werden, in dem Sie die [**ApiInformation.IsTypePresent**](https://msdn.microsoft.com/library/windows/apps/dn949016)-Methode verwenden, um festzustellen, ob die API verfügbar ist. Diese Methode gibt nur „true“ auf mobilen Geräten zurück, die die Statusleiste unterstützen.

[!code-cs[Sie sollten die Statusleiste ausblenden, wenn die App gestartet wird oder wenn Sie die Vorschau von der Kamera beginnen.](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHideStatusBar)]

HideStatusBar

[!code-cs[Wenn die App beendet wird oder der Benutzer die Medienaufnahmeseite der App verlässt, können Sie das Steuerelement wieder einblenden.](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetShowStatusBar)]

ShowStatusBar Einige mobile Geräte verfügen über eine dedizierte Hardwaretaste für die Kamera, die einige Benutzer einem Steuerelement auf dem Bildschirm vorziehen. Um benachrichtigt zu werden, wenn die Hardwaretaste für die Kamera gedrückt wird, registrieren Sie einen Handler für das [**HardwareButtons.CameraPressed**](https://msdn.microsoft.com/library/windows/apps/dn653805)-Ereignis.

[!code-cs[Da diese API nur auf mobilen Geräten verfügbar ist, müssen Sie erneut **IsTypePresent** verwenden, um sicherzustellen, dass die API auf dem aktuellen Gerät unterstützt wird, bevor Sie versuchen, darauf zuzugreifen.](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhoneUsing)]

[!code-cs[PhoneUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterCameraButtonHandler)]

RegisterCameraButtonHandler

[!code-cs[Im Handler für das **CameraPressed**-Ereignis können Sie die Aufnahme eines Fotos initiieren.](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCameraPressed)]

CameraPressed

[!code-cs[Wenn die App heruntergefahren wird oder der Benutzer die Medienaufnahmeseite der App verlässt, heben Sie die Registrierung des Hardwaretastenhandlers auf.](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterCameraButtonHandler)]

UnregisterCameraButtonHandler **Hinweis** Dieser Artikel ist für Windows 10-Entwickler gedacht, die Apps für die Universelle Windows-Plattform (UWP) schreiben.

## Informationen für die Entwicklung unter Windows 8.x oder Windows Phone 8.x finden Sie in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

* [Verwandte Themen](camera-profiles.md)
* [Kameraprofile](high-dynamic-range-hdr-photo-capture.md)
* [HDR-Fotoaufnahmen (High Dynamic Range)](capture-device-controls-for-photo-and-video-capture.md)
* [Steuerelemente des Aufnahmegeräts für Foto- und Videoaufnahmen](capture-device-controls-for-video-capture.md)
* [Steuerelemente des Aufnahmegeräts für Videoaufnahmen](effects-for-video-capture.md)
* [Effekte für die Videoaufnahme](scene-analysis-for-media-capture.md)
* [Szenenanalyse für die Medienaufnahme](variable-photo-sequence.md)
* [Variable Fotosequenz](get-a-preview-frame.md)
* [Abrufen eines Vorschauframes](http://go.microsoft.com/fwlink/?LinkId=619479)


<!--HONumber=May16_HO2-->


