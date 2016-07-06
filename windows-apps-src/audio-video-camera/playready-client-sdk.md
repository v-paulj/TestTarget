---
author: eliotcowley
ms.assetid: DD8FFA8C-DFF0-41E3-8F7A-345C5A248FC2
description: "In diesem Abschnitt wird beschrieben, wie Sie Ihrer UWP-App (Universelle Windows-Plattform) PlayReady-geschützte Medieninhalte hinzufügen."
title: PlayReady DRM
translationtype: Human Translation
ms.sourcegitcommit: 5cae0870142282eaf2f3db05e0e202db7e74ef26
ms.openlocfilehash: eef128afc0da6f55a76b8c664f9049dc1ec48da1

---

# PlayReady DRM

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


In diesem Abschnitt wird beschrieben, wie Sie Ihrer UWP-App (Universelle Windows-Plattform) PlayReady-geschützte Medieninhalte hinzufügen.

PlayReady DRM ermöglicht Entwicklern das Erstellen von UWP-Apps, die PlayReady-geschützte Inhalte für den Benutzer bereitstellen und gleichzeitig vom Inhaltsanbieter definierte Regeln erzwingen können. In diesem Abschnitt werden die Änderungen beschrieben, die für Windows 10 am Microsoft PlayReady DRM vorgenommen wurden. Außerdem wird erläutert, wie Sie Ihre UWP-App mit PlayReady ändern, um die Änderungen in der Windows 10-Version (gegenüber der Windows 8.1-Version) zu unterstützen.
 
| Thema                                                                     | Beschreibung                                                                                                                                                                                                                                                                             |
|---------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Hardware-DRM](hardware-drm.md)                                           | Dieses Thema enthält eine Übersicht über das Hinzufügen der hardwarebasierten Verwaltung digitaler Rechte (Digital Rights Management, DRM) mit PlayReady zu Ihrer UWP-App.                                                                                                                                                                 |
| [Adaptives Streaming mit PlayReady](adaptive-streaming-with-playready.md) | In diesem Artikel wird beschrieben, wie Sie einer UWP-App (Universelle Windows-Plattform) adaptives Streaming von Multimediainhalten mit Microsoft PlayReady-Inhaltsschutz hinzufügen. Dieses Feature unterstützt derzeit die Wiedergabe von HTTP Live Streaming (HLS)- und Dynamic Streaming over HTTP (DASH)-Inhalten. |

## Neuigkeiten bei PlayReady DRM

In der folgenden Liste werden die neuen Features und Änderungen von PlayReady DRM unter Windows 10 beschrieben.

-   Hardwarebasierte Verwaltung digitaler Rechte (Hardware Digital Rights Management, HWDRM) wurde hinzugefügt.

    Die Unterstützung für hardwarebasierten Inhaltsschutz ermöglicht die sichere Wiedergabe von High-Definition (HD)- und Ultra-High-Definition (UHD)-Inhalten auf mehreren Geräteplattformen. Schlüsselmaterial (einschließlich privater Schlüssel, Inhaltsschlüssel und anderer Schlüsselmaterialien zum Ableiten oder Entsperren dieser Schlüssel) sowie entschlüsselte komprimierte und nicht komprimierte Videobeispiele werden durch Hardwaresicherheit geschützt. Bei Verwendung des Hardware-DRM sind unbekannte Aktivierungen (Wiedergabe auf unbekannter Ausgabe/Wiedergabe auf unbekannter Ausgabe mit „downres”) irrelevant, da die HWDRM-Pipeline die verwendete Ausgabe immer kennt. Weitere Informationen finden Sie unter [Hardware-DRM](hardware-drm.md).

-   PlayReady ist keine AppX-Framework-Komponente mehr, sondern stattdessen eine integrierte Betriebssystemkomponente. Der Namespace wurde von **Microsoft.Media.PlayReadyClient** in [**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454) geändert.
-   Die folgenden Header zur Definition der PlayReady-Fehlercodes sind jetzt Teil des Windows Software Development Kit (SDK): „Windows.Media.Protection.PlayReadyErrors.h” und „Windows.Media.Protection.PlayReadyResults.h”.
-   Unterstützt das proaktive Abrufen nicht persistenter Lizenzen.

    Das proaktive Abrufen nicht persistenter Lizenzen wurde von Vorgängerversionen von PlayReady DRM nicht unterstützt. Diese Funktion wurde in dieser Version hinzugefügt. Mit ihrer Hilfe kann die Zeit bis zum ersten Frame verkürzt werden. Weitere Informationen finden Sie unter [Proaktives Abrufen einer nicht persistenten Lizenz vor der Wiedergabe](#proactively_acquire_a_non_persistent_license_before_playback).

-   Unterstützt das Abrufen mehrerer Lizenzen in einer Nachricht.

    Ermöglicht der Client-App das Abrufen mehrerer nicht persistenter Lizenzen in einer Lizenzabrufnachricht. Da Lizenzen für mehrere Inhalte abgerufen werden, während der Benutzer noch die Inhaltsbibliothek durchsucht, kann dies die Zeit bis zum ersten Frame verkürzen. Diese Funktion verhindert, dass beim Abrufen der Lizenz eine Verzögerung erfolgt, wenn der Benutzer die wiederzugebenden Inhalte auswählt. Darüber hinaus können Audio- und Videodatenströme verschlüsselt werden, um Schlüssel zu trennen, indem ein Inhaltsheader aktiviert wird, der mehrere Schlüsselkennungen (KIDs) enthält. Dadurch können mit einem einzigen Lizenzabruf alle Lizenzen für alle Datenströme in einer Inhaltsdatei abgerufen werden, anstatt zu diesem Zweck benutzerdefinierte Logik und mehrere Lizenzabrufanforderungen verwenden zu müssen.

-   Unterstützung für den Echtzeitablauf, d. h. einer Lizenz mit begrenzter Dauer (Limited Duration License, LDL), wurde hinzugefügt.

    Bietet die Möglichkeit, einen Echtzeitablauf für Lizenzen festzulegen und während der Wiedergabe einen reibungslosen Wechsel zwischen einer ablaufenden Lizenz und einer anderen (gültigen) Lizenz sicherzustellen. In Kombination mit dem Abruf mehrerer Lizenzen in einer Nachricht kann eine App so mehrere LDLs asynchron abrufen, während der Benutzer noch die Inhaltsbibliothek durchsucht, und nur eine Lizenz mit längerer Dauer abrufen, sobald der Benutzer den wiederzugebenden Inhalt ausgewählt hat. Die Wiedergabe wird dann schneller gestartet (weil bereits eine Lizenz verfügbar ist), und da die App beim Ablauf der LDL bereits eine Lizenz mit längerer Dauer abgerufen hat, wird die Wiedergabe unterbrechungsfrei bis zum Ende des Inhalts fortgesetzt.

-   Nicht persistente Lizenzketten wurden hinzugefügt.
-   Unterstützung für zeitbasierte Einschränkungen (einschließlich Ablauf, Ablauf nach der ersten Wiedergabe und Echtzeitablauf) für nicht persistente Lizenzen wurde hinzugefügt.
-   Richtlinienunterstützung für HDCP Typ 1 (Version 2.2 unter Windows 10) wurde hinzugefügt.

    Weitere Informationen finden Sie unter [Zu berücksichtigende Informationen](#things_to_consider).

-   Miracast ist jetzt als Ausgabe implizit.
-   Sicheres Beenden wurde hinzugefügt.

    Dank des sicheren Beendens kann ein PlayReady-Gerät einem Medienstreamingdienst zuverlässig bestätigen, dass die Medienwiedergabe für einen bestimmten Inhalt beendet wurde. Mit dieser Funktion wird sichergestellt, dass Ihre Medienstreamingdienste präzise Erzwingung und Berichte zu Nutzungseinschränkungen auf verschiedenen Geräten für ein bestimmtes Konto bereitstellen.

-   Audio- und Videolizenztrennung wurde hinzugefügt.

    Separate Spuren verhindern, dass Videos als Audio decodiert werden. Dadurch wird ein stabilerer Inhaltsschutz gewährleistet. Neue Standards erfordern separate Schlüssel für Audio- und Videospuren.

-   MaxResDecode-Feature hinzugefügt.

    Dieses Feature wurde hinzugefügt, um die Wiedergabe von Inhalten auf eine maximale Auflösung zu beschränken, auch wenn der Benutzer einen leistungsfähigeren Schlüssel (aber keine Lizenz) besitzt. Dadurch werden Szenarien unterstützt, in denen mehrere Datenstromgrößen mit einem einzelnen Schlüssel codiert werden.

PlayReady DRM wurden die folgenden neuen Schnittstellen, Klassen und Enumerationen hinzugefügt:

-   [
              **IPlayReadyLicenseAcquisitionServiceRequest**
            ](https://msdn.microsoft.com/library/windows/apps/dn986077)-Schnittstelle
-   [
              **IPlayReadyLicenseSession**
            ](https://msdn.microsoft.com/library/windows/apps/dn986080)-Schnittstelle
-   [
              **IPlayReadySecureStopServiceRequest**
            ](https://msdn.microsoft.com/library/windows/apps/dn986090)-Schnittstelle
-   [
              **PlayReadyLicenseSession**
            ](https://msdn.microsoft.com/library/windows/apps/dn986309)-Klasse
-   [
              **PlayReadySecureStopIterable**
            ](https://msdn.microsoft.com/library/windows/apps/dn986371)-Klasse
-   [
              **PlayReadySecureStopIterator**
            ](https://msdn.microsoft.com/library/windows/apps/dn986375)-Klasse
-   [
              **PlayReadyHardwareDRMFeatures**
            ](https://msdn.microsoft.com/library/windows/apps/dn986265)-Enumerator

Es wurde ein neues Beispiel erstellt, um die Verwendung der neuen Features von PlayReady DRM zu veranschaulichen. Das Beispiel kann unter [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670) heruntergeladen werden.

## Zu berücksichtigende Informationen

-   PlayReady DRM unterstützt jetzt HDCP Typ 1 (unterstützt in HDCP-Version 2.1 oder höher). PlayReady enthält in der Lizenz eine zu erzwingende HDCP-Typeinschränkungsrichtlinie für das Gerät. Unter Windows 10 erzwingt diese Richtlinie die Einbindung von HDCP 2.2 oder höher. Dieses Feature kann in der PlayReady Server v3.0 SDK-Lizenz aktiviert werden. (Der Server steuert diese Richtlinie in der Lizenz mit der GUID der HDCP-Typeinschränkung). Weitere Informationen finden Sie unter [PlayReady-Kompatibilität und Stabilitätsregeln](http://www.microsoft.com/playready/licensing/compliance/).
-   Windows Media Video (auch bekannt als VC-1) wird nicht vom Hardware-DRM unterstützt (siehe [Außerkraftsetzen des Hardware-DRM](hardware-drm.md#override-hardware-drm)).
-   PlayReady DRM unterstützt jetzt den High Efficiency Video Coding (HEVC/H.265)-Videokomprimierungsstandard. Zur Unterstützung von HEVC muss Ihre App Inhalte mit Common Encryption Scheme (CENC) Version 2 verwenden, d. h. die Slice-Header des Inhalts bleiben unverschlüsselt. Weitere Informationen finden Sie in ISO/IEC 23001-7 (Information technology – MPEG systems technologies – Part 7: Common encryption in ISO base media file format files; Spezifikationsversion ISO/IEC 23001-7:2015 oder höher). Microsoft empfiehlt außerdem die Verwendung von CENC Version 2 für alle HWDRM-Inhalte. Zudem wird HEVC nicht von allen Hardware-DRM-Typen unterstützt (siehe [Außerkraftsetzen des Hardware-DRM](hardware-drm.md#override-hardware-drm)).
-   Um bestimmte neue Features von PlayReady 3.0 nutzen zu können (u. a. SL3000 für hardwarebasierte Clients, Abruf mehrerer nicht persistenter Lizenzen in einer Lizenzabrufnachricht und zeitbasierte Einschränkungen für nicht persistente Lizenzen), muss für den PlayReady-Server Microsoft PlayReady Server Software Development Kit v3.0.2769 oder höher verwendet werden.
-   Je nachdem, welche Ausgabeschutzrichtlinie in der Inhaltslizenz festgelegt ist, kann die Medienwiedergabe für Endbenutzer fehlschlagen, wenn die verbundene Ausgabe der Benutzer diese Anforderungen nicht erfüllt. In der folgenden Tabelle sind die Fehler aufgeführt, die in diesem Zusammenhang häufig auftreten. Weitere Informationen finden Sie unter [PlayReady-Kompatibilität und Stabilitätsregeln](http://www.microsoft.com/playready/licensing/compliance/).

| Fehler                                                   | Wert      | Beschreibung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|---------------------------------------------------------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ERROR\_GRAPHICS\_OPM\_OUTPUT\_DOES\_NOT\_SUPPORT\_HDCP  | 0xC0262513 | Die Ausgabeschutzrichtlinie der Lizenz erfordert, dass der Monitor HDCP einbindet, die Einbindung war aber nicht möglich.                                                                                                                                                                                                                                                                                                                                                                                              |
| MF\_E\_POLICY\_UNSUPPORTED                              | 0xC00D7159 | Die Ausgabeschutzrichtlinie der Lizenz erfordert, dass der Monitor HDCP Typ 1 einbindet, die Einbindung war aber nicht möglich.                                                                                                                                                                                                                                                                                                                                                                                |
| DRM\_E\_TEE\_OUTPUT\_PROTECTION\_REQUIREMENTS\_NOT\_MET | 0x8004CD22 | Dieser Fehlercode tritt nur bei der Verwendung des Hardware-DRM auf. Die Ausgabeschutzrichtlinie der Lizenz erfordert, dass der Monitor HDCP einbindet oder die effektive Auflösung des Inhalts verringert, die Einbindung von HDCP war jedoch nicht möglich, und die effektive Auflösung des Inhalts konnte nicht verringert werden, weil das Hardware-DRM die Verringerung der effektiven Auflösung von Inhalten nicht unterstützt. Bei Verwendung des Software-DRM wird der Inhalt wiedergegeben. Weitere Informationen finden Sie unter [Überlegungen zur Verwendung des Hardware-DRM](hardware-drm.md#considerations-for-using-hardware-drm). |
| ERROR\_GRAPHICS\_OPM\_NOT\_SUPPORTED                    | 0xc0262500 | Der Grafiktreiber unterstützt Ausgabeschutz nicht. Dies kann z. B. der Fall sein, wenn der Monitor über VGA verbunden ist oder kein entsprechender Grafiktreiber für die digitale Ausgabe installiert ist. In letzterem Fall ist in der Regel der Treiber „Microsoft Basic Display Adapter” installiert, und das Problem kann durch Installieren eines entsprechenden Grafiktreibers behoben werden.                                                                                                                                                  |

## Ausgabeschutz

Im folgenden Abschnitt wird das Verhalten bei Verwendung von PlayReady DRM für Windows 10 mit Ausgabeschutzrichtlinien in einer PlayReady-Lizenz beschrieben.

PlayReady DRM unterstützt die Ausgabeschutzebenen in der **erweiterbaren Medienrechtespezifikation von Microsoft PlayReady**. Dieses Dokument ist Teil des Dokumentationspakets, das zusammen mit lizenzierten PlayReady-Produkten bereitgestellt wird.

> **Hinweis**
            &nbsp;&nbsp;Die zulässigen Werte für Ausgabesicherheitsebenen, die von einem Lizenzserver festgelegt werden können, unterliegen den [PlayReady-Kompatibilitätsregeln](https://www.microsoft.com/playready/licensing/compliance/).

PlayReady DRM ermöglicht die Wiedergabe von Inhalten mit Ausgabeschutzrichtlinien nur über Ausgabeanschlüsse gemäß Angabe in den PlayReady-Kompatibilitätsregeln. Weitere Informationen zu in den PlayReady-Kompatibilitätsregeln angegebenen Ausgabeanschlussbedingungen finden Sie in den [definierten Bedingungen für PlayReady-Kompatibilität und Stabilitätsregeln](https://www.microsoft.com/playready/licensing/compliance/).

Dieser Abschnitt konzentriert sich auf Ausgabeschutzszenarien mit PlayReady DRM für Windows 10 und PlayReady Hardware DRM für Windows 10 (auch auf einigen Windows-Clients verfügbar). Mit PlayReady HWDRM wird der gesamte Ausgabeschutz innerhalb der Windows-TEE-Implementierung erzwungen (siehe [Hardware-DRM](hardware-drm.md)). Aus diesem Grund unterscheidet sich das Verhalten in einigen Fällen von der Verwendung von PlayReady SWDRM (Software-DRM):

* Unterstützung der Ausgabeschutzebene 270 (Output Protection Level, OPL) für nicht komprimierte digitale Videos: PlayReady HWDRM für Windows 10 unterstützt keine Abwärtsauflösung und erzwingt die Einbindung von HDCP (High-bandwidth Digital Content Protection). Es empfiehlt sich, bei HD-Inhalten für das HWDRM einen OPL-Wert zu verwenden, der größer als 270 ist (dies ist jedoch nicht zwingend erforderlich). Darüber hinaus sollten Sie die HDCP-Typeinschränkung in der Lizenz festlegen (HDCP-Version 2.2 oder höher).
* Im Gegensatz zum SWDRM wird beim HWDRM der Ausgabeschutz für alle Monitore basierend auf dem langsamsten Monitor erzwungen. Wenn der Benutzer beispielsweise zwei Monitore angeschlossen hat und nur einer davon HDCP unterstützt, ist die Wiedergabe nicht möglich, falls die Lizenz HDCP erfordert. Dies gilt auch dann, wenn der Inhalt nur auf dem Monitor gerendert wird, der HDCP unterstützt. Beim SWDRM wird der Inhalt wiedergegeben, solange das Rendering nur auf dem Monitor erfolgt, der HDCP unterstützt.
* Es ist nicht garantiert, dass das HWDRM vom Client verwendet wird und dass das Verfahren sicher ist, es sei denn, von den Inhaltsschlüsseln und -lizenzen werden die folgenden Bedingungen erfüllt:
    * Die für den Videoinhaltsschlüssel verwendete Lizenz muss mindestens die Sicherheitsstufe 3.000 besitzen.
    * Audiodaten müssen mit einem anderen Inhaltsschlüssel verschlüsselt werden als Videodaten, und die für Audiodaten verwendete Lizenz muss mindestens die Sicherheitsstufe 2.000 besitzen. Alternativ können Audiodaten auch unverschlüsselt bleiben.
* In allen SWDRM-Szenarien darf die Sicherheitsebene der PlayReady-Lizenz für den Audio- und/oder Videoinhaltsschlüssel maximal 2.000 betragen.

### Ausgabeschutzebenen

Die folgende Tabelle veranschaulicht die Zuordnungen zwischen verschiedenen OPLs in der PlayReady-Lizenz und deren Erzwingung durch PlayReady DRM für Windows 10:

#### Video

<table>
    <tr>
        <th rowspan="2">OPL</th>
        <th>Komprimierte digitale Videos</th>
        <th colspan="2">Unkomprimierte digitale Videos</th>
        <th>TV (analog)</th>
    </tr>
    <tr>
        <th>Beliebig</th>
        <th colspan="2">HDMI, DVI, DisplayPort, MHL</th>
        <th>Komponente, Composite</th>
    </tr>
    <tr>
        <th>0–100</th>
        <td rowspan="7">Windows 10 übergibt komprimierte digitale Videoinhalte unabhängig vom nachfolgenden OPL-Wert niemals an Ausgaben. Weitere Informationen zu komprimierten digitalen Videoinhalten finden Sie in den [Kompatibilitätsregeln für PlayReady-Produkte](https://www.microsoft.com/playready/licensing/compliance/).</td>
        <td rowspan="3" colspan="2">Inhalt wird übergeben.</td>
        <td>Inhalt wird übergeben.</td>
    </tr>
    <tr>
        <th>101–150</th>
        <td>Inhalt wird übergeben, wenn CGMS-A CopyNever eingebunden wird oder CGMS-A nicht eingebunden werden kann.</td>
    </tr>
    <tr>
        <th>151–200</th>
        <td>Inhalt wird übergeben, wenn CGMS-A CopyNever eingebunden wird.</td>
    </tr>
    <tr>
        <th>201–250</th>
        <td colspan="2">Versucht, HDCP einzubinden, die Inhaltsübergabe erfolgt aber unabhängig vom Ergebnis.</td>
        <td rowspan="4">Inhalt wird NICHT übergeben.</td>
    </tr>
    <tr>
        <th>251–270</th>
        <td>**SWDRM**: Versucht, HDCP einzubinden. Sollte die Einbindung von HDCP nicht möglich sein, beschränkt der PC die effektive Auflösung 520.000 Pixel pro Frame und übergibt den Inhalt.</td>
        <td>**HWDRM**: Inhalt wird mit HDCP übergeben. Sollte die Einbindung von HDCP nicht möglich sein, wird die Wiedergabe über HDMI/DVI-Anschlüsse blockiert.</td>
    </tr>
    <tr>
        <th>271-300</th>
        <td colspan="2">
            <p>
                **Wenn die HDCP-Typbeschränkung NICHT DEFINIERT ist:** Übergibt Inhalte mit HDCP. Sollte die Einbindung von HDCP nicht möglich sein, wird die Wiedergabe über HDMI/DVI-Anschlüsse blockiert.
            </p>
            <p>
                **Wenn die HDCP-Typeinschränkung DEFINIERT ist**: Übergibt Inhalte mit HDCP 2.2, und der Inhaltsdatenstromtyp wird auf 1 festgelegt. Wenn die Einbindung von HDCP nicht möglich ist oder der Inhaltsdatenstromtyp nicht auf 1 festgelegt werden kann, wird die Wiedergabe über HDMI/DVI-Anschlüsse blockiert.
            </p>
        </td>
    </tr>
    <tr>
        <th>\>300</th>
        <td colspan="2">Inhalt wird NICHT übergeben.</td>
    </tr>
</table>

#### Audio

<table>
    <tr>
        <th rowspan="2">OPL</th>
        <th>Komprimierte digitale Audiodaten</th>
        <th>Unkomprimierte digitale Audiodaten</th>
        <th>Analog- oder USB-Audio</th>
    </tr>
    <tr>
        <th>HDMI, DisplayPort, MHL</th>
        <th>HDMI, DisplayPort, MHL</th>
        <th>Beliebig</th>
    </tr>
    <tr>
        <th>0–100</th>
        <td rowspan="2">Inhalt wird übergeben.</td>
        <td>Inhalt wird übergeben.</td>
        <td rowspan="4">Inhalt wird übergeben.</td>
    </tr>
    <tr>
        <th>101–200</th>
        <td rowspan="4">Inhalt wird NICHT übergeben.</td>
    </tr>
    <tr>
        <th>201–250</th>
        <td>Inhalt wird übergeben, wenn HDCP für HDMI, DisplayPort oder MHL eingebunden wird oder SCMS eingebunden und auf „CopyNever“ festgelegt wird.</td>
    </tr>
    <tr>
        <th>251–300</th>
        <td>Inhalt wird übergeben, wenn HDCP für HDMI, DisplayPort oder MHL eingebunden wird.</td>
    </tr>
    <tr>
        <th>\>300</th>
        <td>Inhalt wird NICHT übergeben.</td>
        <td>Inhalt wird NICHT übergeben.</td>
    </tr>
</table>

### Miracast

PlayReady DRM ermöglicht die Wiedergabe von Inhalten per Miracast-Ausgabe, sobald HDCP 2.0 oder höher eingebunden wird. Unter Windows 10 gilt Miracast jedoch als *digitale* Ausgabe. Weitere Informationen zu Miracast-Szenarien finden Sie in den [PlayReady-Kompatibilitätsregeln](https://www.microsoft.com/playready/licensing/compliance/). Die folgende Tabelle veranschaulicht die Zuordnungen zwischen verschiedenen OPLs in der PlayReady-Lizenz und deren Erzwingung durch PlayReady DRM für Miracast-Ausgaben:

<table>
    <tr>
        <th>OPL</th>
        <th>Komprimierte digitale Audiodaten</th>
        <th>Unkomprimierte digitale Audiodaten</th>
        <th>Komprimierte digitale Videos</th>
        <th>Unkomprimierte digitale Videos</th>
    </tr>
    <tr>
        <th>0–100</th>
        <td rowspan="3">Inhalt wird übergeben, wenn HDCP 2.0 oder höher eingebunden wird. Wenn die Einbindung nicht erfolgreich ist, wird der Inhalt NICHT übergeben.</td>
        <td>Inhalt wird übergeben, wenn HDCP 2.0 oder höher eingebunden wird. Wenn die Einbindung nicht erfolgreich ist, wird der Inhalt NICHT übergeben.</td>
        <td rowspan="4">Windows 10 übergibt komprimierte digitale Videoinhalte unabhängig vom nachfolgenden OPL-Wert niemals an Ausgaben. Weitere Informationen zu komprimierten digitalen Videoinhalten finden Sie in den [PlayReady-Kompatibilitätsregeln](https://www.microsoft.com/playready/licensing/compliance/).</td>
        <td rowspan="2">Inhalt wird übergeben, wenn HDCP 2.0 oder höher eingebunden wird. Wenn die Einbindung nicht erfolgreich ist, wird der Inhalt NICHT übergeben.</td>
    </tr>
    <tr>
        <th>101-270</th>
        <td rowspan="3">Inhalt wird NICHT übergeben.</td>
    </tr>
    <tr>
        <th>271-300</th>
        <td>
            <p>
                **Wenn die HDCP-Typeinschränkung NICHT DEFINIERT ist:** Übergibt Inhalt, wenn HDCP 2.0 oder höher aktiviert ist. Wenn die Einbindung nicht erfolgreich ist, wird der Inhalt NICHT übergeben.
            </p>
            <p>
                **Wenn die HDCP-Typeinschränkung DEFINIERT ist**: Übergibt Inhalte mit HDCP 2.2, und der Inhaltsdatenstromtyp wird auf 1 festgelegt. Wenn die Einbindung von HDCP nicht möglich ist oder der Inhaltsdatenstromtyp nicht auf 1 festgelegt werden kann, wird der Inhalt NICHT übergeben.
            </p>        
        </td>
    </tr>
    <tr>
        <th>\>300</th>
        <td>Inhalt wird NICHT übergeben.</td>
        <td>Inhalt wird NICHT übergeben.</td>
    </tr>
</table>

### Zusätzliche explizite Ausgabeeinschränkungen

Die folgende Tabelle beschreibt die Implementierung expliziter Einschränkungen des Ausgabeschutzes für digitale Videos von PlayReady DRM für Windows 10:

<table>
    <tr>
        <th>Szenario</th>
        <th>GUID</th>
        <th>Situation</th>
        <th>Folge</th>
    </tr>
    <tr>
        <th>Maximale effektive Auflösung (Decodierungsgröße)</th>
        <td>9645E831-E01D-4FFF-8342-0A720E3E028F</td>
        <td>Verbundene Ausgabe: digitale Videoausgabe, Miracast, HDMI, DVI usw.</td>
        <td>
            <p>
                Inhalt wird übergeben, wenn folgende Einschränkungen vorliegen:  
            </p>
            <ul>
                <li>(a) Die Breite des Frames muss kleiner oder gleich der maximalen Framebreite (in Pixel) und die Höhe des Frames muss kleiner oder gleich der maximalen Framehöhe (in Pixel) sein. Oder:</li>
                <li>(a) Die Höhe des Frames muss kleiner oder gleich der maximalen Framebreite (in Pixel) und die Breite des Frames muss kleiner oder gleich der maximalen Framehöhe (in Pixel) sein.</li>
            </ul>                   
        </td>
    </tr>
    <tr>
        <th>HDCP-Typeinschränkung</th>
        <td>ABB2C6F1-E663-4625-A945-972D17B231E7</td>
        <td>Verbundene Ausgabe: digitale Videoausgabe, Miracast, HDMI, DVI usw.</td>
        <td>Inhalt wird mit HDCP 2.2 und Inhaltsdatenstrom-Typ „1“ übergeben. Sollte die Einbindung von HDCP 2.2 nicht möglich sein oder der Inhaltsdatenstrom-Typ nicht auf „1“ festgelegt werden können, wird der Inhalt NICHT übergeben. Außerdem muss die Ausgabeschutzebene für unkomprimierte digitale Videos muss mindestens auf 271 festgelegt werden.</td>
    </tr>
</table>

Die folgende Tabelle beschreibt die Implementierung expliziter Einschränkungen des Ausgabeschutzes für analoge Videos von PlayReady DRM für Windows 10.

<table>
    <tr>
        <th>Szenario</th>
        <th>GUID</th>
        <th>Situation</th>
        <th colspan="2">Folge</th>
    </tr>
    <tr>
        <th>Analoger Computermonitor</th>
        <td>D783A191-E083-4BAF-B2DA-E69F910B3772</td>
        <td>Verbundene Ausgabe: VGA, DVI&ndash;analog usw.</td>
        <td>**SWDRM:** Der PC schränkt die effektive Auflösung auf 520.000 epx pro Frame ein und übergibt den Inhalt.</td>
        <td>**HWDRM:** Inhalt wird NICHT übergeben.</td>
    </tr>
    <tr>
        <th>Analoge Komponente</th>
        <td>811C5110-46C8-4C6E-8163-C0482A15D47E</td>
        <td>Verbundene Ausgabe: Komponente</td>
        <td>**SWDRM:** Der PC schränkt die effektive Auflösung auf 520.000 epx pro Frame ein und übergibt den Inhalt.</td>
        <td>**HWDRM:** Inhalt wird NICHT übergeben.</td>
    </tr>
    <tr>
        <th rowspan="2">Analoge TV-Ausgaben</th>
        <td>2098DE8D-7DDD-4BAB-96C6-32EBB6FABEA3</td>
        <td>OPL für Analog-TV ist kleiner als 151.</td>
        <td colspan="2">CGMS-A muss eingebunden werden.</td>
    </tr>
    <tr>
        <td>225CD36F-F132-49EF-BA8C-C91EA28E4369</td>
        <td>OPL für Analog-TV ist kleiner als 101, und „2098DE8D-7DDD-4BAB-96C6-32EBB6FABEA3“ ist nicht in der Lizenz enthalten.</td>
        <td colspan="2">Einbindung von CGMS-A muss versucht werden, der Inhalt kann jedoch unabhängig vom Ergebnis wiedergegeben werden.</td>
    </tr>
    <tr>
        <th>Automatische Verstärkungsregelung und Farbstreifen</th>
        <td>C3FD11C6-F8B7-4D20-B008-1DB17D61F2DA</td>
        <td>Inhalt wird mit einer Auflösung von maximal 520.000 Pixeln an eine analoge TV-Ausgabe übergeben.</td>
        <td colspan="2">Die automatische Verstärkungsregelung wird nur für Komponentenvideos und den PAL-Modus festgelegt, wenn die Auflösung weniger als 520.000 Pixel beträgt. Automatische Verstärkungsregelung und Farbstreifeninformationen für NTSC werden festgelegt, wenn die Auflösung weniger als 520.000 Pixel beträgt (siehe Tabelle 3.5.7.3. in den Kompatibilitätsregeln).</td>
    </tr>
    <tr>
        <th>Nur digitale Ausgabe</th>
        <td>760AE755-682A-41E0-B1B3-DCDF836A7306</td>
        <td>Verbundene Ausgabe: analog</td>
        <td colspan="2">Inhalt wird nicht übergeben.</td>
    </tr>
</table>

> **Hinweis:** Wenn für die Wiedergabe ein Adapterdongle (etwa Mini-DisplayPort auf VGA) verwendet wird, wird die Ausgabe von Windows 10 als digitale Videoausgabe betrachtet, sodass folglich keine Richtlinien für analoge Videos erzwungen werden können.

Die folgende Tabelle beschreibt die Implementierung von PlayReady DRM für Windows 10 für die Wiedergabe in anderen Fällen:

<table>
    <tr>
        <th>Szenario</th>
        <th>GUID</th>
        <th>Situation</th>
        <th colspan="2">Folge</th>
    </tr>
    <tr>
        <th>Unbekannte Ausgabe</th>
        <td>786627D8-C2A6-44BE-8F88-08AE255B01A7</td>
        <td>Die Ausgabe kann nicht vernünftig ermittelt werden, oder für den Grafiktreiber ist kein OPM möglich.</td>
        <td>**SWDRM:** Inhalt wird übergeben.</td>
        <td>**HWDRM:** Inhalt wird NICHT übergeben.</td>
    </tr>
    <tr>
        <th>Unbekannte Ausgabe mit Einschränkung</th>
        <td>B621D91F-EDCC-4035-8D4B-DC71760D43E9</td>
        <td>Die Ausgabe kann nicht vernünftig ermittelt werden, oder für den Grafiktreiber ist kein OPM möglich.</td>
        <td>**SWDRM:** Der PC schränkt die effektive Auflösung auf 520.000 epx pro Frame ein und übergibt den Inhalt.</td>
        <td>**HWDRM:** Inhalt wird NICHT übergeben.</td>
    </tr>
</table>

## Voraussetzungen

Bevor Sie mit der Erstellung Ihrer PlayReady-geschützten UWP-App beginnen, müssen Sie die folgende Software auf Ihrem System installieren:

-   Windows 10.
-   Wenn Sie Beispiele für PlayReady DRM für UWP-Apps kompilieren, müssen Sie Microsoft Visual Studio 2015 oder höher zum Kompilieren der Beispiele verwenden. Microsoft Visual Studio 2013 kann weiterhin zum Kompilieren der Beispiele aus PlayReady DRM für Windows 8.1-Store-Apps verwendet werden.

Wenn Sie planen, MPEG-2/H.262-Inhalte in Ihrer App wiederzugeben, müssen Sie auch [Windows 8.1 Media Center Pack](http://go.microsoft.com/fwlink/p/?LinkId=626876) herunterladen und installieren.

## Migrationshandbuch für Windows Store-Apps mit PlayReady

Dieser Abschnitt enthält Informationen zum Migrieren Ihrer vorhandenen Windows 8.x-Store-Apps mit PlayReady zu Windows 10.

Der Namespace für UWP-Apps mit PlayReady unter Windows 10 wurde von **Microsoft.Media.PlayReadyClient** in [**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454) geändert. Sie müssen also in Ihrem Code den alten Namespace suchen und durch den neuen Namespace ersetzen. Sie verweisen weiterhin auf eine WINMD-Datei. Sie ist Teil von „windows.media.winmd“ im Betriebssystem Windows 10. Sie ist in „windows.winmd“ als Teil des TH Windows SDK enthalten. Bei UWP wird darauf in „windows.foundation.univeralappcontract.winmd“ verwiesen.

Zum Wiedergeben von PlayReady-geschützten High-Definition (HD)-Inhalten (1080p) und Ultra-High-Definition (UHD)-Inhalten müssen Sie das Hardware-DRM mit PlayReady implementieren. Informationen zum Implementieren des Hardware-DRM mit PlayReady finden Sie unter [Hardware-DRM](hardware-drm.md).

Manche Inhalte werden nicht vom Hardware-DRM unterstützt. Informationen zum Deaktivieren des Hardware-DRM und Aktivieren des Software-DRM finden Sie unter [Außerkraftsetzen des Hardware-DRM](hardware-drm.md#override-hardware-drm).

Stellen Sie im Medienschutz-Manager sicher, dass für Ihren Code die folgenden Einstellungen festgelegt sind (sofern noch nicht geschehen):

```cs
var mediaProtectionManager = new Windows.Media.Protection.MediaProtectionManager();

mediaProtectionManager.properties["Windows.Media.Protection.MediaProtectionSystemId"] = 
             '{F4637010-03C3-42CD-B932-B48ADF3A6A54}'
var cpsystems = new Windows.Foundation.Collections.PropertySet();
cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = 
                "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput";
mediaProtectionManager.properties["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;

mediaProtectionManager.properties["Windows.Media.Protection.MediaProtectionContainerGuid"] = 
                "{9A04F079-9840-4286-AB92-E65BE0885F95}";
```

## Proaktives Abrufen einer nicht persistenten Lizenz vor der Wiedergabe

In diesem Abschnitt wird beschrieben, wie Sie vor dem Start der Wiedergabe proaktiv nicht persistente Lizenzen abrufen.

In früheren Versionen von PlayReady DRM konnten nicht persistente Lizenzen nur reaktiv während der Wiedergabe abgerufen werden. In dieser Version können Sie nicht persistente Lizenzen proaktiv abrufen, bevor die Wiedergabe beginnt.

1.  Erstellen Sie proaktiv eine Wiedergabesitzung, in der die nicht persistente Lizenz gespeichert werden kann. Beispiel:

    ```cs
    var cpsystems = new Windows.Foundation.Collections.PropertySet();       
    cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput"; // PlayReady

    var pmpSystemInfo = new Windows.Foundation.Collections.PropertySet();
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemId"] = "{F4637010-03C3-42CD-B932-B48ADF3A6A54}";
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;
    var pmpServer = new Windows.Media.Protection.MediaProtectionPMPServer( pmpSystemInfo );
    ```

2.  Binden Sie die Wiedergabesitzung an die Lizenzabrufklasse. Beispiel:

    ```cs
    var licenseSessionProperties = new Windows.Foundation.Collections.PropertySet();
    licenseSessionProperties["Windows.Media.Protection.MediaProtectionPMPServer"] = pmpServer;
    var licenseSession = new Windows.Media.Protection.PlayReady.PlayReadyLicenseSession( licenseSessionProperties );
    ```

3.  Erstellen Sie eine Lizenzserviceanfrage. Beispiel:

    ```cs
    var laSR = licenseSession.CreateLAServiceRequest();
    ```

4.  Führen Sie den Lizenzerwerb mit der Serviceanfrage aus, die Sie in Schritt 3 erstellt haben. Die Lizenz wird in der Wiedergabesitzung gespeichert.
5.  Binden Sie die Wiedergabesitzung an die Medienquelle für die Wiedergabe. Beispiel:

    ```cs
    licenseSession.configureMediaProtectionManager( mediaProtectionManager );
    videoPlayer.msSetMediaProtectionManager( mediaProtectionManager );
    ```
    
## Hinzufügen des sicheren Beendens

In diesem Abschnitt wird beschrieben, wie Sie Ihrer UWP-App die Funktion für sicheres Beenden hinzufügen.

Dank des sicheren Beendens kann ein PlayReady-Gerät einem Medienstreamingdienst zuverlässig bestätigen, dass die Medienwiedergabe für einen bestimmten Inhalt beendet wurde. Mit dieser Funktion wird sichergestellt, dass Ihre Medienstreamingdienste präzise Erzwingung und Berichte zu Nutzungseinschränkungen auf verschiedenen Geräten für ein bestimmtes Konto bereitstellen.

Es gibt zwei primäre Szenarien für das Senden einer Abfrage für sicheres Beenden:

-   Wenn die Mediendarstellung beendet wird, weil das Ende des Inhalts erreicht wurde, oder wenn die Mediendarstellung vor ihrem Ende vom Benutzer beendet wurde.
-   Wenn die vorherige Sitzung unerwartet beendet wurde (z. B. aufgrund eines System- oder App-Absturzes). Die App muss beim Starten oder Herunterfahren alle ausstehenden Sitzungen für sicheres Beenden abfragen und von anderen Medienwiedergaben getrennte Abfragen senden.

Eine Beispielimplementierung für das sichere Beenden finden Sie in der Datei „securestop.cs” im PlayReady-Beispiel unter [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670).

 

 







<!--HONumber=Jun16_HO5-->


