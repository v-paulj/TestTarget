---
title: Laden von Ressourcen im DirectX-Spiel
description: meisten Spiele Ressourcen Objekte (Shader, Texturen, vordefinierte Gitter, andere Grafikdaten) aus lokalem Speicher oder über anderen Datenstrom geladen
ms.assetid: e45186fa-57a3-dc70-2b59-408bff0c0b41
---

# Laden von Ressourcen im DirectX-Spiel


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

In den meisten Spielen werden Ressourcen und Objekte (wie Shader, Texturen, vordefinierte Gitter oder andere Grafikdaten) an bestimmten Stellen aus dem lokalen Speicher oder über einen anderen Datenstrom geladen. Dieser Abschnitt enthält eine allgemeine Übersicht über die Aspekte, die beim Laden dieser Dateien zur Verwendung in Ihrem UWP-Spiel zu berücksichtigen sind.

Es kann beispielsweise sein, dass die Gitter für polygonale Objekte im Spiel mit einem anderen Tool erstellt und in einem bestimmten Format exportiert wurden. Die kann auch besonders für Texturen der Fall sein: Während eine flache, unkomprimierte Bitmapgrafik in der Regel von den meisten Tools geschrieben und von den meisten Grafik-APIs verarbeitet werden kann, ist dieses Format zur Verwendung im Spiel möglicherweise sehr ineffizient. Sie werden durch die grundlegenden Schritte des Ladens von drei unterschiedlichen Arten von Grafikressourcen geführt, die in Verbindung mit Direct3D verwendet werden: Gitter (Modelle), Texturen (Bitmaps) und kompilierte Shaderobjekte.

## Wissenswertes


### Technologien

-   Parallel Patterns Library (PPL)

### Voraussetzungen

-   Informationen zur grundlegenden Windows-Runtime
-   Grundlegendes zu asynchronen Aufgaben
-   Grundbegriffe der Programmierung von 3D-Grafiken

Dieses Beispiel enthält auch drei Codedateien zum Laden und Verwalten von Ressourcen. Sie werden in diesem Thema häufiger auf die in diesen Dateien definierten Codeobjekte treffen.

-   BasicLoader.h/.cpp
-   BasicReaderWriter.h/.cpp
-   DDSTextureLoader.h/.cpp

Sie können über die folgenden Links auf den vollständigen Code für diese Beispiele zugreifen.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Complete code for BasicLoader](complete-code-for-basicloader.md)</p></td>
<td align="left"><p>Vollständiger Code für eine Klasse und Methoden, die gitterförmige Grafikobjekte konvertieren und in den Speicher laden.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Complete code for BasicReaderWriter](complete-code-for-basicreaderwriter.md)</p></td>
<td align="left"><p>Vollständiger Code für eine Klasse und Methoden zum allgemeinen Lesen und Schreiben von Binärdatendateien. Wird von der [BasicLoader](complete-code-for-basicloader.md)-Klasse verwendet.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Complete code for DDSTextureLoader](complete-code-for-ddstextureloader.md)</p></td>
<td align="left"><p>Vollständiger Code für eine Klasse und Methode, die eine DDS-Textur aus dem Speicher lädt.</p></td>
</tr>
</tbody>
</table>

 

## Anweisungen

### Asynchrones Laden

Das asynchrone Laden wird mithilfe der **task**-Vorlage der Parallel Patterns Library (PPL) durchgeführt. Eine **task** enthält einen Methodenaufruf gefolgt von einer Lambda-Funktion, mit der die Ergebnisse des asynchronen Aufrufs nach dessen Abschluss verarbeitet werden. Dabei wird normalerweise das folgende Format verwendet:

`task<generic return type>(async code to execute).then((parameters for lambda){ lambda code contents });`.

Aufgaben können mithilfe der **.then()**-Syntax verkettet werden. Wenn ein Vorgang abgeschlossen ist, kann ein anderer asynchroner Vorgang ausgeführt werden, der von den Ergebnissen des vorherigen Vorgangs abhängt. Auf diese Weise können Sie komplexe Objekte in separaten Threads für den Spieler nahezu unbemerkt laden, konvertieren und verwalten.

Weitere Informationen finden Sie unter [Asynchrone Programmierung in C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

Als Nächstes werfen wir einen Blick auf die Grundstruktur zum Deklarieren und Erstellen einer Methode für das asynchrone Laden von Dateien: **ReadDataAsync**.

```cpp
#include <ppltasks.h>

// ...
concurrency::task<Platform::Array<byte>^> ReadDataAsync(
        _In_ Platform::String^ filename);

// ...

using concurrency;

task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
    )
{
    return task<StorageFile^>(m_location->GetFileAsync(filename)).then([=](StorageFile^ file)
    {
        return FileIO::ReadBufferAsync(file);
    }).then([=](IBuffer^ buffer)
    {
        auto fileData = ref new Platform::Array<byte>(buffer->Length);
        DataReader::FromBuffer(buffer)->ReadBytes(fileData);
        return fileData;
    });
}
```

Wenn in Ihrem Code basierend auf diesem Code die oben definierte **ReadDataAsync**-Methode aufgerufen wird, wird eine Aufgabe zum Auslesen eines Puffers aus dem Dateisystem erstellt. Nach Abschluss dieses Vorgangs übernimmt eine verkettete Aufgabe den Puffer und führt für die Bytes dieses Puffers einen Datenstrom in ein Array durch, indem der statische [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119)-Typ verwendet wird.

```cpp
m_basicReaderWriter = ref new BasicReaderWriter();

// ...
return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
      // Perform some operation with the data when the async load completes.          
    });
```

Dies ist der Aufruf an **ReadDataAsync**. Nach Abschluss des Vorgangs empfängt der Code ein Array mit Bytes, die aus der bereitgestellten Datei ausgelesen wurden. Da **ReadDataAsync** selbst als Aufgabe definiert ist, können Sie mithilfe einer Lambda-Funktion einen bestimmten Vorgang durchführen, wenn das Bytearray zurückgegeben wird. Dies kann beispielsweise die Übergabe dieser Bytedaten an eine geeignete DirectX-Funktion sein.

Wenn Ihr Spiel nicht zu komplex aufgebaut ist, können Sie die Ressourcen mit einer Methode dieser Art laden, wenn Benutzer das Spiel starten. Sie können diesen Schritt ausführen, bevor Sie an einem bestimmten Punkt der Aufrufabfolge Ihrer [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505)-Implementierung die Hauptspielschleife starten. Sie rufen auch hier wieder die Methoden zum Laden der Ressourcen asynchron auf, damit das Spiel schneller gestartet werden kann und Spieler nicht auf den Abschluss des Ladevorgangs warten müssen, bevor die ersten Interaktionen möglich sind.

Das eigentliche Spiel sollten jedoch erst richtig gestartet werden, nachdem das asynchrone Laden vollständig abgeschlossen ist! Erstellen Sie eine Methode, mit der angezeigt wird, wann der Ladevorgang abgeschlossen ist, z. B. ein bestimmtes Feld. Verwenden Sie dann die Lambda-Funktionen für die Lademethoden, um den Abschluss des Vorgangs anzuzeigen. Überprüfen Sie die Variable, bevor Sie Komponenten starten, in denen die geladenen Ressourcen verwendet werden.

In diesem Beispiel werden die in „BasicLoader.cpp“ definierten asynchronen Methoden verwendet, um Shader, ein Gitter und eine Textur zu laden, wenn das Spiel gestartet wird. Beachten Sie, dass für das Spielobjekt ein bestimmtes Feld **m\_loadingComplete** festgelegt wird, nachdem alle Lademethoden abgeschlossen sind.

```cpp
void ResourceLoading::CreateDeviceResources()
{
    // DirectXBase is a common sample class that implements a basic view provider. 
    
    DirectXBase::CreateDeviceResources(); 

    // ...

    // This flag will keep track of whether or not all application
    // resources have been loaded.  Until all resources are loaded,
    // only the sample overlay will be drawn on the screen.
    m_loadingComplete = false;

    // Create a BasicLoader, and use it to asynchronously load all
    // application resources.  When an output value becomes non-null,
    // this indicates that the asynchronous operation has completed.
    BasicLoader^ loader = ref new BasicLoader(m_d3dDevice.Get());

    auto loadVertexShaderTask = loader->LoadShaderAsync(
        "SimpleVertexShader.cso",
        nullptr,
        0,
        &m_vertexShader,
        &m_inputLayout
        );

    auto loadPixelShaderTask = loader->LoadShaderAsync(
        "SimplePixelShader.cso",
        &m_pixelShader
        );

    auto loadTextureTask = loader->LoadTextureAsync(
        "reftexture.dds",
        nullptr,
        &m_textureSRV
        );

    auto loadMeshTask = loader->LoadMeshAsync(
        "refmesh.vbo",
        &m_vertexBuffer,
        &m_indexBuffer,
        nullptr,
        &m_indexCount
        );

    // The && operator can be used to create a single task that represents
    // a group of multiple tasks. The new task's completed handler will only
    // be called once all associated tasks have completed. In this case, the
    // new task represents a task to load various assets from the package.
    (loadVertexShaderTask && loadPixelShaderTask && loadTextureTask && loadMeshTask).then([=]()
    {
        m_loadingComplete = true;
    });

    // Create constant buffers and other graphics device-specific resources here.
}
```

Die Aufgaben werden mithilfe des Operators „&&“ aggregiert. So wird die Lambda-Funktion, mit der das Kennzeichen für „Laden abgeschlossen“ festgelegt wird, erst ausgelöst, wenn alle Aufgaben abgeschlossen sind. Beachten Sie, dass Sie bei Verwendung mehrerer Kennzeichen auch Racebedingungen nutzen können. Wenn mit der Lambda-Funktion beispielsweise zwei Kennzeichen hintereinander für denselben Wert festgelegt werden, wird von einem anderen Thread möglicherweise nur das erste Kennzeichen erkannt, falls die Prüfung auf Kennzeichen vor dem Festlegen des zweiten Kennzeichens durchgeführt wird.

Sie haben erfahren, wie Ressourcendateien asynchron geladen werden. Das synchrone Laden von Dateien ist wesentlich einfacher. Entsprechende Beispiele dafür finden Sie unter [Vollständiger Code für BasicReaderWriter](complete-code-for-basicreaderwriter.md) und [Vollständiger Code für BasicLoader](complete-code-for-basicloader.md).

Für unterschiedliche Arten von Ressourcen und Objekten ist häufig eine zusätzliche Verarbeitung oder Konvertierung erforderlich, bevor Sie diese in Ihrer Grafikpipeline verwenden. Wir sehen uns drei spezielle Typen von Ressourcen an: Gitter, Texturen und Shader.

### Laden von Gittern

Bei Gittern handelt es sich um Vertexdaten, die entweder mithilfe von Prozeduren im Code des Spiels generiert oder aus einer anderen App (wie 3DStudio MAX oder Alias WaveFront) oder einem anderen Tool in eine Datei exportiert werden. Diese Gitter stellen die Modelle Ihres Spiels dar, von einfachen Grundtypen wie Würfel und Kugeln bis zu Autos, Häusern und Figuren. Je nach Format sind darin auch Farb- und Animationsdaten enthalten. Wir konzentrieren uns hier auf Gitter, die nur Vertexdaten enthalten.

Um ein Gitter richtig laden zu können, müssen Sie das Format der Daten in der Datei für das Gitter kennen. Mit dem obigen einfachen **BasicReaderWriter**-Typ werden die Daten lediglich als Bytestream gelesen. Dabei ist dem Typ nicht bekannt, dass die Bytedaten ein Gitter darstellen, geschweige denn, dass es sich um ein bestimmtes Gitterformat handelt, das von einer anderen Anwendung exportiert wurde! Sie müssen die Konvertierung durchführen, wenn Sie die Gitterdaten in den Arbeitsspeicher stellen.

(Sie sollten stets versuchen, Objektdaten in einem Format zu verpacken, das möglichst genau mit der internen Darstellung übereinstimmt. Dadurch wird der Ressourceneinsatz verringert und Zeit gespart.)

Als Nächstes werden die Bytedaten aus der Datei des Gitters abgerufen. Im Beispiel wird angenommen, dass die Datei in einem speziellen Format für Beispiele vorliegt und die Erweiterung ".vbo" aufweist. (Wieder gilt, dass dieses Format nicht dem OpenGL-VBO-Format entspricht.) Jeder Vertex ist selbst dem **BasicVertex**-Typ zugeordnet. Dies ist eine Struktur, die im Code für das Konvertierungstool obj2vbo definiert ist. Die Vertexdaten in der VBO-Datei haben das folgende Layout:

-   Die ersten 32 Bits (4 Byte) des Datenstroms enthalten die Anzahl an Vertices (numVertices) im Gitter, dargestellt als uint32-Wert.
-   Die nächsten 32 Bits (4 Byte) des Datenstroms enthalten die Anzahl an Indizes (numIndices) im Gitter, dargestellt als uint32-Wert.
-   Die nachfolgenden Bits (numVertices \* sizeof(**BasicVertex**)) enthalten die Vertexdaten.
-   Die letzten Bits (numIndices \* 16) enthalten die Indexdaten, dargestellt als Abfolge von uint16-Werten.

Entscheidend ist: Sie sollten das Bitebenenlayout der geladenen Gitterdaten kennen. Stellen Sie außerdem sicher, dass für Endian-Konsistenz gesorgt ist. Für alle Windows 8-Plattformen wird „Little Endian“ verwendet.

Im Beispiel wird die „CreateMesh“-Methode aus der **LoadMeshAsync**-Methode aufgerufen, um diese Interpretation auf Bitebene durchzuführen.

```cpp
task<void> BasicLoader::LoadMeshAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ meshData)
    {
        CreateMesh(
            meshData->Data,
            vertexBuffer,
            indexBuffer,
            vertexCount,
            indexCount,
            filename
            );
    });
}
```

**CreateMesh** interpretiert die aus der Datei geladenen Bytedaten und erstellt einen Vertexpuffer und einen Indexpuffer für das Gitter, indem die Vertex- bzw. Indexliste an [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) übergeben und entweder D3D11\_BIND\_VERTEX\_BUFFER oder D3D11\_BIND\_INDEX\_BUFFER angegeben wird. In **BasicLoader** wird der folgende Code verwendet:

```cpp
void BasicLoader::CreateMesh(
    _In_ byte* meshData,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount,
    _In_opt_ Platform::String^ debugName
    )
{
    // The first 4 bytes of the BasicMesh format define the number of vertices in the mesh.
    uint32 numVertices = *reinterpret_cast<uint32*>(meshData);

    // The following 4 bytes define the number of indices in the mesh.
    uint32 numIndices = *reinterpret_cast<uint32*>(meshData + sizeof(uint32));

    // The next segment of the BasicMesh format contains the vertices of the mesh.
    BasicVertex* vertices = reinterpret_cast<BasicVertex*>(meshData + sizeof(uint32) * 2);

    // The last segment of the BasicMesh format contains the indices of the mesh.
    uint16* indices = reinterpret_cast<uint16*>(meshData + sizeof(uint32) * 2 + sizeof(BasicVertex) * numVertices);

    // Create the vertex and index buffers with the mesh data.

    D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
    vertexBufferData.pSysMem = vertices;
    vertexBufferData.SysMemPitch = 0;
    vertexBufferData.SysMemSlicePitch = 0;
    CD3D11_BUFFER_DESC vertexBufferDesc(numVertices * sizeof(BasicVertex), D3D11_BIND_VERTEX_BUFFER);

    m_d3dDevice->CreateBuffer(
            &vertexBufferDesc,
            &vertexBufferData,
            vertexBuffer
            );
    
    D3D11_SUBRESOURCE_DATA indexBufferData = {0};
    indexBufferData.pSysMem = indices;
    indexBufferData.SysMemPitch = 0;
    indexBufferData.SysMemSlicePitch = 0;
    CD3D11_BUFFER_DESC indexBufferDesc(numIndices * sizeof(uint16), D3D11_BIND_INDEX_BUFFER);
    
    m_d3dDevice->CreateBuffer(
            &indexBufferDesc,
            &indexBufferData,
            indexBuffer
            );
  
    if (vertexCount != nullptr)
    {
        *vertexCount = numVertices;
    }
    if (indexCount != nullptr)
    {
        *indexCount = numIndices;
    }
}
```

Normalerweise erstellen Sie für jedes im Spiel eingesetzte Gitter ein Vertex/Index-Pufferpaar. Wo und wann Sie die Gitter laden, ist Ihre Entscheidung. Falls Sie viele Gitter verwenden, kann es ratsam sein, nur an bestimmten Stellen im Spiel einige dieser Gitter vom Datenträger zu laden, z. B. während spezieller vordefinierter Ladezustände. Für große Gitter, z. B. Geländedaten, können Sie die Vertices per Datenstrom aus einem Cache laden. Das ist jedoch eine komplexere Prozedur, die den Rahmen dieses Themas sprengt.

Wieder gilt: Es ist wichtig, dass Sie mit dem Format Ihrer Vertexdaten vertraut sind! Es gibt sehr viele Möglichkeiten, Vertexdaten in den Tools darzustellen, die zum Erstellen von Modellen verwendet werden. Außerdem haben Sie viele unterschiedliche Optionen, was die Darstellung des Eingabelayouts der Vertexdaten für Direct3D betrifft, z. B. Dreieckslisten und -ketten. Weitere Informationen zu Vertexdaten finden Sie unter [Einführung in Puffer in Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476898) und [Grundtypen](https://msdn.microsoft.com/library/windows/desktop/bb147291).

Als Nächstes sehen wir uns das Laden von Texturen an.

### Laden von Texturen

Das am häufigsten in Spielen verwendete Objekt – und das Objekt mit den meisten Dateien auf dem Datenträger und im Arbeitsspeicher – sind Texturen. Wie Gitter auch, können Texturen in vielen unterschiedlichen Formaten vorliegen, und Sie können Texturen in ein Format konvertieren, das von Direct3D beim Laden verwendet werden kann. Außerdem gibt es viele verschiedene Arten von Texturen, die zum Erzeugen unterschiedlicher Effekte eingesetzt werden. MIP-Ebenen für Texturen können verwendet werden, um das Aussehen und die Leistung von entfernten Objekten zu verbessern. Verschmutzungs- und Beleuchtungsmaps werden verwendet, um Effekte und Details über einer Basistextur anzuordnen. Normale Maps werden zur Berechnung der Beleuchtung pro Pixel eingesetzt. In modernen Spielen kann eine typische Szene über Tausende einzelner Texturen verfügen, die im Code alle effektiv verwaltet werden müssen!

Wieder analog zu Gittern gibt es einige spezielle Formate, die mit dem Ziel einer effizienteren Arbeitsspeichernutzung eingesetzt werden. Da für Texturen häufig ein großer Anteil des GPU-Speichers (und Systemspeichers) verbraucht wird, werden diese Daten meist komprimiert. Es besteht keine Notwendigkeit, für die Texturen Ihres Spiels die Komprimierung zu verwenden. Sie können beliebige Algorithmen für die Komprimierung bzw. Dekomprimierung nutzen, solange Sie für die Direct3D-Shader Daten in einem geeigneten Format bereitstellen (z. B. eine [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)-Bitmap).

Direct3D bietet Unterstützung für die DXT-Texturkomprimierungsalgorithmen. Es kann jedoch sein, dass nicht alle DXT-Formate von der Grafikhardware des Spielers unterstützt werden. DDS-Dateien enthalten DXT-Texturen (sowie andere Texturkomprimierungsformate) und weisen die Erweiterung ".dds" auf.

Eine DDS-Datei ist eine Binärdatei mit den folgenden Informationen:

-   DWORD ("Magic Number") mit dem vierstelligen Codewert "DDS" (0x20534444)

-   Beschreibung der Daten in der Datei

    Die Daten werden mit einer Headerbeschreibung per [**DDS\_HEADER**](https://msdn.microsoft.com/library/windows/desktop/bb943982) beschrieben. Das Pixelformat wird mit [**DDS\_PIXELFORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb943984) definiert. Beachten Sie, dass die Strukturen **DDS\_HEADER** und **DDS\_PIXELFORMAT** die veralteten DirectDraw 7-Strukturen „DDSURFACEDESC2“, „DDSCAPS2“ und „DDPIXELFORMAT“ ersetzen. **DDS\_HEADER** ist die binäre Entsprechung von „DDSURFACEDESC2“ und „DDSCAPS2“. **DDS\_PIXELFORMAT** ist die binäre Entsprechung von „DDPIXELFORMAT“.

    ```cpp
    DWORD               dwMagic;
    DDS_HEADER          header;
    ```

    Wenn der Wert von **dwFlags** in [**DDS\_PIXELFORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb943984) auf DDPF\_FOURCC und **dwFourCC** auf „DX10“ festgelegt ist, ist eine zusätzliche [**DDS\_HEADER\_DXT10**](https://msdn.microsoft.com/library/windows/desktop/bb943983)-Struktur vorhanden. Darin werden Texturarrays oder DXGI-Formate aufgenommen, die nicht als RGB-Pixelformat ausgedrückt werden können, z. B. Gleitkommaformate, sRGB-Formate usw. Wenn die **DDS\_HEADER\_DXT10**-Struktur vorhanden ist, sieht die gesamte Datenbeschreibung wie folgt aus.

    ```cpp
    DWORD               dwMagic;
    DDS_HEADER          header;
    DDS_HEADER_DXT10    header10;
    ```

-   Ein Zeiger auf ein Bytearray, in dem die Hauptoberflächendaten enthalten sind.
    ```cpp
    BYTE bdata[]
    ```

-   Ein Zeiger auf ein Bytearray, in dem die verbleibenden Oberflächen enthalten sind, z. B. Mipmap-Ebenen, Seiten einer Würfelmap, Tiefen in einer Volumentextur. Weitere Informationen zum Layout der DDS-Datei finden Sie unter den folgenden Links: [texture](https://msdn.microsoft.com/library/windows/desktop/bb205578) [Cubemap](https://msdn.microsoft.com/library/windows/desktop/bb205577) oder [Volumentextur](https://msdn.microsoft.com/library/windows/desktop/bb205579).

    ```cpp
    BYTE bdata2[]
    ```

In vielen Tools erfolgt der Export im DDS-Format. Falls Sie nicht über ein Tool verfügen, mit dem Exporte der Textur in diesem Format möglich sind, können Sie ein Tool erstellen. Weitere Informationen zum DDS-Format und zur Verwendung im Code finden Sie unter [Programmieranleitung für DDS](https://msdn.microsoft.com/library/windows/desktop/bb943991). In unserem Beispiel wird DDS verwendet.

Wie bei anderen Ressourcentypen auch, lesen Sie die Daten aus einer Datei als Bytestream aus. Nachdem die Aufgabe für den Ladevorgang abgeschlossen ist, führt der Lambda-Aufruf Code aus (**CreateTexture**-Methode), um den Bytestream in ein für Direct3D geeignetes Format zu bringen.

```cpp
task<void> BasicLoader::LoadTextureAsync(
    _In_ Platform::String^ filename,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ textureData)
    {
        CreateTexture(
            GetExtension(filename) == "dds",
            textureData->Data,
            textureData->Length,
            texture,
            textureView,
            filename
            );
    });
}
```

Im vorherigen Codeausschnitt wird mit der Lambda-Funktion geprüft, ob der Dateiname die Erweiterung "dds" aufweist. Wenn ja, wird angenommen, dass es sich um eine DDS-Textur handelt. Wenn nicht, verwenden Sie die Windows-Bilderstellungskomponenten-APIs (WIC), um das Format zu ermitteln und die Daten als Bitmap zu decodieren. In beiden Fällen ist das Ergebnis eine [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)-Bitmap (oder ein Fehler).

```cpp
void BasicLoader::CreateTexture(
    _In_ bool decodeAsDDS,
    _In_reads_bytes_(dataSize) byte* data,
    _In_ uint32 dataSize,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView,
    _In_opt_ Platform::String^ debugName
    )
{
    ComPtr<ID3D11ShaderResourceView> shaderResourceView;
    ComPtr<ID3D11Texture2D> texture2D;

    if (decodeAsDDS)
    {
        ComPtr<ID3D11Resource> resource;

        if (textureView == nullptr)
        {
            CreateDDSTextureFromMemory(
                m_d3dDevice.Get(),
                data,
                dataSize,
                &resource,
                nullptr
                );
        }
        else
        {
            CreateDDSTextureFromMemory(
                m_d3dDevice.Get(),
                data,
                dataSize,
                &resource,
                &shaderResourceView
                );
        }

        resource.As(&texture2D);
    }
    else
    {
        if (m_wicFactory.Get() == nullptr)
        {
            // A WIC factory object is required in order to load texture
            // assets stored in non-DDS formats.  If BasicLoader was not
            // initialized with one, create one as needed.
            CoCreateInstance(
                    CLSID_WICImagingFactory,
                    nullptr,
                    CLSCTX_INPROC_SERVER,
                    IID_PPV_ARGS(&m_wicFactory));
        }

        ComPtr<IWICStream> stream;
        m_wicFactory->CreateStream(&stream);

        stream->InitializeFromMemory(
                data,
                dataSize);

        ComPtr<IWICBitmapDecoder> bitmapDecoder;
        m_wicFactory->CreateDecoderFromStream(
                stream.Get(),
                nullptr,
                WICDecodeMetadataCacheOnDemand,
                &bitmapDecoder);

        ComPtr<IWICBitmapFrameDecode> bitmapFrame;
        bitmapDecoder->GetFrame(0, &bitmapFrame);

        ComPtr<IWICFormatConverter> formatConverter;
        m_wicFactory->CreateFormatConverter(&formatConverter);

        formatConverter->Initialize(
                bitmapFrame.Get(),
                GUID_WICPixelFormat32bppPBGRA,
                WICBitmapDitherTypeNone,
                nullptr,
                0.0,
                WICBitmapPaletteTypeCustom);

        uint32 width;
        uint32 height;
        bitmapFrame->GetSize(&width, &height);

        std::unique_ptr<byte[]> bitmapPixels(new byte[width * height * 4]);
        formatConverter->CopyPixels(
                nullptr,
                width * 4,
                width * height * 4,
                bitmapPixels.get());

        D3D11_SUBRESOURCE_DATA initialData;
        ZeroMemory(&initialData, sizeof(initialData));
        initialData.pSysMem = bitmapPixels.get();
        initialData.SysMemPitch = width * 4;
        initialData.SysMemSlicePitch = 0;

        CD3D11_TEXTURE2D_DESC textureDesc(
            DXGI_FORMAT_B8G8R8A8_UNORM,
            width,
            height,
            1,
            1
            );

        m_d3dDevice->CreateTexture2D(
                &textureDesc,
                &initialData,
                &texture2D);

        if (textureView != nullptr)
        {
            CD3D11_SHADER_RESOURCE_VIEW_DESC shaderResourceViewDesc(
                texture2D.Get(),
                D3D11_SRV_DIMENSION_TEXTURE2D
                );

            m_d3dDevice->CreateShaderResourceView(
                    texture2D.Get(),
                    &shaderResourceViewDesc,
                    &shaderResourceView);
        }
    }


    if (texture != nullptr)
    {
        *texture = texture2D.Detach();
    }
    if (textureView != nullptr)
    {
        *textureView = shaderResourceView.Detach();
    }
}
```

Nach Abschluss dieses Codes befindet sich ein [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)-Objekt im Arbeitsspeicher, das aus einer Bilddatei geladen wurde. Wie bei Gittern auch, enthalten Ihr Spiel und die einzelnen Szenen eine große Zahl dieser Objekte. Es ist ratsam, pro Szene oder Level Caches für Texturen einzurichten, auf die regelmäßig zugegriffen wird, anstatt alle Texturen zu Beginn des Spiels oder eines Levels zu laden.

(Die **CreateDDSTextureFromMemory**-Methode, die im obigen Beispiel aufgerufen wird, ist in ganzer Länge unter [Vollständiger Code für DDSTextureLoader](complete-code-for-ddstextureloader.md) zu finden.)

Außerdem können einzelne Texturen oder Texturdesigns bestimmten Gitterpolygonen oder Oberflächen zugeordnet sein. Diese Zuordnungsdaten werden normalerweise mit dem Tool exportiert, das von einem Spezialisten oder Designer zum Erstellen des Modells und der Texturen verwendet wurde. Stellen Sie sicher, dass Sie beim Laden der exportierten Daten auch diese Informationen erfassen. Sie benötigen diese Daten, um die richtigen Texturen den entsprechenden Oberflächen zuzuordnen, wenn Sie die Fragmentschattierung erstellen.

### Laden von Shadern

Bei Shadern handelt es sich um kompilierte High Level Shader Language (HLSL)-Dateien, die in den Arbeitsspeicher geladen und an bestimmten Stellen der Grafikpipeline aufgerufen werden. Die am häufigsten verwendeten und wichtigsten Shader sind die Vertex- und Pixel-Shader, mit denen die einzelnen Vertices des Gitters bzw. die Pixel in den Viewports der Szenen verarbeitet werden. Der HLSL-Code wird ausgeführt, um die Geometrie zu transformieren, Beleuchtungseffekte und Texturen anzuwenden und die Nachbearbeitung der gerenderten Szene durchzuführen.

Ein Direct3D-Spiel kann über eine Reihe unterschiedlicher Shader verfügen, die jeweils zu einer separaten CSO-Datei (Compiled Shader Object, .cso) kompiliert werden. Normalerweise sind nicht so viele Dateien vorhanden, dass diese dynamisch geladen werden müssen. In den meisten Fällen können Sie die Dateien einfach zu Beginn des Spiels oder pro Level (z. B. einen Shader für Regeneffekte) laden.

Im Code der **BasicLoader**-Klasse werden einige Überladungen für unterschiedliche Shader bereitgestellt, einschließlich Vertex-, Geometry-, Pixel- und Hull-Shader. Im folgenden Code ist ein Beispiel für Pixel-Shader enthalten. (Den gesamten Code finden Sie unter [Vollständiger Code für BasicLoader](complete-code-for-basicloader.md).)

```cpp
concurrency::task<void> LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11PixelShader** shader
    );

// ...

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11PixelShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        
       m_d3dDevice->CreatePixelShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader);
    });
}

```

In diesem Beispiel verwenden Sie die **BasicReaderWriter**-Instanz (**m\_basicReaderWriter**), um die bereitgestellte kompilierte Shaderobjektdatei (.cso) als Bytestream einzulesen. Nach Abschluss der Aufgabe ruft die Lambda-Funktion [**ID3D11Device::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513) mit den aus der Datei geladenen Bytedaten auf. In Ihrem Rückruf muss ein Kennzeichen dafür festgelegt werden, dass der Ladevorgang erfolgreich war, und der Code muss dieses Kennzeichen überprüfen, bevor der Shader ausgeführt wird.

Vertex-Shader sind etwas komplexer. Für einen Vertex-Shader laden Sie zusätzlich ein separates Eingabelayout, mit dem die Vertexdaten definiert werden. Mit dem folgenden Code kann ein Vertex-Shader zusammen mit einem benutzerdefinierten Vertexeingabelayout asynchron geladen werden. Achten Sie darauf, dass die aus den Gittern geladenen Vertexinformationen von diesem Eingabelayout richtig dargestellt werden können!

Wir erstellen das Eingabelayout, bevor der Vertex-Shader geladen wird.

```cpp
void BasicLoader::CreateInputLayout(
    _In_reads_bytes_(bytecodeSize) byte* bytecode,
    _In_ uint32 bytecodeSize,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC* layoutDesc,
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11InputLayout** layout
    )
{
    if (layoutDesc == nullptr)
    {
        // If no input layout is specified, use the BasicVertex layout.
        const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
        {
            { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
            { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
            { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT,    0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
        };

        m_d3dDevice->CreateInputLayout(
                basicVertexLayoutDesc,
                ARRAYSIZE(basicVertexLayoutDesc),
                bytecode,
                bytecodeSize,
                layout);
    }
    else
    {
        m_d3dDevice->CreateInputLayout(
                layoutDesc,
                layoutDescNumElements,
                bytecode,
                bytecodeSize,
                layout);
    }
}
```

In diesem speziellen Layout werden für jeden Vertex vom Vertex-Shader die folgenden Daten verarbeitet:

-   Eine 3D-Koordinatenposition (x, y, z) im Koordinatenbereich des Modells, dargestellt in Form von drei 32-Bit-Gleitkommawerten.
-   Ein normaler Vektor für den Vertex, ebenfalls dargestellt in Form von drei 32-Bit-Gleitkommawerten.
-   Ein transformierter 2D-Texturkoordinatenwert (u, v), dargestellt als 32-Bit-Gleitkommawertpaar.

Diese Eingabeelemente pro Vertex werden als [HLSL-Semantik](https://msdn.microsoft.com/library/windows/desktop/bb509647) bezeichnet. Es handelt sich dabei um eine Gruppe definierter Register, die zum Übergeben von Daten in das bzw. aus dem kompilierten Shaderobjekt verwendet werden. Die Pipeline führt den Vertex-Shader einmal für jeden Vertex des geladenen Gitters aus. Mit der Semantik wird die Eingabe in den Vertex-Shader (sowie auch die Ausgabe) während der Ausführung definiert, und diese Daten werden dann für die Berechnungen pro Vertex im HLSL-Code des Shaders bereitgestellt.

Laden Sie als Nächstes das Vertex-Shaderobjekt.

```cpp
concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
        _In_ uint32 layoutDescNumElements,
        _Out_ ID3D11VertexShader** shader,
        _Out_opt_ ID3D11InputLayout** layout
        );

// ...

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11VertexShader** shader,
    _Out_opt_ ID3D11InputLayout** layout
    )
{
    // This method assumes that the lifetime of input arguments may be shorter
    // than the duration of this task.  In order to ensure accurate results, a
    // copy of all arguments passed by pointer must be made.  The method then
    // ensures that the lifetime of the copied data exceeds that of the task.

    // Create copies of the layoutDesc array as well as the SemanticName strings,
    // both of which are pointers to data whose lifetimes may be shorter than that
    // of this method's task.
    shared_ptr<vector<D3D11_INPUT_ELEMENT_DESC>> layoutDescCopy;
    shared_ptr<vector<string>> layoutDescSemanticNamesCopy;
    if (layoutDesc != nullptr)
    {
        layoutDescCopy.reset(
            new vector<D3D11_INPUT_ELEMENT_DESC>(
                layoutDesc,
                layoutDesc + layoutDescNumElements
                )
            );

        layoutDescSemanticNamesCopy.reset(
            new vector<string>(layoutDescNumElements)
            );

        for (uint32 i = 0; i < layoutDescNumElements; i++)
        {
            layoutDescSemanticNamesCopy->at(i).assign(layoutDesc[i].SemanticName);
        }
    }

    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
       m_d3dDevice->CreateVertexShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader);

        if (layout != nullptr)
        {
            if (layoutDesc != nullptr)
            {
                // Reassign the SemanticName elements of the layoutDesc array copy to point
                // to the corresponding copied strings. Performing the assignment inside the
                // lambda body ensures that the lambda will take a reference to the shared_ptr
                // that holds the data.  This will guarantee that the data is still valid when
                // CreateInputLayout is called.
                for (uint32 i = 0; i < layoutDescNumElements; i++)
                {
                    layoutDescCopy->at(i).SemanticName = layoutDescSemanticNamesCopy->at(i).c_str();
                }
            }

            CreateInputLayout(
                bytecode->Data,
                bytecode->Length,
                layoutDesc == nullptr ? nullptr : layoutDescCopy->data(),
                layoutDescNumElements,
                layout);   
        }
    });
}

```

In diesem Code erstellen Sie den Vertex-Shader per Aufruf von [**ID3D11Device::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524), nachdem Sie die Bytedaten für die CSO-Datei des Vertex-Shaders eingelesen haben. Danach erstellen Sie das Eingabelayout für den Shader in derselben Lambda-Funktion.

Für andere Arten von Shadern, z. B. Geometry- und Hull-Shader, kann ebenfalls eine spezielle Konfiguration erforderlich sein. Den vollständigen Code für verschiedene Methoden zum Laden von Shadern finden Sie unter [Vollständiger Code für BasicLoader](complete-code-for-basicloader.md) und [Beispiel für das Laden der Direct3D-Ressource]( http://go.microsoft.com/fwlink/p/?LinkID=265132).

## Hinweise

Sie sollten jetzt mit den Methoden zum asynchronen Laden häufig verwendeter Ressourcen und Objekte für Spiele, wie Gittern, Texturen und kompilierten Shadern, vertraut sein und diese Methoden erstellen und ändern können.

## Verwandte Themen

* [Beispiel für das Laden von Direct3D-Ressourcen]( http://go.microsoft.com/fwlink/p/?LinkID=265132)
* [Vollständiger Code für „BasicLoader“](complete-code-for-basicloader.md)
* [Vollständiger Code für BasicReaderWriter](complete-code-for-basicreaderwriter.md)
* [Vollständiger Code für DDSTextureLoader](complete-code-for-ddstextureloader.md)

 

 






<!--HONumber=Mar16_HO1-->


