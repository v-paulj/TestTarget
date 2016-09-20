---
author: TylerMSFT
title: Lebenszyklus von Windows 10-UWP-Apps
description: "In diesem Thema wird der Lebenszyklus einer Windows 10-UWP-App (Universelle Windows-Plattform) von ihrer Aktivierung bis zum Schließen beschrieben."
keywords: App-Lebenszyklus angehalten fortsetzen starten aktivieren
ms.assetid: 6C469E77-F1E3-4859-A27B-C326F9616D10
translationtype: Human Translation
ms.sourcegitcommit: 79d0b4c7208d2fa79ceb7f5e212255ff94d6fe3a
ms.openlocfilehash: 86e0f10e45445df82585bd58b0f2583c07a19cb7

---

# Lebenszyklus von Windows 10-UWP-Apps (Universelle Windows-Plattform)

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

In diesem Thema wird der Lebenszyklus einer UWP-App (Universelle Windows-Plattform) vom Starten bis zum Schließen beschrieben.

## Etwas zur Vorgeschichte

Vor Windows 8 hatten Apps einen einfachen Lebenszyklus. Win32- und. NET-Apps werden entweder ausgeführt oder nicht. Wenn ein Benutzer eine App minimiert oder verlässt, wird sie weiterhin ausgeführt. Dies war in Ordnung, bis tragbare Geräte und die effiziente Energienutzung immer wichtiger wurden.

In Windows 8 wurde mit Windows Store-Apps ein neues Anwendungsmodell eingeführt. Auf oberer Ebene wurde der neue Zustand „Angehalten“ eingeführt. Kurze Zeit, nachdem der Benutzer eine Windows Store-App minimiert oder zu einer anderen App wechselt, wird sie angehalten. Das bedeutet, dass die App-Threads angehalten werden und die App im Arbeitsspeicher verbleibt, es sei denn, das Betriebssystem muss Ressourcen zurückfordern. Wenn der Benutzer zur App zurückkehrt, kann sie schnell wieder in den Zustand „Ausgeführt“ versetzt werden.

Es gibt verschiedene Möglichkeiten für Apps, die im Hintergrund weiter ausgeführt werden müssen, z. B. [Hintergrundaufgaben](support-your-app-with-background-tasks.md) oder Apps mit [erweiterter Ausführung](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.extendedexecution.aspx) und aktivitätsgestützter Ausführung. (Durch die **BackgroundMediaEnabled**-Funktion kann eine App beispielsweise weiterhin [Medien im Hintergrund wiedergeben](https://msdn.microsoft.com/en-us/windows/uwp/audio-video-camera/background-audio).) Darüber hinaus können Hintergrundübertragungen selbst dann fortgesetzt werden, wenn die App angehalten oder sogar beendet wurde. Weitere Informationen finden Sie unter [So wird’s gemacht: Herunterladen einer Datei](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj152726.aspx#downloading_a_file_using_background_transfer).

Apps, die sich nicht im Vordergrund befinden, werden standardmäßig angehalten. Dies führt zu Energieeinsparungen und dazu, dass mehr Ressourcen für die derzeit im Vordergrund ausgeführte App zur Verfügung stehen.

Der Zustand „Angehalten“ stellt neue Anforderungen an Entwickler, da das Betriebssystem entscheiden kann, eine angehaltene App zu beenden, um Ressourcen freizugeben. Die beendete App wird weiterhin in der Taskleiste angezeigt. Wenn der Benutzer darauf klickt, muss die App ihren Zustand vor dem Beenden wiederherstellen, da dem Benutzer nicht bewusst ist, dass die App vom System geschlossen wurde. Er geht davon aus, dass die App im Hintergrund gewartet hat, während er andere Aufgaben erledigt hat, und erwartet, dass sie denselben Zustand wie zu dem Zeitpunkt aufweist, als er die App verlassen hat. In diesem Thema wird erläutert, wie Sie dies erreichen.

In Windows 10, Version 1607, werden zwei weitere Zustände im App-Modell eingeführt: die **Ausführung im Vordergrund** und die **Ausführung im Hintergrund**. In den folgenden Abschnitten werden auch diese neuen Zustände eingehender betrachtet.

## App-Ausführungszustand

In der folgenden Abbildung sind die möglichen Zustände im App-Modell dargestellt, die ab Windows 10, Version 1607, möglich sind. Betrachten wir nun den typischen Lebenszyklus einer Windows Store-App.

![Zustandsdiagramm mit Übergängen zwischen App-Ausführungszuständen](images/updated-lifecycle.png)

Apps treten in den Zustand „Ausführung im Hintergrund“ ein, wenn sie gestartet oder aktiviert werden. Diese Begriffe sind scheinbar ähnlich, beziehen sich aber auf die unterschiedlichen Möglichkeiten, die das Betriebssystem zum Starten einer App hat. Zunächst betrachten wir das Starten einer App.

## Starten einer App

Die [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)-Methode wird aufgerufen, wenn eine App gestartet wird. An die Methode wird ein [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731)-Parameter übergeben, der u. a. die an die App übergebenen Argumente, den Bezeichner der Kachel, durch die die App gestartet wurde, und den vorherigen Zustand der App bereitstellt.

Den vorherigen Zustand Ihrer App können Sie von der [LaunchActivatedEventArgs.PreviousExecutionState](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.activation.launchactivatedeventargs.previousexecutionstate)-Eigenschaft abrufen, die eine [ApplicationExecutionState](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.applicationexecutionstate.aspx)-Enumeration zurückgibt. Im Folgenden sind die Werte und die Aktionen aufgeführt, die im jeweiligen Zustand ausgeführt werden sollten:

| ApplicationExecutionState | Erläuterung | Auszuführende Aktion |
|-------|-------------|----------------|
| **NotRunning** | Eine App kann diesen Zustand aufweisen, wenn sie seit dem letzten Neustart oder der letzten Anmeldung durch den Benutzer nicht gestartet wurde. Sie kann sich auch in diesem Zustand befinden, wenn sie während der Ausführung abgestürzt ist oder weil sie zuvor vom Benutzer geschlossen wurde.| Initialisieren Sie die App, als würde sie erstmalig in der aktuellen Benutzersitzung ausgeführt. |
|**Suspended** | Der Benutzer hat die App entweder minimiert oder verlassen und ist nicht innerhalb weniger Sekunden zurückgekehrt. | Beim Anhalten der App wurde ihr Zustand im Arbeitsspeicher beibehalten. Sie müssen lediglich Dateihandles oder andere Ressourcen erneut abrufen, die beim Anhalten der App freigegeben wurden. |
| **Terminated** | Die App wurde zuvor angehalten, dann aber irgendwann beendet, weil das System Arbeitsspeicher freigeben musste. | Stellen Sie den Zustand wieder her, in dem sich die App befand, als der Benutzer daraus gewechselt hat.|
|**ClosedByUser** | Der Benutzer hat die App im Tablet-Modus mit der Geste zum Schließen oder mit ALT+F4 geschlossen. Wenn der Benutzer die App schließt, wird sie zunächst angehalten und anschließend beendet. | Da die App im Wesentlichen dieselben Phasen durchlaufen hat, die zum Zustand „Terminated“ führen, behandeln Sie sie genauso wie eine App im Zustand „Terminated“.|
|**Running** | Die App war bereits geöffnet, als der Benutzer versucht hat, sie erneut zu starten. | Keine. Beachten Sie, dass keine weitere App-Instanz gestartet wird. Es wird einfach die bereits ausgeführte Instanz aktiviert. |


**Hinweis:** *Aktuelle Benutzersitzung* basiert auf der Windows-Anmeldung. Solange sich der aktuelle Benutzer nicht abgemeldet oder Windows heruntergefahren oder neu gestartet hat, bleibt die aktuelle Benutzersitzung unabhängig von bestimmten Ereignissen – wie Sperrbildschirmauthentifizierung, Benutzerwechsel usw. – bestehen. 

Folgendes sollte unbedingt beachtet werden: Wenn das Gerät über genügend Ressourcen verfügt, startet das Betriebssystem häufig verwendete Apps, für die dieses Verhalten zulässig ist, vorab, um die Reaktionsfähigkeit zu verbessern. Eine vorab gestartete App wird Hintergrund gestartet und dann schnell angehalten. Bei Bedarf kann sie fortgesetzt werden, wodurch der Benutzer schneller darauf zugreifen kann als bei einem Start der App.

Aufgrund des Vorabstarts kann die **OnLaunched()**-Methode der App eher vom System als vom Benutzer initiiert werden. Da der Vorabstart der App im Hintergrund erfolgt, müssen Sie in **OnLaunched()** u. U. unterschiedlich darauf reagieren. Wenn die App nach dem Starten beispielsweise Musik wiedergibt, weiß keiner, woher der Sound kommt, weil die App im Hintergrund vorab gestartet wurde. Auf den Vorabstart der App im Hintergrund folgt ein Aufruf von **Application.Suspending**. Wenn der Benutzer die App dann startet, wird sowohl das resuming-Ereignis als auch die **OnLaunched()**-Methode aufgerufen. Weitere Informationen dazu, wie Sie beim Vorabstartszenario vorgehen, finden Sie unter [Behandeln des Vorabstarts von Apps](handle-app-prelaunch.md). Es werden nur Apps vorab gestartet, für die dieser Vorgang zulässig ist.

Beim Start einer App zeigt Windows einen Begrüßungsbildschirm für die App an. Informationen zum Konfigurieren des Begrüßungsbildschirms finden Sie unter [Hinzufügen eines Begrüßungsbildschirms](https://msdn.microsoft.com/library/windows/apps/xaml/hh465331).

Während der Begrüßungsbildschirm angezeigt wird, sollte die App Ereignishandler registrieren und eine angepasste Benutzeroberfläche einrichten, die sie für die Startseite benötigt. Diese im Konstruktor der Anwendung und in **OnLaunched()** ausgeführten Aufgaben sollten möglichst innerhalb weniger Sekunden abgeschlossen sein, damit das System nicht annimmt, dass Ihre App nicht mehr reagiert, und sie beendet. Wenn eine App Daten aus dem Netzwerk anfordern oder große Datenmengen von einem Datenträger abrufen muss, sollten diese Aktivitäten außerhalb des Startvorgangs erfolgen. Eine App kann ihre eigene angepasste Ladebenutzeroberfläche oder einen erweiterten Begrüßungsbildschirm verwenden, während sie auf den Abschluss von Vorgängen mit langer Ausführungsdauer wartet. Weitere Infos finden Sie unter [Längere Anzeige des Begrüßungsbildschirms](create-a-customized-splash-screen.md) und im [Beispiel für einen Begrüßungsbildschirm](http://go.microsoft.com/fwlink/p/?linkid=234889).

Nach Abschluss des App-Starts wechselt die App in den Zustand **Ausgeführt**, und der Begrüßungsbildschirm sowie alle zugehörigen Ressourcen und Objekte werden ausgeblendet.

## Aktivieren einer App

Eine App kann nicht nur vom Benutzer gestartet, sondern auch vom System aktiviert werden. Eine App kann durch einen Vertrag wie den Freigabe-Vertrag aktiviert werden. Sie kann auch aktiviert werden, um ein benutzerdefiniertes URI-Protokoll oder eine Datei mit einer Erweiterung zu verarbeiten, für deren Verarbeitung die App registriert ist. Eine Liste der Methoden zum Aktivieren Ihrer App finden Sie unter [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693).

Die [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324)-Klasse definiert Methoden, die überschrieben werden können, um die unterschiedlichen Möglichkeiten zum Aktivieren der App zu behandeln.
[**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) kann alle möglichen Aktivierungstypen behandeln. Üblicherweise werden die meistverwendeten Aktivierungstypen jedoch mit spezifischen Methoden behandelt, während **OnActivated** als Fallbackmethode für weniger verbreitete Aktivierungstypen verwendet wird. Im Folgenden die zusätzlichen Methoden für spezifische Aktivierungen:

[**OnCachedFileUpdaterActivated**](https://msdn.microsoft.com/library/windows/apps/hh701797)  
[**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)  
[**OnFileOpenPickerActivated**](https://msdn.microsoft.com/library/windows/apps/hh701799)  
[**OnFileSavePickerActivated**](https://msdn.microsoft.com/library/windows/apps/hh701801)  
[**OnSearchActivated**](https://msdn.microsoft.com/library/windows/apps/br242336)  
[**OnShareTargetActivated**](https://msdn.microsoft.com/library/windows/apps/hh701806)

Die Ereignisdaten für diese Methoden enthalten dieselbe [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729)-Eigenschaft wie oben, an der Sie erkennen, in welchem Zustand sich die App vor der Aktivierung befunden hat. Um den Zustand zu interpretieren und die auszuführende Aktion zu ermitteln, gehen Sie auf dieselbe Weise vor, wie oben im Abschnitt [Starten einer App](#App launch) beschrieben.

**Hinweis** Wenn Sie sich mit dem Administratorkonto des Computers anmelden, können Sie keine UWP-Apps aktivieren.

## Ausführung im Hintergrund ##

Ab Windows 10, Version 1607, haben Sie die Möglichkeit, Code im Hintergrund innerhalb eines einzelnen Prozesses auszuführen. Weitere Informationen dazu finden Sie unter [Hintergrundaktivität mit dem Einzelprozessmodell](https://blogs.windows.com/buildingapps/2016/06/07/background-activity-with-the-single-process-model/#tMmI7wUuYu5CEeRm.99). In diesem Artikel gehen wir nicht weiter auf die Einzelprozessverarbeitung im Hintergrund ein. Sie beeinflusst den App-Lebenszyklus jedoch insofern, dass zwei neue Ereignisse implementiert wurden, die sich auf die Ausführung der App im Hintergrund beziehen. Die Ereignisse sind [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) und [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground).

Sie geben auch wieder, ob die Benutzeroberfläche Ihrer App für den Benutzer sichtbar ist.

Die Hintergrundausführung ist der Standardzustand, in dem eine Anwendung gestartet, aktiviert oder fortgesetzt wird. In diesem Zustand ist die Benutzeroberfläche Ihrer Anwendung noch nicht sichtbar.

## Ausführung im Vordergrund ##

Bei der Ausführung im Vordergrund ist die Benutzeroberfläche Ihrer App sichtbar.

Das **LeavingBackground**-Ereignis wird ausgelöst, kurz bevor die Benutzeroberfläche der Anwendung sichtbar wird und der Zustand „Ausführung im Vordergrund“ eintritt. Es wird auch ausgelöst, wenn der Benutzer wieder zu Ihrer App wechselt.

Zuvor war die beste Stelle zum Laden von UI-Ressourcen der **Activated**- oder **Resuming**-Ereignishandler. Ab jetzt bietet **LeavingBackground** die beste Möglichkeit, zu überprüfen, ob die Benutzeroberfläche bereit ist.

Außerdem sollte bis zu diesem Zeitpunkt sichergestellt sein, dass visuelle Ressourcen einsatzbereit sind, da Sie hier letzte Hand anlegen können, bevor die Anwendung für den Benutzer sichtbar wird. Alle UI-Vorgänge in diesem Ereignishandler sollten schnell abgeschlossen sein, da sie die vom Benutzer empfundene Dauer bis zum Starten oder Fortsetzen der App beeinflussen. In **LeavingBackground** sollte sichergestellt werden, dass der erste UI-Frame einsatzbereit ist. Anschließend sollten Speicher- oder Netzwerkaufrufe mit langer Ausführungsdauer asynchron behandelt werden, damit der Ereignishandler zurückgegeben werden kann.

Sobald der Benutzer die App verlässt, wechselt sie wieder in den Zustand „Ausführung im Hintergrund“.

## Zurückwechseln in den Hintergrundzustand

Das **EnteredBackground**-Ereignis gibt an, dass die App nicht mehr im Vordergrund angezeigt wird. Auf dem Desktop wird **EnteredBackground** ausgelöst, wenn Ihre App minimiert wird, und auf dem Handy, wenn der Benutzer zum Startbildschirm oder zu einer anderen App wechselt.

### Verringern des Speicherbedarfs Ihrer App

Da die App nicht mehr für den Benutzer sichtbar ist, bietet sich hier die Gelegenheit, das Rendern der Benutzeroberfläche und die Ausführung von Animationen zu beenden. Mit **LeavingBackground** können Sie diese Vorgänge erneut starten.

Falls Aufgaben im Hintergrund ausgeführt werden sollen, können Sie hier entsprechende Vorbereitungen treffen. Sie sollten [MemoryManager.AppMemoryUsageLevel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.memorymanager.appmemoryusagelevel.aspx) überprüfen und den von der App genutzten Arbeitsspeicher ggf. reduzieren, wenn sie im Hintergrund ausgeführt wird. So verringert sich das Risiko, dass die App vom System beendet wird, um Ressourcen freizugeben.

Weitere Details finden Sie unter [Reduzieren der Speicherverwendung, wenn die App in den Hintergrundzustand eintritt](reduce-memory-usage.md).

### Speichern des Zustands

Zuvor bot der suspending-Ereignishandler die beste Möglichkeit, den App-Zustand zu speichern. Wenn Sie jedoch Aufgaben im Hintergrund ausführen (z. B. die Audiowiedergabe oder eine erweiterte Ausführungssitzung nutzen), empfiehlt es sich, Ihre Daten asynchron im **EnteredBackground**-Ereignishandler zu speichern. Dies liegt daran, dass die App beendet werden kann, während sie sich im Hintergrund befindet. Und da die App in diesem Fall nicht den Zustand „Angehalten“ durchlaufen hat, gehen Ihre Daten verloren.

Durch das Speichern der Daten im **EnteredBackground**-Ereignishandler, bevor Hintergrundaktivitäten starten, ist eine optimale Benutzererfahrung gewährleistet, wenn der Benutzer Ihre App zurück in den Vordergrund holt. Zum Speichern von Daten und Einstellungen können Sie die Anwendungsdaten-APIs verwenden. Weitere Informationen finden Sie unter [Speichern und Abrufen von Einstellungen und anderen App-Daten](https://msdn.microsoft.com/library/windows/apps/mt299098).

Nachdem Sie Ihre Daten gespeichert haben, können Sie bei einer Überschreitung der maximalen Speicherauslastung die Daten aus dem Speicher freigeben, da Sie sie später erneut laden können. Dadurch wird Speicher freigegeben, der von den für Hintergrundaktivitäten benötigten Ressourcen genutzt werden kann.

Beachten Sie Folgendes: Wenn in Ihrer App Hintergrundaktivitäten ausgeführt werden, kann sie vom Zustand „Ausführung im Hintergrund“ in den Zustand „Ausführung im Vordergrund“ wechseln, ohne jemals den Zustand „Angehalten“ zu erreichen.

### Asynchrone Vorgänge und Verzögerungen

Wenn Sie innerhalb Ihres Handlers einen asynchronen Aufruf ausführen, wird die Steuerung sofort von diesem asynchronen Aufruf zurückgegeben. Das bedeutet, dass die Ausführung dann vom Ereignishandler zurückgegeben werden kann und die App in den nächsten Zustand übergeht, obwohl der asynchrone Aufruf noch nicht abgeschlossen wurde. Verwenden Sie die [**GetDeferral**](http://aka.ms/Kt66iv)-Methode für das an den Ereignishandler übergebene [**EnteredBackgroundEventArgs**](http://aka.ms/Ag2yh4)-Objekt, um das Anhalten zu verzögern, bis Sie für das zurückgegebene [**Windows.Foundation.Deferral**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.foundation.deferral.aspx)-Objekt die [**Complete**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.foundation.deferral.complete.aspx)-Methode aufgerufen haben.

Durch eine Verzögerung verlängert sich nicht die Zeit, die Ihr Code ausgeführt werden muss, bevor die App beendet wird. Dabei wird nur die Beendigung verzögert, bis die *Complete*-Methode der Verzögerung aufgerufen wird oder die Frist abläuft, *je nachdem, was zuerst eintritt*.

Falls Sie mehr Zeit zum Speichern des Zustands benötigen, sollten Sie nach Möglichkeiten suchen, den Zustand phasenweise zu speichern, bevor die App in den Hintergrundzustand eintritt, sodass weniger im **EnteredBackground**-Ereignishandler gespeichert werden muss. Alternativ können Sie auch [ExtendedExecutionSession](https://msdn.microsoft.com/en-us/magazine/mt590969.aspx) anfordern, um mehr Zeit zu erhalten. Da jedoch nicht sichergestellt ist, dass die Anforderung gewährt wird, sollten Sie Wege suchen, die zum Speichern des Zustands benötigte Zeit zu minimieren.

## Anhalten einer App

Wenn der Benutzer eine App minimiert, wird von Windows einige Sekunden abgewartet, ob der Benutzer zur App zurückkehrt. Wenn er nicht innerhalb dieses Zeitfensters zurückkehrt und keine erweiterte Ausführung, Hintergrundaufgabe oder aktivitätsgestützte Ausführung aktiv ist, wird die App von Windows angehalten. Eine App wird auch angehalten, wenn der Sperrbildschirm angezeigt wird, es sei denn, eine erweiterte Ausführungssitzung usw. ist in der App aktiv.

Wenn eine App angehalten wird, ruft sie das [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341)-Ereignis auf. Die UWP-Projektvorlagen in Visual Studio enthalten für dieses Ereignis den Handler mit dem Namen **OnSuspending** in **„App.Xaml.cs“**. Vor Windows 10, Version 1607, hätten Sie den Code zum Speichern des Zustands hier eingefügt. Mittlerweile wird empfohlen, den Zustand beim Eintreten in den Hintergrundzustand zu speichern, wie oben beschrieben.

Sie sollten exklusive Ressourcen und Dateihandles freigeben, damit andere Apps darauf zugreifen können, während Ihre App angehalten ist. Beispiele für exklusive Ressourcen sind Kameras, E/A-Geräte, externe Geräte und Netzwerkressourcen. Durch die explizite Freigabe exklusiver Ressourcen und Dateihandles kann sichergestellt werden, dass andere Apps darauf zugreifen können, während Ihre App angehalten ist. Wenn die App fortgesetzt wird, sollte sie ihre exklusiven Ressourcen und Dateihandles erneut abrufen.

### Achten Sie auf die Ablauffrist

Um zu gewährleisten, dass Geräte schnell und flexibel reagieren, ist die Zeit zum Ausführen von Code im suspending-Ereignishandler begrenzt. Sie wird als Frist bezeichnet, ist für jedes Gerät unterschiedlich und kann mithilfe einer Eigenschaft des [**SuspendingOperation**](https://msdn.microsoft.com/library/windows/apps/br224688)-Objekts ermittelt werden.

Hier verhält es sich genauso wie beim **EnteredBackground**-Ereignishandler: Wenn Sie innerhalb Ihres Handlers einen asynchronen Aufruf ausführen, wird die Steuerung sofort von diesem asynchronen Aufruf zurückgegeben. Das bedeutet, dass die Ausführung dann vom Ereignishandler zurückgegeben werden kann und die App in den Zustand „Angehalten“ übergeht, obwohl der asynchrone Aufruf noch nicht abgeschlossen wurde. Verwenden Sie die [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690)-Methode für das [**SuspendingOperation**](https://msdn.microsoft.com/library/windows/apps/br224688)-Objekt (verfügbar über die Ereignisargumente), um den Wechsel in den Zustand „Angehalten“ zu verzögern, bis Sie die [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685)-Methode für das zurückgegebene [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684)-Objekt aufgerufen haben.

Falls Sie mehr Zeit benötigen, können Sie auch [ExtendedExecutionSession](https://msdn.microsoft.com/en-us/magazine/mt590969.aspx) anfordern. Da jedoch nicht sichergestellt ist, dass die Anforderung gewährt wird, sollten Sie Wege suchen, die im **Suspended**-Ereignishandler benötigte Zeit zu minimieren.

### Beenden einer App

Das System versucht, die App und die zugehörigen Daten im Arbeitsspeicher beizubehalten, während sie angehalten ist. Wenn das System aber nicht über die notwendigen Ressourcen verfügt, um die App im Arbeitsspeicher beizubehalten, wird die App beendet. Apps erhalten keine Benachrichtigung, dass sie beendet werden. App-Daten können also nur im **OnSuspension**-Ereignishandler oder asynchron im **EnteredBackground**-Handler gespeichert werden.

Wenn Ihre App feststellt, dass sie nach dem Beenden aktiviert wurde, sollte sie die gespeicherten Anwendungsdaten laden, damit die App denselben Zustand wie vor dem Beenden aufweist. Wenn der Benutzer wieder zu einer angehaltenen App wechselt, die beendet wurde, sollte die App ihre Anwendungsdaten mithilfe der [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)-Methode wiederherstellen. Das System benachrichtigt eine App nicht, wenn sie beendet wird. Bevor Ihre App angehalten wird, muss sie die Anwendungsdaten speichern und exklusive Ressourcen und Dateihandles freigeben und diese wiederherstellen, wenn sie nach der Beendigung erneut aktiviert wird.

**Hinweis zum Debuggen mit Visual Studio:** Visual Studio verhindert, dass in Windows eine an den Debugger angefügte App angehalten wird. Dies hat den Zweck, dem Benutzer das Anzeigen der Debugging-Benutzeroberfläche von Visual Studio zu ermöglichen, während die App ausgeführt wird. Beim Debuggen einer App können Sie mit VisualStudio ein Anhalteereignis an die App senden. Stellen Sie sicher, dass die Symbolleiste **Debugspeicherort** angezeigt wird, und klicken Sie dann auf das Symbol **Anhalten**.

## Fortsetzen einer App

Eine angehaltene App wird fortgesetzt, wenn der Benutzer zu ihr wechselt oder es sich um die aktive App handelt und der Energiesparmodus für das Gerät beendet wird.

Wenn eine App aus dem Zustand **Angehalten** fortgesetzt wird, wechselt sie in den Zustand **Ausführung im Hintergrund**, und das System stellt die App so wieder her, wie sie verlassen wurde. So entsteht für den Benutzer der Eindruck, die App wäre im Hintergrund weiter ausgeführt worden. Im Arbeitsspeicher gespeicherte App-Daten gehen nicht verloren. Daher müssen die meisten Apps den Zustand nicht wiederherstellen, wenn sie fortgesetzt werden. Sie sollten allerdings alle Datei- oder Gerätehandles erneut abrufen, die sie beim Anhalten freigegeben haben, und jeden Zustand wiederherstellen, der beim Anhalten der App explizit freigegeben wurde.

Eine App kann für Stunden oder Tage angehalten werden. Wenn Inhalte oder Netzwerkverbindungen der App inzwischen veraltet sind, sollten diese beim Fortsetzen der App aktualisiert werden. Wenn eine App einen Ereignishandler für das [**Application.Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339)-Ereignis registriert hat, wird dieser beim Fortsetzen der App aus dem Zustand **Angehalten** aufgerufen. Sie können die App-Inhalte und -Daten in diesem Ereignishandler aktualisieren.

Wenn eine angehaltene App für die Teilnahme an einem App-Vertrag oder einer Erweiterung aktiviert wird, empfängt sie zunächst das **Resuming**-Ereignis und dann das **Activated**-Ereignis.

Wenn die angehaltene App beendet wurde, gibt es kein **Resuming**-Ereignis, und stattdessen wird **OnLaunched()** mit einem **ApplicationExecutionState** von **Terminated** aufgerufen. Da Sie den Zustand beim Anhalten der App gespeichert haben, können Sie ihn während **OnLaunched()** wiederherstellen. So hat der Benutzer den Eindruck, dass die App denselben Zustand aufweist wie zu dem Zeitpunkt, als er sie verlassen hat.

Angehaltene Apps empfangen keine Netzwerkereignisse, für deren Empfang sie registriert sind. Diese Netzwerkereignisse werden nicht in die Warteschlange gestellt, sondern einfach verpasst. Apps sollten beim Fortsetzen daher den Netzwerkstatus prüfen.

**Hinweis**  Da das [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339)-Ereignis nicht vom UI-Thread ausgelöst wird, muss ein Dispatcher verwendet werden, wenn der Code im Resume-Handler mit der Benutzeroberfläche kommuniziert. Ein Codebeispiel zur Vorgehensweise finden Sie unter [Aktualisieren des UI-Threads von einem Hintergrundthread](https://github.com/Microsoft/Windows-task-snippets/blob/master/tasks/UI-thread-access-from-background-thread.md).

Allgemeine Richtlinien finden Sie unter [Richtlinien für das Anhalten und Fortsetzen von Apps](https://msdn.microsoft.com/library/windows/apps/hh465088).

## Schließen einer App

Im Allgemeinen müssen Benutzer Apps nicht schließen, sondern können die Verwaltung Windows überlassen. Benutzer können Apps jedoch mit der Geste zum Schließen, durch Drücken von ALT+F4 oder mithilfe der Aufgabenumschaltfunktion in Windows Phone schließen.

Es gibt kein Ereignis zum Angeben, dass der Benutzer die App geschlossen hat. Wenn eine App durch den Benutzer geschlossen wird, wird sie zuerst angehalten, damit Sie ihren Zustand speichern können. In Windows 8.1 und höher wird eine App nach dem Schließen durch den Benutzer vom Bildschirm und aus der Umschaltliste entfernt, aber nicht explizit beendet.

**Verhalten nach dem Schließen durch den Benutzer:**  Wenn sich die App nach dem Schließen durch den Benutzer anders verhalten soll als nach dem Schließen durch Windows, können Sie mithilfe des Ereignishandlers für die Aktivierung ermitteln, ob die App durch den Benutzer oder durch Windows beendet wurde. Beschreibungen zu den Status **ClosedByUser** und **Terminated** finden Sie in der Referenz für die [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694)-Enumeration.

Wir raten dazu, dass Apps sich selbst nur dann programmgesteuert schließen sollten, wenn dies absolut erforderlich ist. Wenn eine App beispielsweise einen Arbeitsspeicherverlust erkennt, kann sie sich selbst schließen, um die Sicherheit der persönlichen Daten des Benutzers zu wahren.

## App-Absturz

Der Systemabsturz ist dafür vorgesehen, dass die Benutzer ihre vorherigen Aktivitäten so schnell wie möglich wieder aufnehmen können. Sie sollten kein Warnungsdialogfeld oder eine andere Benachrichtigung bereitstellen, da dies für den Benutzer eine Verzögerung verursacht.

Wenn die App abstürzt, nicht mehr reagiert oder eine Ausnahme generiert, wird über die [Feedback- und Diagnoseeinstellungen](http://go.microsoft.com/fwlink/p/?LinkID=614828) des Benutzers ein Problembericht an Microsoft gesendet. Microsoft stellt Ihnen einige der Fehlerdaten im Problembericht bereit, mit denen Sie Ihre App verbessern können. Sie können diese Daten auf der Seite „Qualität“ Ihrer App im Dashboard einsehen.

Wenn der Benutzer eine App nach einem Absturz aktiviert, empfängt ihr Ereignishandler einen [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694)-Wert **NotRunning**, und es sollten einfach die ursprüngliche Benutzeroberfläche und die ursprünglichen Daten der App angezeigt werden. Nach einem Absturz sollten Sie die App-Daten, die Sie für **Resuming** verwendet hätten, nicht routinemäßig mit **Suspended** verwenden, da diese Daten beschädigt sein können. Weitere Informationen finden Sie unter [Richtlinien für das Anhalten und Fortsetzen von Apps](https://msdn.microsoft.com/library/windows/apps/hh465088).

## Entfernen einer App

Löscht ein Benutzer eine App, wird die App mit allen zugehörigen lokalen Daten entfernt. Das Entfernen von Apps hat keine Folgen für die Benutzerdaten, die an allgemeinen Speicherorten, z.B. in den Dokument- oder Bildbibliotheken, gespeichert wurden.

## App-Lebenszyklus und Visual Studio-Projektvorlagen

Der grundlegende Code, der für den Lebenszyklus der App relevant ist, wird in den Visual Studio-Projektvorlagen bereitgestellt. Die einfache App behandelt die Startaktivierung, stellt einen Speicherort zum Wiederherstellen Ihrer App-Daten bereit und zeigt die primäre Benutzeroberfläche an, bevor Sie eigenen Code hinzugefügt haben. Weitere Informationen finden Sie unter [C#-, VB- und C++-Projektvorlagen für Apps](https://msdn.microsoft.com/library/windows/apps/hh768232).

## Wichtige APIs für den Anwendungslebenszyklus

-   [**Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691)-Namespace
-   [**Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766)-Namespace
-   [**Windows.ApplicationModel.Core**](https://msdn.microsoft.com/library/windows/apps/br205865)-Namespace
-   [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324)-Klasse (XAML)
-   [**Windows.UI.Xaml.Window**](https://msdn.microsoft.com/library/windows/apps/br209041)-Klasse (XAML)

**Hinweis**  
Dieser Artikel ist für Windows 10-Entwickler bestimmt, die Apps für die universelle Windows-Plattform (UWP) schreiben. Wenn Sie für Windows8.x oder Windows Phone8.x entwickeln, finden Sie Informationen in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

## Verwandte Themen

* [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694)
* [Richtlinien für das Anhalten und Fortsetzen von Apps](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [Behandeln des Vorabstarts von Apps](handle-app-prelaunch.md)
* [Behandeln der App-Aktivierung](activate-an-app.md)
* [Behandeln des Anhaltens von Apps](suspend-an-app.md)
* [Behandeln der App-Fortsetzung](resume-an-app.md)
* [Behandeln des Vorabstarts von Apps](handle-app-prelaunch.md)
* [Hintergrundaktivität mit dem Einzelprozessmodell](https://blogs.windows.com/buildingapps/2016/06/07/background-activity-with-the-single-process-model/#tMmI7wUuYu5CEeRm.99)
* [Wiedergeben von Medien im Hintergrund](https://msdn.microsoft.com/en-us/windows/uwp/audio-video-camera/background-audio)

 

 



<!--HONumber=Aug16_HO3-->


