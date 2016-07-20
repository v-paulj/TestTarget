---
author: jnHs
Description: "Auf der Seite „Pakete“ werden alle Paketdateien (XAP, APPX, APPXUPLOAD und/oder APPXBUNDLE) für die App hochgeladen, die Sie übermitteln. Sie können in diesem Schritt Pakete für jedes Betriebssystem hochladen, auf das die App ausgerichtet ist."
title: Hochladen von App-Paketen
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
translationtype: Human Translation
ms.sourcegitcommit: 7f1a40f33a3137e4e0ded674b5bfdf35f11135dc
ms.openlocfilehash: f628820747f51f7200e2748c2c3f41b58455b2fa

---

# Hochladen von App-Paketen


Auf der Seite **Pakete** werden alle Paketdateien (XAP, APPX, APPXUPLOAD und/oder APPXBUNDLE) für die App hochgeladen, die Sie übermitteln. Sie können in diesem Schritt Pakete für jedes Betriebssystem hochladen, auf das die App ausgerichtet ist. Wenn ein Kunde Ihre App herunterlädt, durchsucht der Store alle verfügbaren Pakete Ihrer App und stellt jedem Kunden das Paket bereit, das am besten für sein Gerät geeignet ist.

Ausführliche Informationen zu Inhalt und Struktur der Pakete finden Sie unter [App-Paketanforderungen](app-package-requirements.md). Sie erfahren außerdem, [wie sich Versionsnummern von Paketen darauf auswirken, welche Pakete bestimmten Kunden bereitgestellt werden](package-version-numbering.md) und [wie Pakete für verschiedene Betriebssysteme verteilt werden](guidance-for-app-package-management.md).

## Hochladen von Paketen für Ihre Übermittlung


Um Pakete hochzuladen, ziehen Sie sie in das Uploadfeld oder klicken, um Ihre Dateien zu durchsuchen. Auf der Seite **Pakete** können Sie die XAP-, APPX-, APPXUPLOAD- und/oder APPXBUNDLE-Dateien hochladen.

Falls Sie [Flight-Pakete](package-flights.md) für Ihre App erstellt haben, wird eine Dropdownliste mit der Option zum Kopieren von Paketen aus einem Ihrer Flight-Pakete angezeigt. Wählen Sie das Flight-Paket mit den Paketen aus, die Sie übertragen möchten. Anschließend können Sie einige oder alle der Pakete auswählen, um sie in diese Übermittlung aufzunehmen.

> 
            **Wichtig**  Für Windows10 muss immer die APPXUPLOAD-Datei (nicht die APPX- oder die APPXBUNDLE-Datei) hochgeladen werden. Weitere Informationen zum Verpacken von UWP-Apps für den Store finden Sie unter [Verpacken universeller Windows-Apps für Windows10](../packaging/packaging-uwp-apps.md).

Wenn beim Überprüfen Ihrer Pakete Probleme erkannt werden, müssen Sie das Paket entfernen, das Problem beheben, und dann erneut versuchen, es hochzuladen. Weitere Informationen finden Sie unter [Beheben von Paketuploadfehlern](resolve-package-upload-errors.md).

In anderen Fällen werden Warnungen zu Fehlern angezeigt, die Probleme verursachen können, Sie jedoch nicht daran hindern, Ihre Übermittlung fortzusetzen.

## Paketdetails


Nachdem die Pakete erfolgreich hochgeladen wurden, werden sie nach Zielbetriebssystem aufgelistet. Name, Version und Architektur des Pakets werden angezeigt. Klicken Sie auf **Details**, um weitere Informationen zu erhalten, wie z. B. die unterstützten Sprachen, die App-Funktionen und die Dateigröße jedes Pakets.

Bei Verwendung der [Windows-Anzeigenvermittlung](../monetize/use-ad-mediation-to-maximize-revenue.md) sehen Sie außerdem einen Link zum Konfigurieren der Anzeigenvermittlung für die einzelnen Pakete.

Wenn Sie ein Paket aus der Einsendung entfernen müssen, klicken Sie dazu im Abschnitt **Details** des Pakets unten auf den Link **Entfernen**.

## Entfernen redundanter Pakete


Wenn wir feststellen, dass mindestens eines Ihrer Pakete redundant ist, wird eine Warnung mit dem Vorschlag angezeigt, die redundanten Pakete aus dieser Übermittlung zu entfernen. Dieser Fehler tritt häufig auf, wenn Sie über zuvor hochgeladene Pakete verfügen und nun Pakete mit einer höheren Versionsnummer bereitstellen, die die gleiche Kundengruppe unterstützen. In diesem Fall würde das redundante Paket keinem Kunde bereitgestellt, weil nun ein besseres Paket (mit einer höheren Versionsnummer) verfügbar ist, um diese Kunden zu unterstützen.

Wenn wir feststellen, dass redundante Pakete vorhanden sind, wird eine Option bereitgestellt, um die redundanten Pakete aus dieser Übermittlung zu entfernen. Sie können die Pakete jedoch auch einzeln aus der Übermittlung entfernen.


 







<!--HONumber=Jun16_HO5-->


