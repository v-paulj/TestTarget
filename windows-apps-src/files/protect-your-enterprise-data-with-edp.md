---
author: TylerMSFT
Description: 'Dieses Thema enthält Beispiele für Programmieraufgaben, die in einigen der gängigsten, dateibezogenen EDP-Szenarien durchgeführt werden müssen.'
MS-HAID: 'dev\_files.protect\_your\_enterprise\_data\_with\_edp'
MSHAttr: 'PreferredLib:/library/windows/apps'
Search.Product: eADQiWindows 10XVcnh
title: 'Schützen von Dateien mithilfe des Unternehmensdatenschutzes (EDP)'
---

# Schützen von Dateien mithilfe des Unternehmensdatenschutzes (EDP)

__Hinweis__ EDP-Richtlinien (Enterprise Data Protection, Unternehmensdatenschutz) können nicht unter Windows 10 (Version 1511, Build 10586 oder älter) verwendet werden.

Dieses Thema enthält Beispiele für Programmieraufgaben, die in einigen der gängigsten, dateibezogenen EDP-Szenarien durchgeführt werden müssen. Umfassende Informationen zu den Zusammenhängen zwischen EDP und Dateien, Datenströmen, Zwischenablage, Netzwerk, Hintergrundaufgaben und dem Schutz von Daten im Sperrzustand finden Sie unter [Unternehmensdatenschutz (EDP)](../enterprise/edp-hub.md).

**Hinweis**  Das [Unternehmensdatenschutz-Beispiel (EDP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409) veranschaulicht viele der in diesem Thema beschriebenen Dateiszenarien.

## Voraussetzungen

-   **Vorbereitung für EDP**

    Weitere Informationen finden Sie unter [Einrichten des Computers für EDP](../enterprise/edp-hub.md#set-up-your-computer-for-EDP).

-   **Ausrichtung auf unternehmensoptimierte Apps**

    Eine App, die von sich aus dafür sorgt, dass Unternehmensdaten unter der Kontrolle des verwaltenden Unternehmens bleiben, wird als unternehmensoptimierte App bezeichnet. Eine optimierte App ist leistungsfähiger, intelligenter, flexibler und vertrauenswürdiger als eine nicht optimierte App. Durch Deklaration der eingeschränkten **enterpriseDataPolicy**-Funktion teilt die App dem System mit, dass sie optimiert ist. Zur Optimierung gehört jedoch mehr als nur das Festlegen einer Funktion. Weitere Informationen finden Sie unter [Unternehmensoptimierte Apps](../enterprise/edp-hub.md#enterprise-enlightened-apps).

-   **Kenntnisse in der asynchronen Programmierung für Apps für die universelle Windows-Plattform (UWP)**

    Informationen zum Schreiben von asynchronen Apps in C# oder Visual Basic finden Sie unter [Aufrufen asynchroner APIs in C\# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

## Lokaler Ordnerpfad und Anzeige geschützter Dateien im Datei-Explorer


Zu Testzwecken können Sie eine App erstellen, die die in diesem Thema behandelten Codebeispiele hostet. Durch die Codebeispiele werden Dateien in den lokalen Ordner der App geschrieben. Der genaue Pfad dieses Ordners auf dem Datenträger ist für die App eindeutig. Zur Ermittlung des Pfads des lokalen App-Ordners können Sie den folgenden Code hinzufügen.

```CSharp
// Put a breakpoint on the line after the line of code below. Use the value of localFolderPath
// in File Explorer to find the output file that the later code examples create.
string localFolderPath = ApplicationData.Current.LocalFolder.Path;
```

Nachdem Sie den Pfad kennen, ist es einfach, die von der App erstellten Dateien im Datei-Explorer zu finden. Dadurch können Sie sicherstellen, dass die Dateien geschützt sind und der Schutz der richtigen Identität zugeordnet ist.

Aktivieren Sie im Datei-Explorer unter **Ordner- und Suchoptionen ändern** auf der Registerkarte **Ansicht** die Option **Verschlüsselte Dateien in Farbe anzeigen**. Außerdem können Sie im Datei-Explorer den Befehl **Ansicht**&gt;**Spalten hinzufügen** verwenden, um die Spalte **Verschlüsselt für** hinzuzufügen. Dadurch wird die Unternehmensidentität angezeigt, für die die Dateien geschützt sind.

## Schützen von Unternehmensdaten in einer neuen Datei (für eine interaktive App)


Es gibt viele Wege, wie Unternehmensdaten in die App gelangen, z. B. über bestimmte Netzwerkendpunkte, Dateien, die Zwischenablage oder den Freigabe-Vertrag. Auch die App selbst kann neue Unternehmensdaten generieren. Wie auch immer die optimierte App Unternehmensdaten erhält, sie muss den Schutz der Daten für die verwaltete Unternehmensidentität gewährleisten, sobald die Daten in einer neuen Datei gespeichert werden.

Die grundlegenden Schritte sind: Erstellen der Datei mithilfe der regulären Speicher-API, Schützen der Datei für die jeweilige Unternehmensidentität mit einer EDI-API und Schreiben der Daten in die Datei (wieder mithilfe der regulären Speicher-APIs). Der Schutz der Datei muss vor dem Schreiben in die Datei sichergestellt sein (siehe Beispiel unten). Zum Schützen der Datei verwenden Sie die [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157)-Methode. Wie üblich, ist der Schutz für eine Identität nur sinnvoll, wenn die Identität verwaltet wird. Weitere Informationen zu den Gründen und dazu, wie die App die Identität des Unternehmens ermittelt, unter der sie ausgeführt wird, finden Sie unter [Sicherstellen, dass eine Identität verwaltet wird](../enterprise/edp-hub.md#confirming_an_identity_is_managed).

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void SaveEnterpriseDataToFile(string enterpriseData, string identity)
{
    // You should only protect to an identity that is managed.
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFile storageFile = storageFolder.CreateFileAsync("sample.txt",
        CreationCollisionOption.ReplaceExisting);

    // It's important to protect a file *before* writing enterprise data to it.
    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(storageFile, identity);

    // If the file is now protected, to the intended identity, then we can write to it.
    if (fileProtectionInfo.Identity == identity &&
        fileProtectionInfo.Status == FileProtectionStatus.Protected)
        await Windows.Storage.FileIO.WriteTextAsync(storageFile, enterpriseData);
}
```

## Schutz von Unternehmensdaten in einer neuer Datei (für eine Hintergrundaufgabe)


Die im vorangehenden Abschnitt verwendete [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157)-API eignet sich nur für interaktive Apps. Bei einer Hintergrundaufgabe kann Ihr Code auch ausgeführt werden, während der Sperrbildschirm aktiviert ist. Unter Umständen verwendet die Organisation auch eine sichere Richtlinie vom Typ DPL (Data Protection Under Lock, Schutz von Daten bei Sperre), durch die Entschlüsselungsschlüssel, die für den Zugriff auf geschützte Ressourcen erforderlich sind, bei der Sperrung des Geräts vorübergehend aus dem Arbeitsspeicher entfernt werden. Dies beugt Datenverlusten vor, wenn das Gerät verloren geht. Das Feature entfernt auch Schlüssel für geschützte Dateien, wenn deren Handles geschlossen werden. Es ist jedoch möglich, während des Sperrzeitfensters (Zeitraum zwischen dem Sperren und Entsperren des Geräts) neue geschützte Dateien zu erstellen, auf sie zuzugreifen und gleichzeitig das Dateihandle offen zu halten. **StorageFolder.CreateFileAsync** schließt das Handle nach dem Erstellen der Datei, sodass dieser Algorithmus nicht verwendet werden kann.

1.  Erstellen Sie eine neue Datei mithilfe von **StorageFolder.CreateFileAsync**.
2.  Verschlüsseln Sie sie mithilfe von **FileProtectionManager.ProtectAsync**.
3.  Öffnen Sie die Datei, und führen Sie einen Schreibvorgang aus.

Da das Dateihandle spätestens in Schritt 2 geschlossen wird, ist Schritt 3 nicht möglich, weil die für den Dateizugriff erforderlichen Verschlüsselungsschlüssel nicht mehr verfügbar sind.

Zur Vermeidung dieses Szenarios können Sie mithilfe von [**FileProtectionManager.CreateProtectedAndOpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn705153) eine geschützte Datei erstellen und ein **IRandomAccessStream**-Element an sie zurückgeben. Dadurch können Apps mehrere Schreibvorgänge in eine Datei durchführen und dabei das Dateihandle geöffnet lassen.

Der Beispielcode zeigt eine einfache E-Mail-App, die bei Eingang einer neuen E-Mail eine neue Datei schreibt. E-Mail-Apps müssen E-Mails synchronisieren, während das Gerät gesperrt ist. Wenn diese App eine neue E-Mail empfängt, erstellt sie mithilfe von [**CreateProtectedAndOpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn705153) eine neue Datei, ruft einen Ausgabedatenstrom ab und schreibt den E-Mail-Text in den Datenstrom.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;
using Windows.Storage.Streams;

...

private async void SaveEnterpriseDataToFile(string enterpriseData, string identity)
{
    // You should only protect to an identity that is managed.
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

    ProtectedFileCreateResult protectedFileCreateResult =
        await FileProtectionManager.CreateProtectedAndOpenAsync(storageFolder,
            "sample.txt", identity, CreationCollisionOption.ReplaceExisting);

    // It's important to successfully protect a file *before* writing enterprise data to it.
    if (protectedFileCreateResult.ProtectionInfo.Identity == identity &&
        protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        using (IOutputStream outputStream =
            protectedFileCreateResult.Stream.GetOutputStreamAt(0))
        {
            using (DataWriter writer = new DataWriter(outputStream))
            {
                writer.WriteString(enterpriseData);
                await writer.StoreAsync();
                await writer.FlushAsync();
            }
        }
    }
    else if (protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.AccessSuspended)
    {
        // Perform any special processing for the access suspended case.
    }
}
```

## Schützen von Ordnern für Unternehmensdaten


Sie können auch sicherstellen, dass jedes Element in einem Ordner geschützt ist. Mithilfe von [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) können Sie einen leeren Ordner schützen. Alle Dateien oder Ordner, die daraufhin in diesem Order erstellt werden, sind ebenfalls geschützt. Sie können einen vorhandenen oder neu erstellten Ordner schützen (im folgenden Beispiel wird ein neuer Ordner erstellt). Um zuverlässigen Schutz zu gewährleisten, muss der Ordner jedoch in beiden Fällen zunächst leer sein. Falls dies nicht der Fall ist, enthält [**FileProtectionInfo.Status**](https://msdn.microsoft.com/library/windows/apps/dn705150) einen [**FileProtectionStatus.NotProtectable**](https://msdn.microsoft.com/library/windows/apps/dn279147)-Wert.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void CreateANewFolderAndProtectItAsync(string folderName, string identity)
{
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFolder newStorageFolder =
        await storageFolder.CreateFolderAsync(folderName);

    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(newStorageFolder, identity);

    if (fileProtectionInfo.Identity != identity ||
        fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // Protection failed.
    }
}
```

## Kopieren des Schutzes zwischen Dateien


In diesem Szenario verfügen Sie bereits über eine Datei, von der Sie wissen, dass sie für die entsprechende Unternehmensidentität geschützt ist. Sie können diesen Schutz ausgesprochen einfach auf eine andere Datei replizieren. Dazu muss die Identität nicht einmal bekannt sein: Sie müssen nur sicherstellen, dass es sich um die richtige Identität handelt.

Durch Aufruf von [**StorageFile.CopyAsync**](https://msdn.microsoft.com/library/windows/apps/br227190) erstellen Sie eine einfache Kopie einer geschützten Datei. Die resultierende Kopie der Datei verfügt über den gleichen Schutz wie die Originaldatei.

Wenn Sie eine vorhandene nicht geschützte Datei schützen möchten, bevor Sie Unternehmensdaten in die Datei schreiben, können Sie statt [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) (früheres Szenario, bei der die Übergabe einer verwalteten Identität notwendig war) [**FileProtectionManager.CopyProtectionAsync**](https://msdn.microsoft.com/library/windows/apps/dn705152) aufrufen, wie im Codebeispiel dargestellt.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void CopyProtectionFromOneFileToAnother
    (StorageFile sourceStorageFile, StorageFile targetStorageFile)
{
    bool copyResult = await
        FileProtectionManager.CopyProtectionAsync(sourceStorageFile, targetStorageFile);

    if (!copyResult)
    {
        // Copying failed. To diagnose, you could check the file's status.
        // (call FileProtectionManager.GetProtectionInfoAsync and
        // check FileProtectionInfo.Status).
    }
}
```

## Behandeln von Problemen beim Zugriff auf geschützte Dateien


In diesem Szenario versucht die App, auf eine Datei zuzugreifen, die zuvor von der App geschützt wurde. Der Zugriff wird jedoch verweigert. Sie müssen den Status der Datei überprüfen, um das Problem zu ermitteln. In diesem Codebeispiel ruft die App die [**FileProtectionManager.GetProtectionInfoAsync**](https://msdn.microsoft.com/library/windows/apps/dn705154)-API auf, um den Status abzufragen und zu ermitteln, ob der Dateizugriff zum gegebenen Zeitpunkt aufgrund der Remoteverwaltung verweigert wurde.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async System.Threading.Tasks.Task<bool> IsFileProtectionStatusRevoked
    (StorageFile storageFile)
{
    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.GetProtectionInfoAsync(storageFile);

    if (fileProtectionInfo.Status == FileProtectionStatus.Revoked)
    {
        // Inform the user that their data has been revoked.
    }
    return fileProtectionInfo.Status == FileProtectionStatus.Revoked;
}
```

## Aktivieren der UI-Richtlinienerzwingung auf der Grundlage der geschützten Identität einer Datei


Wenn Ihre App im Begriff ist, den Inhalt einer geschützten Datei (etwa einer PDF-Datei) auf der Benutzeroberfläche anzuzeigen, muss sie die UI-Richtlinienerzwingung auf der Grundlage der geschützten Identität der Datei aktivieren. Fragen Sie die Schutzinformationen der Datei ab, und aktivieren Sie die UI-Richtlinienerzwingung auf der Grundlage der abgerufenen Identität.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void EnableUIPolicyFromFile(StorageFile storageFile)
{
    FileProtectionInfo fileProtectionInfo = 
        await FileProtectionManager.GetProtectionInfoAsync(storageFile);

    if (fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // No policy enforcement required, unless the file is inaccessible
        // (Revoked, ProtectedToOtherIdentity) in which case that should
        // be handled in an app-specific way.
        return;
    }

    ProtectionPolicyManager.TryApplyProcessUIPolicy(fileProtectionInfo.Identity);
}
```

**Hinweis**  Dieser Artikel ist für Windows 10-Entwickler gedacht, die Apps für die Universelle Windows-Plattform (UWP) schreiben. Wenn Sie für Windows 8.x oder Windows Phone 8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Verwandte Themen


[Unternehmensdatenschutz (EDP) – Beispiel](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Windows.Security.EnterpriseData-Namespace**](https://msdn.microsoft.com/library/windows/apps/dn279153)

 

 





<!--HONumber=May16_HO2-->


