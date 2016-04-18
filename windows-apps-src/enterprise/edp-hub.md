---
Description: 'Dies ist ein Übersichtsthema mit umfassenden Informationen zum Zusammenhang zwischen dem Unternehmensdatenschutz (Enterprise Data Protection, EDP) und Dateien, Puffern, Zwischenablage, Netzwerk, Hintergrundaufgaben und dem Schutz von Daten bei Sperre.'
MS-HAID: 'dev\_enterprise.edp\_hub'
MSHAttr: 'PreferredLib:/library/windows/apps'
Search.Product: eADQiWindows 10XVcnh
title: 'Unternehmensdatenschutz'
---

# Unternehmensdatenschutz

__Hinweis__ EDP-Richtlinien (Enterprise Data Protection, Unternehmensdatenschutz) können nicht unter Windows 10- Version 1511 (Build 10586) oder älter verwendet werden.

Dies ist ein Übersichtsthema mit umfassenden Informationen zum Zusammenhang zwischen dem Unternehmensdatenschutz (Enterprise Data Protection, EDP) und Dateien, Puffern, Zwischenablage, Netzwerk, Hintergrundaufgaben und dem Schutz von Daten bei Sperre.

Weitere Informationen zu EDP aus der Sicht von Endbenutzern und Administratoren finden Sie unter [Unternehmensdatenschutz (EDP) – Überblick](https://technet.microsoft.com/library/dn985838(v=vs.85).aspx).

## Was ist EDP?

Der Unternehmensdatenschutz (Enterprise Data Protection, EDP) ist ein Satz von Features auf Desktops, Laptops, Tablets und Smartphones für die mobile Geräteverwaltung (Mobile Device Management, MDM). EDP bietet Unternehmen eine bessere Kontrolle über die Behandlung von Daten (Unternehmensdateien und Datenblobs) auf Geräten, die das Unternehmen verwaltet.

-   Unternehmensdaten werden mit Verschlüsselung gekennzeichnet. Hierbei handelt es sich um „durch das Unternehmen geschützte Daten“ oder einfach „geschützte Daten“.
-   Nur Apps, die vom verwaltenden Unternehmen explizit über EDP-Gruppenrichtlinien zugelassen werden, können auf durch das Unternehmen geschützte Daten zugreifen, z. B. in Dateien.
-   Nur Apps, die über EDP-Gruppenrichtlinien explizit zugelassen werden, verfügen über VPN-Zugriff (virtuelles privates Netzwerk).
-   Die Richtlinien für die App-Einschränkung bestimmen auch, wie zulässige Apps Unternehmensdaten behandeln sollen.
-   Richtlinienbasierte Einschränkungen gelten sogar für Unternehmensinhalte, die über die Windows-Zwischenablage oder über den Freigabe-Vertrag ausgetauscht werden.
-   Bei Bedarf kann das verwaltende Unternehmen den Zugriff eines Geräts auf geschützte Inhalte widerrufen und so im Grunde alle Unternehmensdaten vom Gerät löschen, während private Daten nicht geändert werden.
-   Eine Kanal-App ist eine App, die geschützte Daten herunterlädt. Beispiele sind E-Mail-Apps und Apps für die Dateisynchronisierung.

EDP verbessert das [Verschlüsselnde Dateisystem (Encrypting File System, EFS)](http://technet.microsoft.com/library/cc700811.aspx) und das [Windows Selective Wipe](https://technet.microsoft.com/library/dn486874.aspx) (selektives Zurücksetzen von Windows), um weitere Optionen für Sicherheit und Flexibilität bereitzustellen. Mit neuen EDP-APIs können Sie Apps erstellen, die den Zugriff auf Unternehmensinhalte schützen und widerrufen, mit geschützten Dateieigenschaften arbeiten und auf verschlüsselte Daten in der unformatierten Form zugreifen. Zusätzlich zur Einführung von neuen APIs für den Schutz von Dateien und Ordnern werden auch APIs zum Schutz von Puffern und Datenströmen bereitgestellt. Außerdem wird eine Reihe von APIs hinzugefügt, mit denen das Unternehmen identifiziert und angegeben wird, das die Datenschutzrichtlinien erzwingen muss.

Damit das verwaltende Unternehmen den Zugriff auf die geschützten Daten steuern kann, definiert die App-Einschränkungsrichtlinie eine Liste von Apps und Einschränkungen für diese Apps. Standardmäßig kann eine App nicht selbst auf geschützte Daten zugreifen. Für den Zugriff muss die App einer Liste hinzugefügt werden, die als Liste zulässiger Apps bezeichnet wird. Die in dieser Liste enthaltenen Apps werden als zulässige Apps bezeichnet. Auf einem verwalteten Gerät kann Windows den Zugriff auf geschützte Daten in der Zwischenablage oder über den Freigabe-Vertrag einschränken und/oder überwachen, sodass der Zugriff durch eine nicht auf der Liste der zulässigen Apps aufgeführte App die Zustimmung des Benutzers erfordert. Andernfalls wird der Zugriff vollständig blockiert.

Die Richtlinien für EDP werden durch das verwaltende Unternehmen auf einem Gerät bereitgestellt (hierdurch wird das Gerät zu einem „verwalteten Gerät“). Die Bereitstellung von Richtlinien kann durch eine Registrierung durch den Benutzer in der mobilen Geräteverwaltung (Mobile Device Management (MDM), durch eine manuelle Konfiguration in der IT-Abteilung oder durch einen anderen Mechanismus für Verwaltung und Richtlinienbereitstellung erfolgen, z. B. System Center Configuration Manager (SCCM).

Der EDP-Dateischutz nutzt RMS-Schlüssel (Rights Management Service, Rechteverwaltungsdienst), wenn diese bereitgestellt werden, da diese Schlüssel auf verschiedenen Geräten bereitgestellt werden können, wodurch ein Roaming der geschützten Daten ermöglicht wird. Wenn keine RMS-Schlüssel vorhanden sind, greifen diese APIs auf lokale Schlüssel für die selektive Zurücksetzung zurück und schränken die Roaming-Funktionen ein. Auf Daten mit verschlüsseltem Roaming kann auf Geräten mit älteren Windows-Versionen und auf Drittanbietergeräten über plattformspezifische RMS-Apps zugegriffen werden, die von Microsoft bereitgestellt werden, sowie mit für RMS optimierten Apps von Drittanbietern.

Zusammengefasst können mit den EDP-APIs geschützte Daten durch das Unternehmen verwaltet werden, sodass Sie Ihre App so erstellen können, dass das Unternehmen seine Daten schützen und verwalten kann. Anders ausgedrückt können Sie eine App speziell für Unternehmen erstellen. Der Rest dieses Handbuchs soll Sie genau dabei unterstützen.

## Einrichten des Computers für EDP


Damit Sie Ihre App richtig entwickeln und ihr Verhalten im Unternehmen testen können, sollten Sie Ihren Computer oder Ihr Gerät mit den richtigen Bedingungen einrichten. Hierzu gehören einige Aufgaben, die normalerweise in das Tätigkeitsgebiet von IT-Administratoren fallen.

-   Registrieren Sie Ihren Entwicklungscomputer in der mobilen Geräteverwaltung (Mobile Device Management, MDM).
-   Fügen Sie Ihre App über den [AppLocker-Konfigurationsdienstanbieter](https://msdn.microsoft.com/library/windows/hardware/dn920019) zur Liste der zulässigen Apps hinzu.

Die nächste Aufgabe besteht darin, eine App zu erstellen, die die verwaltete Identität des Unternehmens, in dem sie ausgeführt wird, sowie die geltenden Schutzrichtlinien erkennt und dynamisch darauf reagieren kann. Das bedeutet, dass Ihre App „optimiert“ wird. Bei Apps, die für das Einhalten von Richtlinien optimiert sind, ist die Wahrscheinlichkeit, dass sie in der Liste der zum Zugriff auf Unternehmensdaten berechtigten Apps enthalten sind, deutlich höher.

## Für Unternehmen optimierte Apps


Nachdem Ihre App der Liste der zulässigen Apps hinzugefügt wurde, kann sie geschützte Daten lesen. Außerdem werden standardmäßig alle Datenausgaben Ihrer App automatisch durch das System geschützt. Dieser automatische Schutz ist darauf zurückzuführen, dass das verwaltende Unternehmen auf irgendeine Weise sicherstellen muss, dass die Unternehmensdaten immer unter seiner Kontrolle bleiben. Die App so stark einzuschränken, ist aber nur das Standardverfahren hierfür. Eine bessere Möglichkeit besteht darin, zu erreichen, dass das System Ihnen ausreichend vertraut und mehr Möglichkeiten und Flexibilität bereitstellt. Hierfür müssen Sie Ihre App intelligenter machen. Dies bedeutet, einen Schritt weiter zu gehen, als die App in die Liste der zulässigen Apps aufzunehmen. Die App muss für Unternehmen optimiert und dementsprechend deklariert werden.

Ihre App ist optimiert, wenn sie mithilfe der im Folgenden beschriebenen Verfahren selbst für den Schutz von Unternehmensdaten sorgt, unabhängig davon, ob die Daten nur gespeichert, verwendet oder übertragen werden. Die optimierte App erkennt Datenquellen im Unternehmen sowie Unternehmensdaten und schützt diese, wenn sie in Ihrer App eingehen. Die Optimierung bedeutet auch, dass EDP-Richtlinien erkannt und eingehalten werden, wenn Unternehmensdaten die App verlassen. Hierzu gehört es, zu verhindern, dass Inhalte an einen Endpunkt außerhalb des Unternehmens gesendet werden, die Daten vor dem Zulassen des Roamings in einem portablen verschlüsselten Format zu umschließen und ggf. (abhängig von Richtlinieneinstellungen) den Benutzer zur Zustimmung aufzufordern, bevor Unternehmensdaten in einer App eingefügt werden, die nicht in der Liste der zulässigen Apps enthalten ist. Nachdem Sie Ihre App optimiert haben, teilt sie dies dem System mit, indem die eingeschränkte **enterpriseDataPolicy**-Funktion deklariert wird. Weitere Informationen zum Arbeiten mit eingeschränkten Funktionen finden Sie unter [Spezielle und eingeschränkte Funktionen](https://msdn.microsoft.com/library/windows/apps/mt270968#special_and_restricted_capabilities).

Im Idealfall sind alle Unternehmensdaten geschützte Daten, sowohl im gespeicherten Zustand als auch während der Übertragung. Zwangsläufig muss es allerdings einen kurzen Zeitraum zwischen dem ersten Erstellen der Unternehmensdaten und ihrem Schutz geben. Und in einigen Fällen können Unternehmensdaten auf dem Endpunkt eines Unternehmensnetzwerks vorhanden sein, ohne verschlüsselt zu werden. Eine optimierte App kann solche Daten selbst schützen. Für zulässige Apps, die jedoch nicht optimiert wurden, muss der Schutz durch das System erfolgen.

Dies liegt daran, dass eine nicht optimierte App immer im Unternehmensmodus ausgeführt wird. Dies wird vom System sichergestellt. Eine optimierte App kann jedoch frei zwischen dem Unternehmensmodus und dem privaten Modus wechseln, abhängig von den Daten, mit denen die App jeweils arbeitet. Es ist für eine optimierte App auch wichtig, private Daten zu erkennen und sie nicht als Unternehmensdaten zu kennzeichnen. Eine optimierte App kann gleichzeitig sowohl Unternehmensdaten als auch private Daten verarbeiten, solange diese Voraussetzungen erfüllt werden. Im nächsten Abschnitt wird veranschaulicht, wie die Modi im Code gewechselt werden.

## Bestätigen der Identität als verwaltet und Bestimmen der Erzwingungsstufe für die Schutzrichtlinie

Ihre App ruft eine Unternehmensidentität in der Regel aus einer externen Ressource ab, z. B. aus einer E-Mail-Postfachadresse, einer verwalteten Domäne oder einem URI-Hostnamen. Sie können [**ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync**](https://msdn.microsoft.com/library/windows/apps/dn706027) aufrufen, um die verwaltete Identität des Hostnamens eines Netzwerkendpunkts abzurufen, falls vorhanden.

Dieser Code zeigt die allgemeine Struktur für optimiertes Verhalten, u. a. das Ermitteln, ob eine Unternehmensidentität tatsächlich verwaltet ist, und welche Richtlinien derzeit in Kraft sind.

```CSharp
using Windows.Security.EnterpriseData;

...

string identity = "contoso.com";

if (ProtectionPolicyManager.IsIdentityManaged(identity))
{
    EnforcementLevel enforcementLevel = ProtectionPolicyManager.GetEnforcementLevel();

    // During UI activities or network access, call ProtectionPolicyManager APIs
    // (taking the enforcement level into account) to ensure that the system
    // tags data with the identity as appropriate.

    ProtectionPolicyManager.GetForCurrentView().Identity = identity;
    // The app is now in enterprise mode.

    ProtectionPolicyManager.GetForCurrentView().Identity = string.Empty;
    // The app is back in personal mode.
}
else
{
    // No policy enforcement is done on this identity.
}
```

Wie gezeigt, bestimmen Sie zunächst, ob eine EDP-Richtlinie für die Identität des Unternehmens festgelegt wurde. Der Begriff „verwaltet“ ist die Kurzform von „durch eine EDP-Richtlinie verwaltet“. Wenn für eine bestimmte Identität eine EDP-Richtlinie festgelegt wurde, gibt [**ProtectionPolicyManager.IsIdentityManaged**](https://msdn.microsoft.com/library/windows/apps/dn705171) für diese Identität „true“ zurück. Dies ist für Sie der Hinweis darauf, EDP-APIs mit dieser Identität zu verwenden. Auch wenn die Datei- und Puffer-APIs in der Hinsicht außergewöhnlich sind, dass sie auch bei einer nicht verwalteten Identität erwartungsgemäß funktionieren, ist dieses Szenario nicht sinnvoll. Wenn ein Gerät verwaltet ist, wird es für eine Unternehmensidentität verwaltet. Wenn eine Identität nicht verwaltet ist, ist das Schützen von Daten für diese Identität ohne Bedeutung.

Im nächsten Schritt müssen Sie die Erzwingungsstufe für die Richtlinie bestimmen und implementieren. Rufen Sie zum Bestimmen der Erzwingungsstufe für die Richtlinie die [**GetEnforcementLevel**](https://msdn.microsoft.com/library/windows/apps/mt608406)-Methode auf. Wenn eine Unternehmensrichtlinie für die Identität erzwungen wird, muss die optimierte App das System beim Erzwingen der Richtlinie unterstützen, indem [**ProtectionPolicyManager**](https://msdn.microsoft.com/library/windows/apps/dn705170)-APIs während UI-Aktivitäten oder Netzwerkzugriffen aufgerufen werden, um sicherzustellen, dass das System Datenübertragungen ggf. mit dieser Identität kennzeichnet. Weitere Informationen dazu, wie Sie die Erzwingungsstufe interpretieren und umsetzen, finden Sie in diesem Handbuch. Im Codebeispiel wird auch gezeigt, wie Sie in den Unternehmensmodus wechseln und zum privaten Modus zurückkehren, indem Sie den Wert von [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) auf die Unternehmensidentität bzw. die leere Zeichenfolge festlegen. Beachten Sie auch hier, dass das Wechseln in den und aus dem Unternehmensmodus nur bei einer verwalteten Identität sinnvoll ist.

## EDP-Features auf einen Blick


**Schutz von Dateien und Puffern.**

-   Ihre App kann einer Unternehmensidentität zugeordnete Daten schützen, zurücksetzen und in einem Container zusammenfassen.
-   Die Verwaltung der Schlüssel wird von Windows behandelt. Windows verwendet die RMS-Schlüssel des Unternehmens, wenn diese auf dem Gerät verfügbar sind. Andernfalls greift Windows auf lokalen Schutz durch selektive Zurücksetzung zurück.

**Geräterichtlinienverwaltung.**

-   Ihre App kann die Identität (Unternehmen oder Organisation) abfragen, die das Gerät verwaltet.
-   Ihre App kann Benutzer vor einer unbeabsichtigten Offenlegung von Daten schützen, indem den betreffenden Daten eine Identität zugeordnet wird.
-   Ihre App kann Unternehmensressourcen über das Netzwerk schützen, indem nach Netzwerkendpunktverbindungen (Server, IP-Bereiche) des Unternehmens gesucht wird und die Daten einer verwalteten (d. h. in MDM registrierten) Identität zugeordnet werden.
-   Die EDP-APIs funktionieren nur mit verwalteten Identitäten, für die eine EDP-Richtlinie auf dem Gerät definiert wurde. Wenn eine Identität nicht verwaltet ist, kommunizieren die APIs dies ggf. an die Anwendung.

Im Folgenden finden Sie eine Liste mit Links zu Themen, in denen EDP-APIs und spezielle Szenarien zu diesen Funktionsbereichen genauer betrachtet werden.

## Dateien

Siehe [Schützen von Dateien mithilfe von EDP](../files/protect-your-enterprise-data-with-edp.md)

## Datenströme und Puffer

Siehe [Schützen von Datenströmen und Puffern mithilfe von EDP](../files/use-edp-to-protect-streams-and-buffers.md)

## Zwischenablage, Freigabe und Austauschen von Daten zwischen Apps

Siehe [Schützen von zwischen Apps übertragenen Unternehmensdaten mithilfe von EDP](../app-to-app/use-edp-to-protect-enterprise-data-transferred-between-apps.md)

## Netzwerke

Siehe [Markieren von Netzwerkverbindungen mit EDP-Identität](../networking/tagging_network_connections_with_edp_identity.md)

## Schutz von Daten bei Sperre (Data Protection Under Lock, DPL) und Hintergrundaufgaben

Unter Umständen verwendet die Organisation eine sichere Richtlinie vom Typ DPL (Data Protection Under Lock, Schutz von Daten bei Sperre), durch die Entschlüsselungsschlüssel, die für den Zugriff auf geschützte Ressourcen erforderlich sind, bei der Sperrung des Geräts vorübergehend aus dem Arbeitsspeicher entfernt werden. Informationen zum Vorbereiten Ihrer App auf diesen Fall finden Sie in diesem Artikel im Abschnitt [Behandeln von Gerätesperrereignissen und Vermeiden von nicht geschützten Inhalten im Arbeitsspeicher](#handle_lock_events). Wenn Ihre App über eine Hintergrundaufgabe verfügt, die Dateien schützen muss, finden Sie weitere Informationen unter [Schutz von Unternehmensdaten in einer neuen Datei (für eine Hintergrundaufgabe)](../files/protect-your-enterprise-data-with-edp.md#protect_data_new_file_bg).

## Erzwingen von UI-Richtlinien


In diesem Abschnitt wird das Beispiel einer optimierten E-Mail-App betrachtet, in der derzeit ein Unternehmenspostfach neben einer Reihe von anderen Postfächern angezeigt wird, zu denen sowohl Unternehmenspostfächer als auch private Postfächer des Benutzers gehören. Um sicherzustellen, dass keine Datenlecks von den Unternehmensdaten im Unternehmenspostfach auftreten, ruft die App [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791) auf, um zu gewährleisten, dass das Betriebssystem den aktuellen Kontext der App kennt (Unternehmen oder privat). Die API gibt „false“ zurück, wenn die Identität nicht durch eine Unternehmensrichtlinie verwaltet wird.

```CSharp
using Windows.Security.EnterpriseData;

...

public class Mailbox
{
    public bool HasEnterpriseMail { get { /* implementation */ } }
    public string Identity { get { /* implementation */ } }
}

private void SwitchMailbox(Mailbox targetMailbox)
{
    // Code goes here to perform setup for "targetMailbox".

    if (targetMailbox.HasEnterpriseMail)
    {
        bool result = 
            ProtectionPolicyManager.TryApplyProcessUIPolicy(targetMailbox.Identity);

        // Code goes here to process "result", which indicates whether
        // or not policy enforcement is in place for the identity.
    }
    else
    {
        // For personal mailboxes, we clear policy enforcement (in case
        // it is still set from when we processed an enterprise mailbox).
        ProtectionPolicyManager.ClearProcessUIPolicy();
    }
}
```

## Behandeln von Gerätesperrereignissen und Vermeiden von nicht geschützten Inhalten im Arbeitsspeicher


In diesem Szenario wird das Beispiel einer optimierten E-Mail-App betrachtet, die für die Verwaltung von Unternehmens- und privaten E-Mails konzipiert wurde. Wenn die App in einer Organisation ausgeführt wird, in der eine DPL-Richtlinie (Data Protection Under Lock, Schutz von Daten bei Sperre) verwaltet wird, muss die App alle vertraulichen Daten aus dem Arbeitsspeicher entfernen, während das Gerät gesperrt ist. Hierzu registriert sie sich für das [**ProtectionPolicyManager.ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/dn705787)-Ereignis und das [**ProtectionPolicyManager.ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/dn705786)-Ereignis, damit eine Benachrichtigung erfolgt, wenn das Gerät gesperrt und entsperrt wird (für den Fall, dass DPL aktiv ist).

[**ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/dn705787) wird ausgelöst, bevor auf dem Gerät bereitgestellte Datenschutzschlüssel vorübergehend entfernt werden. Diese Schlüssel werden entfernt, wenn das Gerät gesperrt ist, um sicherzustellen, dass kein nicht autorisierter Zugriff auf verschlüsselte Daten erfolgen kann, während das Gerät gesperrt ist und möglicherweise nicht von seinem Besitzer verwendet wird. [**ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/dn705786) wird ausgelöst, wenn die Schlüssel nach dem Entsperren des Geräts wieder verfügbar sind.

Durch das Behandeln dieser beiden Ereignisse kann die App sicherstellen, dass vertrauliche Inhalte im Arbeitsspeicher mit [**DataProtectionManager**](https://msdn.microsoft.com/library/windows/apps/dn706017) geschützt werden. Außerdem sollten alle für die geschützten Dateien geöffneten Dateidatenströme geschlossen werden, um sicherzustellen, dass das System keine sensiblen Daten im Arbeitsspeicher zwischenspeichert. Hierzu stehen folgende Möglichkeiten zur Verfügung. Um einen von einer Open-Methode einer **StorageFile** zurückgegebenen Dateidatenstrom zu schließen, rufen Sie die **Dispose**-Methode für den Datenstrom auf. Sie können die Verwendung des Datenstroms mit einer using-Anweisung (C\# oder VB) beschränken. Alternativ können Sie den Datenstrom mit einem **DataReader**-Objekt oder einem **DataWriter**-Objekt umschließen und die **Dispose**-Methode oder using-Anweisung mit diesem Objekt verwenden.

**Hinweis**  
In einer Umgebung ohne konfigurierte DPL-Richtlinie wird das [**ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/dn705786)-Ereignis ausgelöst, nicht jedoch das [**ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/dn705787)-Ereignis. Beachten Sie dies in Ihrem Code, und achten Sie darauf, nicht davon auszugehen, dass die Ereignisse in jedem System immer paarweise auftreten oder dass Sie diese Ereignisse immer verwenden können, um den gesperrten/entsperrten Zustand des Geräts zu bestimmen. Sie können in diesem Codebeispiel sehen, dass darauf geachtet wird, keine Annahmen über den Schutzzustand der momentan angezeigten E-Mail oder über den Öffnungszustand des Datenbankdatei-Datenstroms zu treffen.

Achten Sie außerdem darauf, dass bei der Fortsetzung nach Sperre auf einem Gerät ohne konfigurierte DPL-Richtlinie [**ProtectedAccessResumedEventArgs.Identities**](https://msdn.microsoft.com/library/windows/apps/dn705772) eine leere Auflistung ist.

Aus Platzgründen wurde dieses Codebeispiel etwas vereinfacht, und der Schwerpunkt liegt auf dem Verarbeiten von Unternehmens-E-Mails. In einer tatsächlichen App würden private E-Mails in eine andere, nicht geschützte E-Mail-Datenbankdatei geschrieben, und Sie würden private E-Mails bei Sperre nicht schützen.

```CSharp
using Windows.Security.Cryptography;
using Windows.Security.EnterpriseData;
using Windows.Storage;
using Windows.Storage.Streams;

...

public class DisplayedMail
{
    public string Body { get; set; }
    public IBuffer ProtectedBody { get; set; }
    public bool IsProtected { get; set; }
}

private IOutputStream mailDatabaseStream = null;
private string currentlyDisplayedMailIdentity = "contoso.com";
private DisplayedMail currentlyDisplayedMail = new DisplayedMail()
    { Body = "Lorem ipsum dolor...", ProtectedBody = null, IsProtected = false };

// Gets the app's protected mail database file, then opens and stores a stream on it.
private async void OpenMailDatabase()
{
    // Only attempt to open the database file stream if we know it&#39;s closed.
    if (this.mailDatabaseStream == null)
    {
        StorageFolder appDataStorageFolder = ApplicationData.Current.LocalFolder;
        StorageFile storageFile = await appDataStorageFolder.GetFileAsync("maildb.dat");
        using (IRandomAccessStream randomAccessStream =
            await storageFile.OpenAsync(FileAccessMode.ReadWrite))
        {
            this.mailDatabaseStream = randomAccessStream.GetOutputStreamAt(0);
        }
    }
}

// Called once by the app when starting up.
private void AppSetup()
{
    ProtectionPolicyManager.ProtectedAccessSuspending +=
        this.ProtectionPolicyManager_ProtectedAccessSuspending;
    ProtectionPolicyManager.ProtectedAccessResumed +=
        this.ProtectionPolicyManager_ProtectedAccessResumed;
    this.OpenMailDatabase();
}

// Background work called when the app receives an email.
private async void AppMailReceived(string fauxEmail)
{
    // Only attempt to write to the database file stream if we know it&#39;s open.
    if (this.mailDatabaseStream != null)
    {
        IBuffer emailAsBuffer = CryptographicBuffer.ConvertStringToBinary
            (fauxEmail, BinaryStringEncoding.Utf8);
        await this.mailDatabaseStream.WriteAsync(emailAsBuffer);
        await this.mailDatabaseStream.FlushAsync();
    }
    else
    {
        // Code goes here to handle the case where the device is
        // locked and we can't access the protected mail database.
    }
}

// Called by ProtectionPolicyManager when the device is locked if under DPL.
private async void ProtectionPolicyManager_ProtectedAccessSuspending
    (object sender, ProtectedAccessSuspendingEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.currentlyDisplayedMailIdentity))
    {
        // This event is not for our identity. Another will be sent for our identity.
        return;
    }

    // Get suspension deferral.
    Windows.Foundation.Deferral deferral = e.GetDeferral();

    // Protect the displayed mail content.
    if (!this.currentlyDisplayedMail.IsProtected)
    {
        IBuffer mailBodyBuffer = CryptographicBuffer.ConvertStringToBinary
            (this.currentlyDisplayedMail.Body, BinaryStringEncoding.Utf8);
        BufferProtectUnprotectResult result = await DataProtectionManager.ProtectAsync
            (mailBodyBuffer, this.currentlyDisplayedMailIdentity);
        if (result.ProtectionInfo.Status == DataProtectionStatus.Protected)
        {
            // Save the protected version and clear the unprotected version.
            this.currentlyDisplayedMail.ProtectedBody = result.Buffer;
            this.currentlyDisplayedMail.Body = null;
        }
    }

    // Close the mail database file stream to make sure that we have no unprotected
    // content in memory.
    this.mailDatabaseStream.Dispose();
    this.mailDatabaseStream = null;

    // Optionally, code goes here to use e.Deadline to determine whether we have more
    // than 15 seconds left before the suspension deadline. If we do then process any
    // messages queued up for sending while we are still able to access them.

    // All done. Complete deferral.
    deferral.Complete();
}

// Called by ProtectionPolicyManager when the device is unlocked.
private async void ProtectionPolicyManager_ProtectedAccessResumed
    (object sender, ProtectedAccessResumedEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.currentlyDisplayedMailIdentity))
    {
        // This event is not for our identity. Another will be sent for our identity.
        return;
    }

    // Unprotect the displayed mail content.
    if (this.currentlyDisplayedMail.IsProtected)
    {
        BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync
            (this.currentlyDisplayedMail.ProtectedBody);
        if (result.ProtectionInfo.Status == DataProtectionStatus.Unprotected)
        {
            // Restore the unprotected version.
            this.currentlyDisplayedMail.Body = CryptographicBuffer.ConvertBinaryToString
                (BinaryStringEncoding.Utf8, result.Buffer);
            this.currentlyDisplayedMail.ProtectedBody = null;
        }
    }

    // Reopen the closed mail database.
    this.OpenMailDatabase();
}
```

## Registrieren von Benachrichtigungen beim Widerrufen von geschützten Inhalten


Betrachten Sie ein Szenario, in dem eine E-Mail-App ein Unternehmenspostfach auf dem Gerät eines Benutzers eingerichtet hat. Irgendwann möchte das Unternehmen aus einem von mehreren möglichen Gründen den Zugriff auf die durch das Unternehmen geschützten E-Mails und anderen Ressourcen auf dem Gerät widerrufen. Es gibt mehrere Gründe für einen Widerruf. Es ist wahrscheinlich, dass die jeweilige Unternehmensrichtlinie einen Widerruf oder eine Aufhebung der Registrierung auslöst. Ein mögliches Szenario besteht also darin, dass der Benutzer die Registrierung seines Geräts im Unternehmen aufgehoben hat (z. B. weil er das Gerät verschenkt oder verkauft oder ein anderes Gerät verwenden möchte, weil er den Arbeitsplatz wechselt oder in den Ruhestand geht). Ein weiteres Szenario besteht darin, dass eine Benachrichtigung über die Aufhebung der Registrierung remote von einem IT-Administrator über die mobile Geräteverwaltung (Mobile Device Management, MDM) gesendet wird, z. B. wenn das Gerät als verloren gemeldet wird.

Unabhängig von den Einzelheiten des Falls sendet das Unternehmen eine Anforderung, um alle E-Mails vom Gerät des Benutzers zu löschen, da sie dort nicht mehr benötigt werden. Ein Remoteverwaltungsclient auf einem Gerät empfängt die Anforderung vom Remoteverwaltungsserver des Unternehmens und ruft [**ProtectionPolicyManager.RevokeContent**](https://msdn.microsoft.com/library/windows/apps/dn705790) auf, um die Schlüssel zu widerrufen, die für den Zugriff auf Inhalte benötigt werden, die auf diesem Gerät für diese Unternehmensidentität geschützt sind.

Wenn Ihre App über einen Widerruf benachrichtigt werden muss, können Sie sie beim [**ProtectionPolicyManager.ProtectedContentRevoked**](https://msdn.microsoft.com/library/windows/apps/dn705788)-Ereignis registrieren. Wenn Ihre App das Ereignis empfängt, kann sie alle dem Unternehmenspostfach zugeordneten Metadaten löschen, die jetzt nicht mehr benötigt werden.

```CSharp
using Windows.Security.EnterpriseData;

...

private string mailIdentity = "contoso.com";

void MailAppSetup()
{
    ProtectionPolicyManager.ProtectedContentRevoked += ProtectionPolicyManager_ProtectedContentRevoked;
    // Code goes here to set up mailbox for &#39;mailIdentity&#39;.
}

private void ProtectionPolicyManager_ProtectedContentRevoked(object sender, ProtectedContentRevokedEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.mailIdentity))
    {
        // This event is not for our identity.
        return;
    }

    // Code goes here to delete any metadata associated with &#39;mailIdentity&#39;.
}
```

## Remoteverwaltungsclient initiiert das Zurücksetzen von durch das Unternehmen geschützten Daten.


Ein Benutzer hat mehrere Unternehmensdateien auf seinem persönlichen Gerät gespeichert, die durch die Unternehmensidentität geschützt werden. Der Benutzer verliert das Gerät. Wenn das Unternehmen darüber benachrichtigt wird, dass der Benutzer das Gerät verloren hat, sendet das Unternehmen eine Anforderung, um alle vertraulichen Daten vom Gerät des Benutzers zu löschen und so ein Datenleck zu verhindern. Der Remoteverwaltungsclient auf dem Gerät empfängt diese Anforderung vom Remoteverwaltungsserver des Unternehmens und ruft [**ProtectionPolicyManager.RevokeContent**](https://msdn.microsoft.com/library/windows/apps/dn705790) auf, um die Schlüssel zu widerrufen, die für den Zugriff auf Inhalte benötigt werden, die auf diesem Gerät für die Unternehmensidentität geschützt sind.

```CSharp
Windows.Security.EnterpriseData.ProtectionPolicyManager.RevokeContent("contoso.com");
```

 

 





<!--HONumber=Mar16_HO5-->


