---
description: Mit dem Windows.ApplicationModel.Contacts-Namespace verfügen Sie über mehrere Optionen zum Auswählen von Kontakten.
title: Auswählen von Kontakten
ms.assetid: 35FEDEE6-2B0E-4391-84BA-5E9191D4E442
keywords: Kontakte, Auswählen
keywords: Auswählen eines Kontakts
keywords: Auswählen mehrerer Kontakte
keywords: Kontakte, Auswählen mehrerer
keywords: Auswählen bestimmter Kontaktdaten
keywords: Kontakt, Auswählen bestimmter Daten
keywords: Kontakt, Auswählen bestimmter Felder
---

# Auswählen von Kontakten

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Mit dem [**Windows.ApplicationModel.Contacts**](https://msdn.microsoft.com/library/windows/apps/BR225002)-Namespace verfügen Sie über mehrere Optionen zum Auswählen von Kontakten. Wir zeigen Ihnen hier, wie Sie einen einzelnen Kontakt oder mehrere Kontakte auswählen und wie Sie die Kontaktauswahl so konfigurieren, dass nur die von der App benötigten Kontaktinformationen abgerufen werden.

## Einrichten der Kontaktauswahl

Erstellen Sie eine Instanz von [**Windows.ApplicationModel.Contacts.ContactPicker**](https://msdn.microsoft.com/library/windows/apps/BR224913), und weisen Sie sie einer Variablen zu.

```cs
var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
```

## Festlegen des Auswahlmodus (optional)

Standardmäßig ruft die Kontaktauswahl alle verfügbaren Daten für die von Benutzern ausgewählten Kontakte ab. Mithilfe der [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/BR224913-selectionmode)-Eigenschaft können Sie die Kontaktauswahl so konfigurieren, dass nur die von der App benötigten Datenfelder abgerufen werden. Diese effiziente Verwendungsweise der Kontaktauswahl eignet sich besonders dann, wenn Sie nur einen Teil der verfügbaren Kontaktdaten benötigen.

Legen Sie zunächst die [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/BR224913-selectionmode)-Eigenschaft auf **Fields** fest.

```cs
contactPicker.SelectionMode = Windows.ApplicationModel.Contacts.ContactSelectionMode.Fields;
```

Geben Sie dann mithilfe der [**desiredFieldsWithContactFieldType**](https://msdn.microsoft.com/library/windows/apps/BR224913-desiredfieldswithcontactfieldtype)-Eigenschaft die Felder an, die von der Kontaktauswahl abgerufen werden sollen. In diesem Beispiel wird die Kontaktauswahl so konfiguriert, dass E-Mail-Adressen abgerufen werden:

``` cs
contactPicker.DesiredFieldsWithContactFieldType.Add(Windows.ApplicationModel.Contacts.ContactFieldType.Email);
```

## Starten der Auswahl

```cs
Contact contact = await contactPicker.PickContactAsync();
```

Verwenden Sie [**pickContactsAsync**](https://msdn.microsoft.com/library/windows/apps/BR224913-pickcontactsasync), wenn es dem Benutzer möglich sein soll, einen oder mehrere Kontakte auszuwählen.

```cs
public IList&lt;Contact&gt; contacts;
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
    rootPage.NotifyUser(&quot;No contact was selected.&quot;, NotifyType.ErrorMessage);
}
```

Dieses Beispiel zeigt die Verarbeitung mehrerer Kontakte.

```cs
if (contacts != null &amp;&amp; contacts.Count &gt; 0)
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

private void AppendContactFieldValues&lt;T&gt;(TextBlock content, IList&lt;T&gt; fields)
{
    if (fields.Count &gt; 0)
    {
        StringBuilder output = new StringBuilder();

        if (fields[0].GetType() == typeof(ContactEmail))
        {
            foreach (ContactEmail email in fields as IList&lt;ContactEmail&gt;)
            {
                output.AppendFormat(&quot;Email: {0} ({1})\n&quot;, email.Address, email.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactPhone))
        {
            foreach (ContactPhone phone in fields as IList&lt;ContactPhone&gt;)
            {
                output.AppendFormat(&quot;Phone: {0} ({1})\n&quot;, phone.Number, phone.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactAddress))
        {
            List&lt;String&gt; addressParts = null;
            string unstructuredAddress = &quot;&quot;;

            foreach (ContactAddress address in fields as IList&lt;ContactAddress&gt;)
            {
                addressParts = (new List&lt;string&gt; { address.StreetAddress, address.Locality, address.Region, address.PostalCode });
                unstructuredAddress = string.Join(&quot;, &quot;, addressParts.FindAll(s =&gt; !string.IsNullOrEmpty(s)));
                output.AppendFormat(&quot;Address: {0} ({1})\n&quot;, unstructuredAddress, address.Kind);
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
public IList&lt;Contact&gt; contacts;

private async void PickContactsButton-Click(object sender, RoutedEventArgs e)
{
    var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
    contactPicker.CommitButtonText = &quot;Select&quot;;
    contacts = await contactPicker.PickContactsAsync();

    // Clear the ListView.
    OutputContacts.Items.Clear();

    if (contacts.Count &gt; 0)
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
        if (contact.Emails.Count &gt; 0)
        {
            SecondaryText = contact.Emails[0].Address;
        }
        else if (contact.Phones.Count &gt; 0)
        {
            SecondaryText = contact.Phones[0].Number;
        }
        else if (contact.Addresses.Count &gt; 0)
        {
            List&lt;string&gt; addressParts = (new List&lt;string&gt; { contact.Addresses[0].StreetAddress, 
              contact.Addresses[0].Locality, contact.Addresses[0].Region, contact.Addresses[0].PostalCode });
              string unstructuredAddress = string.Join(&quot;, &quot;, addressParts.FindAll(s =&gt; !string.IsNullOrEmpty(s)));
            SecondaryText = unstructuredAddress;
        }
    }
}
```

## Zusammenfassung und nächste Schritte

Sie verfügen nun über grundlegende Kenntnisse zum Abrufen von Kontaktinformationen mithilfe der Kontaktauswahl. Laden Sie die [Beispiele für universelle Windows-Apps](http://go.microsoft.com/fwlink/p/?linkid=619979) von GitHub herunter, um sich weitere Beispiele für die Verwendung von Kontakten und Kontaktauswahl anzusehen.



<!--HONumber=Mar16_HO1-->


