---
ms.assetid: EFCF84D0-2F4C-454D-97DA-249E9EAA806C
description: Mit der SystemMediaTransportControls-Klasse kann Ihre App die Steuerelemente für den Systemmedientransport verwenden, die in Windows integriert sind, und die Metadaten aktualisieren, die die Steuerelemente zu den von der App aktuell wiedergegebenen Medien anzeigen.
title: Steuerelemente für den Systemmedientransport
---

# Steuerelemente für den Systemmedientransport

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Mit der [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)-Klasse kann Ihre App die Steuerelemente für den Systemmedientransport verwenden, die in Windows integriert sind und die Metadaten aktualisieren, die die Steuerelemente zu den von der App aktuell wiedergegebenen Medien anzeigen.

Die Systemsteuerelemente für den Medientransport unterscheiden sich von den Transportsteuerelementen des [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)-Objekts. Die Systemsteuerelemente für den Medientransport sind die Steuerelemente, die angezeigt werden, wenn Hardwaretasten für Medien betätigt werden (z. B. der Lautstärkeregler an Kopfhörern oder die Medientasten auf Tastaturen). Wenn der Benutzer die PAUSE-Taste auf einer Tastatur drückt und Ihre App die [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) unterstützt, wird die App benachrichtigt, und Sie können die entsprechende Aktion durchführen.

Ihre App kann auch die Medieninfos aktualisieren, die von den [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) angezeigt werden (z. B. Songtitel und Miniaturansicht).

**Hinweis**  
Im UWP-Beispiel [Steuerelemente für den Systemmedientransport](http://go.microsoft.com/fwlink/?LinkId=619488) wird der in dieser Übersicht besprochene Code implementiert. Sie können das Beispiel herunterladen, um den Code im Kontext anzuzeigen oder ihn als Ausgangspunkt für Ihre eigene App zu verwenden.

## Einrichten von Transportsteuerelementen

Definieren Sie in der XAML-Datei für die Seite eine [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)-Klasse, die von den Steuerelementen für den Systemmedientransport gesteuert wird. Die [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375)- und [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/br227394)-Ereignisse werden zum Aktualisieren der Steuerelemente für den Systemmedientransport verwendet und später in diesem Artikel erläutert.

[!code-xml[MediaElementSystemMediaTransportControls](./code/SMTCWin10/cs/MainPage.xaml#SnippetMediaElementSystemMediaTransportControls)]

Fügen Sie eine Schaltfläche zu der XAML-Datei hinzu, mit der die Benutzer eine Datei auswählen können, die wiedergegeben werden soll.

[!code-xml[OpenButton](./code/SMTCWin10/cs/MainPage.xaml#SnippetOpenButton)]

Fügen Sie in der CodeBehind-Seite using-Direktiven für die folgenden Namespaces hinzu.

[!code-cs[Namespace](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetNamespace)]

Fügen Sie einen Klickhandler für die Schaltfläche hinzu, der eine [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)-Klasse verwendet, damit Benutzer eine Datei auswählen können, und rufen Sie dann die [**SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338)-Methode auf, um diese als aktive Datei für das **MediaElement**-Objekt festzulegen.

[!code-cs[OpenMediaFile](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetOpenMediaFile)]

Rufen Sie [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708) auf, um eine Instanz der [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) abzurufen.

Legen Sie zum Aktivieren der von Ihrer App verwendeten Schaltflächen die entsprechende "IsEnabled"-Eigenschaft des **SystemMediaTransportControls**-Objekts fest, z. B. [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714), [**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713), [**IsNextEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278712) und [**IsPreviousEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278715). Eine vollständige Liste der verfügbaren Steuerelemente finden Sie in der Referenzdokumentation zu **SystemMediaTransportControls**.

Registrieren Sie einen Handler für das [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706)-Ereignis, um Benachrichtigungen zu empfangen, wenn der Benutzer eine Schaltfläche betätigt.

[!code-cs[SystemMediaTransportControlsSetup](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsSetup)]

## Behandeln von Schaltflächenbetätigungen an Steuerelementen für den Systemmedientransport

Das [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706)-Ereignis wird von den Systemsteuerelementen für den Medientransport ausgelöst, wenn eine der aktivierten Schaltflächen betätigt wird. Die an den Ereignishandler übergebene [**Button**](https://msdn.microsoft.com/library/windows/apps/dn278685)-Eigenschaft der [**SystemMediaTransportControlsButtonPressedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn278683)-Klasse ist ein Element der [**SystemMediaTransportControlsButton**](https://msdn.microsoft.com/library/windows/apps/dn278681)-Enumeration, die angibt, welche der aktivierten Schaltflächen betätigt wurde.

Zum Aktualisieren von Objekten im UI-Thread des [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706)-Ereignishandlers (z. B. ein [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)-Objekt) müssen Sie die Aufrufe über den [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) marshallen. Dies ist der Fall, da der **ButtonPressed**-Ereignishandler nicht vom Benutzeroberflächenthread aufgerufen wird, und daher eine Ausnahme ausgelöst wird, wenn Sie versuchen, die Benutzeroberfläche direkt zu ändern.

[!code-cs[SystemMediaTransportControlsButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsButtonPressed)]

## Aktualisieren der Steuerelemente für den Systemmedientransport mit dem aktuellen Medienstatus

Melden Sie der [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)-Klasse eine Änderung des Medienstatus, damit das System die Steuerelemente entsprechend dem aktuellen Zustand aktualisieren kann. Legen Sie dazu die [**PlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278719)-Eigenschaft auf den entsprechenden [**MediaPlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278665)-Wert innerhalb des [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375)-Ereignisses der [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)-Klasse fest, die bei Änderungen des Medienstatus ausgelöst wird.

[!code-cs[SystemMediaTransportControlsStateChange](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsStateChange)]

## Aktualisieren der Steuerelemente für den Systemmedientransport mit Medieninformationen und Miniaturansichten

Verwenden Sie die [**SystemMediaTransportControlsDisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278686)-Klasse, um die von den Transportsteuerelementen angezeigten Medieninfos (z. B. den Songtitel oder das Albumbild) für das aktuell wiedergegebene Medienelement zu aktualisieren. Rufen Sie eine Instanz dieser Klasse mit der [**SystemMediaTransportControls.DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707)-Eigenschaft ab. Für gewöhnliche Szenarien wird empfohlen, die Metadaten mit dem Aufruf der [**CopyFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn278694)-Methode und in der aktuell wiedergegebenen Mediendatei zu übergeben. Die Anzeigeaktualisierung wird die Metadaten und das Miniaturbild automatisch aus der Datei extrahieren.

Rufen Sie die [**Update**](https://msdn.microsoft.com/library/windows/apps/dn278701)-Methode auf, damit die Steuerelemente für den Systemmedientransport ihre Benutzeroberfläche mit den neuen Metadaten und der Miniaturansicht aktualisieren.

[!code-cs[SystemMediaTransportControlsUpdater](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsUpdater)]

Wenn es Ihr Szenario erfordert, können Sie die von den Steuerelementen für den Systemmedientransport angezeigten Metadaten manuell aktualisieren, indem Sie die Werte der [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/dn278696)-, [**ImageProperties**](https://msdn.microsoft.com/library/windows/apps/dn278695)- oder [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/dn278702)-Objekte festlegen, die von der [**DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707)-Klasse zur Verfügung gestellt werden.

[!code-cs[SystemMediaTransportControlsUpdaterManual](./code/SMTCWin10/cs/MainPage.xaml.cs#SystemMediaTransportControlsUpdaterManual)]

## Aktualisieren der Zeitskalaeigenschaften der Steuerelemente für den Systemmedientransport

Die Steuerelemente für den Systemmedientransport zeigen Informationen über die Zeitskala des aktuell wiedergegebenen Medienelements an, z. B. die aktuelle Wiedergabeposition, die Startzeit und die Endzeit des Medienelements. Erstellen Sie zum Aktualisieren der Zeitskalaeigenschaften der Steuerelemente für den Systemmedientransport ein neues [**SystemMediaTransportControlsTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218746)-Objekt. Legen Sie die Eigenschaften des Objekts so fest, dass sich der aktuelle Zustand des wiedergegebenen Medienelements widerspiegelt. Rufen Sie die [**SystemMediaTransportControls.UpdateTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218760)-Methode auf, damit die Steuerelemente die Zeitskala aktualisieren.

[!code-cs[UpdateTimelineProperties](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetUpdateTimelineProperties)]

-   Sie müssen einen Wert für die [**StartTime**](https://msdn.microsoft.com/library/windows/apps/mt218751)-, [**EndTime**](https://msdn.microsoft.com/library/windows/apps/mt218747)- und [**Position**](https://msdn.microsoft.com/library/windows/apps/mt218755)-Eigenschaften angeben, um eine Zeitskala für das wiedergegebene Element anzuzeigen.

-   [
							Mit den **MinSeekTime**-
						](https://msdn.microsoft.com/library/windows/apps/mt218749) und [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748)-Eigenschaften können Sie den Bereich innerhalb der Zeitskala angeben, den die Benutzer durchsuchen können. Ein typisches Szenario hierfür ist es, den Anbietern Werbepausen in ihren Medien zu ermöglichen.

    Sie müssen die [**MinSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218749)- und [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748)-Eigenschaften festlegen, um [**PositionChangeRequest**](https://msdn.microsoft.com/library/windows/apps/mt218755) auszulösen.

-   Es wird empfohlen, die Steuerelemente mit der Medienwiedergabe zu synchronisieren, indem diese Eigenschaften etwa alle fünf Sekunden während der Wiedergabe und bei Änderung des Wiedergabestatus aktualisiert werden, z. B. beim Anhalten der Wiedergabe oder bei der Suche nach einer neuen Wiedergabeposition.

## Reaktion auf Änderungen der Playereigenschaften

Es gibt eine Reihe von Steuerelementeigenschaften für den Systemmedientransport, die sich auf den Zustand des Media Players selbst, und nicht auf den Zustand des wiedergegebenen Medienelements beziehen. Jede dieser Eigenschaften ist einem Ereignis zugeordnet, das ausgelöst wird, wenn der Benutzer das zugeordnete Steuerelement anpasst. Diese Eigenschaften und Ereignisse sind folgende:

| Eigenschaft                                                                  | Ereignis                                                                                                   |
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| [**AutoRepeatMode**](https://msdn.microsoft.com/library/windows/apps/mt218753) | [**AutoRepeatModeChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218754) |
| [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/mt218756)     | [**PlaybackRateChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218757)     |
| [**ShuffleEnabled**](https://msdn.microsoft.com/library/windows/apps/mt218758) | [**ShuffleEnabledChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218759) |

 
Registrieren Sie zum Behandeln von Benutzerinteraktionen mit einem dieser Steuerelemente zuerst einen Handler für das zugeordnete Ereignis.

[!code-cs[RegisterPlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetRegisterPlaybackChangedHandler)]

Stellen Sie im Handler für das Ereignis zunächst sicher, dass der angeforderte Wert innerhalb eines gültigen und erwarteten Bereichs liegt. Legen Sie, wenn dies der Fall ist, die entsprechende Eigenschaft für die [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)-Klasse fest und legen Sie dann die entsprechende Eigenschaft für das [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)-Objekt fest.

[!code-cs[PlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetPlaybackChangedHandler)]

-   Sie müssen einen Anfangswert für die Eigenschaft festlegen, damit eines dieser Ereignisse der Playereigenschaft ausgelöst wird. [
            **PlaybackRateChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218757) wird beispielsweise erst ausgelöst, wenn ein Wert für die [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/mt218756)-Eigenschaft mindestens ein Mal festgelegt wurde.

## Verwenden der Steuerelemente für den Systemmedientransport für Audiowiedergabe im Hintergrund

Um die Steuerelemente für den Systemmedientransport für die Audiowiedergabe im Hintergrund zu verwenden, müssen Sie die Schaltflächen „Wiedergeben“ und „Anhalten“ durch Festlegen der [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714)- und [**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713)-Eigenschaften auf "true" aktivieren. Außerdem muss die App das [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706)-Ereignis behandeln.

Zum Abrufen einer Instanz der [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)-Klasse aus der Hintergrundaufgabe Ihrer App müssen Sie [**BackgroundMediaPlayer.Current.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635) anstelle von [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708) verwenden, das nur im App-Vordergrund verwendet werden kann.

Weitere Informationen zur Audiowiedergabe im Hintergrund finden Sie unter [Audiowiedergabe im Hintergrund](background-audio.md).

 

 






<!--HONumber=Mar16_HO1-->


