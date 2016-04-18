---
title: Das App-Objekt und DirectX
description: Für die Universelle Windows-Plattform (UWP) mit DirectX-Spielen werden nur wenige der UI-Elemente und -objekte der Windows-Benutzeroberfläche genutzt.
ms.assetid: 46f92156-29f8-d65e-2587-7ba1de5b48a6
---

# Das App-Objekt und DirectX


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Für die Universelle Windows-Plattform (UWP) mit DirectX-Spielen werden nur wenige der UI-Elemente und -objekte der Windows-Benutzeroberfläche genutzt. Da sie auf einer niedrigeren Ebene des Windows-Runtime-Stapels ausgeführt werden, müssen sie stattdessen auf eine grundlegendere Weise mit dem Benutzeroberflächenframework interagieren, und zwar indem sie direkt auf das App-Objekt zugreifen und mit diesem interagieren. Im Folgenden erfahren Sie, zu welchem Zeitpunkt und auf welche Weise eine solche Interaktion erfolgt und wie Sie dieses Modell als DirectX-Entwickler beim Entwickeln von UWP-Apps effizient nutzen können.

## Die wichtigen Benutzeroberflächen-Hauptnamespaces


Zunächst sind die Windows-Runtime-Namespaces zu erwähnen, die Sie (mit **using**) in Ihre UWP-App einschließen müssen. Darauf gehen wir unten noch ausführlicher ein.

-   [**Windows.ApplicationModel.Core**](https://msdn.microsoft.com/library/windows/apps/br205865)
-   [**Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766)
-   [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)
-   [**Windows.System**](https://msdn.microsoft.com/library/windows/apps/br241814)
-   [**Windows.Foundation**](https://msdn.microsoft.com/library/windows/apps/br226021)

> **Hinweis**   Wenn Sie keine UWP-App entwickeln, verwenden Sie die Benutzeroberflächenkomponenten in den JavaScript- oder XAML-spezifischen Bibliotheken und -Namespaces anstelle der Typen in diesen Namespaces.

 

## Das Windows-Runtime-App-Objekt


Sie möchten in Ihrer UWP-App ein Fenster und einen Ansichtsanbieter abrufen, von dem Sie wiederum eine Ansicht abrufen und mit dem Sie Ihre Swapchain (Ihre Anzeigepuffer) verbinden können. Sie können diese Ansicht auch in die fensterspezifischen Ereignisse für die ausgeführte App einbinden. Zum Abrufen des übergeordneten Fensters für das App-Objekt, das durch den [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Typ definiert ist, erstellen Sie einen Typ, der [**IFrameworkViewSource**](https://msdn.microsoft.com/library/windows/apps/hh700482) implementiert, wie im vorherigen Codeausschnitt veranschaulicht.

Im Folgenden werden die grundlegenden Schritte zum Abrufen eines Fensters mit dem zentralen Benutzeroberflächenframework erläutert:

1.  Erstellen Sie einen Typ, der [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) implementiert. Dies ist Ihre Ansicht.

    Definieren Sie in diesem Typ Folgendes:

    -   Eine [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495)-Methode, die eine Instanz von [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) als Parameter annimmt. Sie können eine Instanz dieses Typs abrufen, indem Sie [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278) aufrufen. Das App-Objekt ruft die Methode beim Starten der App auf.
    -   Eine [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509)-Methode, die eine Instanz von [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) als Parameter annimmt. Sie können eine Instanz dieses Typs abrufen, indem Sie auf die [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019)-Eigenschaft in der neuen [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017)-Instanz zugreifen.
    -   Eine [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501)-Methode, die eine Zeichenfolge für einen Einstiegspunkt als einzigen Parameter annimmt. Das App-Objekt stellt den Einstiegspunkt bereit, wenn Sie diese Methode aufrufen. Hier richten Sie Ressourcen ein. Hier erstellen Sie Ihre Geräteressourcen. Das App-Objekt ruft die Methode beim Starten der App auf.
    -   Eine [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505)-Methode, die das [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Objekt aktiviert und den Fensterereignisverteiler startet. Der Aufruf durch das App-Objekt erfolgt, wenn der Prozess der App gestartet wird.
    -   Eine [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523)-Methode, die die im Aufruf von [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501) eingerichteten Ressourcen bereinigt. Das App-Objekt ruft diese Methode beim Schließen der App auf.

2.  Erstellen Sie einen Typ, der [**IFrameworkViewSource**](https://msdn.microsoft.com/library/windows/apps/hh700482) implementiert. Dies ist Ihr Ansichtsanbieter.

    Definieren Sie in diesem Typ Folgendes:

    -   Eine Methode mit dem Namen [**CreateView**](https://msdn.microsoft.com/library/windows/apps/hh700491), die eine Instanz Ihrer [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478)-Implementierung zurückgibt, die in Schritt 1 erstellt wurde.

3.  Übergeben Sie eine Instanz des Ansichtsanbieters an [**CoreApplication.Run**](https://msdn.microsoft.com/library/windows/apps/hh700469) aus **main**.

Da Sie sich nun mit den Grundlagen vertraut gemacht haben, betrachten Sie nun weitere verfügbare Optionen, mit denen Sie diesen Ansatz ausweiten können.

## Zentrale Benutzeroberflächentypen


Im Folgenden werden weitere zentrale Benutzeroberflächentypen in der Windows-Runtime vorgestellt, die möglicherweise hilfreich sein können:

-   [**Windows.ApplicationModel.Core.CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017)
-   [**Windows.UI.Core.CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)
-   [**Windows.UI.Core.CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)

Mit diesen Typen können Sie auf die Ansicht der App zugreifen, insbesondere auf die Bits, mit denen die Inhalte des übergeordneten Fensters der App gezeichnet werden, und Sie können die für dieses Fenster ausgelösten Ereignisse behandeln. Der Prozess des App-Fensters befindet sich in einem *Anwendungs-Single-Threaded-Apartment* (ASTA), das isoliert ist und alle Rückrufe behandelt.

Die Ansicht Ihrer App wird durch den Ansichtsanbieter für das App-Fenster generiert und in den meisten Fällen durch ein spezifisches Frameworkpaket oder direkt vom System implementiert, sodass Sie die Implementierung nicht selbst ausführen müssen. Für DirectX müssen Sie wie zuvor beschrieben einen kompakten Ansichtsanbieter implementieren. Zwischen den folgenden Komponenten und Verhalten besteht eine spezifische 1:1-Beziehung:

-   Die Ansicht einer App, die durch den [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017)-Typ dargestellt wird und die die Methode(n) zum Aktualisieren des Fensters definiert.
-   Ein ASTA, dessen Attribute das Threadingverhalten der App definieren. Sie können keine Instanzen von STA-attributierten COM-Typen für ein ASTA erstellen.
-   Ein Ansichtsanbieter, den Ihre App vom System erhält oder der von Ihnen implementiert wird.
-   Ein übergeordnetes Fenster, das durch den [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Typ dargestellt wird.
-   Quellentnahme für alle Aktivierungsereignisse. Sowohl Ansichten als auch Fenster verfügen über separate Aktivierungsereignisse.

Dies lässt sich wie folgt zusammenfassen: Das App-Objekt stellt eine Ansichtsanbieterfactory bereit. Es erstellt einen Ansichtsanbieter und instanziiert ein übergeordnetes Fenster für die App. Der Ansichtsanbieter definiert die Ansicht der App für das übergeordnete Fenster der App. Im Folgenden wird genauer auf die Ansicht und das übergeordnete Fenster eingegangen.

## CoreApplicationView – Verhalten und Eigenschaften


[**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) stellt die aktuelle App-Ansicht dar. Das App-Singleton erstellt die App-Ansicht während der Initialisierung; die Ansicht bleibt jedoch so lange ruhend, bis sie aktiviert wird. Sie können das [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) abrufen, in dem die Ansicht angezeigt wird, indem Sie auf die zugehörige [**CoreApplicationView.CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019)-Eigenschaft zugreifen: Sie können Aktivierungs- und Deaktivierungsereignisse für die Ansicht behandeln, indem Sie Delegaten beim [**CoreApplicationView.Activated**](https://msdn.microsoft.com/library/windows/apps/br225018)-Ereignis registrieren.

## CoreWindow – Verhalten und Eigenschaften


Das übergeordnete Fenster, das eine [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Instanz darstellt, wird erstellt und an den Ansichtsanbieter übergeben, wenn das App-Objekt initialisiert wird. Wenn die App über ein anzuzeigendes Fenster verfügt, wird dieses angezeigt. Andernfalls wird lediglich die Ansicht initialisiert.

[**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) stellt eine Reihe von Ereignissen bereit, die spezifisch für das Eingabeverhalten und das grundlegende Fensterverhalten sind. Sie können diese Ereignisse behandeln, indem Sie eigene Delegaten für diese registrieren.

Sie können auch den Fensterereignisverteiler für das Fenster abrufen, indem Sie auf die [**CoreWindow.Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br208264)-Eigenschaft zugreifen, die eine Instanz von [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) bereitstellt.

## CoreDispatcher – Verhalten und Eigenschaften


Sie können das Threadingverhalten der Ereignisverteilung für ein Fenster mit dem [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)-Typ bestimmen. Bei diesem Typ gibt es eine besonders wichtige Methode: die [**CoreDispatcher.ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215)-Methode, die die Fensterereignisverarbeitung startet. Wenn Sie diese Methode mit der falschen Option für Ihre App aufrufen, kann dies zu allen möglichen unerwarteten Verhaltensweisen bei der Ereignisverarbeitung führen.

| CoreProcessEventsOption-Option                                                           | Beschreibung                                                                                                                                                                                                                                  |
|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [**CoreProcessEventsOption.ProcessOneAndAllPending**](https://msdn.microsoft.com/library/windows/apps/br208217) | Verteilen Sie alle derzeit in der Warteschlange verfügbaren Ereignisse. Wenn keine Ereignisse ausstehen, warten Sie auf das nächste neue Ereignis.                                                                                                                                 |
| [**CoreProcessEventsOption.ProcessOneIfPresent**](https://msdn.microsoft.com/library/windows/apps/br208217)     | Verteilen Sie ein Ereignis, wenn es in der Warteschlange aussteht. Wenn keine Ereignisse ausstehen, warten Sie nicht, dass ein neues Ereignis ausgelöst wird, sondern kehren Sie stattdessen sofort zurück.                                                                                          |
| [**CoreProcessEventsOption.ProcessUntilQuit**](https://msdn.microsoft.com/library/windows/apps/br208217)        | Warten Sie auf neue Ereignisse, und verteilen Sie alle verfügbaren Ereignisse. Setzen Sie dieses Verhalten fort, bis das Fenster geschlossen wird oder die Anwendung die [**Close**](https://msdn.microsoft.com/library/windows/apps/br208260)-Methode für die [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Instanz aufruft. |
| [**CoreProcessEventsOption.ProcessAllIfPresent**](https://msdn.microsoft.com/library/windows/apps/br208217)     | Verteilen Sie alle derzeit in der Warteschlange verfügbaren Ereignisse. Wenn keine Ereignisse ausstehen, kehren Sie sofort zurück.                                                                                                                                          |

 

UWP mit DirectX muss die [**CoreProcessEventsOption.ProcessAllIfPresent**](https://msdn.microsoft.com/library/windows/apps/br208217)-Option verwenden, um blockierendes Verhalten zu verhindern, das Grafikaktualisierungen beeinträchtigen könnte.

## Überlegungen zu ASTA für DirectX-Entwickler


Das App-Objekt, das die Laufzeitdarstellung Ihrer UWP- und DirectX-App definiert, verwendet das ASTA-Threadingmodell (Anwendungs-Single-Threaded-Apartment), um die UI-Ansichten der App zu hosten. Wenn Sie eine UWP- und DirectX-App entwickeln, sind Sie mit den Eigenschaften eines ASTAs vertraut, da alle Threads, die von Ihrer UWP- und DirectX-App gesendet werden, die [**Windows::System::Threading**](https://msdn.microsoft.com/library/windows/apps/br229642)-APIs oder [**CoreWindow::CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) verwenden müssen. (Sie können das [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Objekt für das ASTA abrufen, indem Sie [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589) in Ihrer App aufrufen.)

Vor allem sollten Sie als Entwickler einer UWP DirectX-App darauf achten, dass Ihr App-Thread MTA-Threads senden kann. Legen Sie dazu **Platform::MTAThread** auf **main()** fest.

```cpp
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto myDXAppSource = ref new MyDXAppSource(); // your view provider factory 
    CoreApplication::Run(myDXAppSource);
    return 0;
}
```

Wenn das App-Objekt für Ihre UWP DirectX-App aktiviert wird, erstellt es ein ASTA, das für die UI-Ansicht verwendet wird. Der neue ASTA-Thread ruft die Factory für Ihren Ansichtsanbieter auf, um den Ansichtsanbieter für Ihr App-Objekt zu erstellen. Der Code für Ihren Ansichtsanbieter wird dann in dem ASTA-Thread ausgeführt.

Alle Threads, die aus einem ASTA ausgegliedert werden, müssen sich außerdem in einem MTA befinden. Beachten Sie, dass es durch ausgelagerte MTA-Threads weiterhin zu Eintrittsinvarianzen und somit zu Sperren kommen kann.

Wenn Sie vorhandenen Code zur Ausführung in einem ASTA-Thread portieren, sollten Sie folgende Punkte berücksichtigen:

-   Das Verhalten von Wartegrundtypen wie [**CoWaitForMultipleObjects**](https://msdn.microsoft.com/library/windows/desktop/hh404144) unterscheidet sich je nachdem, ob es sich um einen ASTA oder einen STA handelt.
-   Die gebundene Schleife eines COM-Aufrufs wird in einem ASTA anders ausgeführt. Solange ein ausgehender Aufruf aktiv ist, können Sie keine Aufrufe ohne entsprechenden Bezug mehr empfangen. Beispielsweise führt das folgende Verhalten zu einer ASTA-Sperre (und zum unmittelbaren Absturz der App):
    1.  Das ASTA ruft ein MTA-Objekt auf und übergibt den Schnittstellenzeiger P1.
    2.  Dasselbe MTA-Objekt wird später vom ASTA aufgerufen. Das MTA-Objekt ruft P1 vor der Rückgabe an das ASTA auf.
    3.  Der Schnittstellenzeiger P1 kann aufgrund der Blockierung durch einen Aufruf ohne Bezug nicht auf das ASTA zugreifen. Der MTA-Thread wird jedoch bei dem Versuch, P1 aufzurufen, blockiert.

    Sie können dieses Problem wie folgt beheben:
    -   Verwenden Sie das **async**-Muster, das in der Parallel Patterns Library (PPLTasks.h) definiert wurde.
    -   Rufen Sie [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) im ASTA Ihrer App (dem Hauptthread Ihrer App) so früh wie möglich auf, um zufällige Aufrufe zuzulassen.

    Sie können sich daher nicht auf die unmittelbare Übermittlung von Aufrufen ohne Bezug zum ASTA Ihrer App verlassen. Weitere Informationen über asynchrone Aufrufe finden Sie unter [Asynchrone Programmierung in C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

Im Allgemeinen sollten Sie bei der Entwicklung Ihrer UWP-App den [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) für [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) und [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) Ihrer App verwenden, um alle UI-Threads zu behandeln, anstatt eigene MTA-Threads zu erstellen und zu verwalten. Wenn Sie einen separaten Thread benötigen, der nicht mit dem **CoreDispatcher** behandelt werden kann, verwenden Sie asynchrone Muster, und beachten Sie die bereits erwähnten Ratschläge zur Vermeidung von Problemen mit Einstiegsinvarianzen.

 

 






<!--HONumber=Mar16_HO1-->


