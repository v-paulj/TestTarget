---
author: mtoepke
title: "Bewegungs-/Blicksteuerungen für Spiele"
description: "Hier erfahren Sie, wie Sie Ihrem DirectX-Spiel herkömmliche Bewegungs-/Blicksteuerungen für Maus und Tastatur (auch als Maussteuerungen bezeichnet) hinzufügen."
ms.assetid: 4b4d967c-3de9-8a97-ae68-0327f00cc933
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 7adbfdb77af6992be9448969f635bdebac58344b

---

# <span id="dev_gaming.tutorial__adding_move-look_controls_to_your_directx_game"></span>Bewegungs-/Blicksteuerungen für Spiele


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Hier erfahren Sie, wie Sie Ihrem DirectX-Spiel herkömmliche Bewegungs-/Blicksteuerungen für Maus und Tastatur (auch als Maussteuerungen bezeichnet) hinzufügen.

Außerdem erörtern wir die Bewegungs-/Blicksteuerungsunterstützung für Toucheingabegräte mit dem als linker unterer Bildschirmabschnitt definierten Bewegungscontroller, der als Richtungseingabe fungiert, und dem für den restlichen Bildschirm definierten Blickcontroller, dessen Kamera mittig auf die letzte Stelle ausgerichtet wird, die der Spieler in diesem Bereich berührt hat.

Falls Ihnen dieses Steuerungskonzept nicht vertraut ist, können Sie sich das wie folgt vorstellen: Die Tastatur (oder das auf Toucheingabe basierende Richtungseingabefeld) steuert Ihre Beine im 3D-Raum und verhält sich so als könnten sich Ihre Beine nur vorwärts oder rückwärts bzw. nur nach links und rechts bewegen. Die Maus (oder der Fingereingabezeiger) steuert Ihren Kopf. Mit dem Kopf schauen Sie in eine Richtung – nach links oder rechts, nach oben oder unten oder zu einer Stelle in dieser Ebene. Befindet sich ein Ziel in Ihrer Sicht, richten Sie die Kameraansicht mit der Maus mittig auf dieses Ziel aus und drücken die Vorwärts-Taste, um sich zum Ziel hin zu bewegen, oder die Rückwärts-Taste, um sich vom Ziel weg zu bewegen. Um das Ziel einzukreisen, halten Sie die Kameraansicht auf das Ziel ausgerichtet und bewegen sich gleichzeitig nach links oder rechts. Wie Sie sehen, ist dies eine sehr effektive Steuerungsmethode für die Navigation in 3D-Umgebungen.

Diese Steuerungen werden bei Spielen im Allgemeinen als WASD-Steuerungen bezeichnet: Die Tasten W, A, S und D werden für die feste Kamerabewegung in der Z-Ebene verwendet, und die Maus wird zum Steuern der Kameradrehung um die X- und Y-Achsen verwendet.

## Ziele


-   Hinzufügen einfacher Bewegungs-/Blicksteuerungen zum DirectX-Spiel für Maus und Tastatur sowie Touchscreens
-   Implementieren einer First-Person-Kamera zum Navigieren in einer 3D-Umgebung

## Hinweis zur Implementierung der Fingereingabesteuerung


Für die Toucheingabesteuerungen werden zwei Controller implementiert: der Bewegungscontroller, der die Bewegung in der Z-Ebene relativ zum Blickpunkt der Kamera behandelt, und der Blickcontroller, der den Blickpunkt der Kamera ausrichtet. Der Bewegungscontroller ist den WASD-Tasten auf der Tastatur zugeordnet und der Blickcontroller der Maus. Für die Fingereingabesteuerungen müssen wir aber einen Bereich des Bildschirms festlegen, der als Richtungseingabe bzw. als virtuelle WASD-Tasten dient, während der restliche Bildschirm den Eingabebereich für die Blicksteuerungen darstellt.

Der Bildschirm sieht wie folgt aus:

![Layout des Bewegungs-/Blickrichtungscontrollers](images/movelook-touch.png)

Wenn Sie in der linken unteren Bildschirmecke den Toucheingabezeiger (nicht die Maus!) nach oben bewegen, bewegt sich die Kamera vorwärts. Bei allen Abwärtsbewegungen bewegt sich die Kamera rückwärts. Das Gleiche gilt für Bewegungen nach links und rechts im Zeigerbereich des Bewegungscontrollers. Außerhalb dieses Bereichs wird der Zeiger zum Blickcontroller – Sie ziehen die Kamera einfach durch Berühren des Bildschirms in die gewünschte Blickrichtung.

## Einrichten der grundlegenden Eingabeereignisinfrastruktur


Zunächst müssen wir die Steuerungsklasse erstellen, mit der wir Eingabeereignisse von Maus und Tastatur behandeln, und die Kameraperspektive auf der Grundlage dieser Eingabe aktualisieren. Da wir Bewegungs-/Blicksteuerungen implementieren, nennen wir diese Klasse **MoveLookController**.

```cpp
using namespace Windows::UI::Core;
using namespace Windows::System;
using namespace Windows::Foundation;
using namespace Windows::Devices::Input;
#include <DirectXMath.h>

// Methods to get input from the UI pointers
ref class MoveLookController
{
};  // class MoveLookController
```

Jetzt erstellen wir einen Header, der den Zustand des Bewegungs-/Blickcontrollers und der First-Person-Kamera definiert, sowie die grundlegenden Methoden und Ereignishandler, die die Steuerungen implementieren und den Zustand der Kamera aktualisieren.

```cpp
#define ROTATION_GAIN 0.004f    // Sensitivity adjustment for the look controller
#define MOVEMENT_GAIN 0.1f      // Sensitivity adjustment for the move controller

ref class MoveLookController
{
private:
    // Properties of the controller object
    DirectX::XMFLOAT3 m_position;               // The position of the controller
    float m_pitch, m_yaw;           // Orientation euler angles in radians

    // Properties of the Move control
    bool m_moveInUse;               // Specifies whether the move control is in use
    uint32 m_movePointerID;         // Id of the pointer in this control
    DirectX::XMFLOAT2 m_moveFirstDown;          // Point where initial contact occurred
    DirectX::XMFLOAT2 m_movePointerPosition;   // Point where the move pointer is currently located
    DirectX::XMFLOAT3 m_moveCommand;            // The net command from the move control

    // Properties of the Look control
    bool m_lookInUse;               // Specifies whether the look control is in use
    uint32 m_lookPointerID;         // Id of the pointer in this control
    DirectX::XMFLOAT2 m_lookLastPoint;          // Last point (from last frame)
    DirectX::XMFLOAT2 m_lookLastDelta;          // For smoothing

    bool m_forward, m_back;         // States for movement
    bool m_left, m_right;
    bool m_up, m_down;


public:

    // Methods to get input from the UI pointers
    void OnPointerPressed(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );

    void OnPointerMoved(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );

    void OnPointerReleased(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );

    void OnKeyDown(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::KeyEventArgs^ args
        );

    void OnKeyUp(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::KeyEventArgs^ args
        );

    // Set up the Controls that this controller supports
    void Initialize( _In_ Windows::UI::Core::CoreWindow^ window );

    void Update( Windows::UI::Core::CoreWindow ^window );
    
internal:
    // Accessor to set position of controller
    void SetPosition( _In_ DirectX::XMFLOAT3 pos );

    // Accessor to set position of controller
    void SetOrientation( _In_ float pitch, _In_ float yaw );

    // Returns the position of the controller object
    DirectX::XMFLOAT3 get_Position();

    // Returns the point  which the controller is facing
    DirectX::XMFLOAT3 get_LookPoint();


};  // class MoveLookController
```

Unser Code enthält vier Gruppen privater Felder. Im Folgenden klären wir, wofür die einzelnen Felder dienen.

Als Erstes definieren wir einige nützliche Felder, die die aktualisierten Informationen zur Kameraansicht enthalten.

-   **m\_position** ist die Position der Kamera (und daher die Bildebene) in der 3D-Szene (in Szenenkoordinaten).
-   **m\_pitch** ist der Nickwinkel der Kamera bzw. ihre Drehung nach oben oder unten um die X-Achse der Bildebene (in Radianten).
-   **m\_yaw** ist der Gierwinkel der Kamera bzw. ihre Drehung nach links oder rechts um die Y-Achse der Bildebene (in Radianten).

Als Nächstes definieren wir die Felder, in denen Informationen zum Status und zur Position der Controller gespeichert werden. Die Felder, die wir für den fingereingabebasierten Bewegungscontroller benötigen, definieren wir zuerst. (Für die Tastaturimplementierung des Bewegungscontrollers sind keine speziellen Felder erforderlich. Es werden nur Tastaturereignisse mit bestimmten Handlern gelesen.)

-   **m\_moveInUse** gibt an, ob der Bewegungscontroller verwendet wird.
-   **m\_movePointerID** ist die eindeutige ID für den aktuellen Bewegungszeiger. Damit wird beim Überprüfen der Zeiger-ID zwischen dem Blickzeiger und dem Bewegungszeiger unterschieden.
-   **m\_moveFirstDown** ist der Punkt auf dem Bildschirm, an dem der Spieler den Zeigerbereich des Bewegungscontrollers zuerst berührt hat. Dieser Wert wird später verwendet, um einen inaktiven Bereich festzulegen, damit die Ansicht bei geringfügigen Bewegungen nicht zittert.
-   **m\_movePointerPosition** ist der Punkt auf dem Bildschirm, an den der Spieler den Zeiger gerade bewegt hat. Dieser Wert wird mit **m\_moveFirstDown** verglichen, um die beabsichtigte Bewegungsrichtung des Spielers zu bestimmen.
-   **m\_moveCommand** ist der berechnete endgültige Befehl für den Bewegungscontroller: nach oben (vorwärts), nach unten (rückwärts), nach links oder nach rechts.

Jetzt definieren wir die Felder für den Blickcontroller (für Maus- und Toucheingabeimplementierungen).

-   **m\_lookInUse** gibt an, ob die Blicksteuerung verwendet wird.
-   **m\_lookPointerID** ist die eindeutige ID für den aktuellen Blickzeiger. Damit wird beim Überprüfen der Zeiger-ID zwischen dem Blickzeiger und dem Bewegungszeiger unterschieden.
-   **m\_lookLastPoint** ist der letzte Punkt, der im vorherigen Frame erfasst wurde (in Szenenkoordinaten).
-   **m\_lookLastDelta** ist die berechnete Differenz zwischen der aktuellen **m\_position** und **m\_lookLastPoint**.

Zum Schluss definieren wir sechs boolesche Werte für die sechs Bewegungsgrade, mit denen der aktuelle Zustand der einzelnen Bewegungsaktionen angegeben wird (Ein oder Aus):

-   **m\_forward**, **m\_back**, **m\_left**, **m\_right**, **m\_up** und **m\_down**.

Die Eingabedaten zum Aktualisieren des Zustands der Controller werden mit sechs Ereignishandlern erfasst:

-   **OnPointerPressed**. Der Spieler hat die linke Maustaste gedrückt oder den Bildschirm berührt, während sich der Zeiger im Spielbildschirm befand.
-   **OnPointerMoved**. Der Spieler hat die Maus oder den Toucheingabezeiger auf dem Bildschirm bewegt, während sich der Zeiger im Spielbildschirm befand.
-   **OnPointerReleased**. Der Spieler hat die linke Maustaste losgelassen oder aufgehört, den Bildschirm zu berühren, während sich der Zeiger im Spielbildschirm befand.
-   **OnKeyDown**. Der Spieler hat eine Taste gedrückt.
-   **OnKeyUp**. Der Spieler hat eine Taste losgelassen.

Die folgenden Methoden und Eigenschaften verwenden wir, um die Zustandsinformationen der Controller zu initialisieren, auf sie zuzugreifen und sie zu aktualisieren.

-   **Initialize**. Die App ruft diesen Ereignishandler auf, um die Steuerungen zu initialisieren und an das [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Objekt anzufügen, das das Anzeigefenster beschreibt.
-   **SetPosition**. Die App ruft diese Methode auf, um die Koordinaten (X, Y und Z) der Steuerungen im Szenenbereich festzulegen.
-   **SetOrientation**. Die App ruft diese Methode auf, um den Nick- und Gierwinkel der Kamera festzulegen.
-   **get\_Position**. Die App greift auf diese Eigenschaft zu, um die aktuelle Position der Kamera im Szenenbereich abzurufen. Diese Eigenschaft wird verwendet, um der App die aktuelle Kameraposition mitzuteilen.
-   **get\_LookPoint**. Die App greift auf diese Eigenschaft zu, um den aktuellen Punkt abzurufen, auf den die Kamera gerichtet ist.
-   **Update**. Diese Methode liest den Zustand der Bewegungs- und Blickcontroller und aktualisiert die Kameraposition. Diese Methode wird in der Hauptschleife der App kontinuierlich aufgerufen, um die Kameracontrollerdaten und die Kameraposition im Szenenbereich zu aktualisieren.

Jetzt haben Sie alle Komponenten, die Sie zum Implementieren der Bewegungs-/Blicksteuerungen benötigen. Als Nächstes setzen wir diese Teile zusammen.

## Erstellen der grundlegenden Eingabeereignisse


Der Ereignisverteiler der Windows-Runtime stellt fünf Ereignisse bereit, die von Instanzen der **MoveLookController**-Klasse behandelt werden sollen:

-   [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278)
-   [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276)
-   [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279)
-   [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208271)
-   [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208270)

Diese Ereignisse sind im [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Typ implementiert. Es wird davon ausgegangen, dass Sie über ein **CoreWindow**-Objekt verfügen, mit dem Sie arbeiten können. Informationen dazu, wie Sie ein solches Objekt erhalten, finden Sie bei Bedarf unter [Einrichten der UWP-C++-App (Universals Windows Plattform) zum Anzeigen einer DirectX-Ansicht](https://msdn.microsoft.com/library/windows/apps/hh465077).

Wenn diese Ereignisse während der Ausführung der App ausgelöst werden, aktualisieren die Handler die in den privaten Feldern definierten Zustandsinformationen der Controller.

Als Erstes füllen wir die Ereignishandler für die Maus und den Toucheingabezeiger auf. Im ersten Ereignishandler (**OnPointerPressed()**), rufen wir die X- und Y-Koordinaten des Zeigers vom [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Objekt ab, das die Anzeige verwaltet, wenn der Benutzer im Bereich des Blickcontrollers mit der Maus klickt oder den Bildschirm berührt.

**OnPointerPressed**

```cpp
void MoveLookController::OnPointerPressed(
_In_ CoreWindow^ sender,
_In_ PointerEventArgs^ args)
{
    // Get the current pointer position.
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    auto device = args->CurrentPoint->PointerDevice;
    auto deviceType = device->PointerDeviceType;
    if ( deviceType == PointerDeviceType::Mouse )
    {
        // Action, Jump, or Fire
    }

    // Check  if this pointer is in the move control.
    // Change the values  to percentages of the preferred screen resolution.
    // You can set the x value to <preferred resolution> * <percentage of width>
    // for example, ( position.x < (screenResolution.x * 0.15) ).

    if (( position.x < 300 && position.y > 380 ) && ( deviceType != PointerDeviceType::Mouse ))
    {
        if ( !m_moveInUse ) // if no pointer is in this control yet
        {
            // Process a DPad touch down event.
            m_moveFirstDown = position;                 // Save the location of the initial contact.
            m_movePointerPosition = position;
            m_movePointerID = pointerID;                // Store the id of the pointer using this control.
            m_moveInUse = TRUE;
        }
    }
    else // This pointer must be in the look control.
    {
        if ( !m_lookInUse ) // If no pointer is in this control yet...
        {
            m_lookLastPoint = position;                         // save the point for later move
            m_lookPointerID = args->CurrentPoint->PointerId;  // store the id of pointer using this control
            m_lookLastDelta.x = m_lookLastDelta.y = 0;          // these are for smoothing
            m_lookInUse = TRUE;
        }
    }
}
```

Dieser Ereignishandler überprüft, ob es sich beim Zeiger um die Maus handelt (da in diesem Beispiel sowohl Maus als auch Toucheingabe unterstützt werden) und ob sich der Zeiger im Bereich des Bewegungscontrollers befindet. Treffen beide Kriterien zu, überprüft er, ob der Zeiger gerade erst gedrückt wurde (d. h. ob dieses Click-Ereignis nicht mit einer vorherigen Bewegungs- oder Blickeingabe zusammenhängt), indem er testet, ob **m\_moveInUse** auf „false“ festgelegt ist. Ist dies der Fall, erfasst der Handler den entsprechenden Punkt im Bereich des Bewegungscontrollers und legt **m\_moveInUse** auf „true“ fest, damit er die Startposition der Eingabeinteraktion für den Bewegungscontroller bei einem erneuten Aufruf nicht überschreibt. Außerdem aktualisiert er die Zeiger-ID des Bewegungscontrollers mit der ID des aktuellen Zeigers.

Wenn es sich beim Zeiger um die Maus handelt oder der Toucheingabezeiger sich nicht im Bereich des Bewegungscontrollers befindet, muss er sich im Bereich des Blickcontrollers befinden. Der Handler legt **m\_lookLastPoint** auf die aktuelle Position fest, an der der Benutzer die Maustaste gedrückt oder den Bildschirm berührt und gedrückt hat, setzt die Differenz (Delta) zurück und aktualisiert die Zeiger-ID des Blickcontrollers mit der aktuellen Zeiger-ID. Außerdem legt er den Zustand des Blickcontrollers auf „Aktiv“ fest.

**OnPointerMoved**

```cpp
void MoveLookController::OnPointerMoved(
    _In_ CoreWindow ^sender,
    _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2(args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y);

    // Decide which control this pointer is operating.
    if (pointerID == m_movePointerID)           // This is the move pointer.
    {
        // Move control
        m_movePointerPosition = position;       // Save the current position.

    }
    else if (pointerID == m_lookPointerID)      // This is the look pointer.
    {
        // Look control

        DirectX::XMFLOAT2 pointerDelta;
        pointerDelta.x = position.x - m_lookLastPoint.x;        // How far did pointer move
        pointerDelta.y = position.y - m_lookLastPoint.y;

        DirectX::XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x * ROTATION_GAIN;   // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y * ROTATION_GAIN;

        m_lookLastPoint = position;                     // Save for the next time through.

                                                        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;                     // Mouse y increases down, but pitch increases up.
        m_yaw -= rotationDelta.x;                       // Yaw is defined as CCW around the y-axis.

                                                        // Limit the pitch to straight up or straight down.
        m_pitch = (float)__max(-DirectX::XM_PI / 2.0f, m_pitch);
        m_pitch = (float)__min(+DirectX::XM_PI / 2.0f, m_pitch);
    }
}
```

Der **OnPointerMoved**-Ereignishandler wird bei jeder Bewegung des Zeigers ausgelöst (in diesem Fall, wenn ein Toucheingabezeiger bewegt oder der Mauszeiger mit gedrückter linker Maustaste bewegt wird). Ist die Zeiger-ID mit der Zeiger-ID des Bewegungscontrollers identisch, ist es der Bewegungszeiger. Andernfalls wird überprüft, ob es sich beim aktiven Zeiger um den Zeiger des Blickcontrollers handelt.

Wenn es sich um den Zeiger des Bewegungscontrollers handelt, wird nur die Zeigerposition aktualisiert. Die Position wird weiter aktualisiert, solange das [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276)-Ereignis ausgelöst wird, da die endgültige Position mit der ersten Position verglichen werden soll, die mit dem **OnPointerPressed**-Ereignishandler erfasst wurde.

Wenn es sich um den Zeiger des Blickcontrollers handelt, wird es etwas komplizierter. Wir müssen einen neuen Blickpunkt berechnen und die Kamera auf diesen Punkt ausrichten. Dazu berechnen wir die Differenz zwischen dem letzten Blickpunkt und der aktuellen Bildschirmposition und multiplizieren sie mit dem Skalierungsfaktor, den wir einstellen können, um die Blickbewegungen in Bezug zur Distanz der Bildschirmbewegung zu verkleinern oder zu vergrößern. Anhand dieses Werts berechnen wir den Neigungswinkel und den Schwenkwinkel.

Abschließend müssen wir die Bewegungs- oder Blickcontrollerverhalten deaktivieren, wenn der Spieler aufhört, die Maus zu bewegen oder den Bildschirm zu berühren. Dazu wird der **OnPointerReleased**-Ereignishandler verwendet. Diesen Handler rufen wir auf, wenn [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) ausgelöst wird, um **m\_moveInUse** oder **m\_lookInUse** auf „false“ festzulegen, die Schwenkbewegung der Kamera zu deaktivieren und die Zeiger-ID auf Null festzulegen.

**OnPointerReleased**

```cpp
void MoveLookController::OnPointerReleased(
_In_ CoreWindow ^sender,
_In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );


    if ( pointerID == m_movePointerID )    // This was the move pointer.
    {
        m_moveInUse = FALSE;
        m_movePointerID = 0;
    }
    else if (pointerID == m_lookPointerID ) // This was the look pointer.
    {
        m_lookInUse = FALSE;
        m_lookPointerID = 0;
    }
}
```

Damit haben wir alle Touchscreenereignisse behandelt. Als Nächstes behandeln wir die Tasteneingabeereignisse für einen tastaturbasierten Bewegungscontroller.

**OnKeyDown**

```cpp
void MoveLookController::OnKeyDown(
                                   __in CoreWindow^ sender,
                                   __in KeyEventArgs^ args )
{
    Windows::System::VirtualKey Key;
    Key = args->VirtualKey;

    // Figure out the command from the keyboard.
    if ( Key == VirtualKey::W )     // Forward
        m_forward = true;
    if ( Key == VirtualKey::S )     // Back
        m_back = true;
    if ( Key == VirtualKey::A )     // Left
        m_left = true;
    if ( Key == VirtualKey::D )     // Right
        m_right = true;
}
```

Solange eine dieser Tasten gedrückt wird, legt der Ereignishandler den entsprechenden Bewegungszustand auf „true“ fest.

**OnKeyUp**

```cpp
void MoveLookController::OnKeyUp(
                                 __in CoreWindow^ sender,
                                 __in KeyEventArgs^ args)
{
    Windows::System::VirtualKey Key;
    Key = args->VirtualKey;

    // Figure out the command from the keyboard.
    if ( Key == VirtualKey::W )     // forward
        m_forward = false;
    if ( Key == VirtualKey::S )     // back
        m_back = false;
    if ( Key == VirtualKey::A )     // left
        m_left = false;
    if ( Key == VirtualKey::D )     // right
        m_right = false;
}
```

Wenn die Taste losgelassen wird, legt der Ereignishandler den Zustand wieder auf „false“ fest. Wenn **Update** aufgerufen wird, überprüft der Ereignishandler diese Bewegungszustände und bewegt die Kamera entsprechend. Dies ist etwas einfacher als die Implementierung für Fingereingabe.

## Initialisieren der Fingereingabesteuerungen und des Controllerzustands


Jetzt verbinden wir die Ereignisse und initialisieren alle Controllerzustandsfelder.

**Initialize**

```cpp
void MoveLookController::Initialize( _In_ CoreWindow^ window )
{

    // Opt in to recieve touch/mouse events.
    window->PointerPressed += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

    window->PointerMoved += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

    window->PointerReleased += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);

    window->CharacterReceived +=
    ref new TypedEventHandler<CoreWindow^, CharacterReceivedEventArgs^>(this, &MoveLookController::OnCharacterReceived);

    window->KeyDown += 
    ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

    window->KeyUp += 
    ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);

    // Initialize the state of the controller.
    m_moveInUse = FALSE;                // No pointer is in the Move control.
    m_movePointerID = 0;

    m_lookInUse = FALSE;                // No pointer is in the Look control.
    m_lookPointerID = 0;

    //  Need to init this as it is reset every frame.
    m_moveCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

    SetOrientation( 0, 0 );             // Look straight ahead when the app starts.

}
```

Die **Initialize**-Methode akzeptiert als Parameter einen Verweis auf die [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Instanz der App und registriert die von uns entwickelten Ereignishandler für die entsprechenden Ereignisse in dieser **CoreWindow**-Instanz. Sie initialisiert die Bewegungs- und Blickzeiger-IDs, legt den Befehlsvektor für die Touchscreen-Bewegungscontrollerimplementierung auf Null fest und richtet die Kameraansicht gerade nach vorn aus, wenn die App gestartet wird.

## Abrufen und Festlegen der Position und Ausrichtung der Kamera


Als Nächstes definieren wir einige Methoden zum Abrufen und Festlegen der Kameraposition in Bezug zum Viewport.

```cpp
void MoveLookController::SetPosition( _In_ DirectX::XMFLOAT3 pos )
{
    m_position = pos;
}

// Accessor to set the position of the controller.
void MoveLookController::SetOrientation( _In_ float pitch, _In_ float yaw )
{
    m_pitch = pitch;
    m_yaw = yaw;
}

// Returns the position of the controller object.
DirectX::XMFLOAT3 MoveLookController::get_Position()
{
    return m_position;
}

// Returns the point at which the camera controller is facing.
DirectX::XMFLOAT3 MoveLookController::get_LookPoint()
{
    float y = sinf(m_pitch);        // Vertical
    float r = cosf(m_pitch);        // In the plane
    float z = r*cosf(m_yaw);        // Fwd-back
    float x = r*sinf(m_yaw);        // Left-right
    DirectX::XMFLOAT3 result(x,y,z);
    result.x += m_position.x;
    result.y += m_position.y;
    result.z += m_position.z;

    // Return m_position + DirectX::XMFLOAT3(x, y, z);
    return result;
}
```

## Aktualisieren der Zustandsinformationen der Controller


Jetzt führen wir die Berechnungen aus, mit denen die in **m\_movePointerPosition** erfassten Zeigerkoordinaten in neue Koordinaten für das Spielwelt-Koordinatensystem konvertiert werden. Die App ruft diese Methode bei jeder Aktualisierung der Hauptschleife auf. Daher berechnen wir an dieser Stelle die neuen Informationen zur Blickpunktposition, die an die App übergeben werden sollen, um die Ansichtsmatrix vor der Projektion in den Viewport zu aktualisieren.

```cpp
void MoveLookController::Update(CoreWindow ^window)
{
    // Check for input from the Move control.
    if (m_moveInUse)
    {
        DirectX::XMFLOAT2 pointerDelta(m_movePointerPosition);
        pointerDelta.x -= m_moveFirstDown.x;
        pointerDelta.y -= m_moveFirstDown.y;

        // Figure out the command from the touch-based virtual joystick.
        if (pointerDelta.x > 16.0f)      // Leave 32 pixel-wide dead spot for being still.
            m_moveCommand.x = 1.0f;
        else
            if (pointerDelta.x < -16.0f)
            m_moveCommand.x = -1.0f;

        if (pointerDelta.y > 16.0f)      // Joystick y is up, so change sign.
            m_moveCommand.y = -1.0f;
        else
            if (pointerDelta.y < -16.0f)
            m_moveCommand.y = 1.0f;
    }

    // Poll our state bits that are set by the keyboard input events.
    if (m_forward)
        m_moveCommand.y += 1.0f;
    if (m_back)
        m_moveCommand.y -= 1.0f;

    if (m_left)
        m_moveCommand.x -= 1.0f;
    if (m_right)
        m_moveCommand.x += 1.0f;

    if (m_up)
        m_moveCommand.z += 1.0f;
    if (m_down)
        m_moveCommand.z -= 1.0f;

    // Make sure that 45 degree cases are not faster.
    DirectX::XMFLOAT3 command = m_moveCommand;
    DirectX::XMVECTOR vector;
    vector = DirectX::XMLoadFloat3(&command);

    if (fabsf(command.x) > 0.1f || fabsf(command.y) > 0.1f || fabsf(command.z) > 0.1f)
    {
        vector = DirectX::XMVector3Normalize(vector);
        DirectX::XMStoreFloat3(&command, vector);
    }
    

    // Rotate command to align with our direction (world coordinates).
    DirectX::XMFLOAT3 wCommand;
    wCommand.x = command.x*cosf(m_yaw) - command.y*sinf(m_yaw);
    wCommand.y = command.x*sinf(m_yaw) + command.y*cosf(m_yaw);
    wCommand.z = command.z;

    // Scale for sensitivity adjustment.
    wCommand.x = wCommand.x * MOVEMENT_GAIN;
    wCommand.y = wCommand.y * MOVEMENT_GAIN;
    wCommand.z = wCommand.z * MOVEMENT_GAIN;

    // Our velocity is based on the command.
    // Also note that y is the up-down axis. 
    DirectX::XMFLOAT3 Velocity;
    Velocity.x = -wCommand.x;
    Velocity.z = wCommand.y;
    Velocity.y = wCommand.z;

    // Integrate
    m_position.x += Velocity.x;
    m_position.y += Velocity.y;
    m_position.z += Velocity.z;

    // Clear movement input accumulator for use during the next frame.
    m_moveCommand = DirectX::XMFLOAT3(0.0f, 0.0f, 0.0f);

}
```

Damit die Bewegung nicht "zittert", wenn der Spieler den fingereingabebasierten Bewegungscontroller verwendet, legen wir einen virtuellen inaktiven Bereich mit einem Durchmesser von 32 Pixel um den Zeiger fest. Außerdem fügen wir die Geschwindigkeit hinzu, die sich aus dem Befehlswert plus einer Bewegungsrate berechnet. (Sie können dieses Verhalten wie gewünscht anpassen, um die Bewegungsrate basierend auf der Distanz, um die der Zeiger im Bereich des Bewegungscontrollers bewegt wird, zu erhöhen oder zu verringern.)

Beim Berechnen der Geschwindigkeit setzen wir außerdem die von den Bewegungs- und Blickcontrollern empfangenen Koordinaten in die Bewegung des tatsächlichen Blickpunkts um, die wir an die Methode zum Berechnen der Ansichtsmatrix für die Szene senden. Als Erstes invertieren wir die X-Koordinate, da sich der Blickpunkt in der Szene in entgegengesetzter Richtung dreht (was die Drehung einer Kamera um ihre Mittelachse simuliert), wenn wir den Blickcontroller per Mausklick oder Toucheingabe nach links oder rechts bewegen. Anschließend vertauschen wir die Y- und Z-Achse, da eine Aufwärts-/Abwärtsbewegung mittels Taste oder Toucheingabe (interpretiert als Y-Achsenverhalten) mit dem Bewegungscontroller in eine Kameraaktion umgesetzt werden soll, durch die der Blickpunkt in den oder aus dem Bildschirm (Z-Achse) bewegt wird.

Die endgültige Position des Blickpunkts für den Spieler ist die letzte Position plus die berechnete Geschwindigkeit. Dies ist die Position, die der Renderer liest, wenn er die **get\_Position**-Methode aufruft (wahrscheinlich während der Einrichtung für die einzelnen Frames). Danach setzen wir den Bewegungsbefehl auf Null zurück.

## Aktualisieren der Ansichtsmatrix mit der neuen Kameraposition


Wir können eine Koordinate des Szenenbereichs abrufen, auf die die Kamera ausgerichtet ist und die jedes Mal aktualisiert wird, wenn die App dazu angewiesen wird (z. B. alle 60 Sekunden in der Hauptschleife der App). Dieser Pseudocode zeigt das Aufrufverhalten, das Sie implementieren können:

```cpp
myMoveLookController->Update( m_window );   

// Update the view matrix based on the camera position.
myFirstPersonCamera->SetViewParameters(
                 myMoveLookController->get_Position(),       // Point we are at
                 myMoveLookController->get_LookPoint(),      // Point to look towards
                 DirectX::XMFLOAT3( 0, 1, 0 )                   // Up-vector
                 ); 
```

Herzlichen Glückwunsch! Sie haben einfache Bewegungs-/Blicksteuerungen für Touchscreens und Tastatur-/Mauseingabe in Ihrem Spiel implementiert.

> **Hinweis**  
Dieser Artikel ist für Windows 10-Entwickler gedacht, die Apps für die universelle Windows-Plattform (UWP) schreiben. Wenn Sie für Windows 8.x oder Windows Phone 8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

 

 







<!--HONumber=Jun16_HO4-->


