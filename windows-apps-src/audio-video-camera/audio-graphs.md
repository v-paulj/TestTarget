---
author: drewbatgit
ms.assetid: CB924E17-C726-48E7-A445-364781F4CCA1
description: "In diesem Artikel wird gezeigt, wie die APIs im Windows.Media.Audio-Namespace zum Erstellen von Audiodiagrammen für Audiorouting sowie Misch- und Verarbeitungsszenarien verwendet werden."
title: Audiodiagramme
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 7e8df66a1fc4c95cb8b0b4be9eded8ef58b6803a

---

# Audiodiagramme

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


In diesem Artikel wird gezeigt, wie die APIs im [**Windows.Media.Audio**](https://msdn.microsoft.com/library/windows/apps/dn914341)-Namespace zum Erstellen von Audiodiagrammen für Audiorouting sowie Misch- und Verarbeitungsszenarien verwendet werden.

Ein Audiodiagramm ist ein Satz von miteinander verbundenen Audioknoten, über die Audiodaten fließen. Audioeingabeknoten liefern Audiodaten von Audioeingangsgeräten, Audiodateien oder benutzerdefiniertem Code an das Diagramm. Audioausgabeknoten sind das Ziel von Audiodaten, die von dem Diagramm verarbeitet wurden. Audiodaten können außerhalb des Diagramms an Audioausgabegeräte, Audiodateien oder benutzerdefinierten Code weitergeleitet werden. Der letzte Knotentyp ist ein Submixknoten, der Audiodaten von einem oder mehreren Knoten in einer einzigen Ausgabe kombiniert, die an andere Knoten in dem Diagramm weitergeleitet werden kann. Nachdem alle Knoten erstellt und die Verbindungen zwischen ihnen eingerichtet wurden, starten Sie einfach das Audiodiagramm. Daraufhin fließen die Audiodaten von den Eingabeknoten über Submixknoten an die Ausgabeknoten. Dank dieses Modells können Szenarien wie die Aufzeichnung vom Mikrofon eines Geräts in einer Audiodatei, das Wiedergeben von Audiodaten aus einer Datei auf einem Gerätelautsprecher oder das Mischen von Audiodaten aus mehreren Quellen schnell und einfach implementiert werden.

Weitere Szenarien werden durch das Hinzufügen von Audioeffekten zum Audiodiagramm ermöglicht. Jeder Knoten in einem Audiodiagramm kann mit null oder mehr Audioeffekten gefüllt werden, die die Audioverarbeitung für die Audiodaten durchführen, die den Knoten durchlaufen. Es gibt verschiedene integrierte Effekte wie Echo, Equalizer, Begrenzungen und Halleffekt, die mit nur wenigen Codezeilen an einen Audioknoten angefügt werden können. Sie können auch eigene benutzerdefinierte Audioeffekte erstellen, die genau wie die integrierten Effekte funktionieren.

**Hinweis**  
Im [UWP-Beispiel für AudioGraph](http://go.microsoft.com/fwlink/?LinkId=619481) wird der in dieser Übersicht erläuterte Code implementiert. Sie können das Beispiel herunterladen, um den Code im Kontext anzuzeigen oder ihn als Ausgangspunkt für Ihre eigene App zu verwenden.

## Auswählen von Windows-Runtime-AudioGraph oder -XAudio2

Die Windows-Runtime-Audiodiagramm-APIs bieten Funktionen, die auch über die COM-basierten [XAudio2-APIs](https://msdn.microsoft.com/library/windows/desktop/hh405049) implementiert werden können. Nachfolgend sind die Features des Windows-Runtime-Audiodiagramm-Frameworks aufgeführt, die von XAudio2 abweichen.

-   Die Windows-Runtime-Audiodiagramm-APIs sind wesentlich benutzerfreundlicher als XAudio2.
-   Die Windows-Runtime-Audiodiagramm-APIs können von C# verwendet werden - und werden auch für C++ unterstützt.
-   Die Windows-Runtime-Audiodiagramm-APIs können Audiodateien, einschließlich komprimierter Dateiformate, direkt verwenden. XAudio2 funktioniert nur auf Audiopuffern und stellt keine Datei-E/A-Funktionen bereit.
-   Die Windows-Runtime-Audiodiagramm-APIs können die Audiopipeline mit geringer Latenzzeit in Windows 10 verwenden.
-   Die Windows-Runtime-Audiodiagramm-APIs unterstützen eine automatische Endpunktumschaltung, wenn standardmäßige Endpunktparameter verwendet werden. Wenn der Benutzer beispielsweise vom Lautsprecher eines Geräts zu einem Headset wechselt, werden die Audiodaten automatisch an den neuen Eingang umgeleitet.

## AudioGraph-Klasse

Die [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/dn914176)-Klasse ist das übergeordnete Element aller Knoten, aus denen das Diagramm besteht. Verwenden Sie dieses Objekt, um Instanzen aller Audioknotentypen zu erstellen. Erstellen Sie eine Instanz der **AudioGraph**-Klasse, indem Sie ein [**AudioGraphSettings**](https://msdn.microsoft.com/library/windows/apps/dn914185)-Objekt, das Konfigurationseinstellungen für das Diagramm enthält, initialisieren und dann [**AudioGraph.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn914216) aufrufen. Die zurückgegebene [**CreateAudioGraphResult**](https://msdn.microsoft.com/library/windows/apps/dn914273)-Klasse ermöglicht den Zugriff auf das erstellte Audiodiagramm oder gibt einen Fehler zurück, wenn bei der Erstellung des Audiodiagramms ein Fehler auftritt.

[!code-cs[DeclareAudioGraph](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareAudioGraph)]

[!code-cs[InitAudioGraph](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetInitAudioGraph)]

-   Alle Audioknotentypen werden mithilfe der Create\*-Methoden der **AudioGraph**-Klasse erstellt.
-   Die [**AudioGraph.Start**](https://msdn.microsoft.com/library/windows/apps/dn914244)-Methode bewirkt, dass das Audiodiagramm mit der Verarbeitung der Audiodaten beginnt. Die [**AudioGraph.Stop**](https://msdn.microsoft.com/library/windows/apps/dn914245)-Methode beendet die Audioverarbeitung. Jeder Knoten im Diagramm kann während der Ausführung des Diagramms unabhängig gestartet und beendet werden. Es sind aber keine Knoten aktiv, wenn das Diagramm beendet wird. [
              **ResetAllNodes**
            ](https://msdn.microsoft.com/library/windows/apps/dn914242) bewirkt, dass alle Knoten im Diagramm alle Daten löschen, die sich derzeit in ihren Audiopuffern befinden.
-   Das [**QuantumStarted**](https://msdn.microsoft.com/library/windows/apps/dn914241)-Ereignis tritt auf, wenn das Diagramm die Verarbeitung eines neuen Quantums von Audiodaten beginnt. Das [**QuantumProcessed**](https://msdn.microsoft.com/library/windows/apps/dn914240)-Ereignis tritt auf, wenn die Verarbeitung eines Quantums abgeschlossen ist.

-   Als einzige [**AudioGraphSettings**](https://msdn.microsoft.com/library/windows/apps/dn914185)-Eigenschaft ist [**AudioRenderCategory**](https://msdn.microsoft.com/library/windows/apps/dn297724) erforderlich. Durch Angabe dieses Werts kann das System die Audiopipeline für die angegebene Kategorie optimieren.
-   Die Quantumgröße des Audiodiagramms bestimmt die Anzahl der Samples, die gleichzeitig verarbeitet werden. Standardmäßig beträgt die Quantumgröße 10 ms basierend auf der Standard-Samplingrate. Wenn Sie eine benutzerdefinierte Quantumgröße durch Festlegen der [**DesiredSamplesPerQuantum**](https://msdn.microsoft.com/library/windows/apps/dn914205)-Eigenschaft angeben, müssen Sie auch die [**QuantumSizeSelectionMode**](https://msdn.microsoft.com/library/windows/apps/dn914208)-Eigenschaft auf **ClosestToDesired** festlegen, oder der angegebene Wert wird ignoriert. Wenn dieser Wert verwendet wird, wählt das System eine Quantumgröße aus, die möglich nah an der von Ihnen angegebenen Größe liegt. Um die tatsächliche Quantumgröße zu bestimmen, überprüfen Sie die [**SamplesPerQuantum**](https://msdn.microsoft.com/library/windows/apps/dn914243)-Eigenschaft der **AudioGraph**-Klasse, nachdem sie erstellt wurde.
-   Wenn Sie das Audiodiagramm nur mit Dateien verwenden möchten und keine Ausgabe an ein Audiogerät planen, wird empfohlen, dass Sie die Standard-Quantumgröße verwenden, indem Sie die [**DesiredSamplesPerQuantum**](https://msdn.microsoft.com/library/windows/apps/dn914205)-Eigenschaft nicht festlegen.
-   Die [**DesiredRenderDeviceAudioProcessing**](https://msdn.microsoft.com/library/windows/apps/dn958522)-Eigenschaft bestimmt Verarbeitungsleistung, die das primäre Darstellungsgerät für die Ausgabe des Audiodiagramms durchführt. Über die **Default**-Einstellung kann das System die Standardaudioverarbeitung für die angegebene Audiowiedergabekategorie verwenden. Durch diese Verarbeitung kann der Sound der Audiodaten auf einigen Geräten wesentlich verbessert werden, insbesondere auf mobilen Geräten mit kleinen Lautsprechern. Durch die **Raw**-Einstellung kann die Leistung durch Minimieren der Signalverarbeitungsleistung verbessert werden. Dies kann jedoch zu einer schlechteren Tonqualität auf einigen Geräten führen.
-   Wenn die [**QuantumSizeSelectionMode**](https://msdn.microsoft.com/library/windows/apps/dn914208)-Eigenschaft auf **LowestLatency** festgelegt wird, verwendet das Audiodiagramm für [**DesiredRenderDeviceAudioProcessing**](https://msdn.microsoft.com/library/windows/apps/dn958522) automatisch **Raw**.
-   Die [**EncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn958523)-Eigenschaft bestimmt das vom Diagramm verwendete Audioformat. Es werden nur 32-Bit-Float-Formate unterstützt.
-   Die [**PrimaryRenderDevice**](https://msdn.microsoft.com/library/windows/apps/dn958524)-Eigenschaft legt das primäre Darstellungsgerät für das Audiodiagramm fest. Wenn Sie diese Eigenschaft nicht festlegen, wird das Standardsystemgerät verwendet. Das primäre Darstellungsgerät wird zur Berechnung der Quantumgrößen für andere Knoten im Diagramm verwendet. Wenn in dem System keine Audiowiedergabegeräte vorhanden sind, tritt bei der Erstellung des Audiodiagramms ein Fehler auf.

Sie können für das Audiodiagramm das Standard-Audiowiedergabegerät festlegen oder die [**Windows.Devices.Enumeration.DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393)-Klasse verwenden, um eine Liste der verfügbaren Audiowiedergabegeräte abzurufen, indem Sie [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) aufrufen und die von [**Windows.Media.Devices.MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/br226817) zurückgegebene Auswahl des Audiowiedergabegeräts übergeben. Sie können eines der zurückgegebenen **DeviceInformation**-Objekte programmgesteuert auswählen oder die Benutzeroberfläche anzeigen, damit der Benutzer ein Gerät auswählen und dieses dann zum Festlegen der [**PrimaryRenderDevice**](https://msdn.microsoft.com/library/windows/apps/dn958524)-Eigenschaft verwenden kann.

[!code-cs[EnumerateAudioRenderDevices](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetEnumerateAudioRenderDevices)]

##  Geräteingabeknoten

Eine Geräteingabeknoten liefert Audiodaten von einem an das System angeschlossenen Audioaufnahmegerät wie etwa einem Mikrofon an das Diagramm. Erstellen Sie ein [**DeviceInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914082)-Objekt, das das standardmäßige Audioaufnahmegerät des Systems verwendet, indem Sie die [**CreateDeviceInputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914218)-Methode aufrufen. Geben Sie eine [**AudioRenderCategory**](https://msdn.microsoft.com/library/windows/apps/dn297724)-Enumeration an, damit das System die Audiopipeline für die angegebene Kategorie optimieren kann.

[!code-cs[DeclareDeviceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareDeviceInputNode)]


[!code-cs[CreateDeviceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateDeviceInputNode)]

Wenn Sie ein spezielles Audioaufnahmegerät für den Geräteeingabeknoten festlegen möchten, können Sie die [**Windows.Devices.Enumeration.DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393)-Klasse verwenden, um eine Liste der verfügbaren Audioaufnahmegeräte abzurufen, indem Sie [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) aufrufen und die von [**Windows.Media.Devices.MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/br226817) zurückgegebene Auswahl des Audioaufnahmegeräts übergeben. Sie können eines der zurückgegebenen **DeviceInformation**-Objekte programmgesteuert auswählen oder Benutzeroberfläche anzeigen, damit der Benutzer ein Gerät auswählen und dieses dann an die [**CreateDeviceInputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914218)-Methode übergeben kann.

[!code-cs[EnumerateAudioCaptureDevices](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetEnumerateAudioCaptureDevices)]

##  Gerätausgabeknoten

Ein Gerätausgabeknoten überträgt Audiodaten von dem Diagramm an ein Audiowiedergabegerät, z. B. Lautsprecher oder ein Headset. Erstellen Sie eine [**DeviceOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914098)-Klasse, indem Sie die [**CreateDeviceOutputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn958525)-Methode aufrufen. Der Ausgabeknoten verwendet die [**PrimaryRenderDevice**](https://msdn.microsoft.com/library/windows/apps/dn958524)-Eigenschaft des Audiodiagramms.

[!code-cs[DeclareDeviceOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareDeviceOutputNode)]

[!code-cs[CreateDeviceOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateDeviceOutputNode)]

##  Dateieingabeknoten

Mit einem Dateieingabeknoten können Sie Daten aus einer Audiodatei in das Diagramm übertragen. Erstellen Sie eine [**AudioFileInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914108)-Klasse, indem Sie die [**CreateFileInputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914226)-Methode aufrufen.

[!code-cs[DeclareFileInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFileInputNode)]


[!code-cs[CreateFileInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFileInputNode)]

-   Dateieingabeknoten unterstützen die Dateiformate MP3, WAV, WMA und M4A.
-   Legen Sie die [**StartTime**](https://msdn.microsoft.com/library/windows/apps/dn914130)-Eigenschaft so fest, dass in der Datei der Zeitoffset angegeben wird, an dem die Wiedergabe beginnen soll. Wenn diese Eigenschaft null ist, wird der Anfang der Datei verwendet. Legen Sie die [**EndTime**](https://msdn.microsoft.com/library/windows/apps/dn914118)-Eigenschaft so fest, dass in der Datei der Zeitoffset angegeben wird, an dem die Wiedergabe enden soll. Wenn diese Eigenschaft null ist, wird das Ende der Datei verwendet. Die Startzeit muss vor der Endzeit liegen, und der Wert für die Endzeit muss kleiner oder gleich der Dauer der Audiodatei sein. Sie können die Richtigkeit anhand des [**Duration**](https://msdn.microsoft.com/library/windows/apps/dn914116)-Eigenschaftswerts prüfen.
-   Suchen Sie eine Position in der Audiodatei, indem Sie die [**Seek**](https://msdn.microsoft.com/library/windows/apps/dn914127)-Methode aufrufen und den Zeitoffset in der Datei angeben, an den die Wiedergabeposition verschoben werden soll. Der angegebene Wert muss zwischen den Eigenschaften [**StartTime**](https://msdn.microsoft.com/library/windows/apps/dn914130) und [**EndTime**](https://msdn.microsoft.com/library/windows/apps/dn914118) liegen. Die aktuelle Wiedergabeposition des Knotens können Sie mit der schreibgeschützten [**Position**](https://msdn.microsoft.com/library/windows/apps/dn914124)-Eigenschaft abrufen.
-   Aktivieren Sie Schleifen für die Audiodatei, indem Sie die [**LoopCount**](https://msdn.microsoft.com/library/windows/apps/dn914120)-Eigenschaft festlegen. Wenn diese nicht Null ist, gibt dieser Wert die Anzahl der Wiederholungen der Datei nach der ersten Wiedergabe an. Wenn Sie **LoopCount** beispielsweise auf 1 festlegen, wird die Datei insgesamt zweimal wiedergegeben. Wenn Sie den Wert auf 5 festlegen, wird die Datei insgesamt sechs Mal wiedergegeben. Indem Sie **LoopCount** auf Null setzen, wird die Datei in einer Schleife unbegrenzt wiedergegeben. Um die Schleife zu beenden, setzen Sie den Wert auf 0 fest.
-   Legen Sie zum Anpassen der Geschwindigkeit, mit der die Audiodatei wiedergegeben wird, die [**PlaybackSpeedFactor**](https://msdn.microsoft.com/library/windows/apps/dn914123)-Eigenschaft fest. Der Wert 1 zeigt die ursprüngliche Geschwindigkeit der Datei an. Der Wert 0,5 legt die halbe Geschwindigkeit und der Wert 2 ist die doppelte Geschwindigkeit fest.

##  Dateiausgabeknoten

Mit einem Dateiausgabeknoten können Sie die Audiodaten aus dem Diagramm in eine Audiodatei umleiten. Erstellen Sie eine [**AudioFileOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914133)-Klasse, indem Sie die [**CreateFileOutputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914227)-Methode aufrufen.

[!code-cs[DeclareFileOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFileOutputNode)]


[!code-cs[CreateFileOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFileOutputNode)]

-   Dateiausgabeknoten unterstützen die Dateiformate MP3, WAV, WMA und M4A.
-   Um die Verarbeitung des Knotens zu beenden, rufen Sie erst die [**AudioFileOutputNode.Stop**](https://msdn.microsoft.com/library/windows/apps/dn914144)-Methode und dann die [**AudioFileOutputNode.FinalizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914140)-Methode auf. Andernfalls wird eine Ausnahme ausgelöst.

##  Audioframe-Eingabeknoten

Mit einem Audioframe-Eingabeknoten können Sie Audiodaten, die Sie in Ihrem eigenen Code generieren, in das Audiodiagramm übertragen. Dies ermöglicht Szenarien wie das Erstellen eines benutzerdefinierten Softwaresynthesizers. Erstellen Sie eine [**AudioFrameInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914147)-Klasse, indem Sie die [**CreateFrameInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914230)-Methode aufrufen.

[!code-cs[DeclareFrameInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFrameInputNode)]


[!code-cs[CreateFrameInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFrameInputNode)]

Das [**FrameInputNode.QuantumStarted**](https://msdn.microsoft.com/library/windows/apps/dn958507)-Ereignis wird ausgelöst, wenn das Audiodiagramm bereit ist, die Verarbeitung des nächsten Quantums von Audiodaten zu verarbeiten. Sie stellen die benutzerdefinierten generierten Audiodaten vom Handler für dieses Ereignis bereit.

[!code-cs[QuantumStarted](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetQuantumStarted)]

-   Durch das an den **QuantumStarted**-Ereignishandler übergebene [**FrameInputNodeQuantumStartedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn958533)-Objekt wird die [**RequiredSamples**](https://msdn.microsoft.com/library/windows/apps/dn958534)-Eigenschaft verfügbar, die angibt, wie viele Sample das Audiodiagramm füllen muss, damit das Quantum verarbeitet wird.
-   Rufen Sie die [**AudioFrameInputNode.AddFrame**](https://msdn.microsoft.com/library/windows/apps/dn914148)-Methode auf, um ein mit Audiodaten gefülltes [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871)-Objekt an das Diagramm zu übergeben.
-   Nachfolgend sehen Sie ein Beispiel einer Implementierung der **GenerateAudioData**-Hilfsmethode.

Zum Auffüllen eines [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871)-Objekts mit Audiodaten benötigen Sie Zugriff auf den zugrunde liegenden Speicherpuffer des Audioframes. Initialisieren Sie zu diesem Zweck die **IMemoryBufferByteAccess**-COM-Schnittstelle, indem Sie dem Namespace den folgenden Code hinzufügen.

[!code-cs[ComImportIMemoryBufferByteAccess](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetComImportIMemoryBufferByteAccess)]

Der folgende Code zeigt ein Beispiel einer Implementierung der **GenerateAudioData**-Hilfsmethode, die ein [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871)-Objekt erstellt und dieses mit Audiodaten füllt.

[!code-cs[GenerateAudioData](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetGenerateAudioData)]

-   Da diese Methode auf den Rohdatenpuffer zugreift, der Windows-Runtime-Typen zugrunde liegt, muss diese mithilfe des Schlüsselworts **unsafe** deklariert werden. Außerdem müssen Sie das Projekt in Microsoft Visual Studio so konfigurieren, dass die Kompilierung von unsicherem Code zugelassen wird. Öffnen Sie hierfür die Seite **Eigenschaften** des Projekts, klicken Sie auf die Eigenschaftenseite **Build**, und aktivieren Sie das Kontrollkästchen **Unsicheren Code zulassen**.
-   Initialisieren Sie im **Windows.Media**-Namespace eine neue Instanz des [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871)-Objekts, indem Sie die gewünschte Puffergröße an den Konstruktor übergeben. Die Puffergröße ist die Sample-Anzahl multipliziert mit der Größe der einzelnen Sample.
-   Rufen Sie das [**AudioBuffer**](https://msdn.microsoft.com/library/windows/apps/dn958454)-Objekt des Audioframes ab, indem Sie die [**LockBuffer**](https://msdn.microsoft.com/library/windows/apps/dn930878)-Methode aufrufen.
-   Rufen Sie eine Instanz der [**IMemoryBufferByteAccess**](https://msdn.microsoft.com/library/windows/desktop/mt297505)-COM-Schnittstelle aus dem Audiopuffer ab, indem Sie [**CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn958457) aufrufen.
-   Rufen Sie einen Zeiger auf die Rohdaten des Audiopuffers ab, indem Sie die [**IMemoryBufferByteAccess.GetBuffer**](https://msdn.microsoft.com/library/windows/desktop/mt297506)-Methode aufrufen und in den Beispieldatentyp der Audiodaten umwandeln.
-   Füllen Sie den Puffer mit Daten, und geben Sie das [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871)-Objekt für die Übermittlung an das Audiodiagramm zurück.

##  Audioframe-Ausgabeknoten

Mit einem Audioframe-Ausgabeknoten können Sie eine Audiodatenausgabe aus dem Audiodiagramm mit benutzerdefiniertem Code empfangen und verarbeiten. Ein Beispielszenario hierfür ist das Durchführen einer Signalanalyse für die Audioausgabe. Erstellen Sie ein [**AudioFrameOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914166)-Objekt, indem Sie die [**CreateFrameOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914233)-Methode aufrufen.

[!code-cs[DeclareFrameOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFrameOutputNode)]

[!code-cs[CreateFrameOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFrameOutputNode)]

Das [**AudioGraph.QuantumProcessed**](https://msdn.microsoft.com/library/windows/apps/dn914240)-Ereignis wird ausgelöst, wenn das Audiodiagramm die Verarbeitung eines Quantums von Audiodaten abgeschlossen hat. Sie können innerhalb des Handlers für dieses Ereignis auf die Audiodaten zugreifen.

[!code-cs[QuantumProcessed](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetQuantumProcessed)]

-   Rufen Sie die [**GetFrame**](https://msdn.microsoft.com/library/windows/apps/dn914171)-Methode auf, um ein mit Audiodaten gefülltes [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871)-Objekt mit Audiodaten aus dem Diagramm abzurufen.
-   Nachfolgend sehen Sie ein Beispiel einer Implementierung der **ProcessFrameOutput**-Hilfsmethode.

[!code-cs[ProcessFrameOutput](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetProcessFrameOutput)]

-   Wie in dem Beispiel oben für den Audioframe-Eingabeknoten müssen Sie die **IMemoryBufferByteAccess**-COM-Schnittstelle deklarieren und das Projekt so konfigurieren, dass unsicherer Code zugelassen wird, damit Sie auf den zugrunde liegenden Audiopuffer zugreifen können.
-   Rufen Sie das [**AudioBuffer**](https://msdn.microsoft.com/library/windows/apps/dn958454)-Objekt des Audioframes ab, indem Sie die [**LockBuffer**](https://msdn.microsoft.com/library/windows/apps/dn930878)-Methode aufrufen.
-   Rufen Sie eine Instanz der **IMemoryBufferByteAccess**-COM-Schnittstelle aus dem Audiopuffer ab, indem Sie [**CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn958457) aufrufen.
-   Rufen Sie einen Zeiger auf die Rohdaten des Audiopuffers ab, indem Sie die **IMemoryBufferByteAccess.GetBuffer**-Methode aufrufen und in den Beispieldatentyp der Audiodaten umwandeln.

## Knotenverbindungen und Submixknoten

Alle Eingabeknotentypen machen die **AddOutgoingConnection**-Methode verfügbar, die die vom Knoten produzierten Audiodaten an den Knoten weiterleitet, der an die Methode übergeben wird. Im folgenden Beispiel wird eine Verbindung zwischen [**AudioFileInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914108) und [**AudioDeviceOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914098) hergestellt. Dabei handelt es sich um ein einfaches Setup für die Wiedergabe einer Audiodatei über den Lautsprecher des Geräts.

[!code-cs[AddOutgoingConnection1](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection1)]

Von einem Eingabeknoten können mehrere Verbindungen zu anderen Knoten erstellt werden. Im folgenden Beispiel wird eine Verbindung zwischen [**AudioFileInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914108) und [**AudioFileOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914133) hinzugefügt. Die Audiodaten aus der Audiodatei werden nun über den Lautsprecher des Geräts wiedergegeben und auch in eine Audiodatei geschrieben.

[!code-cs[AddOutgoingConnection2](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection2)]

Ausgabeknoten können auch mehrere Verbindungen von anderen Knoten empfangen. Im folgenden Beispiel wird eine Verbindung zwischen den Knoten [**AudioDeviceInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914082) und [**AudioDeviceOutput**](https://msdn.microsoft.com/library/windows/apps/dn914098) hergestellt. Da der Ausgabeknoten Verbindungen vom Dateieingabeknoten und dem Geräteingabeknoten aufweist, enthält die Ausgabe eine Audiomischung aus beiden Quellen. **AddOutgoingConnection** bietet eine Überladung, mit der Sie einen Verstärkungswert für das über die Verbindung laufende Signal angeben können.

[!code-cs[AddOutgoingConnection3](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection3)]

Obwohl Ausgabeknoten Verbindungen von mehreren Knoten annehmen können, sollten Sie eine Zwischenmischung von Signalen von einem oder mehreren Knoten erstellen, bevor Sie die Mischung an eine Ausgabe übergeben. Angenommen, Sie möchten die Ebene festlegen oder Effekte auf eine Untergruppe der Audiosignale in einem Diagramm anwenden. Verwenden Sie hierfür die [**AudioSubmixNode**](https://msdn.microsoft.com/library/windows/apps/dn914247)-Klasse. Sie können keine Verbindung zu einem Submixknoten von einer oder mehreren Eingabeknoten oder anderen Submixknoten herstellen. Im folgenden Beispiel wird ein neuer Submixknoten zu [**AudioGraph.CreateSubmixNode**](https://msdn.microsoft.com/library/windows/apps/dn914236) erstellt. Anschließend werden Verbindungen von einem Dateiausgabeknoten und einem Frameausgabeknoten zum Submixknoten hinzugefügt. Schließlich wird der Submixknoten mit einem Dateiausgabeknoten verbunden.

[!code-cs[CreateSubmixNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateSubmixNode)]

## Starten und Beenden von Audiodiagrammknoten

Wenn die [**AudioGraph.Start**](https://msdn.microsoft.com/library/windows/apps/dn914244)-Methode aufgerufen wird, beginnt das Audiodiagramm mit der Verarbeitung der Audiodaten. Jeder Knoten umfasst eine **Start**- und **Stop**-Methode, die bewirken, dass der jeweilige Knoten die Verarbeitung von Daten beginnt oder beendet. Bei Aufruf der [**AudioGraph.Stop**](https://msdn.microsoft.com/library/windows/apps/dn914245)-Methode wird die gesamte Verarbeitung in allen Knoten unabhängig vom Status der einzelnen Knoten beendet. Der Status der einzelnen Knoten kann jedoch festgelegt werden, während das Audiodiagramm beendet wird. Sie können beispielsweise die **Stop**-Methode in einem einzelnen Knoten aufrufen, während das Diagramm beendet wird. Rufen Sie anschließend die **AudioGraph.Start**-Methode auf, damit der einzelne Knoten angehalten bleibt.

Mit allen Knotentypen wird die **ConsumeInput**-Eigenschaft verfügbar. Diese lest bei Festlegung auf „false“ zu, dass der Knoten die Audioverarbeitung fortsetzt. Gleichzeitig verhinder sie, dass Audiodaten von anderen Knoten verbraucht werden.

Mit allen Knotentypen wird die **Reset** -Methode verfügbar. Sie bewirkt, dass der Knoten alle Audiodaten löscht, die sich aktuell in seinem Puffer befinden.

## Hinzufügen von Audioeffekten

Mit der Audiodiagramm-API können Sie Audioeffekte zu jedem Knotentyp in einem Diagramm hinzufügen. Ausgabeknoten, Eingabeknoten und Submixknoten können jeweils eine unbegrenzte Anzahl von Audioeffekten aufweisen. Eine Einschränkung erfolgt lediglich durch die Funktionen der Hardware. Im folgenden Beispiel ist das Hinzufügen des integrierten Echoeffekts zu einem Submixknoten dargestellt.

[!code-cs[AddEffect](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddEffect)]

-   Alle Audioeffekte implementieren die [**IAudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608044)-Schnittstelle. Mit jedem Knoten wird eine **EffectDefinitions**-Eigenschaft verfügbar, welche die Liste der auf diesen Knoten angewendeten Effekte darstellt. Fügen Sie einen Effekt hinzu, indem Sie sein Definitionsobjekt zu der Liste hinzufügen.
-   Es gibt mehrere Effektdefinitionsklassen, die im **Windows.Media.Audio**-Namespace bereitgestellt werden. Dazu zählen:
    -   [**EchoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn914276)
    -   [**EqualizerEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn914287)
    -   [**LimiterEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn914306)
    -   [**ReverbEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn914313)
-   Sie können Ihre eigenen Audioeffekte zum Implementieren von [**IAudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608044) erstellen. Wenden Sie diese anschließend auf einen beliebigen Knoten in einem Audiodiagramm an.
-   Mit jedem Knotentyp wird eine **DisableEffectsByDefinition**-Methode verfügbar, die alle Effekte in der **EffectDefinitions**-Liste des Knotens deaktiviert, die mithilfe der angegebenen Definition hinzugefügt wurden. **EnableEffectsByDefinition** aktiviert die Effekte mit der angegebenen Definition.

 

 







<!--HONumber=Jun16_HO4-->


