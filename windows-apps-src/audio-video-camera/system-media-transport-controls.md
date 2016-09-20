---
author: drewbatgit
ms.assetid: EFCF84D0-2F4C-454D-97DA-249E9EAA806C
description: "Mit der SystemMediaTransportControls-Klasse kann Ihre App die Steuerelemente für den Systemmedientransport verwenden, die in Windows integriert sind, und die Metadaten aktualisieren, die die Steuerelemente zu den von der App aktuell wiedergegebenen Medien anzeigen."
title: "Manuelle Steuerung der Steuerelemente für den Systemmedientransport"
translationtype: Human Translation
ms.sourcegitcommit: 2cf432bc9d6eb0e564b6d6aa7fdbfd78c7eef272
ms.openlocfilehash: 6643f6bee55c1c9631ca20d2fe7eb6ac1c5ae3e2

---

# Manuelle Steuerung der Steuerelemente für den Systemmedientransport

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \].

Ab Windows10, Version1607, werden UWP-Apps, welche die [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer)-Klasse für die Medienwiedergabe verwenden, standardmäßig und automatisch in die Steuerelemente für den Systemmedientransport (System Media Transport Controls, SMTC) integriert. Dies ist für die meisten Szenarien die empfohlene Methode für die Interaktion mit den SMTC. Weitere Informationen zum Anpassen der standardmäßigen SMTC-Integration mit **MediaPlayer** finden Sie unter [Integration in die Steuerelemente für den Systemmedientransport](integrate-with-systemmediatransportcontrols.md).

In verschiedenen Szenarien müssen Sie eine manuelle Steuerung der SMTC implementieren. Hierzu gehört die Verwendung einer [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController)-Klasse zum Steuern der Wiedergabe eines oder mehrerer Medienplayer. Ein anderes Szenario ist die Verwendung mehrerer Medienplayer mit nur einer Instanz der SMTC für Ihre App. Bei der Medienwiedergabe mithilfe der [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaElement)-Klasse müssen Sie die SMTC manuell steuern.

## Einrichten von Transportsteuerelementen
Wenn Sie für die Medienwiedergabe **MediaPlayer** verwenden, können Sie eine Instanz der [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.SystemMediaTransportControls)-Klasse abrufen, indem Sie auf die [**MediaPlayer.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.SystemMediaTransportControls)-Eigenschaft zugreifen. Wenn Sie die SMTC manuell steuern möchten, sollten Sie die automatische Integration von **MediaPlayer** deaktivieren, indem Sie die [**CommandManager.IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled)-Eigenschaft auf „false“ festlegen.

[!code-cs[InitSMTCMediaPlayer](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetInitSMTCMediaPlayer)]

Sie können auch eine Instanz der [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)-Klasse abrufen, indem Sie [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708) aufrufen. Sie müssen das Objekt mit dieser Methode abrufen, wenn Sie für die Medienwiedergabe **MediaElement** verwenden.

[!code-cs[InitSMTCMediaElement](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetInitSMTCMediaElement)]

Legen Sie zum Aktivieren der von Ihrer App verwendeten Schaltflächen die entsprechende „IsEnabled“-Eigenschaft des **SystemMediaTransportControls**-Objekts fest, z.B. [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714), [**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713), [**IsNextEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278712) und [**IsPreviousEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278715). Eine vollständige Liste der verfügbaren Steuerelemente finden Sie in der Referenzdokumentation zu **SystemMediaTransportControls**.

[!code-cs[EnableContols](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetEnableContols)]

Registrieren Sie einen Handler für das [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706)-Ereignis, um Benachrichtigungen zu empfangen, wenn der Benutzer eine Schaltfläche betätigt.

[!code-cs[RegisterButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetRegisterButtonPressed)]

## Behandeln von Klicks auf Schaltflächen von Systemmedientransport-Steuerelementen

Das [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706)-Ereignis wird von den Systemsteuerelementen für den Medientransport ausgelöst, wenn eine der aktivierten Schaltflächen betätigt wird. Die an den Ereignishandler übergebene [**Button**](https://msdn.microsoft.com/library/windows/apps/dn278685)-Eigenschaft der [**SystemMediaTransportControlsButtonPressedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn278683)-Klasse ist ein Element der [**SystemMediaTransportControlsButton**](https://msdn.microsoft.com/library/windows/apps/dn278681)-Enumeration, die angibt, welche der aktivierten Schaltflächen betätigt wurde.

Zum Aktualisieren von Objekten im UI-Thread des [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706)-Ereignishandlers (z.B. ein [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)-Objekt) müssen Sie die Aufrufe über den [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) marshallen. Der Grund hierfür ist, dass der **ButtonPressed**-Ereignishandler nicht vom Benutzeroberflächenthread aufgerufen wird und daher eine Ausnahme ausgelöst wird, wenn Sie versuchen, die Benutzeroberfläche direkt zu ändern.

[!code-cs[SystemMediaTransportControlsButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsButtonPressed)]

## Aktualisieren der Steuerelemente für den Systemmedientransport mit dem aktuellen Medienstatus

Melden Sie der [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)-Klasse eine Änderung des Medienstatus, damit das System die Steuerelemente entsprechend dem aktuellen Zustand aktualisieren kann. Legen Sie dazu die [**PlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278719)-Eigenschaft auf den entsprechenden [**MediaPlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278665)-Wert innerhalb des [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375)-Ereignisses der [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)-Klasse fest, die bei Änderungen des Medienstatus ausgelöst wird.

[!code-cs[SystemMediaTransportControlsStateChange](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsStateChange)]

## Aktualisieren der Steuerelemente für den Systemmedientransport mit Medieninformationen und Miniaturansichten

Verwenden Sie die [**SystemMediaTransportControlsDisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278686)-Klasse, um die von den Transportsteuerelementen angezeigten Medieninfos (z.B. den Songtitel oder das Albumbild) für das aktuell wiedergegebene Medienelement zu aktualisieren. Rufen Sie eine Instanz dieser Klasse mit der [**SystemMediaTransportControls.DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707)-Eigenschaft ab. Für gewöhnliche Szenarien wird empfohlen, die Metadaten mit dem Aufruf der [**CopyFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn278694)-Methode und in der aktuell wiedergegebenen Mediendatei zu übergeben. Die Anzeigeaktualisierung wird die Metadaten und das Miniaturbild automatisch aus der Datei extrahieren.

Rufen Sie die [**Update**](https://msdn.microsoft.com/library/windows/apps/dn278701)-Methode auf, damit die Benutzeroberflächen der Steuerelemente für den Systemmedientransport mit den neuen Metadaten und der Miniaturansicht aktualisiert werden.

[!code-cs[SystemMediaTransportControlsUpdater](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsUpdater)]

Wenn es Ihr Szenario erfordert, können Sie die von den Steuerelementen für den Systemmedientransport angezeigten Metadaten manuell aktualisieren, indem Sie die Werte der [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/dn278696)-, [**ImageProperties**](https://msdn.microsoft.com/library/windows/apps/dn278695)- oder [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/dn278702)-Objekte festlegen, die von der [**DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707)-Klasse zur Verfügung gestellt werden.

[!code-cs[SystemMediaTransportControlsUpdaterManual](./code/SMTCWin10/cs/MainPage.xaml.cs#SystemMediaTransportControlsUpdaterManual)]

## Aktualisieren der Zeitskalaeigenschaften der Steuerelemente für den Systemmedientransport

Die Steuerelemente für den Systemmedientransport zeigen Informationen über die Zeitskala des aktuell wiedergegebenen Medienelements an, z.B. die aktuelle Wiedergabeposition, die Startzeit und die Endzeit des Medienelements. Erstellen Sie zum Aktualisieren der Zeitskalaeigenschaften der Steuerelemente für den Systemmedientransport ein neues [**SystemMediaTransportControlsTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218746)-Objekt. Legen Sie die Eigenschaften des Objekts so fest, dass sich der aktuelle Zustand des wiedergegebenen Medienelements widerspiegelt. Rufen Sie die [**SystemMediaTransportControls.UpdateTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218760)-Methode auf, damit die Zeitskala durch die Steuerelemente aktualisiert wird.

[!code-cs[UpdateTimelineProperties](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetUpdateTimelineProperties)]

-   Sie müssen einen Wert für die [**StartTime**](https://msdn.microsoft.com/library/windows/apps/mt218751)-, [**EndTime**](https://msdn.microsoft.com/library/windows/apps/mt218747)- und [**Position**](https://msdn.microsoft.com/library/windows/apps/mt218755)-Eigenschaften angeben, um eine Zeitskala für das wiedergegebene Element anzuzeigen.

-   [**MinSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218749) und [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748) ermöglichen Ihnen die Angabe des Bereichs innerhalb der Zeitskala, den die Benutzer durchsuchen können. Ein typisches Szenario hierfür ist es, den Anbietern Werbepausen in ihren Medien zu ermöglichen.

    Sie müssen die [**MinSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218749)- und [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748)-Eigenschaften festlegen, um [**PositionChangeRequest**](https://msdn.microsoft.com/library/windows/apps/mt218755) auszulösen.

-   Es wird empfohlen, die Steuerelemente mit der Medienwiedergabe zu synchronisieren, indem diese Eigenschaften etwa alle fünf Sekunden während der Wiedergabe und bei Änderung des Wiedergabestatus aktualisiert werden, z. B. beim Anhalten der Wiedergabe oder bei der Suche nach einer neuen Wiedergabeposition.

## Reaktion auf Änderungen der Playereigenschaften

Es gibt eine Reihe von Steuerelementeigenschaften für den Systemmedientransport, die sich auf den Zustand des Media Players selbst, und nicht auf den Zustand des wiedergegebenen Medienelements beziehen. Jede dieser Eigenschaften ist einem Ereignis zugeordnet, das ausgelöst wird, wenn der Benutzer das zugeordnete Steuerelement anpasst. Diese Eigenschaften und Ereignisse sind folgende:

| Eigenschaft                                                                  | Ereignis                                                                                                   |
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| [**AutoRepeatMode**](https://msdn.microsoft.com/library/windows/apps/mt218753) | [**AutoRepeatModeChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218754) |
| [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/mt218756)     | [**PlaybackRateChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218757)     |
| [**ShuffleEnabled**](https://msdn.microsoft.com/library/windows/apps/mt218758) | [**ShuffleEnabledChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218759) |

 
Registrieren Sie zum Behandeln von Benutzerinteraktionen mit einem der folgenden Steuerelemente zunächst einen Handler für das zugeordnete Ereignis.

[!code-cs[RegisterPlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetRegisterPlaybackChangedHandler)]

Stellen Sie im Handler für das Ereignis zunächst sicher, dass der angeforderte Wert innerhalb eines gültigen und erwarteten Bereichs liegt. Wenn dies der Fall ist, legen Sie die entsprechende Eigenschaft für die [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)-Klasse fest und anschließend die entsprechende Eigenschaft für das [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)-Objekt.

[!code-cs[PlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetPlaybackChangedHandler)]

-   Sie müssen einen Anfangswert für die Eigenschaft festlegen, damit eines dieser Ereignisse der Playereigenschaft ausgelöst wird. [**PlaybackRateChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218757) wird beispielsweise erst ausgelöst, wenn ein Wert für die [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/mt218756)-Eigenschaft mindestens ein Mal festgelegt wurde.

## Verwenden der Steuerelemente für den Systemmedientransport für Audiowiedergabe im Hintergrund

Wenn Sie die automatische SMTC-Integration von **MediaPlayer** nicht verwenden, müssen Sie die SMTC manuell integrieren, um Hintergrundaudio zu aktivieren. Sie sollten für Ihre App mindestens die Schaltflächen „Wiedergeben“ und „Anhalten“ aktivieren, indem Sie [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714) und [**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713) auf „true“ festlegen. Außerdem muss die App das [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706)-Ereignis behandeln. Wenn Ihre App diese Anforderungen nicht erfüllt, wird die Audiowiedergabe bei Verschieben der App in den Hintergrund beendet.

Apps, in denen das neue Einzelprozessmodell für Hintergrundaudio verwendet wird, sollten eine Instanz der [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)-Klasse abrufen, indem sie [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708) aufrufen. Apps, die das vorherige Modell mit zwei Prozessen für die Audiowiedergabe im Hintergrund verwenden, müssen [**BackgroundMediaPlayer.Current.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635) verwenden, um über den Hintergrundprozess Zugriff auf die SMTC zu erhalten.

Weitere Informationen zur Audiowiedergabe im Hintergrund finden Sie unter [Wiedergeben von Medien im Hintergrund](background-audio.md).

## Verwandte Themen
* [Medienwiedergabe](media-playback.md)
* [Integration in die Steuerelemente für den Systemmedientransport](integrate-with-systemmediatransportcontrols.md) 
* [Beispiel für den Systemmedientransport](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls) 

 







<!--HONumber=Aug16_HO3-->


