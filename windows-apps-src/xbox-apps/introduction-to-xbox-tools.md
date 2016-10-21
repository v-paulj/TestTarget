---
author: Mtoepke
title: "Einführung in Xbox One-Tools"
description: Xbox One-spezifisches Tool Dev Home (unter Verwendung des Windows Device Portal).
translationtype: Human Translation
ms.sourcegitcommit: b3e1a6f1dfe3537d0db8e917163cfbba7b8705fe
ms.openlocfilehash: 6030f666f213865a92d071210fe66f587c1bffb1

---

# Einführung in Xbox One-Tools

In diesem Abschnitt wird das Xbox One-spezifische Tool _Dev Home_ unter Verwendung des Windows Device Portal behandelt.

## Dev Home

_Dev Home_ ist ein Tool im Xbox One Development Kit, das die Produktivität von Entwicklern unterstützen soll. Dev Home bietet Funktionen zum Verwalten und Konfigurieren Ihres Dev Kit.

Wählen Sie zum Öffnen von Dev Home die Kachel **Dev Home** im Startbildschirm. Ist keine Kachel vorhanden, befindet sich die Konsole nicht im Entwicklermodus.

  ![Windows Device Portal](images/windowsdeviceportal_1.png)

### Benutzeroberfläche
Die Dev Home-Benutzeroberfläche ist in die in den folgenden Abschnitten beschriebenen Bereiche unterteilt. Beachten Sie, dass die Konsolen-IP-Adresse und der Anzeigenamen hier angezeigt werden.

  ![DevHome-UI](images/devhome_ui.png)

#### Header
Der Header enthält wichtige globale Informationen über das Dev Kit „auf einen Blick“. Dazu gehören der Konsolenname, die IP-Adresse, die entsprechende Xbox Live-Sandbox und die Version des ausgeführten Betriebssystems. Auf der rechten Seite des Header werden die aktuelle Uhrzeit und das aktuelle Datum des Systems angezeigt.

#### Tool-Fenster
Unter dem Header befindet sich der Bereich der App, der eine Reihe von konfigurierbaren Tool-Fenstern enthält. Diese sollen Entwicklern die Anpassung der App ermöglichen, um Zugriff auf verschiedene Tools und Informationsgruppen zu bieten. Weitere Informationen zu den Tools finden Sie in den folgenden Beschreibungen der einzelnen Tools. Informationen zum Konfigurieren von Layout und Darstellung der Tool-Fenster finden Sie im Abschnitt [Anpassen von Dev Home](#customizing-dev-home) später auf dieser Seite.

#### Hauptmenü
Durch Drücken auf die Schaltfläche **Menü** auf Ihrem Controller oder durch Navigieren zur Menüschaltfläche („Hamburger“) oben links im Bildschirm können Sie auf das Hauptmenü zugreifen, das Ihnen die Konfiguration der Designfarbe und des Hintergrundbilds für den App-Workspace ermöglicht, und Feedback zur App geben.

  ![Hauptmenü](images/devhome_mainmenu.png)

#### Andockmodus
Tools in Dev Home können auf der Seite angedockt werden, während Sie den Titel ausführen. Auf diese Weise haben Sie einfachen Zugriff auf Tools beim Testen.

Um den Modus **Andocken** zu verwenden, wählen Sie den Titel des entsprechenden Tools, drücken Sie die Schaltfläche **Anzeigen** auf Ihrem Controller, und wählen Sie im Kontextmenü **Andocken**.

  ![Andockmodus](images/devhome_snapmode.png)

Dev Home wird rechts angedockt. Sie können den Kontext umschalten, indem Sie wie gewohnt auf die Schaltfläche **Nexus** doppeltippen.

  ![Nexus](images/devhome_nexus.png)

#### Tool-Beschreibungen
| Tool  | Funktionen |
|-------|--------------|
| Spiele und Apps  | Listet die Titel und Anwendungen, die im Dev Kit installiert sind, sowie die Möglichkeit auf, sie schnell zu öffnen. Sie können auch den Status der Prozesslebensdauerverwaltung (PLM) von Spielen und Apps anzeigen und den PLM-Status in einem Kontextmenü ändern. |
| Benutzer | Listet die derzeit in der Konsole registrierten Benutzer auf. Ermöglicht das An- und Abmelden mit einem Mausklick, das Hinzufügen von Benutzern und Gästen und das Anzeigen von Informationen zu Benutzern und Gästen. |
| [Konsoleneinstellungen](#console-settings) | Stellt eine Übersicht und Bearbeitungsoptionen der Konsoleneinstellungen und Informationen bereit. |
| Visual Studio | Ermöglicht es Ihnen, die Konsole mit einer Instanz von Visual Studio zu koppeln, um die Bereitstellung zuzulassen. Deaktivieren Sie ggf. alle vorhandenen gekoppelten VS-Instanzen, um die UWP-App-Bereitstellung für ein Kit zu verhindern. |
| [Windows Device Portal](#windows-device-portal) | Ermöglicht WDP (browserbasiertes Tool zur Geräteverwaltung) im Kit. |
| Xbox Live-Status | Enthält den aktuellen Status des Xbox Live-Diensts. |
<br/>
### Verwalten der Größe der Entwicklerspeicherzuteilung

Um den Festplattenplatz zu vergrößern oder zu verkleinern, der für den Entwicklerspeicher verwendet wird, wählen Sie im Hauptmenü **Dev-Speicher verwalten** aus. Ändern Sie den Wert der Leiste **Dev-Speicher**, und wählen Sie dann **Speichern und neu starten** aus, um die Konsole neu zu starten.

  ![Verwalten der Entwicklerspeicherzuteilung](images/devhome_storage.png)

### Anpassen von Dev Home

Dev Home kann angepasst und personalisiert werden. Sie können ein Hintergrundbild und eine Designfarbe wählen, um Dev Home zu personalisieren. Diese Optionen finden Sie im Hauptmenü.

#### Ändern der Größe und Neuanordnen von Tools
Um die Größe oder Position eines Tools zu ändern, verwenden Sie die Kontextmenüschaltfläche (Schaltfläche **Anzeigen** auf dem Controller), während der Titel im Fokus steht. Wählen Sie im Kontextmenü **Verschieben** oder **Größe ändern**.

  ![Verschieben oder Größe ändern](images/devhome_move.png)

#### Ändern der Designfarbe und des Hintergrundbilds
Sie können im Hauptmenü **Designfarbe ändern** auswählen. Um die Designfarbe zum Hervorheben des Fokus zu aktualisieren, wählen Sie eine neue Farbe, und klicken Sie dann auf **Speichern**.

  ![Ändern der Designfarbe](images/devhome_colors.png)

### Bereitstellen von Feedback
Wählen Sie zum Bereitstellen von Feedback zu Dev Home oder einem beliebigen Tool-Prozess die Option **Feedback bereitstellen** im Hauptmenü.

  ![Feedback bereitstellen](images/devhome_feedback.png)

## Konsoleneinstellungen
Das Tool für Konsoleneinstellungen bietet schnellen Zugriff auf die Einstellungen des Dev Kit.

### Festlegen eines Hostnamens für die Konsole
Bei der Kommunikation mit der Konsole von Ihrem Entwicklungs-PC können Sie einen Anzeigenamen (_Hostname_) für das Xbox One Dev Kit als Alternative für die Konsolen-IP-Adresse festlegen. Ihr Entwicklungs-PC und das Dev Kit müssen sich für Hostnamen-Konnektivität im selben Subnetz befinden.  

Um einen Hostnamen für einen Dev Kit zu definieren, navigieren Sie zum Tool für die Konsoleneinstellungen und geben im Feld __Hostname__ den Hostnamen ein.  

> [!NOTE]
> Die Eindeutigkeit des Namens wird beim Erstellen des Hostnamens nicht erzwungen. Vermeiden Sie doppelte Namen. Eine Möglichkeit ist, den Hostnamen vom Namen Ihres Entwicklungscomputers abzuleiten, der in der Regel eindeutig in einer Organisation ist.

## Windows Device Portal
Das Windows Device Portal (WDP) ist ein OneCore-Tool zur Geräteverwaltung, das eine browserbasierte Geräteverwaltung ermöglicht.

> [!NOTE]
> Weitere Informationen zu WDP finden Sie unter [Übersicht über das Windows Device Portal](../debug-test-perf/device-portal.md).

So aktivieren Sie WDP auf Ihrer Xbox One-Konsole

1. Wählen Sie die Kachel „Dev Home“ auf dem Startbildschirm aus.

  ![Auswählen der Kachel „Dev Home“](images/windowsdeviceportal_1.png)

2. Navigieren Sie in Dev Home zum Tool **Remoteverwaltung**.

  ![Tool „Remoteverwaltung"](images/windowsdeviceportal_2.png)

3. Wählen Sie __Windows Device Portal verwalten__, und drücken Sie __A__.
4. Aktivieren Sie das Kontrollkästchen __Windows Device Portal aktivieren__.
5. Geben Sie __Benutzername__ und __Kennwort__ ein, und speichern Sie. Diese werden zum Authentifizieren des Zugriffs auf Ihr Dev Kit über einen Browser verwendet.
6. Schließen Sie die Seite __Einstellungen__, und notieren Sie sich die URL, die im Tool _Remoteverwaltung_ zum Verbinden aufgelistet ist.
7. Geben Sie die URL in Ihrem Browser ein, und melden Sie sich mit den Anmeldeinformationen an, die Sie konfiguriert haben.
8. Sie erhalten eine Warnung zum Zertifikat, das bereitgestellt wurde, ähnlich wie folgender Screenshot, dass das von Ihrer Xbox One-Konsole signierte Sicherheitszertifikat nicht als bekannter vertrauenswürdiger Herausgeber betrachtet wird. Klicken Sie auf **Laden dieser Website fortsetzen**, um auf das Windows Device Portal zuzugreifen.

  ![Sicherheitszertifikatwarnung](images/security_cert_warning.jpg)

## Begleitung für den Xbox-Entwicklermodus
Die Begleitung für den Xbox-Entwicklermodus ist ein Tool, mit dessen Hilfe Sie auf der Konsole arbeiten können, ohne den PC verlassen zu müssen. Die App ermöglicht Ihnen die Anzeige des Konsolenbildschirms und das Senden von Eingaben an diesen. Weitere Informationen finden Sie unter [Begleitung für den Xbox-Entwicklermodus](xbox-dev-mode-companion.md).

## Siehe auch
- [Verwenden von Fiddler mit Xbox One bei der Entwicklung für UWP](uwp-fiddler.md)
- [Übersicht über das Windows Device Portal](../debug-test-perf/device-portal.md)
- [UWP auf XboxOne](index.md)


----



<!--HONumber=Sep16_HO1-->


