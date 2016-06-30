---
author: mcleanbyron
ms.assetid: 9ca1f880-2ced-46b4-8ea7-aba43d2ff863
description: "Erfahren Sie mehr über bekannte Probleme mit der aktuellen Version der Microsoft Advertising-Bibliotheken im Microsoft Store Engagement and Monetization SDK."
title: Bekannte Probleme mit den Microsoft Advertising-Bibliotheken
translationtype: Human Translation
ms.sourcegitcommit: 5b6d3e034b73e6ae693fbeab3ecd3b2b81f38bb1
ms.openlocfilehash: cfaa108cc93b6bae903e86ad141656bf613f185d

---

# Bekannte Probleme mit den Microsoft Advertising-Bibliotheken


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

In diesem Thema werden bekannte Probleme mit der aktuellen Version der Microsoft Advertising-Bibliotheken im Microsoft Store Engagement and Monetization SDK aufgelistet.

## Installation erfordert Visual Studio-Tools für universelle Windows-Apps

Um das [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk) mit Visual Studio 2015 zu installieren, muss Version 1.1 oder höher der Visual Studio-Tools für universelle Windows-Apps installiert sein. Weitere Informationen finden Sie in den [Versionshinweisen](http://go.microsoft.com/fwlink/?LinkID=624516) für Visual Studio.

## Windows Phone 8.x Silverlight-Projekte

Um die Microsoft Advertising-Assemblys für Windows Phone 8.x Silverlight-Projekte zu erhalten, installieren Sie das [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk), öffnen das Projekt in Visual Studio und wechseln anschließend zu **Projekt** > **Verbundenen Dienst hinzufügen** > **Ad Mediator**. Die Assemblys werden anschließend automatisch heruntergeladen. Im Anschluss daran können Sie die Ad Mediator-Referenzen aus Ihrem Projekt entfernen, wenn Sie Ad Mediator nicht verwenden möchten. Weitere Informationen finden Sie unter [AdControl in Windows Phone Silverlight](adcontrol-in-windows-phone-silverlight.md).

## AdControl-Schnittstelle in XAML nicht bekannt

Das XAML-Markup für eine [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) zeigt möglicherweise fälschlicherweise eine blaue Kurvenlinie an, die anzeigt, dass die Schnittstelle nicht bekannt ist. Dies geschieht nur im Zusammenhang mit x86 und kann ignoriert werden.

## „lastError“ aus vorheriger Anzeigenanforderung

Wenn es einen **lastError** aus der vorherigen Anzeigenanforderung gibt, wird das Ereignis möglicherweise während des nächsten Anzeigenaufrufs zweimal ausgelöst. Obwohl die neue Anzeigenanforderung dennoch ausgeführt werden kann und möglicherweise zu einer gültigen Anzeige führt, kann dieses Verhalten zu Verwirrung führen.

## Interstitielle Anzeigen und Navigationsschaltflächen auf Telefonen

Auf Telefonen (oder Emulatoren), die über Softwareschaltflächen für **Zurück**, **Starten** und **Suchen** anstelle von Hardwaretasten verfügen, werden die Countdown-Timer und Klickschaltflächen für Videointerstitialanzeigen möglicherweise verdeckt.

## Vor kurzem erstellte Anzeigen werden Ihrer App nicht bereitgestellt

Wenn Sie vor kurzem (weniger als einem Tag) eine Anzeige erstellt haben, ist diese möglicherweise nicht sofort verfügbar. Wenn die Anzeige hinsichtlich ihrer redaktionellen Inhalte genehmigt wurde, wird sie bereitgestellt, nachdem der Anzeigenserver sie verarbeitet hat und die Anzeige als Bestand verfügbar ist.

## In Ihrer App werden keine Anzeigen angezeigt

Es gibt viele Gründe, warum möglicherweise keine Anzeigen angezeigt werden, einschließlich Netzwerkfehlern. Andere Gründe können sein:

* Auswahl einer Anzeigeneinheit im Windows Dev Center mit einer Größe, die größer oder kleiner als die Größe der **AdControl** im Code Ihrer App ist.

* Anzeigen werden nicht angezeigt, wenn Sie einen [Testmoduswert](test-mode-values.md) für Ihre Anzeigeneinheiten-ID verwenden, wenn eine Live-App ausgeführt wird.

* Wenn Sie in der letzten halben Stunde eine neue Anzeigeneinheiten-ID erstellt haben, wird eine Anzeige möglicherweise erst angezeigt, wenn der Server neue Daten durch das System propagiert hat. Vorhandene IDs, die zuvor bereits Anzeigen angezeigt haben, sollten Anzeigen sofort anzeigen.

Wenn Sie in der App Testanzeigen sehen können, funktioniert Ihr Code und kann Anzeigen anzeigen. Bei Problemen wenden Sie sich an den [Produktsupport](https://go.microsoft.com/fwlink/p/?LinkId=331508). Wählen Sie auf dieser Seite **In App-Werbung** aus.

Sie können auch im [Forum](http://go.microsoft.com/fwlink/p/?LinkId=401266) eine Frage stellen.

## In Ihrer App werden Testanzeigen anstelle von Liveanzeigen angezeigt.

Testanzeigen können angezeigt werden, auch wenn Sie Liveanzeigen erwarten. Dies kann in den folgenden Szenarien vorkommen:

* Microsoft Advertising kann die Liveanwendungs-ID nicht überprüfen oder finden, die im App Store verwendet wird. Wenn eine Anzeigeneinheit von einem Benutzer erstellt wird, kann in diesem Fall der Status als live (Nicht-Test) beginnen, jedoch innerhalb von 6 Stunden nach der ersten Anzeigenanforderung in den Teststatus wechseln. Er wechselt zurück zum Livestatus, wenn es 10 Tage keine Anforderungen von Test-Apps gibt.

* Quergeladene Apps oder im Emulator ausgeführte Apps zeigen keine Liveanzeigen an.

Wenn eine Liveanzeigeneinheit Testanzeigen bereitstellt, wird der Status der Anzeigeeinheit im Windows Dev Center als **Aktiv und Testanzeigen bereitstellend** angezeigt. Dies gilt zurzeit nicht für Telefon-Apps.

## Veraltete Testwerte für Anzeigeneinheiten-ID und Anwendungs-ID funktionieren nicht mehr

Die folgenden Testwerte für Windows Phone Silverlight-Apps sind veraltet und funktionieren nicht mehr. Wenn ein vorhandenes Projekt diese Testwerte verwendet, aktualisieren Sie dieses Projekt, sodass es die unter [Testmoduswerte](test-mode-values.md) angegebenen Werte verwendet.

| Anwendungs-ID  |  Anzeigeeinheiten-ID    |
|-----------------|----------------|
| test_client     |  Image320_50   |
| test_client     |  Image300_50   |
| test_client     |  TextAd   |
| test_client     |  Image480_80   |

<span id="reference_errors"/>
## Referenzfehler, die durch die Ausrichtung auf eine beliebige CPU (Any CPU) in Ihrem Projekt verursacht werden

Wenn Sie die Microsoft Advertising-Bibliotheken verwenden, können Sie in Ihrem Projekt als Ziel nicht **Any CPU** angeben. Wenn Ihr Projekt auf die Plattform **Any CPU** ausgerichtet ist, wird Ihnen möglicherweise eine Warnung angezeigt, nachdem Sie einen Verweis wie diesen hinzugefügt haben.

![referenceerror\-solutionexplorer](images/13-19629921-023c-42ec-b8f5-bc0b63d5a191.jpg)

Um diese Warnung zu entfernen, müssen Sie eine architekturspezifische Buildausgabe verwenden (beispielsweise **x86**) und das Projekt entsprechend aktualisieren. Verwenden Sie den **Konfigurations-Manager**, um die Plattformziele für Debug- und Releasekonfigurationen festzulegen.

![configurationmanagerwin10](images/13-87074274-c10d-4dbd-9a06-453b7184f8de.png)

Achten Sie darauf, die beabsichtigten Architekturen einzuschließen, wenn Sie App-Pakete für die Übermittlung an den Store erstellen (wie in den folgenden Bildern gezeigt). Sie können x64 auslassen, wenn Sie x86-Builds auf dem x64-Betriebssystem ausführen möchten.

![projectstorecreateapppackages](images/13-a99b05a4-8917-4c53-822e-2548fadf828a.png)

![createapppackages](images/13-16280cb1-a838-42b9-9256-eac7f33f5603.png)

## Z-Reihenfolge in JavaScript/HTML-Apps

JavaScript/HTML-Apps müssen Elemente nicht in den reservierten MAX 10-Bereich der Z-Reihenfolge platzieren. Die einzigen Ausnahmen sind Interrupt-Overlays wie eingehende Anrufbenachrichtigungen für Skype-Apps.

<span id="bkmk-ui"/>
## Verwenden Sie keine Rahmen

Die Festlegung von Rahmeneigenschaften, die **AdControl** von seiner übergeordneten Klasse erbt, führt zu einer falschen Platzierung der Anzeige.

## Weitere Informationen


Weitere Informationen zu den neuesten bekannten Problemen im Zusammenhang mit den Microsoft Advertising-Bibliotheken finden Sie im [Forum](http://go.microsoft.com/fwlink/p/?LinkId=401266). Dort können Sie auch Fragen veröffentlichen.

## Support


Um sich an den Produktsupport für Probleme mit den Microsoft Advertising-Bibliotheken zu wenden, besuchen Sie die [Supportseite](https://go.microsoft.com/fwlink/p/?LinkId=331508) und wählen **In-App-Werbung** aus.

 

 



<!--HONumber=Jun16_HO4-->


