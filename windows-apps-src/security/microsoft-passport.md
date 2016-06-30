---
title: "Microsoft Passport und Windows Hello"
description: "In diesem Artikel wird die neue Microsoft Passport-Technologie beschrieben, die im Lieferumfang des Windows 10-Betriebssystems enthalten ist. Zudem wird erörtert, wie Entwickler diese Technologie implementieren, um ihre UWP (Universelle Windows-Plattform)-Apps und Back-End-Dienste zu schützen. Der Artikel hebt die spezifischen Funktionen dieser Technologien hervor, die dabei helfen, aus der Verwendung herkömmlicher Anmeldeinformationen erwachsende Bedrohungen zu mindern. Darüber hinaus bietet er eine Anleitung dazu, wie diese Technologien als Bestandteil Ihrer Windows 10-Einführung entworfen und bereitgestellt werden."
ms.assetid: 0B907160-B344-4237-AF82-F9D47BCEE646
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: 781866bbbef755815422d1aa5bf71010bb08630d

---

# Microsoft Passport und Windows Hello


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


In diesem Artikel wird die neue Microsoft Passport-Technologie beschrieben, die im Lieferumfang des Windows 10-Betriebssystems enthalten ist. Zudem wird erörtert, wie Entwickler diese Technologie implementieren, um ihre UWP (Universelle Windows-Plattform)-Apps und Back-End-Dienste zu schützen. Der Artikel hebt die spezifischen Funktionen dieser Technologien hervor, die dabei helfen, aus der Verwendung herkömmlicher Anmeldeinformationen erwachsende Bedrohungen zu mindern. Darüber hinaus bietet er eine Anleitung dazu, wie diese Technologien als Bestandteil Ihrer Windows 10-Einführung entworfen und bereitgestellt werden.

Beachten Sie, dass der Schwerpunkt dieses Artikels auf der App-Entwicklung liegt. Weitere Informationen zu den Architektur- und Implementierungsdetails von Microsoft Passport und Windows Hello finden Sie unter [Microsoft Passport-Anleitung auf TechNet](https://technet.microsoft.com/library/mt589441.aspx).

Ein vollständiges Codebeispiel finden Sie im [Microsoft Passport-Codebeispiel auf GitHub](http://go.microsoft.com/fwlink/?LinkID=717812).

Ein schrittweises Vorgehen zum Erstellen einer UWP-App mit Microsoft Passport und dem zugrunde liegenden Authentifizierungsdienst finden Sie in den Artikeln [Microsoft Passport-Anmelde-App](microsoft-passport-login.md) und [Microsoft Passport-Anmeldedienst](microsoft-passport-login-auth-service.md).

## 1 Einführung


Eine grundlegende Annahme über die Informationssicherheit besteht darin, dass ein System denjenigen ermitteln kann, der es verwendet. Durch das Ermitteln eines Benutzers kann das System entscheiden, ob der Benutzer ordnungsgemäß identifiziert wurde (wird auch als Authentifizierung bezeichnet), und anschließend wird bestimmt, welche Aktionen der ordnungsgemäß authentifizierte Benutzer ausführen darf (Autorisierung). Die große Mehrheit der weltweit bereitgestellten Computersysteme ist davon abhängig, dass Authentifizierungs- und Autorisierungsentscheidungen auf der Grundlage von Benutzeranmeldeinformationen getroffen werden. Demzufolge bilden wiederverwendbare, vom Benutzer erstellte Kennwörter die Sicherheitsbasis für diese Systeme. Die oftmals zitierte Regel, dass die Authentifizierung etwas beinhalten kann, was man „weiß, hat oder ist“, fasst das Problem gut zusammen: Ein wiederverwendbares Kennwort ist an sich schon ein Authentifizierungsfaktor. Jeder, der das Kennwort kennt, kann sich demnach als der Benutzer ausgeben, dem das Kennwort gehört.

## 1.1 Probleme mit herkömmlichen Anmeldeinformationen


Seit der Mitte der 1960er Jahre, als Fernando Corbató und sein Team im Massachusetts Institute of Technology die Einführung des Kennworts meisterten, müssen Benutzer und Administratoren mit der Verwendung von Kennwörtern für die Benutzerauthentifizierung und -autorisierung leben. Mit der Zeit hat sich der Leistungsstandard für die Kennwortspeicherung und die Verwendung ein wenig verbessert (beispielsweise durch Kennworthashing und Salt. Es gibt jedoch weiterhin zwei Probleme. Kennwörter lassen sich leicht klonen und stehlen. Außerdem werden sie durch Implementierungsfehler möglicherweise unsicher. Zudem müssen die Benutzer das Praktische und die Sicherheit ausgleichen.

## 1.1.1. Diebstahl von Anmeldeinformationen


Das größte Risiko in Bezug auf Kennwörtern ist: Ein Angreifer kann sie leicht stehlen. Jeder Ort, an dem ein Kennwort eingegeben, verarbeitet oder gespeichert wird, ist anfällig. So kann ein Angreifer beispielsweise eine Sammlung an Kennwörtern oder Hashwerten von einem Authentifizierungsserver stehlen, indem er Lauschangriffe auf Netzwerkdatenverkehr eines Anwendungsservers ausführt, Schadsoftware in eine Anwendung oder auf einem Gerät einschleust, die Benutzertastaturanschläge auf einem Gerät aufzeichnet oder dem Benutzer zusieht, welche Zeichen er eingibt. Diese sind nur die häufigsten Angriffsmethoden.

Ein weiteres verwandtes Risiko besteht im sogenannten „Credential Replay“ (Wiedergabe von Anmeldeinformationen). Hierbei erfasst ein Angreifer gültige Anmeldeinformationen, indem er einen Lauschangriff auf ein unsicheres Netzwerk ausführt und die Anmeldeinformationen später wiedergibt, um einen gültigen Benutzer zu imitieren. Die meisten Authentifizierungsprotokolle (einschließlich Kerberos und OAuth) schützen vor Replay-Angriffen, indem ein Zeitstempel im Anmeldeinformationsaustauschprozess eingefügt wird. Dadurch wird jedoch nur das Token geschützt, das vom Authentifizierungssystem ausgestellt wird, nicht aber das Kennwort, das vom Benutzer bereitgestellt wird, um sich ursprünglich anzumelden.

## 1.1.2 Wiederverwendung von Anmeldeinformationen




Der häufige Ansatz, eine E-Mail-Adresse als den Benutzernamen zu verwenden, verschlimmert das Problem nur noch. Ein Angreifer, der eine Kombination aus Benutzernamen und Kennwort erfolgreich aus einem kompromittierten System wiederhergestellt hat, kann anschließend dieselbe Kombination auf anderen Systemen probieren. Diese Taktik funktioniert erstaunlicherweise oft. Auf diese Weise können Angreifer ein kompromittiertes System als Sprungbrett zu anderen Systemen verwenden. Zudem führt das Verwenden von E-Mail-Adressen als Benutzernamen zu weiteren Problemen, die später in dieser Anleitung erörtert werden.

## 1.3 Beheben von Anmeldeinformationsproblemen


Das Lösen von Problemen, die Kennwörter mit sich bringen, ist nicht einfach. Die Verschärfung von Kennwortrichtlinien allein wird das Problem nicht beheben: Die Benutzer könnten ihre Kennwörter einfach wiederverwenden, teilen oder aufschreiben. Auch wenn Benutzerschulungen für die Authentifizierungssicherheit wichtig sind, wird das Problem durch die Schulung allein ebenfalls nicht beseitigt.

Microsoft Passport ersetzt Kennwörter durch eine sichere zweistufige Authentifizierung (2FA). Dabei werden vorhandene Anmeldeinformationen überprüft und gerätespezifische Anmeldeinformationen erstellt, die durch eine Benutzergeste (Biometrie- oder PIN-basiert) geschützt sind. 

## 2 Was ist Microsoft Passport?


## 2.1 Was ist Windows Hello?


Windows Hello ist der Name des neuen in Windows 10 integrierten biometrischen Anmeldesystems von Microsoft. Da es direkt im Betriebssystem integriert ist, ermöglicht Windows Hello die Gesichts- oder Fingerabdruckidentifikation zum Entsperren von Benutzergeräten. Die Authentifizierung erfolgt, wenn der Benutzer seine eindeutige biometrische ID übermittelt, um auf die gerätespezifischen Microsoft Passport-Anmeldeinformationen zuzugreifen. Demzufolge kann sich ein Angreifer, der das Gerät gestohlen hat, nicht anmelden, es sei denn, der Angreifer verfügt über die PIN. Der sichere Windows-Anmeldeinformationsspeicher schützt die biometrischen Daten auf dem Gerät. Durch das Verwenden von Windows Hello zum Entsperren eines Geräts erhält autorisierte Benutzer Zugriff auf seine sämtlichen Windows-Funktionen, -Apps, -Daten, -Websites und -Dienste.

Der Windows Hello-Authentifikator wird auch als Hello bezeichnet. Ein Hello ist für die Kombination aus einem einzelnen Gerät und einem bestimmten Benutzer eindeutig. Es wird nicht auf mehreren Geräten verwendet, nicht für einen Server oder eine aufrufende App freigegeben und kann von einem Gerät nicht einfach extrahiert werden. Wenn mehrere Benutzer ein Gerät gemeinsam verwenden, muss jeder Benutzer ein eigenes Konto einrichten. Jedes Konto erhält ein eindeutiges Hello für dieses Gerät. Sie können sich ein Hello als Token vorstellen, das Sie zum Entsperren (oder Freigeben) von gespeicherten Anmeldeinformationen verwenden können. Mit dem Hello an sich werden Sie nicht für eine App oder einen Dienst authentifiziert, es gibt jedoch die dafür benötigten Anmeldeinformationen frei. Anders ausgedrückt, handelt es sich beim Hello nicht um Benutzeranmeldeinformationen, sondern um einen zweiten Faktor für die Microsoft Passport-Anmeldung.

## 2.2 Was ist Microsoft Passport?


Windows Hello bietet Geräten eine zuverlässige Möglichkeit, einzelne Benutzer zu erkennen. Dies betrifft den ersten Teil des Wegs zwischen einem Benutzer und einem angeforderten Dienst- oder Datenelement. Nachdem das Gerät den Benutzer erkannt hat, muss es den Benutzer jedoch erst noch authentifizieren, bevor es entscheidet, ob er auf die angeforderte Ressource zugreifen darf. Microsoft Passport bietet die sichere 2FA, die vollständig in Windows integriert ist, und ersetzt wiederverwendbare Kennwörter durch eine Kombination aus einem bestimmten Gerät und einer biometrischen Geste oder PIN.

Bei Microsoft Passport handelt es sich jedoch nicht um einen bloßen Ersatz für herkömmliche 2FA-Systeme. Konzeptionell gesehen ähnelt es Smartcards. Die Authentifizierung wird mithilfe von kryptografischen Primitiven ausgeführt, anstelle Zeichenfolgen zu vergleichen. Zudem sind die Schlüssel des Benutzers in der vor Manipulationen geschützten Hardware sicher. Microsoft Passport erfordert auch keine zusätzlichen Infrastrukturkomponenten für die Smartcard-Bereitstellung. Insbesondere benötigen Sie keine Public Key-Infrastruktur (PKI) zum Verwalten von Zertifikaten, wenn Sie derzeit keine haben. Microsoft Passport kombiniert die wichtigsten Vorteile von Smartcards – Flexibilität bei der Bereitstellung virtueller Smartcards und zuverlässige Sicherheit für physische Smartcards – und birgt keinen ihrer Nachteile.

## 2.3 Funktionsweise von Microsoft Passport


Wenn der Benutzer Microsoft Passport auf seinem Computer einrichtet, generiert Microsoft Passport ein neues Schlüsselpaar bestehend aus öffentlichem und privatem Schlüssel auf dem Gerät. Das TPM generiert und schützt diesen privaten Schlüssel. Wenn das Gerät nicht über ein TPM verfügt, wird der private Schlüssel verschlüsselt und durch Software geschützt. Zudem generieren TPM-fähige Geräte einen Datenblock, mit dem bestätigt werden kann, dass ein Schlüssel an ein TPM gebunden ist. Die Nachweisinformationen können in Ihrer Lösung verwendet werden, um z. B. zu entscheiden, ob dem Benutzer eine andere Autorisierungsstufe gewährt wird.

Zum Aktivieren von Microsoft Passport auf einem Gerät muss der Benutzer entweder sein Azure Active Directory-Konto oder sein Microsoft-Konto in den Windows-Einstellungen verbinden.

## 2.3.1 Schutz von Schlüsseln


Bei jedem Generieren von Schlüsselmaterialien muss dieses gegen Angriffe geschützt werden. Die zuverlässigste Möglichkeit bietet spezialisierte Hardware. Lange Zeit wurden Hardwaresicherheitsmodule (Hardware Security Modules, HSMs) verwendet, um Schlüssel für sicherheitskritische Anwendungen zu generieren, zu speichern und zu verarbeiten. Bei Smartcards handelt es sich um einen besonderen HSM-Typ, ebenso wie Geräte, die mit dem Trusted Computing Group TPM-Standard konform sind. Nach Möglichkeit profitiert die Microsoft Passport-Implementierung von der integrierten TPM-Hardware, um Schlüssel zu generieren, zu speichern und zu verarbeiten. Für Microsoft Passport und Microsoft Passport for Work ist jedoch kein integriertes TPM erforderlich.

Microsoft empfiehlt, nach Möglichkeit TPM-Hardware zu verwenden. Das TPM schützt vor einer Vielzahl von unbekannten und potenziellen Angriffen einschließlich Brute-Force-Angriffen auf die PIN. Das TPM stellt auch nach einer Kontosperre eine zusätzliche Schutzebene bereit. Wenn das Schlüsselmaterial von TPM gesperrt wurde, muss der Benutzer die PIN zurücksetzen. Beim Zurücksetzen der PIN werden alle mit dem alten Schlüsselmaterial verschlüsselten Schlüssel und Zertifikate entfernt.

## 2.3.2 Authentifizierung


Wenn ein Benutzer auf geschütztes Schlüsselmaterial zugreifen möchte, beginnt der Authentifizierungsprozess damit, dass der Benutzer das Gerät durch Eingabe einer PIN oder eine geometrische Geste entsperrt. Dieser Vorgang wird auch als Freigeben des Schlüssels bezeichnet.

Eine Anwendung kann niemals die Schlüssel einer anderen Anwendung bzw. ein Benutzer niemals die Schlüssel anderer Benutzer verwenden. Diese Schlüssel werden zum Signieren von Anforderungen verwendet, die an den Identitätsanbieter oder IDP gesendet werden, um Zugriff auf angegebene Ressourcen zu erhalten. Anwendungen können bestimmte APIs verwenden, um Vorgänge anzufordern, für die Schlüsselmaterial für bestimmte Aktionen erforderlich sind. Für den Zugriff über diese APIs ist keine explizite Überprüfung anhand einer Benutzergeste erforderlich, und das Schlüsselmaterial wird gegenüber der anfordernden Anwendung nicht offengelegt. Stattdessen fordert die Anwendung eine bestimmte Aktion wie das Signieren eines Datenelements an, und die Microsoft Passport-Ebene führt die Aktion aus und gibt die Ergebnisse zurück.

## 2.4 Vorbereitung auf die Implementierung von Passport


Nachdem wir nun wissen, wie Microsoft Passport und Windows Hello grundsätzlich funktionieren, befassen wir uns damit, wie wir sie in unseren eigenen Anwendungen implementieren. Zuvor noch eine Klarstellung: Wenn wir von APIs reden, sind in der Regel Microsoft Passport-APIs gemeint. Derzeit sind keine APIs für Windows Hello verfügbar.

Es gibt verschiedene Szenarien, die mithilfe von Microsoft Passport implementiert werden können. Beispielsweise die Anmeldung bei der App auf einem Gerät. Das andere häufige Szenario wäre dagegen die Authentifizierung bei einem Dienst. Anstelle von Benutzernamen und Kennwort verwenden Sie Microsoft Passport. In den folgenden Kapiteln besprechen wir die Implementierung einiger verschiedener Szenarien, z. B. die Authentifizierung gegenüber Diensten mit Microsoft Passport und der Umwandlung eines vorhandenen Benutzername-/Kennwortsystems in ein Microsoft Passport-System.

Beachten Sie schließlich, dass die Microsoft Passport-APIs die Verwendung des Windows 10-SDKs erfordert, die dem Betriebssystem entspricht, unter dem die App verwendet wird. Mit anderen Worten: Das Windows-SDK 10.0.10240 muss für Apps verwendet werden, die unter Windows 10 bereitgestellt werden, und 10.0.10586 muss für Apps verwendet werden, die für Windows 10, Version 1511 bereitgestellt werden.

## 3 Implementieren von Microsoft Passport


In diesem Kapitel wir beginnen mit einem „frischen“ Szenario ohne vorhandenes Authentifizierungssystem und erläutern die Implementierung von Microsoft Passport.

Im nächsten Abschnitt wird die Migration von einem System mit vorhandenem Benutzernamen/Kennwort behandelt. Auch wenn Sie am nächsten Kapitel mehr interessiert sind, sollten Sie sich dieses ansehen, um ein grundlegendes Verständnis des Prozesses und des erforderlichen Codes zu erhalten.

## 3.1 Registrieren neuer Benutzer


Wir beginnen mit einem völlig neuen Dienst, der Microsoft Passport verwendet, und einem hypothetischen neuen Benutzer, der zur Registrierung auf einem neuen Gerät bereit ist.

Im ersten Schritt wird sichergestellt, dass Benutzer Passport verwenden kann. Die App überprüft die Benutzereinstellungen und Computerfunktionen, um sicherzustellen, dass Benutzer-ID-Schlüssel erstellt werden können. Wenn die App ermittelt, dass der Benutzer Microsoft Passport noch nicht aktiviert hat, fordert sie ihn dazu auf, Passport vor der Verwendung der App einzurichten.

Zum Aktivieren von Microsoft Passport muss der Benutzer lediglich eine PIN in Windows-Einstellungen einrichten, es sei denn, der Benutzer hat diese bereits auf der Willkommensseite (Out of Box Experience, OOBE) eingerichtet.

Die folgenden Codezeilen zeigen eine einfache Methode zum Überprüfen, ob der Benutzer Passport eingerichtet hat.

```cs
var keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
if (!keyCredentialAvailable)
{
   // User didn't set up PIN yet
   return;
}
```

Im nächsten Schritt werden die Informationen angefordert, mit denen sich der Benutzer bei Ihrem Dienst registriert. Sie können den Vornamen, Nachnamen, die E-Mail-Adresse und einen eindeutigen Benutzernamen vom Benutzer anfordern. Die E-Mail-Adresse können Sie ggf. als eindeutigen Bezeichner verwenden.

In diesem Szenario verwenden wir die E-Mail-Adresse als eindeutigen Bezeichner für den Benutzer. Nachdem sich der Benutzer registriert hat, sollten Sie eine Validierungs-E-Mail senden, um sicherzustellen, dass die Adresse gültig ist. Dadurch erhalten Sie einen Mechanismus zum Zurücksetzen des Kontos, falls dies erforderlich ist.

Wenn der Benutzer seine PIN eingerichtet hat, erstellt die App das [**KeyCredential**](https://msdn.microsoft.com/library/windows/apps/dn973029) des Benutzers. Die App ruft außerdem die optionalen Schlüsselnachweisinformationen ab, um einen kryptografischen Beweis dafür zu erhalten, dass der Schlüssel im TPM generiert wird. Der generierte öffentliche Schlüssel und optional der Nachweis werden an den Back-End-Server gesendet, um das verwendete Gerät zu registrieren. Jedes Schlüsselpaar, das auf den einzelnen Geräten generiert wird, ist eindeutig.

Der Code zum Erstellen von [**KeyCredential**](https://msdn.microsoft.com/library/windows/apps/dn973029) sieht wie folgt aus:

```cs
var keyCreationResult = await KeyCredentialManager
    .RequestCreateAsync(AccountId, KeyCredentialCreationOption.ReplaceExisting);
```

[
            **RequestCreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn973048) ist der Teil, durch den öffentlicher und privater Schlüssel erstellt werden. Wenn das Gerät über den richtigen TPM-Chip verfügt, fordern die APIs den TPM-Chip zum Erstellen des privaten und öffentlichen Schlüssels an und speichern das Ergebnis. Wenn kein TPM-Chip verfügbar ist, erstellt das Betriebssystem das Schlüsselpaar im Code. Die App hat keine Möglichkeit, direkt auf die erstellten privaten Schlüssel zuzugreifen. Bei der Erstellung von Schlüsselpaaren geht es auch um die resultierenden Nachweisinformationen. (Weitere Informationen zu Nachweisen finden Sie im nächsten Abschnitt.)

Nachdem das Schlüsselpaar und die Nachweisinformationen auf dem Gerät erstellt wurden, müssen der öffentliche Schlüssel, die optionalen Nachweisinformationen und der eindeutige Bezeichner (z. B. die E-Mail-Adresse) an den Back-End-Registrierungsdienst gesendet und im Back-End gespeichert werden.

Damit der Benutzer über mehrere Geräte auf die App zugreifen kann, muss der Back-End-Dienst mehrere Schlüssel für denselben Benutzer speichern können. Jeder Schlüssel ist für jedes Gerät eindeutig. Deshalb werden alle Schüssel gespeichert, die demselben Benutzer zugeordnet sind. Ein Gerätebezeichner dient zur Optimierung der Serverseite bei der Benutzerauthentifizierung. Darauf gehen wir im nächsten Kapitel genauer ein.

Ein Beispieldatenbankschema zum Speichern dieser Informationen im Back-End kann wie folgt aussehen:

![Passport-Beispieldatenbankschema](images/passport-db.png)

Die Registrierungslogik kann wie folgt aussehen:

![Registrierungslogik für Passport](images/passport-registration.png)

Die von Ihnen erfassten Registrierungsinformationen umfassen natürlich viel mehr Identifikationsinformationen, als wir in dieses einfache Szenario einbinden. Wenn Ihre App beispielsweise auf einen gesicherten Dienst – z. B. Onlinebanking – zugreift, müssen Sie beim Anmeldevorgang einen Identitätsnachweis und andere Dinge anfordern. Nachdem alle Bedingungen erfüllt wurden, wird der öffentliche Schlüssel dieses Benutzers im Back-End gespeichert und für Überprüfungszwecke verwendet, wenn der Benutzer den Dienst das nächste Mal verwendet.

```cs
using System;
using System.Runtime;
using System.Threading.Tasks;
using Windows.Storage.Streams;
using Windows.Security.Credentials;

static async void RegisterUser(string AccountId)
{
    var keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
    if (!keyCredentialAvailable)
    {
        // The user didn't set up a PIN yet
        return;
    }

    var keyCreationResult = await KeyCredentialManager.RequestCreateAsync(AccountId, KeyCredentialCreationOption.ReplaceExisting);
    if (keyCreationResult.Status == KeyCredentialStatus.Success)
    {
        var userKey = keyCreationResult.Credential;
        var publicKey = userKey.RetrievePublicKey();
        var keyAttestationResult = await userKey.GetAttestationAsync();
        IBuffer keyAttestation = null;
        IBuffer certificateChain = null;
        bool keyAttesttionIncluded = false;
        bool keyAttestationCanBeRetrievedLAter = false;

        KeyCredentialAttestationResult keyAttestationResult = await userKey.GetAttestationAsync();
        KeyCredentialAttestationStatus keyAttestationRetryType = 0;

        if (keyAttestationResult.Status == KeyCredentialAttestationStatus.Success)
        {
            keyAttestationIncluded = true;
            keyAttestation = keyAttestationResult.AttestationBuffer;
            certificateChain = keyAttestationResult.CertificateChainBuffer;
        }
        else if (keyAttestationResult.Status == KeyCredentialAttestationStatus.TemporaryFailure)
        {
            keyAttestationRetryType = KeyCredentialAttestationStatus.TemporaryFailure;
            keyAttestationCanBeRetrievedLater = true;
        }
        else if (keyAttestationResult.Status == KeyCredentialAttestationStatus.NotSupported)
        {
            keyAttestationRetryType = KeyCredentialAttestationStatus.NotSupported;
            keyAttestationCanBeRetrievedLAter = true;
        }
    }
    else if (keyCreationResult.Status == KeyCredentialStatus.UserCanceled ||
        keyCreationResult.Status == KeyCredentialStatus.UserPrefersPassword)
    {
        // Show error message to the user to get confirmation that user
        // does not want to enroll.
    }
}
```

## 3.1.1 Nachweis


Beim Erstellen des Schlüsselpaars besteht auch die Möglichkeit, die vom TPM-Chip generierten Nachweisinformationen anzufordern. Diese optionalen Informationen können im Rahmen des Registrierungsvorgangs an den Server gesendet werden. Der TPM-Schlüsselnachweis ist ein Protokoll, durch das kryptografisch überprüft wird, ob ein Schlüssel TPM-gebunden ist. Mithilfe dieses Nachweistyps kann gewährleistet werden, dass ein bestimmter kryptografischer Vorgang im TPM eines bestimmten Computers ausgeführt wurde.

Wenn der Server den generierten RSA-Schlüssel, die Nachweisanweisung und das AIK-Zertifikat empfängt, überprüft er folgende Voraussetzungen:

-   Die AIK-Zertifikatsignatur muss gültig sein.
-   Das AIK-Zertifikat muss mit einer vertrauenswürdigen Stammzertifizierungsstelle verkettet sein.
-   Das AIK-Zertifikat und dessen Kette müssen für EKU-OID „2.23.133.8.3“ aktiviert sein (der Anzeigename lautet „Attestation Identity Key-Zertifikat“).
-   Das AIK-Zertifikat muss zeitlich gültig sein.
-   Die Zertifikate aller ausstellenden Zertifizierungsstellen in der Kette müssen zeitlich gültig und dürfen nicht widerrufen sein.
-   Die Nachweisanweisung muss das richtige Format aufweisen.
-   Die Signatur für den [**KeyAttestation**](https://msdn.microsoft.com/library/windows/apps/dn298288)-BLOB verwendet einen öffentlichen AIK-Schlüssel.
-   Der im [**KeyAttestation**](https://msdn.microsoft.com/library/windows/apps/dn298288)-BLOB enthaltene öffentliche Schlüssel muss mit dem öffentlichen RSA-Schlüssel übereinstimmen, den der Client zusammen mit der Nachweisanweisung gesendet hat.

Ihre App weist dem Benutzer je nach diesen Umständen möglicherweise eine andere Autorisierungsstufe zu. Wenn beispielsweise eine dieser Voraussetzungen nicht erfüllt ist, registriert die App den Benutzer möglicherweise nicht bzw. schränkt dessen Befugnisse ein.

## 3.2 Anmeldung mit Microsoft Passport


Nachdem sich der Benutzer in Ihrem System registriert ist, kann er die App nutzen. Je nach Szenario können Sie die Authentifizierung des Benutzers anfordern, bevor er mit der App-Nutzung beginnen darf, oder die Authentifizierung danach mithilfe der Back-End-Dienste anfordern.

## 3.3 Erzwingen der erneuten Benutzeranmeldung


In einigen Szenarien sollte der Benutzer nachweisen, dass es sich um die derzeit angemeldete Person handelt, bevor er auf die App zugreift bzw. in manchen Fällen bevor er eine bestimmte Aktion innerhalb der App ausführt. Bevor beispielsweise eine Banking-App den Befehl zum Überweisen von Geld an den Server sendet, sollten Sie sicherstellen, dass es sich um den richtigen Benutzer handelt, anstatt einer Person, die ein angemeldetes Gerät gefunden hat und versucht, eine Transaktion ausführen. Sie können eine erneute Benutzeranmeldung in der App erzwingen, indem Sie die [**UserConsentVerifier**](https://msdn.microsoft.com/library/windows/apps/dn279134)-Klasse verwenden. Durch die folgende Codezeile wird die erneute Eingabe von Anmeldeinformationen erzwungen.

Durch die folgende Codezeile wird die erneute Eingabe von Anmeldeinformationen erzwungen.

```cs
UserConsentVerificationResult consentResult = await UserConsentVerifier.RequestVerificationAsync("userMessage");
if (consentResult.Equals(UserConsentVerificationResult.Verified))
{
   // continue
}
```

Natürlich können Sie auch den Abfrage-/Rückmeldungsmechanismus verwenden, bei dem der Server den Benutzer zur Eingabe seines PIN-Codes von biometrischen Anmeldeinformationen auffordert. Dies hängt von dem Szenario ab, das Sie als Entwickler implementieren müssen. Dieser Mechanismus wird im folgenden Abschnitt beschrieben.

## 3.4 Authentifizierung im Back-End


Wenn eine App versucht, auf einen geschützten Back-End-Dienst zuzugreifen, sendet dieser eine Abfrage an die App. Die App verwendet den privaten Schlüssel des Benutzers, um die Abfrage zu signieren, und sendet sie an den Server zurück. Da der Server den öffentlichen Schlüssel für diesen Benutzer gespeichert hat, stellt er anhand standardmäßiger Krypto-APIs sicher, dass die Nachricht tatsächlich mit dem richtigen privaten Schlüssel signiert wurde. Auf dem Client erfolgt die Signierung mithilfe der Microsoft Passport-APIs; der Entwickler selbst erhält niemals Zugriff auf den privaten Schlüssel eines Benutzers.

Neben der Schlüsselüberprüfung kann der Dienst auch den Schlüsselnachweis überprüfen, um festzustellen, ob Beschränkungen hinsichtlich der Speicherung der Schlüssel auf dem Gerät bestehen. Wenn das Gerät beispielsweise TPM zum Schützen der Schlüssel verwendet, ist die Sicherheit höher als bei Geräten, auf denen Schlüssel ohne TPM gespeichert werden. Die Back-End-Logik könnte z. B. festlegen, dass der Benutzer Geld nur bis zu einer bestimmten Höhe überweisen darf, wenn kein TPM zur Risikominderung eingesetzt wird.

Ein Nachweis ist nur für Geräte mit einem TPM-Chip der Version 2.0 oder höher verfügbar. Daher müssen Sie berücksichtigen, dass diese Informationen nicht immer auf jedem Gerät zur Verfügung stehen.

Der Clientworkflow sieht ggf. wie das folgende Diagramm aus:

![Passport-Clientworkflow](images/passport-client-workflow.png)

Wenn die App den Dienst im Back-End aufruft, sendet der Server eine Abfrage. Die Abfrage ist mit dem folgenden Code signiert:

```cs
var openKeyResult = await KeyCredentialManager.OpenAsync(AccountId);

if (openKeyResult.Status == KeyCredentialStatus.Success)
{
    var userKey = openKeyResult.Credential;
    var publicKey = userKey.RetrievePublicKey();
    var signResult = await userKey.RequestSignAsync(message);
    
    if (signResult.Status == KeyCredentialStatus.Success)
    {
        return signResult.Result;
    }
    else if (signResult.Status == KeyCredentialStatus.UserPrefersPassword)
    {
        
    }
}
```

Durch die erste Zeile [**KeyCredentialManager.OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn973046) wird das Betriebssystem aufgefordert, den Schlüsselhandle zu öffnen. Wenn dieser Schritt erfolgreich ist, können Sie die Abfragenachricht mit der [**KeyCredential.RequestSignAsync**](https://msdn.microsoft.com/library/windows/apps/dn973058)-Methode signieren. Dadurch wird das Betriebssystem aufgefordert, die PIN oder biometrische Daten des Benutzers über Windows Hello anzufordern. Der Entwickler hat zu keiner Zeit Zugriff auf den privaten Schlüssel des Benutzers. Dieser wird durch die APIs gesichert.

Die APIs fordern das Betriebssystem auf, die Abfrage mit dem privaten Schlüssel zu signieren. Das System fordert dann vom Benutzer einen PIN-Code oder konfigurierte biometrische Anmeldeinformationen an. Wenn die richtigen Informationen eingegeben werden, kann das System den TPM-Chip zum Ausführen der kryptografischen Funktionen und zum Signieren der Abfrage auffordern. (Falls kein TPM verfügbar ist, verwenden Sie die Fallback-Softwarelösung.) Der Client muss die signierte Abfrage zurück an den Server senden.

In diesem Sequenzdiagramm ist ein grundlegender Abfrage/Rückmeldungsablauf dargestellt:

![Passport-Rückmeldung auf Abfrage](images/passport-challenge-response.png)

Als nächstes muss der Server die Signatur überprüfen. Wenn Sie den öffentlichen Schlüssel anfordern und ihn zur späteren Überprüfung an den Server senden, verwenden Sie ein ASN.1-codiertes PublicKeyInfo-BLOB. Anhand des [Microsoft Passport-Codebeispiels auf GitHub](http://go.microsoft.com/fwlink/?LinkID=717812) erkennen Sie, dass Hilfsklassen die Crypt32-Funktionen umschließen, um das ASN.1-codierte BLOB in ein gängigeres CNG-BLOB zu übersetzen. Das BLOB enthält den Algorithmus des öffentlichen Schlüssels, also RSA, und den öffentlichen RSA-Schlüssel.

Sobald das CNG-BLOB verfügbar ist, müssen Sie die signierte Abfrage mit dem öffentlichen Schlüssel des Benutzers abgleichen. Da jeder seine eigene System- oder Back-End-Technologie verwendet, gibt es keine allgemeine Vorgehensweise zur Implementierung dieser Logik. Wir verwenden SHA256 als Hashalgorithmus und Pkcs1 für SignaturePadding, um sicherzustellen, dass Sie diese Funktionen zur Überprüfung der signierten Antwort vom Client verwenden. An dieser Stelle verweisen wir erneut auf das Beispiel, in dem die Vorgehensweise mit .NET 4.6 auf dem Server beschrieben wird. In der Regel werden jedoch etwa folgende Schritte ausgeführt:

```cs
using (RSACng pubKey = new RSACng(publicKey))
{
   retval = pubKey.VerifyData(originalChallenge, responseSignature,  HashAlgorithmName.SHA256, RSASignaturePadding.Pkcs1); 
}
```

Wir lesen den gespeicherten öffentlichen Schlüssel, bei dem es sich um einen RSA-Schlüssel handelt. Wir gleichen die Nachricht mit der signierten Abfrage mit dem öffentlichen Schlüssel ab. Nach erfolgreicher Überprüfung wird der Benutzer autorisiert. Sobald der Benutzer authentifiziert wurde, kann die App die Back-End-Dienste wie gewohnt aufrufen.

Der vollständige Code sieht etwa wie folgt aus:

```cs
using System;
using System.Runtime;
using System.Threading.Tasks;
using Windows.Storage.Streams;
using Windows.Security.Cryptography;
using Windows.Security.Cryptography.Core;
using Windows.Security.Credentials;

static async Task<IBuffer> GetAuthenticationMessageAsync(IBuffer message, String AccountId)
{
    var openKeyResult = await KeyCredentialManager.OpenAsync(AccountId);

    if (openKeyResult.Status == KeyCredentialStatus.Success)
    {
        var userKey = openKeyResult.Credential;
        var publicKey = userKey.RetrievePublicKey();
        var signResult = await userKey.RequestSignAsync(message);
        if (signResult.Status == KeyCredentialStatus.Success)
        {
            return signResult.Result;
        }
        else if (signResult.Status == KeyCredentialStatus.UserCanceled)
        {
            // Launch app-specific flow to handle the scenario 
            return null;
        }
    }
    else if (openKeyResult.Status == KeyCredentialStatus.NotFound)
    {
        // PIN reset has occurred somewhere else and key is lost.
        // Repeat key registration
        return null;
    }
    else
    {
        // Show custom UI because unknown error has happened.
        return null;
    }
}
```

Die Implementierung des richtigen Abfrage-/Rückmeldungsmechanismus wird in diesem Dokument nicht erörtert. Dennoch erfordert dieses Thema Aufmerksamkeit, um erfolgreich einen sicheren Mechanismus zu erstellen, der Replay- oder Man-in-the-Middle-Angriffe wirksam abwehrt.

## 3.5 Registrieren eines anderen Geräts


Heutzutage ist es üblich, dass Benutzer über mehrere Geräte verfügen, auf denen die gleichen Apps installiert sind. Wie funktioniert dies bei der Verwendung von Microsoft Passport mit mehreren Geräten?

Bei Verwendung von Microsoft Passport erstellt jedes Gerät eine eindeutige Kombination bestehend aus einem privaten und einem öffentlichen Schlüssel. Wenn ein Benutzer in der Lage sein soll, mehrere Geräte zu verwenden, müssen im Back-End folglich mehrere öffentliche Schlüssel dieses Benutzers gespeichert werden können. Im Datenbankdiagramm in Abschnitt 2.1 finden Sie ein Beispiel für die Tabellenstruktur.

Die Registrierung eines weiteren Geräts funktioniert fast genauso wie die erstmalige Registrierung eines Benutzers. Sie müssen weiterhin sicherstellen, dass es sich bei dem Benutzer, der sich für dieses neue Gerät registriert, tatsächlich um den Benutzer handelt, der er zu sein vorgibt. Sie können dazu einen beliebigen aktuellen Mechanismus für die zweistufige Authentifizierung verwenden. Es gibt mehrere Möglichkeiten zur sicheren Umsetzung dieses Schritts. Die Vorgehensweise hängt vom jeweiligen Szenario ab.

Wenn Sie z. B. immer noch Anmeldenamen und Kennwort verwenden, können Sie den Benutzer darüber authentifizieren und ihn zur Verwendung einer seiner Überprüfungsmethoden wie SMS oder E-Mail auffordern. Wenn Sie keinen Benutzernamen und kein Kennwort besitzen, können Sie auch eines der bereits registrierten Geräte verwenden, um eine Benachrichtigung an die App auf dem Gerät zu senden. Ein Beispiel dafür ist die MSA-Authentifikator-App. Kurz gesagt: Sie sollten einen gängigen 2FA-Mechanismus verwenden, um zusätzliche Geräte für den Benutzer zu registrieren.

Der Code zum Registrieren des neuen Geräts entspricht daher genau dem Code, der für die erstmalige Registrierung des Benutzers (innerhalb der App) verwendet wird.

```cs
var keyCreationResult = await KeyCredentialManager.RequestCreateAsync(
    AccountId, KeyCredentialCreationOption.ReplaceExisting);
```

Damit der Benutzer einfacher erkennen kann, welche Geräte registriert sind, können Sie den Gerätenamen oder einen anderen Bezeichner mit der Registrierung senden. Dies ist z. B. auch hilfreich, wenn Sie einen Dienst für das Back-End implementieren möchten, durch den die Benutzer die Registrierung bei Geräteverlust aufheben können.

## 3.6 Verwenden mehrerer Konten in der App


Neben der Unterstützung von mehreren Geräten für ein Konto ist es auch gängig, mehrere Konten in einer App zu unterstützen. Ein Beispiel dafür ist die Anmeldung an mehreren Twitter-Konten innerhalb Ihrer App. Mit Microsoft Passport können Sie mehrere Schlüsselpaare erstellen und mehrere Konten in Ihrer App unterstützen.

Eine Möglichkeit besteht darin, den im vorherigen Kapitel beschriebenen Benutzernamen oder eindeutigen Bezeichner in einem isolierten Speicher zu speichern. Daher wird jedes Mal, wenn Sie ein neues Konto erstellen, die Konto-ID im isolierten Speicher abgelegt.

Auf der App-Benutzeroberfläche bieten Sie dem Benutzer an, eines der zuvor erstellten Konten auszuwählen oder sich mit einem neuen Konto zu registrieren. Die Erstellung eines neuen Kontos ist identisch mit den oben beschriebenen Schritten. Für die Auswahl eines Kontos müssen die gespeicherten Konten auf dem Bildschirm aufgelistet werden. Nachdem der Benutzer ein Konto ausgewählt hat, wird er über die Konto-ID bei der App angemeldet:

```cs
var openKeyResult = await KeyCredentialManager.OpenAsync(AccountId);
```

Die übrigen Schritte wurden oben bereits beschrieben. Es sei noch einmal betont, dass alle diese Konten durch dieselbe PIN oder biometrische Geste geschützt werden, da sie in diesem Szenario auf einem Gerät mit demselben Windows-Konto verwendet werden.

## 4 Migrieren eines vorhandenen Systems zu Microsoft Passport


In diesem kurzen Abschnitt gehen wir davon aus, dass eine UWP (Universelle Windows-Plattform)-App vorhanden ist und das Back-End-System eine Datenbank verwendet, in der der Benutzername und der Kennworthash gespeichert sind. Diese Apps erfassen beim Start der App die Anmeldeinformationen des Benutzers und verwenden diese, wenn das Back-End-System die Authentifizierungsaufforderung zurückgibt.

In diesem Abschnitt wird beschrieben, welche Elemente geändert oder ersetzt werden müssen, damit Microsoft Passport funktioniert.

Die meisten der Verfahren haben wir bereits in den vorhergehenden Kapiteln behandelt. Das Hinzufügen von Microsoft Passport zu Ihrem vorhandenen System beinhaltet das Hinzufügen einiger verschiedener Workflows im Registrierungs- und Authentifizierungsteil Ihres Codes.

Ein Ansatz hierfür ist, den Benutzer entscheiden zu lassen, wann das Upgrade durchgeführt wird. Wenn Sie nach der Anmeldung des Benutzers bei der App erkennen, dass Microsoft Passport von der App und dem Betriebssystem unterstützt wird, können Sie den Benutzer fragen, ob er seine Anmeldeinformationen auf dieses moderne und sicherheitsoptimierte System umstellen möchte. Anhand des folgenden Codes können Sie überprüfen, ob der Benutzer Microsoft Passport verwenden kann.

```cs
var keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
```

Die Benutzeroberfläche sieht ungefähr wie folgt aus:

![Benutzeroberfläche von Passport](images/passport-ui.png)

Wenn sich der Benutzer entschließt, auf Microsoft Passport umzusteigen, müssen Sie die [**KeyCredential**](https://msdn.microsoft.com/library/windows/apps/dn973029) in der oben beschriebenen Weise erstellen. Der Registrierungsserver im Back-End fügt der Datenbank den öffentlichen Schlüssel und die optionale Nachweisanweisung hinzu. Da der Benutzer bereits anhand von Benutzernamen und Kennwort authentifiziert wurde, kann der Server die neuen Anmeldeinformationen mit den aktuellen Benutzerinformationen in der Datenbank verknüpfen. Das Datenbankmodell kann dem oben beschriebenen Beispiel entsprechen.

Wenn die App die [**KeyCredential**](https://msdn.microsoft.com/library/windows/apps/dn973029) des Benutzers erstellen konnte, speichert sie die Benutzer-ID im isolierten Speicher, damit der Benutzer dieses Konto aus der Liste auswählen kann, wenn er die App das nächste Mal startet. Ab diesem Punkt folgt die Steuerung exakt den Beispielen in früheren Kapiteln.

Der letzte Schritt bei der Migration zu einem vollständigen Microsoft Passport-Szenario besteht darin, den Anmeldenamen und die Kennwortoption in der App zu deaktivieren und die gespeicherten Kennworthashwerte aus der Datenbank zu entfernen.

## 5 Zusammenfassung


Mit Windows 10 wird eine höhere Sicherheitsebene eingeführt, die außerdem einfach umzusetzen ist. Windows Hello bietet ein neues biometrisches Anmeldesystem, das den Benutzer erkennt und alle Versuche, die Identifizierung zu umgehen, aktiv verhindert. Zusammen mit Windows Hello stellt Microsoft Passport mehrere Schlüssel- und Zertifikatebenen bereit, die außerhalb des TPMs (Trusted Platform Module) niemals aufgedeckt oder verwendet werden können. Eine weitere Sicherheitsebene wird außerdem durch die optionale Verwendung von Attestation Identity Keys (AIK) und Zertifikaten bereitgestellt.

Dieser Leitfaden enthält Informationen zum Entwurf und zur Bereitstellung dieser Technologien. Damit können Sie als Entwickler Ihre Windows 10-Rollouts im Handumdrehen mit sicherer Authentifizierung ausstatten, die den Schutz von Apps und Back-End-Diensten gewährleistet. Der erforderliche Code ist überschaubar und leicht verständlich. Die eigentliche Arbeit überlassen Sie einfach Windows 10.

Flexible Implementierungsoptionen ermöglichen Microsoft Passport und Windows Hello, das vorhandene Authentifizierungssystem zu ersetzen oder es einzubinden. Der Bereitstellung erfolgt schnell und kosteneffizient. Zur Bereitstellung von Windows 10-Sicherheit ist keine zusätzliche Infrastruktur erforderlich. Durch die Integration von Microsoft Passport und Microsoft Hello in das Betriebssystem ist Windows 10 die sicherste Antwort auf Authentifizierungsprobleme, mit denen Entwickler heute konfrontiert sind.

Mission erfüllt! Sie haben das Internet gerade sicherer gemacht!

## 6 Ressourcen


### 6.1 Artikel und Beispielcode

-   [Übersicht über Windows Hello](http://windows.microsoft.com/windows-10/getstarted-what-is-hello)
-   [Implementierungsdetails für Microsoft Passport und Windows Hello](https://msdn.microsoft.com/library/mt589441)
-   [Microsoft Passport-Codebeispiel auf GitHub](http://go.microsoft.com/fwlink/?LinkID=717812)

### 6.2 Terminologie

|                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AIK                 | Ein Attestation Identity Key (AIK) liefert einen solchen kryptografischen Nachweis (TPM-Schlüsselnachweis), indem die Eigenschaften des nicht migrierbaren Schlüssel signiert und Eigenschaften und Signatur der vertrauenden Seite zur Überprüfung überlassen werden. Die resultierende Signatur wird als „Nachweisanweisung“ bezeichnet. Da die Signatur mithilfe des privaten AIK-Schlüssels erstellt wird – der nur in dem TPM verwendet werden kann, von dem er erstellt wurde – kann die vertrauende Seite sicher davon ausgehen, dass der nachgewiesene Schlüssel nicht migrierbar ist und außerhalb dieses TPMs nicht verwendet werden kann. |
| AIK-Zertifikat     | Ein AIK-Zertifikat wird verwendet, um das Vorhandensein eines AIKs in einem TPM nachzuweisen. Außerdem wird damit nachgewiesen, dass die anderen vom AIK zertifizierten Schlüssel ebenfalls von diesem TPM stammen.                                                                                                                                                                                                                                                                                                                                              |
| IDP                 | Ein IDP (Identity Provider) ist ein Identitätsanbieter. Ein Beispiel ist der von Microsoft für Microsoft-Konten erstellte IDP. Jedes Mal, wenn eine Anwendung sich mit einem Microsoft-Konto (MSA) authentifizieren muss, kann sie den MSA-IDP aufrufen.                                                                                                                                                                                                                                                                                                                                        |
| PKI                 | Public Key-Infrastruktur (PKI) bezeichnet üblicherweise eine von einer Organisation gehostete Umgebung, bei der die Organisation selbst für die Erstellung und den Widerruf von Schlüsseln usw. verantwortlich ist.                                                                                                                                                                                                                                                                                                                                                           |
| TPM                 | Mit dem TPM (Trusted Platform Module) können kryptografische Schlüsselpaare bestehend aus einem öffentlichen und privaten Schlüssel so erstellt werden, dass der private Schlüssel außerhalb des TPMs niemals aufgedeckt oder verwendet werden kann (nicht migrierbarer Schlüssel).                                                                                                                                                                                                                                                                                                               |
| TPM-Schlüsselnachweis | Ein Protokoll, durch das kryptografisch geprüft wird, ob ein Schlüssel TPM-gebunden ist. Mithilfe dieses Nachweistyps kann gewährleistet werden, dass ein bestimmter kryptografischer Vorgang im TPM eines bestimmten Computers ausgeführt wurde.                                                                                                                                                                                                                                                                                                                       |

 

## Verwandte Themen

* [Microsoft Passport-Anmelde-App](microsoft-passport-login.md)
* [Microsoft Passport-Anmeldedienst](microsoft-passport-login-auth-service.md)


<!--HONumber=Jun16_HO4-->


