---
author: drewbatgit
Description: 'In diesem Artikel wird beschrieben, wie Sie eine Windows-Runtime-Komponente erstellen, die die IBasicVideoEffect-Schnittstelle implementiert, mit der Sie benutzerdefinierte Effekte für Videostreams erstellen können.'
MS-HAID: 'dev\_audio\_vid\_camera.custom\_video\_effects'
MSHAttr: 'PreferredLib:/library/windows/apps'
Search.Product: eADQiWindows 10XVcnh
title: Benutzerdefinierte Videoeffekte
---

# Benutzerdefinierte Videoeffekte


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


\[Einige Informationen beziehen sich auf die Vorabversion, die vor der kommerziellen Freigabe möglicherweise wesentlichen Änderungen unterliegt. Microsoft übernimmt für die hier bereitgestellten Informationen keine Garantie, weder ausdrücklicher noch impliziter Art.\]

In diesem Artikel wird beschrieben, wie Sie eine Windows-Runtime-Komponente erstellen, die die [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788)-Schnittstelle implementiert, mit der Sie benutzerdefinierte Effekte für Videostreams erstellen können. Benutzerdefinierte Effekte können mit verschiedenen Windows-Runtime-APIs verwendet werden, z. B. [MediaCapture](https://msdn.microsoft.com/library/windows/apps/br241124), die den Zugriff auf die Kamera eines Gerätes ermöglicht, sowie [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646), mit der Sie komplexe Kompositionen aus Medienclips erstellen können.

## Hinzufügen eines benutzerdefinierten Effekts zu Ihrer App


Sie definieren einen benutzerdefinierte Videoefekt in einer Klasse, die die [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788)-Schnittstelle implementiert. Diese Klasse kann nicht direkt in Ihr App-Projekt integriert werden. Stattdessen müssen Sie eine Windows-Runtime-Komponente verwenden, um Ihre Videoeffektklasse zu hosten.

**Hinzufügen einer Windows-Runtime Komponente für den Videoeffekt**

1.  Wechseln Sie in Microsoft Visual Studio bei geöffneter Projektmappe zum Menü **Datei**, und wählen Sie **Hinzufügen -&gt; Neues Projekt...** aus.
2.  Wählen Sie den Projekttyp **Komponente für Windows-Runtime (Universal Windows)** aus.
3.  Nennen Sie das Projekt in diesem Beispiel „VideoEffectComponent“. Auf diesen Namen wird später im Code verwiesen.
4.  Klicken Sie auf **OK**.
5.  Die Projektvorlage erstellt eine Klasse namens „Class1.cs“. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Symbol für „Class1.cs“, und wählen Sie **Umbenennen** aus.
6.  Benennen Sie die Datei in „ExampleVideoEffect.cs“ um. Visual Studio zeigt eine Eingabeaufforderung an, in der Sie gefragt werden, ob Sie alle Verweise mit dem neuen Namen aktualisieren möchten. Klicken Sie auf **Ja**.
7.  Öffnen Sie „ExampleVideoEffect.cs“, und aktualisieren Sie die Definition der Klasse, um die [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788)-Schnittstelle zu implementieren.

[!code-cs[ImplementIBasicVideoEffect](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetImplementIBasicVideoEffect)]


Sie müssen die folgenden Namespaces in Ihre Effektklassendatei aufnehmen, um auf alle Typen, die in den Beispielen in diesem Artikel verwendet werden, zugreifen zu können.

[!code-cs[EffectUsing](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetEffectUsing)]


## Implementieren der IBasicVideoEffect-Schnittstelle mit Softwareverarbeitung


Der Videoeffekt muss alle Methoden und Eigenschaften der [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788)-Schnittstelle implementieren. In diesem Abschnitt wird der Prozess einer einfachen Implementierung der Schnittstelle erläutert, bei dem die Softwareverarbeitung verwendet wird.

### Close-Methode

Das System ruft die [**Close**](https://msdn.microsoft.com/library/windows/apps/dn764789)-Methode für die Klasse auf, wenn der Effekt beendet werden soll. Sie sollten diese Methode verwenden, um alle Ressourcen, die Sie erstellt haben, zu löschen. Das Argument der Methode ist ein „MediaEffectClosedReason“, der Ihnen mitteilt, ob der Effekt normal geschlossen wurde, ob ein Fehler aufgetreten ist oder ob der Effekt das erforderliche Codierungsformat nicht unterstützt.

[!code-cs[Close](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetClose)]


### DiscardQueuedFrames-Methode

Die [**DiscardQueuedFrames**](https://msdn.microsoft.com/library/windows/apps/dn764790)-Methode wird aufgerufen, wenn der Effekt zurückgesetzt werden soll. Ein typisches Szenario hierfür ist, wenn der Effekt zuvor verarbeitete Frames zum Verarbeiten des aktuellen Frames speichert. Wenn diese Methode aufgerufen wird, sollten Sie vorherige Frames, die Sie gespeichert haben, löschen. Dennoch kann diese Methode verwendet werden, um alle Zustände im Zusammenhang mit den vorherigen Frames zurückzusetzen, nicht nur Videoframes, die sich angesammelt haben.


[!code-cs[DiscardQueuedFrames](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetDiscardQueuedFrames)]



### IsReadOnly-Eigenschaft

Die [**IsReadOnly**](https://msdn.microsoft.com/library/windows/apps/dn764792)-Eigenschaft teilt dem System mit, ob Ihr Effekt in die Ausgabe des Effekts schreibt. Wenn Ihre App die Videoframes nicht ändert – z. B. ein Effekt, der nur eine Analyse der Videoframes durchführt –, sollten Sie diese Eigenschaft auf „true“ festlegen. Dann kopiert das System für Sie die Frameeingabe in die Frameausgabe.

**Tipp**  Wenn die [**IsReadOnly**](https://msdn.microsoft.com/library/windows/apps/dn764792)-Eigenschaft auf „true“ festgelegt ist, kopiert das System den Eingabeframe in den Ausgabeframe, bevor [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764794) aufgerufen wird. Das Festlegen der **IsReadOnly**-Eigenschaft auf „true“ schränkt Sie nicht darin ein, in die Ausgabeframes in **ProcessFrame** zu schreiben.

[!code-cs[IsReadOnly](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetIsReadOnly)] 


### SetEncodingProperties-Methode

Das System ruft [**SetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn919884) für den Effekt auf, um Ihnen die Codierungseigenschaften für den Videostream mitzuteilen, für den der Effekt gilt. Diese Methode bietet auch einen Verweis auf das Direct3D-Gerät, welches für das Hardwarerendering verwendet wird. Die Verwendung dieses Geräts wird im Beispiel zur Hardwareverarbeitung weiter unten in diesem Artikel gezeigt.

[!code-cs[SetEncodingProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSetEncodingProperties)]


### SupportedEncodingProperties-Eigenschaft

Das System überprüft die [**SupportedEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn764799) Eigenschaft, um festzustellen, welche Codierungseigenschaften von dem Effekt unterstützt werden. Beachten Sie Folgendes: Wenn der Nutzer Ihres Effekts das Video mit den von Ihnen angegebenen Eigenschaften nicht codieren kann, wird [**Close**](https://msdn.microsoft.com/library/windows/apps/dn764789) für den Effekt aufgerufen und er wird aus der Videopipeline entfernt.


[!code-cs[SupportedEncodingProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSupportedEncodingProperties)]


**Hinweis**  Wenn Sie eine leere Liste mit [**VideoEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701217)-Objekten von **SupportedEncodingProperties** zurückgeben, verwendet das System standardmäßig die ARGB32-Codierung.

 

### SupportedMemoryTypes-Eigenschaft

Das System überprüft die [**SupportedMemoryTypes**](https://msdn.microsoft.com/library/windows/apps/dn764801) Eigenschaft, um festzustellen, ob der Effekt auf Videoframes im Softwarespeicher oder im Hardwarearbeitsspeicher (GPU) zugreift. Wenn Sie [**MediaMemoryTypes.Cpu**](https://msdn.microsoft.com/library/windows/apps/dn764822) zurückgeben, werden an Ihren Effekt Ein- und Ausgabeframes übergeben, die Bilddaten in [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358)-Objekten enthalten. Wenn Sie **MediaMemoryTypes.Gpu** zurückgeben, werden an Ihren Effekt Ein- und Ausgabeframes übergeben, die Bilddaten in [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505)-Objekten enthalten.

[!code-cs[SupportedMemoryTypes](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSupportedMemoryTypes)]


**Hinweis**  Wenn Sie [**MediaMemoryTypes.GpuAndCpu**](https://msdn.microsoft.com/library/windows/apps/dn764822) angeben, verwendet das System entweder den GPU- oder Systemarbeitsspeicher, je nachdem, welcher für die Pipeline effizienter ist. Wenn Sie diesen Wert verwenden, müssen Sie in der [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764794)-Methode prüfen, ob [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) oder [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505), das an die Methode übergeben wird, Daten enthält, und den Frame dann entsprechend verarbeiten.

 

### TimeIndependent-Eigenschaft

Die [**TimeIndependent**](https://msdn.microsoft.com/library/windows/apps/dn764803)-Eigenschaft teilt dem System mit, ob der Effekt ein einheitliches Timing erfordert. Bei Festlegung auf „true“ kann das System Optimierungen verwenden, die die Leistung des Effekts verbessern.

[!code-cs[TimeIndependent](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetTimeIndependent)]

### SetProperties-Methode

Die [**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986)-Methode ermöglicht es der App, die Ihren Effekt verwendet, die Effektparameter anzupassen. Die Eigenschaften werden als [**IPropertySet**](https://msdn.microsoft.com/library/windows/apps/br226054)-Zuordnung von Eigenschaftsnamen und -werten übergeben.


[!code-cs[SetProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSetProperties)]


In diesem einfachen Beispiel werden die Pixel in jedem Videoframe entsprechend einem angegebenen Wert abgeblendet. Eine Eigenschaft wird deklariert, und „TryGetValue“ wird verwendet, um den von der aufrufenden App festgelegten Wert abzurufen. Wenn kein Wert festgelegt wurde, wird der Standardwert 0,5 verwendet.

[!code-cs[FadeValue](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetFadeValue)]


### ProcessFrame-Methode

Die [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764794)-Methode ist die Stelle, an der Ihr Effekt die Bilddaten des Videos ändert. Die Methode wird einmal pro Frame aufgerufen, und ihr wird ein [**ProcessVideoFrameContext**](https://msdn.microsoft.com/library/windows/apps/dn764826)-Objekt übergeben. Dieses Objekt enthält ein [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917)-Eingabeobjekt, das den eingehenden Frame umfasst, der verarbeitet werden soll, und ein **VideoFrame**-Ausgabeobjekt, in das Sie Bilddaten schreiben, die dem Rest der Videopipeline übergeben werden. Jedes dieser **VideoFrame**-Objekte verfügt über eine [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn930926)-Eigenschaft und eine [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn930920)-Eigenschaft. Welche aber verwendet werden kann, wird von dem Wert bestimmt, den Sie von der [**SupportedMemoryTypes**](https://msdn.microsoft.com/library/windows/apps/dn764801)-Eigenschaft zurückgeben.

Dieses Beispiel zeigt eine einfache Implementierung der **ProcessFrame**-Methode mit Softwareverarbeitung. Weitere Informationen zum Arbeiten mit  [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358)-Objekten finden Sie unter [Bildverarbeitung](imaging.md). Ein Beispiel für die **ProcessFrame**-Implementierung mit Hardwareverarbeitung wird später in diesem Artikel gezeigt.

Für den Zugriff auf den Datenpuffer einer **SoftwareBitmap** ist COM-Interoperabilität erforderlich. Daher sollten Sie den Namespace **System.Runtime.InteropServices** in Ihre Effektklassendatei aufnehmen.

[!code-cs[COMUsing](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetCOMUsing)]


Fügen Sie den folgenden Code in den Namespace für den Effekt ein, um die Schnittstelle für den Zugriff auf den Bildpuffer zu importieren.

[!code-cs[COMImport](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetCOMImport)]


**Hinweis**  Da diese Technik auf einen systemeigenen, nicht verwalteten Bildpuffer zugreift, müssen Sie Ihr Projekt konfigurieren, um unsicheren Code zuzulassen.
1.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt „VideoEffectComponent“, und wählen Sie „Eigenschaften...“ aus.
2.  Wählen Sie die Registerkarte „Erstellen“ aus.
3.  Aktivieren Sie das Kontrollkästchen für „Unsicheren Code zulassen“.

 

Sie können nun die **ProcessFrame**-Methodenimplementierung hinzufügen. Zunächst erhält diese Methode ein [**BitmapBuffer**](https://msdn.microsoft.com/library/windows/apps/dn887325)-Objekt aus den Ein- und Ausgabesoftwarebitmaps. Beachten Sie, dass der Ausgabeframe zum Schreiben und die Eingabe zum Lesen geöffnet wird. Als Nächstes wird ein [**IMemoryBufferReference**](https://msdn.microsoft.com/library/windows/apps/dn921671)-Objekt für jeden Puffer durch Aufrufen von [**CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn949046) abgerufen. Danach wird der tatsächliche Datenpuffer durch Umwandeln der **IMemoryBufferReference**-Objekte wie die oben definierte COMInterop-Schnittstelle, **IMemoryByteAccess**, und anschließendes Aufrufen von **GetBuffer** abgerufen.

Nun, da die Datenpuffer abgerufen wurden, können Sie aus dem Eingabepuffer lesen und in den Ausgabepuffer schreiben. Das Layout des Puffers wird durch Aufrufen von [**GetPlaneDescription**](https://msdn.microsoft.com/library/windows/apps/dn887330) abgerufen, das Informationen über die Breite und den anfänglichen Versatz des Puffers bereitstellt. Die Bits pro Pixel werden durch die Codierungseigenschaften bestimmt, die zuvor mit der [**SetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn919884)-Methode festgelegt wurden. Anhand der Informationen zum Pufferformat wird der Index im Puffer für jedes Pixel gefunden. Der Pixelwert aus dem Quellpuffer wird in den Zielpuffer kopiert, wobei die Farbwerte mit der FadeValue-Eigenschaft multipliziert werden, die für diesen Effekt definiert ist, um sie um den angegebenen Wert abzublenden.

[!code-cs[ProcessFrameSoftwareBitmap](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetProcessFrameSoftwareBitmap)]


## Implementieren der IBasicVideoEffect-Schnittstelle mit Hardwareverarbeitung


Das Erstellen eines benutzerdefinierten Videoeffekts mit der Hardwareverarbeitung (GPU) ist nahezu identisch mit der oben beschriebenen Verfahrensweise mit Softwareverarbeitung. In diesem Abschnitt werden die wenigen Unterschiede in einem Effekt erläutert, bei dem die Hardwareverarbeitung verwendet wird. In diesem Beispiel wird die Win2D-Windows-Runtime-API verwendet. Weitere Informationen zur Verwendung von Win2D finden Sie in der [Win2D-Dokumentation](http://go.microsoft.com/fwlink/?LinkId=519078).

Führen Sie die folgenden Schritte aus, um das Win2D-NuGet-Paket zu dem Projekt hinzuzufügen, das Sie wie im Abschnitt [Hinzufügen eines benutzerdefinierten Effekts zu Ihrer App](#addacustomeffect) zu Beginn dieses Artikels beschrieben erstellt haben.

**Hinzufügen des Win2D-NuGet-Pakets zum Effektprojekt**

1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **VideoEffectComponent**, und wählen Sie **NuGet-Pakete verwalten...** aus.
2.  Wählen Sie oben im Fenster die Registerkarte **Durchsuchen** aus.
3.  Geben Sie im Suchfeld „Win2D“ ein.
4.  Klicken Sie auf **Win2D.uwp**und dann im rechten Bereich auf „Installieren“.
5.  Im Dialogfeld **Änderungen überprüfen** wird das zu installierende Paket angezeigt. Klicken Sie auf **OK**.
6.  Akzeptieren Sie die Paketlizenz.

Zusätzlich zu den Namespaces, die bei der grundlegenden Einrichtung des Projekts verwendet wurden, müssen Sie die folgenden von Win2D bereitgestellten Namespaces hinzufügen.

[!code-cs[UsingWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetUsingWin2D)]


Da dieser Effekt GPU-Speicher für die Aktionen mit den Bilddaten verwendet, sollten Sie [**MediaMemoryTypes.Gpu**](https://msdn.microsoft.com/library/windows/apps/dn764822) aus der [**SupportedMemoryTypes**](https://msdn.microsoft.com/library/windows/apps/dn764801)-Eigenschaft zurückgeben.

[!code-cs[SupportedMemoryTypesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSupportedMemoryTypesWin2D)]


Legen Sie die Codierungseigenschaften, die vom Effekt unterstützt werden, mit der [**SupportedEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn764799)-Eigenschaft fest. Wenn Sie mit Win2D arbeiten, müssen Sie die ARGB32-Codierung verwenden.

[!code-cs[SupportedEncodingPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSupportedEncodingPropertiesWin2D)]


Verwenden Sie die [**SetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn919884)-Methode, um ein neues Win2D-**CanvasDevice**-Objekt vom [**IDirect3DDevice**](https://msdn.microsoft.com/library/windows/apps/dn895092) zu erstellen, das an die Methode übergeben wird.

[!code-cs[SetEncodingPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSetEncodingPropertiesWin2D)]


Die Implementierung von [**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986) ist mit dem Beispiel der Softwareverarbeitung oben identisch. Bei diesem Beispiel wird eine **BlurAmount**-Eigenschaft verwendet, um einen Win2D-Weichzeichnereffekt zu konfigurieren.

[!code-cs[SetPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSetPropertiesWin2D)]

[!code-cs[BlurAmountWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetBlurAmountWin2D)]


Der letzte Schritt besteht darin, die [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764794)-Methode zu implementieren, die die Bilddaten tatsächlich verarbeitet.

Unter Verwendung von Win2D-APIs wird eine **CanvasBitmap** von der [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn930920)-Eigenschaft des Eingabeframes erstellt. Ein **CanvasRenderTarget** wird von der **Direct3DSurface** des Ausgabeframes und eine **CanvasDrawingSession** von diesem Renderziel erstellt. Es wird ein neuer Win2D-**GaussianBlurEffect** initialisiert. Mithilfe der **BlurAmount**-Eigenschaft wird der Effekt über [**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986) bereitgestellt. Schließlich wird die **CanvasDrawingSession.DrawImage**-Methode aufgerufen, um die Eingabebitmap mithilfe des Weichzeichnereffekts in das Renderziel zu ziehen.

[!code-cs[ProcessFrameWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetProcessFrameWin2D)]


## Hinzufügen des benutzerdefinierten Effekts zu Ihrer App


Um den Videoeffekt aus Ihrer App zu verwenden, müssen Sie einen Verweis zum Effektprojekt zu Ihrer App hinzufügen.

1.  Klicken Sie im Projektmappen-Explorer unter Ihrem App-Projekt mit der rechten Maustaste auf „Verweise“, und wählen Sie „Verweis hinzufügen…“ aus.
2.  Erweitern Sie die Registerkarte „Projekte“, klicken Sie auf „Projektmappe“, und aktivieren Sie das Kontrollkästchen für den Projektnamen des Effekts. In diesem Beispiel heißt das Projekt „VideoEffectComponent“.
3.  Klicken Sie auf „OK“.

### Hinzufügen Ihres benutzerdefinierten Effekts zu einem Kameravideostream

Sie können einen einfachen Vorschaustream von der Kamera einrichten, indem Sie die Schritte im Artikel [Einfacher Zugriff auf die Kameravorschau](simple-camera-preview-access.md) ausführen. Wenn Sie diese Schritte ausführen, erhalten Sie ein initialisiertes [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124)-Objekt, das für den Zugriff auf den Videostream der Kamera verwendet wird.

Um Ihren benutzerdefinierten Videoeffekt einem Kamerastream hinzuzufügen, erstellen Sie zuerst ein neues [**VideoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608055)-Objekt, wobei der Namespace und der Klassenname für Ihren Effekt übergeben wird. Rufen Sie anschließend die [**AddVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn878035)-Methode des **MediaCapture**-Objekts auf, um den Effekt dem angegebenen Stream hinzuzufügen. In diesem Beispiel wird der [**MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640)-Wert verwendet, um anzugeben, dass der Effekt dem Vorschaustream hinzugefügt werden sollte. Wenn Ihre App die Videoaufnahme unterstützt, könnten Sie auch **MediaStreamType.VideoRecord** verwenden, um den Effekt zum Aufnahmestream hinzuzufügen. **AddVideoEffect** gibt ein [**IMediaExtension**](https://msdn.microsoft.com/library/windows/apps/br240985)-Objekt zurück, das Ihren benutzerdefinierten Effekt darstellt. Mit der SetProperties-Methode können Sie die Konfiguration für den Effekt festlegen.

Nachdem der Effekt hinzugefügt wurde, wird [**StartPreviewAsync** ](https://msdn.microsoft.com/library/windows/apps/br226613) aufgerufen, um den Vorschaustream zu starten.

[!code-cs[AddVideoEffectAsync](./code/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs#SnippetAddVideoEffectAsync)]



### Hinzufügen Ihres benutzerdefinierten Effekts zu einem Clip in einer Medienkomposition

Allgemeine Informationen zum Erstellen von Medienkompositionen von Videoclips finden Sie unter [Medienkompositionen und -bearbeitung](media-compositions-and-editing.md). Der folgende Codeausschnitt zeigt die Erstellung einer einfachen Medienkomposition unter Verwendung eines benutzerdefinierten Videoeffekts. Durch Aufruf von [**CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652607) wird ein [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596)-Objekt erstellt. Dabei wird eine Videodatei übergeben, die von dem Benutzer mit einer [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) ausgewählt wurde, und der Clip wird zu einer neuen [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646) hinzugefügt. Als Nächstes wird ein neues [**VideoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608055)-Objekt erstellt, wobei der Namespace und der Klassenname für Ihren Effekt an den Konstruktor übergeben wird. Schließlich wird die Effektdefinition zur [**VideoEffectDefinitions**](https://msdn.microsoft.com/library/windows/apps/dn652643)-Sammlung des **MediaClip**-Objekts hinzugefügt.


[!code-cs[AddEffectToComposition](./code/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs#SnippetAddEffectToComposition)]


## Verwandte Themen


[Einfacher Zugriff auf die Kameravorschau](simple-camera-preview-access.md)
            
          
            [Medienkompositionen und -bearbeitung](media-compositions-and-editing.md)
            
          
            [Win2D-Dokumentation](http://go.microsoft.com/fwlink/?LinkId=519078)
 

 





<!--HONumber=May16_HO2-->


