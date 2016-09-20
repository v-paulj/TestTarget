---
author: normesta
description: Veranschaulicht die Integration von Social Media-Feeds in die Kontakte-App
MSHAttr: PreferredLib:/library/windows/apps
title: Bereitstellen von Social Media-Feeds in der Kontakte-App
translationtype: Human Translation
ms.sourcegitcommit: 767acdc847e1897cc17918ce7f49f9807681f4a3
ms.openlocfilehash: c5b9666d8654a4065bc0e4e400d3e47de4773b8b

---

# Bereitstellen von Social Media-Feeds in der Kontakte-App

Integrieren Sie Daten aus Social Media-Feeds aus Ihrer Datenbank in die Kontakte-App.

Die Feeddaten werden auf den **Neuigkeiten**-Seiten der Kontakte-App und auf der **Profil**-Seite von Kontakten angezeigt.

Benutzer können auf ein Element eines Feeds tippen, um Ihre App zu öffnen.

![Social Media-Feeds in der Kontakte-App](images/social-feeds.png)

Erstellen Sie zunächst eine Vordergrund-App, die Kontakte, für die Social Media-Feeds angezeigt werden sollen, markiert, sowie einen Hintergrund-Agent, der die Daten aus den Feeds an die der Kontakte-App überträgt.

Ein ausführlicheres Beispiel finden Sie unter [Beispiel für Informationen aus Social Media](https://github.com/Microsoft/Windows-Social-Samples/tree/master/SocialInfoSampleApp).

## Erstellen einer Vordergrund-App

Erstellen Sie zunächst ein UWP-Projekt und fügen dem Projekt dann die **Windows Mobile-Erweiterungen für UWP** hinzu.

![Mobile Erweiterungen](images/mobile-extensions.png)

### Suchen oder Erstellen von Kontakten

Sie können Kontakte über Namen, E-Mail-Adresse oder Telefonnummer suchen.

```cs
ContactStore contactStore = await ContactManager.RequestStoreAsync();

IReadOnlyList<Contact> contacts = null;

contacts = await contactStore.FindContactsAsync(emailAddress);

Contact contact = contacts[0];
```
Sie können auch Kontakte erstellen und diese anschließend einer Kontaktliste hinzufügen.

```cs
Contact contact = new Contact();
contact.FirstName = "TestContact";

ContactEmail email = new ContactEmail();
email.Address = "TestContact@contoso.com";
email.Kind = ContactEmailKind.Other;
contact.Emails.Add(email);

ContactPhone phone = new ContactPhone();
phone.Number = "4255550101";
phone.Kind = ContactPhoneKind.Mobile;
contact.Phones.Add(phone);

ContactStore store = await
    ContactManager.RequestStoreAsync(ContactStoreAccessType.AppContactsReadWrite);

ContactList contactList;

IReadOnlyList<ContactList> contactLists = await store.FindContactListsAsync();

if (0 == contactLists.Count)
    contactList = await store.CreateContactListAsync("TestContactList");
else
    contactList = contactLists[0];

await contactList.SaveContactAsync(contact);
```

### Kennzeichnen von Kontakten mit einer Anmerkung

Diese *Anmerkung* weist die Kontakte-App an, Feeddaten für den Kontakt von Ihrem Hintergrund-Agent anzufordern.

Wenn Sie diese Anmerkungen erstellen, ordnen Sie die ID des Kontakts einer ID zu, die Ihre App intern verwendet, um diesen Kontakt zu identifizieren.

```cs
ContactAnnotationStore annotationStore = await
   ContactManager.RequestAnnotationStoreAsync(ContactAnnotationStoreAccessType.AppAnnotationsReadWrite);

ContactAnnotationList annotationList;

IReadOnlyList<ContactAnnotationList> annotationLists = await annotationStore.FindAnnotationListsAsync();
if (0 == annotationLists.Count)
    annotationList = await annotationStore.CreateAnnotationListAsync();
else
    annotationList = annotationLists[0];

ContactAnnotation annotation = new ContactAnnotation();
annotation.ContactId = contact.Id;
annotation.RemoteId = "user22";

annotation.SupportedOperations = ContactAnnotationOperations.SocialFeeds;

await annotationList.TrySaveAnnotationAsync(annotation);

```
### Bereitstellen des Hintergrund-Agents

Stellen Sie sicher, dass der API-Vertrag [SocialInfoContract](https://msdn.microsoft.com/library/windows/apps/dn706146.aspx) auf dem Gerät, auf dem Ihre App ausgeführt werden soll, verfügbar ist.

Wenn sie verfügbar ist, stellen Sie den Hintergrund-Agent bereit.

```cs
if (Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
"Windows.ApplicationModel.SocialInfo.SocialInfoContract",
1))
{
    bool isProvisionSuccessful = await Windows.ApplicationModel.SocialInfo.Provider.SocialInfoProviderManager.ProvisionAsync();

    // Throw an exception if the app could not be provisioned
    if (!isProvisionSuccessful)
    {
        throw new Exception("Could not provision the app with the SocialInfoProviderManager");
    }
}
```
## Erstellen des Hintergrund-Agents

Der Hintergrund-Agent ist eine Komponente für Windows-Runtime, die auf Anfragen auf Datenfeeds aus der Kontakte-App antwortet.

In dem Agent werden Sie auf diese Anfragen reagieren, indem Sie Feeddaten aus Ihrer Datenbank an die Kontakte-App weiterleiten.

### Erstellen einer Komponente für Windows-Runtime

Fügen Sie der Projektmappe ein **Komponente für Windows-Runtime (Universal-Windows)**-Projekt hinzu.

![Komponente für Windows-Runtime](images/windows-runtime-component.png)

### Registrieren des Hintergrund-Agents als App-Dienst

Registrieren Sie die Vorgänge, indem Sie dem ``Extensions``-Element des Manifests Protokollhandler hinzufügen.

```xml
<Extensions>
  <uap:Extension Category="windows.appService" EntryPoint="SocialFeeds.BackgroundAgent">
    <uap:AppService Name="com.microsoft.windows.social-feeds" />
  </uap:Extension>
</Extensions>
```
Sie können diese auch in der Registerkarte **Deklarationen** im Manifest-Designer in Visual Studio hinzufügen.

![App-Dienst im Manifest-Designer](images/manifest-designer-app-service.png)

### Anfordern von Vorgängen in der Kontakte-App

Fragen Sie die Kontakte-App welche Art von Daten sie als Nächstes möchte. Die Kontakte-App antwortet auf Ihre Anfrage mit einem Code, der angibt, für welchen Feed sie Daten benötigt.

In der folgenden Tabelle werden diese Feeds beschrieben:

| Feed | Beschreibung |
|-------|-------------|
| Home | Ein Feed, der auf der Neuigkeiten-Seite der Kontakte-App angezeigt wird |
| Contact | Ein Feed, der auf der Neuigkeiten-Seite eines Kontakts angezeigt wird |
| Dashboard | Ein Feed, der auf der Visitenkarte neben dem Profilbild angezeigt wird |
<br>
Sie fordern die Kontakte-App an, indem Sie einen *Vorgang* anfordern. Implementieren Sie die [IBackgroundTask](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.aspx)-Schnittstelle, und überschreiben Sie die [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx)-Methode.

Übermitteln Sie in der [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx)-Methode zwei Schlüssel-Wert-Paare an die Kontakte-App. Das eine Paar enthält die Version des Protokolls und das andere den Typ des Vorgangs.

Horchen Sie dann auf eine Antwort von der Kontakte-App. Diese Antwort enthält einen Code.

```cs
public sealed class BackgroundAgent : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {
        BackgroundTaskDeferral backgroundTaskDeferral = taskInstance.GetDeferral();

        AppServiceTriggerDetails triggerDetails = taskInstance.TriggerDetails as AppServiceTriggerDetails;

        AppServiceConnection appServiceConnection = triggerDetails.AppServiceConnection;

        bool continueProcessing = true;

        while (continueProcessing)
        {
            // Get next operation.
            ValueSet fields =
                new ValueSet();

            fields.Add("Version",
                (1 << 16) | 0);

            fields.Add(
                "Type", 0x10);

            AppServiceResponse response =
                await appServiceConnection.SendMessageAsync(fields);

            if (response == null || response.Status != AppServiceResponseStatus.Success)
            { /* throw exception */ }

            UInt32 type = (UInt32)response.Message["Type"];

            switch (type)
            {
                //Get Next Operation.
                case 0x10:
                    break;
                //Download Home Feed Operation.
                case 0x11:
                    break;
                //Download Contact Feed Operation.
                case 0x13:
                    break;
                //Download Dashboard Feed Operation.
                case 0x15:
                    break;
                //Operation Result.
                case 0x80:
                    break;
                //Good Bye.
                case 0xF1:
                    continueProcessing = false;
                    break;
            }
        }
        // Complete the task deferral
        backgroundTaskDeferral.Complete();

        backgroundTaskDeferral = null;
        appServiceConnection = null;
    }

}
```

Sie finden diesen Code im Element ``Type`` der [AppServiceResponse.Message](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceresponse.message.aspx)-Eigenschaft und können ihn dort abrufen. Nachstehend finden Sie eine vollständige Liste der Codes.

| Typ| Beschreibung |
|-----|-------------|
| 0x10 | Eine Anfrage an die Kontakte-App, um den nächsten Vorgang abzurufen |
| 0x11 | Eine Anfrage der Kontakte-App, um den „Home“-Feed für den primären Benutzer bereitzustellen |
| 0x13 | Eine Anfrage der Kontakte-App, um den „Contact“-Feed für den ausgewählten Kontakt abzurufen |
| 0x15 | Eine Anfrage der Kontakte-App, um den „Dashboard“-Feed für den ausgewählten Kontakt abzurufen |
| 0x80 | Gibt an, dass der Vorgang abgeschlossen ist. Dies teilt der Kontakte-App mit, dass die Daten jetzt verfügbar sind. |
| 0xF1 | Eine Nachricht von der Kontakte-App, die angibt, dass keine anderen Vorgänge erforderlich sind. Der Hintergrund-Agent kann jetzt beendet werden. |
<br>
Die Eigenschaft [AppServiceResponse.Message](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceresponse.message.aspx) gibt außerdem eine Sammlung von weiteren Schlüssel-Wert-Paaren zurück, die die Antwort beinhalten. Nachstehend finden Sie eine Liste der Antworten.

| Schlüssel | Typ | Beschreibung |
|-----|------|-------------|
| Version | UINT32 | (Erforderlich) Gibt die Version des Nachrichtenprotokolls an. Die oberen 16 Bit enthalten die Hauptversion, und die unteren 16 Bit die Nebenversion. |
| Typ | UINT32 | (Erforderlich) Der Typ des auszuführenden Vorgangs. In dem Beispiel oben wird der Schlüssel „Type“ verwendet, um zu ermitteln, welchen Vorgang die Kontakte-App anfordert.
| OperationId | UINT32 | Die ID des Vorgangs. |
| OwnerRemoteId | Zeichenfolge | Die ID, die Ihre App intern verwendet, um diesen Kontakt zu identifizieren |
| LastFeedItemTimeStamp | Zeichenfolge | Die ID des letzten Feedelements, das abgerufen wurde |
| LastFeedItemTimeStamp | DateTime | Der Zeitstempel des letzten Feedelements, das abgerufen wurde |
| ItemCount | UINT32 | Die Anzahl der Elemente, die die Kontakte-App anfordert |
| IsFetchMore | BOOLEAN | Bestimmt, wann der interne Cache aktualisiert wird |
| ErrorCode | UINT32 | Der Fehlercode, der dem Vorgang des Hintergrund-Agents zugeordnet ist. |
<br>
### Bereitstellen ein Datenfeeds für die Kontakte-App

Wenn der Wert für **Type** ``0x11``, ``0x13`` oder ``0x15`` lautet, ist dies eine Anfrage der Kontakte-App auf Feeddaten.  

Die nächsten Codeausschnitte zeigen einen Ansatz zum Bereitstellen dieser Daten für die Kontakte-App.

> [!NOTE]
> Diese Codeausschnitte stammen aus den [Beispiel für Informationen aus Social Media](https://github.com/Microsoft/Windows-Social-Samples/tree/master/SocialInfoSampleApp). Sie enthalten Verweise auf Schnittstellen, Klassen und Mitglieder, die an anderer Stelle im Beispiel definiert werden. Verwenden Sie diese Codeausschnitte zusammen mit den anderen Beispielen in diesem Thema, um den Ablauf der Aufgaben zu verstehen. Sie können das Beispiel auch nutzen, um tiefer in diesen Steck von Schnittstellen, Klassen und Typen einzutauchen.

**Abrufen der Feedelemente von Kontakten**

```cs
public override async Task DownloadFeedAsync()
{
    // Get the contact feeds
    IEnumerable<FeedItem> contactFeedItems =
        InMemorySocialCache.Instance.GetContactFeeds(OwnerRemoteId, ItemCount);

    // Check if the platform supports the SocialInfo APIs
    if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
         "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
    {

        return;
    }

    // Create the social feed updater.
    SocialFeedUpdater feedUpdater = await SocialInfoProviderManager.CreateSocialFeedUpdaterAsync(
        SocialFeedKind.ContactFeed,
        SocialFeedUpdateMode.Replace,
        OwnerRemoteId);

    // Translate the sample feed into Social info feed items.
    foreach (FeedItem fi in contactFeedItems)
    {
        SocialFeedItem item = new SocialFeedItem();

        item.Timestamp = fi.Timestamp;
        item.RemoteId = fi.Id;
        item.TargetUri = fi.TargetUri;
        item.Author.DisplayName = fi.AuthorDisplayName;
        item.Author.RemoteId = fi.AuthorId;
        item.PrimaryContent.Title = fi.Title;
        item.PrimaryContent.Message = fi.Message;

        if (fi.ImageUri != null)
        {
            item.Thumbnails.Add(new SocialItemThumbnail()
            {
                TargetUri = fi.TargetUri,
                ImageUri = fi.ImageUri
            });
        }

        feedUpdater.Items.Add(item);
    }

    await feedUpdater.CommitAsync();
}
```

**Abrufen der Dashboardelemente**

```cs
public override async Task DownloadFeedAsync()
{
    // Get the dashboard feed item from your database.
    FeedItem dashboardFeedItem = InMemorySocialCache.Instance.GetDashboardFeed(OwnerRemoteId);

    if (dashboardFeedItem != null)
    {
        // Check if the platform supports the social info APIs.
        if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
             "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
        {

            return;
        }

        SocialDashboardItemUpdater dashboard =
            await SocialInfoProviderManager.CreateDashboardItemUpdaterAsync(OwnerRemoteId);

        dashboard.Content.Message = dashboardFeedItem.Message;
        dashboard.Content.Title = dashboardFeedItem.Title;
        dashboard.Timestamp = dashboardFeedItem.Timestamp;

        // The TargetUri of the dashboard always has to be set.
        dashboard.TargetUri = dashboardFeedItem.TargetUri;

        // For a thumbnail, there must always be a target Uri.
        if ((dashboardFeedItem.ImageUri != null) && (dashboardFeedItem.TargetUri != null))
        {
            dashboard.Thumbnail = new SocialItemThumbnail()
            {
                ImageUri = dashboardFeedItem.ImageUri,
                TargetUri = dashboardFeedItem.TargetUri
            };
        }

        await dashboard.CommitAsync();
    }
}
```

**Abrufen der Feedelemente für den Startbildschirm**

```cs
public override async Task DownloadFeedAsync()
{
    // Query the "database" for the home feeds.
    IEnumerable<FeedItem> homeFeedItems =
        InMemorySocialCache.Instance.GetHomeFeeds(OwnerRemoteId, ItemCount);

    // Check if the platform supports the social info APIs.
    if (!Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent(
         "Windows.ApplicationModel.SocialInfo.SocialInfoContract", 1))
    {

        return;
    }

    // Create the social feed updater.
    SocialFeedUpdater feedUpdater = await SocialInfoProviderManager.CreateSocialFeedUpdaterAsync(
        SocialFeedKind.HomeFeed,
        SocialFeedUpdateMode.Replace,
        OwnerRemoteId);

    // Generate each of the feed items.
    foreach (FeedItem fi in homeFeedItems)
    {
        SocialFeedItem item = new SocialFeedItem();

        item.Timestamp = fi.Timestamp;
        item.RemoteId = fi.Id;
        item.TargetUri = fi.TargetUri;
        item.Author.DisplayName = fi.AuthorDisplayName;
        item.Author.RemoteId = fi.AuthorId;
        item.PrimaryContent.Title = fi.Title;
        item.PrimaryContent.Message = fi.Message;

        if (fi.ImageUri != null)
        {
            item.Thumbnails.Add(new SocialItemThumbnail()
            {
                TargetUri = fi.TargetUri,
                ImageUri = fi.ImageUri
            });
        }

        feedUpdater.Items.Add(item);
    }

    await feedUpdater.CommitAsync();
}
```

### Zurücksenden einer Benachrichtigung über Erfolg oder Fehler an die Kontakte-App

Kapseln Sie Ihre Anrufe in einem Try-Catch-Block und geben Sie dann eine Nachricht über Erfolg oder Fehler an die Kontakte-App zurück, nachdem Sie die Feeddaten bereitgestellt haben.

```cs
try
{
    // Calls to methods that fetch data and populate feed.
}
catch (Exception exception)
{
    errorCode = (UInt32)exception.HResult;
}

// Send status back to the people app.
ValueSet fields =
    new ValueSet();

fields.Add("ErrorCode", errorCode);

UInt32 operationID = (UInt32)response.Message["OperationId"];

fields.Add("OperationId", operationID);

await this.mAppServiceConnection.SendMessageAsync(fields);

```



<!--HONumber=Aug16_HO4-->


