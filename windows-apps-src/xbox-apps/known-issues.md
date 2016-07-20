---
author: Mtoepke
title: Bekannte Probleme mit UWP auf Xbox One Developer Preview
description: 
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: e016be20af9a0d7a67fa383cbdc93083d12a1113

---

# Bekannte Probleme mit UWP auf Xbox Developer Preview

Dieses Thema beschreibt bekannte Probleme mit der UWP auf Xbox Developer Preview. Weitere Informationen zu dieser Developer Preview finden Sie unter [UWP auf Xbox](index.md). 

\[Wenn Sie über einen Link in einem API-Referenzthema hierher gelangten und nach Informationen zu APIs für die Universal-Gerätefamilie suchen, lesen Sie bitte [UWP-Funktionen, die noch nicht auf Xbox One unterstützt werden](http://go.microsoft.com/fwlink/?LinkID=760755).\]

Das Xbox Developer Preview System Update ist eine Vorabversion der Software für Testzwecke. Dies bedeutet, dass einige beliebte Spiele und Apps nicht wie erwartet funktionieren und gelegentlich Abstürze und Datenverlust auftreten. Wenn Sie die Developer Preview verlassen, wird Ihre Konsole auf die Werkseinstellungen zurückgesetzt, und Sie müssen alle Spiele, Apps und Inhalte erneut installieren.

Für Entwickler bedeutet dies, dass nicht alle Entwickler-Tools und -APIs wie erwartet funktionieren. Nicht alle für die endgültige Version vorgesehenen Features sind enthalten oder weisen die endgültige Qualität auf. 
**Insbesondere spiegelt die Systemleistung in dieser Preview nicht die Systemleistung der endgültigen Version wider.**

In der folgenden Liste werden einige bekannte Probleme aufgelistet, die in dieser Version auftreten können. Diese Liste ist jedoch nicht vollständig. 


              **Wir möchten gerne Ihr Feedback erhalten.** Melden Sie daher alle festgestellten Probleme im Forum für das [Entwickeln von universellen Windows-Apps](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

Wenn Sie Probleme haben, lesen Sie die Informationen in diesem Thema, informieren Sie sich in den [häufig gestellten Fragen](frequently-asked-questions.md), und nutzen Sie die Foren, um Hilfe zu erhalten.


<!--## Developing games-->

## Unterstützung des Mausmodus

Ab dieser Preview-Version ist der _Mausmodus_ standardmäßig für XAML- und gehostete Web-Apps aktiviert. Alle Anwendungen, für die die Option nicht deaktiviert wurde, erhalten einen Mauszeiger (ähnlich dem Zeiger im Edge-Browser von Xbox).

**Entwicklern wird dringend empfohlen, den Mausmodus zu deaktivieren und die Navigation per Controller (X-Y) zu optimieren.**

Befolgen Sie für das Deaktivieren des Mausmodus in XAML folgendes Beispiel:

```code
public App() {
    this.InitializeComponent();
    this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
    this.Suspending += OnSuspending;
}
```

Befolgen Sie für das Deaktivieren des Mausmodus in einer HTML-/JavaScript-App folgendes Beispiel:

```code
// Turn off mouse mode
navigator.gamepadInputEmulation = "keyboard";
```

Weitere Informationen einschließlich Informationen zum Aktivieren der direktionalen Navigation in einer HTML/JavaScript-App finden Sie unter [Deaktivieren des Mausmodus](how-to-disable-mouse-mode.md#html).

> 
              **Hinweis**&nbsp;&nbsp;Wenn der Mausmodus aktiviert ist, kann in dieser Developer Preview die Konsole bei der Verschiebung mit dem rechten Joystick am Controller einfrieren. Wenn dieses Problem auftritt, müssen Sie die Konsole neu starten.

Weitere Informationen zur Unterstützung des Mausmodus finden Sie im Thema [Entwerfen für Xbox und Fernsehgeräte](https://msdn.microsoft.com/windows/uwp/input-and-devices/designing-for-tv?f=255&MSPPError=-2147217396#mouse-mode). Dieses Thema enthält Informationen zum Aktivieren und Deaktivieren des Mausmodus, sodass Sie das richtige Verhalten für Ihre App auswählen können.

## Ein Benutzer muss angemeldet sein, damit Sie eine App bereitstellen können (Fehler 0x87e10008).

Damit Apps gestartet werden können, muss jetzt ein Benutzer angemeldet sein (damit Sie das Debuggen (F5) in VS 2015 starten können, muss ein Benutzer angemeldet sein). Die von Visual Studio derzeit ausgegebene Fehlermeldung ist nicht intuitiv:
 
![Die Windows Store-App kann nicht aktiviert werden.](images/windows-store-app-activation-error.jpg)
 
Um dieses Problem zu umgehen, melden Sie sich vor dem Bereitstellen Ihrer App als Benutzer über die Xbox-Shell oder DevHome an.
 
## Speicherlimits für Hintergrund-Apps werden noch nicht erzwungen.
 
Das Limit von 128MB für im Hintergrund ausgeführte Apps wird in dieser Preview-Version nicht erzwungen. Wenn Ihre App bei ihrer Ausführung im Hintergrund 128MB überschreitet, kann sie dennoch Speicher zuweisen.
 
Zurzeit kann dieses Problem nicht umgangen werden. Legen Sie entsprechende Richtlinien für die Speichernutzung fest. In einer künftigen Preview-Version werden für Ihre App Speicherzuweisungsfehler ausgegeben, wenn sie das Limit von 128MB überschreitet.
 
## Fehler bei der Bereitstellung aus VS bei aktiviertem Jugendschutz

Beim Starten Ihrer App in VS tritt ein Fehler auf, wenn in den Einstellungen der Konsole der Jugendschutz aktiviert ist.

Um dieses Problem zu umgehen, deaktivieren Sie vorübergehend den Jugendschutz. Alternative:
1. Bereitstellen Ihrer App in der Konsole mit deaktiviertem Jugendschutz
2. Aktivieren Sie den Jugendschutz.
3. Starten Sie Ihre App von der Konsole.
4. Geben Sie eine PIN oder ein Kennwort ein, um das Starten der App zuzulassen.
5. Die App wird gestartet.
6. Schließen Sie die App.
7. Starten Sie die App in VS mithilfe von F5. Sie wird ohne Benutzeraufforderung gestartet.

Zu diesem Zeitpunkt bleibt die Berechtigung so lange bestehen, __bis Sie den Benutzer abmelden, auch wenn Sie die App deinstallieren und neu installieren.
 
Es gibt eine weitere Ausnahme, die nur für Kinderkonten verfügbar ist. Bei einem Kinderkonto muss sich ein Elternteil anmelden, um die Berechtigung zu erteilen. In diesem Fall kann der Elternteil die Option **Immer** auswählen, um dem Kind das Starten der App zu erlauben. Diese Ausnahme wird in der Cloud gespeichert und bleibt bestehen, auch wenn sich das Kind abmeldet und wieder anmeldet.   

<!--### x86 vs. x64

By the time we release later this year, we will have great support for both x86 and x64, and we do support x86 in this preview. 
However, x64 has had much more testing to date (the Xbox shell and all of the apps running on the console today are x64), and so we recommend using x64 for your projects. 
This is particularly true for games.

If you decide to use x86, please report any issues you see on the forum.

Also see [Switching build flavors can cause deployment failures](known-issues.md#switching-build-flavors-can-cause-deployment-failures) later on this page.-->

<!--### Game engines

We have tested some popular game engines, but not all of them, and our test coverage for this preview has not been comprehensive. 
Your mileage may vary. 

The following game engines have been confirmed to work:
* [Construct 2](https://www.scirra.com/)

There are likely others that are working too. We would love to get your feedback on what you find. 
Please use the forum to report any issues you see.-->

## DirectX 12-Unterstützung

UWP auf Xbox One unterstützt die DirectX11-Featureebene10. DirectX12 wird derzeit nicht unterstützt. Xbox One ist wie alle herkömmlichen Spielekonsolen ein spezielles Hardwaregerät, das ein spezielles SDK erfordert, um das Potenzial vollständig nutzen zu können. Wenn Sie an einem Spiel arbeiten, das Zugriff auf das maximale Potenzial der Xbox One-Hardware benötigt, können Sie sich beim [ID@XBOX](http://www.xbox.com/Developers/id)-Programm registrieren, um Zugriff auf das SDK zu erhalten, das DirectX 12-Unterstützung enthält.

<!-- ### Xbox One Developer Preview disables game streaming to Windows 10

Activating the Xbox One Developer Preview on your console will prevent you from streaming games from your Xbox One to the Xbox app on Windows 10, even if your console is set to retail mode. 
To restore the game streaming feature, you must leave the developer preview. -->

## Bekanntes Problem mit dem sicheren TV-Bereich

Standardmäßig sollte der Anzeigebereich für UWP-Apps auf Xbox durch den sicheren TV-Bereich abgesenkt werden. Die Xbox One Developer Preview enthält jedoch ein bekanntes Problem, das dazu führt, dass der sichere TV-Bereich bei [0, 0] und nicht bei [_offset_, _offset_] beginnt.

> 
              **Hinweis**&nbsp;&nbsp;Dies gilt nur für UWP-Apps, die Javascript verwenden.

Die einfachste Möglichkeit, dieses Problem zu umgehen, besteht darin, den sicheren TV-Bereich zu deaktivieren, wie im folgenden JavaScript-Beispiel gezeigt.

    var applicationView = Windows.UI.ViewManagement.ApplicationView.getForCurrentView();

    applicationView.setDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.useCoreWindow);

Weitere Informationen zum sicheren TV-Bereich finden Sie unter [Entwerfen für Xbox und Fernsehgeräte](https://msdn.microsoft.com/windows/uwp/input-and-devices/designing-for-tv).

<!--## System resources for UWP apps and games on Xbox One

UWP apps and games running on Xbox One share resources with the system and other apps, and so the system governs the resources that are available to any one game or app. 
If you are running into memory or performance issues, this may be why. 
For more details, see [System resources for UWP apps and games on Xbox One](system-resource-allocation.md).-->

<!--
## Networking using traditional sockets

In this developer preview, inbound and outbound network access from the console that uses traditional TCP/UDP sockets (WinSock, Windows.Networking.Sockets) is not available. 
Developers can still use HTTP and WebSockets.
--> 


## UWP-API-Abdeckung

Nicht alle UWP-APIs werden auf Xbox unterstützt. Unter [UWP-Funktionen, die noch nicht auf Xbox One unterstützt werden](http://go.microsoft.com/fwlink/p/?LinkId=760755) finden Sie eine Liste der APIs, von denen bekannt ist, dass sie nicht funktionieren. Wenn Probleme mit anderen APIs auftreten, melden Sie dies bitte in den Foren. 

<!--## XAML controls do not look like or behave like the controls in the Xbox One shell

In this developer preview, the XAML controls are not in their final form. In particular:
* Gamepad X-Y navigation does not work reliably for all controls.
* Controls do not look like controls in the Xbox shell. This includes the control focus rectangle.
* Navigating between controls does not automatically make “navigation sounds.”

These issues will be addressed in a future developer preview.-->

<!--## Visual Studio and deployment issues

### Switching build flavors can cause deployment failures

Switching between Debug and Release builds, or between x86 and x64, or between Managed and .Net Native builds, can cause deployment failures. 

The simplest way to avoid these issues for this preview is to stick to Debug and one architecture. 

If you do hit this issue, uninstalling your app in the Collections app on your Xbox One will typically resolve it.

> ****&nbsp;&nbsp;Uninstalling your app from Windows Device Portal (WDP) will not resolve the issue.

If your issues persist, uninstall your app or game in the Collections app, leave Developer Mode, restart to Retail Mode and then switch back to Developer Mode.
You may also need to restart Visual Studio and clean your solution.

For more information, see the “Fixing deployment failures” section in [Frequently asked questions](frequently-asked-questions.md).

### Uninstalling an app while you are debugging it in Visual Studio will cause it to fail silently

Attempting to uninstall an app that is running under the debugger via the WDP “Installed Apps” tool will cause it to silently fail. 
The workaround is to stop debugging the app in Visual Studio before attempting to remove it via WDP.

### Visual Studio/Xbox PIN pairing failures

It is possible to get into a state where the PIN pairing between Visual Studio and your Xbox One gets out of sync. 
If PIN pairing fails, use the “Remove all pairings” button in Dev Home, restart Xbox One, restart your development PC, and then try again.--> 


## Windows Device Portal (WDP)-Preview

<!--### Starting WDP from Dev Home crashes Dev Home

When you start WDP in Dev Home, it will cause Dev Home to crash after you have entered your user name and password and selected **Save**. 
The credentials are saved but WDP is not started. 
You can start WDP by restarting Xbox One.--> 

<!--### Disabling WDP in Dev Home does not work

If you disable WDP in Dev Home, it will be turned off. 
However, when you restart your Xbox One, WDP will be started again. 
You can work around this issue by using **Reset and keep my games & apps** to delete any stored state on your Xbox One. 
Go to Settings > System > Console info & updates > Reset console, and then select the **Reset and keep my games & apps** button.

> **Caution**&nbsp;&nbsp;Doing this will delete all saved settings on your Xbox One including wireless settings, user accounts and any game progress that has not been saved to cloud storage.

> **Caution**&nbsp;&nbsp;DO NOT select the **Reset and remove everything** button.
This will delete all of your games, apps, settings and content, deactivate Developer Mode, and remove you console from the Developer Preview group.

### The columns in the “Running Apps” table do not update predictably. 

Sometimes this is resolved by sorting a column on the table.-->

### Die WDP-UI wird in Internet Explorer 7 nicht ordnungsgemäß angezeigt. 

Standardmäßig wird die WDP-UI nicht ordnungsgemäß im Browser angezeigt, wenn Sie Internet Explorer 7 verwenden. Sie können dies durch Deaktivieren der Kompatibilitätsansicht von Internet Explorer 7 für WDP beheben.

### Navigieren zu WDP führt zu einer Zertifikatwarnung

Sie erhalten eine Warnung zum Zertifikat, das bereitgestellt wurde, ähnlich wie folgender Screenshot, dass das von Ihrer Xbox One-Konsole signierte Sicherheitszertifikat nicht als bekannter vertrauenswürdiger Herausgeber betrachtet wird. Klicken Sie auf „Laden dieser Website fortsetzen“, um auf das Windows Device Portal zuzugreifen.

![Warnung zu Website-Sicherheitszertifikat](images/security_cert_warning.jpg)

<!--## Dev Home

Occasionally, selecting the “Manage Windows Device Portal” option in Dev Home will cause Dev Home to silently exit to the Home screen. 
This is caused by a failure in the WDP infrastructure on the console and can be resolved by restarting the console.-->

## Siehe auch
- [Häufig gestellte Fragen](frequently-asked-questions.md)
- [UWP auf XboxOne](index.md)



<!--HONumber=Jul16_HO2-->


