---
author: drewbatgit
ms.assetid: 
description: "In diesem Artikel wird beschrieben, wie Sie für Ihre App zur Medienwiedergabe Medienunterbrechungen erstellen, planen und verwalten."
title: Erstellen, Planen und Verwalten von Medienunterbrechungen
translationtype: Human Translation
ms.sourcegitcommit: 2e969e53a29a98223f26353a5444ca9d9ebe2641
ms.openlocfilehash: 0fe495f3eb2c15ccff4a672abd904dc43cfdf193

---

# Erstellen, Planen und Verwalten von Medienunterbrechungen

In diesem Artikel wird beschrieben, wie Sie für Ihre App zur Medienwiedergabe Medienunterbrechungen erstellen, planen und verwalten. Medienunterbrechungen werden normalerweise genutzt, um Audio- und Videoanzeigen in Medieninhalte einzufügen. Ab Windows 10, Version 1607, können Sie die [**MediaBreakManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager)-Klasse verwenden, um schnell und einfach Medienunterbrechungen zu jedem [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem)-Element hinzuzufügen, das Sie mit einem [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) abspielen.


Nachdem Sie eine oder mehrere Medienunterbrechungen geplant haben, spielt das System automatisch die Medieninhalte zum angegebenen Zeitpunkt während der Wiedergabe ab. Der **MediaBreakManager** stellt Ereignisse bereit, damit Ihre App reagieren kann, wenn Medienunterbrechungen beginnen, enden oder vom Benutzer übersprungen werden. Sie können auch auf eine [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession) für Ihre Medienunterbrechnungen zugreifen, um Ereignisse, wie z. B. das Herunterladen und Puffern von Updates, zu überwachen.

## Medienunterbrechungen planen
Jedes **MediaPlaybackItem**-Objekt verfügt über einen eigenen [**MediaBreakSchedule**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakSchedule) zum Konfigurieren der Medienunterbrechungen, die wiedergegeben werden sollen, wenn das Element abgespielt wird. Der erste Schritt zur Verwendung von Medienunterbrechungen in Ihrer App ist die Erstellung eines [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem)-Elements für Ihre Hauptinhalte für die Wiedergabe. 

[!code-cs[MoviePlaybackItem](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetMoviePlaybackItem)]

Weitere Informationen zum Arbeiten mit **MediaPlaybackItem**, [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList) und anderen grundlegenden Medienwiedergabe-APIs finden Sie unter [Medienelemente, Wiedergabelisten und Titel](media-playback-with-mediasource.md).

Das nächste Beispiel zeigt, wie Sie dem **MediaPlaybackItem**-Element eine PreRoll-Unterbrechung hinzufügen, was bedeutet, dass das System die Medienunterbrechung vor der Wiedergabe des Wiedergabeelements, zu dem die Unterbrechung gehört, wiedergibt. Zuerst wird ein neues [**MediaBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreak)-Objekt instanziiert. In diesem Beispiel wird der Konstruktor mit [**MediaBreakInsertionMethod.Interrupt**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakInsertionMethod) aufgerufen, d. h. der Hauptinhalt wird angehalten, während der Unterbrechungsinhalt wiedergegeben wird. 

Als Nächstes wird ein neues **MediaPlaybackItem**-Element für den Inhalt erstellt, der während der Unterbrechung, z. B. in Form einer Anzeige, wiedergegeben wird. Die [**CanSkip**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.CanSkip)-Eigenschaft dieses Wiedergabeelements ist auf "false" festgelegt. Das bedeutet, dass der Benutzer das Element mit den integrierten Mediensteuerelementen nicht überspringen kann. Die App kann dennoch entscheiden, die Anzeige durch den Aufruf von [**SkipCurrentBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager.SkipCurrentBreak) programmgesteuert zu überspringen. 

Die [**PlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreak.PlaybackList)-Eigenschaft der Medienunterbrechung ist eine [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList), mit der Sie mehrere Medienelemente als eine Wiedergabeliste wiedergegeben können. Fügen Sie ein oder mehrere **MediaPlaybackItem**-Objekte aus der Sammlung **Elemente** der Liste hinzu, um sie in die Wiedergabeliste der Medienunterbrechung einzufügen.

Planen Sie dann die Medienunterbrechung, indem Sie die [**BreakSchedule**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.BreakSchedule)-Eigenschaft des Elements für die Wiedergabe des Hauptinhalts. Definieren Sie die Unterbrechung als PreRoll-Unterbrechung, indem Sie sie der [**PrerollBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakSchedule.PrerollBreak)-Eigenschaft des Schedule-Objekts zuweisen.

[!code-cs[PreRollBreak](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetPreRollBreak)]

Jetzt können Sie das Hauptmedienelement wiedergeben, wobei die von Ihnen erstellte Medienunterbrechung vor dem Hauptinhalt wiedergegeben wird. Erstellen Sie ein neues [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer)-Objekt, und legen Sie die [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay)-Eigenschaft optional auf "true" fest, um die Wiedergabe automatisch zu starten. Legen Sie die [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source)-Eigenschaft des **MediaPlayer** auf das Element für die Wiedergabe des Hauptinhalts fest. Es ist zwar nicht erforderlich, aber Sie können den **MediaPlayer** einem [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) zuweisen, um die Medien auf einer XAML-Seite zu rendern. Weitere Informationen zur Verwendung des **MediaPlayer** finden Sie unter [Wiedergeben von Audio- und Videoinhalten mit MediaPlayer](play-audio-and-video-with-mediaplayer.md).

[!code-cs[Wiedergeben](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetPlay)]

Fügen Sie eine PostRoll-Unterbrechung hinzu, die nach Abschluss der Wiedergabe des **MediaPlaybackItem**, das den Hauptinhalt enthält, wiedergegeben wird. Verwenden Sie dabei das gleiche Verfahren wie bei einer PreRoll-Unterbrechung, außer dass Sie Ihr **MediaBreak**-Objekt der [**PostrollBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakSchedule.PostrollBreak)-Eigenschaft zuweisen.

[!code-cs[PostRollBreak](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetPostRollBreak)]

Sie können auch eine oder mehrere MidRoll-Unterbrechungen planen, die zur angegebenen Zeit innerhalb der Wiedergabe des Hauptinhalts wiedergegeben werden. Im folgenden Beispiel wird das [**MediaBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreak)-Objekt mit der Konstruktorüberladung erstellt, die ein **TimeSpan**-Objekt akzeptiert, das innerhalb der Wiedergabe des Hauptmedienelements die Zeit für die Wiedergabe der Unterbrechung angibt. Auch hier wird ein [**MediaBreakInsertionMethod.Interrupt**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakInsertionMethod) festgelegt, um anzugeben, dass die Wiedergabe des Hauptinhalts während der Wiedergabe der Unterbrechung angehalten wird. Die MidRoll-Unterbrechung wird dem Zeitplan durch Aufrufen von [**InsertMidrollBreak**](https://msdn.microsoft.com/library/windows/apps/mt670692) hinzugefügt. Sie können eine schreibgeschützte Liste der aktuellen MidRoll-Unterbrechungen im Zeitplan abrufen, indem Sie auf die [**MidrollBreaks**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakSchedule.MidrollBreaks)-Eigenschaft zugreifen.

[!code-cs[MidrollBreak](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetMidrollBreak)]

Beim nächsten Beispiel für eine MidRoll-Unterbrechung wird die [**MediaBreakInsertionMethod.Replace**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakInsertionMethod)-Einfügemethode verwendet. Dies bedeutet, dass das System während der Wiedergabe der Unterbrechung die Verarbeitung des Hauptinhalts fortsetzt. Diese Option wird normalerweise von Live-Streaming-Medien-Apps verwendet, deren Inhalt nicht angehalten und hinter den Live-Stream zurückfallen soll, während die Anzeige wiedergegeben wird. 

Bei diesem Beispiel wird auch eine Überladung des [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem)-Konstruktors verwendet, der zwei [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/Windows.Foundation.TimeSpan)-Parameter akzeptiert. Der erste Parameter definiert den Startpunkt innerhalb des Medienunterbrechungselements, an dem die Wiedergabe beginnen soll. Der zweite Parameter definiert die Dauer, während der die Medienunterbrechung wiedergegeben werden soll. Im folgenden Beispiel beginnt daher die **MediaBreak**-Wiedergabe bei 20 Minuten im Hauptinhalt. Mit seiner Wiedergabe beginnt das Medienelement 30 Sekunden ab dem Beginn des Unterbrechungsmedienelements und setzt sie 15 Sekunden lang fort, bevor die Wiedergabe des Hauptmedieninhalts wieder aufgenommen wird.

[!code-cs[MidrollBreak2](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetMidrollBreak2)]

## Medienunterbrechungen überspringen
Wie bereits in diesem Artikel erwähnt, kann die [**CanSkip**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.CanSkip)-Eigenschaft eines **MediaPlaybackItem** so festgelegt werden, dass der Benutzer den Inhalt mit den integrierten Steuerelementen nicht überspringen kann. Sie können jedoch [**SkipCurrentBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager.SkipCurrentBreak) im Code jederzeit aufrufen, um die aktuelle Unterbrechung zu überspringen.

[!code-cs[SkipButtonClick](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetSkipButtonClick)]

## Behandeln von MediaBreak-Ereignissen

Es gibt verschiedene Ereignisse im Zusammenhang mit Medienunterbrechungen, für die Sie sich registrieren können, um basierend auf den Zustandsänderungen von Medienunterbrechungen Maßnahmen zu ergreifen.

[!code-cs[RegisterMediaBreakEvents](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetRegisterMediaBreakEvents)]

Das [**BreakStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager.BreakStarted)-Ereignis wird ausgelöst, wenn eine Medienunterbrechung beginnt. Es empfiehlt sich, die Benutzeroberfläche zu aktualisieren, um den Benutzer darauf hinzuweisen, dass Unterbrechungsmedieninhalt wiedergegeben wird. In diesem Beispiel wird das dem Handler übergebene [**MediaBreakStartedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakStartedEventArgs)-Objekt verwendet, um einen Verweis auf die Medienunterbrechung zu erhalten, die begonnen hat. Anschließend wird die [**CurrentItemIndex**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList.CurrentItemIndex)-Eigenschaft verwendet, um zu bestimmen, welches Medienelement in der Wiedergabeliste der Medienunterbrechung wiedergegeben wird. Dann wird die Benutzeroberfläche aktualisiert, um dem Benutzer den aktuellen Anzeigenindex und die Anzahl der in der Unterbrechung verbliebenen Anzeigen anzuzeigen. Beachten Sie, dass Aktualisierungen der Benutzeroberfläche im UI-Thread vorgenommen werden müssen. Daher sollte der Aufruf innerhalb eines Aufrufs von [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) erfolgen. 

[!code-cs[BreakStarted](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakStarted)]

[**BreakEnded**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager.BreakEnded) wird ausgelöst, wenn alle Medienelemente in der Unterbrechung wiedergegeben oder übersprungen wurden. Sie können den Handler für dieses Ereignis zum Aktualisieren der Benutzeroberfläche verwenden, um anzugeben, dass die Wiedergabe der Medienunterbrechung beendet ist.

[!code-cs[BreakEnded](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakEnded)]

Das **BreakSkipped**-Ereignis wird ausgelöst, wenn der Benutzer in der integrierten Benutzeroberfläche die Schaltfläche *Weiter* während der Wiedergabe eines Elements drückt, für das [**CanSkip**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.CanSkip) den Wert true hat, oder wenn Sie eine Unterbrechung in Ihrem Code durch Aufrufen von [**SkipCurrentBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager.SkipCurrentBreak) überspringen.

Im folgenden Beispiel wird die [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source)-Eigenschaft des **MediaPlayer** verwendet, um einen Verweis auf das Medienelement für den Hauptinhalt zu erhalten. Die übersprungene Medienunterbrechung gehört zum Unterbrechungszeitplan dieses Elements. Als Nächstes führt der Code eine Überprüfung durch, um festzustellen, ob die übersprungene Medienunterbrechung der Medienunterbrechung entspricht, für die die [**PrerollBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakSchedule.PrerollBreak)-Eigenschaft des Zeitplans festgelegt wurde. Falls ja, bedeutet dies, dass es sich bei der PreRoll-Unterbrechung um die übersprungene Unterbrechung handelt, und in diesem Fall eine neue MidRoll-Unterbrechung erstellt und für die Wiedergabe von 10 Minuten in den Hauptinhalt geplant wird.

[!code-cs[BreakSkipped](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakSkipped)]

[**BreaksSeekedOver**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager.BreaksSeekedOver) wird ausgelöst, wenn die Wiedergabeposition des Hauptmedienelements den geplanten Zeitpunkt für eine oder mehrere Medienunterbrechungen passiert. Im folgenden Beispiel wird eine Überprüfung durchgeführt, um festzustellen, ob mehr als eine Medienunterbrechung übergangen wurde, wenn die Wiedergabeposition nach vorne verschoben wird und wenn sie um weniger als 10 Minuten nach vorne verschoben wird. Falls ja, wird die erste übergangene Unterbrechung, die aus der [**SeekedOverBreaks**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakSeekedOverEventArgs.SeekedOverBreaks)-Sammlung abgerufen und von den Ereignisargumenten verfügbar gemacht wurde, durch einen Aufruf der [**PlayBreak**](https://msdn.microsoft.com/library/windows/apps/mt670689)-Methode des **MediaPlayer.BreakManager** sofort wiedergegeben.

[!code-cs[BreakSeekedOver](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakSeekedOver)]

## Abrufen von Informationen über die aktuelle Medienunterbrechung
Wie in diesem Artikel bereits erwähnt, kann die [**CurrentItemIndex**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList.CurrentItemIndex)-Eigenschaft verwendet werden, um zu ermitteln, welches Medienelement in einer Medienunterbrechung gerade wiedergegeben wird. Es empfiehlt sich, in regelmäßigen Abständen eine Überprüfung auf das gerade wiedergegebene Element durchzuführen, um die Benutzeroberfläche zu aktualisieren. Überprüfen Sie unbedingt zuerst die [**CurrentBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager.CurrentBreak)-Eigenschaft auf null. Wenn die Eigenschaft null ist, wird gerade keine Medienunterbrechung wiedergegeben.

[!code-cs[GetCurrentBreakItemIndex](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetGetCurrentBreakItemIndex)]

## Zugreifen auf die aktuelle Wiedergabesitzung
Das [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession)-Objekt verwendet die **MediaPlayer**-Klasse, um Daten und Ereignisse in Zusammenhang mit den gerade wiedergegebenen Medieninhalten bereitzustellen. Der [**MediaBreakManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager) verfügt auch über eine **MediaPlaybackSession**, auf die Sie zugreifen können, um Daten und Ereignisse abzurufen, die sich speziell auf den Inhalt der Medienunterbrechung beziehen, der gerade wiedergegeben wird. Zu den Informationen, die Sie aus der Wiedergabesitzung erhalten können, gehören der aktuelle Wiedergabestatus, Wiedergabe oder Angehalten, und die aktuelle Wiedergabeposition innerhalb des Inhalts. Sie können die Eigenschaften [**NaturalVideoWidth**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NaturalVideoWidth) und [**NaturalVideoHeight**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NaturalVideoHeight) sowie [**NaturalVideoSizeChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NaturalVideoSizeChanged) verwenden, um die Video-Benutzeroberfläche anzupassen, wenn der Inhalt der Medienunterbrechung ein anderes Seitenverhältnis besitzt als der Hauptinhalt. Sie können auch Ereignisse empfangen, wie z. B. [**BufferingStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.BufferingStarted), [**BufferingEnded**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.BufferingEnded) und [**DownloadProgressChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.DownloadProgressChanged), die wertvolle Telemetriedaten zur Leistung Ihrer App liefern können.

Im folgenden Beispiel wird ein Handler für das **BufferingProgressChanged-Ereignis** registriert. Im Ereignishandler aktualisiert es die Benutzeroberfläche, um den aktuellen Fortschritt der Pufferung anzuzeigen.

[!code-cs[RegisterBufferingProgressChanged](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetRegisterBufferingProgressChanged)]

[!code-cs[BufferingProgressChanged](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBufferingProgressChanged)]

## Verwandte Themen
* [Medienwiedergabe](media-playback.md)
* [Wiedergeben von Audio- und Videoinhalten mit „MediaPlayer“](play-audio-and-video-with-mediaplayer.md)
* [Manuelle Steuerung der Steuerelemente für den Systemmedientransport](system-media-transport-controls.md)

 

 







<!--HONumber=Aug16_HO3-->


