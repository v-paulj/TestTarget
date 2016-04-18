---
title: Starten, Fortsetzen und Hintergrundaufgaben
description: In diesem Abschnitt wird beschrieben, was passiert, wenn eine UWP-App gestartet, angehalten, fortgesetzt und beendet wird.
ms.assetid: 75011D52-1511-4ECF-9DF6-52CBBDB15BD7
---

# Starten, Fortsetzen und Hintergrundaufgaben


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


In diesem Abschnitt wird beschrieben, was passiert, wenn eine UWP-App gestartet, angehalten, fortgesetzt und beendet wird. Dabei wird erläutert, wie Apps mit einem Vertrag oder einer Erweiterung aktiviert werden können und wie Hintergrundaufgaben verwendet werden können, um UWP-Apps arbeiten zu lassen, selbst wenn die App nicht im Vordergrund ist. Abschließend wird beschrieben, wie Sie Ihrer App einen Begrüßungsbildschirm hinzufügen können.

## Der App-Lebenszyklus

| Thema                                            | Beschreibung                                                                                                     |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| [App-Lebenszyklus](app-lifecycle.md)               | Erfahren Sie mehr über den Lebenszyklus einer UWP-App und was passiert, wenn Windows Ihre App startet, anhält und fortsetzt. |
| [Behandeln des Vorabstarts von Apps](handle-app-prelaunch.md) | Hier erfahren Sie, wie der Vorabstart von Apps behandelt wird.                                                                              |
| [Behandeln der App-Aktivierung](activate-an-app.md)     | Hier erfahren Sie, wie die App-Aktivierung behandelt wird.                                                                             |
| [Behandeln des Anhaltens von Apps](suspend-an-app.md)         | Hier erfahren Sie, wie Sie wichtige Anwendungsdaten speichern, wenn das System die App anhält.                                 |
| [Behandeln der App-Fortsetzung](resume-an-app.md)           | Hier erfahren Sie, wie Sie den angezeigten Inhalt aktualisieren, wenn das System die App fortsetzt.                                        |

 

## Starten von Apps


| URI- und Dateiaktivierung                                                                         | Beschreibung                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Starten einer App für Ergebnisse](how-to-launch-an-app-for-results.md)                               | Hier erfahren Sie, wie Sie eine App aus einer anderen App heraus starten und Daten zwischen den beiden Apps austauschen.                                                                                             |
| [Starten der Standard-App für einen URI](launch-default-app.md)                                      | Hier erfahren Sie, wie Sie die Standard-App für einen Uniform Resource Identifier (URI) starten.                                                                                               |
| [Behandeln der URI-Aktivierung](handle-uri-activation.md)                                              | Erfahren Sie, wie Sie eine App registrieren müssen, damit sie der Standardhandler eines URI-Schemanamens wird.                                                                                          |
| [Starten der Standard-App für eine Datei](launch-the-default-app-for-a-file.md)                      | Erfahren Sie, wie Sie die Standard-App für einen Dateityp starten.                                                                                                                       |
| [Behandeln der Dateiaktivierung](handle-file-activation.md)                                            | Erfahren Sie, wie Sie Ihre App registrieren müssen, damit sie der Standardhandler für einen bestimmten Dateityp wird.                                                                                                  |
| [Richtlinien für Dateitypen und URIs](https://msdn.microsoft.com/library/windows/apps/hh700321) | Wenn Sie den Zusammenhang zwischen Windows UWP-Apps und den Dateitypen und Protokollen verstehen, die diese unterstützen, können Sie für eine einheitlichere und elegantere Bedienung Ihrer App sorgen. |
| [Reservierte Datei- und URI-Schemanamen](reserved-uri-scheme-names.md)                             | Dieses Thema listet die reservierten Datei- und URI-Schemanamen auf, die für Ihre App nicht verfügbar sind.                                                                                |
| Aktivieren integrierter Apps                                                                          | Beschreibung                                                                                                                                                                |
| [Starten der Einstellungs-App von Windows](launch-settings-app.md)                                      | Erfahren Sie, wie Sie die Einstellungs-App von Windows starten.                                                                                                                              |
| [Starten der Windows Store-App](launch-store-app.md)                                            | Erfahren Sie, wie Sie die Windows Store-App starten.                                                                                                                                 |
| [Starten der Windows-Karten-App](launch-maps-app.md)                                              | Erfahren Sie, wie Sie die Windows-Karten-App starten.                                                                                                                                  |

 

## Hintergrundaufgaben und Dienste



| Thema                                                                                                            | Beschreibung                                                                                                                                                                                   |
|------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Unterstützen Ihrer App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md)                             | In den Themen in diesem Abschnitt erfahren Sie, wie Sie einfachen Code im Hintergrund ausführen, indem Sie mit Hintergrundaufgaben auf Trigger reagieren.                                                       |
| [Zugreifen auf Sensoren und Geräte von einer Hintergrundaufgabe](access-sensors-and-devices-from-a-background-task.md)       | [
							Mit **DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) kann Ihre universelle Windows-App im Hintergrund auf Sensoren und Peripheriegeräte zugreifen. Dies ist selbst dann möglich, wenn die Vordergrund-App angehalten wird. |
| [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)                                           | Stellen Sie sicher, dass Ihre App die für die Ausführung von Hintergrundaufgaben erforderlichen Anforderungen erfüllt.                                                                                                                          |
| [Erstellen und Verwenden eines App-Diensts](how-to-create-and-consume-an-app-service.md)                                | Hier erfahren Sie, wie Sie eine UWP-App (Universelle Windows-Plattform) erstellen, die Dienste für andere UWP-Apps bereitstellen kann, und wie Sie diese Dienste nutzen.                                                                                  |
| [Erstellen und Registrieren einer Hintergrundaufgabe](create-and-register-a-background-task.md)                               | Erstellen Sie eine Hintergrundaufgabe, und registrieren Sie diese für die Ausführung, wenn sich die App nicht im Vordergrund befindet.                                                                                                 |
| [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)                                                           | Hier erfahren Sie, wie Sie eine Hintergrundaufgabe einschließlich Hintergrundaufgabenaktivierung und Debugablaufverfolgung im Windows-Ereignisprotokoll debuggen.                                                                        |
| [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md) | Sie können die Verwendung von Hintergrundaufgaben aktivieren, indem Sie diese im App-Manifest als Erweiterungen deklarieren.                                                                                                       |
| [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)                                     | Hier erfahren Sie, wie Sie eine Hintergrundaufgabe erstellen, die Abbruchanforderungen erkennt, die Ausführung beendet und den Abbruch mithilfe des beständigen Speichers an die App meldet.                                     |
| [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)           | Erfahren Sie, wie Ihre App den Status und Abschluss einer Hintergrundaufgabe erkennt.                                                                                                                     |
| [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)                                                     | Hier erfahren Sie, wie eine Funktion erstellt wird, die zum sicheren Registrieren der meisten Hintergrundaufgaben wiederverwendet werden kann.                                                                                                  |
| [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)             | Hier erfahren Sie, wie Sie eine Hintergrundaufgabe erstellen können, die auf [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839)-Ereignisse reagiert.                                                                         |
| [Ausführen einer Hintergrundaufgabe für einen Timer](run-a-background-task-on-a-timer-.md)                                        | Hier erfahren Sie, wie Sie eine einmalige Hintergrundaufgabe planen oder eine regelmäßige Hintergrundaufgabe ausführen.                                                                                                          |
| [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)                 | Erfahren Sie, wie Bedingungen festgelegt werden, die steuern, wann die Hintergrundaufgabe ausgeführt wird.                                                                                                                  |
| [Übertragen von Daten im Hintergrund](https://msdn.microsoft.com/library/windows/apps/mt280377)                                           | Verwenden Sie die API für Hintergrundübertragungen zum Kopieren von Dateien im Hintergrund.                                                                                                                              |
| [Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe](update-a-live-tile-from-a-background-task.md)                       | Verwenden Sie eine Hintergrundaufgabe, um die Live-Kachel Ihrer App mit neuen Inhalten zu aktualisieren.                                                                                                                      |
| [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)                                                       | Hier erfahren Sie, wie Sie die [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517)-Klasse zum Ausführen von einfachem Code im Hintergrund verwenden, während das Gerät angeschlossen ist.                             |

 

## Hinzufügen eines Begrüßungsbildschirms


Sämtliche UWP-Apps müssen einen Begrüßungsbildschirm aufweisen, der sich aus einem Begrüßungsbildschirmbild und einer Hintergrundfarbe zusammensetzt. Beide können angepasst werden.

Ihr Begrüßungsbildschirm wird sofort angezeigt, wenn Benutzer die App starten. Dadurch erhalten die Benutzer eine sofortige Rückmeldung, während die App-Ressourcen initialisiert werden. Sobald die App bereit für Interaktionen ist, wird der Begrüßungsbildschirm geschlossen.

Durch einen gut gestalteten Begrüßungsbildschirm kann Ihre App einladender wirken. Hier ist ein einfacher unauffälliger Begrüßungsbildschirm:

![Eine auf 75 % skalierte Bildschirmaufnahme des Begrüßungsbildschirms aus dem Begrüßungsbildschirmbeispiel.](images/regularsplashscreen.png)

Dieser Begrüßungsbildschirm wird durch Kombinieren eines grünen Hintergrunds mit einem transparenten PNG erstellt.

Das Zusammenstellen eines Bilds und einer Hintergrundfarbe zu einem Begrüßungsbildschirm hilft, diesen ansprechend zu gestalten, und zwar ungeachtet des Geräts, auf dem die betreffende App installiert ist. Bei Anzeige des Begrüßungsbildschirms wird lediglich die Größe des Hintergrunds geändert, um eine Anpassung an die Vielzahl möglicher Bildschirmgrößen zu ermöglichen. Ihr Bild bleibt stets unverändert.

Außerdem können Sie mit der [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763)-Klasse den Start Ihrer App anpassen. Sie können einen erweiterten, von Ihnen erstellten Begrüßungsbildschirm platzieren, damit Ihre App mehr Zeit für das Ausführen zusätzlicher Aufgaben, wie Vorbereiten der UI oder Abschließen von Netzwerkvorgängen, hat. Mit der **SplashScreen**-Klasse können Sie sich auch über das Schließen des Begrüßungsbildschirms benachrichtigen lassen, damit Sie Einführungsanimationen starten können.

| Thema                                                                          | Beschreibung                                                                                                                                                                                       |
|--------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Hinzufügen eines Begrüßungsbildschirms](add-a-splash-screen.md)                                 | Legen Sie das Bild und die Hintergrundfarbe des Begrüßungsbildschirms Ihrer App fest.                                                                                                                                          |
| [Längere Anzeige des Begrüßungsbildschirms](create-a-customized-splash-screen.md) | Verlängern Sie die Anzeige eines Begrüßungsbildschirms, indem Sie für die App einen erweiterten Begrüßungsbildschirm erstellen. Mit diesem erweiterten Bildschirm wird der beim Starten der App angezeigte Begrüßungsbildschirm imitiert, der angepasst werden kann. |

 

 

 





<!--HONumber=Mar16_HO1-->


