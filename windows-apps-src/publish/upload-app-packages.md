---
author: jnHs
Description: "Auf der Seite „Pakete“ werden alle Paketdateien (XAP, APPX, APPXUPLOAD und/oder APPXBUNDLE) für die App hochgeladen, die Sie übermitteln. Sie können in diesem Schritt Pakete für jedes Betriebssystem hochladen, auf das die App ausgerichtet ist."
title: Hochladen von App-Paketen
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
translationtype: Human Translation
ms.sourcegitcommit: 9440d1343141edf92f5d9e8aafc63ca88b176354
ms.openlocfilehash: 2ca755568ff3697d5e0fe94900799f9dd98b7f72

---

# Hochladen von App-Paketen


Auf der Seite **Pakete** werden alle Paketdateien (APPX, APPXUPLOAD, APPXBUNDLE und XAP) für die App hochgeladen, die Sie übermitteln. Sie können in diesem Schritt Pakete für jedes Betriebssystem hochladen, auf das die App ausgerichtet ist. Wenn ein Kunde Ihre App herunterlädt, stellt der Store automatisch für jeden Kunden das Paket bereit, das am besten für sein Gerät geeignet ist. Nachdem Sie Ihre Pakete hochgeladen haben, sehen Sie eine Tabelle, in der angegeben wird, [welche Pakete für bestimmte Windows10-Gerätefamilien angeboten werden](#device-family-availability) (und ggf. für frühere Betriebssystemversionen).

Ausführliche Informationen zu Inhalt und Struktur der Pakete finden Sie unter [App-Paketanforderungen](app-package-requirements.md). Sie erfahren außerdem, [wie sich Versionsnummern von Paketen darauf auswirken, welche Pakete bestimmten Kunden bereitgestellt werden](package-version-numbering.md) und [wie Pakete für verschiedene Betriebssysteme verteilt werden](guidance-for-app-package-management.md).

## Hochladen von Paketen für Ihre Übermittlung

Um Pakete hochzuladen, ziehen Sie sie in das Uploadfeld oder klicken Sie, um Ihre Dateien zu durchsuchen. Auf der Seite **Pakete** können Sie die XAP-, APPX-, APPXUPLOAD- und/oder APPXBUNDLE-Dateien hochladen.

Falls Sie [Flight-Pakete](package-flights.md) für Ihre App erstellt haben, wird eine Dropdownliste mit der Option zum Kopieren von Paketen aus einem Ihrer Flight-Pakete angezeigt. Wählen Sie das Flight-Paket mit den Paketen aus, die Sie übertragen möchten. Anschließend können Sie einige oder alle der Pakete auswählen, um sie in diese Übermittlung aufzunehmen.

> **Wichtig**  Für Windows10 muss immer die APPXUPLOAD-Datei (nicht die APPX- oder die APPXBUNDLE-Datei) hochgeladen werden. Weitere Informationen zum Verpacken von UWP-Apps für den Store finden Sie unter [Verpacken universeller Windows-Apps für Windows10](../packaging/packaging-uwp-apps.md).

Wenn beim Überprüfen Ihrer Pakete Probleme erkannt werden, müssen Sie das Paket entfernen, das Problem beheben, und dann erneut versuchen, es hochzuladen. Weitere Informationen finden Sie unter [Beheben von Paketuploadfehlern](resolve-package-upload-errors.md).

In anderen Fällen werden Warnungen zu Fehlern angezeigt, die Probleme verursachen können, Sie jedoch nicht daran hindern, Ihre Übermittlung fortzusetzen.

## Verfügbarkeit von Gerätefamilien

Nachdem die Pakete erfolgreich hochgeladen wurden, wird im Abschnitt **Gerätefamilienverfügbarkeit** eine Tabelle angezeigt, die angibt, welche Pakete für bestimmte Windows10-Gerätefamilien (und ggf. für frühere Betriebssystemversionen) angeboten werden. In diesem Abschnittkönnen Sie auswählen, ob die Übermittlung Kunden mit bestimmten Windows10-Gerätefamilien angeboten werden soll.

> **Hinweis** Nachdem die Pakete erfolgreich hochgeladen wurden, wird im Abschnitt **Gerätefamilienverfügbarkeit** eine Tabelle mit Kontrollkästchen angezeigt, die angibt, ob die Übermittlung Kunden auf diesen Gerätefamilien angeboten werden soll. In der Tabelle wird angezeigt, sobald Sie Ihre Pakete hochgeladen haben.

Es wird auch ein Kontrollkästchen angezeigt, in dem Sie angeben können, ob Microsoft die App zukünftigen Windows 10-Gerätefamilien zur Verfügung stellen soll. Es wird empfohlen, diese Option aktiviert zu lassen, sodass Ihre App für mehr potenzielle Kunden zur Verfügung gestellt wird, wenn neue Gerätefamilien eingeführt werden.

### Auswählen der unterstützten Gerätefamilien

Sie können das Kontrollkästchen für eine Windows10-Gerätefamilie deaktivieren, wenn Sie Kunden auf diesem Gerät keine Übermittlung anbieten möchten. Wenn das Kontrollkästchen für eine Gerätefamilie deaktiviert ist, können neue Kunden die App auf diesem Gerätetyp nicht erwerben (Kunden, die bereits über die App verfügen, können diese allerdings weiterhin nutzen und erhalten alle von Ihnen übermittelten Aktualisierungen). 

> **Hinweis** Für **Windows 8/8.1** und **Windows Phone 8.x und älter** sind keine Kontrollkästchen vorhanden. Wenn Ihre Übermittlung Pakete umfasst, die unter diesen Betriebssystemversionen ausgeführt werden können, werden diese für Kunden verfügbar gemacht. Wenn Sie das Angebot der App für Kunden mit früheren Betriebssystemversionen beenden möchten, müssen Sie die entsprechenden Pakete aus Ihrer Übermittlung entfernen.

Wenn Ihre App Mobilgeräte- und Desktopgerätefamilien unterstützt, ist zu empfehlen, die Kontrollkästchen für **Windows10 Mobile** und **Windows10 Desktop** zu aktivieren, es sei denn, Sie haben einen bestimmten Grund, die Windows10-Gerätetypen, die Ihre App beziehen können, einzuschränken. Angenommen Sie haben z.B. universelle Windows-Pakete erstellt, müssen jedoch noch einigen Probleme mit der App auf Mobilgeräten testen. Um zu verhindern, dass neue Kunden die App auf Mobilgeräten mit Windows 10 herunterladen, können Sie hier das Kontrollkästchen **Windows 10 Mobile** deaktivieren. Wenn Sie die App dann später für Mobilgeräte mit Windows 10 anbieten möchten, können Sie eine neue Übermittlung Erstellen, bei der das Kontrollkästchen **Windows 10 Mobile** aktiviert ist.

Wenn Ihre App kein Spiel ist (oder wenn sie ein Spiel ist und Sie die [Konzeptgenehmigung](../gaming/concept-approval.md) durchlaufen haben), und Ihre Übermittlung UWP-Pakete enthält, die mit Windows10 SDK Version 14393 kompiliert wurden, können Sie das Kontrollkästchen **Windows10 Xbox** aktivieren, wenn Sie die App Kunden auf Xbox anbieten möchten. 

> **Wichtig** Ihr App kann nur dann auf Xbox-Geräten gestartet werden, wenn sie mit Windows SDK Version 14393 oder höher kompiliert wurde. Wenn Sie „Windows10 Xbox“ aktivieren, wird ein für Xbox geeignete Paket mit der Versionsnummer (d.h., ein Paket, das für die Gerätefamilien „Xbox“ oder „Universell“ bestimmt ist) Kunden auf Xbox jedoch immer angeboten – auch dann, wenn es mit einer früheren Version des SDK kompiliert wurde. Daher müssen Sie unbedingt sicherstellen, dass das für Xbox geeignete Paket mit der höchsten Versionsnummer mit Windows SDK-Version 14393 oder höher kompiliert wurde. Andernfalls wird eine Fehlermeldung angezeigt, in der darauf hingewiesen wird, dass Xbox-Kunden die App nicht starten können. 
> 
> Um diesen Fehler zu beheben, können Sie einen der folgenden Schritteausführen:
> - Ersetzen Sie die entsprechenden Pakete durch neue, die mit Windows SDK-Version 14393 oder höher kompiliert wurden.
> - Wenn Sie bereits über ein Paket verfügen, das Xbox unterstützt und mit Windows SDK-Version 14393 oder höher kompiliert wurde, erhöhen Sie die Versionsnummer, sodass dieses Paket die höchste Versionsnummer innerhalb der Übermittlung trägt.
> - Deaktivieren Sie das Kontrollkästchen für **Windows10 Xbox**.
>   
> Wenn Sie das Problem immer noch nicht beheben können, wenden Sie sich an den Support.

Wenn Sie Ihre App getestet und sichergestellt haben, dass sie unter Microsoft HoloLens ausgeführt werden kann, können Sie auch das Kontrollkästchen **Windows 10 Holographic** aktivieren, um sie HoloLens-Kunden anzubieten. Weitere Informationen zum Erstellen, Testen und Veröffentlichen von Hologramm-Apps finden Sie in der [Entwicklungsübersicht für Windows Holographic](http://dev.windows.com/holographic/development_overview).

> **Wichtig** Um zu verhindern, dass eine bestimmte Windows 10-Gerätefamilie Ihre App herunterladen kann, müssen Sie das [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903)-Element im Appx-Manifest so aktualisieren, dass die App nur auf die Gerätefamilie abzielt, die Sie unterstützen möchten (d. h. Windows.Mobile oder Windows.Desktop), statt den Windows.Universal-Wert zu übernehmen (für die universelle Gerätefamilie), den Microsoft Visual Studio standardmäßig im Appx-Manifest einschließt.

Beachten Sie, dass die hier getroffene Auswahl nur für neue Verkäufe gilt. Kunden, die Ihre App bereits verwenden, können dies weiterhin tun und erhalten alle zur Verfügung gestellten Updates, selbst wenn Sie diese Gerätefamilie an dieser Stelle entfernen. Dies gilt auch für Kunden, die Ihre App vor dem Upgrade auf Windows 10 erworben haben. Beispiel: Wenn Sie eine App mit Windows Phone 8.1-Paketen veröffentlicht haben und später ein Windows 10 (UWP)-Paket für die gleiche App hinzufügen, das auf die universelle Gerätefamilie abzielt, wird Kunden mit Mobilgeräten unter Windows 10, die bereits über das Windows Phone 8.1-Paket verfügen, ein Update auf dieses Windows 10 (UWP)-Paket angeboten, selbst wenn Sie das Kontrollkästchen für **Windows 10 Mobile** deaktiviert haben (da dies kein neuer Verkauf ist, sondern ein Update). Wenn Sie kein Windows 10 (UWP)-Paket bereitstellen, das auf die universelle oder Mobilgerätefamilie abzielt, bleibt Kunden mit Mobilgeräten mit Windows 10 weiterhin das Windows Phone 8.1-Paket zur Verfügung.

Weitere Informationen über Gerätefamilien finden Sie unter [Anleitung für UWP-Apps (Universal Windows Platform)](https://msdn.microsoft.com/library/windows/apps/dn894631) und [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903).

### Grundlegendes zur Bewertung

In diesem Abschnitt erfahren Sie nicht nur, wie Sie angeben, welche Windows10-Gerätefamilien Ihre Übermittlung herunterladen können, sondern auch, genau welche Pakete für bestimmte Gerätefamilien zur Verfügung gestellt werden. Wenn mehrere Ihrer Pakete auf einer bestimmten Gerätefamilie ausgeführt werden können, wird in der Tabelle die Reihenfolge angegeben, in der Pakete basierend auf der Versionsnummer angeboten werden. Weitere Informationen dazu, wie der Store Pakete auf Grundlage der Versionsnummern bewertet, finden Sie unter [Paketversionsnummern](package-version-numbering.md). 

Angenommen, Sie haben die beiden Pakete Package_A.appxupload und Package_B.appxupload. Wenn für eine bestimmte Gerätefamilie, Package_A.appxupload den Rang 1 und Package_B.appxupload den Rang 2 hat, bedeutet dies, das der Store an einen Kunden mit diesem Gerätetyp, der Ihre App erwirbt, zunächst Package_A.appxupload ausliefert. Wenn Package_A.appxupload auf den Gerät des Kunden nicht ausgeführt werden kann, bietet der Store Package_B.appxupload an. Wenn auf dem Gerät des Kunden eines der Pakete für diese Gerätefamilie nicht ausgeführt werden kann – wenn z.B. die von Ihrer App unterstützte **MinVersion** höher als die Version auf dem Gerät des Kunden ist – kann der Kunden die App nicht auf dem Gerät herunterladen.

> **Hinweis** Die Versionsnummern in XAP-Paketen werden beim Ermitteln der für einen gegebenen Kunden bereitzustellenden Pakete ignoriert. Daher wird bei mehreren gleichrangigen XAP-Paketen keine Nummer, sondern ein Sternchen angezeigt, und die Kunden können jedes der Pakete erhalten. Wenn ein XAP-Paket für einen Kunden auf ein neueres aktualisiert werden soll, stellen Sie sicher, dass die älteren XAP-Dateien aus der neuen Übermittlung entfernt werden.



## Paketdetails

Nachdem die Pakete erfolgreich hochgeladen wurden, werden sie nach Zielbetriebssystem aufgelistet. Name, Version und Architektur des Pakets werden angezeigt. Klicken Sie auf **Details anzeigen**, um weitere Informationen zu erhalten, z. B. die unterstützten Sprachen, die App-Funktionen oder die Dateigröße der einzelnen Pakete.

Bei Verwendung der [Windows-Anzeigenvermittlung](../monetize/use-ad-mediation-to-maximize-revenue.md) sehen Sie außerdem einen Link zum Konfigurieren der Anzeigenvermittlung für die einzelnen Pakete.

Wenn Sie ein Paket aus der Einsendung entfernen müssen, klicken Sie dazu im Abschnitt **Details** des Pakets unten auf den Link **Entfernen**.

## Entfernen redundanter Pakete

Wenn wir feststellen, dass mindestens eines Ihrer Pakete redundant ist, wird eine Warnung mit dem Vorschlag angezeigt, die redundanten Pakete aus dieser Übermittlung zu entfernen. Dieser Fehler tritt häufig auf, wenn Sie über zuvor hochgeladene Pakete verfügen und nun Pakete mit einer höheren Versionsnummer bereitstellen, die die gleiche Kundengruppe unterstützen. In diesem Fall würde das redundante Paket keinem Kunde bereitgestellt, weil nun ein besseres Paket (mit einer höheren Versionsnummer) verfügbar ist, um diese Kunden zu unterstützen.

Wenn wir feststellen, dass redundante Pakete vorhanden sind, wird eine Option bereitgestellt, um die redundanten Pakete aus dieser Übermittlung zu entfernen. Sie können die Pakete jedoch auch einzeln aus der Übermittlung entfernen.

## Schrittweises Paketrollout

Wenn Ihre Übermittlung ein Update einer bereits veröffentlichte App ist, wird ein Kontrollkästchen mit der Bezeichnung **Update-Rollout schrittweise nach Veröffentlichung dieser Übermittlung (nur für Windows10-Kunden)** angezeigt. Dadurch können Sie den Prozentsatz von Kunden auswählen, die die Pakete aus der Übermittlung erhalten. So können Sie Feedback und Analysedaten beobachten und vor einem umfassenden Rollout sicherstellen, dass das Update verlässlich ist. Sie können den Prozentsatz jederzeit erhöhen (oder das Update stoppen), ohne eine neue Übermittlung zu erstellen. 

Weitere Informationen finden Sie unter [Schrittweises Paketrollout](gradual-package-rollout.md).

## Erforderliches Update

Wenn die Übermittlung ein Update für eine bereits veröffentlichte App ist, wird ein Kontrollkästchen mit der Bezeichnung **Dieses Update obligatorisch machen** angezeigt. Damit können Sie Datum und Uhrzeit für ein obligatorisches Update festlegen, vorausgesetzt, Sie haben Ihrer App mithilfe der Windows.Services.Store-APIs erlaubt, programmgesteuert nach Paketupdates zu suchen und die aktualisierten Pakete herunterzuladen und zu installieren. Ihre App kann diese Option nur dann verwenden, wenn sie für Windows10, Version 1607 oder höher, bestimmt ist.

Weitere Informationen finden Sie unter [Herunterladen und Installieren von Paketupdates für Ihre App](../packaging/self-install-package-updates.md).

 







<!--HONumber=Aug16_HO5-->


