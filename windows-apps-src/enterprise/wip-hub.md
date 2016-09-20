---
author: normesta
Description: "Dies ist ein Übersichtsthema mit umfassenden Informationen für Entwickler zum Zusammenhang zwischen der Windows Information Protection (WIP) und Dateien, Puffern, der Zwischenablage, dem Netzwerk, Hintergrundaufgaben und dem Schutz von Daten bei Sperre."
MS-HAID: dev\_enterprise.edp\_hub
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Windows Information Protection (WIP)
translationtype: Human Translation
ms.sourcegitcommit: 1070561ea95cd1d884303fdd476b40a9ec88e390
ms.openlocfilehash: 2beec354ed7dbb3cc2d4cb502977ce028b4eaf1d

---

# Windows Information Protection (WIP)

__Hinweis__ Die Windows Information Protection (WIP)-Richtlinie kann auf Windows 10, Version 1607 angewendet werden.

WIP schützt Daten, die zu einem Unternehmen gehören, indem Richtlinien durchgesetzt werden, die von dem Unternehmen definiert sind. Wenn Ihre App diese Richtlinien enthält, unterliegen alle Daten von der App den Richtlinien. Dieses Thema unterstützt Sie beim Erstellen von Apps, die diese Richtlinien besser durchsetzen, ohne Auswirkung auf persönliche Daten des Benutzers.

## Als Erstes: Was ist WIP?

WIP ist ein Satz von Features auf Desktops, Laptops, Tablets und Smartphones, welche das Mobile Device Management (MDM)-System des Unternehmens unterstützen. WIP kann dem Unternehmen mehr Kontrolle darüber geben, wie Daten des Unternehmens auf Geräten, die das Unternehmen verwaltet, behandelt werden. Administratoren können beispielsweise angeben, welche Apps auf Dateien zugreifen dürfen, die dem Unternehmen gehören, und ob Benutzer Daten aus diesen Dateien kopieren und dann in persönliche Dokumente einsetzen können.

So funktioniert’s. Benutzer registrieren ihre Geräte im System für Verwaltung mobiler Geräte (Mobile Device Management, MDM). Ein Administrator im Verwaltungsunternehmen verwendet Microsoft Intune oder System Center Configuration Manager (SCCM) zur Definition einer Richtlinie und anschließender Bereitstellung auf den registrierten Geräten.

Diese Richtlinie identifiziert die Apps, die Zugriff auf Unternehmensdaten haben dürfen (auch *Liste der zugelassenen Apps*der Richtlinie genannt). Diese Apps können auf geschützte Unternehmensdateien, virtuelle Private Netzwerke (VPN) und Unternehmensdaten in der Zwischenablage oder über einen Freigabe-Vertrag zugreifen. Die Richtlinie definiert auch die Regeln, die für die Daten gelten. Beispielsweise, ob Daten von unternehmenseigenen Dateien kopiert und dann in nicht unternehmenseigene Dateien eingesetzt werden können.

Wenn der Benutzer das Gerät von dem MDM-System des Unternehmens aufhebt, können Administratoren ferngesteuert Unternehmensdaten vom Gerät löschen.

![WIP-Lebenszyklus](images/wip-lifecycle.png)

> **Weitere Informationen zu WIP** <br>
* [Einführung in Windows Information Protection](https://blogs.technet.microsoft.com/windowsitpro/2016/06/29/introducing-windows-information-protection/)
* [Schützen von Unternehmensdaten mit Windows Information Protection (WIP)](https://technet.microsoft.com/library/dn985838(v=vs.85).aspx)

Wenn Ihre App auf der Liste der zugelassenen Apps steht, unterliegen alle von der App erstellten Daten den Einschränkungen der Richtlinien. Das bedeutet: Wenn Administratoren den Zugriff des Benutzers auf Unternehmensdaten widerrufen, geht dem Benutzer der Zugriff auf alle Daten verloren, die Ihre App erstellt hat.

Dies ist in Ordnung, wenn Ihre App nur für Unternehmen entwickelt wurde. Wenn Ihre App Daten erstellt, die die Benutzer als persönlich erachten, sollten Sie Ihre App *optimieren*, intelligent zwischen Unternehmens- und persönlichen Daten zu unterscheiden. Wir bezeichnen diese Art App *unternehmensoptimiert*, da sie problemlos eine Unternehmensrichtlinie durchsetzen und gleichzeitig die Integrität der persönlichen Daten des Benutzers beibehalten kann.

## Erstellen Sie eine unternehmensoptimierte App

Verwenden Sie WIP-APIs, um Ihre App zu optimieren und dann als unternehmensoptimiert zu deklarieren.

Optimieren Sie Ihre App, wenn diese für Unternehmens- und persönliche Zwecke verwendet wird.

Optimierung der App, wenn Sie das Durchsetzen der Richtlinienelemente problemlos behandeln möchten.

Beispiel: Wenn die Richtlinie Benutzern erlaubt, Unternehmensdaten in einem privaten Dokument einzusetzen, können Sie verhindern, dass Benutzer auf ein Zustimmungsdialogfeld reagieren müssen, bevor sie die Daten einsetzen können. Auf ähnliche Weise können Sie benutzerdefinierte Dialogfelder zu Informationszwecken als Antwort auf diese Art von Ereignissen darstellen.

Wenn Sie bereit sind, die App zu optimieren, sehen Sie sich eines dieser Handbücher an:

**Für Universelle Windows-Plattform (UWP)-Apps, die Sie mithilfe von C erstellen#**

[Erstellen Sie eine optimierte App, die Unternehmensdaten und persönlichen Daten verwendet](wip-dev-guide.md).

**Für Desktop-Apps, die Sie mit C++ erstellen**

[Erstellen Sie eine optimierte App, die Unternehmensdaten und persönlichen Daten verwendet (C++)](http://go.microsoft.com/fwlink/?LinkId=822192).

Unter anderem bieten unternehmensoptimierte Apps die folgenden Vorteile:

* Sie schützen Unternehmensdaten, unabhängig davon, ob die Daten gespeichert, verwendet oder übertragen werden.
* Sie erkennen persönliche Daten und verhindern, dass diese Daten den Richtlinien unterliegen.
* Sie erkennen Unternehmensdaten und schützen diese Daten, sobald sie in der App eingehen.
* Sie schützen Unternehmensdaten, die die App verlassen.

  Beispielsweise verhindern sie, dass Daten an einen Endpunkt außerhalb des Unternehmens gesendet werden, umschließen Daten vor dem Zulassen des Roamings in einem portablen verschlüsselten Format und fordern den Benutzer ggf. (abhängig von Richtlinieneinstellungen) zur Zustimmung auf, bevor Unternehmensdaten in einer App eingefügt werden, die nicht in der Liste der zulässigen Apps enthalten ist.

> **Hinweis**  Der WIP-Dateischutz nutzt RMS-Schlüssel (Rights Management Service, Rechteverwaltungsdienst), wenn diese bereitgestellt werden, da diese Schlüssel auf verschiedenen Geräten bereitgestellt werden können, wodurch ein Roaming der geschützten Daten ermöglicht wird. Wenn keine RMS-Schlüssel vorhanden sind, greifen diese APIs auf lokale Schlüssel für die selektive Zurücksetzung zurück und schränken die Roaming-Funktionen ein. Auf Daten mit verschlüsseltem Roaming kann auf Geräten mit älteren Windows-Versionen und auf Drittanbietergeräten über plattformspezifische RMS-Apps zugegriffen werden, die von Microsoft bereitgestellt werden, sowie mit für RMS optimierten Apps von Drittanbietern.








 



<!--HONumber=Aug16_HO3-->


