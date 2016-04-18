---
title: Einführung in die Entwicklung sicherer Windows-Apps
description: In diesem einführenden Artikel erhalten App-Architekten und -Entwickler weitere Informationen zu den verschiedenen Windows 10-Plattformfunktionen, die die Entwicklung von UWP-Apps (Universelle Windows-Plattform) beschleunigen.
ms.assetid: 6AFF9D09-77C2-4811-BB1A-BBF4A6FF511E
author: awkoren
---

# Einführung in die Entwicklung sicherer Windows-Apps


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


In diesem einführenden Artikel erhalten App-Architekten und -Entwickler weitere Informationen zu den verschiedenen Windows 10-Plattformfunktionen, die die Entwicklung von UWP-Apps (Universelle Windows-Plattform) beschleunigen. Sie erfahren mehr über die Verwendung der verfügbaren Windows-Sicherheitsfeatures der Authentifizierungs-, In-Flight-Daten- und At-Rest-Daten-Stufe. In den in jedem Kapitel enthaltenen zusätzlichen Ressourcen finden Sie ausführlichere Informationen zu jedem Thema.

## 1 Einführung


Die Entwicklung einer sicheren App kann eine Herausforderung darstellen. In der heutigen schnelllebigen Welt von mobilen und sozialen Apps sowie von Cloud- und komplexen Unternehmens-Apps erwarten Kunden, dass Apps schneller als je zuvor verfügbar sind und aktualisiert werden. Zudem verwenden sie viele verschiedene Gerätetypen – eine weitere Herausforderung bei der Gestaltung der App-Oberflächen. Beim Entwickeln für die Universelle Windows-Plattform (UWP) von Windows 10 müssen Sie also neben traditionellen Geräten wie Desktops, Laptops, Tablets und mobilen Geräten auch immer mehr neue Technologien berücksichtigen. Diese reichen vom Internet der Dinge über Xbox One und Microsoft Surface Hub bis zu HoloLens. Als Entwickler müssen Sie die Sicherheit Ihrer Apps beim Kommunizieren und Speichern von Daten auf allen Plattformen oder Geräten gewährleisten.

Nachfolgend erhalten Sie einen Überblick über die Vorteile der Sicherheitsfeatures in Windows 10.

-   Sie profitieren von standardisierter Sicherheit auf allen Geräten, die Windows 10 unterstützen, indem Sie einheitliche APIs für Sicherheitskomponenten und Technologien verwenden.
-   Sie schreiben, testen und warten weniger Code als beim Implementieren von benutzerdefiniertem Code für diese Sicherheitsszenarien.
-   Ihre Apps werden stabiler und sicherer, da Sie über das Betriebssystem festlegen, wie die App auf ihre Ressourcen sowie auf lokale oder Remote-Systemressourcen zugreift.

Während der Authentifizierung wird die Identität des Benutzers überprüft, der den Zugriff auf einen bestimmten Dienst anfordert. Microsoft Passport und Windows Hello unterstützen Sie in Windows 10 beim Erstellen eines sichereren Authentifizierungsmechanismus in Windows-Apps. Mit ihnen können Sie eine persönliche Identifikationsnummer (PIN) oder biometrische Daten wie Fingerabdrücke, Gesichts- oder Iriserkennung für die mehrstufige Authentifizierung bei Ihren Apps verwenden.

In-Flight-Daten beziehen sich auf die Verbindung und die darüber übertragenen Nachrichten. Ein Beispiel hierfür ist das Abrufen von Daten von einem Remoteserver über Webdienste. Die Verwendung von SSL (Secure Sockets Layer) und HTTPS (Secure Hypertext Transfer Protocol) gewährleistet eine sichere Verbindung. Um In-Flight-Daten zu schützen, muss Zwischenbenutzern der Zugriff auf diese Nachrichten und nicht autorisierten Apps die Kommunikation mit Webdiensten verweigert werden.

Als At-Rest-Daten werden Daten bezeichnet, die sich im Speicher oder auf Speichermedien befinden. Windows 10 verfügt über ein App-Modell, das unbefugten Datenzugriff zwischen Apps verhindert und bietet Verschlüsselungs-APIs zum weiteren Sichern von Daten auf dem Gerät. Mit dem Schließfach für Anmeldeinformationen können Benutzeranmeldeinformationen sicher auf dem Gerät gespeichert werden. Das Betriebssystem verhindert, dass andere Apps Zugriff auf diese erhalten.

## 2 Authentifizierungsfaktoren


Um die Daten zu schützen, muss die Person, die den Zugriff anfordert, sich identifizieren und zum Zugriff auf die gewünschten Datenressourcen autorisiert sein. Der Vorgang, bei dem ein Benutzer identifiziert wird, wird als Authentifizierung bezeichnet. Der Vorgang, bei dem bestimmt wird, ob ein Benutzer berechtigt ist, auf eine Ressource zuzugreifen, wird als Autorisierung bezeichnet. Beide Vorgänge sind eng verwandt und für den Benutzer möglicherweise kaum zu unterscheiden. Der Vorgang kann relativ einfach oder komplex sein. Dies hängt von zahlreichen Faktoren ab, beispielsweise davon, ob die Daten auf einem Server gespeichert oder auf viele Systeme verteilt sind. Der Server, der die Authentifizierungs- und Autorisierungsdienste bereitstellt, wird als Identitätsanbieter bezeichnet.

Um sich bei einem bestimmten Dienst und/oder einer App zu authentifizieren, verwendet der Benutzer als Anmeldeinformationen eine Information, ein Gerät und/oder ein Merkmal. Diese Punkte werden als Authentifizierungsfaktoren bezeichnet.

-   **Benutzerinformation** ist in der Regel ein Kennwort, kann aber auch eine Geheimzahl (PIN) oder eine Kombination aus „geheimer“ Frage und Antwort sein.
-   **Benutzergerät** ist häufig ein Hardwarespeichergerät wie ein USB-Stick mit den eindeutigen Authentifizierungsdaten des Benutzers.
-   **Benutzermerkmal** umfasst häufig Fingerabdrücke, mittlerweile kommen jedoch vermehrt auch Faktoren wie Sprach-, Gesichts- oder Augenmerkmale und Verhaltensmuster des Benutzers zum Einsatz. Werden sie als Daten gespeichert, bezeichnet man diese als Biometrie.

Ein vom Benutzer erstelltes Kennwort ist naturgemäß ein Authentifizierungsfaktor, reicht aber häufig nicht aus. Jeder, der das Kennwort kennt, kann die Identität des Benutzers annehmen, dem das Kennwort gehört. Eine Smartcard bietet u. U. höhere Sicherheit, kann aber gestohlen, verloren oder verlegt werden. Ein System, das einen Benutzer anhand seines Fingerabdrucks oder eines Augenscans authentifizieren kann, bietet u. U. die höchste und komfortabelste Sicherheit, erfordert jedoch teure und spezialisierte Hardware (z. B. eine Intel RealSense-Kamera für die Gesichtserkennung), die möglicherweise nicht allen Benutzern zur Verfügung steht.

Das Entwerfen der von einem Computersystem verwendeten Authentifizierungsmethode ist ein komplexer und wichtiger Aspekt der Datensicherheit. Im Allgemeinen gilt: Je mehr Faktoren bei der Authentifizierung zum Einsatz kommen, umso sicherer ist das System. Zur gleichen Zeit muss die Authentifizierung praktikabel sein. Da sich ein Benutzer normalerweise mehrmals täglich anmeldet, muss das Verfahren schnell sein. Bei der Wahl des Authentifizierungstyps muss zwischen Sicherheit und einfacher Bedienung abgewogen werden. Die Single-Factor Authentication ist am unsichersten und einfachsten, während die Multi-Factor Authentication immer sicherer, auch komplexer wird, je mehr Faktoren hinzukommen.

## 2.1 Single-Factor Authentication


Diese Art der Authentifizierung basiert auf einer einzelnen Benutzeranmeldeinformation. In der Regel ist dies ein Kennwort, es kann aber auch eine PIN verwendet werden.

Die Single-Factor Authentication läuft wie folgt ab:

-   Der Benutzer stellt seinen Benutzernamen und sein Kennwort dem Identitätsanbieter zur Verfügung. Der Identitätsanbieter ist der Serverprozess, der die Identität des Benutzers überprüft.
-   Der Identitätsanbieter überprüft, ob Benutzername und Kennwort mit den im System gespeicherten Angaben übereinstimmen. In den meisten Fällen wird das Kennwort verschlüsselt. Das bietet zusätzliche Sicherheit, da andere Personen es nicht lesen können.
-   Der Identitätsanbieter gibt einen Authentifizierungsstatus zurück, der angibt, ob die Authentifizierung erfolgreich war.
-   Wenn der Vorgang erfolgreich war, beginnt der Datenaustausch. Wenn er nicht erfolgreich war, muss der Benutzer erneut authentifiziert werden.

![Single-Factor Authentication](images/secure-sfa.png)

Dies ist heute die gebräuchlichste, dienstunabhängig verwendete Authentifizierungsmethode. Wenn sie als einzige Authentifizierungsmethode verwendet wird, bietet sie allerdings auch die geringste Sicherheit. Kennwortkomplexität, „geheime Fragen“ und regelmäßige Kennwortänderungen können die Sicherheit von Kennwörtern verbessern, verursachen aber einen Mehraufwand für Benutzer und sind kein wirksames Abschreckungsmittel für Hacker.

Das Problem bei Kennwörtern ist, dass sie einfacher erraten werden können als Systeme, die mehrere Faktoren verwenden. Wenn Hacker eine Datenbank mit Benutzerkonten und Kennworthash aus einem kleinen Onlineshop stehlen, können sie die Kennwörter auf anderen Websites verwenden. Benutzer tendieren dazu, Konten wiederverwenden, da komplexe Kennwörter schwierig zu merken sind. Die Verwaltung von Kennwörtern stellt die IT-Abteilung vor die Herausforderung, Zurücksetzungsmechanismen bieten zu müssen, häufige Änderungen der Kennwörter zu verlangen und sicher zu speichern.

Die Single-Factor Authentication hat zwar einige Nachteile, überlässt dem Benutzer jedoch die Kontrolle über seine Anmeldedaten. Der Benutzer erstellt und ändert die Anmeldeinformationen, und zur Authentifizierung wird lediglich eine Tastatur benötigt. Dies ist der Hauptaspekt, der die Single-Factor Authentication von der Multi-Factor Authentication unterscheidet.

## 2.1.1 Webauthentifizierungsbroker


Wie bereits erwähnt liegt eine der Herausforderungen in der Kennwortauthentifizierung für IT-Abteilung in der Verwaltung der Benutzernamen/Kennwörter, Zurücksetzungsmechanismen usw. Eine weitere, immer beliebtere Option der Einsatz von dritten Identitätsanbietern. Diese bieten die Authentifizierung über OAuth an, einen offenen Authentifizierungsstandard.

Mit OAuth können IT-Abteilungen die Komplexität der Verwaltung einer Datenbank mit Benutzernamen und Kennwörtern, das Zurücksetzen von Kennwörtern usw. einem dritten Identitätsanbieter wie Facebook, Twitter oder Microsoft übertragen.

Benutzer haben auf diesen Plattformen die vollständige Kontrolle über ihre Identität. Apps können jedoch nach der Authentifizierung des Benutzers mit dessen Einverständnis ein Token vom Anbieter anfordern, welches zur Autorisierung von authentifizierten Benutzern verwendet werden kann.

Der Webauthentifizierungsbroker in Windows 10 bietet mehrere APIs und eine Infrastruktur für Apps zum Verwenden von Authentifizierungs- und Autorisierungsprotokollen wie OAuth und OpenID. Apps können Authentifizierungsvorgänge über die [**WebAuthenticationBroker**](https://msdn.microsoft.com/library/windows/apps/br227025)-API initiieren, wodurch [**WebAuthenticationResult**](https://msdn.microsoft.com/library/windows/apps/br227038) zurückgegeben wird. In der folgenden Abbildung erhalten Sie einen Überblick über den Kommunikationsfluss.

![WAB-Workflow](images/secure-wab.png)

Die App fungiert als Vermittler und initiiert die Authentifizierung gegenüber dem Identitätsanbieter über [**WebView**](https://msdn.microsoft.com/library/windows/apps/br227702) in der App. Nachdem der Benutzer vom Identitätsanbieter authentifiziert wurde, gibt er ein Token an die App zurück, mit dem vom Identitätsanbieter Informationen zum Benutzer angefordert werden können. Aus Sicherheitsgründen muss die App beim Identitätsanbieter registriert werden, bevor sie als Authentifizierungsprozessbroker für den Identitätsanbieter fungieren kann. Diese Registrierungsschritte unterscheiden sich je nach Anbieter.

Nachfolgend finden Sie den allgemeinen Workflow beim Aufrufen der [**WebAuthenticationBroker**](https://msdn.microsoft.com/library/windows/apps/br227025)-API für die Kommunikation mit dem Anbieter.

-   Erstellen Sie die Anforderungszeichenfolgen, die an den Identitätsanbieter gesendet werden sollen. Die Anzahl der Zeichenfolgen und die Informationen in den einzelnen Zeichenfolgen sind bei jedem Webdienst anders. Normalerweise enthalten sie aber je zwei URI-Anforderungszeichenfolgen mit einer URL – eine, an die die Authentifizierungsanforderung gesendet wird und eine, an die der Benutzer nach Abschluss der Autorisierung weitergeleitet wird.
-   Rufen Sie [**WebAuthenticationBroker.AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066) auf, indem Sie die Anforderungszeichenfolgen übergeben, und warten Sie auf die Antwort des Identitätsanbieters.
-   Rufen Sie [**WebAuthenticationResult.ResponseStatus**](https://msdn.microsoft.com/library/windows/apps/br227041) auf, um beim Empfang der Antwort den Status zu erhalten.
-   Wenn die Kommunikation erfolgreich verläuft: Verarbeiten der vom Identitätsanbieter zurückgegebenen Antwortzeichenfolge. Falls sie nicht erfolgreich ist: Verarbeiten des Fehlers.

Wenn die Kommunikation erfolgreich verläuft: Verarbeiten der vom Identitätsanbieter zurückgegebenen Antwortzeichenfolge. Falls sie nicht erfolgreich ist: Verarbeiten des Fehlers.

C#-Beispielcode für dieses Verfahren finden Sie unten. Weitere Informationen und eine ausführliche exemplarische Vorgehensweise finden Sie unter [WebAuthenticationBroker](web-authentication-broker.md). Ein vollständiges Codebeispiel finden Sie im [WebAuthenticationBroker-Beispiel auf GitHub](http://go.microsoft.com/fwlink/p/?LinkId=620622).

```cs
string startURL = "https://<providerendpoint>?client_id=<clientid>;
string endURL = "http://<appendpoint>";

var startURI = new System.Uri(startURL);
var endURI = new System.Uri(endURL);

try
{
    WebAuthenticationResult webAuthenticationResult = 
        await WebAuthenticationBroker.AuthenticateAsync( 
            WebAuthenticationOptions.None, startURI, endURI);

    switch (webAuthenticationResult.ResponseStatus)
    {
        case WebAuthenticationStatus.Success:
            // Successful authentication. 
            break;
        case WebAuthenticationStatus.ErrorHttp:
            // HTTP error. 
            break;
        default:
            // Other error.
        break;
    }
}
catch (Exception ex)
{
    // Authentication failed. Handle parameter, SSL/TLS, and
    // Network Unavailable errors here. 
}
```

## 2.2 Multi-Factor Authentication


Die Multi-Factor Authentication nutzt mehr als einen Authentifizierungsfaktor. In der Regel wird „etwas, das Sie kennen“, z. B. ein Kennwort, mit „etwas, das Sie haben“ kombiniert. Das kann z. B. ein Mobiltelefon oder eine Smartcard sein. Selbst wenn ein Angreifer das Kennwort des Benutzers ausspioniert, kann er ohne das Gerät oder den Code trotzdem nicht auf das Konto zugreifen. Wird nur das Gerät oder die Karte manipuliert, sind diese ohne das Kennwort für den Angreifer nutzlos. Daher ist die Multi-Factor Authentication sicherer, aber auch komplexer als die Single-Factor Authentication.

Dienste, die die Multi-Factor Authentication nutzen, lassen dem Benutzer häufig die Wahl, wie die zweite Anmeldeinformation übermittelt werden soll. Ein Beispiel für diesen Authentifizierungstyp ist ein häufig verwendetes Verfahren, bei dem ein Überprüfungscode per SMS an das Mobiltelefon des Benutzers gesendet wird.

-   Der Benutzer stellt seinen Benutzernamen und sein Kennwort dem Identitätsanbieter zur Verfügung.
-   Der Identitätsanbieter überprüft den Benutzernamen und das Kennwort wie bei der Single-Factor Authorization und ermittelt dann die im System gespeicherte Mobiltelefonnummer des Benutzers.
-   Der Server sendet eine SMS mit einem generierten Überprüfungscode an das Mobiltelefon des Benutzers.
-   Der Benutzer übergibt den Überprüfungscode mithilfe eines für den Benutzer bereitgestellten Formulars an den Identitätsanbieter.
-   Der Identitätsanbieter gibt einen Authentifizierungsstatus zurück, der angibt, ob die Authentifizierung beider Anmeldeinformationen erfolgreich war.
-   Wenn der Vorgang erfolgreich war, beginnt der Datenaustausch. Andernfalls muss der Benutzer erneut authentifiziert werden.

![Two-Factor Authentication](images/secure-tfa.png)

Wie Sie sehen, unterscheidet sich dieser Vorgang auch von der Single-Factor Authentication, da die zweite Benutzeranmeldeinformation an den Benutzer gesendet wird und nicht vom Benutzer erstellt bzw. angegeben wird. Daher hat der Benutzer nicht die vollständige Kontrolle über die erforderlichen Anmeldeinformationen. Dies gilt auch, wenn eine Smartcard für die zweite Anmeldeinformation verwendet wird: Die Organisation ist dafür zuständig, die Smartcard zu erstellen und an den Benutzer zu übergeben.

## 2.2.1 Azure Active Directory


Azure Active Directory (Azure AD) ist ein cloudbasierter Identitäts- und Zugriffsverwaltungsdienst, der als Identitätsanbieter in der Single-Factor Authentication oder Multi-Factor Authentication verwendet werden kann. Die Azure AD-Authentifizierung kann mit oder ohne Überprüfungscode verwendet werden.

In Azure AD kann auch die Single-Factor Authentication implementiert werden. Unternehmen benötigen jedoch die höhere Sicherheit der Multi-Factor Authentication. Bei der Konfiguration der Multi-Factor Authentification kann ein Benutzer, der sich bei einem Azure AD-Konto authentifiziert, einen Überprüfungscode entweder als SMS an sein Mobiltelefon oder an die Azure Authenticator-App auf seinem Mobiltelefon senden lassen.

Darüber hinaus kann Azure AD als OAuth-Anbieter verwendet werden und Standardbenutzer erhalten einen Authentifizierungs- und Autorisierungsmechanismus für Apps auf verschiedenen Plattformen. Weitere Informationen hierzu finden Sie unter [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) und [Multi-Factor Authentication von Azure](https://azure.microsoft.com/services/multi-factor-authentication/).

## 2.4 Microsoft Passport und Windows Hello


In Windows 10 ist ein praktischer mehrstufiger Authentifizierungsmechanismus in das Betriebssystem integriert. Die beiden beteiligten Komponenten heißen Microsoft Passport und Windows Hello. Windows Hello ist das neue biometrische Anmeldesystem in Windows 10. Da es direkt im Betriebssystem integriert ist, ermöglicht Windows Hello die Gesichts- oder Fingerabdruckidentifikation zum Entsperren von Benutzergeräten. Der sichere Windows-Anmeldeinformationsspeicher schützt die biometrischen Daten auf dem Gerät.

Windows Hello bietet Geräten eine zuverlässige Möglichkeit, einzelne Benutzer zu erkennen. Dies betrifft den ersten Teil des Wegs zwischen einem Benutzer und einem angeforderten Dienst- oder Datenelement. Nachdem das Gerät den Benutzer erkannt hat, muss es den Benutzer jedoch erst noch authentifizieren, bevor es entscheidet, ob er auf die angeforderte Ressource zugreifen darf. Microsoft Passport bietet die sichere 2FA (Two-Factor Authentication), die vollständig in Windows integriert ist, und ersetzt wiederverwendbare Kennwörter durch eine Kombination aus einem bestimmten Gerät und einer biometrischen Geste oder einer PIN. Die PIN wird vom Benutzer im Rahmen seiner Microsoft Passport-Registrierung angegeben.

Bei Microsoft Passport handelt es sich jedoch nicht um einen bloßen Ersatz der herkömmlichen 2FA-Systeme. Konzeptionell gesehen ähnelt es Smartcards. Die Authentifizierung wird mithilfe von kryptografischen Primitiven ausgeführt, anstelle Zeichenfolgen zu vergleichen. Zudem sind die Schlüssel des Benutzers in der vor Manipulationen geschützten Hardware sicher. Für Microsoft Passport sind keine zusätzlichen Infrastrukturkomponenten für die Smartcard-Bereitstellung erforderlich. Insbesondere benötigen Sie keine Public Key-Infrastruktur (PKI) zum Verwalten von Zertifikaten, wenn Sie derzeit keine haben. Microsoft Passport kombiniert die wichtigsten Vorteile von Smartcards – Flexibilität bei der Bereitstellung virtueller Smartcards und zuverlässige Sicherheit für physische Smartcards – und birgt keinen ihrer Nachteile.

Ein Gerät muss bei Microsoft Passport registriert werden, bevor sich Benutzer damit authentifizieren können. Microsoft Passport verwendet eine asymmetrische Verschlüsselung (öffentliche/private Schlüssel). Hier verwendet eine Partei einen öffentlichen Schlüssel zum Verschlüsseln von Daten, die die andere Partei mithilfe eines privaten Schlüssels entschlüsseln kann. Im Fall von Microsoft Passport werden mehrere Paare aus öffentlichen und privaten Schlüsseln erstellt und die privaten Schlüssel in den TPM-Chip (Trusted Platform Module) des Geräts geschrieben. Nachdem ein Gerät registriert wurde, können UWP-Apps System-APIs aufrufen, um den öffentlichen Schlüssel des Benutzers abzurufen, der zum Registrieren des Benutzers auf dem Server verwendet werden kann.

Der Registrierungsworkflow einer App könnte wie folgt aussehen:

![Microsoft Passport-Registrierung](images/secure-passport.png)

Die von Ihnen erfassten Registrierungsinformationen umfassen möglicherweise viel mehr Identifikationsinformationen als in diesem einfachen Szenario. Wenn Ihre App auf einen gesicherten Dienst – z. B. Onlinebanking – zugreift, müssen Sie beim Anmeldevorgang einen Identitätsnachweis und andere Dinge anfordern. Nachdem alle Bedingungen erfüllt wurden, wird der öffentliche Schlüssel dieses Benutzers im Back-End gespeichert und für Überprüfungszwecke verwendet, wenn der Benutzer den Dienst das nächste Mal verwendet.

Weitere Informationen zu Microsoft Passport und Windows Hello finden Sie in der [Microsoft Passport-Anleitung](https://msdn.microsoft.com/library/mt589441) und im [Microsoft Passport-Entwicklerhandbuch](microsoft-passport.md).

## 3 Sicherheitsmethoden für In-Flight-Daten


Sicherheitsmethoden für In-Flight-Daten beziehen sich auf Daten, die zwischen den mit einem Netzwerk verbundenen Geräten übertragen werden. Die Daten können zwischen Systemen in der hochsicheren Umgebung eines privaten Unternehmensintranets oder zwischen einem Client und Webdienst in der nicht sicheren Umgebung des Webs übertragen werden. Windows 10-Apps unterstützen Standards wie SSL über ihre Netzwerk-APIs und arbeiten mit Technologien wie Azure API Management, mit der Entwickler die geeignete Sicherheitsstufe für ihre Apps gewährleisten können.

## 3.1 Remotesystemauthentifizierung


Es gibt zwei allgemeine Szenarien, in denen die Kommunikation über ein Remotecomputersystem erfolgt.

-   Ein lokaler Server authentifiziert einen Benutzer über eine direkte Verbindung. Beispielsweise, wenn sich der Server und Client in einem Unternehmensintranet befinden.
-   Mit einem Webdienst wird über das Internet kommuniziert.

Für die Kommunikation zwischen Webdiensten gelten höhere Sicherheitsanforderungen als in Szenarien mit direkten Verbindungen, da Daten nicht mehr nur ein Teil eines sicheren, vertrauenswürdigen Netzwerks sind. Zudem besteht ein höheres Risiko, dass Angreifer versuchen, Daten abzufangen. Da verschiedene Gerätetypen auf den Dienst zugreifen werden, werden sie vermutlich als RESTful-Dienste erstellt und nicht als WCF. Die Authentifizierung und Autorisierung für den Dienst stellen Sie daher ebenfalls vor neue Herausforderungen. Wir werden zwei Anforderungen an die sichere Kommunikation mit Remotesystemen untersuchen.

Die erste Anforderung ist Nachrichtenvertraulichkeit: Die zwischen dem Client und den Webdiensten übertragenen Informationen (z. B. die Identität des Benutzers und andere persönliche Informationen) dürfen während der Übertragung nicht von Dritten gelesen werden. Dies wird normalerweise verhindert, indem die Verbindung, über die Nachrichten gesendet werden, und die Nachricht selbst verschlüsselt werden. Bei der Verschlüsselung mit privatem/öffentlichem Schlüssel ist der öffentliche Schlüssel für jeden verfügbar und dient zum Verschlüsseln von Nachrichten, die an einen bestimmten Empfänger gesendet werden sollen. Der private Schlüssel ist nur dem Empfänger bekannt und wird zum Entschlüsseln der Nachricht verwendet.

Die zweite Anforderung ist die Nachrichtenintegrität: Der Client und der Webdienst müssen überprüfen können, ob die empfangenen Nachrichten diejenigen sind, die die Gegenseite senden wollte, und ob die Nachricht während der Übertragung nicht manipuliert wurde. Dazu werden Nachrichten mit digitalen Signaturen signiert und die Zertifikatauthentifizierung verwendet.

## 3.2 SSL-Verbindungen


Webdienste können das von HTTPS (Secure Hypertext Transfer Protocol) unterstützte SSL (Secure Sockets Layer) verwenden, um sichere Verbindungen mit Clients herzustellen und zu verwalten. SSL gewährleistet Nachrichtenvertraulichkeit und -integrität durch die Unterstützung der Verschlüsselung mit öffentlichem Schlüssel sowie von Serverzertifikaten. SSL wird von Transport Layer Security (TLS) abgelöst, aber TLS wird häufig als SSL bezeichnet.

Wenn ein Client Zugriff auf eine Ressource auf einem Server anfordert, startet SSL einen Aushandlungsprozess mit dem Server. Dies wird als SSL-Handshake bezeichnet. Als Grundlage für die gesamte Kommunikation für die Dauer der SSL-Verbindung werden eine Verschlüsselungsstufe, eine Reihe öffentlicher und privater Verschlüsselungsschlüssel und die Identitätsinformationen im Client- und Serverzertifikat vereinbart. Der Server kann zu diesem Zeitpunkt außerdem verlangen, dass der Client authentifiziert wird. Sobald die Verbindung hergestellt ist, werden alle Nachrichten mit dem ausgehandelten öffentlichen Schlüssel verschlüsselt, bis die Verbindung geschlossen wird.

## 3.2.1 SSL-Pinning


Während SSL mithilfe von Verschlüsselung und Zertifikaten für Nachrichtenvertraulichkeit sorgt, kann mit dieser Technologie nicht sichergestellt werden, dass der Server, mit dem kommuniziert wird, auch der richtige Server ist. Das Serververhalten kann durch einen nicht autorisierten Dritten nachgeahmt werden, um sensible, vom Client übertragene Daten abzufangen. Dies kann mit einer als „SSL-Pinning“ bezeichneten Technik verhindert werden, die sicherstellt, dass das Zertifikat auf dem Server dem Zertifikat entspricht, das der Client erwartet und als vertrauenswürdig anerkennt.

SSL-Pinning kann in Apps auf verschiedene Weisen implementiert werden, die jeweils eigene Vor- und Nachteile bieten. Die einfachste Ansatz besteht in der Verwendung der Zertifikatdeklaration im App-Paketmanifest. Diese Deklaration ermöglicht es dem App-Paket, digitale Zertifikate zu installieren und eine exklusive Vertrauensstellung dafür festzulegen. Dies führt dazu, dass SSL-Verbindungen zwischen der App und den Servern nur mit den entsprechenden Zertifikaten in ihrer Zertifikatkette zulässig sind. Dieser Mechanismus ermöglicht auch die sichere Verwendung selbstsignierter Zertifikate, da keine Abhängigkeit von Drittanbietern für vertrauenswürdige öffentliche Zertifizierungsstellen besteht.

![SSL-Manifest](images/secure-ssl-manifest.png)

Für eine bessere Kontrolle über die Validierungslogik überprüfen APIs die vom Server als Antwort auf eine HTTPS-Anforderung zurückgegebenen Zertifikate. Beachten Sie, dass für diese Methode eine Anforderung gesendet und die Antwort überprüft werden muss. Daher muss diese Überprüfung durchgeführt werden, bevor sensible Daten in einer Anforderung tatsächlich gesendet werden.

Der folgende C#-Code veranschaulicht diese Methode des SSL-Pinnings. Die Methode **ValidateSSLRoot** verwendet die Klasse [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) zum Ausführen einer HTTP-Anforderung. Nach dem Senden der Antwort verwendet der Client die Collection [**RequestMessage.TransportInformation.ServerIntermediateCertificates**](https://msdn.microsoft.com/library/windows/apps/dn279681) zum Prüfen der vom Server zurückgegebenen Zertifikate. Der Client kann dann die gesamte Zertifikatkette mit den enthaltenen Fingerabdrücken prüfen. Für diese Methode müssen die Fingerabdrücke der Zertifikate in der App aktualisiert werden, wenn das Serverzertifikat abläuft und erneuert wird.

```cs
private async Task ValidateSSLRoot()
{
    // Send a get request to Bing
    var httpClient = new HttpClient();
    var bingUri = new Uri("https://www.bing.com");
    HttpResponseMessage response = 
        await httpClient.GetAsync(bingUri);

    // Get the list of certificates that were used to
    // validate the server&#39;s identity
    IReadOnlyList<Certificate> serverCertificates = response.RequestMessage.TransportInformation.ServerIntermediateCertificates;
  
    // Perform validation
    if (!ValidateCertificates(serverCertificates))
    {
        // Close connection as chain is not valid
        return;
    }
    // Validation passed, continue with connection to service
}

private bool ValidateCertificates(IReadOnlyList<Certificate> certs)
{
    // In this example, we iterate through the certificates
    // and check that the chain contains
    // one specific certificate we are expecting
    foreach (var cert in certs)
    {
        byte[] thumbprint = cert.GetHashValue();

        // Check if the thumbprint matches whatever you 
        // are expecting
        var expected = new byte[] { 212, 222, 32, 208, 94, 102, 
            252, 83, 254, 26, 80, 136, 44, 120, 219, 40, 82, 202, 
            228, 116 };

        // ThumbprintMatches does the byte[] comparison 
        if (ThumbprintMatches(thumbprint, expected))
        {
            return true;
        }
    }
    return false;
}
```

## 3.3 Veröffentlichen und Sichern des Zugriffs auf REST-APIs


Um autorisierten Zugriff auf Webdienste zu gewährleisten, müssen sie bei jedem API-Aufruf eine Authentifizierung anfordern. Die Steuerung der Leistung und Skalierung ist eine weitere Möglichkeit, wenn Webdienste über das Web verfügbar gemacht werden. Mit Azure API Management können Sie APIs über das Web verfügbar machen, wenn Sie Funktionen auf drei Ebenen bereitstellen.

**Herausgeber/Administratoren** der API können die API einfach über das Herausgeberportal von Azure API Management konfigurieren. Hier können API-Sätze erstellt werden, und der Zugriff darauf kann verwaltet werden. So können Sie steuern, wer Zugriff auf welche APIs hat.

**Entwickler**, die auf diese APIs zugreifen möchten, können Anfragen über das Entwicklerportal stellen. Sie können sofort Zugriff gewähren oder eine Genehmigung durch den Herausgeber/Administrator vorschreiben. Entwickler können auch die API-Dokumentation und Beispielcode im Entwicklerportal anzeigen, um schnell die vom Webdienst angebotenen APIs anzuwenden.

Die von diesen Entwicklern erstellten **apps** können dann über den von Azure API Management angebotenen Proxy auf die API zugreifen. Der Proxy ermöglicht das Ausblenden des tatsächlichen Endpunkts der API auf dem Server des Herausgebers/Administrators und kann zudem zusätzliche Logik wie die API-Übersetzung ermöglichen, um sicherzustellen, dass die verfügbar gemachte API einheitlich ist, wenn eine API an eine andere weitergeleitet wird. Der Dienst kann mithilfe der IP-Filterung auch API-Aufrufe von einer bestimmten oder mehreren IP-Domänen blockieren. Zum Schutz seiner Webdienste verwendet Azure API Management eine Reihe öffentlicher Schlüssel, die sogenannten API-Schlüssel, um jeden API-Aufruf zu authentifizieren und zu autorisieren. Wenn die Autorisierung einen Fehler verursacht, wird der Zugriff auf die API und die unterstützten Funktionen blockiert.

Azure API Management kann auch die Anzahl der API-Aufrufe eines Diensts (durch die sogenannte Drosselung) reduzieren, um die Leistung des Webdiensts zu optimieren. Weitere Informationen finden Sie unter [Azure API Management](https://azure.microsoft.com/services/api-management/) und [Azure API Management at AzureCon 2015](https://channel9.msdn.com/events/Microsoft-Azure/AzureCon-2015/ACON313) (in englischer Sprache).

## 4 Sicherheitsmethoden für At-Rest-Daten


Daten die von einem Gerät empfangen werden, bezeichnen wir als „At-Rest-Daten“. Diese Daten müssen auf sichere Weise auf dem Gerät gespeichert werden, um den Zugriff durch nicht autorisierte Benutzer oder Apps zu verhindern. Das App-Modell in Windows 10 stellt auf vielfältige Weise sicher, dass die von einer App gespeicherten Daten auch nur für diese App verfügbar sind. Gleichzeitig bietet es APIs, über die die Daten bei Bedarf gemeinsam genutzt werden können. Darüber hinaus sind APIs verfügbar, mit denen sichergestellt wird, dass Daten verschlüsselt und Anmeldeinformationen sicher gespeichert werden können.

## 4.1 Windows-App-Modell


Bisher war in Windows keine App-Definition vorgesehen. Meist wurde sie als ausführbare Datei (.exe) bezeichnet und umfasste niemals Faktoren wie Installation, Zustandsspeicherung, Ausführungslänge, Versionsverwaltung, Betriebssystemintegration oder App-zu-App-Kommunikation. Das UWP-Modell verfügt über eine App-Modelldefinition, in der Installation, Laufzeitumgebung, Ressourcenverwaltung, Updates, Datenmodell und Deinstallation berücksichtigt werden.

Windows 10-Apps werden in einem Container ausgeführt. Das bedeutet, dass sie standardmäßig eingeschränkte Berechtigungen haben. (Zusätzliche Berechtigungen können angefordert und vom Benutzer genehmigt werden.) Möchte eine App beispielsweise auf Dateien auf dem System zugreifen, muss eine Dateiauswahl aus dem Namespace [**Windows.Storage.Pickers**](https://msdn.microsoft.com/library/windows/apps/br207928) verwendet werden, damit der Benutzer eine Datei auswählen kann. (Es ist kein direkter Zugriff auf Dateien möglich.) Ein weiteres Beispiel: Wenn eine App auf Positionsdaten des Benutzers zugreifen möchte, müssen die Standortdienste des Geräts aktiviert werden. Der Benutzer wird beim Download informiert, dass diese App Zugriff auf die Position des Benutzers benötigt. Wenn die App erstmals auf den Standort des Benutzers zugreifen möchte, wird abermals die Zustimmung des Benutzers zum Zugriff auf diese Daten angefordert.

Beachten Sie, dass dieses App-Modell als „Gefängnis“ für Apps fungiert, diese also über keine Reichweite verfügen. Jedoch ist es keine „Burg“, die nicht von außen erreicht werden kann (Anwendungen mit Administratorrechten können die Apps noch erreichen). Mit Device Guard in Windows 10 können Unternehmen/IT festlegen, welche (Win32-) Apps ausgeführt werden dürfen und diesen Zugriff weiter einschränken.

Das App-Modell verwaltet auch den App-Lebenszyklus. Es beschränkt die Hintergrundausführung von Apps standardmäßig, Wird beispielsweise eine App in den Hintergrund verschoben, wird der Prozess angehalten (nachdem der App ein kurzer Zeitraum gewährt wird, um die App im Code anzuhalten), und der Speicher wird eingefroren. Das Betriebssystem bietet Mechanismen für Apps, um die Ausführung bestimmter Hintergrundaufgaben anzufordern (nach Zeitplan, durch verschiedene Ereignisse wie Internet-/Bluetooth-Verbindung ausgelöst, Änderung der Stromversorgung usw. und in bestimmten Szenarien wie der Wiedergabe von Musik oder GPS-Tracking).

Wenn Speicherressourcen auf dem Gerät knapp werden, gibt Windows Speicher durch die Beendigung von Apps frei. Dieses Lebenszyklusmodell zwingt Apps, Daten beizubehalten, wenn sie angehalten werden, da zwischen dem Anhalten und Beenden keine Karenzzeit verfügbar ist.

Weitere Informationen finden Sie unter [It's Universal: Understanding the Lifecycle of a Windows 10 Application](https://visualstudiomagazine.com/articles/2015/09/01/its-universal.aspx) (in englischer Sprache).

## 4.2 Schutz gespeicherter Anmeldeinformationen


Windows-Apps, die häufig auf authentifizierte Dienste zugreifen, bieten Benutzern die Möglichkeit, ihre Anmeldedaten auf dem lokalen Gerät zu speichern. Diese Option erhöht die Benutzerfreundlichkeit. Wenn der Benutzer Benutzernamen und Kennwort angibt, werden diese bei nachfolgenden Startvorgängen von der App automatisch verwendet. Da ein Sicherheitsproblem entsteht, wenn ein Angreifer Zugriff auf diese gespeicherten Daten erhält, können Windows-Apps unter Windows 10 Benutzeranmeldeinformationen in einem sicheren Schließfach für Anmeldeinformationen speichern. Die App ruft die API des Schließfachs für Anmeldeinformationen auf und ruft die Anmeldeinformationen aus dem Schließfach ab, anstatt sie im Speichercontainer der App zu speichern. Das Schließfach für Anmeldeinformationen wird vom Betriebssystem verwaltet und stellt eine sicher verwaltete Lösung für die Speicherung von Anmeldeinformationen dar. Der Zugriff darauf ist jedoch der App vorbehalten, die die Daten speichert.

Wenn ein Benutzer die zu speichernden Anmeldeinformationen angibt, ruft die App mithilfe des Objekts [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) im Namespace [**Windows.Security.Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089) einen Verweis auf das Schließfach für Anmeldeinformationen ab. Anschließend wird ein Objekt [**PasswordCredential**](https://msdn.microsoft.com/library/windows/apps/br227061) erstellt, das einen Bezeichner für die Windows-App sowie den Benutzernamen und das Kennwort enthält. Dieses wird an die Methode [**PasswordVault.Add**](https://msdn.microsoft.com/library/windows/apps/hh701231) übergeben, um die Anmeldeinformationen im Schließfach zu speichern. Dies wird im folgenden C#-Codebeispiel veranschaulicht.

```cs
var vault = new PasswordVault();
vault.Add(new PasswordCredential("My App", username, password));
```

Im folgenden C#-Codebeispiel fordert die App alle Anmeldeinformationen an, die der App entsprechen, indem sie die Methode [**FindAllByResource**](https://msdn.microsoft.com/library/windows/apps/br227083) des Objekts [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) aufruft. Wenn mehr als eine zurückgegeben wird, fordert sie den Benutzer auf, seinen Benutzernamen einzugeben. Wenn die Anmeldeinformationen nicht im Schließfach enthalten sind, werden sie durch die App vom Benutzer angefordert. Anschließend wird der Benutzer mit den Anmeldeinformationen beim Server angemeldet.

```cs
private string resourceName = "My App";
private string defaultUserName;

private void Login()
{
    PasswordCredential loginCredential = GetCredentialFromLocker();

    if (loginCredential != null)
    {
        // There is a credential stored in the locker.
        // Populate the Password property of the credential
        // for automatic login.
        loginCredential.RetrievePassword();
    }
    else
    {
        // There is no credential stored in the locker.
        // Display UI to get user credentials.
        loginCredential = GetLoginCredentialUI();
    }
    // Log the user in.
    ServerLogin(loginCredential.UserName, loginCredential.Password);
}

private PasswordCredential GetCredentialFromLocker()
{
    PasswordCredential credential = null;

    var vault = new PasswordVault();
    var credentialList = vault.FindAllByResource(resourceName);

    if (credentialList.Count == 1)
    {
        credential = credentialList[0];
    }
    else if (credentialList.Count > 0)
    {
        // When there are multiple usernames,
        // retrieve the default username. If one doesn’t
        // exist, then display UI to have the user select
        // a default username.
        defaultUserName = GetDefaultUserNameUI();

        credential = vault.Retrieve(resourceName, defaultUserName);
    }
    return credential;
}
```

Weitere Informationen finden Sie unter [Schließfach für Anmeldeinformationen](credential-locker.md).

## 4.3 Schutz gespeicherter Daten


Wenn gespeicherte Daten – häufig auch At-Rest-Daten genannt – verschlüsselt werden, können nicht autorisierte Benutzer am Zugriff auf den Inhalt gespeicherter Daten gehindert werden. Die beiden gängigen Mechanismen zum Verschlüsseln von Daten verwenden symmetrische oder asymmetrische Schlüssel. Die Datenverschlüsselung kann jedoch nicht sicherstellen, dass die Daten zwischen dem Zeitpunkt des Absendens und Speicherns nicht manipuliert werden. Anders ausgedrückt bedeutet dies, dass keine Datenintegrität gewährleistet werden kann. Nachrichtenauthentifizierungscodes, Hashes und digitale Signaturen werden häufig eingesetzt, um dieses Problem zu lösen.

## 4.3.1 Datenverschlüsselung


Bei der symmetrischen Verschlüsselung weisen Absender und Empfänger denselben Schlüssel und verwenden diesen auch zum Verschlüsseln und Entschlüsseln der Daten. Die Herausforderung bei diesem Ansatz besteht normalerweise darin, den Schlüssel sicher weiterzugeben, damit er beiden Parteien bekannt ist.

Eine Antwort stellt die asymmetrische Verschlüsselung dar, bei der ein öffentliches/privates Schlüsselpaar verwendet wird. Der öffentliche Schlüssel ist für jeden frei verfügbar, der eine Nachricht verschlüsseln möchte. Der private Schlüssel ist stets geheim, damit er ausschließlich von Ihnen zum Entschlüsseln der Daten verwendet werden kann. Ein gängiges Verfahren, den öffentlichen Schlüssel offenzulegen, besteht in der Verwendung digitaler Zertifikate, die häufig nur als „Zertifikate“ bezeichnet werden. Das Zertifikat enthält Informationen zum öffentlichen Schlüssel sowie weitere Informationen zum Benutzer oder Server, z. B. Namen, Aussteller, E-Mail-Adresse und Land.

Entwickler von Windows-Apps können mithilfe der Klasse [**SymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241537) und der Klasse [**AsymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241478) die symmetrische und asymmetrische Verschlüsselung in ihre UWP-Apps implementieren. Darüber hinaus kann die Klasse [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) verwendet werden, um Daten zu verschlüsseln und entschlüsseln, Inhalte zu signieren und digitale Signaturen zu überprüfen. Apps können auch die Klasse [**DataProtectionProvider**](https://msdn.microsoft.com/library/windows/apps/br241559) im Namespace [**Windows.Security.Cryptography.DataProtection**](https://msdn.microsoft.com/library/windows/apps/br241585) zum Verschlüsseln und Entschlüsseln lokaler gespeicherter Daten verwenden.

## 4.3 Erkennen von Nachrichtenmanipulationen (MACs, Hashes und Signaturen)


Ein MAC ist ein Code (oder Tag). Er resultiert aus der Verwendung eines symmetrischen Schlüssels (dem so genannten geheimen Schlüssel) oder einer Nachricht als Eingabe für einen MAC-Verschlüsselungsalgorithmus. Der geheime Schlüssel und Algorithmus werden vom Absender und Empfänger vor der Übertragung der Nachricht vereinbart.

MACs überprüfen Nachrichten wie folgt.

-   Der Absender leitet das MAC-Tag ab, indem er den geheimen Schlüssel als Eingabe für den MAC-Algorithmus verwendet.
-   Der Absender sendet das MAC-Tag und die Nachricht an den Empfänger.
-   Der Empfänger leitet das MAC-Tag ab, indem er den geheimen Schlüssel und die Nachricht als Eingaben für den MAC-Algorithmus verwendet.
-   Der Empfänger vergleicht das MAC-Tag mit dem MAC-Tag des Absenders. Wenn sie identisch sind, wissen wir, dass die Nachricht nicht manipuliert wurde.

![](images/secure-macs.png)

Windows-Apps können die MAC-Nachrichtenüberprüfung implementieren, indem Sie die Klasse [**MacAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241530) aufrufen, um den Schlüssel zu generieren, und die Klasse [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490), um den MAC-Verschlüsselungsalgorithmus auszuführen.

## 4.3.1 Verwenden von Hashes


Eine Hashfunktion ist ein kryptografischer Algorithmus, der für einen an ihn übergebenen Datenblock beliebiger Länge eine Bitzeichenfolge fester Größe zurückgibt, die als Hashwert bezeichnet wird. Für diese Aufgabe steht eine ganze Familie von Hashfunktionen zur Verfügung.

Im oben veranschaulichten Nachrichtenübertragungsszenario kann ein Hashwert anstelle eines MACs verwendet werden. Der Absender sendet einen Hashwert und eine Nachricht, und der Empfänger leitet seinen eigenen Hashwert vom Hashwert und der Nachricht des Absenders ab und vergleicht die beiden Hashwerte. Apps unter Windows 10 können die Klasse [**HashAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241511) aufrufen, um die verfügbaren Hashalgorithmen aufzulisten und einen davon auszuführen. Die Klasse [**CryptographicHash**](https://msdn.microsoft.com/library/windows/apps/br241498) stellt den Hashwert dar. Mit der Methode [**CryptographicHash.GetValueAndReset**](https://msdn.microsoft.com/library/windows/apps/hh701376) kann für unterschiedliche Daten mehrmals ein Hashwert generiert werden, ohne dass das Objekt jedes Mal neu erstellt werden muss. Mit der Append-Methode der Klasse **CryptographicHash** werden einem Puffer, von dem ein Hash erstellt werden soll, neue Daten hinzugefügt. Das gesamte Verfahren wird im folgenden C#-Codebeispiel veranschaulicht.

```cs
public void SampleReusableHash()
{
    // Create a string that contains the name of the
    // hashing algorithm to use.
    string strAlgName = HashAlgorithmNames.Sha512;

    // Create a HashAlgorithmProvider object.
    HashAlgorithmProvider objAlgProv = HashAlgorithmProvider.OpenAlgorithm(strAlgName);

    // Create a CryptographicHash object. This object can be reused to continually
    // hash new messages.
    CryptographicHash objHash = objAlgProv.CreateHash();

    // Hash message 1.
    string strMsg1 = "This is message 1";
    IBuffer buffMsg1 = CryptographicBuffer.ConvertStringToBinary(strMsg1, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg1);
    IBuffer buffHash1 = objHash.GetValueAndReset();

    // Hash message 2.
    string strMsg2 = "This is message 2";
    IBuffer buffMsg2 = CryptographicBuffer.ConvertStringToBinary(strMsg2, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg2);
    IBuffer buffHash2 = objHash.GetValueAndReset();

    // Convert the hashes to string values (for display);
    string strHash1 = CryptographicBuffer.EncodeToBase64String(buffHash1);
    string strHash2 = CryptographicBuffer.EncodeToBase64String(buffHash2);
}
```

## 4.3.2 Digitale Signaturen


Die Datenintegrität einer digital signierten gespeicherten Nachricht wird auf ähnliche Weise wie bei der MAC-Authentifizierung überprüft. Hier wird veranschaulicht, wie der Workflow für die digitale Signatur funktioniert.

-   Der Absender leitet einen Hashwert (auch Digest genannt) ab, indem er die Nachricht als Eingabe für einen Hashalgorithmus verwendet.
-   Der Absender verschlüsselt den Digest mit seinem privaten Schlüssel.
-   Der Absender sendet die Nachricht, den verschlüsselten Digest und den Namen des verwendeten Hashalgorithmus.
-   Der Empfänger verwendet den öffentlichen Schlüssel, um den empfangenen verschlüsselten Digest zu entschlüsseln. Anschließend verwendet er den Hashalgorithmus, um einen Hash für die Nachricht und einen eigenen Digest zu erstellen. Zum Schluss vergleicht der Empfänger die beiden Digests (den empfangenen und entschlüsselten und den selbst erstellten). Nur, wenn die beiden übereinstimmen, kann der Empfänger sicher sein, dass die Nachricht vom Besitzer des privaten Schlüssels gesendet wurde und dass dieser tatsächlich der Absender ist, der er vorgibt zu sein, und dass die Nachricht während der Übertragung nicht verändert wurde.

![Digitale Signaturen](images/secure-digital-signatures.png)

Da Hashalgorithmen sehr schnell arbeiten, können Hashwerte selbst von umfangreichen Nachrichten schnell abgeleitet werden. Der resultierende Hashwert hat eine beliebige Länge und kann kürzer als die vollständige Nachricht sein. Daher stellt die Verwendung öffentlicher und privater Schlüssel zum ausschließlichen Verschlüsseln und Entschlüsseln des Digests anstatt der vollständigen Nachricht eine Optimierung dar.

Weitere Informationen hierzu finden Sie in den Artikeln zu [digitalen Signaturen](https://msdn.microsoft.com/library/windows/desktop/aa381977), [MACs, Hashes und Signaturen](macs-hashes-and-signatures.md) sowie zu [Kryptografie.](cryptography.md)

## 5 Zusammenfassung


Die Universelle Windows-Plattform in Windows 10 bietet verschiedene Möglichkeiten, um mithilfe von Betriebssystemfunktionen sicherere Apps zu erstellen. In anderen Authentifizierungsszenarien, z. B. einfache, mehrstufige oder vermittelte Authentifizierung über einen OAuth-Identitätsanbieter, verringern APIs die häufigsten Herausforderungen bei der Authentifizierung. Windows Hello bietet ein neues biometrisches Anmeldesystem, das den Benutzer erkennt und alle Versuche, die Identifizierung zu umgehen, aktiv verhindert. Zusammen mit Windows Hello stellt Microsoft Passport mehrere Schlüssel- und Zertifikatebenen bereit, die außerhalb des TPMs (Trusted Platform Module) niemals aufgedeckt oder verwendet werden können. Eine weitere Sicherheitsebene wird außerdem durch die optionale Verwendung von Attestation Identity Keys (AIK) und Zertifikaten bereitgestellt.

Um In-Flight-Daten zu schützen, kommunizieren APIs über SSL sicher mit Remotesystemen und ermöglichen dennoch die Überprüfung der Serverauthentizität mit SSL-Pinning. Bei der sicheren und kontrollierten Veröffentlichung von APIs unterstützt Sie Azure API -Management durch die Bereitstellung von leistungsstarken Konfigurationsoptionen für das Verfügbarmachen von APIs im Internet über einen Proxyserver, der zusätzliches Verbergen des API-Endpunkts bietet. Der Zugriff auf diese APIs wird mithilfe von API-Schlüsseln gesichert und API-Aufrufe können zum Steuern der Leistung gedrosselt werden.

Wenn die Daten auf dem Gerät empfangen werden, bietet das Windows-App-Modell mehr Kontrolle darüber, wie die App installiert und aktualisiert wird und auf Daten zugreift. Zudem verhindert es, dass sie unbefugt auf Daten von anderen Apps zugreift. Das Schließfach für Anmeldeinformationen ermöglicht die sichere Speicherung von Anmeldeinformationen von Benutzern, die vom Betriebssystem verwaltet werden. Durch Verschlüsselungs- und Hashing-APIs der Universellen Windows-Plattform können auch andere Daten auf dem Gerät geschützt werden.

## 6 Ressourcen


### 6.1 Anleitungen

-   [Authentifizierung und Benutzeridentität](authentication-and-user-identity.md)
-   [Microsoft Passport](microsoft-passport.md)
-   [Schließfach für Anmeldeinformationen](credential-locker.md)
-   [Webauthentifizierungsbroker](web-authentication-broker.md)
-   [Biometrischer Fingerabdruck](fingerprint-biometrics.md)
-   [Smartcards](smart-cards.md)
-   [Freigegebene Zertifikate](share-certificates.md)
-   [Kryptografie](cryptography.md)
-   [Zertifikate](certificates.md)
-   [Kryptografische Schlüssel](cryptographic-keys.md)
-   [Datenschutz](data-protection.md)
-   [MACs, Hashes und Signaturen](macs-hashes-and-signatures.md)
-   [Exportbeschränkungen hinsichtlich Kryptografie](export-restrictions-on-cryptography.md)
-   [Allgemeine Kryptografieaufgaben](common-cryptography-tasks.md)

### 6.2 Codebeispiele

-   [Schließfach für Anmeldeinformationen](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/PasswordVault)
-   [Auswahl von Anmeldeinformationen](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/CredentialPicker)
-   [Gerätesperrmodus mit Azure-Anmeldung](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/DeviceLockdownAzureLogin)
-   [Schutz von Unternehmensdaten](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/EnterpriseDataProtection)
-   [KeyCredentialManager](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/KeyCredentialManager)
-   [Smartcards](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/SmartCard)
-   [Webkontoverwaltung](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/WebAccountManagement)
-   [WebAuthenticationBroker](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/WebAuthenticationBroker)

### 6.3 API-Referenz

-   [**Windows.Security.Authentication.OnlineId**](https://msdn.microsoft.com/library/windows/apps/hh701371)
-   [**Windows.Security.Authentication.Web**](https://msdn.microsoft.com/library/windows/apps/br227044)
-   [**Windows.Security.Authentication.Web.Core**](https://msdn.microsoft.com/library/windows/apps/dn921967)
-   [**Windows.Security.Authentication.Web.Provider**](https://msdn.microsoft.com/library/windows/apps/dn921965)
-   [**Windows.Security.Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089)
-   [**Windows.Security.Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089)
-   [**Windows.Security.Credentials.UI**](https://msdn.microsoft.com/library/windows/apps/hh701356)
-   [**Windows.Security.Cryptography**](https://msdn.microsoft.com/library/windows/apps/br241404)
-   [**Windows.Security.Cryptography.Certificates**](https://msdn.microsoft.com/library/windows/apps/br241476)
-   [**Windows.Security.Cryptography.Core**](https://msdn.microsoft.com/library/windows/apps/br241547)
-   [**Windows.Security.Cryptography.DataProtection**](https://msdn.microsoft.com/library/windows/apps/br241585)
-   [**Windows.Security.ExchangeActiveSyncProvisioning**](https://msdn.microsoft.com/library/windows/apps/hh701506)
-   [**Windows.Security.EnterpriseData**](https://msdn.microsoft.com/library/windows/apps/dn279153)

<!--HONumber=Mar16_HO5-->


