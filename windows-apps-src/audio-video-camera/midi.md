---
ms.assetid: 9146212C-8480-4C16-B74C-D7F08C7086AF
description: In diesem Artikel wird beschrieben, wie Sie MIDI-Geräte (Musical Instrument Digital Interface) aufzählen und MIDI-Nachrichten in einer universellen Windows-App senden und empfangen.
title: MIDI
---

# MIDI

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


In diesem Artikel wird beschrieben, wie Sie MIDI-Geräte (Musical Instrument Digital Interface) aufzählen und MIDI-Nachrichten in einer universellen Windows-App senden und empfangen.

## Aufzählen von MIDI-Geräten

Fügen Sie vor dem Aufzählen und Verwenden von MIDI-Geräten Ihrem Projekt die folgenden Namespaces hinzu.

[!code-cs[Using](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetUsing)]

Fügen Sie der XAML-Seite ein [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868)-Steuerelement hinzu, über das der Benutzer eines der mit dem System verbundenen MIDI-Eingabegeräte auswählen kann. Fügen Sie ein weiteres Steuerelement zum Auflisten der MIDI-Ausgabegeräte hinzu.

[!code-xml[MidiListBoxes](./code/MIDIWin10/cs/MainPage.xaml#SnippetMidiListBoxes)]

Die [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432)-Methode der [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393)-Klasse wird verwendet, um viele verschiedene Arten von Geräten aufzulisten, die von Windows erkannt werden. Um anzugeben, dass die Methode nur nach MIDI-Eingabegeräten suchen soll, verwenden Sie die von [**MidiInPort.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn894779) zurückgegebene Auswahlzeichenfolge. **FindAllAsync** gibt eine [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395) zurück, die eine **DeviceInformation** für jedes beim System registrierte MIDI-Eingabegerät enthält. Wenn die zurückgegebene Sammlung keine Elemente enthält, sind keine MIDI-Eingabegeräte verfügbar. Wenn die Sammlung Elemente enthält, führen Sie eine Schleife durch die **DeviceInformation**-Objekte durch, und fügen Sie den Namen der einzelnen Geräte zum **ListBox** mit MIDI-Eingabegeräten hinzu.

[!code-cs[EnumerateMidiInputDevices](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiInputDevices)]

Das Aufzählen von MIDI-Ausgabegeräten funktioniert genau wie das Aufzählen von Eingabegeräten, bis auf die Ausnahme, dass Sie beim Aufrufen von **FindAllAsync** die von [**MidiOutPort.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn894845) zurückgegebene Auswahlzeichenfolge angeben müssen.

[!code-cs[EnumerateMidiOutputDevices](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiOutputDevices)]

## Erstellen einer Geräteüberwachungselement-Hilfsklasse

Der [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459)-Namespace enthält die [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/br225446)-Klasse, die eine App benachrichtigen kann, wenn Geräte hinzugefügt oder aus dem System entfernt werden, oder wenn die Informationen für ein Gerät aktualisiert werden. Da MIDI-fähige Apps in der Regel sowohl an Eingabe- als auch an Ausgabegeräten interessiert sind, wird in diesem Beispiel eine Hilfsklasse erstellt, die das **DeviceWatcher**-Muster implementiert, sodass derselbe Code für MIDI-Eingabe- und -Ausgabegeräte verwendet werden kann, ohne dass eine Duplizierung erforderlich ist.

Fügen Sie dem Projekt eine neue Klasse hinzu, die als Geräteüberwachungselement dienen soll. In diesem Beispiel hat die Klasse den Namen **MyMidiDeviceWatcher**. Der restliche Code in diesem Abschnitt dient zur Implementierung der Hilfsklasse.

Fügen Sie der Klasse einige Membervariablen hinzu:

-   Ein [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/br225446)-Objekt, das Geräteänderungen überwacht.
-   Eine Geräteauswahlzeichenfolge, die die Auswahlzeichenfolge des MIDI-Eingabeports für eine Instanz und die Auswahlzeichenfolge des MIDI-Ausgabeports für eine andere Instanz enthält.
-   Ein [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868)-Steuerelement, das mit den Namen der verfügbaren Geräte gefüllt wird.
-   Einen [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211), der erforderlich ist, um die UI von einem anderen als dem UI-Thread aus zu aktualisieren.

[!code-cs[WatcherVariables](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherVariables)]

Fügen Sie eine [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395)-Eigenschaft hinzu, mit der von außerhalb der Hilfsklasse auf die aktuelle Geräteliste zugegriffen werden kann.

[!code-cs[DeclareDeviceInformationCollection](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetDeclareDeviceInformationCollection)]

Im Klassenkonstruktor übergibt die aufrufende Funktion die Auswahlzeichenfolge für das MIDI-Gerät, das **ListBox** zum Auflisten der Geräte und den zum Aktualisieren der Benutzeroberfläche benötigten **Dispatcher**.

Rufen Sie [**DeviceInformation.CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/br225427) auf, um eine neue Instanz der **DeviceWatcher**-Klasse zu erstellen, und übergeben Sie die Auswahlzeichenfolge für das MIDI-Gerät.

Registrieren Sie Handler für die Ereignishandler des Überwachungselements.

[!code-cs[WatcherConstructor](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherConstructor)]

**DeviceWatcher** hat die folgenden Ereignisse:

-   [**Added**](https://msdn.microsoft.com/library/windows/apps/br225450) – Wird ausgelöst, wenn dem System ein neues Gerät hinzugefügt wird.
-   [**Removed**](https://msdn.microsoft.com/library/windows/apps/br225453) – Wird ausgelöst, wenn ein Gerät aus dem System entfernt wird.
-   [**Updated**](https://msdn.microsoft.com/library/windows/apps/br225458) – Wird ausgelöst, wenn die Informationen im Zusammenhang mit einem vorhandenen Gerät aktualisiert werden.
-   [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/br225451) – Wird ausgelöst, wenn das Überwachungselement die Aufzählung des angeforderten Gerätetyps abgeschlossen hat.

Im Ereignishandler für jedes dieser Ereignisse wird die **UpdateDevices**-Hilfsmethode aufgerufen, um das **ListBox** mit der aktuellen Geräteliste zu aktualisieren. Da **UpdateDevices** UI-Elemente aktualisiert und diese Ereignishandler nicht für den UI-Thread aufgerufen werden, muss jeder Aufruf von einem Aufruf von [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) umschlossen werden, wodurch der angegebene Code für den UI-Thread ausgeführt wird.

[!code-cs[WatcherEventHandlers](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherEventHandlers)]

Die **UpdateDevices**-Hilfsmethode ruft [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) auf und aktualisiert das **ListBox** mit den Namen der zurückgegebenen Geräte, wie zuvor in diesem Artikel beschrieben.

[!code-cs[WatcherUpdateDevices](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherUpdateDevices)]

Fügen Sie Methoden zum Starten und Beenden des Überwachungselements hinzu; verwenden hierzu die [**Start**](https://msdn.microsoft.com/library/windows/apps/br225454)- bzw. die [**Stop**](https://msdn.microsoft.com/library/windows/apps/br225456)-Methode des **DeviceWatcher**-Objekts.

[!code-cs[WatcherStopStart](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherStopStart)]

Geben Sie einen Destruktor an, um die Registrierung der Überwachungselement-Ereignishandler aufzuheben und das Geräteüberwachungselement auf NULL festzulegen.

[!code-cs[WatcherDestructor](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherDestructor)]

## Erstellen von MIDI-Ports zum Senden und Empfangen von Nachrichten

Deklarieren Sie im CodeBehind für die Seite Membervariablen für zwei Instanzen der **MyMidiDeviceWatcher**-Hilfsklasse, eine für Eingabe- und eine für Ausgabegeräte.

[!code-cs[DeclareDeviceWatchers](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetDeclareDeviceWatchers)]

Erstellen Sie eine neue Instanz der Überwachungselement-Hilfsklassen, und übergeben Sie die Geräteauswahlzeichenfolge, das zu füllende **ListBox** und das **CoreDispatcher**-Objekt, auf das über die **Dispatcher**-Eigenschaft der Seite zugegriffen werden kann. Rufen Sie dann die Methode zum Starten des **DeviceWatcher** jedes Objekts auf.

Kurz nach dem Starten der einzelnen **DeviceWatcher** wird die Enumeration der aktuellen mit dem System verbundenen Geräte beendet, und das [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/br225451)-Ereignis wird ausgelöst, das die Aktualisierung der einzelnen **ListBox**-Objekte mit den aktuellen MIDI-Geräten bewirkt.

[!code-cs[StartWatchers](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetStartWatchers)]

Kurz nach dem Starten der einzelnen **DeviceWatcher** wird die Enumeration der aktuellen mit dem System verbundenen Geräte beendet, und das [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/br225451)-Ereignis wird ausgelöst, das die Aktualisierung der einzelnen **ListBox**-Objekte mit den aktuellen MIDI-Geräten bewirkt.

Wenn der Benutzer ein Element im **ListBox** für MIDI-Eingabegeräte auswählt, wird das [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776)-Ereignis ausgelöst. Greifen Sie im Handler für dieses Ereignis auf die **DeviceInformationCollection**-Eigenschaft der Hilfsklasse zu, um die aktuelle Geräteliste abzurufen. Wenn die Liste Einträge enthält, wählen Sie das **DeviceInformation**-Objekt aus, dessen Index dem **ListBox** des [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/br209768)-Steuerelements entspricht.

Erstellen Sie das [**MidiInPort**](https://msdn.microsoft.com/library/windows/apps/dn894770)-Objekt, das das ausgewählte Eingabegerät darstellt, indem Sie [**MidiInPort.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn894776) aufrufen und die [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437)-Eigenschaft des ausgewählten Geräts übergeben.

Registrieren Sie einen Handler für das [**MessageReceived**](https://msdn.microsoft.com/library/windows/apps/dn894781)-Ereignis, das ausgelöst wird, wenn eine MIDI-Nachricht über das angegebene Gerät empfangen wird.

[!code-cs[InPortSelectionChanged](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetInPortSelectionChanged)]

Wenn der **MessageReceived**-Handler aufgerufen wird, ist die Nachricht in der [**Message**](https://msdn.microsoft.com/library/windows/apps/dn894783)-Eigenschaft von **MidiMessageReceivedEventArgs** enthalten. Der [**Type**](https://msdn.microsoft.com/library/windows/apps/dn894726) des Nachrichtenobjekts ist ein Wert aus der [**MidiMessageType**](https://msdn.microsoft.com/library/windows/apps/dn894786)-Enumeration, der den Typ der empfangenen Nachricht angibt. Die Daten der Nachricht hängen vom Typ der Nachricht ab. In diesem Beispiel wird überprüft, ob die Nachricht ein Hinweis auf eine Nachricht ist; wenn dies der Fall ist, werden der MIDI-Kanal, der Hinweis und die Geschwindigkeit der Nachricht ausgegeben.

[!code-cs[MessageReceived](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetMessageReceived)]

Der [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776)-Handler für das **ListBox** mit Ausgabegeräten funktioniert genauso wie der Handler für Eingabegeräte, mit der Ausnahme, dass kein Ereignishandler registriert ist.

[!code-cs[OutPortSelectionChanged](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetOutPortSelectionChanged)]

Nachdem das Ausgabegerät erstellt wurde, können Sie eine Nachricht senden, indem Sie eine neue [**IMidiMessage**](https://msdn.microsoft.com/library/windows/apps/dn911508) für den Typ der Nachricht erstellen, die Sie senden möchten. In diesem Beispiel ist die Nachricht eine [**NoteOnMessage**](https://msdn.microsoft.com/library/windows/apps/dn894817). Die [**SendMessage**](https://msdn.microsoft.com/library/windows/apps/dn894730)-Methode des [**IMidiOutPort**](https://msdn.microsoft.com/library/windows/apps/dn894727)-Objekts wird zum Senden der Nachricht aufgerufen.

[!code-cs[SendMessage](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetSendMessage)]

Achten Sie beim Deaktivieren Ihrer App darauf, die App-Ressourcen zu bereinigen. Heben Sie die Registrierung der Ereignishandler auf, und legen Sie die Objekte für MIDI-Eingabeport und -Ausgabeport auf NULL fest. Beenden Sie die Geräteüberwachungselemente, und legen Sie sie auf NULL fest.

[!code-cs[CleanUp](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetCleanUp)]

## Verwenden des integrierten Windows General MIDI-Synthesizers

Beim Aufzählen der MIDI-Ausgabegeräte mit dem oben beschriebenen Verfahren erkennt Ihre App ein MIDI-Gerät namens „Microsoft GS Wavetable Synth“. Hierbei handelt es sich um einen integrierten General MIDI-Synthesizer, der von Ihrer App aus wiedergeben werden kann. Bei dem Versuch, einen MIDI-Out-Port für dieses Gerät zu erstellen, tritt jedoch ein Fehler auf, wenn die SDK-Erweiterung für den integrierten Synthesizer nicht in Ihrem Projekt enthalten ist.

**So fügen Sie die General MIDI Synth SDK-Erweiterung in Ihrem App-Projekt hinzu**

1.  Klicken Sie im **Projektmappen-Explorer** unter Ihrem Projekt mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen...** aus.
2.  Erweitern Sie den Knoten **Universelle Windows-App**.
3.  Wählen Sie **Erweiterungen**.
4.  Wählen Sie aus der Liste der Erweiterungen **Microsoft General MIDI-DLS für universelle Windows-Apps**.
    **Hinweis** Wenn mehrere Versionen der Erweiterung vorhanden sind, müssen Sie die Version auswählen, die Ihre App als Ziel aufweist. Auf der Registerkarte **Anwendung** der Projekteigenschaften können Sie sehen, welche SDK-Version Ihre App als Ziel aufweist.

 

 






<!--HONumber=Mar16_HO1-->


