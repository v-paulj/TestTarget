---
author: drewbatgit
ms.assetid: 831123A7-1F40-4B74-AE9F-69AC9883B4AD
description: "In diesem Artikel wird veranschaulicht, wie Sie manuelle Gerätesteuerelemente verwenden, um erweiterte Foto- und Videoaufnahmeszenarien (einschließlich optischer Bildstabilisierung und fließendem Zoom) zu ermöglichen."
title: "Manuelle Kamerasteuerelemente für Foto- und Videoaufnahmen"
translationtype: Human Translation
ms.sourcegitcommit: 4c6a7aabb39b3835e042481ccae7da60e899e7cf
ms.openlocfilehash: 13a767d8e75a64dc0e65bbfbc85f6c6cd2491f38

---

# Manuelle Kamerasteuerelemente für Foto- und Videoaufnahmen

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \].


In diesem Artikel wird veranschaulicht, wie Sie manuelle Gerätesteuerelemente verwenden, um erweiterte Foto- und Videoaufnahmeszenarien (einschließlich optischer Bildstabilisierung und fließendem Zoom) zu ermöglichen.

Die in diesem Artikel beschriebenen Steuerelemente werden Ihrer App alle mithilfe desselben Musters hinzugefügt. Überprüfen Sie zunächst, ob das Steuerelement auf dem aktuellen Gerät unterstützt wird, auf dem Ihre App ausgeführt wird. Wenn das Steuerelement unterstützt wird, legen Sie den gewünschten Modus für das Steuerelement fest. Wenn ein bestimmtes Steuerelement auf dem aktuellen Gerät nicht unterstützt wird, sollten Sie das UI-Element, über das der Benutzer das Feature aktivieren kann, deaktivieren oder ausblenden.

Der Code in diesem Artikel wurde aus dem [Camera Manual Controls SDK-Beispiel](http://go.microsoft.com/fwlink/p/?LinkId=619479) übernommen und angepasst. Sie können das Beispiel herunterladen, um den verwendeten Code im Kontext anzuzeigen oder das Beispiel als Ausgangspunkt für Ihre eigene App zu verwenden.

> [!NOTE]
> Dieser Artikel baut auf Konzepten und Codebeispielen auf, die unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md) erläutert werden. Dort werden die Schritte für die Implementierung einer grundlegenden Foto- und Videoaufnahme beschrieben. Wir empfehlen Ihnen, sich mit dem grundlegenden Medienaufnahmemuster in diesem Artikel vertraut zu machen, bevor Sie sich komplexeren Aufnahmeszenarien zuwenden. Bei dem Code in diesem Artikel wird davon ausgegangen, dass Ihre App bereits eine Instanz von MediaCapture aufweist, die ordnungsgemäß initialisiert wurde.

Alle in diesem Artikel beschriebenen Gerätesteuerelement-APIs gehören dem [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902)-Namespace an.

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

## Belichtung

Mit [**ExposureControl**](https://msdn.microsoft.com/library/windows/apps/dn278910) können Sie die Verschlusszeit für Foto- und Videoaufnahmen festlegen.

Dieses Beispiel enthält ein [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614)-Steuerelement, um den aktuellen Belichtungswert anzupassen, und ein Kontrollkästchen, um die automatische Belichtungsanpassung ein- oder auszuschalten.

[!code-xml[ExposureXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetExposureXAML)]

Überprüfen Sie anhand der [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297710)-Eigenschaft, ob das aktuelle Aufnahmegerät **ExposureControl** unterstützt. Wenn das Steuerelement unterstützt wird, können Sie die UI für dieses Feature anzeigen und aktivieren. Legen Sie den Aktivierungszustand des Kontrollkästchens für den Wert der [**Auto**](https://msdn.microsoft.com/library/windows/apps/dn278911)-Eigenschaft fest, um anzugeben, ob die automatische Belichtungsanpassung derzeit aktiv ist.

Der Belichtungswert muss innerhalb des vom Gerät unterstützten Bereichs liegen und ein inkrementeller Wert der unterstützten Schrittgröße sein. Ermitteln Sie die unterstützten Werte für das aktuelle Gerät anhand der Eigenschaften [**Min**](https://msdn.microsoft.com/library/windows/apps/dn278919), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn278914) und [**Step**](https://msdn.microsoft.com/library/windows/apps/dn278930). Sie werden verwendet, um die entsprechenden Eigenschaften des Schieberegler-Steuerelements festzulegen.

Legen Sie den Wert des Schieberegler-Steuerelements auf den aktuellen Wert von **ExposureControl** fest, nachdem Sie die Registrierung des [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737)-Ereignishandlers aufgehoben haben. Das Ereignis wird dann nicht ausgelöst, wenn der Wert festgelegt wird.

[!code-cs[ExposureControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureControl)]

Rufen Sie im **ValueChanged**-Ereignishandler den aktuellen Wert des Steuerelements ab, und legen Sie dann den Belichtungswert fest, indem Sie [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn278927) aufrufen.

[!code-cs[ExposureSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureSlider)]

Aktivieren oder deaktivieren Sie im **CheckedChanged**-Ereignishandler des Kontrollkästchens für die automatische Belichtung die automatische Belichtungsanpassung, indem Sie [**SetAutoAsync**](https://msdn.microsoft.com/library/windows/apps/dn278920) aufrufen und einen booleschen Wert übergeben.

[!code-cs[ExposureCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureCheckBox)]

> [!IMPORTANT]
> Der automatische Belichtungsmodus wird nur unterstützt, während der Vorschaudatenstrom ausgeführt wird. Vergewissern Sie sich, dass der Vorschaudatenstrom aktiv ist, bevor Sie die automatische Belichtung aktivieren.

## Belichtungskorrektur

Mit [**ExposureCompensationControl**](https://msdn.microsoft.com/library/windows/apps/dn278897) können Sie die Belichtungskorrektur für Foto- und Videoaufnahmen festlegen.

In diesem Beispiel wird ein [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614)-Steuerelement verwendet, um den aktuellen Belichtungskorrekturwert anzupassen.

[!code-xml[EvXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetEvXAML)]

Überprüfen Sie anhand der [Supported](supported-codecs.md)-Eigenschaft, ob das aktuelle Aufnahmegerät **ExposureCompensationControl** unterstützt. Wenn das Steuerelement unterstützt wird, können Sie die UI für dieses Feature anzeigen und aktivieren.

Der Belichtungskorrekturwert muss innerhalb des vom Gerät unterstützten Bereichs liegen und ein inkrementeller Wert der unterstützten Schrittgröße sein. Ermitteln Sie die unterstützten Werte für das aktuelle Gerät anhand der Eigenschaften [**Min**](https://msdn.microsoft.com/library/windows/apps/dn278901), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn278899) und [**Step**](https://msdn.microsoft.com/library/windows/apps/dn278904). Sie werden verwendet, um die entsprechenden Eigenschaften des Schieberegler-Steuerelements festzulegen.

Legen Sie den Wert des Schieberegler-Steuerelements auf den aktuellen Wert von **ExposureCompensationControl** fest, nachdem Sie die Registrierung des [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737)-Ereignishandlers aufgehoben haben. Das Ereignis wird dann nicht ausgelöst, wenn der Wert festgelegt wird.

[!code-cs[EvControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEvControl)]

Rufen Sie im **ValueChanged**-Ereignishandler den aktuellen Wert des Steuerelements ab, und legen Sie dann den Belichtungswert fest, indem Sie [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn278903) aufrufen.

[!code-cs[EvValueChanged](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEvValueChanged)]

## Blitz

Mit [**FlashControl**](https://msdn.microsoft.com/library/windows/apps/dn297725) können Sie den Blitz aktivieren und deaktivieren oder den automatischen Blitz aktivieren, um das System dynamisch entscheiden zu lassen, ob der Blitz verwendet werden soll. Dieses Steuerelement ermöglicht es auch, auf Geräten, die diese Funktion unterstützen, die Reduzierung des Rote-Augen-Effekts zu aktivieren. Alle diese Einstellungen gelten für das Aufnehmen von Fotos. [**TorchControl**](https://msdn.microsoft.com/library/windows/apps/dn279077) ist ein separates Steuerelement zum Ein- und Ausschalten der Taschenlampe bei Videoaufnahmen.

In diesem Beispiel wird eine Gruppe von Optionsfeldern verwendet, damit Benutzer den Blitz ein- und ausschalten und den automatischen Blitz aktivieren können. Außerdem ist ein Kontrollkästchen vorhanden, um die Reduzierung des Rote-Augen-Effekts und die Taschenlampenfunktion für Videos ein- oder auszuschalten.

[!code-xml[FlashXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetFlashXAML)]

Überprüfen Sie anhand der [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297837)-Eigenschaft, ob das aktuelle Aufnahmegerät **FlashControl** unterstützt. Wenn das Steuerelement unterstützt wird, können Sie die UI für dieses Feature anzeigen und aktivieren. Die Unterstützung von **FlashControl** bedeutet nicht automatisch, dass auch die Reduzierung des Rote-Augen-Effekts unterstützt wird. Prüfen Sie daher vor dem Aktivieren der UI auch die [**RedEyeReductionSupported**](https://msdn.microsoft.com/library/windows/apps/dn297766)-Eigenschaft. Da **TorchControl** nicht direkt mit dem Blitzsteuerelement zusammenhängt, müssen Sie vor der Verwendung auch dessen [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn279081)-Eigenschaft prüfen.

Aktivieren oder deaktivieren Sie die gewünschte Blitzeinstellung im [**Checked**](https://msdn.microsoft.com/library/windows/apps/br209796)-Ereignishandler für die einzelnen Blitz-Optionsfelder. Beachten Sie Folgendes: Damit der Blitz immer verwendet wird, müssen Sie die [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn297733)-Eigenschaft auf „true“ und die [**Auto**](https://msdn.microsoft.com/library/windows/apps/dn297728)-Eigenschaft auf „false“ festlegen.

[!code-cs[FlashControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFlashControl)]

[!code-cs[FlashRadioButtons](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFlashRadioButtons)]

Legen Sie im Handler des Kontrollkästchens für die Reduzierung des Rote-Augen-Effekts die [**RedEyeReduction**](https://msdn.microsoft.com/library/windows/apps/dn297758)-Eigenschaft auf den gewünschten Wert fest.

[!code-cs[RedEye](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetRedEye)]

Legen Sie schließlich im Handler des Kontrollkästchens für die Videotaschenlampe die [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn279078)-Eigenschaft auf den gewünschten Wert fest.

[!code-cs[Taschenlampe](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTorch)]

> [!NOTE] 
>  Auf einigen Geräten sendet die Taschenlampe auch dann kein Licht aus, wenn [**TorchControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn279078) auf „true“ festgelegt ist. Es sei denn, für das Gerät wird ein Vorschaudatenstrom ausgeführt, und die Videoaufnahme ist aktiv. Folgende Reihenfolge wird empfohlen: Schalten Sie die Videovorschau ein, und aktivieren Sie dann die Taschenlampe, indem Sie **Enabled** auf „true“ festlegen. Initiieren Sie anschließend die Videoaufnahme. Auf einigen Geräten wird das Licht der Taschenlampe aktiviert, nachdem die Vorschau gestartet wurde. Auf anderen Geräten kann es sein, dass die Taschenlampe erst aufleuchtet, wenn die Videoaufnahme gestartet wird.

## Fokus

Das [**FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn297788)-Objekt unterstützt drei unterschiedliche gängige Methoden zum Anpassen des Kamerafokus: fortlaufender Autofokus, Tippen zum Scharfstellen und manueller Fokus. Eine Kamera-App kann auch alle drei Methoden unterstützen. Zum besseren Verständnis werden sie in diesem Artikel aber separat beschrieben. In diesem Abschnitt wird auch beschrieben, wie Sie das Zusatzlicht für den Fokus aktivieren.

### Fortlaufender Autofokus

Wenn der fortlaufende Autofokus aktiviert wird, wird die Kamera angewiesen, den Fokus dynamisch anzupassen. So wird versucht, das Motiv des Fotos oder Videos im Fokus zu behalten. In diesem Beispiel wird ein Optionsfeld verwendet, um den fortlaufenden Autofokus ein- und auszuschalten.

[!code-xml[CAFXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetCAFXAML)]

Überprüfen Sie anhand der [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297785)-Eigenschaft, ob das aktuelle Aufnahmegerät **FocusControl** unterstützt. Prüfen Sie als Nächstes anhand der [**SupportedFocusModes**](https://msdn.microsoft.com/library/windows/apps/dn608079)-Liste, ob der fortlaufende Autofokus unterstützt wird. Wenn sie den Wert [**FocusMode.Continuous**](https://msdn.microsoft.com/library/windows/apps/dn608084) enthält, zeigen Sie das Optionsfeld für den fortlaufenden Autofokus an.

[!code-cs[CAF](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetCAF)]

Verwenden Sie im [**Checked**](https://msdn.microsoft.com/library/windows/apps/br209796)-Ereignishandler für das Optionsfeld für den fortlaufenden Autofokus die [**VideoDeviceController.FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn279091)-Eigenschaft, um eine Instanz des Steuerelements abzurufen. Rufen Sie [**UnlockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608081) auf, um das Steuerelement zu entsperren, falls Ihre App [**LockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608075) aufgerufen hat, um einen der anderen Fokusmodi zu aktivieren.

Erstellen Sie ein neues [**FocusSettings**](https://msdn.microsoft.com/library/windows/apps/dn608085)-Objekt, und legen Sie die [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn608090)-Eigenschaft auf **Continuous** fest. Legen Sie die [**AutoFocusRange**](https://msdn.microsoft.com/library/windows/apps/dn608086)-Eigenschaft auf einen Wert fest, der für Ihr App-Szenario geeignet ist oder vom Benutzer über die UI ausgewählt wird. Übergeben Sie Ihr **FocusSettings**-Objekt an die [**Configure**](https://msdn.microsoft.com/library/windows/apps/dn608067)-Methode, und rufen Sie anschließend [**FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794) auf, um den fortlaufenden Autofokus zu initiieren.

[!code-cs[CafFocusRadioButton](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetCafFocusRadioButton)]

> [!IMPORTANT]
> Der Autofokusmodus wird nur unterstützt, während der Vorschaudatenstrom ausgeführt wird. Vergewissern Sie sich, dass der Vorschaudatenstrom ausgeführt wird, bevor Sie den fortlaufenden Autofokus aktivieren.

### Tippen zum Scharfstellen

Beim Verfahren „Tippen zum Scharfstellen“ werden die Elemente [**FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn297788) und [**RegionsOfInterestControl**](https://msdn.microsoft.com/library/windows/apps/dn279064) verwendet, um einen Unterbereich des Aufnahmerahmens anzugeben, der vom Aufnahmegerät scharf gestellt werden soll. Der Fokusbereich wird bestimmt, indem der Benutzer auf den Bildschirm tippt, auf dem der Vorschaudatenstrom angezeigt wird.

In diesem Beispiel wird ein Optionsfeld verwendet, um den Modus „Tippen zum Scharfstellen“ zu aktivieren und zu deaktivieren.

[!code-xml[TapFocusXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetTapFocusXAML)]

Überprüfen Sie anhand der [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297785)-Eigenschaft, ob das aktuelle Aufnahmegerät **FocusControl** unterstützt. **RegionsOfInterestControl** muss unterstützt werden und selbst mindestens einen Bereich unterstützen, um dieses Verfahren verwenden zu können. Ermitteln Sie anhand der Eigenschaften [**AutoFocusSupported**](https://msdn.microsoft.com/library/windows/apps/dn279066) und [**MaxRegions**](https://msdn.microsoft.com/library/windows/apps/dn279069), ob das Optionsfeld für „Tippen zum Scharfstellen“ ein- oder ausgeblendet werden soll.

[!code-cs[TapFocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocus)]

Verwenden Sie im [**Checked**](https://msdn.microsoft.com/library/windows/apps/br209796)-Ereignishandler für das Optionsfeld für „Tippen zum Scharfstellen“ die [**VideoDeviceController.FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn279091)-Eigenschaft, um eine Instanz des Steuerelements abzurufen. Rufen Sie [**LockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608075) auf, um das Steuerelement zu sperren, falls die App bereits [**UnlockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608081) aufgerufen hat, um den fortlaufenden Autofokus zu aktivieren. Dann wird darauf gewartet, dass der Benutzer auf den Bildschirm tippt, um den Fokus zu ändern.

[!code-cs[TapFocusRadioButton](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocusRadioButton)]

In diesem Beispiel wird ein Bereich scharf gestellt, wenn der Benutzer auf den Bildschirm tippt, und der Vorgang wird rückgängig gemacht, wenn der Benutzer erneut auf den Bildschirm tippt. Es funktioniert also wie ein Umschalter. Verwenden Sie eine boolesche Variable, um den aktuellen Schaltzustand nachzuverfolgen.

[!code-cs[IsFocused](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsFocused)]

Im nächsten Schritt geht es um das Lauschen auf das Ereignis, wenn der Benutzer auf den Bildschirm tippt. Hierzu wird das [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985)-Ereignis von [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) behandelt, mit dem derzeit der Vorschaudatenstrom für die Aufnahme angezeigt wird. Falls die Vorschau der Kamera gerade nicht aktiv ist oder der Modus „Tippen zum Scharfstellen“ deaktiviert ist, können Sie den Handler verlassen, ohne eine Aktion auszuführen.

Wenn die Nachverfolgungsvariable *\_isFocused* auf „false“ festgelegt ist und die Kamera gerade nicht scharf stellt (was anhand der [**FocusState**](https://msdn.microsoft.com/library/windows/apps/dn608074)-Eigenschaft von **FocusControl** ermittelt wird), können Sie den Prozess „Tippen zum Scharfstellen“ starten. Ermitteln Sie anhand der an den Handler übergebenen Ereignisargumente die Position, auf die der Benutzer getippt hat. In diesem Beispiel wird diese Gelegenheit auch genutzt, um die Größe des Bereichs auszuwählen, der scharf gestellt wird. In diesem Fall beträgt die Größe ein Viertel der kleinsten Abmessung des Capture-Elements. Übergeben Sie die Tippposition und die Bereichsgröße an die im nächsten Abschnitt definierte **TapToFocus**-Hilfsmethode.

Wenn der Umschalter *\_isFocused* auf „true“ festgelegt ist, muss durch den Tippvorgang des Benutzers der Fokus für den vorherigen Bereich entfernt werden. Dies wird mit der im Anschluss gezeigten **TapUnfocus**-Hilfsmethode erreicht.

[!code-cs[TapFocusPreviewControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocusPreviewControl)]

Legen Sie in der **TapToFocus**-Hilfsmethode zuerst den Umschalter *\_isFocused* auf „true“ fest, damit der per Tippvorgang ausgewählte Fokusbereich beim nächsten Tippen auf den Bildschirm entfernt wird.

Die nächste Aufgabe besteht bei dieser Hilfsmethode darin, das Rechteck im Vorschaudatenstrom zu bestimmen, das dem Fokussteuerelement zugewiesen wird. Hierfür sind zwei Schritte erforderlich. Im ersten Schritt wird der Rechteckbereich bestimmt, der vom Vorschaudatenstrom im [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278)-Steuerelement eingenommen wird. Dies hängt von den Abmessungen des Vorschaudatenstroms und der Ausrichtung des Geräts ab. Mit der **GetPreviewStreamRectInControl**-Hilfsmethode, die am Ende dieses Abschnitts veranschaulicht wird, wird diese Aufgabe durchgeführt, und das Rechteck mit dem Vorschaudatenstrom wird zurückgegeben.

Die nächste Aufgabe in **TapToFocus** besteht darin, die Tippposition und die gewünschten Größe des Fokusrechtecks, die im **CaptureElement.Tapped**-Ereignishandler ermittelt wurden, in Koordinaten im Aufnahmedatenstrom zu konvertieren. Diese Konvertierung wird mithilfe der **ConvertUiTapToPreviewRect**-Hilfsmethode durchgeführt, auf die weiter unten in diesem Abschnitt eingegangen wird. Außerdem wird das Rechteck (mit Koordinaten des Aufnahmedatenstroms) zurückgegeben, für das der Fokus angefordert wird.

Nachdem Sie nun über das Zielrechteck verfügen, erstellen Sie ein neues [**RegionOfInterest**](https://msdn.microsoft.com/library/windows/apps/dn279058)-Objekt und legen die [**Bounds**](https://msdn.microsoft.com/library/windows/apps/dn279062)-Eigenschaft auf das Zielrechteck fest, das mit den vorherigen Schritten ermittelt wurde.

Rufen Sie das [**FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn297788)-Element des Aufnahmegeräts ab. Erstellen Sie ein neues [**FocusSettings**](https://msdn.microsoft.com/library/windows/apps/dn608085)-Objekt, und legen Sie die Elemente [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn608076) und [**AutoFocusRange**](https://msdn.microsoft.com/library/windows/apps/dn608086) auf die gewünschten Werte fest. Vergewissern Sie sich aber zuvor, dass sie von **FocusControl** unterstützt werden. Rufen Sie schließlich [**Configure**](https://msdn.microsoft.com/library/windows/apps/dn608067) für das **FocusControl**-Element auf, um die Einstellungen zu aktivieren und dem Gerät zu signalisieren, dass mit dem Scharfstellen des angegebenen Bereichs begonnen werden soll.

Rufen Sie als Nächstes das [**RegionsOfInterestControl**](https://msdn.microsoft.com/library/windows/apps/dn279064)-Element des Aufnahmegeräts ab, und rufen Sie [**SetRegionsAsync**](https://msdn.microsoft.com/library/windows/apps/dn279070) auf, um den aktiven Bereich festzulegen. Auf Geräten mit entsprechender Unterstützung können mehrere gewünschte Bereiche festgelegt werden. In diesem Beispiel wird aber nur ein Bereich festgelegt.

Rufen Sie schließlich [**FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794) für **FocusControl** auf, um mit dem Scharfstellen zu beginnen.

> [!IMPORTANT]
> Beim Implementieren von „Tippen zum Scharfstellen“ ist die Reihenfolge der Vorgänge wichtig. Die APIs müssen in folgender Reihenfolge aufgerufen werden:
>
> 1. [**FocusControl.Configure**](https://msdn.microsoft.com/library/windows/apps/dn608067)
> 2. [**RegionsOfInterestControl.SetRegionsAsync**](https://msdn.microsoft.com/library/windows/apps/dn279070)
> 3. [**FocusControl.FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794)

[!code-cs[TapToFocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapToFocus)]

Rufen Sie in der **TapUnfocus**-Hilfsmethode das **RegionsOfInterestControl**-Element auf, und rufen Sie [**ClearRegionsAsync**](https://msdn.microsoft.com/library/windows/apps/dn279068) auf, um den Bereich zu löschen, der mit dem Steuerelement in der **TapToFocus**-Hilfsmethode registriert wurde. Rufen Sie dann das **FocusControl**-Element ab, und rufen Sie [**FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794) auf, damit das Gerät eine erneute Scharfstellung ohne Zielbereich durchführt.

[!code-cs[TapUnfocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapUnfocus)]

Für die **GetPreviewStreamRectInControl**-Hilfsmethode werden die Auflösung des Vorschaudatenstroms und die Ausrichtung des Geräts verwendet, um das Rechteck im Vorschauelement zu bestimmen, das den Vorschaudatenstrom enthält. Letterbox-Abstandselemente, die vom Steuerelement ggf. bereitgestellt werden, werden gekürzt, um das Seitenverhältnis des Datenstroms beizubehalten. Bei dieser Methode werden Klassenmembervariablen verwendet, die im Beispielcode für einfache Medienaufnahmen unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md) definiert sind.

[!code-cs[GetPreviewStreamRectInControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetGetPreviewStreamRectInControl)]

Die **ConvertUiTapToPreviewRect**-Hilfsmethode verwendet als Argumente die Position des Tippereignisses, die gewünschte Größe des Fokusbereichs und das Rechteck mit dem Vorschaudatenstrom, das über die **GetPreviewStreamRectInControl**-Hilfsmethode abgerufen wird. Diese Methode verwendet diese Werte und die aktuelle Ausrichtung des Geräts, um das Rechteck im Vorschaudatenstrom zu berechnen, das den gewünschten Bereich enthält. Auch bei dieser Methode werden wieder Klassenmembervariablen verwendet, die im Beispielcode für einfache Medienaufnahmen unter [Aufnehmen von Fotos und Videos mit MediaCapture](capture-photos-and-video-with-mediacapture.md) definiert sind.

[!code-cs[ConvertUiTapToPreviewRect](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetConvertUiTapToPreviewRect)]

### Manueller Fokus

Für das Verfahren „Manueller Fokus“ wird ein **Slider**-Steuerelement verwendet, um die aktuelle Fokustiefe des Aufnahmegeräts festzulegen. Ein Optionsfeld dient zum Ein- und Ausschalten des manuellen Fokus.

[!code-xml[ManualFocusXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetManualFocusXAML)]

Überprüfen Sie anhand der [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297837)-Eigenschaft, ob das aktuelle Aufnahmegerät **FocusControl** unterstützt. Wenn das Steuerelement unterstützt wird, können Sie die UI für dieses Feature anzeigen und aktivieren.

Der Fokuswert muss innerhalb des vom Gerät unterstützten Bereichs liegen und ein inkrementeller Wert der unterstützten Schrittgröße sein. Ermitteln Sie die unterstützten Werte für das aktuelle Gerät anhand der Eigenschaften [**Min**](https://msdn.microsoft.com/library/windows/apps/dn297808), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn297802) und [**Step**](https://msdn.microsoft.com/library/windows/apps/dn297833). Sie werden verwendet, um die entsprechenden Eigenschaften des Schieberegler-Steuerelements festzulegen.

Legen Sie den Wert des Schieberegler-Steuerelements auf den aktuellen Wert von **FocusControl** fest, nachdem Sie die Registrierung des [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737)-Ereignishandlers aufgehoben haben. Das Ereignis wird dann nicht ausgelöst, wenn der Wert festgelegt wird.

[!code-cs[Fokus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocus)]

Rufen Sie im **Checked**-Ereignishandler für das Optionsfeld für den manuellen Fokus das **FocusControl**-Objekt ab, und rufen Sie [**LockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608075) auf, falls Ihre App den Fokus durch einen Aufruf von [**UnlockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608081) entsperrt hat.

[!code-cs[ManualFocusChecked](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetManualFocusChecked)]

Rufen Sie im **ValueChanged**-Ereignishandler des Schiebereglers für den manuellen Fokus den aktuellen Wert des Steuerelements ab, und legen Sie den Fokuswert fest, indem Sie [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn297828) aufrufen.

[!code-cs[FocusSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusSlider)]

### Aktivieren des Fokuslichts

Auf Geräten mit entsprechender Unterstützung können Sie ein Hilfslicht aktivieren, um die Scharfstellung des Geräts zu unterstützen. In diesem Beispiel wird ein Kontrollkästchen verwendet, um das Fokushilfslicht zu aktivieren und zu deaktivieren.

[!code-xml[FocusLightXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetFocusLightXAML)]

Überprüfen Sie anhand der [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297785)-Eigenschaft, ob das aktuelle Aufnahmegerät **FlashControl** unterstützt. Überprüfen Sie auch [**AssistantLightSupported**](https://msdn.microsoft.com/library/windows/apps/dn608066), um sich zu vergewissern, dass das Hilfslicht ebenfalls unterstützt wird. Wenn beides unterstützt wird, können Sie die UI für dieses Feature anzeigen und aktivieren.

[!code-cs[FocusLight](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusLight)]

Rufen Sie im **CheckedChanged**-Ereignishandler das [**FlashControl**](https://msdn.microsoft.com/library/windows/apps/dn297725)-Objekt des Aufnahmegeräts ab. Legen Sie die [**AssistantLightEnabled**](https://msdn.microsoft.com/library/windows/apps/dn608065)-Eigenschaft fest, um das Fokuslicht zu aktivieren oder zu deaktivieren.

[!code-cs[FocusLightCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusLightCheckBox)]

## ISO-Geschwindigkeit

Mit [**IsoSpeedControl**](https://msdn.microsoft.com/library/windows/apps/dn297850) können Sie die ISO-Geschwindigkeit für Foto- und Videoaufnahmen festlegen.

Dieses Beispiel enthält ein [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614)-Steuerelement, um den aktuellen Belichtungskorrekturwert anzupassen, und ein Kontrollkästchen, um die automatische Anpassung der ISO-Geschwindigkeit ein- oder auszuschalten.

[!code-xml[IsoXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetIsoXAML)]

Überprüfen Sie anhand der [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297869)-Eigenschaft, ob das aktuelle Aufnahmegerät **IsoSpeedControl** unterstützt. Wenn das Steuerelement unterstützt wird, können Sie die UI für dieses Feature anzeigen und aktivieren. Legen Sie den Aktivierungszustand des Kontrollkästchens für den Wert der [**Auto**](https://msdn.microsoft.com/library/windows/apps/dn608093)-Eigenschaft fest, um anzugeben, ob die automatische Anpassung der ISO-Geschwindigkeit derzeit aktiv ist.

Der Wert für die ISO-Geschwindigkeit muss innerhalb des vom Gerät unterstützten Bereichs liegen und ein inkrementeller Wert der unterstützten Schrittgröße sein. Ermitteln Sie die unterstützten Werte für das aktuelle Gerät anhand der Eigenschaften [**Min**](https://msdn.microsoft.com/library/windows/apps/dn608095), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn608094) und [**Step**](https://msdn.microsoft.com/library/windows/apps/dn608129). Sie werden verwendet, um die entsprechenden Eigenschaften des Schieberegler-Steuerelements festzulegen.

Legen Sie den Wert des Schieberegler-Steuerelements auf den aktuellen Wert von **IsoSpeedControl** fest, nachdem Sie die Registrierung des [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737)-Ereignishandlers aufgehoben haben. Das Ereignis wird dann nicht ausgelöst, wenn der Wert festgelegt wird.

[!code-cs[IsoControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoControl)]

Rufen Sie im **ValueChanged**-Ereignishandler den aktuellen Wert des Steuerelements ab, und legen Sie dann den ISO-Geschwindigkeitswert fest, indem Sie [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn608128) aufrufen.

[!code-cs[IsoSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoSlider)]

Aktivieren Sie im **CheckedChanged**-Ereignishandler des Kontrollkästchens für die automatische ISO-Geschwindigkeit die automatische Anpassung der ISO-Geschwindigkeit, indem Sie [**SetAutoAsync**](https://msdn.microsoft.com/library/windows/apps/dn608127) aufrufen. Deaktivieren Sie die automatische Anpassung der ISO-Geschwindigkeit, indem Sie [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn608128) aufrufen und den aktuellen Wert des Schieberegler-Steuerelements übergeben.

[!code-cs[IsoCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoCheckBox)]

## Optische Bildstabilisierung

Die optische Bildstabilisierung (Optical Image Stabilization, OIS) stabilisiert einen aufgenommenen Videodatenstrom mittels mechanischer Manipulation des Hardwareaufnahmegeräts. Dies ermöglicht ein besseres Ergebnis als bei der digitalen Stabilisierung. Auf Geräten, die OIS nicht unterstützen, können Sie den VideoStabilizationEffect zur digitalen Stabilisierung des aufgenommenen Videos verwenden. Weitere Informationen finden Sie unter [Effekte für die Videoaufnahme](effects-for-video-capture.md).

Überprüfen Sie anhand der [**OpticalImageStabilizationControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926689)-Eigenschaft, ob OIS auf dem aktuellen Gerät unterstützt wird.

Das OIS-Steuerelement unterstützt drei Modi: „Ein“, „Aus“ und „Automatisch“. Im automatischen Modus ermittelt das Gerät dynamisch, ob OIS die Medienaufnahme verbessern würde, und aktiviert OIS, wenn dies der Fall ist. Überprüfen Sie, ob die [**OpticalImageStabilizationControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926690)-Sammlung den gewünschten Modus enthält, um zu ermitteln, ob auf einem Gerät ein bestimmter Modus unterstützt wird.

Legen Sie [**OpticalImageStabilizationControl.Mode**](https://msdn.microsoft.com/library/windows/apps/dn926691) auf den gewünschten Modus fest, um OIS zu aktivieren oder zu deaktivieren.

[!code-cs[SetOpticalImageStabilizationMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetOpticalImageStabilizationMode)]

## Leitungsfrequenz
Einige Kamerageräte unterstützen die Anti-Flacker-Verarbeitung. Hierfür muss die Wechselstromfrequenz der Stromleitungen in der derzeitigen Umgebung bekannt sein. Einige Geräte unterstützen die automatische Ermittlung der Leitungsfrequenz, und bei anderen Geräten muss die Frequenz manuell festgelegt werden. Im folgenden Codebeispiel wird veranschaulicht, wie Sie die Unterstützung der Leitungsfrequenz für das Gerät ermitteln und, falls erforderlich, die Frequenz manuell festlegen. 

Rufen Sie zuerst die **VideoDeviceController**-Methode auf [**TryGetPowerlineFrequency**](https://msdn.microsoft.com/library/windows/apps/br206898), indem Sie einen Ausgabeparameter vom Typ [**PowerlineFrequency**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.PowerlineFrequency) übergeben. Wenn dieser Aufruf nicht erfolgreich ist, wird die Steuerung der Leitungsfrequenz auf dem aktuellen Gerät nicht unterstützt. Wenn die Funktion unterstützt wird, können Sie ermitteln, ob der automatische Modus auf dem Gerät verfügbar ist, indem Sie versuchen, den automatischen Modus festzulegen. Rufen Sie hierzu [**TrySetPowerlineFrequency**](https://msdn.microsoft.com/library/windows/apps/br206899) auf, und übergeben Sie den Wert **Auto**. Wenn der Aufruf erfolgreich ist, bedeutet dies, dass die automatische Leitungsfrequenz unterstützt wird. Wenn die Steuerung der Leitungsfrequenz auf dem Gerät unterstützt wird, die automatische Frequenzerkennung aber nicht, können Sie die Frequenz trotzdem manuell mit **TrySetPowerlineFrequency** festlegen. In diesem Beispiel ist **MyCustomFrequencyLookup** eine benutzerdefinierte Methode, die Sie implementieren, um für die aktuelle Position des Geräts die richtige Frequenz zu ermitteln. 

[!code-cs[PowerlineFrequency](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetPowerlineFrequency)]

## Weißabgleich

Mit [**WhiteBalanceControl**](https://msdn.microsoft.com/library/windows/apps/dn279104) können Sie den Weißabgleich für Foto- und Videoaufnahmen festlegen.

In diesem Beispiel werden ein [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348)-Steuerelement für die Auswahl integrierter Farbtemperatureinstellungen und ein [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614)-Steuerelement für die Anpassung des manuellen Weißabgleichs verwendet.

[!code-xml[WhiteBalanceXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetWhiteBalanceXAML)]

Überprüfen Sie anhand der [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn279120)-Eigenschaft, ob das aktuelle Aufnahmegerät **WhiteBalanceControl** unterstützt. Wenn das Steuerelement unterstützt wird, können Sie die UI für dieses Feature anzeigen und aktivieren. Legen Sie die Elemente des Kombinationsfelds auf die Werte der [**ColorTemperaturePreset**](https://msdn.microsoft.com/library/windows/apps/dn278894)-Enumeration fest. Legen Sie außerdem das ausgewählte Element auf den aktuellen Wert der [**Preset**](https://msdn.microsoft.com/library/windows/apps/dn279110)-Eigenschaft fest.

Für die manuelle Steuerung muss der Weißabgleichwert innerhalb des Bereichs liegen, der vom Gerät unterstützt wird, und es muss ein inkrementeller Wert der unterstützten Schrittgröße sein. Ermitteln Sie die unterstützten Werte für das aktuelle Gerät anhand der Eigenschaften [**Min**](https://msdn.microsoft.com/library/windows/apps/dn279109), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn279107) und [**Step**](https://msdn.microsoft.com/library/windows/apps/dn279119). Sie werden verwendet, um die entsprechenden Eigenschaften des Schieberegler-Steuerelements festzulegen. Stellen Sie vor dem Aktivieren der manuellen Steuerung sicher, dass der Bereich zwischen den kleinsten und größten unterstützten Werten größer als die Schrittgröße ist. Andernfalls wird die manuelle Steuerung auf dem aktuellen Gerät nicht unterstützt.

Legen Sie den Wert des Schieberegler-Steuerelements auf den aktuellen Wert von **WhiteBalanceControl** fest, nachdem Sie die Registrierung des [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737)-Ereignishandlers aufgehoben haben. Das Ereignis wird dann nicht ausgelöst, wenn der Wert festgelegt wird.

[!code-cs[WhiteBalance](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalance)]

Rufen Sie im [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776)-Ereignishandler des Kombinationsfelds mit den Farbtemperatureinstellungen die derzeit ausgewählte Voreinstellung ab, und legen Sie den Wert des Steuerelements fest, indem Sie [**SetPresetAsync**](https://msdn.microsoft.com/library/windows/apps/dn279113) aufrufen. Deaktivieren Sie den Schieberegler für den manuellen Weißabgleich, wenn der ausgewählte Voreinstellungswert nicht **Manual** lautet.

[!code-cs[WhiteBalanceComboBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalanceComboBox)]

Rufen Sie im **ValueChanged**-Ereignishandler den aktuellen Wert des Steuerelements ab, und legen Sie dann den Weißabgleichwert fest, indem Sie [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn278927) aufrufen.

[!code-cs[WhiteBalanceSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalanceSlider)]

> [!IMPORTANT]
> Das Anpassen des Weißabgleichs wird nur unterstützt, während der Vorschaudatenstrom ausgeführt wird. Vergewissern Sie sich, dass der Vorschaudatenstrom ausgeführt wird, bevor Sie den Wert oder die Voreinstellung für den Weißabgleich festlegen.

> [!IMPORTANT]
> Mit dem **ColorTemperaturePreset.Auto**-Voreinstellungswert wird das System angewiesen, die Einstellung des Weißabgleichs automatisch anzupassen. Für einige Szenarien (etwa beim Aufnehmen einer Bildserie, bei der die Weißabgleicheinstellungen für jedes Bild gleich sein sollen) kann es ratsam sein, das Steuerelement mit dem aktuellen automatischen Wert zu sperren. Rufen Sie hierzu [**SetPresetAsync**](https://msdn.microsoft.com/library/windows/apps/dn279113) auf, und geben Sie die Voreinstellung **Manual** an. Legen Sie für das Steuerelement keinen Wert mit [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn279114) fest. Das Gerät wird dann mit dem aktuellen Wert gesperrt. Versuchen Sie nicht, den Wert des aktuellen Steuerelements auszulesen. Übergeben Sie den zurückgegebenen Wert anschließend an **SetValueAsync**, da nicht garantiert ist, dass dieser Wert stimmt.

## Zoom

Mit [**ZoomControl**](https://msdn.microsoft.com/library/windows/apps/dn608149) können Sie den Zoomfaktor für Foto- und Videoaufnahmen festlegen.

In diesem Beispiel wird ein [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614)-Steuerelement verwendet, um den aktuellen Zoomfaktor anzupassen. Im folgenden Abschnitt wird veranschaulicht, wie Sie den Zoom basierend auf einer Zusammendrückbewegung auf dem Bildschirm anpassen.

[!code-xml[ZoomXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetZoomXAML)]

Überprüfen Sie anhand der [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn633819)-Eigenschaft, ob das aktuelle Aufnahmegerät **ZoomControl** unterstützt. Wenn das Steuerelement unterstützt wird, können Sie die UI für dieses Feature anzeigen und aktivieren.

Der Zoomfaktorwert muss innerhalb des vom Gerät unterstützten Bereichs liegen und ein inkrementeller Wert der unterstützten Schrittgröße sein. Ermitteln Sie die unterstützten Werte für das aktuelle Gerät anhand der Eigenschaften [**Min**](https://msdn.microsoft.com/library/windows/apps/dn633817), [**Max**](https://msdn.microsoft.com/library/windows/apps/dn608150) und [**Step**](https://msdn.microsoft.com/library/windows/apps/dn633818). Sie werden verwendet, um die entsprechenden Eigenschaften des Schieberegler-Steuerelements festzulegen.

Legen Sie den Wert des Schieberegler-Steuerelements auf den aktuellen Wert von **ZoomControl** fest, nachdem Sie die Registrierung des [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737)-Ereignishandlers aufgehoben haben. Das Ereignis wird dann nicht ausgelöst, wenn der Wert festgelegt wird.

[!code-cs[ZoomControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetZoomControl)]

Erstellen Sie im **ValueChanged**-Ereignishandler eine neue Instanz der [**ZoomSettings**](https://msdn.microsoft.com/library/windows/apps/dn926722)-Klasse, indem Sie die [**Value**](https://msdn.microsoft.com/library/windows/apps/dn926724)-Eigenschaft auf den aktuellen Wert des Zoomschiebereglers festlegen. Wenn die [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926721)-Eigenschaft von **ZoomControl** das [**ZoomTransitionMode.Smooth**](https://msdn.microsoft.com/library/windows/apps/dn926726)-Element enthält, bedeutet das, dass das Gerät sanfte Übergänge zwischen Zoomfaktoren (Smooth Zoom) unterstützt. Da dieser Modus grundsätzlich vorzuziehen ist, empfiehlt es sich, diesen Wert für die [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn926723)-Eigenschaft des **ZoomSettings**-Objekts zu verwenden.

Ändern Sie schließlich die aktuellen Zoomeinstellungen, indem Sie Ihr **ZoomSettings**-Objekt an die [**Configure**](https://msdn.microsoft.com/library/windows/apps/dn926719)-Methode des **ZoomControl**-Objekts übergeben.

[!code-cs[ZoomSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetZoomSlider)]

### „Smooth Zoom“ per Zusammendrückbewegung

Im vorherigen Abschnitt wurde schon auf Folgendes hingewiesen: Auf Geräten mit SmoothZoom-Unterstützung ermöglicht dieser Modus einen sanften Übergang zwischen digitalen Zoomfaktoren. Benutzer können den Zoomfaktor somit während der Aufzeichnung dynamisch und ohne einzelne störende Übergänge anpassen. In diesem Abschnitt wird beschrieben, wie Sie den Zoomfaktor als Reaktion auf eine Zusammendrückbewegung anpassen.

Ermitteln Sie zunächst anhand der [**ZoomControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn633819)-Eigenschaft, ob das Steuerelement für digitalen Zoom auf dem aktuellen Gerät unterstützt wird. Ermitteln Sie dann, ob der SmoothZoom-Modus verfügbar ist, indem Sie [**ZoomControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926721) überprüfen, um festzustellen, ob darin der Wert [**ZoomTransitionMode.Smooth**](https://msdn.microsoft.com/library/windows/apps/dn926726) enthalten ist.

[!code-cs[IsSmoothZoomSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsSmoothZoomSupported)]

Auf einem Gerät mit Mehrfingereingabe wird der Zoomfaktor üblicherweise durch eine Zusammendrückbewegung mit zwei Fingern angepasst. Legen Sie die [**ManipulationMode**](https://msdn.microsoft.com/library/windows/apps/br208948)-Eigenschaft des [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278)-Steuerelements auf [**ManipulationModes.Scale**](https://msdn.microsoft.com/library/windows/apps/br227934) fest, um die Zusammendrückbewegung zu aktivieren. Nehmen Sie dann eine Registrierung für das [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946)-Ereignis vor, das ausgelöst wird, wenn sich die Größe der Zusammendrückbewegung ändert.

[!code-cs[RegisterPinchGestureHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterPinchGestureHandler)]

Aktualisieren Sie den Zoomfaktor im Handler für das **ManipulationDelta**-Ereignis basierend auf der Änderung der Zusammendrückbewegung. Der [**ManipulationDelta.Scale**](https://msdn.microsoft.com/library/windows/apps/br242016)-Wert stellt die Änderung der Skalierung der Zusammendrückbewegung wie folgt dar: Eine geringfügige Vergrößerung der Zusammendrückbewegung wird durch eine Zahl dargestellt, die etwas größer als1 ist. Eine geringfügige Verkleinerung wird durch eine Zahl dargestellt, die etwas kleiner als1 ist. In diesem Beispiel wird der aktuelle Wert des Zoom-Steuerelements mit dem Skalierungsdelta multipliziert.

Bevor Sie den Zoomfaktor festlegen, müssen Sie sich vergewissern, dass der Wert nicht kleiner als der vom Gerät unterstützte Mindestwert (angegeben durch die [**ZoomControl.Min**](https://msdn.microsoft.com/library/windows/apps/dn633817)-Eigenschaft) ist. Vergewissern Sie sich außerdem, dass der Wert maximal dem [**ZoomControl.Max**](https://msdn.microsoft.com/library/windows/apps/dn608150)-Wert entspricht. Und schließlich müssen Sie sich vergewissern, dass der Zoomfaktor ein Vielfaches der vom Gerät unterstützten Zoomschrittgröße (angegeben durch die [**Step**](https://msdn.microsoft.com/library/windows/apps/dn633818)-Eigenschaft) ist. Falls der Zoomfaktor diese Anforderungen nicht erfüllt, wird eine Ausnahme ausgelöst, wenn Sie versuchen, den Zoomfaktor für das Aufnahmegerät festzulegen.

Legen Sie den Zoomfaktor auf dem Aufnahmegerät fest, indem Sie ein neues [**ZoomSettings**](https://msdn.microsoft.com/library/windows/apps/dn926722)-Objekt erstellen. Legen Sie die [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn926723)-Eigenschaft auf [**ZoomTransitionMode.Smooth**](https://msdn.microsoft.com/library/windows/apps/dn926726) fest, und legen Sie dann die [**Value**](https://msdn.microsoft.com/library/windows/apps/dn926724)-Eigenschaft auf den gewünschten Zoomfaktor fest. Rufen Sie schließlich [**ZoomControl.Configure**](https://msdn.microsoft.com/library/windows/apps/dn926719) auf, um den neuen Zoomwert auf dem Gerät festzulegen. Es erfolgt ein sanfter Übergang zum neuen Zoomwert.

[!code-cs[ManipulationDelta](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

## Verwandte Themen

* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)



<!--HONumber=Aug16_HO3-->


