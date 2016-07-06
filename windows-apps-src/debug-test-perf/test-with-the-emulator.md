---
author: mcleblanc
ms.assetid: 7234DD5F-8E86-424E-99A0-93D01F1311F2
title: "Testen mit dem Emulator für Microsoft Windows 10 Mobile"
description: "Mit den Tools des Emulators für Microsoft Windows 10 Mobile können Sie die praktische Interaktion mit einem Gerät simulieren und die Features Ihrer App testen."
translationtype: Human Translation
ms.sourcegitcommit: 9a33710315486c23a204a528d3d87421c6990b85
ms.openlocfilehash: c53bda2329cd984e3a03d4a166e7353097e62cef

---
# Testen mit dem Emulator für Microsoft Windows 10 Mobile

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Mit den Tools des Emulators für Microsoft Windows 10 Mobile können Sie die praktische Interaktion mit einem Gerät simulieren und die Features Ihrer App testen. Der Emulator ist eine Desktopanwendung zur Emulierung eines mobilen Geräts unter Windows 10. Die Anwendung stellt eine virtualisierte Umgebung bereit, in der Sie Windows-Apps ohne physisches Gerät debuggen und testen können. Außerdem steht Ihnen eine isolierte Umgebung für Ihre Anwendungsprototypen zur Verfügung.

Die Leistung des Emulators ist mit der Leistung eines echten Geräts vergleichbar. Es empfiehlt sich jedoch, die App vor der Veröffentlichung im Windows Store auf einem physischen Gerät zu testen.

Sie können Ihre universelle App mithilfe eines eindeutigen Windows 10 Mobile-Emulatorimages für verschiedene Bildschirmauflösungen und -größen testen. Mit den enthaltenen Tools des Microsoft-Emulators können Sie die praktische Interaktion mit einem Gerät simulieren und verschiedene Features Ihrer App testen.

## Systemanforderungen

Ihr Computer muss folgende Anforderungen erfüllen:

BIOS

-   Hardwareunterstützte Virtualisierung
-   SLAT (Second Level Address Translation)
-   Hardwarebasierte Datenausführungsverhinderung (Data Execution Prevention, DEP)

RAM

-   Mindestens 4 GB

Betriebssystem

-   Mindestens Windows 8 (Windows 10 empfohlen)
-   64 Bit
-   Mindestens Pro-Edition

Informationen zum Überprüfen der BIOS-Anforderungen finden Sie unter [So wird's gemacht: Aktivieren von Hyper-V für den Emulator für Windows Phone 8](https://msdn.microsoft.com/library/windows/apps/xaml/jj863509.aspx).

Klicken Sie zum Überprüfen der RAM- und Betriebssystemanforderungen in der Systemsteuerung auf **System und Sicherheit** und anschließend auf **System**.

Für den Microsoft-Emulator für Windows 10 Mobile wird Visual Studio 2015 benötigt. Er ist nicht mit älteren Versionen von Visual Studio kompatibel.

Der Microsoft-Emulator für Windows 10 Mobile kann keine Apps laden, die für eine Windows Phone-Betriebssystemversion vor Windows Phone OS 7.1 konzipiert sind.

## Installation und Deinstallation

-   **Installation**.

    Der Microsoft-Emulator für Windows 10 Mobile ist im Windows 10-SDK enthalten. Das Windows 10-SDK und der Emulator können zusammen mit Visual Studio 2015 installiert werden. Weitere Informationen finden Sie auf der [Downloadseite für Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=534785).

    Der Microsoft-Emulator für Windows 10 Mobile kann auch über das Microsoft-Emulator-Setup installiert werden. Weitere Informationen finden Sie auf der [Downloadseite für Windows 10-Tools](https://go.microsoft.com/fwlink/p/?LinkID=534189).

-   **Deinstallation**.

    Der Microsoft-Emulator für Windows 10 Mobile kann über die Setup-/Reparaturfunktion von Visual Studio deinstalliert werden Alternativ können Sie in der Systemsteuerung**** zu **Programme und Features** navigieren und den Emulator dort entfernen.

    Wenn Sie den Microsoft-Emulator für Windows 10 Mobile deinstallieren, wird automatisch auch der virtuelle, für den Emulator erstellte Hyper-V-Ethernetadapter entfernt. Dieser virtuelle Adapter kann in der Systemsteuerung**** unter **Netzwerkverbindungen** manuell entfernt werden.

## Neues beim Microsoft-Emulator für Windows 10 Mobile

Neben der Unterstützung der Universal Windows Platform (UWP) bietet der Emulator folgende zusätzliche Funktionen:

-   Unterstützung des Mauseingabemodus zur Unterscheidung zwischen Maus- und einzelner Toucheingabe.
-   NFC-Unterstützung. Der Emulator ermöglicht NFC-Simulationen sowie das Testen und Entwickeln universeller Apps mit NFC- und Näherungsfunktionen.
-   Systemeigene Hardwarebeschleunigung verbessert die Grafikleistung im Emulator durch die Verwendung der lokalen Grafikkarte. Um die Beschleunigung verwenden zu können, muss eine unterstützte Grafikkarte installiert sein, und Sie müssen die Beschleunigung auf der Registerkarte **Sensoren** auf der Einstellungsbenutzeroberfläche **Zusätzliche Tools** des Emulators aktivieren.

## Features, die Sie im Emulator testen können

Zusätzlich zu den neuen Features, die im vorherigen Abschnitt erwähnt wurden, können Sie im Emulator für Microsoft Windows 10 Mobile die folgenden häufig verwendeten Features testen.

-   **Bildschirmauflösung, Bildschirmgröße und Arbeitsspeicher**. Stellen Sie Ihre App einem breiten Markt zur Verfügung, indem Sie sie unter verschiedenen Emulatorimages testen, die verschiedene Bildschirmauflösungen, physische Größen und Arbeitsspeicherbeschränkungen simulieren.

    ![Verfügbare Emulatoren mit Auflösung, Größe und Arbeitsspeicher](images/em-list.png)

-   **Bildschirmkonfiguration**. Ändern Sie den Emulatormodus von Hochformat in Querformat. Ändern Sie die Zoomeinstellung, um den Emulator an Ihren Desktopbildschirm anzupassen.

-   **Netzwerke**. Der Windows Phone-Emulator verfügt über eine integrierte Netzwerkunterstützung. Die Netzwerkfunktionen sind standardmäßig aktiviert. In den meisten Umgebungen müssen Sie für den Windows Phone-Emulator keine Netzwerktreiber installieren oder manuell Netzwerkoptionen konfigurieren.

    Der Emulator verwendet die Netzwerkverbindung des Hostcomputers. Er erscheint nicht als separates Gerät im Netzwerk. Dadurch werden einige der Konfigurationsprobleme vermieden, die bei Benutzern mit dem Emulator aus dem Windows Phone SDK 8.0 aufgetreten sind.

-   **Sprach- und Regionseinstellungen**. Bereiten Sie Ihre App für den internationalen Markt vor, indem Sie die Anzeigesprache und die Regionseinstellungen im Windows Phone-Emulator ändern.

    Navigieren Sie im ausgeführten Emulator zur **Einstellungs**-App, wählen Sie die **System**einstellungen aus, und wählen Sie dann **Sprache** oder **Region** aus. Ändern Sie die Einstellungen, die Sie testen möchten. Wenn Sie dazu aufgefordert werden, klicken Sie auf **restart phone**, um die neuen Einstellungen anzuwenden und den Emulator neu zu starten.

-   **Anwendungslebenszyklus und Markieren als veraltet**. Testen Sie das Verhalten Ihrer App, wenn diese deaktiviert oder als veraltet markiert wird, indem Sie auf der Seite **Debug** der Projekteigenschaften den Wert der Option **Tombstone upon deactivation while debugging** ändern.

-   **Lokaler Ordnerspeicher (ehemals „isolierter Speicher“)**. Daten im isolierten Speicher bleiben während der Emulatorausführung erhalten, gehen beim Schließen des Emulators aber verloren.

-   **Mikrofon**. Erfordert und nutzt das Mikrofon des Hostcomputers.

-   **Phone-Tastatur**. Der Emulator unterstützt die Zuordnung der Hardware-Tastatur auf Ihrem Entwicklungscomputer zu einer Windows Phone-Tastatur. Das Verhalten der Tasten ist das gleiche wie auf einem Windows Phone-Gerät

-   **Sperrbildschirm**. Drücken Sie bei geöffnetem Emulator auf Ihrer Computertastatur zweimal F12. Die F12-TASTE emuliert die Ein/Aus-Taste des Smartphones. Mit dem ersten Tastendruck wird das Display ausgeschaltet. Mit dem zweiten Tastendruck wird das Display wieder eingeschaltet, und der Sperrbildschirm ist aktiviert. Entsperren Sie den Bildschirm, indem Sie den Sperrbildschirm mithilfe der Maus nach oben schieben.

## Features, die Sie im Emulator nicht testen können

Testen Sie die folgenden Features nur auf einem physischen Gerät:

-   Kompass
-   Gyroskop
-   Vibrationscontroller
-   Helligkeit. Die Helligkeitsstufe des Emulators entspricht immer "Hoch".
-   Videos mit hoher Auflösung. Videos mit einer höheren Auflösung als VGA (640 x 480) können nicht zuverlässig angezeigt werden. Dies gilt insbesondere für Emulatorimages mit nur 512 MB Arbeitsspeicher.

## Mauseingabe

Simuliert Mauseingaben über die physische Maus oder das physische Trackpad Ihres Windows-PCs und die Mauseingabeschaltfläche auf der Symbolleiste des Emulators. Dieses Feature ist hilfreich, wenn der Benutzer in Ihrer App eine mit seinem Windows 10-Gerät gekoppelte Maus verwenden kann.

Tippen Sie auf der Symbolleiste des Emulators auf die Mauseingabeschaltfläche, um die Mauseingabe zu aktivieren. Jedes Klickereignis innerhalb des Emulators wird nun als Mausereignis an das im Emulator ausgeführte Windows 10 Mobile-Betriebssystem gesendet.

![Emulatorbildschirm mit aktivierter Mauseingabe](images/emulator-with-mouse-enabled.png)

Der Emulatorbildschirm mit aktivierter Mauseingabe.

![Die Mauseingabeschaltfläche auf der Symbolleiste des Emulators.](images/emulator-showing-mouse-input-button-bar.png)

Die Mauseingabeschaltfläche auf der Symbolleiste des Emulators.

## Tastatureingabe

Der Emulator unterstützt die Zuordnung der Hardware-Tastatur auf Ihrem Entwicklungscomputer zu einer Windows Phone-Tastatur. Das Verhalten der Tasten ist das gleiche wie auf einem Windows Phone-Gerät. 

Die Hardware-Tastatur ist standardmäßig nicht aktiviert. Diese Implementierung entspricht eine gleitende Tastatur, die bereitgestellt werden muss, bevor Sie verwendet werden kann. Bevor Sie die Hardwaretastatur aktivieren, akzeptiert der Emulator Tastatureingaben nur von den Steuerelementtasten.

Sonderzeichen auf der Tastatur einer lokalisierten Version eines Windows-Entwicklungscomputer werden vom Emulator nicht unterstützt. Um Sonderzeichen einzugeben, die auf einer lokalisierten Tastatur vorhanden sind, verwenden Sie stattdessen den Eingabebereich (Software Input Panel, SIP). 

Drücken Sie F4, um die Tastatur des Computers im Emulator zu verwenden.

Drücken Sie F4, um die Verwendung der Tastatur des Computers im Emulator zu beenden.

Die folgende Tabelle enthält die Tasten einer Hardwaretastatur, die Sie verwenden können, um Schaltflächen und andere Steuerelemente auf einem Windows Phone zu emulieren.

Beachten Sie, dass mit Emulator Build 10.0.14332 die Zuordnung der Computerhardwaretasten geändert wurde. Die Einträge in der zweiten Spalte der folgenden Tabelle bezeichnen diese neuen Tasten. 

Computerhardwaretasten (Emulator Build 10.0.14295 und früher) | Computerhardwaretasten (Emulator Build 10.0.14332 und höher) | Windows Phone-Hardwaretaste | Hinweise
--------------------- | ------------------------- | ----------------------------- | -----
F1 | WIN + ESC | Zurück | Eine lange Betätigung funktioniert wie erwartet.
F2 | WIN + F2 | Start | Eine lange Betätigung funktioniert wie erwartet.
F3 | WIN + F3 | Suche |  
F4 | F4 (keine Änderung) | Schaltet die Verwendung der lokalen Computertastatur ein oder aus. | 
F6 | WIN + F6 | Kamera halb | Eine dedizierte Kamerataste, die halb gedrückt wird.
F7 | WIN + F7 | Kamera ganz | Eine dedizierte Kamerataste
F9 | WIN + F9 | Lauter | 
F10 | WIN + F10 | Leiser | 
F12 | WIN + F12 | Stromversorgung | Drücken Sie die Taste F12 zweimal, um den Sperrbildschirm zu aktivieren. Eine lange Betätigung funktioniert wie erwartet.
ESC | WIN + ESC | Zurück | Eine lange Betätigung funktioniert wie erwartet.
 


## Near Field Communication (NFC)

Erstellen und testen Sie Apps mit NFC-Features (Near Field Communication) unter Windows 10 Mobile mithilfe der Registerkarte **NFC** im Menü **Zusätzliche Tools** des Emulators. NFC kann in einer Vielzahl von Szenarien eingesetzt werden – von Näherungsszenarien (z. B. „Zum Senden berühren“) bis hin zur Emulierung von Karten (z. B. „Zum Bezahlen berühren“).

Zum Testen Ihrer App können Sie mithilfe eines Emulatorpaars zwei sich berührende Smartphones simulieren. Alternativ können Sie Ihre App testen, indem Sie eine Berührung mit einem Tag simulieren. Unter Windows 10 verfügen mobile Geräte zudem über HCE (Host Karte Emulation). Mithilfe des Smartphone-Emulators können Sie die Berührung Ihres Geräts mit einem Bezahlterminal für APDU-Befehl/Antwort-Datenverkehr simulieren.

Die NFC-Registerkarte unterstützt drei Modi.

-   Näherungsmodus
-   HCE-Modus (Host Card Emulation)
-   Smartcardlesemodus

In allen Modi bietet das Emulatorfenster drei interessante Bereiche.

-   Der Abschnitt links oben ist modusspezifisch. Die Features dieses Abschnitts sind abhängig vom gewählten Modus und werden in den modusspezifischen Abschnitten weiter unten ausführlich beschrieben.
-   Rechts oben befinden sich die Protokolle. Wenn Sie ein Gerätepaar aneinander oder ein Gerät an das POS-Terminal halten, wird dieses Ereignis protokolliert. Gleiches gilt, wenn die Geräte wieder voneinander getrennt werden. In diesem Abschnitt wird auch erfasst, ob Ihre App vor dem Verbindungsabbruch reagiert hat, und alle Aktionen, die Sie auf der Benutzeroberfläche des Emulators ausgeführt haben, werden mit Zeitstempel dokumentiert. Die Protokolle bleiben beim Moduswechsel erhalten und können mithilfe der Löschschaltfläche**** über dem Protokollbildschirm**** jederzeit gelöscht werden.
-   Die untere Bildschirmhälfte fungiert als Meldungsprotokoll und zeigt abhängig vom ausgewählten Modus die Aufzeichnung aller Meldungen, die über die derzeit ausgewählte Verbindung gesendet oder empfangen werden.

> **Wichtig**  Beim erstmaligen Starten des Tapper-Tools erscheint eine Eingabeaufforderung der Windows-Firewall. Aktivieren Sie unbedingt alle drei Kontrollkästchen, und lassen Sie das Tool durch die Firewall, da es ansonsten nicht funktioniert.

Halten Sie sich nach dem Starten des Schnellstart-Installationsprogramms an die obige Anweisung, und aktivieren Sie in der Eingabeaufforderung der Firewall alle drei Kontrollkästchen. Das Tapper-Tool muss außerdem auf dem gleichen physischen Hostcomputer installiert und verwendet werden wie der Microsoft-Emulator.

### Näherungsmodus

Zum Simulieren der Berührung zweier Smartphones müssen Sie ein Windows Phone 8-Emulatorpaar starten. Da Visual Studio die Ausführung zweier identischer Emulatoren nicht unterstützt, muss für die Emulatoren jeweils eine andere Auflösung ausgewählt werden.

![Die Näherungsseite für NFC](images/emulator-nfc-proximity.png)

Wenn Sie das Kontrollkästchen **Enable discovery of peer devices** aktivieren, enthält das Dropdownfeld **Peer device** Microsoft-Emulatoren (die auf dem gleichen physischen Hostcomputer oder im lokalen Netzwerk ausgeführt werden) sowie die Windows-Computer, auf denen der Simulatortreiber ausgeführt wird (auf dem gleichen Computer oder im lokalen Netzwerk).

Wenn beide Emulatoren ausgeführt werden, gehen Sie wie folgt vor:

-   Wählen Sie in der Liste mit den Peergeräten**** den gewünschten Ziel-Emulator aus.
-   Aktivieren Sie das Optionsfeld **Send to peer device** (An Peergerät senden).
-   Klicken Sie auf die Schaltfläche **Tap** (Berühren). Dadurch wird die Berührung der beiden Geräte simuliert und der entsprechende NFC-Benachrichtigungssound ausgegeben.
-   Klicken Sie zum Trennen der beiden Geräte einfach auf die Schaltfläche **Untap** (Trennen).

Alternativ können Sie das Kontrollkästchen **Automatically untap in (seconds)** aktivieren und angeben, nach wie vielen Sekunden die Berührung automatisch beendet werden soll. Damit wird die praktische Verwendung der Funktion simuliert, da Benutzer ihre Smartphones üblicherweise nur kurz aneinander halten. Beachten Sie jedoch, dass das Meldungsprotokoll momentan nach dem Trennen der Verbindung nicht mehr verfügbar ist.

So simulieren Sie das Lesen der Meldungen eines Tags oder das Empfangen der Meldungen eines anderen Geräts:

-   Aktivieren Sie das Optionsfeld **Send to self** (An mich senden), um Szenarien mit nur einem NFC-fähigen Gerät zu testen.
-   Klicken Sie auf die Schaltfläche **Tap** (Berühren). Dadurch wird die Berührung eines Geräts mit einem Tag simuliert und der entsprechende NFC-Benachrichtigungssound ausgegeben.
-   Klicken Sie zum Trennen der Verbindung einfach auf die Schaltfläche **Untap** (Trennen).

Im Näherungsmodus können Sie Meldungen von einem Tag oder von einem anderen Peergerät simulieren. Folgende Meldungstypen können mit dem Tool gesendet werden:

-   WindowsURI
-   WindowsMime
-   WritableTag
-   Pairing:Bluetooth
-   NDEF
-   NDEF:MIME
-   NDEF:URI
-   NDEF:wkt.U

Zum Erstellen dieser Meldungen können Sie entweder die Nutzlastfenster**** bearbeiten oder die Meldungen in einer Datei bereitstellen. Weitere Informationen zu diesen Typen und zu deren Verwendung finden Sie im Anmerkungsabschnitt der[**ProximityDevice.PublishBinaryMessage**](https://msdn.microsoft.com/library/windows/apps/Hh701129)-Referenzseite.

Das Windows 8-Treiberkit (Windows Driver Kit, WDK) enthält ein Treiberbeispiel, in dem das gleiche Protokoll verfügbar gemacht wird wie im Windows Phone 8-Emulator. Laden Sie das WDK herunter, erstellen Sie den Beispieltreiber, installieren Sie ihn auf einem Gerät unter Windows 8, und fügen Sie anschließend die IP-Adresse oder den Hostnamen des Geräts der Geräteliste hinzu, oder koppeln Sie es entweder mit einem anderen Gerät unter Windows 8 oder mit einem Windows Phone 8-Emulator.

### HCE-Modus (Host Card Emulation)

Im HCE-Modus (Host Card Emulation) können Sie Ihre HCE-basierte Kartenemulationsanwendung mit selbst erstellten Skripts testen, um ein Smartcardleseterminal (beispielsweise ein POS-Terminal) zu simulieren. Dieses Tool setzt voraus, dass Sie mit den ISO-7816-4-konformen Befehl/Antwort-Paaren vertraut sind, die zwischen einem Leseterminal (wie etwa einem POS-, Ausweis- oder Transit-Kartenleser) und der (in Ihrer Anwendung emulierten) Smartcard übermittelt werden.

![Die HCE-Seite für NFC](images/emulator-nfc-hce.png)

-   Klicken Sie im Skript-Editor-Abschnitt auf die Schaltfläche **Hinzufügen**, um ein neues Skript zu erstellen. Nach der Bearbeitung können Sie einen Namen für Ihr Skript angeben und es durch Klicken auf die Schaltfläche **Speichern** speichern.
-   Gespeicherte Skripts sind beim nächsten Start des Emulators verfügbar.
-   Führen Sie Ihre Skripts aus, indem Sie im Skript-Editor-Fenster auf die Wiedergabeschaltfläche**** klicken. Mit dieser Aktion wird eine Berührung zwischen Smartphone und Terminal sowie das Senden von Befehlen aus Ihrem Skript simuliert. Alternativ können Sie auf die Schaltfläche **Tap** (Berühren) und anschließend auf die Wiedergabeschaltfläche**** tippen. Das Skript wird erst ausgeführt, wenn Sie auf die Wiedergabeschaltfläche**** tippen.
-   Durch Klicken auf die Schaltfläche **Beenden** wird die Befehlsübermittlung an Ihre Anwendung beendet. Die Geräte bleiben jedoch gekoppelt, bis Sie auf die Schaltfläche **Untap** (Trennen) klicken.
-   Wenn Sie ein Skript löschen möchten, wählen Sie es im Dropdownmenü aus, und klicken Sie anschließend auf die Schaltfläche **Löschen**.
-   Die Syntax Ihrer Skripts wird vom Emulator-Tool erst überprüft, wenn Sie das Skript mithilfe der Wiedergabeschaltfläche**** ausführen. Die vom Skript gesendeten Meldungen sind abhängig von der Implementierung Ihrer Kartenemulations-App.

Zahlungs-Apps können auch mithilfe des Terminalsimulators von MasterCard ([https://www.terminalsimulator.com/](https://www.terminalsimulator.com/ )) getestet werden.

-   Aktivieren Sie unterhalb des Skript-Editor-Fensters das Kontrollkästchen für den MasterCard-Listener****, und starten Sie den Simulator von MasterCard.
-   Mithilfe dieses Tools können Sie Befehle generieren, die dann über das NFC-Tool an Ihre im Emulator ausgeführte Anwendung weitergeleitet werden.

Weitere Informationen zur HCE-Unterstützung sowie zur Entwicklung von HCE-Apps in Windows 10 Mobile finden Sie im [Blog des NFC-Teams von Microsoft](http://go.microsoft.com/fwlink/?LinkId=534749).

### So wird's gemacht: Erstellen von Skripts für HCE-Tests

Die Skripts werden als C#-Code geschrieben, und die Run-Methode Ihres Skripts wird durch Klicken auf die Wiedergabeschaltfläche**** aufgerufen. Diese Methode verwendet eine IScriptProcessor-Schnittstelle, um APDU Befehle zu senden/empfangen, im Protokollfenster auszugeben und das Zeitlimit für eine APDU-Antwort des Smartphones zu steuern.

Im Anschluss finden Sie Informationen zu den verfügbaren Funktionen:

```csharp     
        public interface IScriptProcessor
        {
            // Sends an APDU command given as a hex-encoded string, and returns the APDU response
            string Send(string s);

            // Sends an APDU command given as a byte array, and returns the APDU response
            byte[] Send(byte[] s);

            // Logs a string to the log window
            void Log(string s);

            // Logs a byte array to the log window
            void Log(byte[] s);

            // Sets the amount of time the Send functions will wait for an APDU response, after which
            // the function will fail
            void SetResponseTimeout(double seconds);
        }
```

### Smartcardlesemodus

Der Emulator kann mit einem Smartcard-Lesegerät Ihres Hostcomputers verbunden werden. Dadurch erscheinen eingelegte oder in die Nähe gehaltene Smartcards in Ihrer Smartphone-Anwendung, und die APDU-basierte Kommunikation über die [**Windows.Devices.SmartCards.SmartCardConnection**](https://msdn.microsoft.com/library/windows/apps/Dn608002) -Klasse wird ermöglicht. Hierzu muss Ihr Computer über ein kompatibles Smartcard-Lesegerät verfügen. USB-Smartcardleser mit NFC (kontaktlos) und Einschub (Kontakt) sind weit verbreitet. Um die Verwendung eines angeschlossenen Smartcardlesers mit dem Emulator zu ermöglichen, wählen Sie zunächst den Kartenlesermodus**** aus. Daraufhin erscheint eine Dropdownliste mit allen angeschlossenen und kompatiblen Smartcardlesern des Hostsystems, in der Sie den gewünschten Smartcardleser auswählen können.

Beachten Sie, dass einige NFC-fähige Smartcardleser bestimmte Arten von NFC-Karten und die APDU-Standardbefehle für PC/SC-Speicherkarten nicht unterstützen.

## Multitoucheingabe

Simulieren Sie Multitoucheingaben für das Zusammendrücken und Zoomen, Drehen und Verschieben von Objekten mithilfe der Schaltfläche **Multi-touch Input** auf der Symbolleiste des Emulators. Dieses Feature ist hilfreich, wenn Ihre App Fotos, Karten oder andere visuelle Elemente anzeigt, die Benutzer zusammendrücken und vergrößern/verkleinern, drehen und verschieben können.

1.  Tippen Sie auf der Symbolleiste des Emulators auf die Schaltfläche **Multitoucheingabe**, um die Multitoucheingabe zu aktivieren. Auf dem Bildschirm des Emulators erscheinen zwei Berührungspunkte, die um einen Mittelpunkt angeordnet sind.
2.  Klicken Sie mit der rechten Maustaste auf einen der Berührungspunkte, und ziehen Sie ihn an die gewünschte Stelle, um die Punkte ohne Bildschirmberührung zu positionieren.
3.  Klicken Sie mit der linken Maustaste auf einen der Berührungspunkte, und ziehen Sie ihn, um das Zusammendrücken und Zoomen bzw. das Drehen oder Verschieben zu simulieren.
4.  Tippen Sie auf der Symbolleiste des Emulators auf die Schaltfläche für die Einzeltoucheingabe****, um zur normalen Eingabe zurückzukehren.

Der folgende Screenshot zeigt die Multitoucheingabe:

1.  Das kleine Bild links zeigt die Schaltfläche **Multi-touch Input** auf der Symbolleiste des Emulators.
2.  Das mittlere Bild zeigt den Bildschirm des Emulators mit den Berührungspunkten nach dem Tippen auf die Schaltfläche **Multi-touch Input**.
3.  Das rechte Bild zeigt den Bildschirm des Emulators, nachdem die Berührungspunkte gezogen wurden, um das Bild zu vergrößern/verkleinern.

![Multitoucheingabe-Option auf der Symbolleiste des Emulators](images/em-multipoint.png)

## Beschleunigungsmesser

Verwenden Sie die Registerkarte **Accelerometer** der zusätzlichen Tools**** des Emulators, um Apps zu testen, die die Bewegungen des Smartphones nachverfolgen.

Sie können den Beschleunigungsmesser mit einer Liveeingabe oder mit einer zuvor aufgezeichneten Eingabe testen. Als aufgezeichnete Daten ist nur das simulierte Schütteln des Smartphones verfügbar. Für den Beschleunigungsmesser können keine eigenen Simulationen aufgezeichnet oder gespeichert werden.

1.  Wählen Sie die gewünschte Ausgangsausrichtung in der Dropdownliste **Orientation** aus.

2.  -   Wählen Sie die Art der Eingabe aus.

        **So führen Sie die Simulation mit einer Liveeingabe durch**

        Ziehen Sie den farbigen Punkt in der Mitte des Beschleunigungsmessersimulators, um eine Bewegung des Geräts in einer 3D-Ebene zu simulieren.

        Wenn Sie den Punkt entlang der horizontalen Achse bewegen, dreht sich der Simulator hin und her. Wenn Sie den Punkt entlang der vertikalen Achse bewegen, bewegt sich der Simulator vor und zurück. (Mit anderen Worten: Er dreht sich um die X-Achse.) Während der Bewegung des Punkts werden die X-, Y- und Z-Koordinaten auf der Grundlage der Drehungsberechnungen aktualisiert. Sie können den Punkt nicht außerhalb des Begrenzungskreises im Touchpadbereich bewegen.

        Optional: Klicken Sie auf **Reset**, um die Ausgangsausrichtung wiederherzustellen.

    -   **So führen Sie die Simulation mit einer aufgezeichneten Eingabe durch**

        Klicken Sie im Abschnitt **Recorded Data** auf die Schaltfläche **Play**, um die Wiedergabe der simulierten Daten zu starten. Die Liste **Recorded Data** enthält nur die Option für Schütteln. Während der Wiedergabe der Daten bewegt sich der Simulator auf dem Bildschirm nicht.

![Seite "Accelerometer" in den zusätzlichen Tools des Emulators](images/em-accelerometer.png)

## Position und Fahrt

Verwenden Sie die Registerkarte **Location** der zusätzlichen Tools**** des Emulators, um Apps mit Navigationsfunktionen oder Geofencing zu testen. Dieses Feature ermöglicht das Simulieren der Fortbewegung per Auto, Fahrrad oder zu Fuß unter praxisnahen Bedingungen.

Sie können während des App-Tests Ortswechsel mit unterschiedlichen Geschwindigkeiten und Genauigkeitsprofilen simulieren. Mithilfe des Positionssimulators können Sie Veränderungen bei der Nutzung der Standort-API ermitteln, die die Benutzerfreundlichkeit erhöhen. So können Sie mithilfe des Tools beispielsweise feststellen, ob Sie zur erfolgreichen Erkennung von Geofences in unterschiedlichen Szenarien die Geofence-Parameter (wie Größe oder Verweildauer) justieren müssen.

Die Registerkarte **Location** unterstützt drei Modi. In allen Modi gilt: Wenn der Emulator eine neue Position erhält, kann mit dieser Position das [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/BR225540)-Ereignis ausgelöst oder auf einen [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/Hh973536)-Anruf in Ihrer App mit Positionsbestimmung reagiert werden.

-   Im Modus **Pin** markieren Sie Orte auf der Karte. Wenn Sie auf **Play all points** klicken, sendet der Positionssimulator die Position der einzelnen Markierungen nacheinander an den Emulator und verwendet dabei das im Textfeld **Seconds per pin** angegebene Intervall.

-   Im Modus **Live** markieren Sie Orte auf der Karte. Der Positionssimulator sendet die Position der einzelnen Markierungen sofort an den Emulator, wenn die jeweilige Markierung gesetzt wird.

-   Im Modus **Route** platzieren Sie Wegpunkte auf der Karte, und der Positionssimulator berechnet automatisch eine Route. Entlang der Route befinden sich unsichtbare Markierungen im Abstand von jeweils einer Sekunde. Haben Sie also beispielsweise das Geschwindigkeitsprofil **Walking** mit einer angenommenen Geschwindigkeit von fünf Kilometern pro Stunde ausgewählt, wird alle 1,39 Meter eine unsichtbare Markierung generiert. Wenn Sie auf **Play all points** klicken, sendet der Positionssimulator die Position der einzelnen Markierungen nacheinander an den Emulator und verwendet dabei das Intervall, das sich aufgrund des in der Dropdownliste ausgewählten Geschwindigkeitsprofils ergibt.

In allen Modi des Positionssimulators haben Sie folgende Möglichkeiten:

-   Sie können mithilfe des Suchfelds**** nach einer Position suchen.

-   Sie können die Karte vergrößern**** und verkleinern****.

-   Sie können die aktuellen Datenpunkte in einer XML-Datei speichern und die Datei später laden, um die gleichen Datenpunkte erneut zu verwenden.

-   Sie können den Markierungsmodus ein- und ausschalten**** sowie alle Punkte löschen****.

In den Modi "Pin" und "Route" haben Sie außerdem folgende Möglichkeiten:

-   Speichern einer erstellten Route zur späteren Verwendung.

-   Laden einer zuvor erstellten Route. Sie können sogar Routendateien laden, die mit einer älteren Version des Tools erstellt wurden.

-   Ändern einer Route durch Löschen von Markierungen (im Modus "Pin") oder Wegpunkten (im Modus "Route").

**Genauigkeitsprofile**

Über die Dropdownliste **Accuracy profile** können Sie in jedem Modus des Positionssimulators ein Genauigkeitsprofil auswählen.

| Profil  | Beschreibung                                        |
|----------|----------------------------------------------------|
| Pinpoint | Geht von absolut exakten Positionsdaten aus. Diese Einstellung ist zwar nicht realistisch, aber hilfreich, um die Logik Ihrer App zu testen.  |
| Urban    | Geht davon aus, dass aufgrund von Gebäuden die Anzahl der Satelliten mit Sichtverbindung eingeschränkt ist, gleichzeitig aber eine hohe Dichte an Mobilfunkmasten und WLAN-Zugriffspunkten herrscht, die zur Positionsbestimmung herangezogen werden können. |
| Suburban | Geht davon aus, dass die Positionsbestimmung per Satellit relativ gut funktioniert und dass ein ausreichend dichtes Netz von Mobilfunkmasten vorhanden, die Dichte von WLAN-Zugriffspunkten aber nicht besonders hoch ist.  |
| Rural    | Geht davon aus, dass die Positionsbestimmung per Satellit gut funktioniert, aber nur wenige Mobilfunkmasten und nahezu gar keine WLAN-Zugriffspunkte für die Positionsbestimmung zur Verfügung stehen. |

**Geschwindigkeitsprofile**

Im Modus **Route** können Sie über die Dropdownliste eines der folgenden Geschwindigkeitsprofile auswählen:

| Profil | Geschwindigkeit pro Stunde               | Geschwindigkeit pro Sekunde | Beschreibung | 
|---------|------------------------------|------------------|-------------|
| Speed Limit | Die Geschwindigkeitsbegrenzung der Route. | Nicht zutreffend   | Die Route wird unter Berücksichtigung der geltenden Geschwindigkeitsbegrenzung durchlaufen. |
| Walking     | 5 km/h                   | 1,39 m/s           | Die Route wird mit einer natürlichen Schrittgeschwindigkeit von 5 km/h durchlaufen. |
| Biking      | 25 km/h                  | 6,94 m/s           | Die Route wird mit einer natürlichen Radfahrergeschwindigkeit von 25 km/h durchlaufen. |
| Fast        |                          |                  |Die Route wird ohne Berücksichtigung der geltenden Geschwindigkeitsbegrenzung durchlaufen. | 

**Modus "Route"**

Der Modus "Route" besitzt folgende Features und Einschränkungen.

-   Der Modus "Route" erfordert eine Internetverbindung.

-   Bei Verwendung des Genauigkeitsprofils "Urban", "Suburban" oder "Rural" berechnet der Positionssimulator für jede Markierung eine simulierte satellitenbasierte Position, eine simulierte WLAN-basierte Position und eine simulierte mobilfunkbasierte Position. Ihre App erhält lediglich eine dieser Positionen. Die drei Koordinatensätze für die aktuelle Position werden auf der Karte und in der Liste **Current location** in unterschiedlichen Farben angezeigt.

-   Die Genauigkeit der Markierungen entlang der Route ist nicht einheitlich. Für einige der Markierungen wird die Satellitengenauigkeit, für andere die WLAN- oder die Mobilfunkgenauigkeit verwendet.

-   Für die Route können maximal 20 Wegpunkte ausgewählt werden.

-   Positionen für die sichtbaren und unsichtbaren Markierungen auf der Karte werden nur generiert, wenn Sie ein neues Genauigkeitsprofil auswählen. Wenn Sie die Route innerhalb einer Emulatorsitzung mehrmals mit dem gleichen Genauigkeitsprofil durchlaufen, werden dabei die zuvor generierten Positionen wiederverwendet.

Der folgende Screenshot zeigt den Modus "Route": Die orangefarbene Linie ist die Route. Der blaue Punkt ist die per Satellit bestimmte, exakte Position des Fahrzeugs. Die roten und grünen Punkte sind weniger genaue Positionen, die mithilfe der Positionsbestimmung per WLAN und Mobilfunk und unter Verwendung des Genauigkeitsprofils "Suburban" berechnet wurden. Die drei berechneten Positionen werden auch in der Liste **Current location** angezeigt.

![Seite "Location" in den zusätzlichen Tools des Emulators](images/em-drive.png)

**Weitere Informationen zum Positionssimulator**

-   Sie können eine Position mit der standardmäßigen Genauigkeitseinstellung anfordern. Die Einschränkung, die in der Windows Phone 8-Version des Positionssimulators galt und beim Anfordern einer Position die Verwendung der Genauigkeitseinstellung "High" erforderte, wurde aufgehoben.

-   Erstellen Sie zum Testen des Geofencings im Emulator eine Simulation, die dem Geofencing-Modul eine Aufwärmphase zugesteht, sodass das Modul die Bewegungsmuster analysieren und sich darauf einstellen kann.

-   Die einzigen simulierten Positionseigenschaften sind Breitengrad, Längengrad, Genauigkeit und Positionsquelle. Andere Eigenschaften wie Geschwindigkeit, Richtung usw. werden vom Positionssimulator nicht simuliert.

## Netzwerk

Verwenden Sie die Registerkarte **Netzwerk** der zusätzlichen Tools**** des Emulators, um Ihre App mit unterschiedlicher Netzwerkgeschwindigkeit und unterschiedlicher Signalstärke zu testen. Dieses Feature ist hilfreich, wenn Ihre App Webdienste aufruft oder Daten überträgt.

Mit dem Netzwerksimulationsfeature können Sie sicherstellen, dass Ihre App in der Praxis gut funktioniert. Der Windows Phone-Emulator wird in der Regel auf einem Computer mit schneller WLAN- oder Ethernetverbindung ausgeführt. Ihre App wird dagegen auf Smartphones ausgeführt, die üblicherweise über eine langsamere Mobilfunkverbindung verfügen.

1.  Aktivieren Sie das Kontrollkästchen **Netzwerksimulation aktivieren**, um Ihre App mit unterschiedlicher Netzwerkgeschwindigkeit und unterschiedlicher Signalstärke zu testen.
2.  Wählen Sie in der Dropdownliste **Netzwerkgeschwindigkeit** eine der folgenden Optionen aus:
    -   Kein Netzwerk
    -   2G
    -   3G
    -   4G

3.  Wählen Sie in der Dropdownliste **Signalstärke** eine der folgenden Optionen aus:
    -   Gut
    -   Mittelmäßig
    -   Schlecht

4.  Deaktivieren Sie das Kontrollkästchen **Enable network simulation**, um zum Standardverhalten zurückzukehren und wieder die Netzwerkeinstellungen des Entwicklungscomputers zu verwenden.

Die Registerkarte **Network** gibt auch Aufschluss über die aktuellen Netzwerkeinstellungen.

![Seite "Network" in den zusätzlichen Tools des Emulators](images/em-network.png)

## SD-Karte

Verwenden Sie die Registerkarte **SD-Karte** der zusätzlichen Tools**** des Emulators, um Ihre App mit einer simulierten, austauschbaren SD-Karte zu testen. Dieses Feature ist hilfreich, wenn Ihre App Dateien liest oder schreibt.

![Seite „SD-Karte“ in den zusätzlichen Tools des Emulators](images/em-sdcard.png)

Die Registerkarte **SD-Karte** simuliert mithilfe eines Ordner auf dem Entwicklungscomputer eine austauschbare SD-Karte im Smartphone.

1.  **Wählen Sie einen Ordner aus**.

    Klicken Sie auf **Browse**, um auf dem Entwicklungscomputer einen Ordner für die Inhalte der simulierten SD-Karte auszuwählen.

2.  **Setzen Sie die SD-Karte ein**.

    Klicken Sie nach dem Auswählen eines Ordners auf **Insert SD card**. Nach dem Einsetzen der SD-Karte kann Folgendes passieren:

    -   Falls Sie keinen oder einen ungültigen Ordner angegeben haben, tritt ein Fehler auf.
    -   Die Dateien aus dem angegebenen Ordner auf dem Entwicklungscomputer werden in den Stammordner der simulierten SD-Karte des Emulators kopiert. Eine Statusanzeige informiert über den Status der Synchronisierung.
    -   Die Schaltfläche **Insert the SD card** wird zu **Eject SD card**.
    -   Wenn Sie während des Synchronisierungsvorgangs auf **Eject SD card** klicken, wird der Vorgang abgebrochen.

3.  Optional: Aktivieren oder deaktivieren Sie das Kontrollkästchen **Sync updated files back to the local folder when I eject the SD card**.

    Diese Option ist standardmäßig aktiviert. Wenn diese Option aktiviert ist, werden Dateien aus dem Emulator beim Auswerfen der SD-Karte wieder mit dem Ordner auf dem Entwicklungscomputer synchronisiert.

4.  **Werfen Sie die SD-Karte aus**.

    Klicken Sie auf **Eject SD card**. Nach dem Auswerfen der SD-Karte kann Folgendes passieren:

    -   Wenn Sie das Kontrollkästchen **Sync updated files back to the local folder when I eject the SD card** aktiviert haben, passiert Folgendes:
        -   Die Dateien auf der simulierten SD-Karte des Emulators werden in den angegebenen Ordner auf dem Entwicklungscomputer kopiert. Eine Statusanzeige informiert über den Status der Synchronisierung.
        -   Die Schaltfläche **Eject SD card** wird zu **Cancel sync**.
        -   Wenn Sie während des Synchronisierungsvorgangs auf **Synchronisierung abbrechen** klicken, wird die Karte ausgeworfen, und die Ergebnisse des Synchronisierungsvorgangs sind unvollständig.
    -   Die Schaltfläche **Eject SD card** wird wieder zu **Insert SD card**.

> **Hinweis**  Da die vom Smartphone verwendete SD-Karte mit dem FAT32-Dateisystem formatiert ist, beträgt die maximale Dateigröße 32 GB.

Die Geschwindigkeit von Lese- und Schreibvorgängen wird für die simulierte SD-Karte realistisch gedrosselt. Der Zugriff auf eine SD-Karte dauert länger als der Zugriff auf die Festplatte des Computers.

## Benachrichtigungen

Verwenden Sie die Registerkarte **Benachrichtigungen** der **zusätzlichen Tools** des Emulators, um Pushbenachrichtigungen an Ihre App zu senden. Dieses Feature ist hilfreich, wenn Ihre App Pushbenachrichtigungen empfängt.

Sie können ganz einfach Pushbenachrichtigungen testen, ohne den funktionsfähigen Clouddienst zu erstellen, der nach der Veröffentlichung Ihrer App benötigt wird.

1.  **Aktivieren Sie die Simulation.**

    Wenn Sie **Enabled** auswählen, verwenden alle im Emulator bereitgestellten Apps anstelle der WNS oder des MPN-Diensts das Simulationsmodul, bis Sie die Simulation wieder deaktivieren.

2.  **Wählen Sie eine App für den Empfang von Benachrichtigungen aus.**

    Die Liste **AppId** wird automatisch mit allen Apps aufgefüllt, die für den Emulator bereitgestellt wurden und für Pushbenachrichtigungen geeignet sind. Wählen Sie in der Dropdownliste eine App aus.

    Falls Sie nach der Simulationsaktivierung eine weitere, für Pushbenachrichtigungen geeignete App bereitgestellt haben, klicken Sie auf **Aktualisieren**, um die App der Liste hinzuzufügen.

3.  **Wählen Sie einen Benachrichtigungskanal aus.**

    Nachdem Sie in der Liste **AppId** eine App ausgewählt haben, wird die Liste **URI** automatisch mit allen Benachrichtigungskanälen aufgefüllt, die für die ausgewählte App registriert sind. Wählen Sie in der Dropdownliste einen Benachrichtigungskanal aus.

4.  **Wählen Sie einen Benachrichtigungstyp aus.**

    Nachdem Sie in der Liste **URI** einen Benachrichtigungskanal ausgewählt haben, wird die Liste **Notification Type** automatisch mit allen Typen aufgefüllt, die für den Benachrichtigungsdienst verfügbar sind. Wählen Sie in der Dropdownliste einen Benachrichtigungstyp aus.

    Der Simulator ermittelt anhand des URI-Formats des Benachrichtigungskanals, ob die App WNS- oder MPN-Pushbenachrichtigungen verwendet.

    Die Simulation unterstützt alle Benachrichtigungstypen. Der standardmäßige Benachrichtigungstyp ist **Tile**.

    -   Folgende WNS-Benachrichtigungstypen werden unterstützt:

        -   Raw
        -   Toast

            Wenn Ihre App WNS-Benachrichtigungen verwendet und Sie den Benachrichtigungstyp **Toast** auswählen, enthält die Simulationsregisterkarte die Felder **Tag** und **Group**. Sie können diese Optionen aktivieren und Werte für **Tag** und **Group** eingeben, um Popupbenachrichtigungen im Benachrichtigungscenter zu verwalten.

        -   Tile
        -   Badge

    -   Folgende MPN-Benachrichtigungstypen werden unterstützt:

        -   Raw
        -   Toast
        -   Tile

5.  **Wählen Sie eine Benachrichtigungsvorlage aus.**

    Nachdem Sie in der Liste **Notification Type** einen Benachrichtigungstyp ausgewählt haben, wird die Liste **Templates** automatisch mit allen Vorlagen aufgefüllt, die für den Benachrichtigungstyp verfügbar sind. Wählen Sie in der Dropdownliste eine Vorlage aus.

    Die Simulation unterstützt alle Vorlagentypen.

6.  **Optional: Ändern Sie die Benachrichtigungsnutzlast.**

    Nachdem Sie in der Liste **Templates** eine Vorlage ausgewählt haben, wird das Feld **Notification Payload** automatisch mit einer Beispielnutzlast für die Vorlage aufgefüllt. Sehen Sie sich die Beispielnutzlast im Textfeld **Notification Payload** an.

    -   Sie können die Beispielnutzlast unverändert senden.

    -   Sie können die Beispielnutzlast im Textfeld ändern.

    -   Sie können auf **Load** klicken, um eine Nutzlast aus einer Text- oder XML-Datei zu laden.

    -   Sie können auf **Save** klicken, um den XML-Text der Nutzlast zur späteren Verwendung zu speichern.

    Der Simulator überprüft den XML-Text der Nutzlast nicht.

7.  **Senden Sie die Pushbenachrichtigung.**

    Klicken Sie auf **Send**, um die Pushbenachrichtigung an die ausgewählte App zu senden.

    Auf dem Bildschirm erscheint eine Meldung, die darüber informiert, ob der Vorgang erfolgreich war.

![Seite "Notifications" in den zusätzlichen Tools des Emulators](images/em-notifications.png)

## Sensoren

Verwenden Sie die Registerkarte **Sensors** der zusätzlichen Tools**** des Emulators, um zu testen, wie Ihre App auf preisgünstigen Smartphones funktioniert, die nicht über alle optionalen Sensoren oder Kamerafeatures verfügen. Dieses Feature ist hilfreich, wenn Ihre App die Kamera oder Sensoren des Smartphones verwendet und Sie mit Ihrer App den größtmöglichen Markt erreichen möchten.

-   Standardmäßig sind alle Sensoren in der Liste **Optional sensors** aktiviert. Aktivieren oder deaktivieren Sie einzelne Kontrollkästchen, um einzelne Sensoren zu aktivieren oder zu deaktivieren.
-   Klicken Sie auf **Apply**, nachdem Sie Ihre Auswahl geändert haben. Anschließend müssen Sie den Emulator neu starten.
-   Wenn Sie Änderungen vornehmen und anschließend die Registerkarte wechseln oder das Fenster mit den zusätzlichen Tools**** schließen, ohne auf **Apply** zu klicken, werden Ihre Änderungen verworfen.
-   Ihre Einstellungen bleiben zwischen Emulatorsitzungen erhalten, bis Sie sie ändern oder zurücksetzen. Wenn Sie einen Prüfpunkt aufzeichnen, werden die Einstellungen zusammen mit dem Prüfpunkt gespeichert. Die Einstellungen bleiben nur für den spezifischen Emulator erhalten, den Sie verwenden – also beispielsweise für **Emulator 8.1 WVGA 4" 512MB**.

![Seite "Sensors" in den zusätzlichen Tools des Emulators](images/em-sensors.png)

**Sensoroptionen**

Sie können folgende optionale Hardwaresensoren aktivieren oder deaktivieren:

-   Umgebungslichtsensor
-   Nach vorne gerichtete Kamera
-   Gyroskop
-   Kompass (Magnetometer)
-   NFC
-   Softwareschaltflächen (nur bei einigen hochauflösenden Emulatorbildern)

**Kameraoptionen**

Sie können die optionale, nach vorne gerichtete Kamera aktivieren oder deaktivieren, indem Sie in der Liste **Optional sensors** das entsprechende Kontrollkästchen aktivieren oder deaktivieren.

Außerdem können Sie in der Dropdownliste **Camera** eines der folgenden Kameraprofile auswählen:

-   Windows Phone 8.0 camera.
-   Windows Phone 8.1 camera.

Im Anschluss folgt eine Liste mit den Kamerafeatures, die von den einzelnen Profilen unterstützt werden:

| Feature            | Windows Phone 8.0 camera | Windows Phone 8.1 camera  |
|--------------------|--------------------------|---------------------------|
| Auflösung         | 640 x 480 (VGA)          | 640 x 480 (VGA) oder höher |
| Autofokus          | Ja                      | Ja                       |
| Blitz              | Nein                       | Ja                       |
| Zoom               | 2x (digital oder optisch)  | 2x (digital oder optisch)   |
| Videoauflösung   | 640 x 480 (VGA)          | 640 x 480 (VGA) oder höher |
| Vorschauauflösung | 640 x 480 (VGA)          | 640 x 480 (VGA)           |

## Bildfrequenzzähler

Mithilfe der Bildfrequenzzähler des Windows Phone-Emulators können Sie die Leistung Ihrer App im Betrieb überwachen.

![Bildfrequenzzähler im Windows Phone-Emulator](images/em-frameratecounters.PNG)

**Beschreibung der Bildfrequenzzähler**

Die folgende Tabelle beschreibt die einzelnen Bildfrequenzzähler:

| Bildfrequenzzähler                           | Beschreibung        |
|----------------------------------------------|--------------------|
| Composition (Render) Thread Frame Rate (FPS) | Die Bildschirmaktualisierungsrate.  |
| User Interface Thread Frame Rate (FPS)       | Die Ausführungsrate des UI-Threads.    |
| Texture Memory Usage                         | Die Video- und Systemspeicherkopien von in der App verwendeten Texturen.    |
| Surface Counter                              | Die Anzahl expliziter Oberflächen, die zur Verarbeitung an die GPU übergeben werden.     |
| Intermediate Surface Counter                 | Die Anzahl impliziter Oberflächen, die aufgrund von zwischengespeicherten Oberflächen generiert wurden.    |
| Screen Fill Rate Counter                     | Die Anzahl von Pixeln, die pro Frame im Sinne von Bildschirmen gezeichnet werden. Der Wert "1" steht für die Anzahl von Pixeln in der aktuellen Bildschirmauflösung (beispielsweise 480 x 800 Pixel). |

**Aktivieren und Deaktivieren der Bildfrequenzzähler**

Sie können die Anzeige der Bildfrequenzzähler in Ihrem Code aktivieren oder deaktivieren. Wenn Sie in Visual Studio ein Windows Phone-App-Projekt erstellen, wird der Datei „App.xaml.cs” automatisch der folgende Code hinzugefügt, um die Bildfrequenzzähler zu aktivieren. Wenn Sie die Bildfrequenzzähler deaktivieren möchten, legen Sie **EnableFrameRateCounter** auf **false** fest, oder kommentieren Sie die Codezeile aus.

> [!div class="tabbedCodeSnippets"]
>```csharp
>// Show graphics profiling information while debugging.
>if (System.Diagnostics.Debugger.IsAttached)
>{
>    // Display the current frame rate counters.
>    Application.Current.Host.Settings.EnableFrameRateCounter = true;
>    
>    // other code…
>}
>```
>```vb
>' Show graphics profiling information while debugging.
>If System.Diagnostics.Debugger.IsAttached Then
>
>    ' Display the current frame rate counters.
>    Application.Current.Host.Settings.EnableFrameRateCounter = True
>
>    ' other code...
>End If
>```

## Bekannte Probleme

Im Folgenden werden bekannte Probleme mit dem Emulator sowie Möglichkeiten beschrieben, diese zu umgehen.

### Fehlermeldung: „Fehler beim Entfernen des virtuellen Ethernet-Switchs.“

In bestimmten Situationen (etwa beim Aktualisieren auf einen neuen Windows 10-Test-Flight), kann es vorkommen, dass ein dem Emulator zugeordneter virtueller Netzwerkswitch in einen Zustand versetzt wird, in dem er nicht mehr über die Benutzeroberfläche gelöscht werden kann.

Führen Sie zum Beheben dieses Problems an einer Eingabeaufforderung mit Administratorrechten den Befehl „Netcfg -d“ aus: `C:\Program Files (x86)\Microsoft XDE\<version>\XdeCleanup.exe`. Starten Sie den Computer nach Ausführung des Befehls neu, um den Wiederherstellungsvorgang abzuschließen.

**Hinweis**  Dieser Befehl löscht nicht nur die mit dem Emulator verknüpften Geräte, sondern alle Netzwerkgeräte. Beim Neustart des Computers werden alle Hardwarenetzwerkgeräte automatisch erkannt.
 
### Die Emulatoren können nicht gestartet werden.

Der Microsoft-Emulator enthält „XDECleanup.exe“ – ein Tool, das alle VMs, differenzierenden Datenträger und emulatorspezifischen Netzwerkswitches löscht. Dieses Tool ist bereits in den Binärdateien (XDE) des Emulators enthalten. Verwenden Sie dieses Tool zum Bereinigen von Emulator-VMs, wenn diese einen fehlerhaften Zustand aufweisen. Führen Sie das Tool über eine Eingabeaufforderung mit Administratorrechten aus: .`C:\Program Files (x86)\Microsoft XDE\<version>\XdeCleanup.exe`

> **Hinweis**  „XDECleanup.exe“ löscht alle emulatorspezifischen Hyper-V-VMs sowie alle VM-Prüfpunkte und gespeicherten Zustände.

### Deinstallieren des Windows 10 Mobile-Image

Wenn Sie den Emulator installieren, wird ein VHD-Image mit Windows 10 Mobile installiert. Dieses erhält einen eigenen Eintrag in der Liste **Programme und Features** in der Systemsteuerung. Wenn Sie das Image deinstallieren möchten, suchen Sie in der Liste mit den installierten Programmen nach **Windows 10 Mobile Image – <version>**, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Deinstallieren** aus.

In der aktuellen Version müssen Sie die VHD-Datei für den Emulator manuell löschen. Wenn Sie den Emulator unter dem Standardpfad installiert haben, befindet sich die VHD-Datei unter „C:\\Programme (x86)\\Windows Kits\\10\\Emulation\\Mobile\\<version>\\flash.vhd“.

###So deaktivieren sie hardwarebeschleunigte Grafiken

Standardmäßig verwendet der Windows 10 Mobile-Emulator hardwarebeschleunigte Grafiken. Wenn Probleme beim Starten des Emulators mit aktivierter Hardwarbeschleunigung auftreten, können Sie diese durch Festlegen eines Registrierungswerts deaktivieren.

So deaktivieren sie die Hardwarbeschleunigung:

1. Starten Sie den Registrierungs-Editor.
2. Erstellen Sie den folgenden Registrierungsunterschlüssel, wenn er nicht vorhanden ist: HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Xde\10.0
3. Klicken Sie auf den Ordner „10.0”, zeigen Sie auf **Neu**, und klicken Sie dann auf **DWORD-Wert**.
4. Geben Sie **DisableRemoteFx** ein, und drücken Sie die EINGABETASTE.
5. Doppelklicken Sie auf **DisableRemoteFx**, geben Sie im Feld **Wert** den Wert 1 ein, wählen Sie die Option **Decimal** aus, und klicken Sie dann auf **OK**.
6. Schließen Sie den Registrierungs-Editor.

**Hinweis:** Nach dem Festlegen dieses Registrierungswerts müssen Sie die virtuelle Maschine im Hyper-V-Manager für diejenige Konfiguration löschen, die Sie in Visual Studio gestartet haben, und dann den Emulator mit Softwarerendering neu starten.

## Supportressourcen

Antworten und Problemlösungen für die Windows 10-Tools finden Sie im [Forum für Windows 10-Tools](http://go.microsoft.com/fwlink/?LinkId=534765). Eine Liste mit allen Foren für Windows 10-Entwickler finden Sie [hier](http://go.microsoft.com/fwlink/?LinkId=535000).

## Verwandte Themen

* [Ausführen von Windows Phone-Apps im Emulator](https://msdn.microsoft.com/library/windows/apps/xaml/dn632391.aspx)
* [Windows und Windows Phone SDK-Archiv](https://dev.windows.com/downloads/sdk-archive)
 




<!--HONumber=Jun16_HO4-->


