---
author: mtoepke
title: "Spieletechnologien für UWP-Apps"
description: "Dieses Handbuch enthält Informationen über verfügbare Technologien für die Entwicklung von Spielen für die Universelle Windows-Plattform (UWP)."
ms.assetid: bc4d4648-0d6e-efbb-7608-80bd09decd6e
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 83c3fda490d7ab821e8e584291ded642c9c11dd1

---

# Spieletechnologien für UWP-Apps (Universelle Windows-Plattform)


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Dieses Handbuch enthält Informationen über verfügbare Technologien für die Entwicklung von Spielen für die Universelle Windows-Plattform (UWP).

##  Vorteile von Windows10 für die Spielentwicklung


Mit der Einführung von UWP in Windows10 können Ihre Windows10-Titel auf allen Microsoft-Plattformen ausgeführt werden. Dank der kostenlosen Migration von früheren Windows-Versionen wird die Zahl der Windows10-Clients ständig zunehmen. Die Kombination dieser beiden Umstände bedeutet, dass Ihre Windows10-Titel über den Windows Store eine große Zahl von Kunden erreichen werden.

Darüber hinaus bietet Windows10 zahlreiche neue Features, die besonders für Spiele nützlich sind:

-   Reduzierte Speicherauslagerung und reduzierte Gesamtgröße des Speichersystems
-   Mit der verbesserten Grafikspeicherverwaltung wird aktiv mehr Speicher für das Spiel im Vordergrund zugeordnet und reserviert.

## UWP-Spiele mit C++ und DirectX


Echtzeitspiele, die hohe Leistung erfordern, sollten DirectX-APIs verwenden. DirectX ist eine Sammlung systemeigener APIs zum Erstellen von Spielen und Multimedia-Anwendungen, die hohe Leistung erfordern, z.B. 3D-Spiele. Da es sich bei DirectX-APIs um systemeigene APIs handelt, ist C++ die einzige für die Verwendung mit DirectX unterstützte Sprache.

## Entwicklungsumgebung


Um Spiele für UWP zu erstellen, müssen Sie Ihre Entwicklungsumgebung einrichten, indem Sie eine Kopie von Visual Studio 2015 installieren. Visual Studio2015 ermöglicht Ihnen das Erstellen von UWP-Apps und stellt Tools für die Spielentwicklung bereit:

-   Visual Studio-Tools für die DX-Spielprogrammierung – Visual Studio stellt Tools zum Erstellen, Bearbeiten, Anzeigen einer Vorschau und Exportieren von Bild-, Modell- und Shaderressourcen bereit. Außerdem sind Tools verfügbar, mit denen Sie Ressourcen zur Erstellungszeit konvertieren und DirectX-Grafikcode debuggen können. Weitere Informationen finden Sie unter [Visual Studio-Tools für die Spieleprogrammierung](set-up-visual-studio-for-game-development.md).
-   Visual Studio-Grafikdiagnosefeatures – Grafikdiagnosetools stehen nun als optionales Feature in Windows zur Verfügung. Mit den Diagnosetools können Sie Grafiken debuggen, Grafikframeanalysen ausführen und die GPU-Nutzung in Echtzeit überwachen. Weitere Informationen finden Sie unter [Tools für die Grafikdiagnose](use-the-directx-runtime-and-visual-studio-graphics-diagnostic-features.md).

Weitere Informationen finden Sie unter „Vorbereiten der Universellen Windows-Plattform und der DirectX-Umgebung für die Programmierung von Spielen“.

## Erste Schritte mit DirectX-Spielprojektvorlagen


Nach Einrichtung der Entwicklungsumgebung können Sie eine verwandte DirectX-Projektvorlage zum Erstellen Ihres DirectX-Spiels für UWP verwenden. Visual Studio2015 enthält drei Vorlagen für das Erstellen neuer UWP-DirectX-Projekte, **DirectX11-App (Universelles Windows)**, **DirectX12-App (Universelles Windows)** und **DirectX11- und XAML-App (Universelles Windows)**. Weitere Informationen finden Sie unter [DirectX-Spielprojektvorlagen](user-interface.md).

## Windows10-APIs


Windows 10 bietet eine umfangreiche Sammlung von APIs, die für die Spieleentwicklung hilfreich sind. Es gibt APIs für praktisch alle Aspekte von Spielen: 3D- und 2D-Grafiken, Audio, Eingabe, Textressourcen, Benutzeroberfläche und Netzwerk.

Es gibt viele verwandte APIs für die Spielentwicklung, nicht für alle Spiele müssen jedoch alle APIs verwendet werden. Einige Spiele werden beispielsweise nur 3D-Grafiken und Direct3D verwenden; andere Spielen werden nur 2D-Grafiken und Direct2D verwenden; und einige Spiele werden möglicherweise beides verwenden. Im folgenden Diagramm werden die mit der Entwicklung von Spielen zusammenhängenden APIs nach Funktionalitätstyp aufgeführt.

![Technologien für die Spielplattform](images/gameplatformtechnologies.png)

-   3D-Grafiken – Windows10 unterstützt zwei Sätze von 3D-Grafik-APIs, Direct3D11 und [Direct3D12](https://msdn.microsoft.com/library/windows/desktop/dn899121). Mit diesen beiden APIs können 3D- und 2D-Grafiken erstellt werden. Direct3D 11 und Direct3D 12 werden nicht zusammen verwendet, können jedoch zusammen mit APIs in der 2D-Grafik- und Benutzeroberflächengruppe verwendet werden. Weitere Informationen zum Verwenden der Grafik-APIs in Ihrem Spiel finden Sie unter [Grundlegendes zu 3D-Grafiken für DirectX-Spiele](an-introduction-to-3d-graphics-with-directx.md).

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Beschreibung</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Direct3D12</td>
    <td align="left"><p>Direct3D12 stellt die nächste Version von Direct3D vor, der 3D-Grafik-API im Herzen von DirectX. Diese Version von Direct3D ist schneller und effizienter als frühere Versionen von Direct3D. Der Kompromiss für die höhere Geschwindigkeit von Direct3D12 besteht darin, dass die Ebene niedrigerer ist. Sie müssen Ihre Grafikressourcen selbst verwalten und über eine umfassendere Grafikprogrammiererfahrung verfügen, um die höhere Geschwindigkeit zu realisieren.</p>
    <p><strong>Gründe für die Verwendung</strong></p>
    <p>Verwenden Sie Direct3D 12, wenn Sie die Leistung Ihres Spiels optimieren müssen und das Spiel CPU-gebunden ist.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie in der [Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn899121)-Dokumentation.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Direct3D11</td>
    <td align="left"><p>Direct3D11 ist die vorherige Version von Direct3D, mit der Sie 3D-Grafiken unter Verwendung einer höheren Ebene der Hardwareabstraktion als bei D3D12 erstellen können.</p>
    <p><strong>Gründe für die Verwendung</strong></p>
    <p>Verwenden Sie Direct3D 11, wenn Sie über vorhandenen Direct3D 11-Code verfügen, Ihr Spiel nicht CPU-gebunden ist, oder Sie davon profitieren möchten, dass Ihre Ressourcen verwaltet werden.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie in der [Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476080)-Dokumentation.</p></td>
    </tr>
    </tbody>
    </table>

     

-   2D-Grafiken und Benutzeroberfläche – APIs für 2D-Grafiken z.B. Text und Benutzeroberflächen. Alle 2D-Grafiken und Benutzeroberflächen-APIs sind optional.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Beschreibung</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Direct2D</td>
    <td align="left"><p>Direct2D ist eine hardwarebeschleunigte 2D-Grafik-API mit unmittelbarem Modus, die das Rendern mit hoher Leistung und in hoher Qualität für 2D-Geometrie, Bitmaps und Text bereitstellt. Die Direct2D-API baut auf Direct3D auf und ist für die Verwendung mit GDI, GDI+ und Direct3D konzipiert.</p>
    <p><strong>Gründe für die Verwendung</strong></p>
    <p>Direct2D kann anstelle von Direct3D zum Bereitstellen von Grafiken für reine 2D-Spiele verwendet werden, beispielsweise Side-Scroller oder Brettspiele. Es kann auch mit Direct3D für die vereinfachte Erstellung von 2D-Grafiken in einem 3D-Spiel verwendet werden, beispielsweise einer Benutzeroberfläche oder einer Heads-Up-Anzeige.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie in der [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990)-Dokumentation.</p></td>
    </tr>
    <tr class="even">
    <td align="left">DirectWrite</td>
    <td align="left"><p>DirectWrite stellt zusätzliche Funktionen für das Arbeiten mit Text bereit und kann mit Direct3D oder Direct2D zum Bereitstellen der Textausgabe für Benutzeroberflächen oder andere Bereiche verwendet werden, in denen Text erforderlich ist. DirectWrite unterstützt Messung, Zeichnung und Treffererkennung von Text in mehreren Formaten. DirectWrite behandelt Text in allen unterstützten Sprachen für globale und lokalisierte Anwendungen. DirectWrite stellt außerdem eine API für Glyphenrendering auf niedriger Ebene für Entwickler zur Ausführung von eigenem Layout und eigener Unicode-zu-Glyphen-Verarbeitung bereit.</p>
    <p><strong>Gründe für die Verwendung</strong></p>
    <p></p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie in der [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038)-Dokumentation.</p></td>
    </tr>
    <tr class="odd">
    <td align="left">DirectComposition</td>
    <td align="left"><p>DirectComposition ist eine Windows-Komponente, die eine Hochleistungs-Bitmapanordnung mit Übergängen, Effekten und Animationen ermöglicht. Anwendungsentwickler können die DirectComposition-API zum Erstellen von visuell ansprechenden Benutzeroberflächen verwenden, die umfassende und fließende animierte Übergänge von einem visuellen Objekt zum nächsten ermöglichen.</p>
    <p><strong>Gründe für die Verwendung</strong></p>
    <p>DirectComposition wurde für die vereinfachte Anordnung von visuellen Objekten und Erstellen von animierten Übergängen entwickelt. Wenn Ihr Spiel eine komplexe Benutzeroberfläche erfordert, können Sie DirectComposition zum vereinfachten Erstellen und Verwalten der Benutzeroberfläche verwenden.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie in der [DirectComposition](https://msdn.microsoft.com/library/windows/desktop/hh437371)-Dokumentation.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Audio – APIs für die Audiowiedergabe und die Anwendung von Audioeffekten. Informationen zum Verwenden von Audio-APIs in Ihrem Spiel finden Sie unter [Audio für Spiele](working-with-audio-in-your-directx-game.md).

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Beschreibung</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">XAudio2</td>
    <td align="left"><p>XAudio2 ist eine Low-Level-Audio-API, die eine grundlegende Signalverarbeitung und -abmischung bereitstellt. XAudio wurde für eine hohe Reaktionsfähigkeit in Bezug auf Audiomodule von Spielen entwickelt. Gleichzeitig können weiterhin benutzerdefinierte Audioeffekte und komplexe Ketten von Audioeffekten und -filtern erstellt werden.</p>
    <p><strong>Gründe für die Verwendung</strong></p>
    <p>Verwenden Sie XAudio2, wenn Ihr Spiel Sounds mit minimalem Aufwand und minimaler Verzögerung wiedergeben muss.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie in der [XAudio2](https://msdn.microsoft.com/library/windows/desktop/hh405049)-Dokumentation.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Media Foundation</td>
    <td align="left"><p>Microsoft Media Foundation wurde für die Wiedergabe von Mediendateien und Streams (Audio und Video) entwickelt, kann jedoch auch in Spielen verwendet werden, wenn eine Funktionalität auf höherer Ebene als XAudio2 erforderlich ist und der zusätzliche Aufwand akzeptabel ist.</p>
    <p><strong>Gründe für die Verwendung</strong></p>
    <p>Media Foundation ist bei Filmszenen oder nichtinteraktiven Komponenten des Spiels besonders nützlich. Media Foundation ist auch für die Decodierung von Audiodateien für die Wiedergabe mit XAudio2 nützlich.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie unter [Microsoft Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197).</p></td>
    </tr>
    </tbody>
    </table>

     

-   Eingabe – APIs für die Eingabe über Tastatur, Maus, Gamepad und andere Benutzereingabequellen.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Beschreibung</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">XInput</td>
    <td align="left"><p>Mit der XInput-Gamecontroller-API können Anwendungen Eingaben von Gamecontrollern erhalten.</p>
    <p><strong>Gründe für die Verwendung</strong></p>
    <p>Wenn Ihr Spiel die Gamepadeingabe unterstützen muss und Sie über vorhandenen XInput-Code verfügen, können Sie weiterhin XInput verwenden. XInput wurde durch Windows.Gaming.Input für UWP ersetzt, und Sie sollten beim Schreiben von neuem Eingabecode Windows.Gaming.Input anstelle von XInput verwenden.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie in der [XInput](https://msdn.microsoft.com/library/windows/desktop/hh405053)-Dokumentation.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Windows.Gaming.Input</td>
    <td align="left"><p>Die Windows.Gaming.Input-API ersetzt XInput und bietet die gleiche Funktionalität mit den folgenden Vorteilen gegenüber Xinput:</p>
    <ul>
    <li>Geringere Ressourcenverwendung</li>
    <li>Geringere Latenz des API-Aufrufs zum Abrufen von Eingaben</li>
    <li>Die Möglichkeit, mit mehr als 4 Gamepads gleichzeitig zu arbeiten</li>
    <li>Die Möglichkeit zum Zugreifen auf zusätzliche Xbox One-Gamepadfeatures, z.B. Triggervibrationsmotoren.</li>
    <li>Die Möglichkeit, beim Herstellen oder Trennen einer Verbindung durch ein Ereignis anstelle von Abruf benachrichtigt zu werden.</li>
    <li>Die Möglichkeit zum Zuweisen der Eingabe an einen bestimmten Benutzer (Windows.System.User)</li>
    </ul>
    <p><strong>Gründe für die Verwendung</strong></p>
    <p>Wenn Ihr Spiel Eingaben über ein Gamepad unterstützen muss und einen vorhandenen XInput-Code nicht verwendet, oder wenn Sie einen der oben genannten Vorteile nutzen möchten, sollten Sie Windows.Gaming.Input verwenden.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie in der Dokumentation für [<strong>Windows.Gaming.Input</strong>](https://msdn.microsoft.com/library/windows/apps/dn707817).</p></td>
    </tr>
    <tr class="odd">
    <td align="left">Windows.UI.Core.CoreWindow</td>
    <td align="left"><p>Mit der Windows.UI.Core.CoreWindow-Klasse können Sie nachverfolgen, wann der Zeiger gedrückt oder verschoben wird oder wann eine Taste gedrückt wird.</p>
    <p><strong>Gründe für die Verwendung</strong></p>
    <p>Verwenden Sie die Windows.UI.Core.CoreWindows-Ereignisse, wenn Sie nachverfolgen möchten, wann die Maus oder eine Taste in Ihrem Spiel gedrückt wird.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen zum Verwenden der Maus oder Tastatur in Ihrem Spiel finden Sie unter [Bewegungs-/Blicksteuerungen für Spiele](tutorial--adding-move-look-controls-to-your-directx-game.md).</p></td>
    </tr>
    </tbody>
    </table>

     

-   Math-APIs – APIs zum Vereinfachen häufig verwendeter mathematischer Vorgänge.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Beschreibung</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">DirectXMath</td>
    <td align="left"><p>Die DirectXMath-API bietet SIMD-freundliche C++-Typen und Funktionen für allgemeine lineare Algebra- und mathematische Grafikvorgänge für Spiele.</p>
    <p><strong>Gründe für die Verwendung</strong></p>
    <p>Die Verwendung von DirectXMath ist optional und vereinfacht allgemeine mathematische Vorgänge.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie in der [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833)-Dokumentation.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Netzwerke – APIs für die Kommunikation mit anderen Computern und Geräten entweder über das Internet oder private Netzwerke.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Beschreibung</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Windows.Networking.Sockets</td>
    <td align="left"><p>Der Windows.Networking.Sockets-Namespace stellt TCP- und UDP-Sockets bereit, die eine zuverlässige bzw. nicht zuverlässige Netzwerkkommunikation ermöglichen.</p>
    <p><strong>Gründe für die Verwendung</strong></p>
    <p>Verwenden Sie Windows.Networking.Sockets, wenn Ihr Spiel mit anderen Computern und Geräten über das Netzwerk kommunizieren muss.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie unter [Netzwerk für Spiele](work-with-networking-in-your-directx-game.md).</p></td>
    </tr>
    <tr class="even">
    <td align="left">Windows.Web.HTTP</td>
    <td align="left"><p>Der Windows.Web.HTTP-Namespace stellt eine zuverlässige Verbindung mit HTTP-Servern bereit, die für den Zugriff auf eine Website verwendet werden kann.</p>
    <p><strong>Gründe für die Verwendung</strong></p>
    <p>Verwenden Sie Windows.Web.HTTP, wenn Ihr Spiel zum Abrufen oder Speichern von Informationen auf eine Website zugreifen muss.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie unter [Netzwerk für Spiele](work-with-networking-in-your-directx-game.md).</p></td>
    </tr>
    </tbody>
    </table>

     

-   Support-Dienstprogramme - Bibliotheken, die auf Grundlage von Windows 10-APIs entwickelt wurden.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">Bibliothek</th>
    <th align="left">Beschreibung</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">DirectX-Toolkit</td>
    <td align="left"><p>Das DirectX-Toolkit (DirectXTK) ist eine Sammlung von Hilfsklassen für DirectX 11.x-Code in C++.</p>
    <p><strong>Gründe für die Verwendung</strong></p>
    <p>Verwenden Sie das DirectX-Toolkit, wenn Sie C++-Entwickler und auf der Suche nach einem modernen Ersatz für älteren D3DX-Hilfsprogrammcode sind, oder ein XNA Game Studio-Entwickler sind, der zu systemeigenen C++ wechselt.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie auf der DirectX-Toolkit-Projektseite [https://github.com/Microsoft/DirectXTK](https://github.com/Microsoft/DirectXTK).</p></td>
    </tr>
    <tr class="even">
    <td align="left">Win2D</td>
    <td align="left"><p>Win2D ist eine einfach zu verwendende Windows-Runtime-API für 2D-Grafikrendering im unmittelbaren Modus.</p>
    <p><strong>Gründe für die Verwendung</strong></p>
    <p>Verwenden Sie Win2D, wenn Sie C++-Entwickler sind und einen einfacher zu verwendenden WinRT-Wrapper für Direct2D und DirectWrite benötigen, oder C#-Entwickler sind und Direct2D und DirectWrite verwenden möchten.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie auf der Win2D-Projektseite [https://github.com/Microsoft/Win2D](https://github.com/Microsoft/Win2D)</p></td>
    </tr>
    </tbody>
    </table>

     

## Xbox Live-Dienste


Xbox Live-Features – Geräteübergreifende Spiele mit Xbox, Erfolgen, Gamerscore uvm. sind bald für Windows 10 erhältlich. Bald werden Sie unter Verwendung von ID@Xbox Live zu Ihren UWP-Spielen hinzufügen können! In Zukunft werden wir Sie auch bei der Bereitstellung von UWP-Spielen zusammen mit Xbox One unterstützen. Weitere Informationen finden Sie auf der [ID@Xbox](http://www.xbox.com/developers/id)-Seite.

##  Alternativen zum Schreiben von Spielen mit DirectX und UWP


### UWP-Spiele ohne DirectX

Einfachere Spiele mit minimalem Leistungsanforderungen, z.B. Kartenspiele oder Brettspiele, können ohne DirectX und müssen nicht unbedingt in C++ geschrieben werden. Bei dieser Art von Spielen können alle von UWP unterstützten Sprachen verwendet werden, z.B. C#, Visual Basic, C++ und HTML/JavaScript. Wenn Ihr Spiel weder eine hohe Leistung noch rechenintensive Grafiken erfordert, sollten Sie sich das [Beispiel für ein JavaScript- und HTML5-Fingereingabespiel](http://code.msdn.microsoft.com/windowsapps/JavaScript-and-HTML5-touch-d96f6031) ansehen.

### Spielengines

Eine Alternative zum Schreiben eigener Spielemodule mithilfe der Windows-APIs für die Entwicklung von Spielen stellen die zahlreichen qualitativ hochwertigen Spielemodule dar, die auf den Windows-APIs für die Entwicklung von Spielen basieren und für die Entwicklung von Spielen auf Windows-Plattformen zur Verfügung stehen. Wenn Sie eine Spielengine oder Bibliothek in Erwägung ziehen, können Sie zwischen den folgenden Möglichkeiten wählen:

-   Vollständige Spielengine – Eine vollständige Spielengine umfasst die meisten bzw. alle Windows 10-APIs, die Sie zum Schreiben einer eigenen Spielengine verwenden würden, z.B. Grafik, Eingabe und Netzwerk. Vollständige Spielengines können auch Spiellogikfunktionalität bereitstellen, z.B. künstliche Intelligenz und Pfadsuche.
-   Grafikengine – Grafikengines umfassen Windows 10-Grafik-APIs, verwalten Grafikressourcen und unterstützen zahlreiche Modell- und Weltformate.
-   Audioengine – Audioengines umfassen Windows 10-Audio-APIs, verwalten Audioressourcen und stellen erweiterte Audiowiedergabe und -Effekte bereit.
-   Netzwerkengine – Netzwerkengines umfassen Windows 10-Netzwerk-APIs zum Hinzufügen von Peer-zu-Peer- oder serverbasierter Multiplayerunterstützung zu Ihrem Spiel und enthalten ggf. erweiterte Netzwerkfunktionalität zur Unterstützung einer großen Anzahl von Spielern.
-   Engine für künstliche Intelligenz und Pfadsuche – Engines für künstliche Intelligenz und Pfadsuche stellen ein Framework zur Steuerung des Agentverhaltens in einem Spiel bereit.
-   Engines für besondere Zwecke – Es stehen unterschiedliche weitere Engines zur Bewältigung von nahezu allen möglichen Spielentwicklungsaufgaben zur Verfügung, z.B. Erstellen von Inventarsystemen und Dialogstrukturen.

## Übermitteln eines Spiels an den Store


Wenn Ihr Spiel zur Veröffentlichung bereit steht, müssen Sie ein Entwicklerkonto erstellen und Ihr Spiel an den Windows Store übermitteln.

Informationen zum Übermitteln Ihres Spiels an den Windows Store finden Sie unter <https://dev.windows.com/publish>.

 

 







<!--HONumber=Aug16_HO3-->


