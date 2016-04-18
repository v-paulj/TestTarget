---
title: Definieren des UWP-App-Frameworks (Universelle Windows-Plattform) für das Spiel
description: Wenn Sie Code für ein UWP-Spiel mit DirectX erstellen, müssen Sie zunächst das Framework erstellen, das die Interaktion der Spielobjekte mit Windows ermöglicht.
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
---

#  Definieren des UWP-App-Frameworks (Universelle Windows-Plattform) für das Spiel


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Wenn Sie Code für ein UWP-Spiel mit DirectX erstellen, müssen Sie zunächst das Framework erstellen, das die Interaktion der Spielobjekte mit Windows ermöglicht. Hierzu gehören Windows-Runtime-Eigenschaften wie die Behandlung von Anhalte-/Fortsetzungsereignissen, Fensterfokus und Andocken sowie die Ereignisse, Interaktionen und Übergänge für die Benutzeroberfläche. Wir erläutern, wie das Beispielspiel strukturiert ist und wie der übergeordnete Zustandsautomat für die Interaktion zwischen Spieler und System definiert wird.

## Ziel


-   Hier lernen Sie, wie Sie das Framework für ein UWP-DirectX-Spiel einrichten und den Zustandsautomaten implementieren, der den allgemeinen Spielablauf definiert.

## Initialisieren und Starten des Ansichtsanbieters


In jedem UWP-DirectX-Spiel müssen Sie einen Ansichtsanbieter abrufen, mit dem das App-Singleton (also das Windows-Runtime-Objekt, das eine Instanz Ihrer ausgeführten App definiert) auf die benötigten Grafikressourcen zugreifen kann. Über die Windows-Runtime ist Ihre App zwar direkt mit der Grafikschnittstelle verbunden, Sie müssen aber angeben, welche Ressourcen Sie benötigen und wie diese behandelt werden sollen.

Wie bereits unter [Einrichten des Spieleprojekts](tutorial--setting-up-the-games-infrastructure.md) beschrieben, stellt Microsoft Visual Studio 2015 in der Datei **Sample3DSceneRenderer.cpp** die Implementierung eines einfachen Renderers für DirectX bereit. Die Datei steht bei Auswahl der Vorlage **DirectX 11-App (Universelle Windows-App)** zur Verfügung.

Weitere Informationen zum Verständnis sowie zur Erstellung eines Ansichtsanbieters und eines Renderers finden Sie unter [So wird's gemacht: Einrichten Ihrer UWP-App mit C++ und DirectX für das Anzeigen einer DirectX-Ansicht](https://msdn.microsoft.com/library/windows/apps/hh465077).

Die Implementierung muss für fünf Methoden bereitgestellt werden, die vom App-Singleton aufgerufen werden:

-   [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495)
-   [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509)
-   [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501)
-   [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505)
-   [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523)

In der Vorlage „DirectX 11-App (Universelle Windows-App)“ werden diese fünf Methoden im **App**-Objekt in [App.h](#code_sample) definiert. Im nächsten Abschnitt erfahren Sie, wie sie in das Spiel implementiert werden.

Die Initialize-Methode des Ansichtsanbieters

```cpp
void App::Initialize(
    _In_ CoreApplicationView^ applicationView
    )
{
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);

    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);

    m_controller = ref new MoveLookController();
    m_renderer = ref new GameRenderer();
    m_game = ref new Simple3DGame();
}
```

Als Erstes wird **Initialize** vom App-Singleton aufgerufen. Daher müssen die elementaren Verhaltensweisen eines UWP-Spiels unbedingt von dieser Methode behandelt werden. Hierzu zählt beispielsweise die Behandlung der Aktivierung des Hauptfensters. Außerdem muss von der Methode sichergestellt werden, dass das Spiel eine überraschende Unterbrechung (sowie eine mögliche spätere Fortsetzung) behandeln kann.

Bei der Initialisierung der Spiele-App ordnet sie spezifischen Speicher für den Controller zu, damit der Spieler Eingaben vornehmen kann. Sie erstellt auch neue, nicht initialisierte Instanzen des Renderers und des Zustandsautomaten für das Spiel. Weitere Details finden Sie unter [Definieren des Hauptobjekts für das Spiel](tutorial--defining-the-main-game-loop.md).

Die Spiele-App kann nun eine Anhalte- oder Fortsetzungsmeldung behandeln und verfügt über zugeordneten Speicher für den Controller, den Renderer und das Spiel selbst. Aber es gibt noch kein Fenster, und das Spiel ist noch nicht initialisiert. Wir haben also noch ein wenig Arbeit vor uns.

Die SetWindow-Methode des Ansichtsanbieters

```cpp
void App::SetWindow(
    _In_ CoreWindow^ window
    )
{
    window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->Closed +=
        ref new TypedEventHandler<CoreWindow^, CoreWindowEventArgs^>(this, &App::OnWindowClosed);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);

    DisplayProperties::LogicalDpiChanged +=
        ref new DisplayPropertiesEventHandler(this, &App::OnLogicalDpiChanged);

    m_controller->Initialize(window);

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, window->Bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(window->Bounds.Width - GameConstants::TouchRectangleSize, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(window->Bounds.Width, window->Bounds.Height)
        );

    m_renderer->Initialize(window, DisplayProperties::LogicalDpi);
    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    ShowGameInfoOverlay();
}
```

Mit dem Aufruf einer Implementierung von [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509) stellt das App-Singleton ein [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Objekt bereit, das das Hauptfenster des Spiels darstellt, und macht seine Ressourcen und Ereignisse für das Spiel verfügbar. Da nun ein Fenster zur Verfügung steht, kann mit dem Hinzufügen der grundlegenden Benutzeroberflächenkomponenten und -ereignisse begonnen werden. Hierbei handelt es sich um einen Zeiger (wird sowohl von der Maus- als auch von der Fingereingabesteuerung genutzt) sowie um die grundlegenden Ereignisse für die Anpassung der Fenstergröße, für das Schließen des Fensters sowie für DPI-Änderungen (für den Fall, dass sich das Anzeigegerät ändert).

Die Spiele-App initialisiert auch den Controller, da nun die Interaktion mit einem Fenster möglich ist, sowie das Spielobjekt selbst. Sie kann Eingaben des Controllers (Fingereingabe, Maus oder Xbox 360-Controller) lesen.

Nach dem Initialisieren des Controllers definiert die App zwei rechteckige Bereiche in den Ecken unten links und unten rechts auf dem Bildschirm für die Bewegungs- bzw. Kamerasteuerungen. Der Spieler verwendet das durch den Aufruf von **SetMoveRect** definierte Rechteck links unten als virtuelles Bedienfeld, um die Kamera vor und zurück sowie seitlich zu bewegen. Das durch die **SetFireRect**-Methode definierte Rechteck rechts unten wird als virtuelle Taste zum Abfeuern der Munition verwendet.

So langsam kommen wir der Sache näher.

Die Load-Methode des Ansichtsanbieters

```cpp
void App::Load(
    Platform::String^ entryPoint
    )
{
    task<void>([this]()
    {
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // in which the m_renderer object was created because the D3D device context
        // can be accessed only on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState();

        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // In the middle of a game, so spin up the async task to load the level.
            create_task([this]()
            {
                return m_game->LoadLevelAsync();

            }).then([this]()
            {
                // The m_game object may need to deal with D3D device context work, so
                // the finalizer code needs to run in the same thread
                // context as the m_renderer object was created because the D3D 
                // device context can  be accessed only on a single thread.
                m_game->FinalizeLoadLevel();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
    }, task_continuation_context::use_current());
}
```

Nachdem das Hauptfenster festgelegt ist, ruft das App-Singleton die **Load**-Methode auf. Im Beispiel verwendet diese Methode eine Reihe asynchroner Aufgaben (deren Syntax in der [Parallel Patterns Library](https://msdn.microsoft.com/library/windows/apps/dd492418.aspx) definiert ist), um die Spielobjekte zu erstellen, Grafikressourcen zu laden und den Zustandsautomaten des Spiels zu initialisieren. Mithilfe des asynchronen Aufgabenmusters wird die Load-Methode rasch beendet und ermöglicht der App, mit der Verarbeitung von Eingaben zu beginnen. In dieser Methode zeigt die App eine Fortschrittsleiste an, während die Ressourcendateien geladen werden.

Der Ressourcenladevorgang wird in zwei getrennte Phasen unterteilt, da der Zugriff auf den Direct3D 11-Gerätekontext auf den Thread beschränkt ist, für den der Gerätekontext erstellt wurde, während der Zugriff auf das Direct3D 11-Gerät für die Objekterstellung keiner Threadbeschränkung unterliegt. Die Aufgabe **CreateGameDeviceResourcesAsync** wird in einem anderen Thread ausgeführt als die im Originalthread ausgeführte Aufgabe zur Vervollständigung (*FinalizeCreateGameDeviceResources*). Wir verwenden ein ähnliches Muster zum Laden von Levelressourcen mit **LoadLevelAsync** und **FinalizeLoadLevel**.

Nach dem Laden der Objekte des Spiels und der Grafikressourcen initialisieren wir den Zustandsautomaten des Spiels mit den Startbedingungen (Festlegen der Munitionsmenge, der Levelnummer und der Objektpositionen am Spielanfang und Ähnliches). Wenn der Spielzustand angibt, dass der Spieler ein Spiel fortsetzt, wird das aktuelle Level (in dem sich der Spieler bei der Unterbrechung des Spiels befand) geladen.

In der **Load**-Methode treffen wir alle nötigen Vorbereitungen für den Spielstart (wie Festlegen aller Startzustände oder globalen Werte). Diese Methode eignet sich auch besser zum Vorabrufen von Spieldaten oder Ressourcen als **SetWindow** oder **Initialize**. Verwenden Sie für alle Ladevorgänge asynchrone Aufgaben, da Windows ein Zeitlimit vorgibt, innerhalb dessen das Spiel mit der Verarbeitung von Eingaben beginnen muss. Falls das Laden einige Zeit dauert (etwa aufgrund einer Vielzahl von Ressourcen), sollten Sie für Benutzer eine regelmäßig aktualisierte Statusleiste bereitstellen.

Erstellen Sie Ihren Startcode bei der Entwicklung Ihres eigenen Spiels mithilfe dieser Methoden. Es folgt eine Liste mit einfachen Vorschlägen für die einzelnen Methoden:

-   Verwenden Sie **Initialize**, um Ihre Hauptklassen zuzuordnen und die grundlegenden Ereignishandler zu verbinden.
-   Verwenden Sie **SetWindow**, um das Hauptfenster zu erstellen und fensterspezifische Ereignisse zu verbinden.
-   Verwenden Sie **Load** für verbleibende Einrichtungsaufgaben und um das asynchrone Erstellen von Objekten und Laden von Ressourcen zu initiieren. Wenn temporäre Dateien oder Daten erstellt werden müssen, wie beispielsweise Assets, die im Rahmen von Prozeduren generiert werden, ist dies hier ebenfalls die richtige Stelle.

Fassen wir zusammen: Das Beispielspiel erstellt eine Instanz des Zustandsautomaten für das Spiel und legt ihn auf die Startkonfiguration fest. Es behandelt alle System- und Eingabeereignisse. Es stellt ein Fenster zum Anzeigen des Inhalts bereit. Jetzt ist es Zeit für den Gameplaycode.

Die Run-Methode des Ansichtsanbieters

```cpp
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::Snapped:
                if (!m_renderNeeded)
                {
                    // The app is not currently the active window, so just wait for events.
                    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // Otherwise, fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close.  Make sure to save the state.
}
```

Kommen wir nun zum spielerischen Teil der Spiele-App. Nachdem die Vorbereitungen mithilfe der drei Methoden abgeschlossen wurden, führt die Spiele-App die **Run**-Methode aus, und der Spaß kann beginnen.

In unserem Spielbeispiel starten wir eine While-Schleife, die beendet wird, wenn der Spieler das Spielfenster schließt. Der Beispielcode geht im Zustandsautomaten der Spielengine in einen von zwei Zuständen über:

-   Das Spielfenster wird deaktiviert (verliert also den Fokus) oder angedockt. In diesem Fall hält das Spiel die Ereignisverarbeitung an und wartet auf den Fensterfokus bzw. darauf, dass das Fenster wieder abgedockt wird.
-   Andernfalls aktualisiert das Spiel den eigenen Zustand und rendert die Grafik für die Anzeige.

Wenn Ihr Spiel den Fokus hat, müssen Sie jedes in der Meldungswarteschlange eingehende Ereignis behandeln. Daher müssen Sie [**CoreWindowDispatch.ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) mit der Option **ProcessAllIfPresent** aufrufen. Andere Optionen können zu einer verzögerten Verarbeitung von Meldungsereignissen führen. So entsteht der Eindruck, dass das Spiel nicht reagiert oder Toucheingaben nur träge umgesetzt werden.

Wenn die App nicht sichtbar ist, angehalten oder angedockt wurde, möchten wir natürlich nicht, dass sie Meldungen ausgibt, die niemals ankommen, und dabei auch noch Ressourcen beansprucht. Daher muss das Spiel **ProcessOneAndAllPending** verwenden, was eine Blockierung bis zum Eingang eines Ereignisses zur Folge hat. Dieses Ereignis wird dann zusammen mit anderen Ereignissen verarbeitet, die während der Verarbeitung des ersten Ereignisses in der Prozesswarteschlange eingehen. [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) springt nach der Verarbeitung der Warteschlange sofort wieder zurück.

Das Spiel läuft! Die Ereignisse, die es für den Wechsel des Spielzustands nutzt, werden verteilt und verarbeitet. Die Grafik wird im Rahmen der Spielschleifendurchläufe aktualisiert. Wir hoffen, der Spieler hat Spaß. Aber jeder Spaß ist einmal zu Ende...

...und wir müssen alles wieder aufräumen. An dieser Stelle kommt die **Uninitialize**-Eigenschaft ins Spiel.

Die Uninitialize-Methode des Ansichtsanbieters

```cpp
void App::Uninitialize()
{
}
```

In unserem Spielbeispiel lassen wir das App-Singleton für das Spiel nach Spielende alles wieder aufräumen. Unter Windows 10 wird beim Schließen des App-Fensters nicht die Beendigung des App-Prozesses erzwungen. Stattdessen wird der Zustand des App-Singletons in den Speicher geschrieben. Sollte eine besondere Aktion wie eine spezielle Bereinigung von Ressourcen erforderlich sein, wenn das System diesen Speicher freigeben muss, platzieren Sie den Code für diese Bereinigung in dieser Methode.

Wir kommen an anderer Stelle in diesem Tutorial nochmal auf diese fünf Methoden zurück, behalten Sie sie also im Hinterkopf. Nun widmen wir uns aber erst einmal der Gesamtstruktur der Spielengine und den Zustandsautomaten, die sie definieren.

## Initialisieren des Zustands der Spielengine


Da ein Benutzer eine angehaltene UWP-Spiele-App jederzeit fortsetzen kann, kann die App eine ganze Reihe von möglichen Zuständen haben.

Beim Start kann sich das Spielbeispiel in einem von drei Zuständen befinden:

-   Die Spielschleife wurde ausgeführt und war gerade mitten in einem Level.
-   Die Spielschleife wurde nicht ausgeführt, da gerade ein Spiel abgeschlossen wurde. (Der Highscore wird festgelegt.)
-   Es wurde kein Spiel gestartet, oder das Spiel befand sich zwischen zwei Leveln. (Der Highscore ist 0.)

Ihr eigenes Spiel kann selbstverständlich mehr oder auch weniger Zustände besitzen. Berücksichtigen Sie dabei aber wie gesagt, dass Ihr UWP-Spiel jederzeit beendet werden kann. Beim Fortsetzen des Spiels erwartet der Spieler, dass sich das Spiel so verhält, als sei es nie unterbrochen worden.

In unserem Spielbeispiel sieht der Codefluss so aus:

```cpp
void App::InitializeGameState()
{
    //
    // Set up the initial state machine for handling game playing state.
    //
    if (m_game->GameActive() && m_game->LevelActive())
    {
        m_updateState = UpdateEngineState::WaitingForResources;
        // ...

    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        m_updateState = UpdateEngineState::WaitingForPress;
        // ...
    }
    else
    {
        m_updateState = UpdateEngineState::WaitingForResources;
        // ...
    }
    SetAction(GameInfoOverlayCommand::PleaseWait);
    ShowGameInfoOverlay();
}
```

Bei der Initialisierung geht es weniger um einen Kaltstart der App als vielmehr darum, die App nach ihrer Beendigung wieder zu starten. Das Beispielspiel speichert stets den Zustand, sodass der Eindruck entsteht, als würde die App ohne Unterbrechung ausgeführt. Der Anhaltezustand ist genau das, was der Name vermuten lässt: Das Gameplay wird angehalten, die Ressourcen des Spiels befinden sich aber nach wie vor im Speicher. Ebenso verhält es sich mit dem Fortsetzungsereignis: Dieses gibt an, dass das Beispielspiel an der Stelle fortgesetzt wird, an der es zuletzt angehalten oder beendet wurde. Wird das Beispielspiel neu gestartet, nachdem es beendet wurde, startet das Spiel ganz normal und ermittelt dann den letzten bekannten Zustand, sodass der Spieler sofort weiterspielen kann.

Das Flussdiagramm gibt Aufschluss über die Anfangszustände und Übergänge für den Initialisierungsprozess des Spielbeispiels.

![Der Prozess zum Initialisieren und Vorbereiten des Spiels vor dem Start der Hauptschleife](images/simple3dgame-appstartup.png)

Je nach Zustand hat der Spieler unterschiedliche Möglichkeiten. Wird das Spiel mitten in einem Level fortgesetzt, entsteht der Eindruck, es sei  pausiert, und auf dem Overlay steht eine Fortsetzungsoption zur Verfügung. Befindet sich das Spiel bei der Fortsetzung in einem Zustand, in dem das Spiel abgeschlossen ist, werden die Highscores und eine Option zum Starten eines neuen Spiels angezeigt. Wird das Spiel dagegen vor dem Start eines Levels fortgesetzt, sieht der Spieler eine Startoption auf dem Overlay.

Das Spielbeispiel unterscheidet nicht zwischen einem Kaltstart des Spiels (also einem Spiel, das zum ersten Mal und ohne Anhalteereignis gestartet wird) und der Fortsetzung eines angehaltenen Spiels. Dieses Design wird bei jeder UWP-App erwartet.

## Behandeln von Ereignissen


Unser Beispielcode hat eine Reihe von Handlern für bestimmte Ereignisse in **Initialize**, **SetWindow** und **Load** registriert. Wahrscheinlich haben Sie sich bereits gedacht, dass diese Ereignisse von Bedeutung sein müssen, da sie registriert wurden, bevor der Code auch nur in die Nähe von Spielmechanik oder Grafikentwicklung kam. Gut erkannt! Diese Ereignisse sind für die einwandfreie Verwendung einer UWP-App von entscheidender Bedeutung. Da eine UWP-App nämlich jederzeit aktiviert, deaktiviert, vergrößert, verkleinert, angedockt, abgedockt oder fortgesetzt werden kann, müssen genau diese Ereignisse so früh wie möglich registriert und so behandelt werden, dass der Spieler keine Unterbrechungen oder Überraschungen erlebt.

Hier sehen Sie die Ereignishandler im Beispiel sowie die jeweils behandelten Ereignisse. Den vollständigen Code für diese Ereignishandler finden Sie unter [Vollständiger Code für diesen Abschnitt](#code_sample).

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Ereignishandler</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">OnActivated</td>
<td align="left">Behandelt [<strong>CoreApplicationView::Activated</strong>](https://msdn.microsoft.com/library/windows/apps/br225018). Die Spiele-App befindet sich im Vordergrund, weshalb das Hauptfenster aktiviert ist.</td>
</tr>
<tr class="even">
<td align="left">OnLogicalDpiChanged</td>
<td align="left">Behandelt [<strong>DisplayProperties::LogicalDpiChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br226150). Der DPI-Wert für das Hauptfenster des Spiels hat sich geändert, und die Spiele-App passt ihre Ressourcen entsprechend an.
<div class="alert">
<strong>Hinweis:</strong> [<strong>CoreWindow</strong>](https://msdn.microsoft.com/library/windows/desktop/hh404559)-Koordinaten werden genau wie in [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370987) in DIPs (Device Independent Pixels; geräteunabhängige Pixel) angegeben. Daher müssen Sie Direct2D über die DPI-Änderung informieren, damit die 2D-Ressourcen oder -Grundtypen korrekt angezeigt werden.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left">Behandelt [<strong>CoreApplication::Resuming</strong>](https://msdn.microsoft.com/library/windows/apps/br205859). Die Spiele-App stellt das Spiel aus einem Anhaltezustand wieder her.</td>
</tr>
<tr class="even">
<td align="left">OnSuspending</td>
<td align="left">Behandelt [<strong>CoreApplication::Suspending</strong>](https://msdn.microsoft.com/library/windows/apps/br205860). Die Spiele-App speichert den eigenen Zustand auf einem Datenträger. Der Speichervorgang für den Zustand darf maximal fünf Sekunden dauern.</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left">Behandelt [<strong>CoreWindow::VisibilityChanged</strong>](https://msdn.microsoft.com/library/windows/apps/hh701591). Die Sichtbarkeit der Spiele-App hat sich geändert: Die App wurde entweder sichtbar, oder sie wurde durch eine andere sichtbar gewordene App unsichtbar.</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left">Behandelt [<strong>CoreWindow::Activated</strong>](https://msdn.microsoft.com/library/windows/apps/br208255). Das Hauptfenster der Spiele-App wurde deaktiviert oder aktiviert, weshalb der Fokus entfernt und das Spiel angehalten oder der Fokus wiedererlangt werden muss. In beiden Fällen gibt das Overlay an, dass das Spiel angehalten wurde.</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left">Behandelt [<strong>CoreWindow::Closed</strong>](https://msdn.microsoft.com/library/windows/apps/br208261). Die Spiele-App schließt das Hauptfenster und hält das Spiel an.</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left">Behandelt [<strong>CoreWindow::SizeChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br208283). Die Spiele-App ordnet die Grafikressourcen und das Overlay neu zu, um die Größenänderung umzusetzen, und aktualisiert anschließend das Renderziel.</td>
</tr>
</tbody>
</table>

 

Ihr eigenes Spiel muss diese Ereignisse behandeln, da sie Teil des Designs von UWP-Apps sind.

## Aktualisieren der Spielengine


In **Run** wird innerhalb der Spielschleife des Beispiels ein einfacher Zustandsautomat zur Behandlung aller wichtigen Aktionen implementiert, die der Spieler ausführen kann. Die höchste Ebene dieses Zustandsautomats behandelt das Laden eines Spiels, das Spielen eines bestimmten Levels oder das Fortsetzen eines Levels, nachdem das Spiel (vom System oder vom Spieler) angehalten wurde.

Im Spielbeispiel kann sich das Spiel in drei Hauptzuständen (UpdateEngineState) befinden:

-   **Waiting for resources**. Die Spielschleife wird durchlaufen, und ein Übergang ist erst möglich, wenn Ressourcen (insbesondere Grafikressourcen) verfügbar sind. Nach Abschluss der asynchronen Aufgaben zum Laden der Ressourcen wird der Zustand zu **ResourcesLoaded** aktualisiert. Dieser Fall tritt üblicherweise zwischen Leveln ein, wenn für ein Level neue Ressourcen von einem Datenträger geladen werden müssen. Im Spielbeispiel simulieren wir dieses Verhalten, da an dieser Stelle keine zusätzlichen levelspezifischen Ressourcen für das Spiel geladen werden müssen.
-   **Waiting for press**. Die Spielschleife wird durchlaufen, bis eine bestimmte Benutzereingabe erfolgt. Bei der Eingabe handelt es sich um eine Spieleraktion zum Laden eines Spiels, zum Starten eines Levels oder zum Fortsetzen eines Levels. Diese untergeordneten Zustände sind im Beispielcode als PressResultState-Aufzählungswerte enthalten.
-   **Dynamics**. Die Spielschleife wird ausgeführt, und der Spieler spielt. Während dieser Zeit prüft das Spiel drei Bedingungen, bei deren Erfüllung ein Übergang erfolgen kann: Ablauf des Zeitlimits für ein Level, Abschluss eines Levels durch den Spieler oder Abschluss aller Level durch den Spieler.

Hier sehen Sie die Codestruktur. Den vollständigen Code finden Sie unter [Vollständiger Code für diesen Abschnitt](#code_sample).

Die Struktur des Zustandsautomaten zur Aktualisierung der Spielengine

```cpp
void App::Update()
{
    m_controller->Update();

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        // Waiting for initial load.  Display an update once per 60 updates.
        loadCount++;
        if ((loadCount % 60) == 0)
        {
            m_loadingCount++;
            SetGameInfoOverlay(m_gameInfoOverlayState);
        }
        break;

    case UpdateEngineState::ResourcesLoaded:
        switch (m_pressResult)
        {
        case PressResultState::LoadGame:
            // ...
            break;

        case PressResultState::PlayLevel:
            // ...
            break;

        case PressResultState::ContinueLevel:
            // ...
            break;
        }
        // ...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete() || m_pressComplete)
        {
            m_pressComplete = false;

            switch (m_pressResult)
            {
            case PressResultState::LoadGame:
                // ...
                break;

            case PressResultState::PlayLevel:
                // ...
                break;

            case PressResultState::ContinueLevel:
                // ...
                break;
            }
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested() || m_pauseRequested)
        {
            // ...
        }
        else 
        {
            GameState runState = m_game->RunGame();
            switch (runState)
            {
            case GameState::TimeExpired:
                // ...
                break;

            case GameState::LevelComplete:
                // ...
                break;

            case GameState::GameComplete:
                // ...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // transitioning state, so enable waiting for the press event
            m_controller->WaitForPress(m_game->GameInfoOverlayUpperLeft(), m_game->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller until resources are loaded
            m_controller->Active(false);
        }

        break;
    }
}
```

Der Hauptzustandsautomat des Spiels sieht wie folgt aus:

![Hauptzustandsautomat für unser Spiel](images/simple3dgame-mainstatemachine.png)

Mit der Spiellogik selbst beschäftigen wir uns ausführlicher in [Definieren des Hauptobjekts für das Spiel](tutorial--defining-the-main-game-loop.md). Entscheidend ist für den Moment, dass Ihr Spiel ein Zustandsautomat ist. Jeder spezifische Zustand muss anhand sehr spezifischer Kriterien definiert sein, und die Übergänge zwischen den Zuständen müssen auf separaten Benutzereingaben oder Systemaktionen (wie dem Laden von Grafikressourcen) basieren. Erstellen Sie in der Planungsphase des Spiels ein Diagramm nach dem Vorbild des hier verwendeten Diagramms. Vergewissern Sie sich dabei, dass alle möglichen Aktionen des Benutzers und des Systems auf hoher Ebene abgedeckt sind. Spiele können sehr kompliziert sein, und der Zustandsautomat ist ein wirksames Tool, um diese Komplexität darzustellen und den Umgang damit deutlich zu vereinfachen.

Wie Sie sehen, gibt es innerhalb von Zustandsautomaten weitere Zustandsautomaten. Es gibt einen für den Controller, der alle zulässigen Eingaben des Spielers behandelt. Im Diagramm ist ein Tastendruck (press) eine Art von Benutzereingabe. Für diesen Zustandsautomaten ist es unerheblich, worum es sich dabei handelt, da er auf einer höheren Ebene arbeitet. Daher wird vorausgesetzt, dass der Zustandsautomat für den Controller sämtliche Übergänge in Verbindung mit Bewegungs- und Schießverhalten sowie die zugehörigen Rendering-Updates behandelt. Auf die Verwaltung von Eingabezuständen gehen wir in [Hinzufügen von Steuerelementen](tutorial--adding-controls.md) näher ein.

## Aktualisieren der Benutzeroberfläche


Wir müssen dafür sorgen, dass der Spieler stets über den Zustand des Systems im Bilde ist, und es ihm ermöglichen, den übergeordneten Zustand in einer Weise zu ändern, die mit den Regeln des Spiels in Einklang steht. Bei den meisten Spielen (und auch bei diesem Spielbeispiel) kommt hierzu ein Head-up-Display mit einer Darstellung des Spielzustands und anderen spielspezifischen Infos wie Punktestand, Munitionsmenge oder Anzahl der verbleibenden Versuche zum Einsatz. Wir bezeichnen dies als Overlay, da es getrennt von der Haupt-Grafikpipeline gerendert und im Vordergrund der 3D-Projektion angezeigt wird. Im Beispielspiel erstellen wir dieses Overlay mithilfe der Direct2D-APIs. Wir können das Overlay allerdings auch per XAML erstellen. Informationen hierzu finden Sie unter [Erweitern des Spielbeispiels](tutorial-resources.md).

Die Benutzeroberfläche umfasst zwei Komponenten:

-   Das Head-up-Display mit dem Punktestand und Infos zum aktuellen Zustand des Gameplays.
-   Die Bitmap für den Pausenmodus – ein schwarzes Rechteck mit Text, das im angehaltenen/unterbrochenen Zustand des Spiels im Vordergrund angezeigt wird. Dies ist die Spielüberlagerung. Ausführlichere Informationen hierzu finden Sie unter [Hinzufügen einer Benutzeroberfläche](tutorial--adding-a-user-interface.md).

Wie nicht anders zu erwarten, besitzt auch das Overlay einen Zustandsautomaten. Das Overlay kann eine Meldung für den Levelstart oder eine Meldung mit dem Hinweis anzeigen, dass das Spiel vorbei ist. Es ist im Grunde ein Canvas-Element, mit dem Sie beliebige Infos zum Spielzustand  dem Spieler anzeigen können, wenn das Spiel unterbrochen oder angehalten wurde.

Hier sehen Sie die Struktur des Overlay-Zustandsautomaten für das Spielbeispiel:

```cpp
void App::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {

    case GameInfoOverlayState::Loading:
        m_renderer->InfoOverlay()->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        // ...
        break;

    case GameInfoOverlayState::LevelStart:
        // ...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        // ...
        break;

    case GameInfoOverlayState::GameOverExpired:
        // ...
        break;

    case GameInfoOverlayState::Pause:
        // ...
        break;
    }
}
```

Je nach Zustand des Spiels zeigt das Overlay einen von sechs Zustandsbildschirmen: einen ressourcenladenden Bildschirm bei Spielbeginn, einen Gameplay-Bildschirm, einen Bildschirm mit der Information, dass ein Level gestartet wird, einen Bildschirm für ein Spielende, bei dem alle Level vor Ablauf der Zeit absolviert wurden, einen Bildschirm für ein Spielende, bei dem die Zeit abgelaufen ist, und einen Bildschirm mit einem Pausenmenü.

Dank der getrennten Behandlung von Benutzeroberfläche und Spielgrafikpipeline können Sie Arbeiten an der Benutzeroberfläche unabhängig von der Grafikengine des Spiels vornehmen. Außerdem wird der Code Ihres Spiels dadurch deutlich übersichtlicher.

## Nächste Schritte


So viel also zur Grundstruktur des Spielbeispiels. Sie verfügen nun über ein gutes Modell für die Entwicklung von UWP-Apps mit DirectX. Das war natürlich noch längst nicht alles. Wir haben uns hier lediglich mit dem Grundgerüst des Spiels befasst. Im Anschluss setzen wir uns ausführlicher mit dem Spiel und der Spielmechanik sowie mit der Implementierung dieser Spielmechanik als Kernobjekt des Spiels auseinander. Die entsprechenden Informationen finden Sie unter [Definieren des Hauptobjekts für das Spiel](tutorial--defining-the-main-game-loop.md).

Außerdem ist es Zeit für eine ausführlichere Betrachtung der Grafikengine des Beispielspiels. Diesen Teil behandeln wir in [Zusammensetzen der Renderingpipeline](tutorial--assembling-the-rendering-pipeline.md).

## Vollständiger Beispielcode für diesen Abschnitt


App.h

```cpp
///// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "Simple3DGame.h"

enum class UpdateEngineState
{
    WaitingForResources,
    ResourcesLoaded,
    WaitingForPress,
    Dynamics,
    Snapped,
    Suspended,
    Deactivated,
};

enum class PressResultState
{
    LoadGame,
    PlayLevel,
    ContinueLevel,
};

enum class GameInfoOverlayState
{
    Loading,
    GameStats,
    GameOverExpired,
    GameOverCompleted,
    LevelStart,
    Pause,
};

ref class App : public Windows::ApplicationModel::Core::IFrameworkView
{
internal:
    App();

public:
    // IFrameworkView Methods
    virtual void Initialize(_In_ Windows::ApplicationModel::Core::CoreApplicationView^ applicationView);
    virtual void SetWindow(_In_ Windows::UI::Core::CoreWindow^ window);
    virtual void Load(_In_ Platform::String^ entryPoint);
    virtual void Run();
    virtual void Uninitialize();

private:
    void InitializeGameState();

    // Event Handlers
    void OnSuspending(
        _In_ Platform::Object^ sender,
        _In_ Windows::ApplicationModel::SuspendingEventArgs^ args
        );

    void OnResuming(
        _In_ Platform::Object^ sender,
        _In_ Platform::Object^ args
        );

    void UpdateViewState();

    void OnWindowActivationChanged(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::WindowActivatedEventArgs^ args
        );

    void OnWindowSizeChanged(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::WindowSizeChangedEventArgs^ args
        );

    void OnWindowClosed(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::CoreWindowEventArgs^ args
        );

    void OnLogicalDpiChanged(
        _In_ Platform::Object^ sender
        );

    void OnActivated(
        _In_ Windows::ApplicationModel::Core::CoreApplicationView^ applicationView,
        _In_ Windows::ApplicationModel::Activation::IActivatedEventArgs^ args
        );

    void OnVisibilityChanged(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::VisibilityChangedEventArgs^ args
        );

    void Update();
    void SetGameInfoOverlay(GameInfoOverlayState state);
    void SetAction (GameInfoOverlayCommand command);
    void ShowGameInfoOverlay();
    void HideGameInfoOverlay();
    void SetSnapped();
    void HideSnapped();

    bool                                                m_windowClosed;
    bool                                                m_renderNeeded;
    bool                                                m_haveFocus;
    bool                                                m_visible;

    MoveLookController^                                 m_controller;
    GameRenderer^                                       m_renderer;
    Simple3DGame^                                       m_game;

    UpdateEngineState                                   m_updateState;
    UpdateEngineState                                   m_updateStateNext;
    PressResultState                                    m_pressResult;
    GameInfoOverlayState                                m_gameInfoOverlayState;
    GameInfoOverlayCommand                              m_gameInfoOverlayCommand;
    uint32                                              m_loadingCount;
};

ref class Direct3DApplicationSource : Windows::ApplicationModel::Core::IFrameworkViewSource
{
public:
    virtual Windows::ApplicationModel::Core::IFrameworkView^ CreateView();
};
```

App.cpp

```cpp
//--------------------------------------------------------------------------------------
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "App.h"

using namespace concurrency;
using namespace DirectX;
using namespace Windows::ApplicationModel;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::ApplicationModel::Core;
using namespace Windows::Foundation;
using namespace Windows::Graphics::Display;
using namespace Windows::UI::Core;
using namespace Windows::UI::Input;
using namespace Windows::UI::ViewManagement;


App::App() :
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
}

//--------------------------------------------------------------------------------------

void App::Initialize(
    _In_ CoreApplicationView^ applicationView
    )
{
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);

    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);

    m_controller = ref new MoveLookController();
    m_renderer = ref new GameRenderer();
    m_game = ref new Simple3DGame();
}

//--------------------------------------------------------------------------------------

void App::SetWindow(
    _In_ CoreWindow^ window
    )
{
    window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);

    PointerVisualizationSettings^ visualizationSettings = PointerVisualizationSettings::GetForCurrentView();
    visualizationSettings->IsContactFeedbackEnabled = false;
    visualizationSettings->IsBarrelButtonFeedbackEnabled = false;

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->Closed +=
        ref new TypedEventHandler<CoreWindow^, CoreWindowEventArgs^>(this, &App::OnWindowClosed);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);

    DisplayProperties::LogicalDpiChanged +=
        ref new DisplayPropertiesEventHandler(this, &App::OnLogicalDpiChanged);

    m_controller->Initialize(window);

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, window->Bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(window->Bounds.Width - GameConstants::TouchRectangleSize, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(window->Bounds.Width, window->Bounds.Height)
        );

    m_renderer->Initialize(window, DisplayProperties::LogicalDpi);
    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::Load(
    _In_ Platform::String^ /* entryPoint */
    )
{
    create_task([this]()
    {
        // Asynchronously initialize the game class and load the renderer device resources.
        // By doing all this asynchronously, the game gets to its main loop more quickly
        // and loads all the necessary resources in parallel on other threads.
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can ONLY be accessed on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState();

        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // In the middle of a game, so spin up the async task to load the level.
            create_task([this]()
            {
                return m_game->LoadLevelAsync();

            }).then([this]()
            {
                // The m_game object may need to deal with D3D device context work, so
                // again the finalize code needs to run in the same thread
                // context as the m_renderer object was created because the D3D 
                // device context can ONLY be accessed on a single thread.
                m_game->FinalizeLoadLevel();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
    }, task_continuation_context::use_current());
}

//--------------------------------------------------------------------------------------

void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::Snapped:
                if (!m_renderNeeded)
                {
                    // The App is not currently the active window, so just wait for events.
                    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // Otherwise, fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close.  Make sure to save state.
}

//--------------------------------------------------------------------------------------

void App::Uninitialize()
{
}

//--------------------------------------------------------------------------------------

void App::OnWindowSizeChanged(
    _In_ CoreWindow^ window,
    _In_ WindowSizeChangedEventArgs^ /* args */
    )
{
    UpdateViewState();
    m_renderer->UpdateForWindowSizeChange();

    // The location of the GameInfoOverlay may have changed with the size change, so update the controller.
    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, window->Bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(window->Bounds.Width - GameConstants::TouchRectangleSize, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(window->Bounds.Width, window->Bounds.Height)
        );

    if (m_updateState == UpdateEngineState::WaitingForPress)
    {
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
    }
}

//--------------------------------------------------------------------------------------

void App::OnWindowClosed(
    _In_ CoreWindow^ /* sender */,
    _In_ CoreWindowEventArgs^ /* args */
    )
{
    m_windowClosed = true;
}

//--------------------------------------------------------------------------------------

void App::OnLogicalDpiChanged(
    _In_ Platform::Object^ /* sender */
    )
{
    m_renderer->SetDpi(DisplayProperties::LogicalDpi);

    // The GameInfoOverlay may have been recreated as a result of DPI changes, so
    // regenerate the data.
    SetGameInfoOverlay(m_gameInfoOverlayState);
    SetAction(m_gameInfoOverlayCommand);
}

//--------------------------------------------------------------------------------------

void App::OnActivated(
    _In_ CoreApplicationView^ /* applicationView */,
    _In_ IActivatedEventArgs^ /* args */
    )
{
    CoreWindow::GetForCurrentThread()->Activated +=
        ref new TypedEventHandler<CoreWindow^, WindowActivatedEventArgs^>(this, &App::OnWindowActivationChanged);
    CoreWindow::GetForCurrentThread()->Activate();
}

//--------------------------------------------------------------------------------------

void App::OnVisibilityChanged(
    _In_ CoreWindow^ /* sender */,
    _In_ VisibilityChangedEventArgs^ args
    )
{
    m_visible = args->Visible;
}

//--------------------------------------------------------------------------------------

void App::InitializeGameState()
{
    // Set up the initial state machine for handling the Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle
        // of a level.
        // We are waiting for the user to continue the game.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::ContinueLevel;
        SetGameInfoOverlay(GameInfoOverlayState::Pause);
        SetAction(GameInfoOverlayCommand::PleaseWait);
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        m_updateState = UpdateEngineState::WaitingForPress;
        m_pressResult = PressResultState::LoadGame;
        SetGameInfoOverlay(GameInfoOverlayState::GameStats);
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        SetAction(GameInfoOverlayCommand::TapToContinue);
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::PlayLevel;
        SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
        SetAction(GameInfoOverlayCommand::PleaseWait);
    }
    ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::Update()
{
    static uint32 loadCount = 0;

    m_controller->Update();

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        // Waiting for the initial load.  Display an update once per 60 updates.
        loadCount++;
        if ((loadCount % 60) == 0)
        {
            m_loadingCount++;
            SetGameInfoOverlay(m_gameInfoOverlayState);
        }
        break;

    case UpdateEngineState::ResourcesLoaded:
        switch (m_pressResult)
        {
        case PressResultState::LoadGame:
            SetGameInfoOverlay(GameInfoOverlayState::GameStats);
            break;

        case PressResultState::PlayLevel:
            SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
            break;

        case PressResultState::ContinueLevel:
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            break;
        }
        m_updateState = UpdateEngineState::WaitingForPress;
        SetAction(GameInfoOverlayCommand::TapToContinue);
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        ShowGameInfoOverlay();
        m_renderNeeded = true;
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            switch (m_pressResult)
            {
            case PressResultState::LoadGame:
                m_updateState = UpdateEngineState::WaitingForResources;
                m_pressResult = PressResultState::PlayLevel;
                m_controller->Active(false);
                m_game->LoadGame();
                SetAction(GameInfoOverlayCommand::PleaseWait);
                SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
                ShowGameInfoOverlay();

                m_game->LoadLevelAsync().then([this]()
                {
                    m_game->FinalizeLoadLevel();
                    m_updateState = UpdateEngineState::ResourcesLoaded;

                }, task_continuation_context::use_current());
                break;

            case PressResultState::PlayLevel:
                m_updateState = UpdateEngineState::Dynamics;
                HideGameInfoOverlay();
                m_controller->Active(true);
                m_game->StartLevel();
                break;

            case PressResultState::ContinueLevel:
                m_updateState = UpdateEngineState::Dynamics;
                HideGameInfoOverlay();
                m_controller->Active(true);
                m_game->ContinueGame();
                break;
            }
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            m_game->PauseGame();
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            SetAction(GameInfoOverlayCommand::TapToContinue);
            m_updateState = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            ShowGameInfoOverlay();
        }
        else
        {
            GameState runState = m_game->RunGame();
            switch (runState)
            {
            case GameState::TimeExpired:
                SetAction(GameInfoOverlayCommand::TapToContinue);
                SetGameInfoOverlay(GameInfoOverlayState::GameOverExpired);
                ShowGameInfoOverlay();
                m_updateState = UpdateEngineState::WaitingForPress;
                m_pressResult = PressResultState::LoadGame;
                break;

            case GameState::LevelComplete:
                SetAction(GameInfoOverlayCommand::PleaseWait);
                SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
                ShowGameInfoOverlay();
                m_updateState = UpdateEngineState::WaitingForResources;
                m_pressResult = PressResultState::PlayLevel;

                m_game->LoadLevelAsync().then([this]()
                {
                    m_game->FinalizeLoadLevel();
                    m_updateState = UpdateEngineState::ResourcesLoaded;

                }, task_continuation_context::use_current());
                break;

            case GameState::GameComplete:
                SetAction(GameInfoOverlayCommand::TapToContinue);
                SetGameInfoOverlay(GameInfoOverlayState::GameOverCompleted);
                ShowGameInfoOverlay();
                m_updateState  = UpdateEngineState::WaitingForPress;
                m_pressResult = PressResultState::LoadGame;
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event.
            m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller until resources are loaded.
            m_controller->Active(false);
        }
        break;
    }
}

//--------------------------------------------------------------------------------------

void App::OnWindowActivationChanged(
    _In_ Windows::UI::Core::CoreWindow^ /* sender */,
    _In_ Windows::UI::Core::WindowActivatedEventArgs^ args
    )
{
    if (args->WindowActivationState == CoreWindowActivationState::Deactivated)
    {
        m_haveFocus = false;

        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // From Dynamic mode, when coming out of Deactivated rather than going directly back into game play
            // go to the paused state waiting for user input to continue.
            m_updateStateNext = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            ShowGameInfoOverlay();
            m_game->PauseGame();
            m_updateState = UpdateEngineState::Deactivated;
            SetAction(GameInfoOverlayCommand::None);
            m_renderNeeded = true;
            break;

        case UpdateEngineState::WaitingForResources:
        case UpdateEngineState::WaitingForPress:
            m_updateStateNext = m_updateState;
            m_updateState = UpdateEngineState::Deactivated;
            SetAction(GameInfoOverlayCommand::None);
            ShowGameInfoOverlay();
            m_renderNeeded = true;
            break;
        }
        // Don't have focus, so shutdown input processing.
        m_controller->Active(false);
    }
    else if (args->WindowActivationState == CoreWindowActivationState::CodeActivated
        || args->WindowActivationState == CoreWindowActivationState::PointerActivated)
    {
        m_haveFocus = true;

        if (m_updateState == UpdateEngineState::Deactivated)
        {
            m_updateState = m_updateStateNext;

            if (m_updateState == UpdateEngineState::WaitingForPress)
            {
                SetAction(GameInfoOverlayCommand::TapToContinue);
                m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
            }
            else if (m_updateStateNext == UpdateEngineState::WaitingForResources)
            {
                SetAction(GameInfoOverlayCommand::PleaseWait);
            }
        }
    }
}

//--------------------------------------------------------------------------------------

void App::OnSuspending(
    _In_ Platform::Object^ /* sender */,
    _In_ SuspendingEventArgs^ args
    )
{
    // Save application state.
    // If your application needs time to complete a lengthy operation, it can request a deferral.
    // The SuspendingOperation has a deadline time. Make sure all your operations are complete by that time!
    // If the app doesn't return from this handler within five seconds, it will be terminated.
    SuspendingOperation^ op = args->SuspendingOperation;
    SuspendingDeferral^ deferral = op->GetDeferral();

    create_task([=]()
    {
        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // Game is in the active game play state, Stop Game Timer and Pause play and save the state.
            SetAction(GameInfoOverlayCommand::None);
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            m_updateStateNext = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            m_game->PauseGame();
            break;

        case UpdateEngineState::WaitingForResources:
        case UpdateEngineState::WaitingForPress:
            m_updateStateNext = m_updateState;
            break;

        default:
            // If it is any other state, don't save as the next state as they are transient states and have already set m_updateStateNext
            break;
        }
        m_updateState = UpdateEngineState::Suspended;

        m_controller->Active(false);
        m_game->OnSuspending();

        deferral->Complete();
    });
}

//--------------------------------------------------------------------------------------

void App::OnResuming(
    _In_ Platform::Object^ /* sender */,
    _In_ Platform::Object^ /* args */
    )
{
    if (m_haveFocus)
    {
        m_updateState = m_updateStateNext;
    }
    else
    {
        m_updateState = UpdateEngineState::Deactivated;
    }

    if (m_updateState == UpdateEngineState::WaitingForPress)
    {
        SetAction(GameInfoOverlayCommand::TapToContinue);
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
    }
    m_game->OnResuming();
    ShowGameInfoOverlay();
    m_renderNeeded = true;
}

//--------------------------------------------------------------------------------------

void App::UpdateViewState()
{
    m_renderNeeded = true;

    if (ApplicationView::Value == ApplicationViewState::Snapped)
    {
        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // From Dynamic mode, when coming out of SNAPPED layout rather than going directly back into game play,
            // go to the paused state and wait for user input to continue.
            m_updateStateNext = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            SetAction(GameInfoOverlayCommand::TapToContinue);
            m_game->PauseGame();
            break;

        case UpdateEngineState::WaitingForResources:
        case UpdateEngineState::WaitingForPress:
            // Avoid corrupting the m_updateStateNext on a transition from Snapped -> Snapped.
            // Otherwise, just cache the current state and return to it when leaving SNAPPED layout.

            m_updateStateNext = m_updateState;
            break;

        default:
            break;
        }

        m_updateState = UpdateEngineState::Snapped;
        m_controller->Active(false);
        HideGameInfoOverlay();
        SetSnapped();
    }
    else if (ApplicationView::Value == ApplicationViewState::Filled ||
        ApplicationView::Value == ApplicationViewState::FullScreenLandscape ||
        ApplicationView::Value == ApplicationViewState::FullScreenPortrait)
    {
        if (m_updateState == UpdateEngineState::Snapped)
        {

            HideSnapped();
            ShowGameInfoOverlay();
            m_renderNeeded = true;

            if (m_haveFocus)
            {
                if (m_updateStateNext == UpdateEngineState::WaitingForPress)
                {
                    SetAction(GameInfoOverlayCommand::TapToContinue);
                    m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
                }
                else if (m_updateStateNext == UpdateEngineState::WaitingForResources)
                {
                    SetAction(GameInfoOverlayCommand::PleaseWait);
                }

                m_updateState = m_updateStateNext;
            }
            else
            {
                m_updateState = UpdateEngineState::Deactivated;
                SetAction(GameInfoOverlayCommand::None);
            }
        }
    }
}

//--------------------------------------------------------------------------------------

void App::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_renderer->InfoOverlay()->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        m_renderer->InfoOverlay()->SetGameStats(
            m_game->HighScore().levelCompleted + 1,
            m_game->HighScore().totalHits,
            m_game->HighScore().totalShots
            );
        break;

    case GameInfoOverlayState::LevelStart:
        m_renderer->InfoOverlay()->SetLevelStart(
            m_game->LevelCompleted() + 1,
            m_game->CurrentLevel()->Objective(),
            m_game->CurrentLevel()->TimeLimit(),
            m_game->BonusTime()
            );
        break;

    case GameInfoOverlayState::GameOverCompleted:
        m_renderer->InfoOverlay()->SetGameOver(
            true,
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::GameOverExpired:
        m_renderer->InfoOverlay()->SetGameOver(
            false,
            m_game->LevelCompleted(),
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::Pause:
        m_renderer->InfoOverlay()->SetPause();
        break;
    }
}

//--------------------------------------------------------------------------------------

void App::SetAction (GameInfoOverlayCommand command)
{
    m_gameInfoOverlayCommand = command;
    m_renderer->InfoOverlay()->SetAction(command);
}

//--------------------------------------------------------------------------------------

void App::ShowGameInfoOverlay()
{
    m_renderer->InfoOverlay()->ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::HideGameInfoOverlay()
{
    m_renderer->InfoOverlay()->HideGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::SetSnapped()
{
    m_renderer->InfoOverlay()->SetPause();
    m_renderer->InfoOverlay()->ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::HideSnapped()
{
    m_renderer->InfoOverlay()->HideGameInfoOverlay();
    SetGameInfoOverlay(m_gameInfoOverlayState);
}

//--------------------------------------------------------------------------------------

IFrameworkView^ DirectXAppSource::CreateView()
{
    return ref new App();
}

//--------------------------------------------------------------------------------------

[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}

//--------------------------------------------------------------------------------------
```

 

 






<!--HONumber=Mar16_HO1-->


