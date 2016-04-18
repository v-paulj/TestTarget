---
ms.assetid: 23FE28F1-89C5-4A17-A732-A722648F9C5E
title: Asynchrone Programmierung
description: In diesem Thema werden die asynchrone Programmierung auf der Universellen Windows-Plattform (UWP) und ihre Darstellung in C#, Microsoft Visual Basic .NET, Visual C\+\+-Komponentenerweiterungen (C\+\+/CX) und JavaScript erläutert.
---
# Asynchrone Programmierung

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


In diesem Thema werden die asynchrone Programmierung auf der Universellen Windows-Plattform (UWP) und ihre Darstellung in C#, Microsoft Visual Basic .NET, Visual C++-Komponentenerweiterungen (C++/CX) und JavaScript erläutert.

Mit asynchroner Programmierung können Sie die Reaktionsfähigkeit Ihrer App bei der Ausführung von zeitintensiven Vorgängen aufrechterhalten. Zum Beispiel muss eine App, die Inhalte aus dem Internet herunterlädt, eventuell mehrere Sekunden warten, bis die Inhalte übermittelt sind. Wenn Sie die Inhalte mit einer synchronen Methode für den UI-Thread abrufen, ist die App so lange blockiert, bis der Methodenaufruf beendet ist. In diesem Zeitraum reagiert die App nicht auf Benutzerinteraktionen, und da sie nicht zu antworten scheint, ist der Benutzer möglicherweise verärgert. Die asynchrone Programmierung eignet sich hier sehr viel besser, denn die App wird weiterhin ausgeführt und reagiert auch auf die UI, während ein anderer Vorgang noch abgeschlossen wird.

Für Methoden, deren Aufruf möglicherweise recht lange dauert, ist die asynchrone Programmierung in der Universellen Windows-Plattform das Standardverfahren. JavaScript, C#, Visual Basic und C++/CX unterstützen allesamt asynchrone Methoden.

## Asynchrone Programmierung auf der UWP

Viele UWP-Features wie die [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/BR241124)- und [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171)-APIs werden als asynchrone APIs bereitgestellt. Die Namen asynchroner APIs enden üblicherweise auf „Async“, um anzugeben, dass ein Teil der Ausführung erfolgt, nachdem die API aufgerufen wurde.

Bei der Verwendung asynchroner APIs in einer UWP-App (Universelle Windows-Plattform) führt der Code einheitlich nicht blockierende Aufrufe aus. Bei Implementierung dieser asynchronen Muster in Ihren API-Definitionen können Aufrufer den Code auf vorhersagbare Weise nachvollziehen und verwenden.

Nachfolgend finden Sie eine Reihe häufiger Aufgaben, für die der Aufruf asynchroner UWP-APIs erforderlich ist.

-   Anzeigen von Meldungsdialogfeldern

-   Verwenden des Dateisystems (Anzeigen einer Dateiauswahl)

-   Senden und Empfangen von Daten an das/aus dem Internet

-   Verwenden von Sockets, Streams, Konnektivität

-   Verwenden von Terminen, Kontakten, Kalendern

-   Verwenden von Dateitypen (wie etwa Öffnen von PDF-Dateien oder Decodieren von Bild- oder Medienformaten)

-   Interagieren mit einem Gerät oder Dienst

Mit asynchronen UWP-Mustern können Sie möglicherweise die explizite Verwaltung von Threads gänzlich vermeiden. Jede Programmiersprache unterstützt das asynchrone Muster für die UWP auf spezifische Art und Weise:

| Programmiersprache | Asynchrone Darstellung           |
|----------------------|---------------------------------------|
| C#                  | **async**-Schlüsselwort, **await**-Operator |
| Visual Basic         | **Async**-Schlüsselwort, **Await**-Operator |
| C++/CX               | **task**-Klasse, **.then**-Methode      |
| JavaScript           | zugesagtes Objekt, **then**-Funktion     |

 

## Asynchrone Muster auf der UWP mit C# und Visual Basic


Ein typischer Code-Abschnitt in C# oder Visual Basic wird synchron ausgeführt. Das heißt, dass die Ausführung einer Zeile beendet wird, bevor die nächste Zeile ausgeführt wird. Es gab bereits Microsoft .NET-Programmierungsmodelle für eine asynchrone Ausführung, aber der entsprechende Code überbetont die Funktionsweise des asynchronen Codes als solcher gegenüber der Aufgabe, die mit dem Code ausgeführt werden soll. Die Compiler für UWP, .NET Framework, C# und Visual Basic verfügen über erweiterte Features, die die asynchrone Funktionsweise aus dem Code abstrahieren. Für .NET und die UWP können Sie somit asynchronen Code schreiben, der sich auf die Aufgaben und nicht auf die Ausführung als solche konzentriert. Ihr asynchroner Code wird sich nicht großartig von synchronem Code unterscheiden. Weitere Informationen finden Sie unter [Aufrufen asynchroner APIs in C# oder Visual Basic](call-asynchronous-apis-in-csharp-or-visual-basic.md).

## Asynchrone Muster auf der UWP mit C++


In C++/CX basiert die asynchrone Programmierung auf der [**task-Klasse**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh750113.aspx) und deren [**then-Methode**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh750044.aspx). Die Syntax ist ähnlich aufgebaut wie eine JavaScript-Zusage. Die **task-Klasse** und die zugehörigen Typen erlauben es außerdem, den Threadkontext abzubrechen und zu verwalten. Weitere Informationen finden Sie unter [Asynchrone Programmierung in C++](asynchronous-programming-in-cpp-universal-windows-platform-apps.md).

Die [**create\_async function**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh750102.aspx) unterstützt die Erstellung asynchroner APIs, die über JavaScript oder eine andere Sprache mit Unterstützung für UWP verwendet werden können. Weitere Informationen finden Sie unter [Erstellen asynchroner Vorgänge in C++](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh750082.aspx).

## Asynchrone Muster in UWP mit JavaScript

In JavaScript basiert die asynchrone Programmierung auf dem vorgeschlagenen [Common JS Promises/A](http://wiki.commonjs.org/wiki/Promises/A)-Standard. Dabei werden von asynchronen Methoden zugesagte Objekte zurückgegeben. Zusagen werden sowohl auf der UWP als auch in der Windows-Bibliothek für JavaScript verwendet.

Ein zugesagtes Objekt entspricht einem Wert, der in der Zukunft erfüllt wird. Auf der UWP werden zugesagte Objekte über Factoryfunktionen abgerufen. Die Namen solcher Funktionen enden üblicherweise auf „Async“.

Asynchrone Funktionen können häufig genauso einfach wie konventionelle Funktionen aufgerufen werden. Anders ist nur, dass Sie die [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728)- oder [**done**](https://msdn.microsoft.com/library/windows/apps/Hh701079)-Methode verwenden, um die Handler für Ergebnisse oder Fehler zuzuweisen und den Vorgang zu starten.

## Verwandte Themen

* [Aufrufen asynchroner APIs in C# oder Visual Basic](call-asynchronous-apis-in-csharp-or-visual-basic.md)
* [Asynchrone Programmierung mit Async und Await (C# und Visual Basic)](http://msdn.microsoft.com/library/hh191443(vs.110).aspx)
* [Szenarien für Reversi-Beispielfeatures: Asynchroner Code](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj712233.aspx#async)



<!--HONumber=Mar16_HO1-->


