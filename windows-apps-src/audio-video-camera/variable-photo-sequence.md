---
ms.assetid: 7DBEE5E2-C3EC-4305-823D-9095C761A1CD
description: In diesem Artikel wird erläutert, wie Sie eine variable Fotosequenz aufnehmen, um mehrere Frames von Bildern in schneller Folge aufzunehmen und jeden Frame so zu konfigurieren, dass unterschiedliche Einstellungen für Fokus, Blitz, ISO, Belichtung und Belichtungskorrektur verwendet werden.
title: Variable Fotosequenz
---

# Variable Fotosequenz

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


In diesem Artikel wird erläutert, wie Sie eine variable Fotosequenz aufnehmen, um mehrere Frames von Bildern in schneller Folge aufzunehmen und jeden Frame so zu konfigurieren, dass unterschiedliche Einstellungen für Fokus, Blitz, ISO, Belichtung und Belichtungskorrektur verwendet werden. Mit diesem Feature können Sie z. B. HDR-Bilder (High Dynamic Range) erstellen.

Wenn Sie HDR-Bilder aufnehmen, aber nicht Ihre eigenen Algorithmen zur Bildverarbeitung implementieren möchten, können Sie mithilfe der [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) API die in Windows integrierten HDR-Funktionen verwenden. Weitere Informationen finden Sie unter [HDR-Fotoaufnahmen (High Dynamic Range)](high-dynamic-range-hdr-photo-capture.md).

**Hinweis**  
Dieser Artikel baut auf Konzepten und Codebeispielen auf, die unter [Aufnehmen von Fotos und Videos mit MediaCapture](capture-photos-and-video-with-mediacapture.md) erläutert werden. Dort werden die Schritte für die Implementierung einer grundlegenden Foto- und Videoaufnahme beschrieben. Es wird empfohlen, dass Sie sich mit dem grundlegenden Muster für die Medienerfassung in diesem Artikel vertraut machen, bevor Sie in fortgeschrittene Aufnahmeszenarien einsteigen. Bei dem Code in diesem Artikel wird davon ausgegangen, dass Ihre App bereits eine Instanz von MediaCapture aufweist, die ordnungsgemäß initialisiert wurde.

## Einrichten Ihrer App mithilfe der Aufnahme einer variablen Fotosequenz

Neben den Namespaces für die grundlegende Medienerfassung sind für die Implementierung der Aufnahme einer variablen Fotosequenz folgende Namespaces erforderlich.

[!code-cs[VPSUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSUsing)]

Deklarieren Sie eine Membervariable, um das [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564)-Objekt zu speichern, das verwendet wird, um die Aufnahme der Fotosequenz zu initiieren. Deklarieren Sie ein Array von [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358)-Objekten, um die einzelnen aufgenommenen Bilder in der Sequenz zu speichern. Deklarieren Sie außerdem ein Array, um das [**CapturedFrameControlValues**](https://msdn.microsoft.com/library/windows/apps/dn608020)-Objekt für jeden Frame zu speichern. Dazu können Sie Ihren Algorithmus zur Bildverarbeitung verwenden, um zu ermitteln, welche Einstellungen zum Aufnehmen der einzelnen Frames verwendet wurden. Abschließend deklarieren Sie einen Index, mit dem Sie nachverfolgen können, welches Bild in der Sequenz gerade aufgenommen wird.

[!code-cs[VPSMemberVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSMemberVariables)]

## Vorbereiten der Aufnahme einer variablen Fotosequenz

Nachdem Sie [MediaCapture](capture-photos-and-video-with-mediacapture.md) initialisiert haben, müssen Sie sicherstellen, dass die variablen Fotosequenzen auf dem aktuellen Gerät unterstützt werden, indem Sie eine Instanz von [**VariablePhotoSequenceController**](https://msdn.microsoft.com/library/windows/apps/dn640573) aus dem [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) der Medienerfassung abrufen und die [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn640580)-Eigenschaft überprüfen.

[!code-cs[IsVPSSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsVPSSupported)]

Rufen Sie ein [**FrameControlCapabilities**](https://msdn.microsoft.com/library/windows/apps/dn652548)-Objekt von dem Controller der variablen Fotosequenz ab. Dieses Objekt besitzt eine Eigenschaft für jede Einstellung, die pro Frame einer Fotosequenz konfiguriert werden kann. Dazu zählen:

-   [**Exposure**](https://msdn.microsoft.com/library/windows/apps/dn652552)
-   [**ExposureCompensation**](https://msdn.microsoft.com/library/windows/apps/dn652560)
-   [**Flash**](https://msdn.microsoft.com/library/windows/apps/dn652566)
-   [**Focus**](https://msdn.microsoft.com/library/windows/apps/dn652570)
-   [**IsoSpeed**](https://msdn.microsoft.com/library/windows/apps/dn652574)
-   [**PhotoConfirmation**](https://msdn.microsoft.com/library/windows/apps/dn652578)

In diesem Beispiel wird ein anderer Belichtungskorrekturwert für jeden Frame festgelegt. Um sicherzustellen, dass die Belichtungskorrektur für die Fotosequenzen auf dem aktuellen Gerät unterstützt wird, überprüfen Sie die [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn278905)-Eigenschaft des [**FrameExposureCompensationCapabilities**](https://msdn.microsoft.com/library/windows/apps/dn652628)-Objekts, auf das Sie über die **ExposureCompensation**-Eigenschaft zugreifen.

[!code-cs[IsExposureCompensationSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsExposureCompensationSupported)]

Erstellen Sie ein neues [**FrameController**](https://msdn.microsoft.com/library/windows/apps/dn652582)-Objekt für die einzelnen Frames, die Sie aufnehmen möchten. In diesem Beispiel werden drei Frames aufgenommen. Legen Sie die Werte für die Steuerelemente fest, die Sie für jeden Frame variieren möchten. Löschen Sie dann die [**DesiredFrameControllers**](https://msdn.microsoft.com/library/windows/apps/dn640574)-Auflistung von **VariablePhotoSequenceController**, und fügen Sie der Auflistung die einzelnen Framecontroller hinzu.

[!code-cs[InitFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitFrameControllers)]

Erstellen Sie ein [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993)-Objekt, um die Codierung festzulegen, die Sie für die aufgenommenen Bilder verwenden möchten. Rufen Sie die statische Methode [**MediaCapture.PrepareVariablePhotoSequenceCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/dn608097) auf, und übergeben Sie die Codierungseigenschaften. Diese Methode gibt ein [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564)-Objekt zurück. Registrieren Sie abschließend Ereignishandler für das [**PhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/dn652573)-Ereignis und das [**Stopped**](https://msdn.microsoft.com/library/windows/apps/dn652585)-Ereignis.

[!code-cs[PrepareVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPrepareVPS)]

## Starten der Aufnahme einer variablen Fotosequenz

Rufen Sie zum Starten der Aufnahme der variablen Fotosequenz [**VariablePhotoSequenceCapture.StartAsync**](https://msdn.microsoft.com/library/windows/apps/dn652577) auf. Achten Sie darauf, dass Sie Arrays zum Speichern der aufgenommenen Bilder und Werte des Frame-Steuerelements initialisieren und den aktuellen Index auf 0 festlegen. Legen Sie für Ihre App die Aufnahmezustandsvariable fest, und aktualisieren Sie Ihre Benutzeroberfläche, um das Starten einer weiteren Aufnahme zu deaktivieren, während diese Aufnahme läuft.

[!code-cs[StartVPSCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartVPSCapture)]

## Empfangen der aufgenommenen Frames

Das [**PhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/dn652573)-Ereignis wird für jeden aufgenommenen Frame ausgelöst. Speichern Sie die Werte des Frame-Steuerelements und das aufgenommene Bild für den Frame, und erhöhen Sie dann den aktuellen Frameindex. Dieses Beispiel zeigt, wie Sie eine [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358)-Darstellung der einzelnen Frames abrufen können. Weitere Informationen zur Verwendung von **SoftwareBitmap** finden Sie unter [Bildverarbeitung](imaging.md).

[!code-cs[OnPhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnPhotoCaptured)]

## Abschluss der Aufnahme der variablen Fotosequenz

Das [**Stopped**](https://msdn.microsoft.com/library/windows/apps/dn652585)-Ereignis wird ausgelöst, nachdem alle Frames in der Sequenz aufgenommen wurden. Aktualisieren Sie den Aufnahmestatus Ihrer App und Ihre Benutzeroberfläche, damit Benutzer neue Aufzeichnungen initiieren können. An dieser Stelle können Sie die aufgenommenen Bilder und Werte des Frame-Steuerelements an Ihren Bildverarbeitungscode übergeben.

[!code-cs[OnStopped](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnStopped)]

## Aktualisieren von Framecontrollern

Wenn Sie eine weitere Aufnahme einer variablen Fotosequenz mit anderen Einstellungen pro Frame durchführen möchten, müssen Sie **VariablePhotoSequenceCapture** nicht komplett neu initialisieren. Löschen Sie entweder die [**DesiredFrameControllers**](https://msdn.microsoft.com/library/windows/apps/dn640574)-Auflistung, und fügen Sie neue Framecontroller hinzu, oder ändern Sie die vorhandenen Framecontrollerwerte. Im folgenden Beispiel wird das [**FrameFlashCapabilities**](https://msdn.microsoft.com/library/windows/apps/dn652657)-Objekt überprüft, um sicherzustellen, dass das aktuelle Gerät Blitz und Blitzleistung für Frames variabler Fotosequenzen unterstützt. In diesem Fall wird jeder Frame aktualisiert, um den Blitz bei 100 % Energieverbrauch zu aktivieren. Die Belichtungskorrekturwerte, die zuvor für jeden Frame festgelegt wurden, sind weiterhin aktiv.

[!code-cs[UpdateFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateFrameControllers)]

## Bereinigen der Aufnahme einer variablen Fotosequenz

Bereinigen Sie nach der Aufnahme variabler Fotosequenzen oder nach dem Anhalten Ihrer App das entsprechende Objekt, indem Sie [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/dn652569) aufrufen. Heben Sie die Registrierung der Ereignishandler für das Objekt auf, und legen Sie es auf NULL fest.

[!code-cs[CleanUpVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpVPS)]

## Verwandte Themen

* [Aufnehmen von Fotos und Videos mit „MediaCapture“](capture-photos-and-video-with-mediacapture.md)
 

 






<!--HONumber=Mar16_HO1-->


