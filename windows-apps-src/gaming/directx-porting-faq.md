---
title: Portierung von DirectX 11 – häufig gestellte Fragen
description: Antworten auf häufig gestellte Fragen zur Portierung von Spielen zur Universellen Windows-Plattform (UWP)
ms.assetid: 79c3b4c0-86eb-5019-97bb-5feee5667a2d
---

# DirectX 11 – Häufig gestellte Fragen zur Portierung


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Antworten auf häufig gestellte Fragen zur Portierung von Spielen zur Universellen Windows-Plattform (UWP)

## Kann ich mein Spiel durch Suchen und Ersetzen von API-Methoden portieren, oder ist eine sorgfältigere Planung des Portierungsprozesses erforderlich?


Direct3D 11 wurde gegenüber Direct3D 9 deutlich aktualisiert. Es wurden mehrere Paradigmenwechsel vorgenommen, darunter separate APIs für den virtualisierten Grafikadapter und seinen Kontext sowie eine neue Polymorphieebene für Geräteressourcen. Im Wesentlichen kann Ihr Spiel Grafikhardware weiter wie zuvor verwenden. Sie müssen sich allerdings mit der neuen API-Architektur von Direct3D 11 vertraut machen und alle Teile Ihres Grafikcodes zur Verwendung der richtigen API-Komponenten aktualisieren. Siehe [Portierungskonzepte und Überlegungen zur Portierung](porting-considerations.md).

## Wofür dient der neue Gerätekontext? Muss ich mein Direct3D 9-Gerät durch das Direct3D 11-Gerät ersetzen, den Gerätekontext oder beides?


Das Direct3D-Gerät wird jetzt zum Erstellen von Ressourcen im Videospeicher verwendet. Der Gerätekontext dient dagegen zum Festlegen des Pipelinestatus und Generieren von Renderbefehlen. Weitere Informationen finden Sie unter [Was sind die wichtigsten Änderungen seit Direct3D 9?](understand-direct3d-11-1-concepts.md)

##  Muss ich meinen Spieltimer für UWP aktualisieren?


[**QueryPerformanceCounter**](https://msdn.microsoft.com/library/windows/desktop/ms644904) in Verbindung mit [**QueryPerformanceFrequency**](https://msdn.microsoft.com/library/windows/desktop/ms644905) ist nach wie vor die beste Methode, um einen Spieltimer für UWP-Apps zu implementieren.

Eine Kleinigkeit sollten Sie im Hinblick auf Timer und den UWP-App-Lebenszyklus beachten. Das Anhalten und Fortsetzen ist nicht das Gleiche wie das erneute Starten eines Desktopspiels durch den Spieler, da das Spiel in diesem Fall eine Momentaufnahme ab dem Zeitpunkt fortsetzt, zu dem das Spiel zuletzt gespielt wurde. Ist viel Zeit vergangen – z. B. einige Wochen – können bei einigen Timerimplementierungen Fehler auftreten. Sie können App-Lebenszyklusereignisse verwenden, um Ihren Timer beim Fortsetzen des Spiels zurückzusetzen.

Spiele, die noch immer die RDTSC-Anweisung verwenden, müssen aktualisiert werden. Siehe [Zeitliche Steuerung von Spielen und Multi-Core-Prozessoren](https://msdn.microsoft.com/library/windows/desktop/ee417693).

## Mein Spielcode basiert auf D3DX und DXUT. Gibt es irgendetwas, das ich zum Migrieren des Codes verwenden kann?


Das Communityprojekt [DirectX-Toolkit (DirectXTK)](http://go.microsoft.com/fwlink/p/?LinkID=248929) bietet Hilfsklassen zur Verwendung mit Direct3D 11.

##  Wie verwalte ich Codepfade für den Desktop und den Windows Store?


In der Artikelserie [Dual-use Coding Techniques for Games](http://go.microsoft.com/fwlink/p/?LinkID=286210) von Chuck Walbourn finden Sie Informationen zum Freigeben von Code zwischen den Desktop- und Windows Store-Codepfaden.

##  Wie lade ich Bildressourcen in meine DirectX-UWP-App?


Zum Laden von Bildern sind zwei API-Pfade verfügbar:

-   Die Inhaltspipeline konvertiert Bilder in DDS-Dateien, die als Direct3D-Texturressourcen verwendet werden. Siehe [Verwenden von 3D-Ressourcen in Spielen oder Apps](https://msdn.microsoft.com/library/windows/apps/hh972446.aspx).
-   Mit der [Windows-Bilderstellungskomponente](https://msdn.microsoft.com/library/windows/desktop/ee719902) können Bilder in vielen verschiedenen Formaten geladen werden. Sie kann sowohl für Direct2D-Bitmaps als auch für Direct3D-Texturressourcen verwendet werden.

Sie können auch den DDSTextureLoader und den WICTextureLoader aus [DirectXTK](http://go.microsoft.com/fwlink/p/?LinkID=248929) oder [DirectXTex](http://go.microsoft.com/fwlink/p/?LinkID=248926) verwenden.

## Wo finde ich das DirectX SDK?


Das DirectX SDK ist im Windows SDK enthalten. Das neueste DirectX SDK, das vom Windows SDK getrennt war, wurde im Juni 2010 veröffentlicht. Direct3D-Beispiele sind wie die anderen Windows-App-Beispiele in der Codegalerie enthalten.

## Was ist mit verteilbaren DirectX-Komponenten?


Der Großteil der Komponenten im Windows SDK ist bereits in unterstützten Versionen des Betriebssystems enthalten oder verfügt nicht über eine DLL-Komponente (z. B. DirectXMath). Alle Direct3D-API-Komponenten, die von UWP-Apps verwendet werden können, werden bereits für Ihr Spiel verfügbar sein. Sie müssen sie nicht neu verteilen.

Win32-Desktopanwendungen verwenden weiterhin DirectSetup. Wenn Sie die Desktopversion Ihres Spiels aktualisieren, gehen Sie daher wie unter [Direct3D 11-Bereitstellung für Spielentwickler](https://msdn.microsoft.com/library/windows/desktop/ee416644) beschrieben vor.

## Kann ich meinen Desktopcode vor der Umstellung von Effects auf DirectX 11 aktualisieren?


Siehe [Effects für Direct3D 11-Update](http://go.microsoft.com/fwlink/p/?LinkId=271568). Mit Effects 11 können Abhängigkeiten von älteren DirectX-SDK-Headern entfernt werden. Es ist als Portierungshilfsmittel vorgesehen und kann nur für Desktop-Apps verwendet werden.

##  Gibt es einen Pfad für die Portierung meines DirectX 8-Spiels zu UWP?


Ja:

-   Lesen Sie [Konvertieren in Direct3D 9](https://msdn.microsoft.com/library/windows/desktop/bb204851).
-   Stellen Sie sicher, dass Ihr Spiel keine Überreste der festen Pipeline enthält (siehe [Veraltete Funktionen](https://msdn.microsoft.com/library/windows/desktop/cc308047)).
-   Verwenden Sie anschließend den DirectX 9-Portierungspfad: [Portieren von D3D 9 zu UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

##  Kann ich mein DirectX 10- oder 11-Spiel zu UWP portieren?


DirectX 10.x- und 11-Desktopspiele können ohne großen Aufwand zu UWP portiert werden. Siehe [Migrieren zu Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476190).

## Wie wähle ich bei einem Multi-Monitor-System das richtige Anzeigegerät aus?


Der Benutzer wählt aus, auf welchem Monitor Ihre App angezeigt wird. Rufen Sie [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) auf, und legen Sie den ersten Parameter auf **nullptr** fest, damit Windows den korrekten Adapter auswählt. Rufen Sie anschließend die [**IDXGIDevice interface**](https://msdn.microsoft.com/library/windows/desktop/bb174527) des Geräts ab, rufen Sie [**GetAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174531) auf, und erstellen Sie die Swapchain mit dem DXGI-Adapter.

## Wie aktiviere ich Antialiasing?


Antialiasing (Multisampling) wird beim Erstellen des Direct3D-Geräts aktiviert. Listen Sie die Multisamplingunterstützung auf, indem Sie [**CheckMultisampleQualityLevels**](https://msdn.microsoft.com/library/windows/desktop/ff476499) aufrufen, und legen Sie dann Multisamplingoptionen in der [**DXGI\_SAMPLE\_DESC-Struktur**](https://msdn.microsoft.com/library/windows/desktop/bb173072) fest, wenn Sie [**CreateSurface**](https://msdn.microsoft.com/library/windows/desktop/bb174530) aufrufen.

## Mein Spiel rendert mit Multithreading und/oder verzögertem Rendering. Was muss ich in diesem Zusammenhang bei Direct3D 11 beachten?


Lesen Sie [Einführung in das Multithreading in Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476891). Dort werden die ersten Schritte erläutert. Eine Liste der wichtigsten Unterschiede finden Sie unter [Unterschiede beim Threading zwischen Direct3D-Versionen](https://msdn.microsoft.com/library/windows/desktop/ff476890). Beachten Sie, dass beim verzögerten Rendering ein *verzögerter Gerätekontext* anstelle eines *unmittelbaren Kontextes* verwendet wird.

## Wo finde ich weitere Informationen zur programmierbaren Pipeline seit Direct3D 9?


Lesen Sie die folgenden Themen:

-   [Programmieranleitung für HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509635)
-   [Häufig gestellte Fragen zu Direct3D 10](https://msdn.microsoft.com/library/windows/desktop/ee416643)

## Was sollte ich anstelle des X-Dateiformats für meine Modelle verwenden?


Es gibt zwar keinen offiziellen Ersatz für das X-Dateiformat, in vielen Beispielen wird aber das SDKMesh-Format verwendet. Visual Studio bietet außerdem eine [Inhaltspipeline](https://msdn.microsoft.com/library/windows/apps/hh972446.aspx), die verschiedene gängige Formate in CMO-Dateien kompiliert, die mit Code aus dem Visual Studio 3D-Starter Kit oder mit dem [DirectXTK](http://go.microsoft.com/fwlink/p/?LinkID=248929) geladen werden können.

## Wie debugge ich meine Shader?


Microsoft Visual Studio 2015 umfasst Diagnosetools für DirectX-Grafiken. Siehe [Debuggen von DirectX-Grafiken](https://msdn.microsoft.com/library/windows/apps/hh315751.aspx).

##  Was ist das Direct3D 11-Äquivalent für die *x*-Funktion?


Informationen hierzu finden Sie in der [Funktionszuordnung](feature-mapping.md#function-mapping) in „Zuordnung von DirectX 9-Features zu DirectX 11-APIs“.

##  Was ist das DXGI\_FORMAT-Äquivalent des *y*-Oberflächenformats?


Informationen hierzu finden Sie in der [Oberflächenformatzuordnung](feature-mapping.md#surface-format-mapping) in „Zuordnung von DirectX 9-Funktionen zu DirectX 11-APIs“.

 

 






<!--HONumber=Mar16_HO1-->


