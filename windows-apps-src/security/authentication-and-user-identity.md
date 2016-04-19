---
title: Authentifizierung und Benutzeridentität
description: Apps für die universelle Windows-Plattform (UWP) bieten mehrere Optionen für die Benutzerauthentifizierung – vom einfachen einmaligen Anmelden (Single Sign-On, SSO) mit einem Webauthentifizierungsbroker bis hin zur äußerst sicheren zweistufigen Authentifizierung.
ms.assetid: 53E36DDC-200A-4850-AAF0-07ECA3662BB9
author: awkoren
---

# Authentifizierung und Benutzeridentität


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

UWP (Universelle Windows-Plattform)-Apps bieten mehrere Optionen für die Benutzerauthentifizierung, vom einfachen einmaligen Anmelden (Single Sign-On, SSO) mit einem [Webauthentifizierungsbroker](web-authentication-broker.md) bis hin zur äußerst sicheren zweistufigen Authentifizierung.

Normale App-Verbindungen mit Identitätsanbieterdiensten von Drittanbietern, z. B. Facebook, Twitter, Flickr usw., verwenden den [Webauthentifizierungsbroker](web-authentication-broker.md). Verwenden Sie das [Schließfach für Anmeldeinformationen](credential-locker.md), um die Benutzerfreundlichkeit beim Speichern und Roaming der Anmeldeinformationen des Benutzers zu verbessern.

Für Unternehmen, die Windows 10 nutzen, empfiehlt es sich unbedingt, [Microsoft Passport und Windows Hello](microsoft-passport.md) zu verwenden, um eine äußerst sichere zweistufige Authentifizierung zu erzielen. Wenn Microsoft Passport nicht verwendet werden kann, bieten [Smartcards](smart-cards.md) und der [biometrische Fingerabdruck](fingerprint-biometrics.md) eine zusätzliche Sicherheitsstufe.

| Thema                                                                                 | Beschreibung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|---------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Schließfach für Anmeldeinformationen](credential-locker.md)                                             | In diesem Artikel wird beschrieben, wie Apps mit dem Schließfach für Anmeldeinformationen Benutzeranmeldeinformationen sicher speichern und abrufen können und sie per Roaming mit dem Microsoft-Konto des Benutzers zwischen Geräten übertragen können. |
| [Biometrischer Fingerabdruck](fingerprint-biometrics.md)                                   | In diesem Artikel wird erläutert, wie Sie Ihrer App einen biometrischen Fingerabdruck hinzufügen. Sie können die Sicherheit Ihrer App erhöhen, indem Sie die Anforderung einer Authentifizierung per Fingerabdruck integrieren, wenn die Zustimmung des Benutzers für eine bestimmte Aktion erforderlich ist. So können Sie beispielsweise die Authentifizierung per Fingerabdruck vor der Autorisierung eines In-App-Kaufs oder vor dem Zugriff auf eingeschränkte Ressourcen anfordern. Die Verwaltung der Authentifizierung per Fingerabdruck erfolgt mithilfe der [UserConsentVerifier](https://msdn.microsoft.com/library/windows/apps/dn279134)-Klasse im [Windows.Security.Credentials.UI](https://msdn.microsoft.com/library/windows/apps/hh701356)-Namespace. |
| [Microsoft Passport und Windows Hello](microsoft-passport.md)                         | In diesem Artikel wird die neue Microsoft Passport-Technologie von Windows 10 beschrieben und erörtert, wie Entwickler diese Technologie implementieren, um ihre Apps und Back-End-Dienste zu schützen. Darin werden die spezifischen Funktionen dieser Technologien hervorgehoben, die dabei helfen, aus herkömmlichen Anmeldeinformationen erwachsende Bedrohungen zu mindern. Darüber hinaus bietet sie eine Anleitung darüber, wie diese Technologien als Bestandteil Ihrer Windows 10-Einführung entworfen und bereitgestellt werden. |
| [Erstellen einer Microsoft Passport-Anmelde-App](microsoft-passport-login.md)                  | Teil 1 der umfassenden exemplarischen Vorgehensweise zum Erstellen einer Windows 10-App für die universelle Windows-Plattform (UWP), die Microsoft Passport als Alternative zu herkömmlichen Authentifizierungssystemen mit Benutzername und Kennwort verwendet. |
| [Erstellen eines Microsoft Passport-Anmeldediensts](microsoft-passport-login-auth-service.md) | Teil 2 der umfassenden exemplarischen Vorgehensweise zum Verwenden von Microsoft Passport als Alternative zu herkömmlichen Authentifizierungssystemen mit Benutzername und Kennwort in Windows 10-Apps für die universelle Windows-Plattform (UWP). |
| [Smartcards](smart-cards.md)                                                         | In diesem Thema wird erläutert, wie Apps Smartcards verwenden können, um Benutzer mit sicheren Netzwerkdiensten zu verbinden. Hier finden Sie auch Informationen zum Zugriff auf physische Smartcardleser, zum Erstellen virtueller Smartcards, zum Kommunizieren mit Smartcards, zum Authentifizieren von Benutzern, zum Zurücksetzen von Benutzer-PINs und zum Entfernen oder Trennen von Smartcards. |
| [Freigabe von Zertifikaten zwischen Apps](share-certificates.md)                              | UWP-Apps, die über eine Kombination aus Benutzer-ID und Kennwort hinaus eine sichere Authentifizierung benötigen, können Zertifikate für die Authentifizierung verwenden. Die Zertifikatauthentifizierung bietet eine hohe Vertrauenswürdigkeit bei der Benutzerauthentifizierung. Es kann vorkommen, dass eine Gruppe von Diensten einen Benutzer für mehrere Apps authentifizieren möchte. In diesem Artikel wird veranschaulicht, wie Sie mehrere Apps mit dem gleichen Zertifikat authentifizieren und für einen Benutzer geeigneten Code zum Importieren eines Zertifikats bereitstellen, das für den Zugriff auf sichere Webdienste bestimmt ist. |
| [Entsperren von Windows mit Begleitgeräten](companion-device-unlock.md)   | Ein Begleitgerät ist ein Gerät, das in Verbindung mit dem Windows 10-Desktopgerät zur Verbesserung der Benutzerauthentifizierung verwendet werden kann. Mit dem Begleitgeräteframework kann ein Begleitgerät umfangreiche Funktionen für Microsoft Passport bereitstellen, auch wenn Windows Hello nicht verfügbar ist (beispielsweise, wenn das Windows 10-Desktopgerät über keine Kamera für die Gesichtsauthentifizierung oder über kein Fingerabdrucklesegerät verfügt). | 
| [Webauthentifizierungsbroker](web-authentication-broker.md)                             | In diesem Artikel wird erläutert, wie Ihre App eine Verbindung mit einem Onlineidentitätsanbieter herstellen kann, der Authentifizierungsprotokolle wie OpenID oder OAuth verwendet (beispielsweise Twitter, Facebook, Flickr, Instagram usw.). Die [AuthenticateAsync](https://msdn.microsoft.com/library/windows/apps/br212066)-Methode sendet eine Anforderung an den Onlineidentitätsanbieter und erhält als Antwort ein Zugriffstoken, das die für die App zugänglichen Anbieterressourcen beschreibt. |


<!--HONumber=Mar16_HO5-->

