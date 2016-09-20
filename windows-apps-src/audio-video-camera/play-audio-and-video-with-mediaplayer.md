---
author: drewbatgit
ms.assetid: 
description: "In diesem Artikel erfahren Sie, wie Sie in Ihrer universellen Windows-App mithilfe von „MediaPlayer“ Medien wiedergeben."
title: "Wiedergeben von Audio- und Videoinhalten mit „MediaPlayer“"
translationtype: Human Translation
ms.sourcegitcommit: 3d6f79ea55718d988415557bc4ac9a1f746f9053
ms.openlocfilehash: 32df2810710e78eeb8c257548c39c0d5d978e888

---

# Wiedergeben von Audio- und Videoinhalten mit „MediaPlayer“

In diesem Artikel erfahren Sie, wie Sie in Ihrer universellen Windows-App mithilfe der [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer)-Klasse Medien wiedergeben. In Windows10, Version1607, wurden wesentliche Verbesserungen an den APIs zur Medienwiedergabe vorgenommen, unter anderem ein vereinfachtes Einzelprozessdesign für die Audiowiedergabe im Hintergrund, automatische Integration in die Steuerelemente für den Systemmedientransport (System Media Transport Controls, SMTC), die Möglichkeit zum Synchronisieren mehrerer Media Player, die Option einer Windows.UI.Composition-Oberfläche und eine anwenderfreundliche Schnittstelle zum Erstellen und Planen von Medienunterbrechungen in Ihren Inhalten. Um diese Verbesserungen optimal zu nutzen, sollten Sie für die Medienwiedergabe anstelle von **MediaElement** die **MediaPlayer**-Klasse verwenden. Das einfache XAML-Steuerelement [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) wurde eingeführt, damit Sie Medieninhalte auf einer XAML-Seite rendern können. Viele der von **MediaElement** bereitgestellten Wiedergabesteuerelemente und Status-APIs sind jetzt über das neue [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession)-Objekt verfügbar. **MediaElement** gewährleistet weiterhin die Abwärtskompatibilität. Dieser Klasse werden jedoch keine weiteren Features hinzugefügt.

Dieser Artikel erläutert die Features von **MediaPlayer**, die von einer typischen App zur Medienwiedergabe verwendet werden. Beachten Sie, dass **MediaPlayer** die [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource)-Klasse als Container für alle Medienelemente verwendet. Diese Klasse ermöglicht Ihnen das Laden und Wiedergeben von Medien aus verschiedenen Quellen – z.B. aus lokalen Dateien, Speicherdatenströmen und Netzwerkquellen – über die gleiche Schnittstelle. Verschiedene Klassen auf höherer Ebene arbeiten ebenfalls mit **MediaSource**, z.B. [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) und [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList). Sie stellen erweiterte Features bereit wie Wiedergabelisten und die Möglichkeit, Medienquellen mit mehreren Audio-, Video- und Metadatentiteln zu verwalten, Weitere Informationen zu **MediaSource** und verwandten APIs finden Sie unter [Medienelemente, Wiedergabelisten und Titel](media-playback-with-mediasource.md).


##Wiedergeben einer Mediendatei mit „MediaPlayer“  
Die grundlegende Medienwiedergabe mit **MediaPlayer** ist sehr einfach zu implementieren. Erstellen Sie zunächst eine neue Instanz der **MediaPlayer**-Klasse. In Ihrer App können mehrere **MediaPlayer**-Instanzen gleichzeitig aktiv sein. Legen Sie dann für die [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source)-Eigenschaft des Players ein Objekt fest, welches die [**IMediaPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.IMediaPlaybackSource) implementiert, z.B. eine [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource), ein [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) oder eine [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList). In diesem Beispiel wird ein **MediaSource**-Objekt aus einer Datei im lokalen Speicher der App erstellt. Anschließend wird aus der Quelle ein **MediaPlaybackItem**-Objekt erstellt und der **Source**-Eigenschaft des Players zugewiesen.

Anders als **MediaElement** startet **MediaPlayer** nicht standardmäßig automatisch mit der Wiedergabe. Sie können die Wiedergabe starten, indem Sie [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play) aufrufen, für die [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay) -Eigenschaft „true“ festlegen oder warten, bis der Benutzer die Wiedergabe mit den integrierten Media-Steuerelementen startet.

[!code-cs[SimpleFilePlayback](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSimpleFilePlayback)]

Wenn Ihre App die Verwendung eines **MediaPlayers** beendet hat, sollten Sie die Methode[**Close**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Close) aufrufen (**Dispose**-Methode in C#), um die vom Player verwendeten Ressourcen zu bereinigen.

[!code-cs[CloseMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCloseMediaPlayer)]

##Rendern von Videos in XAML mit MediaPlayerElement
Sie können Medien in einem **MediaPlayer** wiedergeben, ohne sie in XAML anzuzeigen. Viele Medienwiedergabe-Apps versuchen jedoch, die Medien auf einer XAML-Seite zu rendern. Verwenden Sie hierfür das einfache [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement)-Steuerelement. Wie mit **MediaElement** können Sie mit **MediaPlayerElement** festlegen, ob die integrierten Transport-Steuerelemente angezeigt werden sollen.

[!code-xml[MediaPlayerElementXAML](./code/MediaPlayer_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

Sie können die **MediaPlayer** -Instanz festlegen, an die das Element gebunden ist, indem Sie [**SetMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/mt708764) aufrufen.

[!code-cs[SetMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetMediaPlayer)]

Sie können für das **MediaPlayerElement** auch die Wiedergabequelle festlegen. Das Element erstellt dann automatisch eine neue **MediaPlayer**-Instanz, auf die Sie mithilfe der [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.MediaPlayer)-Eigenschaft zugreifen können.

[!code-cs[GetPlayerFromElement](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetGetPlayerFromElement)]

##Allgemeine MediaPlayer-Aufgaben
In diesem Abschnitt erfahren Sie, wie Sie verschiedene Features des **MediaPlayers** verwenden.

###Festlegen der AudioCategory-Eigenschaft
Legen Sie für die [**Audiocategory** ](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioCategory)-Eigenschaft eines **MediaPlayers** einen der Werte der [**MediaPlayerAudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerAudioCategory)-Enumeration fest, um dem System mitzuteilen, welche Art von Medien Sie wiedergeben. Spiele sollten als Kategorie ihrer Musikdatenströme **GameMedia** angeben, sodass die Musik des Spiels automatisch auf stumm geschaltet wird, wenn eine andere Anwendung im Hintergrund Musik wiedergibt. Musik- oder Video-Apps sollten als Kategorien für ihre Datenströme **Media** oder **Movie** angeben, sodass ihnen gegenüber **GameMedia**-Datenströmen Priorität eingeräumt wird.

[!code-cs[SetAudioCategory](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioCategory)]

###Ausgabe an einen bestimmten Audio-Endpunkt
Die Audioausgabe eines **MediaPlayers** wird standardmäßig zum Standard-Audio-Endpunkt des Systems geleitet. Sie können jedoch auch einen bestimmten Audio-Endpunkt als Ausgabe für den **MediaPlayer** festlegen. Im folgenden Beispiel gibt [**MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.MediaDevice.GetAudioRenderSelector) eine Zeichenfolge zur eindeutigen Identifizierung der Audiorendering-Kategorie von Geräten zurück. Als Nächstes wird die [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation)-Methode [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation.FindAllAsync) aufgerufen, um eine Liste aller verfügbaren Geräte des ausgewählten Typs zu erstellen. Sie können programmgesteuert festlegen, welches Gerät Sie verwenden möchten, oder die zurückgegebenen Geräte zu einer [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ComboBox) hinzufügen, damit der Benutzer ein Gerät auswählen kann.

[!code-cs[SetAudioEndpointEnumerate](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpointEnumerate)]

Im [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.Selector.SelectionChanged)-Ereignis für das Geräte-Kombinationsfeld wird die [**AudioDevice**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioDevice)-Eigenschaft des **MediaPlayers** auf das ausgewählte Gerät festgelegt, die in der [**Tag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.Tag)-Eigenschaft des **ComboBoxItem** gespeichert wurde.

[!code-cs[SetAudioEndpontSelectionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpontSelectionChanged)]

###Wiedergabesitzung
Wie zuvor in diesem Artikel beschrieben, wurden viele der von der **MediaElement**-Klasse verfügbar gemachten Funktionen in die [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession)-Klasse verschoben. Dazu gehören Informationen über den Wiedergabestatus des Players, z.B. die aktuelle Wiedergabeposition, ob der Player Medien wiedergibt bzw. angehalten wurde sowie die aktuelle Wiedergabegeschwindigkeit. **MediaPlaybackSession** stellt außerdem einige Ereignisse bereit, um Sie bei Statusänderungen zu benachrichtigen. Dazu gehören der aktuelle Puffer- und Download-Status der wiedergegebenen Inhalte sowie die natürliche Größe und das Seitenverhältnis des aktuell wiedergegebenen Videoinhalts.

Das folgende Beispiel zeigt, wie Sie einen Klickhandler für Schaltflächen implementieren können, der bei der Medienwiedergabe 10 Sekunden überspringt. Zuerst wird das **MediaPlaybackSession**-Objekt für den Player mit der [**PlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.PlaybackSession)-Eigenschaft abgerufen. Als Nächstes wird die [**Position**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.Position)-Eigenschaft auf die aktuelle Wiedergabeposition plus 10 Sekunden festgelegt.

[!code-cs[SkipForwardClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSkipForwardClick)]

Das nächste Beispiel zeigt, wie durch Einstellen der [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.PlaybackRate)-Eigenschaft der Sitzung mithilfe einer Schaltfläche zwischen der normalen Wiedergabegeschwindigkeit und zweifacher Geschwindigkeit gewechselt werden kann.

[!code-cs[SpeedChecked](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSpeedChecked)]

###Zwei-Finger-Zoomen von Video
**MediaPlayer** ermöglicht Ihnen, im Videoinhalt das zu rendernde Quellrechteck festzulegen und so in das Video hineinzuzoomen. Das angegebene Rechteck bezieht sich auf ein normalisiertes Rechteck (0,0,1,1) wobei 0,0 der oberen linken Ecke des Frames entspricht und 1,1 die volle Breite und Höhe des Frames angibt. Um beispielsweise das Zoomrechteck so festzulegen, dass der obere rechte Quadrant des Videos gerendert wird, müssten Sie das Rechteck wie folgt angeben: (.5,0,.5,.5).  Es ist wichtig, die Werte zu überprüfen, um sicherzustellen, dass Ihr Quellrechteck sich innerhalb des normalisierten Rechtecks (0,0,1,1) befindet. Durch den Versuch, einen Wert außerhalb dieses Bereichs festzulegen, wird eine Ausnahme ausgelöst.

Um den Zwei-Finger-Zoom mithilfe von Multitouchbewegungen zu implementieren, müssen Sie zunächst angeben, welche Gesten unterstützt werden sollen. In diesem Beispiel wurden Gesten zum Skalieren und Übersetzen angefordert. Das [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.ManipulationDelta)-Ereignis wird ausgelöst, wenn eine der abonnierten Gesten auftritt. Das [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped)-Ereignis wird verwendet, um den Zoom auf den vollständigen Frame zurückzusetzen. 

[!code-cs[RegisterPinchZoomEvents](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPinchZoomEvents)]

Deklarieren Sie als Nächstes ein **Rect**-Objekt, welches das aktuelle Zoom-Quellrechteck speichert.

[!code-cs[DeclareSourceRect](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareSourceRect)]

Der **ManipulationDelta**-Handler passt die Skalierung oder die Übersetzung des Zoom-Rechtecks an. Ist der Deltawert für die Skalierung nicht 1, bedeutet dies, dass der Benutzer eine Zwei-Finger-Zoom-Geste ausgeführt hat. Wenn der Wert größer als 1 ist, muss das Quellrechteck verkleinert werden, um den Inhalt zu vergrößern. Wenn der Wert kleiner als 1 ist, sollte das Quellrechteck vergrößert werden, um den Inhalt zu verkleinern. Vor Festlegen der neuen Skalierungswerte wird das resultierende Rechteck überprüft, um sicherzustellen, dass es vollständig innerhalb der Grenzen von (0,0,1,1) liegt.

Wenn der Skalierungswert 1 ist, wird die Übersetzungsgeste behandelt. Das Rechteck wird wie folgt übersetzt: Anzahl der Pixel in der Geste geteilt durch die Breite und Höhe des Steuerelements. Wieder wird das resultierende Rechteck überprüft, um sicherzustellen, dass es innerhalb der Grenzen von (0,0,1,1) liegt.

Schließlich wird die [**NormalizedSourceRect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NormalizedSourceRect)-Eigenschaft der **MediaPlaybackSession** auf das neu angepasste Rechteck festgelegt. Dabei wird der Bereich innerhalb des Video-Frames angegeben, der gerendert werden soll.

[!code-cs[ManipulationDelta](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

Im [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped)-Ereignishandler wird das Quellrechteck wieder auf (0,0,1,1) festgelegt, damit der gesamte Videoframe gerendert wird.

[!code-cs[DoubleTapped](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDoubleTapped)]
        
##Rendern von Videos auf einer Windows.UI.Composition-Oberfläche mit MediaPlayerSurface
Ab Windows10, Version1607, können Sie mit **MediaPlayer** Videos auf einer [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ICompositionSurface) rendern. Dadurch kann der Player mit den APIs im [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition)-Namespace verwendet werden. Das Kompositions-Framework ermöglicht Ihnen, auf der visuellen Ebene zwischen XAML und den DirectX-Grafik-APIs auf niedriger Ebene Grafiken zu verwenden. Dies ermöglicht Szenarien wie das Rendering von Videos in alle XAML-Steuerelemente. Weitere Informationen zur Verwendung der Composition-APIs finden Sie unter [visuelle Ebene](https://msdn.microsoft.com/windows/uwp/graphics/visual-layer).

Das folgende Beispiel veranschaulicht das Rendern von Inhalten des Videoplayers auf ein [**Canvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Canvas)-Steuerelement. Die mediaplayerspezifischen Aufrufe in diesem Beispiel sind [**SetSurfaceSize**](https://msdn.microsoft.com/library/windows/apps/mt489968) und [**GetSurface**](https://msdn.microsoft.com/library/windows/apps/mt489963). **SetSurfaceSize** teilt dem System die Größe des Puffers mit, der für das Rendern von Inhalten zugeordnet werden muss. **GetSurface** verwendet einen [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.Compositor) als Argument und ruft eine Instanz der [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface)-Klasse ab. Diese Klasse ermöglicht den Zugriff auf den **MediaPlayer** und den **Compositor**, mit denen die Oberfläche erstellt wird. Die Klasse macht außerdem die Oberfläche selbst über die [**CompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface.CompositionSurface)-Eigenschaft verfügbar.

Der restliche Code in diesem Beispiel erstellt ein [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual)-Element, in den das Video gerendert wird, und legt als Größe die Größe des Canvas-Elements fest, das das Visual anzeigt. Als Nächstes wird ein [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.CompositionBrush) aus der [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface) erstellt und der [**Brush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual.Brush)-Eigenschaft des Visuals zugeordnet. Dann wird ein [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ContainerVisual) erstellt, und das **SpriteVisual** wird auf der oberen Ebene der visuellen Struktur eingefügt. Schließlich wird [**SetElementChildVisual**](https://msdn.microsoft.com/library/windows/apps/mt608981) aufgerufen, um das Container-Visual dem **Canvas** zuzuordnen.

[!code-cs[Compositor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCompositor)]
        
##Synchronisieren von Inhalten zwischen mehreren Playern mit MediaTimelineController
Wie in diesem Artikel bereits erläutert, können in Ihrer App mehrere **MediaPlayer**-Objekte gleichzeitig aktiv sein. Standardmäßig funktioniert jeder von Ihnen erstellte **MediaPlayer** unabhängig. In einigen Szenarien, z.B. beim Synchronisieren einer Kommentarspur mit einem Video, müssen möglicherweise der Status des Players, die Wiedergabeposition und die Wiedergabegeschwindigkeit von mehreren Playern synchronisiert werden. Ab Windows10, Version1607, können Sie dieses Verhalten mithilfe der [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController)-Klasse implementieren.

###Implementieren der Wiedergabe-Steuerelemente
Das folgende Beispiel zeigt, wie Sie mit einem **MediaTimelineController** zwei Instanzen des **MediaPlayers** steuern können. Zuerst werden alle Instanzen des **MediaPlayers** instanziiert und eine Mediendatei als **Source** festgelegt. Als Nächstes wird eine neue **MediaTimelineController**-Klasse erstellt. Bei jedem **MediaPlayer** wird der mit den einzelnen Playern verknüpfte [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) deaktiviert, indem die [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled)-Eigenschaft auf „false“ festgelegt wird. Anschließend wird die [**TimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineController)-Eigenschaft auf de Zeitachsencontroller festgelegt.

[!code-cs[DeclareMediaTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaTimelineController)]

[!code-cs[SetTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetTimelineController)]

**Achtung** Die [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager)-Klasse stellt eine automatische Integration zwischen **MediaPlayer** und den Steuerelementen für den Systemmedientransport (System Media Transport Controls, SMTC) bereit. Diese automatische Integration kann jedoch nicht für Media Player verwendet werden, die über eine **MediaTimelineController**-Klasse gesteuert werden. Daher müssen Sie vor dem Festlegen des Zeitachsencontrollers des Players den Befehlsmanager des Media Players deaktivieren. Andernfalls wird eine Ausnahme mit der Benachrichtigung ausgelöst, dass das Anfügen des Medienzeitachsencontrollers im aktuellen Objektzustand blockiert wird. Weitere Informationen zur Integration des Media Players in die SMTC finden Sie unter [Integration in die Steuerelemente für den Systemmedientransport](integrate-with-systemmediatransportcontrols.md). Auch wenn Sie eine **MediaTimelineController**-Klasse verwenden, können Sie die SMTC weiterhin manuell steuern. Weitere Informationen finden Sie unter [Manuelle Steuerung der Steuerelemente für den Systemmedientransport](system-media-transport-controls.md).

Nachdem Sie eine **MediaTimelineController**-Klasse einem oder mehreren Media Player zugewiesen haben, können Sie den Wiedergabestatus mit den vom Controller bereitgestellten Methoden steuern. Im folgenden Beispiel wird [**Start**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController.Start) aufgerufen, um die Wiedergabe aller zugeordneten Media Player zu Beginn des Mediums zu starten.

[!code-cs[PlayButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPlayButtonClick)]

Dieses Beispiel veranschaulicht das Anhalten und Fortsetzen aller zugeordneten Media Player.

[!code-cs[PauseButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPauseButtonClick)]

Für den schnellen Vorlauf aller verbundenen Media Player muss die Wiedergabegeschwindigkeit auf einen Wert größer als 1 festgelegt werden.

[!code-cs[FastForwardButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetFastForwardButtonClick)]

Das nächste Beispiel zeigt die Verwendung eines **Slider**-Steuerelements, um die aktuelle Wiedergabeposition des Zeitachsencontrollers in Relation zum Inhalt eines der verbundenen Media Player anzuzeigen. Zunächst wird eine neue **MediaSource** erstellt und ein Handler für das [**OpenOperationCompleted**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource.OpenOperationCompleted)-Ereignis der Medienquelle registriert. 

[!code-cs[CreateSourceWithOpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCreateSourceWithOpenCompleted)]

Der **OpenOperationCompleted**-Handler bietet eine Möglichkeit, um die Dauer des Inhalts der Medienquelle festzustellen. Sobald die Dauer bestimmt ist, wird der Höchstwert des **Slider**-Steuerelements auf die Gesamtzahl der Sekunden des Medienelements festgelegt. Der Wert wird innerhalb eines Aufrufs von [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) festgelegt, um sicherzustellen, dass es im UI-Thread ausgeführt wird.

[!code-cs[DeclareDuration](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareDuration)]

[!code-cs[OpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOpenCompleted)]

Als Nächstes wird ein Handler für das [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController.PositionChanged)-Ereignis des Zeitachsencontrollers registriert. Dieses wird in regelmäßigen Abständen durch das System aufgerufen, ungefähr viermal pro Sekunde.

[!code-cs[RegisterPositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPositionChanged)]

Im Handler für **PositionChanged** wird der Schiebereglerwert aktualisiert, sodass er die aktuelle Position des Zeitachsencontrollers wiedergibt.

[!code-cs[PositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPositionChanged)]

###Versetzen der Wiedergabeposition in Relation zur Zeitachsenposition
In einigen Fällen soll möglicherweise die Wiedergabeposition eines oder mehrerer mit einem Zeitachsencontroller verknüpften Media Player in Relation zu den anderen Playern versetzt werden. Dafür können Sie die [**TimelineControllerPositionOffset**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineControllerPositionOffset)-Eigenschaft des **MediaPlayer**-Objekts festlegen, welches versetzt werden soll. Im folgenden Beispiel wird anhand der jeweiligen Dauer des Inhalts von zwei Media Playern der Minimal- und Maximalwert von zwei Schieberegler-Steuerelementen festgelegt, um die Länge des Elements zu verkürzen bzw. zu verlängern.  

[!code-cs[OffsetSliders](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOffsetSliders)]

Im [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.RangeBase.ValueChanged)-Ereignis jedes Schiebereglers wird **TimelineControllerPositionOffset** für jeden Player auf den entsprechenden Wert festgelegt.

[!code-cs[TimelineOffset](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetTimelineOffset)]

Beachten Sie: Wird der Offsetwert eines Players einer negativen Wiedergabeposition zugeordnet, wird der Clip angehalten, bis der Offset den Wert Null erreicht. Anschließend beginnt die Wiedergabe. Wenn der Offsetwert einer Wiedergabeposition zugeordnet ist, welche die Dauer des Medientitels überschreitet, wird entsprechend das letzte Bild angezeigt. Dies entspricht dem Vorgehen, wenn ein einzelner Media Player das Ende der Inhaltswiedergabe erreicht.

## Verwandte Themen
* [Medienwiedergabe](media-playback.md)
* [Medienelemente, Wiedergabelisten und Titel](media-playback-with-mediasource.md)
* [Integration in die Steuerelemente für den Systemmedientransport](integrate-with-systemmediatransportcontrols.md)
* [Erstellen, Planen und Verwalten von Medienunterbrechungen](create-schedule-and-manage-media-breaks.md)
* [Wiedergeben von Medien im Hintergrund](background-audio.md)





 

 







<!--HONumber=Aug16_HO3-->


