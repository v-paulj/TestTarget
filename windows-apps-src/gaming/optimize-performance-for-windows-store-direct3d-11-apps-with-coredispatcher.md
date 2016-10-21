---
author: mtoepke
title: "Optimieren der Eingabelatenz für UWP-DirectX-Spiele (Universelle Windows-Plattform)"
description: "Die Eingabelatenz kann das Spielerlebnis erheblich beeinträchtigen, und Spiele wirken professioneller, wenn in diesem Bereich eine Optimierung vorgenommen wird."
ms.assetid: e18cd1a8-860f-95fb-098d-29bf424de0c0
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: ae99f88126192866ed18df55497af6390bc38c26

---

#  Optimieren der Eingabelatenz für UWP-DirectX-Spiele (Universelle Windows-Plattform)


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Die Eingabelatenz kann das Spielerlebnis erheblich beeinträchtigen. Spiele wirken professioneller, wenn in diesem Bereich eine Optimierung vorgenommen wird. Außerdem kann eine richtige Optimierung der Eingabeereignisse zu einer besseren Akkulaufzeit führen. Hier erfahren Sie, wie Sie die richtigen Verarbeitungsoptionen für CoreDispatcher-Eingabeereignisse auswählen, um sicherzustellen, dass die Eingaben im Spiel so reibungslos wie möglich verarbeitet werden.

## Eingabelatenz


Die Eingabelatenz ist der Zeitraum, den das System benötigt, um auf Benutzereingaben zu reagieren. Die Reaktion ist häufig eine Änderung der Anzeige auf dem Bildschirm oder der Audioausgabe.

Jedes Eingabeereignis – ob über einen Toucheingabezeiger, einen Mauszeiger oder eine Tastatur – generiert eine Nachricht, die von einem Ereignishandler bearbeitet wird. Moderne Touchdigitalisierungsgeräte und Gaming-Peripheriegeräte melden Eingabeereignisse mit mindestens 100 Hz pro Zeiger. Dies bedeutet, dass Ihre App pro Sekunde pro Zeiger (oder Tastaturanschlag) mindestens 100 Ereignisse empfangen kann. Diese Aktualisierungsrate wird noch verstärkt, wenn mehrere Zeiger gleichzeitig oder ein Eingabegerät mit höherer Präzision (z. B. eine Gamingmaus) verwendet werden. Die Warteschlange für Ereignismeldungen kann sich also sehr schnell füllen.

Es ist wichtig, dass Sie für Ihr Spiel die Anforderungen an die Eingabelatenz verstehen, damit Ereignisse für den jeweiligen Fall auf bestmögliche Weise verarbeitet werden. Es gibt keine Standardlösung für alle Spiele.

## Effiziente Nutzung der Energie


Im Zusammenhang mit der Eingabelatenz geht es bei der effizienten Nutzung der Energie darum, wie stark die GPU vom Spiel genutzt wird. Ein Spiel, bei dem weniger GPU-Ressourcen genutzt werden, ist energieeffizienter und ermöglicht eine längere Akkulaufzeit. Dies gilt auch für die CPU.

Wenn ein Spiel den gesamten Bildschirm mit weniger als 60Frames pro Sekunde zeichnen kann (derzeit die maximale Rendergeschwindigkeit der meisten Displays), ohne das Spielerlebnis zu beeinträchtigen, ist die Energieeffizienz höher, weil weniger oft gezeichnet werden muss. Bei einigen Spielen wird der Bildschirm nur als Reaktion auf Benutzereingaben aktualisiert, sodass die gleichen Inhalte nicht immer wieder mit 60Frames pro Sekunde gezeichnet werden müssen.

## Auswählen der Optimierungsziele


Beim Entwerfen einer DirectX-App müssen Sie einige Entscheidungen treffen. Müssen für die App 60Frames pro Sekunde gerendert werden, um eine reibungslose Animation zu gewährleisten, oder muss nur als Reaktion auf Eingaben gerendert werden? Muss die geringstmögliche Eingabelatenz verwendet werden, oder ist eine kurze Verzögerung tolerierbar? Erwarten die Benutzer, dass bei der App auf die Akkunutzung geachtet wird?

Die Antworten auf diese Fragen führen für die App in der Regel zu einem der folgenden Fälle:

1.  Rendern bei Bedarf: Für Spiele dieser Kategorie muss der Bildschirm nur als Reaktion auf bestimmte Arten von Eingaben aktualisiert werden. Die Energieeffizienz ist sehr gut, weil von der App identische Frames nicht immer wieder gerendert werden, und die Eingabelatenz ist niedrig, da die App die meiste Zeit mit dem Warten auf eine Eingabe verbringt. Brettspiele und Newsreader sind Beispiele für Apps, die ggf. in diese Kategorie fallen.
2.  Rendern bei Bedarf mit kurzlebigen Animationen Dieser Fall ähnelt dem ersten Fall, mit der Ausnahme, dass bestimmte Arten von Eingaben eine Animation auslösen, die nicht von Folgeeingaben des Benutzers abhängig ist. Die Energieeffizienz ist gut, weil identische Frames vom Spiel nicht immer wieder gerendert werden, und die Eingabelatenz ist niedrig, während im Spiel keine Animationen aktiv sind. Interaktive Spiele für Kinder und Brettspiele, bei denen jeder Zug animiert ist, sind Beispiele für Apps, die ggf. in diese Kategorie fallen.
3.  Rendern von 60Frames pro Sekunde: In diesem Fall wird der Bildschirm vom Spiel ständig aktualisiert. Die Energieeffizienz ist nicht gut, weil die maximale Anzahl Frames, die vom Display dargestellt werden kann, gerendert wird. Die Eingabelatenz ist hoch, da der Thread während der Darstellung der Inhalte von DirectX blockiert wird. So wird der Thread daran gehindert, mehr Frames an das Display zu senden, als dem Benutzer angezeigt werden können. Ego-Shooter, Echtzeit-Strategiespiele und Spiele auf physischer Basis sind Beispiele für Apps, die ggf. in diese Kategorie fallen.
4.  Rendern von 60Frames pro Sekunde und Erzielen der geringstmöglichen Eingabelatenz Die App aktualisiert den Bildschirm, ähnlich wie im dritten Fall, ständig, und die Energieeffizienz ist schlecht. Der Unterschied besteht darin, dass das Spiel über einen separaten Thread auf Eingaben reagiert. Daher wird die Eingabeverarbeitung beim Darstellen von Grafiken auf dem Display nicht blockiert. Onlinespiele mit mehreren Spielern, Kampfspiele oder Spiele auf Rhythmus- bzw. Zeitbasis fallen ggf. in diese Kategorie, da Eingaben innerhalb von extrem engen Ereignisfenstern unterstützt werden.

## Implementierung


Die meisten DirectX-Spiele basieren auf der so genannten Spielschleife. Der grundlegende Algorithmus besteht darin, die folgenden Schritte auszuführen, bis der Benutzer das Spiel oder die App beendet:

1.  Eingabe verarbeiten
2.  Spielzustand aktualisieren
3.  Inhalte des Spiels zeichnen

Wenn die Inhalte eines DirectX-Spiels gerendert und bereit für die Darstellung auf dem Bildschirm sind, wartet die Spielschleife, bis die GPU einen neuen Frame empfangen kann, bevor erneut Eingaben verarbeitet werden.

Die Implementierung der Spielschleife wird unten für die einzelnen beschriebenen Fälle veranschaulicht, indem der Prozess eines einfachen Puzzlespiels durchlaufen wird. Die Entscheidungspunkte, Vorteile und Nachteile, die Teil jeder Implementierung sind, dienen Ihnen als Unterstützung beim Optimieren Ihrer Apps in Bezug auf eine niedrige Eingabelatenz und eine hohe Energieeffizienz.

## Szenario1: Rendern bei Bedarf


Beim ersten Durchlauf des Puzzlespiels wird der Bildschirm nur aktualisiert, wenn ein Benutzer ein Puzzleteil verschiebt. Benutzer können ein Puzzleteil entweder an seinen Platz ziehen oder das Teil auswählen und dann auf die richtige Position tippen. Im letzteren Fall wird das Puzzleteil ohne Animation oder Effekte eingefügt.

Der Code umfasst eine Spielschleife mit einem einzelnen Thread in der [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505)-Methode, für die das **CoreProcessEventsOption::ProcessOneAndAllPending**-Element verwendet wird. Mit dieser Option werden alle derzeit verfügbaren Ereignisse in der Warteschlange verteilt. Falls keine Ereignisse ausstehen, wartet die Spielschleife, bis ein Ereignis vorhanden ist.

``` syntax
void App::Run()
{
    
    while (!m_windowClosed)
    {
        // Wait for system events or input from the user.
        // ProcessOneAndAllPending will block the thread until events appear and are processed.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);

        // If any of the events processed resulted in a need to redraw the window contents, then we will re-render the
        // scene and present it to the display.
        if (m_updateWindow || m_state->StateChanged())
        {
            m_main->Render();
            m_deviceResources->Present();

            m_updateWindow = false;
            m_state->Validate();
        }
    }
}
```

## Szenario2: Rendern bei Bedarf mit kurzlebigen Animationen


Beim zweiten Durchlauf wird das Spiel modifiziert. Wenn Benutzer ein Puzzleteil auswählen und dann auf die richtige Position für das Teil tippen, wird es auf dem Bildschirm per Animation an seine Zielposition verschoben.

Wie im ersten Szenario verfügt der Code über eine Spielschleife mit einem einzelnen Thread, die mithilfe von **ProcessOneAndAllPending** die in der Warteschlange enthaltenen Eingabeereignisse verteilt. Der Unterschied besteht jetzt darin, dass die Schleife während der Animation zu **CoreProcessEventsOption::ProcessAllIfPresent**, damit nicht auf neue Eingabeereignisse gewartet wird. Wenn keine Ereignisse ausstehen, erfolgt die Rückgabe für [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) sofort, und die App kann den nächsten Frame der Animation darstellen. Nachdem die Animation abgeschlossen ist, wechselt die Schleife zurück zu **ProcessOneAndAllPending**, um die Bildschirmaktualisierungen zu begrenzen.

``` syntax
void App::Run()
{

    while (!m_windowClosed)
    {
        // 2. Switch to a continuous rendering loop during the animation.
        if (m_state->Animating())
        {
            // Process any system events or input from the user that is currently queued.
            // ProcessAllIfPresent will not block the thread to wait for events. This is the desired behavior when
            // you are trying to present a smooth animation to the user.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_state->Update();
            m_main->Render();
            m_deviceResources->Present();
        }
        else
        {
            // Wait for system events or input from the user.
            // ProcessOneAndAllPending will block the thread until events appear and are processed.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);

            // If any of the events processed resulted in a need to redraw the window contents, then we will re-render the
            // scene and present it to the display.
            if (m_updateWindow || m_state->StateChanged())
            {
                m_main->Render();
                m_deviceResources->Present();

                m_updateWindow = false;
                m_state->Validate();
            }
        }
    }
}
```

Zur Unterstützung des Übergangs zwischen **ProcessOneAndAllPending** und **ProcessAllIfPresent** muss der Status von der App nachverfolgt werden, um ermitteln zu können, ob die Animation aktiv ist. In der Puzzle-App fügen Sie dazu eine neue Methode hinzu, die während der Spielschleife für die GameState-Klasse aufgerufen werden kann. Der Animationszweig der Spielschleife bewirkt die Aktualisierungen des Animationsstatus, indem die neue Update-Methode für GameState aufgerufen wird.

## Szenario3: Rendern von 60Frames pro Sekunde


Beim dritten Durchlauf zeigt die App einen Zeitgeber an, um Benutzern mitzuteilen, wie lange sie bereits an der Lösung des Puzzles gearbeitet haben. Da die verstrichene Dauer bis auf Millisekunden genau angegeben wird, müssen 60Frames pro Sekunde gerendert werden, um die Anzeige aktuell zu halten.

Wie in den Szenarien1 und2 auch, verfügt die App über eine Spielschleife mit einem einzelnen Thread. Bei diesem Szenario besteht der Unterschied darin, dass aufgrund des ständigen Renderns keine Änderungen des Spielstatus mehr nachverfolgt werden müssen, wie dies bei den ersten beiden Szenarien der Fall war. Daher kann für die Verarbeitung von Ereignissen standardmäßig **ProcessAllIfPresent** verwendet werden. Wenn keine Ereignisse ausstehen, erfolgt die Rückgabe für **ProcessEvents** sofort, und es wird mit dem Rendern des nächsten Frames fortgefahren.

``` syntax
void App::Run()
{

    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            // 3. Continuously render frames and process system events and input as they appear in the queue.
            // ProcessAllIfPresent will not block the thread to wait for events. This is the desired behavior when
            // trying to present smooth animations to the user.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_state->Update();
            m_main->Render();
            m_deviceResources->Present();
        }
        else
        {
            // 3. If the window isn't visible, there is no need to continuously render.
            // Process events as they appear until the window becomes visible again.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

Dieser Ansatz ist die einfachste Möglichkeit, ein Spiel zu schreiben, da für die Ermittlung des Renderzeitpunkts kein weiterer Status nachverfolgt werden muss. Dabei werden die kürzestmöglichen Renderzeiträume und eine angemessene Reaktionsfähigkeit auf Eingaben pro Timerintervall erzielt.

Die Einfachheit dieses Entwicklungsansatzes hat aber auch einen Nachteil. Beim Rendern mit 60 Frames pro Sekunde wird mehr Energie als beim Rendern bei Bedarf verbraucht. Am besten eignet sich **ProcessAllIfPresent**, wenn die Anzeige vom Spiel für jeden Frame geändert wird. Außerdem wird damit die Eingabelatenz um bis zu 16,7 ms erhöht, da die App die Spielschleife nun während des Synchronisierungsintervalls des Displays und nicht bei **ProcessEvents** blockiert. Unter Umständen werden einige Eingabeereignisse verworfen, da die Warteschlange nur einmal pro Frame (60 Hz) verarbeitet wird.

## Szenario4: Rendern von 60Frames pro Sekunde und Erzielen der geringstmöglichen Eingabelatenz


Bei einigen Spielen kann es möglich sein, den Anstieg der Eingabelatenz aus Szenario3 zu ignorieren oder auszugleichen. Wenn eine geringe Eingabelatenz für das Spielerlebnis und die Spielerrückmeldungen aber von entscheidender Bedeutung ist, müssen Spiele, die 60Frames pro Sekunde rendern, die Eingabe in einem separaten Thread verarbeiten.

Der vierte Durchlauf des Puzzlespiels baut auf Szenario3 auf, indem die Eingabeverarbeitung und das Rendern der Grafiken aus der Spielschleife in separate Threads unterteilt wird. Mit der Nutzung separater Threads wird sichergestellt, dass die Eingabe durch die Grafikausgabe nicht verzögert werden kann. Der Code wird dadurch aber komplexer. In Szenario 4 ruft der Eingabethread [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) mit [**CoreProcessEventsOption::ProcessUntilQuit**](https://msdn.microsoft.com/library/windows/apps/br208217) auf. Damit wird auf neue Ereignisse gewartet, und alle verfügbaren Ereignisse werden verteilt. Dieses Verhalten wird beibehalten, bis das Fenster geschlossen wird oder das Spiel [**CoreWindow::Close**](https://msdn.microsoft.com/library/windows/apps/br208260) aufruft.

``` syntax
void App::Run()
{
    // 4. Start a thread dedicated to rendering and dedicate the UI thread to input processing.
    m_main->StartRenderThread();

    // ProcessUntilQuit will block the thread and process events as they appear until the App terminates.
    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
}

void JigsawPuzzleMain::StartRenderThread()
{
    // If the render thread is already running, then do not start another one.
    if (IsRendering())
    {
        return;
    }

    // Create a task that will be run on a background thread.
    auto workItemHandler = ref new WorkItemHandler([this](IAsyncAction^ action)
    {
        // Notify the swap chain that this app intends to render each frame faster
        // than the display's vertical refresh rate (typically 60 Hz). Apps that cannot
        // deliver frames this quickly should set this to 2.
        m_deviceResources->SetMaximumFrameLatency(1);

        // Calculate the updated frame and render once per vertical blanking interval.
        while (action->Status == AsyncStatus::Started)
        {
            // Execute any work items that have been queued by the input thread.
            ProcessPendingWork();

            // Take a snapshot of the current game state. This allows the renderers to work with a
            // set of values that won't be changed while the input thread continues to process events.
            m_state->SnapState();

            m_sceneRenderer->Render();
            m_deviceResources->Present();
        }

        // Ensure that all pending work items have been processed before terminating the thread.
        ProcessPendingWork();
    });

    // Run the task on a dedicated high priority background thread.
    m_renderLoopWorker = ThreadPool::RunAsync(workItemHandler, WorkItemPriority::High, WorkItemOptions::TimeSliced);
}
```

In der Vorlage **DirectX 11- und XAML-App (Universelle Windows-App)** in Microsoft Visual Studio 2015 wird die Spielschleife auf ähnliche Weise in mehrere Threads unterteilt. Dabei wird das [**Windows::UI::Core::CoreIndependentInputSource**](https://msdn.microsoft.com/library/windows/apps/dn298460)-Objekt verwendet, um einen Thread für die Behandlung der Eingabe zu starten. Außerdem wird ein Renderthread erstellt, der unabhängig vom XAML-UI-Thread ist. Weitere Informationen zu diesen Vorlagen finden Sie unter [Erstellen eines UWP- und eines DirectX-Spieleprojekts aus einer Vorlage](user-interface.md).

## Weitere Möglichkeiten zur Reduzierung der Eingabelatenz


### Verwenden von Swapchains mit Wartemöglichkeit

DirectX-Spiele reagieren auf Benutzereingaben mit der Aktualisierung der Inhalte, die Benutzer auf dem Bildschirm sehen. Bei einem 60Hz-Display wird der Bildschirm alle 16,7ms (1Sekunde/60Frames) aktualisiert. In Abbildung1 sind der ungefähre Lebenszyklus und die Reaktion auf ein Eingabeereignis relativ zum Aktualisierungssignal nach 16,7ms (VBlank) für eine App dargestellt, die mit 60Frames pro Sekunde gerendert wird:

Abbildung1

![Abbildung 1: Eingabelatenz in DirectX ](images/input-latency1.png)

Unter Windows 8.1 wurde mit DXGI das **DXGI\_SWAP\_CHAIN\_FLAG\_FRAME\_LATENCY\_WAITABLE\_OBJECT**-Flag für die Swapchain eingeführt. Damit können Apps diese Latenz leicht reduzieren, ohne dass eine Heuristik implementiert werden muss, mit der die Present-Warteschlange leer gehalten wird. Mit diesem Flag erstellte Swapchains werden als Swapchains mit Wartemöglichkeit bezeichnet. Abbildung2 zeigt den ungefähren Lebenszyklus und die Reaktion auf ein Eingabeereignis bei Verwendung von Swapchains mit Wartemöglichkeit:

Abbildung2

![Abbildung 2: Eingabelatenz in DirectX mit Wartemöglichkeit](images/input-latency2.png)

Diese Abbildungen zeigen, dass die Eingabelatenz bei Spielen um zwei volle Frames reduziert werden kann, wenn das Rendern und Darstellen der einzelnen Frames innerhalb des Zeitraums von 16,7 ms durchgeführt werden kann, der durch die Aktualisierungsrate des Displays vorgegeben ist. Im Beispiel mit dem Puzzle werden Swapchains mit Wartemöglichkeit verwendet, und das Limit der Present-Warteschlange wird per Aufruf des folgenden Elements gesteuert:` m_deviceResources->SetMaximumFrameLatency(1);`

 

 







<!--HONumber=Aug16_HO3-->


