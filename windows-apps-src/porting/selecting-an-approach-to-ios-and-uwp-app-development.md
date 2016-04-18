---
description: Welche Optionen gibt es beim Entwickeln von plattformübergreifenden Apps?
title: Auswählen eines Ansatzes für die Entwicklung von iOS- und UWP-Apps
ms.assetid: 5CDAB313-07B7-4A32-A49B-026361DCC853
---

# Auswählen eines Ansatzes für die Entwicklung von iOS- und UWP-Apps

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Welche Optionen gibt es beim Entwickeln von plattformübergreifenden Apps?

## Wie werden iOS und Windows am besten unterstützt?

Windows und iOS sind anscheinend sehr unterschiedlich, es stehen jedoch immer mehr Tools und Methoden zur Verfügung, mit denen Sie Apps entwickeln können, die beide Plattformen (sowie Android) unterstützen. Die beste Lösung hängt vom Typ der App ab, die Sie entwickeln. Ausschlaggebend ist zudem auch, ob Sie die App von Grund auf neu erstellen oder ein vorhandenes Projekt portieren.

## Schreiben einer neuen App

Wenn Sie eine neue App erstellen, stehen zahlreiche Optionen zur Verfügung, u. a.:

-   [Xamarin](http://go.microsoft.com/fwlink/p/?LinkID=320484)

    Mit Xamarin können Sie Ihre App in C# schreiben, sie unter Windows ausführen und zudem systemeigene iOS-Apps entwickeln. Unterstützung für Xamarin ist in Visual Studio integriert. Wählen Sie einfach den richtigen Projekttyp aus.

-   [Apache Cordova](http://go.microsoft.com/fwlink/p/?LinkID=400439)

    Falls Sie Javascript und HTML bevorzugen, unterstützt Sie Apache Cordova (auch als PhoneGap bezeichnet) beim Entwickeln plattformübergreifender Apps für iOS, Windows und Android. Dieser Projekttyp ist ebenfalls in Visual Studio integriert.

-   Spielengines

    Mit Tools wie [Unity3D](http://go.microsoft.com/fwlink/p/?LinkID=320479) und [Unreal Engine](http://go.microsoft.com/fwlink/p/?LinkID=394062) können Sie Spiele in AAA-Qualität für Windows und zahlreiche andere Plattformen, einschließlich iOS, programmieren. Unity unterstützt C#-Skripting, Unreal verwendet C++.

-   [MonoGame](http://go.microsoft.com/fwlink/p/?LinkID=320483)

    Der geistige Nachfolger von XNA. Nun handelt es sich dabei um ein plattformübergreifendes Open Source-Framework. Das bedeutet, Sie können Apps in C# für zahlreiche Plattformen mit Unterstützung für Physik-Engines sowie 2D- und 3D-Grafiken schreiben.

## Anpassen einer vorhandenen App

Bei einer vorhandenen iOS-App stehen weniger Optionen zur Verfügung. Es ist jedoch nicht alles verloren.

-   [Windows-Brücke für iOS](https://go.microsoft.com/fwlink/p/?LinkId=619014)

    Dieses noch in der Entwicklung befindliche Tool ist auch unter dem Namen „Projekt Islandwood“ bekannt. Mit ihm können Xcode-Projekte direkt in Visual Studio importiert werden. Die Erstellung und das Debugging von Objective-C-Code kann in Visual Studio ausgeführt werden. Falls Ihr Projekt Bibliotheken verwendet, beispielsweise Cocos für Grafiken, ist dies möglicherweise eine praktische Möglichkeit zum schnellen Portieren Ihrer App.

-   Sie können Ihren C++-Code wiederverwenden.

    Wenn die Kerngeschäftslogik nicht in Objective-C oder Swift, sondern in C++ geschrieben ist, können Sie diesen Code häufig mit geringfügigen Änderungen in Ihrem Projekt verwenden. Anschließend können Sie wie bei anderen Windows-Apps mithilfe von XAML die Benutzeroberfläche definieren und den C++-Code bei Bedarf nutzen.

-   [Verwenden von ANGLE zum Ausführen von OpenGL ES unter Windows](http://go.microsoft.com/fwlink/p/?linkid=618387)

    Ein Zwischenschritt zum Portieren Ihres OpenGL ES 2.0-Projekts ist die Verwendung von ANGLE. Mit ANGLE können Sie OpenGL ES-Inhalte unter Windows ausführen, indem Sie OpenGL ES-API-Aufrufe in DirectX 11-API-Aufrufe übersetzen.

## Andere plattformübergreifende Erstellungstools

-   [GameSalad](http://go.microsoft.com/fwlink/p/?LinkID=320480)

    Eine Spielerstellungsumgebung

-   [Construct 2]( http://go.microsoft.com/fwlink/p/?LinkID=320481)

    Eine Spielerstellungsumgebung

-   [Titanium Studio](http://go.microsoft.com/fwlink/p/?LinkID=320482)

    Eine plattformübergreifende Erstellungsumgebung

-   [Cocos2D-x](http://go.microsoft.com/fwlink/p/?LinkID=320485)

    Eine plattformübergreifende Codebibliothek zur Spritebehandlung und Physikmodellierung

-   [Impact.js](http://go.microsoft.com/fwlink/p/?LinkID=320486)

    Eine HTML-basierte Spielbibliothek.

-   [Marmalade](http://go.microsoft.com/fwlink/p/?LinkID=320487)

    Ein plattformübergreifendes SDK

-   [OpenFL](http://go.microsoft.com/fwlink/p/?LinkID=320488)

    Ein plattformübergreifendes Entwicklungstool

-   [GameMaker](http://go.microsoft.com/fwlink/p/?LinkID=320490)

    Eine spezielle Entwicklungsumgebung für Spiele

-   [PlayCanvas](http://go.microsoft.com/fwlink/p/?LinkID=394061)

    Ein HTML-basiertes Tool für die Spielentwicklung



<!--HONumber=Mar16_HO1-->


