---
title: Bekannte Probleme mit UWP auf Xbox One – Developer Preview
description: 
area: Xbox
---

# Bekannte Probleme mit UWP auf Xbox One – Developer Preview

Das Xbox Developer Preview System Update ist eine Vorabversion der Software für Testzwecke. 
Dies bedeutet, dass einige beliebte Spiele und Apps nicht wie erwartet funktionieren und gelegentlich Abstürze und Datenverlust auftreten. 
Wenn Sie die Developer Preview verlassen, wird Ihre Konsole auf die Werkseinstellungen zurückgesetzt, und Sie müssen alle Spiele, Apps und Inhalte erneut installieren.

Für Entwickler bedeutet dies, dass nicht alle Entwickler-Tools und -APIs wie erwartet funktionieren. 
Dies bedeutet auch, dass nicht alle für die endgültige Version vorgesehenen Features enthalten sind oder diese nicht die endgültige Qualität aufweisen. 
**Insbesondere bedeutet dies, dass die Systemleistung in dieser Preview nicht die Systemleistung der endgültigen Version widerspiegelt.**

In der folgenden Liste werden einige bekannte Probleme aufgelistet, die in dieser Version auftreten können. Dies ist aber keine vollständige Liste. 

**Wir benötigen Feedback.** Melden Sie alle ermittelten Probleme im Forum zum [Entwickeln von universellen Windows-Apps](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

Wenn Sie Probleme haben, informieren Sie sich hier und unter [Häufig gestellte Fragen](frequently-asked-questions.md), und verwenden Sie das Forum.


## Spiele-Entwicklung

### x86 im Vergleich zu x64

Zum Zeitpunkt der Veröffentlichung Ende des Jahres werden x86 und x64 unterstützt. In dieser Preview wird x86 unterstützt. 
Allerdings wurde x64 bisher ausgiebiger getestet (Xbox-Shell und alle Apps auf der Konsole = x64). Daher empfehlen wir die Verwendung von x64 für Ihre Projekte. 
Dies gilt insbesondere für Spiele.

Wenn Sie x86 verwenden möchten, melden Sie Probleme im Forum.

Weitere Informationen finden Sie auch unter [Wechsel zwischen Build-Varianten kann Bereitstellungsfehler verursachen](known-issues.md#switching-build-flavors-can-cause-deployment-failures) später auf dieser Seite.

### Spiele-Engines

Wir haben einige beliebte Spiele-Engines getestet, und unser Testbereich für diese Preview war nicht umfassend. 
Ihre Erfahrungen können variieren. 
Wir freuen uns auf Ihr Feedback. 
Verwenden Sie das Forum, um Probleme zu melden.

### DirectX 12-Unterstützung

UWP auf Xbox One unterstützt die DirectX 11-Featureebene 10. 
DirectX 12 wird derzeit nicht unterstützt. 
Xbox One ist wie alle herkömmlichen Spielekonsolen ein spezielles Hardwaregerät, das ein spezielles SDK erfordert, um das Potenzial vollständig nutzen zu können. 
Wenn Sie an einem Spiel arbeiten, das das maximale Potenzial der Xbox One-Hardware benötigt, können Sie sich beim [ID@XBOX](http://www.xbox.com/en-us/Developers/id)-Programm registrieren, um Zugriff auf das SDK zu erhalten, das DirectX 12-Unterstützung enthält.


## Systemressourcen für UWP-Apps und -Spiele auf Xbox One

UWP-Apps und -Spiele auf Xbox One teilen Ressourcen mit dem System und anderen Apps. Das heißt, das System steuert die Ressourcen, die für alle Spiele oder Apps verfügbar sind. 
Wenn Arbeitsspeicher- oder Leistungsprobleme auftreten, kann dies der Grund sein. 
Weitere Informationen finden Sie unter [Systemressourcen für UWP-Apps und -Spiele auf Xbox One](system-resource-allocation.md).


## Netzwerke mit herkömmlichen Sockets

In dieser Developer Preview ist eingehender und ausgehender Netzwerkzugriff von der Konsole, die herkömmliche TCP/UDP-Sockets (WinSock, Windows.Networking.Sockets) verwendet, nicht verfügbar. 
Entwickler können weiterhin HTTP und WebSockets verwenden. 


## UWP-API-Abdeckung

Nicht alle UWP-APIs in dieser Preview arbeiten auf Xbox erwartungsgemäß. 
Unter [Featurebereich-Einschränkungen für UWP-Gerätefamilie auf Xbox](http://go.microsoft.com/fwlink/p/?LinkId=760755) finden Sie die Liste der APIs, die nicht funktionieren. 
Wenn Probleme mit anderen APIs auftreten, melden Sie sie in den Foren. 

## Aussehen oder Verhalten von XAML-Steuerelementen entspricht nicht den Steuerelementen in der Xbox One-Shell

In dieser Developer Preview sind die XAML-Steuerelemente nicht endgültig. Insbesondere:
* Die Gamepad-X-Y-Navigation funktioniert nicht zuverlässig für alle Steuerelemente.
* Das Aussehen der Steuerelemente entspricht nicht den Steuerelementen in der Xbox-Shell. Dies umfasst das Steuerelement-Fokusrechteck.
* Beim Navigieren zwischen Steuerelementen entstehen nicht automatisch „Navigationstöne“.

Diese Probleme werden in einer zukünftigen Developer Preview behoben.

## Visual Studio und Bereitstellungsprobleme

### Wechsel zwischen Build-Varianten kann Bereitstellungsfehler verursachen

Der Wechsel zwischen Debug und Release Builds oder zwischen x86 und x64 oder zwischen verwalteten und systemeigenen .NET-Builds kann Bereitstellungsfehler verursachen. 

Die einfachste Möglichkeit zur Vermeidung dieser Probleme für diese Preview ist, Debug Builds und eine einzige Architektur zu verwenden. 

Wenn dieses Problem auftritt, wird es durch die Deinstallation Ihrer App in der Collections-App auf Ihrer Xbox One in der Regel nicht gelöst.

> **Hinweis**&nbsp;&nbsp;Die Deinstallation Ihrer App vom Windows Device Portal (WDP) behebt das Problem nicht.

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
Gehen Sie zu „Einstellungen > System > Konsoleninfo und -updates > Konsole zurücksetzen“, und wählen Sie dann die Schaltfläche **Zurücksetzen und meine Spiele und Apps beibehalten**.

> **Vorsicht**&nbsp;&nbsp;Dadurch werden alle gespeicherten Einstellungen auf Ihrer Xbox One gelöscht, einschließlich drahtlose Einstellungen, Benutzerkonten und Spielestatus, die nicht im Cloud-Speicher gespeichert wurden.

> **Vorsicht**&nbsp;&nbsp;Verwenden Sie NICHT die Schaltfläche **Zurücksetzen und alles entfernen**.
Dadurch werden alle Spiele, Apps, Einstellungen und Inhalte gelöscht, der Entwicklermodus wird deaktiviert und Ihre Konsole wird aus der Developer Preview-Gruppe entfernt.

### Die Spalten in der Tabelle „Ausgeführte Apps“ werden nicht wie erwartet aktualisiert. 

Manchmal wird dieses Problem gelöst, indem Sie eine Spalte in der Tabelle sortieren.

### Die WDP-UI wird in Internet Explorer 11 nicht ordnungsgemäß angezeigt. 

Standardmäßig wird die WDP-UI nicht ordnungsgemäß im Browser angezeigt, wenn Sie Internet Explorer 11 verwenden. 
Sie können dies durch Deaktivieren der Kompatibilitätsansicht von Internet Explorer 11 für WDP beheben.

### Navigieren zu WDP führt zu einer Zertifikatwarnung

Sie erhalten eine Warnung zum Zertifikat, das bereitgestellt wurde, ähnlich dem folgenden Screenshot, 
da das von Ihrer Xbox One-Konsole signierte Sicherheitszertifikat nicht als bekannter vertrauenswürdigen Herausgeber betrachtet wird. 
Klicken Sie auf „Laden dieser Website fortsetzen“, um auf das Windows Device Portal zuzugreifen.

![Warnung zu Website-Sicherheitszertifikat](images/security_cert_warning.jpg)

## Dev Home
Gelegentlich führt die Auswahl der Option „Windows Device Portal verwalten“ dazu, dass Dev Home im Hintergrund zum Startbildschirm wechselt. 
Dies wird durch einen Fehler in der WDP-Infrastruktur auf der Konsole verursacht und kann durch Neustarten der Konsole behoben werden.

## Siehe auch
- [Häufig gestellte Fragen](frequently-asked-questions.md)
- [UWP auf Xbox One](index.md)


<!--HONumber=Mar16_HO5-->


