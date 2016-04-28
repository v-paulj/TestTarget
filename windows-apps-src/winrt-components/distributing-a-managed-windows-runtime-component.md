---
title: Verteilen einer verwalteten Windows-Runtime-Komponente
description: Sie können Ihre Windows-Runtime-Komponente durch Kopieren der Dateien verteilen.
ms.assetid: 80262992-89FC-42FC-8298-5AABF58F8212
---


# Verteilen einer verwalteten Komponente für Windows-Runtime


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


\[Einige Informationen beziehen sich auf die Vorabversion, die vor der kommerziellen Freigabe möglicherweise wesentlichen Änderungen unterliegt. Microsoft übernimmt keine Garantie, weder ausdrücklicher noch impliziter Art, für die hier bereitgestellten Informationen.\]

Sie können Ihre Windows-Runtime-Komponente durch Kopieren der Dateien verteilen. Wenn Ihre Komponente aus vielen Dateien besteht, kann die Installation für Ihre Benutzer aber sehr mühsam sein. Außerdem verursachen Fehler beim Platzieren der Dateien oder beim Festlegen von Verweisen möglicherweise Probleme. Sie können eine komplexe Komponente als Visual Studio-Erweiterungs-SDK packen, um die Installation und Verwendung einfacher zu gestalten. Benutzer müssen nur einen Verweis für das gesamte Paket festlegen. Benutzer können Ihre Komponente mit dem Dialogfeld **Erweiterungen und Updates** problemlos suchen und installieren, wie in der MSDN Library unter [Suchen und Verwenden von Visual Studio-Erweiterungen](https://msdn.microsoft.com/library/vstudio/dd293638.aspx) beschrieben.

## Planen einer verteilbaren Komponente für Windows-Runtime

Wählen Sie eindeutige Namen für Binärdateien, z. B. für Ihre WINMD-Dateien. Wir empfehlen das folgende Format, um die Eindeutigkeit sicherzustellen:

``` syntax
company.product.purpose.extension
For example: Microsoft.Cpp.Build.dll
```

Ihre Binärdateien werden in App-Paketen möglicherweise mit Binärdateien von anderen Entwicklern installiert. Siehe „Erweiterungs-SDKs” unter [Gewusst wie: Erstellen eines Software Development Kit (SDK)](https://msdn.microsoft.com/library/hh768146.aspx) in der MSDN Library.

Die Entscheidung darüber, wie Sie Ihre Komponente verteilen, hängt von Komplexität der Komponente ab. Sie sollten ein Erweiterungs-SDK oder einen ähnlichen Paket-Manager verwenden, wenn:

-   Ihre Komponente aus mehreren Dateien besteht.
-   Sie Versionen Ihrer Komponente für mehrere Plattformen (z. B. x86 und ARM) bereitstellen.
-   Sie Debug- und Releaseversionen Ihrer Komponente bereitstellen.
-   Ihre Komponente über Dateien und Assemblys verfügt, die nur zur Entwurfszeit verwendet werden.

Ein Erweiterungs-SDK ist besonders nützlich, wenn mindestens einer der oben genannten Punkte zutrifft.

> **Hinweis**  Für komplexe Komponenten bietet das NuGet-Paketverwaltungssystem eine Open-Source-Alternative zu Erweiterungs-SDKs. Wie mit Erweiterungs-SDKs können Sie mit NuGet Pakete erstellen, die die Installation komplexer Komponenten vereinfachen. Ein Vergleich von NuGet-Paketen und Visual Studio-Erweiterungs-SDKs finden Sie unter [Hinzufügen von Referenzen mithilfe von NuGet im Vergleich zu einer SDK-Erweiterung](https://msdn.microsoft.com/library/jj161096.aspx) in der MSDN Library.

## Verteilung durch Kopieren von Dateien

Wenn Ihre Komponente aus einer einzigen WINMD-Datei oder einer WINMD-Datei und einer Ressourcenindexdatei (PRI) besteht, können Sie einfach die WINMD-Datei so bereitstellen, dass Benutzer sie kopieren können. Benutzer können die Datei an einer beliebige Position in ein Projekt einfügen, im Dialogfeld **Vorhandenes Element hinzufügen** die WINMD-Datei dem Projekt hinzufügen und dann mit dem Dialogfeld „Verweis-Manager” einen Verweis erstellen. Wenn Sie eine PRI-Datei oder eine XML-Datei einbeziehen, sollten Sie die Benutzer anweisen, diese Dateien in dasselbe Verzeichnis wie die WINMD-Datei zu kopieren.

> **Hinweis**  Visual Studio erzeugt stets eine PRI-Datei, wenn Sie Ihre Windows-Runtime-Komponente erstellen, auch wenn Ihr Projekt keine Ressourcen enthält. Wenn Sie über eine Test-App für Ihre Komponente verfügen, können Sie feststellen, ob die PRI-Datei verwendet wird, indem Sie den Inhalt des App-Pakets im Ordner bin\\debug\\AppX" überprüfen. Wenn die PRI-Datei Ihrer Komponente dort nicht angezeigt wird, müssen Sie sie nicht verteilen. Sie können auch mit dem Tool [MakePRI.exe](https://msdn.microsoft.com/library/windows/apps/jj552945.aspx) die Ressourcendatei aus Ihrem Komponentenprojekt für Windows-Runtime ausgeben. Geben Sie z. B. im Eingabeaufforderungsfenster von Visual Studio Folgendes ein: makepri dump /if MyComponent.pri /of MyComponent.pri.xml. Weitere Informationen zu PRI-Dateien finden Sie unter [Ressourcenverwaltungssystem (Windows)](https://msdn.microsoft.com/library/windows/apps/jj552947.aspx).

## Verteilung durch Erweiterungs-SDK

Eine komplexe Komponente enthält in der Regel Windows-Ressourcen, aber lesen Sie den Hinweis zum Erkennen von leeren PRI-Dateien im vorherigen Abschnitt.

**So erstellen Sie ein Erweiterungs-SDK**

1.  Überprüfen Sie, ob das Visual Studio-SDK installiert ist. Sie können das Visual Studio-SDK von der Seite [Visual Studio-Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) herunterladen.
2.  Erstellen eines neues Projekts mit der VSIX-Projektvorlage Sie finden die Vorlage in der Kategorie „Erweiterbarkeit” unter Visual C# oder Visual Basic. Diese Vorlage wird als Teil des Visual Studio-SDK installiert. ([Exemplarische Vorgehensweise: Erstellen eines SDK mit C# oder Visual Basic](https://msdn.microsoft.com/library/jj127119.aspx) oder [Exemplarische Vorgehensweise: Erstellen eines SDK mit C++](https://msdn.microsoft.com/library/jj127117.aspx)zeigt die Verwendung dieser Vorlage in einem sehr einfachen Szenario. )
3.  Legen Sie die Ordnerstruktur für Ihr SDK fest. Die Ordnerstruktur beginnt auf der Stammebene des VSIX-Projekts mit den Ordnern **References**, **Redist** und **DesignTime**.

    -   **References** ist der Speicherort für Binärdateien, die Ihre Benutzer für die Programmierung verwenden können. Das Erweiterungs-SDK erstellt in den Visual Studio-Projekten Ihrer Benutzer Verweise auf diese Dateien.
    -   **Redist** ist der Speicherort für andere Dateien, die mit Ihren Binärdateien in Apps, die mit Ihrer Komponente erstellt werden, verteilt werden müssen.
    -   **DesignTime** ist der Speicherort für Dateien, die nur verwendet werden, wenn Entwickler Apps erstellen, die Ihre Komponente verwenden.

    In jedem dieser Ordner können Sie Konfigurationsordner erstellen. Die zulässigen Namen sind „debug”, „retail” und „CommonConfiguration”. Der Ordner „CommonConfiguration” ist für Dateien vorgesehen, die für „retail”- und „debug”-Builds identisch sind. Wenn Sie nur „retail”-Builds Ihrer Komponente verteilen, können Sie alles in „CommonConfiguration” einfügen und die beiden anderen Ordner weglassen.

    In jedem Konfigurationsordner können Sie Architekturordner für plattformspezifische Dateien bereitstellen. Wenn Sie dieselben Dateien für alle Plattformen verwenden, können Sie einen einzelnen Ordner mit dem Namen „neutral” vorsehen. Einzelheiten zur Ordnerstruktur, einschließlich anderer Namen für Architekturordner, finden Sie unter [Gewusst wie: Erstellen eines Software Development Kit (SDK)](https://msdn.microsoft.com/library/hh768146.aspx) in der MSDN Library. (Dieser Artikel behandelt Plattform-SDKs und Erweiterungs-SDKs. Reduzieren Sie den Abschnitt über Plattform-SDKs, um Missverständnisse zu vermeiden. )

4.  Erstellen Sie eine SDK-Manifestdatei. Das Manifest enthält den Namen, die Version, die Architekturen, die Ihr SDK unterstützt, die .NET Framework-Versionen und Informationen darüber, wie Visual Studio Ihr SDK verwendet. Ausführliche Informationen und ein Beispiel dazu finden Sie unter [Gewusst wie: Erstellen eines Software Development Kit (SDK)](https://msdn.microsoft.com/library/hh768146.aspx).
5.  Erstellen und verteilen Sie das Erweiterungs-SDK. Ausführliche Informationen einschließlich Lokalisierung und Signierung des VSIX-Pakets finden Sie unter „VSIX Deployment” in der MSDN Library.

## Verwandte Themen

* [Erstellen eines Software Development Kit](https://msdn.microsoft.com/library/hh768146.aspx)
* [NuGet-Paketverwaltungssystem](https://github.com/NuGet/Home)
* [Ressourcenverwaltungssystem (Windows)](https://msdn.microsoft.com/library/windows/apps/jj552947.aspx)
* [Suchen und Verwenden von Visual Studio-Erweiterungen](https://msdn.microsoft.com/library/dd293638.aspx)
* [Befehlsoptionen für MakePRI.exe](https://msdn.microsoft.com/library/windows/apps/jj552945.aspx)



<!--HONumber=Mar16_HO1-->


