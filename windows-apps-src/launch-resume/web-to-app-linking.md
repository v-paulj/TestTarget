---
author: TylerMSFT
title: "Unterstützung der Verknüpfung zwischen Web und App mit App-URI-Handlern"
description: "Fördern Sie die Bindung der Nutzer zu Ihrer App mithilfe von App-URI-Handler"
keywords: Deep Linking Windows
translationtype: Human Translation
ms.sourcegitcommit: 9ef86dcd4ae3d922b713d585543f1def48fcb645
ms.openlocfilehash: c9833f29d6080509c849e9d624f2bfcd0b0af04c

---

# Unterstützung der Verknüpfung zwischen Web und App mit App-URI-Handlern

Lernen Sie, wie sie die Bindung zwischen den Benutzern und Ihrer App durch die Unterstützung von Web-zu-App-Verlinkungen fördern. Web-zu-App-Verlinkungen erlauben es Ihnen, eine App mit einer Website zu verbinden Statt des Browsers wird Ihre app wird gestartet, wenn Benutzer einen HTTP- oder Https-Link zu Ihrer Website öffnen. Wenn Ihre App nicht installiert ist, wird ein Link zum Öffnen Ihrer Website bereitgestellt. Benutzer können dieser Erfahrung vertrauen, da nur Urheber verifizierten Contents registrieren können.

Um die Web-to-App Verlinkung zu aktivieren, benötigen Sie folgendes.
- Identifizieren Sie die URIs, die Ihrer App in der Manifestdatei behandeln wird.
- Eine JSON-Datei mit dem Paketfamiliennamen der App im selben Stammverzeichnis wie die App-Manifest Deklaration.
- Behandeln Sie die Aktivierung in der App

## Registrieren Sie sich, um HTTP- und Https-Links im App-Manifest zu behandeln.

Ihre App muss die URIs für die Websites zu identifizieren, die sie behandeln soll. Fügen Sie hierzu die **Windows.appUriHandler** Erweiterung Registrierung zu Ihrer app-Manifestdatei hinzu **Package.appxmanifest**.

Wenn beispielsweise die Adresse Ihrer Website "msn.com" lautet, würden Sie den folgenden Eintrag in Ihrem App-Manifest machen:

```xml
<Applications>
    ...
  <Extensions>
     <uap3:Extension Category="windows.appUriHandler">
      <uap3:AppUriHandler>
        <uap3:Host Name="msn.com" />
      </uap3:AppUriHandler>
    </uap3:Extension>
  </Extensions>
    ...
</Applications>
```

Die obige Deklaration registriert Ihre App zur Behandlung von Links vom angegebenen Host. Wenn Ihre Website mehrere Adressen hat (z. B.: m.example.com, www.example.com und example.com), fügen Sie einen separaten `<uap3:Host Name=... />` Eintrag innerhalb des `<uap3:AppUriHandler>` für die einzelnen Adressen hinzu.

## Verknüpfen Sie Ihre App und die Website mit einer JSON-Datei

Um sicherzustellen, dass nur Ihre App die Inhalte auf Ihrer Website öffnen kann, sollten Sie den Paketfamiliennamen Ihrer App in eine JSON-Datei einbinden, die auf dem Webserver-Stammverzeichnis oder in einem bekannten Verzeichnis der Domäne liegt. Dies bedeutet, dass Ihre Website die Zustimmung gibt, dass die aufgeführten Apps Inhalte auf Ihrer Website öffnen können. Sie können den Paketfamiliennamen im Packages-Abschnitt Pakete des App-Manifest-Designers finden.

>[!Important]
> Die JSON-Datei sollte kein .json Dateisuffix aufweisen.

Erstellen Sie eine JSON-Datei (ohne die Erweiterung .json) mit dem Namen **Windows-App-Web-Link** und stellen Sie den Paketfamiliennamen Ihrer App bereit. Zum Beispiel:

``` JSON
[{
  "packageFamilyName": "YourAppsPFN",
  "paths": [ "*" ],
  "excludePaths" : [ "/news/*, /blog/*" ]
 }]
```

Windows wird eine Https-Verbindung mit Ihrer Website herstellen und nach der entsprechenden JSON Datei auf dem Webserver suchen.

### Platzhalter

Das obige für eine JSON-Datei veranschaulicht die Verwendung von Platzhaltern. Platzhalter erlauben es Ihnen eine Vielzahl von Links mit weniger Codezeilen zu unterstützen. Die Web-zu-App-Verknüpfung unterstützt zwei Arten von Platzhaltern in der JSON-Datei:

| **Platzhalter** | **Beschreibung**               |
|--------------|-------------------------------|
| *****       | Repräsentiert eine beliebige Teilzeichenfolge      |
| **?**        | Steht für ein einzelnes Zeichen |

Zum Beispiel, wenn `"excludePaths" : [ "/news/*, /blog/*" ]` in dem obigen Beispiel gegeben ist, wird Ihre App alle Pfade unterstützen, die mit Ihrer Website-Adresse (z.B. msn.com) beginnen, **mit Ausnahme** der Pfade unter `/news/` und `/blog/`. **msn.com/weather.html** wird unterstützt, aber nicht ****msn.com/news/topnews.html****.


### Mehrere Apps

Wenn Sie zwei Apps haben, die Sie mit Ihrer Website verknüpfen möchten, listen Sie beide Paketfamiliennamen der Apps in der **Windows-App-Web-Link** JSON-Datei auf. Beide Apps können unterstützt werden. Dem Benutzer wird eine Auswahl für den Standardlink angezeigt, wenn beide Apps installiert sind. Falls sie den Standardlink später ändern möchten, können ändern sie ihn unter **Einstellungen > Apps für Websites** ändern. Entwickler können auch die JSON-Datei jederzeit ändern und die Änderung noch am selben Tag, aber bis spätestens acht Tage nach dem Update einsehen.

``` JSON
[{
  "packageFamilyName": "YourAppsPFN",
  "paths": [ "*" ],
  "excludedPaths" : [ "/news/*, /blog/*" ]
 },
 {
  "packageFamilyName": "Your2ndAppsPFN",
  "paths": [ "/example/*, /links/*" ]
 }]
```

Um Ihren Benutzern die bestmögliche Erfahrung zu bieten, benutzen Sie ausgeschlossene Pfade, um sicherzustellen, dass der nur online verfügbare Inhalt von den unterstützten Pfaden in der JSON-Datei ausgenommen ist.

Ausgeschlossene Pfade werden zuerst überprüft, und wenn eine Übereinstimmung vorliegt wird die entsprechende Seite mit dem Browser anstelle der angegebenen App geöffnet. Im obigen Beispiel enthält "/ News / \ *" alle Seiten unter diesem Pfad, während "/ News\ *" (keine vorwärts-Slash "news") Pfade unter "News\ *" wie "Newslocal /", "Newsinternational /" usw.. enthält.

## Behandeln Sie Links auf Aktivierung, um Links mit Inhalt zu verbinden.

Navigieren Sie zu **App.xaml.cs** in der Visual Studio Lösung für Ihrer App und fügen Sie in **OnActivated()** die Behandlung für verknüpfte Inhalte hinzu. Im folgenden Beispiel hängt die Seite, die in der App geöffnet wird, vom URI-Pfad ab:

``` CS
protected override void OnActivated(IActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame == null)
    {
        ...
    }

    // Check ActivationKind, Parse URI, and Navigate user to content
    Type deepLinkPageType = typeof(MainPage);
    if (e.Kind == ActivationKind.Protocol)
    {
        var protocolArgs = (ProtocolActivatedEventArgs)e;        
        switch (protocolArgs.Uri.AbsolutePath)
        {
            case "/":
                break;
            case "/index.html":
                break;
            case "/sports.html":
                deepLinkPageType = typeof(SportsPage);
                break;
            case "/technology.html":
                deepLinkPageType = typeof(TechnologyPage);
                break;
            case "/business.html":
                deepLinkPageType = typeof(BusinessPage);
                break;
            case "/science.html":
                deepLinkPageType = typeof(SciencePage);
                break;
        }
    }

    if (rootFrame.Content == null)
    {
        // Default navigation
        rootFrame.Navigate(deepLinkPageType, e);
    }

    // Ensure the current window is active
    Window.Current.Activate();
}
```

**Wichtig** Stellen Sie sicher, dass das finale `if (rootFrame.Content == null)` logic durch `rootFrame.Navigate(deepLinkPageType, e);` ersetzt wird, wie im obigen Beispiel gezeigt wird.

## Testen Sie: Lokales Überprüfungswerkzeug

Sie können die Konfiguration Ihrer App und Website durch Ausführen des App-Host Registration Verifier Werkzeugs prüfen, der hier verfügbar ist:

%windir%\\system32\\**AppHostRegistrationVerifier.exe**

Testen Sie die Konfiguration Ihrer App und, indem Sie dieses Werkzeug mit folgenden Parametern ausführen.

**AppHostRegistrationVerifier.exe** *hostname packagefamilyname filepath*

-   Hostname: Ihre Website (z. B. microsoft.com)
-   Paketfamiliennamen (PFN): Ihre App-PFN
-   Dateipfad: die JSON-Datei für die lokale Überprüfung (z. B. C:\\SomeFolder\\windows-App-Web-link)

## Testen Sie es: Web-Überprüfung

Schließen Sie die Anwendung, um sicherzustellen, dass die App aktiviert wird, wenn Sie auf einen Link klicken. Kopieren Sie dann die Adresse eines unterstützten Pfades in Ihrer Website. Wenn Ihre Websiteadresse beispielsweise "msn.com" lautet, und einer der unterstützen Pfade "path1" ist, verwenden Sie `http://msn.com/path1`

Stellen Sie sicher, dass Ihre app geschlossen ist. Drücken Sie die **Windows-Taste + R** zum Öffnen des **ausführen**-Dialogfelds fügen Sie den Link im Fenster ein. Ihre app sollte anstelle des Webbrowsers gestartet werden.

Darüber hinaus können Sie Ihre App testen, indem Sie sie über eine andere app mithilfe der [LaunchUriAsync](https://msdn.microsoft.com/en-us/library/windows/apps/hh701480.aspx) API starten. Diese API können auch Sie nutzen, um dies auf Telefonen zu testen.

Wenn Sie der protocol activation logic zu folgen möchten, legen Sie einen Haltepunkt im **OnActivated** -Ereignishandler fest.

**Hinweis:** Wenn Sie auf einen Link im Microsoft Edge Browser klicken, wird das nicht Ihre App starten, sondern Sie zu Ihrer Website führen.

## AppUriHandlers Tipps:

- Stellen Sie sicher, dass Sie nur Links angeben, die mit Ihrer App kompatibel sind.

- Listen Sie alle Hosts auf, die Sie unterstützen werden.  Beachten Sie, dass www.example.com und example.com verschiedene Hosts sind.

- Benutzer können in den Einstellungen auswählen, welche App sie zum Öffnen von Websites bevorzugen.

- Ihre JSON-Datei muss auf einen Https-Server hochgeladen werden.

- Wenn Sie die Pfade, die Sie unterstützen möchten, ändern müssen, können Sie JSON-Datei erneut hochladen, ohne Ihre App erneut veröffentlichen zu müssen. Benutzern wird die Änderungen in 1 bis 8 Tagen angezeigt.

- Alle quergeladenen Apps mit AppUriHandlern werden validierte Links für den Host on Install haben Sie müssen kein JSON-Datei hochgeladen haben, um das Feature zu testen.

- Dieses Feature funktioniert, wann immer Ihre App eine UWP-App ist, die mit  [LaunchUriAsync](https://msdn.microsoft.com/en-us/library/windows/apps/hh701480.aspx) gestartet ist, oder eine Windows-Desktop-App, gestartet mit  [ShellExecuteEx](https://msdn.microsoft.com/en-us/library/windows/desktop/bb762154(v=vs.85).aspx). Wenn die URL einen registrierten URI App Handler entspricht, wird die App anstelle des Browsers gestartet werden.

## Siehe auch

[Windows.Protocol-Registrierung](https://msdn.microsoft.com/en-us/library/windows/apps/br211458.aspx)

[Behandeln der URI-Aktivierung](https://msdn.microsoft.com/en-us/windows/uwp/launch-resume/handle-uri-activation)

[Das Association Launching Sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching) veranschaulicht die Verwendung des LaunchUriAsync() API.



<!--HONumber=Aug16_HO4-->


