---
author: mtoepke
title: Grundlagen am Beispiel von Marble Maze
description: In diesem Dokument werden die fundamentalen Eigenschaften des Marble Maze-Projekts beschrieben, beispielsweise wie Visual C++ in der Windows Runtime-Umgebung verwendet wird, wie es erstellt und strukturiert wird und wie es aufgebaut ist.
ms.assetid: 73329b29-62e3-1b36-01db-b7744ee5b4c3
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c9aafbf7d8061893180a1a823c2c1cafd9ef7a7f

---

# Grundlagen am Beispiel von Marble Maze


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


In diesem Dokument werden die fundamentalen Eigenschaften des Marble Maze-Projekts beschrieben, beispielsweise wie Visual C++ in der Windows Runtime-Umgebung verwendet wird, wie es erstellt und strukturiert wird und wie es aufgebaut ist. Das Dokument enthält auch eine Beschreibung verschiedener Konventionen, die im Code verwendet werden.

> **Hinweis**   Den Beispielcode für dieses Dokument finden Sie im [DirectX-Beispielspiel Marble Maze](http://go.microsoft.com/fwlink/?LinkId=624011).

 
## 
Es folgen einige wichtige Punkte, die in diesem Dokument erläutert werden und die beim Planen und Entwickeln Ihres UWP-Spiels relevant sind.

-   Verwenden Sie die Vorlage **DirectX 11-App (Universelle Windows-App)** in einer C++-Anwendung, um das DirectX-UWP-Spiel zu erstellen. Verwenden Sie Visual Studio, um ein UWP-App-Projekt wie ein Standardprojekt zu erstellen.
-   Windows-Runtime stellt Klassen und Schnittstellen bereit, sodass Sie UWP-Apps auf moderne, objektorientierte Art und Weise entwickeln können.
-   Verwenden Sie Objektreferenzen mit Hütchensymbol (^), um die Lebensdauer von Windows-Runtime-Variablen zu verwalten, [**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx) zum Verwalten der Lebensdauer von COM-Objekten und [**std::shared\_ptr**](https://msdn.microsoft.com/library/windows/apps/bb982026.aspx) oder [**std::unique\_ptr**](https://msdn.microsoft.com/library/windows/apps/ee410601.aspx) zum Verwalten der Lebensdauer aller anderen, vom Heap zugewiesenen C++-Objekte.
-   In den meisten Fällen verwenden Sie für die Behandlung unerwarteter Fehler die Ausnahmebehandlung anstelle von Ergebniscodes.
-   Verwenden Sie SAL-Anmerkungen in Kombination mit Codeanalysetools, um Fehler in der App zu finden.

## Erstellen des Visual Studio-Projekts


Wenn Sie das Beispiel heruntergeladen und extrahiert haben, können Sie die Projektmappendatei "MarbleMaze.sln" in Visual Studio öffnen, und der gesamte Code liegt offen vor Ihnen. Sie können die Quelle auch auf der MSDN Sample Gallery-Seite mit dem [DirectX-Beispielspiel Marble Maze](http://go.microsoft.com/fwlink/?LinkId=624011) anzeigen, indem Sie die Registerkarte **Code durchsuchen** auswählen.

Beim Erstellen des Visual Studio-Projekts für Marble Maze haben wir mit einem bereits vorhandenen Projekt begonnen. Wenn Sie jedoch noch kein vorhandenes Projekt haben, das die grundlegende, für Ihr DirectX-UWP-Spiel erforderliche Funktionalität bereitstellt, wird empfohlen, ein Projekt basierend auf der Visual Studio-Vorlage **DirectX 11-App (Universelle Windows-App)** zu erstellen, da sie eine grundlegende, funktionierende 3D-Anwendung enthält.

Eine wichtige Projekteinstellung in der Voralge **DirectX 11-App (Universelle Windows-App)** ist die Option **/ZW**. Diese Option ermöglicht dem Programm die Verwendung der Windows-Runtime-Spracherweiterungen. Sie ist standardmäßig aktiviert, wenn Sie die Visual Studio-Vorlage verwenden.

> **Achtung**   Die Option **/ZW** ist nicht kompatibel mit Optionen wie **/clr**. Im Fall von **/clr** bedeutet dies, dass Sie ein Visual C++-Projekt nicht gleichzeitig auf das .NET Framework und die Windows-Runtime ausrichten können.

 

Jede UWP-App, die Sie aus dem Windows Store erwerben, wird in Form eines App-Pakets bereitgestellt. Ein App-Paket enthält ein App-Manifest, das wiederum Informationen zur App beinhaltet. Sie können beispielsweise die Funktionen (d.h. den erforderlichen Zugriff auf geschützte Systemressourcen oder Benutzerdaten) Ihrer App angeben. Wenn Sie festlegen, dass für Ihre App bestimmte Funktionen erforderlich sind, verwenden Sie das Paketmanifest, um die erforderlichen Funktionen zu deklarieren. Das Manifest ermöglicht Ihnen auch die Angabe von Projekteigenschaften wie der Rotation unterstützter Geräte, Kachelbildern und Begrüßungsbildschirm. Weitere Informationen zu App-Paketen finden Sie unter [Verpacken von Apps](https://msdn.microsoft.com/library/windows/apps/mt270969).

##  Erstellen, Bereitstellen und Ausführen des Spiels


Erstellen Sie ein UWP-App-Projekt analog zu einem Standardprojekt. (Klicken Sie auf der Menüleiste auf **Erstellen, Projektmappe erstellen**.) In diesem Buildschritt wird der Code kompiliert und zur Nutzung als UWP-App gepackt.

Nachdem Sie das Projekt erstellt haben, müssen Sie es bereitstellen. (Klicken Sie auf der Menüleiste auf **Erstellen, Projektmappe bereitstellen**.) Visual Studio stellt das Projekt auch bereit, wenn Sie das Spiel über den Debugger ausführen.

Wählen Sie im Anschluss an die Bereitstellung des Projekts die Marble Maze-Kachel aus, um das Spiel auszuführen. Alternativ können Sie auf der Menüleiste von Visual Studio auf **Debuggen, Debugging starten** klicken.

###  Steuern des Spiels

Für die Steuerung von Marble Maze können Sie die Fingereingabe, den Beschleunigungsmesser, den Xbox 360-Controller oder die Maus verwenden.

-   Mithilfe des Steuerkreuzes am Controller können Sie das aktive Menüelement ändern.
-   Zum Auswählen eines Menüelements können Sie die A-Taste, die Start-Taste oder die Maus verwenden.
-   Über die Fingereingabe, den Beschleunigungsmesser, den linken Ministick oder die Maus können Sie das Labyrinth neigen.
-   Zum Schließen von Menüs wie der Highscore-Tabelle können Sie die A-Taste, die Start-Taste oder die Maus verwenden.
-   Mithilfe der Start-Taste oder der P-Taste können Sie das Spiel pausieren oder fortsetzen.
-   Zum Neustarten des Spiels können Sie die Zurück-Taste am Controller oder die Pos1-Taste auf der Tastatur verwenden.
-   Wenn die Highscore-Tabelle angezeigt wird, können Sie mit der Zurück- oder der Pos1-Taste alle Punktergebnisse löschen.

##  Codekonventionen


Die Windows-Runtime ist eine Programmierschnittstelle, die Sie zum Erstellen von UWP-Apps verwenden können, die nur in einer speziellen Anwendungsumgebung ausgeführt werden. Solche Apps verwenden autorisierte Funktionen, Datentypen und Geräte, und sie werden über den Windows Store verteilt. Die Windows-Runtime besteht auf der untersten Ebene aus einer binären Anwendungsschnittstelle (Application Binary Interface, ABI). Die ABI ist ein binärer Vertrag auf unterer Ebene, der Windows-Runtime-APIs für mehrere Programmiersprachen zur Verfügung stellt, beispielsweise für JavaScript, die .NET-Sprachen und Visual C++.

Diese Sprachen erfordern für den Aufruf von Windows-Runtime-APIs aus JavaScript und .NET Projektionen, die für die jeweilige Sprachumgebung spezifisch sind. Wenn Sie eine Windows-Runtime-API aus JavaScript oder .NET aufrufen, rufen Sie die Projektion auf, die wiederum die zugrunde liegende ABI-Funktion aufruft. Sie können zwar die ABI-Funktionen direkt aus Standard-C++ aufrufen, jedoch stellt Microsoft auch Projektionen für C++ bereit, da diese die Verwendung der Windows-Runtime-APIs stark vereinfachen und dennoch eine hohe Leistung aufrecht erhalten. Microsoft stellt außerdem Spracherweiterungen für Visual C++ bereit, die spezielle Unterstützung für die Windows-Runtime-Projektionen bieten. Viele dieser Spracherweiterungen ähneln der Syntax für die Sprache C++/CLI. Anstelle einer Zielgruppenadressierung für die Common Language Runtime (CLR) durchzuführen, verwenden systemeigene Apps diese Syntax zum Erreichen der Windows-Runtime. Der Modifizierer in Form einer Objektreferenz, bzw. des Hütchensymbols (^), ist ein wichtiger Teil dieser neuen Syntax, da er die automatische Löschung von Runtime-Objekten anhand einer Referenzzählung ermöglicht. Anstatt Methoden wie **AddRef** und **Release** zum Verwalten der Lebensdauer eines Windows-Runtime-Objekts aufzurufen, löscht die Runtime das Objekt, wenn keine andere Komponente darauf verweist. Dies ist beispielsweise der Fall, wenn es den Bereich verlässt oder wenn Sie alle Verweise auf **nullptr** festlegen. Ein weiterer wichtiger Aspekt beim Erstellen von UWP-Apps ist das **ref new**-Schlüsselwort. Verwenden Sie **ref new** anstelle von **new**, um Windows-Runtime-Objekte mit Verweiszählung zu erstellen. Weitere Informationen finden Sie unter [Typsystem (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822).

> **Wichtig**  
Wenn Sie Windows-Runtime-Objekte oder Komponenten für die Windows-Runtime erstellen, müssen Sie nur **^** und **ref new** verwenden. Sie können die C++-Standardsyntax verwenden, wenn Sie Kernanwendungscode schreiben, in dem die Windows-Runtime nicht genutzt wird.

In Marble Maze werden mit **^** und [**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx) vom Heap zugewiesene Objekte verwaltet und Arbeitsspeicherverluste minimiert. Es wird empfohlen, mit ^ die Lebensdauer von Windows-Runtime-Variablen zu verwalten, mit **ComPtr** die Lebensdauer von COM-Variables zu verwalten (z.B. bei Verwendung von DirectX) und mit std::[**std::shared\_ptr**](https://msdn.microsoft.com/library/windows/apps/bb982026) oder [**std::unique\_ptr**](https://msdn.microsoft.com/library/windows/apps/ee410601) die Lebensdauer sämtlicher vom Heap zugewiesenen C++-Objekte zu verwalten.

 

Weitere Informationen zu den Spracherweiterungen, die für eine C++-UWP-App verfügbar sind, finden Sie unter [Sprachreferenz zu Visual C++ (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh699871).

###  Fehlerbehandlung

In Marble Maze werden unerwartete Fehler in erster Linie mithilfe der Ausnahmebehandlung verarbeitet. Obwohl für Spielecode zur Angabe von Fehlern traditionell die Protokollierung oder Fehlercodes verwendet werden, beispielsweise **HRESULT**-Werte, bietet die Ausnahmebehandlung zwei wesentliche Vorteile. Zunächst kann sie das Lesen und Verwalten von Code erleichtern. Aus Codeperspektive stellt die Ausnahmebehandlung ein effizienteres Verfahren dar, um einen Fehler an eine Routine weiterzugeben, die den Fehler behandeln kann. Bei der Verwendung von Fehlercodes muss i.d.R. jede Funktion explizit Fehler weitergeben. Ein zweiter Vorteil besteht darin, dass Sie den Visual Studio-Debugger so konfigurieren können, dass er beim Auftreten einer Ausnahme unterbrochen wird, damit die Ausführung unmittelbar an der Position und im Kontext des Fehlers angehalten wird. Die Windows-Runtime macht ebenfalls extensiven Gebrauch von der Ausnahmebehandlung. Deshalb können Sie durch die Verwendung von Ausnahmebehandlung im Code die gesamte Fehlerbehandlung in einem einzigen Modell zusammenfassen.

Es wird empfohlen, in Ihrem Fehlerbehandlungsmodell die folgenden Konventionen zu verwenden:

-   Kommunizieren Sie unerwartete Fehler anhand von Ausnahmen.
-   Verwenden Sie Ausnahmen nicht zum Steuern des Codeflusses.
-   Fangen Sie nur die Ausnahmen ab, die Sie auf jeden Fall behandeln und beheben können. Fangen Sie andernfalls die Ausnahme nicht ab, und ermöglichen Sie das Beenden der App.
-   Wenn Sie eine DirectX-Routine aufrufen, die **HRESULT** zurückgibt, verwenden Sie die **DX::ThrowIfFailed**-Funktion. Diese Funktion wird in DirectXSample.h definiert. **ThrowIfFailed** löst eine Ausnahme aus, wenn das bereitgestellte **HRESULT**-Element ein Fehlercode ist. **E\_POINTER** bewirkt beispielsweise, dass **ThrowIfFailed** [**Platform::NullReferenceException**](https://msdn.microsoft.com/library/windows/apps/hh755823.aspx) auslöst.

    Wenn Sie **ThrowIfFailed** verwenden, platzieren Sie den DirectX-Aufruf in einer separaten Zeile, um die Lesbarkeit des Codes zu verbessern. Dies wird im folgenden Beispiel veranschaulicht.

    ```cpp
    // Identify the physical adapter (GPU or card) this device is running on.
    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );
    ```

-   Obwohl empfohlen wird, die Verwendung von **HRESULT** für unerwartete Fehler zu vermeiden, ist es wichtiger, auf die Nutzung der Ausnahmebehandlung zur Steuerung des Codeflusses zu verzichten. Demzufolge wird bevorzugt, bei Bedarf einen **HRESULT**-Rückgabewert zu verwenden, um den Codefluss zu steuern.

###  SAL-Anmerkungen

Verwenden Sie SAL-Anmerkungen in Kombination mit Codeanalysetools, um Fehler in der App zu finden.

Mithilfe der Microsoft-Quellcodeanmerkungssprache (Source Code Annotation Language, SAL) können Sie anmerken bzw. beschreiben, wie eine Funktion die zugehörigen Parameter verwendet. SAL-Anmerkungen werden auch zum Beschreiben von Rückgabewerten verwendet. SAL-Anmerkungen können mit dem C/C++-Codeanalysetool verwendet werden, um mögliche Fehler im C- und C++-Quellcode zu finden. Häufige Codierungsfehler, die vom Tool gemeldet werden, beinhalten Pufferüberläufe, nicht initialisierte Speicher, Nullzeiger-Dereferenzierungen und Speicher- und Ressourcenverluste.

Ziehen Sie die **BasicLoader::LoadMesh**-Methode in Erwägung, die in BasicLoader.h deklariert wird. Diese Methode verwendet \_In\_ für die Angabe, dass *filename* ein Eingabeparameter ist (und folglich nur daraus gelesen wird). Zudem verwendet sie \_Out\_ für die Angabe, dass *vertexBuffer* und *indexBuffer* und Ausgabeparameter sind (und demzufolge in diese Parameter geschrieben wird). Schlussendlich verwendet die Methode \_Out\_opt\_, um anzugeben, dass *vertexCount* und *indexCount* optionale Ausgabeparameter sind (und möglicherweise in diese Parameter geschrieben wird). Da *vertexCount* und *indexCount* optionale Ausgabeparameter sind, dürfen sie **nullptr** sein. Das C/C++-Codeanalysetool untersucht Aufrufe dieser Methode, um sicherzustellen, dass die von ihr übergebenen Parameter diese Kriterien erfüllen.

```cpp
void LoadMesh(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    );
```

Wenn Sie eine Codeanalyse für Ihre App ausführen möchten, wählen Sie auf der Menüleiste die Optionen **Erstellen, Codeanalyse für Lösung ausführen** aus. Weitere Informationen zur Codeanalyse finden Sie unter [Analysieren der C/C++-Codequalität mithilfe der Codeanalyse](https://msdn.microsoft.com/library/windows/apps/ms182025.aspx).

Die vollständige Liste der verfügbaren Anmerkungen wird in sal.h definiert. Weitere Informationen finden Sie unter [SAL-Anmerkungen](https://msdn.microsoft.com/library/windows/apps/ms235402.aspx).

## Nächste Schritte


Lesen Sie die Informationen unter [Marble Maze-Anwendungsstruktur](marble-maze-application-structure.md), um zu erfahren, wie der Marble Maze-Anwendungscode strukturiert ist und wodurch sich die Struktur einer DirectX-UWP-App von der einer herkömmlichen Desktopanwendung unterscheidet.

## Verwandte Themen


* [Marble Maze-Anwendungsstruktur](marble-maze-application-structure.md)
* [Entwickeln von Marble Maze, einem UWP-Spiel in C++ und DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 







<!--HONumber=Aug16_HO3-->


