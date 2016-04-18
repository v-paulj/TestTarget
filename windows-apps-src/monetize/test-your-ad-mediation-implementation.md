---
ms.assetid: 54ECD653-7FC2-4A95-AC5A-972C4FB5A54B
description: Vor dem Übermitteln Ihrer App empfehlen wir, die Implementierung der Anzeigenvermittlung zu testen.
title: Testen der Implementierung der Anzeigenvermittlung
---

# Testen der Implementierung der Anzeigenvermittlung


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Vor dem Übermitteln Ihrer App empfehlen wir, die Implementierung der Anzeigenvermittlung zu testen.

## Testen mit Testkonfigurationswerten für das Anzeigennetzwerk


Wenn Sie Ihre App ausführen, ohne **Verbundene Dienste** für Ihr Projekt in Visual Studio aufzurufen und Angaben zur Konfiguration des Anzeigennetzwerks zu machen, verwendet die Anzeigenvermittlung automatisch Testkonfigurationswerte beim Ausführen der App auf dem Entwicklungscomputer (für Apps für die universelle Windows-Plattform (UWP) und Windows 8.1 XAML-Apps) oder auf dem Emulator oder Gerät (für Windows Phone-Apps). So können Sie Ihre App schnell testen und sicherstellen, dass sie ordnungsgemäß codiert ist, bevor Sie die erforderlichen Parameter Ihres Anzeigennetzwerks eingegeben haben.

Die Anzeigennetzwerke werden der Reihe nach rotierend angezeigt, d. h. ein Netzwerk nach dem anderen wird gleich lang angezeigt. Achten Sie darauf, mehrere Zyklen zu durchlaufen, damit Sie alle Anzeigennetzwerke anzeigen und die Wahrscheinlichkeit von temporären Konnektivitätsproblemen verringern können, die u. U. auftreten.

Testanzeigen werden für Anzeigennetzwerke angezeigt, die diese unterstützen. Beachten Sie, dass die Testanzeigen manchmal wie Fehler aussehen. Überprüfen Sie unbedingt Ihre Ereignisse, um festzustellen, ob Fehler aufgetreten sind.

**Hinweis**  Beim Testen einer Windows Phone-Silverlight-App gibt Google AdMob immer den Fehler **Ungültige Anforderung** zurück, da keine Testmetadaten verwendet werden. Zum Überprüfen Ihrer Google AdMob-Implementierung müssen Sie wie im nächsten Abschnitt beschrieben die erforderlichen Parameter eingeben.

 

Wenn Sie den in [Hinzufügen und Verwenden des Ad Mediator-Steuerelements](add-and-use-the-ad-mediator-control.md) gezeigten Ereignishandlercode verwenden, werden Fehler in der Ausgabe der Konsole angezeigt.

## Testen mit Ihren Konfigurationswerten für das Anzeigennetzwerk


Nachdem Sie Ihre App mit Testkonfigurationsdaten getestet haben, testen Sie die App mit den tatsächlichen Konfigurationswerten für Ihr Anzeigennetzwerk, die Sie für die App-Version verwenden, die Sie im Windows Store veröffentlichen möchten.

Öffnen Sie zuerst das Fenster **Verbundenen Dienst hinzufügen** (Visual Studio 2015) oder **Dienst-Manager** (Visual Studio 2013), und konfigurieren Sie jedes Anzeigennetzwerk wie unter [Hinzufügen und Verwenden des Ad Mediator-Steuerelements](add-and-use-the-ad-mediator-control.md) beschrieben. Geben Sie die erforderlichen Parameter für die einzelnen Anzeigennetzwerke ein.

Jetzt sind Sie zum Testen Ihrer App bereit. Achten Sie darauf, die App lange genug auszuführen, um überprüfen zu können, ob jedes Anzeigennetzwerk eine Anzeige ordnungsgemäß anzeigen kann. Suchen Sie nach Ausnahmen, und beheben Sie Codierungsfehler, bevor Sie Ihre App übermitteln.

Beim Übermitteln Ihres App-Pakets an das Windows Dev Center-Dashboard werden die in Visual Studio eingegebenen Konfigurationswerte automatisch in die Dashboardseite **Monetisierung durch Werbeanzeigen** übernommen. Sie können diese Werte ändern, um das Verhalten des Anzeigennetzwerks für Ihre App im Windows Store zu konfigurieren. Weitere Informationen finden Sie unter [Übermitteln der App und Konfigurieren der Anzeigenvermittlung](submit-your-app-and-configure-ad-mediation.md).

## Verwandte Themen

* [Auswählen und Verwalten der Anzeigennetzwerke](select-and-manage-your-ad-networks.md)
* [Hinzufügen und Verwenden des Steuerelements für die Anzeigenvermittlung](add-and-use-the-ad-mediator-control.md)
* [Übermitteln der App und Konfigurieren der Anzeigenvermittlung](submit-your-app-and-configure-ad-mediation.md)
* [Problembehandlung bei der Anzeigenvermittlung](troubleshoot-ad-mediation.md)
 

 





<!--HONumber=Mar16_HO1-->


