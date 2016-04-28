---
ms.assetid: E0189423-1DF3-4052-AB2E-846EA18254C4
description: In diesem Thema werden die Effekte für Videoaufnahmeszenarien beschrieben. Dazu zählt auch der Videostabilisierungseffekt.
title: Effekte für die Videoaufnahme
---

# Effekte für die Videoaufnahme

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

In diesem Thema werden die Effekte für Videoaufnahmeszenarien beschrieben. Dazu zählt auch der Videostabilisierungseffekt.

**Hinweis**  
Dieser Artikel baut auf Konzepten und Codebeispielen auf, die unter [Aufnehmen von Fotos und Videos mit MediaCapture](capture-photos-and-video-with-mediacapture.md) erläutert werden. Dort werden die Schritte für die Implementierung einer grundlegenden Foto- und Videoaufnahme beschrieben. Es wird empfohlen, dass Sie sich mit dem grundlegenden Muster für die Medienaufnahme in diesem Artikel vertraut machen, bevor Sie in fortgeschrittene Aufnahmeszenarien einsteigen. Bei dem Code in diesem Artikel wird davon ausgegangen, dass Ihre App bereits eine Instanz von MediaCapture aufweist, die ordnungsgemäß initialisiert wurde.

## Videostabilisierungseffekt

Der Videostabilisierungseffekt ändert die Frames eines Videostreams, um das Wackeln durch Halten des Aufnahmegeräts in der Hand zu minimieren. Da durch diese Technik Pixel nach rechts, links, oben und unten verschoben werden und der Effekt den Inhalt außerhalb des Videoframes nicht kennen kann, wird das stabilisierte Video im Vergleich zum Originalvideo leicht zugeschnitten. Eine Hilfsfunktion wird bereitgestellt, damit Sie Ihre Videocodierungseinstellungen zum optimalen Verwalten der durch den Effekt vorgenommenen Zuschneidung anpassen können.

Auf Geräten, die dies unterstützen, stabilisiert OIS (Optical Image Stabilization) das Video durch mechanisches Manipulieren das Aufnahmegeräts, sodass die Kanten der Videoframes nicht zugeschnitten werden müssen. Weitere Informationen finden Sie unter [Steuerelemente des Aufnahmegeräts für Videoaufnahmen](capture-device-controls-for-video-capture.md).

### Einrichten der App für die Verwendung der Videostabilisierung

Neben den Namespaces, die für die grundlegende Medienaufzeichnung erforderlich sind, erfordert die Verwendung des Videostabilisierungseffekts den folgenden Namespace.

[!code-cs[VideoStabilizationEffectUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoStabilizationEffectUsing)]

Deklarieren Sie eine Membervariable zum Speichern des [**VideoStabilizationEffect**](https://msdn.microsoft.com/library/windows/apps/dn926760)-Objekts. Im Rahmen der Effektimplementierung ändern Sie die Codierungseigenschaften, die Sie für das Codieren des aufgezeichneten Videos verwenden. Deklarieren Sie zwei Variablen zum Speichern einer Sicherungskopie der ursprünglichen Eingabe- und Ausgabecodierungseigenschaften, sodass Sie diese später wiederherstellen können, wenn der Effekt deaktiviert ist. Deklarieren Sie abschließend eine Membervariable vom Typ [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026), da auf dieses Objekt von mehreren Positionen innerhalb des Codes aus zugegriffen wird.

[!code-cs[DeclareVideoStabilizationEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareVideoStabilizationEffect)]

In der grundlegenden Implementierung der Videoaufnahme, die im Artikel [Aufnehmen von Fotos und Videos mit MediaCapture](capture-photos-and-video-with-mediacapture.md) beschrieben wird, wird das Mediencodierungsprofil-Objekt einer lokalen Variablen zugewiesen, da es an keiner anderen Stelle im Code verwendet wird. Für dieses Szenario sollten Sie das Objekt einer Membervariablen zuweisen, damit Sie später darauf zugreifen können.

[!code-cs[EncodingProfileMember](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEncodingProfileMember)]

### Initialisieren des Videostabilisierungseffekts

Erstellen Sie nach dem Initialisieren des **MediaCapture**-Objekts eine neue Instanz des [**VideoStabilizationEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn926762)-Objekts. Rufen Sie [**MediaCapture.AddVideoEffectAsync**](https://msdn.microsoft.com/library/windows/apps/dn878035) auf, um den Effekt der Videopipeline hinzuzufügen und eine Instanz der [**VideoStabilizationEffect**](https://msdn.microsoft.com/library/windows/apps/dn926760)-Klasse abzurufen. Geben Sie [**MediaStreamType.VideoRecord**](https://msdn.microsoft.com/library/windows/apps/br226640) an, um anzuzeigen, dass der Effekt auf den Videoaufnahmedatenstrom angewendet werden soll.

Registrieren Sie einen Ereignishandler für das Ereignis [**EnabledChanged**](https://msdn.microsoft.com/library/windows/apps/dn948982), und rufen Sie die Hilfsmethode **SetUpVideoStabilizationRecommendationAsync** auf. Beide werden später in diesem Artikel erläutert. Legen Sie abschließend die [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926775)-Eigenschaft des Effekts auf „true“ fest, um den Effekt zu aktivieren.

[!code-cs[CreateVideoStabilizationEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateVideoStabilizationEffect)]

### Verwenden empfohlener Codierungseigenschaften

Wie weiter oben in diesem Artikel beschrieben wurde, verursacht die Technik, die der Videostabilisierungseffekt verwendet, eine geringe Zuschneidung des stabilisierten Videos im Vergleich zum Quellvideo. Definieren Sie die folgende Hilfsfunktion in Ihrem Code, um die Videocodierungseigenschaften anzupassen und diese Beschränkung des Effekts optimal zu verarbeiten. Dieser Schritt ist nicht erforderlich, um den Videostabilisierungseffekt zu verwenden. Wenn Sie diesen Schritt jedoch nicht ausführen, wird das resultierende Video leicht hochskaliert und hat dadurch eine etwas niedrigere Bildqualität.

Rufen Sie [**GetRecommendedStreamConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn948983) für die Instanz des Videostabilisierungseffekts auf, übergeben Sie das [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825)-Objekt, das den Effekt über Ihre aktuellen Eingabedatenstrom-Codierungseigenschaften informiert, und Ihr **MediaEncodingProfile**, sodass der Effekt Ihre aktuellen Ausgabecodierungseigenschaften kennt. Diese Methode gibt ein [**VideoStreamConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn926727)-Objekt zurück, das neue empfohlene Eingabe- und Ausgabestream-Codierungseigenschaften enthält.

Die empfohlenen Eingabecodierungseigenschaften bieten, sofern vom Gerät unterstützt, eine höhere Auflösung als die ursprünglichen Einstellungen, die Sie bereitgestellt haben, sodass es einen minimalen Datenverlust bei der Auflösung nach dem Anwenden der Zuschneidung des Effekts gibt.

Rufen Sie [**VideoDeviceController.SetMediaStreamPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh700895) auf, um die neuen Codierungseigenschaften festzulegen. Verwenden Sie vor dem Festlegen der neuen Eigenschaften die Membervariable, um die ursprünglichen Codierungseigenschaften zu speichern, damit Sie die Einstellungen wieder zurückändern können, wenn Sie den Effekt deaktivieren.

Wenn der Videostabilisierungseffekt das Ausgabevideo zuschneiden muss, entsprechen die empfohlenen Ausgabecodierungseigenschaften der Größe des zugeschnittenen Videos. Das bedeutet, dass die Ausgabeauflösung mit der Größe des zugeschnittenen Videos übereinstimmt. Wenn Sie die empfohlenen Ausgabeeigenschaften nicht verwenden, wird das Video hochskaliert, um der anfänglichen Ausgabegröße zu entsprechen, was zu einem Verlust an Bildqualität führt.

Legen Sie die [**Video**](https://msdn.microsoft.com/library/windows/apps/hh701124)-Eigenschaft des **MediaEncodingProfile**-Objekts fest. Verwenden Sie vor dem Festlegen der neuen Eigenschaften die Membervariable, um die ursprünglichen Codierungseigenschaften zu speichern, damit Sie die Einstellungen wieder zurückändern können, wenn Sie den Effekt deaktivieren.

[!code-cs[SetUpVideoStabilizationRecommendationAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetUpVideoStabilizationRecommendationAsync)]

### Behandeln des Deaktivierens des Videostabilisierungseffekts

Das System kann den Videostabilisierungseffekt automatisch deaktivieren, wenn Sie der Pixeldurchsatz zu groß für die Verarbeitung durch den Effekt ist oder wenn erkannt wird, dass der Effekt langsam ausgeführt wird. In diesem Fall wird das „EnabledChanged“-Ereignis ausgelöst. Die **VideoStabilizationEffect**-Instanz im Parameter *sender* gibt den neuen Zustand des Effekts (aktiviert oder deaktiviert) an. [
            **VideoStabilizationEffectEnabledChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn948979) hat einen [**VideoStabilizationEffectEnabledChangedReason**](https://msdn.microsoft.com/library/windows/apps/dn948981)-Wert, der angibt, warum der Effekt aktiviert oder deaktiviert wurde. Beachten Sie, dass dieses Ereignis auch ausgelöst wird, wenn Sie den Effekt programmgesteuert aktivieren oder deaktivieren. In diesem Fall ist der Grund **Programmatic**.

In der Regel verwenden Sie dieses Ereignis zum Anpassen der Benutzeroberfläche Ihrer App, um den aktuellen Status der Videostabilisierung anzugeben.

[!code-cs[VideoStabilizationEnabledChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoStabilizationEnabledChanged)]

### Bereinigen des Videostabilisierungseffekts

Rufen Sie zum Bereinigen des Videostabilisierungseffekts [**ClearEffectsAsync**](https://msdn.microsoft.com/library/windows/apps/br226592) auf, um alle Effekte aus der Videopipeline zu löschen. Wenn die Membervariablen mit den ursprünglichen Codierungseigenschaften nicht null sind, verwenden Sie diese zum Wiederherstellen der Codierungseigenschaften. Entfernen Sie schließlich den Ereignishandler **EnabledChanged**, und legen Sie den Effekt auf Null fest.

[!code-cs[CleanUpVisualStabilizationEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpVisualStabilizationEffect)]

## Verwandte Themen

* [Aufnehmen von Fotos und Videos mit „MediaCapture“](capture-photos-and-video-with-mediacapture.md)
 

 






<!--HONumber=Mar16_HO1-->


