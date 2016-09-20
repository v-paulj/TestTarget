---
author: TylerMSFT
title: Starten der Einstellungs-App von Windows
description: "Erfahren Sie, wie Sie die Windows-Einstellungs-App aus Ihrer App starten können. In diesem Thema wird das ms-settings-URI-Schema beschrieben. Verwenden Sie dieses URI-Schema, um die Windows-Einstellungs-App mit bestimmten Einstellungsseiten zu starten."
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.sourcegitcommit: 3cf9dd4ab83139a2b4b0f44a36c2e57a92900903
ms.openlocfilehash: e52a4245e8697a68bfc5c5605dc54e5ea510c662

---

# Starten der Einstellungs-App von Windows


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

Erfahren Sie, wie Sie die Windows-Einstellungs-App aus Ihrer App starten können. In diesem Thema wird das **ms-settings:**-URI-Schema beschrieben. Verwenden Sie dieses URI-Schema, um die Windows-Einstellungs-App mit bestimmten Einstellungsseiten zu starten.

Das Starten der Einstellungs-App ist ein wichtiger Bestandteil beim Schreiben einer datenschutzbewussten App. Wenn Ihre App nicht auf eine sensible Ressource zugreifen kann, wird empfohlen, dem Benutzer einen praktischen Link zu den Datenschutzeinstellungen für diese Ressource bereitzustellen. Weitere Informationen finden Sie unter [Richtlinien für Apps mit Berücksichtigung von Datenschutz](https://msdn.microsoft.com/library/windows/apps/hh768223).

## So wird's gemacht: Starten der Einstellungs-App


Wenn Ihre App aufgrund von Datenschutzeinstellungen nicht auf eine sensible Ressource zugreifen kann, empfehlen wir, in der **Einstellungs**-App einen praktischen Link zu den Datenschutzeinstellungen anzugeben. Dadurch können Benutzer ihre Einstellungen leichter ändern.

Um direkt die **Einstellungs**-App zu starten, verwenden Sie das in den folgenden Beispielen beschriebene `ms-settings:`-URI-Schema.

In diesem Beispiel wird ein Hyperlink-XAML-Steuerelement verwendet, um die Datenschutzeinstellungsseite für das Mikrofon mit der `ms-settings:privacy-microphone`-URI zu starten.

```xml
<!--Set Visibility to Visible when access to the microphone is denied -->  
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic"
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access the microphone. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-microphone">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the microphone privacy settings."/>
</TextBlock>
```

Alternativ kann Ihre App die [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)-Methode aufrufen, um die **Einstellungs**-App per Code zu starten.

```cs
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

In diesem Beispiel wird gezeigt, wie die Datenschutzeinstellungsseite für die Kamera mit dem `ms-settings:privacy-webcam`-URI gestartet werden kann.

![Datenschutzeinstellungen für die Kamera.](images/privacyawarenesssettingsapp.png)

Weitere Informationen zum Starten von URIs finden Sie unter [Starten der Standard-App für einen URI](launch-default-app.md).

## ms-settings: URI-Schemareferenz


Verwenden Sie die folgenden URIs, um verschiedenen Seiten der Einstellungs-App zu öffnen. Beachten Sie, dass in der Spalte „Unterstützte SKUs“ angegeben ist, ob die Einstellungsseite in Windows 10 für Desktopeditionen (Home, Pro, Enterprise und Education), Windows 10 Mobile oder beiden enthalten ist.

| Kategorie           | Einstellungsseite                          | Unterstützte SKUs | URI                                       |
|--------------------|----------------------------------------|----------------|-------------------------------------------|
| Startseite          | Angebotsseite für Einstellungen              | Beide           | ms-settings:                              |
| System             | Anzeige                                | Beide           | ms-settings:screenrotation                |
|                    | Benachrichtigungen & Infos                | Beide           | ms-settings:notifications                 |
|                    | Telefon                                  | nur Mobile    | ms-settings:phone                         |
|                    | Nachrichten                              | nur Mobile    | ms-settings:messaging                     |
|                    | Stromsparmodus                          | Mobil- und Desktopeditionen auf Geräten mit einem Akku, z.B. Tablet    | ms-settings:batterysaver                  |
|                    | Stromsparmodus/Einstellungen für Stromsparmodus | Mobil- und Desktopeditionen auf Geräten mit einem Akku, z.B. Tablet | ms-settings:batterysaver-settings         |
|                    | Stromsparmodus/Akkunutzung            | Mobil- und Desktopeditionen auf Geräten mit einem Akku, z.B. Tablet    | ms-settings:batterysaver-usagedetails     |
|                    | Ein/Aus/Ruhezustand                          | nur Desktop   | ms-settings:powersleep                    |
|                    | Desktop: Info                         | Beide           | ms-settings:deviceencryption              |
|                    |                                        |                |                                           |
|                    | Mobile: Geräteverschlüsselung              |                |                                           |
|                    | Offlinekarten                           | Beide           | ms-settings:maps                          |
|                    | Info                                  | Beide           | ms-settings:about                         |
| Geräte            | Standardkamera                         | nur Mobile    | ms-settings:camera                        |
|                    | Bluetooth                              | nur Desktop   | ms-settings:bluetooth                     |
|                    | Maus und Touchpad                       | Beide           | ms-settings:mousetouchpad                 |
|                    | NFC                                    | Beide           | ms-settings:nfctransactions               |
| Netzwerk und WLAN | WLAN                                  | Beide           | ms-settings:network-wifi                  |
|                    | Flugzeugmodus                          | Beide           | ms-settings:network-airplanemode          |
| Netzwerk und Internet | Datennutzung                             | Beide           | ms-settings:datausage                     |
|                    | Mobilfunk und SIM                         | Beide           | ms-settings:network-cellular              |
|                    | Mobiler Hotspot                         | Beide           | ms-settings:network-mobilehotspot         |
|                    | Proxy                                  | Beide           | ms-settings:network-proxy                 |
| Personalisierung    | Personalisierung (Kategorie)             | Beide           | ms-settings:personalization               |
|                    | Hintergrund                             | nur Desktop   | ms-settings:personalization-background    |
|                    | Farben                                 | Beide           | ms-settings:personalization-colors        |
|                    | Sounds                                 | nur Mobile    | ms-settings:sounds                        |
|                    | Sperrbildschirm                            | Beide           | ms-settings:lockscreen                    |
| Konten           | E-Mail und Konten                | Beide           | ms-settings:emailandaccounts              |
|                    | Arbeitsplatzzugriff                            | Beide           | ms-settings:workplace                     |
|                    | Einstellungen synchronisieren                     | Beide           | ms-settings:sync                          |
| Zeit und Sprache  | Datum und Uhrzeit                            | Beide           | ms-settings:dateandtime                   |
|                    | Region & Sprache                      | nur Desktop   | ms-settings:regionlanguage                |
| Erleichterte Bedienung     | Sprachausgabe                               | Beide           | ms-settings:easeofaccess-narrator         |
|                    | Bildschirmlupe                              | Beide           | ms-settings:easeofaccess-magnifier        |
|                    | Hoher Kontrast                          | Beide           | ms-settings:easeofaccess-highcontrast     |
|                    | Untertitel                        | Beide           | ms-settings:easeofaccess-closedcaptioning |
|                    | Tastatur                               | Beide           | ms-settings:easeofaccess-keyboard         |
|                    | Maus                                  | Beide           | ms-settings:easeofaccess-mouse            |
|                    | Weitere Optionen                          | Beide           | ms-settings:easeofaccess-otheroptions     |
| Datenschutz            | Position                               | Beide           | ms-settings:privacy-location              |
|                    | Kamera                                 | Beide           | ms-settings:privacy-webcam                |
|                    | Mikrofon                             | Beide           | ms-settings:privacy-microphone            |
|                    | Bewegung                                 | Beide           | ms-settings:privacy-motion                |
|                    | Spracherkennung, Freihand und Eingabe                | Beide           | ms-settings:privacy-speechtyping          |
|                    | Kontoinformationen                           | Beide           | ms-settings:privacy-accountinfo           |
|                    | Kontakte                               | Beide           | ms-settings:privacy-contacts              |
|                    | Kalender                               | Beide           | ms-settings:privacy-calendar              |
|                    | Anrufliste                           | Beide           | ms-settings:privacy-callhistory           |
|                    | E-Mail                                  | Beide           | ms-settings:privacy-email                 |
|                    | Nachrichten                              | Beide           | ms-settings:privacy-messaging             |
|                    | Funkempfang                                 | Beide           | ms-settings:privacy-radios                |
|                    | Hintergrund-Apps                        | Beide           | ms-settings:privacy-backgroundapps        |
|                    | Weitere Geräte                          | Beide           | ms-settings:privacy-customdevices         |
|                    | Feedback und Diagnose                 | Beide           | ms-settings:privacy-feedback              |
| Update und Sicherheit  | Für Entwickler                         | Beide           | ms-settings:developers                    |
 



<!--HONumber=Jun16_HO5-->


