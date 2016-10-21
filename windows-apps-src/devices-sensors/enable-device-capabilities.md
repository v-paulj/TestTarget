---
author: DBirtolo
ms.assetid: 949D1CE0-DD7D-420E-904D-758FADEBE85A
title: "Aktivieren von Gerätefunktionen"
description: "In diesem Lernprogramm wird beschrieben, wie Gerätefunktionen in Microsoft Visual Studio deklariert werden. Diese Funktionen bieten Ihnen die Möglichkeit, in Ihrer App Kameras, Mikrofone, Positionssensoren und andere Geräte zu verwenden."
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: b36dd4d77821a65b1f435d755f7bb415b2e386ee

---
# Aktivieren von Gerätefunktionen

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


In diesem Lernprogramm wird beschrieben, wie Gerätefunktionen in Microsoft Visual Studio deklariert werden. Diese Funktionen bieten Ihnen die Möglichkeit, in Ihrer App Kameras, Mikrofone, Positionssensoren und andere Geräte zu verwenden.

## Angeben der von der App verwendeten Gerätefunktionen


Windows-Apps erfordern eine Angabe im App-Paketmanifest, wenn Sie bestimmte Gerätetypen verwenden. In Visual Studio können Sie die meisten Funktionen mit dem [Manifest-Designer](https://msdn.microsoft.com/library/windows/apps/xaml/br230259.aspx) deklarieren oder die Funktionen wie unter [So wird's gemacht: Angeben von Gerätefunktionen in einem Paketmanifest (manuell)](https://msdn.microsoft.com/library/windows/apps/Dn263092) beschrieben manuell hinzufügen. In diesem Lernprogramm wird vorausgesetzt, dass Sie den Manifest-Designer verwenden.

**Hinweis**  
Einige Gerätetypen (z.B. Drucker, Scanner und Sensoren) müssen nicht im App-Paketmanifest deklariert werden.

-   Doppelklicken Sie im Projektmappen-Explorer von Visual Studio auf die Paketmanifestdatei **Package.appxmanifest**.
-   Öffnen Sie die Registerkarte **Funktionen**.
-   Wählen Sie die Gerätefunktionen aus, die Ihre App verwendet. Wenn die gewünschte Funktion nicht im Manifest-Designer angezeigt wird, fügen Sie sie manuell hinzu. Weitere Informationen finden Sie unter [So wird's gemacht: Angeben von Gerätefunktionen in einem Paketmanifest](https://msdn.microsoft.com/library/windows/apps/Dn263092).

| Gerätefunktion | Manifest-Designer | Beschreibung |
|-------------------|-------------------|-------------|    
| AllJoyn | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Ermöglicht es AllJoyn-fähigen Apps und Geräten in einem Netzwerk, sich gegenseitig zu erkennen und miteinander zu interagieren. Alle Apps, die auf APIs im [**Windows.Devices.AllJoyn**](https://msdn.microsoft.com/library/windows/apps/Dn894971)-Namespace zugreifen, müssen diese Funktion verwenden. |
| Blockierte Chatnachrichten | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Ermöglicht Apps das Lesen von SMS- und MMS-Nachrichten, die von der Spamfilter-App blockiert wurden. |
| Chatnachrichtzugriff | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Ermöglicht Apps das Lesen und Löschen von Textnachrichten. Ermöglicht Apps darüber hinaus das Speichern von Chatnachrichten im Systemdatenspeicher. |
| Codegenerierung | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Ermöglicht Apps das dynamische Generieren von Code. |
| Unternehmensauthentifizierung | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Diese Funktion betrifft die Windows Store-Richtlinie. Sie bietet die Möglichkeit zum Herstellen einer Verbindung mit Intranetressourcen im Unternehmen, die Domänenanmeldeinformationen erfordern. Diese Funktion ist in der Regel für die meisten Apps nicht erforderlich. | 
| Internet (Client) | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Bietet ausgehenden Zugriff auf das Internet und auf Netzwerke an öffentlichen Orten wie Flughäfen und Cafés. Beispielsweise Intranetnetzwerke, für die der Benutzer das Netzwerk als „öffentlich“ festgelegt hat. Die Funktion sollte von den meisten Apps verwendet werden, die den Internetzugriff benötigen. |
| Internet (Client &amp; Server) | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Bietet ein- und ausgehenden Zugriff auf das Internet und auf Netzwerke an öffentlichen Orten wie Flughäfen und Cafés. Diese Funktion ist eine Obermenge von **Internet (Client)**. **Internet (Client)** muss nicht aktiviert sein, wenn diese Funktion ebenfalls aktiviert ist. Der eingehende Zugriff auf kritische Ports ist immer blockiert. |
| Ort| ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Bietet Zugriff auf die aktuelle Position. Die Position wird von spezieller Hardware (z.B. einem GPS-Sensor im PC) abgerufen oder aus verfügbaren Netzwerkinformationen abgeleitet. | 
| Mikrofon | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Bietet Zugriff auf den Audiofeed des Mikrofons. Mit dieser Funktion kann die App Audio von angeschlossenen Mikrofonen aufzeichnen. | 
| Musikbibliothek | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Ermöglicht das Hinzufügen, Ändern oder Löschen von Dateien in der **Musikbibliothek** für den lokalen PC und die PCs der **Heimnetzgruppe**. | 
| 3D-Objekte | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Bietet programmgesteuerten Zugriff auf die **3D-Objekte** des Benutzers, wodurch die App alle Dateien in der Bibliothek auflisten und ohne Eingreifen des Benutzers darauf zugreifen kann. Diese Funktion wird in der Regel in 3D-Apps und -Spielen verwendet, die auf die gesamte **3D-Objektbibliothek** zugreifen müssen. | 
| Telefonanruf | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Ermöglicht Apps den Zugriff auf alle Telefonleitungen auf dem Gerät sowie das Ausführen der folgenden Funktionen: Tätigen eines Anrufs über die Telefonleitung und Anzeigen der systemeigenen Wählhilfe ohne Benutzeraufforderung; Zugreifen auf Leitungsmetadaten; Zugreifen auf Leitungstrigger. Festlegen und Überprüfen der Liste „Blockieren“ und der Informationen zum Anrufursprung durch die vom Benutzer ausgewählte Spamfilter-App. | 
| Bildbibliothek | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Ermöglicht das Hinzufügen, Ändern oder Löschen von Dateien in der **Bildbibliothek** für den lokalen PC und die PCs der **Heimnetzgruppe**. | 
| Private Netzwerke (Client &amp; Server) | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Bietet ein- und ausgehenden Zugriff auf Intranetnetzwerke, die über einen authentifizierten Domänencontroller verfügen oder vom Benutzer als Heim- oder Firmennetzwerke festgelegt wurden. Der eingehende Zugriff auf kritische Ports ist immer blockiert. | 
| Näherung | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Bietet die Möglichkeit, über Nahfeldkommunikation (Near-Field Communication, NFC) eine Verbindung mit Geräten in unmittelbarer Nähe zum PC herzustellen. Die Nahfeldnäherung kann verwendet werden, um Dateien zu senden oder mit einer App auf dem anderen Gerät in der Nähe zu kommunizieren. | 
| Wechselmedien | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Bietet die Möglichkeit zum Hinzufügen, Ändern oder Löschen von Dateien auf Wechselmedien. Die App kann nur auf die Dateitypen auf Wechselmedien zugreifen, die mithilfe der Deklaration für **Dateitypzuordnungen** im Manifest definiert sind. Die App kann nicht auf Wechselmedien auf PCs der **Heimnetzgruppe** zugreifen. | 
| Freigegebene Benutzerzertifikate | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Diese Funktion betrifft die Windows Store-Richtlinie. Sie bietet die Möglichkeit, zum Überprüfen der Identität des Benutzers auf Software- und Hardwarezertifikate (wie Smartcardzertifikate) zuzugreifen. Wenn verwandte APIs zur Laufzeit aufgerufen werden, muss der Benutzer eine Maßnahme ergreifen (Karte einfügen, Zertifikat auswählen usw.). Diese Funktion ist nicht erforderlich, wenn Ihre App über die **Certificates**-Deklaration ein privates Zertifikat enthält. | 
| Benutzerkontoinformationen | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Bietet Apps die Möglichkeit, auf den Namen und das Bild des Benutzers zuzugreifen. Diese Funktion ist für den Zugriff auf einige APIs im [**Windows.System.UserProfile**](https://msdn.microsoft.com/library/windows/apps/BR241881)-Namespace erforderlich. | 
| Videobibliothek | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Ermöglicht das Hinzufügen, Ändern oder Löschen von Dateien in der **Videobibliothek** für den lokalen PC und die PCs der **Heimnetzgruppe**. | 
| VOIP-Anruf | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Ermöglicht Apps den Zugriff auf die VOIP-Anruf-APIs im [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266)-Namespace. | 
| Webcam | ![Verfügbar im Manifest-Designer](images/ap-tools.png) | Bietet Zugriff auf den Videofeed der integrierten Kamera oder der angeschlossenen Webcam. Mit dieser Funktion kann die App Schnappschüsse und Filme aufnehmen. | 
| USB | | Bietet Zugriff auf benutzerdefinierte USB-Geräte. Diese Funktion erfordert untergeordnete Elemente. Dieses Feature wird für WindowsPhone nicht unterstützt. | 
| Eingabegerät (Human Interface Device, HID) | | Bietet Zugriff auf Eingabegeräte. Diese Funktion erfordert untergeordnete Elemente. Weitere Informationen finden Sie unter [So wird's gemacht: Angeben von Gerätefunktionen für HID](https://msdn.microsoft.com/library/windows/apps/Dn263091). | 
| BluetoothGATT | | Bietet über eine Sammlung von primären Diensten, enthaltenen Diensten, Merkmalen und Deskriptoren Zugriff auf BluetoothLE-Geräte. Diese Funktion erfordert untergeordnete Elemente. Weitere Informationen finden Sie unter [So wird's gemacht: Angeben von Gerätefunktionen für Bluetooth](https://msdn.microsoft.com/library/windows/apps/Dn263090). | 
| BluetoothRFCOMM |  | Bietet Zugriff auf APIs, die den Transport mit Standardrate/erweiterter Datenrate (Basic Rate/Extended Data Rate, BR/EDR) unterstützen, und bietet Ihrer WindowsStore-App außerdem Zugriff auf ein Gerät, das Serial Port Profile (SPP) implementiert. Diese Funktion erfordert untergeordnete Elemente. Weitere Informationen finden Sie unter [So wird's gemacht: Angeben von Gerätefunktionen für Bluetooth](https://msdn.microsoft.com/library/windows/apps/Dn263090). |
| pointOfService |  | Bietet Zugriff auf PointofService (POS)-Barcodescanner und -Magnetstreifenleser. Dieses Feature wird unter WindowsPhone nicht unterstützt. | 

## Verwenden der Windows-Runtime-API für die Kommunikation mit dem Gerät

In der folgende Tabelle werden einige der Funktionen mit Windows-Runtime-APIs verbunden.

| Gerätefunktion        | API             | 
|--------------------------|-----------------|
| AllJoyn                  | [**Windows.Devices.AllJoyn**](https://msdn.microsoft.com/library/windows/apps/Dn894971) | 
| Blockierte Chatnachrichten    | [**Windows.ApplicationModel.CommunicationBlocking**](https://msdn.microsoft.com/library/windows/apps/Dn974207) | 
| Ort                 | Weitere Informationen finden Sie unter [Übersicht über Karten und Position](https://msdn.microsoft.com/library/windows/apps/Mt219699). | 
| Telefonanruf               | [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) | 
| Benutzerkontoinformationen | [**Windows.System.UserProfile**](https://msdn.microsoft.com/library/windows/apps/BR241881) | 
| VOIP-Anruf             | [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) | 
| USB                      | [**Windows.Devices.Usb**](https://msdn.microsoft.com/library/windows/apps/Dn278466) | 
| HID                      | [**Windows.Devices.HumanInterfaceDevice**](https://msdn.microsoft.com/library/windows/apps/Dn264174) | 
| BluetoothGATT           | [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685) | 
| BluetoothRFCOMM         | [**Windows.Devices.Bluetooth.Rfcomm**](https://msdn.microsoft.com/library/windows/apps/Dn263529) | 
| PointofService (POS)   | [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071) |




<!--HONumber=Aug16_HO3-->


