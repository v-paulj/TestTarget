---
author: mtoepke
title: Zuordnung von DirectX 9-Funktionen zu DirectX 11-APIs
description: Erfahren Sie, wie die Features Ihres Direct3D 9-Spiels zu Direct3D 11 und zur Universellen Windows-Plattform (UWP) zugeordnet werden.
ms.assetid: 3aa8a114-4e47-ae0a-9447-88ba324377b8
---

# Zuordnung von DirectX 9-Funktionen zu DirectX 11-APIs


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

**Zusammenfassung**

-   [Planen der DirectX-Portierung](plan-your-directx-port.md)
-   [Wichtige Änderungen beim Wechsel von Direct3D 9 zu Direct3D 11](understand-direct3d-11-1-concepts.md)
-   Featurezuordnung


Erfahren Sie, wie die Features Ihres Direct3D 9-Spiels zu Direct3D 11 und zur Universellen Windows-Plattform (UWP) zugeordnet werden.

## Zuordnen von Direct3D 9-Features zu DirectX 11-APIs


[Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466) ist nach wie vor die Grundlage von DirectX-Grafiken, die API wurde seit DirectX 9 jedoch geändert:

-   Zum Einrichten von Grafikadaptern wird die Microsoft DirectX Graphics Infrastructure (DXGI) verwendet. Verwenden Sie [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534) zum Auswählen von Pufferformaten, Erstellen von Swapchains, Darstellen von Frames und Erstellen freigegebener Ressourcen. Siehe [Übersicht über DXGI](https://msdn.microsoft.com/library/windows/desktop/bb205075).
-   Ein Direct3D-Gerätekontext wird zum Festlegen des Pipelinestatus und Generieren von Renderbefehlen verwendet. In den meisten unserer Beispiele wird ein unmittelbarer Kontext verwendet, um direkt auf dem Gerät zu rendern. Direct3D 11 unterstützt auch das Multithread-Rendering, wobei dann verzögerte Kontexte verwendet werden. Siehe [Einführung in ein Gerät in Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476880).
-   Einige Funktionen sind veraltet und nicht mehr verfügbar. Zu beachten ist insbesondere, dass es die Pipeline mit fester Funktion nicht mehr gibt. Siehe [Veraltete Funktionen](https://msdn.microsoft.com/library/windows/desktop/cc308047).

Eine vollständige Liste der Direct3D 11-Funktionen finden Sie unter [Features von Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476342) und [Features von Direct3D 11.1](https://msdn.microsoft.com/library/windows/desktop/hh404562).

## Umstellung von Direct2D 9 auf Direct2D 11


[Direct2D (Windows)](https://msdn.microsoft.com/library/windows/desktop/dd370990) ist weiterhin ein wichtiger Bestandteil von DirectX-Grafiken und Windows. Sie können mit Direct2D weiterhin 2D-Spiele und Direct3D-basierte Überlagerungen (HUDs) zeichnen.

Direct2D baut auf Direct3D auf. 2D-Spiele können mit beiden APIs implementiert werden. Ein mit Direct3D implementiertes 2D-Spiel kann z. B. die orthografische Projektion verwenden, Z-Werte zum Steuern der Zeichnungsreihenfolge von Grundtypen festlegen und mit Pixelshadern Spezialeffekte hinzufügen.

Da Direct2D auf Direct3D basiert, verwendet es ebenfalls DXGI und Gerätekontexte. Siehe [Übersicht über die Direct2D-API](https://msdn.microsoft.com/library/windows/desktop/dd317121).

Die [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038)-API bietet Unterstützung für formatierten Text mit Direct2D. Siehe [Einführung in DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd371554).

## Ersetzen veralteter Hilfsbibliotheken


D3DX und DXUT sind veraltet und können nicht in UWP-Spielen verwendet werden. Von diesen Hilfsbibliotheken wurden Ressourcen für Aufgaben wie das Laden von Texturen und Gittern bereitgestellt.

-   In der exemplarischen Vorgehensweise [Einfache Portierung von Direct3D 9 zu UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md) wird gezeigt, wie ein Fenster eingerichtet, Direct3D initialisiert und einfaches 3D-Rendering durchgeführt wird.
-   In der exemplarischen Vorgehensweise [Erstellen eines einfachen UWP-Spiels mit DirectX](tutorial--create-your-first-metro-style-directx-game.md) werden allgemeine Aufgaben der Spielprogrammierung erläutert, u. a. Grafiken, Laden von Dateien, UI, Steuerelemente und Sound.
-   Das Communityprojekt [DirectX-Toolkit](http://go.microsoft.com/fwlink/p/?LinkID=248929) bietet Hilfsklassen zur Verwendung mit Direct3D 11 und UWP-Apps.

## Umstellung von Shaderprogrammen von FX auf HLSL


Die D3DX-Hilfsprogrammbibliothek (D3DX 9, D3DX 10 und D3DX 11), einschließlich Effects, ist veraltet und für UWP nicht mehr verfügbar. Alle DirectX-Spiele für UWP steuern die Grafikpipeline mit [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561) ohne Effects.

Visual Studio verwendet FXC weiterhin im Hintergrund zum Kompilieren von Shaderobjekten. UWP-Spielshader werden vorab kompiliert. Der Bytecode wird zur Laufzeit geladen. Anschließend wird jede Shaderressource während des entsprechenden Renderingdurchgangs an die Grafikpipeline gebunden. Shader sollten in eigene separate HLSL-Dateien verschoben werden, und Renderingtechniken sollten im C++-Code implementiert werden.

Einen kurzen Überblick über das Laden von Shaderressourcen finden Sie unter [Einfache Portierung von Direct3D 9 zu UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

In Direct3D 11 wurde Shadermodell 5 eingeführt, das die Direct3D-Funktionsebene 11\_0 (oder höher) erfordert. Siehe [Funktionen von HLSL-Shadermodell 5 für Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff471419).

## Ersetzen von XNAMath und D3DXMath


Code mit XNAMath (oder D3DXMath) sollte zu [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833) migriert werden. DirectXMath enthält Typen, die zwischen x86, x64 und ARM portierbar sind. Siehe [Codemigration von der XNAMath-Bibliothek](https://msdn.microsoft.com/library/windows/desktop/ee418730).

Beachten Sie, dass sich Float-Typen von DirectXMath zur Verwendung mit Shadern eignen. Mit [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608) und [**XMFLOAT4X4**](https://msdn.microsoft.com/library/windows/desktop/ee419621) können z. B. auf einfache Weise Daten für Konstantenpuffer ausgerichtet werden.

## Ersetzen von DirectSound durch XAudio2 (und Hintergrundaudio)


DirectSound wird für UWP nicht unterstützt:

-   Verwenden Sie [XAudio2](https://msdn.microsoft.com/library/windows/desktop/hh405049), um Ihrem Spiel Soundeffekte hinzuzufügen.

##  Ersetzen von DirectInput durch XInput und UWP-APIs


DirectInput wird für UWP nicht unterstützt:

-   Verwenden Sie [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Eingabeereignisrückrufe für Maus-, Tastatur- und Toucheingabe.
-   Verwenden Sie [XInput](https://msdn.microsoft.com/library/windows/desktop/ee417001) 1.4 zur Unterstützung von Gamecontrollern (und Gamecontroller-Headsets). Wenn Sie eine freigegebene Codebasis für Desktop und UWP verwenden, finden Sie unter [XInput-Versionen](https://msdn.microsoft.com/library/windows/desktop/hh405051) Informationen zur Abwärtskompatibilität.
-   Registrieren Sie [**EdgeGesture**](https://msdn.microsoft.com/library/windows/apps/hh701600)-Ereignisse, wenn die App-Leiste in Ihrem Spiel verwendet werden muss.

## Verwenden von Microsoft Media Foundation anstelle von DirectShow


DirectShow ist nicht mehr in der DirectX-API (oder der Windows-API) enthalten. [Microsoft Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) stellt Videoinhalte mithilfe von freigegebenen Oberflächen für Direct3D bereit. Siehe [Direct3D 11-Video-APIs](https://msdn.microsoft.com/library/windows/desktop/hh447677).

## Ersetzen von DirectPlay durch Netzwerkcode


Microsoft DirectPlay ist veraltet und nicht mehr verfügbar. Falls Ihr Spiel Netzwerkdienste verwendet, müssen Sie Netzwerkcode bereitstellen, der den UWP-Anforderungen entspricht. Verwenden Sie die folgenden APIs:

-   [Win32 und COM für Windows Store-Apps (Netzwerk) (Windows)](https://msdn.microsoft.com/library/windows/apps/br205759)
-   [**Windows.Networking-Namespace (Windows)**](https://msdn.microsoft.com/library/windows/apps/br207124)
-   [**Windows.Networking.Sockets-Namespace (Windows)**](https://msdn.microsoft.com/library/windows/apps/br226960)
-   [**Windows.Networking.Connectivity-Namespace (Windows)**](https://msdn.microsoft.com/library/windows/apps/br207308)
-   [**Windows.ApplicationModel.Background-Namespace (Windows)**](https://msdn.microsoft.com/library/windows/apps/br224847)

In den folgenden Artikeln finden Sie Informationen zum Hinzufügen von Netzwerkfunktionen und Deklarieren der Netzwerkunterstützung im Paketmanifest Ihrer App.

-   [Verbinden mit Sockets (Windows Store-Apps mit C#/VB/C++ und XAML) (Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh452976)
-   [Verbinden mit WebSockets (Windows Store-Apps mit C#/VB/C++ und XAML) (Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh994396)
-   [Herstellen einer Verbindung mit Webdiensten (Windows Store-Apps mit C#/VB/C++ und XAML) (Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504)
-   [Grundlagen zum Netzwerk](https://msdn.microsoft.com/library/windows/apps/mt280233)

Beachten Sie, dass alle UWP-Apps (einschließlich Spiele) bestimmte Hintergrundaufgaben verwenden, um die Verbindung aufrechtzuerhalten, während die App angehalten ist. Wenn Ihr Spiel den Verbindungsstatus aufrechterhalten muss, während es angehalten ist, gehen Sie wie unter [Grundlagen zum Netzwerk](https://msdn.microsoft.com/library/windows/apps/mt280233) beschrieben vor.

## Funktionszuordnung


Ziehen Sie beim Konvertieren von Code von Direct3D 9 in Direct3D 11 die folgende Tabelle zurate. Diese Informationen können Ihnen auch die Unterscheidung zwischen Gerät und Gerätekontext erleichtern.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Direct3D9</th>
<th align="left">Direct3D 11-Entsprechung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[<strong>IDirect3DDevice9</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174336)</p></td>
<td align="left"><p>[<strong>ID3D11Device2</strong>] (https://msdn.microsoft.com/library/windows/desktop/dn280493)</p>
<p>[<strong>ID3D11DeviceContext2</strong>] (https://msdn.microsoft.com/library/windows/desktop/dn280498)</p>
<p>Die Grafikpipelinestufen werden unter [Grafikpipeline](https://msdn.microsoft.com/library/windows/desktop/ff476882) erläutert.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3D9</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174300)</p></td>
<td align="left"><p>[<strong>IDXGIFactory2</strong>] (https://msdn.microsoft.com/library/windows/desktop/hh404556)</p>
<p>[<strong>IDXGIAdapter2</strong>] (https://msdn.microsoft.com/library/windows/desktop/hh404537)</p>
<p>[<strong>IDXGIDevice3</strong>] (https://msdn.microsoft.com/library/windows/desktop/dn280345)</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>IDirect3DDevice9::Present</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174423)</p></td>
<td align="left"><p>[<strong>IDXGISwapChain1::Present1</strong>] (https://msdn.microsoft.com/library/windows/desktop/hh446797)</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3DDevice9::TestCooperativeLevel</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174472)</p></td>
<td align="left"><p>Rufen Sie [<strong>IDXGISwapChain1::Present1</strong>] (https://msdn.microsoft.com/library/windows/desktop/hh446797) mit gesetztem Flag DXGI_PRESENT_TEST auf.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>IDirect3DBaseTexture9</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174322)</p>
<p>[<strong>IDirect3DTexture9</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb205909)</p>
<p>[<strong>IDirect3DCubeTexture9</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174329)</p>
<p>[<strong>IDirect3DVolumeTexture9</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb205941)</p>
<p>[<strong>IDirect3DIndexBuffer9</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb205865)</p>
<p>[<strong>IDirect3DVertexBuffer9</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb205915)</p></td>
<td align="left"><p>[<strong>ID3D11Buffer</strong>] (https://msdn.microsoft.com/library/windows/desktop/ff476351)</p>
<p>[<strong>ID3D11Texture1D</strong>] (https://msdn.microsoft.com/library/windows/desktop/ff476633)</p>
<p>[<strong>ID3D11Texture2D</strong>] (https://msdn.microsoft.com/library/windows/desktop/ff476635)</p>
<p>[<strong>ID3D11Texture3D</strong>] (https://msdn.microsoft.com/library/windows/desktop/ff476637)</p>
<p>[<strong>ID3D11ShaderResourceView</strong>] (https://msdn.microsoft.com/library/windows/desktop/ff476628)</p>
<p>[<strong>ID3D11RenderTargetView</strong>] (https://msdn.microsoft.com/library/windows/desktop/ff476582)</p>
<p>[<strong>ID3D11DepthStencilView</strong>] (https://msdn.microsoft.com/library/windows/desktop/ff476377)</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3DVertexShader9</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb205922)</p>
<p>[<strong>IDirect3DPixelShader9</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb205869)</p></td>
<td align="left"><p>[<strong>ID3D11VertexShader</strong>] (https://msdn.microsoft.com/library/windows/desktop/ff476641)</p>
<p>[<strong>ID3D11PixelShader</strong>] (https://msdn.microsoft.com/library/windows/desktop/ff476576)</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>IDirect3DVertexDeclaration9</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb205919)</p></td>
<td align="left"><p>[<strong>ID3D11InputLayout</strong>] (https://msdn.microsoft.com/library/windows/desktop/ff476575)</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3DDevice9::SetRenderState</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb205805)</p>
<p>[<strong>IDirect3DDevice9::SetSamplerState</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb205806)</p></td>
<td align="left"><p>[<strong>ID3D11BlendState1</strong>] (https://msdn.microsoft.com/library/windows/desktop/hh404571)</p>
<p>[<strong>ID3D11DepthStencilState</strong>] (https://msdn.microsoft.com/library/windows/desktop/ff476375)</p>
<p>[<strong>ID3D11RasterizerState1</strong>] (https://msdn.microsoft.com/library/windows/desktop/hh446828)</p>
<p>[<strong>ID3D11SamplerState</strong>] (https://msdn.microsoft.com/library/windows/desktop/ff476588)</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>IDirect3DDevice9::DrawIndexedPrimitive</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174369)</p>
<p>[<strong>IDirect3DDevice9::DrawPrimitive</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174371)</p></td>
<td align="left"><p>[<strong>ID3D11DeviceContext::Draw</strong>] (https://msdn.microsoft.com/library/windows/desktop/ff476407)</p>
<p>[<strong>ID3D11DeviceContext::DrawIndexed</strong>] (https://msdn.microsoft.com/library/windows/desktop/ff476409)</p>
<p>[<strong>ID3D11DeviceContext::DrawIndexedInstanced</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb173566)</p>
<p>[<strong>ID3D11DeviceContext::DrawInstanced</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb173567)</p>
<p>[<strong>ID3D11DeviceContext::IASetPrimitiveTopology</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb173590)</p>
<p>[<strong>ID3D11DeviceContext::DrawAuto</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb173564)</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3DDevice9::BeginScene</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174350)</p>
<p>[<strong>IDirect3DDevice9::EndScene</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174375)</p>
<p>[<strong>IDirect3DDevice9::DrawPrimitiveUP</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174372)</p>
<p>[<strong>IDirect3DDevice9::DrawIndexedPrimitiveUP</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174370)</p></td>
<td align="left"><p>Keine direkte Entsprechung</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>IDirect3DDevice9::ShowCursor</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174470)</p>
<p>[<strong>IDirect3DDevice9::SetCursorPosition</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174429)</p>
<p>[<strong>IDirect3DDevice9::SetCursorProperties</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174430)</p></td>
<td align="left"><p>Verwenden Sie standardmäßige Cursor-APIs.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3DDevice9::Reset</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174425)</p></td>
<td align="left"><p>"LOST device" und POOL_MANAGED sind nicht mehr verfügbar. [<strong>IDXGISwapChain1::Present1</strong>] (https://msdn.microsoft.com/library/windows/desktop/hh446797) kann mit dem Rückgabewert [<strong>DXGI_ERROR_DEVICE_REMOVED</strong>](https://msdn.microsoft.com/library/windows/desktop/bb509553) fehlschlagen.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>IDirect3DDevice9:DrawRectPatch</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174373)</p>
<p>[<strong>IDirect3DDevice9:DrawTriPatch</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174374)</p>
<p>[<strong>IDirect3DDevice9:LightEnable</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174421)</p>
<p>[<strong>IDirect3DDevice9:MultiplyTransform</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174422)</p>
<p>[<strong>IDirect3DDevice9:SetLight</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb205798)</p>
<p>[<strong>IDirect3DDevice9:SetMaterial</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174437)</p>
<p>[<strong>IDirect3DDevice9:SetNPatchMode</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174438)</p>
<p>[<strong>IDirect3DDevice9:SetTransform</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174463)</p>
<p>[<strong>IDirect3DDevice9:SetFVF</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174433)</p>
<p>[<strong>IDirect3DDevice9:SetTextureStageState</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174462)</p></td>
<td align="left"><p>Die Pipeline mit fester Funktion ist veraltet und nicht mehr verfügbar.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>IDirect3DDevice9:CheckDepthStencilMatch</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174308)</p>
<p>[<strong>IDirect3DDevice9:CheckDeviceFormat</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174309)</p>
<p>[<strong>IDirect3DDevice9:GetDeviceCaps</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb174320)</p>
<p>[<strong>IDirect3DDevice9:ValidateDevice</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb205859)</p></td>
<td align="left"><p>Funktionsbits werden durch Funktionsebenen ersetzt. Nur einige wenige Format- und Funktionsverwendungsfälle sind für alle Funktionsebenen optional. Diese können mit [<strong>ID3D11Device::CheckFeatureSupport</strong>] (https://msdn.microsoft.com/library/windows/desktop/ff476497) und [<strong>ID3D11Device::CheckFormatSupport</strong>] (https://msdn.microsoft.com/library/windows/desktop/bb173536) überprüft werden.</p></td>
</tr>
</tbody>
</table>

 

## Oberflächenformatzuordnung


Ziehen Sie beim Konvertieren von Direct3D 9-Formaten in DXGI-Formate die folgende Tabelle heran.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Direct3D 9-Format</th>
<th align="left">Direct3D 11-Format</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>D3DFMT_UNKNOWN</p></td>
<td align="left"><p>DXGI_FORMAT_UNKNOWN</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R8G8B8</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8R8G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_B8G8R8A8_UNORM</p>
<p>DXGI_FORMAT_B8G8R8A8_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X8R8G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_B8G8R8X8_UNORM</p>
<p>DXGI_FORMAT_B8G8R8X8_UNORM_SRGB</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R5G6B5</p></td>
<td align="left"><p>DXGI_FORMAT_B5G6R5_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X1R5G5B5</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A1R5G5B5</p></td>
<td align="left"><p>DXGI_FORMAT_B5G5R5A1_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A4R4G4B4</p></td>
<td align="left"><p>DXGI_FORMAT_B4G4R4A4_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R3G3B2</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8</p></td>
<td align="left"><p>DXGI_FORMAT_A8_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8R3G3B2</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X4R4G4B4</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A2B10G10R10</p></td>
<td align="left"><p>DXGI_FORMAT_R10G10B10A2</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8B8G8R8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UNORM</p>
<p>DXGI_FORMAT_R8G8B8A8_UNORM_SRGB</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_X8B8G8R8</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G16R16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A2R10G10B10</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A16B16G16R16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8P8</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_P8</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8_UNORM</p>
<div class="alert">
<strong>Hinweis</strong>   Verwenden Sie zum Herstellen des Direct3D 9-Verhaltens den Swizzle-Operator ".r" im Shader, um Rot für andere Komponenten zu duplizieren.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_UNORM</p>
<div class="alert">
<strong>Hinweis</strong>   Verwenden Sie zum Herstellen des Direct3D 9-Verhaltens den Swizzle-Operator ".rrrg" im Shader, um Rot zu duplizieren und Grün in die Alpha-Komponenten zu verschieben.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A4L4</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_V8U8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L6V5U5</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X8L8V8U8</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_Q8W8V8U8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_SNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_V16U16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_W11V11U10</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A2W10V10U10</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_UYVY</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R8G8_B8G8</p></td>
<td align="left"><p>DXGI_FORMAT_G8R8_G8B8_UNORM</p>
<div class="alert">
<strong>Hinweis</strong>   In Direct3D 9 wurden die Daten um 255,0f skaliert. Dies kann jedoch im Shader behandelt werden.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_YUY2</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G8R8_G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_B8G8_UNORM</p>
<div class="alert">
<strong>Hinweis</strong>   In Direct3D 9 wurden die Daten um 255,0f skaliert. Dies kann jedoch im Shader behandelt werden.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT1</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM & DXGI_FORMAT_BC1_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_DXT2</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM & DXGI_FORMAT_BC1_UNORM_SRGB</p>
<div class="alert">
<strong>Hinweis</strong>   DXT1 und DXT2 sind hinsichtlich API und Hardware identisch. Der einzige Unterschied besteht darin, ob prämultipliziertes Alpha verwendet wird, was von einer App nachverfolgt werden kann und kein separates Format erfordert.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT3</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM & DXGI_FORMAT_BC2_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_DXT4</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM & DXGI_FORMAT_BC2_UNORM_SRGB</p>
<div class="alert">
<strong>Hinweis</strong>   DXT3 und DXT4 sind hinsichtlich API und Hardware identisch. Der einzige Unterschied besteht darin, ob prämultipliziertes Alpha verwendet wird, was von einer App nachverfolgt werden kann und kein separates Format erfordert.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT5</p></td>
<td align="left"><p>DXGI_FORMAT_BC3_UNORM & DXGI_FORMAT_BC3_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D16 & D3DFMT_D16_LOCKABLE</p></td>
<td align="left"><p>DXGI_FORMAT_D16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D32</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D15S1</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D24S8</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D24X8</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D24X4S4</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D16</p></td>
<td align="left"><p>DXGI_FORMAT_D16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D32F_LOCKABLE</p></td>
<td align="left"><p>DXGI_FORMAT_D32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D24FS8</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_S1D15</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_S8D24</p></td>
<td align="left"><p>DXGI_FORMAT_D24_UNORM_S8_UINT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_X8D24</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X4S4D24</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L16</p></td>
<td align="left"><p>DXGI_FORMAT_R16_UNORM</p>
<div class="alert">
<strong>Hinweis</strong>   Verwenden Sie zum Herstellen des D3D9-Verhaltens den Swizzle-Operator ".r" im Shader, um Rot für andere Komponenten zu duplizieren.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_INDEX16</p></td>
<td align="left"><p>DXGI_FORMAT_R16_UINT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_INDEX32</p></td>
<td align="left"><p>DXGI_FORMAT_R32_UINT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_Q16W16V16U16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_MULTI2_ARGB8</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_G16R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A16B16G16R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G32R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A32B32G32R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_CxV8U8</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT1</p></td>
<td align="left"><p>DXGI_FORMAT_R32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT2</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT3</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT4</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPED3DCOLOR</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_UBYTE4</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UINT</p>
<div class="alert">
<strong>Hinweis</strong>   Der Shader ruft UINT-Werte ab. Wenn ganzzahlige Direct3D 9-Gleitkommawerte benötigt werden (0.0f, 1,0f ... 255,0f), kann UINT im Shader jedoch einfach in float32 konvertiert werden.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SINT</p>
<div class="alert">
<strong>Hinweis</strong>   Der Shader ruft SINT-Werte ab. Wenn ganzzahlige Direct3D 9-Gleitkommawerte benötigt werden, kann SINT im Shader jedoch einfach in float32 konvertiert werden.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<div class="alert">
<strong>Hinweis</strong>   Der Shader ruft SINT-Werte ab. Wenn ganzzahlige Direct3D 9-Gleitkommawerte benötigt werden, kann SINT im Shader jedoch einfach in float32 konvertiert werden.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_UBYTE4N</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT2N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT4N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_USHORT2N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_USHORT4N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_UDEC3</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_DEC3N</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT16_2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT16_4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>FourCC 'ATI1'</p></td>
<td align="left"><p>DXGI_FORMAT_BC4_UNORM</p>
<div class="alert">
<strong>Hinweis</strong>   Erfordert Funktionsebene 10.0 oder höher:
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>FourCC 'ATI2'</p></td>
<td align="left"><p>DXGI_FORMAT_BC5_UNORM</p>
<div class="alert">
<strong>Hinweis</strong>   Erfordert Funktionsebene 10.0 oder höher:
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

 

 






<!--HONumber=May16_HO2-->


