---
author: Mtoepke
title: "Häufig gestellte Fragen"
description: "Häufig gestellte Fragen zu UWP auf Xbox."
area: Xbox
ms.sourcegitcommit: bdf7a32d2f0673ab6c176a775b805eff2b7cf437
ms.openlocfilehash: 34e186049039d5a8366f34e985ad7250ef664f00

---

# Häufig gestellte Fragen

Funktioniert etwas nicht wie erwartet? Auf dieser Seite finden Sie häufig gestellte Fragen. Lesen Sie außerdem das Thema [Bekannte Probleme](known-issues.md) und besuchen Sie das Forum zum [Entwickeln von universellen Windows-Apps](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

### Warum funktionieren meine Spiele und Apps nicht?

Wenn Ihre Spiele und Apps nicht funktionieren oder Sie keinen Zugriff auf den Store oder die Live-Dienste haben, ist vermutlich der Entwicklermodus aktiviert. Wenn Sie die Startseite auswählen und auf der rechten Seite des Bildschirms statt der üblichen Gold-/Live-Inhalte eine große Dev Home-Kachel sehen, ist der Entwicklermodus aktiviert. Wenn Sie spielen möchten, können Sie Dev Home öffnen und wieder in den Einzelhandelsmodus wechseln, indem Sie die Schaltfläche **Leave developer mode** verwenden.

### Warum kann ich über Visual Studio keine Verbindung zu meiner Xbox One herstellen?

Vergewissern Sie sich zunächst, dass der Entwicklermodus und nicht der Einzelhandelsmodus aktiviert ist. Im Einzelhandelsmodus können Sie keine Verbindung mit der Xbox One herstellen. Dies können Sie einfach überprüfen, indem Sie auf die Schaltfläche **Home** klicken und auf der rechten Seite des Bildschirms nach der Dev Home-Kachel suchen. Wenn die Kachel nicht vorhanden ist und stattdessen Gold-/Live-Inhalte angezeigt werden, befinden Sie sich im Einzelhandelsmodus. Sie müssen die DevMode-Aktivierungs-App ausführen, um in den Entwicklermodus zu wechseln.

> **Hinweis**
            &nbsp;&nbsp;Damit Sie eine App bereitstellen können, muss ein Benutzer angemeldet sein.

Weitere Informationen finden Sie unter [Beheben von Bereitstellungsfehlern](frequently-asked-questions.md#fixing-deployment-failures) weiter unten auf dieser Seite.

### Wie wechsle ich zwischen Einzelhandels- und Entwicklermodus?

Folgen Sie den Anweisungen zur [Aktivierung des Xbox One-Entwicklermodus](devkit-activation.md), um mehr über diese Modi zu erfahren.

### Woran erkenne ich, ob ich mich im Einzelhandels- oder im Entwicklermodus befinde?

Folgen Sie den Anweisungen zur [Aktivierung des Xbox One-Entwicklermodus](devkit-activation.md), um mehr über diese Modi zu erfahren. 

Sie können dies einfach überprüfen, indem Sie auf die Schaltfläche **Home** klicken und die rechte Seite des Bildschirms betrachten. Wenn Sie sich im Entwicklermodus befinden, wird auf der rechten Seite die Dev Home-Kachel angezeigt. Befinden Sie sich im Einzelhandelsmodus, sehen Sie die üblichen Gold-/Live-Inhalte.

### Funktionieren meine Spiele und Apps auch, wenn ich den Entwicklermodus aktiviere?

Ja, Sie können vom Entwicklermodus in den Einzelhandelsmodus wechseln, in dem Sie Ihre Spiele spielen können. Weitere Informationen finden Sie auf der Seite zur [Aktivierung des Xbox One-Entwicklermodus](devkit-activation.md). 

<!-- > **CAUTION**&nbsp;&nbsp;The Xbox Developer Preview System Update includes experimental and early pre-release software. 
This means that some popular games and apps will not work as expected and you may experience occasional crashes and data loss. -->

### Werden meine Spiele und Apps oder gespeicherte Änderungen verloren gehen?

Wenn Sie das Developer Preview-Programm verlassen, müssen Sie möglicherweise eine Zurücksetzung auf die Werkseinstellungen durchführen, bei der alle Inhalte Ihrer Konsole gelöscht werden. In diesem Fall müssen Sie sämtliche Spiele und Apps erneut installieren. Wenn Sie beim Spielen online waren, werden Ihre gespeicherten Spiele im Cloudprofil Ihres Live-Kontos gespeichert und gehen nicht verloren.

### Wie verlasse ich die Entwicklervorschau?

Weitere Informationen dazu, wie Sie die Entwicklervorschau verlassen, finden Sie im Thema zur [Deaktivierung des Xbox One-Entwicklermodus](devkit-deactivation.md).

### Ich habe meine Xbox One verkauft und im Entwicklermodus belassen. Wie deaktiviere ich den Entwicklermodus?

Wenn Sie keinen Zugriff mehr auf Ihre Xbox One haben, können Sie sie in Windows Dev Center deaktivieren. Weitere Informationen finden Sie im Abschnitt **Deaktivieren Ihrer Konsole mit Windows Dev Center** im Thema zur [Deaktivierung des Xbox One-Entwicklermodus](devkit-deactivation.md#deactivate-your-console-through-windows-dev-center).

### Ich habe die Entwicklervorschau mithilfe von Windows Dev Center verlassen, der Entwicklermodus ist aber weiterhin aktiviert. Wie gehe ich vor?

Starten Sie Dev Home, und wählen Sie die Schaltfläche **Leave developer mode** aus. Dadurch wird die Konsole im Einzelhandelsmodus neu gestartet. 

### Kann ich meine App veröffentlichen?

Das Veröffentlichen von Apps wird später in diesem Jahr über Dev Center möglich sein. Die auf einer Xbox One Einzelhandels-Konsole erstellten und getesteten UWP-Apps durchlaufen denselben Aufnahme-, Überprüfungs- und Veröffentlichungsprozess, den Windows heutzutage durchführt. Mithilfe zusätzlicher Überprüfungen werden die aktuellen Xbox One-Standards erfüllt.

### Kann ich mein Spiel veröffentlichen?

Sie können UWP und die Xbox One im Entwicklermodus verwenden, um Ihre Spiele auf der Xbox One zu erstellen und zu testen. Um UWP-Spiele veröffentlichen zu können, müssen Sie sich bei [ID@XBOX](http://www.xbox.com/en-us/Developers/id) registrieren. 
[ID@Xbox](http://www.xbox.com/en-us/Developers/id) bietet Entwicklern den vollständigen Zugriff auf Xbox Live-APIs für ihre Spiele, einschließlich von Gamerscore und Erfolgen, sowie der Möglichkeit, Multiplayer zwischen Geräten, Cloudspeicherung und alle Features von Xbox Live auf Xbox One zu nutzen. 
[ID@Xbox](http://www.xbox.com/en-us/Developers/id) kann darüber hinaus den Zugriff auf Xbox One-Entwicklungskits für Spiele bereitstellen, die das maximale Potenzial der Xbox One Hardware benötigen.

### Können die standardmäßigen Spielengines verwendet werden?

Informationen hierzu finden Sie auf der Seite [Bekannte Probleme](known-issues.md) für diese Vorschauversion.

### Welche Funktionen und Systemressourcen sind für UWP-Spiele auf Xbox One verfügbar? 

Informationen hierzu finden Sie unter [Systemressourcen für UWP-Apps und -Spiele auf Xbox One](system-resource-allocation.md).

### Kann ein DirectX 12-UWP-Spiel, das ich erstellt habe, auf meiner Xbox One im Entwicklermodus ausgeführt werden?

Informationen hierzu finden Sie unter [Systemressourcen für UWP-Apps und -Spiele auf Xbox One](system-resource-allocation.md).

### Wird die gesamte UWP-API-Oberfläche auf Xbox verfügbar sein?

Informationen hierzu finden Sie auf der Seite [Bekannte Probleme](known-issues.md) für diese Vorschauversion.

### Beheben von Bereitstellungsfehlern

Wenn die Bereitstellung Ihrer App in Visual Studio fehlschlägt, können Ihnen die folgenden Schritte beim Beheben des Problems helfen. Wenn Sie nicht vorankommen, bitten Sie im Forum um Hilfe.

> **Hinweis**
            &nbsp;&nbsp;Damit Sie eine App bereitstellen können, muss ein Benutzer angemeldet sein. Wenn Sie eine 0x87e10008-Fehlermeldung erhalten, vergewissern Sie sich, dass ein Benutzer angemeldet ist, und versuchen Sie es noch einmal.

Wenn Visual Studio keine Verbindung mit Ihrer Xbox One herstellen kann:

1. Stellen Sie sicher, dass Sie sich im Entwicklermodus befinden (wie weiter oben auf dieser Seite erläutert).
2. Vergewissern Sie sich, dass der Entwicklungscomputer richtig eingerichtet wurde. Haben Sie *alle* Anweisungen in [Erste Schritte bei der Entwicklung von UWP-Apps auf Xbox One](getting-started.md) befolgt? 

3. Ist dies nicht der Fall, lesen Sie sich das Thema zur [Einrichtung der Entwicklungsumgebung](development-environment-setup.md) und das Thema zur [Einführung in Xbox One-Tools](introduction-to-xbox-tools.md) durch.

4. Vergewissern Sie sich, dass Sie von Ihrem Entwicklungscomputer aus einen Pingbefehl an die IP-Adresse Ihrer Konsole senden können.
> **Hinweis**
            &nbsp;&nbsp;Es wird empfohlen, eine Kabelverbindung mit Ihrer Konsole zu verwenden, um die bestmögliche Leistung bei der Bereitstellung zu erzielen.

5. Stellen Sie sicher, dass in der Dropdownliste für die Authentifizierung auf der Registerkarte **Debuggen** die Option „Universell (unverschlüsseltes Protokoll)“ ausgewählt ist. Weitere Informationen finden Sie unter [Einrichtung der Entwicklungsumgebung](development-environment-setup.md).

<!--6. Make sure you are not hitting a PIN pairing issue; see "Visual Studio/Xbox PIN pairing failures" in the [Known Issues](known-issues.md) topic.-->

<!--
If Visual Studio can connect, but deployment is failing (for example you get this error message: "DEP0700 : Registration of the app failed.(0x80073cf9)"):

1. Make sure that your app is not installed by uninstalling it from the Collections app in the Xbox One shell. 

> **Note**&nbsp;&nbsp;Uninstalling your app from Windows Device Portal (WDP) will not resolve the issue.

2. If your issues persist, uninstall your app or game in the Collections app, leave Developer Mode, restart to Retail Mode, and then switch back to Developer Mode. 
This will clear Dev Storage.

3. If your issues persist, follow the steps above and then use **Reset and keep my games & apps** to delete any stored state on your Xbox One. 
Go to Settings > System > Console info & updates > Reset console, and select the **Reset and keep my games & apps** button.

> **Caution**&nbsp;&nbsp;Doing this will delete all saved settings on your Xbox One including wireless settings, user accounts and any game progress that has not been saved to cloud storage.

> **Caution**&nbsp;&nbsp;DO NOT select the **Reset and remove everything** button.
This will delete all of your games, apps, settings and content, deactivate Developer Mode, and remove you console from the Developer Preview group.
-->

### Wie kann ich beim Erstellen einer App mit HTML/JavaScript die Gamepad-Navigation aktivieren?

TVHelpers ist ein Satz von JavaScript- und XAML-/C#-Beispielen und -Bibliotheken, mit denen Sie großartige Xbox One-Inhalte und TV-Inhalte in JavaScript und C# erstellen können. TVJS ist eine Bibliothek, mit der Sie Premium-UWP-Apps für Xbox One erstellen können. TVJS bietet u. a. Unterstützung für die automatische Controllernavigation, Rich-Media-Wiedergabe und Suche. Sie können TVJS mit einer gehosteten Web-App genauso einfach wie mit einer gepackten UWP-Web-App mit vollständigem Zugriff auf Windows-Runtime-APIs verwenden.

Weitere Informationen finden Sie im Projekt [TVHelpers](https://github.com/Microsoft/TVHelpers) und im Projekt [wiki](https://github.com/Microsoft/TVHelpers/wiki).

## Siehe auch
- [Bekannte Probleme mit UWP auf Xbox One – Developer Preview](known-issues.md)
- [UWP auf Xbox One](index.md)



<!--HONumber=Jun16_HO5-->


