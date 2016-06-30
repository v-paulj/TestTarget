---
title: Kryptografie
description: "Der Artikel enthält eine Übersicht über die für universelle Windows-Plattform (UWP)-Apps verfügbaren Kryptografiefeatures. Ausführliche Informationen zu bestimmten Aufgaben finden Sie in der Tabelle am Ende dieses Artikels."
ms.assetid: 9C213036-47FD-4AA4-99E0-84006BE63F47
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: b41fc8994412490e37053d454929d2f7cc73b6ac
ms.openlocfilehash: b8cccf54e414de084c5b3fd080007b225b9a9b12

---

# Kryptografie


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Der Artikel enthält eine Übersicht über die für universelle Windows-Plattform (UWP)-Apps verfügbaren Kryptografiefeatures. Ausführliche Informationen zu bestimmten Aufgaben finden Sie in der Tabelle am Ende dieses Artikels.

## Terminologie


Die folgende Terminologie wird bei der Kryptografie und bei Public Key-Infrastrukturen (PKI) häufig verwendet.

| Begriff                        | Beschreibung                                                                                                                                                                                           |
|-----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Verschlüsselung                  | Das Verfahren zum Transformieren von Daten mithilfe von Kryptografiealgorithmus und -schlüssel. Die transformierten Daten können nur mithilfe desselben Algorithmus und desselben (symmetrischen) oder zugehörigen (öffentlichen) Schlüssels wiederhergestellt werden. |
| Entschlüsselung                  | Das Verfahren zur Rückführung verschlüsselter Daten in ihre ursprüngliche Form.                                                                                                                                         |
| Klartext                   | Bezog sich ursprünglich auf eine nicht verschlüsselte Textnachricht. Wird heute für jegliche Form unverschlüsselter Daten verwendet.                                                                                                         |
| Chiffretext                  | Bezog sich ursprünglich auf eine verschlüsselte und daher nicht lesbare Textnachricht. Wird heute für jegliche Form verschlüsselter Daten verwendet.                                                                                  |
| Hashing                     | Das Verfahren zum Umwandeln von Daten mit variabler Länge in einen Wert mit einer bestimmten Länge, der in der Regel kleiner ist. Indem Sie Hashes vergleichen, können Sie sich mit hoher Zuverlässigkeit versichern, dass zwei oder mehr Daten übereinstimmen.            |
| Signatur                   | Verschlüsselter Hash digitaler Daten, der in der Regel zur Authentifizierung des Absenders von Daten oder zur Sicherstellung genutzt wird, dass die Daten während der Übertragung nicht manipuliert wurden.                                               |
| Algorithmus                   | Eine schrittweise Prozedur zur Verschlüsselung von Daten.                                                                                                                                                         |
| Schlüssel                         | Eine zufällige oder Pseudozufallszahl, die als Eingabe für einen Kryptografiealgorithmus zur Verschlüsselung und Entschlüsselung von Daten verwendet wird.                                                                                               |
| Kryptografie mit symmetrischem Schlüssel  | Kryptografie, bei der zur Ver- und Entschlüsselung derselbe Schlüssel verwendet wird. Auch bekannt als Kryptografie mit geheimem Schlüssel.                                                                                      |
| Kryptografie mit asymmetrischem Schlüssel | Kryptografie, bei der zur Ver- und Entschlüsselung verschiedene, mathematisch jedoch in Beziehung stehende Schlüssel verwendet werden. Auch bekannt als Kryptografie mit öffentlichem Schlüssel.                                                          |
| Codierung                    | Das Verfahren zur Codierung von digitalen Nachrichten, einschließlich Zertifikaten, für die Übertragung über ein Netzwerk.                                                                                                     |
| Algorithmusanbieter          | Eine DLL-Datei, die einen Kryptografiealgorithmus implementiert.                                                                                                                                                      |
| Schlüsselspeicheranbieter        | Ein Container zum Speichern von Schlüsselmaterial. Derzeit können Schlüssel in Software, Smartcards und dem Trusted Platform Module (TPM) gespeichert werden.                                                                   |
| X.509-Zertifikat           | Ein digitales Dokument, das in der Regel von einer Zertifizierungsstelle ausgegeben wird, um die Identität einer Person, eines Systems oder einer Entität für andere interessierte Parteien zu überprüfen.                                            |

 
## Namespaces

Die folgenden Namespaces stehen für die Verwendung in einer App zur Verfügung:

### Windows.Security.Cryptography

Enthält die Klasse "CryptographicBuffer" und statische Methoden, die Ihnen Folgendes ermöglichen:

-   Konvertieren von Daten in und aus Zeichenfolgen
-   Konvertieren von Daten in und aus Bytearrays
-   Codieren von Nachrichten zur Netzwerkübertragung
-   Codieren von Nachrichten nach der Übertragung

### Windows.Security.Cryptography.Certificates

Enthält Klassen, Schnittstellen und Enumerationstypen, die Ihnen Folgendes ermöglichen:

-   Erstellen einer Zertifikatanforderung
-   Installieren einer Zertifikatantwort
-   Importieren eines Zertifikats in einer PFX-Datei
-   Angeben und Abrufen von Zertifikatanforderungseigenschaften

### Windows.Security.Cryptography.Core

Enthält Klassen und Enumerationstypen, die Ihnen Folgendes ermöglichen:

-   Verschlüsseln und Entschlüsseln von Daten
-   Hashing von Daten
-   Signieren von Daten und Überprüfen von Signaturen
-   Erstellen, Importieren und Exportieren von Schlüsseln
-   Arbeit mit Anbietern von Algorithmen asymmetrischer Schlüssel
-   Arbeit mit Anbietern von Algorithmen symmetrischer Schlüssel
-   Arbeit mit Anbietern von Hashalgorithmen
-   Arbeit mit MAC (Machine Authentication Code)-Algorithmusanbietern
-   Arbeit mit Anbietern von Schlüsselableitungsalgorithmen

### Windows.Security.Cryptography.DataProtection

Enthält Klassen, die Ihnen Folgendes ermöglichen:

-   Asynchrone Verschlüsselung und Entschlüsselung statischer Daten
-   Asynchrone Verschlüsselung und Entschlüsselung von Datenströmen

## Krypto- und PKI-Anwendungsfunktionen


Die vereinfachte Schnittstelle für die Anwendungsprogrammierung, die für Apps verfügbar ist, bietet folgende Kryptografie- und PKI-Funktionen (Public Key Interface):

### Kryptografieunterstützung

Sie können folgende Kryptografieaufgaben ausführen. Weitere Informationen finden Sie im [**Windows.Security.Cryptography.Core**](https://msdn.microsoft.com/library/windows/apps/br241547) -Namespace.

-   Erstellen symmetrischer Schlüssel
-   Ausführen der symmetrischen Verschlüsselung
-   Erstellen asymmetrischer Schlüssel
-   Ausführen der asymmetrischen Verschlüsselung
-   Ableiten kennwortbasierter Schlüssel
-   Erstellen von Nachrichtenauthentifizierungscodes (MACs)
-   Hashinhalt
-   Digitales Signieren von Inhalt

Das SDK enthält außerdem eine vereinfachte Schnittstelle für kennwortbasierten Datenschutz. Diese können Sie für folgende Aufgaben verwenden. Weitere Informationen finden Sie im [**Windows.Security.Cryptography.DataProtection**](https://msdn.microsoft.com/library/windows/apps/br241585) -Namespace.

-   Asynchroner Schutz statischer Daten
-   Asynchroner Schutz eines statischen Datenstroms

### Codierungsunterstützung

Eine App kann kryptografische Daten für die Übertragung in einem Netzwerk codieren und Daten decodieren, die aus einer Netzwerkquelle empfangen wurden. Weitere Informationen finden Sie unter den statischen Methoden, die im [**Windows.Security.Cryptography**](https://msdn.microsoft.com/library/windows/apps/br241404) -Namespace verfügbar sind.

### PKI-Unterstützung

Apps können folgende PKI-Aufgaben ausführen. Weitere Informationen finden Sie im [**Windows.Security.Cryptography.Certificates**](https://msdn.microsoft.com/library/windows/apps/br241476) -Namespace.

-   Erstellen eines Zertifikats
-   Erstellen eines selbstsignierten Zertifikats
-   Installieren einer Zertifikatantwort
-   Importieren eines Zertifikats im PFX-Format
-   Verwenden von Smartcardzertifikaten und -schlüsseln (sharedUserCertificates-Funktion ist festgelegt)
-   Verwenden von Zertifikaten aus dem MY-Speicher des Benutzers (sharedUserCertificates-Funktion ist festgelegt)

Sie können das Manifest außerdem für folgende Aktionen verwenden:

-   Angeben von anwendungsspezifisch vertrauenswürdigen Stammzertifikaten
-   Angeben von anwendungsspezifisch für Peers vertrauenswürdigen Zertifikaten
-   Explizites Deaktivieren der Vererbung von der Systemvertrauensstellung
-   Angeben von Kriterien für die Zertifikatauswahl
    -   Nur Hardwarezertifikate
    -   Mit einem angegebenen Satz an Ausstellern verkettete Zertifikate
    -   Automatisches Auswählen eines Zertifikats aus dem Anwendungsspeicher

## Detaillierte Artikel


Die folgenden Artikel enthalten weitere Informationen zu Sicherheitsszenarien:

| Thema                                                                         | Beschreibung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|-------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Zertifikate](certificates.md)                                               | In diesem Artikel wird die Verwendung von Zertifikaten in UWP-Apps beschrieben. Mit digitalen Zertifikaten wird in der Kryptografie für öffentliche Schlüssel ein öffentlicher Schlüssel an eine Person, einen Computer oder eine Organisation gebunden. Die gebundenen Identitäten werden meist dazu verwendet, eine Entität für die andere zu authentifizieren. Zertifikate werden z. B. häufig dazu verwendet, einen Webserver für einen Benutzer und einen Benutzer für einen Webserver zu authentifizieren. Sie können Zertifikatanforderungen erstellen und ausgestellte Zertifikate installieren oder importieren. Außerdem können Sie ein Zertifikat in einer Zertifikathierarchie registrieren. |
| [Kryptografische Schlüssel](cryptographic-keys.md)                                   | In diesem Artikel wird erläutert, wie Sie mithilfe standardmäßiger Schlüsselableitungsfunktionen Schlüssel ableiten und wie Sie Inhalte mithilfe symmetrischer und asymmetrischer Schlüssel verschlüsseln können.                                                                                                                                                                                                                                                                                                                                                                             |
| [Datenschutz](data-protection.md)                                         | In diesem Artikel wird erläutert, wie Sie mithilfe der [DataProtectionProvider](https://msdn.microsoft.com/library/windows/apps/br241559)-Klasse im [Windows.Security.Cryptography.DataProtection](https://msdn.microsoft.com/library/windows/apps/br241585)-Namespace digitale Daten in einer UWP-App verschlüsseln und entschlüsseln können.                                                                                                                                                                                                                  |
| [MACs, Hashes und Signaturen](macs-hashes-and-signatures.md)               | In diesem Artikel wird erläutert, wie Nachrichtenmanipulationen mithilfe von Nachrichtenauthentifizierungscodes (Message Authentication Codes, MACs), Hashes und Signaturen in UWP-Apps erkannt werden können.                                                                                                                                                                                                                                                                                                                                                                                |
| [Exporteinschränkungen hinsichtlich Kryptografie](export-restrictions-on-cryptography.md) | Anhand der Informationen in diesem Abschnitt können Sie ermitteln, ob Ihre App Kryptografiefunktionen in einer Weise verwendet, die unter Umständen dazu führt, dass sie im Windows Store nicht angezeigt wird.                                                                                                                                                                                                                                                                                                                                                                                            |
| [Allgemeine Kryptografieaufgaben](common-cryptography-tasks.md)                     | Die folgenden Artikel enthalten Beispielcode für allgemeine UWP-Kryptografieaufgaben, z. B. Erstellen zufälliger Zahlen, Vergleichen von Puffern, Konvertieren zwischen Zeichenfolgen und binären Daten, Kopieren in und aus Bytearrays sowie Codieren und Decodieren von Daten.                                                                                                                                                                                                                                                                                    |

 


<!--HONumber=Jun16_HO4-->


