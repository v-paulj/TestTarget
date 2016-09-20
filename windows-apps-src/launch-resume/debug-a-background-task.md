---
author: TylerMSFT
title: Debuggen einer Hintergrundaufgabe
description: "Hier erfahren Sie, wie Sie eine Hintergrundaufgabe einschließlich Hintergrundaufgabenaktivierung und Debugablaufverfolgung im Windows-Ereignisprotokoll debuggen."
ms.assetid: 24E5AC88-1FD3-46ED-9811-C7E102E01E9C
translationtype: Human Translation
ms.sourcegitcommit: c05c004f99e5892291aaca0dcc85c176a821d351
ms.openlocfilehash: f7b311cef1d6a28b472f47985ebca437b7a80e3d

---

# Debuggen einer Hintergrundaufgabe

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Wichtige APIs**
-   [Windows.ApplicationModel.Background](https://msdn.microsoft.com/library/windows/apps/br224847)

Hier erfahren Sie, wie Sie eine Hintergrundaufgabe einschließlich Hintergrundaufgabenaktivierung und Debugablaufverfolgung im Windows-Ereignisprotokoll debuggen.

## Debuggen von Multiprozess- im Vergleich zu Einzelprozess-Hintergrundaufgaben
In diesem Thema werden hauptsächlich Hintergrundaufgaben behandelt, die in einem von der Host-App separaten Prozess ausgeführt werden. Wenn Sie eine Einzelprozess-Hintergrundaufgabe debuggen, verfügen Sie über kein separates Hintergrundaufgabenprojekt und können einen Haltepunkt auf **OnBackgroundActivated()** (wo Ihr Einzelprozess-Hintergrundcode ausgeführt wird) festlegen. Anleitungen zum Auslösen des Hintergrundcodes finden Sie unter Schritt 2 in [Manuelles Auslösen von Hintergrundaufgaben, um Hintergrundaufgabencode zu debuggen](#Trigger-background-tasks-manually-to-debug-background-task-code) unten.

## Sicherstellen, dass das Hintergrundaufgabenprojekt richtig eingerichtet wurde

In diesem Thema wird vorausgesetzt, dass Sie bereits über eine App mit einer Hintergrundaufgabe verfügen, die Sie debuggen möchten. Der folgende Code ist speziell für Hintergrundaufgaben geeignet, die in einem separaten Prozess ausgeführt werden und gilt nicht für Einzelprozess-Hintergrundaufgaben.

-   Stellen Sie für C# und C++ sicher, dass das Hauptprojekt auf das Hintergrundaufgabenprojekt verweist. Ist dieser Verweis nicht vorhanden, wird die Hintergrundaufgabe nicht in das App-Paket eingebunden.
-   Stellen Sie für C# und C++ sicher, dass der **Ausgabetyp** des Hintergrundaufgabenprojekts „Komponente für Windows-Runtime“ lautet.
-   Die Hintergrundklasse muss im Einstiegspunktattribut im Paketmanifest deklariert werden.

## Manuelles Auslösen von Hintergrundaufgaben, um Hintergrundaufgabencode zu debuggen

Hintergrundaufgaben können mit Microsoft Visual Studio manuell ausgelöst werden. Anschließend können Sie den Code schrittweise durchlaufen und ihn debuggen.

1.  Setzen Sie in C# einen Haltepunkt in der Run-Methode der Hintergrundklasse (für Einzelprozessaufgaben den Haltepunkt in App.OnBackgroundActivated()) festlegen) und/oder schreiben Sie mithilfe von [**System.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/xaml/hh441592.aspx) eine Debuggerausgabe.

    Setzne Sie in C++ einen Haltepunkt in der Run-Funktion der Hintergrundklasse (für Einzelprozessaufgaben den Haltepunkt in App.OnBackgroundActivated()) festlegen, und/oder schreiben Sie mithilfe von [**OutputDebugString**](https://msdn.microsoft.com/library/windows/desktop/aa363362) eine Debuggerausgabe.

2.  Führen Sie Ihre Anwendung im Debugger aus, und lösen Sie dann die Hintergrundaufgabe über die Symbolleiste **Zyklusereignisse** aus. In diesem Dropdownmenü werden die Namen der Hintergrundaufgaben angezeigt, die von Visual Studio aktiviert werden können.

    Dies funktioniert nur dann, wenn die Hintergrundaufgabe bereits registriert ist und noch auf den Auslöser wartet. Wenn eine Hintergrundaufgabe z. B. mit einem einmaligen TimeTrigger registriert wurde und der Trigger bereits ausgelöst wurde, hat ein Start der Aufgabe mit Visual Studio keine Wirkung.

> [!Note]
> Hintergrundaufgaben, die folgende Trigger verwenden, können nicht auf diese Weise aktiviert werden: [**Anwendungs-Trigger**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.applicationtrigger.aspx), [**MediaProcessing Trigger**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.mediaprocessingtrigger.aspx), [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032), [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) und Hintergrundaufgaben, die einen [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) mit dem Triggertyp [**SmsReceived**](https://msdn.microsoft.com/library/windows/apps/br224839) verwenden.  
> **Anwendungs-Trigger** und **MediaProcessingTrigger** können im Code mit `trigger.RequestAsync()` manuell signalisiert werden.     

    ![debugging background tasks](images/debugging-activation.png)

3.  Wenn die Hintergrundaufgabe aktiviert wird, wird sie vom Debugger übernommen, der die Debuggerausgabe dann in VS anzeigt.

## Debuggen der Aktivierung der Hintergrundaufgabe

> [!NOTE]
> Dieser Abschnitt behandelt speziell Hintergrundaufgaben, die in einem separaten Prozess ausgeführt werden und gilt nicht für Einzelprozess-Hintergrundaufgaben.

Die Aktivierung von Hintergrundaufgaben hängt von drei Dingen ab:

-   Der Name und Namespace der Hintergrundaufgabenklasse
-   Das im Paketmanifest angegebene Einstiegspunktattribut
-   Der Einstiegspunkt, der von der App bei der Registrierung der Hintergrundaufgabe angegeben wird

1.  Verwenden Sie Visual Studio, um den Einstiegspunkt der Hintergrundaufgabe zu notieren:

    -   Notieren Sie für C# und C++, den Namen und den Namespace der Hintergrundaufgabenklasse, die im Hintergrundaufgabenprojekt angegeben ist.

2.  Verwenden Sie den Manifest-Designer, um zu überprüfen, ob die Hintergrundaufgabe ordnungsgemäß im Paketmanifest deklariert wurde:

    -   Für C# und C++ muss das Einstiegspunktattribut dem Namespace der Hintergrundaufgabe gefolgt vom Klassennamen entsprechen. Beispiel: RuntimeComponent1.MyBackgroundTask.
    -   Alle Triggerarten, die mit der Aufgabe verwendet werden, müssen ebenfalls angegeben sein.
    -   Die ausführbare Datei DARF NICHT angegeben werden, es sei denn, Sie verwenden [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) oder [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543).

3.  Nur Windows Durch Aktivieren der Debugablaufverfolgung und Verwendung des Windows-Ereignisprotokolls können Sie den Einstiegspunkt ermitteln, der von Windows zum Aktivieren der Hintergrundaufgabe verwendet wird.

    Wenn Sie sich an dieses Verfahren halten und das Ereignisprotokoll den falschen Einstiegspunkt oder Trigger für die Hintergrundaufgabe anzeigt, wird die Hintergrundaufgabe nicht ordnungsgemäß von der App registriert. Weitere Informationen zu dieser Aufgabe finden Sie unter [Registrieren einer Hintergrundaufgabe](register-a-background-task.md).

    1.  Öffnen Sie die Ereignisanzeige, indem Sie zum Startbildschirm wechseln und nach „eventvwr.exe“ suchen.
    2.  Gehen Sie in der Ereignisanzeige zu **Anwendungs- und Dienstprotokolle** -&gt; **Microsoft** -&gt; **Windows** -&gt; **BackgroundTaskInfrastructure**.
    3.  Wählen Sie im Aktionsbereich **Ansicht** -&gt; **Analytische und Debugprotokolle einblenden**, um die Diagnoseprotokollierung zu aktivieren.
    4.  Wählen Sie die Option **Diagnoseprotokoll** aus, und klicken Sie auf **Protokoll aktivieren**.
    5.  Versuchen Sie nun, die App zu verwenden, um die Hintergrundaufgabe erneut zu registrieren und zu aktivieren.
    6.  Zeigen Sie detaillierte Fehlerinformationen im Diagnoseprotokoll an. Dies schließt den Einstiegspunkt ein, der für die Hintergrundaufgabe registriert ist.

![Ereignisanzeige für Debuginformationen für Hintergrundaufgaben](images/event-viewer.png)

## Hintergrundaufgaben und Bereitstellung des Visual Studio-Pakets

Wenn eine App mit Hintergrundaufgaben mit Visual Studio bereitgestellt wird und die im Manifest-Designer angegebene Haupt- oder Nebenversion aktualisiert wird, kann ein erneutes Bereitstellen der App mit Visual Studio dazu führen, dass die Hintergrundaufgabe der App hängt. Dieses Problem kann wie folgt behoben werden:

-   Stellen Sie die aktualisierte App mit Windows PowerShell (anstelle von Visual Studio) bereit, indem Sie das für das Paket erstellte Skript ausführen.
-   Wenn Sie die App bereits mit Visual Studio bereitgestellt haben und die Hintergrundaufgaben der App nun hängen, sollten Sie den Computer neu starten oder sich ab- und wieder anmelden, damit die Hintergrundaufgaben der App wieder funktionieren.
-   Sie können dies in C#-Projekten vermeiden, indem Sie die Debugoption "Paket immer neu installieren" auswählen.
-   Erhöhen Sie die Paketversion erst dann, wenn die App endgültig bereitgestellt werden kann (ändern Sie sie nicht während des Debuggens).

## Hinweise

-   Stellen Sie sicher, dass Ihre App nach vorhandenen Registrierungen von Hintergrundaufgaben sucht, bevor sie die Hintergrundaufgabe erneut registriert. Eine mehrfache Registrierung derselben Hintergrundaufgabe kann zu unerwarteten Ergebnissen führen, da die Hintergrundaufgabe mehrfach ausgelöst und ausgeführt wird.
-   Wenn für die Hintergrundaufgabe der Zugriff auf den Sperrbildschirm erforderlich ist, müssen Sie dafür sorgen, dass die App auf dem Sperrbildschirm platziert wird, bevor Sie versuchen, die Hintergrundaufgabe zu debuggen. Weitere Informationen zum Angeben von Manifestoptionen für sperrbildschirmfähige Apps finden Sie unter [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md).
-   Parameter für die Registrierung von Hintergrundaufgaben werden zum Zeitpunkt der Registrierung überprüft. Bei ungültigen Registrierungsparametern wird ein Fehler zurückgegeben. Stellen Sie sicher, dass Ihre App Szenarien, in denen die Registrierung von Hintergrundaufgaben einen Fehler verursacht, problemlos verarbeitet.

Weitere Informationen zum Debuggen einer Hintergrundaufgabe mit VS finden Sie unter [So wird's gemacht: Auslösen von Anhalte-, Fortsetzungs- und Hintergrundereignissen in Windows Store-Apps](https://msdn.microsoft.com/library/windows/apps/xaml/hh974425.aspx).

## Verwandte Themen

* [Erstellen und Registrieren einer in einem separaten Prozess ausgeführten Hintergrundaufgabe](create-and-register-a-background-task.md)
* [Erstellen und Registrieren einer Einzelprozess-Hintergrundaufgabe](create-and-register-a-singleprocess-background-task.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md)
* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
* [So wird's gemacht: Auslösen von Anhalte-, Fortsetzungs- und Hintergrundereignissen in Windows Store-Apps](https://msdn.microsoft.com/library/windows/apps/xaml/hh974425.aspx)
* [Analysieren der Codequalität von Windows Store-Apps mit der Codeanalyse von Visual Studio](https://msdn.microsoft.com/library/windows/apps/xaml/hh441471.aspx)

 

 



<!--HONumber=Aug16_HO3-->


