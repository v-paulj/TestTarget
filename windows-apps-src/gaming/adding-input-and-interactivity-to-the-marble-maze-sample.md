---
author: mtoepke
title: "Hinzufügen von Eingaben und Interaktivität zum Marble Maze-Beispiel"
description: "UWP-Spiele können auf den verschiedensten Geräten ausgeführt werden, beispielsweise auf Desktopcomputern, Laptops und Tablets."
ms.assetid: b946bf62-c0ca-f9ec-1a87-8195b89a5ab4
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 563ee292ec7189b0c365ae5ee0d1c41fd6fd1a09

---

# Hinzufügen von Eingaben und Interaktivität zum Marble Maze-Beispiel


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


UWP-Spiele können auf den verschiedensten Geräten ausgeführt werden, beispielsweise auf Desktopcomputern, Laptops und Tablets. Für ein Gerät sind viele verschiedene Eingabe- und Steuerungsmechanismen möglich. Unterstützen Sie mehrere Eingabegeräte, damit die Kunden Ihr Spiel ganz nach ihren persönlichen Vorlieben und Fähigkeiten spielen können. In diesem Dokument werden die wichtigsten Methoden beschrieben, die Sie berücksichtigen sollten, wenn Sie mit Eingabegeräten arbeiten. Außerdem erfahren Sie, wie diese Methoden in Marble Maze angewendet werden.

> **Hinweis**  Den Beispielcode für dieses Dokument finden Sie im [DirectX-Beispielspiel Marble Maze](http://go.microsoft.com/fwlink/?LinkId=624011).

 
Hier sind einige der wichtigsten in diesem Dokument erörterten Punkte für das Arbeiten mit Eingaben in Ihrem Spiel:

-   Unterstützen Sie nach Möglichkeit mehrere Eingabegeräte, damit die Kunden Ihr Spiel ganz nach ihren persönlichen Vorlieben und Fähigkeiten spielen können. Die Verwendung eines Gamecontrollers und Sensors ist zwar optional, wird aber dringend empfohlen, um die Benutzerfreundlichkeit für die Spieler zu erhöhen. Die API für Gamecontroller und Sensoren soll Ihnen die Integration dieser Eingabegeräte erleichtern.
-   Zum Initialisieren der Toucheingabe müssen Sie sich für Windows-Ereignisse registrieren, beispielsweise für das Aktivieren, Freigeben und Verschieben des Zeigers. Zum Initialisieren des Beschleunigungsmessers erstellen Sie ein [**Windows::Devices::Sensors::Accelerometer**](https://msdn.microsoft.com/library/windows/apps/br225687)-Objekt, wenn Sie die Anwendung initialisieren. Der Xbox 360-Controller muss nicht initialisiert werden.
-   Überlegen Sie bei Spielen für einen Spieler, ob Sie die Eingaben aller möglichen Xbox 360-Controller kombinieren möchten. Auf diese Weise müssen Sie nicht nachverfolgen, welche Eingabe von welchem Controller kommt.
-   Verarbeiten Sie erst Windows-Ereignisse und dann Eingabegeräte.
-   Der Xbox 360-Controller und der Beschleunigungsmesser unterstützen Abrufe. Das heißt, Sie können Daten abrufen, wenn Sie sie benötigen. Für die Toucheingabe sollten Sie Touchereignisse in Datenstrukturen aufzeichnen, die für Ihren Eingabeverarbeitungscode verfügbar sind.
-   Überlegen Sie, ob Sie Eingabewerte in ein gemeinsames Format normalisieren möchten. Auf diese Weise können Sie die Interpretation der Eingabe durch andere Komponenten des Spiels wie beispielsweise die Simulation von Physikeffekten vereinfachen und leichter Spiele schreiben, die sich für unterschiedliche Bildschirmauflösungen eignen.

## Von Marble Maze unterstützte Eingabegeräte


Marble Maze unterstützt allgemeine Xbox 360-Controllergeräte, Maus und Toucheingabe zum Auswählen von Menüelementen und Xbox 360-Controller, Maus, Toucheingabe und Beschleunigungsmesser zum Steuern des Spielverlaufs. Marble Maze ruft die Eingaben vom Controller mithilfe der XInput-API ab. Bei der Toucheingabe können Anwendungen Eingaben mit der Fingerspitze nachverfolgen und darauf reagieren. Ein Beschleunigungsmesser ist ein Sensor, der die Kraft misst, die entlang der X-, Y- und Z-Achsen angewendet wird. Mit der Windows-Runtime können Sie den aktuellen Zustand des Beschleunigungsmessers abrufen und Touchereignisse über den Ereignisbehandlungsmechanismus der Windows-Runtime empfangen.

> **Hinweis**  In diesem Dokument wird der Begriff Toucheingabe für Touch- und Mauseingabe sowie Zeigereingabe verwendet. Damit sind alle Geräte gemeint, die Zeigerereignisse verwenden. Da bei der Touch- und Mauseingabe Standardzeigerereignisse verwendet werden, können Sie jedes der Geräte verwenden, um Menüelemente auszuwählen und den Spielverlauf zu steuern.

 

> **Hinweis**  Das Paketmanifest legt das Querformat als unterstützte Drehung für das Spiel fest. Damit wird verhindert, dass sich die Ausrichtung ändert, wenn Sie das Gerät drehen, um die Murmel zu bewegen.

 

## Initialisieren von Eingabegeräten


Der Xbox 360-Controller muss nicht initialisiert werden. Zum Initialisieren der Toucheingabe müssen Sie sich für Windows-Ereignisse registrieren, beispielsweise für das Aktivieren (z. B. wenn der Benutzer die Maustaste drückt oder den Bildschirm berührt), Freigeben und Verschieben des Zeigers. Zum Initialisieren des Beschleunigungsmessers müssen Sie ein [**Windows::Devices::Sensors::Accelerometer**](https://msdn.microsoft.com/library/windows/apps/br225687)-Objekt erstellen, wenn Sie die Anwendung initialisieren.

Im folgenden Beispiel wird dargestellt, wie der **DirectXPage**-Konstruktor für die Zeigerereignisse [**Windows::UI::Core::CoreIndependentInputSource::PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn298471), [**Windows::UI::Core::CoreIndependentInputSource::PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn298472) und [**Windows::UI::Core::CoreIndependentInputSource::PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn298469) des [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)-Elements registriert wird. Diese Ereignisse werden bei der Initialisierung der App und vor der Spielschleife registriert.

Die Ereignisse werden in einem separaten Thread behandelt, über den die Ereignishandler aufgerufen werden.

Weitere Informationen zum Initialisieren der Anwendung finden Sie unter [Marble Maze-Anwendungsstruktur](marble-maze-application-structure.md).

```cpp
coreInput->PointerPressed += ref new TypedEventHandler<Object^, PointerEventArgs^>(this, &DirectXPage::OnPointerPressed);
coreInput->PointerMoved += ref new TypedEventHandler<Object^, PointerEventArgs^>(this, &DirectXPage::OnPointerMoved);
coreInput->PointerReleased += ref new TypedEventHandler<Object^, PointerEventArgs^>(this, &DirectXPage::OnPointerReleased);
```

Die MarbleMaze-Klasse erstellt außerdem ein "std::map"-Objekt für Touchereignisse. Der Schlüssel für dieses Zuordnungsobjekt ist ein Wert, der den Eingabezeiger eindeutig identifiziert. Jeder Schlüssel ist der Entfernung zwischen jedem Touchpunkt und der Mitte des Bildschirms zugeordnet. Diese Werte verwendet Marble Maze später, um die Neigung des Labyrinths zu berechnen.

```cpp
typedef std::map<int, XMFLOAT2> TouchMap;
TouchMap        m_touches;
```

Die MarbleMaze-Klasse enthält ein Beschleunigungsmesserobjekt.

```cpp
Windows::Devices::Sensors::Accelerometer^           m_accelerometer;
```

Das Beschleunigungsmesserobjekt wird wie im folgenden Beispiel in der "MarbleMaze::Initialize"-Methode initialisiert. Die "Windows::Devices::Sensors::Accelerometer::GetDefault"-Methode gibt eine Instanz des Standardbeschleunigungsmessers zurück. Wenn kein Standardbeschleunigungsmesser vorhanden ist, bleibt der „m\_accelerometer“-Wert von „Accelerometer::GetDefault“ auf „nullptr“ festgelegt.

```cpp
// Returns accelerometer ref if there is one; nullptr otherwise.
m_accelerometer = Windows::Devices::Sensors::Accelerometer::GetDefault();
```

##  Navigieren in den Menüs


###  Nachverfolgen von Xbox 360-Controllereingaben

Sie haben folgende Möglichkeiten, mit der Maus, mit Toucheingabe oder mit dem Xbox 360-Controller in den Menüs zu navigieren:

-   Verwenden Sie das Steuerkreuz, um das aktive Menüelement zu ändern.
-   Verwenden Sie Toucheingabe, die A-Taste oder die Starttaste, um ein Menüelement auszuwählen oder das aktuelle Menü zu schließen, beispielsweise die Highscoretabelle.
-   Verwenden Sie die Starttaste, um das Spiel anzuhalten oder fortzusetzen.
-   Klicken Sie mit der Maus auf ein Menüelement, um die jeweilige Aktion auszuwählen.

###  Nachverfolgen von Touch- und Mauseingaben

Zum Nachverfolgen von Eingaben mit dem Xbox 360-Controller definiert die **MarbleMaze::Update**-Methode ein Array mit Tasten, die das Eingabeverhalten definieren. XInput stellt nur den aktuellen Zustand des Controllers bereit. Daher definiert **MarbleMaze::Update** außerdem zwei Arrays, die für jeden möglichen Xbox 360-Controller nachverfolgen, ob im vorherigen Frame eine der Tasten gedrückt wurde und ob eine der Tasten zurzeit gedrückt wird.

```cpp
// This array contains the constants from XINPUT that map to the 
// particular buttons that are used by the game. 
// The index of the array is used to associate the state of that button in 
// the wasButtonDown, isButtonDown, and combinedButtonPressed variables. 

static const WORD buttons[] = {
    XINPUT_GAMEPAD_A,
    XINPUT_GAMEPAD_START,
    XINPUT_GAMEPAD_DPAD_UP,
    XINPUT_GAMEPAD_DPAD_DOWN,
    XINPUT_GAMEPAD_DPAD_LEFT,
    XINPUT_GAMEPAD_DPAD_RIGHT,
    XINPUT_GAMEPAD_BACK,
};

static const int buttonCount = ARRAYSIZE(buttons);
static bool wasButtonDown[XUSER_MAX_COUNT][buttonCount] = { false, };
bool isButtonDown[XUSER_MAX_COUNT][buttonCount] = { false, };
```

Sie können an ein Windows-Gerät bis zu vier Xbox 360-Controller anschließen. Damit nicht ermittelt werden muss, welcher Controller aktiv ist, kombiniert die **MarbleMaze::Update**-Methode die Eingaben aller Controller.

```cpp
bool combinedButtonPressed[buttonCount] = { false, };
```

Wenn Ihr Spiel mehrere Spieler unterstützt, müssen Sie die Eingabe für jeden Spieler getrennt nachverfolgen.

Die **MarbleMaze::Update**-Methode ruft in einer Schleife die Eingaben jedes Controllers ab und liest den Zustand jeder Taste.

```cpp
// Account for input on any connected controller.
XINPUT_STATE inputState = {0};
for (DWORD userIndex = 0; userIndex < XUSER_MAX_COUNT; ++userIndex)
{
    DWORD result = XInputGetState(userIndex, &inputState);
    if(result != ERROR_SUCCESS) 
        continue;

    SHORT thumbLeftX = inputState.Gamepad.sThumbLX;
    if (abs(thumbLeftX) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftX = 0;

    SHORT thumbLeftY = inputState.Gamepad.sThumbLY;
    if (abs(thumbLeftY) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftY = 0;

    combinedTiltX += static_cast<float>(thumbLeftX) / 32768.0f;
    combinedTiltY += static_cast<float>(thumbLeftY) / 32768.0f;

    for (int i = 0; i < buttonCount; ++i)
        isButtonDown[userIndex][i] = (inputState.Gamepad.wButtons & buttons[i]) == buttons[i];
}
```

Wenn die **MarbleMaze::Update**-Methode die Eingaben abgerufen hat, aktualisiert sie das kombinierte Eingabearray. Das kombinierte Eingabearray verfolgt nur nach, welche Tasten gedrückt sind, die vorher nicht gedrückt waren. So kann im Spiel eine Aktion nur beim ersten Drücken einer Taste ausgeführt werden und nicht, wenn die Taste gedrückt gehalten wird.

```cpp
bool combinedButtonPressed[buttonCount] = { false, };
for (int i = 0; i < buttonCount; ++i)
{
    for (DWORD userIndex = 0; userIndex < XUSER_MAX_COUNT; ++userIndex)
    {
        bool pressed = !wasButtonDown[userIndex][i] && isButtonDown[userIndex][i];
        combinedButtonPressed[i] = combinedButtonPressed[i] || pressed;
    }
}
```

Wenn die **MarbleMaze::Update**-Methode die Tasteneingabe erfasst hat, führt sie alle erforderlichen Aktionen aus. Wenn beispielsweise die Starttaste („XINPUT\_GAMEPAD\_START“) gedrückt wird, ändert sich der Spielzustand von aktiv in angehalten oder von angehalten in aktiv.

```cpp
// Check whether the user paused or resumed the game. 
// XINPUT_GAMEPAD_START  
if (combinedButtonPressed[1] || m_pauseKeyPressed)
{
    m_pauseKeyPressed = false;
    if (m_gameState == GameState::InGameActive)
        SetGameState(GameState::InGamePaused);
    else if (m_gameState == GameState::InGamePaused)
        SetGameState(GameState::InGameActive);
}
```

Wenn das Hauptmenü aktiv ist, ändert sich das aktive Menüelement, sobald das Steuerkreuz nach oben oder nach unten gedrückt wird. Wenn der Benutzer die aktuelle Auswahl auswählt, wird das entsprechende UI-Element als ausgewählt markiert.

```cpp
// Handle menu navigation. 

// XINPUT_GAMEPAD_A or XINPUT_GAMEPAD_START 
bool chooseSelection = (combinedButtonPressed[0] || combinedButtonPressed[1]);

// XINPUT_GAMEPAD_DPAD_UP 
bool moveUp = combinedButtonPressed[2];

// XINPUT_GAMEPAD_DPAD_DOWN 
bool moveDown = combinedButtonPressed[3];                                           

switch (m_gameState)
{
case GameState::MainMenu:
    if (chooseSelection)
    {
        m_audio.PlaySoundEffect(MenuSelectedEvent);

        if (m_startGameButton.GetSelected())
            m_startGameButton.SetPressed(true);

        if (m_highScoreButton.GetSelected())
            m_highScoreButton.SetPressed(true);
    }
    if (moveUp || moveDown)
    {
        m_startGameButton.SetSelected(!m_startGameButton.GetSelected());
        m_highScoreButton.SetSelected(!m_startGameButton.GetSelected());

        m_audio.PlaySoundEffect(MenuChangeEvent);
    }
    break;

case GameState::HighScoreDisplay:
    if (chooseSelection || anyPoints)
        SetGameState(GameState::MainMenu);
    break;

case GameState::PostGameResults:
    if (chooseSelection || anyPoints)
        SetGameState(GameState::HighScoreDisplay);
    break;

case GameState::InGamePaused:
    if (m_pausedText.IsPressed())
    {
        m_pausedText.SetPressed(false);
        SetGameState(GameState::InGameActive); 
    }
    break;
}
```

Wenn die **MarbleMaze::Update**-Methode die Controllereingabe verarbeitet hat, speichert sie den aktuellen Eingabezustand für den nächsten Frame.

```cpp
// Update the button state for the next frame.
memcpy(wasButtonDown, isButtonDown, sizeof(wasButtonDown));
```

### Nachverfolgen von Touch- und Mauseingaben

Bei der Touch- und Mauseingabe wird ein Menüelement ausgewählt, wenn der Benutzer das Element berührt oder darauf klickt. Das folgende Beispiel zeigt, wie die **MarbleMaze::Update**-Methode die Zeigereingabe verarbeitet, um Menüelemente auszuwählen. Die **m\_pointQueue**-Membervariable verfolgt die Stellen auf dem Bildschirm nach, die der Benutzer berührt hat oder auf die er geklickt hat. Im Abschnitt "Verarbeiten von Zeigereingaben" wird ausführlicher beschrieben, wie Marble Maze die Zeigereingaben erfasst.

```cpp
// Check whether the user chose a button from the UI. 
bool anyPoints = !m_pointQueue.empty();
while (!m_pointQueue.empty())
{
    UserInterface::GetInstance().HitTest(m_pointQueue.front());
    m_pointQueue.pop();
}
```

Die **UserInterface::HitTest**-Methode ermittelt, ob sich der angegebene Punkt innerhalb der Grenzen eines UI-Elements befindet. Alle UI-Elemente, die diesen Test bestanden haben, werden als berührt markiert. Diese Methode ermittelt mit der **PointInRect**-Hilfsfunktion, ob sich der angegebene Punkt innerhalb der Grenzen der einzelnen UI-Elemente befindet.

```cpp
void UserInterface::HitTest(D2D1_POINT_2F point)
{
    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if (!(*iter)->IsVisible())
            continue;

        TextButton* textButton = dynamic_cast<TextButton*>(*iter);
        if (textButton != nullptr)
        {
            D2D1_RECT_F bounds = (*iter)->GetBounds();
            textButton->SetPressed(PointInRect(point, bounds));
        }
    }
}
```

### Aktualisieren des Spielzustands

Wenn die **MarbleMaze::Update**-Methode die Controller- und Toucheingabe verarbeitet hat und eine Taste gedrückt wurde, aktualisiert sie den Spielzustand.

```cpp
// Update the game state if the user chose a menu option. 
if (m_startGameButton.IsPressed())
{
    SetGameState(GameState::PreGameCountdown);
    m_startGameButton.SetPressed(false);
}
if (m_highScoreButton.IsPressed())
{
    SetGameState(GameState::HighScoreDisplay);
    m_highScoreButton.SetPressed(false);
}
```

##  Steuern des Spielverlaufs


Die Spielschleife und die **MarbleMaze::Update**-Methode aktualisieren gemeinsam den Zustand der Spielobjekte. Wenn Ihr Spiel Eingaben von mehreren Geräten akzeptiert, können Sie die Eingaben aller Geräte in einem Satz von Variablen sammeln. Auf diese Weise können Sie Code schreiben, der sich leichter verwalten lässt. Die **MarbleMaze::Update**-Methode definiert einen Variablensatz, mit dem Bewegungen von allen Geräten gesammelt werden.

```cpp
float combinedTiltX = 0.0f;
float combinedTiltY = 0.0f;
```

Die Eingabemechanismen der einzelnen Eingabegeräte können dabei unterschiedlich sein. Beispielsweise werden Zeigereingaben mit dem Ereignisbehandlungsmodell der Windows-Runtime behandelt. Die Eingabedaten des Xbox 360-Controllers dagegen rufen Sie bei Bedarf ab. Am besten halten Sie sich immer an die Eingabemechanismen, die für ein bestimmtes Gerät vorgeschrieben sind. In diesem Abschnitt wird beschrieben, wie Marble Maze die Eingaben von den einzelnen Geräten liest, die kombinierten Eingabewerte aktualisiert und sie verwendet, um den Spielzustand zu aktualisieren.

###  Verarbeiten von Zeigereingaben

Wenn Sie mit Zeigereingaben arbeiten, rufen Sie die [**Windows::UI::Core::CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208217)-Methode auf, um Windows-Ereignisse zu verarbeiten. Rufen Sie diese Methode in Ihrer Spielschleife auf, bevor Sie die Szene aktualisieren oder rendern. Marble Maze übergibt **CoreProcessEventsOption::ProcessAllIfPresent** an diese Methode, um alle Ereignisse in der Warteschlange zu verarbeiten. Anschließend erfolgt sofort die Rückgabe. Nach der Verarbeitung der Ereignisse rendert Marble Maze den nächsten Frame und zeigt ihn an.

```cpp
// Process windowing events.
CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
```

Die Windows-Runtime ruft den registrierten Handler für jedes aufgetretene Ereignis auf. Die **DirectXApp**-Klasse registriert sich für Ereignisse und leitet Zeigerinformationen an die **MarbleMaze**-Klasse weiter.

```cpp
void DirectXApp::OnPointerPressed(
    _In_ Windows::UI::Core::CoreWindow^ sender,
    _In_ Windows::UI::Core::PointerEventArgs^ args
    )
{
    m_renderer->AddTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}

void DirectXApp::OnPointerReleased(
    _In_ Windows::UI::Core::CoreWindow^ sender,
    _In_ Windows::UI::Core::PointerEventArgs^ args
    )
{
    m_renderer->RemoveTouch(args->CurrentPoint->PointerId);
}

void DirectXApp::OnPointerMoved(
    _In_ Windows::UI::Core::CoreWindow^ sender,
    _In_ Windows::UI::Core::PointerEventArgs^ args
    )
{
    m_renderer->UpdateTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}
```

Die **MarbleMaze**-Klasse reagiert auf Zeigerereignisse, indem sie das Zuordnungsobjekt aktualisiert, das Touchereignisse enthält. Die **MarbleMaze::AddTouch**-Methode wird aufgerufen, wenn der Zeiger zum ersten Mal gedrückt wird. Dies ist beispielsweise der Fall, wenn der Benutzer zum ersten Mal den Bildschirm eines touchfähigen Geräts berührt. Die **MarbleMaze::AddTouch**-Methode wird aufgerufen, wenn die Zeigerposition verschoben wird. Die **MarbleMaze::RemoveTouch**-Methode wird aufgerufen, wenn der Zeiger losgelassen wird. Dies ist beispielsweise der Fall, wenn der Benutzer den Bildschirm nicht mehr berührt.

```cpp
void MarbleMaze::AddTouch(int id, Windows::Foundation::Point point)
{
    m_touches[id] = PointToTouch(point, m_windowBounds);

    m_pointQueue.push(D2D1::Point2F(point.X, point.Y));
}

void MarbleMaze::UpdateTouch(int id, Windows::Foundation::Point point)
{
    if (m_touches.find(id) != m_touches.end())
        m_touches[id] = PointToTouch(point, m_windowBounds);
}

void MarbleMaze::RemoveTouch(int id)
{
    m_touches.erase(id);
}
```

Die "PointToTouch"-Funktion verschiebt die aktuelle Zeigerposition, sodass sich der Ursprung in der Mitte des Bildschirms befindet. Dann skaliert sie die Koordinaten so, dass sie ungefähr zwischen -1,0 und +1,0 liegen. Dadurch kann die einheitliche Neigung des Labyrinths für verschiedene Eingabemethoden leichter berechnet werden.

```cpp
inline XMFLOAT2 PointToTouch(Windows::Foundation::Point point, Windows::Foundation::Rect bounds)
{
    float touchRadius = min(bounds.Width, bounds.Height);
    float dx = (point.X - (bounds.Width / 2.0f)) / touchRadius;
    float dy = ((bounds.Height / 2.0f) - point.Y) / touchRadius;

    return XMFLOAT2(dx, dy);
}
```

Die **MarbleMaze::Update**-Methode aktualisiert die kombinierten Eingabewerte, indem sie den Neigungsfaktor um einen konstanten Skalierungswert erhöht. Dieser Skalierungswert wurde durch Experimentieren mit verschiedenen Werten ermittelt.

```cpp
// Account for touch input. 
const float touchScalingFactor = 2.0f;
for (TouchMap::const_iterator iter = m_touches.cbegin(); iter != m_touches.cend(); ++iter)
{
    combinedTiltX += iter->second.x * touchScalingFactor;
    combinedTiltY += iter->second.y * touchScalingFactor;
}
```

### Verarbeiten von Beschleunigungsmessereingaben

Zur Verarbeitung von Beschleunigungsmessereingaben ruft die **MarbleMaze::Update**-Methode die [**Windows::Devices::Sensors::Accelerometer::GetCurrentReading**](https://msdn.microsoft.com/library/windows/apps/br225699)-Methode auf. Diese Methode gibt ein [**Windows::Devices::Sensors::AccelerometerReading**](https://msdn.microsoft.com/library/windows/apps/br225688)-Objekt zurück, das die Ablesung eines Beschleunigungsmessers darstellt. Die Eigenschaften **Windows::Devices::Sensors::AccelerometerReading::AccelerationX** und **Windows::Devices::Sensors::AccelerometerReading::AccelerationY** enthalten die Schwerkraftbeschleunigung entlang der X-Achse bzw. der Y-Achse.

Das folgende Beispiel zeigt, wie die **MarbleMaze::Update**-Methode den Beschleunigungsmesser abruft und die kombinierten Eingabewerte aktualisiert. Wenn Sie das Gerät neigen, bewegt sich die Murmel aufgrund der Schwerkraft schneller.

```cpp
// Account for sensors. 
const float acceleromterScalingFactor = 3.5f;
if (m_accelerometer != nullptr)
{
    Windows::Devices::Sensors::AccelerometerReading^ reading =
        m_accelerometer->GetCurrentReading();

    if (reading != nullptr)
    {
        combinedTiltX += static_cast<float>(reading->AccelerationX) * acceleromterScalingFactor;
        combinedTiltY += static_cast<float>(reading->AccelerationY) * acceleromterScalingFactor;
    }
}
```

Da Sie nicht sicher sein können, dass der Computer des Benutzers über einen Beschleunigungsmesser verfügt, müssen Sie immer sicherstellen, dass Sie ein gültiges Beschleunigungsmesserobjekt haben, und erst dann die Werte vom Beschleunigungsmesser abrufen.

### Verarbeiten von Xbox 360-Controllereingaben

Das folgende Beispiel zeigt, wie die **MarbleMaze::Update**-Methode Werte vom Xbox 360-Controller abliest und die kombinierten Eingabewerte aktualisiert. Die **MarbleMaze::Update**-Methode verwendet eine „for“-Schleife, damit Eingaben von einem beliebigen angeschlossenen Controller empfangen werden können. Die **XInputGetState**-Methode füllt ein „XINPUT\_STATE“-Objekt mit dem aktuellen Zustand des Controllers. Die Werte **combinedTiltX** und **combinedTiltY** werden gemäß den X- und Y-Werten des linken Ministicks aktualisiert.

```cpp
// Account for input on any connected controller.
XINPUT_STATE inputState = {0};
for (DWORD userIndex = 0; userIndex < XUSER_MAX_COUNT; ++userIndex)
{
    DWORD result = XInputGetState(userIndex, &inputState);
    if(result != ERROR_SUCCESS) 
        continue;

    SHORT thumbLeftX = inputState.Gamepad.sThumbLX;
    if (abs(thumbLeftX) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftX = 0;

    SHORT thumbLeftY = inputState.Gamepad.sThumbLY;
    if (abs(thumbLeftY) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftY = 0;

    combinedTiltX += static_cast<float>(thumbLeftX) / 32768.0f;
    combinedTiltY += static_cast<float>(thumbLeftY) / 32768.0f;

    for (int i = 0; i < buttonCount; ++i)
        isButtonDown[userIndex][i] = (inputState.Gamepad.wButtons & buttons[i]) == buttons[i];
}
```

XInput definiert die **XINPUT\_GAMEPAD\_LEFT\_THUMB\_DEADZONE**-Konstante für den linken Ministick. Dieser Schwellenwert für den inaktiven Bereich eignet sich für die meisten Spiele.

> **Wichtig**  Berücksichtigen Sie immer den inaktiven Bereich, wenn Sie mit dem Xbox 360-Controller arbeiten. Der inaktive Bereich bezieht sich auf die unterschiedliche Empfindlichkeit von Gamepads für die ersten Bewegungen. Bei manchen Controllern ergibt eine kleine Bewegung keinen Messwert, während sie bei anderen zu einem messbaren Ergebnis führt. Berücksichtigen Sie dies in Ihrem Spiel, indem Sie für die ersten Bewegungen des Ministicks einen Bereich für Nichtbewegungen erstellen. Weitere Informationen zum inaktiven Bereich finden Sie unter [Erste Schritte mit XInput](https://msdn.microsoft.com/library/windows/desktop/ee417001).

 

###  Anwenden von Eingaben auf den Spielzustand

Geräte melden Eingabewerte auf unterschiedliche Weise. So wird eine Zeigereingabe möglicherweise in Bildschirmkoordinaten angegeben, eine Controllereingabe aber in einem völlig anderen Format. Beim Kombinieren der Eingaben von mehreren Geräten in einem Satz von Eingabewerten besteht eine Herausforderung darin, die Werte in ein gemeinsames Format zu normalisieren oder konvertieren. Marble Maze normalisiert Werte durch Skalieren auf einen Bereich \[-1,0, 1,0\]. Beim Normalisieren von Xbox 360-Controllereingaben teilt Marble Maze die Eingabewerte durch 32768, da Eingabewerte von Ministicks immer zwischen -32768 und 32767 liegen. Mit der weiter oben in diesem Abschnitt beschriebenen **PointToTouch**-Funktion wird ein ähnliches Ergebnis erzielt, indem Bildschirmkoordinaten in normalisierte Werte konvertiert werden. Diese Werte liegen ungefähr zwischen -1,0 und +1,0.

> **Tipp**  Auch wenn Ihre Anwendung eine einzige Eingabemethode verwendet, sollten Sie die Eingabewerte immer normalisieren. Auf diese Weise können Sie die Interpretation der Eingabe durch andere Komponenten des Spiels wie beispielsweise die Simulation von Physikeffekten vereinfachen und leichter Spiele schreiben, die sich für unterschiedliche Bildschirmauflösungen eignen.

 

Nachdem die **MarbleMaze::Update**-Methode die Eingabe verarbeitet hat, erstellt sie einen Vektor, der die Auswirkung der Neigung des Labyrinths auf die Murmel darstellt. Das folgende Beispiel zeigt, wie Marble Maze mit der **XMVector3Normalize**-Funktion einen normalisierten Schwerkraftvektor erstellt. Die *MaxTilt*-Variable schränkt die Neigung des Labyrinths ein und verhindert, dass das Labyrinth auf die Seite gekippt wird.

```cpp
const float maxTilt = 1.0f / 8.0f;
XMVECTOR gravity = XMVectorSet(combinedTiltX * maxTilt, combinedTiltY * maxTilt, 1.0f, 0.0f);
gravity = XMVector3Normalize(gravity);
```

Als letzten Schritt beim Aktualisieren der Szenenobjekte übergibt Marble Maze den aktualisierten Schwerkraftvektor der Simulation für Physikeffekte, aktualisiert diese Simulation für den seit dem vorherigen Frame verstrichenen Zeitraum und aktualisiert die Position und Ausrichtung der Murmel. Wenn die Murmel durch das Labyrinth gefallen ist, platziert die **MarbleMaze::Update**-Methode die Murmel wieder am letzten Prüfpunkt, den die Murmel berührt hat, und setzt den Zustand der Simulation für Physikeffekte zurück.

```cpp
XMFLOAT3 g;
XMStoreFloat3(&g, gravity);
m_physics.SetGravity(g);
```

```cpp
// Only update physics when gameplay is active.
m_physics.UpdatePhysicsSimulation(timeDelta);
```

```cpp
// Check whether the marble fell off of the maze. 
const float fadeOutDepth = 0.0f;
const float resetDepth = 80.0f;
if (marblePosition.z >= fadeOutDepth)
{
    m_targetLightStrength = 0.0f;
}
if (marblePosition.z >= resetDepth)
{
    // Reset marble.
    memcpy(&marblePosition, &m_checkpoints[m_currentCheckpoint], sizeof(XMFLOAT3));
    oldMarblePosition = marblePosition;
    m_physics.SetPosition((const XMFLOAT3&)marblePosition);
    m_physics.SetVelocity(XMFLOAT3(0, 0, 0));
    m_lightStrength = 0.0f;
    m_targetLightStrength = 1.0f;

    m_resetCamera = true;
    m_resetMarbleRotation = true;
    m_audio.PlaySoundEffect(FallingEvent);
}
```

In diesem Abschnitt wird nicht beschrieben, wie die Simulation für Physikeffekte funktioniert. Details hierzu finden Sie in den Quellen für Marble Maze in "Physics.h" und "Physics.cpp".

## Nächste Schritte


Lesen Sie [Hinzufügen von Audio zum Marble Maze-Beispiel](adding-audio-to-the-marble-maze-sample.md). Dort finden Sie Informationen zu einigen der wichtigen Methoden, die Sie beim Arbeiten mit Audio berücksichtigen sollten. In diesem Dokument wird erörtert, wie Marble Maze mithilfe von Microsoft Media Foundation und XAudio2 Audioressourcen lädt, mischt und wiedergibt.

## Verwandte Themen


* [Hinzufügen von Audiodaten zum Marble Maze-Beispiel](adding-audio-to-the-marble-maze-sample.md)
* [Hinzufügen von visuellen Inhalten zum Marble Maze-Beispiel](adding-visual-content-to-the-marble-maze-sample.md)
* [Entwickeln von Marble Maze, einem UWP-Spiel in C++ und DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 







<!--HONumber=Jun16_HO4-->


