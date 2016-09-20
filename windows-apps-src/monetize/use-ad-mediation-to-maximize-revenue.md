---
author: mcleanbyron
ms.assetid: 772DEBF2-1578-4330-9C14-70BCC6F55005
description: "Microsoft bietet Unterstützung für eine Anzeigenvermittlung, mit der Sie Ihren Umsatz mit In-App-Werbung durch die Vermittlung von Banneranzeigenanforderungen mehrerer Anzeigennetzwerke optimieren können."
title: Maximieren des Umsatzes mit einer Anzeigenvermittlung
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c0669e35b285ee7dfeda0c039d8455a4237960f5

---

#  Maximieren des Umsatzes mit einer Anzeigenvermittlung


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Microsoft bietet Unterstützung für eine Anzeigenvermittlung, mit der Sie Ihren Umsatz mit In-App-Werbung durch die Vermittlung von Banneranzeigenanforderungen mehrerer Anzeigennetzwerke optimieren können. Verschiedene Anzeigennetzwerke weisen in bestimmten Märkten unterschiedliche Vorteile auf. Bei einigen fallen höhere Kosten pro 1.000 Sichtkontakten (eCPM) an, andere bieten eine höhere Füllrate (Prozentsatz der bereitgestellten Anzeigen, wenn Ihre App eine Anforderung sendet). Mit nur einem Anzeigennetzwerk werden Anzeigenanforderungen u.U. nicht ausreichend erfüllt, wodurch Einnahmeverluste entstehen können. Durch die konsequente Anzeige von Liveanzeigen hilft Ihnen die Anzeigenvermittlung, die Erträge Ihrer Anzeigen zu maximieren.

Unterstützung für die Anzeigenvermittlung ist über das [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk) verfügbar. Nachdem Sie das SDK installiert und Ihrer App das AdMediator-Steuerelement hinzugefügt haben, können Sie durch Aktualisieren Ihrer Vermittlungskonfiguration in Dev Center festlegen, wie oft die einzelnen Netzwerke verwendet werden sollen. Sie können eine Optimierung nach Marktbedingungen vornehmen, sodass in den geeigneten Regionen die effektivsten Anzeigennetzwerke zum Einsatz kommen. Zudem können Sie Änderungen bezüglich der Verwendung der einzelnen Anzeigennetzwerke vornehmen, ohne die App erneut zu veröffentlichen.

## Erste Schritte mit der Anzeigenvermittlung


Führen Sie die folgenden Schritte zum Einrichten und Konfigurieren der Anzeigenvermittlung in Ihrer App aus:

1.  Überprüfen Sie die Liste der von der Anzeigenvermittlung unterstützten Anzeigennetzwerke und Projekttypen, richten Sie Konten mit den gewünschten Anzeigennetzwerken ein, und folgen Sie den Anweisungen jedes Anzeigennetzwerk zum Integrieren einer App. Weitere Informationen finden Sie unter [Auswählen und Verwalten der Anzeigennetzwerke](select-and-manage-your-ad-networks.md).

2.  Installieren Sie das [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk) mit Visual Studio2015 oder Visual Studio2013.

3.  Öffnen Sie in Visual Studio Ihr Projekt, oder erstellen Sie ein neues Projekt. Öffnen Sie die Seite, auf der Sie Anzeigen hosten möchten, und ziehen Sie ein **AdMediatorControl**-Element auf die Seite. Bei Verwendung von Microsoft Advertising passen Sie Höhe und Breite des Steuerelements an die unterstützten Anzeigengrößen an. Weitere Informationen finden Sie unter [Hinzufügen und Verwenden des Ad Mediator-Steuerelements](add-and-use-the-ad-mediator-control.md).

4.  Führen Sie **Verbundene Dienste** aus, um die gewünschten Anzeigennetzwerke auszuwählen und die erforderlichen Parameter für die einzelnen Anzeigennetzwerke zu konfigurieren. Weitere Informationen finden Sie unter [Hinzufügen und Verwenden des Ad Mediator-Steuerelements](add-and-use-the-ad-mediator-control.md).

5.  Testen Sie die Implementierung der Anzeigenvermittlung in Ihrer App. Weitere Informationen finden Sie unter [Testen der Implementierung der Anzeigenvermittlung](test-your-ad-mediation-implementation.md).

6.  Übermitteln Sie die App an das Windows Dev Center-Dashboard, und konfigurieren Sie die Einstellungen für die Anzeigenvermittlung im Dashboard. Weitere Informationen finden Sie unter [Übermitteln der App und Konfigurieren der Anzeigenvermittlung](submit-your-app-and-configure-ad-mediation.md).

7.  Überprüfen Sie die Anzeigenvermittlungsberichte im Dev Center-Dashboard. Weitere Informationen finden Sie unter [Bericht „Anzeigenvermittlung“](https://msdn.microsoft.com/library/windows/apps/mt148521).

## Verwenden von Microsoft Advertising ohne Anzeigenvermittlung


Wenn Sie keine Anzeigenvermittlung verwenden möchten oder der Projekttyp derzeit von der Anzeigenvermittlung nicht unterstützt wird, können Sie Banneranzeigen von Microsoft Advertising auch ohne Anzeigenvermittlung bereitstellen. Weitere Informationen finden Sie unter [AdControl in XAML und .NET](https://msdn.microsoft.com/library/mt313186.aspx) und [AdControl in HTML 5 und JavaScript](https://msdn.microsoft.com/library/mt313130.aspx).

## Verwandte Themen

* [Auswählen und Verwalten der Anzeigennetzwerke](select-and-manage-your-ad-networks.md)
* [Hinzufügen und Verwenden des Steuerelements für die Anzeigenvermittlung](add-and-use-the-ad-mediator-control.md)
* [Testen der Implementierung der Anzeigenvermittlung](test-your-ad-mediation-implementation.md)
* [Übermitteln der App und Konfigurieren der Anzeigenvermittlung](submit-your-app-and-configure-ad-mediation.md)
* [Problembehandlung bei der Anzeigenvermittlung](troubleshoot-ad-mediation.md)
 

 



<!--HONumber=Jun16_HO4-->


