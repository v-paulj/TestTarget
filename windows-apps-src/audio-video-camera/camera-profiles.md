---
author: drewbatgit
ms.assetid: 42A06423-670F-4CCC-88B7-3DCEEDDEBA57
description: Dieser Artikel beschreibt, wie Sie Kameraprofile verwenden, um die Funktionen verschiedener Videoaufzeichnungsgeräte zu ermitteln und zu verwalten.
title: Kameraprofile
---

# Kameraprofile

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Dieser Artikel beschreibt, wie Sie Kameraprofile verwenden, um die Funktionen verschiedener Videoaufzeichnungsgeräte zu ermitteln und zu verwalten.

**Hinweis**  
Dieser Artikel baut auf Konzepten und Code auf, die unter [Aufnehmen von Fotos und Videos mit MediaCapture](capture-photos-and-video-with-mediacapture.md) erläutert werden. Darin werden die Schritte für die Implementierung einer grundlegenden Foto- und Videoaufnahme beschrieben. Es wird empfohlen, dass Sie sich mit dem grundlegenden Muster für die Medienerfassung in diesem Artikel vertraut machen, bevor Sie in fortgeschrittene Aufnahmeszenarien einsteigen. Bei dem Code in diesem Artikel wird davon ausgegangen, dass Ihre App bereits eine Instanz von MediaCapture aufweist, die ordnungsgemäß initialisiert wurde.

 

## Informationen zu Kameraprofilen

Kameras auf verschiedenen Geräten unterstützen unterschiedliche Funktionen; dazu gehören beispielsweise die unterschiedlichen unterstützten Auflösungen, die Bildfrequenz für Videoaufnahmen, und ob HDR oder Aufnahmen mit variabler Bildfrequenz unterstützt werden. Dieser Satz von Funktionen wird vom UWP-Medienaufnahmeframework (Universelle Windows-Plattform) in einer [**MediaCaptureVideoProfileMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926695)-Klasse gespeichert. Eine Kameraprofil, dargestellt durch ein [**MediaCaptureVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926694)-Objekt, verfügt über drei Gruppen von Medienbeschreibungen: eine für die Fotoaufnahme, eine für die Videoaufnahme und eine weitere für die Videovorschau.

Vor dem Initialisieren des [MediaCapture](capture-photos-and-video-with-mediacapture.md)-Objekts können Sie die Aufnahmegeräte auf dem aktuellen Gerät abfragen, um festzustellen, welche Profile unterstützt werden. Wenn Sie ein unterstütztes Profil auswählen, wissen Sie, dass das Aufnahmegerät alle Funktionen in den Medienbeschreibungen des Profils unterstützt. Dadurch entfällt die Notwendigkeit eines Trial-and-Error-Ansatzes, um zu ermitteln, welche Kombinationen von Funktionen auf einem bestimmten Gerät unterstützt werden.

Im Artikel zur grundlegenden Medienaufzeichnung ([Aufnehmen von Fotos und Videos mit MediaCapture](capture-photos-and-video-with-mediacapture.md)) wird die für die Initialisierung der Medienaufnahme verwendete [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573)-Klasse nur mit der ID-Zeichenfolge des Aufnahmegeräts erstellt, was der minimalen Menge von Daten entspricht, die für die Initialisierung erforderlich ist.

[!code-cs[BasicInitExample](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetBasicInitExample)]

In den Codebeispielen in diesem Artikel wird diese minimale Initialisierung durch die Ermittlung der Kameraprofile ersetzt. Diese unterstützten unterschiedliche Funktionen, mit denen das Medienaufnahmegerät initialisiert wird.

## Suchen eines Videogeräts, das Kameraprofile unterstützt

Bevor Sie unterstützte Kameraprofile suchen, sollten Sie ein Aufnahmegerät suchen, das die Verwendung von Kameraprofilen unterstützt. Die im folgenden Beispiel definierte **GetVideoProfileSupportedDeviceIdAsync**-Hilfsmethode verwendet die [**DeviceInformaion.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432)-Methode zum Abrufen einer Liste aller verfügbaren Videoaufnahmegeräte. Sie durchläuft alle Geräte in der Liste und ruft die statische Methode [**IsVideoProfileSupported**](https://msdn.microsoft.com/library/windows/apps/dn926714) für jedes Gerät auf, um festzustellen, ob es Videoprofile unterstützt. Sie durchläuft auch die [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906)-Eigenschaft für jedes Gerät, sodass Sie angeben können, ob Sie eine Kamera auf der Vorderseite oder auf der Rückseite des Geräts bevorzugen.

Wenn in dem angegebenen Bereich ein Gerät gefunden wird, das Kameraprofile unterstützt, wird der [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437)-Wert mit der ID-Zeichenfolge des Geräts zurückgegeben.

[!code-cs[GetVideoProfileSupportedDeviceIdAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetVideoProfileSupportedDeviceIdAsync)]

Wenn die von der **GetVideoProfileSupportedDeviceIdAsync**-Hilfsmethode zurückgegebene Geräte-ID NULL oder eine leere Zeichenfolge ist, gibt es in dem angegebenen Bereich kein Gerät, das Kameraprofile unterstützt. In diesem Fall sollten Sie das Medienaufnahmegerät ohne Profile initialisieren.

[!code-cs[GetDeviceWithProfileSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetDeviceWithProfileSupport)]

## Auswählen eines Profils basierend auf der unterstützten Auflösung und Bildfrequenz

Um ein Profil mit bestimmten Funktionen auszuwählen, zum Beispiel mit der Fähigkeit, eine bestimmte Auflösung oder Bildfrequenz zu erzielen, sollten Sie zuerst die oben definierte Hilfsmethode aufrufen, um die ID eines Aufnahmegeräts abzurufen, das die Verwendung von Kameraprofilen unterstützt.

Erstellen Sie ein neues [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573)-Objekt, und übergeben Sie die ausgewählte Geräte-ID. Als Nächstes rufen Sie die statische Methode [**MediaCapture.FindAllVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926708) auf, um eine Liste aller vom Gerät unterstützten Kameraprofile zu erhalten.

In diesem Beispiel wird eine „Linq“-Abfragemethode verwendet, die im verwendeten **System.Linq**-Namespace enthalten ist, um ein Profil mit einem [**SupportedRecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926705)-Objekt auszuwählen. Die Eigenschaften [**Width**](https://msdn.microsoft.com/library/windows/apps/dn926700), [**Height**](https://msdn.microsoft.com/library/windows/apps/dn926697) und [**FrameRate**](https://msdn.microsoft.com/library/windows/apps/dn926696) entsprechen dabei den angeforderten Werten. Wenn eine Übereinstimmung gefunden wird, werden [**VideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926679) und [**RecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926678) der **MediaCaptureInitializationSettings**-Klasse auf die Werte des anonymen Typs festgelegt, der von der „Linq“-Abfrage zurückgegeben wurde. Wenn keine Übereinstimmung gefunden wird, wird das Standardprofil verwendet.

[!code-cs[FindWVGA30FPSProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindWVGA30FPSProfile)]

Nachdem Sie das **MediaCaptureInitializationSettings**-Objekt mit dem gewünschten Kameraprofil aufgefüllt haben, rufen Sie einfach [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) in Ihrem Medienaufnahmeobjekt auf, um es mit dem gewünschten Profil zu konfigurieren.

[!code-cs[InitCaptureWithProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitCaptureWithProfile)]

## Auswählen eines Profils, das mehrere Kameras gleichzeitig unterstützt

Sie können Kameraprofile verwenden, um zu ermitteln, ob ein Gerät Videoaufnahmen von mehreren Kameras gleichzeitig unterstützt. Für dieses Szenario benötigen Sie zwei Sätze von Aufnahmeobjekten, eines für die Kamera auf der Vorderseite und eines für die Kamera auf der Rückseite. Erstellen Sie für jede Kamera ein **MediaCapture**-Objekt, ein **MediaCaptureInitializationSettings**-Objekt und eine Zeichenfolge für die Aufnahmegeräte-ID. Fügen Sie außerdem eine boolesche Variable hinzu, die aufzeichnet, ob eine gleichzeitige Verwendung mehrerer Kameras unterstützt wird.

[!code-cs[ConcurrencySetup](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConcurrencySetup)]

Die statische Methode [**MediaCapture.FindConcurrentProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926709) gibt eine Liste von Kameraprofilen zurück, die vom angegebenen Aufnahmegerät unterstützt werden, das auch die gleichzeitige Verwendung mehrerer Kameras unterstützt. Verwenden Sie eine Linq-Abfrage, um ein Profil zu suchen, das die gleichzeitige Verwendung mehrerer Kameras unterstützt und das sowohl von der Kamera auf der Vorder- als auch auf der Rückseite unterstützt wird. Wenn ein Profil gefunden wird, das diese Anforderungen erfüllt, legen Sie das Profil in jedem **MediaCaptureInitializationSettings**-Objekt fest. Setzen Sie zudem die boolesche Variable für die Unterstützung mehrerer Kameras auf „true“.

[!code-cs[FindConcurrencyDevices](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindConcurrencyDevices)]

Rufen Sie **MediaCapture.InitializeAsync** für die primäre Kamera für Ihr App-Szenario auf. Wenn die gleichzeitige Verwendung mehrerer Kameras unterstützt wird, initialisieren Sie auch die zweite Kamera.

[!code-cs[InitConcurrentMediaCaptures](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitConcurrentMediaCaptures)]

## Verwenden bekannter Profile zum Suchen eines Profils, das HDR-Video unterstützt

Das Auswählen eines Profils, das HDR unterstützt, beginnt wie die anderen Szenarien. Erstellen Sie ein **MediaCaptureInitializationSettings**-Objekt und eine Zeichenfolge für die Aufnahmegerät-ID. Fügen Sie eine boolesche Variable hinzu, die aufzeichnet, ob HDR-Video unterstützt wird.

[!code-cs[GetHdrProfileSetup](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetHdrProfileSetup)]

Verwenden Sie die oben definierte **GetVideoProfileSupportedDeviceIdAsync**-Hilfsmethode, um die Geräte-ID für ein Aufnahmegerät abzurufen, das Kameraprofile unterstützt.

[!code-cs[FindDeviceHDR](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindDeviceHDR)]

Die statische Methode [**MediaCapture.FindKnownVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926710) gibt die Kameraprofile zurück, die von dem angegebenen Gerät unterstützt werden, das wiederum von dem angegebenen [**KnownVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn948843)-Wert kategorisiert wird. In diesem Szenario wird der **VideoRecording**-Wert angegeben, um nur Kameraprofile zurückzugeben, die Videoaufnahmen unterstützen.

Durchlaufen Sie die zurückgegebene Liste der Kameraprofile. Durchlaufen Sie für jedes Kameraprofil jede [**VideoProfileMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926695)-Klasse im Profil, und überprüfen Sie, ob die [**IsHdrVideoSupported**](https://msdn.microsoft.com/library/windows/apps/dn926698)-Eigenschaft „true“ ist. Sobald eine passende Medienbeschreibung gefunden wird, unterbrechen Sie die Schleife und weisen die Profil- und Beschreibungsobjekte dem **MediaCaptureInitializationSettings**-Objekt zu.

[!code-cs[FindHDRProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindHDRProfile)]

## Ermitteln, ob ein Gerät die gleichzeitige Aufnahme von Fotos und Videos unterstützt

Viele Geräte unterstützen die gleichzeitige Aufnahme von Fotos und Videos. Um festzustellen, ob ein Aufnahmegerät dies unterstützt, rufen Sie [**MediaCapture.FindAllVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926708) auf, um alle vom Gerät unterstützten Kameraprofile abzurufen. Verwenden Sie eine Linkabfrage, um ein Profil zu suchen, das mindestens einen Eintrag für [**SupportedPhotoMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926703) und [**SupportedRecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926705) aufweist, was bedeutet, dass das Profil die gleichzeitige Aufnahme unterstützt.

[!code-cs[GetPhotoAndVideoSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPhotoAndVideoSupport)]

Sie können diese Abfrage einschränken und nur nach Profilen suchen, die zusätzlich zur gleichzeitigen Videoaufnahme bestimmte Auflösungen oder andere Funktionen unterstützen. Sie können auch die [**MediaCapture.FindKnownVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926710)-Methode verwenden und den [**BalancedVideoAndPhoto**](https://msdn.microsoft.com/library/windows/apps/dn948843)-Wert angeben, um Profile abzurufen, die die gleichzeitige Aufnahme unterstützen. Das Abfragen aller Profile liefert aber genauere Ergebnisse.

## Verwandte Themen

* [Aufnehmen von Fotos und Videos mit MediaCapture](capture-photos-and-video-with-mediacapture.md)
 

 






<!--HONumber=May16_HO2-->


