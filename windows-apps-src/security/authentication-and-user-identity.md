---
title: "Authentifizierung und Benutzeridentität"
description: "Apps für die universelle Windows-Plattform (UWP) bieten mehrere Optionen für die Benutzerauthentifizierung – vom einfachen einmaligen Anmelden (Single Sign-On, SSO) mit einem Webauthentifizierungsbroker bis hin zur äußerst sicheren zweistufigen Authentifizierung."
ms.assetid: 53E36DDC-200A-4850-AAF0-07ECA3662BB9
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: 3c890ac8d8363982d9f014c36b6cba59bee39f20
ms.openlocfilehash: 5ed154b3d31e8741972edc8c8a0fd343f41d4167

---

# Authentifizierung und Benutzeridentität


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

UWP (Universelle Windows-Plattform)-Apps bieten mehrere Optionen für die Benutzerauthentifizierung, vom einfachen einmaligen Anmelden (Single Sign-On, SSO) mit einem [Webauthentifizierungsbroker](web-authentication-broker.md) bis hin zur äußerst sicheren zweistufigen Authentifizierung.

Normale App-Verbindungen mit Identitätsanbieterdiensten von Drittanbietern, z.B. Facebook, Twitter, Flickr usw., verwenden den [Webauthentifizierungsbroker](web-authentication-broker.md). Verwenden Sie das [Schließfach für Anmeldeinformationen](credential-locker.md), um die Benutzerfreundlichkeit beim Speichern und Roaming der Anmeldeinformationen des Benutzers zu verbessern.

Für Unternehmen, die Windows10 nutzen, empfiehlt es sich unbedingt, [MicrosoftPassport und WindowsHello](microsoft-passport.md) zu verwenden, um eine äußerst sichere zweistufige Authentifizierung zu erzielen. Wenn MicrosoftPassport nicht verwendet werden kann, bieten [Smartcards](smart-cards.md) und der [biometrische Fingerabdruck](fingerprint-biometrics.md) eine zusätzliche Sicherheitsstufe.

<table>
<tr><th>Thema</th><th>Beschreibung</th></tr>
<tr><td>[Schließfach für Anmeldeinformationen](credential-locker.md)</td><td>In diesem Artikel wird beschrieben, wie Apps mit dem Schließfach für Anmeldeinformationen Benutzeranmeldeinformationen sicher speichern und abrufen können und sie per Roaming mit dem Microsoft-Konto des Benutzers zwischen Geräten übertragen können.</td></tr>

<tr><td>[Biometrischer Fingerabdruck](fingerprint-biometrics.md) </td><td>In diesem Artikel wird beschrieben, wie Sie Ihrer App einen biometrischen Fingerabdruck hinzufügen. Sie können die Sicherheit Ihrer App erhöhen, indem Sie die Anforderung einer Authentifizierung per Fingerabdruck integrieren, wenn die Zustimmung des Benutzers für eine bestimmte Aktion erforderlich ist. So können Sie beispielsweise die Authentifizierung per Fingerabdruck vor der Autorisierung eines In-App-Kaufs oder vor dem Zugriff auf eingeschränkte Ressourcen anfordern. Die Verwaltung der Authentifizierung per Fingerabdruck erfolgt mithilfe der [UserConsentVerifier](https://msdn.microsoft.com/library/windows/apps/dn279134)-Klasse im [Windows.Security.Credentials.UI](https://msdn.microsoft.com/library/windows/apps/hh701356)-Namespace.</td></tr>
<tr><td>[Microsoft Passport und Windows Hello](microsoft-passport.md)</td><td>In diesem Artikel wird die neue Microsoft Passport-Technologie von Windows10 beschrieben und erörtert, wie Entwickler diese Technologie implementieren, um ihre Apps und Back-End-Dienste zu schützen. Darin werden die spezifischen Funktionen dieser Technologien hervorgehoben, die dabei helfen, aus herkömmlichen Anmeldeinformationen erwachsende Bedrohungen zu mindern. Darüber hinaus bietet sie eine Anleitung darüber, wie diese Technologien als Bestandteil Ihrer Windows10-Einführung entworfen und bereitgestellt werden. </td></tr>
<tr><td>[Erstellen einer Microsoft Passport-Anmelde-App](microsoft-passport-login.md)</td><td>Teil1 der umfassenden exemplarischen Vorgehensweise zum Erstellen einer Windows10-App für die Universelle Windows-Plattform (UWP), die Microsoft Passport als Alternative zu herkömmlichen Authentifizierungssystemen mit Benutzername und Kennwort verwendet.</td></tr>
<tr><td>[Erstellen eines Microsoft Passport-Anmeldediensts](microsoft-passport-login-auth-service.md)</td><td>Teil2 der umfassenden exemplarischen Vorgehensweise zum Verwenden von MicrosoftPassport als Alternative zu herkömmlichen Authentifizierungssystemen mit Benutzername und Kennwort in Windows10-Apps für die universelle Windows-Plattform (UWP).</td></tr>
<tr><td>[Smartcards](smart-cards.md)</td><td>In diesem Thema wird erläutert, wie Apps Smartcards verwenden können, um Benutzer mit sicheren Netzwerkdiensten zu verbinden, einschließlich Informationen für den Zugriff auf physische Smartcardleser, zum Erstellen virtueller Smartcards, zum Kommunizieren mit Smartcards, zum Authentifizieren von Benutzern, zum Zurücksetzen von Benutzer-PINs und zum Entfernen oder Trennen von Smartcards.</td></tr>
<tr><td>[Freigabe von Zertifikaten zwischen Apps](share-certificates.md)</td><td>UWP-Apps, die über eine Kombination aus Benutzer-ID und Kennwort hinaus eine sichere Authentifizierung benötigen, können Zertifikate für die Authentifizierung verwenden. Die Zertifikatauthentifizierung bietet eine hohe Vertrauenswürdigkeit bei der Benutzerauthentifizierung. Es kann vorkommen, dass eine Gruppe von Diensten einen Benutzer für mehrere Apps authentifizieren möchte. In diesem Artikel wird gezeigt, wie Sie mehrere Apps mit demselben Zertifikat authentifizieren und für einen Benutzer geeigneten Code zum Importieren eines Zertifikats bereitstellen können, das für den Zugriff auf sichere Webdienste bestimmt ist.</td></tr>
<tr><td>[Entsperren von Windows mit Begleitgeräten (IoT)](companion-device-unlock.md)</td><td>Ein Begleitgerät ist ein Gerät, das in Verbindung mit dem Windows10-Desktopgerät zur Verbesserung der Benutzerauthentifizierung verwendet werden kann. Mit dem Begleitgeräteframework kann ein Begleitgerät umfangreiche Funktionen für Microsoft Passport bereitstellen, auch wenn Windows Hello nicht verfügbar ist (beispielsweise, wenn das Windows10-Desktopgerät über keine Kamera für die Gesichtsauthentifizierung oder kein Fingerabdrucklesegerät verfügt).</td></tr>
<tr><td>[Web Account Manager](web-account-manager.md)</td><td>In diesem Artikel wird beschrieben, wie Sie AccountsSettingsPane anzeigen und Ihre App für die universelle Windows-Plattform (UWP-App) mit externen Identitätsanbietern wie Microsoft oder Facebook verbinden. Dazu verwenden Sie die neuen Web Account Manager-APIs in Windows 10. Sie erfahren, wie Sie die Berechtigung eines Benutzers für die Verwendung des Microsoft-Kontos des Benutzers anfordern können, ein Zugriffstoken erhalten und mit diesem grundlegende Vorgänge wie das Abrufen von Profildaten oder das Hochladen von Daten in das OneDrive-Verzeichnis des Benutzers durchführen können. </td></tr>
<tr><td>[Webauthentifizierungsbroker](web-authentication-broker.md)</td><td>In diesem Artikel wird erläutert, wie Ihre App eine Verbindung mit einem Onlineidentitätsanbieter herstellen kann, der Authentifizierungsprotokolle wie OpenID oder OAuth verwendet, wie Twitter, Facebook, Flickr, Instagram usw. Die [AuthenticateAsync](https://msdn.microsoft.com/library/windows/apps/br212066)-Methode sendet eine Anforderung an den Onlineidentitätsanbieter und erhält als Antwort ein Zugriffstoken, das die für die App zugänglichen Anbieterressourcen beschreibt.</td></tr>
</table>




<!--HONumber=Aug16_HO3-->


