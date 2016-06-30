---
author: mtoepke
title: "Hinzufügen von visuellem Inhalt zum Marble Maze-Beispiel"
description: "In diesem Dokument wird beschrieben, wie das Spiel Marble Maze Direct3D und Direct2D in der App-Umgebung der universellen Windows Plattform verwendet wird, sodass Sie die Muster erlernen und anpassen können, wenn Sie mit Ihrem eigenen Spielinhalt arbeiten."
ms.assetid: 6e43422e-e1a1-b79e-2c4b-7d5b4fa88647
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 16eb493595bbac4f15ed0e755258104693dfba05

---

# Hinzufügen von visuellem Inhalt zum Marble Maze-Beispiel


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


In diesem Dokument wird beschrieben, wie das Spiel Marble Maze Direct3D und Direct2D in der UWP-App-Umgebung (Universelle Windows-Plattform) verwendet, sodass Sie die Muster erlernen und anpassen können, wenn Sie mit Ihrem eigenen Spielinhalt arbeiten. Informationen dazu, wie visuelle Spielkomponenten in die Anwendungsgesamtstruktur von Marble Maze passen, finden Sie unter [Marble Maze-Anwendungsstruktur](marble-maze-application-structure.md).

Bei der Entwicklung der visuellen Aspekte von Marble Maze haben wir die folgenden grundlegenden Schritte ausgeführt:

1.  Erstellen eines Basisframeworks zur Initialisierung der Direct3D- und Direct2D-Umgebungen.
2.  Verwenden von Bild- und Modellbearbeitungsprogrammen zum Entwerfen der 2-D- und 3-D-Objekte, die im Spiel auftauchen.
3.  Sicherstellen, dass die 2-D- und 3-D-Objekte ordnungsgemäß geladen und im Spiel angezeigt werden.
4.  Integrieren von Vertex- und Pixelshadern zur Verbesserung der visuellen Qualität der Spielobjekte.
5.  Integrieren der Spiellogik, beispielsweise Animationen und Benutzereingabe.

Ferner haben wir uns darauf konzentriert, erst die 3-D-Objekte und dann die 2-D-Objekte hinzuzufügen. Beispielsweise haben wir den Schwerpunkt zunächst auf die grundlegende Spiellogik gelegt, bevor wir das Menüsystem und den Timer hinzugefügt haben.

Darüber hinaus mussten wir einige dieser Schritte während des Entwicklungsprozesses mehrmals durchlaufen. Beim Vornehmen von Änderungen an den Gitter- und Labyrinthmodellen mussten wir beispielsweise auch einen Teil des Shader-Codes ändern, der diese Modelle unterstützt.

> **Hinweis**  Den Beispielcode für dieses Dokument finden Sie im [DirectX-Beispielspiel Marble Maze](http://go.microsoft.com/fwlink/?LinkId=624011).

 
Es folgen einige wichtige Punkte, die in diesem Dokument erläutert werden und die beim Arbeiten mit DirectX und visuellen Spielinhalten relevant sind, und zwar beim Initialisieren der DirectX-Grafikbibliotheken, beim Laden von Szenenressourcen und beim Aktualisieren und Rendern der Szene:

-   Das Hinzufügen von Spielinhalt umfasst normalerweise viele Schritte. Für diese Schritte ist oftmals auch eine Iteration erforderlich. Spielentwickler konzentrieren sich oftmals zunächst auf das Hinzufügen von 3-D-Spielinhalt und anschließend auf das Hinzufügen von 2-D-Inhalt.
-   Erreichen Sie mehr Kunden, und liefern Sie ihnen eine großartiges Spielerlebnis, indem Sie möglichst viele Grafikhardwarekomponenten unterstützen.
-   Nehmen Sie eine saubere Trennung der Entwurfszeit- und Laufzeitformate vor. Strukturieren Sie Ihre Entwurfszeitobjekte, um die Flexibilität zu maximieren und schnelle Inhaltsiterationen zu ermöglichen. Formatieren und komprimieren Sie die Objekte so, dass sie zur Laufzeit so effizient wie möglich geladen und gerendert werden.
-   Die Erstellung der Direct3D- und Direct2D-Geräte in einer UWP-App ähnelt weitestgehend der Vorgehensweise für eine klassische Windows-Desktop-App. Ein wichtiger Unterschied besteht in der Art und Weise der Zuordnung der Swapchain zum Ausgabefenster.
-   Stellen Sie beim Entwickeln des Spiels sicher, dass das von Ihnen ausgewählte Gitterformat die wichtigen Szenarien unterstützt. Wenn für Ihr Spiel beispielsweise eine Kollision erforderlich ist, stellen Sie sicher, dass Sie Kollisionsdaten aus Ihren Gittern abrufen.
-   Separieren Sie die Spiellogik von der Renderlogik, indem Sie zunächst alle Szenenobjekte aktualisieren, bevor Sie sie rendern.
-   Normalerweise zeichnen Sie zunächst die 3-D-Szenenobjekte und anschließend die 2-D-Objekte, die vor der Szene angezeigt werden.
-   Synchronisieren Sie die Zeichnung mit der vertikalen Austastung, um sicherzustellen, dass das Spiel keine Zeit für das Zeichen von Frames verwendet, die letztendlich nie auf dem Bildschirm angezeigt werden.

## Erste Schritte mit DirectX-Grafiken


Bei der Planung des Spiels für die universelle Windows-Plattform (UWP) – Marble Maze – haben wir uns für C++ und Direct3D 11.1 entschieden, da dies jeweils die beste Wahl zum Erstellen von 3D-Spielen ist, die maximale Kontrolle über das Rendern und hohe Leistung erfordern. DirectX 11.1 unterstützt Hardware von DirectX 9 bis DirectX 11 und kann Sie daher dabei unterstützen, mehr Kunden auf effizientere Art und Weise zu erreichen, da Sie so den Code für frühere DirectX-Versionen nicht neu schreiben müssen.

Marble Maze verwendet Direct3D 11.1 zum Rendern der 3-D-Spielobjekte, und zwar für die Murmel und das Labyrinth. Marble Maze nutzt zudem Direct2D, DirectWrite und die Windows-Bilderstellungskomponente (Windows Imaging Component, WIC) zum Zeichnen der 2-D-Spielobjekte, beispielsweise der Menüs und des Timers. Zuletzt nutzt Marble Maze XAML-Code, um eine App-Leiste bereitzustellen, und Sie können XAML-Steuerelemente hinzufügen.

Die Entwicklung eines Spiels erfordert Planung. Wenn Sie sich noch nicht mit der DirectX-Grafik auskennen, empfehlen wir das Lesen des Themas "Erstellen eines DirectX-Spiels", um sich mit den Grundkonzepten der Erstellung eines UWP-DirectX-Spiels vertraut zu machen. Beim Durchlesen dieses Dokuments und Durchgehen des Marble Maze-Quellcodes können Sie sich auf die folgenden Ressourcen beziehen, um detailliertere Informationen zu DirectX-Grafiken zu erhalten.

-   [Direct3D 11-Grafiken](https://msdn.microsoft.com/library/windows/desktop/ff476080) Beschreibt Direct3D 11, eine leistungsstarke, hardwarebeschleunigte 3D-Grafik-API zum Rendern von 3D-Geometrien auf der Windows-Plattform.
-   [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990) Beschreibt Direct2D, eine hardwarebeschleunigte 2D-Grafik-API, die das Rendern mit hoher Leistung und in hoher Qualität für 2D-Geometrien, Bitmaps und Text bereitstellt.
-   [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) Enthält eine Beschreibung von DirectWrite, womit das Rendern von Text in hoher Qualität unterstützt wird.
-   [Windows-Bilderstellungskomponente](https://msdn.microsoft.com/library/windows/desktop/ee719902) Beschreibt WIC, eine erweiterbare Plattform, die eine untergeordnete API für digitale Bilder bereitstellt.

### Featureebenen

In Direct3D 11 wird ein Paradigma mit der Bezeichnung Featureebenen eingeführt. Eine Featureebene ist ein klar definierter Satz mit GPU-Funktionen. Mithilfe von Featureebenen können Sie Ihr Spiel für die Ausführung mit früheren Versionen von Direct3D-Hardware vorsehen. Marble Maze unterstützt die Featureebene 9.1, da keine erweiterten Features von den höheren Ebenen erforderlich sind. Es wird empfohlen, einen möglichst großen Hardwarebereich zu unterstützen und den Spielinhalt zu skalieren, sodass all Ihre Kunden von einem großartigen Spielerlebnis profitieren können – ganz gleich, ob Sie einen normalen Computer oder Highend-Computer besitzen. Weitere Informationen zu Featureebenen finden Sie unter [Direct3D 11 unter kompatibler Hardware](https://msdn.microsoft.com/library/windows/desktop/ff476872).

## Initialisieren von Direct3D und Direct2D


Ein Gerät repräsentiert die Grafikkarte. Die Erstellung der Direct3D- und Direct2D-Geräte in einer UWP-App ähnelt weitestgehend der Vorgehensweise für eine klassische Windows-Desktop-App. Der wesentliche Unterschied besteht in der Art und Weise, wie Sie die Direct3D-Swapchain mit dem Windowing-System verbinden.

Bei der *DirectX 11- und XAML-App (Universal Windows)* sind einige generische Betriebssystem- und 3D-Renderfunktionen aus den spielspezifischen Funktionen ausgeklammert. Die **DeviceResources**-Klasse ist eine Grundlage für die Verwaltung von Direct3D und Direct2D. Mit der Klasse wird die allgemeine Infrastruktur behandelt, keine spielspezifischen Objekte. Marble Maze definiert die **MarbleMaze**-Klasse für die Behandlung spielspezifischer Objekte. Sie enthält einen Verweis auf ein **DeviceResources**-Objekt, um den Zugriff auf Direct3D und Direct2D zu ermöglichen.

Während der Initialisierung erstellt die **DeviceResources::Initialize**-Methode geräteabhängige Ressourcen und die Direct3D- und Direct2D-Geräte.

```cpp
// Initialize the Direct3D resources required to run. 
void DeviceResources::DeviceResources(CoreWindow^ window, float dpi)
{
    m_window = window;

    CreateDeviceIndependentResources();
    CreateDeviceResources();
    CreateWindowSizeDependentResources();
    SetDpi(dpi);
}
```

Die **DeviceResources**-Klasse separiert diese Funktion, sodass sie leichter reagieren kann, wenn sich die Umgebung ändert. Beispielsweise ruft sie die **CreateWindowSizeDependentResources**-Methode auf, wenn sich die Fenstergröße ändert.

###  Initialisieren der Direct2D-, DirectWrite- und WIC-Factorys

Die **DeviceResources::CreateDeviceIndependentResources**-Methode erstellt die Factorys für Direct2D, DirectWrite und WIC. In DirectX-Grafiken bilden Factorys den Ausgangspunkt zum Erstellen von Grafikressourcen. Marble Maze gibt **D2D1\_FACTORY\_TYPE\_SINGLE\_THREADED** an, weil dies der Ausführung aller Zeichnungen im Hauptthread dient.

```cpp
// These are the resources required independent of hardware. 
void DeviceResources::CreateDeviceIndependentResources()
{
    D2D1_FACTORY_OPTIONS options;
    ZeroMemory(&options, sizeof(D2D1_FACTORY_OPTIONS));

#if defined(_DEBUG)
     // If the project is in a debug build, enable Direct2D debugging via SDK Layers.
    options.debugLevel = D2D1_DEBUG_LEVEL_INFORMATION;
#endif

    DX::ThrowIfFailed(
        D2D1CreateFactory(
            D2D1_FACTORY_TYPE_SINGLE_THREADED,
            __uuidof(ID2D1Factory1),
            &options,
            &m_d2dFactory
            )
        );

    DX::ThrowIfFailed(
        DWriteCreateFactory(
            DWRITE_FACTORY_TYPE_SHARED,
            __uuidof(IDWriteFactory),
            &m_dwriteFactory
            )
        );

    DX::ThrowIfFailed(
        CoCreateInstance(
            CLSID_WICImagingFactory,
            nullptr,
            CLSCTX_INPROC_SERVER,
            IID_PPV_ARGS(&m_wicFactory)
            )
        );
}
```

###  Erstellen der Direct3D- und Direct2D-Geräte

Die **DeviceResources::CreateDeviceResources**-Methode ruft [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) auf, um das Geräteobjekt zu erstellen, das die Direct3D-Grafikkarte repräsentiert. Da Marble Maze die Featureebene 9.1 und höher unterstützt, gibt die **DeviceResources::CreateDeviceResources**-Methode die Ebenen 9.1 bis 11.1 im Array der **\\**-Werte an. Direct3D geht die Liste der Reihe nach durch und stellt die erste verfügbare Featureebene für die App bereit. Demzufolge werden die **D3D\_FEATURE\_LEVEL**-Arrayeinträge vom höchsten zum niedrigsten Wert aufgelistet, sodass die App die höchste Featureebene erhält, die verfügbar ist. Die **DeviceResources::CreateDeviceResources**-Methode erhält das Direct3D 11.1-Gerät durch Abfragen des Direct3D 11-Geräts, das von **D3D11CreateDevice** zurückgegeben wird.

```cpp
// This array defines the set of DirectX hardware feature levels this app will support. 
// Note the ordering should be preserved. 
// Don't forget to declare your application's minimum required feature level in its 
// description.  All applications are assumed to support 9.1 unless otherwise stated.
D3D_FEATURE_LEVEL featureLevels[] = 
{
    D3D_FEATURE_LEVEL_11_1,
    D3D_FEATURE_LEVEL_11_0,
    D3D_FEATURE_LEVEL_10_1,
    D3D_FEATURE_LEVEL_10_0,
    D3D_FEATURE_LEVEL_9_3,
    D3D_FEATURE_LEVEL_9_2,
    D3D_FEATURE_LEVEL_9_1
};

// Create the DX11 API device object, and get a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> context;
DX::ThrowIfFailed(
    D3D11CreateDevice(
        nullptr,                    // Specify null to use the default adapter.
        D3D_DRIVER_TYPE_HARDWARE,
        0,                          // Leave as 0 unless it is a software device.
        creationFlags,              // Optionally, set debug and Direct2D compatibility flags.
        featureLevels,              // A list of feature levels that this app can support.
        ARRAYSIZE(featureLevels),   // The number of entries in the above list.
        D3D11_SDK_VERSION,          // Always set this to D3D11_SDK_VERSION for modern.
        &device,                    // Returns the Direct3D device created.
        &m_featureLevel,            // Returns the feature level of the device created.
        &context                    // Returns the device immediate context.
        )
    );    

// Get the Direct3D 11.1 device by querying the Direct3D 11 device.
DX::ThrowIfFailed(
    device.As(&m_d3dDevice)
    );
```

Anschließend erstellt die **DeviceResources::CreateDeviceResources**-Methode das Direct2D-Gerät. Direct2D verwendet Microsoft DXGI (DirectX Graphics Infrastructure) für die Interoperabilität mit Direct3D. DXGI ermöglicht das Freigeben von Videospeicheroberflächen zwischen Grafiklaufzeiten. Marble Maze verwendet das zugrunde liegende DXGI-Gerät vom Direct3D-Gerät zum Erstellen des Direct2D-Geräts aus der Direct2D-Factory.

```cpp
// Obtain the underlying DXGI device of the Direct3D 11.1 device.
DX::ThrowIfFailed(
    m_d3dDevice.As(&dxgiDevice)
    );

// Obtain the Direct2D device for 2-D rendering.
DX::ThrowIfFailed(
    m_d2dFactory->CreateDevice(dxgiDevice.Get(), &m_d2dDevice)
    );

// And get its corresponding device context object.
DX::ThrowIfFailed(
    m_d2dDevice->CreateDeviceContext(
        D2D1_DEVICE_CONTEXT_OPTIONS_NONE,
        &m_d2dContext
        )
    );
```

```cpp
// Obtain the underlying DXGI device of the Direct3D 11.1 device.
DX::ThrowIfFailed(
    m_d3dDevice.As(&dxgiDevice)
    );

// Obtain the Direct2D device for 2-D rendering.
DX::ThrowIfFailed(
    m_d2dFactory->CreateDevice(dxgiDevice.Get(), &m_d2dDevice)
    );

// And get its corresponding device context object.
DX::ThrowIfFailed(
    m_d2dDevice->CreateDeviceContext(
        D2D1_DEVICE_CONTEXT_OPTIONS_NONE,
        &m_d2dContext
        )
    );
```

Weitere Informationen zu DXGI und zur Interoperabilität zwischen Direct2D und Direct3D finden Sie unter [Übersicht über DXGI](https://msdn.microsoft.com/library/windows/desktop/bb205075) und [Übersicht über die Interoperabilität von Direct2D und Direct3D](https://msdn.microsoft.com/library/windows/desktop/dd370966).

### Zuordnen von Direct3D zur Ansicht

Die **DeviceResources::CreateWindowSizeDependentResources**-Methode erstellt die Grafikressourcen, die von einer bestimmten Fenstergröße abhängig sind, beispielsweise von der Swapchain und den Direct3D- und Direct2D-Renderzielen. Ein wichtiger Aspekt, in dem sich eine DirectX-UWP-App von einer Desktop-App unterscheidet, ist die Art und Weise, wie die Swapchain dem Ausgabefenster zugeordnet wird. Eine Swapchain dient der Anzeige des Puffers, in dem das Gerät rendert, auf dem Monitor. Im Dokument für die Marble Maze-Anwendungsstruktur wird beschrieben, inwiefern sich das Windowing-System für eine UWP-App von einer Desktop-App unterscheidet. Da eine Windows Store-App nicht für **HWND**-Objekte verwendet werden kann, muss Marble Maze die [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559)-Methode zum Zuordnen der Geräteausgabe zur Ansicht verwenden. Im folgenden Beispiel wird der Teil der **DeviceResources::CreateWindowSizeDependentResources**-Methode gezeigt, mit dem die Swapchain erstellt wird.

```cpp
// Obtain the final swap chain for this window from the DXGI factory.
DX::ThrowIfFailed(
    dxgiFactory->CreateSwapChainForCoreWindow(
        m_d3dDevice.Get(),
        reinterpret_cast<IUnknown*>(m_window),
        &swapChainDesc,
        nullptr,    // Allow on all displays.
        &m_swapChain
        )
    );
```

Zur Minimierung des Stromverbrauchs, was vor allem für batteriebetriebene Geräte wie Laptops und Tablets wichtig ist, ruft die **DeviceResources::CreateWindowSizeDependentResources**-Methode die [**IDXGIDevice1::SetMaximumFrameLatency**](https://msdn.microsoft.com/library/windows/desktop/ff471334)-Methode auf, um sicherzustellen, dass das Spiel erst nach der vertikalen Austastung gerendert wird. Die Synchronisierung mit der vertikalen Austastung wird im Abschnitt "Darstellen der Szene" dieses Dokuments detaillierter beschrieben.

```cpp
// Ensure that DXGI does not queue more than one frame at a time. This both reduces  
// latency and ensures that the application will only render after each VSync, minimizing  
// power consumption.
DX::ThrowIfFailed(
    dxgiDevice->SetMaximumFrameLatency(1)
    );
```

Die **DeviceResources::CreateWindowSizeDependentResources**-Methode initialisiert die Grafikressourcen anhand einer Vorgehensweise, die für die meisten Spiele geeignet ist.

> **Hinweis**  Der Begriff *Ansicht* hat in der Windows-Runtime eine andere Bedeutung als in Direct3D. In der Windows-Runtime bezieht sich eine Ansicht auf die Sammlung von Einstellungen zur Benutzeroberfläche für eine App. Dazu gehören auch der Anzeigebereich und das Eingabeverhalten sowie der zum Verarbeiten verwendete Thread. Die benötigte Konfiguration und die benötigten Einstellungen geben Sie beim Erstellen einer Ansicht an. Der Einrichtungsprozess der App-Ansicht wird unter [Marble Maze-Anwendungsstruktur](marble-maze-application-structure.md) beschrieben. In Direct3D hat der Begriff "Ansicht" mehrere Bedeutungen. Zunächst werden anhand einer Ressourcenansicht die Unterressourcen definiert, auf die eine Ressource zugreifen kann. Wenn beispielsweise ein Texturobjekt einer Shader-Ressourcenansicht zugeordnet wird, kann dieser Shader später auf die Textur zugreifen. Ein Vorteil einer Ressourcenansicht besteht darin, dass Daten auf unterschiedliche Art und Weise in verschiedenen Stufen der Rendering-Pipeline interpretiert werden können. Weitere Informationen zu Ressourcenansichten finden Sie unter [Texturansichten (Direct3D 10)](https://msdn.microsoft.com/library/windows/desktop/bb205128). Bei der Verwendung im Kontext einer Ansichtstransformation oder Ansichtstransformationsmatrix bezieht sich der Begriff Ansicht auf den Standort und die Ausrichtung der Kamera. Bei einer Ansichtstransformation werden Objekte in der Welt um die Position und Ausrichtung der Kamera herum neu angeordnet. Weitere Informationen zu Ansichtstransformationen finden Sie unter [Ansichtstransformation (Direct3D 9)](https://msdn.microsoft.com/library/windows/desktop/bb206342). Die Verwendung von Ressourcen- und Matrixansichten in Marble Maze werden in diesem Thema detaillierter beschrieben.

 

## Laden von Szenenressourcen


Marble Maze verwendet die **BasicLoader**-Klasse, die in BasicLoader.h deklariert wird, zum Laden von Texturen und Shadern. Marble Maze verwendet die **SDKMesh**-Klasse zum Laden der 3D-Gitter für die Murmel und das Labyrinth.

Um eine reagierende App zu gewährleisten, lädt Marble Maze die Szenenressourcen asynchron oder im Hintergrund. Während die Objekte im Hintergrund geladen werden, kann das Spiel auf Fensterereignisse reagieren. Dieser Prozess wird in diesem Handbuch unter [Laden von Spielobjekten im Hintergrund](marble-maze-application-structure.md#loading_game_assets) detaillierter erklärt.

###  Laden der 2D-Überlagerung und Benutzeroberfläche

In Marble Maze ist die Überlagerung das Bild, das am oberen Rand des Bildschirms angezeigt wird. Die Überlagerung wird immer vor der Szene angezeigt. In Marble Maze enthält die Überlagerung das Windows-Logo und die Textzeichenfolge "DirectX-Beispielspiel Marble Maze". Die Verwaltung der Überlagerung wird von der **SampleOverlay**-Klasse ausgeführt, die unter SampleOverlay.h definiert ist. Obwohl wir die Überlagerung als Teil der Direct3D-Beispiele verwenden, können Sie diesen Code anpassen, um beliebige Bilder anzuzeigen, die vor Ihrer Szene erscheinen.

Ein wichtiger Aspekt der Überlagerung ist, dass die **SampleOverlay**-Klasse, da sich ihre Inhalte nicht ändern, die zugehörigen Inhalte bei der Initialisierung in einem [**ID2D1Bitmap1**](https://msdn.microsoft.com/library/windows/desktop/hh404349)-Objekt zeichnet bzw. dort speichert. Beim Zeichnen muss die **SampleOverlay**-Klasse lediglich die Bitmap auf dem Bildschirm zeichnen. Auf diese Art und Weise müssen nicht für jeden Frame teure Routinen wie Textzeichnungen ausgeführt werden.

Die Benutzeroberfläche (UI) besteht aus 2-D-Komponenten, beispielsweise Menüs und Heads-up-Anzeigen (HUDs), die vor Ihrer Szene angezeigt werden. Marble Maze definiert die folgenden UI-Elemente:

-   Menüelemente, die dem Benutzer das Starten des Spiels oder das Anzeigen von Bestenlisten ermöglichen.
-   Einen Timer, der drei Sekunden lang abwärts zählt, bevor das Spiel beginnt.
-   Einen Timer, der die verstrichene Spielzeit verfolgt.
-   Eine Tabelle, in der die schnellsten Zeiten aufgelistet werden.
-   Text "Pausiert", der angezeigt wird, wenn das Spiel pausiert wird.

Marble Maze definiert spielspezifische UI-Elemente in UserInterface.h. Marble Maze definiert die **ElementBase**-Klasse als Basistyp für alle UI-Elemente. Die **ElementBase**-Klasse definiert Attribute wie Größe, Position, Ausrichtung und Transparenz eines UI-Elements. Zudem kontrolliert sie, wie Elemente aktualisiert und gerendert werden.

```cpp
class ElementBase
{
public:
    virtual void Initialize() { }
    virtual void Update(float timeTotal, float timeDelta) { }
    virtual void Render() { }

    void SetAlignment(AlignType horizontal, AlignType vertical);
    virtual void SetContainer(const D2D1_RECT_F& container);
    void SetVisible(bool visible);

    D2D1_RECT_F GetBounds();

    bool IsVisible() const { return m_visible; }

protected:
    ElementBase();

    virtual void CalculateSize() { }

    Alignment       m_alignment;
    D2D1_RECT_F     m_container;
    D2D1_SIZE_F     m_size;
    bool            m_visible;
};
```

Durch die Bereitstellung einer allgemeinen Basisklasse für UI-Elemente muss die **UserInterface**-Klasse, mit der die Benutzeroberfläche verwaltet wird, lediglich eine Sammlung von **ElementBase**-Objekten enthalten. Dadurch werden die UI-Verwaltung vereinfacht und ein wiederverwendbarer Benutzeroberflächenmanager bereitgestellt. Marble Maze definiert Typen, die aus **ElementBase** abgeleitet werden und spielspezifische Verhalten implementieren. **HighScoreTable** definiert beispielsweise das Verhalten für die Highscore-Tabelle. Informationen zu diesen Typen finden Sie im Quellcode.

> **Hinweis**  Da Ihnen XAML eine einfachere Erstellung komplexer Benutzeroberflächen ermöglicht, wie sie beispielsweise in Simulations- und Strategiespielen vorkommen, denken Sie darüber nach, für die Definition Ihrer Benutzeroberfläche XAML zu verwenden. Informationen zum Entwickeln einer Benutzeroberfläche in XAML in einem UWP-Spiel finden Sie unter [Erweitern des Beispielspiels (Windows)](tutorial-resources.md). Dieses Dokument bezieht sich auf ein Beispiel für einen DirectX-3D-Shooter.

 

###  Laden von Shadern

Marble Maze verwendet die **BasicLoader::LoadShader**-Methode zum Laden eines Shaders aus einer Datei.

Shader bilden derzeit in Spielen die grundlegende Einheit der GPU-Programmierung. Nahezu die gesamte 3-D-Grafikverarbeitung erfolgt über Shader. Dies schließt die Modelltransformation und Szenenbeleuchtung genauso ein wie komplexere Geometrieverarbeitungen (angefangen von der Figurengestaltung bis hin zu Texturen). Weitere Informationen zum Shader-Programmierungsmodell finden Sie unter [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561).

Marble Maze verwendet Vertex- und Pixel-Shader. Ein Vertex-Shader wird immer für einen Eingabevertex ausgeführt und erzeugt einen Vertex als Ausgabe. Ein Pixel-Shader verwendet numerische Werte, Texturdaten, interpolierte Vertex-Werte und andere Daten, um eine Pixelfarbe als Ausgabe zu erzeugen. Da ein Shader jeweils ein Element transformiert, kann Grafikhardware, die mehrere Shader-Pipelines bereitstellt, Elementgruppen parallel bearbeiten. Die Anzahl paralleler Pipelines, die für die GPU zur Verfügung stehen, kann deutlich größer sein als die Anzahl, die für die CPU verfügbar ist. Daher kann der Durchsatz selbst mit einfachen Shadern deutlich verbessert werden.

Die **MarbleMaze::LoadDeferredResources**-Methode lädt einen Vertex- und einen Pixel-Shader, nachdem die Überlagerung geladen wurde. Die Entwurfstzeitversionen dieser Shader werden in "BasicVertexShader.hlsl" bzw. "BasicPixelShader.hlsl" definiert. Marble Maze wendet diese Shader in der Renderphase sowohl auf die Kugel als auch auf das Labyrinth an.

Das Marble Maze-Projekt beinhaltet sowohl HLSL (Entwurfszeitformat)- als auch CSO (Laufzeitformat)-Versionen der Shader-Dateien. Zur Erstellungszeit verwendet Visual Studio den Effektcompiler "fxc.exe" zum Kompilieren der HLSL-Quelldatei in einen CSO-Binär-Shader. Weitere Informationen zum Effektcompiler-Tool finden Sie unter [Effektcompiler-Tool](https://msdn.microsoft.com/library/windows/desktop/bb232919).

Der Vertex-Shader verwendet die bereitgestellten Modell-, Ansichts- und Projektionsmatrizen zum Umwandeln der Eingabegeometrie. Die Positionsdaten aus der Eingabegeometrie werden umgewandelt und zweimal ausgegeben: einmal im Bildschirmbereich, der zum Rendern benötigt wird, und noch einmal im Raum, um dem Pixel-Shader das Ausführen von Beleuchtungsberechnungen zu ermöglichen. Der Oberflächennormalvektor wird in einen Raum umgewandelt, der auch vom Pixel-Shader für die Beleuchtung verwendet wird. Die Texturkoordinaten werden unverändert an den Pixel-Shader übergeben.

```hlsl
sPSInput main(sVSInput input)
{
    sPSInput output;
    float4 temp = float4(input.pos, 1.0f);
    temp = mul(temp, model);
    output.worldPos = temp.xyz / temp.w;
    temp = mul(temp, view);
    temp = mul(temp, projection);
    output.pos = temp;
    output.tex = input.tex;
    output.norm = mul(float4(input.norm, 0.0f), model).xyz;
    return output;
}
```

Der Pixel-Shader empfängt die Ausgabe des Vertex-Shaders als Eingabe. Dieser Shader führt Beleuchtungsberechnungen durch, um einen auslaufenden Blickpunkt nachzuahmen, der über das Labyrinth schwebt und an der Position der Kugel ausgerichtet ist. Die Beleuchtung ist für die Oberflächen am stärksten, die direkt auf das Licht zeigen. Die diffuse Komponente läuft auf null aus, während die Oberflächennormale eine senkrechte Position zum Licht einnimmt. Zudem nimmt die Umgebungsbeleuchtung ab, während die Normale vom Licht weg zeigt. Punkte, die sich näher an der Kugel befinden (und demzufolge näher an der Mitte des Blickpunkts), werden stärker beleuchtet. Die Beleuchtung wird jedoch für Punkte unterhalb der Kugel moduliert, um einen sanften Schatten zu simulieren. In einer echten Umgebung würde ein Objekt wie die weiße Kugel den Blickpunkt diffus auf andere Objekte in der Szene reflektieren. Dies wird für die Oberflächen angenähert, die im Sichtbereich der hellen Kugelhälfte liegen. Die zusätzlichen Beleuchtungsfaktoren befinden sich im relativen Winkel und Abstand zur Kugel. Die resultierende Pixelfarbe ist eine Zusammenstellung der Stichprobentextur und dem Ergebnis der Beleuchtungsberechnungen.

```hlsl
float4 main(sPSInput input) : SV_TARGET
{
    float3 lightDirection = float3(0, 0, -1);
    float3 ambientColor = float3(0.43, 0.31, 0.24);
    float3 lightColor = 1 - ambientColor;
    float spotRadius = 50;

    // Basic ambient (Ka) and diffuse (Kd) lighting from above.
    float3 N = normalize(input.norm);
    float NdotL = dot(N, lightDirection);
    float Ka = saturate(NdotL + 1);
    float Kd = saturate(NdotL);

    // Spotlight.
    float3 vec = input.worldPos - marblePosition;
    float dist2D = sqrt(dot(vec.xy, vec.xy));
    Kd = Kd * saturate(spotRadius / dist2D);

    // Shadowing from ball.
    if (input.worldPos.z > marblePosition.z)
        Kd = Kd * saturate(dist2D / (marbleRadius * 1.5));

    // Diffuse reflection of light off ball.
    float dist3D = sqrt(dot(vec, vec));
    float3 V = normalize(vec);
    Kd += saturate(dot(-V, N)) * saturate(dot(V, lightDirection))
        * saturate(marbleRadius / dist3D);

    // Final composite.
    float4 diffuseTexture = Texture.Sample(Sampler, input.tex);
    float3 color = diffuseTexture.rgb * ((ambientColor * Ka) + (lightColor * Kd));
    return float4(color * lightStrength, diffuseTexture.a);
}
```

> **Achtung**  Der kompilierte Pixel-Shader enthält 32 arithmetische Anweisungen und 1 Texturanweisung. Dieser Shader sollte auf Desktopcomputern und Tablets im Highend-Bereich gute Ergebnisse liefern. Ein normaler Computer kann diesen Shader jedoch möglicherweise nicht verarbeiten und weiterhin eine interaktive Framerate bereitstellen. Ziehen Sie die typische Hardware Ihrer Zielgruppe in Erwägung, und entwickeln Sie die Shader so, dass sie zu den Hardwarefunktionen passen.

 

Die **MarbleMaze::LoadDeferredResources**-Methode verwendet die **BasicLoader::LoadShader**-Methode zum Laden der Shader. Im folgenden Beispiel wird der Vertex-Shader geladen. Das Laufzeitformat für diesen Shader lautet "BasicVertexShader.cso". Die **m\_vertexShader**-Membervariable ist ein [**ID3D11VertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476641)-Objekt.

```cpp
\loader->LoadShader(
    L"BasicVertexShader.cso",
    layoutDesc,
    ARRAYSIZE(layoutDesc),
    &m_vertexShader,
    &m_inputLayout
    );
```

Die **m\_inputLayout**-Membervariable ist ein [**ID3D11InputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476575)-Objekt. Das Objekt für das Eingabelayout kapselt den Eingabestatus der Eingabeassemblerphase (IA-Phase). Eine Aufgabe der IA-Phase besteht darin, die Effizienz der Shader zu erhöhen. Dazu werden systemgenerierte Werte verwendet, die auch als *Semantik* bezeichnet werden, um nur die Grundtypen oder Vertizes zu verarbeiten, die noch nicht verarbeitet wurden. Verwenden Sie die [**ID3D11Device::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512)-Methode, um ein Eingabelayout aus einem Array mit Eingabeelementbeschreibungen zu erstellen. Das Array enthält ein oder mehrere Eingabeelemente. Jedes Eingabeelement beschreibt dabei ein Vertex-Datenelement von einem Vertex-Puffer. Der vollständige Satz der Eingabeelementbeschreibungen dient der Beschreibung aller Vertex-Datenelemente von sämtlichen Vertex-Puffern, die an die IA-Phase gebunden werden. Im folgenden Beispiel wird die Layoutbeschreibung gezeigt, die von Marble Maze verwendet wird. Die Layoutbeschreibung beschreibt einen Vertex-Puffer, der vier Vertex-Datenelemente enthält. Die wichtigen Teile der einzelnen Einträge im Array sind der semantische Name, das Datenformat und das Byte-Offset. Das **POSITION**-Element gibt beispielsweise die Vertex-Position im Objektraum an. Es startet beim Byte-Offset 0 und enthält drei Gleitkommakomponenten (für insgesamt 12 Byte). Das **NORMAL**-Element gibt den Normalvektor an. Es startet beim Byte-Offset 12, da es im Layout direkt nach **POSITION** erscheint, wofür 12 Byte erforderlich sind. Das **NORMAL**-Element enthält eine nicht signierte ganze Zahl mit vier Komponenten und 32 Bit.

```cpp
D3D11_INPUT_ELEMENT_DESC layoutDesc[] = 
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TEXCOORD",  0, DXGI_FORMAT_R32G32_FLOAT,   0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TANGENT", 0, DXGI_FORMAT_R32G32B32_FLOAT,  0, 32, D3D11_INPUT_PER_VERTEX_DATA, 0 }, 
};
m_vertexStride = 44; // You must set this to match the size of layoutDesc above.
```

Vergleichen Sie das Eingabelayout mit der vom Vertex-Shader definierten **sVSInput**-Struktur, wie im folgenden Beispiel dargestellt. Die **sVSInput**-Struktur definiert die Elemente **POSITION**, **NORMAL** und **TEXCOORD0**. Die DirectX-Laufzeit ordnet jedes Element im Layout der Eingabestruktur zu, die vom Shader definiert wird.

```hlsl
struct sVSInput
{
    float3 pos : POSITION;
    float3 norm : NORMAL;
    float2 tex : TEXCOORD0;
};

struct sPSInput
{
    float4 pos : SV_POSITION;
    float3 norm : NORMAL;
    float2 tex : TEXCOORD0;
    float3 worldPos : TEXCOORD1;
};

sPSInput main(sVSInput input)
{
    sPSInput output;
    float4 temp = float4(input.pos, 1.0f);
    temp = mul(temp, model);
    output.worldPos = temp.xyz / temp.w;
    temp = mul(temp, view);
    temp = mul(temp, projection);
    output.pos = temp;
    output.tex = input.tex;
    output.norm = mul(float4(input.norm, 0.0f), model).xyz;
    return output;
}
```

Im Dokument [Semantik](https://msdn.microsoft.com/library/windows/desktop/bb509647) werden alle verfügbaren Semantiken detaillierter beschrieben.

> **Hinweis**  In einem Layout können Sie zusätzliche Komponenten angeben, die nicht dazu verwendet werden, mehreren Shadern die gemeinsame Verwendung desselben Layouts zu ermöglichen. Beispielsweise wird das **TANGENT**-Element nicht vom Shader verwendet. Sie können das **TANGENT**-Element verwenden, wenn Sie mit Techniken wie der Normalzuordnung experimentieren möchten. Mithilfe der Normalzuordnung, die auch als Bumpmapping bezeichnet wird, können Sie auf den Oberflächen von Objekten den Bumpeffekt erzeugen. Weitere Informationen zum Bumpmapping finden Sie unter [Bumpmapping (Direct3D 9)](https://msdn.microsoft.com/library/windows/desktop/bb172379).

 

Weitere Informationen zum Status der Eingabeassembly-Phase finden Sie unter [Eingabeassembler-Phase](https://msdn.microsoft.com/library/windows/desktop/bb205116) und [Erste Schritte mit der Eingabeassembler-Phase](https://msdn.microsoft.com/library/windows/desktop/bb205117).

Der Prozess der Verwendung von Vertex- und Pixel-Shadern zum Rendern der Szene wird später in diesem Dokument im Abschnitt [Rendern der Szene](#rendering_the_scene) beschrieben.

### Erstellen des Konstantenpuffers

Der Direct3D-Puffer gruppiert eine Sammlung von Daten. Ein Konstantenpuffer ist ein Puffertyp, mit dessen Hilfe Sie Daten an Shader übergeben können. Marble Maze verwendet einen Konstantenpuffer, um die Modellansicht (oder Weltansicht) sowie die Projektmatrizen für das aktive Szenenobjekt aufzunehmen.

Im folgenden Beispiel wird gezeigt, wie die **MarbleMaze::LoadDeferredResources**-Methode einen Konstantenpuffer erstellt, der später die Matrixdaten aufnimmt. In dem Beispiel wird eine **D3D11\_BUFFER\_DESC**-Struktur erstellt, die das **D3D11\_BIND\_CONSTANT\_BUFFER**-Kennzeichen zum Angeben der Verwendung als Konstantenpuffer verwendet. Anschließend übergibt dieses Beispiel diese Struktur an die [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501)-Methode. Die **M\_constantBuffer**-Variable ist ein [**ID3D11Buffer**](https://msdn.microsoft.com/library/windows/desktop/ff476351)-Objekt.

```cpp
// Create the constant buffer for updating model and camera data.
D3D11_BUFFER_DESC constantBufferDesc = {0};
constantBufferDesc.ByteWidth           = ((sizeof(ConstantBuffer) + 15) / 16) * 16; // Multiple of 16 bytes
constantBufferDesc.Usage               = D3D11_USAGE_DEFAULT;
constantBufferDesc.BindFlags           = D3D11_BIND_CONSTANT_BUFFER;
constantBufferDesc.CPUAccessFlags      = 0;
constantBufferDesc.MiscFlags           = 0;
// This will not be used as a structured buffer, so this parameter is ignored.
constantBufferDesc.StructureByteStride = 0;

DX::ThrowIfFailed(
    m_d3dDevice->CreateBuffer(
        &constantBufferDesc,
        nullptr,             // Leave the buffer uninitialized.
        &m_constantBuffer
        )
    );
```

Die **MarbleMaze::Update**-Methode aktualisiert später **ConstantBuffer**-Objekte, und zwar ein Objekt für die Kugel und ein Objekt für das Labyrinth. Anschließend bindet die **MarbleMaze::Render**-Methode die einzelnen **ConstantBuffer**-Objekte an den Konstantenpuffer, bevor die einzelnen Objekte gerendert werden. Im folgenden Beispiel wird die **ConstantBuffer**-Struktur gezeigt, die sich in "MarbleMaze.h" befindet.

```cpp
// Describes the constant buffer that draws the meshes.
struct ConstantBuffer
{
    float4x4 model;
    float4x4 view;
    float4x4 projection;

    float3 marblePosition;
    float marbleRadius;
    float lightStrength;
};
```

Um die Zuordnung von Konstantenpuffern zum Shader-Code besser zu verstehen, vergleichen Sie die **ConstantBuffer**-Struktur mit dem **SimpleConstantBuffer**-Konstantenpuffer, der durch den Vertex-Shader in „BasicVertexShader.hlsl“ definiert wird:

```hlsl
cbuffer ConstantBuffer : register(b0)
{
    matrix model;
    matrix view;
    matrix projection;
    float3 marblePosition;
    float marbleRadius;
    float lightStrength;
};
```

Das Layout der **ConstantBuffer**-Struktur stimmt mit dem **cbuffer**-Objekt überein. Die **cbuffer**-Variable gibt das Register b0 an. Demnach werden die Daten des Konstantenpuffers im Register 0 gespeichert. Die **MarbleMaze::Render**-Methode gibt das Register 0 an, wenn sie den Konstantenpuffer aktiviert. Auf diesen Prozess wird später in diesem Dokument detaillierter eingegangen.

Weitere Informationen zu Konstantenpuffern finden Sie unter [Einführung in Puffer in Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476898). Weitere Informationen zum Schlüsselwort Register finden Sie unter [**register**](https://msdn.microsoft.com/library/windows/desktop/dd607359).

###  Laden von Gittern

Marble Maze verwendet SDK-Mesh als Laufzeitformat, da dieses Format eine grundlegende Methode zum Laden von Gitterdaten für Beispielanwendungen bereitstellt. Für die Produktionsverwendung sollten Sie ein Gitterformat nutzen, das die spezifischen Anforderungen Ihres Spiels erfüllt.

Die **MarbleMaze::LoadDeferredResources**-Methode lädt die Gitterdaten nach dem Laden der Vertex- und Pixel-Shader. Bei einem Gitter handelt es sich um eine Sammlung von Vertex-Daten, die oftmals Informationen wie Positionen, Normale, Farben, Materialien und Texturkoordinaten enthält. Gitter werden normalerweise in 3-D-Erstellungssoftware erstellt und in Dateien verwaltet, die vom Anwendungscode separiert sind. Die Kugel und das Labyrinth sind zwei Beispiele für Gitter, die im Spiel verwendet werden.

Marble Maze verwendet die **SDKMesh**-Klasse zum Verwalten von Gittern. Diese Klasse wird in "SDKMesh.h" deklariert. **SDKMesh** stellt Methoden zum Laden, Rendern und Löschen von Gitterdaten bereit.

> **Wichtig**  Marble Maze verwendet das SDK-Mesh-Format, und die Bereitstellung der **SDKMesh**-Klasse dient nur der Veranschaulichung. Obwohl das SDK-Mesh-Format hilfreich zum Lernen und Erstellen von Prototypen ist, handelt es sich dabei um ein sehr einfaches Format, das die Anforderungen der meisten Spielentwicklungen möglicherweise nicht erfüllt. Es wird empfohlen, ein Gitterformat zu verwenden, das die spezifischen Anforderungen Ihres Spiels erfüllt.

 

Im folgenden Beispiel wird veranschaulicht, wie die **MarbleMaze::LoadDeferredResources**-Methode die **SDKMesh::Create**-Methode zum Laden von Gitterdaten für das Labyrinth und die Kugel verwendet.

```cpp
// Load the meshes.
DX::ThrowIfFailed(
    m_mazeMesh.Create(
        m_d3dDevice.Get(),
        L"Media\\Models\\maze1.sdkmesh",
        false
        )
    );

DX::ThrowIfFailed(
    m_marbleMesh.Create(
        m_d3dDevice.Get(),
        L"Media\\Models\\marble2.sdkmesh",
        false
        )
    );
```

###  Laden von Kollisionsdaten

Obwohl der Schwerpunkt dieses Abschnitts nicht darauf liegt, wie Marble Maze die physikalische Simulation zwischen der Kugel und dem Labyrinth implementiert, gilt es zu beachten, dass die Gittergeometrie für das physikalische System beim Laden der Gitter gelesen wird.

```cpp
// Extract mesh geometry for the physics system.
DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_walls",
        m_collision.m_wallTriList
        )
    );

DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_Floor",
        m_collision.m_groundTriList
        )
    );

DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_floorSides",
        m_collision.m_floorTriList
        )
    );

m_physics.SetCollision(&m_collision);
float radius = m_marbleMesh.GetMeshBoundingBoxExtents(0).x / 2;
m_physics.SetRadius(radius);
```

Die Methode zum Laden von Kollisionsdaten ist in hohem Maße vom verwendeten Laufzeitformat abhängig. Weitere Informationen dazu, wie Marble Maze die Kollisionsgeometrie aus einer SDK-Mesh-Datei lädt, finden Sie in der **MarbleMaze::ExtractTrianglesFromMesh**-Methode im Quellcode.

## Aktualisieren des Spielzustands


Marble Maze separiert die Spiellogik von der Renderlogik, indem zunächst alle Szenenobjekte vor dem Rendern aktualisiert werden.

Im Dokument "Marble Maze-Anwendungsstruktur" wird die Hauptspielschleife beschrieben. Die Aktualisierung der Szene, die Bestandteil der Spielschleife ist, erfolgt nach dem Verarbeiten von Windows-Ereignissen und Eingaben und vor dem Rendern der Szene. Die **MarbleMaze::Update**-Methode übernimmt die Aktualisierung der UI und des Spiels.

### Aktualisieren der Benutzeroberfläche

Die **MarbleMaze::Update**-Methode ruft die **UserInterface::Update**-Methode auf, um den Zustand der UI zu aktualisieren.

```cpp
UserInterface::GetInstance().Update(timeTotal, timeDelta);
```

Die **UserInterface::Update**-Methode aktualisiert jedes Element in der UI-Sammlung.

```cpp
void UserInterface::Update(float timeTotal, float timeDelta)
{
    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        (*iter)->Update(timeTotal, timeDelta);
    }
}
```

Von **ElementBase** abgeleitete Klassen implementieren die **Update**-Methode zum Ausführen spezifischer Verhalten. Die **StopwatchTimer::Update**-Methode aktualisiert beispielsweise die abgelaufene Zeit anhand der bereitgestellten Menge und aktualisiert den später angezeigten Text.

```cpp
void StopwatchTimer::Update(float timeTotal, float timeDelta)
{
    if (m_active)
    {
        m_elapsedTime += timeDelta;

        WCHAR buffer[16];
        GetFormattedTime(buffer);
        SetText(buffer);
    }

    TextElement::Update(timeTotal, timeDelta);
}
```

###  Aktualisieren der Szene

Die **MarbleMaze::Update**-Methode aktualisiert das Spiel basierend auf dem aktuellen Zustand des Zustandsautomaten. Wenn das Spiel einen aktiven Zustand aufweist, aktualisiert Marble Maze die Kamera, um die Kugel zu verfolgen. Zudem werden der Ansichtsmatrixteil der Konstantenpuffer und die physikalische Simulation aktualisiert.

Das folgende Beispiel zeigt, wie die **MarbleMaze::Update**-Methode die Position der Kamera aktualisiert. Marble Maze verwendet die **m\_resetCamera**-Variable für die Kennzeichnung, dass die Kamera für die Anordnung direkt über der Kugel zurückgesetzt werden muss. Die Kamera wird zurückgesetzt, wenn das Spiel startet oder die Kugel durch das Labyrinth fällt. Wenn das Hauptmenü oder die Highscoreanzeige aktiv ist, wird die Kamera auf eine konstante Position festgelegt. Andernfalls verwendet Marble Maze den *timeDelta*-Parameter zum Interpolieren der Kameraposition zwischen den Ist- und Zielpositionen. Die Zielposition befindet sich leicht über und vor der Kugel. Die Verwendung der abgelaufenen Framezeit ermöglicht der Kamera das schrittweise Folgen oder Verfolgen der Kugel.

```cpp
static float eyeDistance = 200.0f;
static float3 eyePosition = float3(0, 0, 0);

// Gradually move the camera above the marble.
float3 targetEyePosition = marblePosition - (eyeDistance * float3(g.x, g.y, g.z));
if (m_resetCamera)
{
    eyePosition = targetEyePosition;
    m_resetCamera = false;
}
else
{
    eyePosition = eyePosition + ((targetEyePosition - eyePosition) * min(1, timeDelta * 8));
}

// Look at the marble. 
if ((m_gameState == GameState::MainMenu) || (m_gameState == GameState::HighScoreDisplay))
{
    // Override camera position for menus.
    eyePosition = marblePosition + float3(75.0f, -150.0f, -75.0f);
    m_camera->SetViewParameters(eyePosition, marblePosition, float3(0.0f, 0.0f, -1.0f));
}
else
{
    m_camera->SetViewParameters(eyePosition, marblePosition, float3(0.0f, 1.0f, 0.0f));
}
```

Das folgende Beispiel zeigt, wie die **MarbleMaze::Update**-Methode die Konstantenpuffer für die Kugel und das Labyrinth aktualisiert. Die Modell- oder Weltmatrix des Labyrinths bleibt immer die Identitätsmatrix. Außer der Hauptdiagonale, deren Elemente alle Eins lauten, handelt es sich bei der Identitätsmatrix um eine Quadratmatrix, die aus Nullen besteht. Die Modellmatrix der Kugel basiert auf der zugehörigen Positionsmatrix multipliziert mit der zugehörigen Rotationsmatrix. Die Funktionen **mul** und **translation** werden in „BasicMath.h“ definiert.

```cpp
// Update the model matrices based on the simulation.
m_mazeConstantBufferData.model = identity();
m_marbleConstantBufferData.model = mul(
    translation(marblePosition.x, marblePosition.y, marblePosition.z),
    marbleRotationMatrix
    );

// Update the view matrix based on the camera.
float4x4 view;
m_camera->GetViewMatrix(&view);
m_mazeConstantBufferData.view = view;
m_marbleConstantBufferData.view = view;
```

Informationen dazu, wie die **MarbleMaze::Update**-Methode die Benutzereingabe liest und die Bewegung der Kugel simuliert, finden Sie unter [Hinzufügen von Eingaben und Interaktivität zum Marble Maze-Beispiel](adding-input-and-interactivity-to-the-marble-maze-sample.md).

## Rendern der Szene


Das Rendern einer Szene umfasst normalerweise die folgenden Schritte.

1.  Festlegen des Tiefenschablonenpuffers für das aktuelle Renderziel.
2.  Löschen der Render- und Schablonenansichten.
3.  Vorbereiten der Vertex- und Pixel-Shader für die Zeichnung.
4.  Rendern der 3-D-Objekte in der Szene.
5.  Rendern der 2-D-Objekte, die vor der Szene angezeigt werden sollen.
6.  Darstellen des gerenderten Bilds auf dem Monitor.

Die **MarbleMaze::Render**-Methode bindet das Renderziel und die Tiefenschablonenansichten, sie zeichnet die Szene und zeichnet dann die Überlagerung.

###  Vorbereiten der Renderziele

Vor dem Rendern der Szene müssen Sie den Tiefenschablonenpuffer für das aktuelle Renderziel festlegen. Wenn nicht feststeht, dass die Szene über alle Bildschirmpixel gezeichnet wird, löschen Sie auch die Render- und Schablonenansichten. Marble Maze löscht die Render- und Schablonenansichten in jedem Frame, um sicherzustellen, dass keine sichtbaren Artefakte aus dem vorherigen Frame vorhanden sind.

Im folgenden Beispiel wird gezeigt, wie die **MarbleMaze::Render**-Methode die [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464)-Methode aufruft, um das Renderziel und den Tiefenschablonenpuffer richtig festzulegen. Die **m\_renderTargetView**-Membervariable, ein [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)-Objekt, und die **m\_depthStencilView**-Membervariable, ein [**ID3D11DepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476377)-Objekt, werden von der **DirectXBase**-Klasse definiert und initialisiert.

```cpp
// Bind the render targets.
m_d3dContext->OMSetRenderTargets(
    1,
    m_renderTargetView.GetAddressOf(),
    m_depthStencilView.Get()
    );

// Clear the render target and depth stencil to default values. 
const float clearColor[4] = { 0.0f, 0.0f, 0.0f, 1.0f };

m_d3dContext->ClearRenderTargetView(
    m_renderTargetView.Get(),
    clearColor
    );

m_d3dContext->ClearDepthStencilView(
    m_depthStencilView.Get(),
    D3D11_CLEAR_DEPTH,
    1.0f,
    0
    );
```

Die [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)-Schnittstelle und die [**ID3D11DepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476377)-Schnittstelle unterstützen den Texturansichtsmechanismus, der von Direct3D 10 und höher bereitgestellt wird. Weitere Informationen zu Texturansichten finden Sie unter [Texturansichten (Direct3D 10)](https://msdn.microsoft.com/library/windows/desktop/bb205128). Die [**OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464)-Methode bereitet die Ausgabezusammenführungsphase der Direct3D-Pipeline vor. Weitere Informationen zur Ausgabezusammenführungsphase finden Sie unter [Ausgabezusammenführungsphase](https://msdn.microsoft.com/library/windows/desktop/bb205120).

### Vorbereiten der Vertex- und Pixel-Shader

Führen Sie vor dem Rendern der Szenenobjekte die folgenden Schritte aus, um die Vertex- und Pixel-Shader für die Zeichnung vorzubereiten:

1.  Legen Sie das Shader-Eingabelayout als aktuelles Layout fest.
2.  Legen Sie die Vertex und Pixel-Shader als aktuelle Shader fest.
3.  Aktualisieren Sie die Konstantenpuffer mit Daten, die Sie an die Shader übergeben müssen.

> **Wichtig**  Marble Maze verwendet für alle 3-D-Objekte ein Paar Vertex- und Pixel-Shader. Wenn für Ihr Spiel mehrere Shader-Paare verwendet werden, müssen Sie diese Schritte immer dann ausführen, wenn Sie Objekte zeichnen, für die verschiedene Shader verwendet werden. Zur Reduzierung des Aufwands bezüglich der Änderung des Shader-Zustands wird empfohlen, die Renderaufrufe für alle Objekte zu gruppieren, die dieselben Shader verwenden.

 

Im Abschnitt [Laden von Shadern](#loading_shaders) in diesem Dokument wird beschrieben, wie das Eingabelayout beim Erstellen des Vertex-Shaders erstellt wird. Im folgenden Beispiel wird gezeigt, wie die **MarbleMaze::Render**-Methode die [**ID3D11DeviceContext::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454)-Methode verwendet, um dieses Layout als aktuelles Layout festzulegen.

```cpp
m_d3dContext->IASetInputLayout(m_inputLayout.Get());
```

Im folgenden Beispiel wird gezeigt, wie die **MarbleMaze::Render**-Methode die [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493)-Methode und die [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472)-Methode verwendet, um die Vertex- bzw. Pixel-Shader als aktuelle Shader festzulegen.

```cpp
// Set the vertex shader stage state.
m_d3dContext->VSSetShader(
    m_vertexShader.Get(),   // Use this vertex shader. 
    nullptr,                // Don't use shader linkage.
    0                       // Don't use shader linkage.
    );

// Set the pixel shader stage state.
m_d3dContext->PSSetShader(
    m_pixelShader.Get(),    // Use this pixel shader. 
    nullptr,                // Don't use shader linkage.
    0                       // Don't use shader linkage.
    );

m_d3dContext->PSSetSamplers(
    0,                       // Starting at the first sampler slot
    1,                       // set one sampler binding
    m_sampler.GetAddressOf() // to use this sampler.
    );
```

Nachdem die Shader und das zugehörige Eingabelayout von **MarbleMaze::Render** festgelegt wurden, wird die [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486)-Methode zum Aktualisieren des Konstantenpuffers mit den Modell-, Ansichts- und Projektionsmatrizen für das Labyrinth verwendet. Die **UpdateSubresource**-Methode kopiert die Matrixdaten aus dem CPU-Speicher in den GPU-Speicher. Denken Sie daran, dass die Modell- und Ansichtskomponenten der **ConstantBuffer**-Struktur in der **MarbleMaze::Update**-Methode aktualisiert werden. Die **MarbleMaze::Render**-Methode ruft dann die [**ID3D11DeviceContext::VSSetConstantBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476491)-Methode und die [**ID3D11DeviceContext::PSSetConstantBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476470)-Methode auf, um diesen Konstantenpuffer als aktuellen Puffer festzulegen.

```cpp
// Update the constant buffer with the new data.
m_d3dContext->UpdateSubresource(
    m_constantBuffer.Get(),
    0,
    nullptr,
    &m_mazeConstantBufferData,
    0,
    0
    );

m_d3dContext->VSSetConstantBuffers(
    0,                // Starting at the first constant buffer slot
    1,                // set one constant buffer binding
    m_constantBuffer.GetAddressOf() // to use this buffer.
    );

m_d3dContext->PSSetConstantBuffers(
    0,                // Starting at the first constant buffer slot
    1,                // set one constant buffer binding
    m_constantBuffer.GetAddressOf() // to use this buffer.
    );
```

Die **MarbleMaze::Render**-Methode führt ähnliche Schritte aus, um die zu rendernde Kugel vorzubereiten.

### Rendern des Labyrinths und der Kugel

Nachdem Sie die aktuellen Shader aktiviert haben, können Sie die Szenenobjekte zeichnen. Die **MarbleMaze::Render**-Methode ruft die **SDKMesh::Render**-Methode auf, um das Labyrinth-Gitter zu rendern.

```cpp
m_mazeMesh.Render(m_d3dContext.Get(), 0, INVALID_SAMPLER_SLOT, INVALID_SAMPLER_SLOT);
```

Die **MarbleMaze::Render**-Methode führt ähnliche Schritte aus, um die Kugel zu rendern.

Wie bereits in diesem Dokument erwähnt wurde, wird die **SDKMesh**-Klasse zu Demonstrationszwecken bereitgestellt. Sie wird jedoch nicht für die Verwendung in einem Spiel in Produktionsqualität empfohlen. Beachten Sie jedoch, dass die **SDKMesh::RenderMesh**-Methode, die von **SDKMesh::Render** aufgerufen wird, die [**ID3D11DeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456)-Methode und die [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453)-Methode zum Festlegen der aktuellen Vertex- und Indexpuffer verwendet, mit deren Hilfe das Gitter definiert wird. Die [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476410)-Methode wird zum Zeichnen der Puffer verwendet. Weitere Informationen zum Arbeiten mit Vertex- und Indexpuffern finden Sie unter [Einführung in Puffer in Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476898).

### Zeichnen der Benutzeroberfläche und Überlagerung

Nach dem Zeichnen der 3-D-Szenenobjekte zeichnet Marble Maze die 2-D-UI-Elemente, die vor der Szene angezeigt werden.

Die **MarbleMaze::Render**-Methode endet mit dem Zeichnen der Benutzeroberfläche und der Überlagerung.

```cpp
// Draw the user interface and the overlay.
UserInterface::GetInstance().Render();

m_sampleOverlay->Render();
```

Die **UserInterface::Render**-Methode verwendet ein [**ID2D1DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/hh404479)-Objekt zum Zeichnen der UI-Elemente. Diese Methode legt den Zeichnungszustand fest, sie zeichnet alle aktiven UI-Elemente und stellt dann den vorherigen Zeichnungszustand wieder her.

```cpp
void UserInterface::Render()
{
    m_d2dContext->SaveDrawingState(m_stateBlock.Get());
    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    m_d2dContext->SetTextAntialiasMode(D2D1_TEXT_ANTIALIAS_MODE_GRAYSCALE);

    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if ((*iter)->IsVisible())
            (*iter)->Render();
    }

    m_d2dContext->EndDraw();
    m_d2dContext->RestoreDrawingState(m_stateBlock.Get());
}
```

Die **SampleOverlay::Render**-Methode verwendet eine ähnliche Technik zum Zeichnen der Überlagerungsbitmap.

###  Darstellen der Szene

Nach dem Zeichnen aller 2-D- und 3-D-Szenenobjekte stellt Marble Maze das gerenderte Bild auf dem Monitor dar. Das Spiel synchronisiert die Zeichnung mit der vertikalen Austastung, um sicherzustellen, dass keine Zeit für das Zeichnen von Frames verwendet wird, die letztendlich nie auf dem Bildschirm angezeigt werden. Marble Maze verarbeitet beim Darstellen der Szene auch Geräteänderungen.

Nach dem Aufrufen der **MarbleMaze::Render**-Methode ruft die Spielschleife die **MarbleMaze::Present**-Methode auf, um das gerenderte Bild an den Monitor oder das Display zu senden. Die **MarbleMaze**-Klasse überschreibt nicht die **DirectXBase::Present**-Methode. Die **DirectXBase::Present**-Methode ruft [**IDXGISwapChain1::Present**](https://msdn.microsoft.com/library/windows/desktop/hh446797) auf, um den aktuellen Vorgang auszuführen. Dies wird im folgenden Beispiel veranschaulicht:

```cpp
// The application may optionally specify "dirty" or "scroll" rects 
// to improve efficiency in certain scenarios. 
// In this sample, however, we do not utilize those features.
DXGI_PRESENT_PARAMETERS parameters = {0};
parameters.DirtyRectsCount = 0;
parameters.pDirtyRects = nullptr;
parameters.pScrollRect = nullptr;
parameters.pScrollOffset = nullptr;

// The first argument instructs DXGI to block until VSync, putting the  
// application to sleep until the next VSync.  
// This ensures we don't waste any cycles rendering frames that will  
// never be displayed to the screen.
HRESULT hr = m_swapChain->Present1(1, 0, &parameters);
```

In diesem Beispiel ist **M\_swapChain** ein [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)-Objekt. Die Initialisierung dieses Objekts wird im Abschnitt [Initialisieren von Direct3D und Direct2D](#initializing) in diesem Dokument beschrieben.

Der erste Parameter für [**IDXGISwapChain1::Present**](https://msdn.microsoft.com/library/windows/desktop/hh446797), *SyncInterval*, gibt die Anzahl vertikaler Austastungen an, die vor dem Darstellen des Frames abgewartet werden müssen. Marble Maze gibt den Wert 1 an, sodass bis zur nächsten vertikalen Austastung gewartet wird. Eine vertikale Austastung ist die Zeit zwischen der Fertigstellung der Zeichnung eines Frames auf dem Monitor und dem Start für den nächsten Frame.

Die [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797)-Methode gibt einen Fehlercode zurück, der angibt, dass das Gerät entfernt wurde oder ein anderer Fehler aufgetreten ist. In diesem Fall initialisiert Marble Maze das Gerät erneut.

```cpp
// Reinitialize the renderer if the device was disconnected  
// or the driver was upgraded. 
if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
{
    Initialize(m_window, m_dpi);
}
else
{
    DX::ThrowIfFailed(hr);
}
```

## Nächste Schritte


Lesen Sie den Abschnitt [Hinzufügen von Eingaben und Interaktivität zum Marble Maze-Beispiel](adding-input-and-interactivity-to-the-marble-maze-sample.md), um Informationen zu einigen wichtigen Verfahren zu erhalten, die Sie beim Arbeiten mit Eingabegeräten beachten sollten. In diesem Dokument wird erläutert, wie Marble Maze Fingereingaben und Eingaben vom Beschleunigungsmesser, Xbox 360-Controller und von der Maus unterstützt.

## Verwandte Themen


* [Hinzufügen von Eingaben und Interaktivität zum Marble Maze-Beispiel](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Marble Maze-Anwendungsstruktur](marble-maze-application-structure.md)
* [Entwickeln von Marble Maze, einem UWP-Spiel in C++ und DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 







<!--HONumber=Jun16_HO4-->


