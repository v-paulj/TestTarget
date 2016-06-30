---
author: drewbatgit
ms.assetid: 09BA9250-A476-4803-910E-52F0A51704B1
description: "In diesem Artikel erfahren Sie, wie Sie die IMediaEncodingProperties-Schnittstelle verwenden, um die Auflösung und Framerate des Kameravorschau-Datenstroms sowie von aufgenommenen Fotos und Videos festzulegen."
title: Festlegen von Mediencodierungseigenschaften
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: d7b44ce9db2e3d540036525c4b43e155a9500010

---

# Festlegen von Mediencodierungseigenschaften

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


In diesem Artikel erfahren Sie, wie Sie die [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011)-Schnittstelle verwenden, um die Auflösung und Framerate des Kameravorschau-Datenstroms sowie von aufgenommenen Fotos und Videos festzulegen. In ihm wird auch gezeigt, wie Sie sicherstellen, dass das Seitenverhältnis des Vorschaudatenstroms mit dem Seitenverhältnis der aufgenommenen Medien übereinstimmt.

Kameraprofile bieten eine erweiterte Möglichkeit zum Ermitteln und Festlegen der Datenstromeigenschaften der Kamera, sie werden jedoch nicht für alle Geräte unterstützt. Weitere Informationen finden Sie unter [Kameraprofile](camera-profiles.md).

Der Code in diesem Artikel wurde aus dem [CameraResolution-Beispiel](http://go.microsoft.com/fwlink/p/?LinkId=624252&clcid=0x409) übernommen und angepasst. Sie können das Beispiel herunterladen, um den verwendeten Code im Kontext anzuzeigen oder das Beispiel als Ausgangspunkt für Ihre eigene App zu verwenden.

**Hinweis**  
Dieser Artikel baut auf Konzepten und Codebeispielen auf, die unter [Aufnehmen von Fotos und Videos mit MediaCapture](capture-photos-and-video-with-mediacapture.md) erläutert werden. Dort werden die Schritte für die Implementierung einer grundlegenden Foto- und Videoaufnahme beschrieben. Es wird empfohlen, dass Sie sich mit dem grundlegenden Muster für die Medienerfassung in diesem Artikel vertraut machen, bevor Sie in fortgeschrittene Aufnahmeszenarien einsteigen. Bei dem Code in diesem Artikel wird davon ausgegangen, dass Ihre App bereits eine Instanz von MediaCapture aufweist, die ordnungsgemäß initialisiert wurde.

## Eine Hilfsklasse mit Mediencodierungseigenschaften

Durch das Erstellen einer einfachen Hilfsklasse zum Umschließen der Funktionalität der [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011)-Schnittstelle ist es einfacher, eine Reihe von Codierungseigenschaften auszuwählen, die bestimmte Kriterien erfüllen. Aufgrund des folgenden Verhaltens des Codierungseigenschaftenfeatures ist diese Hilfsklasse besonders hilfreich:

**Warnung**  
Die [**VideoDeviceController.GetAvailableMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211994)-Methode akzeptiert einen Member der [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/br226640)-Enumeration, z. B. **VideoRecord** oder **Photo**, und gibt eine Liste von [**ImageEncodingProperties-Objekten**](https://msdn.microsoft.com/library/windows/apps/hh700993) oder [**VideoEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701217)-Objekten zurück, die die Datenstrom-Codierungseinstellungen, z. B. die Auflösung des aufgenommenen Fotos oder Videos, angeben. Die Ergebnisse des Aufrufs von **GetAvailableMediaStreamProperties** können **ImageEncodingProperties** oder **VideoEncodingProperties** enthalten, unabhängig vom angegebenen **MediaStreamType**-Wert. Aus diesem Grund sollten Sie vor dem Zugriff auf die Eigenschaftswerte immer den Typ jedes zurückgegebenen Werts überprüfen und ihn in den entsprechenden Typ umwandeln.

Die nachfolgend definierte Hilfsklasse behandelt die Typüberprüfung und -umwandlung für [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) oder [**VideoEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701217), damit der App-Code nicht zwischen den beiden Typen unterscheiden muss. Darüber hinaus macht die Hilfsklasse Eigenschaften für das Seitenverhältnis der Eigenschaften, die Framerate (nur für Videocodierungseigenschaften) und einen Anzeigenamen verfügbar, der das Anzeigen der Codierungseigenschaften auf der App-UI erleichtert.

Sie müssen den [**Windows.Media.MediaProperties**](https://msdn.microsoft.com/library/windows/apps/hh701296)-Namespace in die Quelldatei für die Hilfsklasse einschließen.

[!code-cs[MediaEncodingPropertiesUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMediaEncodingPropertiesUsing)]

[!code-cs[StreamPropertiesHelper](./code/BasicMediaCaptureWin10/cs/StreamPropertiesHelper.cs#SnippetStreamPropertiesHelper)]

## Bestimmen, ob der Vorschau- und Aufnahmedatenstrom voneinander unabhängig sind

Auf einigen Geräten wird für den Vorschau- und Aufnahmedatenstrom der gleiche Hardwarekontakt verwendet. Bei diesen Geräten werden durch Festlegen der Codierungseigenschaften eines Datenstroms auch die Codierungseigenschaften des anderen Datenstroms festgelegt. Für Geräte, die unterschiedliche Hardwarekontakte für die Aufnahme und Vorschau verwenden, können die Eigenschaften für jeden Datenstrom unabhängig voneinander festgelegt werden. Mit dem folgenden Code können Sie bestimmen, ob der Vorschau- und Aufnahmedatenstrom unabhängig voneinander sind. Sie sollten die Benutzeroberfläche anpassen, um abhängig vom Ergebnis dieses Tests das eigenständige Festlegen der Datenströme zu aktivieren oder zu deaktivieren.

[!code-cs[CheckIfStreamsAreIdentical](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCheckIfStreamsAreIdentical)]

## Abrufen einer Liste der verfügbaren Datenstromeigenschaften

Rufen Sie eine Liste der verfügbaren Datenstromeigenschaften für ein Aufnahmegerät ab. Hierzu rufen Sie den [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) für das [MediaCapture](capture-photos-and-video-with-mediacapture.md)-Objekt der App ab, rufen [**GetAvailableMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211994) auf und übergeben einen der [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/br226640)-Werte, d. h. **VideoPreview**, **VideoRecord** oder **Photo** In diesem Beispiel wird für jeden der von **GetAvailableMediaStreamProperties** zurückgegebenen [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011)-Werte unter Verwendung von Linq-Syntax eine Liste von **StreamPropertiesHelper**-Objekten erstellt, die zuvor in diesem Artikel definiert wurden. In diesem Beispiel werden Linq-Erweiterungsmethoden verwendet, um die zurückgegebenen Eigenschaften zunächst nach Auflösung und dann nach Bildfrequenz zu sortieren.

Wenn für die App bestimmte Anforderungen an die Auflösung oder Bildfrequenz gelten, können Sie programmgesteuert eine Reihe von Mediencodierungseigenschaften auswählen. Eine Kamera-App macht stattdessen die Liste der verfügbaren Eigenschaften auf der Benutzeroberfläche verfügbar und ermöglicht es dem Benutzer, die gewünschten Einstellungen auszuwählen. Für jedes Listenelement von **StreamPropertiesHelper**-Objekten in der Liste wird ein **ComboBoxItem** erstellt. Der Inhalt wird auf den von der Hilfsklasse zurückgegebenen Anzeigenamen festgelegt, und das Tag wird auf die Hilfsklasse selbst festgelegt, sodass es später zum Abrufen der zugeordneten Codierungseigenschaften verwendet werden kann. Anschließend werden dem an die Methode übergebenen **ComboBox**-Element die einzelnen **ComboBoxItem**-Elemente hinzugefügt.

[!code-cs[PopulateStreamPropertiesUI](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPopulateStreamPropertiesUI)]

## Festlegen der gewünschten Datenstromeigenschaften

Weisen Sie den Videogerätecontroller an, die gewünschten Codierungseigenschaften zu verwenden, indem Sie [**SetMediaStreamPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh700895) aufrufen und den **MediaStreamType**-Wert übergeben, der angibt, ob Foto-, Video- oder Vorschaueigenschaften festgelegt werden sollen. In diesem Beispiel werden die angeforderten Codierungseigenschaften festgelegt, wenn der Benutzer ein Element in einem der **ComboBox**-Objekte auswählt, die mithilfe der **PopulateStreamPropertiesUI**-Hilfsmethode aufgefüllt wurden.

[!code-cs[PreviewSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewSettingsChanged)]

[!code-cs[PhotoSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhotoSettingsChanged)]

[!code-cs[VideoSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoSettingsChanged)]

## Abgleichen des Seitenverhältnisses von Vorschau- und Aufnahmedatenstrom

Eine typische Kamera-App stellt zwar UI-Elemente bereit, mit denen der Benutzer die Auflösung für Video- oder Fotoaufnahmen festlegen kann, die Auflösung der Vorschau wird jedoch üblicherweise programmgesteuert festgelegt. Für die Wahl der optimalen Auflösung für den Vorschaudatenstrom Ihrer App gibt es unterschiedliche Strategien:

-   Wählen Sie die höchste verfügbare Vorschauauflösung aus, und überlassen Sie jegliche erforderliche Vorschauskalierung dem Benutzeroberflächenframework.

-   Wählen Sie die Vorschauauflösung aus, die der Aufnahmeauflösung am nächsten kommt, um eine möglichst geringe Diskrepanz zwischen Vorschau und tatsächlicher Medienaufnahme zu erreichen.

-   Wählen Sie die Vorschauauflösung aus, die möglichst genau der Größe des [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278)-Elements entspricht, damit nicht mehr Pixel als nötig die Pipeline für den Vorschaudatenstrom durchlaufen.

**Wichtig**  
Bei einigen Geräten kann für den Vorschau- und Aufnahmedatenstrom der Kamera ein unterschiedliches Seitenverhältnis festgelegt werden. Durch diese Abweichung verursachtes Zuschneiden von Frames kann dazu führen, dass in den aufgenommenen Medien Inhalt vorhanden ist, der in der Vorschau nicht sichtbar war, sodass eine negative Benutzererfahrung die Folge ist. Es wird dringend empfohlen, für den Vorschau- und Aufnahmedatenstrom das gleiche Seitenverhältnis innerhalb eines kleinen Toleranzfensters zu verwenden. Solange das Seitenverhältnis nahezu übereinstimmt, können für Aufnahme und Vorschau vollkommen unterschiedliche Auflösungen aktiviert sein.


Um sicherzustellen, dass das Seitenverhältnis des Foto- oder Videoaufnahme-Datenstroms mit dem Seitenverhältnis des Vorschaudatenstroms übereinstimmt, wird in diesem Beispiel [**VideoDeviceController.GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211995) aufgerufen und der **VideoPreview**-Enumerationswert übergeben, um die aktuellen Datenstromeigenschaften des Vorschaudatenstroms anzufordern. Anschließend wird ein kleines Toleranzfenster für das Seitenverhältnis definiert, damit Seitenverhältnisse eingeschlossen werden können, die nicht genau mit dem Seitenverhältnis des Vorschaudatenstroms übereinstimmen, solange die Abweichung nicht groß ist. Dann werden mithilfe einer Linq-Erweiterungsmethode nur die **StreamPropertiesHelper**-Objekte ausgewählt, in denen das Seitenverhältnis im definierten Toleranzbereich des Vorschaudatenstroms liegt.

[!code-cs[MatchPreviewAspectRatio](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMatchPreviewAspectRatio)]

 

 







<!--HONumber=Jun16_HO4-->


