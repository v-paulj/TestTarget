---
author: Xansky
description: "Mit dem Windows.ApplicationModel.Contacts-Namespace verfügen Sie über mehrere Optionen zum Auswählen von Kontakten."
title: "Auswählen von Kontakten"
ms.assetid: 35FEDEE6-2B0E-4391-84BA-5E9191D4E442
keywords: contact, selecting specific fields
translationtype: Human Translation
ms.sourcegitcommit: 50c37d71d3455fc2417d70f04e08a9daff2e881e
ms.openlocfilehash: 6f2c6a546ed3daa0ef0311bc54ca47f31d01f3d8

---

# Auswählen von Kontakten

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \].


Mit dem [**Windows.ApplicationModel.Contacts**](https://msdn.microsoft.com/library/windows/apps/BR225002)-Namespace verfügen Sie über mehrere Optionen zum Auswählen von Kontakten. Wir zeigen Ihnen hier, wie Sie einen einzelnen Kontakt oder mehrere Kontakte auswählen und wie Sie die Kontaktauswahl so konfigurieren, dass nur die von der App benötigten Kontaktinformationen abgerufen werden.

## Einrichten der Kontaktauswahl

Erstellen Sie eine Instanz von [**Windows.ApplicationModel.Contacts.ContactPicker**](https://msdn.microsoft.com/library/windows/apps/BR224913), und weisen Sie sie einer Variablen zu.

```cs
var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
```

## Festlegen des Auswahlmodus (optional)

Standardmäßig ruft die Kontaktauswahl alle verfügbaren Daten für die von Benutzern ausgewählten Kontakte ab. Mithilfe der [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.contacts.contactpicker.selectionmode)-Eigenschaft können Sie die Kontaktauswahl so konfigurieren, dass nur die von der App benötigten Datenfelder abgerufen werden. Diese effiziente Verwendungsweise der Kontaktauswahl eignet sich besonders dann, wenn Sie nur einen Teil der verfügbaren Kontaktdaten benötigen.

Legen Sie zunächst die [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.contacts.contactpicker.selectionmode)-Eigenschaft auf **Fields** fest.

```cs
contactPicker.SelectionMode = Windows.ApplicationModel.Contacts.ContactSelectionMode.Fields;
```

Geben Sie dann mithilfe der [**DesiredFieldsWithContactFieldType**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.contacts.contactpicker.desiredfieldswithcontactfieldtype)-Eigenschaft die Felder an, die von der Kontaktauswahl abgerufen werden sollen. In diesem Beispiel wird die Kontaktauswahl so konfiguriert, dass E-Mail-Adressen abgerufen werden:

``` cs
contactPicker.DesiredFieldsWithContactFieldType.Add(Windows.ApplicationModel.Contacts.ContactFieldType.Email);
```

## Starten der Auswahl

```cs
Contact contact = await contactPicker.PickContactAsync();
```

Verwenden Sie [**PickContactsAsync**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.contacts.contactpicker.pickcontactsasync), wenn der Benutzer mindestens einen Kontakt auswählen soll.

```cs
public IList<Contact> contacts;
contacts = await contactPicker.PickContactsAsync();
```

## Verarbeiten der Kontakte

Überprüfen Sie in der Auswahl, ob der Benutzer Kontakte ausgewählt hat. Wenn ja, verarbeiten Sie die Kontaktinformationen.

Dieses Beispiel zeigt, wie ein einzelner Kontakt verarbeitet wird. Hier wird der Name des Kontakts abgerufen und in ein [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)-Steuerelement namens *OutputName* kopiert.

```cs
if (contact != null)
{
    OutputName.Text = contact.DisplayName;
}
else
{
    rootPage.NotifyUser("No contact was selected.", NotifyType.ErrorMessage);
}
```

Dieses Beispiel zeigt die Verarbeitung mehrerer Kontakte.

```cs
if (contacts != null && contacts.Count > 0)
{
    foreach (Contact contact in contacts)
    {
        // Do something with the contact information.
    }
}
```

## Vollständiges Beispiel (einzelner Kontakt)

In diesem Beispiel werden mithilfe der Kontaktauswahl der Name sowie E-Mail-Adresse, Standort oder Telefonnummer eines einzelnen Kontakts abgerufen.

```cs
// ...
using Windows.ApplicationModel.Contacts;
// ...

private async void PickAContactButton-Click(object sender, RoutedEventArgs e)
{
    ContactPicker contactPicker = new ContactPicker();

    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.Email);
    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.Address);
    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.PhoneNumber);

    Contact contact = await contactPicker.PickContactAsync();

    if (contact != null)
    {
        OutputFields.Visibility = Visibility.Visible;
        OutputEmpty.Visibility = Visibility.Collapsed;

        OutputName.Text = contact.DisplayName;

        AppendContactFieldValues(OutputEmails, contact.Emails);
        AppendContactFieldValues(OutputPhoneNumbers, contact.Phones);
        AppendContactFieldValues(OutputAddresses, contact.Addresses);
    }
    else
    {
        OutputEmpty.Visibility = Visibility.Visible;
        OutputFields.Visibility = Visibility.Collapsed;
    }
}

private void AppendContactFieldValues<T>(TextBlock content, IList<T> fields)
{
    if (fields.Count > 0)
    {
        StringBuilder output = new StringBuilder();

        if (fields[0].GetType() == typeof(ContactEmail))
        {
            foreach (ContactEmail email in fields as IList<ContactEmail>)
            {
                output.AppendFormat("Email: {0} ({1})\n", email.Address, email.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactPhone))
        {
            foreach (ContactPhone phone in fields as IList<ContactPhone>)
            {
                output.AppendFormat("Phone: {0} ({1})\n", phone.Number, phone.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactAddress))
        {
            List<String> addressParts = null;
            string unstructuredAddress = "";

            foreach (ContactAddress address in fields as IList<ContactAddress>)
            {
                addressParts = (new List<string> { address.StreetAddress, address.Locality, address.Region, address.PostalCode });
                unstructuredAddress = string.Join(", ", addressParts.FindAll(s => !string.IsNullOrEmpty(s)));
                output.AppendFormat("Address: {0} ({1})\n", unstructuredAddress, address.Kind);
            }
        }

        content.Visibility = Visibility.Visible;
        content.Text = output.ToString();
    }
    else
    {
        content.Visibility = Visibility.Collapsed;
    }
}
```

## Vollständiges Beispiel (mehrere Kontakte)

In diesem Beispiel werden mithilfe der Kontaktauswahl mehrere Kontakte abgerufen und einem [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)-Steuerelement namens `OutputContacts` hinzugefügt.

```cs
MainPage rootPage = MainPage.Current;
public IList<Contact> contacts;

private async void PickContactsButton-Click(object sender, RoutedEventArgs e)
{
    var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
    contactPicker.CommitButtonText = "Select";
    contacts = await contactPicker.PickContactsAsync();

    // Clear the ListView.
    OutputContacts.Items.Clear();

    if (contacts != null && contacts.Count > 0)
    {
        OutputContacts.Visibility = Windows.UI.Xaml.Visibility.Visible;
        OutputEmpty.Visibility = Visibility.Collapsed;

        foreach (Contact contact in contacts)
        {
            // Add the contacts to the ListView.
            OutputContacts.Items.Add(new ContactItemAdapter(contact));
        }
    }
    else
    {
        OutputEmpty.Visibility = Visibility.Visible;
    }         
}
```

``` cs
public class ContactItemAdapter
{
    public string Name { get; private set; }
    public string SecondaryText { get; private set; }

    public ContactItemAdapter(Contact contact)
    {
        Name = contact.DisplayName;
        if (contact.Emails.Count > 0)
        {
            SecondaryText = contact.Emails[0].Address;
        }
        else if (contact.Phones.Count > 0)
        {
            SecondaryText = contact.Phones[0].Number;
        }
        else if (contact.Addresses.Count > 0)
        {
            List<string> addressParts = (new List<string> { contact.Addresses[0].StreetAddress,
              contact.Addresses[0].Locality, contact.Addresses[0].Region, contact.Addresses[0].PostalCode });
              string unstructuredAddress = string.Join(", ", addressParts.FindAll(s => !string.IsNullOrEmpty(s)));
            SecondaryText = unstructuredAddress;
        }
    }
}
```

## Zusammenfassung und nächste Schritte

Sie verfügen nun über grundlegende Kenntnisse zum Abrufen von Kontaktinformationen mithilfe der Kontaktauswahl. Laden Sie die [Beispiele für universelle Windows-Apps](http://go.microsoft.com/fwlink/p/?linkid=619979) von GitHub herunter, um sich weitere Beispiele für die Verwendung von Kontakten und Kontaktauswahl anzusehen.



<!--HONumber=Jul16_HO1-->


