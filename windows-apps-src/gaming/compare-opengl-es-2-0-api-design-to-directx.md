---
title: Planen der Portierung von OpenGL ES 2.0 zu Direct3D
description: Wenn Sie ein Spiel von der iOS- oder Android-Plattform portieren, haben Sie vermutlich erheblich in OpenGL ES 2.0 investiert.
ms.assetid: a31b8c5a-5577-4142-fc60-53217302ec3a
---

# Planen der Portierung von OpenGL ES 2.0 zu Direct3D


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**Wichtige APIs**

-   [Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476080)
-   [Visual C++](https://msdn.microsoft.com/en-us/library/windows/apps/60k1461a.aspx)

Wenn Sie ein Spiel von der iOS- oder Android-Plattform portieren, haben Sie vermutlich erheblich in OpenGL ES 2.0 investiert. Beim Vorbereiten der Portierung Ihrer Grafikpipeline-Codebasis zu Direct3D 11 und der Windows-Runtime sind im Vorfeld einige Dinge zu beachten.

Meist beinhaltet das Portieren eine anfängliche Überprüfung der Codebasis und die Zuordnung allgemeiner APIs und Muster zwischen den beiden Modellen. Sie können diesen Vorgang vereinfachen, indem Sie sich die Zeit für die Lektüre dieses Themas nehmen.

Folgende Punkte sind beim Portieren von Grafiken von OpenGL ES 2.0 zu Direct3D 11 zu beachten.

## Hinweise zu bestimmten OpenGL ES 2.0-Anbietern


Die Themen zur Portierung in diesem Abschnitt beziehen sich auf die Windows-Implementierung der von der Khronos Group erstellten OpenGL ES 2.0-Spezifikation. Alle OpenGL ES 2.0-Codebeispiele wurden mit Visual Studio 2012 und einfacher Windows C-Syntax entwickelt. Bedenken Sie im Fall einer Objective-C (iOS)- oder Java (Android)-Codebasis, dass in den gezeigten OpenGL ES 2.0-Codebeispielen möglicherweise keine ähnliche API-Aufrufsyntax oder ähnlichen Parameter verwendet werden. Wir haben versucht, diesen Leitfaden so plattformagnostisch wie möglich zu halten.

In dieser Dokumentation werden nur die APIs der 2.0-Spezifikation für den OpenGL ES-Code und die zugehörige Referenz verwendet. Falls Sie von OpenGL ES 1.1 oder 3.0 portieren, kann diese Dokumentation trotzdem hilfreich sein, auch wenn Ihnen einige der OpenGL ES 2.0-Codebeispiele und der Kontext vielleicht nicht vertraut sind.

In den Direct3D 11-Beispielen in diesem Thema wird Microsoft Windows C++ mit Komponentenerweiterungen (CX) verwendet. Weitere Informationen zu dieser Version der C++-Syntax finden Sie unter [Visual C++](https://msdn.microsoft.com/library/windows/apps/60k1461a.aspx), [Komponentenerweiterungen für Laufzeitplattformen](https://msdn.microsoft.com/library/windows/apps/xey702bw.aspx) und [Kurzreferenz (C++\\CX)](https://msdn.microsoft.com/library/windows/apps/br212455.aspx).

## Verständnis der Hardwareanforderungen und Ressourcen


Die von OpenGL ES 2.0 unterstützten Grafikverarbeitungsfunktionen entsprechen in etwa den in Direct3D 9.1 verfügbaren Funktionen. Wenn Sie die Vorteile der umfassenderen Funktionen von Direct3D 11 nutzen möchten, lesen Sie beim Planen der Portierung die Dokumentation zu [Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476080) oder im Anschluss an die Planung die Themen zum [Portieren von DirectX 9 zur Universellen Windows-Plattform (UWP)](porting-your-directx-9-game-to-windows-store.md).

Um Ihre erste Portierung möglichst einfach zu machen, sollten Sie mit einer Visual Studio Direct3D-Vorlage beginnen. Sie enthält einen einfachen bereits konfigurierten Renderer und unterstützt Features von UWP-Apps wie das Neuerstellen von Ressourcen bei Fensteränderungen und Direct3D-Funktionsebenen.

## Verständnis der Direct3D-Funktionsebenen


Direct3D 11 bietet Unterstützung für die Hardwarefunktionsebenen 9\_1 (Direct3D 9.1) bis 11\_1. Diese Funktionsebenen geben die Verfügbarkeit bestimmter Grafikfunktionen und -ressourcen an. Die meisten OpenGL ES 2.0-Plattformen unterstützen eine Direct3D 9.1-Funktionsgruppe (Funktionsebene 9\_1).

## Überprüfen der DirectX-Grafikfunktionen und -APIs


| API-Familie                                                | Beschreibung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534)                     | Die DirectX Graphics Infrastructure (DXGI) stellt eine Schnittstelle zwischen der Grafikhardware und Direct3D bereit. Sie legt die Geräteadapter- und Hardwarekonfiguration mit den COM-Schnittstellen [**IDXGIAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174523) und [**IDXGIDevice1**](https://msdn.microsoft.com/library/windows/desktop/hh404543) fest. Verwenden Sie sie zum Erstellen und Konfigurieren Ihrer Puffer und anderen Fensterressourcen. Beachten Sie insbesondere, dass das [**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556)-Factorymuster zum Erfassen der Grafikressourcen verwendet wird, einschließlich der Swapchain (eine Gruppe von Framepuffern). Da DXGI der Besitzer der Swapchain ist, wird die [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)-Schnittstelle verwendet, um Frames auf dem Bildschirm darzustellen. |
| [Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476080)       | Direct3D ist der Satz von APIs, die eine virtuelle Darstellung der Grafikschnittstelle bereitstellen und es Ihnen ermöglichen, damit Grafiken zu zeichnen. Version 11 ist von den Funktionen her in etwa mit OpenGL 4.3 vergleichbar. (OpenGL ES .0 ähnelt hinsichtlich der Funktionen dagegen DirectX9 und OpenGL 2.0, verwendet aber die einheitliche Shaderpipeline von OpenGL 3.0.) Der Großteil der Arbeit wird mit den ID3D11Device1- und ID3D11DeviceContext1-Schnittstellen erledigt, die Zugriff auf einzelne Ressourcen und Unterressourcen bzw. den Renderkontext bieten.                                                                                                                                          |
| [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990)                      | Direct2D stellt einen Satz von APIs für GPU-beschleunigtes 2D-Rendering bereit. Hinsichtlich des Verwendungszwecks ist es mit OpenVG vergleichbar.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038)            | DirectWrite stellt einen Satz von APIs für GPU-beschleunigtes Schriftartrendering in hoher Qualität bereit.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833)                  | DirectXMath stellt einen Satz von APIs und Makros für die Behandlung allgemeiner linearer Algebra sowie trigonometrischer Typen, Werte und Funktionen bereit. Diese Typen und Funktionen sind so konzipiert, dass sie gut mit Direct3D und den zugehörigen Shadervorgängen funktionieren.                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| [DirectX HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509580) | Die von Direct3D-Shadern verwendete aktuelle HLSL-Syntax. Sie implementiert das Direct3D-Shadermodell 5.0.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

 

## Überprüfen der Windows-Runtime-APIs und -Vorlagenbibliothek


Die Windows-Runtime-APIs stellen die allgemeine Infrastruktur für UWP-Apps bereit. Weitere Informationen finden [hier](https://msdn.microsoft.com/library/windows/apps/br211377).

Folgende wichtige Windows-Runtime-APIs werden beim Portieren Ihrer Grafikpipeline verwendet:

-   [**Windows::UI::Core::CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)
-   [**Windows::UI::Core::CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)
-   [**Windows::ApplicationModel::Core::IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478)
-   [**Windows::ApplicationModel::Core::CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017)

Die C++-Vorlagenbibliothek für Windows-Runtime (WRL) ist eine Vorlagenbibliothek, die eine einfache Methode zum Erstellen und Verwenden von Komponenten für Windows-Runtime bietet. Die Direct3D 11-APIs für UWP-Apps lassen sich am besten in Verbindung mit den Schnittstellen und Typen in dieser Bibliothek wie etwa intelligenten Zeigern ([ComPtr](https://msdn.microsoft.com/library/windows/apps/br244983.aspx)) verwenden. Weitere Informationen zur WRL finden Sie unter [C++-Vorlagenbibliothek für Windows-Runtime (WRL)](https://msdn.microsoft.com/en-us/library/windows/apps/hh438466.aspx).

## Ändern des Koordinatensystems


Ein Unterschied, der bei der Portierung anfänglich für Verwirrung sorgen kann, ist die Umstellung vom traditionellen rechtshändigen Koordinatensystem von OpenGL auf das standardmäßige linkshändige Koordinatensystem von Direct3D. Diese Änderung der Koordinatenmodellierung wirkt sich auf viele Teile des Spiels aus – angefangen bei der Einrichtung und Konfiguration der Scheitelpunktpuffer bis hin zu vielen der mathematischen Matrixfunktionen. Die folgenden zwei Änderungen sind am wichtigsten:

-   Spiegeln Sie die Reihenfolge von Dreieckscheitelpunkten, sodass sie von Direct3D von vorne im Uhrzeigersinn durchlaufen werden. Wenn die Scheitelpunkte in der OpenGL-Pipeline beispielsweise als 0, 1 und 2 indiziert wurden, übergeben Sie sie stattdessen als 0, 2, 1 an Direct3D.
-   Verwenden Sie die Ansichtsmatrix, um den Raum um -1.0f in der z-Richtung zu skalieren, sodass die z-Achsenkoordinaten umgekehrt werden. Kehren Sie dazu das Vorzeichen der Werte an den Positionen M31, M32 und M33 in der Ansichtsmatrix um (beim Portieren in den [**Matrix**](https://msdn.microsoft.com/library/windows/desktop/bb147180)-Typ). Falls M34 nicht 0 ist, kehren Sie das Vorzeichen dieser Position ebenfalls um.

Direct3D kann jedoch ein rechtshändiges Koordinatensystem unterstützen. DirectXMath bietet eine Reihe von Funktionen, die sowohl für links- als auch für rechtshändige Koordinatensysteme funktionieren. Mithilfe dieser Funktionen können Sie einen Teil Ihrer ursprünglichen Gitterdaten und der Matrixverarbeitung beibehalten. Dazu zählen:

| DirectXMath-Matrixfunktion                                                   | Beschreibung                                                                                                                 |
|-------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| [**XMMatrixLookAtLH**](https://msdn.microsoft.com/library/windows/desktop/ee419969)                               | Erstellt eine Ansichtsmatrix für ein linkshändiges Koordinatensystem mit einer Kameraposition, einer Aufwärtsrichtung und einem Fokus.       |
| [**XMMatrixLookAtRH**](https://msdn.microsoft.com/library/windows/desktop/ee419970)                               | Erstellt eine Ansichtsmatrix für ein rechtshändiges Koordinatensystem mit einer Kameraposition, einer Aufwärtsrichtung und einem Fokus.      |
| [**XMMatrixLookToLH**](https://msdn.microsoft.com/library/windows/desktop/ee419971)                               | Erstellt eine Ansichtsmatrix für ein linkshändiges Koordinatensystem mit einer Kameraposition, einer Aufwärtsrichtung und einer Kamerarichtung.  |
| [**XMMatrixLookToRH**](https://msdn.microsoft.com/library/windows/desktop/ee419972)                               | Erstellt eine Ansichtsmatrix für ein rechtshändiges Koordinatensystem mit einer Kameraposition, einer Aufwärtsrichtung und einer Kamerarichtung. |
| [**XMMatrixOrthographicLH**](https://msdn.microsoft.com/library/windows/desktop/ee419975)                   | Erstellt eine orthogonale Projektionsmatrix für ein linkshändiges Koordinatensystem.                                                 |
| [**XMMatrixOrthographicOffCenterLH**](https://msdn.microsoft.com/library/windows/desktop/ee419976) | Erstellt eine benutzerdefinierte orthogonale Projektionsmatrix für ein linkshändiges Koordinatensystem.                                           |
| [**XMMatrixOrthographicOffCenterRH**](https://msdn.microsoft.com/library/windows/desktop/ee419977) | Erstellt eine benutzerdefinierte orthogonale Projektionsmatrix für ein rechtshändiges Koordinatensystem.                                          |
| [**XMMatrixOrthographicRH**](https://msdn.microsoft.com/library/windows/desktop/ee419978)                   | Erstellt eine orthogonale Projektionsmatrix für ein rechtshändiges Koordinatensystem.                                                |
| [**XMMatrixPerspectiveFovLH**](https://msdn.microsoft.com/library/windows/desktop/ee419979)               | Erstellt eine linkshändige perspektivische Projektionsmatrix auf der Grundlage eines Sichtfelds.                                                |
| [**XMMatrixPerspectiveFovRH**](https://msdn.microsoft.com/library/windows/desktop/ee419980)               | Erstellt eine rechtshändige perspektivische Projektionsmatrix auf der Grundlage eines Sichtfelds.                                               |
| [**XMMatrixPerspectiveLH**](https://msdn.microsoft.com/library/windows/desktop/ee419981)                     | Erstellt eine linkshändige perspektivische Projektionsmatrix.                                                                         |
| [**XMMatrixPerspectiveOffCenterLH**](https://msdn.microsoft.com/library/windows/desktop/ee419982)   | Erstellt eine benutzerdefinierte Version einer linkshändigen perspektivischen Projektionsmatrix.                                                     |
| [**XMMatrixPerspectiveOffCenterRH**](https://msdn.microsoft.com/library/windows/desktop/ee419983)   | Erstellt eine benutzerdefinierte Version einer rechtshändigen perspektivischen Projektionsmatrix.                                                    |
| [**XMMatrixPerspectiveRH**](https://msdn.microsoft.com/library/windows/desktop/ee419984)                     | Erstellt eine rechtshändige perspektivische Projektionsmatrix.                                                                        |

 

## Häufig gestellte Fragen zur Portierung von OpenGL ES 2.0 zu Direct3D 11


-   Frage: Kann ich generell nach bestimmten Zeichenfolgen oder Mustern in meinem OpenGL-Code suchen und sie durch die Direct3D-Entsprechungen ersetzen?
-   Antwort: Nein. OpenGL ES 2.0 und Direct3D 11 stammen aus verschiedenen Generationen der Grafikpipelinemodellierung. Obwohl es Ähnlichkeiten zwischen den Konzepten und APIs gibt (z. B. der Renderkontext und die Instanziierung von Shadern), sollten Sie sowohl diesen Leitfaden als auch die Direct3D 11-Referenz lesen, um beim Neuerstellen Ihrer Pipeline die beste Wahl treffen zu können. Eine 1:1-Zuordnung ist nicht zu empfehlen. Wenn Sie aber von GLSL zu HLSL portieren, kann das Erstellen eines allgemeinen Satzes von Aliasen für Variablen, systeminterne Elemente und Funktionen von GLSL nicht nur die Portierung vereinfachen, sondern es bietet Ihnen auch die Möglichkeit, mit einem einzigen Satz von Shadercodedateien zu arbeiten.

 

 






<!--HONumber=Mar16_HO1-->


