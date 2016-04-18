---
ms.assetid: 79C284CA-C53A-4C24-807E-6D4CE1A29BFA
description: In diesem Abschnitt wird beschrieben, wie Sie Ihre Web-App mit PlayReady ändern, um die Änderungen zu unterstützen, die gegenüber der Windows 8.1-Version in der Version für Windows 10 vorgenommen wurden.
title: Verschlüsselte Medienerweiterung von PlayReady
---

# Verschlüsselte Medienerweiterung von PlayReady

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


In diesem Abschnitt wird beschrieben, wie Sie Ihre Web-App mit PlayReady ändern, um die Änderungen zu unterstützen, die gegenüber der Windows 8.1-Version in der Version für Windows 10 vorgenommen wurden.

Die Verwendung von PlayReady-Medienelementen in Internet Explorer ermöglicht Entwicklern das Erstellen von Web-Apps, die PlayReady-geschützte Inhalte für den Benutzer bereitstellen und gleichzeitig vom Inhaltsanbieter definierte Regeln erzwingen können. In diesem Abschnitt wird beschrieben, wie Sie Ihren vorhandenen Web-Apps PlayReady-Medienelemente hinzufügen, indem Sie nur HTML5 und JavaScript verwenden.

## Neuigkeiten in der verschlüsselten Medienerweiterung von PlayReady

Dieser Abschnitt enthält eine Liste der Änderungen, die an der verschlüsselten Medienerweiterung von PlayReady vorgenommen wurden, um PlayReady-Inhaltsschutz unter Windows 10 bereitzustellen.

In der folgenden Liste werden die neuen Features und Änderungen der verschlüsselten Medienerweiterung von PlayReady unter Windows 10 beschrieben:

-   Hardwarebasierte Verwaltung digitaler Rechte (Digital Rights Management, DRM) wurde hinzugefügt.

    Die Unterstützung für hardwarebasierten Inhaltsschutz ermöglicht die sichere Wiedergabe von High-Definition (HD)- und Ultra-High-Definition (UHD)-Inhalten auf mehreren Geräteplattformen. Schlüsselmaterial (einschließlich privater Schlüssel, Inhaltsschlüssel und anderer Schlüsselmaterialien zum Ableiten oder Entsperren dieser Schlüssel) sowie entschlüsselte komprimierte und nicht komprimierte Videobeispiele werden durch Hardwaresicherheit geschützt.

-   Unterstützt das proaktive Abrufen nicht persistenter Lizenzen.
-   Unterstützt das Abrufen mehrerer Lizenzen in einer Nachricht.

    Sie können entweder wie in Windows 8.1 ein PlayReady-Objekt mit mehreren Schlüsselkennungen (KeyIDs) oder [Content Decryption Model (CDM)-Daten (CDMData)](https://go.microsoft.com/fwlink/p/?LinkID=626819) mit mehreren KeyIDs verwenden.

    **Hinweis**  In Windows 10 werden mehrere Schlüsselkennungen unter &lt;KeyID&gt; in CDMData unterstützt.

     

-   Unterstützung für den Echtzeitablauf, d. h. einer Lizenz mit begrenzter Dauer (Limited Duration License, LDL), wurde hinzugefügt.

    Bietet die Möglichkeit, den Echtzeitablauf für Lizenzen festzulegen.

-   Richtlinienunterstützung für HDCP Typ 1 (Version 2.2) wurde hinzugefügt.
-   Miracast ist jetzt als Ausgabe implizit.
-   Sicheres Beenden wurde hinzugefügt.

    Dank des sicheren Beendens kann ein PlayReady-Gerät einem Medienstreamingdienst zuverlässig bestätigen, dass die Medienwiedergabe für einen bestimmten Inhalt beendet wurde.

-   Audio- und Videolizenztrennung wurde hinzugefügt.

    Separate Spuren verhindern, dass Videos als Audio decodiert werden. Dadurch wird ein stabilerer Inhaltsschutz gewährleistet. Neue Standards erfordern separate Schlüssel für Audio- und Videospuren.

-   MaxResDecode-Feature hinzugefügt.

    Dieses Feature wurde hinzugefügt, um die Wiedergabe von Inhalten auf eine maximale Auflösung zu beschränken, auch wenn der Benutzer einen leistungsfähigeren Schlüssel (aber keine Lizenz) besitzt. Dadurch werden Szenarien unterstützt, in denen mehrere Datenstromgrößen mit einem einzelnen Schlüssel codiert werden.

## Unterstützung von verschlüsselten Medienerweiterungen in PlayReady

In diesem Abschnitt wird die Version der verschlüsselten Medienerweiterungen von W3C beschrieben, die von PlayReady unterstützt werden.

PlayReady für Web-Apps ist derzeit an die Entwurfsversion vom 10. Mai 2013 ([W3C Encrypted Media Extension (EME) draft of May 10, 2013](http://www.w3.org/TR/2013/WD-encrypted-media-20130510/)) gebunden. Für zukünftige Versionen von Windows wird diese Unterstützung in die aktualisierte EME-Spezifikation geändert.

## Verwenden des Hardware-DRM

In diesem Abschnitt wird beschrieben, wie Ihre Web-App das Hardware-DRM mit PlayReady verwenden kann und wie Sie das Hardware-DRM deaktivieren, wenn der geschützte Inhalt es nicht unterstützt.

Um das Hardware-DRM mit PlayReady zu verwenden, sollte Ihre JavaScript-Web-App die EME-Methode **isTypeSupported** mit einem `com.microsoft.playready.hardware`-Schlüsselsystembezeichner verwenden, um die Unterstützung des Hardware-DRM mit PlayReady vom Browser abzufragen.

Mitunter werden Inhalte jedoch nicht vom Hardware-DRM unterstützt. Cocktail-Inhalte werden nie vom Hardware-DRM unterstützt. Wenn Sie Cocktail-Inhalte wiedergeben möchten, müssen Sie das Hardware-DRM außer Kraft setzen. Einige Hardware-DRM-Typen unterstützen HEVC, andere dagegen nicht. Wenn Sie HEVC-Inhalte wiedergeben möchten und diese nicht vom Hardware-DRM unterstützt werden, sollten Sie das Hardware-DRM in diesem Fall ebenfalls außer Kraft setzen.

**Hinweis**  Um festzustellen, ob HEVC-Inhalte unterstützt werden, verwenden Sie nach der Instanziierung von `com.microsoft.playready` die [**PlayReadyStatics.CheckSupportedHardware**](https://msdn.microsoft.com/library/windows/apps/dn986441)-Methode.

 

## Hinzufügen des sicheren Beendens zur Web-App

In diesem Abschnitt wird beschrieben, wie Sie Ihrer Web-App die Funktion für sicheres Beenden hinzufügen.

Dank des sicheren Beendens kann ein PlayReady-Gerät einem Medienstreamingdienst zuverlässig bestätigen, dass die Medienwiedergabe für einen bestimmten Inhalt beendet wurde. Mit dieser Funktion wird sichergestellt, dass Ihre Medienstreamingdienste präzise Erzwingung und Berichte zu Nutzungseinschränkungen auf verschiedenen Geräten für ein bestimmtes Konto bereitstellen.

Es gibt zwei primäre Szenarien für das Senden einer Abfrage für sicheres Beenden:

-   Wenn die Mediendarstellung beendet wird, weil das Ende des Inhalts erreicht wurde, oder wenn die Mediendarstellung vor ihrem Ende vom Benutzer beendet wurde.
-   Wenn die vorherige Sitzung unerwartet beendet wurde (z. B. aufgrund eines System- oder App-Absturzes). Die App muss beim Starten oder Herunterfahren alle ausstehenden Sitzungen für sicheres Beenden abfragen und von anderen Medienwiedergaben getrennte Abfragen senden.

In den folgenden Verfahren wird beschrieben, wie Sie das sichere Beenden für verschiedene Szenarien einrichten.

So richten Sie das sichere Beenden für die normale Beendigung einer Mediendarstellung ein

1.  Registrieren Sie das **onEnded**-Ereignis vor dem Start der Wiedergabe.
2.  Der **onEnded**-Ereignishandler muss `removeAttribute(“src”)` über das Video-/Audioelementobjekt aufrufen, um die Quelle auf **NULL** festzulegen, damit Media Foundation die Topologie unterbricht, die Decryptors zerstört und den Beendigungszustand festlegt.
3.  Sie können die CDM-Sitzung für sicheres Beenden innerhalb des Handlers oder an späterer Stelle starten, um die Abfrage für sicheres Beenden an den Server zu senden und ihn darüber zu benachrichtigen, dass die Wiedergabe beendet wurde.

So richten Sie das sichere Beenden für den Fall ein, dass der Benutzer die Seite verlässt oder die Registerkarte oder den Browser schließt

-   Zum Aufzeichnen des Beendigungszustands ist keine App-Aktion erforderlich. Der Zustand wird automatisch aufgezeichnet.

So richten Sie das sichere Beenden für benutzerdefinierte Steuerelemente oder Benutzeraktionen ein (z. B. benutzerdefinierte Navigationsschaltflächen oder das Starten einer neuen Mediendarstellung vor Abschluss der aktuellen Mediendarstellung)

-   Wenn die benutzerdefinierte Aktion stattfindet, muss die App die Quelle auf **NULL** festlegen, damit Media Foundation die Topologie unterbricht, die Decryptors zerstört und den Beendigungszustand festlegt.

Das folgende Beispiel zeigt, wie Sie das sichere Beenden in Ihrer Web-App verwenden:

``` syntax
// JavaScript source code

var g_prkey = null;
var g_keySession = null;
var g_fUseSpecificSecureStopSessionID = false;
var g_encodedMeteringCert = 'Base64 encoded of your metering cert (aka publisher cert)';

// Note: g_encodedLASessionId is the CDM session ID of the proactive or reactive license acquisition 
//       that we want to initiate the secure stop process.
var g_encodedLASessionId = null;

function main()
{
    ...

    g_prkey = new MSMediaKeys("com.microsoft.playready");

    ...

    // add 'onended' event handler to the video element
    // Assume 'myvideo' is the ID of the video element
    var videoElement = document.getElementById("myvideo");
    videoElement.onended = function (e) { 

        //
        // Calling removeAttribute("src") will set the source to null
        // which will trigger the MF to tear down the topology, destroy the
        // decryptor(s) and set the stop state.  This is required in order
        // to set the stop state.
        //
        videoElement.removeAttribute("src");

        onEndOfStream();
    };
}

function onEndOfStream()
{
    ...

    createSecureStopCDMSession();

    ...    
}

function createSecureStopCDMSession()
{
    try{    
        var targetMediaCodec = "video/mp4";
        var customData = "my custom data";

        var encodedSessionId = g_encodedLASessionId;
        if( !g_fUseSpecificSecureStopSessionID )
        {
            // Use "*" (wildcard) as the session ID to include all secure stop sessions
            // TODO: base64 encode "*" and place encoded result to encodedSessionId
        }

        var int8ArrayCDMdata = formatSecureStopCDMData( encodedSessionId, customData,  g_encodedMeteringCert );
        var emptyArrayofInitData = new Uint8Array();

        g_keySession = g_prkey.createSession(targetMediaCodec, emptyArrayofInitData, int8ArrayCDMdata);

        addPlayreadyKeyEventHandler();

    } catch( e )
    {
        // TODO: Handle exception
    }
}

function addPlayreadyKeyEventHandler()
{
    // add 'keymessage' eventhandler   
    g_keySession.addEventListener('mskeymessage', function (event) {

        // TODO: Get the keyMessage from event.message.buffer which contains the secure stop challenge
        //       The keyMessage format for the secure stop is similar to LA as below:
        //
        //            <PlayReadyKeyMessage type="SecureStop" >
        //              <SecureStop version="1.0" >
        //                <Challenge encoding="base64encoded">
        //                    secure stop challenge
        //                </Challenge>
        //                <HttpHeaders>
        //                    <HttpHeader>
        //                      <name>Content-Type</name>
        //                         <value>"content type data"</value>
        //                    </HttpHeader>
        //                    <HttpHeader>
        //                         <name>SOAPAction</name>
        //                         <value>soap action</value>
        //                     </HttpHeader>
        //                    ....
        //                </HttpHeaders>
        //              </SecureStop>
        //            </PlayReadyKeyMessage>
                
        // TODO: send the secure stop challenge to a server that handles the secure stop challenge

        // TODO: Recevie and response and call event.target.Update() to proecess the response
    });
    
    // add 'keyerror' eventhandler
    g_keySession.addEventListener('mskeyerror', function (event) {
        var session = event.target;
        
        ...

        session.close();
    });
    
    // add 'keyadded' eventhandler
    g_keySession.addEventListener('mskeyadded', function (event) {
        
        var session = event.target;

        ...

        session.close();             
    });
}

/**
* desc@ formatSecureStopCDMData
*   generate playready CDMData
*   CDMData is in xml format:
*   <PlayReadyCDMData type="SecureStop">
*     <SecureStop version="1.0">
*       <SessionID>B64 encoded session ID</SessionID>
*       <CustomData>B64 encoded custom data</CustomData>
*       <ServerCert>B64 encoded server cert</ServerCert>
*     </SecureCert>
* </PlayReadyCDMData>        
*/
function formatSecureStopCDMData(encodedSessionId, customData, encodedPublisherCert) 
{
    var encodedCustomData = null;

    // TODO: base64 encode the custom data and place the encoded result to encodedCustomData

    var CDMDataStr = "<PlayReadyCDMData type=\"SecureStop\">" +
                     "<SecureStop version=\"1.0\" >" +
                     "<SessionID>" + encodedSessionId + "</SessionID>" +
                     "<CustomData>" + encodedCustomData + "</CustomData>" +
                     "<ServerCert>" + encodedPublisherCert + "</ServerCert>" +
                     "</SecureStop></PlayReadyCDMData>";
    
    var int8ArrayCDMdata = null

    // TODO: Convert CDMDataStr to Uint8 byte array and palce the converted result to int8ArrayCDMdata

    return int8ArrayCDMdata;
}
```

**Hinweis**  Für die `<SessionID>B64 encoded session ID</SessionID>` der Daten für sicheres Beenden im obigen Beispiel kann ein Sternchen (\*) angegeben werden. Das Sternchen ist ein Platzhalter für alle aufgezeichneten Sitzungen für sicheres Beenden. Beim **SessionID**-Tag kann es sich also um eine bestimmte Sitzung oder einen Platzhalter (\*) zur Auswahl aller Sitzungen für sicheres Beenden handeln.

## Überlegungen zur Programmierung für verschlüsselte Medienerweiterungen (Encrypted Media Extensions, EME)

Dieser Abschnitt enthält Hinweise zur Programmierung, die Sie beim Erstellen Ihrer PlayReady-fähigen Web-App für Windows 10 berücksichtigen sollten.

Die von Ihrer App erstellten Objekte **MSMediaKeys** und **MSMediaKeySession** müssen aktiv bleiben, bis die App geschlossen wird. Eine Möglichkeit, um sicherzustellen, dass diese Objekte aktiv bleiben, ist deren Zuweisung als globale Variablen (die Variablen würden den Gültigkeitsbereich verlassen und der Garbage Collection zugeführt, wenn sie innerhalb einer Funktion als lokale Variable deklariert werden). Im folgende Beispiel werden z. B. die Variablen *g\_msMediaKeys* und *g\_mediaKeySession* als globale Variablen zugewiesen, die dann den Objekten **MSMediaKeys** und **MSMediaKeySession** in der Funktion zugewiesen werden.

``` syntax
var g_msMediaKeys;
var g_mediaKeySession;

function foo() {
  ...
  g_msMediaKeys = new MSMediaKeys("com.microsoft.playready");
  ...
  g_mediaKeySession = g_msMediaKeys.createSession("video/mp4", intiData, null);
  g_mediaKeySession.addEventListener(this.KEYMESSAGE_EVENT, function (e) 
  {
    ...
    downloadPlayReadyKey(url, keyMessage, function (data) 
    {
      g_mediaKeySession.update(data);
    });
  });
  g_mediaKeySession.addEventListener(this.KEYADDED_EVENT, function () 
  {
    ...
    g_mediaKeySession.close();
    g_mediaKeySession = null;
  });
}
```

Weitere Informationen finden Sie in den [Beispielanwendungen](https://code.msdn.microsoft.com/windowsapps/PlayReady-samples-for-124a3738) .

 

 






<!--HONumber=Mar16_HO1-->


