---
ms.assetid: 25B18BA5-E584-4537-9F19-BB2C8C52DFE1
title: Deklarationen der App-Funktionen
description: Funktionen müssen für den Zugriff auf bestimmte APIs oder Ressourcen (etwa Bilder, Musik oder Geräte wie die Kamera oder das Mikrofon) im Paketmanifest der UWP-App deklariert werden.
---
# Deklarationen der App-Funktionen

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Funktionen müssen im [Paketmanifest](https://msdn.microsoft.com/library/windows/apps/BR211474) der UWP-App für den Zugriff auf bestimmte APIs oder Ressourcen deklariert werden, z. B. Bilder, Musik oder Geräte wie die Kamera oder das Mikrofon.

Zum Anfordern des Zugriffs auf bestimmte Ressourcen oder APIs werden Funktionen im [Paketmanifest](https://msdn.microsoft.com/library/windows/apps/BR211474) Ihrer App deklariert. Sie können allgemeine Funktionen mit dem [Manifest-Designer](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/br230259.aspx) in Microsoft Visual Studio deklarieren oder manuell hinzufügen. Weitere Informationen finden Sie unter [So wird's gemacht: Angeben von Funktionen in einem Paketmanifest](https://msdn.microsoft.com/library/windows/apps/BR211477). Wichtig: Wenn Kunden Ihre App über den Store beziehen, werden sie über alle von der App deklarierten Funktionen informiert. Deklarieren Sie daher keine Funktionen, die Ihre App nicht benötigt.

Einige Funktionen bieten Apps Zugriff auf eine *sensible Ressource*. Diese Ressourcen gelten als sensibel, da sie auf persönliche Daten des Benutzers zugreifen oder für diesen Kosten verursachen. Mit Datenschutzeinstellungen, die von der Einstellungs-App verwaltet werden, können Benutzer den Zugriff auf sensible Ressourcen dynamisch steuern. Daher ist es wichtig, dass Ihre App nicht davon ausgeht, dass eine sensible Ressource immer verfügbar ist. Weitere Informationen zum Zugriff auf sensible Ressourcen finden Sie unter [Richtlinien für Apps mit Berücksichtigung von Datenschutz](https://msdn.microsoft.com/library/windows/apps/Hh768223). Funktionen, die Apps den Zugriff auf eine *sensible Ressource* ermöglichen, sind durch ein Sternchen (\*) neben dem Funktionsszenario gekennzeichnet.

In diesem Artikel werden vier Kategorien der nachfolgend beschriebenen Funktionen behandelt.

-   Funktionen zur allgemeinen Verwendung: gelten für die meisten allgemeinen App-Szenarien.

-   Gerätefunktionen: ermöglichen Ihrer App den Zugriff auf Peripheriegeräte und interne Geräte.

-   Sonderfunktionen: erfordern ein spezielles Unternehmenskonto für die Einreichung beim Store. Weitere Informationen zu Unternehmenskonten finden Sie unter [Kontotypen, Standorte und Gebühren](https://msdn.microsoft.com/library/windows/apps/JJ863494).

-   Eingeschränkte Funktionen: nur verfügbar für Microsoft und seine Partner.

## Funktionen zur allgemeinen Verwendung

Funktionen zur allgemeinen Verwendung gelten für die meisten allgemeinen App-Szenarien.

<table>
        <thead>
            <tr>
                <th>Funktionsszenario</th>
                <th>Funktionsnutzung</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>**Musik***</td>
                <td>
                    The **musicLibrary** capability provides programmatic access to the user's Music, allowing the app to enumerate and access all files in the library without user interaction. This capability is typically used in jukebox apps that make use of the entire Music library.

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **musicLibrary** capability only when the scenarios for your app require programmatic access and can't be realized by using the **file picker**.

                    The **musicLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="musicLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Bilder***</td>
                <td>
                    Die **picturesLibrary**-Funktion bietet programmgesteuerten Zugriff auf die Bilder des Benutzers, wodurch die App alle Dateien in der Bibliothek auflisten und ohne Eingreifen des Benutzers darauf zugreifen kann. Diese Funktion wird in der Regel in Foto-Apps verwendet, die die gesamte Bildbibliothek nutzen.

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **picturesLibrary** capability only when the scenarios for your app require programmatic access and can't be realized them by using the **file picker**.

                    The **picturesLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="picturesLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Videos***</td>
                <td>
                    The **videosLibrary** capability provides programmatic access to the user's Videos, allowing the app to enumerate and access all files in the library without user interaction. This capability is typically used in movie-playback apps that make use of the entire Videos library.

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **videosLibrary** capability only when the scenarios for your app require programmatic access and can't be realized by using the **file picker**.

                    The **videosLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="videosLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Wechselmedien**</td>
                <td>
                    The **removableStorage** capability provides programmatic access to files on removable storage, like USB keys and external hard drives, filtered to the file-type associations declared in the package manifest. For example, if a document-reader app declares a .doc file-type association, it can open .doc files on the removable storage device, but not other types of files. Be careful when you declare this capability, because users may include a variety of info in their removable storage devices, and will expect your app to provide a valid justification for programmatic access to the removable storage for all files of the declared type.

                    Users will expect your app to handle any file associations that you declare. So don't declare file associations that your app cannot handle responsibly. The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app.

                    Declare the **removableStorage** capability only when the scenarios for your app require programmatic access and can't be realized by using the [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847).

                    The **removableStorage** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="removableStorage"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Internet und öffentliche Netzwerke***</td>
                <td>
                    There are two capabilities that provide different levels of access to the Internet and public networks.

                    The **internetClient** capability indicates that apps can receive incoming data from the Internet. Cannot act as a server. No local network access.

                    The **internetClientServer** capability indicates that apps can receive incoming data from the Internet. Can act as a server. No local network access.

                    Most apps that have a web service component will use **internetClient**. Apps that enable peer-to-peer (P2P) scenarios where the app needs to listen for incoming network connections should use **internetClientServer**. The **internetClientServer** capability includes the access that the **internetClient** capability provides, so you don't need to specify **internetClient** when you specify **internetClientServer**.
                </td>
            </tr>
            <tr>
                <td>**Homes and work networks***</td>
                <td>
                    The **privateNetworkClientServer** capability provides inbound and outbound access to home and work networks through the firewall. This capability is typically used for games that communicate across the local area network (LAN), and for apps that share data across a variety of local devices. If your app specifies **musicLibrary**, **picturesLibrary**, or **videosLibrary**, you don't need to use this capability to access the corresponding library in a Home Group. On Windows, this capability does not provide access to the Internet.
                </td>
            </tr>
            <tr>
                <td>**Appointments**</td>
                <td>
                    The **appointments** capability provides access to the user’s appointment store. This capability allows read access to appointments obtained from the synced network accounts and to other apps that write to the appointment store. With this capability, your app can create new calendars and write appointments to calendars that it creates.

                    The **appointments** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="appointments"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Kontakte***</td>
                <td>
                    The **contacts** capability provides access to the aggregated view of the contacts from various contacts stores. This capability gives the app limited access (network permitting rules apply) to contacts that were synced from various networks and the local contact store.

                    The **contacts** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="contacts"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Codegenerierung**</td>
                <td>
                    The **codeGeneration** capability allows apps to access the following functions which provide JIT capabilities to apps.

                    - [**VirtualProtectFromApp**](https://msdn.microsoft.com/library/windows/desktop/Mt169846)
                    - [**CreateFileMappingFromApp**](https://msdn.microsoft.com/library/windows/desktop/Hh994453)
                    - [**OpenFileMappingFromApp**](https://msdn.microsoft.com/library/windows/desktop/Mt169844)
                    - [**MapViewOfFileFromApp**](https://msdn.microsoft.com/library/windows/desktop/Hh994454)
                </td>
            </tr>
            <tr>
                <td>**AllJoyn**</td>
                <td>
                    The **allJoyn** capability allows AllJoyn-enabled apps and devices on a network to discover and interact with each other.

                    All apps that access APIs in the [**Windows.Devices.AllJoyn**](https://msdn.microsoft.com/library/windows/apps/Dn894971) namespace must use this capability.
                </td>
            </tr>
            <tr>
                <td>**Phone calls**</td>
                <td>
                    The **phoneCall** capability allows apps to access all of the phone lines on the device and perform the following functions.

                    - Place a call on the phone line and show the system dialer without prompting the user.
                    - Access line-related metadata.
                    - Access line-related triggers.
                    - Allows the user-selected spam filter app to set and check block list and call origin information.

                    The **phoneCall** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="phoneCall"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                    The **phoneCallHistoryPublic** capability allows apps to read cellular and some VOIP call history information on the device. This capability also allows the app to write VOIP call history entries. This capability is required to access all members of the [**PhoneCallHistoryStore**](https://msdn.microsoft.com/library/windows/apps/Dn705931) class.
                </td>
            </tr>
            <tr>
                <td>**Ordner „Aufgezeichnete Anrufe“***</td>
                <td>
                    <p>Die **recordedCallsFolder**-Gerätefunktion ermöglicht Apps den Zugriff auf den Ordner mit aufgezeichneten Anrufen.</p>
                    <p>Die **recordedCallsFolder**-Funktion muss den **mobile**-Namespace enthalten, wenn sie im App-Paketmanifest wie unten dargestellt deklariert wird.</p>
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;mobile:Capability Name="recordedCallsFolder"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Benutzerkontoinformationen***</td>
                <td>
                    <p>Die **userAccountInformation**-Funktion ermöglicht Apps den Zugriff auf Name und Bild des Benutzers.</p>
                    <p>Diese Funktion wird für den Zugriff auf einige APIs aus dem Windows.System.User-Namespace benötigt.</p>
                    <p>Die **userAccountInformation**-Funktion muss den **uap**-Namespace enthalten, wenn sie im App-Paketmanifest wie unten dargestellt deklariert wird.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="userAccountInformation"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**VOIP-Anruf**</td>
                <td>
                    <p>Die **voipCall**-Gerätefunktion ermöglicht Apps den Zugriff auf die VOIP-Anruf-APIs im [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266)-Namespace.</p>
                    <p>Die **voipCall**-Funktion muss den **uap**-Namespace enthalten, wenn sie im App-Paketmanifest wie unten dargestellt deklariert wird.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="voipCall"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**3D-Objekte**</td>
                <td>
                    <p>Die **objects3D**-Funktion ermöglicht Apps den programmgesteuerten Zugriff auf die 3D-Objektdateien. Diese Funktion wird in der Regel in 3D-Apps und -Spielen verwendet, die auf die gesamte 3D-Objektbibliothek zugreifen müssen.</p>
                    <p>Diese Funktion ist für das Zugreifen auf den Ordner mit den 3D-Objekten erforderlich, indem die APIs im [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346)-Namespace verwendet werden.</p>
                    <p>Die **objects3D**-Funktion muss den **uap**-Namespace enthalten, wenn sie im App-Paketmanifest wie unten dargestellt deklariert wird.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="objects3d"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Blockierte Nachrichten lesen***</td>
                <td>
                    <p>Die **blockedChatMessages**-Funktion ermöglicht Apps das Lesen von SMS- und MMS-Nachrichten, die von der Spamfilter-App blockiert wurden.</p>
                    <p>Diese Funktion wird für den Zugriff auf die blockierten Nachrichten benötigt, indem APIs im [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321)-Namespace verwendet werden.</p>
                    <p>Die **blockedChatMessages**-Funktion muss den **uap**-Namespace enthalten, wenn sie im App-Paketmanifest wie unten dargestellt deklariert wird.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="blockedChatMessages"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Chatnachrichtzugriff**</td>
                <td>
                    <p>Die **chat**-Funktion ermöglicht Apps das Lesen und Löschen von Textnachrichten. Diese Funktion ermöglicht Apps darüber hinaus das Speichern von Chatnachrichten im Systemdatenspeicher.</p>
                    <p>Für diese Funktion sind einige APIs aus dem [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321)-Namespace erforderlich.</p>
                    <p>Die **chat**-Funktion muss den **uap**-Namespace enthalten, wenn sie im App-Paketmanifest wie unten dargestellt deklariert wird.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="chat"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**IoT – Low-Level Bushardware**</td>
                <td>
                    <p>Die **lowLevelDevices**-Funktion ermöglicht auf IoT-Geräten ausgeführten Apps den Zugriff auf Low-Level-Bushardware, z. B. I2C, GPIO, SPI, ADC und PWM.</p>
                    <p>Diese Funktion wird für den Zugriff auf einige APIs aus dem [**Windows.Devices.Spi**](https://msdn.microsoft.com/library/windows/apps/Dn708178)-Namespace benötigt.</p>
                    <p>Die **lowLevelDevices**-Funktion muss den **iot**-Namespace enthalten, wenn sie im App-Paketmanifest wie unten dargestellt deklariert wird.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;iot:Capability Name="lowLevelDevices"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**IoT – Systemverwaltung**</td>
                <td>
                    <p>Die **systemManagement**-Funktion ermöglicht Apps, über grundlegende Berechtigungen zur Systemadministration zu verfügen, z. B. Herunterfahren oder Neustarten, Gebietsschema und Zeitzone.</p>
                    <p>Diese Funktion wird für den Zugriff auf einige APIs aus dem [**Windows.System**](https://msdn.microsoft.com/library/windows/apps/BR241814)-Namespace benötigt.</p>
                    <p>Die **systemManagement**-Funktion muss den **iot**-Namespace enthalten, wenn sie im App-Paketmanifest wie unten dargestellt deklariert wird.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;iot:Capability Name="systemManagement"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
        </tbody>
</table>

 
## Gerätefunktionen

Gerätefunktionen ermöglichen Ihrer App den Zugriff auf Peripheriegeräte und interne Geräte. Gerätefunktionen werden mit dem **DeviceCapability**-Element in Ihrem App-Paketmanifest angegeben. Dieses Element erfordert unter Umständen zusätzliche untergeordnete Elemente. Einige Gerätefunktionen müssen dem Paketmanifest manuell hinzugefügt werden. Weitere Informationen finden Sie unter [Angeben von Gerätefunktionen in einem Paketmanifest](https://msdn.microsoft.com/library/windows/apps/Dn263092) und der [**Schemareferenz zu DeviceCapability**](https://msdn.microsoft.com/library/windows/apps/BR211430).

| Funktionsszenario | Funktionsnutzung |
|---------------------|------------------|
| **Ort**\* | Die **location**-Funktion ermöglicht den Zugriff auf die Funktion für den Standort, der von spezieller Hardware wie einem GPS-Sensor im PC abgerufen oder aus verfügbaren Netzwerkinformationen abgeleitet wird. Apps müssen Fälle verarbeiten können, in denen Benutzer die Positionsdienste über den Charm **Einstellungen** deaktiviert haben. |
| **Mikrofon** | Die **microphone**-Funktion ermöglicht den Zugriff auf den Audiofeed des Mikrofons, mit dem die App über ein verbundenes Mikrofon Audio aufzeichnen kann. Apps müssen Fälle verarbeiten können, in denen Benutzer das Mikrofon über den Charm **Einstellungen** deaktiviert haben. |
| **Näherung** | Die **proximity**-Funktion ermöglicht die Kommunikation mehrerer Geräte miteinander, die sich in der Nähe voneinander befinden. Diese Funktion ist typisch für Casual-Multiplayer-Spiele und Apps, die Informationen austauschen. Geräte versuchen, die Kommunikationstechnologie mit der besten Verbindung (wie Bluetooth, WLAN oder Internet) zu verwenden. Die Funktion dient lediglich dazu, die Kommunikation zwischen den Geräten zu initiieren. |
| **Webcam** | Die **webcam**-Funktion bietet Zugriff auf den Videofeed einer integrierten Kamera oder einer externen Webcam, sodass die App Fotos und Videos aufnehmen kann. Unter Windows müssen Apps Fälle verarbeiten können, in denen Benutzer die Kamera über den Charm **Einstellungen** deaktiviert haben.<br/>Die **webcam**-Funktion gewährt nur Zugriff auf den Videostream. Um außerdem Zugriff auf den Audiostream zu erhalten, muss die **microphone**-Funktion hinzugefügt werden. |
| **USB** | Die **usb**-Gerätefunktion ermöglicht den Zugriff auf APIs in [Aktualisieren des App-Manifestpakets für ein USB-Gerät](http://go.microsoft.com/fwlink/p/?LinkId=302259). |
| **Eingabegerät (Human Interface Device, HID)** | Die **humaninterfacedevice**-Gerätefunktion ermöglicht den Zugriff auf APIs in [So wird's gemacht: Angeben von Gerätefunktionen für HID](https://msdn.microsoft.com/library/windows/apps/Dn263091). |
| **Point of Service (POS)** | Die **pointOfService**-Gerätefunktion ermöglicht den Zugriff auf APIs im [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071)-Namespace. Dieser Namespace bietet Ihrer App Zugriff auf Point of Service (POS)-Barcodescanner und -Magnetstreifenleser. Der Namespace stellt eine anbieterneutrale Oberfläche für den Zugriff auf POS-Geräte verschiedener Hersteller über eine Windows Store-App bereit. |
| **Bluetooth** | Die **Bluetooth**-Gerätefunktion ermöglicht Apps die Kommunikation mit bereits gekoppelten Bluetooth-Geräten über das Generic Attribut (GATT)- oder Classic Basic Rate (RFCOMM)-Protokoll.<br/>Für diese Funktion sind einige APIs aus dem [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)-Namespace erforderlich. |
| **WLAN** | Die **wiFiControl**-Gerätefunktion ermöglicht Apps das Suchen nach WLANs sowie das Herstellen einer Verbindung.<br/>Für diese Funktion sind einige APIs aus dem [**Windows.Devices.WiFi**](https://msdn.microsoft.com/library/windows/apps/Dn975224)-Namespace erforderlich. |
| **Funkstatus** | Die **radios**-Gerätefunktion ermöglicht Apps das Umschalten zwischen WLAN- und Bluetooth-Funkempfängern.<br/>Für diese Funktion sind die APIs im [**Windows.Devices.Radios**](https://msdn.microsoft.com/library/windows/apps/Dn996447)-Namespace erforderlich.  |
| **Optischer Datenträger** | Die **optical**-Gerätefunktion ermöglicht Apps den Zugriff auf Funktionen für optische Laufwerke (z. B. für CDs, DVDs und Blu-Rays).<br/>Für diese Funktion sind einige APIs aus dem [**Windows.Devices.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn263667)-Namespace erforderlich. |
| **Bewegungsaktivität** | Mit der **activity**-Gerätefunktion können Apps die aktuelle Bewegung des Geräts erkennen.<br/>Für diese Funktion sind einige APIs aus dem [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)-Namespace erforderlich. |

## Spezielle und eingeschränkte Funktionen

**Wichtig**  
Funktionen zur speziellen und eingeschränkten Verwendung sind für besondere Szenarien vorgesehen. Die Verwendung dieser Funktionen ist stark eingeschränkt und unterliegt zusätzlichen Store-Onboarding-Richtlinien und -Prüfungen.

Es gibt Fälle, in denen solche Funktionen notwendig und angemessen sind. Dazu gehört beispielsweise das Banking mit zweistufiger Authentifizierung, bei der Benutzer eine Smartcard mit einem digitalen Zertifikat bereitstellen, das ihre Identität bestätigt. Andere Apps werden unter Umständen in erster Linie für Unternehmenskunden entworfen und erfordern ggf. Zugriff auf Unternehmensressourcen, auf die nur mit den Domänenanmeldeinformationen des Benutzers zugegriffen werden kann.

Bei Apps mit Sonderfunktionen wird für die Einreichung beim Store ein Unternehmenskonto benötigt. Im Gegensatz dazu stehen eingeschränkte Funktionen nicht für Entwickler zur Verfügung und erfordern auch kein spezielles Unternehmenskonto für den Store. Eingeschränkte Funktionen sind nur für Apps verfügbar, die von Microsoft und seinen Partnern entwickelt werden. Weitere Informationen zu Unternehmenskonten finden Sie unter [Kontotypen, Standorte und Gebühren](https://msdn.microsoft.com/library/windows/apps/JJ863494).

Alle eingeschränkten Funktionen müssen den **rescap**-Namespace enthalten, wenn sie anders als andere Funktionen im App-Paketmanifest deklariert werden. Im folgenden Beispiel wird veranschaulicht, wie die **appCaptureSettings**-Funktion deklariert wird.

```xml
<Capabilities>
    <rescap:Capability Name="appCaptureSettings"/>
</Capabilities>
```

Sie müssen auch die **xmlns:rescap**-Namespacedeklaration oben in der Datei „Package.appxmanifest“ hinzufügen, wie unten dargestellt.

```xml
<Package
    xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
    xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
    xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="uap mp wincap rescap">
```

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Funktionsszenario</th>
<th align="left">Funktionsnutzung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**Große Unternehmen**</td>
<td align="left"><p>Windows-Domänenanmeldeinformationen ermöglichen es Benutzern, sich unter Angabe ihrer Anmeldeinformationen bei Remoteressourcen anzumelden und sich so zu verhalten, als ob ein Benutzer ihren Benutzernamen und ihr Kennwort eingegeben hätte. Die **enterpriseAuthentication**-Sonderfunktion wird üblicherweise für Branchen-Apps verwendet, die Verbindungen mit Unternehmensservern herstellen.</p>
<p>Für die allgemeine Kommunikation über das Internet wird diese Funktion nicht benötigt.</p>

<p>Die **enterpriseAuthentication**-Sonderfunktion ist für die Unterstützung von Branchen-Apps vorgesehen. Deklarieren Sie sie nicht in Apps, die keinen Zugriff auf Unternehmensressourcen erfordern. Die [**Dateiauswahl**](https://msdn.microsoft.com/library/windows/apps/BR207847) bietet einen stabilen UI-Mechanismus, mit dem Benutzer Dateien auf einer Netzwerkfreigabe zur Verwendung mit einer App öffnen können. Deklarieren Sie die **enterpriseAuthentication**-Sonderfunktion nur, wenn die Szenarien für Ihre App programmgesteuerten Zugriff erfordern und nicht mithilfe der **Dateiauswahl** verwirklicht werden können.</p>
<p>Die **enterpriseAuthentication**-Funktion muss den **uap**-Namespace enthalten, wenn sie im App-Paketmanifest wie unten dargestellt deklariert wird.</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="enterpriseAuthentication"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div>
<p>Mit der **enterpriseDataPolicy**-Funktion können Apps unternehmensspezifische Richtlinien für das Gerät definieren und verwenden. Diese Funktion ist zur Verwendung aller Member der folgenden Klassen erforderlich.</p>
<ul>
<li>[**FileProtectionManager**](https://msdn.microsoft.com/library/windows/apps/Dn705151)</li>
<li>[**DataProtectionManager**](https://msdn.microsoft.com/library/windows/apps/Dn706017)</li>
<li>[**ProtectionPolicyManager**](https://msdn.microsoft.com/library/windows/apps/Dn705170)</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">**Freigegebene Benutzerzertifikate**</td>
<td align="left"><p>Mit der **sharedUserCertificates**-Sonderfunktion können Apps software- und hardwarebasierte Zertifikate im freigegebenen Benutzerspeicher, beispielsweise auf einer Smartcard gespeicherte Zertifikate, hinzufügen und darauf zugreifen. Diese Funktion wird typischerweise für Finanz- und Unternehmens-Apps verwendet, die eine Smartcard-basierte Authentifizierung erfordern.</p>
<p>Die **sharedUserCertificates**-Funktion muss den **uap**-Namespace enthalten, wenn sie im App-Paketmanifest wie unten dargestellt deklariert wird.</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="sharedUserCertificates"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div></td>
</tr>
<tr class="odd">
<td align="left">**Dokumente***</td>
<td align="left"><p>Die **documentsLibrary**-Sonderfunktion bietet programmgesteuerten Zugriff auf die Dokumente des Benutzers (gefiltert nach den im Paketmanifest deklarierten Dateitypzuordnungen), um den Offlinezugriff auf OneDrive zu ermöglichen. Wenn zum Beispiel eine DOC-Lese-App eine DOC-Dateitypzuordnung deklariert, kann die App DOC-Dateien unter „Dokumente“ öffnen, jedoch keine anderen Dateitypen.</p>
<p>Apps, die die **documentsLibrary**-Sonderfunktion deklarieren, können nicht auf Dokumente auf Heimnetzgruppen-Computern zugreifen. Die [file picker](https://msdn.microsoft.com/library/windows/apps/Hh465174) bietet einen stabilen UI-Mechanismus, mit dem Benutzer Dateien zur Verwendung mit einer App öffnen können. Deklarieren Sie die **documentsLibrary**-Sonderfunktion nur, wenn Sie die Dateiauswahl nicht verwenden können.</p>
<p>Zur Verwendung der **documentsLibrary**-Sonderfunktion muss die App folgende Voraussetzungen erfüllen:</p>
<ul>
<li>Ermöglichen des plattformübergreifenden Offlinezugriffs auf bestimmte OneDrive-Inhalte mit gültigen OneDrive-URLs oder Ressourcen-IDs</li>
<li>Automatisches Speichern geöffneter Dateien im Offlinemodus im OneDrive des Benutzers</li>
</ul>
<p>Apps, die die **documentsLibrary**-Sonderfunktion für diese beiden Zwecke verwenden, können mit der Funktion auch eingebettete Inhalte in anderen Dokumenten öffnen. Es werden nur die oben genannten Nutzungsmöglichkeiten der **documentsLibrary**-Sonderfunktion unterstützt.</p>
<ul>
<li><p>Die App kann nicht auf die Dokumentbibliothek im internen Speicher des Geräts zugreifen. Wenn eine andere App einen Ordner „Dokumente“ auf der optionalen SD-Karte erstellt, ist dieser Ordner für Ihre App sichtbar.</p></li>
</ul>
<p>Die **documentsLibrary**-Funktion muss den **uap**-Namespace enthalten, wenn sie im App-Paketmanifest wie unten dargestellt deklariert wird.</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="documentsLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div></td>
</tr>
<tr class="even">
<td align="left">**Einstellungen für Game DVR**</td>
<td align="left"><p>Die eingeschränkte **appCaptureSettings**-Funktion ermöglicht Apps die Steuerung der Benutzereinstellungen für Game DVR.</p>
<p>Für diese Funktion sind einige APIs aus dem [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/BR226738)-Namespace erforderlich.</p></td>
</tr>
<tr class="odd">
<td align="left">**Mobilfunk**</td>
<td align="left"><p>Die eingeschränkte **cellularDeviceControl**-Funktion ermöglicht Apps die Kontrolle über das Mobiltelefon.</p>
<p>Die **cellularDeviceIdentity**-Funktion ermöglicht Apps den Zugriff auf Mobilfunkidentifikationsdaten.</p>
<p>Die **cellularMessaging**-Funktion ermöglicht Apps die Nutzung von SMS und RCS.</p>
<p>Für diese Funktionen sind einige APIs aus den [**Windows.Devices.Sms**](https://msdn.microsoft.com/library/windows/apps/BR206567)-Namespaces erforderlich.</p>
<p>Ab Windows 10 rufen Apps [**AppIDList**](https://msdn.microsoft.com/library/windows/apps/Dn393996) auf).</p></td>
</tr>
<tr class="even">
<td align="left">**Geräteentsperrung**</td>
<td align="left"><p>Die eingeschränkte **deviceUnlock**-Funktion ermöglicht Apps das Entsperren eines Geräts für Querladeszenarien für Entwickler und Unternehmen.</p></td>
</tr>
<tr class="odd">
<td align="left">**Dual SIM-Kacheln**</td>
<td align="left"><p>Die eingeschränkte **dualSimTiles**-Funktion ermöglicht Apps das Erstellen eines zusätzlichen App-Listeneintrags auf Geräten mit mehreren SIMs.</p>
<p>Für diese Funktion sind einige APIs aus dem [**Windows.UI.StartScreen**](https://msdn.microsoft.com/library/windows/apps/BR242235)-Namespace erforderlich.</p></td>
</tr>
<tr class="even">
<td align="left">**Im Unternehmen freigegebener Speicher**</td>
<td align="left"><p>Die eingeschränkte **enterpriseDeviceLockdown**-Funktion ermöglicht Apps die Verwendung der API zur Gerätesperrung und den Zugriff auf im Unternehmen freigegebene Speicherordner.</p></td>
</tr>
<tr class="odd">
<td align="left">**Systemeingabeeinfügung**</td>
<td align="left"><p>Die eingeschränkte **inputInjection**-Funktion ermöglicht Apps das programmgesteuerte Einfügen verschiedener Eingabearten (z. B. HID, Touch, Stift, Tastatur oder Maus) ins System. Diese Funktion wird in der Regel für Apps für die Zusammenarbeit verwendet, die die Kontrolle über das System übernehmen können.</p>
<div class="alert">
**Hinweis**  Bei einem PC wird die Eingabeeinfügung durch eine App, die über diese Funktion verfügt, nur von Prozessen im selben App-Container empfangen.
</div>
</td>
</tr>
<tr class="even">
<td align="left">**Eingabeüberwachung***</td>
<td align="left"><p>Die eingeschränkte **inputObservation**-Funktion ermöglicht Apps das Überwachen verschiedener Arten von Rohdateneingaben (z. B. HID, Touch, Stift, Tastatur oder Maus), die vom System empfangen werden, unabhängig vom endgültigen Ziel.</p></td>
</tr>
<tr class="odd">
<td align="left">**Eingabeunterdrückung**</td>
<td align="left"><p>Die eingeschränkte **inputSuppression**-Funktion ermöglicht es Apps, den Empfang verschiedener Arten von Rohdateneingaben (z. B. HID, Touch, Stift, Tastatur oder Maus) durch das System zu unterdrücken.</p></td>
</tr>
<tr class="even">
<td align="left">**VPN-App**</td>
<td align="left"><p>Die eingeschränkte **networkingVpnProvider**-Funktion ermöglicht Apps den Vollzugriff auf VPN-Features. Sie haben dann u. a. die Möglichkeit, Verbindungen zu verwalten und VPN-Plug-In-Funktionen bereitzustellen.</p>
<p>Für diese Funktion sind einige APIs aus dem [**Windows.Networking.Vpn**](https://msdn.microsoft.com/library/windows/apps/Dn434040)-Namespace erforderlich.</p></td>
</tr>
<tr class="odd">
<td align="left">**Verwaltung anderer Apps**</td>
<td align="left"><p>Die eingeschränkte **packageManagement**-Funktion ermöglicht Apps die direkte Verwaltung anderer Apps.</p>
<p>Die **packageQuery**-Gerätefunktion ermöglicht Apps die Erfassung von Informationen zu anderen Apps.</p>
<p>Diese Funktionen werden für den Zugriff auf einige Methoden und Eigenschaften der [**PackageManager**](https://msdn.microsoft.com/library/windows/apps/BR240960)-Klasse benötigt.</p></td>
</tr>
<tr class="even">
<td align="left">**Externe Anzeige**</td>
<td align="left"><p>Die eingeschränkte **screenDuplication**-Funktion ermöglicht es Apps, den Bildschirm auf ein anderes Gerät zu projizieren.</p>
<p>Diese Funktion wird für die Verwendung von APIs im DirectX-Namespace benötigt.</p></td>
</tr>
<tr class="odd">
<td align="left">**Benutzerprinzipalnamen**</td>
<td align="left"><p>Mit der eingeschränkten **userPrincipalName**-Funktion können Apps den Miniaturansichtscache von Fotos ändern bzw. auf diesen zugreifen.</p>
<p>Diese Funktion wird zum Aufrufen der [**GetUserNameEx**](https://msdn.microsoft.com/library/windows/desktop/ms724435)-Funktion benötigt.</p></td>
</tr>
<tr class="even">
<td align="left">**Brieftasche**</td>
<td align="left"><p>Die eingeschränkte **walletSystem**-Funktion gewährt Apps vollständigen Zugriff auf die gespeicherten Ausweiskarten.</p>
<p>Für diese Funktion sind einige APIs aus dem [**Windows.ApplicationModel.Wallet.System**](https://msdn.microsoft.com/library/windows/apps/Mt171610)-Namespace erforderlich.</p></td>
</tr>
<tr class="odd">
<td align="left">**Positionsverlauf**</td>
<td align="left"><p>Die eingeschränkte **locationHistory**-Funktion ermöglicht Apps den Zugriff auf den Standortverlauf des Geräts.</p>
<p>Für diese Funktion sind die APIs im [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/BR225603)-Namespace erforderlich.</p></td>
</tr>
<tr class="even">
<td align="left">**App-Schließbestätigung**</td>
<td align="left"><p>Die eingeschränkte **confirmAppClose**-Funktion ermöglicht es Apps, sich selbst und eigene Fenster zu schließen und das Schließen der App zu verzögern.</p>
<p>Für diese Funktion sind einige APIs aus dem [**Windows.UI.ViewManagement**](https://msdn.microsoft.com/library/windows/apps/BR242295)-Namespace erforderlich.</p></td>
</tr>
<tr class="odd">
<td align="left">**Anrufliste***</td>
<td align="left"><p>Die eingeschränkte **phoneCallHistory**-Funktion ermöglicht Apps das Lesen und Löschen von Einträgen im Anrufverlauf.</p>
<p>Für diese Funktion sind APIs aus dem [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321)-Namespace erforderlich.</p></td>
</tr>
<tr class="even">
<td align="left">**Zugriff auf Termine auf Systemebene**</td>
<td align="left"><p>Die eingeschränkte **appointmentsSystem**-Funktion ermöglicht Apps das Lesen und Ändern aller Termine im Kalender des Benutzers.</p>
<p>Für diese Funktion sind APIs aus dem [**Windows.ApplicationModel.Appointment**](https://msdn.microsoft.com/library/windows/apps/Dn263359)-Namespace erforderlich.</p></td>
</tr>
<tr class="odd">
<td align="left">**Zugriff auf Chatnachrichten auf Systemebene***</td>
<td align="left"><p>Die eingeschränkte **chatSystem**-Funktion ermöglicht Apps das Lesen und Schreiben aller SMS- und MMS-Nachrichten.</p>
<p>Für diese Funktion sind APIs aus dem [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321)-Namespace erforderlich.</p></td>
</tr>
<tr class="even">
<td align="left">**Zugriff auf Kontakte auf Systemebene**</td>
<td align="left"><p>Die eingeschränkte **contactsSystem**-Funktion ermöglicht Apps das Lesen von Kontaktinformationen, die als eingeschränkt oder vertraulich gekennzeichnet sind, sowie das Ändern vorhandener Kontaktinformationen.</p>
<p>Für diese Funktion sind APIs aus dem [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321)-Namespace erforderlich.</p></td>
</tr>
<tr class="odd">
<td align="left">**E-Mail-Zugriff***</td>
<td align="left"><p>Die eingeschränkte **email**-Funktion ermöglicht Apps das Lesen, Auswählen und Senden von Benutzer-E-Mails.</p>
<p>Für diese Funktion sind APIs aus dem [**Windows.ApplicationModel.Email**](https://msdn.microsoft.com/library/windows/apps/Dn631285)-Namespace erforderlich.</p></td>
</tr>
<tr class="even">
<td align="left">**E-Mail-Zugriff auf Systemebene**</td>
<td align="left"><p>Die eingeschränkte **emailSystem**-Funktion ermöglicht Apps das Lesen, Auswählen und Senden eingeschränkter oder vertraulicher Benutzer-E-Mails.</p>
<p>Für diese Funktion sind APIs aus dem [**Windows.ApplicationModel.Email**](https://msdn.microsoft.com/library/windows/apps/Dn631285)-Namespace erforderlich.</p></td>
</tr>
<tr class="odd">
<td align="left">**Zugriff auf Anrufliste auf Systemebene**</td>
<td align="left"><p>Die eingeschränkte **phoneCallHistorySystem**-Funktion ermöglicht Apps die vollständige Änderung des Anrufverlaufs. Sie haben dann die Möglichkeit, vorhandene Einträge zu ändern und neue Einträge zu erstellen.</p>
<p>Für diese Funktion sind APIs aus dem [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266)-Namespace erforderlich.</p></td>
</tr>
<tr class="even">
<td align="left">**Senden von SMS-Nachrichten***</td>
<td align="left"><p>Die eingeschränkte **smsSend**-Funktion ermöglicht Apps das Senden von SMS- und MMS-Nachrichten.</p>
<p>Für diese Funktion sind APIs aus dem [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321)-Namespace erforderlich.</p></td>
</tr>
<tr class="odd">
<td align="left">**Zugriff auf alle Benutzerdaten auf Systemebene**</td>
<td align="left"><p>Die eingeschränkte **userDataSystem**-Funktion ermöglicht Apps den Zugriff auf Benutzerdaten im Systemdatenspeicher.</p></td>
</tr>
<tr class="even">
<td align="left">**Store-Vorschaufeatures**</td>
<td align="left"><p>Die eingeschränkte **previewStore**-Funktion ermöglicht Apps das Abrufen und Erwerben von SKUs von In-App-Produkten.</p>
<p>Für diese Funktion sind bestimmte APIs aus dem [**Windows.ApplicationModel.Store.Preview**](https://msdn.microsoft.com/library/windows/apps/Mt185546)-Namespace erforderlich.</p></td>
</tr>
<tr class="odd">
<td align="left">**Einstellungen beim ersten Anmelden**</td>
<td align="left"><p>Die eingeschränkte **firstSignInSettings**-Funktion ermöglicht Apps den Zugriff auf Benutzereinstellungen, die beim ersten Anmelden des Benutzers mit dem Gerät festgelegt wurden.</p></td>
</tr>
<tr class="even">
<td align="left">**Windows-Teambenutzeroberfläche**</td>
<td align="left"><p>Die eingeschränkte **teamEditionExperience**-Funktion ermöglicht Apps den Zugriff auf interne APIs, die zahlreiche erfahrungsbezogene Aspekte der Windows-Teamsitzung steuern. Eine Windows-Teamsitzung wird wahrscheinlich auf einem Teamgerät ausgeführt, z. B. Microsoft Surface Hub.</p></td>
</tr>
<tr class="odd">
<td align="left">**Remoteentsperrung**</td>
<td align="left"><p>Die eingeschränkte **remotePassportAuthentication**-Funktion ermöglicht Apps den Zugriff auf die Anmeldeinformationen, die zum Entsperren eines Remotecomputers verwendet werden können.</p></td>
</tr>
<tr class="even">
<td align="left">**Vorschaukomposition**</td>
<td align="left"><p>Die eingeschränkte **previewUiComposition**-Funktion ermöglicht Apps eine Vorschau des [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Dn706878)-Namespaces für ihre Benutzeroberfläche, sodass sie Feedback zur API bereitstellen können, bevor dieser abgeschlossen ist. Weitere Informationen erhalten Sie unter wincomposition@microsoft.com.</p></td>
</tr>
<tr class="odd">
<td align="left">**Sperrmodus für sichere Bewertungen**</td>
<td align="left"><p>Die eingeschränkte **secureAssessment**-Funktion ermöglicht es Apps, Windows in den Einzel-App-Modus zu versetzen, um sichere Bewertungen zu ermöglichen.</p></td>
</tr>
<tr class="even">
<td align="left">**Bereitstellung des Verbindungs-Managers**</td>
<td align="left"><p>Die eingeschränkte **networkConnectionManagerProvisioning**-Funktion ermöglicht Apps das Definieren der Richtlinien, die das Gerät mit WWAN- und WLAN-Schnittstellen verbinden. Apps, die diese Funktion verwenden, werden von Mobilfunkanbietern zum Steuern der Geräteverbindung mit dem mobilen Netzwerk erstellt.</p></td>
</tr>
<tr class="odd">
<td align="left">**Datenplanbereitstellung**</td>
<td align="left"><p>Die eingeschränkte **networkDataPlanProvisioning**-Funktion ermöglicht Apps das Erfassen von Informationen über Datenpläne auf dem Gerät und das Lesen der Netzwerkverwendung. Apps, die diese Funktion verwenden, werden von Mobilfunkanbietern zum Integrieren der tatsächlichen Datennutzung ihrer Kunden in der Einstellung für die Betriebssystemdatennutzung erstellt.</p></td>
</tr>
<tr class="even">
<td align="left">**Softwarelizenzierung**</td>
<td align="left"><p>Die eingeschränkte **slapiQueryLicenseValue**-Funktion ermöglicht Apps das Abfragen der Softwarelizenzierungsrichtlinien.</p></td>
</tr>
<tr class="odd">
<td align="left">**Erweiterte Ausführung**</td>
<td align="left"><p>Die eingeschränkte **extendedExecutionBackgroundAudio**-Funktion ermöglicht Apps die Audiowiedergabe, wenn die App nicht im Vordergrund ist.</p>
<p>Die eingeschränkte **extendedExecutionCritical**-Funktion ermöglicht es Apps, eine wichtige erweiterte Ausführungssitzung zu starten.</p>
<p>Die eingeschränkte **extendedExecutionUnconstrained**-Funktion ermöglicht es Apps, eine unbeschränkte erweiterte Ausführungssitzung zu starten.</p></td>
</tr>
<tr class="even">
<td align="left">**Mobile Geräteverwaltung**</td>
<td align="left"><p>Die eingeschränkte **deviceManagementDmAccount**-Funktion ermöglicht Apps das Bereitstellen und Konfigurieren von MO OMA-DM-Konten (Mobile Operator Open Mobile Alliance – Device Management, MO OMA-DM).</p>
<p>Die eingeschränkte **deviceManagementFoundation**-Funktion ermöglicht Apps grundlegenden Zugriff auf die CSP-Infrastruktur (Configuration Service Provider, Konfigurationsdienstanbieter) der Mobilen Geräteverwaltung (Mobile Device Management, MDM) auf dem Gerät. Beachten Sie, dass andere Funktionen benötigt werden, um auf bestimmte CSPs zuzugreifen.</p>
<p>Die eingeschränkte **deviceManagementWapSecurityPolicies**-Funktion ermöglicht Apps das Konfigurieren WAP-basierter (Wireless Application-Protokoll) Dienste, z. B. MMs, SI/SL (Service Indication/Service Loading) und von OMA-CP (Open Mobile Alliance – Client Provisioning).</p>
<p>Die eingeschränkte **deviceManagementEmailAccount**-Funktion ermöglicht den von Mobilfunkanbietern erstellten Apps das Hinzufügen und Verwalten eines E-Mail-Kontos auf Geräten, die für Benutzer bereitgestellt werden.</p></td>
</tr>
<tr class="odd">
<td align="left">**Paketrichtliniensteuerung**</td>
<td align="left"><p>Die eingeschränkte **packagePolicySystem**-Funktion ermöglicht Apps das Steuern der Systemrichtlinien für Apps, die auf dem Gerät installiert sind.</p></td>
</tr>
<tr class="even">
<td align="left">**Spieleliste**</td>
<td align="left"><p>Die eingeschränkte **gameList**-Funktion ermöglicht Apps das Abrufen einer Liste mit bekannten Spielen, die auf dem System installiert sind.</p></td>
</tr>
<tr class="odd">
<td align="left">**Xbox-Zubehör**</td>
<td align="left"><p>Die eingeschränkte **xboxAccessoryManagement**-Funktion ermöglicht Apps das direkte Verwalten von Xbox-Geräten, die der Xbox-Hardwarespezifikation entsprechen.</p></td>
</tr>
<tr class="even">
<td align="left">**Spracherkennung für Zubehör**</td>
<td align="left"><p>Die eingeschränkte **cortanaSpeechAccessory**-Funktion ermöglicht es Apps, Befehle aufzurufen und an Cortana zu übergeben.</p></td>
</tr>
<tr class="odd">
<td align="left">**Zubehörverwaltung**</td>
<td align="left"><p>Die eingeschränkte **accessoryManager**-Funktion ermöglicht Apps das Registrieren als Zubehör-App und Abonnieren von bestimmten App-Benachrichtigungen, damit sie an Zubehör weitergeleitet und dem Benutzer angezeigt werden können.</p></td>
</tr>
<tr class="even">
<td align="left">**Treiberzugriff**</td>
<td align="left"><p>Die eingeschränkte **interopServices**-Funktion ermöglicht Apps die direkte Interaktion mit Treibern.</p></td>
</tr>
<tr class="odd">
<td align="left">**Vordergrundbeobachtung**</td>
<td align="left"><p>Die eingeschränkte **InputForegroundObservation**-Funktion ermöglicht es Apps im Vordergrund, Tastatureingaben abzufangen und die gesamte Verarbeitung von Nicht-App-Tastatureingaben zu umgehen. Durch diese Funktion können keine SAS-Kombinationen abgefangen werden. Sie ist für den Zugriff auf alle Member der [**KeyboardDeliveryInterceptor**](https://msdn.microsoft.com/library/windows/apps/Mt608395)-Klasse erforderlich.</p></td>
</tr>
<tr class="even">
<td align="left">**OEM- und MO-Partner-Apps**</td>
<td align="left"><p>Die eingeschränkte **oemDeployment**-Funktion ermöglicht es Apps, die von Microsoft-Partnern erstellt wurden, neue Apps zu installieren und derzeit auf dem Gerät installierte Apps abzufragen.</p>
<p>Die eingeschränkte **oemPublicDirectory**-Funktion ermöglicht es Apps, die von Microsoft-Partnern erstellt wurden, auf den freigegebenen App-Ordner zuzugreifen.</p></td>
</tr>
<tr class="odd">
<td align="left">**App-Lizenzierung**</td>
<td align="left"><p>Die eingeschränkte **appLicensing**-Funktion ermöglicht die Ausführung von Apps ohne Lizenz. Apps, in deren Manifest diese Funktion deklariert wird, können nicht an den Store übermittelt werden. Jegliche Anforderung des Zugriffs auf diese Funktion mit dem Ziel der Übermittlung an den Store wird abgelehnt.</p></td>
</tr>
<tr class="even">
<td align="left">**Ortssystem**</td>
<td align="left"><p>Mit der eingeschränkten **locationSystem**-Funktion können Apps bestimmte privilegierte Ortskonfigurationen durchführen, um etwa den Standardort für das Gerät festzulegen. Apps, in deren Manifest diese Funktion deklariert wird, können nicht an den Store übermittelt werden. Jegliche Anforderung des Zugriffs auf diese Funktion mit dem Ziel der Übermittlung an den Store wird abgelehnt.</p></td>
</tr>
</tbody>
</table>

**Hinweis**  
Dieser Artikel ist für Windows 10-Entwickler gedacht, die UWP-Apps schreiben. Wenn Sie für Windows 8.x oder Windows Phone 8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

## Verwandte Themen

* [Manifest-Designer](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/br230259.aspx)
* [Richtlinien für Apps mit Berücksichtigung von Datenschutz](https://msdn.microsoft.com/library/windows/apps/Hh768223)
* [Angeben von Funktionen in einem Paketmanifest](https://msdn.microsoft.com/library/windows/apps/BR211477)
* [Angeben von Gerätefunktionen in einem Paketmanifest](https://msdn.microsoft.com/library/windows/apps/Dn263092)
 


<!--HONumber=Mar16_HO5-->


