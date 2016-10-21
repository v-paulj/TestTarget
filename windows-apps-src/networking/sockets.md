---
author: DelfCo
description: "Sie können als UWP-App-Entwickler (Universelle Windows-Plattform) Windows.Networking.Sockets und Winsock zur Kommunikation mit anderen Geräten verwenden."
title: Sockets
ms.assetid: 23B10A3C-E33F-4CD6-92CB-0FFB491472D6
translationtype: Human Translation
ms.sourcegitcommit: 4557fa59d377edc2ae5bf5a9be63516d152949bb
ms.openlocfilehash: 49a9ae4d7d3994ad7fbb78fc9dc60cdd9dca07c3

---

# Sockets

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Wichtige APIs**

-   [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)
-   [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms740673)

Sie können als UWP-App-Entwickler (Universelle Windows-Plattform) [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) und [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms737523) zur Kommunikation mit anderen Geräten verwenden. In diesem Thema finden Sie umfassende Anleitungen zur Verwendung des **Windows.Networking.Sockets**-Namespace, um Netzwerkvorgänge durchzuführen.

>**Hinweis**: Zur Sicherstellung der [Netzwerkisolation](https://msdn.microsoft.com/library/windows/apps/hh770532.aspx) verbietet das System die Einrichtung von Socketverbindungen (Sockets oder WinSock) zwischen zwei UWP-Apps, die auf demselben Computer ausgeführt werden, über die lokale Loopbackadresse (127.0.0.0) oder durch explizite Angabe der lokalen IP-Adresse. Dies bedeutet, dass Sie Sockets nicht für die Kommunikation zwischen zwei UWP-Apps verwenden können. UWP stellt andere Mechanismen für die Kommunikation zwischen Apps zur Verfügung. Einzelheiten finden Sie unter [App-zu-App-Kommunikation](https://msdn.microsoft.com/windows/uwp/app-to-app/index).

## Grundlegende TCP-Socketvorgänge

Ein TCP-Socket ermöglicht Low-Level-Übertragungen von Netzwerkdaten in beide Richtungen für langlebige Verbindungen. TCP-Sockets sind das zugrunde liegende Feature, das von den meisten im Internet genutzten Netzwerkprotokollen verwendet wird. In diesem Abschnitt erfahren Sie, wie Sie in einer UWP-App das Senden und Empfangen von Daten mit einem TCP-Datenstromsocket mithilfe der [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)-Klasse und der [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906)-Klasse als Teil des [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)-Namespace aktivieren. In diesem Abschnitt wird eine einfache App erstellt, die als Echoserver und -client fungiert, um grundlegende TCP-Vorgänge zu veranschaulichen.

**Erstellen eines TCP-Echoservers**

Im folgenden Codebeispiel wird gezeigt, wie Sie ein [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906)-Objekt erstellen und mit dem Überwachen eingehender TCP-Verbindungen beginnen.

```csharp
try
{
    //Create a StreamSocketListener to start listening for TCP connections.
    Windows.Networking.Sockets.StreamSocketListener socketListener = new Windows.Networking.Sockets.StreamSocketListener();

    //Hook up an event handler to call when connections are received.
    socketListener.ConnectionReceived += SocketListener_ConnectionReceived;

    //Start listening for incoming TCP connections on the specified port. You can specify any port that' s not currently in use.
    await socketListener.BindServiceNameAsync("1337");
}
catch (Exception e)
{
    //Handle exception.
}
```

Im folgenden Codebeispiel wird der SocketListener\_ConnectionReceived-Ereignishandler implementiert, der im vorhergehenden Codebeispiel an das [**StreamSocketListener.ConnectionReceived**](https://msdn.microsoft.com/library/windows/apps/hh701494)-Ereignis angefügt wurde. Dieser Ereignishandler wird immer, wenn ein Remoteclient eine Verbindung mit dem Echoserver hergestellt hat, von der [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906)-Klasse aufgerufen.

```csharp
private async void SocketListener_ConnectionReceived(Windows.Networking.Sockets.StreamSocketListener sender, 
    Windows.Networking.Sockets.StreamSocketListenerConnectionReceivedEventArgs args)
{
    //Read line from the remote client.
    Stream inStream = args.Socket.InputStream.AsStreamForRead();
    StreamReader reader = new StreamReader(inStream);
    string request = await reader.ReadLineAsync();
    
    //Send the line back to the remote client.
    Stream outStream = args.Socket.OutputStream.AsStreamForWrite();
    StreamWriter writer = new StreamWriter(outStream);
    await writer.WriteLineAsync(request);
    await writer.FlushAsync();
}
```

**Erstellen eines TCP-Echoclients**

Im folgenden Codebeispiel wird veranschaulicht, wie Sie ein [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)-Objekt erstellen, eine Verbindung mit dem Remoteserver herstellen, eine Anforderung senden und eine Antwort empfangen.

```csharp
try
{
    //Create the StreamSocket and establish a connection to the echo server.
    Windows.Networking.Sockets.StreamSocket socket = new Windows.Networking.Sockets.StreamSocket();
    
    //The server hostname that we will be establishing a connection to. We will be running the server and client locally,
    //so we will use localhost as the hostname.
    Windows.Networking.HostName serverHost = new Windows.Networking.HostName("localhost");
    
    //Every protocol typically has a standard port number. For example HTTP is typically 80, FTP is 20 and 21, etc.
    //For the echo server/client application we will use a random port 1337.
    string serverPort = "1337";
    await socket.ConnectAsync(serverHost, serverPort);

    //Write data to the echo server.
    Stream streamOut = socket.OutputStream.AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    string request = "test";
    await writer.WriteLineAsync(request);
    await writer.FlushAsync();

    //Read data from the echo server.
    Stream streamIn = socket.InputStream.AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string response = await reader.ReadLineAsync();
}
catch (Exception e)
{
    //Handle exception here.            
}
```

## Grundlegende UDP-Socketvorgänge

Ein UDP-Socket bietet die Low-Level-Übertragungen von Netzwerkdaten in beide Richtungen für Netzwerkkommunikation, bei der keine bestehende Verbindung erforderlich ist. Da UDP-Sockets die Verbindung auf beiden Endpunkten nicht aufrechterhalten, stellen Sie eine schnelle und einfache Lösung für Netzwerkverbindungen zwischen Remotecomputern dar. Allerdings stellen UDP-Sockets die Integrität von Netzwerkpaketen oder überhaupt ein Erreichen des Remoteziels nicht sicher. Beispiele für Anwendungen, die UDP-Sockets verwenden, sind Clients für die Erkennung im lokalen Netzwerk und für lokalen Chat. In diesem Abschnitt wird die Verwendung der [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)-Klasse zum Senden und Empfangen von UDP-Nachrichten am Beispiel eines einfachen Echoservers und -clients veranschaulicht.

**Erstellen eines UDP-Echoservers**

Im folgenden Codebeispiel wird veranschaulicht, wie ein [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)-Objekt erstellt und an einen bestimmten Port gebunden wird, damit Sie eingehende UDP-Nachrichten überwachen können.

```csharp
Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();

socket.MessageReceived += Socket_MessageReceived;

//You can use any port that is not currently in use already on the machine.
string serverPort = "1337";

//Bind the socket to the serverPort so that we can start listening for UDP messages from the UDP echo client.
await socket.BindServiceNameAsync(serverPort);
```

Im folgenden Codebeispiel wird der **Socket\_MessageReceived**-Ereignishandler zum Lesen einer Nachricht implementiert, die von einem Client empfangen wurde. Die gleiche Nachricht wird daraufhin zurückgesendet.

```csharp
private async void Socket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    //Read the message that was received from the UDP echo client.
    Stream streamIn = args.GetDataStream().AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string message = await reader.ReadLineAsync();

    //Create a new socket to send the same message back to the UDP echo client.
    Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();
    
    //Use a separate port number for the UDP echo client because both will be unning on the same machine.
    string clientPort = "1338"
    Stream streamOut = (await socket.GetOutputStreamAsync(args.RemoteAddress, clientPort)).AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    await writer.WriteLineAsync(message);
    await writer.FlushAsync();
}
```

**Erstellen eines UDP-Echoclients**

Im folgenden Codebeispiel wird veranschaulicht, wie Sie ein [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)-Objekt erstellen und an einen bestimmten Port binden, damit Sie eingehende UDP-Nachrichten überwachen und eine UDP-Nachricht an den UDP-Echoserver senden können.

```csharp
private async void testUdpSocketServer()
{
    Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();
    
    socket.MessageReceived += Socket_MessageReceived;
    
    //You can use any port that is not currently in use already on the machine. We will be using two separate and random 
    //ports for the client and server because both the will be running on the same machine.
    string serverPort = "1337";
    string clientPort = "1338";
    
    //Because we will be running the client and server on the same machine, we will use localhost as the hostname.
    Windows.Networking.HostName serverHost = new Windows.Networking.HostName("localhost");
    
    //Bind the socket to the clientPort so that we can start listening for UDP messages from the UDP echo server.
    await socket.BindServiceNameAsync(clientPort);
                
    //Write a message to the UDP echo server.
    Stream streamOut = (await socket.GetOutputStreamAsync(serverHost, serverPort)).AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    string message = "Hello, world!";
    await writer.WriteLineAsync(message);
    await writer.FlushAsync();
}
```

Im folgenden Codebeispiel wird der **Socket\_MessageReceived**-Ereignishandler zum Lesen einer Nachricht implementiert, die vom UDP-Echoserver empfangen wurde.

```csharp
private async void Socket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, 
    Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    //Read the message that was received from the UDP echo server.
    Stream streamIn = args.GetDataStream().AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string message = await reader.ReadLineAsync();
}
```

## Hintergrundvorgänge und der Socketbroker

Wenn Ihre App Verbindungen oder Daten für Sockets empfängt, müssen Sie darauf vorbereitet sein, diese Vorgänge ordnungsgemäß auszuführen, während die App nicht im Vordergrund ist. Hierzu verwenden Sie den Socketbroker. Weitere Informationen zur Verwendung des Socketbrokers finden Sie unter [Netzwerkkommunikation im Hintergrund](network-communications-in-the-background.md).

## Sendevorgänge im Batch

Ab Windows 10 unterstützt Windows.Networking.Sockets Sendevorgänge im Batch. Dadurch können Sie mehrere Datenpuffer zusammen mit viel niedrigerem Kontextwechselaufwand senden, als wenn Sie die einzelnen Puffer separat senden würden. Dies ist besonders hilfreich, wenn Ihre App VoIP, VPN oder andere Aufgaben ausführt, die mit dem effizienten Verschieben großer Datenmengen verbunden sind.

Bei jedem Aufruf von WriteAsync für einen Socket wird ein Kernelübergang zum Erreichen des Netzwerkstapels ausgelöst. Wenn eine App viele Puffer zu einem Zeitpunkt schreibt, erfolgt bei jedem Schreibvorgang ein separater Kernelübergang und führt zu erheblichem Aufwand. Das neue im Batch-Sendemuster optimiert die Häufigkeit der Kernelübergänge. Diese Funktionalität ist derzeit auf [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) und verbundene [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)-Instanzen begrenzt.

Hier ist ein Beispiel dazu, wie eine App eine große Anzahl von Puffern auf eine nicht optimale Weise senden würde.

```csharp
// Send a set of packets inefficiently, one packet at a time.
// This is not recommended if you have many packets to send
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = stream.OutputStream;

foreach (IBuffer packet in packetsToSend)
{
    // Incurs kernel transition overhead for each packet
    await outputStream.WriteAsync(packet);
}
```

Dieses Beispiel zeigt eine effiziente Möglichkeit, eine große Anzahl von Puffern zu senden. Da diese Methode Funktionen der C#-Sprache verwendet, steht sie nur C#-Programmierern zur Verfügung. Durch das Senden mehrerer Pakete zu einem bestimmten Zeitpunkt, kann das System in diesem Beispiel Sendevorgänge in Batches vornehmen und damit Kernelübergänge zur Verbesserung der Leistung optimieren.

```csharp
// More efficient way to send packets.
// This way enables the system to do batched sends
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = stream.OutputStream;

int i = 0;
Task[] pendingTasks = new Tast[packetsToSend.Count];
foreach (IBuffer packet in packetsToSend)
{
    // track all pending writes as tasks, but do not wait on one before peforming the next
    pendingTasks[i++] = outputStream.WriteAsync(packet).AsTask();
    // Do not modify any buffer' s contents until the pending writes are complete.
}
// Now, wait for all of the pending writes to complete
await Task.WaitAll(pendingTasks);
```

Dieses Beispiel zeigt eine weitere Möglichkeit, eine große Anzahl von Puffern in einer Weise zu senden, die mit dem Senden in Batches kompatibel ist. Und da hierbei keine C#-spezifischen Funktionen verwendet werden, gilt es für alle Sprachen (obwohl es hier im C#-Code gezeigt wird). Stattdessen wird das geänderte Verhalten in dem **OutputStream**-Member der [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)- und [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)-Klassen verwendet, das mit Windows 10 neu hinzugekommen ist.

```csharp
// More efficient way to send packets in Windows 10, using the new behavior of OutputStream.FlushAsync().
int i = 0;
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = socket.OutputStream;

var pendingWrites = new IAsyncOperationWithProgress<uint,uint> [packetsToSend.Count];

foreach (IBuffer packet in packetsToSend)
{
    pendingWrites[i++] = outputStream.WriteAsync(packet);
    // Do not modify any buffer' s contents until the pending writes are complete.
}

// Wait for all pending writes to complete. This step enables batched sends on the output stream.
await outputStream.FlushAsync();
```

In früheren Versionen von Windows wurde sofort **FlushAsync** zurückgegeben und dadurch konnte nicht garantiert werden, dass alle Vorgänge für den Stream bereits abgeschlossen wurden. In Windows 10 hat sich das Verhalten geändert. **FlushAsync** kehrt jetzt garantiert zurück, nachdem alle Vorgänge im Ausgabedatenstrom abgeschlossen sind.

Es gibt einige wichtige Einschränkungen, die für Schreibvorgänge im Batch in Ihrem Code gelten.

-   Sie können den Inhalt der zu schreibenden **IBuffer**-Instanzen nicht ändern, bis der asynchrone Schreibvorgang abgeschlossen ist.
-   Das **FlushAsync**-Muster funktioniert nur bei **StreamSocket.OutputStream** und **DatagramSocket.OutputStream**.
-   Das **FlushAsync**-Muster funktioniert nur ab Windows 10.
-   Verwenden Sie in anderen Fällen **Task.WaitAll** statt des **FlushAsync**-Musters.

## Portfreigabe für DatagramSocket

Windows 10 führt eine neue [**DatagramSocketControl**](https://msdn.microsoft.com/library/windows/apps/hh701190)-Eigenschaft ein, [**MulticastOnly**](https://msdn.microsoft.com/library/windows/apps/dn895368). Damit können Sie angeben, dass der betreffende **DatagramSocket** parallel zu anderen Win32- oder WinRT-Multicastsockets vorhanden sein kann, die an die gleiche Adresse/den gleichen Port gebunden sind.

## Bereitstellen eines Clientzertifikats mit der StreamSocket-Klasse

Die [**Windows.Networking.StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)-Klasse unterstützt die Verwendung von SSL/TLS zum Authentifizieren des Servers, mit dem die App kommuniziert. In bestimmten Fällen muss auch die App selbst mit einem TLS-Clientzertifikat am Server authentifiziert werden. In Windows 10 können Sie ein Clientzertifikat zu dem [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226893)-Objekt bereitstellen (dies muss festgelegt werden, bevor der TLS-Handshake gestartet wird). Wenn der Server das Clientzertifikat anfordert, reagiert Windows mit dem bereitgestellten Zertifikat.

Hier ist ein Codeausschnitt, in dem die Implementierung dazu dargestellt wird:

```csharp
var socket = new StreamSocket();
Windows.Security.Cryptography.Certificates.Certificate certificate = await GetClientCert();
socket.Control.ClientCertificate = certificate;
await socket.ConnectAsync(destination, SocketProtectionLevel.Tls12);
```

## Ausnahmen in "Windows.Networking.Sockets"

Der Konstruktor für die [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113)-Klasse in Verbindung mit Sockets kann eine Ausnahme auslösen, wenn die übergebene Zeichenfolge kein gültiger Hostname ist (enthält Zeichen, die in einem Hostnamen nicht zulässig sind). Wenn eine App vom Benutzer eine Eingabe für das **HostName**-Element erhält, sollte sich der Konstruktor innerhalb eines try/catch-Blocks befinden. Wenn eine Ausnahme ausgelöst wird, kann die App den Benutzer benachrichtigen und einen neuen Hostnamen anfordern.

Der [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)-Namespace enthält praktische Hilfsmethoden und Enumerationen für die Behandlung von Fehlern bei der Verwendung von Sockets und WebSockets. Mit ihnen lassen sich spezifische Netzwerkausnahmen in der App unterschiedlich behandeln.

Ein Fehler in einem [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)-, [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)- oder [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906)-Vorgang wird als **HRESULT**-Wert zurückgegeben. Mit der [**SocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701462)-Methode wird ein Netzwerkfehler aus einem Socketvorgang in einen [**SocketErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh701457)-Enumerationswert konvertiert. Die meisten **SocketErrorStatus**-Enumerationswerte entsprechen einem vom systemeigenen WindowsSockets-Vorgang zurückgegebenen Fehler. Eine App kann nach bestimmten **SocketErrorStatus**-Enumerationswerten filtern, um das App-Verhalten je nach Ausnahmeursache zu ändern.

Ein Fehler in einem [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)- oder [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)-Vorgang wird als **HRESULT**-Wert zurückgegeben. Mit der [**WebSocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701529)-Methode wird ein Netzwerkfehler aus einem WebSocket-Vorgang in einen [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818)-Enumerationswert konvertiert. Die meisten **WebErrorStatus**-Enumerationswerte entsprechen einem vom systemeigenen HTTP-Clientvorgang zurückgegebenen Fehler. Eine App kann nach bestimmten **WebErrorStatus**-Enumerationswerten filtern, um das App-Verhalten je nach Ausnahmeursache zu ändern.

Bei Parameterprüfungsfehlern kann eine App den **HRESULT**-Wert aus der Ausnahme auch verwenden, um ausführlichere Informationen zum zugehörigen Fehler zu erhalten. Mögliche **HRESULT**-Werte sind in der Headerdatei *Winerror.h* aufgeführt. Für die meisten Parameterüberprüfungsfehler wird der **HRESULT**-Wert **E\_INVALIDARG** zurückgegeben.

## Die Winsock-API

Sie können [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms740673) auch in Ihrer UWP-App verwenden. Die unterstützte Winsock-API basiert auf der von Windows Phone 8.1 Microsoft Silverlight und unterstützt weiterhin die meisten Typen, Eigenschaften und Methoden (einige APIs, die als veraltet eingestuft wurden, wurden entfernt). Weitere Informationen zur Winsock-Programmierung finden Sie [hier](https://msdn.microsoft.com/library/windows/desktop/ms740673).





<!--HONumber=Aug16_HO3-->


