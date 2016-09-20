---
title: "Verbinden mit Identitätsanbietern über Web Account Manager"
description: "In diesem Artikel wird beschrieben, wie Sie AccountsSettingsPane verwenden, um Ihre App für die universelle Windows-Plattform (UWP) mit externen Identitätsanbietern wie Microsoft oder Facebook zu verbinden. Dazu verwenden Sie die neuen Web Account Manager-APIs in Windows 10."
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: f3cdb187ec4056d4c7db6acde471b0bc91c78390
ms.openlocfilehash: 093ca8906853121bbf33a729c523717d26cb7b0d

---
# Verbinden mit Identitätsanbietern über Web Account Manager

In diesem Artikel wird beschrieben, wie Sie AccountsSettingsPane anzeigen und Ihre App für die universelle Windows-Plattform (UWP) mit externen Identitätsanbietern wie Microsoft oder Facebook verbinden. Dazu verwenden Sie die neuen Web Account Manager-APIs in Windows 10. Sie erfahren, wie Sie die Berechtigung eines Benutzers für die Verwendung seines Microsoft-Kontos anfordern können, ein Zugriffstoken erhalten und damit grundlegende Vorgänge (wie das Abrufen von Profildaten oder das Hochladen von Dateien in das OneDrive-Verzeichnis des Benutzers) durchführen können. Die Schritte ähneln denen, die zum Abrufen einer Benutzerberechtigung und für den Zugriff über einen beliebigen Identitätsanbieter ausgeführt werden, der Web Account Manager unterstützt.

> Hinweis: Ein vollständiges Codebeispiel finden Sie im [WebAccountManagement-Beispiel auf GitHub](http://go.microsoft.com/fwlink/p/?LinkId=620621).

## Vorbereitung

Erstellen Sie zunächst eine neue leere App in Visual Studio. 

Um eine Verbindung mit Identitätsanbietern herzustellen, müssen Sie die App als Nächstes mit dem Store verknüpfen. Klicken Sie dazu mit der rechten Maustaste auf das Projekt, wählen Sie **Store** > **App mit Store verknüpfen** aus, und folgen Sie den Anweisungen des Assistenten. 

Erstellen Sie drittens eine allgemein verwendete Benutzeroberfläche, die eine einfache XAML-Schaltfläche und zwei Textfelder umfasst.

```XML
<StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
    <Button x:Name="LoginButton" Content="Log in" Click="LoginButton_Click" />
    <TextBlock x:Name="UserIdTextBlock"/>
    <TextBlock x:Name="UserNameTextBlock"/>
</StackPanel>
```

Im CodeBehind ist ein Ereignishandler an die Schaltfläche angefügt:

```C#
private void LoginButton_Click(object sender, RoutedEventArgs e)
{   
}
```

Fügen Sie zum Schluss die folgenden Namespaces hinzu, damit später keine Verweisprobleme auftreten: 

```C#
using System;
using Windows.Security.Authentication.Web.Core;
using Windows.System;
using Windows.UI.ApplicationSettings;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.Data.Json;
using Windows.UI.Xaml.Navigation;
using Windows.Web.Http;
```

## Anzeigen von AccountSettingsPane

Das System bietet mit AccountSettingsPane eine integrierte Benutzeroberfläche für die Verwaltung von Identitätsanbietern und Webkonten. Sie können sie wie folgt anzeigen:

```C#
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    AccountsSettingsPane.Show(); 
}
```

Wenn Sie Ihre App ausführen und auf die Anmeldeschaltfläche klicken, sollte ein leeres Fenster angezeigt werden. 

![Bereich mit Kontoeinstellungen](images/tb-1.png)

Der Bereich ist leer, weil das System nur eine UI-Shell bereitstellt. Der Entwickler kann den Bereich programmgesteuert mit Identitätsanbietern auffüllen. 

## Registrieren für AccountCommandsRequested

Um dem Bereich Befehle hinzuzufügen, führen wir zunächst eine Registrierung für den AccountCommandsRequested-Ereignishandler durch. Dadurch wird das System angewiesen, unsere Buildlogik auszuführen, wenn der Benutzer die Anzeige des Bereichs anfordert (d. h. auf die XAML-Schaltfläche klickt). 

Überschreiben Sie im CodeBehind das OnNavigatedTo-Ereignis und das OnNavigatedFrom-Ereignis, und fügen Sie ihnen den folgenden Code hinzu: 

```C#
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested += BuildPaneAsync; 
}
```

```C#
protected override void OnNavigatedFrom(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested -= BuildPaneAsync; 
}
```

Da Benutzer nicht sehr häufig mit Konten interagieren, empfiehlt es sich, den Ereignishandler in der beschriebenen Weise zu registrieren bzw. seine Registrierung aufzuheben, um Arbeitsspeicherverluste zu vermeiden. Dadurch wird der angepasste Bereich nur im Arbeitsspeicher vorgehalten, wenn eine hohe Wahrscheinlichkeit vorliegt, dass er vom Benutzer angefordert wird (der Benutzer befindet sich z. B. auf der Seite „Einstellungen“ oder „Anmelden“). 

## Erstellen des Bereichs mit Kontoeinstellungen

Die BuildPaneAsync-Methode wird immer dann aufgerufen, wenn AccountSettingsPane angezeigt wird. Dort legen wir den Code zur Anpassung der im Bereich angezeigten Befehle ab. 

Sie beginnen, indem Sie eine Verzögerung abrufen. Dadurch wird das System angewiesen, die Anzeige von AccountsSettingsPane so lange zu verzögern, bis der Buildvorgang abgeschlossen ist.

```C#
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    deferral.Complete(); 
}
```

Als Nächstes rufen Sie einen Anbieter mit der WebAuthenticationCoreManager.FindAccountProviderAsync-Methode ab. Die Anbieter-URL ist je nach Anbieter verschieden und kann in der Anbieterdokumentation nachgeschlagen werden. Für Microsoft-Konten und Azure Active Directory lautet die URL „https://login.microsoft.com“. 

```C#
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    var msaProvider = await WebAuthenticationCoreManager.FindAccountProviderAsync(
        "https://login.microsoft.com", "consumers"); 
        
    deferral.Complete(); 
}
```

Beachten Sie, dass auch die Zeichenfolge „consumers“ an den optionalen *authority*-Parameter übergeben wird. Dies liegt daran, dass Microsoft zwei verschiedene Authentifizierungsmethoden bereitstellt: Microsoft-Konten (MSA) für „Consumer“ und Azure Active Directory (AAD) für „Organisationen“. Durch die Autorität „consumers“ wird dem Anbieter mitgeteilt, dass die erste Methode bevorzugt wird.

Wenn Sie eine Unternehmens-App entwickeln, können Sie stattdessen den AAD Graph-Endpunkt verwenden. Weitere Informationen finden Sie im vollständigen [WebAccountManagement-Beispiel auf GitHub](http://go.microsoft.com/fwlink/p/?LinkId=620621) und in der Azure-Dokumentation. 

Schließlich fügen Sie AccountsSettingsPane den Anbieter hinzu, indem Sie wie folgt einen neuen WebAccountProviderCommand erstellen: 

```C#
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();

    var msaProvider = await WebAuthenticationCoreManager.FindAccountProviderAsync(
        "https://login.microsoft.com", "consumers");

    var command = new WebAccountProviderCommand(msaProvider, GetMsaTokenAsync);  

    e.WebAccountProviderCommands.Add(command);

    deferral.Complete(); 
}
```

Beachten Sie, dass die an unseren neuen WebAccountProviderCommand übergebene GetMsaToken-Methode noch nicht vorhanden ist (da sie erst im nächsten Schritt erstellt wird). Sie können sie vorläufig als leere Methode hinzufügen.

Führen Sie den vorangehenden Code aus. Der Bereich sollte jetzt wie folgt aussehen: 

![Bereich mit Kontoeinstellungen](images/tb-2.png)

### Anfordern eines Tokens

Sobald die Microsoft-Konto-Option in AccountsSettingsPane angezeigt wird, müssen wir festlegen, was bei der Auswahl der Option durch den Benutzer passiert. Die GetMsaToken-Methode wurde so registriert, dass sie ausgelöst wird, wenn sich der Benutzer für die Anmeldung mit seinem Microsoft-Konto entscheidet. Deshalb fordern wir an dieser Stelle das Token an. 

Um ein Token anzufordern, verwenden Sie die RequestTokenAsync-Methode wie folgt: 

```C#
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

In diesem Beispiel wird die Zeichenfolge „wl.basic“ an den scope-Parameter übergeben. „scope“ steht für den Typ von Informationen, die Sie vom bereitstellenden Dienst für einen bestimmten Benutzer anfordern. Bestimmte Bereiche ermöglichen nur den Zugriff auf grundlegende Informationen eines Benutzers wie den Namen und die E-Mail-Adresse. Andere Bereiche können dagegen Zugriff auf vertrauliche Informationen gewähren wie die Fotos oder den E-Mail-Posteingang des Benutzers. Normalerweise sollte die App den Bereich mit den geringsten Berechtigungen verwenden, sofern sie nicht explizit eine zusätzliche Berechtigung benötigt. Beispielsweise sollte kein Zugriff auf vertrauliche Informationen angefordert werden, wenn diese von der App nicht unbedingt benötigt werden. 

Dienstanbieter stellen in ihrer Dokumentation Informationen dazu bereit, welche Bereiche angegeben werden müssen, um die für den betreffenden Dienst zu verwendenden Token anzufordern. 

Informationen zu den Bereichen Office 365 und Outlook.com finden Sie unter (Authentifizieren von Office 365- und Outlook.com-APIs mit dem v2.0-Authentifizierungsendpunkt)[https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2]. 

Informationen zu OneDrive finden Sie unter (Authentifizierung und Anmeldung bei OneDrive)[https://dev.onedrive.com/auth/msa_oauth.htm#authentication-scopes]. 

## Verwenden des Tokens

Die RequestTokenAsync-Methode sendet ein WebTokenRequestResult-Objekt zurück, das die Ergebnisse für Ihre Anforderung enthält. Wenn die Anforderung erfolgreich war, enthält sie ein Token.  

```C#
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        string token = result.ResponseData[0].Token; 
    }
}
```

> Hinweis: Wenn Sie eine Fehlermeldung erhalten, wenn ein Token angefordert haben, stellen Sie sicher, dass Sie Ihre App im Store wie in Schritt 1 beschrieben zugeordnet haben. Ihre App wird nicht in der Lage sein, ein Token abzurufen, wenn Sie diesen Schritt übersprungen haben. 

Sobald Sie im Besitz des Tokens sind, können Sie darüber die API Ihres Anbieters aufrufen. Im folgenden Code werden Microsoft Live-APIs aufgerufen, um grundlegende Informationen zum Benutzer zu erhalten und sie auf der Benutzeroberfläche anzuzeigen. 

```C#
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        string token = result.ResponseData[0].Token; 
        
        var restApi = new Uri(@"https://apis.live.net/v5.0/me?access_token=" + token);

        using (var client = new HttpClient())
        {
            var infoResult = await client.GetAsync(restApi);
            string content = await infoResult.Content.ReadAsStringAsync();

            var jsonObject = JsonObject.Parse(content);
            string id = jsonObject["id"].GetString();
            string name = jsonObject["name"].GetString();

            UserIdTextBlock.Text = "Id: " + id; 
            UserNameTextBlock.Text = "Name: " + name;
        }
    }
}
```

Die Methode zum Aufrufen verschiedener REST-APIs ist je nach Anbieter verschieden. Informationen zur Verwendung des Tokens finden Sie in der API-Dokumentation des Anbieters. 

## Speichern des Kontozustands

Token sind hilfreich, um sofort Informationen über einen Benutzer abzurufen, in der Regel haben sie jedoch unterschiedliche Ablaufzeiten. MSA-Token sind beispielsweise nur wenige Stunden gültig. Sie müssen AccountsSettingsPane jedoch nicht jedes Mal erneut anzeigen, wenn ein Token abläuft. Nachdem Ihre App einmal von einem Benutzer autorisiert wurde, können Sie die Kontoinformationen des Benutzers für die zukünftige Verwendung speichern. 

Zu diesem Zweck verwenden Sie die WebAccount-Klasse. Ein WebAccount wird zeitgleich mit einer Tokenanforderung zurückgegeben:

```C#
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        WebAccount account = result.ResponseData[0].WebAccount; 
    }
}
```

Sobald Sie über ein WebAccount verfügen, können Sie es ganz einfach speichern. Im folgenden Beispiel verwenden wir LocalSettings: 

```C#
private async void StoreWebAccount(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"] = account.WebAccountProvider.Id;
    ApplicationData.Current.LocalSettings.Values["CurrentUserId"] = account.Id; 
}
```

Wenn der Benutzer die App das nächste Mal startet, können Sie wie folgt versuchen, ein Token im Hintergrund abzurufen: 

```C#
private async Task<string> GetTokenSilentlyAsync()
{
    string providerId = ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"]?.ToString();
    string accountId = ApplicationData.Current.LocalSettings.Values["CurrentUserId"]?.ToString();

    if (null == providerId || null == accountId)
    {
        return null; 
    }

    WebAccountProvider provider = await WebAuthenticationCoreManager.FindAccountProviderAsync(providerId);
    WebAccount account = await WebAuthenticationCoreManager.FindAccountAsync(provider, accountId);

    WebTokenRequest request = new WebTokenRequest(provider, "wl.basic");

    WebTokenRequestResult result = await WebAuthenticationCoreManager.GetTokenSilentlyAsync(request, account);
    if (result.ResponseStatus == WebTokenRequestStatus.UserInteractionRequired)
    {
        // Unable to get a token silently - you'll need to show the UI
        return null; 
    }
    else if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        // Success
        return result.ResponseData[0].Token;
    }
    else
    {
        // Other error 
        return null; 
    }
}
```

Da der Tokenabruf im Hintergrund sehr einfach ist, sollten Sie diese Methode verwenden, um Ihr Token zwischen Sitzungen zu aktualisieren, anstatt ein vorhandenes Token zwischenzuspeichern (da das Token jederzeit ablaufen kann).

Beachten Sie, dass durch das vorangehende Beispiel nur einfache Erfolgs- und Fehlerereignisse dargestellt werden. Ihre App sollte auch außergewöhnliche Szenarien berücksichtigen (z. B. wenn ein Benutzer die Berechtigung für die App widerruft oder das Konto aus Windows entfernt) und sie angemessen behandeln.  

## Abmelden von einem Konto 

Wenn Sie ein WebAccount beibehalten, können Sie den Benutzern eine Abmeldefunktion zur Verfügung stellen, damit sie Konten wechseln oder die Zuordnung ihres Kontos zur App aufheben können. Entfernen Sie zuerst alle gespeicherten Konto- und Anbieterinformationen. Rufen Sie dann WebAccount.SignOutAsync() auf, um den Cache zu löschen und vorhandene App-Tokens für ungültig zu erklären. 

```C#
private async Task SignOutAccountAsync(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserProviderId");
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserId"); 
    account.SignOutAsync(); 
}
```

## Hinzufügen von Anbietern, die WebAccountManager nicht unterstützen

Wenn Sie die Authentifizierung über einen Dienst in Ihre App integrieren möchten, der Dienst WebAccountManager jedoch nicht unterstützt (z. B. Google+ oder Twitter), können Sie AccountsSettingsPane den Anbieter trotzdem manuell hinzufügen. Erstellen Sie dazu ein neues WebAccountProvider-Objekt unter Angabe Ihres eigenen Namens und PNG-Symbols. Fügen Sie das Objekt dann WebAccountProviderCommands hinzu. Stubcode-Beispiel: 

 ```C#
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 

    var twitterProvider = new WebAccountProvider("twitter", "Twitter", new Uri(@"ms-appx:///Assets/twitter-auth-icon.png")); 
    var twitterCmd = new WebAccountProviderCommand(twitterProvider, GetTwitterTokenAsync);
    e.WebAccountProviderCommands.Add(twitterCmd);   
    
    // other code here
}

private async void GetTwitterTokenAsync(WebAccountProviderCommand command)
{
    // Manually handle Twitter login here
}

```

Dadurch wird AccountsSettingsPane lediglich ein Symbol hinzugefügt und die Methode ausgeführt, die beim Klicken auf das Symbol ausgeführt werden soll (in diesem Fall GetTwitterTokenAsync). Sie müssen den Code angeben, durch den die eigentliche Authentifizierung behandelt wird. Weitere Informationen finden Sie unter (Webauthentifizierungsbroker)[web-authentication-broker], der Hilfsmethoden für die Authentifizierung mithilfe von REST-Diensten bereitstellt. 

## Hinzufügen eines benutzerdefinierten Headers

Sie können den Bereich mit den Kontoeinstellungen mithilfe der HeaderText-Eigenschaft wie folgt anpassen: 

```C#
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    args.HeaderText = "MyAwesomeApp works best if you're signed in.";   
    
    // other code here
}
```

![Bereich mit Kontoeinstellungen](images/tb-3.png)

Halten Sie Headertext kurz und einfach, und vermeiden Sie überflüssige Informationen. Wenn der Anmeldevorgang komplex ist und weitere Informationen angezeigt werden müssen, verknüpfen Sie den Benutzer über einen benutzerdefinierten Link mit einer separaten Seite. 

## Hinzufügen von benutzerdefinierten Links

Sie können dem AccountsSettingsPane benutzerdefinierte Befehle hinzufügen, die als Links unter den unterstützten WebAccountProviders angezeigt werden. Benutzerdefinierte Befehle eignen sich hervorragend für einfache Aufgaben in Verbindung mit Benutzerkonten, z. B. das Anzeigen einer Datenschutzrichtlinie oder das Öffnen einer Supportseite, wenn auf Benutzerseite ein Problem auftritt. 

Beispiel: 

```C#
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    var settingsCmd = new SettingsCommand(
        "settings_privacy", 
        "Privacy policy", 
        async (x) => await Launcher.LaunchUriAsync(new Uri(@"https://privacy.microsoft.com/en-US/"))); 

    e.Commands.Add(settingsCmd); 
    
    // other code here
}
```

![Bereich mit Kontoeinstellungen](images/tb-4.png)

Einstellungsbefehle lassen sich grundsätzlich überall verwenden. Es wird jedoch empfohlen, diese Art Befehle auf die oben beschriebenen intuitiven Kontoszenarien zu beschränken. 

## Siehe auch

[Windows.Security.Authentication.Web.Core-Namespace](https://msdn.microsoft.com/library/windows/apps/windows.security.authentication.web.core.aspx)

[Windows.Security.Credentials-Namespace](https://msdn.microsoft.com/library/windows/apps/windows.security.credentials.aspx)

[AccountsSettingsPane](https://msdn.microsoft.com/library/windows/apps/windows.ui.applicationsettings.accountssettingspane)

[Webauthentifizierungsbroker](web-authentication-broker.md)

[WebAccountManagement-Beispiel](http://go.microsoft.com/fwlink/p/?LinkId=620621)



<!--HONumber=Aug16_HO3-->


