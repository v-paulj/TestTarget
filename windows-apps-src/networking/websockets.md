---
author: DelfCo
description: WebSockets stellt einen Mechanismus für die schnelle und sichere bidirektionale Webkommunikation zwischen einem Client und einem Server mithilfe von HTTP(S) bereit.
title: WebSockets
ms.assetid: EAA9CB3E-6A3A-4C13-9636-CCD3DE46E7E2
---

# WebSockets

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

**Wichtige APIs**

-   [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)
-   [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)

WebSockets stellt einen Mechanismus für die schnelle und sichere bidirektionale Webkommunikation zwischen einem Client und einem Server mithilfe von HTTP(S) bereit.

Mit dem [WebSocket-Protokoll](http://tools.ietf.org/html/rfc6455) werden Daten unmittelbar über eine Vollduplex-Einzelsocketverbindung übertragen, wodurch Nachrichten von beiden Endpunkten in Echtzeit gesendet und empfangen werden können. WebSockets eignen sich ideal für den Einsatz in Echtzeitspielen, bei denen Sofortbenachrichtigungen aus sozialen Netzwerken und die aktuelle Anzeige von Daten (z. B. Spielstatistiken) sicher sein und eine schnelle Datenübertragung verwenden müssen. Entwickler von UWP (Universelle Windows-Plattform)-Apps können die [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)-Klasse und die [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)-Klasse zum Herstellen einer Verbindung mit Servern verwenden, die das Websocket-Protokoll unterstützen.

| MessageWebSocket                                                         | StreamWebSocket                                                                               |
|--------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| Geeignet für die meisten Szenarien, in denen die Nachrichten nicht besonders groß sind.   | Geeignet für Szenarien, in denen große Dateien (z. B. Fotos oder Videos) übertragen werden. |
| Ermöglicht die Benachrichtigung, dass eine gesamte WebSocket-Nachricht erhalten wurde. | Ermöglicht, dass bei jedem Lesevorgang Abschnitte einer Nachricht gelesen werden.                             |
| Unterstützt UTF-8-Nachrichten und binäre Nachrichten.                                 | Unterstützt nur binäre Nachrichten.                                                                |
| Ähnlich wie ein UDP- oder Datagrammsocket.                                     | Ähnlich wie ein TCP- oder Streamsocket.                                                            |

In den meisten Fällen sollten Sie eine sichere WebSocket-Verbindung verwenden, um gesendete und empfangenen Daten zu verschlüsseln. Dadurch ist es wahrscheinlicher, dass die Verbindung funktioniert, da andernfalls viele Proxys unverschlüsselte WebSocket-Verbindungen ablehnen. Das WebSocket-Protokoll definiert die folgenden zwei URI-Schemas.

-   ws: - wird für unverschlüsselte Verbindungen verwendet.
-   wss: - wird für Verbindungen verwendet, die verschlüsselt werden sollten.

Verwenden Sie zum Verschlüsseln einer WebSocket-Verbindung das wss:-URI-Schema, z. B. `wss://www.contoso.com/mywebservice`.

## Verwenden von MessageWebSocket

Mit dem [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) können bei jedem Lesevorgang Abschnitte einer Nachricht gelesen werden. Ein **MessageWebSocket** wird normalerweise in Szenarien verwendet, in denen Nachrichten nicht besonders groß sind. Es werden sowohl UTF-8- als auch Binärdateien unterstützt.

Der Code in diesem Abschnitt erstellt ein neues [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)-Element, stellt eine Verbindung mit einem WebSocket-Server her und sendet Daten an den Server. Nachdem erfolgreich eine Verbindung hergestellt wurde, wartet die App auf das Auslösen des [**MessageWebSocket.MessageReceived**](https://msdn.microsoft.com/library/windows/apps/br241358)-Ereignisses, das angibt, dass Daten empfangen wurden.

Dieses Beispiel verwendet den WebSocket.org-Echoserver, einen Dienst, der Zeichenfolgen zurück an den Absender sendet. Mithilfe des wss:-Protokollspezifizierers verwendet dieses Beispiel eine sichere Verbindung zum Senden und Empfangen von Nachrichten.

> [!div class="tabbedCodeSnippets"]
> ```cpp
> void Game::InitWebSockets()
> {
>     // Create a new web socket
>     m_messageWebSocket = ref new MessageWebSocket();
> 
>     // Set the message type to UTF-8
>     m_messageWebSocket->Control->MessageType = Windows::Networking::Sockets::SocketMessageType::Utf8;
> 
>     // Register callbacks for notifications of interest
>     m_messageWebSocket->MessageReceived += 
>        ref new TypedEventHandler<MessageWebSocket^, MessageWebSocketMessageReceivedEventArgs^>(this, &Game::WebSocketMessageReceived);
>     m_messageWebSocket->Closed += ref new TypedEventHandler<IWebSocket^, WebSocketClosedEventArgs^>(this, &Game::WebSocketClosed);
> 
>     // This test code uses the websocket.org echo service to illustrate sending a string and receiving the echoed string back
>     // Note that wss: makes this an encrypted connection.
>     m_serverUri = ref new Uri("wss://echo.websocket.org");
> 
>     // Establish the connection, and set m_socketConnected on success
>     create_task(m_messageWebSocket->ConnectAsync(m_serverUri)).then([this] (task<void> previousTask)
>     {
>         try
>         {
>             // Try getting all exceptions from the continuation chain above this point.
>             previousTask.get();
> 
>             // websocket connected. update state variable
>             m_socketConnected = true;
>             OutputDebugString(L"Successfully initialized websockets\n");
>         }
>         catch (Platform::COMException^ exception)
>         {
>             // Add code here to handle any exceptions
>             // HandleException(exception);
> 
>         }
>     });
> }
> ```
> ```cs
> MessageWebSocket webSock = new MessageWebSocket();
> 
> //In this case we will be sending/receiving a string so we need to set the MessageType to Utf8.
> webSock.Control.MessageType = SocketMessageType.Utf8;
> 
> //Add the MessageReceived event handler.
> webSock.MessageReceived += WebSock_MessageReceived;
> 
> //Add the Closed event handler.
> webSock.Closed += WebSock_Closed;
> 
> Uri serverUri = new Uri("wss://echo.websocket.org");
> 
> try
> {
>     //Connect to the server.
>     await webSock.ConnectAsync(serverUri);
> 
>     //Send a message to the server.
>     await WebSock_SendMessage(webSock, "Hello, world!");
> }
> catch (Exception ex)
> {
>     //Add code here to handle any exceptions
> }
> ```

Nachdem Sie die WebSocket-Verbindung initialisiert haben, muss Ihr Code folgende Schritte ausführen, um ordnungsgemäß Daten zu senden und zu empfangen.

### Implementieren eines Rückrufs für das MessageWebSocket.MessageReceived-Ereignis

Vor dem Herstellen einer Verbindung und dem Senden von Daten mit einem WebSocket muss Ihre App einen Ereignisrückruf registrieren, um die Benachrichtigung beim Empfang von Daten zu empfangen. Wenn das [**MessageWebSocket.MessageReceived**](https://msdn.microsoft.com/library/windows/apps/br241358)-Ereignis eintritt, wird der registrierte Rückruf aufgerufen, und es werden Daten von [**MessageWebSocketMessageReceivedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br226852) empfangen. Bei diesem Beispiel wird davon ausgegangen, dass die gesendeten Nachrichten im UTF-8-Format vorliegen.

Die folgende Beispielfunktion empfängt eine Zeichenfolge von einem verbundenen WebSocket-Server und gibt die Zeichenfolgen im Debugger-Ausgabefenster aus.

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::WebSocketMessageReceived(MessageWebSocket^ sender, MessageWebSocketMessageReceivedEventArgs^ args)
>{
>    DataReader^ messageReader = args->GetDataReader();
>    messageReader->UnicodeEncoding = Windows::Storage::Streams::UnicodeEncoding::Utf8;
>
>    String^ readString = messageReader->ReadString(messageReader->UnconsumedBufferLength);
>    // Data has been read and is now available from the readString variable.
>    swprintf(m_debugBuffer, 511, L"WebSocket Message received: %s\n", readString->Data());
>    OutputDebugString(m_debugBuffer);
>}
>```
>```csharp
>//The MessageReceived event handler.
>private void WebSock_MessageReceived(MessageWebSocket sender, MessageWebSocketMessageReceivedEventArgs args)
>{
>    DataReader messageReader = args.GetDataReader();
>    messageReader.UnicodeEncoding = UnicodeEncoding.Utf8;
>    string messageString = messageReader.ReadString(messageReader.UnconsumedBufferLength);
>
>    //Add code here to do something with the string that is received.
>}
>```

###  Implementieren eines Rückrufs für das MessageWebSocket.Closed-Ereignis

Vor dem Herstellen einer Verbindung und Senden von Daten mit einem WebSocket muss Ihre App ein Rückrufereignis registrieren, um eine Benachrichtigung zu empfangen, wenn der WebSocket vom WebSocket-Server geschlossen wird. Wenn das [**MessageWebSocket.Closed**](https://msdn.microsoft.com/library/windows/apps/hh701364)-Ereignis eintritt, wird der registrierte Rückruf aufgerufen, um anzugeben, dass die Verbindung vom WebSocket-Server geschlossen wurde.

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::WebSocketClosed(IWebSocket^ sender, WebSocketClosedEventArgs^ args)
>{
>    // The method may be triggered remotely by the server sending unsolicited close frame or locally by Close()/delete operator.
>    // This method assumes we saved the connected WebSocket to a variable called m_messageWebSocket
>    if (m_messageWebSocket != nullptr)
>    {
>        delete m_messageWebSocket;
>        m_messageWebSocket = nullptr;
>        OutputDebugString(L"Socket was closed\n");
>    }
>    m_socketConnected = false;
> }
>```
>```csharp
>//The Closed event handler
>private void WebSock_Closed(IWebSocket sender, WebSocketClosedEventArgs args)
>{
>    //Add code here to do something when the connection is closed locally or by the server
>}
>```

###  Senden einer Nachricht auf einem WebSocket

Sobald eine Verbindung hergestellt ist, kann der WebSocket-Client Daten an den Server senden. Die [**DataWriter.StoreAsync**](https://msdn.microsoft.com/library/windows/apps/br208171)-Methode gibt einen Parameter zurück, der einer ganzen Zahl ohne Vorzeichen zugeordnet ist. Dadurch ändern sich die Definitionen der Aufgaben, die zum Senden der Nachricht und Herstellen der Verbindung ausgeführt werden.

**Hinweis**   Wenn Sie ein neues DataWriter-Objekt mit dem OutputStream von MessageWebSocket erstellen, übernimmt der DataWriter den Besitz am OutputStream, und die Zuordnung von Outputstream wird aufgehoben, wenn der DataWriter den Gültigkeitsbereich verlässt. Dadurch schlagen alle nachfolgenden Versuche, den OutputStream zu verwenden, mit einem HRESULT-Wert 0x80000013 fehl. Um zu vermeiden, dass der OutputStream neu zugeordnet wird, ruft dieser Code die DetachStream-Methode des DataWriters auf, die den Besitzer des Datenstroms an das WebSocket-Objekt zurückgibt.

Die folgende Funktion sendet die angegebene Zeichenfolge an einen verbundenen WebSocket und gibt eine Bestätigungsnachricht im Ausgabefenster des Debuggers aus.

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::SendWebSocketMessage(Windows::Networking::Sockets::MessageWebSocket^ sendingSocket, Platform::String^ message)
>{
>    if (m_socketConnected)
>    {
>        // WebSocket is connected, so send a message
>        m_messageWriter = ref new DataWriter(sendingSocket->OutputStream);
>
>        m_messageWriter->WriteString(message);
>
>        // Send the data as one complete message
>        create_task(m_messageWriter->StoreAsync()).then([this] (unsigned int)
>        {
>            // Send Completed
>            m_messageWriter->DetachStream();    // give the stream back to m_messageWebSocket
>            OutputDebugString(L"Sent websocket message\n");
>        })
>            .then([this] (task<void>> previousTask)
>        {
>            try
>            {
>                // Try getting all exceptions from the continuation chain above this point.
>                previousTask.get();
>            }
>            catch (Platform::COMException ^ex)
>            {
>                // Add code to handle the exception
>                // HandleException(exception);
>            }
>        });
>    }
>}
>```
>```csharp
>//Send a message to the server.
>private async Task WebSock_SendMessage(MessageWebSocket webSock, string message)
>{
>    DataWriter messageWriter = new DataWriter(webSock.OutputStream);
>    messageWriter.WriteString(message);
>    await messageWriter.StoreAsync();
>}
>```

## Verwenden erweiterter Steuerelemente mit WebSockets

[
              **MessageWebSocket**
            ](https://msdn.microsoft.com/library/windows/apps/br226842) und [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) folgen dem gleichen Modell für die Verwendung erweiterter Steuerelemente. Den zuvor genannten Hauptklassen entsprechen verwandte Klassen für den Zugriff auf erweiterte Steuerelemente.

[
              **MessageWebSocketControl**
            ](https://msdn.microsoft.com/library/windows/apps/br226843) stellt Socketsteuerungsdaten für ein [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)-Objekt bereit.
[
              **StreamWebSocketControl**
            ](https://msdn.microsoft.com/library/windows/apps/br226924) stellt Socketsteuerungsdaten für ein [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)-Objekt bereit.
Das Grundmodell für die Verwendung erweiterter Steuerelemente ist für beide WebSocket-Typen gleich. In der folgenden Erläuterung wird exemplarisch ein [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)-Objekt verwendet, der gleiche Prozess kann jedoch auch mit einem Objekt vom Typ [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) ausgeführt werden.

1.  Erstellen Sie das [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)-Objekt.
2.  Verwenden Sie die [**StreamWebSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226934)-Eigenschaft, um die [**StreamWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226924)-Instanz abzurufen, die dem [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)-Objekt zugeordnet ist.
3.  Rufen Sie Eigenschaften für die [**StreamWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226924)-Instanz ab, oder legen Sie sie fest, um bestimmte erweiterte Steuerelemente abzurufen oder festzulegen.

[
            **StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) und [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) stellen Anforderungen für den Zeitpunkt der Festlegung erweiterter Steuerelemente.

-   Für alle erweiterten Steuerelemente von [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) muss die App vor dem Ausgeben eines Verbindungsvorgangs immer die Eigenschaft festlegen. Aufgrund dieser Anforderung empfiehlt es sich, alle Eigenschaften des Steuerelements unmittelbar nach dem Erstellen des **StreamWebSocket**-Objekts festzulegen. Versuchen Sie nicht, eine Steuerelementeigenschaft festzulegen, nachdem die [**StreamWebSocket.ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/br226933)-Methode aufgerufen wurde.
-   Für alle erweiterte Steuerelemente auf dem [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) mit Ausnahme des Nachrichtentyps müssen Sie die Eigenschaft vor dem Ausgeben eines Verbindungsvorgangs festlegen. Es empfiehlt sich, alle Eigenschaften des Steuerelements unmittelbar nach dem Erstellen des **MessageWebSocket**-Objekts festzulegen. Versuchen Sie mit Ausnahme der Nachrichtentyps nicht, eine Steuerelementeigenschaft zu ändern, nachdem [**MessageWebSocket.ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/br226859) aufgerufen wurde.

## WebSocket-Informationsklassen

[
              **MessageWebSocket**
            ](https://msdn.microsoft.com/library/windows/apps/br226842) und [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) besitzen jeweils eine entsprechende Klasse, die zusätzliche Informationen über eine WebSocket-Instanz bereitstellt.

[
              **MessageWebSocketInformation**
            ](https://msdn.microsoft.com/library/windows/apps/br226849) enthält Informationen zu [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842), und Sie rufen eine Instanz der Informationsklasse mit der [**MessageWebSocket.Information**](https://msdn.microsoft.com/library/windows/apps/br226861)-Eigenschaft ab.

[
              **StreamWebSocketInformation**
            ](https://msdn.microsoft.com/library/windows/apps/br226929) enthält Informationen zu [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923), und Sie rufen eine Instanz der Informationsklasse mit der [**StreamWebSocket.Information**](https://msdn.microsoft.com/library/windows/apps/br226935)-Eigenschaft ab.

Beachten Sie, dass alle Eigenschaften für beide Informationsklassen schreibgeschützt sind und Sie aktuelle Informationen zu einem beliebigen Zeitpunkt während der Lebensdauer eines Websocket-Objekts abrufen können.

## Behandeln von Netzwerkausnahmen

Ein Fehler in einem [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)-Vorgang oder [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)-Vorgang wird als **HRESULT**-Wert zurückgegeben. Mit der [**WebSocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701529)-Methode wird ein Netzwerkfehler aus einem WebSocket-Vorgang in einen [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818)-Enumerationswert konvertiert. Die meisten **WebErrorStatus**-Enumerationswerte entsprechen einem vom systemeigenen HTTP-Clientvorgang zurückgegebenen Fehler. Ihre App kann nach bestimmten **WebErrorStatus**-Enumerationswerten filtern, um das App-Verhalten je nach Ausnahmeursache zu ändern.

Bei Parameterprüfungsfehlern kann eine App den **HRESULT**-Wert aus der Ausnahme auch verwenden, um ausführlichere Informationen zum zugehörigen Fehler zu erhalten. Mögliche **HRESULT**-Werte sind in der Headerdatei *Winerror.h* aufgeführt. Für die meisten Parameterüberprüfungsfehler wird der **HRESULT**-Wert **E\_INVALIDARG** zurückgegeben.

## Festlegen von Timeouts für WebSocket-Vorgänge

Die MessageWebSocket-Klasse und die StreamWebSocket-Klasse verwenden einen internen Systemdienst, um WebSocket-Clientanforderungen zu senden und Antworten von einem Server zu empfangen. Der standardmäßige Timeoutwert, der für einen WebSocket-Verbindungsvorgang verwendet wird, beträgt 60 Sekunden. Wenn der HTTP-Server, der WebSockets unterstützt, vorübergehend nicht verfügbar oder durch einen Netzwerkausfall blockiert ist und der Server nicht auf die WebSocket-Verbindungsanforderung antwortet oder antworten kann, wartet der interne Systemdienst die standardmäßig festgelegten 60 Sekunden ab, bevor ein Fehler zurückgegeben wird, der in der WebSocket ConnectAsync-Methode zur Auslösung einer Ausnahme führt. Falls die Namensabfrage für einen HTTP-Servernamen im URI mehrere IP-Adressen für den Namen zurückgibt, testet der interne Systemdienst bis zu fünf IP-Adressen für die Website. Dabei wird jeweils das Standardtimeout von 60 Sekunden eingehalten, bevor ein Fehler auftritt. Eine App, die eine WebSocket-Verbindungsanforderung ausführt, kann mehrere Minuten lang erneute Versuche für mehrere IP-Adressen durchführen, bevor ein Fehler zurückgegeben und eine Ausnahme ausgelöst wird. Dieses Verhalten kann für Benutzer den Anschein erwecken, als ob die App nicht mehr reagiert. Das Standardtimeout, das für Sende- und Empfangsvorgänge nach dem Herstellen einer WebSocket-Verbindung verwendet wird, beträgt 30 Sekunden.

Wenn Apps schneller reagieren und diese Probleme verringert werden sollen, können Sie ein kürzeres Timeout für Verbindungsanforderungen festlegen. Somit tritt der Timeoutfehler früher auf als durch die Standardeinstellungen vorgegeben.

Auf ähnliche Weise legen Sie Timeouts für StreamWebSockets und MessageWebSockets fest. Das folgende Beispiel zeigt, wie Sie einen Timeout für StreamWebSocket festlegen, der Vorgang ist für eine MessageWebSocket ähnlich.

1.  Erstellen Sie eine Aufgabe, die nach der angegebenen Verzögerung mit einem Zeitgeber abgeschlossen wird.
2.  Erstellen Sie eine Aufgabe für den WebSocket-Vorgang mit einer cancellation\_token\_source, um den Abbruch zu unterstützen.
3.  Wenn die Aufgabe mit der angegebenen Timeoutverzögerung vor dem WebSocket-Verbindungsvorgang abgeschlossen wird, brechen Sie die Aufgabe für den WebSocket-Vorgang ab.

Das folgende Beispiel erstellt eine Aufgabe, die nach der angegebenen Verzögerung abgeschlossen ist, und erstellt eine zweite Aufgabe, die nach der angegebenen Verzögerung abbricht. Diese Klassen können mit StreamWebSocket und MessageWebSocket beim Herstellen einer Verbindung verwendet werden, um ein bestimmtes Timeout festzulegen. Ein Verwendungsbeispiel ist das Aufrufen der StreamWebSocket.ConnectAsync-Methode in einer Aufgabe mit einer cancellation\_token\_source, die den Abbruch unterstützt. Wenn das Timeout zuerst abläuft, wird die cancellation\_token\_source zum Abbrechen der Aufgabe für den WebSocket-Verbindungsvorgang verwendet.

```cpp
    #include <agents.h>
    #include <ppl.h>
    #include <ppltasks.h>

    using namespace concurrency;
    using namespace std;

    // Creates a task that completes after the specified delay.
    task<void> complete_after(unsigned int timeout)
    {
        // A task completion event that is set when a timer fires.
        task_completion_event<void> tce;

        // Create a non-repeating timer.
        shared_ptr<timer<int>> fire_once(new timer<int>(timeout, 0, nullptr, false));
        
        // Create a call object that sets the completion event after the timer fires.
        shared_ptr<call<int>> callback(new call<int>([tce](int)
        {
            tce.set();
        }));

        // Connect the timer to the callback and start the timer.
        fire_once->link_target(callback.get());
        fire_once->start();

        // Create a task that completes after the completion event is set.
        task<void> event_set(tce);

        // Create a continuation task that cleans up resources and
        // and return that continuation task.
        return event_set.then([callback, fire_once]()
        {
        });
    }

    // Cancels the provided task after the specifed delay, if the task
    // did not complete.
    template<typename T>
    task<T> cancel_after_timeout(task<T> t, cancellation_token_source cts, unsigned int timeout)
    {
        // Create a task that returns true after the specified task completes.
        task<bool> success_task = t.then([](T)
        {
            return true;
        });
        // Create a task that returns false after the specified timeout.
        task<bool> failure_task = complete_after(timeout).then([]
        {
            return false;
        });

        // Create a continuation task that cancels the overall task  
        // if the timeout task finishes first. 
        return (failure_task || success_task).then([t, cts](bool success)
        {
            if (!success)
            {
                // Set the cancellation token. The task that is passed as the 
                // t parameter should respond to the cancellation and stop 
                // as soon as it can.
                cts.cancel();
            }
 
            // Return the original task.
            return t;
        });
    }
```



<!--HONumber=May16_HO2-->


