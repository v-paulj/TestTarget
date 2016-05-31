---
author: awkoren
Description: 'Dieses Thema enthält Beispiele für Programmieraufgaben, die in einigen der gängigsten EDP-Szenarien mit Datenübertragung durchgeführt werden müssen.'
MS-HAID: 'dev\_app\_to\_app.use\_edp\_to\_protect\_enterprise\_data\_transferred\_between\_apps'
MSHAttr: 'PreferredLib:/library/windows/apps'
Search.Product: eADQiWindows 10XVcnh
title: Schützen von zwischen Apps übertragenen Unternehmensdaten mithilfe von EDP
---

# Schützen von zwischen Apps übertragenen Unternehmensdaten mithilfe von EDP

__Hinweis__ EDP-Richtlinien (Enterprise Data Protection, Unternehmensdatenschutz) können nicht unter Windows 10 (Version 1511, Build 10586 oder älter) verwendet werden.

Dieses Thema enthält Beispiele für Programmieraufgaben, die in einigen der gängigsten EDP-Szenarien mit Datenübertragung durchgeführt werden müssen. Umfassende Informationen zu den Zusammenhängen zwischen EDP und Dateien, Datenströmen, Zwischenablage, Netzwerk, Hintergrundaufgaben und dem Schutz von Daten im Sperrzustand finden Sie unter [Unternehmensdatenschutz (EDP)](../enterprise/edp-hub.md).

**Hinweis**  Das [Unternehmensdatenschutz-Beispiel (EDP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409) veranschaulicht viele der in diesem Thema beschriebenen Dateiszenarien.

## Voraussetzungen


-   **Vorbereitung für EDP**

    Weitere Informationen finden Sie unter [Einrichten des Computers für EDP](../enterprise/edp-hub.md#set-up-your-computer-for-EDP).

-   **Ausrichtung auf unternehmensoptimierte Apps**

    Eine App, die von sich aus dafür sorgt, dass Unternehmensdaten unter der Kontrolle des verwaltenden Unternehmens bleiben, wird als unternehmensoptimierte App bezeichnet. Eine optimierte App ist leistungsfähiger, intelligenter, flexibler und vertrauenswürdiger als eine nicht optimierte App. Durch Deklaration der eingeschränkten **enterpriseDataPolicy**-Funktion teilt die App dem System mit, dass sie optimiert ist. Zur Optimierung gehört jedoch mehr als nur das Festlegen einer Funktion. Weitere Informationen finden Sie unter [Unternehmensoptimierte Apps](../enterprise/edp-hub.md#enterprise-enlightened-apps).

-   **Kenntnisse in der asynchronen Programmierung für Apps für die universelle Windows-Plattform (UWP)**

    Informationen zum Schreiben von asynchronen Apps in C# oder Visual Basic finden Sie unter [Aufrufen asynchroner APIs in C\# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

## Einfache Zwischenablagequelle


In diesem Szenario ist Ihre App eine Editor-App, die sowohl für private Dateien als auch für Unternehmensdateien verwendet wird. Hier ist keine Änderung der App-Logik für das Kopieren und Einfügen erforderlich. Die App muss lediglich [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791) aufrufen, wenn der Benutzer ein Unternehmensdokument öffnet und dessen Inhalt anzeigt. Wenn der Inhalt auf der Benutzeroberfläche Ihrer App angezeigt wird, kann ihn der Benutzer kopieren und in eine andere App einfügen. Daher ist es wichtig, die UI-Richtlinie festzulegen. Dadurch kann das Betriebssystem die derzeit festgelegte Richtlinie auf einen Einfügevorgang mit geschützten Daten anwenden. Ebenso wichtig ist es, die UI-Richtlinie wieder zu löschen, sobald sie nicht mehr benötigt wird, damit der Benutzer private Daten wieder ohne Einschränkungen kopieren/einfügen kann. **TryApplyProcessUIPolicy** gibt „false“ zurück, wenn das dazugehörige Identity-Argument nicht durch eine Unternehmensrichtlinie verwaltet wird.

```CSharp
using Windows.Security.EnterpriseData;

...

private void OnFileLoaded(FileProtectionInfo fileProtectionInfo, string contentsOfFile)
{
    if (fileProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy
            (fileProtectionInfo.Identity);
        if (isIdentityManaged)
        {
            // UI policy is now in effect for the file's identity.
        }
        else
        {
            // Enterprise policy is not in effect, because the file's identity
            // is not managed. In this case, we have a file protected to an
            // unmanaged identity, which is not a valid situation.
            // We still have to call ClearProcessUIPolicy if we want to clear the policy.
            ProtectionPolicyManager.ClearProcessUIPolicy();
        }
    }
    else
    {
        // In case we applied the policy for the previous file, now we need to clear it.
        ProtectionPolicyManager.ClearProcessUIPolicy();
    }
    // Code goes here to display "contentsOfFile" in your UI. Ready for copy-paste...
}
```

## Einfaches Zwischenablageziel


In diesem Szenario ist Ihre App eine E-Mail-App mit privaten Konten und Unternehmenskonten. Der Benutzer versucht, über sein privates Konto Daten in eine E-Mail-Antwort einzufügen. In diesem Fall muss die App vor dem Abrufen der Inhalte lediglich [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636) aufrufen. Wenn wir bereits Zugriff haben, wird die Methode umgehend zurückgegeben. Andernfalls löst die Methode die Anzeige eines Dialogfelds aus, in dem die Zustimmung des Benutzers angefordert wird. Gibt dieser seine Zustimmung, wird das Datenpaket entsperrt. Dadurch haben Benutzer die Möglichkeit, einen versehentlich ausgelösten Einfügevorgang abzubrechen.

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private async void OnPasteSimple()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        // In case we don't already have acccess, request consent from the user
        // for us to access the clipboard data.
        await dataPackageView.RequestAccessAsync();

        try
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            // Code goes here to insert the text into the email...
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the clipboard.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```

## Zwischenablageziel als neutrales, leeres Dokument


In diesem Szenario ist Ihre App eine Textverarbeitungs-App. Nach dem Erstellen eines neuen Dokuments behandelt Ihre App das Dokument als neutral (also weder als privates Dokument noch als Unternehmensdokument), solange es leer ist. In dieses Dokument mit neutralem Kontext fügt der Benutzer nun Unternehmensdaten ein. Da sich das Dokument in einem neutralen Kontext befindet, könnten wir es eigentlich einfach in einen Unternehmenskontext versetzen und ganz auf den Aufruf von [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636) verzichten. (Die Anzeige des Dialogfelds ist in diesem Fall ja nicht erforderlich.) Wenn die Daten also geschützt sind, wechseln wir einfach in einen Unternehmenskontext und fügen die Daten ein.

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private async void OnPasteWithApplyPolicy()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        // If the data has no enterprise identity, then we already have access.
        if (!string.IsNullOrEmpty(dataPackageView.Properties.EnterpriseId))
        {
            ProtectionPolicyEvaluationResult policyResult =
                await dataPackageView.RequestAccessAsync(dataPackageView.Properties.EnterpriseId);
            if (this.isNewEmptyDocument &&
                policyResult == ProtectionPolicyEvaluationResult.Allowed)
            {
                // If this is a new and empty document, and we're allowed to access
                // the data, then we can avoid popping the consent dialog.
                bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy(dataPackageView.Properties.EnterpriseId);
                // You can use "isIdentityManaged", but it's not necessary.
            }
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.
                await dataPackageView.RequestAccessAsync();
            }
        }

        try
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            // Code goes here to insert the text into the email...
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the clipboard.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```

## Zwischenablagequelle mit expliziter Unternehmensidentität


In diesem Szenario ist Ihre App eine Textverarbeitungs-App. Diese verwendet einen Hintergrundthread, um ein Commit für Kopiervorgänge des Benutzers auszuführen. Der Benutzer kopiert einige Daten aus einer Unternehmensdatei und wechselt im Anschluss direkt zu einer privaten Datei, was den globalen Kontext der App zu einem privaten Kontext macht. Da sich der globale Zustand geändert hat und nicht verwendet werden soll, muss der Hintergrundthread der App die Zwischenablage an diesem Punkt explizit darüber informieren, dass es sich bei den eingehenden Daten um Unternehmensdaten handelt. Zu diesem Zweck wird die [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861)-Eigenschaft festgelegt.

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private void CopyToClipboard(string stringToCopy, string identity)
{
    // Copy the string to the clipboard.
    var dataPackage = new DataPackage();
    dataPackage.SetText(stringToCopy);
    dataPackage.Properties.EnterpriseId = identity;
    Clipboard.SetContent(dataPackage);
}
```

## Markieren eines spezifischen Fensters mit Unternehmensidentität


In diesem Szenario ist Ihre App eine Textverarbeitungs-App mit einer Mischung aus privaten Dokumenten und Unternehmensdokumenten in unterschiedlichen Fenstern. Die App soll sicherstellen, dass alle Daten, die in ein Fenster mit einem privaten Dokument eingefügt werden, ordnungsgemäß überprüft werden, sodass Unternehmensdaten entweder abgelehnt werden oder eine Zustimmung angefordert wird und ausgehende Daten eines Unternehmensdokumentfenster korrekt gekennzeichnet werden. Hierzu wird die [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785)-Eigenschaft festgelegt.

```CSharp
using Windows.Security.EnterpriseData;

...

private void TagCurrentViewWithEnterpriseId(string identity)
{
    ProtectionPolicyManager.GetForCurrentView().Identity = identity;
}
```

## Markieren eines ausgehenden Drag-Objekts mit Unternehmensidentität


In Ihrer App ist ein privates Fenster mit ziehbarem Unternehmensinhalt geöffnet. Der Benutzer beginnt, einen Teil dieses Inhalts zu ziehen, und Ihre App muss sicherstellen, dass der Inhalt als Unternehmensinhalt gekennzeichnet ist. Dieses Szenario umfasst keine neuen APIs. In diesem Szenario legt Ihre App die [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861)-Eigenschaft fest (siehe [Zwischenablagequelle mit expliziter Unternehmensidentität](#clipboard_source_explicit_id) weiter oben).

## Abfragen der Unternehmensidentität des erhaltenen Ziehobjekts


In Ihrer App ist ein neues, leeres Dokument geöffnet, das im leeren Zustand als neutral betrachtet wird, und der Benutzer fügt per Drag&Drop einige Inhalte in das Dokument. Die App muss nun die Unternehmensidentität des Objekts ermitteln, um den Zustand des Dokuments entsprechend ändern zu können. In diesem Szenario ruft Ihre App die **EnterpriseId**-Eigenschaft aus dem Datenpaket ab, indem sie [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861) liest (siehe [Zwischenablagequelle mit expliziter Unternehmensidentität](#clipboard_source_explicit_id) weiter oben).

## Ihre App als Quelle für einen Freigabe-Vertrag


Wenn Sie in Ihrer App den Freigabe-Vertrag unterstützen, müssen Sie zum Einrichten einer Freigabequelle den Unternehmensidentitätskontext im [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/br205873)-Element festlegen, wie in diesem Codebeispiel zu sehen.

**Hinweis**  Dieses Codebeispiel setzt voraus, dass Sie bereits die Identität für das ProtectionPolicyManager-Objekt für die aktuelle Ansicht festgelegt haben (siehe [Markieren eines spezifischen Fensters mit Unternehmensidentität](#tag_window_with_id)). Andernfalls enthält die [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785)-Eigenschaft eine leere Zeichenfolge.



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

private void OnShareSourceOperation(object sender, RoutedEventArgs e)
{
    // Register the current page as a share source (or you could do this earlier in your app).
    DataTransferManager.GetForCurrentView().DataRequested += OnDataRequested;
    DataTransferManager.ShowShareUI();
}

private void OnDataRequested(DataTransferManager sender, DataRequestedEventArgs args)
{
    if (!string.IsNullOrEmpty(this.shareSourceContent))
    {
        var protectionPolicyManager = ProtectionPolicyManager.GetForCurrentView();
        DataPackage requestData = args.Request.Data;
        requestData.Properties.Title = this.shareSourceTitle;
        requestData.Properties.EnterpriseId = protectionPolicyManager.Identity;
        requestData.SetText(this.shareSourceContent);
    }
}
```

## Ihre App als Ziel für einen Freigabe-Vertrag


In diesem Codebeispiel gilt weiterhin folgende Richtlinie: Solange die verwendete Datei leer ist, kann der Kontext gemäß den eingehenden Daten geändert und möglichst auf die Anzeige einer Zustimmungsanforderung verzichtet werden. Wenn Ihre App also als Ziel eines Freigabe-Vertrags Daten empfängt, muss sie [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791) aufrufen, falls noch keine Inhalte vorhanden sind. Andernfalls muss [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636) aufgerufen werden. Wie das geht, sehen Sie im Codebeispiel.

```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.ApplicationModel.DataTransfer.ShareTarget;

...

string identity = "microsoft.com";

protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    ShareOperation shareOperation = args.ShareOperation;
    if (shareOperation.Data.Contains(StandardDataFormats.Text))
    {
        // If the data has no enterprise identity, then we already have access.
        if (!string.IsNullOrEmpty(shareOperation.Data.Properties.EnterpriseId))
        {
            ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult =
                await ProtectionPolicyManager.RequestAccessAsync
                    (shareOperation.Data.Properties.EnterpriseId, identity);
            if (this.isNewEmptyDocument && protectionPolicyEvaluationResult ==
                ProtectionPolicyEvaluationResult.Allowed)
            {
                // If this is a new and empty document, and we're allowed to access
                // the data, then we can avoid popping the consent dialog.
                bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy
                    (shareOperation.Data.Properties.EnterpriseId);
                // You can use "isIdentityManaged", but it';s not necessary.
            }
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.
                protectionPolicyEvaluationResult = await shareOperation.Data.RequestAccessAsync();
            }
        }

        try
        {
            // Get the text from the share operation...
            App.shareTargetContent = await shareOperation.Data.GetTextAsync();
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the share operation.
        }
    }
    else
    {
        // The value in the share operation is not in the text format.
    }
}
```

## Passives Abfragen der Richtlinie für Kopieren/Einfügen


In diesem Szenario wird die UI für Einfügevorgänge nur aktiviert, wenn sich Daten in der Zwischenablage befinden. Für dieses Feature können Sie die [**ProtectionPolicyManager.CheckAccess**](https://msdn.microsoft.com/library/windows/apps/dn705783)-Methode verwenden, die eine passive Überprüfung der Richtlinie ermöglicht.

**Hinweis**  Dieses Codebeispiel setzt voraus, dass Sie bereits die Identität für das ProtectionPolicyManager-Objekt für die aktuelle Ansicht festgelegt haben (siehe [Markieren eines spezifischen Fensters mit Unternehmensidentität](#tag_window_with_id)). Andernfalls enthält die [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785)-Eigenschaft eine leere Zeichenfolge.



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

private bool IsClipboardPeekAllowedAsync()
{
    ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = ProtectionPolicyEvaluationResult.Blocked;
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);
    }

    // Since this is a convenience feature to allow a peek of clipboard content,
    // if state is Blocked or ConsentRequired, do not show peek if this helper
    // method returns false.
    return (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed);
}
```

## Anfordern des Zugriffs für einen Kopieren/Einfügen-Vorgang


Das Szenario veranschaulicht die Überprüfung des Zugriffs für einen Einfügevorgang.

**Hinweis**  Dieses Codebeispiel setzt voraus, dass Sie bereits die Identität für das ProtectionPolicyManager-Objekt für die aktuelle Ansicht festgelegt haben (siehe [Markieren eines spezifischen Fensters mit Unternehmensidentität](#tag_window_with_id)). Andernfalls enthält die [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785)-Eigenschaft eine leere Zeichenfolge.



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

private async void OnPasteWithRequestAccess()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);

        if (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed)
        {
            try
            {
                string contentsOfClipboard = await dataPackageView.GetTextAsync();
                // Code goes here to insert the text into the app...
                this.fileContentsTextBox.Text = contentsOfClipboard;
            }
            catch (Exception)
            {
                // Code goes here to handle the exception retrieving text from the clipboard.
            }
        }
        else
        {
            // Paste from the enterprise context is not allowed by policy.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```

**Hinweis**  Dieser Artikel ist für Windows 10-Entwickler gedacht, die Apps für die UWP-Apps (Universelle Windows-Plattform) schreiben. Wenn Sie für Windows 8.x oder Windows Phone 8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).



## Verwandte Themen


[Unternehmensdatenschutz (EDP) – Beispiel](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Windows.Security.EnterpriseData-Namespace**](https://msdn.microsoft.com/library/windows/apps/dn279153)







<!--HONumber=May16_HO2-->


