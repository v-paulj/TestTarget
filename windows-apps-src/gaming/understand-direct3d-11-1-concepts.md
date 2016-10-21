---
author: mtoepke
title: "Wichtige Änderungen beim Wechsel von Direct3D9 zu Direct3D11"
description: "In diesem Thema werden allgemeine Unterschiede zwischen DirectX9 und DirectX11 erläutert."
ms.assetid: 35a9e388-b25e-2aac-0534-577b15dae364
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 9812e3a4528b0ce8abd76b1bfcfb93b1268f362c

---

# Wichtige Änderungen beim Wechsel von Direct3D 9 zu Direct3D 11


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

**Zusammenfassung**

-   [Planen der DirectX-Portierung](plan-your-directx-port.md)
-   Wichtige Änderungen beim Wechsel von Direct3D9 zu Direct3D11
-   [Featurezuordnung](feature-mapping.md)


In diesem Thema werden allgemeine Unterschiede zwischen DirectX9 und DirectX11 erläutert.

Bei Direct3D11 handelt es sich im Wesentlichen um die gleiche Art von API wie bei Direct3D9: eine virtualisierte Low-Level-Schnittstelle für Grafikhardware. Sie können damit für viele unterschiedliche Hardwareimplementierungen weiterhin Vorgänge zum Zeichnen von Grafiken durchführen. Das Layout der Grafik-API hat sich seit Direct3D9 geändert. Das Konzept des Gerätekontexts wurde erweitert, und es wurde eine API speziell für die Grafikinfrastruktur hinzugefügt. Für auf dem Direct3D-Gerät gespeicherte Ressourcen ist ein neuer Mechanismus für Datenpolymorphie (die so genannte Ressourcenansicht) verfügbar.

## Wichtige API-Funktionen


In Direct3D9 mussten Sie eine Schnittstelle für die Direct3D-API erstellen, um sie verwenden zu können. In Direct3D11-Spielen für die universelle Windows-Plattform (UWP) rufen Sie die statische Funktion [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) auf, um das Gerät und den Gerätekontext zu erstellen.

## Geräte und Gerätekontext


Ein Direct3D11-Gerät stellt einen virtualisierten Grafikadapter dar. Dieser wird zum Erstellen von Ressourcen im Videospeicher verwendet, z.B. zum Hochladen von Texturen in die GPU, Erstellen von Ansichten für Texturressourcen und Swapchains und Erstellen von Textursamplern. Eine vollständige Liste der Verwendungsmöglichkeiten für eine Direct3D11-Geräteschnittstelle finden Sie unter [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) und [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575).

Ein Direct3D11- Gerätekontext wird verwendet, um den Pipelinestatus festzulegen und Renderbefehle zu erzeugen. Von einer Direct3D11-Renderkette wird beispielsweise ein Gerätekontext verwendet, um die Renderkette einzurichten und die Szene zu zeichnen (siehe unten). Über den Gerätekontext wird auf den Videospeicher zugegriffen (Map), der von den Ressourcen des Direct3D-Geräts verwendet wird. Außerdem werden damit Unterressourcendaten aktualisiert, z.B. Konstantenpufferdaten. Eine vollständige Liste der Verwendungsmöglichkeiten für einen Direct3D11-Gerätekontext finden Sie unter [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) und [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598). Beachten Sie, dass für die meisten Beispiele ein unmittelbarer Kontext für das direkte Rendern auf dem Gerät genutzt wird. Von Direct3D11 werden jedoch auch zurückgestellte Gerätekontexte unterstützt, die hauptsächlich für das Multithreading eingesetzt werden.

In Direct3D11 werden das Handle für das Gerät und das Handle für den Gerätekontext jeweils per Aufruf von [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) abgerufen. Mit dieser Methode fordern Sie auch einen bestimmten Satz von Hardwarefeatures an und rufen Informationen zu Direct3D-Featureebenen ab, die vom Grafikadapter unterstützt werden. Weitere Informationen zu Geräten, Gerätekontexten und Threadaspekten finden Sie unter [Einführung in ein Gerät in Direct3D11](https://msdn.microsoft.com/library/windows/desktop/ff476880).

## Geräteinfrastruktur, Framepuffer und Renderzielansichten


In Direct3D11 werden der Geräteadapter und die Hardwarekonfiguration mithilfe der DirectXGraphicsInfrastructure-API (DXGI) unter Verwendung der COM-Schnittstellen [**IDXGIAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174523) und [**IDXGIDevice1**](https://msdn.microsoft.com/library/windows/desktop/hh404543) festgelegt. Puffer und andere Fensterressourcen (sichtbar oder außerhalb des Bildschirms) werden unter Verwendung spezieller DXGI-Schnittstellen erstellt und konfiguriert. Von der Implementierung des [**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556)-Factorymusters werden DXGI-Ressourcen (etwa der Framepuffer) abgerufen. Da DXGI für die Swapchain zuständig ist, wird eine DXGI-Schnittstelle zum Darstellen von Frames auf dem Bildschirm verwendet. Weitere Informationen finden Sie unter [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631).

Verwenden Sie [**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556) zum Erstellen einer mit Ihrem Spiel kompatiblen Swapchain. Sie müssen eine Swapchain für ein CoreWindow-Objekt oder für die Komposition (XAML-Interoperabilität) erstellen, anstatt eine Swapchain für ein HWND-Element.

## Geräteressourcen und Ressourcenansichten


Von Direct3D11 wird eine zusätzliche Ebene der Polymorphie auf Videospeicherressourcen unterstützt, die als Ansichten bezeichnet werden. Wo vorher ein einzelnes Direct3D9-Objekt für eine Textur vorhanden war, werden nun zwei Objekte genutzt: die Texturressource, in der die Daten enthalten sind, und die Ressourcenansicht, mit der angegeben wird, wie die Ansicht zum Rendern verwendet wird. Mithilfe einer auf einer Ressource basierenden Ansicht kann diese Ressource für einen bestimmten Zweck verwendet werden. Eine 2D-Texturressource wird als [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)-Element erstellt. Anschließend wird dafür eine Shaderressourcenansicht ([**ID3D11ShaderResourceView**](https://msdn.microsoft.com/library/windows/desktop/ff476628)) erstellt, sodass die Verwendung als Textur in einem Shader möglich ist. Außerdem kann eine Renderzielansicht ([**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)) für die gleiche 2D-Texturressource erstellt werden, sodass diese als Zeichenoberfläche verwendet werden kann. In einem anderen Beispiel werden dieselben Pixeldaten in zwei unterschiedlichen Pixelformaten dargestellt, indem zwei separate Ansichten für eine Texturressource genutzt werden.

Die zugrunde liegende Ressource muss mit Eigenschaften erstellt werden, die mit dem Typ der Ansichten kompatibel ist, die auf deren Grundlage erstellt werden. Wenn beispielsweise ein [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)-Element auf eine Oberfläche angewendet wird, wird diese Oberfläche mit dem Kennzeichen [**D3D11\_BIND\_RENDER\_TARGET**](https://msdn.microsoft.com/library/windows/desktop/ff476085) erstellt. Die Oberfläche muss auch über ein DXGI-Oberflächenformat verfügen, das mit dem Rendering kompatibel ist (siehe [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059)).

Die meisten Ressourcen, die Sie zum Rendern verwenden, erben von der [**ID3D11Resource**](https://msdn.microsoft.com/library/windows/desktop/ff476584)-Schnittstelle, die wiederum von [**ID3D11DeviceChild**](https://msdn.microsoft.com/library/windows/desktop/ff476380) erbt. Vertexpuffer, Indexpuffer, Konstantenpuffer und Shader sind Direct3D11-Ressourcen. Eingabelayouts und Samplerzustände erben direkt von [**ID3D11DeviceChild**](https://msdn.microsoft.com/library/windows/desktop/ff476380).

Von Ressourcenansichten wird ein DXGI\_FORMAT-Enumerationswert zum Angeben des Pixelformats verwendet. Nicht jedes D3DFMT-Element wird als DXGI\_FORMAT unterstützt. In DXGI ist beispielsweise kein 24bpp-RGB-Format vorhanden, das zu D3DFMT\_R8G8B8 äquivalent ist. Außerdem sind keine BGR-Äquivalente zu allen RGB-Formaten vorhanden (DXGI\_FORMAT\_R10G10B10A2\_UNORM ist zwar äquivalent zu D3DFMT\_A2B10G10R10, aber es ist kein direktes Äquivalent zu D3DFMT\_A2R10G10B10 vorhanden). Planen Sie ein, alle Inhalte in diesen Legacyformaten zur Buildzeit in unterstützte Formate zu konvertieren. Eine vollständige Liste mit den DXGI-Formaten finden Sie in der [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059)-Enumeration.

Direct3D-Geräteressourcen (und Ressourcenansichten) werden erstellt, bevor die Szene gerendert wird. Gerätekontexte werden, wie unten beschrieben, zum Einrichten der Renderkette verwendet.

## Gerätekontext und die Renderkette


In Direct3D9 und Direct3D10.x war ein einzelnes Direct3D-Geräteobjekt vorhanden, mit dem die Erstellung, der Status und das Zeichnen verwaltet wurde. In Direct3D11 wird die Direct3D-Geräteschnittstelle weiterhin zum Verwalten der Ressourcenerstellung verwendet, aber alle Status- und Zeichenvorgänge werden über einen Direct3D-Gerätekontext abgewickelt. Im Anschluss finden Sie ein Beispiel für die Verwendung des Gerätekontexts ([**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)-Schnittstelle) zum Einrichten der Renderkette:

-   Festlegen und Löschen von Renderzielansichten (und der Tiefenschablonen-Ansicht)
-   Festlegen des Vertexpuffers, Indexpuffers und Eingabelayouts für die Eingabeassemblerphase (IA-Phase)
-   Binden von Vertex- und Pixelshadern an die Pipeline
-   Binden von Konstantenpuffern an Shader
-   Binden von Texturansichten und Samplern an den Pixelshader
-   Zeichnen der Szene

Wenn eine der [**ID3D11DeviceContext::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407)-Methoden aufgerufen wird, wird die Szene in der Renderzielansicht gezeichnet. Nach Abschluss des Zeichenvorgangs wird der DXGI-Adapter zum Darstellen des abgeschlossenen Frames verwendet, indem [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797) aufgerufen wird.

## Zustandsverwaltung


In Direct3D9 wurden Zustandseinstellungen mithilfe einer großen Gruppe individueller Umschaltfunktionen verwaltet, die über die Methoden „SetRenderState“, „SetSamplerState“ und „SetTextureStageState“ festgelegt wurden. Da Direct3D11 nicht über Unterstützung für die ältere Pipeline mit festen Funktionen verfügt, wird SetTextureStageState durch das Schreiben von Pixelshadern (PS) ersetzt. Es ist kein Äquivalent zu einem Direct3D9-Statusblock vorhanden. Bei Direct3D11 wird der Zustand stattdessen mit vier Arten von Zustandsobjekten verwaltet, die ein optimiertes Verfahren zum Gruppieren des Renderzustands ermöglichen.

Anstatt beispielsweise „SetRenderState“ mit D3DRS\_ZENABLE zu verwenden, erstellen Sie ein DepthStencilState-Objekt mit diesen und anderen dazugehörigen Zustandseinstellungen und verwenden es, um den Zustand beim Rendern zu ändern.

Beachten Sie beim Portieren von Direct3D9-Anwendungen zu Statusobjekten, dass die verschiedenen Statuskombinationen als Objekte mit unveränderlichem Status dargestellt werden. Sie sollten einmal erstellt und dann so lange wiederverwendet werden, wie sie gültig sind.

## Direct3D-Featureebenen


Direct3D verfügt über einen neuen Mechanismus zum Bestimmen der Hardwareunterstützung, der die Bezeichnung "Featureebenen" trägt. Featureebenen erleichtern die Ermittlung der Funktionen des Grafikadapters, da Sie einen sorgfältig definierten Satz von GPU-Funktionen anfordern können. Von der Featureebene9\_1 werden beispielsweise die von den Direct3D9-Grafikadaptern bereitgestellten Funktionen (einschließlich Shadermodell2.x) implementiert. Da9\_1 die niedrigste Featureebene ist, können Sie davon ausgehen, dass alle Geräte einen Vertex-Shader und einen Pixelshader unterstützen. Dies sind die gleichen Phasen, die vom programmierbaren Shadermodell unter Direct3D9 unterstützt wurden.

Im Spiel wird [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) zum Erstellen des Direct3D-Geräts und -Gerätekontexts verwendet. Wenn Sie diese Funktion aufrufen, geben Sie eine Liste mit Featureebenen an, die vom Spiel unterstützt werden können. Die höchste unterstützte Featureebene der Liste wird zurückgegeben. Falls in Ihrem Spiel beispielsweise BC4/BC5-Texturen genutzt werden können (Feature von DirectX10-Hardware), müssten Sie mindestens9\_1 und10\_0 in die Liste der unterstützten Featureebenen aufnehmen. Wenn das Spiel auf DirectX9-Hardware ausgeführt wird und keine BC4/BC5-Texturen verwendet werden können, wird vom [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082)-Element9\_1 zurückgegeben. Im Spiel kann dann auf ein anderes Texturformat (und kleinere Texturen) zurückgegriffen werden.

Wenn Sie sich dafür entscheiden, für das Direct3D9-Spiel eine Erweiterung auf die Unterstützung höherer Direct3D-Featureebenen durchzuführen, ist es besser, zuerst das Portieren des vorhandenen Direct3D9-Grafikcodes abzuschließen. Nachdem das Spiel unter Direct3D11 funktioniert, können zusätzliche Renderpfade mit erweiterten Grafiken einfacher hinzugefügt werden.

Eine ausführliche Erklärung der Unterstützung von Featureebenen finden Sie unter [Direct3D-Featureebenen](https://msdn.microsoft.com/library/windows/desktop/ff476876). Eine vollständige Liste mit Direct3D11-Features finden Sie unter [Features vonDirect3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476342) und [Features von Direct3D11.1](https://msdn.microsoft.com/library/windows/desktop/hh404562).

## Featureebenen und die programmierbare Pipeline


Seit Direct3D9 hat sich die Hardware weiterentwickelt, und der programmierbaren Grafikpipeline wurden mehrere neue optionale Phasen hinzugefügt. Die für die Grafikpipeline verfügbaren Optionen variieren je nach Direct3D-Featureebene. Die Featureebene10.0 enthält die Geometrie-Shaderphase mit optionalem Streamout für das Rendern mit mehreren Durchläufen über die GPU. Die Featureebene11\_0 umfasst den Hüllen-Shader und den Domänen-Shader zur Verwendung mit Hardware-Tesselation. Die Featureebene11\_0 verfügt auch über vollständige Unterstützung für DirectCompute-Shader, während die Featureebenen10.x nur über Unterstützung für eine eingeschränkte Form von DirectCompute verfügen.

Alle Shader sind unter Verwendung von HLSL-Code mit einem Shaderprofil geschrieben, das einer Direct3D-Featureebene entspricht. Shaderprofile sind aufwärtskompatibel, sodass ein mit „vs\_4\_0\_level\_9\_1“ oder „ps\_4\_0\_level\_9\_1“ kompilierter HLSL-Shader geräteübergreifend verwendet werden kann. Shaderprofile sind nicht abwärtskompatibel, sodass ein mit „vs\_4\_1“ kompilierter Shader nur auf Geräten mit der Featureebene10\_1, 11\_0 oder 11\_1 verwendet werden kann.

Unter Direct3D9 wurden Konstanten für Shader mithilfe eines freigegebenen Arrays mit „SetVertexShaderConstant“ und „SetPixelShaderConstant“ verwaltet. Unter Direct3D11 werden Konstantenpuffer verwendet, bei denen es sich um Ressourcen handelt, z.B. ein Vertexpuffer oder ein Indexpuffer. Konstantenpuffer sind für eine effiziente Aktualisierung ausgelegt. Anstelle der Anordnung aller Shaderkonstanten in einem einzelnen globalen Array, ordnen Sie die Konstanten in logischen Gruppierungen an und verwalten sie mithilfe eines oder mehrerer Konstantenpuffer. Wenn Sie Ihr Direct3D9-Spiel zu Direct3D11 portieren, sollten Sie die Organisation der Konstantenpuffer so planen, dass diese entsprechend aktualisiert werden können. Gruppieren Sie Shaderkonstanten, die nicht für jeden Frame aktualisiert werden, in einem separaten Konstantenpuffer, damit diese Daten nicht ständig zusammen mit den dynamischeren Shaderkonstanten an den Grafikadapter hochgeladen werden müssen.

> **Hinweis**   In den meisten Direct3D9-Anwendungen werden ausgiebig Shader eingesetzt. Gelegentlich werden sie aber auch mit älteren festen Funktionen kombiniert. Beachten Sie, dass von Direct3D11 nur ein programmierbares Shadermodell verwendet wird. Die Legacyfeatures von Direct3D9 mit festen Funktionen werden als veraltet angesehen.

 

 

 







<!--HONumber=Aug16_HO3-->


