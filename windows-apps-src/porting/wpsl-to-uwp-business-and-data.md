---
author: mcleblanc
description: "Im Hintergrund Ihrer Benutzeroberfläche befinden sich Ihre Geschäfts- und Datenebenen."
title: "Portieren von Windows Phone Silverlight-Geschäfts- und -Datenebenen zu UWP"
ms.assetid: 27c66759-2b35-41f5-9f7a-ceb97f4a0e3f
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 24e94e91adc0e5ef0b7a076d54299eab8c4ba527

---

#  Portieren von Windows Phone Silverlight-Geschäfts- und -Datenebenen zu UWP

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Das vorherige Thema war [Portieren: E/A, Gerät und App-Modell](wpsl-to-uwp-input-and-sensors.md).

Im Hintergrund Ihrer UI befinden sich Ihre Geschäfts- und Datenebenen. Der Code auf diesen Ebenen ruft Betriebssystem- und .NET Framework-APIs auf (z. B. Hintergrundverarbeitung, Position, Kamera, Dateisystem, Netzwerk und andere Datenzugriffsfunktionen). Die meisten dieser APIs sind [für eine UWP (Universelle Windows-Plattform)-App verfügbar](https://msdn.microsoft.com/library/windows/apps/br211369). Sie können also davon ausgehen, dass sich ein Großteil dieses Codes ohne Änderungen portieren lässt.

## Asynchrone Methoden

Einer der Hauptvorteile der universellen Windows-Plattform (UWP) besteht darin, das sie Ihnen die Erstellung von Apps ermöglicht, die wirklich und durchgehend reaktionsfähig sind. Animationen sind immer reibungslos, und Touchinteraktionen wie Verschieben und Wischen erfolgen sofort und ohne Verzögerung, wodurch der Eindruck entsteht, dass die UI quasi an Ihrem Finger „klebt“. Um dies zu erreichen, wurden alle UWP-APIs, deren Ausführung innerhalb von 50 Millisekunden nicht garantiert werden kann, als asynchron konzipiert und mit dem Namenssuffix **Async** versehen. Die Rückgabe Ihres UI-Threads nach dem Aufruf einer **Async**-Methode erfolgt sofort, und die Arbeit wird in einem anderen Thread ausgeführt. Die Nutzung einer **Async**-Methode ist in Bezug auf die Syntax mit dem C#-Operator **await**, JavaScript-Promise-Objekten und C++-Fortsetzungen sehr einfach. Weitere Informationen finden Sie unter [Asynchrone Programmierung](https://msdn.microsoft.com/library/windows/apps/mt187335).

## Hintergrundverarbeitung

Eine Windows Phone Silverlight-App kann ein verwaltetes **ScheduledTaskAgent**-Objekt verwenden, um eine Aufgabe auszuführen, während sich die App nicht im Vordergrund befindet. UWP-Apps verwenden auf ähnliche Weise die [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)-Klasse zum Erstellen und Registrieren von Hintergrundaufgaben. Sie definieren eine Klasse, die die Arbeit Ihrer Hintergrundaufgabe implementiert. Das System führt die Hintergrundaufgabe regelmäßig aus, indem die [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811)-Methode der Klasse zum Ausführen der Arbeit aufgerufen wird. Denken Sie bei einer UWP-App daran, die Deklaration **Hintergrundaufgaben** im App-Paketmanifest festzulegen. Weitere Informationen finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](https://msdn.microsoft.com/library/windows/apps/mt299103).

Um große Datendateien im Hintergrund zu übertragen, verwendet eine Windows Phone Silverlight-App die **BackgroundTransferService**-Klasse. Eine UWP-App verwendet zu diesem Zweck APIs im [**Windows.Networking.BackgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242)-Namespace. Die Features initiieren Übertragungen anhand eines ähnlichen Musters, die neue API verfügt aber über verbesserte Funktionen und eine höhere Leistung. Weitere Informationen finden Sie unter [Übertragen von Daten im Hintergrund](https://msdn.microsoft.com/library/windows/apps/xaml/hh452975).

Eine Windows Phone Silverlight-App verwendet die verwalteten Klassen im **Microsoft.Phone.BackgroundAudio**-Namespace zum Wiedergeben von Audio, während sich die App nicht im Vordergrund befindet. Die UWP verwendet das Windows Phone Store-App-Modell. Weitere Informationen finden Sie unter [Hintergrundaudio](https://msdn.microsoft.com/library/windows/apps/mt282140) und im [Hintergrundaudio](http://go.microsoft.com/fwlink/p/?linkid=619997)-Beispiel.

## Clouddienste, Netzwerk und Datenbanken

Das Hosten von Daten und App-Diensten in der Cloud ist mit Azure möglich. Weitere Informationen finden Sie unter [Erste Schritte mit Mobile Services](http://go.microsoft.com/fwlink/p/?LinkID=403138). Für Lösungen, die sowohl Online- als auch Offlinedaten erfordern: [Verwendung der Offlinedatensynchronisierung in Mobile Services](http://azure.microsoft.com/documentation/articles/mobile-services-windows-store-dotnet-get-started-offline-data/).

Die UWP verfügt über eine Teilunterstützung für die **System.Net.HttpWebRequest**-Klasse, aber die **System.Net.WebClient**-Klasse wird nicht unterstützt. Die empfohlene modernere Alternative ist die [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)-Klasse (oder [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx), wenn Ihr Code auf andere Plattformen mit .NET-Unterstützung portierbar sein soll). Für diese APIs wird [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) verwendet, um eine HTTP-Anforderung darzustellen.

UWP-Apps bieten zurzeit keine integrierte Unterstützung für datenintensive Szenarien wie beispielsweise Branchenszenarien. Sie können jedoch SQLite für lokale Transaktionsdatenbankdienste verwenden. Weitere Informationen finden Sie unter [SQLite](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936).

Übergeben Sie absolute URIs, keine relativen URIs, an Windows-Runtime-Typen. Weitere Informationen finden Sie unter [Übergeben eines URI an die Windows-Runtime](https://msdn.microsoft.com/library/hh763341.aspx).

## Launcher und Chooser

Mit Launchern und Choosern (im **Microsoft.Phone.Tasks**-Namespace) kann eine Windows Phone Silverlight-App mit dem Betriebssystem interagieren, um allgemeine Vorgänge auszuführen (z. B. Erstellen einer E-Mail, Auswählen eines Fotos oder Teilen bestimmter Arten von Daten mit einer anderen App). Den entsprechenden UWP-Typ finden Sie unter **Microsoft.Phone.Tasks** im Thema [Windows Phone Silverlight zu Windows 10: Namespace- und Klassenzuordnungen](wpsl-to-uwp-namespace-and-class-mappings.md). Diese reichen von ähnlichen Mechanismen (so genannten Launchern und Pickern) bis zur Implementierung eines Vertrags zum Teilen von Daten zwischen Apps.

Wenn z. B. die Fotoauswahlaufgabe verwendet wird, kann eine Windows Phone Silverlight-App in einen Ruhezustand versetzt oder sogar als veraltet markiert werden. Eine UWP-App bleibt aktiv und wird weiter ausgeführt, wenn die [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)-Klasse verwendet wird.

## Monetisierung (Testmodus und In-App-Einkäufe)

Für den Großteil der Funktionen, die im Testmodus und für In-App-Einkäufe verwendet werden, kann eine Windows Phone Silverlight-App die UWP [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)-Klasse verwenden. Daher muss kein Code portiert werden. Eine Windows Phone Silverlight-App ruft jedoch **MarketplaceDetailTask.Show** auf, um die App zum Kauf anzubieten:

```csharp
    private void Buy()
    {
        MarketplaceDetailTask marketplaceDetailTask = new MarketplaceDetailTask();
        marketplaceDetailTask.ContentType = MarketplaceContentType.Applications;
        marketplaceDetailTask.Show();
    }
```

Portieren Sie diesen Code zum Aufrufen der UWP [**RequestAppPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/hh967813)-Methode:

```csharp
    private async void Buy()
    {
        await Windows.ApplicationModel.Store.CurrentApp.RequestAppPurchaseAsync(false);
    }
```

Falls Sie über Code verfügen, der Ihre Features für App-Kauf und In-App-Einkäufe zu Testzwecken simuliert, können Sie ihn portieren, um stattdessen die [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)-Klasse zu verwenden.

## Benachrichtigungen für Kachel- oder Popupaktualisierungen

Benachrichtigungen sind eine Erweiterung des Pushbenachrichtigungsmodells für Windows Phone Silverlight-Apps. Wenn Sie eine Benachrichtigung vom Windows-Pushbenachrichtigungsdienst (WNS) empfangen, können Sie die Informationen mit einer Kachelaktualisierung oder einem Popup in der Benutzeroberfläche anzeigen. Informationen zum Portieren des Benutzeroberflächenteils Ihrer Benachrichtigungsfeatures finden Sie unter [Kacheln und Popups](w8x-to-uwp-porting-xaml-and-ui.md#tiles-and-toasts).

Ausführlichere Informationen zur Verwendung von Benachrichtigungen in UWP-Apps finden Sie unter [Senden von Popupbenachrichtigungen](https://msdn.microsoft.com/library/windows/apps/xaml/hh868266).

Informationen und Lernprogramme zur Verwendung von Kacheln, Popups, Signalen, Bannern und Benachrichtigungen in einer Windows-Runtime-App mit C++, C# oder Visual Basic finden Sie unter [Verwenden von Kacheln, Signalen und Popupbenachrichtigungen](https://msdn.microsoft.com/library/windows/apps/xaml/hh868259).

## Speicher (Dateizugriff)

Windows Phone Silverlight-Code, der App-Einstellungen als Schlüssel-Wert-Paare in isoliertem Speicher speichert, kann problemlos portiert werden. Hier sehen Sie ein Vorher-Nachher-Beispiel, und zwar zuerst die Windows Phone Silverlight-Version:

```csharp
    var propertySet = IsolatedStorageSettings.ApplicationSettings;
    const string key = "favoriteAuthor";
    propertySet[key] = "Charles Dickens";
    propertySet.Save();
    string myFavoriteAuthor = propertySet.Contains(key) ? (string)propertySet[key] : "<none>";
```

Und hier die UWP-Entsprechung:

```csharp
    var propertySet = Windows.Storage.ApplicationData.Current.LocalSettings.Values;
    const string key = "favoriteAuthor";
    propertySet[key] = "Charles Dickens";
    string myFavoriteAuthor = propertySet.ContainsKey(key) ? (string)propertySet[key] : "<none>";
```

Obwohl eine Teilmenge des **Windows.Storage**-Namespaces für sie verfügbar ist, führen viele Windows Phone Silverlight-Apps E/A-Dateivorgänge mit der **IsolatedStorageFile**-Klasse aus, da diese schon länger unterstützt wird. Hier sehen Sie ein Vorher-Nachher-Beispiel für das Schreiben und Lesen einer Datei unter Verwendung von **IsolatedStorageFile**, und zwar zuerst die Windows Phone Silverlight-Version:

```csharp
    const string filename = "FavoriteAuthor.txt";
    using (var store = IsolatedStorageFile.GetUserStoreForApplication())
    {
        using (var streamWriter = new StreamWriter(store.CreateFile(filename)))
        {
            streamWriter.Write("Charles Dickens");
        }
        using (var StreamReader = new StreamReader(store.OpenFile(filename, FileMode.Open, FileAccess.Read)))
        {
            string myFavoriteAuthor = StreamReader.ReadToEnd();
        }
    }
```

Und hier die gleiche Funktionalität mit der UWP:

```csharp
    const string filename = "FavoriteAuthor.txt";
    var store = Windows.Storage.ApplicationData.Current.LocalFolder;
    Windows.Storage.StorageFile file = await store.CreateFileAsync(filename, Windows.Storage.CreationCollisionOption.ReplaceExisting);
    await Windows.Storage.FileIO.WriteTextAsync(file, "Charles Dickens");
    file = await store.GetFileAsync(filename);
    string myFavoriteAuthor = await Windows.Storage.FileIO.ReadTextAsync(file);
```

Eine Windows Phone Silverlight-App hat schreibgeschützten Zugriff auf die optionale SD-Karte. Eine UWP-App hat Lese-/Schreibzugriff auf die SD-Karte. Weitere Informationen finden Sie unter [Zugreifen auf die SD-Karte](https://msdn.microsoft.com/library/windows/apps/mt188699).

Informationen zum Zugriff auf Fotos, Musik und Videodateien in einer UWP-App finden Sie unter [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](https://msdn.microsoft.com/library/windows/apps/mt188703).

Weitere Informationen finden Sie unter [Dateien, Ordner und Bibliotheken](https://msdn.microsoft.com/library/windows/apps/mt185399).

Das nächste Thema ist [Portieren für Formfaktor und Benutzerfreundlichkeit](wpsl-to-uwp-form-factors-and-ux.md).

## Verwandte Themen

* [Namespace- und Klassenzuordnungen](wpsl-to-uwp-namespace-and-class-mappings.md)
 




<!--HONumber=Jun16_HO4-->


