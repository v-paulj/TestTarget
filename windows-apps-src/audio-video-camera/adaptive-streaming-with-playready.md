---
ms.assetid: BF877F23-1238-4586-9C16-246F3F25AE35
description: Hier wird beschrieben, wie Sie einer App für die UWP adaptives Streaming von Multimediainhalten mit Microsoft PlayReady-Inhaltsschutz hinzufügen.
title: Adaptives Streaming mit PlayReady
---

# Adaptives Streaming mit PlayReady

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

\[Einige Informationen beziehen sich auf die Vorabversion, die vor der kommerziellen Freigabe möglicherweise wesentlichen Änderungen unterliegt. Microsoft übernimmt keine Garantie, weder ausdrücklich noch stillschweigend, für die hier bereitgestellten Informationen.\]

In diesem Artikel wird beschrieben, wie Sie einer App für die universelle Windows-Plattform (UWP) adaptives Streaming von Multimediainhalten mit Microsoft PlayReady-Inhaltsschutz hinzufügen. Dieses Feature unterstützt derzeit die Wiedergabe von HTTP Live Streaming (HLS)- und Dynamic Streaming over HTTP (DASH)-Inhalten.

Dieser Artikel befasst sich nur mit den Aspekten des für PlayReady spezifischen adaptiven Streamings. Informationen zur allgemeinen Implementierung des adaptiven Streamings finden Sie unter [Adaptives Streaming](adaptive-streaming.md).

Sie benötigen die folgenden using-Anweisungen:

```csharp
using LicenseRequest;
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Runtime.InteropServices;
using System.Threading.Tasks;
using Windows.Foundation.Collections;
using Windows.Media.Protection;
using Windows.Media.Protection.PlayReady;
using Windows.Media.Streaming.Adaptive;
using Windows.UI.Xaml.Controls;
```

Der Namespace **LicenseRequest** stammt aus **CommonLicenseRequest.cs**, einer PlayReady-Datei, die Lizenznehmern von Microsoft zur Verfügung gestellt wird.

Sie müssen einige globale Variablen deklarieren:

```csharp
private AdaptiveMediaSource ams = null;
private MediaProtectionManager protectionManager = null;
private string playReadyLicenseUrl = "";
private string playReadyChallengeCustomData = "";
```

Sie sollten auch die folgende Konstante deklarieren:

```csharp
private const int MSPR_E_CONTENT_ENABLING_ACTION_REQUIRED = -2147174251;
```

## Einrichten von „MediaProtectionManager“

Um Ihrer UWP-App PlayReady-Inhaltsschutz hinzuzufügen, müssen Sie ein [MediaProtectionManager](https://msdn.microsoft.com/library/windows/apps/br207040)-Objekt einrichten. Diese Einrichtung erfolgt beim Initialisieren Ihres [**AdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn946912)-Objekts.

Mit dem folgenden Code wird ein [MediaProtectionManager](https://msdn.microsoft.com/library/windows/apps/br207040)-Objekt eingerichtet:

```csharp
private void SetUpProtectionManager(ref MediaElement mediaElement)
{
    protectionManager = new MediaProtectionManager();

    protectionManager.ComponentLoadFailed += 
        new ComponentLoadFailedEventHandler(ProtectionManager_ComponentLoadFailed);

    protectionManager.ServiceRequested += 
        new ServiceRequestedEventHandler(ProtectionManager_ServiceRequested);

    PropertySet cpSystems = new PropertySet();

    cpSystems.Add(
        "{F4637010-03C3-42CD-B932-B48ADF3A6A54}", 
        "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput");

    protectionManager.Properties.Add("Windows.Media.Protection.MediaProtectionSystemIdMapping", cpSystems);

    protectionManager.Properties.Add(
        "Windows.Media.Protection.MediaProtectionSystemId", 
        "{F4637010-03C3-42CD-B932-B48ADF3A6A54}");

    protectionManager.Properties.Add(
        "Windows.Media.Protection.MediaProtectionContainerGuid", 
        "{9A04F079-9840-4286-AB92-E65BE0885F95}");

    mediaElement.ProtectionManager = protectionManager;
}
```

Dieser Code kann einfach in Ihre App kopiert werden, da er für die Einrichtung des Inhaltsschutzes erforderlich ist.

Das [ComponentLoadFailed](https://msdn.microsoft.com/library/windows/apps/br207041)-Ereignis wird ausgelöst, wenn das Laden von Binärdaten fehlschlägt. Zum Behandeln dieses Ereignisses müssen wir einen Ereignishandler hinzufügen, der signalisiert, dass das Laden nicht abgeschlossen wurde:

```csharp
private void ProtectionManager_ComponentLoadFailed(
    MediaProtectionManager sender, 
    ComponentLoadFailedEventArgs e)
{
    e.Completion.Complete(false);
}
```

Ebenso müssen wir einen Ereignishandler für das [ServiceRequested](https://msdn.microsoft.com/library/windows/apps/br207045)-Ereignis hinzufügen, das beim Anfordern eines Diensts ausgelöst wird. Dieser Code überprüft, um welche Art von Anforderung es sich handelt, und reagiert entsprechend:

```csharp
private async void ProtectionManager_ServiceRequested(
    MediaProtectionManager sender, 
    ServiceRequestedEventArgs e)
{
    if (e.Request is PlayReadyIndividualizationServiceRequest)
    {
        PlayReadyIndividualizationServiceRequest IndivRequest = 
            e.Request as PlayReadyIndividualizationServiceRequest;

        bool bResultIndiv = await ReactiveIndivRequest(IndivRequest, e.Completion);
    }
    else if (e.Request is PlayReadyLicenseAcquisitionServiceRequest)
    {
        PlayReadyLicenseAcquisitionServiceRequest licenseRequest = 
            e.Request as PlayReadyLicenseAcquisitionServiceRequest;

        LicenseAcquisitionRequest(
            licenseRequest, 
            e.Completion, 
            playReadyLicenseUrl, 
            playReadyChallengeCustomData);
    }
}
```

## Individualisierungsdienstanforderung

Der folgende Code sendet reaktiv eine PlayReady-Individualisierungsdienstanforderung. Wir übergeben die Anforderung als Parameter an die Funktion. Wir platzieren den Aufruf in einem try/catch-Block. Wenn keine Ausnahmen auftreten, wurde die Anforderung in unseren Augen erfolgreich abgeschlossen:

```csharp
async Task<bool> ReactiveIndivRequest(
    PlayReadyIndividualizationServiceRequest IndivRequest, 
    MediaProtectionServiceCompletion CompletionNotifier)
{
    bool bResult = false;
    Exception exception = null;

    try
    {
        await IndivRequest.BeginServiceRequest();
    }
    catch (Exception ex)
    {
        exception = ex;
    }
    finally
    {
        if (exception == null)
        {
            bResult = true;
        }
        else
        {
            COMException comException = exception as COMException;
            if (comException != null &amp;&amp; comException.HResult == MSPR_E_CONTENT_ENABLING_ACTION_REQUIRED)
            {
                IndivRequest.NextServiceRequest();
            }
        }
    }

    if (CompletionNotifier != null) CompletionNotifier.Complete(bResult);
    return bResult;
}
```

Alternativ soll vielleicht proaktiv eine Individualisierungsdienstanforderung gesendet werden. In diesem Fall wird anstelle des Codes, der `ReactiveIndivRequest` in `ProtectionManager_ServiceRequested` aufruft, die folgende Funktion aufgerufen:

```csharp
async void ProActiveIndivRequest()
{
    PlayReadyIndividualizationServiceRequest indivRequest = new PlayReadyIndividualizationServiceRequest();
    bool bResultIndiv = await ReactiveIndivRequest(indivRequest, null);
}
```

## Lizenzerwerb-Dienstanforderungen

Falls es sich bei der Anforderung stattdessen um [PlayReadyLicenseAcquisitionServiceRequest](https://msdn.microsoft.com/library/windows/apps/dn986285) handelt, rufen wir die folgende Funktion auf, um die PlayReady-Lizenz anzufordern und zu erwerben. Wir informieren das übergebene MediaProtectionServiceCompletion-Objekt, ob die Anforderung erfolgreich war, und schließen die Anforderung ab:

```csharp
async void LicenseAcquisitionRequest(
    PlayReadyLicenseAcquisitionServiceRequest licenseRequest, 
    MediaProtectionServiceCompletion CompletionNotifier, 
    string Url, 
    string ChallengeCustomData)
{
    bool bResult = false;
    string ExceptionMessage = string.Empty;

    try
    {
        if (!string.IsNullOrEmpty(Url))
        {
            if (!string.IsNullOrEmpty(ChallengeCustomData))
            {
                System.Text.UTF8Encoding encoding = new System.Text.UTF8Encoding();
                byte[] b = encoding.GetBytes(ChallengeCustomData);
                licenseRequest.ChallengeCustomData = Convert.ToBase64String(b, 0, b.Length);
            }

            PlayReadySoapMessage soapMessage = licenseRequest.GenerateManualEnablingChallenge();

            byte[] messageBytes = soapMessage.GetMessageBody();
            HttpContent httpContent = new ByteArrayContent(messageBytes);

            IPropertySet propertySetHeaders = soapMessage.MessageHeaders;

            foreach (string strHeaderName in propertySetHeaders.Keys)
            {
                string strHeaderValue = propertySetHeaders[strHeaderName].ToString();

                if (strHeaderName.Equals("Content-Type", StringComparison.OrdinalIgnoreCase))
                {
                    httpContent.Headers.ContentType = MediaTypeHeaderValue.Parse(strHeaderValue);
                }
                else
                {
                    httpContent.Headers.Add(strHeaderName.ToString(), strHeaderValue);
                }
            }

            CommonLicenseRequest licenseAcquision = new CommonLicenseRequest();

            HttpContent responseHttpContent = 
                await licenseAcquision.AcquireLicense(new Uri(Url), httpContent);

            if (responseHttpContent != null)
            {
                Exception exResult = licenseRequest.ProcessManualEnablingResponse(
                                         await responseHttpContent.ReadAsByteArrayAsync());

                if (exResult != null)
                {
                    throw exResult;
                }
                bResult = true;
            }
            else
            {
                ExceptionMessage = licenseAcquision.GetLastErrorMessage();
            }
        }
        else
        {
            await licenseRequest.BeginServiceRequest();
            bResult = true;
        }
    }
    catch (Exception e)
    {
        ExceptionMessage = e.Message;
    }

    CompletionNotifier.Complete(bResult);
}
```

## Initialisieren von „AdaptiveMediaSource“

Schließlich benötigen Sie eine Funktion zum Initialisieren des [AdaptiveMediaSource](https://msdn.microsoft.com/library/windows/apps/dn946912)-Objekts, das aus einem bestimmten [Uri](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx)- und [MediaElement](https://msdn.microsoft.com/library/windows/apps/br242926)-Objekt erstellt wurde. Bei **Uri** sollte es sich um den Link zur Mediendatei (HLS oder DASH) handeln. **MediaElement** muss in Ihrem XAML-Code definiert werden.

```csharp
async private void InitializeAdaptiveMediaSource(System.Uri uri, MediaElement m)
{
    AdaptiveMediaSourceCreationResult result = await AdaptiveMediaSource.CreateFromUriAsync(uri);
    if (result.Status == AdaptiveMediaSourceCreationStatus.Success)
    {
        ams = result.MediaSource;
        SetUpProtectionManager(ref m);
        m.SetMediaStreamSource(ams);
    }
    else
    {
        // Error handling
    }
}
```

Sie können diese Funktion in jedem Ereignis aufrufen, das den Start des adaptiven Streamings behandelt, z. B. in einem Click-Ereignis für eine Schaltfläche.

 

 






<!--HONumber=Mar16_HO1-->


