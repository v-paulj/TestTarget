---
title: Einrichten des Spieleprojekts
description: Im ersten Schritt für die Spielerstellung richten Sie ein Projekt in Microsoft Visual Studio so ein, dass die Bearbeitung der Codeinfrastruktur einfach ist.
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
---

# Einrichten des Spieleprojekts


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Im ersten Schritt für die Erstellung Ihres Spiels richten Sie ein Projekt in Microsoft Visual Studio so ein, dass Sie möglichst wenig Aufwand mit der Bearbeitung der Codeinfrastruktur haben. Sie können sich eine Menge Zeit und Arbeit ersparen, wenn Sie die richtige Vorlage verwenden und das Projekt speziell für die Spieleentwicklung konfigurieren. Im Anschluss finden Sie die erforderlichen Einrichtungs- und Konfigurationsschritte für ein einfaches Spieleprojekt.

## Ziel


-   Hier lernen Sie, wie Sie ein Direct3D-Spieleprojekt in Visual Studio einrichten.

## Einrichten des Spieleprojekts


Sie können ein Spiel natürlich von Grund auf neu entwickeln. Dafür brauchen Sie eigentlich nur einen praktischen Text-Editor, ein paar Beispiele und eine ganze Menge Kreativität. Das ist allerdings nicht unbedingt die effektivste Vorgehensweise. Viel besser wäre es doch, wenn Sie als Neueinsteiger ohne Erfahrung mit der Entwicklung für die universelle Windows-Plattform (UWP) einige dieser Aufgaben an Visual Studio abtreten könnten. Hier erfahren Sie, wie Sie Ihrem Projekt zu einem guten Start verhelfen.

## 1. Auswählen der richtigen Vorlage


Eine Visual Studio-Vorlage ist eine Sammlung von Einstellungen und Codedateien, die abhängig von der bevorzugten Sprache und Technologie auf eine bestimmte Art von App ausgerichtet sind. In Microsoft Visual Studio 2015 stehen einige Vorlagen zur Verfügung, die die Entwicklung des Spiels und der Grafik deutlich vereinfachen können. Ohne Verwendung einer Vorlage müssen Sie einen Großteil des grundlegenden Rendering- und Anzeigeframeworks für die Grafik selbst entwickeln, was insbesondere für neue Spieleentwickler recht mühsam sein kann.

Die richtige Vorlage für dieses Tutorial ist die Vorlage „DirectX 11-App (Universelle Windows-App)“. Klicken Sie in Visual Studio 2015 auf **Datei...** &gt; **Neues Projekt**, und gehen Sie anschließend wie folgt vor:

1.  Navigieren Sie unter **Vorlagen** zu **Visual C++** > **Windows** > **Universell**.
2.  Wählen Sie im mittleren Bereich **DirectX 11-App (universelle Windows-App)**aus.
3.  Geben Sie Ihrem Spieleprojekt einen Namen, und klicken Sie auf **OK**.

![Auswählen der Vorlage „Direct3D-Anwendung“](images/simple-dx-game-vs-new-proj.png)

Diese Vorlage enthält das grundlegende Framework für eine UWP-App, für die DirectX mit C++ verwendet wird. Drücken Sie am besten gleich F5, um es zu erstellen und auszuführen. Ist das nicht ein hübscher blauer Bildschirm? Nehmen Sie sich einen Moment Zeit, und sehen Sie sich den von der Vorlage bereitgestellten Code an. Mit der Vorlage werden mehrere Codedateien erstellt, die die grundlegenden Funktionen für eine UWP-App mit DirectX und C++ enthalten. Auf die anderen Codedateien gehen wir in [Schritt 3](#3-review-the-included-libraries-and-headers) ein. Jetzt widmen wir uns erst einmal **App.h**.

```cpp
    ref class App sealed : public Windows::ApplicationModel::Core::IFrameworkView
    {
    public:
        App();

        // IFrameworkView Methods.
        virtual void Initialize(Windows::ApplicationModel::Core::CoreApplicationView^ applicationView);
        virtual void SetWindow(Windows::UI::Core::CoreWindow^ window);
        virtual void Load(Platform::String^ entryPoint);
        virtual void Run();
        virtual void Uninitialize();

    protected:
        // Application lifecycle event handlers.
        void OnActivated(Windows::ApplicationModel::Core::CoreApplicationView^ applicationView, Windows::ApplicationModel::Activation::IActivatedEventArgs^ args);
        void OnSuspending(Platform::Object^ sender, Windows::ApplicationModel::SuspendingEventArgs^ args);
        void OnResuming(Platform::Object^ sender, Platform::Object^ args);

        // Window event handlers.
        void OnWindowSizeChanged(Windows::UI::Core::CoreWindow^ sender, Windows::UI::Core::WindowSizeChangedEventArgs^ args);
        void OnVisibilityChanged(Windows::UI::Core::CoreWindow^ sender, Windows::UI::Core::VisibilityChangedEventArgs^ args);
        void OnWindowClosed(Windows::UI::Core::CoreWindow^ sender, Windows::UI::Core::CoreWindowEventArgs^ args);

        // DisplayInformation event handlers.
        void OnDpiChanged(Windows::Graphics::Display::DisplayInformation^ sender, Platform::Object^ args);
        void OnOrientationChanged(Windows::Graphics::Display::DisplayInformation^ sender, Platform::Object^ args);
        void OnDisplayContentsInvalidated(Windows::Graphics::Display::DisplayInformation^ sender, Platform::Object^ args);

    private:
        std::shared_ptr<DX::DeviceResources> m_deviceResources;
        std::unique_ptr<MyAwesomeGameMain> m_main;
        bool m_windowClosed;
        bool m_windowVisible;
    };
```

Bei der Implementierung der [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700469)-Schnittstelle (zum Definieren eines Ansichtsanbieters) erstellen Sie die folgenden fünf Methoden: [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495), [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509), [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501), [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) und [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523). Diese Methoden werden vom App-Singleton ausgeführt, der beim Spielstart erstellt wird. Sie laden alle vom Spiel benötigten Ressourcen und stellen eine Verbindung zwischen den entsprechenden Ereignishandlern her.

Die **main**-Methode befindet sich in der Quelldatei **App.cpp**. Sie sieht ungefähr so aus:

```cpp
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

Im vorliegenden Zustand erstellt sie eine Instanz des Direct3D-Ansichtsanbieters aus der Ansichtsanbieterfactory (**Direct3DApplicationSource**, definiert in **App.h**) und übergibt sie zur Ausführung an das App-Singleton ([**CoreApplication::Run**](https://msdn.microsoft.com/library/windows/apps/hh700469)). Das bedeutet, dass sich der Ausgangspunkt für Ihr Spiel im Implementierungscode der [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505)-Methode (in diesem Fall: **App::Run**) befindet. Hier ist der Code:

```cpp
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

Wenn das Fenster Ihres Spiels nicht geschlossen wird, werden hiermit alle Ereignisse gesendet, der Timer wird aktualisiert, und die Ergebnisse der Grafikpipeline werden gerendert und angezeigt. Auf diesen Punkt gehen wir später in [Definieren des UWP-Frameworks für das Spiel](tutorial--building-the-games-metro-style-app-framework.md) und in [Zusammensetzen der Renderingpipeline](tutorial--assembling-the-rendering-pipeline.md)näher ein. Damit dürften Sie ein gewisses Gespür für die grundlegende Codestruktur eines UWP-DirectX-Spiels entwickelt haben.

## 2. Prüfen und Aktualisieren der Datei „package.appxmanifest“


Die Vorlage hat aber noch mehr zu bieten als nur Codedateien. Die Datei **package.appxmanifest** enthält Metadaten für Ihr Projekt. Diese werden zum Packen und Starten des Spiels sowie zum Übermitteln des Spiels an den Windows Store verwendet. Darüber hinaus enthält sie wichtige Infos, mit deren Hilfe das System des Spielers den Zugriff auf die zum Ausführen des Spiels benötigten Systemressourcen ermöglicht.

Starten Sie den Manifest-Designer****. Doppelklicken Sie hierzu im Projektmappen-Explorer**** auf die Datei **package.appxmanifest**. Sie sehen die folgende Ansicht:

![Manifest-Editor „package.appx“](images/simple-dx-game-vs-app-manifest.png)

Weitere Informationen zur Datei **package.appxmanifest** und zum Packen finden Sie unter [Manifest-Designer](https://msdn.microsoft.com/library/windows/apps/br230259.aspx). Nun widmen wir uns aber erst einmal der Registerkarte **Funktionen** und den dort zur Verfügung stehenden Optionen.

![Standardfunktionen einer Direct3D-App](images/simple-dx-game-vs-capabilities.png)

Wenn Sie die von Ihrem Spiel genutzten Funktionen (etwa den Zugriff auf das Internet**** für eine globale Bestenliste) nicht auswählen, können Sie nicht auf die entsprechenden Ressourcen oder Features zugreifen. Achten Sie beim Erstellen eines neuen Spiels darauf, die Funktionen auszuwählen, die Ihr Spiel für die Ausführung benötigt.

Kommen wir nun zu den restlichen Dateien der Vorlage **DirectX 11-App (Universelle Windows-App)**.

## 3. Anzeigen der enthaltenen Bibliotheken und Header


Ein paar Dateien haben wir uns für den Schluss aufgehoben. Diese Dateien bieten zusätzliche Tools und Unterstützung für die Entwicklung von Direct3D-Spielen.

| Quelldatei der Vorlage         | Beschreibung                                                                                                                                                                                                            |
|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| StepTimer.h                  | Definiert einen Timer mit hoher Auflösung, der sich besonders für Spiele oder Apps mit interaktivem Rendering eignet.                                                                                                                                       |
| Sample3DSceneRenderer.h/.cpp | Definiert eine einfache Renderimplementierung, die eine Direct3D-Swapchain und einen Grafikadapter mit Ihrem UWP-DirectX-Spiel verbindet.                                                                                            |
| DirectXHelper.h              | Implementiert eine einzelne Methode (**DX::ThrowIfFailed**). Mit dieser Methode werden die von DirectX-APIs zurückgegebenen HRESULT-Fehlerwerte in Ausnahmen der Windows-Runtime konvertiert. Verwenden Sie diese Methode, um einen Haltepunkt zum Debuggen von DirectX-Fehlern zu setzen. |
| pch.h/.cpp                   | Enthält alles, was das Windows-System für die von einer Direct3D-App genutzten APIs (einschließlich DirectX 11-APIs) enthält.                                                                                                           |
| SamplePixelShader.hlsl       | Enthält den Code der High-Level-Shader-Language (HLSL) für einen sehr einfachen Pixel-Shader.                                                                                                                                     |
| SampleVertexShader.hlsl      | Enthält den Code der High-Level Shader Language (HLSL) für einen sehr einfachen Vertex-Shader.                                                                                                                                    |

 

### Nächste Schritte

Sie können nun ein Spieleprojekt unter Verwendung von UWP mit DirectX erstellen und kennen die Komponenten und Dateien der Vorlage "DirectX 11-App (Universelle Windows-App)".

Im nächsten Lernprogramm, [Definieren des UWP-App-Frameworks für das Spiel](tutorial--building-the-games-metro-style-app-framework.md), untersuchen wir anhand eines fertigen Spiels die Verwendung und Erweiterung vieler der Konzepte und Komponenten aus dieser Vorlage.

 

 






<!--HONumber=Mar16_HO1-->


