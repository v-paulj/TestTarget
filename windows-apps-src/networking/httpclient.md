---
description: Verwenden Sie HttpClient und die übrige Windows.Web.Http-Namespace-API zum Senden und Empfangen von Informationen über die HTTP-Protokolle 1.1 und 2.0.
title: HttpClient
ms.assetid: EC9820D3-3A46-474F-8A01-AE1C27442750
---

# HttpClient

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

**Wichtige APIs**

-   [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)
-   [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)
-   [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631)

Verwenden Sie [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) und die übrige [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)-Namespace-API zum Senden und Empfangen von Informationen über die HTTP-Protokolle 1.1 und 2.0.

## Übersicht über HttpClient und den Windows.Web.Http-Namespace

Die Klassen im [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)-Namespace und die zugehörigen [**Windows.Web.Http.Headers**](https://msdn.microsoft.com/library/windows/apps/dn252713)- und [**Windows.Web.Http.Filters**](https://msdn.microsoft.com/library/windows/apps/dn298623)-Namespaces stellen eine Programmierschnittstelle für UWP-Apps bereit, die als HTTP-Client ausgeführt werden, um grundlegende GET-Anforderungen auszuführen oder die weiter unten aufgeführte erweiterte HTTP-Funktionalität zu implementieren.

-   Methoden für häufige Verben (**DELETE**, **GET**, **PUT** und **POST**). Jede dieser Anforderungen wird als asynchroner Vorgang gesendet.

-   Unterstützung für häufige Authentifizierungseinstellungen und -muster.

-   Zugriff auf SSL-Details (Secure Sockets Layer) zum Transport.

-   Möglichkeit zum Einbinden benutzerdefinierter Filter in erweiterte Apps.

-   Möglichkeit zum Abrufen, Festlegen und Löschen von Cookies.

-   Verfügbarkeit von Statusinformationen zu HTTP-Anforderungen in asynchronen Methoden.

Die [**Windows.Web.Http.HttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/dn279617)-Klasse stellt eine HTTP-Anforderungsnachricht dar, die von [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) gesendet wurde. Die [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631)-Klasse stellt eine HTTP-Antwortnachricht dar, die von einer HTTP-Anforderung empfangen wurde. HTTP-Nachrichten werden von IETF in [RFC 2616](http://go.microsoft.com/fwlink/p/?linkid=241642) definiert.

Der [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)-Namespace stellt HTTP-Inhalte als HTTP-Entitätskörper und Header dar, darunter auch Cookies. HTTP-Inhalte können einer HTTP-Anforderung oder einer HTTP-Antwort zugeordnet sein. Der **Windows.Web.Http**-Namespace stellt eine Reihe unterschiedlicher Klassen zum Darstellen von HTTP-Inhalten bereit.

-   [**HttpBufferContent**](https://msdn.microsoft.com/library/windows/apps/dn298625). Inhalt als Puffer
-   [**HttpFormUrlEncodedContent**](https://msdn.microsoft.com/library/windows/apps/dn298685). Inhalt als Name und Wert-Tupel, die mit dem MIME-Typ **application/x-www-form-urlencoded** codiert sind
-   [**HttpMultipartContent**](https://msdn.microsoft.com/library/windows/apps/dn298708). Inhalt in Form des MIME-Typs **multipart/\***
-   [**HttpMultipartFormDataContent**](https://msdn.microsoft.com/library/windows/apps/dn279596). Inhalt, der als MIME-Typ **multipart/form-data** codiert ist
-   [**HttpStreamContent**](https://msdn.microsoft.com/library/windows/apps/dn279649). Inhalt als Stream (Der interne Typ wird von der HTTP GET-Methode zum Empfangen von Daten und von der HTTP POST-Methode zum Hochladen von Daten verwendet.)
-   [**HttpStringContent**](https://msdn.microsoft.com/library/windows/apps/dn279661). Inhalt als Zeichenfolge
-   [**IHttpContent**](https://msdn.microsoft.com/library/windows/apps/dn279684) – Basisschnittstelle für Entwickler zum Erstellen eigener Inhaltsobjekte

Der Codeausschnitt im Abschnitt „Senden einer einfachen GET-Anforderung über HTTP“ verwendet die [**HttpStringContent**](https://msdn.microsoft.com/library/windows/apps/dn279661)-Klasse, um die HTTP-Antwort einer HTTP GET-Anforderung als Zeichenfolge darzustellen.

Der [**Windows.Web.Http.Headers**](https://msdn.microsoft.com/library/windows/apps/dn252713)-Namespace unterstützt die Erstellung von HTTP-Headern und -Cookies, die dann als Eigenschaften mit [**HttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/dn279617)- und [**HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631)-Objekten zugeordnet werden.

## Senden einer einfachen GET-Anforderung über HTTP

Wie weiter oben in diesem Artikel beschrieben wurde, können UWP-Apps mit dem [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)-Namespace GET-Anforderungen senden. Der folgende Codeausschnitt veranschaulicht, wie eine GET-Anforderung mithilfe der [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)- und der [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631)-Klasse an „http://www.contoso.com“ gesendet werden kann, um die Antwort der GET-Anforderung zu lesen.

```csharp
//Create an HTTP client object
Windows.Web.Http.HttpClient httpClient = new Windows.Web.Http.HttpClient();

//Add a user-agent header to the GET request. 
var headers = httpClient.DefaultRequestHeaders;

//The safe way to add a header value is to use the TryParseAdd method and verify the return value is true,
//especially if the header value is coming from user input.
string header = "ie";
if (!headers.UserAgent.TryParseAdd(header))
{
    throw new Exception("Invalid header value: " + header);
}

header = "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";
if (!headers.UserAgent.TryParseAdd(header))
{
    throw new Exception("Invalid header value: " + header);
}

Uri requestUri = new Uri("http://www.contoso.com");

//Send the GET request asynchronously and retrieve the response as a string.
Windows.Web.Http.HttpResponseMessage httpResponse = new Windows.Web.Http.HttpResponseMessage();
string httpResponseBody = "";

try
{
    //Send the GET request
    httpResponse = await httpClient.GetAsync(requestUri);
    httpResponse.EnsureSuccessStatusCode();
    httpResponseBody = await httpResponse.Content.ReadAsStringAsync();
}
catch (Exception ex)
{
    httpResponseBody = "Error: " + ex.HResult.ToString("X") + " Message: " + ex.Message;
}
```

## Ausnahmen in „Windows.Web.Http“

Eine Ausnahme wird ausgelöst, wenn eine ungültige Zeichenfolge für einen Uniform Resource Identifier (URI) an den Konstruktor für das [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998)-Objekt übergeben wird.

**.NET: **Der Typ [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) wird in C# und VB als [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) angezeigt.

In C# und Visual Basic kann dieser Fehler vermieden werden, indem die [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx)-Klasse in .NET 4.5 und eine der [**System.Uri.TryCreat**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx)-Methoden zum Testen der von einem Benutzer erhaltenen Zeichenfolge verwendet wird, bevor der URI erstellt wird.

In C++ gibt es keine Methode zum Analysieren einer Zeichenfolge für einen URI. Wenn eine App vom Benutzer eine Eingabe für das [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998)-Element erhält, sollte sich der Konstruktor innerhalb eines try/catch-Blocks befinden. Wenn eine Ausnahme ausgelöst wird, kann die App den Benutzer benachrichtigen und einen neuen Hostnamen anfordern.

[
            **Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) bietet keine Funktion, die die Behandlung von Ausnahmen erleichtert. Eine App, die [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) und andere Klassen in diesem Namespace verwendet, muss daher den **HRESULT**-Wert verwenden.

In Apps in C# und VB.NET, die .NET Framework 4.5 verwenden, stellt [System.Exception](http://msdn.microsoft.com/library/system.exception.aspx) einen Fehler während der App-Ausführung dar, wenn eine Ausnahme auftritt. Die [System.Exception.HResult](http://msdn.microsoft.com/library/system.exception.hresult.aspx)-Eigenschaft gibt den **HRESULT**-Wert zurück, der der jeweiligen Ausnahme zugewiesen ist. Die [System.Exception.Message](http://msdn.microsoft.com/library/system.exception.message.aspx)-Eigenschaft gibt die Meldung zurück, die die Ausnahme beschreibt. Mögliche **HRESULT**-Werte sind in der Headerdatei *Winerror.h* aufgeführt. Eine App kann nach bestimmten **HRESULT**-Werten filtern, um das App-Verhalten je nach Ausnahmeursache zu ändern.

In Apps mit verwaltetem C++ stellt das [Platform::Exception](http://msdn.microsoft.com/library/windows/apps/hh755825.aspx)-Objekt einen Fehler während der App-Ausführung dar, wenn eine Ausnahme auftritt. Die [Platform::Exception::HResult](http://msdn.microsoft.com/library/windows/apps/hh763371.aspx)-Eigenschaft gibt den **HRESULT**-Wert zurück, der der jeweiligen Ausnahme zugewiesen ist. Die [Platform::Exception::Message](http://msdn.microsoft.com/library/windows/apps/hh763375.aspx)-Eigenschaft gibt die vom System bereitgestellte Zeichenfolge zurück, die dem **HRESULT**-Wert zugeordnet ist. Mögliche **HRESULT**-Werte sind in der Headerdatei *Winerror.h* aufgeführt. Eine App kann nach bestimmten **HRESULT**-Werten filtern, um das App-Verhalten je nach Ausnahmeursache zu ändern.

Für die meisten Parameterüberprüfungsfehler wird der **HRESULT**-Wert **E\_INVALIDARG** zurückgegeben. Bei manchen unzulässigen Methodenaufrufen wird der **HRESULT**-Wert **E\_ILLEGAL\_METHOD\_CALL** zurückgegeben.



<!--HONumber=Mar16_HO1-->


