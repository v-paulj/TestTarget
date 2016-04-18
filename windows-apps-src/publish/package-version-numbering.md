---
Description: Der Windows Store-erzwingt bestimmte Regeln, die im Zusammenhang mit Versionsnummern auftreten, die in verschiedenen Betriebssystemversionen unterschiedlich funktionieren.
title: Paketversionsnummern
ms.assetid: DD7BAE5F-C2EE-44EE-8796-055D4BCB3152
---

# Paketversionsnummern


Jedes von Ihnen bereitgestellte Paket muss eine Versionsnummer aufweisen (als Wert im **Version** -Attribut des **Package/Identity**-Elements im App-Manifest). Der Windows Store-erzwingt bestimmte Regeln, die im Zusammenhang mit Versionsnummern auftreten, die in verschiedenen Betriebssystemversionen unterschiedlich funktionieren.

> **Hinweis**  Dieses Thema bezieht sich auf „Pakete“, aber falls nicht angegeben, gelten die gleichen Regeln für Versionsnummern für .appx- und .appxbundle-Dateien.

## Versionsnummern für Windows 10-Pakete


Die Versionsnummer der Windows 10-Pakete muss in jedem Fall höher sein als die der Pakete für Windows 8, Windows 8.1 und/oder Windows Phone 8.1, die sie für dieselbe App veröffentlichen oder bereits veröffentlicht haben. (Weitere Informationen finden Sie unter [Hinzufügen von Paketen für Windows 10 zu einer zuvor veröffentlichten App](guidance-for-app-package-management.md#adding-packages-for-windows-10-to-a-previously-published-app).)

> **Hinweis**  Der letzte (vierte) Abschnitt der Versionsnummer ist für die Store-Verwendung reserviert und muss 0 bleiben.

Wenn Sie ein Paket für Windows 10 aus der veröffentlichten Übermittlung auswählen, verwendet der Windows Store immer das Paket mit der höchsten Versionsnummer, das für das Kundengerät gilt. Dadurch sind Sie flexibler und haben die Kontrolle darüber, welche Pakete Kunden auf bestimmten Gerätetypen bereitgestellt werden. Außerdem können Sie diese Pakete in beliebiger Reihenfolge übermitteln; Sie sind nicht darauf beschränkt, bei nachfolgenden Übermittlungen Pakete mit höheren Versionsnummern bereitzustellen.

Sie können sogar mehrere Pakete für Windows 10 mit der gleichen Versionsnummer bereitstellen. Pakete mit der gleichen Versionsnummer können jedoch nicht dieselbe Architektur aufweisen, da die vollständige Identität eindeutig sein muss, die der Store für die einzelnen Pakete verwendet. Weitere Informationen finden Sie unter [**Identity**](https://msdn.microsoft.com/library/windows/apps/br211441).

Wenn Sie mehrere Pakete für Windows 10 mit der gleichen Versionsnummer angeben, wird die Architektur (in der Reihenfolge x64, x86, ARM, Neutral) verwendet, um zu entscheiden, welches Gerät beim Bereitstellen der Pakete den höheren Rang besitzt. Beim Bewerten der App-Bündel mit der gleichen Versionsnummer gilt der höchste Architekturrang im Bündel: ein App-Bündel, das ein x64-Paket enthält, besitzt einen höheren Rang als ein App-Bündel, das lediglich ein X 86-Paket enthält.

Dies bietet Ihnen ein hohes Maß an Flexibilität, um Ihre App im Laufe der Zeit weiterzuentwickeln. Sie können neue Pakete mit einer niedrigeren Versionsnummer hochladen und übermitteln, um die Unterstützung für kostengünstige Geräte hinzuzufügen, die Sie zuvor nicht unterstützt haben. Sie können Pakete mit höheren Versionsnummern und strengeren Abhängigkeiten hinzufügen, um von Hardware- oder Betriebssystem-Features zu profitieren, oder Sie können Pakete mit höheren Versionsnummern hinzufügen, die für einige oder alle Ihrer vorhandenen Kunden als Aktualisierungen dienen.

Das folgende Beispiel veranschaulicht, wie Versionsnummern verwaltet werden können, um Ihren Kunden über mehrere Übermittlungen die beabsichtigten Pakete bereitzustellen.

### Beispiel: Wechsel zu einem einzelnen Paket über mehrere Übermittlungen

Mit Windows 10 können Sie eine einzelne Codebasis schreiben, die überall ausgeführt werden kann. Dadurch wird das Starten eines neuen plattformübergreifenden Projekts sehr viel einfacher. Wegen einer Reihe von Gründen möchten Sie jedoch vielleicht die vorhandene Codebasis nicht zusammenführen, um direkt ein einzelnes Projekt zu erstellen.

Sie können die Paket-Versionsnummernregeln verwenden, um den Wechsel Ihrer Kunden nach und nach auf ein einzelnes Paket für die universellen Gerätefamilie vorzunehmen. Dabei werden eine Reihe von vorläufigen Aktualisierungen für bestimmte Gerätefamilien (einschließlich derjenigen, die die Windows 10-APIs nutzen) bereitgestellt. Im folgenden Beispiel wird die konsistente Anwendung derselben Regeln veranschaulicht.

| Übermittlung | Inhalt                                                  | Benutzerfreundlichkeit                                                                                                                                                                             |
|------------|-----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1          | -   Paketversion: 1.1.10.0 <br> -   Gerätefamilie: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Paketversion: 1.1.0.0 <br> -   Gerätefamilie: Windows.Mobile, minVersion 10.0.10240.0     | -   Geräte unter Windows 10 Desktop Build 10.0.10240.0 und höher erhalten Versionsnummer 1.1.10.0 <br> -   Geräte unter Windows 10 Mobile Build 10.0.10240.0 und höher erhalten Versionsnummer 1.1.0.0 <br> -   Für andere Gerätefamilien kann die App nicht erworben und installiert werden. |
| 2          | -   Paketversion: 1.1.10.0 <br> -   Gerätefamilie: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Paketversion: 1.1.0.0 <br> -   Gerätefamilie: Windows.Mobile, minVersion 10.0.10240.0 <br> <br> -   Paketversion: 1.0.0.0 <br> -   Gerätefamilie: Windows.Universal, minVersion 10.0.10240.0    | -   Geräte unter Windows 10 Desktop Build 10.0.10240.0 und höher erhalten Versionsnummer 1.1.10.0 <br> -   Geräte unter Windows 10 Mobile Build 10.0.10240.0 und höher erhalten Versionsnummer 1.1.0.0 <br> -   Andere (nicht-Desktop, nicht mobile) Gerätefamilien, erhalten nach Einführung Versionsnummer 1.0.0.0 <br> -   Bei Desktop- und mobilen Geräten, auf denen die App bereits installiert ist, wird kein Updates angezeigt (da diese bereits die beste verfügbare Version besitzen – 1.1.10.0 und 1.1.0.0 sind beide höher als 1.0.0.0) |
| 3          | -   Paketversion: 1.1.10.0 <br> -   Gerätefamilie: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Paketversion: 1.1.5.0 <br> -   Gerätefamilie: Windows.Universal, minVersion 10.0.10250.0 <br> <br> -   Paketversion: 1.0.0.0 <br> -   Gerätefamilie: Windows.Universal, minVersion 10.0.10240.0    | -   Geräte unter Windows 10 Desktop Build 10.0.10240.0 und höher erhalten Versionsnummer 1.1.10.0 <br> -   Geräte unter Windows 10 Mobile Build 10.0.10250.0 und höher erhalten Versionsnummer 1.1.5.0 <br> -   Geräte unter Windows 10 Mobile Build >= 10.0.10240.0 und < 10.010250.0 erhalten Versionsnummer 1.1.0.0 
| 4          | -   Paketversion: 2.0.0.0 <br> -   Gerätefamilie: Windows.Universal, minVersion 10.0.10240.0   | -   Alle Kunden mit allen Gerätefamilien unter Windows 10 Build v10.0.10240.0 und höher erhalten Paket 2.0.0.0 | 

> **Note**  Kundengeräte erhalten in allen Fällen das Paket mit der höchsten Versionsnummer, für die sie sich qualifizieren. In der dritten Übermittlung oben erhalten beispielsweise alle Desktopgeräte v1.1.10.0, auch wenn sie die Betriebssystemversion 10.0.10250.0 oder höher haben und somit auch mit v1.1.5.0 kompatibel wären. Da 1.1.10.0 für sie die höchsten Versionsnummer ist, erhalten sie dieses Paket.

### Verwenden der Versionsnummern zum Durchführen eines Rollbacks auf ein vorheriges Paket für Neuanschaffungen

Wenn Sie Kopien Ihrer früheren Windows 10-Paketdateien beibehalten, können Sie im Store ein Rollback Ihres App-Pakets auf ein früheres Windows 10-Paket durchführen, wenn Sie Probleme mit einer Version haben. Dies ist eine temporäre Möglichkeit, die Unterbrechung für Ihre Kunden zu begrenzen, während Sie das Problem beheben.

Erstellen Sie zu diesem Zweck eine neue Übermittlung. Entfernen Sie das problematische Paket und laden Sie das alte Paket hoch, das Sie im Store bereitstellen möchten. Kunden, die bereits das Paket erhalten haben, für das Sie einen Rollback durchführen, weisen immer noch das problematische Paket auf (da das ältere Paket eine frühere Versionsnummer besitzt). Dadurch wird verhindert, dass jemand das problematische Paket erhält, und die App ist im Store weiterhin verfügbar.

Um die Probleme für die Kunden zu beheben, die das problematische Paket bereits erhalten haben, können Sie ein neues Paket für Windows 10 mit einer höheren Versionsnummer übermitteln. Danach durchläuft die Übermittlung den Zertifizierungsprozess, und alle Kunden werden auf das neue Paket aktualisiert, da es eine höhere Versionsnummer aufweist.

## Versionsnummern für Windows 8.1 (und früher) und Windows Phone 8.1-Pakete

Bei APPX-Paketen für Windows Phone 8.1-Pakete muss die Versionsnummer des Pakets in einer neuen Übermittlung immer höher sein als die des in der letzten Übermittlung (oder vorherigen Übermittlung) enthaltenen Pakets.

Bei APPX-Paketen für Windows 8 und Windows 8.1 gilt die gleiche Regel pro Architektur: die Versionsnummer des Pakets in einer neuen Übermittlung muss immer höher sein als die des zuletzt im Windows Store veröffentlichten Pakets für dieselbe Architektur.

Außerdem muss die Versionsnummer von Windows 8.1-Paketen stets höher sein als die Versionsnummern aller Windows 8-Pakete für dieselbe App. Mit anderen Worten: Die Versionsnummer eines von Ihnen übermittelten Windows 8-Pakets muss niedriger sein als die Versionsnummer eines Windows 8.1-Paket, das Sie für dieselbe App übermittelt haben.

> **Note**  Wenn Sie außerdem Windows 10-Pakete besitzen, muss die Versionsnummer der Windows 10-Pakete höher sein als die der Pakete für Windows 8, Windows 8.1 und/oder Windows Phone 8.1, die Sie veröffentlichen oder veröffentlicht haben. Weitere Informationen finden Sie unter [Hinzufügen von Paketen für Windows 10 zu einer zuvor veröffentlichten App](guidance-for-app-package-management.md#adding-packages-for-windows-10-to-a-previously-published-app).

Hier finden Sie einige Beispiele für verschiedene Versionsnummern-Aktualisierungsszenarios für Windows 8 und Windows 8.1.

| Version der App im Store  | Hochgeladene Version | Nachdem die neue Version in den Windows Store hochgeladen wurde, wird sie als Neukauf installiert. | Nachdem die neue Version in den Windows Store hochgeladen wurde, wird sie aktualisiert, wenn der Kunde die App bereits besitzt. |
|---------------------------------------------|-----------------------------|--------------------------------------------------------------------------------------------|----------|
| Nichts                                     | x86, v1.0.0.0               | x86, v1.0.0.0 auf x86- und x64-PCs                                                | Nichts. |
| x86, v1.0.0.0                               | x64, v1.0.0.0               | v1.0.0.0 für die Architektur des Kunden                                                   | Nichts. Die Versionsnummern sind identisch. |
| x86, v1.0.0.0 <br> x64, v1.0.0.0            | x64, v1.0.0.1               | v1.0.0.0 für Kunden mit einem x86-PC <br> v1.0.0.1 für Kunden mit einem x64-PC                 | Nichts für Kunden, die die App auf einem x86-PC ausführen. <br> v1.0.0.0 wird für Kunden, die die App auf einem x64-PC ausführen, auf v1.0.0.1 aktualisiert. <br> **Note**  Wenn die x86-Version der App auf einem x64-Computer ausgeführt wird, wird die App erst auf die x64-Version aktualisiert, wenn der Kunde die App deinstalliert und wieder neu installiert. |
| Nichts                                     | Neutral, v1.0.0.1           | Neutral, v1.0.0.1 auf allen PCs                                                         | Nichts. |
| Neutral, v1.0.0.1                           | x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | v1.0.0.0 für die Architektur des PC des Kunden.          | Nichts. Kunden mit der neutralen Version v1.0.0.1 verwenden weiter diese Version der App. |
| Neutral, v1.0.0.1 <br> x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | v1.0.0.1 für die Architektur des PC des Kunden. | Nichts für Kunden mit der neutralen Version v1.0.0.1. <br> v1.0.0.0 wird für Kunden, die die Version v1.0.0.0 der App für die spezifische Architektur ihres PC verwenden, auf v1.0.0.1 aktualisiert. |
| x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | x86, v1.0.0.2 <br> x64, v1.0.0.2 <br> ARM, v1.0.0.2 | v1.0.0.2 für die Architektur des PC des Kunden.  | v1.0.0.1 wird für Kunden, die die Version v1.0.0.1 der App für die spezifische Architektur ihres PC verwenden, auf v1.0.0.2 aktualisiert. |
 
> **Hinweis**  Im Gegensatz zu APPX-Paketen werden die Versionsnummern in allen XAP-Paketen beim Ermitteln der für einen gegebenen Kunden bereitzustellenden Pakete ignorier. Um ein Kunde von einem XAP-Paket auf ein neueres zu aktualisieren, stellen Sie sicher, dass die ältere XAP-Datei in der neuen Übermittlung entfernt wird.


<!--HONumber=Mar16_HO4-->


