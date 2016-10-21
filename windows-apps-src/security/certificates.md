---
title: "Einführung in Zertifikate"
description: "In diesem Artikel wird die Verwendung von Zertifikaten in Apps für die universelle Windows-Plattform (UWP) beschrieben."
ms.assetid: 4EA2A9DF-BA6B-45FC-AC46-2C8FC085F90D
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: b41fc8994412490e37053d454929d2f7cc73b6ac
ms.openlocfilehash: e46d31e2f90b9336ea19632099741c1957521578

---

# Einführung in Zertifikate


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


In diesem Artikel wird die Verwendung von Zertifikaten in UWP-Apps (Universelle Windows-Plattform) beschrieben. Mit digitalen Zertifikaten wird ein öffentlicher Schlüssel in der Kryptografie für öffentliche Schlüssel an eine Person, an einen Computer oder an eine Organisation gebunden. Die gebundenen Identitäten werden meist dazu verwendet, eine Entität für die andere zu authentifizieren. Zertifikate werden z. B. häufig dazu verwendet, einen Webserver für einen Benutzer und einen Benutzer für einen Webserver zu authentifizieren. Sie können Zertifikatanforderungen erstellen und ausgestellte Zertifikate installieren oder importieren. Außerdem können Sie ein Zertifikat in einer Zertifikathierarchie registrieren.

### Freigegebene Zertifikatspeicher

UWP-Apps verwenden das neue Isolationsanwendungsmodell, das in Windows8 eingeführt wurde. In diesem Modell werden Apps in einem Low-Level-Betriebssystemkonstrukt ausgeführt, das App-Container genannt wird. In diesem Konstrukt wird verhindert, dass die App auf Ressourcen oder Dateien zugreift, die außerhalb des eigenen Umfangs liegen, wobei der Zugriff auch ausdrücklich erlaubt und dadurch ermöglicht werden kann. In den folgenden Abschnitten werden die Auswirkungen auf die Public Key-Infrastruktur (PKI) erläutert.

### Zertifikatspeicher pro App-Container

Zertifikate, die zur Verwendung in einem bestimmten App-Container vorgesehen sind, werden an Containerspeicherorten für einzelne Benutzer und Apps gespeichert. Eine App, die in einem App-Container ausgeführt wird, hat lediglich auf ihren eigenen Zertifikatspeicher Schreibzugriff. Wenn die App einem ihrer Speicher Zertifikate hinzufügt, können diese Zertifikate von anderen Apps nicht gelesen werden. Beim Deinstallieren einer App werden auch alle zugehörigen Zertifikate entfernt. Darüber hinaus hat eine App Lesezugriff auf Zertifikatspeicher des lokalen Computers, bei denen es sich nicht um die MY- und REQUEST-Speicher handelt.

### Cache

Jeder App-Container verfügt über einen isolierten Cache, in dem er Ausstellerzertifikate für Validierungs-, CRL (Certificate Revocation Lists, Zertifikatsperrliste)- und OCSP (Online Certificate Status-Protokoll)-Antworten speichern kann.

### Freigegebene Zertifikate und Schlüssel

Wenn eine Smartcard in ein Lesegerät eingeführt wird, werden die auf der Karte enthaltenen Zertifikate und Schlüssel in den MY-Speicher des Benutzers kopiert. Von dort aus können sie von allen vertrauenswürdigen Anwendungen, die der Benutzer ausführt, freigegeben werden. Standardmäßig haben App-Cntainer jedoch keinen Zugriff auf den MY-Speicher einzelner Benutzer.

Das Isolationsmodell des App-Containers unterstützt das Funktionskonzept, um dies zu umgehen und Prinzipalgruppen den Zugriff auf Ressourcengruppen zu ermöglichen. Eine Funktion erlaubt einem App-Containerprozess den Zugriff auf eine bestimmte Ressource. Die Funktion "sharedUserCertificates" gewährt einem App-Container Lesezugriff auf die Zertifikate und Schlüssel im MY-Speicher eines Benutzers und den Speicher "Smartcard vertrauenswürdige Stämme". Die Funktion gewährt keinen Lesezugriff auf den REQUEST-Speicher des Benutzers.

Sie können die sharedUserCertificates-Funktion wie im folgenden Beispiel gezeigt im Manifest angeben.

```xml
<Capabilities>
    <Capability Name="sharedUserCertificates" />
</Capabilities>
```

## Zertifikatfelder


Der Standard für X.509-Zertifikate für öffentliche Schlüssel wurde mit der Zeit überarbeitet. In jeder nachfolgenden Version der Datenstruktur wurden die in den Vorversionen vorhandenen Felder beibehalten. Gleichzeitig wurden weitere hinzugefügt, wie in der folgenden Abbildung veranschaulicht.

![x.509-Zertifikatversionen 1, 2 und 3](images/x509certificateversions.png)

Manche dieser Felder und Erweiterungen können direkt angegeben werden, wenn Sie mit der [**CertificateRequestProperties**](https://msdn.microsoft.com/library/windows/apps/br212079)-Klasse eine Zertifikatanforderung erstellen. Die meisten sind dazu nicht in der Lage. Diese Felder können von der ausstellenden Behörde aufgefüllt oder leer gelassen werden. Weitere Informationen zu den Feldern finden Sie in den folgenden Abschnitten:

### Felder von Version 1

| Feld               | Beschreibung                                                                                                                                                                                                                                                                 |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Version             | Gibt die Versionsnummer des codierten Zertifikats an. Derzeit sind die möglichen Werte dieses Felds 0, 1 und 2.                                                                                                                                                       |
| Seriennummer       | Enthält eine positive, eindeutige Ganzzahl, die dem Zertifikat von der Zertifizierungsstelle zugewiesen wurde.                                                                                                                                                                        |
| Signaturalgorithmus | Enthält einen Objektbezeichner (OID), die den Algorithmus angibt, mit dem die Zertifizierungsstelle das Zertifikat signiert. 1.2.840.113549.1.1.5 gibt beispielsweise einen SHA-1-Hashalgorithmus an, der mit dem RSA-Verschlüsselungsalgorithmus von RSA Laboratories kombiniert ist.                            |
| Aussteller              | Enthält den X.500-Distinguished Name (DN) der Zertifizierungsstelle, die das Zertifikat erstellt und signiert hat.                                                                                                                                                                               |
| Gültigkeit            | Gibt die Zeitspanne an, für die das Zertifikat gültig ist. Für Datumsangaben bis Ende 2049 wird das Coordinated Universal Time (Greenwich Mean Time)-Format (yymmddhhmmssz) verwendet. Für Datumsangaben ab dem 1. Januar 2050 wird das generalisierte Zeitformat (yyyymmddhhmmssz) verwendet. |
| Antragsteller             | Enthält einen X.500-Distinguished Name der Entität, die dem öffentlichen Schlüssel im Zertifikat zugeordnet ist.                                                                                                                                                             |
| Öffentlicher Schlüssel          | Enthält den öffentlichen Schlüssel und Informationen zum zugehörigen Algorithmus.                                                                                                                                                                                                               |

 

### Felder von Version 2

Ein X.509-Zertifikat der Version 2 enthält die grundlegenden Felder, die in Version 1 definiert sind, sowie zusätzlich die folgenden Felder.

| Feld                     | Beschreibung                                                                                                                                         |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| Issuer Unique Identifier  | Enthält einen eindeutigen Wert, mit dem der X.500-Name der Zertifizierungsstelle eindeutig festgelegt werden kann, wenn dieser im Lauf der Zeit von verschiedenen Entitäten wiederverwendet wird.                  |
| Issuer Unique Identifier | Enthält einen eindeutigen Wert, mit dem der X.500-Name des Zertifikatantragstellers eindeutig festgelegt werden kann, wenn dieser im Lauf der Zeit von verschiedenen Entitäten wiederverwendet wird. |
 

### Erweiterungen für Version3

Ein X.509-Zertifikat der Version 3 enthält die Felder, die in Version 1 und Version 2 definiert sind, und es werden Zertifikaterweiterungen hinzugefügt.

| Feld                        | Beschreibung                                                                                                                                                                                              |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Authority Key Identifier     | Gibt den öffentlichen Schlüssel der Zertifizierungsstelle an, der dem privaten Schlüssel der Zertifizierungsstelle entspricht, mit dem das Zertifikat signiert wird.                                                                              |
| Basic Constraints            | Gibt an, ob die Entität als Zertifizierungsstelle fungieren kann, und wenn dies der Fall ist, wie viele untergeordnete Zertifizierungsstellen in der Zertifikatkette darunter vorhanden sein können.                                                           |
| Certificate Policies         | Gibt die Richtlinien an, unter denen das Zertifikat ausgestellt wurde, sowie die Zwecke, für die es genutzt werden kann.                                                                                            |
| CRL Distribution Points      | Enthält den URI der Basis-Zertifikatsperrliste (Certificate Revocation List, CRL).                                                                                                                                          |
| Enhanced Key Usage           | Gibt die Art und Weise an, in welcher der öffentliche Schlüssel im Zertifikat verwendet werden kann.                                                                                                                   |
| Issuer Alternative Name      | Gibt einen oder mehrere alternative Namen für den Aussteller der Zertifikatanforderung an.                                                                                                                  |
| Key Usage                    | Gibt die Einschränkungen für die Vorgänge an, die durch den öffentlichen Schlüssel im Zertifikat ausgeführt werden können.                                                                                           |
| Name Constraints             | Gibt den Namespace an, in dem alle Antragstellernamen in einer Zertifikathierarchie liegen müssen. Die Erweiterung wird lediglich in einem Zertifizierungsstellenzertifikat verwendet.                                                       |
| Policy Constraints           | Schränkt die Zertifikatüberprüfung ein, indem die Richtlinienzuordnung untersagt oder gefordert wird, dass jedes Zertifikat in der Hierarchie eine akzeptable Richtlinienkennung aufweisen muss. Die Erweiterung wird lediglich in einem Zertifizierungsstellenzertifikat verwendet. |
| Policy Mappings              | Gibt die Richtlinien in einer untergeordneten Zertifizierungsstelle an, die Richtlinien in der ausstellenden Zertifizierungsstelle entsprechen.                                                                                                                |
| Private Key Usage Period     | Gibt einen anderen Gültigkeitszeitraum für den privaten Schlüssel als für das Zertifikat an, dem der private Schlüssel zugeordnet ist.                                                                             |
| Subject Alternative Name     | Gibt einen oder mehrere alternative Namen für den Antragsteller der Zertifikatanforderung an. Beispiele für alternative Namen sind E-Mail-Adressen, DNS-Namen, IP-Adressen und URIs.                           |
| Subject Directory Attributes | Vermittelt identifizierende Attribute, beispielsweise die Nationalität des Zertifikatantragstellers. Der Erweiterungswert ist eine Folge von OID-Wertepaaren.                                                              |
| Subject Key Identifier       | Unterscheidet zwischen mehreren öffentlichen Schlüsseln im Besitz des Zertifikatantragstellers. Der Erweiterungswert ist normalerweise ein SHA-1-Hash des Schlüssels.                                                                   |




<!--HONumber=Aug16_HO3-->


