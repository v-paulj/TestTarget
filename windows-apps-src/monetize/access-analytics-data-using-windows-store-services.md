---
author: mcleanbyron
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: "Verwenden Sie zum programmgesteuerten Abrufen von Analysedaten für Apps, die für Ihr WindowsDevCenter-Konto oder für das Konto Ihrer Organisation registriert sind, die WindowsStore-Analyse-API."
title: Zugreifen auf Analysedaten mit WindowsStore-Diensten
translationtype: Human Translation
ms.sourcegitcommit: 47e0ac11178af98589e75cc562631c6904b40da4
ms.openlocfilehash: 1293bb5beb927425928d832f887129263db5a895

---

# Zugreifen auf Analysedaten mit WindowsStore-Diensten

Verwenden Sie zum programmgesteuerten Abrufen von Analysedaten für Apps, die für Ihr WindowsDevCenter-Konto oder für das Konto Ihrer Organisation registriert sind, die *WindowsStore-Analyse-API*. Mit dieser API können Sie Daten zum Kauf von Apps und Add-Ons (auch als In-App-Produkte (IAPs) bezeichnet), Fehler, App-Bewertungen und App-Rezensionen abrufen. Diese API verwendet Azure Active Directory (Azure AD), um die Aufrufe von Ihrer App oder Ihrem Dienst zu authentifizieren.

Dazu müssen folgende Schritte ausgeführt werden:

1.  Vergewissern Sie sich, dass Sie alle [Voraussetzungen](#prerequisites) erfüllt haben.
2.  Vor dem Aufrufen einer Methode in der Windows Store-Analyse-API müssen Sie [ein AzureAD-Zugriffstoken anfordern](#obtain-an-azure-ad-access-token). Nach dem Abrufen eines Tokens haben Sie 60Minuten Zeit, um das Token zum Aufrufen der Windows Store-Analyse-API zu nutzen, bevor es abläuft. Nach dem Ablauf des Tokens können Sie ein neues Token generieren.
3.  [Aufrufen der Windows Store-Analyse-API](#call-the-windows-store-analytics-api).

<span id="prerequisites" />
## Schritt1: Erfüllen der Voraussetzungen für die Verwendung der Windows Store-Analyse-API

Stellen Sie sicher, dass Sie die folgenden Voraussetzungen erfüllt haben, bevor Sie mit dem Schreiben von Code zum Aufrufen der Windows Store-Analyse-API beginnen:

* Sie (bzw. Ihre Organisation) müssen über ein Azure AD-Verzeichnis und die Berechtigung [Globaler Administrator](http://go.microsoft.com/fwlink/?LinkId=746654) für das Verzeichnis verfügen. Wenn Sie bereits mit Office 365oder anderen Unternehmensdiensten von Microsoft arbeiten, verfügen Sie schon über ein Azure AD-Verzeichnis. Andernfalls können Sie [innerhalb von Dev Center ohne zusätzliche Kosten eine neue Azure AD-Instanz erstellen](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users).

* Sie müssen Ihrem Dev Center-Konto eine Azure AD-Anwendung zuordnen, die Mandanten-ID und die Client-ID für die Anwendung abrufen und einen Schlüssel generieren. Die Azure AD-Anwendung stellt die App oder den Dienst dar, aus denen Sie die Windows Store-Analyse-API aufrufen möchten. Sie benötigen die Mandanten-ID, die Client-ID und den Schlüssel zum Abrufen eines Azure AD-Zugriffstokens, das Sie an die API übergeben.

  >**Hinweis**&nbsp;&nbsp;Sie müssen dies nur einmal durchführen. Wenn Sie im Besitz der Mandanten-ID, der Client-ID und des Schlüssel sind, können Sie diese Daten jederzeit wiederverwenden, um ein neues Azure AD-Zugriffstoken zu erstellen.

Gehen Sie wie folgt vor, um Ihrem Dev Center-Konto eine Azure AD-Anwendung zuzuordnen und die erforderlichen Werte abzurufen:

1.  Rufen Sie in Dev Center die **Kontoeinstellungen** auf, klicken Sie auf **Benutzer verwalten**, und ordnen Sie das Dev Center-Konto Ihrer Organisation dem Azure AD-Verzeichnis Ihrer Organisation zu. Ausführliche Anweisungen finden Sie unter [Verwalten von Kontobenutzern](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users).

2.  Klicken Sie auf der Seite **Benutzer verwalten** auf **Azure AD-Apps hinzufügen**, und fügen Sie die AzureAD-Anwendung hinzu, die die App oder den Dienst darstellt, mit dem Sie auf Analysedaten für Ihr Dev Center-Konto zugreifen. Weisen Sie ihr anschließend die Rolle **Manager** zu. Wenn diese Anwendung bereits in Ihrem AzureAD-Verzeichnis vorhanden ist, können Sie sie auf der Seite **Azure AD-Apps hinzufügen** auswählen, um sie Ihrem Dev Center-Konto hinzuzufügen. Andernfalls können Sie eine neue Azure AD-Anwendung auf der Seite **Azure AD-Apps hinzufügen** erstellen. Weitere Informationen finden Sie unter [Hinzufügen und Verwalten von Azure AD-Apps](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

3.  Wechseln Sie zurück zur Seite **Benutzer verwalten**, klicken Sie auf den Namen Ihrer Azure AD-Anwendung, um die Anwendungseinstellungen aufzurufen, und kopieren Sie die Werte unter **Mandanten-ID** und **Client-ID**.

4. Klicken Sie auf **Neuen Schlüssel hinzufügen**. Kopieren Sie auf dem folgenden Bildschirm den Wert unter **Schlüssel**. Nach dem Verlassen der Seite können Sie nicht mehr auf diese Informationen zugreifen. Weitere Informationen zum Verwalten von Schlüsseln finden Sie unter [Hinzufügen und Verwalten von Azure AD-Apps](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

<span id="obtain-an-azure-ad-access-token" />
## Schritt 2: Abrufen eines Azure AD-Zugriffstokens

Bevor Sie die Methoden in der Windows Store-Analyse-API aufrufen, müssen Sie zuerst ein Azure AD-Zugriffstoken abrufen, das Sie für jede Methode in der API an den **Authorization**-Header übergeben. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60Minuten Zeit, das Token zu verwenden, bevor es abläuft. Nachdem das Token abgelaufen ist, können Sie es aktualisieren, um es in weiteren Aufrufen der API zu verwenden.

Befolgen Sie zum Abrufen des Zugriffstokens die Anweisungen unter [Aufrufe zwischen Diensten mithilfe von Clientanmeldeinformationen](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/), um eine HTTP POST-Anforderung an den ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```-Endpunkt zu senden. Hier ist ein Beispiel für eine Anforderung angegeben.

```
POST https://login.microsoftonline.com/<your_tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Geben Sie für die Parameter *tenant\_id*, *client\_id* und *client\_secret* die Mandanten-ID, die Client-ID und den Schlüssel für Ihre Anwendung als die Daten an, die Sie im vorherigen Abschnitt aus Dev Center abgerufen haben. Für den Parameter *resource* müssen Sie den URI ```https://manage.devcenter.microsoft.com``` angeben.

Nachdem das Zugriffstoken abgelaufen ist, können Sie es aktualisieren, indem Sie [diese Anleitung](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens) befolgen.

<span id="call-the-windows-store-analytics-api" />
## Schritt 3: Aufrufen der Windows Store-Analyse-API

Nachdem Sie ein AzureAD-Zugriffstoken abgerufen haben, können Sie die WindowsStore-Analyse-API aufrufen. Informationen zur Syntax der einzelnen Methoden finden Sie in den folgenden Artikeln: Sie müssen das Zugriffstoken an den **Authorization**-Header der einzelnen Methoden übergeben.

-   [Abrufen von App-Käufen](get-app-acquisitions.md)
-   [Abrufen von Add-On-Käufen](get-in-app-acquisitions.md)
-   [Abrufen von Fehlerberichtsdaten](get-error-reporting-data.md)
-   [Abrufen von App-Bewertungen](get-app-ratings.md)
-   [Abrufen von App-Rezensionen](get-app-reviews.md)

## Codebeispiel


Im folgenden Codebeispiel wird veranschaulicht, wie Sie ein AzureAD-Zugriffstoken abrufen und die WindowsStore-Analyse-API aus einer C#-Konsolen-App aufrufen. Wenn Sie dieses Codebeispiel verwenden möchten, weisen Sie die Variablen *TenantId*, *ClientId*, *ClientSecret* und *AppID* den entsprechenden Werten für Ihr Szenario zu. In diesem Beispiel wird das [Json.NET-Paket](http://www.newtonsoft.com/json) von Newtonsoft benötigt, um die von der Windows Store-Analyse-API zurückgegebenen JSON-Daten zu deserialisieren.

```CSharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;

namespace TestAnalyticsAPI
{
    class Program
    {
        static void Main(string[] args)
        {
            string tenantId = "<your tenant ID>";
            string clientId = "<your client ID>";
            string clientSecret = "<your secret>";

            string scope = "https://manage.devcenter.microsoft.com";

            // Retrieve an Azure AD access token
            string accessToken = GetClientCredentialAccessToken(
                    tenantId,
                    clientId,
                    clientSecret,
                    scope).Result;

            // This is your app's Store ID. This ID is available on
            // the App identity page of the Dev Center dashboard.
            string appID = "<your app's Store ID>";

            DateTime startDate = DateTime.Parse("08-01-2015");
            DateTime endDate = DateTime.Parse("11-01-2015");
            int pageSize = 1000;
            int startPageIndex = 0;

            // Call the Windows Store analytics API
            CallAnalyticsAPI(accessToken, appID, startDate, endDate, pageSize, startPageIndex);

            Console.Read();
        }

        private static void CallAnalyticsAPI(string accessToken, string appID, DateTime startDate, DateTime endDate, int top, int skip)
        {
            string requestURI;

            // Get app acquisitions
            requestURI = string.Format(
                "https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
                appID, startDate, endDate, top, skip);

            //// Get add-on acquisitions
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app failures
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app ratings
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId={0}&startDate={1}&endDate={2}top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app reviews
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            HttpRequestMessage requestMessage = new HttpRequestMessage(HttpMethod.Get, requestURI);
            requestMessage.Headers.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

            WebRequestHandler handler = new WebRequestHandler();
            HttpClient httpClient = new HttpClient(handler);

            HttpResponseMessage response = httpClient.SendAsync(requestMessage).Result;

            Console.WriteLine(response);
            Console.WriteLine(response.Content.ReadAsStringAsync().Result);

            response.Dispose();
        }

        public static async Task<string> GetClientCredentialAccessToken(string tenantId, string clientId, string clientSecret, string scope)
        {
            string tokenEndpointFormat = "https://login.microsoftonline.com/{0}/oauth2/token";
            string tokenEndpoint = string.Format(tokenEndpointFormat, tenantId);

            dynamic result;
            using (HttpClient client = new HttpClient())
            {
                string tokenUrl = tokenEndpoint;
                using (
                    HttpRequestMessage request = new HttpRequestMessage(
                        HttpMethod.Post,
                        tokenUrl))
                {
                    string content =
                        string.Format(
                            "grant_type=client_credentials&client_id={0}&client_secret={1}&resource={2}",
                            clientId,
                            clientSecret,
                            scope);

                    request.Content = new StringContent(content, Encoding.UTF8, "application/x-www-form-urlencoded");

                    using (HttpResponseMessage response = await client.SendAsync(request))
                    {
                        string responseContent = await response.Content.ReadAsStringAsync();
                        result = JsonConvert.DeserializeObject(responseContent);
                    }
                }
            }

            return result.access_token;
        }
    }
}
```

## Fehlerantworten


Die Windows Store-Analyse-API gibt Fehlerantworten in einem JSON-Objekt zurück, das Fehlercodes und -meldungen enthält. Im folgenden Beispiel wird eine Fehlerantwort veranschaulicht, die durch einen ungültigen Parameter bewirkt wurde.

```json
{
    "code":"BadRequest",
    "data":[],
    "details":[],
    "innererror":{
        "code":"InvalidQueryParameters",
        "data":[
            "top parameter cannot be more than 10000"
        ],
        "details":[],
        "message":"One or More Query Parameters has invalid values.",
        "source":"AnalyticsAPI"
    },
    "message":"The calling client sent a bad request to the service.",
    "source":"AnalyticsAPI"
}
```

## Verwandte Themen

* [Abrufen von App-Käufen](get-app-acquisitions.md)
* [Abrufen von Add-On-Käufen](get-in-app-acquisitions.md)
* [Abrufen von Fehlerberichtsdaten](get-error-reporting-data.md)
* [Abrufen von App-Bewertungen](get-app-ratings.md)
* [Abrufen von App-Rezensionen](get-app-reviews.md)
 



<!--HONumber=Sep16_HO1-->


