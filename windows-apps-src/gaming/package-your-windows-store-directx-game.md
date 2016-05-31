---
author: mtoepke
title: Packen Ihres UWP-DirectX-Spiels (DirectX-Spiels für die universelle Windows-Plattform)
description: Umfangreichere UWP-Spiele (Universelle Windows-Plattform) können leicht relativ groß werden. Dies gilt besonders für Spiele, bei denen mehrere Sprachen mit regionsspezifischen Ressourcen unterstützt werden oder die über optionale HD-Ressourcen verfügen.
ms.assetid: 68254203-c43c-684f-010a-9cfa13a32a77
---

#  Packen Ihres UWP-DirectX-Spiels (DirectX-Spiels für die universelle Windows-Plattform)


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Umfangreichere UWP-Spiele (Universelle Windows-Plattform) können leicht relativ groß werden. Dies gilt besonders für Spiele, bei denen mehrere Sprachen mit regionsspezifischen Ressourcen unterstützt werden oder die über optionale HD-Ressourcen verfügen. In diesem Thema erfahren Sie, wie Sie App-Pakete und App-Bündel zum Anpassen der App verwenden, damit Kunden nur die wirklich benötigten Ressourcen erhalten.

Zusätzlich zum App-Paketmodell bietet Windows 10 Unterstützung für App-Bündel, bei denen zwei Arten von Paketen gruppiert werden:

-   App-Pakete enthalten plattformspezifische ausführbare Dateien und Bibliotheken. Häufig verfügen UWP-Spiele über bis zu drei App-Pakete, und zwar jeweils ein Paket für die x86-, x64- und ARM-CPU-Architekturen. Der Code und die Daten für die entsprechende Hardwareplattform müssen im dazugehörigen App-Paket enthalten sein. Ein App-Paket sollte außerdem alle wichtigen Ressourcen für das Spiel enthalten, damit die Voraussetzungen für eine Ausführung in guter Qualität und mit guter Leistung gegeben sind.
-   Ressourcenpakete enthalten optionale oder erweiterte plattformagnostische Daten, z. B. Spielressourcen (Texturen, Gitter, Sound, Text). Ein UWP-Spiel kann über ein oder mehrere Ressourcenpakete verfügen, z. B. Ressourcenpakete für HD-Ressourcen oder -Texturen, Ressourcen der DirectX-Featureebene 11+ oder sprachspezifische Ressourcen.

Weitere Informationen zu App-Bündeln und App-Paketen finden Sie unter [Definieren von App-Ressourcen](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321).

Sie können zwar alle Inhalte in Ihre App-Pakete integrieren, aber diese Vorgehensweise ist ineffizient und redundant. Warum soll eine große Texturdatei für jede Plattform einzeln vorhanden sein (also dreimal), wenn sie für ARM-Plattformen meist gar nicht genutzt wird? Eine gute Zielsetzung besteht darin, die von Kunden herunterzuladende Datenmenge möglichst gering zu halten. Die Kunden können dann schneller mit dem Spielen beginnen, Speicherplatz auf dem Gerät sparen und potenzielle Kosten für Bandbreite in getakteten Netzwerken vermeiden.

Bei der Nutzung dieses Features des UWP-App-Installers ist es wichtig, zu einem frühen Zeitpunkt des Entwicklungsprozesses das Verzeichnislayout und die Benennungskonventionen für Dateien zu beachten. So ermöglichen Sie Tools und Quellen die korrekte Ausgabe und somit einen einfachen Verpackungsvorgang. Befolgen Sie die in diesem Dokument beschriebenen Regeln, wenn Sie Spiele entwickeln, die Ressourcenentwicklung konfigurieren, Tools und Skripts verwalten und Code erstellen, mit dem Ressourcen geladen werden oder darauf verwiesen wird.

## Warum sollten Ressourcenpakete erstellt werden?


Beim Erstellen einer App – besonders einer Spiele-App, die in vielen Regionen bzw. für viele verschiedene UWP-Hardwareplattformen verkauft werden kann – müssen Sie viele Dateien häufig in mehreren Versionen bereitstellen, um die Unterstützung für diese Gebietsschemas und Plattformen sicherzustellen. Wenn Sie das Spiel beispielsweise sowohl in den USA als auch in Japan veröffentlichen, benötigen Sie ggf. einen Satz Sprachdateien in Englisch für das Gebietsschema "en-us" und einen zweiten Satz in Japanisch für "jp-jp". Falls Sie in Ihrem Spiel ein Bild für ARM-Geräte sowie für x86- und x64-Plattformen verwenden möchten, müssen Sie dieselbe Bildressourcen einmal für jede CPU-Architektur hochladen, also insgesamt dreimal.

Falls im Spiel viele HD-Ressourcen enthalten sind, die für Plattformen mit niedrigeren DirectX-Featureebenen nicht geeignet sind, muss die folgende Frage gestellt werden: Warum sollen diese Ressourcen in das Hauptpaket der App eingebunden werden, sodass Benutzer eine große Menge von Komponenten herunterladen müssen, die vom Gerät gar nicht genutzt werden können? Das Abtrennen dieser HD-Ressourcen als optionales Ressourcenpaket bedeutet, dass Kunden mit Geräten, von denen diese HD-Ressourcen unterstützt werden, Zugriff darauf haben (und bei getakteter Netzwerkbandbreite möglicherweise zusätzliche Kosten anfallen). Kunden, die nicht über diese Geräte verfügen, erhalten das Spiel dann schneller und zu geringeren Netzwerkkosten.

Inhaltskandidaten für Pakete mit Spielressourcen sind:

-   Ressourcen für internationale Gebietsschemas (lokalisierter Text, Audiodaten oder Bilder)
-   Hochaufgelöste Ressourcen für unterschiedliche Geräteskalierungsfaktoren (1,0x, 1,4x und 1,8x)
-   HD-Ressourcen für höhere DirectX-Featureebenen (9, 10 und 11)

All diese Ressourcen werden im "package.appxmanifest" definiert, das Bestandteil des UWP-Projekts ist, sowie in der Verzeichnisstruktur des fertigen Pakets. Wenn Sie sich an den in diesem Dokument beschriebenen Prozess halten, ist aufgrund der neuen Visual Studio-UI normalerweise keine manuelle Bearbeitung erforderlich.

> **Wichtig**   Diese Ressourcen werden über die **Windows.ApplicationModel.Resources**\*-APIs geladen und verwaltet. Wenn Sie diese App-Modellressourcen-APIs zum Laden der richtigen Datei für ein Gebietsschema, einen Skalierungsfaktor oder eine DirectX-Featureebene verwenden, müssen Sie die Ressourcen nicht mithilfe expliziter Dateipfade laden. Stattdessen stellen Sie für die Ressourcen-APIs nur den generalisierten Dateinamen der gewünschten Ressource bereit und überlassen es dem Ressourcenverwaltungssystem, die richtige Variante der Ressource für die aktuelle Plattform und Gebietsschemakonfiguration (direkte Angabe ebenfalls mit diesen APIs) des Benutzers abzurufen.

 

Es gibt zwei grundlegende Möglichkeiten, Ressourcen für das Verpacken anzugeben:

-   Objektdateien weisen den gleichen Dateinamen auf, und die einzelnen Ressourcenpaketversionen werden in Verzeichnisse mit bestimmten Namen eingefügt. Diese Verzeichnisnamen werden vom System reserviert. Zum Beispiel; \\en-us, \\scale-140, \\dxfl-dx11.
-   Objektdateien werden in Ordnern mit beliebigen Namen gespeichert. Die Dateien werden jedoch mit einer gemeinsamen Bezeichnung benannt, die mit Zeichenfolgen angehängt wird, die das System zum Angeben von Sprach- und anderen Qualifizierern reserviert. Diese Qualifiziererzeichenfolgen werden nach einem Unterstrich („\_“) an den generalisierten Dateinamen angehängt. Beispielsweise: \\assets\\menu\_option1\_lang-en-us.png, \\assets\\menu\_option1\_scale-140.png, \\assets\\coolsign\_dxfl-dx11.dds. Sie können diese Zeichenfolgen auch kombinieren. Beispielsweise: \\assets\\menu\_option1\_scale-140\_lang-en-us.png.
    > **Hinweis**   Wenn ein Sprachqualifizierer nicht nur in einem Verzeichnisnamen, sondern in einem Dateinamen verwendet wird, muss dieser die Form „lang-“ haben,<tag>beispielsweise „lang-en-us“ wie unter [So wird’s gemacht: Benennen von Ressourcen mit Qualifizierern](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324) beschrieben.

     

Verzeichnisnamen können beim Packen von Ressourcen kombiniert werden, um die Genauigkeit zu erhöhen. Sie dürfen jedoch nicht redundant sein. Beispielsweise ist \\en-us\\menu\_option1\_lang-en-us.png redundant.

Sie können unter einem Ressourcenverzeichnis einen benötigten nicht reservierten Namen eines Unterverzeichnisses angeben, solange die Verzeichnisstruktur in jedem Ressourcenverzeichnis identisch ist. Beispielsweise: \\dxfl-dx10\\assets\\textures\\coolsign.dds. Wenn Sie eine Ressource laden oder darauf verweisen, muss der Pfadname generalisiert werden. Dabei müssen alle Qualifizierer für Sprache, Skalierung und DirectX-Featureebene unabhängig davon entfernt werden, ob sie sich in Ordnerknoten oder in den Dateinamen befinden. Um beispielsweise im Code auf eine Ressource zu verweisen, die als Variante \\dxfl-dx10\\assets\\textures\\coolsign.dds beinhaltet, verwenden Sie \\assets\\textures\\coolsign.dds. Um auf eine Ressource mit einer Variante \\images\\background\_scale-140.png zu verweisen, verwenden Sie dementsprechend \\images\\background.png.

In der folgenden Tabelle sind die reservierten Verzeichnisnamen und die per Unterstrich angefügten Dateinamenpräfixe aufgeführt:

| Ressourcentyp                   | Verzeichnisname für Ressourcenpaket                                                                                                                  | Dateinamensuffix für Ressourcenpaket                                                                                                    |
|------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| Lokalisierte Ressourcen             | Alle möglichen Sprachen bzw. Kombinationen aus Sprache und Gebietsschema für Windows 10. (Das Qualifiziererpräfix „lang-“ ist in Ordnernamen nicht erforderlich.) | „\_“ gefolgt von Sprache, Gebietsschema oder Gebietsschema/Sprache-Spezifizierer. Beispielsweise: „\_en“, „\_us“ bzw. „\_en-us“. |
| Ressourcen für Skalierungsfaktor        | scale-100, scale-140, scale-180. Gilt für die UI-Skalierungsfaktoren 1,0x, 1,4x bzw. 1,8x.                                     | „\_“ gefolgt von „scale-100“, „scale-140“ oder „scale-180“.                                                                    |
| Ressourcen für die DirectX-Featureebene | dxfl-dx9, dxfl-dx10 und dxfl-dx11. Gilt für die DirectX-Featureebenen 9, 10 bzw. 11.                                     | „\_“ gefolgt von „dxfl-dx9“, „dxfl-dx10“ oder „dxfl-dx11“.                                                                     |

 

## Definieren von Paketen mit lokalisierten Sprachressourcen


Gebietsschemaspezifische Dateien werden in Projektverzeichnisse eingefügt, die nach der Sprache benannt sind (z. B. "en").

Gehen Sie beim Konfigurieren der App für die Unterstützung lokalisierter Ressourcen für mehrere Sprachen wie folgt vor:

-   Erstellen Sie ein App-Unterverzeichnis (bzw. eine Dateiversion) für jede Sprache bzw. jedes Gebietsschema, die bzw. das unterstützt werden soll (z. B. en-us, jp-jp, zh-cn oder fr-fr).
-   Fügen Sie während der Entwicklung Kopien ALLER Ressourcen (z. B. lokalisierte Audiodateien, Texturen und Menügrafiken) auch dann in das Unterverzeichnis für die entsprechende Sprache bzw. das Gebietsschema ein, wenn diese sich über die Sprachen/Gebietsschemas hinweg nicht unterscheiden. Stellen Sie mit Blick auf die bestmögliche Benutzerfreundlichkeit sicher, dass Benutzer darauf hingewiesen werden, falls diese ein verfügbares Sprachressourcenpaket für ihr Gebietsschema nicht abgerufen haben (oder es nach dem Herunterladen oder der Installation versehentlich gelöscht haben).
-   Achten Sie darauf, dass die Ressourcen- und Zeichenfolgendateien (.resw) in jedem Verzeichnis gleich benannt sind. Die Datei „menu\_option1.png“ sollte beispielsweise in den Verzeichnissen „\\en-us“ und „\\jp-jp“ denselben Namen haben, auch wenn der Inhalt in verschiedenen Sprachen vorliegt. Die Dateien werden in diesem Fall als „\\en-us\\menu\_option1.png“ und „\\jp-jp\\menu\_option1.png“ angezeigt.
    > **Hinweis**   Optional können Sie das Gebietsschema an den Dateinamen anhängen und die Dateien im selben Verzeichnis speichern. Beispiele: \\assets\\menu\_option1\_lang-en-us.png und \\assets\\menu\_option1\_lang-jp-jp.png.

     

-   Verwenden Sie die APIs in [**Windows.ApplicationModel.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022) und [**Windows.ApplicationModel.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039), um die gebietsschemaspezifischen Ressourcen für die App anzugeben und zu laden. Verwenden Sie auch Verweise auf Ressourcen, die kein bestimmtes Gebietsschema enthalten. Diese APIs bestimmen das richtige Gebietsschema basierend auf den Einstellungen des jeweiligen Benutzers und rufen somit die richtige Ressource für den Benutzer ab.
-   Wählen Sie in Microsoft Visual Studio 2015 die Option **PROJEKT -> Store -> Anwendungspaket erstellen...**, und erstellen Sie das Paket.

## Definieren von Ressourcenpaketen für den Skalierungsfaktor


Windows 10 verfügt über drei Skalierungsfaktoren für Benutzeroberflächen: 1,0x, 1,4x und 1,8x. Die Skalierungswerte für jede Anzeige werden während der Installation basierend auf verschiedenen kombinierten Faktoren festgelegt: Größe des Bildschirms, Auflösung des Bildschirms und angenommener durchschnittlicher Abstand der Benutzer vom Bildschirm. Der Benutzer kann Skalierungsfaktoren auch anpassen, um die Lesbarkeit zu verbessern. Um die besten Ergebnisse zu erzielen, sollte das Spiel sowohl mit DPI-Werten kompatibel sein als auch den Skalierungsfaktor beachten können. Dies bedeutet, dass Sie Versionen wichtiger Grafikressourcen für alle drei Skalierungsfaktoren erstellen sollten. Dies betrifft auch die Zeigerinteraktion und Treffererkennung!

Konfigurieren Sie eine App, für die Ressourcenpakete für unterschiedliche Skalierungsfaktoren von UWP-Apps unterstützt werden sollen, wie folgt:

-   Erstellen Sie ein Unterverzeichnis (bzw. eine Dateiversion) für jeden zu unterstützenden Skalierungsfaktor (scale-100, scale-140 und scale-180).
-   Fügen Sie während der Entwicklung auf den Skalierungsfaktor zugeschnittene Kopien ALLER Ressourcen in jedes Skalierungsfaktor-Ressourcenverzeichnis ein, auch wenn sich diese für die einzelnen Skalierungsfaktoren nicht unterscheiden.
-   Achten Sie darauf, dass die Ressourcen in jedem Verzeichnis gleich benannt sind. Die Datei „menu\_option1.png“ sollte beispielsweise in den Verzeichnissen „\\scale-100“ und „\\scale-180“ denselben Namen haben, auch wenn der Inhalt unterschiedlich ist. Die Dateien sind werden in diesem Fall als „\\scale-100\\menu\_option1.pngֆ“ und „\\scale-140\\menu\_option1.png“ angezeigt.
    > **Hinweis**   Auch hier können Sie das Skalierungsfaktorsuffix bei Bedarf an den Dateinamen anhängen und die Dateien im selben Verzeichnis ablegen, z. B. \\assets\\menu\_option1\_scale-100.png und \\assets\\menu\_option1\_scale-140.png.

     

-   Verwenden Sie die APIs in [**Windows.ApplicationModel.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) zum Laden der Ressourcen. Verweise auf Ressourcen sollten generalisiert werden (kein Suffix), wobei die spezifische Skalierungsvariante weggelassen wird. Das System ruft die geeignete Skalierungsressource für die Anzeige und die Einstellungen des Benutzers ab.
-   Wählen Sie in Visual Studio 2015 die Option **PROJEKT -> Store -> Anwendungspaket erstellen...**, und erstellen Sie das Paket.

## Definieren von Ressourcenpaketen für DirectX-Featureebenen


Die DirectX-Featureebenen entsprechen den GPU-Featuresätzen für ältere und aktuelle Versionen von DirectX (speziell Direct3D). Dies beinhaltet Spezifikationen und Funktionen von Shadermodellen, Sprachunterstützung für Shader, Unterstützung für Texturkomprimierung und allgemeine Features der Grafikpipeline.

Für Ihr grundlegendes App-Paket sollten Sie eines der grundlegenden Formate für die Texturkomprimierung verwenden: BC1, BC2 oder BC3. Diese Formate können von allen UWP-Geräten genutzt werden, von normalen ARM-Plattformen bis zu dedizierten Arbeitsstationen mit mehreren GPUs und Mediencomputern.

Es ist ratsam, einem Ressourcenpaket Texturformatunterstützung für DirectX-Featureebene 10 oder höher hinzuzufügen, um lokalen Festplattenspeicher und Downloadbandbreite zu sparen. Dies ermöglicht die Verwendung moderner Komprimierungsschemas für Ebene 11, z. B. BC6H und BC7. (Weitere Informationen finden Sie unter [Texturblockkomprimierung in Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/hh308955).) Diese Formate sind für die hochaufgelösten Texturressourcen effizienter, die von modernen GPUs unterstützt werden. Zudem verbessern sie auf Highend-Plattformen das Erscheinungsbild, die Leistung und die Speicheranforderungen Ihres Spiels.

| DirectX-Featureebene | Unterstützte Texturkomprimierung |
|-----------------------|-------------------------------|
| 9                     | BC1, BC2, BC3                 |
| 10                    | BC4, BC5                      |
| 11                    | BC6H, BC7                     |

 

Von allen DirectX-Featureebenen werden außerdem unterschiedliche Shadermodellversionen unterstützt. Kompilierte Shaderressourcen können pro Featureebene erstellt werden und in Ressourcenpakete für DirectX-Featureebenen eingebunden werden. In Verbindung mit einigen neueren Shadermodellen können auch Ressourcen genutzt werden, z. B. Normalmaps, die für frühere Versionen von Shadermodellen nicht geeignet sind. Diese für Shadermodelle spezifischen Ressourcen sollten ebenfalls in ein Ressourcenpaket für DirectX-Featureebenen eingefügt werden.

Der Ressourcenmechanismus ist vorrangig auf die für Ressourcen unterstützten Texturformate ausgerichtet, sodass nur die drei allgemeinen Featureebenen unterstützt werden. Falls Sie separate Shader für Unterebenen (Punktversionen) wie „DX9\_1“ und „DX9\_3“ benötigen, müssen diese vom Code für die Ressourcenverwaltung und zum Rendern explizit behandelt werden.

Gehen Sie wie folgt vor, wenn Sie eine App konfigurieren, für die Ressourcenpakete für unterschiedliche DirectX-Featureebenen unterstützt werden sollen:

-   Erstellen Sie ein Unterverzeichnis (bzw. eine Dateiversion) für jede zu unterstützende DirectX-Featureebene (dxfl-dx9, dxfl-dx10 und dxfl-dx11).
-   Fügen Sie für die Featureebene spezifische Ressourcen während der Entwicklung in jedes Ressourcenverzeichnis für Featureebenen ein. Im Gegensatz zu Gebietsschemas und Skalierungsfaktoren können Sie im Spiel für jede Featureebene unterschiedliche Rendercodeverzweigungen verwenden. Falls Sie über Texturen, kompilierte Shader oder andere Ressourcen verfügen, die nur für eine Featureebene oder eine Teilmenge aller unterstützten Featureebenen verwendet werden, fügen Sie die entsprechenden Ressourcen nur in die Verzeichnisse für die Featureebenen ein, von denen sie genutzt werden. Beachten Sie für Ressourcen, die über alle Featureebenen hinweg geladen werden, dass für jedes Featureebenen-Ressourcenverzeichnis davon eine Version gleichen Namens vorhanden ist. Fügen Sie für eine von der Featureebene unabhängige Textur mit dem Namen „coolsign.dds“ beispielsweise die BC3-komprimierte Version in das Verzeichnis „\\dxfl-dx9“ und die BC7-komprimierte Version in das Verzeichnis „\\dxfl-dx11“ ein.
-   Stellen Sie sicher, dass alle Ressourcen (falls sie für mehrere Featureebenen verfügbar sind) in jedem Verzeichnis denselben Namen haben. Die Datei „coolsign.dds“ sollte beispielsweise in den Verzeichnissen „\\dxfl-dx9“ und „\\dxfl-dx11“ denselben Namen haben, auch wenn der Inhalt unterschiedlich ist. Die Dateien werden in diesem Fall als „\\dxfl-dx9\\coolsign.dds“ und „\\dxfl-dx11\\coolsign.dds“ angezeigt.
    > **Hinweis**   Auch hier können Sie das Featureebenensuffix bei Bedarf an den Dateinamen anhängen und die Dateien im selben Verzeichnis ablegen, z. B. \\textures\\coolsign\_dxfl-dx9.dds und \\textures\\coolsign\_dxfl-dx11.dds.

     

-   Deklarieren Sie die unterstützten DirectX-Featureebenen beim Konfigurieren der Grafikressourcen.
    ```cpp
    D3D_FEATURE_LEVEL featureLevels[] = 
    {
      D3D_FEATURE_LEVEL_11_1,
      D3D_FEATURE_LEVEL_11_0,
      D3D_FEATURE_LEVEL_10_1,
      D3D_FEATURE_LEVEL_10_0,
      D3D_FEATURE_LEVEL_9_3,
      D3D_FEATURE_LEVEL_9_1
    };
    ```

    ```cpp
    ComPtr<ID3D11Device> device;
    ComPtr<ID3D11DeviceContext> context;
    D3D11CreateDevice(
        nullptr,                    // Use the default adapter.
        D3D_DRIVER_TYPE_HARDWARE,
        0,                      // Use 0 unless it is a software device.
        creationFlags,          // defined above
        featureLevels,          // What the app will support.
        ARRAYSIZE(featureLevels),
        D3D11_SDK_VERSION,      // This should always be D3D11_SDK_VERSION.
        &device,                    // created device
        &m_featureLevel,            // The feature level of the device.
        &context                    // The corresponding immediate context.
    );
    ```

-   Verwenden Sie die APIs in [**Windows.ApplicationModel.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) zum Laden der Ressourcen. Verweise auf Ressourcen sollten generalisiert werden (kein Suffix), wobei die Featureebene weggelassen wird. Im Gegensatz zu Sprache und Skalierung bestimmt das System jedoch nicht automatisch, welche Featureebene für eine Anzeige optimal ist; dies bestimmen Sie selbst basierend auf Codelogik. Sobald Sie die richtige Ebene bestimmt haben, informieren Sie das Betriebssystem mithilfe der APIs über die bevorzugte Featureebene. Anschließend kann das System basierend auf dieser Einstellung die richtige Ressource abrufen. Dies ist ein Codebeispiel dafür, wie Ihre App über die aktuelle DirectX-Featureebene für die Plattform informiert wird:
    
    ```cpp
    // Set the current UI thread's MRT ResourceContext's DXFeatureLevel with the right DXFL. 

    Platform::String^ dxFeatureLevel;
        switch (m_featureLevel)
        {
        case D3D_FEATURE_LEVEL_9_1:
        case D3D_FEATURE_LEVEL_9_2:
        case D3D_FEATURE_LEVEL_9_3:
            dxFeatureLevel = L"DX9";
            break;
        case D3D_FEATURE_LEVEL_10_0:
        case D3D_FEATURE_LEVEL_10_1:
            dxFeatureLevel = L"DX10";
            break;
        default:
            dxFeatureLevel = L"DX11";
        }

        ResourceContext::SetGlobalQualifierValue(L"DXFeatureLevel", dxFeatureLevel);
    ```

    > **Hinweis**  Laden Sie die Textur im Code direkt entsprechend dem Namen (oder entsprechend dem Pfad unterhalb des Featureebenenverzeichnisses). Fügen Sie weder den Featureebenen-Verzeichnisnamen noch das Suffix ein. Laden Sie beispielsweise „textures\\coolsign.dds“ und nicht „dxfl-dx11\\textures\\coolsign.dds“ oder „textures\\coolsign\_dxfl-dx11.dds“.

     

-   Suchen Sie jetzt mithilfe der [**ResourceManager**](https://msdn.microsoft.com/library/windows/apps/br206078)-Klasse nach der passenden Datei für die aktuelle DirectX-Featureebene. Die **ResourceManager**-Klasse gibt ein [**ResourceMap**](https://msdn.microsoft.com/library/windows/apps/br206089)-Objekt zurück, das Sie mit [**ResourceMap::GetValue**](https://msdn.microsoft.com/library/windows/apps/br206098) (oder [**ResourceMap::TryGetValue**](https://msdn.microsoft.com/library/windows/apps/jj655438)) und einem bereitgestellten [**ResourceContext**](https://msdn.microsoft.com/library/windows/apps/br206064)-Objekt abfragen können. Daraufhin wird ein [**ResourceCandidate**](https://msdn.microsoft.com/library/windows/apps/br206051)-Objekt zurückgegeben, das der DirectX-Featureebene am ehesten entspricht, die durch den Aufruf von [**SetGlobalQualifierValue**](https://msdn.microsoft.com/library/windows/apps/mt622101) angegeben wurde.
    
    ```cpp
    // An explicit ResourceContext is needed to match the DirectX feature level for the display on which the current view is presented.

    auto resourceContext = ResourceContext::GetForCurrentView();
    auto mainResourceMap = ResourceManager::Current->MainResourceMap;

    // For this code example, loader is a custom ref class used to load resources.
    // You can use the BasicLoader class from any of the 8.1 DirectX samples similarly.


    auto possibleResource = mainResourceMap->GetValue(
        L"Files/BumpPixelShader.cso",
        resourceContext
    );
    Platform::String^ resourceName = possibleResource->ValueAsString;
    ```

-   Wählen Sie in Visual Studio 2015 die Option **PROJEKT -> Store -> Anwendungspaket erstellen...**, und erstellen Sie das Paket.
-   Achten Sie darauf, dass Sie App-Bündel in den Einstellungen des „package.appxmanifest“-Manifests aktivieren.

## Verwandte Themen


* [Definition der App-Ressourcen](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321)
* [Verpacken von Apps](https://msdn.microsoft.com/library/windows/apps/mt270969)
* [App-Objekt-Manager (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767)

 

 






<!--HONumber=May16_HO2-->


