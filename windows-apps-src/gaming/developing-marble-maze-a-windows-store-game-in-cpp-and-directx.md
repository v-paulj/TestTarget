---
title: Entwickeln von Marble Maze, einem UWP-Spiel in C++ und DirectX
description: In diesem Abschnitt der Dokumentation wird das Erstellen von UWP-3D-Spielen (Universelle Windows-Plattform) mit DirectX und Visual C++ beschrieben.
ms.assetid: 43f1977a-7e1d-614c-696e-7669dd8a9cc7
---

# Entwickeln von Marble Maze, einem UWP-Spiel in C++ und DirectX


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


In diesem Abschnitt der Dokumentation wird das Erstellen von UWP-3D-Spielen (Universelle Windows-Plattform) mit DirectX und Visual C++ beschrieben. Die vorliegende Dokumentation veranschaulicht, wie Sie das 3D-Spiel Marble Maze erstellen, das auf neuen Formfaktoren wie Tablet PCs genau wie auf herkömmlichen Desktopcomputern und Laptops gespielt werden kann.

> **Hinweis**   Den Quellcode für Marble Maze können Sie auf der Webseite für das [DirectX-Spielebeispiel Marble Maze](http://go.microsoft.com/fwlink/?LinkId=624011) herunterladen.

 

> **Wichtig**  Aus Marble Maze werden Entwurfsmuster ersichtlich, die als bewährte Methoden zum Erstellen von UWP-Spielen empfohlen werden. Viele der Implementierungsdetails können Sie an Ihre eigenen Vorgehensweisen und die konkreten Anforderungen des von Ihnen entwickelten Spiels anpassen. Wenn es Ihren Anforderungen besser entgegenkommt, können Sie natürlich auch andere Verfahren oder Bibliotheken verwenden. (Achten Sie jedoch stets darauf, dass Ihr Code die Anforderungen des Zertifizierungskits für Windows-Apps erfüllt.) Wenn eine Marble Maze-Implementierung als unerlässlich für die erfolgreiche Spieleentwicklung erachtet wird, ist dies in der vorliegenden Dokumentation entsprechend gekennzeichnet.

 

## Einführung in Marble Maze


Wir haben uns für Marble Maze entschieden, da es sich um ein relativ einfaches Spiel handelt, das dennoch die ganze Breite der Features repräsentiert, die sich in den meisten Spielen finden. Die Verwendung von Grafiken kann ebenso erläutert werden wie die Verarbeitung von Eingaben und Audiodaten. Außerdem kann die Spielmechanik gezeigt werden, etwa Regeln und Ziele.

Marble Maze ähnelt den Labyrinth-Tischspielen, die meist in einem Kasten mit Löchern und einer Stahl- oder Glasmurmel gespielt werden. Das Ziel von Marble Maze ist das gleiche wie bei der Tischversion: Das Labyrinth muss so angekippt werden, dass die Murmel in möglichst kurzer Zeit vom Anfang zum Ende des Labyrinths gerollt wird, ohne dass sie in eines der Löcher fällt. Außerdem gibt es in Marble Maze das Konzept der Kontrollpunkte. Wenn die Murmel in ein Loch fällt, beginnt das Spiel am letzten Kontrollpunkt neu, den die Murmel passiert hat.

Marble Maze bietet dem Benutzer mehrere Möglichkeiten zur Interaktion mit dem Spielbrett. Wenn Sie über ein Gerät verfügen, das Toucheingaben oder einen Beschleunigungsmesser unterstützt, können Sie das Spielbrett mit diesen Geräten bewegen. Außerdem können Sie das Spiel mit einem Xbox 360-Controller oder einer Maus steuern.

![Screenshot des Marble Maze-Spiels](images/marblemaze.png)

## Voraussetzungen


-   Windows 10
-   Microsoft Visual Studio 2015
-   Kenntnisse in der C++-Programmierung
-   Erfahrung mit DirectX und der Terminologie von DirectX
-   Grundkenntnisse in COM

## Wer sollte diese Dokumentation lesen?


Wenn Sie sich für die Erstellung von 3D-Spielen oder anderen grafikintensiven Anwendungen für Windows 10 interessieren, ist diese Dokumentation für Sie gedacht. Wir hoffen, dass Ihnen die in dieser Dokumentation skizzierten Prinzipien und praktischen Vorgehensweisen dabei helfen, eigene UWP-Spiele zu erstellen. Den größten Nutzen aus dieser Dokumentation werden Sie ziehen, wenn Sie bereits über Erfahrungen mit C++ und DirectX verfügen oder sich sehr dafür interessieren. Wenn Sie noch keine Erfahrung mit DirectX, dafür aber mit ähnlichen 3D-Grafikprogrammierumgebungen haben, wird Ihnen die Dokumentation ebenfalls nutzen.

Im Dokument [Exemplarische Vorgehensweise: Erstellen eines einfachen UWP-Spiels mit DirectX](tutorial--create-your-first-metro-style-directx-game.md) wird ein weiteres Beispiel beschrieben, in dem ein einfacher 3D-Shooter mit DirectX und C++ implementiert wird.

## In dieser Dokumentation behandelte Themen


In dieser Dokumentation wird Folgendes beschrieben:

-   Erstellen eines UWP-Spiels mit der Windows-Runtime-API und DirectX
-   Arbeiten mit visuellen [Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476080)- und [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990)-Inhalten wie Modellen, Texturen, Vertex- und Pixelshadern sowie 2D-Überblendungen
-   Integrieren von Eingabemechanismen wie Touch, Beschleunigungsmesser und Xbox 360-Controller
-   Integrieren von Musik und Soundeffekten mit [XAudio2](https://msdn.microsoft.com/library/windows/desktop/hh405049)

## In dieser Dokumentation nicht behandelte Themen


Folgende Aspekte der Spieleentwicklung werden in dieser Dokumentation nicht behandelt. Im Anschluss an diese Aspekte folgen zusätzliche Ressourcen, in denen sie behandelt werden.

-   Entwurfsprinzipien von 3D-Spielen
-   Grundlagen der C++- und DirectX-Programmierung
-   Entwerfen von Ressourcen wie Texturen, Modellen oder Audio
-   Beheben von Verhaltens- oder Leistungsproblemen in Spielen
-   Vorbereiten von Spielen für andere Teile der Welt
-   Zertifizieren und Veröffentlichen von Spielen im Windows Store

Für die Zusammenarbeit von Marble Maze mit 3D-Geometrie und für physikalische Berechnungen wie Kollisionen wird zudem die [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833)-Bibliothek verwendet. DirectXMath wird in diesem Abschnitt nicht eingehend beschrieben. Weitere Informationen zu DirectXMath finden Sie im [DirectXMath-Programmierhandbuch](https://msdn.microsoft.com/library/windows/desktop/hh437833). Weitere Einzelheiten zur Verwendung von DirectXMath in Marble Maze finden Sie im Quellcode.

Zwar bietet Marble Maze viele wiederverwendbare Komponenten, doch stellt es kein vollständiges Framework für die Spieleentwicklung dar. Wenn eine Komponente von Marble Maze als in Ihrem Spiel wiederverwendbar erachtet wird, ist dies in der vorliegenden Dokumentation entsprechend gekennzeichnet.

## Nächste Schritte


Es wird empfohlen, mit den Grundlagen des Marble Maze-Beispiels zu beginnen, um sich mit der Struktur von Marble Maze und einigen Richtlinien zu Codierung und Stil im Marble Maze-Quellcode vertraut zu machen. Die folgende Tabelle skizziert die Dokumente in diesem Abschnitt, sodass Sie einen genaueren Überblick über diese gewinnen.

## Verwandte Themen


| Titel                                                                                                                    | Beschreibung                                                                                                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Grundlagen am Beispiel von Marble Maze](marble-maze-sample-fundamentals.md)                                                   | Bietet einen Überblick über die Struktur des Spiels und einige Richtlinien zu Codierung und Stil im Marble Maze-Quellcode.                                                                                                                                 |
| [Anwendungsstruktur von Marble Maze](marble-maze-application-structure.md)                                               | Beschreibt die Strukturierung der Anwendung Marble Maze und die Unterschiede zwischen der Struktur einer UWP-DirectX-App und der einer herkömmlichen Desktopanwendung.                                                                                    |
| [Hinzufügen von visuellem Inhalt zum Marble Maze-Beispiel](adding-visual-content-to-the-marble-maze-sample.md)                   | Beschreibt einige der zentralen praktischen Verfahren, die bei der Arbeit mit Direct3D und Direct2D zu beachten sind. Beschreibt außerdem, wie diese Verfahren in Marble Maze für visuelle Inhalte umgesetzt wurden.                                                                           |
| [Hinzufügen von Eingaben und Interaktivität zum Marble Maze-Beispiel](adding-input-and-interactivity-to-the-marble-maze-sample.md) | Beschreibt die Zusammenarbeit von Marble Maze mit Beschleunigungsmesser, Touchgeräten und Xbox 360-Controllern, um den Benutzern das Navigieren in den Menüs und die Interaktion mit dem Spielbrett zu ermöglichen. Beschreibt außerdem einige bewährte Methoden, die bei der Arbeit mit Eingaben beachtet werden sollten. |
| [Hinzufügen von Audiodaten zum Marble Maze-Beispiel](adding-audio-to-the-marble-maze-sample.md)                                     | Beschreibt die Zusammenarbeit von Marble Maze mit Audio, um der Spielumgebung Musik und Soundeffekte hinzuzufügen.                                                                                                                                                  |

 

 

 






<!--HONumber=Mar16_HO1-->


