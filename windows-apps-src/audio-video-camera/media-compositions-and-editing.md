---
author: drewbatgit
ms.assetid: C4DB495D-1F91-40EF-A55C-5CABBF3269A2
description: "Mithilfe der APIs im Windows.Media.Editing-Namespace können Sie schnell Apps entwickeln, die Benutzern das Erstellen von Medienkompositionen aus Audio- und Videoquelldateien ermöglichen."
title: Medienkompositionen und -bearbeitung
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 226ee9212f6688c48c4d4d7b3195ec5c27a3afdd

---

# Medienkompositionen und -bearbeitung

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


In diesem Artikel wird beschrieben, wie Sie mithilfe der APIs im [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565)-Namespace schnell Apps entwickeln, die Benutzern das Erstellen von Medienkompositionen aus Audio- und Videoquelldateien ermöglichen. Zu den Features des Frameworks zählen die Möglichkeit, programmgesteuert mehrere Videoclips zusammen anzufügen, Video- und Bildüberlagerungen sowie Hintergrundaudio hinzuzufügen und Audio- und Videoeffekte anzuwenden. Nach der Erstellung können Medienkompositionen zur Wiedergabe oder Freigabe in eine Medienflatfile gerendert werden; alternativ können sie auch auf einen Datenträger serialisiert und von diesem deserialisiert werden, sodass die Benutzer Kompositionen laden und ändern können, die sie zuvor erstellt haben. Alle diese Funktionen werden in einer benutzerfreundlichen Windows-Runtime-Schnittstelle bereitgestellt, die den Umfang und die Komplexität des zum Ausführen dieser Aufgaben erforderlichen Codes im Vergleich zur [Microsoft Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) -Low-Level-API erheblich verringert.

## Erstellen einer neuen Medienkomposition

Die [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646)-Klasse ist der Container für alle Medienclips, aus denen die Komposition besteht, und dient zum Rendern der endgültigen Komposition, zum Laden und Speichern von Kompositionen auf Datenträger und zum Bereitstellen eines Vorschaudatenstroms der Komposition zur Anzeige in der Benutzeroberfläche. Um **MediaComposition** in einer App zu verwenden, schließen Sie den [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565)-Namespace und den [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962)-Namespace ein, der zugehörige benötigte APIs bereitstellt.

[!code-cs[Namespace1](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace1)]

Auf das **MediaComposition**-Objekt wird von mehreren Punkten im Code aus zugegriffen. Daher deklarieren Sie normalerweise eine Membervariable, um das Objekt darin zu speichern.

[!code-cs[DeclareMediaComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaComposition)]

Der Konstruktor für **MediaComposition** akzeptiert keine Argumente.

[!code-cs[MediaCompositionConstructor](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetMediaCompositionConstructor)]

## Hinzufügen von Medienclips zu einer Komposition

Medienkompositionen enthalten in der Regel einen oder mehrere Videoclips. Sie können ein [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/hh738369)-Element verwenden, um Benutzern die Auswahl einer Videodatei zu ermöglichen. Nachdem die Datei ausgewählt wurde, erstellen Sie ein neues [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596)-Objekt als Container für den Videoclip, indem Sie [**MediaClip.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652607) aufrufen. Anschließend fügen Sie den Clip der [**Clips**](https://msdn.microsoft.com/library/windows/apps/dn652648)-Liste des **MediaComposition**-Objekts hinzu.

[!code-cs[PickFileAndAddClip](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetPickFileAndAddClip)]

-   Medienclips werden in der **MediaComposition** in der gleichen Reihenfolge angezeigt wie in der [**Clips**](https://msdn.microsoft.com/library/windows/apps/dn652648)-Liste.

-   Ein **MediaClip** kann nur einmal in eine Komposition eingeschlossen werden. Der Versuch, einen **MediaClip** hinzuzufügen, der bereits von der Komposition verwendet wird, führt zu einem Fehler. Um einen Videoclip mehrmals in einer Komposition wiederzuverwenden, rufen Sie zum Erstellen neuer **MediaClip**-Objekte [**Clone**](https://msdn.microsoft.com/library/windows/apps/dn652599) auf, die dann der Komposition hinzugefügt werden können.

-   Universelle Windows-Apps sind nicht zum Zugreifen auf das gesamte Dateisystem berechtigt. Die [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457)-Eigenschaft der [**StorageApplicationPermissions**](https://msdn.microsoft.com/library/windows/apps/br207456)-Klasse ermöglicht der App, einen Datensatz einer Datei zu speichern, die vom Benutzer ausgewählt wurde, sodass Sie Berechtigungen für den Dateizugriff beibehalten können. Die **FutureAccessList** kann maximal 1000 Einträge enthalten; die App muss die Liste daher verwalten, um sicherzustellen, dass sie nicht zu voll wird. Dies ist besonders wichtig, wenn Sie das Laden und Ändern zuvor erstellter Kompositionen unterstützen möchten.

-   Eine **MediaComposition** unterstützt Videoclips in MP4-Format.

-   Wenn eine Videodatei mehrere eingebettete Audiotitel enthält, können Sie durch Festlegen der [**SelectedEmbeddedAudioTrackIndex**](https://msdn.microsoft.com/library/windows/apps/dn652627)-Eigenschaft auswählen, welcher Audiotitel in der Komposition verwendet werden soll.

-   Um einen **MediaClip** mit einer einzigen Farbfüllung für den gesamten Frame zu erstellen, rufen Sie [**CreateFromColor**](https://msdn.microsoft.com/library/windows/apps/dn652605) auf, und geben Sie eine Farbe und eine Dauer für den Clip an.

-   Um einen **MediaClip** aus einer Bilddatei zu erstellen, rufen Sie [**CreateFromImageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652610) auf, und geben Sie eine Bilddatei und eine Dauer für den Clip an.

-   Um einen **MediaClip** aus einer [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505) zu erstellen, rufen Sie [**CreateFromSurface**](https://msdn.microsoft.com/library/dn764774) auf, und geben Sie eine Oberfläche und eine Dauer für den Clip an.

## Anzeigen einer Vorschau der Komposition in einem MediaElement

Damit der Benutzer die Medienkomposition anzeigen kann, fügen Sie der XAML-Datei, die die Benutzeroberfläche definiert, ein [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) hinzu.

[!code-xml[MediaElement](./code/MediaEditing/cs/MainPage.xaml#SnippetMediaElement)]

Deklarieren Sie eine Membervariable vom Typ [**MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn282716).


[!code-cs[DeclareMediaStreamSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaStreamSource)]

Rufen Sie die [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674)-Methode des **MediaComposition**-Objekts auf, um eine **MediaStreamSource** für die Komposition zu erstellen. Rufen Sie dann die [**SetMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn299029)-Methode von **MediaElement** auf. Jetzt kann die Komposition in der Benutzeroberfläche angezeigt werden.


[!code-cs[UpdateMediaElementSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetUpdateMediaElementSource)]

-   Die **MediaComposition** muss mindestens einen Medienclip enthalten, bevor [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674) aufgerufen wird; andernfalls ist das zurückgegebene Objekt NULL.

-   Die **MediaElement**-Zeitachse wird nicht automatisch mit den Änderungen an der Komposition aktualisiert. Es wird empfohlen, sowohl **GeneratePreviewMediaStreamSource** als auch **SetMediaStreamSource** jedes Mal aufzurufen, wenn Sie eine Reihe von Änderungen an der Komposition vornehmen und die Benutzeroberfläche aktualisieren möchten.

Es wird empfohlen, das **MediaStreamSource**-Objekt und die [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419)-Eigenschaft von **MediaElement** auf NULL festzulegen, wenn der Benutzer die Seite verlässt, um die zugehörigen Ressourcen freizugeben.

[!code-cs[OnNavigatedFrom](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

## Rendern der Komposition in eine Videodatei

Um eine Medienkomposition in eine Videoflatfile zu rendern, sodass sie freigegeben und auf anderen Geräten angezeigt werden kann, müssen Sie APIs aus dem [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105)-Namespace verwenden. Um die Benutzeroberfläche mit dem Fortschritt des asynchronen Vorgangs zu aktualisieren, benötigen Sie außerdem APIs aus dem [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)-Namespace.

[!code-cs[Namespace2](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace2)]

Nachdem Sie dem Benutzer die Auswahl einer Ausgabedatei mit einem [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)-Element erlaubt haben, rendern Sie die Komposition in die ausgewählte Datei, indem Sie die [**RenderToFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652690)-Methode des **MediaComposition**-Objekts aufrufen. Der Rest des Codes im folgenden Beispiel folgt einfach dem Muster zum Behandeln von [**AsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/desktop/br205807).

[!code-cs[RenderCompositionToFile](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetRenderCompositionToFile)]

-   [
            **MediaTrimmingPreference**](https://msdn.microsoft.com/library/windows/apps/dn640561) ermöglicht es Ihnen, der Geschwindigkeit des Transcodierungvorgangs Vorrang vor der Genauigkeit der Kürzung benachbarter Medienclips zu geben. **Fast** bewirkt eine schnellere Transcodierung mit weniger genauer Kürzung, **Precise** bewirkt eine langsamere Transcodierung mit genauerer Kürzung.

## Kürzen eines Videoclips

Um die Dauer eines Videoclips in einer Komposition zu kürzen, legen Sie die [**TrimTimeFromStart**](https://msdn.microsoft.com/library/windows/apps/dn652637)-Eigenschaft, die [**TrimTimeFromEnd**](https://msdn.microsoft.com/library/windows/apps/dn652634)-Eigenschaft (oder beide) des [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596)-Objekts fest.

[!code-cs[TrimClipBeforeCurrentPosition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetTrimClipBeforeCurrentPosition)]

-   Sie können jede beliebige Benutzeroberfläche verwenden, um dem Benutzer die Angabe des Start- und Endwerts für die Kürzung zu ermöglichen. Im vorstehenden Beispiel wird die [**Position**](https://msdn.microsoft.com/library/windows/apps/br227407)-Eigenschaft des **MediaElement** verwendet, um zunächst zu ermitteln, welcher MediaClip an der aktuellen Position in der Komposition wiedergegeben wird, indem [**StartTimeInComposition**](https://msdn.microsoft.com/library/windows/apps/dn652629) und [**EndTimeInComposition**](https://msdn.microsoft.com/library/windows/apps/dn652618) überprüft werden. Anschließend werden die Eigenschaften **Position** und **StartTimeInComposition** erneut verwendet, um die Zeitspanne zu berechnen, um die der Clipanfang gekürzt werden muss. Die **FirstOrDefault**-Methode ist eine Erweiterungsmethode aus dem **System.Linq**-Namespace, die den Code zur Auswahl von Elementen aus einer Liste vereinfacht.
-   Mit der [**OriginalDuration**](https://msdn.microsoft.com/library/windows/apps/dn652625)-Eigenschaft des **MediaClip**-Objekts können Sie die Dauer des Medienclips ermitteln, ohne dass eine Kürzung angewendet wird.
-   Die [**TrimmedDuration**](https://msdn.microsoft.com/library/windows/apps/dn652631)-Eigenschaft informiert Sie über die Dauer des Medienclips nach der Kürzung.
-   Wenn Sie einen Kürzungswert angeben, der größer als die ursprüngliche Dauer des Clips ist, wird kein Fehler ausgelöst. Wenn jedoch eine Komposition nur einen einzigen Clip enthält und dieser durch Angabe eines hohen Kürzungswerts auf die Länge null gekürzt wird, wird bei einem nachfolgenden Aufruf von [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674) NULL zurückgegeben, so als enthielte die Komposition keine Clips.

## Hinzufügen eines Hintergrundaudiotitels zu einer Komposition

Um einer Komposition einen Hintergrundtitel hinzuzufügen, laden Sie eine Audiodatei, und erstellen Sie dann durch Aufrufen der [**BackgroundAudioTrack.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652561)-Factorymethode ein [**BackgroundAudioTrack**](https://msdn.microsoft.com/library/windows/apps/dn652544)-Objekt. Fügen Sie **BackgroundAudioTrack** anschließend zur [**BackgroundAudioTracks**](https://msdn.microsoft.com/library/windows/apps/dn652647)-Eigenschaft der Komposition hinzu.

[!code-cs[AddBackgroundAudioTrack](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddBackgroundAudioTrack)]

-   Eine **MediaComposition** unterstützt Hintergrundaudiotitel in den folgenden Formaten: MP3, WAV, FLAC.

-   Ein Hintergrundaudiotitel

-   Wie bei Videodateien sollten Sie die [**StorageApplicationPermissions**](https://msdn.microsoft.com/library/windows/apps/br207456)-Klasse verwenden, um den Zugriff auf Dateien in der Komposition zu bewahren.

-   Ebenso wie ein **MediaClip** kann auch ein **BackgroundAudioTrack**-Objekt nur einmal in eine Komposition eingeschlossen werden. Der Versuch, einen **BackgroundAudioTrack** hinzuzufügen, der bereits von der Komposition verwendet wird, führt zu einem Fehler. Um einen Audiotitel mehrmals in einer Komposition wiederzuverwenden, rufen Sie [**Clone**](https://msdn.microsoft.com/library/windows/apps/dn652599) zum Erstellen neuer **MediaClip**-Objekte auf, die dann der Komposition hinzugefügt werden können.

-   Standardmäßig beginnt die Wiedergabe von Hintergrundaudiotiteln am Anfang der Komposition. Wenn mehrere Hintergrundtitel vorhanden sind, wird die Wiedergabe aller Titel am Anfang der Komposition gestartet. Damit die Wiedergabe eines Hintergrundaudiotitels zu einem anderen Zeitpunkt beginnt, legen Sie die [**Delay**](https://msdn.microsoft.com/library/windows/apps/dn652563)-Eigenschaft auf die gewünschte Zeitverschiebung fest.

## Hinzufügen einer Überlagerung zu einer Komposition

Mithilfe von Überlagerungen können Sie mehrere Videoebenen in einer Komposition übereinander stapeln. Eine Komposition kann mehrere Überlagerungsebenen enthalten, die wiederum jeweils mehrere Überlagerungen enthalten können. Erstellen Sie ein [**MediaOverlay**](https://msdn.microsoft.com/library/windows/apps/dn764793)-Objekt, indem Sie einen **MediaClip** im Konstruktor übergeben. Legen Sie die Position und die Deckkraft der Überlagerung fest, erstellen Sie anschließend eine neue [**MediaOverlayLayer**](https://msdn.microsoft.com/library/windows/apps/dn764795), und fügen Sie das **MediaOverlay**-Objekt der [**Overlays**](https://msdn.microsoft.com/library/windows/desktop/dn280411)-Liste hinzu. Fügen Sie zum Schluss die **MediaOverlayLayer** zur [**OverlayLayers**](https://msdn.microsoft.com/library/windows/apps/dn764791)-Liste der Komposition hinzu.

[!code-cs[AddOverlay](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddOverlay)]

-   Überlagerungen innerhalb einer Ebene werden basierend auf ihrer Reihenfolge in der **Overlays**-Liste der enthaltenden Ebene in Z-Reihenfolge sortiert. Höhere Indizes innerhalb der Liste werden auf niedrigeren Indizes gerendert. Dies gilt auch für Überlagerungsebenen innerhalb einer Komposition. Eine Ebene mit höherem Index in der **OverlayLayers**-Liste der Komposition wird auf niedrigeren Indizes gerendert.

-   Da Überlagerungen übereinander gestapelt statt nacheinander wiedergegeben werden, beginnt die Wiedergabe alle Überlagerungen standardmäßig am Anfang der Komposition. Damit die Wiedergabe einer Überlagerung zu einem anderen Zeitpunkt beginnt, legen Sie die [**Delay**](https://msdn.microsoft.com/library/windows/apps/dn764810)-Eigenschaft auf die gewünschte Zeitverschiebung fest.

## Hinzufügen von Effekten zu einem Medienclip

Jeder **MediaClip** in einer Komposition enthält eine Liste von Audio- und Videoeffekten, der mehrere Effekte hinzugefügt werden können. Die Effekte müssen [**IAudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608044) bzw. [**IVideoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608047) implementieren. Im folgenden Beispiel wird anhand der aktuellen Medienelementposition der aktuell angezeigte **MediaClip** gewählt und dann eine neue Instanz der [**VideoStabilizationEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn926762) erstellt und an die [**VideoEffectDefinitions**](https://msdn.microsoft.com/library/windows/apps/dn652643)-Liste des Medienclips angefügt.

[!code-cs[AddVideoEffect](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddVideoEffect)]

## Speichern einer Komposition in einer Datei

Medienkompositionen können in eine Datei serialisiert werden, um sie zu einem späteren Zeitpunkt zu ändern. Wählen Sie eine Ausgabedatei aus, und rufen Sie dann die [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646)-Methode [**SaveAsync**](https://msdn.microsoft.com/library/windows/apps/dn640554) auf, um die Komposition zu speichern.

[!code-cs[SaveComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetSaveComposition)]

## Laden einer Komposition aus einer Datei

Medienkompositionen können aus einer Datei deserialisiert werden, sodass der Benutzer die Komposition anzeigen und ändern kann. Wählen Sie eine Kompositionsdatei aus, und rufen Sie dann die [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646)-Methode [**LoadAsync**](https://msdn.microsoft.com/library/windows/apps/dn652684) auf, um die Komposition zu laden.

[!code-cs[OpenComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOpenComposition)]

-   Wenn eine Mediendatei in der Komposition an einem Speicherort abgelegt ist, auf den Ihre App nicht zugreifen kann, und nicht in der [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457)-Eigenschaft der [**StorageApplicationPermissions**](https://msdn.microsoft.com/library/windows/apps/br207456)-Klasse für Ihre App enthalten ist, tritt beim Laden der Komposition ein Fehler auf.

 

 







<!--HONumber=Jun16_HO4-->


