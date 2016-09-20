---
author: TylerMSFT
Description: "Dieses Thema enthält Beispiele für Programmieraufgaben, die in einigen der gängigsten EDP-Szenarien im Zusammenhang mit Datenströmen und Puffern durchgeführt werden müssen."
MS-HAID: dev\_files.use\_edp\_to\_protect\_streams\_and\_buffers
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "Schützen von Datenströmen und Puffern mithilfe des Unternehmensdatenschutzes (Enterprise Data Protection, EDP)"
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: fdde4f7d2ab46b6349273f7c1c9d91cf27aa341a

---

# Schützen von Datenströmen und Puffern mithilfe des Unternehmensdatenschutzes (Enterprise Data Protection, EDP)


            __Hinweis__ EDP-Richtlinien (Enterprise Data Protection, Unternehmensdatenschutz) können nicht unter Windows 10 (Version 1511, Build 10586 oder älter) verwendet werden.

Dieses Thema enthält Beispiele für Programmieraufgaben, die in einigen der gängigsten EDP-Szenarien im Zusammenhang mit Datenströmen und Puffern durchgeführt werden müssen. Umfassende Informationen zu den Zusammenhängen zwischen EDP und Dateien, Datenströmen, Zwischenablage, Netzwerk, Hintergrundaufgaben und dem Schutz von Daten im Sperrzustand finden Sie unter [Unternehmensdatenschutz (EDP)](../enterprise/edp-hub.md).


            **Hinweis**  Das [Unternehmensdatenschutz-Beispiel (EDP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409) veranschaulicht viele der in diesem Thema beschriebenen Dateiszenarien.

## Voraussetzungen


-   **Vorbereitung für EDP**

    Weitere Informationen finden Sie unter [Einrichten des Computers für EDP](../enterprise/edp-hub.md#set-up-your-computer-for-EDP).

-   **Ausrichtung auf unternehmensoptimierte Apps**

    Eine App, die von sich aus dafür sorgt, dass Unternehmensdaten unter der Kontrolle des verwaltenden Unternehmens bleiben, wird als unternehmensoptimierte App bezeichnet. Eine optimierte App ist leistungsfähiger, intelligenter, flexibler und vertrauenswürdiger als eine nicht optimierte App. Durch Deklaration der eingeschränkten **enterpriseDataPolicy**-Funktion teilt die App dem System mit, dass sie optimiert ist. Zur Optimierung gehört jedoch mehr als nur das Festlegen einer Funktion. Weitere Informationen finden Sie unter [Unternehmensoptimierte Apps](../enterprise/edp-hub.md#enterprise-enlightened-apps).

-   **Kenntnisse in der asynchronen Programmierung für Apps für die universelle Windows-Plattform (UWP)**

    Informationen zum Schreiben von asynchronen Apps in C# oder Visual Basic finden Sie unter [Aufrufen asynchroner APIs in C\# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

## Schützen eines Datenstroms für eine Unternehmensidentität



            **Hinweis**  Wenn Sie einen Datenstrom oder Puffer schützen, sollten Sie unbedingt das [**ProtectionPolicyManager.PolicyChanged**](https://msdn.microsoft.com/library/windows/apps/mt608411)-Ereignis abonnieren, damit Ihre App Bescheid weiß, wenn EDP auf dem Gerät deaktiviert wird. In diesem Fall muss der Schutz sämtlicher Datenströme und Puffer aufgehoben werden. Alle weiterhin geschützten Datenströme und Puffer können potenziell widerrufen werden, wenn der Benutzer die Registrierung des Geräts in der mobilen Geräteverwaltung (Mobile Device Management, MDM) aufhebt. Und wenn EDP bei der Ressourcenerstellung deaktiviert wurde, ist dieser Widerruf nicht zulässig. Dies lässt sich vermeiden, wenn bei der Deaktivierung von EDP der Schutz der Ressourcen aufgehoben wird.



In diesem Szenario hat Ihre App Zugriff auf einen ungeschützten Datenstrom mit Unternehmensdaten. Um den Schutz dieses Datenstroms bei der Übertragung innerhalb und außerhalb des Geräts zu gewährleisten, schützt Ihre App die Daten mithilfe der Unternehmensidentität, zu der sie gehören. Dadurch können die Daten bei Bedarf durch das Unternehmen gelöscht werden. Um später zu ermitteln, ob für einen Datenstrom eine Methode zum Aufheben des Schutzes aufgerufen werden soll, muss die App einen Zustand besitzen, der angibt, ob der Datenstrom geschützt war. Daher gibt die Funktion in diesem Codebeispiel diesen Zustand zurück. Wenn die übergebene Identität nicht verwaltet wird oder die App für die Identität nicht zulässig ist, wird der Datenstrom nicht geschützt, und beim Aufruf von [**DataProtectionManager.ProtectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706021) wird der Status „Ungeschützt“ zurückgegeben.

```CSharp
using Windows.Storage.Streams;
using Windows.Security.EnterpriseData;

private async System.Threading.Tasks.Task<bool> ProtectAStream
    (InMemoryRandomAccessStream unprotectedInMemoryRandomAccessStream, string identity,
    InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    IInputStream unprotectedStream = unprotectedInMemoryRandomAccessStream.GetInputStreamAt(0);
    IOutputStream protectedStream = protectedInMemoryRandomAccessStream.GetOutputStreamAt(0);

    // Protect "inputStream".
    DataProtectionInfo info = 
        await DataProtectionManager.ProtectStreamAsync(unprotectedStream, identity, protectedStream);

    // Indicate to the caller whether the stream was protected successfully. It will only be protected if
    // the identity is managed AND this app is allowed for this identity. Similar to buffers, this status
    // must be stored by the app. UnprotectStreamAsync must only be called if the stream was protected
    // successfully earlier.

    return (info.Status == DataProtectionStatus.Protected);
}
```

Zur Veranschaulichung der Verwendung von Methoden wie der aus dem obigen Codebeispiel sehen Sie hier eine Hilfsmethode, die mithilfe der Methode eine Zeichenfolge in einen ungeschützten Datenstrom konvertiert und anschließend den Datenstrom schützt.

```CSharp
using Windows.Storage.Streams;

private async System.Threading.Tasks.Task<bool> ProtectStringAsStreamAsync
    (string unprotectedEnterpriseData, string identity, 
    InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    using (var unprotectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream())
    {
        using (var dataWriter = new DataWriter(unprotectedInMemoryRandomAccessStream))
        {
            dataWriter.WriteString(unprotectedEnterpriseData);
            await dataWriter.StoreAsync();
            await dataWriter.FlushAsync();
            return await this.ProtectAStream(unprotectedInMemoryRandomAccessStream,
                identity, protectedInMemoryRandomAccessStream);
        }
    }
}
```

## Abrufen des Status eines Datenstroms


In diesem Szenario hat Ihre App einen Datenstrom geschützt, um nicht autorisierte Zugriffe zu verhindern. Ihre App kann den Status des Datenstroms überprüfen, um dessen Inhalte bei Bedarf wieder abrufen zu können.

Beachten Sie, dass der Status eines Datenstroms auch von [**DataProtectionManager.UnprotectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706023) zurückgegeben wird. Diese API gibt nie den Status „Ungeschützt“ zurück, da sie den Schutz der Eingaberessource voraussetzt. Daher kann nicht zuverlässig ermittelt werden, ob eine Ressource ungeschützt ist. Hinweis: Wenn Sie über Code verfügen, der das Ergebnis mit „Ungeschützt“ vergleicht, liegt wahrscheinlich ein Entwurfsfehler vor. Dies deutet darauf hin, dass der Schutzstatus des Datenstroms nicht mehr nachvollzogen werden kann.

```CSharp
using Windows.Storage.Streams;
using Windows.Security.EnterpriseData;

private async void CheckProtectedStreamStatus(IInputStream protectedStream)
{
    DataProtectionInfo dataProtectionInfo = 
        await DataProtectionManager.GetStreamProtectionInfoAsync(protectedStream);

    if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
    {
        // Code goes here to handle this situation. Perhaps, show UI
        // saying that the user's data has been revoked.
    }
    else if (dataProtectionInfo.Status != DataProtectionStatus.Protected)
    {
        // Code goes here to handle the unexpected protection status.
    }
}
```

## Aufheben des Schutzes für einen Datenstrom


In diesem Szenario soll Ihre App den Schutz für einen zuvor geschützten Datenstrom aufheben. Im Codebeispiel wird der Schutz eines Datenstroms durch Aufrufen von [**DataProtectionManager.UnprotectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706023) aufgehoben. Damit dies funktioniert, muss der Datenstrom geschützt sein. Anschließend wird eine Zeichenfolge aus dem Datenstrom gelesen und zurückgegeben.

```CSharp
using Windows.Storage.Streams;

private async System.Threading.Tasks.Task<string> UnprotectStreamIntoStringAsync
    (InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    using (var unprotectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream())
    {
        DataProtectionInfo dataProtectionInfo = 
            await DataProtectionManager.UnprotectStreamAsync(protectedInMemoryRandomAccessStream, 
            unprotectedInMemoryRandomAccessStream);

        using (var inputStream = unprotectedInMemoryRandomAccessStream.GetInputStreamAt(0))
        {
            using (var dataReader = new DataReader(inputStream))
            {
                await dataReader.LoadAsync((uint)unprotectedInMemoryRandomAccessStream.Size);
                return dataReader.ReadString((uint)unprotectedInMemoryRandomAccessStream.Size);
            }
        }
    }
}
```

Zur Veranschaulichung einer möglichen Verwendung der bereits behandelten Hilfsmethoden sehen Sie hier einen Ereignishandler, der eine Zeichenfolge aus einem Textfeld in einen Datenstrom schreibt, den Datenstrom schützt, den Schutz für den Datenstrom aufhebt (sofern er erfolgreich geschützt wurde) und schließlich die Zeichenfolge aus dem ungeschützten Datenstrom liest und auf der Benutzeroberfläche anzeigt.

```CSharp
using Windows.Storage.Streams;

private async void ProtectAndThenUnprotectStream_Click(object sender, RoutedEventArgs e)
{
    var protectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream();
    bool isStreamProtected = await this.ProtectStringAsStreamAsync
        (this.enterpriseDataTextBox.Text, MainPage.IDENTITY, protectedInMemoryRandomAccessStream);

    // Only unprotect the stream if we're sure that the stream actually was protected.
    if (isStreamProtected)
    {
        this.resultDataTextBlock.Text = 
            await this.UnprotectStreamIntoStringAsync(protectedInMemoryRandomAccessStream);
    }
}
```

## Abrufen des Status eines statischen Datenpuffers


In diesem Szenario hat Ihre App einen Puffer geschützt, um nicht autorisierte Zugriffe zu verhindern. Ihre App kann den Status des Puffers überprüfen, um dessen Inhalte bei Bedarf wieder abrufen zu können.

Beachten Sie, dass der Status eines Puffers auch von [**DataProtectionManager.UnprotectAsync**](https://msdn.microsoft.com/library/windows/apps/dn706022) zurückgegeben wird. Diese API gibt nie den Status „Ungeschützt“ zurück, da sie den Schutz der Eingaberessource voraussetzt.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

private async void CheckProtectedBufferStatus(IBuffer protectedData)
{
    DataProtectionInfo dataProtectionInfo = 
        await DataProtectionManager.GetProtectionInfoAsync(protectedData);

    if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
    {
        // Code goes here to handle this situation, perhaps show UI
        // saying that the user's data has been revoked.
    }
    else if (dataProtectionInfo.Status != DataProtectionStatus.Protected)
    {
        // Code goes here to handle the unexpected protection status.
    }
}
```

## Schützen statischer Daten aus einer Unternehmensressource


Dieses Szenario behandelt die gleichen Fälle wie die Codebeispiele für Datenströme, nur eben für Datenpuffer. Auch in diesem Fall müssen Sie den Zustand verwalten, um zu wissen, ob der Puffer geschützt wurde (wie hier zu sehen). Wenn die übergebene Identität nicht verwaltet wird oder die App für die Identität nicht zulässig ist, wird der Puffer nicht geschützt, und beim Aufruf von [**DataProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn706020) wird der Status „Ungeschützt“ zurückgegeben.

```CSharp
using Windows.Security.Cryptography;
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

...

private IBuffer data = null;
private bool isProtected = false;

void StoreBuffer(IBuffer data, bool isProtected)
{
    this.data = data;
    this.isProtected = isProtected;
}

IBuffer GetStoredBuffer(out bool isProtected)
{
    isProtected = this.isProtected;
    return this.data;
}

private string identity = "contoso.com";

private async void ProtectAndThenUnprotectBuffer_Click(object sender, RoutedEventArgs e)
{
    BinaryStringEncoding encoding = BinaryStringEncoding.Utf8;
    IBuffer inputData = CryptographicBuffer.ConvertStringToBinary
        (this.enterpriseDataTextBox.Text, encoding);
    BufferProtectUnprotectResult result =
        await DataProtectionManager.ProtectAsync(inputData, this.identity);

    // Record whether the buffer was protected successfully. It will only be protected if
    // the identity is managed AND this app is allowed for this identity. This status
    // must be stored by the app. UnprotectAsync must only be called if the buffer was
    // protected successfully earlier.
    bool isBufferProtected = 
        (result.ProtectionInfo.Status == DataProtectionStatus.Protected);
    IBuffer outputData = result.Buffer;

    // Store the data away...
    this.StoreBuffer(outputData, isBufferProtected);

    // ...and then later retrieve it.
    outputData = this.GetStoredBuffer(out isBufferProtected);

    string storedString = string.Empty;

    if (isBufferProtected)
    {
        result = await DataProtectionManager.UnprotectAsync(outputData);

        if (result.ProtectionInfo.Status != DataProtectionStatus.Unprotected)
        {
            // Code goes here to handle the situation where the buffer
            // is no longer accessible.
            return;
        }
        outputData = result.Buffer;
    }
    this.resultDataTextBlock.Text = CryptographicBuffer.ConvertBinaryToString(encoding, outputData);
}
```

## Aktivieren der UI-Richtlinienerzwingung auf der Grundlage der geschützten Identität eines Datenstroms oder Puffers


Wenn Ihre App im Begriff ist, den Inhalt eines geschützten Datenstroms oder Puffers auf der Benutzeroberfläche anzuzeigen, muss sie die UI-Richtlinienerzwingung auf der Grundlage der geschützten Identität der Ressource aktivieren. Fragen Sie die Schutzinformationen der Ressource ab, und aktivieren Sie die UI-Richtlinienerzwingung auf der Grundlage der abgerufenen Identität.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

...

private async void EnableUIPolicyFromProtectedBuffer(IBuffer buffer)
{
    DataProtectionInfo protectionInfo = 
        await DataProtectionManager.GetProtectionInfoAsync(buffer);

    if (protectionInfo.Status != DataProtectionStatus.Protected)
    {
        // In this case, the app has lost access to the buffer
        // (ProtectedToOtherIdentity, Revoked). This must be handled.
        // 'Unprotected' is never returned for GetProtectionInfoAsync().
        return;
    }

    ProtectionPolicyManager.TryApplyProcessUIPolicy(protectionInfo.Identity);
}

```


            **Hinweis**  Dieser Artikel ist für Windows 10-Entwickler gedacht, die Apps für die Universelle Windows-Plattform (UWP) schreiben. Wenn Sie für Windows8.x oder Windows Phone8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Verwandte Themen


[Unternehmensdatenschutz (EDP) – Beispiel](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Windows.Security.EnterpriseData-Namespace**](https://msdn.microsoft.com/library/windows/apps/dn279153)

 

 






<!--HONumber=Jun16_HO4-->


