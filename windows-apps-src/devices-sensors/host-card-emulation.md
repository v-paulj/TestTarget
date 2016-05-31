---
author: DBirtolo
ms.assetid: 26834A51-512B-485B-84C8-ABF713787588
title: Erstellen einer NFC-Smartcard-App
description: Windows Phone 8.1 hat Apps mit NFC-Kartenemulation per SIM-basiertem sicherem Element unterstützt. Für dieses Modell war es aber erforderlich, dass Apps für das sichere Bezahlen eng mit den Betreibern von mobilen Netzwerken gekoppelt waren.
---
# Erstellen einer NFC-Smartcard-App

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Wichtig**  Dieses Thema trifft nur auf Windows 10 Mobile zu.

Windows Phone 8.1 hat Apps mit NFC-Kartenemulation per SIM-basiertem sicherem Element unterstützt. Für dieses Modell war es aber erforderlich, dass Apps für das sichere Bezahlen eng mit den Betreibern von mobilen Netzwerken gekoppelt waren. Dadurch wurde die Vielfältigkeit möglicher Zahlungslösungen anderer Händler oder Entwickler eingeschränkt, die nicht mit Betreibern von mobilen Netzwerken gekoppelt waren. In Windows 10 Mobile haben wir eine neue Technologie für die Kartenemulation eingeführt, die die Bezeichnung „Host-Kartenemulation“ (Host Card Emulation, HCE) trägt. Mithilfe von HCE-Technologie kann Ihre App direkt mit einem NFC-Kartenleser kommunizieren. In diesem Thema wird veranschaulicht, wie die Host-Kartenemulation (HCE) für Windows 10 Mobile-Geräte funktioniert und wie Sie eine HCE-App entwickeln können, bei der Kunden ohne Zusammenarbeit mit dem Betreiber eines mobilen Netzwerks per Smartphone auf Ihre Dienste zugreifen können, anstatt mit einer physischen Karte.

## Voraussetzungen für die Entwicklung einer HCE-App


Sie müssen Ihre Entwicklungsumgebung entsprechend vorbereiten, um eine HCE-basierte Kartenemulations-App für Windows 10 Mobile entwickeln zu können. Hierzu können Sie die Anwendung Microsoft Visual Studio 2015 installieren, die die Windows-Entwicklertools und den Windows 10 Mobile-Emulator mit NFC-Emulationsunterstützung enthält. Weitere Informationen zur Einrichtung finden Sie unter [Vorbereiten](https://msdn.microsoft.com/library/windows/apps/Dn726766).

Wenn Sie zum Testen anstelle des vorhandenen Windows 10 Mobile-Emulators optional ein echtes Windows 10 Mobile-Gerät verwenden möchten, benötigen Sie außerdem Folgendes:

-   Windows 10 Mobile-Gerät mit NFC-HCE-Unterstützung. Derzeit verfügen das Lumia 730, 830, 640 und 640 XL über die Hardware zur Unterstützung von NFC-HCE-Apps.
-   Lesegeräteinheit, die die Protokolle ISO/IEC 14443-4 und ISO/IEC 7816-4 unterstützt.

Windows 10 Mobile implementiert einen HCE-Dienst mit den folgenden Funktionen:

-   Apps können die Applet-IDs (AIDs) für die Karten registrieren, die emuliert werden sollen.
-   Die Konfliktlösung und das Routing des Anwendungsprotokoll-Dateneinheit-Befehls (Application Protocol Data Unit, APDU) und der Antwort werden mit einer der registrierten Apps basierend auf der Auswahl des externen Kartenlesers und der Benutzereinstellung gekoppelt.
-   Behandlung der Ereignisse und Benachrichtigungen für die Apps als Ergebnis von Benutzeraktionen.

Windows 10 unterstützt die Emulation von Smartcards, die auf ISO-DEP (ISO-IEC 14443-4) basieren, und kommuniziert über APDUs, die in der ISO-IEC 7816-4-Spezifikation definiert sind. Windows 10 unterstützt für HCE-Apps die Technologie „ISO/IEC 14443-4 Typ A“. Für Technologien wie Typ B, Typ F und Nicht-ISO-DEP (z. B. MIFARE) wird die Weiterleitung an die SIM standardmäßig durchgeführt.

Die Kartenemulationsfunkton ist nur für Windows 10 Mobile-Geräte aktiviert. Die SIM-basierte und HCE-basierte Kartenemulation ist für andere Versionen von Windows 10 nicht verfügbar.

Das folgende Diagramm zeigt die Architektur für die Unterstützung der HCE- und SIM-basierten Kartenemulation. 

![](./images/nfc-architecture.png)

## App-Auswahl und AID-Routing

Für die Entwicklung einer HCE-App müssen Sie wissen, wie bei Windows 10 Mobile-Geräten die Weiterleitung von AIDs an eine bestimmte App funktioniert, da Benutzer mehrere unterschiedliche HCE-Apps installieren können. Jede App kann mehrere HCE- und SIM-basierte Karten registrieren. Ältere SIM-basierte Windows Phone 8.1-Apps funktionieren unter Windows 10 Mobile weiterhin, solange der Benutzer im Menü mit den NFC-Einstellungen die Option „SIM-Karte“ als standardmäßige Zahlungskarte auswählt. Diese Standardeinstellung wird festgelegt, wenn das Gerät zum ersten Mal eingeschaltet wird.

Wenn Benutzer mit ihrem Windows 10 Mobile-Gerät ein Terminal berühren, werden die Daten automatisch an die richtige auf dem Gerät installierte App geleitet. Diese Weiterleitung basiert auf den Applet-IDs (AIDs), bei denen es sich um Bezeichner mit 5 - 16 Byte handelt. Während dieses Vorgangs überträgt das externe Terminal eine SELECT-Befehl-APDU, um die AID anzugeben, an die alle nachfolgenden APDU-Befehle geleitet werden sollen. Mit den nachfolgenden SELECT-Befehlen wird die Weiterleitung wieder geändert. Basierend auf den von Apps registrierten AIDs und den Benutzereinstellungen wird der APDU-Datenverkehr an eine bestimmte App geleitet, die dann eine APDU als Antwort sendet. Beachten Sie hierbei, dass ein Terminal während eines Vorgangs unter Umständen versucht, mit mehreren unterschiedlichen Apps zu kommunizieren. Sie müssen also sicherstellen, dass die Hintergrundaufgabe Ihrer App nach der Deaktivierung so schnell wie möglich beendet wird, damit die Hintergrundaufgabe einer anderen App auf die APDU antworten kann. Hintergrundaufgaben werden später in diesem Thema beschrieben.

HCE-Apps müssen sich mit bestimmten AIDs registrieren, die sie verarbeiten können, damit sie APDUs für eine AID erhalten. Apps deklarieren AIDs über AID-Gruppen. Eine AID-Gruppe entspricht vom Konzept her einer individuellen physischen Karte. Beispielsweise wird eine Kreditkarte mit einer AID-Gruppe deklariert, und eine zweite Kreditkarte einer anderen Bank wird mit einer anderen zweiten AID-Gruppe deklariert, auch wenn beide unter Umständen über die gleiche AID verfügen.

## Konfliktlösung für AID-Gruppen für die Zahlung

Wenn eine App physische Karten (AID-Gruppen) registriert, kann sie die AID-Gruppenkategorie entweder als „Payment“ (Zahlung) oder „Other“ (Sonstiges) deklarieren. Es können zwar mehrere AID-Zahlungsgruppen gleichzeitig registriert sein, aber nur jeweils eine dieser AID-Zahlungsgruppen kann für den Vorgang „Bezahlen per Berührung“ aktiviert sein. Die Auswahl erfolgt durch den Benutzer. Der Grund für dieses Verhalten ist, dass Benutzer bewusst auswählen möchten, welche Zahlungs-, Kredit- oder Debitkarte jeweils verwendet wird, damit der Bezahlvorgang nicht über eine unerwünschte Karte abgewickelt wird, wenn das Terminal mit dem Gerät berührt wird.

Gleichzeitig können aber mehrere AID-Gruppen, die unter „Other“ (Sonstiges) registriert sind, ohne Benutzerinteraktion aktiviert sein. Dieses Verhalten ist vorhanden, weil für andere Arten von Karten (z. B. Treue-, Gutschein- oder Transit-Karten) erwartet wird, dass sie ohne jegliches Zutun oder Eingaben funktionieren, wenn das Terminal mit dem Smartphone berührt wird.

Alle AID-Gruppen, die als „Payment“ (Zahlung) registriert sind, werden auf der Seite mit den NFC-Einstellungen in der Kartenliste angezeigt, sodass Benutzer ihre Standardkarte für Zahlungen auswählen können. Wenn eine Standardkarte für Zahlungen ausgewählt wird, wird die App, mit der die jeweilige AID-Zahlungsgruppe registriert wurde, zur standardmäßigen App für Zahlungen. Standardmäßige Apps für Zahlungen können alle eigenen AID-Gruppen ohne Benutzerinteraktion aktivieren und deaktivieren. Wenn der Benutzer die Aufforderung zum Wählen der standardmäßigen App für die Bezahlung ablehnt, wird die derzeit als Standard-App ausgewählte App (falls vorhanden) weiter als Standardeinstellung verwendet. Im folgenden Screenshot ist die Seite mit den NFC-Einstellungen dargestellt.

![](./images/nfc-settings.png)

Gemäß dem obigen Beispiel-Screenshot: Wenn der Benutzer seine Standardkarte für Zahlungen in eine andere Karte ändert, die nicht von „HCE Application 1“ registriert wurde, erstellt das System eine Bestätigungsaufforderung, damit der Benutzer seine Zustimmung geben kann. Falls der Benutzer seine Standardkarte für Zahlungen aber in eine andere Karte ändert, die von „HCE Application 1“ registriert wurde, erstellt das System keine Bestätigungsaufforderung für den Benutzer, weil „HCE Application 1“ bereits die Standard-App für Zahlungen ist.

## Konfliktlösung für andere AID-Gruppen

Karten, die nicht für die Bezahlung bestimmt und in der Kategorie „Other“ (Sonstiges) enthalten sind, werden nicht auf der Seite mit den NFC-Einstellungen angezeigt.

In Ihrer App können nicht für die Bezahlung bestimmte AID-Gruppen genauso wie AID-Zahlungsgruppen erstellt, registriert und aktiviert werden. Der Hauptunterschied besteht darin, dass die Emulationskategorie für nicht für die Bezahlung bestimmte AID-Gruppen auf „Other“ (Sonstiges) festgelegt ist und nicht auf „Payment“ (Zahlung). Nach dem Registrieren der AID-Gruppe im System müssen Sie die AID-Gruppe aktivieren, damit NFC-Datenverkehr empfangen werden kann. Wenn Sie versuchen, eine nicht für die Bezahlung bestimmte AID-Gruppe für den Empfang von Datenverkehr zu aktivieren, wird dem Benutzer keine Bestätigungsaufforderung angezeigt. Dies ist nur der Fall, wenn ein Konflikt mit einer der AIDs besteht, die von einer anderen App bereits im System registriert wurden. Wenn ein Konflikt besteht, werden dem Benutzer Informationen dazu angezeigt, welche Karte und die dazugehörige App deaktiviert werden, wenn der Benutzer die neu registrierte AID-Gruppe aktiviert.

**Koexistenz mit SIM-basierten NFC-Anwendungen**

In Windows 10 Mobile richtet das System die NFC-Controller-Routingtabelle ein, die zum Treffen von Routingentscheidungen auf Controllerebene verwendet wird. Die Tabelle enthält Routinginformationen für die folgenden Elemente:

-   Einzelne AID-Routen
-   Protokollbasierte Route (ISO-DEP)
-   Technologiebasiertes Routing (NFC-A/B/F)

Wenn eine externe Leseeinheit den Befehl „SELECT AID” sendet, überprüft der NFC-Controller zuerst die AID-Routen in der Routingtabelle auf eine Übereinstimmung. Falls keine Übereinstimmung vorhanden ist, wird die protokollbasierte Route als Standardroute für ISO-DEP (14443-4-A)-Datenverkehr verwendet. Für anderen Datenverkehr, der nicht auf ISO-DEP basiert, wird das technologiebasierte Routing verwendet.

Windows 10 Mobile enthält auf der Seite mit den NFC-Einstellungen die Menüoption „SIM-Karte“, damit weiterhin ältere SIM-basierte Windows Phone 8.1-Apps genutzt werden können, bei denen die AIDs nicht im System registriert werden. Wenn Benutzer „SIM-Karte“ als Standardkarte für Zahlungen auswählen, wird die ISO-DEP-Route auf „UICC“ festgelegt. Für alle anderen Auswahlmöglichkeiten im Dropdownmenü verläuft die ISO-DEP-Route zum Host.

Die ISO-DEP-Route wird für Geräte auf „SIM-Karte“ festgelegt, die über eine SE-fähige SIM-Karte verfügen, wenn das Gerät zum ersten Mal mit Windows 10 Mobile gestartet wird. Wenn der Benutzer eine HCE-fähige App installiert und diese App HCE-AID-Gruppenregistrierungen aktiviert, zeigt die ISO-DEP-Route auf den Host. Für neue SIM-basierte Anwendungen müssen die AIDs auf der SIM-Karte registriert werden, damit die jeweiligen AID-Routen in die Routingtabelle des Controllers eingefügt werden können.

## Erstellen einer HCE-basierten App

Ihre HCE-App besteht aus zwei Teilen:

-   Vordergrund-App für die Benutzerinteraktion
-   Hintergrundaufgabe, die vom System zum Verarbeiten von APDUs für eine bestimmte AID ausgelöst wird

Aufgrund der extrem hohen Leistungsanforderungen in Bezug auf das Laden der Hintergrundaufgabe als Antwort auf eine NFC-Berührung ist es ratsam, die gesamte Hintergrundaufgabe in systemeigenem C++/CX-Code zu implementieren (einschließlich aller erforderlichen Abhängigkeiten, Verweise oder Bibliotheken), anstatt in C# oder verwaltetem Code. C# und verwalteter Code bieten zwar normalerweise eine gute Leistung, aber es fällt auch Mehraufwand an, z. B. beim Laden von .NET CLR, der durch die Verwendung von C++/CX vermieden werden kann.
## Erstellen und Registrieren einer Hintergrundaufgabe

Sie müssen in Ihrer HCE-App eine Hintergrundaufgabe für die Verarbeitung und Beantwortung von APDUs erstellen, die vom System weitergeleitet werden. Beim ersten Starten Ihrer App wird im Vordergrund eine HCE-Hintergrundaufgabe registriert, mit der die [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/BR224803)-Schnittstelle implementiert wird, wie im folgenden Code gezeigt.

```csharp
var taskBuilder = new BackgroundTaskBuilder();
taskBuilder.Name = bgTaskName;
taskBuilder.TaskEntryPoint = taskEntryPoint;
taskBuilder.SetTrigger(new SmartCardTrigger(SmartCardTriggerType.EmulatorHostApplicationActivated));
bgTask = taskBuilder.Register();
```

Beachten Sie, dass der Aufgabentrigger auf [**SmartCardTriggerType**](https://msdn.microsoft.com/library/windows/apps/Dn608017) festgelegt ist. **EmulatorHostApplicationActivated**. Dies bedeutet Folgendes: Wenn vom Betriebssystem die APDU eines SELECT AID-Befehls empfangen und für Ihre App aufgelöst wird, wird Ihre Hintergrundaufgabe gestartet.

## Empfangen und Beantworten von APDUs

Wenn eine für Ihre App bestimmte APDU vorhanden ist, startet das System Ihre Hintergrundaufgabe. Ihre Hintergrundaufgabe empfängt die APDU, die über die [**CommandApdu**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.smartcards.smartcardemulatorapdureceivedeventargs.commandapdu.aspx)-Eigenschaft des [**SmartCardEmulatorApduReceivedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Dn894640)-Objekts übergeben wird, und antwortet auf die APDU mit der [**TryRespondAsync**](https://msdn.microsoft.com/en-us/library/windows/apps/mt634299.aspx)-Methode desselben Objekts. Erwägen Sie, die Hintergrundaufgabe aus Leistungsgründen für kleinere Vorgänge beizubehalten. Beantworten Sie die APDUs beispielsweise sofort, und beenden Sie die Hintergrundaufgabe, wenn die gesamte Verarbeitung abgeschlossen ist. Aufgrund der Art von NFC-Transaktionen tendieren Benutzer dazu, ihr Gerät nur für einen sehr kurzen Zeitraum an die Leseeinheit zu halten. Die Hintergrundaufgabe empfängt weiter Datenverkehr von der Leseeinheit, bis die Verbindung deaktiviert wird. Sie erhalten dann ein [**SmartCardEmulatorConnectionDeactivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Dn894644)-Objekt. Die Verbindung kann aus den folgenden Gründen deaktiviert werden, wie in der [**SmartCardEmulatorConnectionDeactivatedEventArgs.Reason**](https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.smartcardemulatorconnectiondeactivatedeventargs.reason)-Eigenschaft angegeben.

-   Wenn die Verbindung mit dem **ConnectionLost**-Wert deaktiviert wird, bedeutet dies, dass der Benutzer das Gerät von der Leseeinheit entfernt hat. Falls Ihre App eine längere Berührung des Terminals erfordert, können Sie für den Benutzer entsprechendes Feedback anzeigen. Beenden Sie die Hintergrundaufgabe schnell (per Abschluss der Verzögerung), um sicherzustellen, dass es bei der erneuten Berührung nicht zu einer Verzögerung kommt, weil auf die Beendigung der vorherigen Hintergrundaufgabe gewartet werden muss.
-   Wenn die Verbindung über **ConnectionRedirected** deaktiviert wird, bedeutet dies, dass vom Terminal eine neue SELECT AID-Befehl-APDU für eine andere AID gesendet wurde. In diesem Fall sollte die App die Hintergrundaufgabe sofort beenden (per Abschluss der Verzögerung), um die Ausführung einer anderen Hintergrundaufgabe zu ermöglichen.

Außerdem sollte die Hintergrundaufgabe für das [**Canceled-Ereignis**](https://msdn.microsoft.com/library/windows/apps/BR224798) unter [**IBackgroundTaskInstance interface**](https://msdn.microsoft.com/library/windows/apps/BR224797) registriert werden und auch hier wieder schnell beendet werden (per Abschluss der Verzögerung), da dieses Ereignis vom System ausgelöst wird, wenn die Verarbeitung der Hintergrundaufgabe abgeschlossen ist. Unten ist Code angegeben, mit dem eine Hintergrundaufgabe einer HCE-App veranschaulicht wird.

```csharp
void BgTask::Run(
    IBackgroundTaskInstance^ taskInstance)
{
    m_triggerDetails = static_cast<SmartCardTriggerDetails^>(taskInstance->TriggerDetails);
    if (m_triggerDetails == nullptr)
    {
        // May be not a smart card event that triggered us
        return;
    }

    m_emulator = m_triggerDetails->Emulator;
    m_taskInstance = taskInstance;

    switch (m_triggerDetails->TriggerType)
    {
    case SmartCardTriggerType::EmulatorHostApplicationActivated:
        HandleHceActivation();
        break;

    case SmartCardTriggerType::EmulatorAppletIdGroupRegistrationChanged:
        HandleRegistrationChange();
        break;

    default:
        break;
    }
}

void BgTask::HandleHceActivation()
{
 try
 {
        auto lock = m_srwLock.LockShared();
        // Take a deferral to keep this background task alive even after this "Run" method returns
        // You must complete this deferal immediately after you have done processing the current transaction
        m_deferral = m_taskInstance->GetDeferral();

        DebugLog(L"*** HCE Activation Background Task Started ***");

        // Set up a handler for if the background task is cancelled, we must immediately complete our deferral
        m_taskInstance->Canceled += ref new Windows::ApplicationModel::Background::BackgroundTaskCanceledEventHandler(
            [this](
            IBackgroundTaskInstance^ sender,
            BackgroundTaskCancellationReason reason)
        {
            DebugLog(L"Cancelled");
            DebugLog(reason.ToString()->Data());
            EndTask();
        });

        if (Windows::Phone::System::SystemProtection::ScreenLocked)
        {
            auto denyIfLocked = Windows::Storage::ApplicationData::Current->RoamingSettings->Values->Lookup("DenyIfPhoneLocked");
            if (denyIfLocked != nullptr && (bool)denyIfLocked == true)
            {
                // The phone is locked, and our current user setting is to deny transactions while locked so let the user know
                // Denied
                DoLaunch(Denied, L"Phone was locked at the time of tap");

                // We still need to respond to APDUs in a timely manner, even though we will just return failure
                m_fDenyTransactions = true;
            }
        }
        else
        {
            m_fDenyTransactions = false;
        }

        m_emulator->ApduReceived += ref new TypedEventHandler<SmartCardEmulator^, SmartCardEmulatorApduReceivedEventArgs^>(
            this, &BgTask::ApduReceived);

        m_emulator->ConnectionDeactivated += ref new TypedEventHandler<SmartCardEmulator^, SmartCardEmulatorConnectionDeactivatedEventArgs^>(
                [this](
                SmartCardEmulator^ emulator,
                SmartCardEmulatorConnectionDeactivatedEventArgs^ eventArgs)
            {
                DebugLog(L"Connection deactivated");
                EndTask();
            });

  m_emulator->Start();
        DebugLog(L"Emulator started");
 }
 catch (Exception^ e)
 {
        DebugLog(("Exception in Run: " + e->ToString())->Data());
        EndTask();
 }
}
```
## Erstellen und Registrieren von AID-Gruppen

Wenn beim ersten Starten der Anwendung die Karte bereitgestellt wird, erstellen und registrieren Sie AID-Gruppen im System. Das System bestimmt die App, mit der eine externe Leseeinheit kommunizieren möchte. Die APDUs werden basierend auf den registrierten AIDs und Benutzereinstellungen weitergeleitet.

Die meisten Zahlungskarten werden für die gleiche AID (PPSE AID) und für weitere spezielle AIDs für Zahlungsnetzwerke registriert. Jede AID-Gruppe steht für eine Karte, und wenn Benutzer die Karte aktivieren, werden alle AIDs der Gruppe aktiviert. Ebenso werden alle AIDs der Gruppe deaktiviert, wenn der Benutzer die Karte deaktiviert.

Zum Registrieren einer AID-Gruppe müssen Sie ein [**SmartCardAppletIdGroup**](https://msdn.microsoft.com/library/windows/apps/Dn910955)-Objekt erstellen und dessen Eigenschaften so festlegen, dass angegeben wird, dass es sich um eine HCE-basierte Zahlungskarte handelt. Ihr Anzeigename sollte für Benutzer aussagekräftig sein, da er im Menü mit den NFC-Einstellungen und in Eingabeaufforderungen angezeigt wird. Für HCE-Zahlungskarten sollte die [**SmartCardEmulationCategory**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationcategory.aspx)-Eigenschaft auf **Payment** und die [**SmartCardEmulationType**](https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationtype)-Eigenschaft auf **Host** festgelegt sein.

```csharp
public static byte[] AID_PPSE =
        {
            // File name "2PAY.SYS.DDF01" (14 bytes)
            (byte)'2', (byte)'P', (byte)'A', (byte)'Y',
            (byte)'.', (byte)'S', (byte)'Y', (byte)'S',
            (byte)'.', (byte)'D', (byte)'D', (byte)'F', (byte)'0', (byte)'1'
        };

var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName", 
                                new List<IBuffer> {AID_PPSE.AsBuffer()},
                                SmartCardEmulationCategory.Payment,
                                SmartCardEmulationType.Host);
```

Für nicht für die Zahlung bestimmte HCE-Karten sollte die [**SmartCardEmulationCategory**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationcategory.aspx)-Eigenschaft auf **Other** und die [**SmartCardEmulationType**](https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationtype)-Eigenschaft auf **Host** festgelegt sein.

```csharp
public static byte[] AID_OTHER =
        {
            (byte)'1', (byte)'2', (byte)'3', (byte)'4',
            (byte)'5', (byte)'6', (byte)'7', (byte)'8',
            (byte)'O', (byte)'T', (byte)'H', (byte)'E', (byte)'R'
        };

var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName", 
                                new List<IBuffer> {AID_OTHER.AsBuffer()},
                                SmartCardEmulationCategory.Other,
                                SmartCardEmulationType.Host);
```

Sie können bis zu neun AIDs (jeweils mit einer Länge von 5 bis 16 Byte) pro AID-Gruppe verwenden.

Verwenden Sie die [**RegisterAppletIdGroupAsync**](https://msdn.microsoft.com/library/windows/apps/Dn894656)-Methode, um Ihre AID-Gruppe im System zu registrieren. Es wird dann ein [**SmartCardAppletIdGroupRegistration**](https://msdn.microsoft.com/library/windows/apps/Dn910955registration)-Objekt zurückgegeben. Standardmäßig ist die [**ActivationPolicy**](https://msdn.microsoft.com/library/windows/apps/Dn910955registration_activationpolicy)-Eigenschaft des Registrierungsobjekts auf **Disabled** festgelegt. Dies bedeutet, dass Ihre AIDs im System zwar registriert sind, aber sie sind noch nicht aktiviert und empfangen auch keinen Datenverkehr.

```csharp
reg = await SmartCardEmulator.RegisterAppletIdGroupAsync(appletIdGroup);
```

Sie können Ihre registrierten Karten (AID-Gruppen) aktivieren, indem Sie die [**RequestActivationPolicyChangeAsync**](https://msdn.microsoft.com/library/windows/apps/Dn910955registration_requestactivationpolicychangeasync)-Methode der [**SmartCardAppletIdGroupRegistration**](https://msdn.microsoft.com/library/windows/apps/Dn910955registration)-Klasse wie unten gezeigt verwenden. Da im System jeweils nur eine Zahlungskarte aktiviert sein kann, entspricht das Festlegen von [**ActivationPolicy**](https://msdn.microsoft.com/library/windows/apps/Dn910955registration_activationpolicy) für eine AID-Zahlungsgruppe auf **Enabled** dem Festlegen als Standardkarte für die Zahlung. Benutzer werden aufgefordert, diese Karte unabhängig davon als Standardkarte für die Bezahlung zuzulassen, ob bereits eine Standardkarte für die Bezahlung ausgewählt ist. Diese Aussage trifft nicht zu, wenn Ihre App bereits die Standardanwendung für Zahlungen ist und lediglich ein Wechsel der eigenen AID-Gruppen durchgeführt wird. Sie können bis zu zehn AID-Gruppen pro App registrieren.

```csharp
reg.RequestActivationPolicyChangeAsync(AppletIdGroupActivationPolicy.Enabled);
```

Sie können die von Ihrer App beim Betriebssystem registrierten AID-Gruppen abfragen und deren Aktivierungsrichtlinie mit der [**GetAppletIdGroupRegistrationsAsync**](https://msdn.microsoft.com/library/windows/apps/Dn894654)-Methode überprüfen.

Benutzern wird nur dann eine Meldung angezeigt, wenn Sie die Aktivierungsrichtlinie einer Zahlungskarte von **Disabled** in **Enabled** ändern, sofern Ihre App nicht bereits die Standard-App für Zahlungen ist. Benutzern wird nur dann eine Meldung angezeigt, wenn Sie die Aktivierungsrichtlinie einer nicht für Zahlungen bestimmten Karte von **Disabled** in **Enabled** ändern, sofern ein AID-Konflikt vorliegt.

```csharp
var registrations = await SmartCardEmulator.GetAppletIdGroupRegistrationsAsync();
    foreach (var registration in registrations)
    {
registration.RequestActivationPolicyChangeAsync (AppletIdGroupActivationPolicy.Enabled);
    }
```

**Ereignisbenachrichtigung bei Änderung der Aktivierungsrichtlinie**

In Ihrer Hintergrundaufgabe können Sie den Empfang von Ereignissen für den Fall einrichten, dass sich die Aktivierungsrichtlinie für eine Ihrer AID-Gruppenregistrierungen außerhalb der App ändert. Es kann zum Beispiel sein, dass Benutzer die Standard-App für Zahlungen über das Menü mit den NFC-Einstellungen von einer Ihrer Karten in eine andere Karte ändern, die von einer anderen App gehostet wird. Falls Ihre App aus Gründen des internen Setups, z. B. der Aktualisierung von Live-Kacheln, über diese Änderung informiert sein muss, können Sie Ereignisbenachrichtigungen über diese Änderung erhalten und in Ihrer App entsprechende Maßnahmen ergreifen.

```csharp
var taskBuilder = new BackgroundTaskBuilder();
taskBuilder.Name = bgTaskName;
taskBuilder.TaskEntryPoint = taskEntryPoint;
taskBuilder.SetTrigger(new SmartCardTrigger(SmartCardTriggerType.EmulatorAppletIdGroupRegistrationChanged));
bgTask = taskBuilder.Register();
```

## Außerkraftsetzung des Vordergrunds

Sie können die [**ActivationPolicy**](https://msdn.microsoft.com/library/windows/apps/Dn910955registration_activationpolicy) für alle AID-Gruppenregistrierungen in **ForegroundOverride** ändern, während sich Ihre App im Vordergrund befindet, ohne Benutzern dies anzuzeigen. Wenn Benutzer mit ihrem Gerät ein Terminal berühren, während sich Ihre App im Vordergrund befindet, wird der Datenverkehr an Ihre App geleitet. Dies ist auch dann der Fall, wenn vom Benutzer keine Ihrer Zahlungskarten ausgewählt wurde. Wenn Sie die Aktivierungsrichtlinie einer Karte in **ForegroundOverride** ändern, ist diese Änderung nur vorübergehend, bis sich Ihre App nicht mehr im Vordergrund befindet. Die aktuelle Standardkarte für Zahlungen, die vom Benutzer festgelegt wurde, ändert sich hierdurch nicht. Sie können die **ActivationPolicy** Ihrer Karten für Zahlungen oder andere Zwecke wie folgt über die App im Vordergrund ändern. Beachten Sie, dass die [**RequestActivationPolicyChangeAsync**](https://msdn.microsoft.com/library/windows/apps/Dn910955registration_requestactivationpolicychangeasync)-Methode nur von einer App im Vordergrund aufgerufen werden kann, nicht von einer Hintergrundaufgabe.

```csharp
reg.RequestActivationPolicyChangeAsync(AppletIdGroupActivationPolicy.ForegroundOverride);
```

Außerdem können Sie eine AID-Gruppe registrieren, die aus einer einzelnen AID mit Nulllänge besteht. Das System leitet dann alle APDUs unabhängig von der AID und einschließlich aller Befehls-APDUs weiter, die vor dem Empfang eines SELECT AID-Befehls gesendet wurden. Eine AID-Gruppe dieser Art funktioniert aber nur, während sich Ihre App im Vordergrund befindet, da sie nur auf **ForegroundOverride** festgelegt und nicht dauerhaft aktiviert werden kann. Dieses Verfahren funktioniert außerdem sowohl für **Host**-Werte als auch für **UICC**-Werte der [**SmartCardEmulationType**](https://msdn.microsoft.com/library/windows/apps/Dn894639)-Enumeration, um den gesamten Datenverkehr entweder an Ihre HCE-Hintergrundaufgabe oder die SIM-Karte zu leiten.

```csharp
public static byte[] AID_Foreground =
        {};

var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName", 
                                new List<IBuffer> {AID_Foreground.AsBuffer()},
                                SmartCardEmulationCategory.Other,
                                SmartCardEmulationType.Host);
reg = await SmartCardEmulator.RegisterAppletIdGroupAsync(appletIdGroup);
reg.RequestActivationPolicyChangeAsync(AppletIdGroupActivationPolicy.ForegroundOverride);
```

## Überprüfen auf NFC- und HCE-Unterstützung

Ihre App sollte überprüfen, ob ein Gerät über NFC-Hardware verfügt und die Kartenemulationsfunktion und die Hostkartenemulation unterstützt, bevor diese Features dem Benutzer angeboten werden.

Die NFC-Smartcard-Emulationsfunktion wird nur unter Windows 10 Mobile aktiviert. Daher kommt es zu Fehlern, wenn versucht wird, Smartcard-Emulator-APIs in anderen Versionen von Windows 10 zu verwenden. Sie können die Smartcard-API-Unterstützung im folgenden Codeausschnitt überprüfen.

```csharp
Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.Devices.SmartCards.SmartCardEmulator");
```

Außerdem können Sie überprüfen, ob das Gerät über NFC-Hardware für eine Form der Kartenemulation verfügt, indem Sie ermitteln, ob die [**SmartCardEmulator.GetDefaultAsync**](https://msdn.microsoft.com/library/windows/apps/Dn608008)-Methode Null zurückgibt. Wenn ja, wird die NFC-Kartenemulation auf dem Gerät nicht unterstützt.

```csharp
var smartcardemulator = await SmartCardEmulator.GetDefaultAsync();<
```

Die Unterstützung für HCE- und AID-basiertes UICC-Routing ist nur auf neueren Geräten verfügbar, z. B. Lumia 730, 830, 640 und 640 XL. Alle neuen NFC-fähigen Geräte, auf denen Windows 10 Mobile oder höher ausgeführt wird, verfügen normalerweise über Unterstützung für HCE. Ihre App kann die HCE-Unterstützung wie folgt überprüfen.

```csharp
Smartcardemulator.IsHostCardEmulationSupported();
```

## Verhalten des Sperrbildschirms und bei „Bildschirm aus“

Windows 10 Mobile enthält Einstellungen für die Kartenemulation auf Geräteebene, die vom Mobilfunknetzbetreiber oder Hersteller des Geräts festgelegt werden können. Das Umschalten der Option „Zum Bezahlen berühren“ ist standardmäßig deaktiviert, und die „Aktivierungsrichtlinie auf Geräteebene“ ist auf „Immer“ festgelegt, es sei denn, der Netzbetreiber oder Hersteller überschreibt diese Werte.

Ihre Anwendung kann den Wert von [**EnablementPolicy**](https://msdn.microsoft.com/library/windows/apps/Dn608006) auf Geräteebene abfragen und abhängig vom gewünschten Verhalten Ihrer App in den einzelnen Zuständen jeweils Maßnahmen ergreifen.

```csharp
SmartCardEmulator emulator = await SmartCardEmulator.GetDefaultAsync();

switch (emulator.EnablementPolicy)
{
case Never:
// you can take the user to the NFC settings to turn "tap and pay" on
await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings-nfctransactions:"));
break;
 
 case Always: 
return "Card emulation always on";

 case ScreenOn:
 return "Card emulation on only when screen is on";

 case ScreenUnlocked:
 return "Card emulation on only when screen unlocked"; 
}
```

Die Hintergrundaufgabe der App wird nur dann gestartet, wenn das Smartphone gesperrt bzw. der Bildschirm ausgeschaltet ist, sofern die externe Leseeinheit eine AID auswählt, die für Ihre App aufgelöst wird. Sie können auf die Befehle der Leseeinheit in Ihrer Hintergrundaufgabe reagieren, aber wenn Sie Eingaben vom Benutzer benötigen oder dem Benutzer eine Meldung anzeigen möchten, können Sie die Vordergrund-App mit einigen Argumenten starten. Ihre Hintergrundaufgabe kann Ihre Vordergrund-App mit dem folgenden Verhalten starten:

-   Unterhalb des Sperrbildschirms eines Geräts (Benutzer sehen die Vordergrund-App erst nach dem Entsperren des Geräts)
-   Oberhalb des Sperrbildschirms (das Gerät befindet sich nach dem Schließen Ihrer App durch den Benutzer weiterhin im gesperrten Zustand)

```csharp
        if (Windows::Phone::System::SystemProtection::ScreenLocked)
        {
            // Launch above the lock with some arguments
            var result = await eventDetails.TryLaunchSelfAsync("app-specific arguments", SmartCardLaunchBehavior.AboveLock);
        } 
```

## AID-Registrierung und andere Updates für SIM-basierte Apps

Kartenemulations-Apps, bei denen die SIM-Karte als sicheres Element verwendet wird, können beim Windows-Dienst registriert werden, um die auf der SIM-Karte unterstützten AIDs zu deklarieren. Diese Registrierung ähnelt einer HCE-basierten App-Registrierung. Der einzige Unterschied ist der [**SmartCardEmulationType**](https://msdn.microsoft.com/library/windows/apps/Dn894639), der für SIM-basierte Apps auf „Uicc“ festgelegt werden sollte. Als Ergebnis der Zahlungskartenregistrierung wird der Anzeigename der Karte auch in das Menü mit den NFC-Einstellungen eingefügt.

```csharp
var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName", 
                                new List<IBuffer> {AID_PPSE.AsBuffer()},
                                SmartCardEmulationCategory.Payment,
                                SmartCardEmulationType.Uicc);
```

** Wichtig **  
Die Legacyunterstützung des binären SMS-Abfangverfahrens in Windows Phone 8.1 wurde entfernt und in Windows 10 Mobile durch eine neue, umfassendere SMS-Unterstützung ersetzt. Alle älteren Windows Phone 8.1-Apps, die hierauf basieren, müssen aktualisiert werden, damit sie die neuen Windows 10 Mobile-SMS-APIs nutzen.





<!--HONumber=May16_HO2-->


