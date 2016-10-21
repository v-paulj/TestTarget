---
author: drewbatgit
ms.assetid: C5623861-6280-4352-8F22-80EB009D662C
description: "In diesem Artikel wird erläutert, wie Sie die MediaSource-Klasse verwenden, die allgemein zum Verweisen auf Medien aus verschiedenen Quellen (etwa lokale Dateien oder Remotedateien) sowie zum Wiedergeben dieser Medien verwendet wird und ein gemeinsames Modell für den Mediendatenzugriff verfügbar macht – unabhängig vom zugrunde liegenden Medienformat."
title: Medienelemente, Wiedergabelisten und Titel
translationtype: Human Translation
ms.sourcegitcommit: c2e337e88f9dda3380dd62c32ca6e5d942366636
ms.openlocfilehash: bb49af7a386356647000e268bcc6983351eaf4b8

---

# Medienelemente, Wiedergabelisten und Titel

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \].

 In diesem Artikel wird erläutert, wie Sie die [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource)-Klasse verwenden, die allgemein zum Verweisen auf Medien aus verschiedenen Quellen (etwa lokale Dateien oder Remotedateien) sowie zum Wiedergeben dieser Medien verwendet wird und ein gemeinsames Modell für den Mediendatenzugriff verfügbar macht – unabhängig vom zugrunde liegenden Medienformat. Die [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939)-Klasse erweitert den Funktionsumfang von **MediaSource** und ermöglicht die Verwaltung und Auswahl mehrerer Audio-, Video- und Metadatentitel in einem Medienelement. [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) ermöglicht die Erstellung von Wiedergabelisten auf der Grundlage von Medienelementen.


## Erstellen und Wiedergeben eines MediaSource-Elements

Erstellen Sie eine neue Instanz von **MediaSource**, indem Sie eine der von der Klasse verfügbar gemachten Factory-Methoden aufrufen:

-   [**CreateFromAdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930906)
-   [**CreateFromIMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn965527)
-   [**CreateFromMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930907)
-   [**CreateFromMseStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930908)
-   [**CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909)
-   [**CreateFromStream**](https://msdn.microsoft.com/library/windows/apps/dn930910)
-   [**CreateFromStreamReference**](https://msdn.microsoft.com/library/windows/apps/dn930911)
-   [**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912)

Nach dem Erstellen einer **MediaSource** können Sie diese mit einer [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652535) durch Festlegen der [**Source**](https://msdn.microsoft.com/library/windows/apps/dn987010)-Eigenschaft abspielen. Ab Windows10, Version 1607, können Sie einen **MediaPlayer** einem [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) durch Aufrufen von [**SetMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/mt708764) zuweisen, um die Media Player-Inhalte in einer XAML-Seite zu rendern. Dies ist die bevorzugte Methode gegenüber der Verwendung von **MediaElement**. Weitere Informationen zur Verwendung des **MediaPlayer** finden Sie unter [**Wiedergeben von Audio- und Videoinhalten mit MediaPlayer**](play-audio-and-video-with-mediaplayer.md).

Das folgende Beispiel zeigt die Wiedergabe einer vom Benutzer ausgewählten Mediendatei in einem **MediaPlayer** mit **MediaSource**.

Für dieses Szenario müssen die Namespaces [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) und [**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562) eingeschlossen werden.

[!code-cs[Using](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetUsing)]

Deklarieren Sie eine Membervariable vom Typ **MediaSource**. Für die Beispiele in diesem Artikel wird die Medienquelle als Klassenmember deklariert, um den Zugriff über mehrere Orte zu ermöglichen.

[!code-cs[DeclareMediaSource](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaSource)]

Deklarieren Sie eine Variable zum Speichern des **MediaPlayer** Objekts. Wenn die Medieninhalte in XAML gerendert werden sollen, fügen Sie der Seite ein **MediaPlayerElement**-Steuerelements hinzu.

[!code-cs[DeclareMediaPlayer](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-xml[MediaPlayerElement](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetMediaPlayerElement)]

Verwenden Sie ein [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)-Element, um dem Benutzer das Auswählen einer wiederzugebenden Mediendatei zu ermöglichen. Initialisieren Sie mit dem von der [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275)-Auswahlmethode zurückgegebenen [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Objekt durch Aufrufen von [**MediaSource.CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909) ein neues Medienobjekt. Legen Sie abschließend durch Aufrufen der [**SetPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/dn899085)-Methode die Medienquelle als Wiedergabequelle für das **MediaElement**-Element fest.

[!code-cs[PlayMediaSource](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetPlayMediaSource)]

Standardmäßig beginnt **MediaPlayer** nicht automatisch mit der Wiedergabe, wenn die Medienquelle festgelegt ist. Sie können die Wiedergabe manuell durch Aufrufen von [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play) wiedergeben.

[!code-cs[Play](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlay)]

Sie können auch die [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay)-Eigenschaft des **MediaPlayer** auf „ true“ festlegen, um dem Player mitzuteilen, dass er mit der Wiedergabe beginnen soll, sobald die Medienquelle festgelegt ist.

[!code-cs[Automatische Wiedergabe](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAutoPlay)]

## Behandeln mehrerer Audio-, Video- und Metadatentitel mit „MediaPlaybackItem“

Die Wiedergabe mithilfe eines [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930905)-Elements ist praktisch, da es allgemein die Wiedergabe von Medien aus unterschiedlichen Quellen ermöglicht. Bei Erstellung eines [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939) aus der **MediaSource** stehen dagegen stärker erweiterte Verhaltensweisen zur Verfügung. Unter anderem können Sie damit auf mehrere Audio-, Video- und Datentitel eines Medienelements zugreifen und die Titel verwalten.

Deklarieren Sie eine Variable zum Speichern Ihres **MediaPlaybackItem**-Elements.

[!code-cs[DeclareMediaPlaybackItem](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackItem)]

Erstellen Sie ein **MediaPlaybackItem**-Element, indem Sie den Konstruktor aufrufen und ein initialisiertes **MediaSource**-Objekt übergeben.

Wenn Ihre App mehrere Audio-, Video- oder Datentitel in einem Medienwiedergabeelement unterstützt, registrieren Sie Ereignishandler für das Ereignis [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948), [**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) oder [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952).

Legen Sie abschließend die Wiedergabequelle des **MediaElement**- oder **MediaPlayer**-Elements auf Ihr **MediaPlaybackItem**-Element fest.

[!code-cs[PlayMediaPlaybackItem](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackItem)]

> [!NOTE] 
> Ein **MediaSource**-Element kann immer nur einem einzelnen **MediaPlaybackItem**-Element zugeordnet werden. Wenn Sie nach der Erstellung eines quellenbasierten **MediaPlaybackItem** -Elements versuchen, auf der Grundlage der gleichen Quelle ein weiteres Wiedergabeelement zu erstellen, tritt ein Fehler auf. Außerdem gilt: Nach der Erstellung eines auf einer Medienquelle basierenden **MediaPlaybackItem**-Elements können Sie das **MediaSource**-Objekt nicht direkt als Quelle für ein **MediaPlayer**-Element festlegen, sondern müssen stattdessen das **MediaPlaybackItem**-Element verwenden.

Das [**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954)-Ereignis wird ausgelöst, wenn ein **MediaPlaybackItem**-Element mit mehreren Videotiteln als Wiedergabequelle zugewiesen wurde, und kann erneut ausgelöst werden, wenn sich die Liste mit den Videotiteln für das Element ändert. Der Handler für dieses Ereignis ermöglicht die Aktualisierung der Benutzeroberfläche, damit der Benutzer zwischen den verfügbaren Titeln wechseln kann. In diesem Beispiel wird zum Anzeigen der verfügbaren Videotitel ein [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348)-Element verwendet.

[!code-xml[VideoComboBox](./code/MediaSource_Win10/cs/MainPage.xaml#SnippetVideoComboBox)]

Durchlaufen Sie im **VideoTracksChanged**-Handler alle Titel, die in der [**VideoTracks**](https://msdn.microsoft.com/library/windows/apps/dn930953)-Liste des Wiedergabeelements enthalten sind. Für die einzelnen Titel wird jeweils ein neues [**ComboBoxItem**](https://msdn.microsoft.com/library/windows/apps/br209349)-Element erstellt. Falls der Titel noch nicht beschriftet ist, wird auf der Grundlage des Titelindex eine Beschriftung generiert. Die [**Tag**](https://msdn.microsoft.com/library/windows/apps/br208745)-Eigenschaft des Kombinationsfeldelements wird zur späteren Identifizierung auf den Titelindex festgelegt. Danach wird das Element dem Kombinationsfeld hinzugefügt. Beachten Sie, dass diese Vorgänge innerhalb eines [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317)-Aufrufs ausgeführt werden, da alle UI-Änderungen im UI-Thread erfolgen müssen und dieses Ereignis in einem anderen Thread ausgelöst wird.

[!code-cs[VideoTracksChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetVideoTracksChanged)]

Im [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776)-Handler für das Kombinationsfeld wird der Titelindex aus der **Tag**-Eigenschaft des ausgewählten Elements abgerufen. Das Festlegen der [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn956634)-Eigenschaft der [**VideoTracks**](https://msdn.microsoft.com/library/windows/apps/dn930953)-Liste des Medienwiedergabeelements bewirkt, dass das **MediaElement**- oder **MediaPlayer**-Element beim aktiven Videotitel zum angegebenen Index wechselt.

[!code-cs[VideoTracksSelectionChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetVideoTracksSelectionChanged)]

Die Verwaltung von Medienelementen mit mehreren Audiotiteln funktioniert genau wie bei den Videotiteln. Behandeln Sie das [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948)-Ereignis, um Ihre Benutzeroberfläche mit den Audiotiteln aus der [**AudioTracks**](https://msdn.microsoft.com/library/windows/apps/dn930947)-Liste des Wiedergabeelements zu aktualisieren. Wenn der Benutzer einen Audiotitel auswählt, legen Sie die [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn930937)-Eigenschaft der **AudioTracks**-Liste fest, damit das **MediaElement**- oder **MediaPlayer**-Element beim aktiven Audiotitel zum angegebenen Index wechselt.

[!code-xml[AudioComboBox](./code/MediaSource_Win10/cs/MainPage.xaml#SnippetAudioComboBox)]

[!code-cs[AudioTracksChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetAudioTracksChanged)]

[!code-cs[AudioTracksSelectionChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetAudioTracksSelectionChanged)]

Neben Audio und Video kann ein **MediaPlaybackItem**-Objekt auch [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580)-Objekte enthalten. Ein zeitgesteuerter Metadatentitel kann Untertitel, Beschriftungstext oder benutzerdefinierte proprietäre Daten Ihrer App enthalten. Ein zeitgesteuerter Metadatentitel enthält eine Liste mit Markern. Diese werden durch Objekte dargestellt, die von [**IMediaCue**](https://msdn.microsoft.com/library/windows/apps/dn930899) erben (etwa [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) oder [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655)). Jeder Marker hat eine Anfangszeit und eine Dauer. Diese bestimmen den Aktivierungszeitpunkt und die Aktivierungsdauer des jeweiligen Markers.

Die zeitgesteuerten Metadatentitel für ein Medienelement können ähnlich wie Audio- und Videotitel durch Behandeln des [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952)-Ereignisses eines **MediaPlaybackItem**-Elements ermittelt werden. Bei zeitgesteuerten Metadatentiteln möchte der Benutzer aber unter Umständen mehrere Metadatentitel gleichzeitig aktivieren. Außerdem empfiehlt sich je nach App-Szenario unter Umständen eine automatische Aktivierung oder Deaktivierung von Metadatentiteln. Dieses Beispiel fügt zur Veranschaulichung ein [**ToggleButton**](https://msdn.microsoft.com/library/windows/apps/br209795)-Element für die einzelnen Metadatentitel in einem Medienelement hinzu, sodass der Benutzer den jeweiligen Titel aktivieren und deaktivieren kann. Die **Tag**-Eigenschaft der einzelnen Schaltflächen wird auf den Index des zugehörigen Metadatentitels festgelegt, sodass sie bei Betätigung der Schaltfläche identifiziert werden können.

[!code-xml[MetaStackPanel](./code/MediaSource_Win10/cs/MainPage.xaml#SnippetMetaStackPanel)]

[!code-cs[TimedMetadataTrackschanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTimedMetadataTrackschanged)]

Da gleichzeitig mehrere Metadatentitel aktiv sein können, legen Sie nicht einfach den aktiven Index für die Metadatentitelliste fest. Rufen Sie stattdessen die [**SetPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn986977)-Methode des **MediaPlaybackItem**-Objekts auf. Übergeben Sie dabei den Index des umzuschaltenden Titels, und geben Sie dann einen Wert aus der [**TimedMetadataTrackPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn987016)-Enumeration an. Der gewählte Darstellungsmodus hängt von der Implementierung Ihrer App ab. In diesem Beispiel wird der Metadatentitel bei der Aktivierung auf **PlatformPresented** festgelegt. Bei textbasierten Titeln werden dadurch automatisch die Textmarker des Titels angezeigt. Wenn die Umschaltfläche deaktiviert wird, wird der Darstellungsmodus auf **Disabled** festgelegt, sodass kein Text angezeigt wird und keine Marker-Ereignisse ausgelöst werden. Marker-Ereignisse werden weiter unten in diesem Artikel behandelt.

[!code-cs[ToggleChecked](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetToggleChecked)]

[!code-cs[ToggleUnchecked](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetToggleUnchecked)]

Während der Verarbeitung der Metadatentitel können Sie auf den Satz von Markern im Titel zugreifen, indem Sie auf die [**Cues**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.TimedMetadataTrack.Cues) oder [**ActiveCues**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.TimedMetadataTrack.ActiveCues)-Eigenschaften zugreifen. Sie können dies tun, um die Benutzeroberfläche zu aktualisieren, damit die Markerspeicherorte für ein Medienelement angezeigt werden.

## Behandeln von nicht unterstützten Codecs und unbekannten Fehler beim Öffnen von Medienelementen
Ab Windows10, Version1607, können Sie überprüfen, ob der Codec zur Wiedergabe eines Medienelements auf dem Gerät unterstützt oder teilweise unterstützt wird, auf dem Ihre App ausgeführt wird. Überprüfen Sie zunächst im Ereignishandler für **MediaPlaybackItem**-Ereignisse mit geänderten Spuren, wie [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.AudioTracksChanged), ob sich bei der Änderung des Titels um das Einfügen eines neuen Titels handelt. Wenn dies der Fall ist, können Sie einen Verweis auf den Titel abrufen, der eingefügt wird, indem Sie den Index verwenden, der in den **IVectorChangedEventArgs.Index**-Parameter mit der entsprechenden Titelsammlung des **MediaPlaybackItem**-Parameter übergeben wurde, z.B. die [**AudioTracks**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.AudioTracks)-Auflistung.

Wenn Sie einen Verweis auf den Titel eingefügt haben, überprüfen Sie den [**DecoderStatus**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrackSupportInfo.DecoderStatus) der [**SupportInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.SupportInfo)-Eigenschaft des Titels. Wenn der Wert [**FullySupported**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus) ist, dann ist der für die Wiedergabe des Titels erforderliche Codec auf dem Gerät vorhanden. Wenn der Wert [**Degraded**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus) ist, kann der Titel vom System wiedergegeben werden, die Wiedergabe wird jedoch zu einem gewissen Grad verschlechtert. Beispielsweise kann ein 5.1-Audiotitel auch als 2-Kanal-Stereo wiedergegeben werden. Wenn dies der Fall ist, sollten Sie die UI aktualisieren, um den Benutzer über die Beeinträchtigung zu informieren. Wenn der Wert [**UnsupportedSubtype**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus) oder [**UnsupportedEncoderProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus) ist, kann der Titel überhaupt nicht mit den aktuell auf dem Gerät vorhandenen Codecs wiedergegeben werden. Sie sollten den Benutzer warnen und die Wiedergabe des Elements überspringen oder UI-Elemente implementieren, um dem Benutzer den Download des richtigen Codec zu ermöglichen. Die [**GetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.GetEncodingProperties)-Methode des Titels kann verwendet werden, um den erforderlichen Codec für die Wiedergabe zu ermitteln.

Sie können schließlich eine Registrierung für das [**OpenFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.OpenFailed)-Ereignis des Titels ausführen, das ausgelöst wird, wenn der Titel auf dem Gerät unterstützt wird, jedoch aufgrund eines unbekannten Fehlers in der Pipeline nicht geöffnet werden konnte.

[!code-cs[AudioTracksChanged_CodecCheck](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksChanged_CodecCheck)]

Im [**OpenFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.OpenFailed)-Ereignishandler können Sie überprüfen, ob der **MediaSource**-Status unbekannt ist. Wenn dies der Fall ist, können Sie programmgesteuert einen anderen Titel auswählen oder dem Benutzer die Auswahl eines anderen Titels bzw. die Aufgabe der Wiedergabe ermöglichen.

[!code-cs[OpenFailed](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetOpenFailed)]

## Festlegen der Anzeigeeigenschaften, die für den Medientransportsteuerelemente des Systems verwendet werden
Ab Windows10, Version1607, werden Medien, die in einem [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) wiedergegeben werden, standardmäßig automatisch in System Media Transport Controls (SMTC) integriert. Sie können die Metadaten angeben, die SMTC anzeigt, indem Sie die Anzeigeeigenschaften für ein **MediaPlaybackItem** aktualisieren. Rufen Sie ein Objekt ab, das die Anzeigeeigenschaften für ein Element darstellt, indem Sie [**GetDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.GetDisplayProperties) aufrufen. Legen Sie fest, ob das Wiedergabeelement Musik oder Video ist, indem Sie die [**Type**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.Type)-Eigenschaft festlegen. Legen Sie anschließend die Eigenschaften von [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.VideoProperties) oder [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.MusicProperties) des Objekts fest. Rufen Sie [**ApplyDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/mt489923) auf, um die Eigenschaften des Elements auf die von Ihnen angegebenen Werte zu aktualisieren bzw. festzulegen. In der Regel ruft eine App die Anzeigewerte dynamisch aus einem Webdienst ab. Das folgende Beispiel zeigt diesen Vorgang jedoch anhand hartcodierter Werte.

[!code-cs[SetVideoProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetVideoProperties)]

[!code-cs[SetMusicProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetMusicProperties)]

## Hinzufügen von externem zeitgesteuerten Text mit „TimedTextSource“

Gelegentlich liegen unter Umständen externe Dateien mit zeitgesteuertem Text für ein Medienelement vor – beispielsweise Dateien mit Untertiteln für verschiedene Gebietsschemas. Mit der [**TimedTextSource**](https://msdn.microsoft.com/library/windows/apps/dn956679)-Klasse können Sie externe zeitgesteuerte Textdateien aus einem Datenstrom oder URI laden.

In diesem Beispiel wird eine **Dictionary**-Sammlung zum Speichern einer Quellenliste mit zeitgesteuertem Text für das Medienelement verwendet. Dabei fungieren der Quell-URI und das **TimedTextSource**-Objekt als Schlüssel-Wert-Paar zum Identifizieren der aufgelösten Titel.

[!code-cs[TimedTextSourceMap](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTimedTextSourceMap)]

Rufen Sie [**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn708190) auf, um für jede externe Datei mit zeitgesteuertem Text ein neues **TimedTextSource**-Element zu erstellen. Fügen Sie dem **Dictionary**-Element einen Eintrag für die Quelle mit zeitgesteuertem Text hinzu. Fügen Sie einen Handler für das [**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540)-Ereignis hinzu, um einen möglichen Fehler beim Laden des Elements zu behandeln oder nach dem erfolgreichen Laden des Elements weitere Eigenschaften festzulegen.

Registrieren Sie alle Ihre **TimedTextSource**-Objekte beim **MediaSource**-Element, indem Sie sie der [**ExternalTimedTextSources**](https://msdn.microsoft.com/library/windows/apps/dn930916)-Sammlung hinzufügen. Beachten Sie, dass externe Quellen mit zeitgesteuertem Text nicht dem auf der Grundlage der Quelle erstellten **MediaSource**-Element, sondern direkt dem **MediaPlaybackItem**-Element hinzugefügt werden. Registrieren und behandeln Sie das **TimedMetadataTracksChanged**-Ereignis wie zuvor in diesem Artikel beschrieben, um Ihre UI mit den externen Texttiteln zu aktualisieren.

[!code-cs[TimedTextSource](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTimedTextSource)]

Ermitteln Sie im Handler für das [**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540)-Ereignis anhand der **Error**-Eigenschaft des an den Handler übergebenen [**TimedTextSourceResolveResultEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn965537)-Elements, ob beim Laden der zeitgesteuerten Textdaten ein Fehler aufgetreten ist. Wurde das Element erfolgreich aufgelöst, können Sie mit diesem Handler zusätzliche Eigenschaften des aufgelösten Titels aktualisieren. In diesem Beispiel wird auf der Grundlage des zuvor im **Dictionary**-Element gespeicherten URI für jede Spur eine Beschriftung hinzufügt.

[!code-cs[TimedTextSourceResolved](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTimedTextSourceResolved)]

## Hinzufügen zusätzlicher Metadatentitel

Sie können im Code dynamisch benutzerdefinierte Metadatentitel erstellen und sie einer Medienquelle zuordnen. Die erstellten Titel können Untertitel, Beschriftungstext oder proprietäre App-Daten enthalten.

Erstellen Sie ein neues [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580)-Element. Rufen Sie hierzu den Konstruktor auf, und geben Sie eine ID, die Sprachen-ID und einen Wert aus der [**TimedMetadataKind**](https://msdn.microsoft.com/library/windows/apps/dn956578)-Enumeration an. Registrieren Sie Handler für die Ereignisse [**CueEntered**](https://msdn.microsoft.com/library/windows/apps/dn956583) und [**CueExited**](https://msdn.microsoft.com/library/windows/apps/dn956584). Diese Ereignisse werden ausgelöst, wenn die Startzeit für einen Marker erreicht wurde und wenn die Dauer für einen Marker abgelaufen ist.

Erstellen Sie ein neues, geeignetes Marker-Objekt für die Art des erstellten Metadatentitels, und legen Sie ID, Startzeit und Dauer für den Titel fest. Dieses Beispiel erstellt eine Datenspur. Es wird also eine Reihe von [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892)-Objekten erstellt, und für die einzelnen Marker wird jeweils ein Puffer mit App-spezifischen Daten bereitgestellt. Fügen Sie den neuen Titel der [**ExternalTimedMetadataTracks**](https://msdn.microsoft.com/library/windows/apps/dn930915)-Sammlung des **MediaSource**-Objekts hinzu, um ihn zu registrieren.

[!code-cs[AddDataTrack](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetAddDataTrack)]

Das **CueEntered**-Ereignis wird ausgelöst, wenn die Startzeit eines Markers erreicht wurde und der zugehörige Titel den Präsentationsmodus **ApplicationPresented**, **Hidden** oder **PlatformPresented** hat. Marker-Ereignisse werden nicht für Metadatentitel ausgelöst, wenn der Präsentationsmodus für den Titel **Disabled** ist. Dieses Beispiel gibt einfach die benutzerdefinierten, dem Marker zugeordneten Daten im Debugfenster aus.

[!code-cs[DataCueEntered](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetDataCueEntered)]

Dieses Beispiel fügt einen benutzerdefinierten Texttitel hinzu. Hierzu wird beim Erstellen des Titels **TimedMetadataKind.Caption** angegeben. Außerdem werden dem Titel mithilfe von [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655)-Objekten Marker hinzugefügt.

[!code-cs[AddTextTrack](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetAddTextTrack)]

[!code-cs[TextCueEntered](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTextCueEntered)]

## Wiedergeben einer Liste mit Medienelementen mit „MediaPlaybackList“

Mithilfe des [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955)-Elements können Sie eine Wiedergabeliste mit Medienelementen (dargestellt durch **MediaPlaybackItem**-Objekte) erstellen.

**Hinweis**  Elemente in einer [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) werden mit lückenloser Wiedergabe gerendert. Das System verwendet die in MP3- oder AAC-codierten Dateien bereitgestellten Metadaten, um die für die lückenlose Wiedergabe erforderliche Verzögerungs- oder Auffüllkorrektur (Delay/Padding) zu ermitteln. Werden diese Metadaten von den MP3- oder AAC-codierten Dateien nicht bereitgestellt, ermittelt das System Verzögerungen und Auffüllungen heuristisch. Bei verlustfreien Formaten wie PCM, FLAC oder ALAC ergreift das System keine Maßnahme, da diese Encoder keine Verzögerungen oder Auffüllungen verursachen.

Deklarieren Sie zunächst eine Variable zum Speichern Ihres **MediaPlaybackList**-Elements.

[!code-cs[DeclareMediaPlaybackList](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackList)]

Erstellen Sie mithilfe des weiter oben beschriebenen Verfahrens für jedes Medienelement, das Sie Ihrer Liste hinzufügen möchten, ein **MediaPlaybackItem**-Element. Initialisieren Sie Ihr **MediaPlaybackList**-Objekt, und fügen Sie die Medienwiedergabeelemente hinzu. Registrieren Sie einen Handler für das [**CurrentItemChanged**](https://msdn.microsoft.com/library/windows/apps/dn930957)-Ereignis. Mit diesem Ereignis können Sie Ihre UI mit dem derzeit wiedergegebenen Medienelement aktualisieren. Legen Sie abschließend die Wiedergabequelle des **MediaPlayer**  auf ihre **MediaPlaybackList** fest.

[!code-cs[PlayMediaPlaybackList](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackList)]

Aktualisieren Sie im **CurrentItemChanged**-Ereignishandler Ihre UI mit dem derzeit wiedergegebenen Element. Dieses kann mithilfe der [**NewItem**](https://msdn.microsoft.com/library/windows/apps/dn930930)-Eigenschaft des an das Ereignis übergebenen [**CurrentMediaPlaybackItemChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn930929)-Objekts abgerufen werden. Zur Erinnerung: Wenn Sie die UI auf der Grundlage dieses Ereignisses aktualisieren, müssen Sie dies innerhalb eines Aufrufs von [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) tun, damit die Aktualisierungen im UI-Thread vorgenommen werden.

> [!NOTE] 
> Das System entfernt Medienelemente nicht automatisch, nachdem sie wiedergegeben wurden. Wenn der Benutzer rückwärts durch die Liste navigiert, können daher zuvor wiedergegebene Titel erneut ohne Lücke wiedergegeben werden. Dies bedeutet jedoch, dass die Speichernutzung der App zunimmt, je mehr Titel gespielt werden. Sie müssen sicherstellen, dass die Ressourcen für zuvor wiedergegebenen Medienelemente in regelmäßigen Abständen freigegeben werden. Dies ist besonders wichtig, wenn Ihre App im Hintergrund wiedergegeben wird und die Ressourcen stärker eingeschränkt sind. 

Sie können das **CurrentItemChanged**-Ereignis nutzen, um die Ressourcen für zuvor wiedergegebene Medienelemente freizugeben. Um einen Verweis auf zuvor wiedergegebene Elemente zu behalten, erstellen Sie eine **Queue**-Auflistung. Sie legen außerdem eine Variable fest, die die maximale Anzahl von Medienelementen festlegt, die im Arbeitsspeicher gespeichert werden dürfen. Fügen Sie im Ereignishandler einen Verweis auf das zuvor wiedergegebene Element hinzu, fügen Sie diesen der Warteschlange hinzu und entfernen Sie den ältesten Eintrag aus der Warteschlange. Rufen Sie [**Reset**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource.Reset) für das zurückgegebene Element auf, um dessen Ressourcen freizugeben. Stellen Sie zuvor jedoch sicher, dass es sich nicht mehr in der Warteschlange befindet oder gerade wiedergegeben wird, um Fälle zu behandeln, in denen das Element mehrmals wiedergegeben wird.

[!code-cs[DeclareItemQueue](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareItemQueue)]

[!code-cs[MediaPlaybackListItemChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetMediaPlaybackListItemChanged)]

Rufen Sie [**MovePrevious**](https://msdn.microsoft.com/library/windows/apps/mt146455) oder [**MoveNext**](https://msdn.microsoft.com/library/windows/apps/mt146454) auf, um bei der MediaPlayer-Wiedergabe zum vorherigen oder nächsten Element Ihres **MediaPlaybackList**-Elements zu wechseln.

[!code-cs[PrevButton](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetPrevButton)]

[!code-cs[NextButton](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetNextButton)]

Geben Sie durch Festlegen der [**ShuffleEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146457)-Eigenschaft an, ob der Media Player die Elemente in Ihrer Liste in zufälliger Reihenfolge wiedergeben soll.

[!code-cs[ShuffleButton](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetShuffleButton)]

Geben Sie durch Festlegen der [**AutoRepeatEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146452)-Eigenschaft an, ob der Media Player die Wiedergabe Ihrer Liste automatisch wiederholen soll.

[!code-cs[RepeatButton](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetRepeatButton)]


###Behandeln des Ausfalls von Medienelementen in einer Wiedergabeliste
Das [**ItemFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList.ItemFailed)-Ereignis wird ausgelöst, wenn ein Element in der Liste nicht geöffnet wird. Die [**ErrorCode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItemError.ErrorCode)-Eigenschaft des [**MediaPlaybackItemError**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItemError)-Objekts, das an den Ereignishandler übergeben wird, listet die genaue Ursache des Fehlers auf, wenn möglich, einschließlich Netzwerkfehlern, Decodierungsfehlern oder Verschlüsselungsfehlern.

[!code-cs[ItemFailed](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetItemFailed)]

## Verwandte Themen
* [Medienwiedergabe](media-playback.md)
* [Wiedergeben von Audio- und Videoinhalten mit „MediaPlayer“](play-audio-and-video-with-mediaplayer.md)
* [Integrieren in die Steuerelemente für den Systemmedientransport](integrate-with-systemmediatransportcontrols.md)
* [Wiedergeben von Medien im Hintergrund](background-audio.md)




<!--HONumber=Aug16_HO3-->


