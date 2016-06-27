---
author: Xansky
description: "Hier erfahren Sie, wie Sie das Dialogfeld zum Verfassen einer E-Mail starten, damit Benutzer eine E-Mail senden können. Sie können die Felder der E-Mail vor dem Anzeigen des Dialogfelds mit Daten füllen. Die Nachricht wird erst gesendet, wenn Benutzer auf die Schaltfläche „Senden“ tippen."
title: Senden von E-Mails
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: contacts, email, send
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: b4b5b029c321256028993e283395a91bd0ed3d7c

---

# Senden von E-Mails

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Hier erfahren Sie, wie Sie das Dialogfeld zum Verfassen einer E-Mail starten, damit Benutzer eine E-Mail senden können. Sie können die Felder der E-Mail vor dem Anzeigen des Dialogfelds mit Daten füllen. Die Nachricht wird erst gesendet, wenn Benutzer auf die Schaltfläche "Senden" tippen.

**Inhalt dieses Artikels**

-   [Starten des Dialogfelds zum Verfassen einer E-Mail](#launch-the-compose-email-dialog)
-   [Zusammenfassung und nächste Schritte](#summary-and-next-steps)
-   [Verwandte Themen](#related-topics)

## Starten des Dialogfelds zum Verfassen einer E-Mail

Erstellen Sie ein neues [**EmailMessage**](https://msdn.microsoft.com/library/windows/apps/Dn631270)-Objekt, und legen Sie die Daten fest, die im Dialogfeld zum Verfassen einer E-Mail bereits vorhanden sein sollen. Rufen Sie [**ShowComposeNewEmailAsync**](https://msdn.microsoft.com/library/windows/apps/Dn631269) auf, um das Dialogfeld anzuzeigen.

``` cs
private async void ComposeEmail(Windows.ApplicationModel.Contacts.Contact recipient, 
    string messageBody, 
    StorageFile attachmentFile)
{
    var emailMessage = new Windows.ApplicationModel.Email.EmailMessage();
    emailMessage.Body = messageBody;

    if (attachmentFile != null)
    {
        var stream = Windows.Storage.Streams.RandomAccessStreamReference.CreateFromFile(attachmentFile);

        var attachment = new Windows.ApplicationModel.Email.EmailAttachment(
            attachmentFile.Name,
            stream);

        emailMessage.Attachments.Add(attachment);
    }

    var email = recipient.Emails.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactEmail>();
    if (email != null)
    {
        var emailRecipient = new Windows.ApplicationModel.Email.EmailRecipient(email.Address);
        emailMessage.To.Add(emailRecipient);
    }

    await Windows.ApplicationModel.Email.EmailManager.ShowComposeNewEmailAsync(emailMessage);
        
}
```

## Zusammenfassung und nächste Schritte

In diesem Thema haben Sie erfahren, wie Sie das Dialogfeld zum Verfassen einer E-Mail starten. Informationen zum Auswählen von Kontakten als E-Mail-Empfänger finden Sie unter [Auswählen von Kontakten](selecting-contacts.md). Informationen zum Auswählen einer Datei als E-Mail-Anlage finden Sie unter [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/JJ635275).

## Verwandte Themen

* [Auswählen von Kontakten](selecting-contacts.md)
* [Fortsetzen von Windows Phone-Apps nach dem Aufrufen einer Dateiauswahl](https://msdn.microsoft.com/library/windows/apps/xaml/Dn614994)
 

 







<!--HONumber=Jun16_HO3-->


