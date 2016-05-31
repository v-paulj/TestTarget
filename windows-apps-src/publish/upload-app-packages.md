---
author: jnHs
Description: Auf der Seite „Pakete“ werden alle Paketdateien (XAP, APPX, APPXUPLOAD und/oder APPXBUNDLE) für die App hochgeladen, die Sie übermitteln. Sie können in diesem Schritt Pakete für jedes Betriebssystem hochladen, auf das die App ausgerichtet ist.
title: Hochladen von App-Paketen
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
---

# Hochladen von App-Paketen


Auf der Seite **Pakete** werden alle Paketdateien (XAP, APPX, APPXUPLOAD und/oder APPXBUNDLE) für die App hochgeladen, die Sie übermitteln. Sie können in diesem Schritt Pakete für jedes Betriebssystem hochladen, auf das die App ausgerichtet ist. Wenn ein Kunde Ihre App herunterlädt, durchsucht der Store alle verfügbaren Pakete Ihrer App und stellt jedem Kunden das Paket bereit, das am besten für sein Gerät geeignet ist.

Ausführliche Informationen zu Inhalt und Struktur der Pakete finden Sie unter [App-Paketanforderungen](app-package-requirements.md). Sie erfahren außerdem, [wie sich Versionsnummern von Paketen darauf auswirken, welche Pakete bestimmten Kunden bereitgestellt werden](package-version-numbering.md) und [wie Pakete für verschiedene Betriebssysteme verteilt werden](guidance-for-app-package-management.md).

## Hochladen von Paketen für Ihre Übermittlung


Um Pakete hochzuladen, ziehen Sie sie in das Uploadfeld oder klicken, um Ihre Dateien zu durchsuchen. Auf der Seite **Pakete** können Sie die XAP-, APPX-, APPXUPLOAD- und/oder APPXBUNDLE-Dateien hochladen.

Falls Sie [Flight-Pakete](package-flights.md) für Ihre App erstellt haben, wird eine Dropdownliste mit der Option zum Kopieren von Paketen aus einem Ihrer Flight-Pakete angezeigt. Wählen Sie das Flight-Paket mit den Paketen aus, die Sie übertragen möchten. Anschließend können Sie einige oder alle der Pakete auswählen, um sie in diese Übermittlung aufzunehmen.

> **Wichtig**  Für Windows 10 muss immer die APPXUPLOAD-Datei (nicht die APPX- oder die APPXBUNDLE-Datei) hochgeladen werden. Weitere Informationen zum Verpacken von UWP-Apps für den Store finden Sie unter [Verpacken universeller Windows-Apps für Windows 10](../packaging/packaging-uwp-apps.md).

Wenn beim Überprüfen Ihrer Pakete Probleme erkannt werden, müssen Sie das Paket entfernen, das Problem beheben, und dann erneut versuchen, es hochzuladen. Weitere Informationen finden Sie unter [Beheben von Paketuploadfehlern](resolve-package-upload-errors.md).

In anderen Fällen werden Warnungen zu Fehlern angezeigt, die Probleme verursachen können, Sie jedoch nicht daran hindern, Ihre Übermittlung fortzusetzen.

## Paketdetails


Nachdem die Pakete erfolgreich hochgeladen wurden, werden sie nach Zielbetriebssystem aufgelistet. Name, Version und Architektur des Pakets werden angezeigt. Klicken Sie auf **Details**, um weitere Informationen zu erhalten, wie z. B. die unterstützten Sprachen, die App-Funktionen und die Dateigröße jedes Pakets.

Bei Verwendung der [Windows-Anzeigenvermittlung](../monetize/use-ad-mediation-to-maximize-revenue.md) sehen Sie außerdem einen Link zum Konfigurieren der Anzeigenvermittlung für die einzelnen Pakete.

Wenn Sie ein Paket aus der Einsendung entfernen müssen, klicken Sie dazu im Abschnitt **Details** des Pakets unten auf den Link **Entfernen**.

## Entfernen redundanter Pakete


Wenn wir feststellen, dass mindestens eines Ihrer Pakete redundant ist, wird eine Warnung mit dem Vorschlag angezeigt, die redundanten Pakete aus dieser Übermittlung zu entfernen. Dieser Fehler tritt häufig auf, wenn Sie über zuvor hochgeladene Pakete verfügen und nun Pakete mit einer höheren Versionsnummer bereitstellen, die die gleiche Kundengruppe unterstützen. In diesem Fall würde das redundante Paket keinem Kunde bereitgestellt, weil nun ein besseres Paket (mit einer höheren Versionsnummer) verfügbar ist, um diese Kunden zu unterstützen.

Wenn wir feststellen, dass redundante Pakete vorhanden sind, wird eine Option bereitgestellt, um die redundanten Pakete aus dieser Übermittlung zu entfernen. Sie können die Pakete aber auch einzeln aus der Übermittlung entfernen.

## Pakete mit Visual Studio Application Insights


Es wird empfohlen, [Visual Studio Application Insights](http://go.microsoft.com/fwlink/?LinkId=615086) in Paketen zu verwenden (oder durch Aktivieren des Kontrollkästchens „Telemetrie im Windows Dev Center anzeigen“ beim Erstellen des Pakets zu aktivieren), damit [Telemetriedaten zur App-Nutzung](usage-report.md) bereitgestellt werden können. Wenn Sie Application Insights in Microsoft Visual Studio nicht konfiguriert haben und wir feststellen, dass es nicht in einem Paket enthalten ist, werden Sie durch eine Meldung darauf hingewiesen, dass Sie sich durch Einreichen Ihres Pakets mit dem Aktivieren der Telemetrie zur App-Nutzung für Ihr Entwicklerkonto einverstanden erklären. Sie können die App-Nutzungstelemetrie jederzeit in den **Kontoeinstellungen** deaktivieren.

 

 






<!--HONumber=May16_HO2-->


