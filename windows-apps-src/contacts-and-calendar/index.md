---
author: Xansky
description: Erfahren Sie, wie Kontakte und Kalenderinformationen in Ihrer UWP-App verwendet werden.
title: Kontakte und Kalender
ms.assetid: b7e53ab5-2828-4fb7-8656-2bec70b3467f
translationtype: Human Translation
ms.sourcegitcommit: 5c0f6ef1f1a346a66ca554a415d9f24c8a314ae1
ms.openlocfilehash: da73790ca9aec3fa16295eac4880c7b80db033ab

---

# Kontakte und Kalender

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Sie können Benutzern den Zugriff auf ihre Kontakte und Termine ermöglichen, sodass sie Inhalte, E-Mails, Kalenderinformationen oder Nachrichten mit anderen Benutzern oder beliebiger, von Ihnen entwickelter Funktionalität teilen können.

Die folgenden Themen enthalten Informationen zu verschiedenen Verfahren, wie Ihre App auf Kontakte und Termine zugreifen kann:

| Thema | Beschreibung |
|-------|-------------|
| [Auswählen von Kontakten](selecting-contacts.md) | Mit dem [<strong>Windows.ApplicationModel.Contacts</strong>](https://msdn.microsoft.com/library/windows/apps/BR225002)-Namespace verfügen Sie über mehrere Optionen zum Auswählen von Kontakten. Wir zeigen Ihnen hier, wie Sie einen einzelnen Kontakt oder mehrere Kontakte auswählen und wie Sie die Kontaktauswahl so konfigurieren, dass nur die von der App benötigten Kontaktinformationen abgerufen werden. |
| [Senden von E-Mails](sending-email.md) | Hier erfahren Sie, wie Sie das Dialogfeld zum Verfassen einer E-Mail starten, damit Benutzer eine E-Mail senden können. Sie können die Felder der E-Mail vor dem Anzeigen des Dialogfelds mit Daten füllen. Die Nachricht wird erst gesendet, wenn Benutzer auf die Schaltfläche „Senden“ tippen. |
| [Senden einer SMS](sending-an-sms-message.md) | In diesem Thema erfahren Sie, wie Sie das Dialogfeld zum Verfassen einer SMS starten, damit Benutzer eine SMS senden können. Sie können die Felder der SMS vor dem Anzeigen des Dialogfelds mit Daten füllen. Die Nachricht wird erst gesendet, wenn Benutzer auf die Schaltfläche „Senden“ tippen. |
| [Verwalten von Terminen](managing-appointments.md) | Mit dem [<strong>Windows.ApplicationModel.Appointments</strong>](https://msdn.microsoft.com/library/windows/apps/Dn263359)-Namespace können Sie in der Kalender-App eines Benutzers Termine erstellen und verwalten. Hier erfahren Sie, wie Sie einen Termin erstellen, einer Kalender-App hinzufügen, in der Kalender-App ersetzen und aus der Kalender-App entfernen. Außerdem wird erläutert, wie Sie eine Zeitspanne für eine Kalender-App anzeigen und ein Terminwiederholungsobjekt erstellen. |
| [Verbinden der App mit Aktionen auf einer Visitenkarte](integrating-with-contacts.md) | Zeigt, wie Ihre App neben Aktionen auf einer Visitenkarte oder einer kleinen Visitenkarte angezeigt werden kann. Benutzer können Ihre App auswählen, um eine Aktion auszuführen, z.B. eine Profilseite zu öffnen, einen Anruf zu tätigen oder eine Nachricht zu senden. |
| [Bereitstellen von Social Media-Feeds in der Kontakte-App](integrating-social-feeds-into-contact-cards.md) | Integrieren Sie Daten aus Social Media-Feeds aus Ihrer Datenbank in die Kontakte-App. Die Feeddaten werden auf den <strong>Neuigkeiten</strong>-Seiten der Kontakte-App und auf der <strong>Profil</strong>-Seite von Kontakten angezeigt. |

 

## Verwandte Themen

* [Beispiel zur Termin-API](http://go.microsoft.com/fwlink/p/?linkid=309836)
* [Beispiel zur Kontakt-Manager-API](http://go.microsoft.com/fwlink/p/?LinkID=310079)
* [Beispiel-App für die Kontaktauswahl](http://go.microsoft.com/fwlink/p/?linkid=231575)
* [Beispiel zum Behandeln von Kontaktaktionen](http://go.microsoft.com/fwlink/p/?LinkID=320151)
* [Beispiel für die Visitenkartenintegration](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration)
* [Beispiel für Informationen aus Social Media](https://github.com/Microsoft/Windows-Social-Samples/tree/master/SocialInfoSampleApp)



<!--HONumber=Aug16_HO5-->


