---
description: In diesem Thema erfahren Sie, wie Sie das Dialogfeld zum Verfassen einer SMS starten, damit Benutzer eine SMS senden können. Sie können die Felder der SMS vor dem Anzeigen des Dialogfelds mit Daten füllen. Die Nachricht wird erst gesendet, wenn Benutzer auf die Schaltfläche "Senden" tippen.
title: Senden einer SMS
ms.assetid: 4D7B509B-1CF0-4852-9691-E96D8352A4D6
keywords: Kontakte, SMS, senden
---

# Senden einer SMS

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


In diesem Thema erfahren Sie, wie Sie das Dialogfeld zum Verfassen einer SMS starten, damit Benutzer eine SMS senden können. Sie können die Felder der SMS vor dem Anzeigen des Dialogfelds mit Daten füllen. Die Nachricht wird erst gesendet, wenn Benutzer auf die Schaltfläche "Senden" tippen.

## Starten des Dialogfelds zum Verfassen einer SMS

Erstellen Sie ein neues [**ChatMessage**](https://msdn.microsoft.com/library/windows/apps/Dn642160)-Objekt, und legen Sie die Daten fest, die im Dialogfeld zum Verfassen einer E-Mail bereits vorhanden sein sollen. Rufen Sie [**ShowComposeSmsMessageAsync**](https://msdn.microsoft.com/library/windows/apps/Dn642160manager-showcomposesmsmessageasync) auf, um das Dialogfeld anzuzeigen.

```cs
private async void ComposeSms(Windows.ApplicationModel.Contacts.Contact recipient, 
    string messageBody, 
    StorageFile attachmentFile, 
    string mimeType)
{
    var chatMessage = new Windows.ApplicationModel.Chat.ChatMessage();
    chatMessage.Body = messageBody;

    if (attachmentFile != null)
    {
        var stream = Windows.Storage.Streams.RandomAccessStreamReference.CreateFromFile(attachmentFile);

        var attachment = new Windows.ApplicationModel.Chat.ChatMessageAttachment(
            mimeType,
            stream);

        chatMessage.Attachments.Add(attachment);
    }

    var phone = recipient.Phones.FirstOrDefault&lt;Windows.ApplicationModel.Contacts.ContactPhone&gt;();
    if (phone != null)
    {
        chatMessage.Recipients.Add(phone.Number);
    }
    await Windows.ApplicationModel.Chat.ChatMessageManager.ShowComposeSmsMessageAsync(chatMessage);
}
```

## Zusammenfassung und nächste Schritte

In diesem Thema haben Sie erfahren, wie Sie das Dialogfeld zum Verfassen einer SMS starten. Informationen zum Auswählen von Kontakten als SMS-Empfänger finden Sie unter [Auswählen von Kontakten](selecting-contacts.md). Laden Sie die [Beispiele für universelle Windows-Apps](http://go.microsoft.com/fwlink/p/?linkid=619979) von GitHub herunter, um sich weitere Beispiele zum Senden und Empfangen von SMS-Nachrichten unter Verwendung einer Hintergrundaufgabe anzusehen.

## Verwandte Themen

* [Auswählen von Kontakten](selecting-contacts.md)




<!--HONumber=Mar16_HO1-->

