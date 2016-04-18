---
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Verwenden Sie zum programmgesteuerten Abrufen von Analysedaten für Apps, die für Ihr Windows Dev Center-Konto oder für das Konto Ihrer Organisation registriert sind, die Windows Store-Analyse-API.
title: Zugreifen auf Analysedaten mit Windows Store-Diensten
---

# Zugreifen auf Analysedaten mit Windows Store-Diensten


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Programmgesteuertes Abrufen von Analysedaten für Apps, die für Ihr Windows Dev Center-Konto oder das Ihrer Organisation registriert sind, mithilfe der *Windows Store-Analyse-API*. Mit dieser API können Sie Daten für App- und IAP-Käufe, Fehler sowie App-Bewertungen und -Rezensionen abrufen. Diese API verwendet Azure Active Directory (Azure AD), um die Aufrufe von Ihrer App oder Ihrem Dienst zu authentifizieren.

## Voraussetzung für die Verwendung der Windows Store-Analyse-API


-   Sie (oder Ihre Organisation) muss ein Azure AD-Verzeichnis besitzen. Wenn Sie bereits mit Office 365 oder anderen Unternehmensdiensten von Microsoft arbeiten, verfügen Sie schon über ein Azure AD-Verzeichnis. Andernfalls können Sie es [kostenlos abrufen](http://go.microsoft.com/fwlink/p/?LinkId=703757).
-   Sie benötigen ein [Benutzerkonto](https://azure.microsoft.com/documentation/articles/active-directory-create-users/) im Azure AD-Verzeichnis, das Sie mit Ihrem Windows Dev Center-Konto verknüpfen möchten.

## Verwenden der Windows Store-Analyse-App


Vor der Verwendung der Windows Store-Analyse-API müssen Sie Ihrem Dev Center-Konto eine Azure AD-Anwendung zuordnen und ein Azure AD-Zugriffstoken abrufen. Die Azure AD-Anwendung stellt die App oder den Dienst dar, aus denen Sie die Windows Store-Analyse-API aufrufen möchten. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie aus der App oder dem Dienst die Windows Store-Analyse-API aufrufen.

Dazu müssen folgende Schritte ausgeführt werden:

1.  [Zuordnen einer Azure AD-Anwendung zu Ihrem Windows Dev Center-Konto](#associate-an-azure-ad-application-with-your-windows-dev-center-account).
2.  [Abrufen eines Azure AD-Zugriffstokens](#obtain-an-azure-ad-access-token).
3.  [Aufrufen der Windows Store-Analyse-API](#call-the-windows-store-analytics-api).


### Zuordnen einer Azure AD-Anwendung zu Ihrem Windows Dev Center-Konto

1.  Rufen Sie in Dev Center die **Kontoeinstellungen** auf, klicken Sie auf **Benutzer verwalten**, und ordnen Sie das Dev Center-Konto Ihrer Organisation dem Azure AD-Verzeichnis Ihrer Organisation zu. Ausführliche Anweisungen finden Sie unter [Verwalten von Kontobenutzern](https://msdn.microsoft.com/library/windows/apps/mt489008). Sie können optional andere Benutzer aus dem Azure AD-Verzeichnis Ihrer Organisation hinzufügen, damit sie ebenfalls auf das Dev Center-Konto zugreifen können.

    **Hinweis**  Einer Azure Active Directory-Instanz kann nur ein Dev Center-Konto zugeordnet werden. Ebenso kann einem Dev Center-Konto auch nur eine Azure Active Directory-Instanz zugeordnet sein. Nachdem Sie diese Zuordnung festgelegt haben, können Sie sie erst nach Rücksprache mit dem Support wieder entfernen.

     

2.  Klicken Sie auf der Seite **Benutzer verwalten** auf **Azure AD-Apps hinzufügen**, und fügen Sie die Azure AD-Anwendung hinzu, mit der Sie auf Analysedaten für Ihr Dev Center-Konto zugreifen. Weisen Sie ihr anschließend die Rolle **Manager** zu. Wenn diese Anwendung bereits in Ihrem Azure AD-Verzeichnis vorhanden ist, können Sie sie auf der Seite **Azure AD-Apps hinzufügen** auswählen, um sie Ihrem Dev Center-Konto hinzuzufügen. Andernfalls können Sie eine neue Azure AD-Anwendung auf der Seite **Azure AD-Apps hinzufügen** erstellen. Weitere Einzelheiten finden Sie im Abschnitt zum Verwalten von Azure AD-Anwendungen unter [Verwalten von Kontobenutzern](https://msdn.microsoft.com/library/windows/apps/mt489008).

3.  Wechseln Sie zurück zur Seite **Benutzer verwalten**, klicken Sie auf den Namen Ihrer Azure AD-Anwendung, um die Anwendungseinstellungen aufzurufen, und klicken Sie auf **Add new key**. Kopieren Sie auf dem folgenden Bildschirm die Werte für **Client-ID** und **Schlüssel**. Weitere Einzelheiten finden Sie im Abschnitt zum Verwalten von Azure AD-Anwendungen unter [Verwalten von Kontobenutzern](https://msdn.microsoft.com/library/windows/apps/mt489008). Sie benötigen die Client-ID und den Schlüssel zum Anfordern eines Azure AD-Zugriffstokens, das beim Aufrufen der Windows Store-Analyse-API verwendet wird. Nach dem Verlassen der Seite können Sie nicht mehr auf diese Informationen zugreifen.


### Abrufen eines Azure AD-Zugriffstokens

Nachdem Sie Ihrem Dev Center-Konto eine Azure AD-Anwendung zugeordnet und die Client-ID und den Schlüssel für die Anwendung abgerufen haben, können Sie mithilfe dieser Informationen ein Azure AD-Zugriffstoken abrufen. Sie benötigen ein Zugriffstoken, bevor Sie eine der Methoden in der Windows Store-Analyse-API aufrufen können.

Befolgen Sie zum Abrufen des Zugriffstokens die Anweisungen unter [Aufrufe zwischen Diensten mithilfe von Clientanmeldeinformationen](https://msdn.microsoft.com/library/azure/dn645543.aspx), um eine HTTP-POST-Anforderung an den nachfolgenden Azure AD-Endpunkt zu senden.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

-   Melden Sie sich zum Abrufen Ihrer Mandanten-ID beim [Azure-Verwaltungsportal](http://manage.windowsazure.com/) an, navigieren Sie zu **Active Directory**, und klicken Sie auf das Verzeichnis, das Sie mit Ihrem Dev Center-Konto verknüpft haben. Die Mandanten-ID für dieses Verzeichnis ist in die URL für diese Seite eingebettet; dies wird in der Zeichenfolge *your\_tenant\_ID* im untenstehenden Beispiel veranschaulicht.

  ```
  https://manage.windowsazure.com/@<your_tenant_name>#Workspaces/ActiveDirectoryExtension/Directory/<your_tenant_ID>/directoryQuickStart
  ```

-   Geben Sie für die Parameter *Client\_id* und *Client\_secret* die Client-ID und den Schlüssel für die Anwendung an, die Sie zuvor aus dem Dev Center abgerufen haben.
-   Geben Sie für den Parameter *resource* den folgenden URI an: ```https://manage.devcenter.microsoft.com```.


### Aufrufen der Windows Store-Analyse-API

Nachdem Sie ein Azure AD-Zugriffstoken abgerufen haben, können Sie die Windows Store-Analyse-API aufrufen. Informationen zur Syntax der einzelnen Methoden finden Sie in den folgenden Artikeln: Sie müssen das Zugriffstoken an den **Authorization**-Header der einzelnen Methoden übergeben.

-   [Abrufen von App-Käufen](get-app-acquisitions.md)
-   [Abrufen von IAP-Käufen](get-in-app-acquisitions.md)
-   [Abrufen von Fehlerberichtsdaten](get-error-reporting-data.md)
-   [Abrufen von App-Bewertungen](get-app-ratings.md)
-   [Abrufen von App-Rezensionen](get-app-reviews.md)

## Codebeispiel


Im folgenden Codebeispiel wird veranschaulicht, wie Sie ein Azure AD-Zugriffstoken abrufen und die Windows Store-Analyse-API aus einer C#-Konsolen-App aufrufen. Wenn Sie dieses Codebeispiel verwenden möchten, weisen Sie die Variablen *TenantId*, *ClientId*, *ClientSecret* und *AppID* den entsprechenden Werten für Ihr Szenario zu. In diesem Beispiel wird das [Json.NET-Paket](http://www.newtonsoft.com/json) von Newtonsoft benötigt, um die von der Windows Store-Analyse-API zurückgegebenen JSON-Daten zu deserialisieren.

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

            // This is your app's product ID. This ID is embedded in the app's listing link
            // on the App identity page of the Dev Center dashboard.
            string appID = "<your app's product ID>";

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

            //// Get IAP acquisitions
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
* [Abrufen von IAP-Käufen](get-in-app-acquisitions.md)
* [Abrufen von Fehlerberichtsdaten](get-error-reporting-data.md)
* [Abrufen von App-Bewertungen](get-app-ratings.md)
* [Abrufen von App-Rezensionen](get-app-reviews.md)
 


<!--HONumber=Mar16_HO3-->


