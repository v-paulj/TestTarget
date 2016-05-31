---
author: eliotcowley
ms.assetid: A7E0DA1E-535A-459E-9A35-68A4150EE9F5
description: Dieses Thema enthält eine Übersicht über das Hinzufügen der hardwarebasierten Verwaltung digitaler Rechte (Digital Rights Management, DRM) mit PlayReady zu Ihrer App für die universelle Windows-Plattform (UWP).
title: Hardwarebasiertes DRM
---

# Hardwarebasiertes DRM

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Dieses Thema enthält eine Übersicht über das Hinzufügen der hardwarebasierten Verwaltung digitaler Rechte (Digital Rights Management, DRM) mit PlayReady zu Ihrer App für die universelle Windows-Plattform (UWP).

**Hinweis**  Das hardwarebasierte DRM wird nur auf bestimmter Hardware mit einer Windows 10-Version der jeweiligen Hardwarefirmware unterstützt. Weitere Informationen zu den Garantien finden Sie unter [PlayReady-Kompatibilität und Stabilitätsregeln](http://www.microsoft.com/playready/licensing/compliance/).

Inhaltsanbieter verwenden zunehmend hardwarebasierten Schutz, um die Berechtigung zur Wiedergabe vollständiger hochwertiger Inhalte in Apps zu gewähren. Aus diesem Grund wurde PlayReady stabile Unterstützung für eine Hardwareimplementierung des kryptografischen Cores hinzugefügt. Diese Unterstützung ermöglicht die sichere Wiedergabe von HD (1080p)- und Ultra-High-Definition (UHD)-Inhalten auf mehreren Geräteplattformen. Schlüsselmaterial (einschließlich privater Schlüssel, Inhaltsschlüssel und anderer Schlüsselmaterialien zum Ableiten oder Entsperren dieser Schlüssel) sowie entschlüsselte komprimierte und nicht komprimierte Videobeispiele werden durch Hardwaresicherheit geschützt.

## Windows-TEE-Implementierung

Dieses Thema enthält eine kurze Übersicht über die Implementierung der vertrauenswürdigen Ausführungsumgebung (Trusted Execution Environment, TEE) in Windows 10.

Auf die Details der Windows-TEE-Implementierung gehen wir in diesem Dokument nicht ein. Eine kurze Erläuterung des Unterschieds zwischen dem Standard-TEE-Port des Porting Kits und dem Windows-Port ist jedoch sinnvoll. Windows implementiert die OEM-Proxyschicht und überträgt die serialisierten PRITEE-Funktionsaufrufe an einen Benutzermodustreiber im Windows Media Foundation-Subsystem. Diese werden letztendlich an den Windows-TrEE (Trusted Execution Environment)-Treiber oder den Grafiktreiber des OEMs weitergeleitet. Die Details dieser Ansätze werden in diesem Dokument nicht erläutert. Das folgende Diagramm zeigt die allgemeine Komponenteninteraktion für den Windows-Port. Wenn Sie eine Windows PlayReady-TEE-Implementierung entwickeln möchten, können Sie sich an <WMLA@Microsoft.com> wenden.

![Diagramm der Windows-TEE-Komponenten](images/windowsteecomponentdiagram720.jpg)

## Überlegungen zur Verwendung des hardwarebasierten DRM

Dieses Thema enthält eine kurze Liste der Punkte, die Sie beim Entwickeln von Apps mit Verwendung des Hardware-DRM berücksichtigen sollten.

-   Protected Media Process (PMP) wird nicht unterstützt.
-   Ausgabeschutzebene (Output Protection Level, OPL) 270 wird unterstützt (keine Abwärtsauflösung). Microsoft empfiehlt, dass für HD-Inhalte für das Hardware-DRM ein OPL-Wert verwendet wird, der größer als 270 ist (obwohl dies nicht zwingend erforderlich ist). Ein höherer OPL-Wert als 270 bedeutet, dass HDCP erforderlich ist. Darüber hinaus empfiehlt Microsoft, HDCP Typ 1 (Version 2.2 oder höher) zu verwenden.
-   Im Gegensatz zum DRM wird der Ausgabeschutz für alle Monitore basierend auf dem langsamsten Monitor erzwungen. Wenn der Benutzer beispielsweise zwei Monitore angeschlossen hat und nur einer davon HDCP unterstützt, ist die Wiedergabe nicht möglich, falls die Lizenz HDCP erfordert. Dies gilt auch dann, wenn der Inhalt nur auf dem Monitor gerendert wird, der HDCP unterstützt. Beim Software-DRM wird der Inhalt wiedergegeben, solange das Rendering nur auf dem Monitor erfolgt, der HDCP unterstützt.
-   Es ist nicht garantiert, dass das Hardware-DRM vom Client verwendet wird und dass das Verfahren sicher ist, es sei denn, von den Inhaltsschlüsseln und -lizenzen werden die folgenden Bedingungen erfüllt:
    -   Die Audiodaten müssen unverschlüsselt oder mit einem anderen Inhaltsschlüssel als die Videodaten verschlüsselt sein. Microsoft empfiehlt, die Audiodaten unverschlüsselt zu lassen, um die Wiedergabeleistung zu verbessern.
    -   Die für den Videoinhaltsschlüssel verwendete Lizenz muss die Sicherheitsstufe 3.000 aufweisen.
-   Mehrere Grafikprozessoren (GPUs) werden für persistente Lizenzen nicht unterstützt.

Sehen Sie sich das folgende Szenario an, bei dem es um die Behandlung von persistenten Lizenzen auf Computern mit mehreren GPUs geht:

1.  Ein Kunde kauft einen neuen Computer mit einer integrierten Grafikkarte.
2.  Der Kunde verwendet eine App, über die persistente Lizenzen beschafft werden, während Hardware-DRM verwendet wird.
3.  Die persistente Lizenz ist jetzt an die Hardwareschlüssel dieser Grafikkarte gebunden.
4.  Der Kunde installiert dann eine neue Grafikkarte.
5.  Alle Lizenzen im Datenspeicher mit Hash (HDS) sind an die integrierte Grafikkarte gebunden, aber der Kunde möchte jetzt geschützte Inhalte mit der neu installierten Grafikkarte wiedergeben.

Um Fehler bei der Wiedergabe zu verhindern, weil die Lizenzen von der Hardware nicht entschlüsselt werden können, nutzt PlayReady für jede Grafikkarte, die erkannt wird, einen separaten Datenspeicher mit Hash (HDS). Daher versucht PlayReady, eine Lizenz für ein Inhaltselement zu beschaffen, für das PlayReady normalerweise bereits über eine Lizenz verfügen würde (beim Software-DRM oder in einem Fall ohne Wechsel der Hardware müsste PlayReady also nicht erneut eine Lizenz beschaffen). Falls die App eine persistente Lizenz beschafft, während Hardware-DRM genutzt wird, muss Ihre App also den Fall behandeln können, in dem diese Lizenz praktisch als „verloren“ gilt, wenn der Endbenutzer eine Grafikkarte installiert (oder deinstalliert). Da dies nicht ein häufiger Fall ist, können Sie sich auch für die Bearbeitung der Supportanfragen entscheiden, wenn Inhalte nach einem Wechsel der Hardware nicht mehr wiedergegeben werden können, anstatt nach einer Lösung für den Umgang mit einem Hardwarewechsel im Client-/Servercode zu suchen.

## Außerkraftsetzen des Hardware-DRM

In diesem Abschnitt wird beschrieben, wie Sie das Hardware-DRM außer Kraft setzen, wenn der wiederzugebende Inhalt das Hardware-DRM nicht unterstützt.

Das Hardware-DRM wird standardmäßig verwendet, wenn es vom System unterstützt wird. Manche Inhalte werden jedoch nicht vom Hardware-DRM unterstützt. Ein Beispiel hierfür sind Cocktail-Inhalte. Ein weiteres Beispiel sind Inhalte, die einen anderen Videocodec als H.264 und HEVC verwenden. HEVC-Inhalte sind ebenfalls ein Beispiel, da HEVC nicht von allen Hardware-DRM-Typen unterstützt wird. Wenn Sie einen bestimmten Inhalt wiedergeben möchten, der auf dem betreffenden System nicht vom Hardware-DRM unterstützt wird, können Sie das Hardware-DRM daher außer Kraft setzen.

Das folgende Beispiel zeigt, wie Sie das Hardware-DRM außer Kraft setzen. Diesen Schritt müssen Sie nur vor dem Wechseln ausführen. Stellen Sie außerdem sicher, dass kein PlayReady-Objekt im Speicher vorhanden ist. Andernfalls kann es zu unerwartetem Verhalten kommen.

```js
var applicationData = Windows.Storage.ApplicationData.current;
var localSettings = applicationData.localSettings.createContainer("PlayReady", Windows.Storage.ApplicationDataCreateDisposition.always);
localSettings.values["SoftwareOverride"] = 1;
```

Wenn Sie das Hardware-DRM wieder verwenden möchten, legen Sie den **SoftwareOverride**-Wert auf **0** fest.

**MediaProtectionManager** muss für jede Medienwiedergabe wie folgt festgelegt werden:

```js
mediaProtectionManager.properties["Windows.Media.Protection.UseSoftwareProtectionLayer"] = true;
```

Die beste Möglichkeit, herauszufinden, ob Sie ein hardwarebasiertes DRM oder ein softwarebasiertes DRM verwenden, besteht darin, C:\\Users\\&lt;username&gt;\\AppData\\Local\\Packages\\&lt;application name&gt;\\LocalState\\PlayReady\\\* zu betrachten.

-   Wenn eine Datei mit der Erweiterung „mspr.hds“ vorhanden ist, verwenden Sie ein softwarebasiertes DRM.
-   Ist eine weitere Datei mit der Erweiterung „\*.hds“ vorhanden, ist Hardware-DRM ausgewählt.
-   Sie können auch den gesamten PlayReady-Ordner löschen und den Test wiederholen.

## Erkennen des Hardware-DRM-Typs

In diesem Abschnitt wird beschrieben, wie Sie den auf dem System unterstützten Hardware-DRM-Typ erkennen.

Sie können die [**PlayReadyStatics.CheckSupportedHardware**](https://msdn.microsoft.com/library/windows/apps/dn986441)-Methode verwenden, um festzustellen, ob das System eine bestimmte Hardwarefunktion für die Verwaltung digitaler Rechte (Digital Rights Management, DRM) unterstützt. Beispiel:

```cpp
boolean PlayReadyStatics->CheckSupportedHardware(PlayReadyHardwareDRMFeatures enum);
```

Die [**PlayReadyHardwareDRMFeatures**](https://msdn.microsoft.com/library/windows/apps/dn986265)-Enumeration enthält die Liste gültige Hardware-DRM-Featurewerte, die abgefragt werden können. Um festzustellen, ob das Hardware-DRM unterstützt wird, verwenden Sie den **HardwareDRM**-Member in der Abfrage. Um festzustellen, ob die Hardware den High Efficiency Video Coding (HEVC)/H.265-Codec unterstützt, verwenden Sie den **HEVC**-Member in der Abfrage.

Sie können auch die [**PlayReadyStatics.PlayReadyCertificateSecurityLevel**](https://msdn.microsoft.com/library/windows/apps/windows.media.protection.playready.playreadystatics.playreadycertificatesecuritylevel.aspx)-Eigenschaft zum Abrufen der Sicherheitsstufe des Clientzertifikats verwenden, um festzustellen, ob das Hardware-DRM unterstützt wird. Sofern die zurückgegebene Zertifikatssicherheitsstufe nicht größer oder gleich 3000 ist, ist entweder der Client nicht individualisiert oder bereitgestellt (in diesem Fall gibt die Eigenschaft 0 zurück) oder das Hardware-DRM wird nicht verwendet (in diesem Fall gibt die Eigenschaft einen Wert kleiner 3000 zurück).



<!--HONumber=May16_HO2-->


