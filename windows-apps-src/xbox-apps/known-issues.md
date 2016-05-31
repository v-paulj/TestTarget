---
author: Mtoepke
title: Bekannte Probleme mit UWP auf Xbox One Developer Preview
description: 
area: Xbox
---

# Bekannte Probleme mit UWP auf Xbox Developer Preview

Dieses Thema beschreibt bekannte Probleme mit der UWP auf Xbox Developer Preview. 
Weitere Informationen zu dieser Developer Preview finden Sie unter [UWP auf Xbox](index.md). 

\[Wenn Sie über einen Link in einem API-Referenzthema hierher gelangten und nach Informationen zu APIs für die Universal-Gerätefamilie suchen, lesen Sie bitte [UWP-Funktionen, die noch nicht auf Xbox One unterstützt werden](http://go.microsoft.com/fwlink/?LinkID=760755).\]

Das Xbox Developer Preview System Update ist eine Vorabversion der Software für Testzwecke. 
Dies bedeutet, dass einige beliebte Spiele und Apps nicht wie erwartet funktionieren und gelegentlich Abstürze und Datenverlust auftreten. 
Wenn Sie die Developer Preview verlassen, wird Ihre Konsole auf die Werkseinstellungen zurückgesetzt, und Sie müssen alle Spiele, Apps und Inhalte erneut installieren.

Für Entwickler bedeutet dies, dass nicht alle Entwickler-Tools und -APIs wie erwartet funktionieren. 
Nicht alle für die endgültige Version vorgesehenen Features sind enthalten oder weisen die endgültige Qualität auf. 
**Insbesondere spiegelt die Systemleistung in dieser Preview nicht die Systemleistung der endgültigen Version wider.**

In der folgenden Liste werden einige bekannte Probleme aufgelistet, die in dieser Version auftreten können. Diese Liste ist jedoch nicht vollständig. 

**Wir möchten gerne Ihr Feedback erhalten.** Melden Sie daher alle festgestellten Probleme im Forum für das [Entwickeln von universellen Windows-Apps](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

Wenn Sie Probleme haben, lesen Sie die Informationen in diesem Thema, informieren Sie sich in den [häufig gestellten Fragen](frequently-asked-questions.md), und nutzen Sie die Foren, um Hilfe zu erhalten.


## Spieleentwicklung

### x86 im Vergleich zu x64

Zum Zeitpunkt der Veröffentlichung später in diesem Jahr werden x86 und x64 unterstützt. In dieser Preview wird x86 unterstützt. 
Allerdings wurde x64 bisher ausgiebiger getestet (Xbox-Shell und alle Apps auf der Konsole = x64). Daher empfehlen wir die Verwendung von x64 für Ihre Projekte. 
Dies gilt insbesondere für Spiele.

Wenn Sie x86 verwenden möchten, melden Sie Probleme im Forum.

Weitere Informationen finden Sie auch unter [Wechsel zwischen Build-Varianten kann Bereitstellungsfehler verursachen](known-issues.md#switching-build-flavors-can-cause-deployment-failures) später auf dieser Seite.

### Spiele-Engines

Wir haben einige verbreitete Spiele-Engines getestet, und unser Testabdeckung für diese Preview war nicht umfassend. 
Ihre Erfahrungen können hiervon abweichen. 

Die folgenden Spiele-Engines funktionieren nachweislich:
* [Construct 2](https://www.scirra.com/)

Wahrscheinlich gibt es weitere funktionierende Spiele-Engines. Wir würden gerne Ihr Feedback hierzu erhalten. 
Verwenden Sie das Forum, um Probleme zu melden.

### DirectX 12-Unterstützung

UWP auf Xbox One unterstützt die DirectX 11-Featureebene 10. 
DirectX 12 wird derzeit nicht unterstützt. 
Xbox One ist wie alle herkömmlichen Spielekonsolen ein spezielles Hardwaregerät, das ein spezielles SDK erfordert, um das Potenzial vollständig nutzen zu können. 
Wenn Sie an einem Spiel arbeiten, das Zugriff auf das maximale Potenzial der Xbox One-Hardware benötigt, können Sie sich beim [ID@XBOX](http://www.xbox.com/en-us/Developers/id)-Programm registrieren, um Zugriff auf das SDK zu erhalten, das DirectX 12-Unterstützung enthält.

### Die Xbox One Developer Preview deaktiviert das Streaming von Spielen für Windows 10

Das Aktivieren der Xbox One Developer Preview auf Ihrer Konsole verhindert das Streaming von Spielen von Ihrer Xbox One an die Xbox-App unter Windows 10. Dies gilt auch dann, wenn für Ihre Konsolen der Retailmodus festgelegt ist. Um das Feature für das Streaming von Spielen wiederherzustellen, müssen Sie die Developer Preview verlassen.

### Bekanntes Problem mit dem sicheren TV-Bereich

Standardmäßig sollte der Anzeigebereich für UWP-Apps auf Xbox durch den sicheren TV-Bereich abgesenkt werden. Die Xbox One Developer Preview enthält jedoch ein bekanntes Problem, das dazu führt, dass der sichere TV-Bereich bei [0, 0] und nicht bei [_offset_, _offset_] beginnt.

Weitere Informationen zum sicheren TV-Bereich finden Sie unter [https://msdn.microsoft.com/windows/uwp/input-and-devices/designing-for-tv](https://msdn.microsoft.com/windows/uwp/input-and-devices/designing-for-tv). 

Die einfachste Möglichkeit, dieses Problem zu umgehen, besteht darin, den sicheren TV-Bereich zu deaktivieren, wie im folgenden JavaScript-Beispiel gezeigt.

    var applicationView = Windows.UI.ViewManagement.ApplicationView.getForCurrentView();

    applicationView.setDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.useCoreWindow);

### Der Mausmodus wird noch nicht unterstützt.

Das _mouse mode_-Feature, das im Thema [https://msdn.microsoft.com/de-de/windows/uwp/input-and-devices/designing-for-tv] (https://msdn.microsoft.com/de-de/windows/uwp/input-and-devices/designing-for-tv?f=255&MSPPError=-2147217396#mouse-mode) beschrieben wird, wird in der Xbox One Developer Preview noch nicht unterstützt.

## Systemressourcen für UWP-Apps und -Spiele auf Xbox One

UWP-Apps und -Spiele auf Xbox One teilen Ressourcen mit dem System und anderen Apps; d. h., das System steuert die Ressourcen, die für alle Spiele oder Apps verfügbar sind. 
Wenn Arbeitsspeicher- oder Leistungsprobleme auftreten, kann dies der Grund sein. 
Weitere Informationen finden Sie unter [Systemressourcen für UWP-Apps und -Spiele auf Xbox One](system-resource-allocation.md).


## Netzwerke mit herkömmlichen Sockets

In dieser Developer Preview ist eingehender und ausgehender Netzwerkzugriff von der Konsole, die herkömmliche TCP/UDP-Sockets (WinSock, Windows.Networking.Sockets) verwendet, nicht verfügbar. 
Entwickler können weiterhin HTTP und WebSockets verwenden. 


## UWP-API-Abdeckung

Nicht alle UWP-APIs in dieser Preview funktionieren auf Xbox erwartungsgemäß. 
Unter [UWP-Funktionen, die noch nicht auf Xbox One unterstützt werden](http://go.microsoft.com/fwlink/p/?LinkId=760755) finden Sie eine Liste der APIs, von denen bekannt ist, dass sie nicht funktionieren. 
Wenn Probleme mit anderen APIs auftreten, melden Sie dies bitte in den Foren. 

## Aussehen oder Verhalten von XAML-Steuerelementen entspricht nicht den Steuerelementen in der Xbox One-Shell

In dieser Developer Preview sind die XAML-Steuerelemente nicht endgültig. Insbesondere:
* Die Gamepad-X-Y-Navigation funktioniert nicht zuverlässig für alle Steuerelemente.
* Das Aussehen der Steuerelemente entspricht nicht den Steuerelementen in der Xbox-Shell. Dies umfasst das Steuerelement-Fokusrechteck.
* Beim Navigieren zwischen Steuerelementen entstehen nicht automatisch „Navigationstöne“.

Diese Probleme werden in einer zukünftigen Developer Preview behoben.

## Visual Studio und Bereitstellungsprobleme

### Wechsel zwischen Buildvarianten kann Bereitstellungsfehler verursachen

Der Wechsel zwischen Debug und Release Builds oder zwischen x86 und x64 oder zwischen verwalteten und .NET Native-Builds kann Bereitstellungsfehler verursachen. 

Die einfachste Möglichkeit zur Vermeidung dieser Probleme für diese Preview ist, Debug Builds und eine einzige Architektur zu verwenden. 

Wenn dieses Problem auftritt, wird es durch die Deinstallation Ihrer App in der Collections-App auf Ihrer Xbox One in der Regel gelöst.

> **Hinweis**
            &nbsp;&nbsp;Die Deinstallation Ihrer App aus dem Windows Device Portal (WDP) behebt das Problem nicht.

Wenn die Probleme weiterhin bestehen, deinstallieren Sie Ihre Anwendung bzw. Ihr Spiel in der Collections-App, verlassen Sie den Entwicklermodus, starten Sie den Einzelhandelsmodus neu, und wechseln Sie wieder in den Entwicklermodus.

Weitere Informationen finden Sie im Abschnitt „Beheben von Bereitstellungsfehlern“ unter [Häufig gestellte Fragen](frequently-asked-questions.md).

### Deinstallation einer App beim Debuggen in Visual Studio führt zu einem Fehler im Hintergrund

Bei dem Versuch, eine App zu deinstallieren, die im Debugger über das WDP-Tool „Installierte Apps“ ausgeführt wird, schlägt die Deinstallation im Hintergrund fehl. 
Um das Problem zu umgehen, stoppen Sie das Debugging der App in Visual Studio, bevor Sie versuchen, sie über WDP zu entfernen.

### Fehler bei Visual Studio/Xbox-PIN-Kopplung

Es ist möglich, dass ein Zustand eintritt, in dem die PIN-Kopplung zwischen Visual Studio und Ihrer Xbox One nicht mehr synchron ist. 
Wenn die PIN-Kopplung fehlschlägt, verwenden Sie die Schaltfläche „Alle Kopplungen entfernen“ in Dev Home, starten Sie Xbox One neu, starten Sie Ihren Entwicklungs-PC neu, und versuchen Sie es dann erneut. 


## Windows Device Portal (WDP)-Preview

### Dev Home stürzt ab, wenn WDP über Dev Home gestartet wird

Wenn Sie WDP in Dev Home starten, stürzt Dev Home ab, nachdem Sie Ihren Benutzernamen und Ihr Kennwort eingegeben und **Speichern** ausgewählt haben. 
Die Anmeldeinformationen werden gespeichert, aber WDP nicht gestartet. 
Sie können WDP starten, indem Sie Xbox One neu starten. 

### Deaktivieren von WDP in Dev Home funktioniert nicht

Wenn Sie WDP in Dev Home deaktivieren, wird es deaktiviert. 
Wenn Sie Ihre Xbox One jedoch neu starten, wird WDP erneut gestartet. 
Sie können dieses Problem umgehen, indem Sie mit **Zurücksetzen und meine Spiele und Apps beibehalten** jeden gespeicherten Zustand auf Ihrer Xbox One löschen. 
Navigieren Sie zu „Einstellungen > System > Konsoleninfo und -updates > Konsole zurücksetzen“, und wählen Sie anschließend die Schaltfläche **Zurücksetzen und meine Spiele und Apps beibehalten** aus.

> **Achtung**
            &nbsp;&nbsp;Hierdurch werden alle gespeicherten Einstellungen auf Ihrer Xbox One gelöscht, einschließlich drahtloser Einstellungen, Benutzerkonten und Spielestatus, die nicht im Cloudspeicher gespeichert wurden.

> **Achtung**
            &nbsp;&nbsp;Verwenden Sie NICHT die Schaltfläche **Reset and remove everything**.
Hierdurch werden alle Spiele, Apps, Einstellungen und Inhalte gelöscht, der Entwicklermodus deaktiviert und Ihre Konsole aus der Developer Preview entfernt.

### Die Spalten in der Tabelle „Ausgeführte Apps“ werden nicht wie erwartet aktualisiert. 

Manchmal wird dieses Problem gelöst, indem Sie eine Spalte in der Tabelle sortieren.

### Die WDP-UI wird in Internet Explorer 11 nicht ordnungsgemäß angezeigt. 

Standardmäßig wird die WDP-UI nicht ordnungsgemäß im Browser angezeigt, wenn Sie Internet Explorer 11 verwenden. 
Sie können dies durch Deaktivieren der Kompatibilitätsansicht von Internet Explorer 11 für WDP beheben.

### Navigieren zu WDP führt zu einer Zertifikatwarnung

Sie erhalten eine Warnung zum Zertifikat, das bereitgestellt wurde, ähnlich wie folgender Screenshot, dass das von Ihrer Xbox One-Konsole signierte Sicherheitszertifikat nicht als bekannter vertrauenswürdiger Herausgeber betrachtet wird. 
Klicken Sie auf „Laden dieser Website fortsetzen“, um auf das Windows Device Portal zuzugreifen.

![Warnung zu Website-Sicherheitszertifikat](images/security_cert_warning.jpg)

## Dev Home

Gelegentlich führt die Auswahl der Option „Windows Device Portal verwalten“ dazu, dass Dev Home im Hintergrund zum Startbildschirm wechselt. 
Dies wird durch einen Fehler in der WDP-Infrastruktur auf der Konsole verursacht und kann durch Neustarten der Konsole behoben werden.

## Siehe auch
- [Häufig gestellte Fragen](frequently-asked-questions.md)
- [UWP auf Xbox One](index.md)


<!--HONumber=May16_HO2-->


