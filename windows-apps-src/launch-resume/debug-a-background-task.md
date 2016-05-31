---
author: mcleblanc
title: Debuggen einer Hintergrundaufgabe
description: Hier erfahren Sie, wie Sie eine Hintergrundaufgabe einschließlich Hintergrundaufgabenaktivierung und Debugablaufverfolgung im Windows-Ereignisprotokoll debuggen.
ms.assetid: 24E5AC88-1FD3-46ED-9811-C7E102E01E9C
---

# Debuggen einer Hintergrundaufgabe


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [Windows.ApplicationModel.Background](https://msdn.microsoft.com/library/windows/apps/br224847)

Hier erfahren Sie, wie Sie eine Hintergrundaufgabe einschließlich Hintergrundaufgabenaktivierung und Debugablaufverfolgung im Windows-Ereignisprotokoll debuggen.

In diesem Thema wird vorausgesetzt, dass Sie bereits über eine App mit einer Hintergrundaufgabe verfügen, die Sie debuggen möchten.

## Sicherstellen, dass das Hintergrundaufgabenprojekt richtig eingerichtet wurde


-   Stellen Sie für C# und C++ sicher, dass das Hauptprojekt auf das Hintergrundaufgabenprojekt verweist. Ist dieser Verweis nicht vorhanden, wird die Hintergrundaufgabe nicht in das App-Paket eingebunden.
-   Stellen Sie für C# und C++ sicher, dass der **Ausgabetyp** des Hintergrundaufgabenprojekts „Komponente für Windows-Runtime“ lautet.
-   Die Hintergrundklasse muss im Einstiegspunktattribut im Paketmanifest deklariert werden.

## Manuelles Auslösen von Hintergrundaufgaben, um Hintergrundaufgabencode zu debuggen


Hintergrundaufgaben können mit Microsoft Visual Studio manuell ausgelöst werden. Anschließend können Sie den Code schrittweise durchlaufen und ihn debuggen.

1.  Setzen Sie in C# einen Haltepunkt in der Run-Methode der Hintergrundklasse, und/oder schreiben Sie mit [**System.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/xaml/hh441592.aspx) eine Debuggerausgabe.

    Setzen Sie in C++ einen Haltepunkt in der Run-Funktion der Hintergrundklasse und/oder schreiben Sie mit [**OutputDebugString**](https://msdn.microsoft.com/library/windows/desktop/aa363362) eine Debuggerausgabe.

2.  Führen Sie Ihre Anwendung im Debugger aus, und lösen Sie dann die Hintergrundaufgabe mithilfe des Dropdownmenüs „Anhalten“ auf der Symbolleiste **Zyklusereignisse** aus. In diesem Dropdownmenü werden die Namen der Hintergrundaufgaben angezeigt, die von Visual Studio aktiviert werden können.

    Dies funktioniert nur dann, wenn die Hintergrundaufgabe bereits registriert ist und noch auf den Auslöser wartet. Wenn eine Hintergrundaufgabe z. B. mit einem einmaligen TimeTrigger registriert und der Trigger bereits ausgelöst wurde, hat ein Start der Aufgabe mit Visual Studio keine Wirkung.

    **Hinweis:**  Hintergrundaufgaben mit [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) oder [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) und Hintergrundaufgaben mit einem [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) mit dem Triggertyp [**SmsReceived**](https://msdn.microsoft.com/library/windows/apps/br224839) können nicht auf diese Weise aktiviert werden.     

    ![Debuggen von Hintergrundaufgaben](images/debugging-activation.png)

3.  Wenn die Hintergrundaufgabe aktiviert wird, wird sie vom Debugger übernommen, der die Debuggerausgabe dann in VS anzeigt.

## Debuggen der Aktivierung der Hintergrundaufgabe


Die Aktivierung von Hintergrundaufgaben hängt davon ab, ob drei Dinge ordnungsgemäß festgelegt sind. Das folgende Verfahren zeigt, wie Sie prüfen und sicherstellen können, dass sämtliche dieser Angaben richtig sind.

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
    2.  Wechseln Sie in der Ereignisanzeige zu **Anwendungs- und Dienstprotokolle** -&gt; **Microsoft** -&gt;**Windows** -&gt;**BackgroundTaskInfrastructure**.
    3.  Wählen Sie im Aktionsbereich **Ansicht** -&gt;**Analytische und Debugprotokolle einblenden**, um die Diagnoseprotokollierung zu aktivieren.
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
-   Wenn unter Windows für die Hintergrundaufgabe der Zugriff auf den Sperrbildschirm erforderlich ist, müssen Sie dafür sorgen, dass die App auf dem Sperrbildschirm platziert wird, bevor Sie versuchen, die Hintergrundaufgabe zu debuggen. Weitere Informationen zum Angeben von Manifestoptionen für sperrbildschirmfähige Apps finden Sie unter [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md).
-   Parameter für die Registrierung von Hintergrundaufgaben werden zum Zeitpunkt der Registrierung überprüft. Bei ungültigen Registrierungsparametern wird ein Fehler zurückgegeben. Stellen Sie sicher, dass Ihre App Szenarien, in denen die Registrierung von Hintergrundaufgaben einen Fehler verursacht, problemlos verarbeitet.

Weitere Informationen zum Debuggen einer Hintergrundaufgabe mit VS finden Sie unter [So wird's gemacht: Auslösen von Anhalte-, Fortsetzungs- und Hintergrundereignissen in Windows Store-Apps](https://msdn.microsoft.com/library/windows/apps/xaml/hh974425.aspx).

## Verwandte Themen

* [Erstellen und Registrieren einer Hintergrundaufgabe](create-and-register-a-background-task.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md)
* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
* [So wird's gemacht: Auslösen von Anhalte-, Fortsetzungs- und Hintergrundereignissen in Windows Store-Apps](https://msdn.microsoft.com/library/windows/apps/xaml/hh974425.aspx)
* [Analysieren der Codequalität von Windows Store-Apps mit der Codeanalyse von Visual Studio](https://msdn.microsoft.com/library/windows/apps/xaml/hh441471.aspx)

 

 





<!--HONumber=May16_HO2-->


