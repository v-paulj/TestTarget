---
author: normesta
description: "Veranschaulicht das Hinzufügen Ihrer App neben Aktionen in einer Visitenkarte"
MSHAttr: PreferredLib:/library/windows/apps
title: Verbinden der App mit Aktionen auf einer Visitenkarte
translationtype: Human Translation
ms.sourcegitcommit: 5c0f6ef1f1a346a66ca554a415d9f24c8a314ae1
ms.openlocfilehash: 034dc2b7be69763416192014abe24b9bf924c443

---

# Verbinden der App mit Aktionen auf einer Visitenkarte

Ihre App kann neben Aktionen auf einer Visitenkarte oder kleinen Kontaktkarte angezeigt werden. Benutzer können Ihre App auswählen, um eine Aktion auszuführen, z. B. eine Profilseite zu öffnen, einen Anruf zu tätigen oder eine Nachricht zu senden.

![Visitenkarte und kleine Kontaktkarte](images/all-contact-cards.png)

Suchen Sie zunächst vorhandene Kontakte oder erstellen Sie neue. Erstellen Sie dann eine *Anmerkung* und einige Paketmanifesteinträge, die beschreiben, welche Aktionen die App unterstützt. Anschließend schreiben Sie Code, der die Aktionen ausführt.

Ein ausführlicheres Beispiel finden Sie unter [Beispiel für die Visitenkarte-Integration](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration).

## Suchen oder Erstellen eines Kontakts

Wenn Ihre App den Kontakt mit anderen Personen unterstützt, durchsuchen Sie Windows nach Kontakten, und versehen Sie sie mit Anmerkungen. Wenn Ihre App Kontakte verwaltet, können Sie sie einer Windows-Kontaktliste hinzufügen und mit Anmerkungen versehen.

### Suchen nach einem Kontakt

Suchen Sie Kontakte über Namen, E-Mail-Adresse oder Telefonnummer.

```cs
ContactStore contactStore = await ContactManager.RequestStoreAsync();

IReadOnlyList<Contact> contacts = null;

contacts = await contactStore.FindContactsAsync(emailAddress);

Contact contact = contacts[0];
```

### Erstellen eines Kontakts

Wenn Ihre App eher wie ein Adressbuch konzipiert ist, erstellen Sie Kontakte und fügen Sie sie dann einer Kontaktliste zu.

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

## Kennzeichnen von Kontakten mit einer Anmerkung

Kennzeichnen Sie alle Kontakte mit einer Liste von Aktionen (Vorgänge), die Ihre App ausführen kann (Beispiel: Videoanrufe und Messaging).

Ordnen Sie anschließend die ID der Kontakte einer ID zu, anhand derer Ihre App den Benutzer intern identifizieren kann.

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

annotation.SupportedOperations = ContactAnnotationOperations.Message |
  ContactAnnotationOperations.AudioCall |
  ContactAnnotationOperations.VideoCall |
 ContactAnnotationOperations.ContactProfile;

await annotationList.TrySaveAnnotationAsync(annotation);
```

## Registrieren der Vorgänge

Registrieren Sie in Ihrem Paketmanifest alle Vorgänge, die Sie in der Anmerkung aufgeführt haben.

Registrieren Sie die Vorgänge, indem Sie dem ``Extensions``-Element des Manifests Protokollhandler hinzufügen.

```xml
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-contact-profile">
      <uap:DisplayName>TestProfileApp</uap:DisplayName>
    </uap:Protocol>
  </uap:Extension>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-ipmessaging">
      <uap:DisplayName>TestMsgApp</uap:DisplayName>
    </uap:Protocol>
  </uap:Extension>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-voip-video">
      <uap:DisplayName>TestVideoApp</uap:DisplayName>
    </uap:Protocol>
  </uap:Extension>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-voip-call">
      <uap:DisplayName>TestCallApp</uap:DisplayName>
    </uap:Protocol>
  </uap:Extension>
</Extensions>
```
Sie können diese auch in der Registerkarte **Deklarationen** im Manifest-Designer in Visual Studio hinzufügen.

![Registerkarte „Deklarationen“ im Manifest-Designer](images/manifest-designer-protocols.png)

## Anzeigen Ihrer App neben Aktionen in einer Visitenkarte

Öffnen Sie die Kontakte-App. Ihre App wird neben den Aktionen (Operationen) angezeigt, die Sie in Ihrer Anmerkung und dem Paketmanifest angegeben haben.

![Visitenkarte](images/a-contact-card.png)

Wenn Benutzer Ihre App für eine Aktion auswählen, wird diese App, wenn ein Benutzer das nächste Mal eine Visitenkarte öffnet, als Standard-App für diese Aktion angezeigt.

## Anzeigen Ihrer App neben Aktionen in einer kleinen Kontaktkarte

In kleinen Kontaktkarten wird Ihre App in Registerkarten angezeigt, die Aktionen darstellen.

![Kleine Kontaktkarte](images/mini-contact-card.png)

Apps wie **Mail** können kleine Kontaktkarten öffnen. Ihre App kann sie auch öffnen. Dieser Code zeigt, wie dies geht:

```cs
public async void OpenContactCard(object sender, RoutedEventArgs e)
{
    // Get the selection rect of the button pressed to show contact card.
    FrameworkElement element = (FrameworkElement)sender;

    Windows.UI.Xaml.Media.GeneralTransform buttonTransform = element.TransformToVisual(null);
    Windows.Foundation.Point point = buttonTransform.TransformPoint(new Windows.Foundation.Point());
    Windows.Foundation.Rect rect =
        new Windows.Foundation.Rect(point, new Windows.Foundation.Size(element.ActualWidth, element.ActualHeight));

   // helper method to find a contact just for illustrative purposes.
    Contact contact = await findContact("contoso@contoso.com");

    ContactManager.ShowContactCard(contact, rect, Windows.UI.Popups.Placement.Default);

}
```

Weitere Beispiele mit kleinen Kontaktkarten finden Sie unter [Visitenkartenbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCards).

Genau wie die Visitenkarte wird für jede Registerkarte die App vermerkt, die der Benutzer zuletzt verwendet hat, damit sie problemlos zu Ihrer App zurückkehren können.

## Ausführen von Vorgängen, wenn Benutzer Ihre App in einer Visitenkarte auswählen

Überschreiben Sie die [Application.OnActivated](https://msdn.microsoft.com/library/windows/apps/br242330)-Methode in Ihrer **App.cs**-Datei, und leiten Sie Benutzer zu einer Seite in Ihrer App. Das [Beispiel für die Integration von Visitenkarten](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration) zeigt eine Möglichkeit, das zu tun.

Überschreiben Sie in dem Code für die Datei der Seite, die [Page.OnNavigatedTo](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.page.onnavigatedto.aspx)-Methode. Die Visitenkarte übergibt dieser Methode den Namen des Vorgangs und die ID des Benutzers.

Informationen zum Starten eines Video- oder Audioanrufs finden Sie in dem [VoIP-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/VoIP). Sie finden die vollständige API im Namespace [Windows.ApplicationModel.Calls](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.calls.aspx).

Informationen zur Vereinfachung des Messaging finden Sie im Namespace [Windows.ApplicationModel.Chat](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.chat.aspx).

Sie können auch eine andere App starten. Dieser Code führt folgende Aktionen aus:

```cs
protected override async void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    var args = e.Parameter as ProtocolActivatedEventArgs;
    // Display the result of the protocol activation if we got here as a result of being activated for a protocol.

    if (args != null)
    {
        var options = new Windows.System.LauncherOptions();
        options.DisplayApplicationPicker = true;

        options.TargetApplicationPackageFamilyName = “ContosoApp”;

        string launchString = args.uri.Scheme + ":" + args.uri.Query;
        var launchUri = new Uri(launchString);
        await Windows.System.Launcher.LaunchUriAsync(launchUri, options);
    }
}
```

Die ```args.uri.scheme```-Eigenschaft enthält den Namen des Vorgangs, und die ```args.uri.Query```-Eigenschaft enthält die ID des Benutzers.



<!--HONumber=Aug16_HO3-->


