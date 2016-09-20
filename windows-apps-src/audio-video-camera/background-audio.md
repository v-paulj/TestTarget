---
author: drewbatgit
ms.assetid: 
description: "In diesem Artikel wird erläutert, wie die Medienwiedergabe erfolgt, während die App im Hintergrund ausgeführt wird."
title: Wiedergeben von Medien im Hintergrund
translationtype: Human Translation
ms.sourcegitcommit: c8cbc538e0979f48b657197d59cb94a90bc61210
ms.openlocfilehash: a477827553ac1780ac625deeee08d84ab638d4c2

---

# Wiedergeben von Medien im Hintergrund
In diesem Artikel wird beschrieben, wie Sie Ihre App so konfigurieren, dass die Medienwiedergabe fortgesetzt wird, wenn die App vom Vordergrund in den Hintergrund wechselt. Dies bedeutet, dass die App weiterhin Audio wiedergeben kann, auch nachdem der Benutzer die App minimiert hat, zur Startseite zurückgekehrt ist oder die App auf andere Weise verlassen hat. 

Beispiele für Szenarien mit der Wiedergabe von Hintergrundaudio:

-   **Langzeit-Wiedergabelisten** Der Benutzer ruft kurz eine Vordergrund-App auf, um eine Wiedergabeliste auszuwählen und zu starten, und erwartet anschließend, dass die Wiedergabeliste kontinuierlich im Hintergrund wiedergegeben wird.

-   **Verwenden der Aufgabenumschaltfunktion:** Der Benutzer ruft kurz eine Vordergrund-App auf, um die Audiowiedergabe zu starten, und wechselt mit der Aufgabenumschaltfunktion dann zu einer anderen geöffneten App. Der Benutzer erwartet, dass die Audiowiedergabe im Hintergrund fortgesetzt wird.

Dank der in diesem Artikel beschriebenen Implementierung von Hintergrundaudio kann die App universell auf allen Windows-Geräten, einschließlich Mobile, Desktop und Xbox, ausgeführt werden.

> [!NOTE]
> Der Code in diesem Artikel wurde aus dem UWP-Beispiel für [Hintergrundaudio](http://go.microsoft.com/fwlink/p/?LinkId=800141) übernommen und angepasst.

## Erläuterung des Einzelprozessmodells
Mit Windows 10, Version 1607, wurde ein neues Einzelprozessmodell eingeführt, das die Aktivierung von Hintergrundaudio erheblich vereinfacht. Zuvor musste Ihre App zusätzlich zur Vordergrund-App einen Hintergrundprozess verwalten und die Statusänderungen zwischen den beiden Prozessen manuell kommunizieren. Beim neuen Modell fügen Sie Ihrem App-Manifest einfach die Hintergrundaudio-Funktion hinzu, damit die Audiowiedergabe automatisch fortgesetzt wird, wenn die App in den Hintergrund wechselt. Durch die beiden neuen App-Lebenszyklusereignisse [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) und [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) weiß Ihre App, wann sie in den Hintergrund wechselt bzw. den Hintergrund wieder verlässt. Beim Übergang der App in den bzw. aus dem Hintergrund können sich die vom System erzwungenen Speicherbeschränkungen ändern. Sie können diese Ereignisse also verwenden, um die aktuelle Arbeitsspeichernutzung zu überprüfen und Ressourcen freizugeben, damit Sie unter dem Grenzwert bleiben.

Durch den Wegfall der komplexen prozessübergreifenden Kommunikation und Zustandsverwaltung ermöglicht Ihnen das neue Modell, Hintergrundaudio mit deutlich geringerem Programmieraufwand viel schneller zu implementieren. Aus Gründen der Abwärtskompatibilität wird das aus zwei Prozessen bestehende Modell in der aktuellen Version jedoch weiterhin unterstützt. Weitere Informationen finden Sie unter [Hintergrundaudio-Modell (Legacy)](background-audio.md).

## Anforderungen für Hintergrundaudio
Ihre App muss die folgenden Anforderungen für die Audiowiedergabe erfüllen, während sie sich im Hintergrund befindet.

* Fügen Sie Ihrem App-Manifest die Funktion **Medienwiedergabe im Hintergrund** wie weiter unten in diesem Artikel beschrieben hinzu.
* Wenn die automatische Integration von **MediaPlayer** in die Steuerelemente für den Systemmedientransport (System Media Transport Controls, SMTC) von der App deaktiviert wird (z. B. durch Festlegen der [**CommandManager.IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled)-Eigenschaft auf FALSE), müssen Sie die manuelle Integration in SMTC implementieren, um die Medienwiedergabe im Hintergrund zu aktivieren. Die manuelle Integration in SMTC ist auch erforderlich, wenn Sie eine andere API als **MediaPlayer** (z. B. [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioGraph)) verwenden, um die Audiowiedergabe beim Wechsel der App in den Hintergrund fortzusetzen. Die Mindestanforderungen für die SMTC-Integration sind im Abschnitt „Verwenden der Steuerelemente für den Systemmedientransport für Audiowiedergabe im Hintergrund“ von [Manuelle Steuerung der Steuerelemente für den Systemmedientransport](system-media-transport-controls.md) beschrieben.
* Während sich Ihre App im Hintergrund befindet, müssen Sie unterhalb der Grenzwerte für die Speicherauslastung bleiben, die vom System für Hintergrund-Apps festgelegt wurden. Eine Anleitung zur Verwaltung des Arbeitsspeichers, während die App im Hintergrund ausgeführt wird, finden Sie später in diesem Artikel.

## Manifestfunktion „Medienwiedergabe im Hintergrund“
Wenn Sie Hintergrundaudio aktivieren möchten, müssen Sie der App-Manifestdatei „Package.appxmanifest“ die Funktion „Medienwiedergabe im Hintergrund“ hinzufügen. 

**So fügen Sie dem App-Manifest mithilfe des Manifest-Designers Funktionen hinzu**

1.  Öffnen Sie in MicrosoftVisual Studio im **Projektmappen-Explorer** den Designer für das Anwendungsmanifest, indem Sie auf das Element **package.appxmanifest** doppelklicken.
2.  Wählen Sie die Registerkarte **Funktionen** aus.
3.  Aktivieren Sie das Kontrollkästchen **Medienwiedergabe im Hintergrund**.

Wenn Sie die Funktion festlegen möchten, indem Sie die XML-Datei des App-Manifests manuell bearbeiten, müssen Sie zunächst sicherstellen, dass das Namespacepräfix *uap3* im **Package**-Element definiert ist. Wenn dies nicht der Fall ist, fügen Sie es gemäß den folgenden Schritten hinzu.
```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
```

Als Nächstes fügen Sie dem **Capabilities**-Element die *backgroundMediaPlayback*-Funktion hinzu:
```xml
<Capabilities>
    <uap3:Capability Name="backgroundMediaPlayback"/>
</Capabilities>
```

##Verarbeiten der Übergänge zwischen Vordergrund und Hintergrund
Wenn Ihre App vom Vordergrund in den Hintergrund wechselt, wird das [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground)-Ereignis ausgelöst. Wechselt die App dann wieder in den Vordergrund, wird das [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground)-Ereignis ausgelöst. Da es sich dabei um App-Lebenszyklusereignisse handelt, sollten Sie bei der App-Erstellung Handler für diese Ereignisse registrieren. In der Standardprojektvorlage wird der Handler dem **App**-Klassenkonstruktor in „App.xaml.cs“ hinzugefügt. Da die Ausführung im Hintergrund die Speicherressourcen belastet, die der App vom System zugewiesen werden, sollten Sie die App auch für das [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased)-Ereignis und das [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging)-Ereignis registrieren. Anhand dieser Ereignisse werden die aktuelle Speicherauslastung der App und der aktuelle Grenzwert überprüft. Die Handler für diese Ereignisse werden in den folgenden Beispielen erläutert. Weitere Informationen zum Anwendungslebenszyklus für UWP-Apps finden Sie unter [App-Lebenszyklus](../\launch-resume\app-lifecycle.md).

[!code-cs[RegisterEvents](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetRegisterEvents)]

Erstellen Sie eine Variable, um nachzuverfolgen, ob die App momentan im Hintergrund ausgeführt wird.

[!code-cs[DeclareBackgroundMode](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetDeclareBackgroundMode)]

Wenn das [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground)-Ereignis ausgelöst wird, legen Sie die Nachverfolgungsvariable fest, um anzugeben, dass die App momentan im Hintergrund ausgeführt wird. Langzeitaufgaben sollten im **EnteredBackground**-Ereignis nicht ausgeführt werden, da der Übergang in den Hintergrund dem Benutzer dadurch langsam erscheinen könnte.

[!code-cs[EnteredBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetEnteredBackground)]

Wenn Ihre App in den Hintergrund wechselt, wird das Arbeitsspeicherlimit der App vom System verringert, um sicherzustellen, dass die aktuelle Vordergrund-App über genügend Ressourcen verfügt, um ein zufriedenstellendes Reaktionsverhalten der App zu gewährleisten. Durch den [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging)-Ereignishandler wird der App mitgeteilt, dass der zugewiesene Arbeitsspeicher verringert wurde. Außerdem wird der neue Grenzwert dem Handler in den übergebenen Ereignisargumenten mitgeteilt. Vergleichen Sie die [**MemoryManager.AppMemoryUsage**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsage)-Eigenschaft, die den aktuell von der App genutzten Arbeitsspeicher angibt, mit der [**NewLimit**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLimitChangingEventArgs.NewLimit)-Eigenschaft der Ereignisargumente, die den neuen Grenzwert angibt. Wenn die Speicherauslastung den Grenzwert überschreitet, müssen Sie die Speicherauslastung verringern. In diesem Beispiel verwenden Sie zu diesem Zweck die Hilfsmethode **ReduceMemoryUsage**, die später in diesem Artikel beschrieben wird.

[!code-cs[MemoryUsageLimitChanging](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetMemoryUsageLimitChanging)]

> [!NOTE] 
> Einige Gerätekonfigurationen lassen es zu, dass eine Anwendung den neuen Grenzwert für die Speicherauslastung überschreiten darf, bis die Systemressourcen knapp werden. Dies gilt jedoch nicht für alle Gerätekonfigurationen. Insbesondere auf der Xbox werden Apps angehalten oder beendet, wenn sie ihre Speicherauslastung nicht innerhalb von 2 > > > Sekunden an die neuen Grenzwerte anpassen. Die beste Erfahrung auf möglichst vielen Geräten erzielen Sie mit diesem Ereignis, durch das die Ressourcenverwendung innerhalb von zwei Sekunden nach Auslösen des Ereignisses unter den Grenzwert gesenkt wird.


Es kann auch vorkommen, dass sich die Speicherauslastung einer App, wenn diese erst in den Hintergrund wechselt, unterhalb des Arbeitsspeicherlimits für Hintergrund-Apps befindet, dann später aber steigt und sich dem Grenzwert nähert. Mit dem Handler [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) können Sie die aktuelle Auslastung überprüfen und bei einem Anstieg ggf. Arbeitsspeicher freigeben. Überprüfen Sie, ob [**AppMemoryUsageLevel**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLevel) den Wert **High** oder **OverLimit** aufweist, und reduzieren Sie ggf. die Speicherauslastung. Der Prozess in diesem Beispiel wird ebenfalls von der Hilfsmethode **ReduceMemoryUsage** verarbeitet. Sie können auch das [**AppMemoryUsageDecreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageDecreased)-Ereignis abonnieren und überprüfen, ob sich die App unter dem Grenzwert befindet. Wenn dies der Fall ist, können Sie zusätzliche Ressourcen zuweisen.

[!code-cs[MemoryUsageIncreased](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetMemoryUsageIncreased)]

Sie können die Hilfsmethode **ReduceMemoryUsage** implementieren, um Arbeitsspeicher freizugeben, wenn die App den Auslastungsgrenzwert für Hintergrund-Apps überschreitet. Die Vorgehensweise bei der Freigabe von Arbeitsspeicher hängt von den Spezifikationen Ihrer App ab. Eine empfohlene Methode besteht darin, die Benutzeroberfläche und andere mit der App-Ansicht zusammenhängende Ressourcen freizugeben. Stellen Sie zunächst sicher, dass die App im Hintergrundmodus ausgeführt wird, und legen Sie die [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content)-Eigenschaft des App-Fensters dann auf NULL fest. Rufen Sie **GC.Collect** auf, um das System anzuweisen, den freigegebenen Arbeitsspeicher sofort zurückzufordern.

[!code-cs[UnloadViewContent](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetUnloadViewContent)]

Wenn der Fensterinhalt gesammelt wird, leitet jeder Frame seinen Trennungsprozess ein. Wenn der Fensterinhalt Seiten in der visuellen Objektstruktur enthält, beginnen diese mit dem Auslösen ihres [**Unloaded**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.Unloaded)-Ereignisses. Seiten können erst vollständig aus dem Arbeitsspeicher gelöscht werden, wenn alle Seitenverweise entfernt wurden. Führen Sie im **Unloaded**-Rückruf die folgenden Schritte aus, um sicherzustellen, dass der Arbeitsspeicher schnell freigegeben wird:
* Löschen Sie große Datenstrukturen auf der Seite, und legen Sie deren Wert auf NULL fest.
* Heben Sie die Registrierung aller Ereignishandler, die über Rückrufmethoden verfügen, innerhalb der Seite auf. Registrieren Sie diese Rückrufe beim Aufrufen des Loaded-Ereignishandlers für die Seite. Das Loaded-Ereignis wird ausgelöst, nachdem die Benutzeroberfläche wiederhergestellt und die Seite der visuellen Objektstruktur hinzugefügt wurde.
* Rufen Sie **GC.Collect** am Ende des Unloaded-Rückrufs auf, um schnell eine Garbage Collection für große Datenstrukturen durchzuführen, die Sie gerade auf NULL festgelegt haben.

[!code-cs[Unloaded](./code/BackgroundAudio_RS1/cs/MainPage.xaml.cs#SnippetUnloaded)]

Im [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground)-Ereignishandler sollten Sie die Nachverfolgungsvariable festlegen, um anzugeben, dass die App nicht mehr im Hintergrund ausgeführt wird. Überprüfen Sie als Nächstes, ob [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) für das aktuelle Fenster NULL ist. Dies ist der Fall, wenn Sie Ihre App-Ansichten geschlossen haben, um bei der Ausführung im Hintergrund Arbeitsspeicher freizugeben. Wenn der Fensterinhalt NULL ist, bauen Sie die App-Ansicht neu auf. In diesem Beispiel wird der Fensterinhalt in der Hilfsmethode **CreateRootFrame** erstellt.

[!code-cs[LeavingBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetLeavingBackground)]

Durch die **CreateRootFrame**-Hilfsmethode wird der Ansichtsinhalt Ihrer App neu erstellt. Der Code in dieser Methode ist identisch mit dem [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)-Handlercode in der Standardprojektvorlage. Der einzige Unterschied besteht darin, dass der **Launching**-Handler den vorherigen Ausführungsstatus anhand der [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs.PreviousExecutionState)-Eigenschaft von [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs) ermittelt und die **CreateRootFrame**-Methode einfach den vorherigen, als Argument übergebenen Ausführungsstatus abruft. Um doppelten Code zu minimieren, können Sie den Standardcode für den Ereignishandler **Launching** umgestalten, sodass ggf. **CreateRootFrame** aufgerufen wird.

[!code-cs[CreateRootFrame](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetCreateRootFrame)]

## Netzwerkverfügbarkeit für im Hintergrund ausgeführte Medien-Apps
Alle netzwerkfähigen Medienquellen, die nicht auf der Basis eines Datenstroms oder einer Datei erstellt wurden, behalten beim Abrufen von Remoteinhalten eine aktive Netzwerkverbindung bei. Andernfalls wird die Verbindung nicht beibehalten. [Insbesondere bei **MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaStreamSource) kommt es darauf an, dass die Anwendung den richtigen Pufferbereich mithilfe von [**SetBufferedRange**](https://msdn.microsoft.com/library/windows/apps/dn282762) an die Plattform übergibt. Nachdem der gesamte Inhalt vollständig gepuffert wurde, wird die Netzwerkverbindung nicht mehr für die App reserviert.

Wenn Sie Netzwerkaufrufe senden müssen, die im Hintergrund ausgeführt werden, wenn kein Mediendownload stattfindet, müssen Sie diese mit einer entsprechenden Aufgabe wie [**ApplicationTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.ApplicationTrigger), [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.MaintenanceTrigger) oder [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.TimeTrigger) umschließen. Weitere Informationen finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](https://msdn.microsoft.com/en-us/windows/uwp/launch-resume/support-your-app-with-background-tasks).

## Verwandte Themen
* [Medienwiedergabe](media-playback.md)
* [Wiedergeben von Audio- und Videoinhalten mit „MediaPlayer“](play-audio-and-video-with-mediaplayer.md)
* [Integration in die Steuerelemente für den Systemmedientransport](integrate-with-systemmediatransportcontrols.md)
* [Hintergrundaudio-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)

 

 







<!--HONumber=Aug16_HO3-->


