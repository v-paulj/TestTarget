---
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: Wenn Sie über einen Katalog mit Apps und In-App-Produkten (IAPs) verfügen, können Sie mithilfe der Windows Store-Sammlungs-API und der Windows Store-Einkaufs-API Besitzerinformationen zu diesen Produkten aus Ihren Diensten abrufen.
title: Anzeigen von Produkten und Gewähren von Produktansprüchen aus einem Dienst
---

# Anzeigen von Produkten und Gewähren von Produktansprüchen aus einem Dienst


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Wenn Sie über einen Katalog mit Apps und In-App-Produkten (IAPs) verfügen, können Sie mithilfe der *Windows Store-Sammlungs-API* und der *Windows Store-Einkaufs-API* Besitzerinformationen zu diesen Produkten aus Ihren Diensten abrufen.

Die APIs bestehen aus REST-Methoden, die für Entwickler mit IAP-Katalogen konzipiert sind, die von plattformübergreifenden Diensten unterstützt werden. Diese APIs bieten folgende Möglichkeiten:

-   Windows Store-Sammlungs-API: Mit dieser API können Sie Apps und IAPs im Besitz eines bestimmten Benutzers abfragen oder ein Verbrauchsprodukt als erfüllt melden.
-   Windows Store-Einkaufs-API: Mit dieser API können Sie einem bestimmten Benutzer kostenlose Apps oder IAPs gewähren.

## Verwenden der Windows Store-Sammlungs-API und Windows Store-Einkaufs-API


Die Windows Store-Sammlungs-API und -Einkaufs-API nutzen die Azure Active Directory (Azure AD)-Authentifizierung, um auf Besitzerinformationen von Kunden zuzugreifen. Bevor Sie diese APIs aufrufen können, müssen Sie im Windows Dev Center-Dashboard Azure AD-Metadaten auf Ihre Anwendung anwenden und mehrere erforderliche Zugriffstoken und -schlüssel generieren. Dazu müssen folgende Schritte ausgeführt werden:

1.  [Konfigurieren Sie eine Webanwendung in Azure AD](#step-1:-configure-a-web-application-in-azure-ad). Diese Anwendung stellt Ihre plattformübergreifenden Dienste im Kontext von Azure AD dar.
2.  [Ordnen Sie Ihre Azure AD-Client-ID im Windows Dev Center-Dashboard Ihrer Anwendung zu](#step-2:-associate-your-azure-ad-client-id-with-your-application-in-the-windows-dev-denter-dashboard).
3.  In Ihrem Dienst: [Generieren Sie Azure AD-Zugriffstoken](#step-3:-retrieve-access-tokens-from-azure-ad), die Ihre Herausgeberidentität darstellen.
4.  Im clientseitigen Code in Ihrer Windows-App: [Generieren Sie einen Windows Store-ID-Schlüssel](#step-4:-generate-a-windows-store-id-key-from-client-side-code-in-your-app), der die Identität des aktuellen Benutzers darstellt, und übergeben Sie den Windows Store-ID-Schlüssel zurück an Ihren Dienst.
5.  Sobald Sie über das erforderliche Azure AD-Zugriffstoken und den Windows Store-ID-Schlüssel verfügen, [rufen Sie die Windows Store-Sammlungs-API oder -Einkaufs-API aus Ihrem Dienst auf](#step-5:-call-the-windows-store-collection-api-or-purchase-api-from-your-service).

Die folgenden Abschnitte enthalten weitere Details zu den einzelnen Schritten.

### Schritt 1: Konfigurieren einer Webanwendung in Azure AD

1.  Führen Sie die Schritte unter [Integrieren von Anwendungen in Azure Active Directory](http://go.microsoft.com/fwlink/?LinkId=722502) aus, um Azure AD eine Webanwendung hinzuzufügen.
    **Hinweis**  Wählen Sie auf der Seite **Erzählen Sie uns von Ihrer Anwendung** die Option **Webanwendung- und/oder Web-API** aus. Dies ist erforderlich, damit Sie für Ihre Anwendung einen Schlüssel (auch *geheimer Clientschlüssel* genannt) erhalten. Zum Aufrufen der Windows Store-Sammlungs-API oder -Einkaufs-API müssen Sie einen geheimen Clientschlüssel angeben, wenn Sie in einem späteren Schritt ein Zugriffstoken von Azure AD anfordern.
2.  Navigieren Sie im [Azure-Verwaltungsportal](http://manage.windowsazure.com/) zu **Active Directory**. Wählen Sie Ihr Verzeichnis aus, klicken Sie oben auf die Registerkarte **Anwendungen**, und wählen Sie Ihre Anwendung aus.
3.  Klicken Sie auf die Registerkarte **Konfigurieren**. Ermitteln Sie auf dieser Registerkarte die Client-ID für Ihre Anwendung, und fordern Sie einen Schlüssel an. Dieser Schlüssel wird in den weiteren Schritten als *geheimer Clientschlüssel* bezeichnet.
4.  Klicken Sie am unteren Bildschirmrand auf **Manifest verwalten**. Laden Sie Ihr Azure AD-Anwendungsmanifest herunter, und ersetzen Sie den Abschnitt `"identifierUris"` durch folgenden Text:

    ```json
    "identifierUris" : [                                
            "https://onestore.microsoft.com",
            "https://onestore.microsoft.com/b2b/keys/create/collections",
            "https://onestore.microsoft.com/b2b/keys/create/purchase"
        ],
    ```

    Diese Zeichenfolgen stellen die von der Anwendung unterstützten Zielgruppen dar. In einem späteren Schritt erstellen Sie Azure AD-Zugriffstoken, die den einzelnen Zielgruppenwerten zugeordnet werden. Weitere Informationen zum Herunterladen Ihres Anwendungsmanifests finden Sie unter [Grundlegendes zum Azure Active Directory-Anwendungsmanifest]( http://go.microsoft.com/fwlink/?LinkId=722500).

5.  Speichern Sie Ihr Anwendungsmanifest, und laden Sie es im [Azure-Verwaltungsportal](http://manage.windowsazure.com/) für Ihre Anwendung hoch.

### Schritt 2: Zuordnen Ihrer Azure AD-Client-ID zu Ihrer Anwendung im Windows Dev Center-Dashboard

Die Windows Store-Sammlungs-API und -Einkaufs-API ermöglichen den Zugriff auf die Besitzerinformationen eines Benutzers nur für Apps und IAPs, die Sie Ihrer Azure AD-Client-ID zugeordnet haben.

1.  Melden Sie sich beim [Windows Dev Center-Dashboard](https://dev.windows.com/overview) an, und wählen Sie Ihre App aus.
2.  Geben Sie unter **Dienste** &gt; Seite **Produktsammlungen und Einkäufe** Ihre Azure AD-Client-ID in eines der verfügbaren Felder ein.

### Schritt 3: Abrufen von Zugriffstoken von Azure AD

Bevor Sie einen Windows Store-ID-Schlüssel abrufen oder die Windows Store-Sammlungs-API oder -Einkaufs-API aufrufen können, muss Ihr Dienst drei Azure AD-Zugriffstoken anfordern, die Ihre Herausgeberidentität darstellen. Jedes dieser Zugriffstoken ist einem anderen Zielgruppen-URI zugeordnet, und jedes Token wird mit einem anderen API-Aufruf verwendet. Jedes Token ist 60 Minuten gültig und kann nach Ablauf aktualisiert werden.

Verwenden Sie zum Erstellen der Zugriffstoken die OAuth 2.0-API in Ihrem Dienst. Eine entsprechende Anleitung finden Sie unter [Aufrufe zwischen Diensten mithilfe von Clientanmeldeinformationen](https://msdn.microsoft.com/library/azure/dn645543.aspx). Geben Sie für jedes Token die folgenden Parameterdaten an:

-   Geben Sie für den *client\_id*-Parameter und den *client\_secret*-Parameter die Client-ID und den geheimen Clientschlüssel Ihrer Anwendung aus dem [Azure-Verwaltungsportal](http://manage.windowsazure.com/) an. Beide Parameter sind erforderlich, um ein Zugriffstoken mit der Authentifizierungsebene zu generieren, die für die Windows Store-Sammlungs-API oder -Einkaufs-API benötigt wird.
-   Geben Sie für den *resource*-Parameter einen der folgenden App-ID-URIs an. (Hierbei handelt es sich um die gleichen URIs, die Sie zuvor im Abschnitt `"identifierUris"` des Anwendungsmanifests hinzugefügt haben). Am Ende dieses Prozesses verfügen Sie über drei Zugriffstoken, denen jeweils einer dieser App-ID-URIs zugeordnet ist:
    -   **https://onestore.microsoft.com/b2b/keys/create/collections**: In einem späteren Schritt verwenden Sie das mit diesem URI erstellte Zugriffstoken zum Anfordern eines Windows Store-ID-Schlüssels, den Sie mit der Windows Store-Sammlungs-API verwenden können.
    -   **https://onestore.microsoft.com/b2b/keys/create/purchase**: In einem späteren Schritt verwenden Sie das mit diesem URI erstellte Zugriffstoken zum Anfordern eines Windows Store-ID-Schlüssels, den Sie mit der Windows Store-Einkaufs-API verwenden können.
    -   **https://onestore.microsoft.com**: In einem späteren Schritt verwenden Sie das mit diesem URI erstellte Zugriffstoken in direkten Aufrufen der Windows Store-Sammlungs-API oder -Einkaufs-API.

    **Wichtig**  Verwenden Sie die Zielgruppe **https://onestore.microsoft.com** nur mit Zugriffstoken, die sicher in Ihrem Dienst gespeichert sind. Durch das Verfügbarmachen von Zugriffstoken mit dieser Zielgruppe außerhalb Ihres Diensts kann er anfällig für Replay-Angriffe werden.

Weitere Informationen zur Struktur eines Zugriffstokens finden Sie unter [Unterstützte Token- und Anspruchstypen](http://go.microsoft.com/fwlink/?LinkId=722501).

**Wichtig**  Azure AD-Zugriffstoken sollten nur im Kontext Ihres Diensts und nicht in Ihrer App erstellt werden. Ihr geheimer Clientschlüssel könnte gefährdet sein, wenn er an Ihre App gesendet wird.

 

### Schritt 4: Generieren eines Windows Store-ID-Schlüssels aus clientseitigem Code in Ihrer App

Bevor Sie die Windows Store-Sammlungs-API oder -Einkaufs-API aufrufen können, muss Ihr Dienst einen Windows Store-ID-Schlüssel abrufen. Hierbei handelt es sich um ein JSON Web Token (JWT), das die Identität des Benutzers darstellt, auf dessen Produktbesitzerinformationen Sie zugreifen möchten. Weitere Informationen zu den Ansprüchen in diesem Schlüssel finden Sie unter [Ansprüche in einem Windows Store-ID-Schlüssel](#windowsstorekey).

Die einzige Möglichkeit zum Abrufen eines Windows Store-ID-Schlüssels besteht momentan darin, eine UWP (Universelle Windows-Plattform)-API aus clientseitigem Code in Ihrer App aufzurufen, um die Identität des derzeit beim Windows Store angemeldeten Benutzers abzurufen. So generieren Sie einen Windows Store-ID-Schlüssel

1.  Übergeben Sie eines der folgenden Zugriffstoken von Ihrem Dienst an Ihre Client-App:
    -   Zum Abrufen eines Windows Store-ID-Schlüssels, den Sie mit der Windows Store-Sammlungs-API verwenden können, übergeben Sie das Azure AD-Zugriffstoken, das Sie mit dem Zielgruppen-URI **https://onestore.microsoft.com/b2b/keys/create/collections** erstellt haben.
    -   Zum Abrufen eines Windows Store-ID-Schlüssels, den Sie mit der Windows Store-Einkaufs-API verwenden können, übergeben Sie das Azure AD-Zugriffstoken, das Sie mit dem Zielgruppen-URI **https://onestore.microsoft.com/b2b/keys/create/purchase** erstellt haben.

2.  Rufen Sie in Ihrem App-Code eine der folgenden Methoden der [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)-Klasse auf, um einen Windows Store-ID-Schlüssel abzurufen.

    -   [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674): Rufen Sie diese Methode auf, wenn Sie die Windows Store-Sammlungs-API verwenden möchten.
    -   [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675): Rufen Sie diese Methode auf, wenn Sie die Windows Store-Einkaufs-API verwenden möchten.

    Übergeben Sie für beide Methoden Ihr Azure AD-Zugriffstoken an den *serviceTicket*-Parameter. Sie können optional eine ID an den *publisherUserId*-Parameter übergeben, der den aktuellen Benutzer im Kontext Ihrer Dienste identifiziert. Wenn Sie Benutzer-IDs für Ihre Dienste verwalten, können Sie diesen Parameter verwenden, um die Benutzer-IDs mit den Aufrufen an die Windows Store-Sammlungs-API oder -Einkaufs-API zu korrelieren.

3.  Übergeben Sie den von Ihrer App erfolgreich abgerufenen Windows Store-ID-Schlüssel zurück an Ihren Dienst.

**Hinweis**  Jeder Windows Store-ID-Schlüssel ist 90 Tage gültig. Nach dem Ablauf eines Schlüssels können Sie [den Schlüssel verlängern](renew-a-windows-store-id-key.md). Wir empfehlen, Windows Store-ID-Schlüssel zu verlängern, anstatt neue zu erstellen.

 

### Schritt 5: Aufrufen der Windows Store-Sammlungs-API oder -Einkaufs-API aus Ihrem Dienst

Wenn Ihr Dienst über einen Windows Store-ID-Schlüssel verfügt, der den Zugriff auf die Produktbesitzerinformationen eines bestimmten Benutzers ermöglicht, kann der Dienst die Windows Store-Sammlungs-API oder -Einkaufs-API aufrufen. Verwenden Sie die passenden Anweisungen für Ihr Szenario:

-   [Produktabfrage](query-for-products.md)
-   [Melden von Verbrauchsprodukten als erfüllt](report-consumable-products-as-fulfilled.md)
-   [Gewähren kostenloser Produkte](grant-free-products.md)

Übergeben Sie bei jedem Szenario die folgenden Informationen an die API:

-   Das Azure AD-Zugriffstoken, das Sie zuvor mit dem Zielgruppen-URI **https://onestore.microsoft.com** erstellt haben. Dieses Token stellt die Herausgeberidentität dar. Übergeben Sie das Token im Anforderungsheader.
-   Den Windows Store-ID-Schlüssel, den Sie aus [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) oder [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675) in Ihrer App abgerufen haben. Dieser Schlüssel stellt die Identität des Benutzers dar, auf dessen Produktbesitzerinformationen Sie zugreifen möchten.

## Ansprüche in einem Windows Store-ID-Schlüssel


Ein Windows Store-ID-Schlüssel ist ein JSON Web Token (JWT), das die Identität des Benutzers darstellt, auf dessen Produktbesitzerinformationen Sie zugreifen möchten. Bei der Decodierung unter Verwendung von Base64 enthält ein Windows Store-ID-Schlüssel die folgenden Ansprüche.

| Anspruchsname                                                             | Beschreibung                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| iat                                                                    | Gibt die Ausstellungszeit des Schlüssels an. Mit diesem Anspruch kann das Alter des Tokens bestimmt werden. Dieser Wert wird als Epoche ausgedrückt.                                                                                                                                                                                                                                       |
| iss                                                                    | Gibt den Aussteller an. Der Wert ist identisch mit dem des *aud*-Anspruchs.                                                                                                                                                                                                                                                                                                                      |
| aud                                                                    | Gibt die Zielgruppe an. Muss einem der folgenden Werte entsprechen: **https://collections.mp.microsoft.com/v6.0/keys** oder **https://purchase.mp.microsoft.com/v6.0/keys**.                                                                                                                                                                                                                    |
| exp                                                                    | Gibt die Ablaufzeit an, nach der der Schlüssel nicht mehr für Verarbeitungsvorgänge akzeptiert wird, außer für die Verlängerung von Schlüsseln. Der Wert dieses Anspruchs wird als Epoche ausgedrückt.                                                                                                                                                                                               |
| nbf                                                                    | Gibt die Zeit an, zu der das Token für die Verarbeitung akzeptiert wird. Der Wert dieses Anspruchs wird als Epoche ausgedrückt.                                                                                                                                                                                                                                                             |
| http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId   | Die Client-ID, die den Entwickler angibt.                                                                                                                                                                                                                                                                                                                                            |
| http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload    | Eine opake (verschlüsselte und Base64-codierte) Nutzlast mit Informationen, die ausschließlich von Windows Store-Diensten verwendet werden sollen.                                                                                                                                                                                                                                                     |
| http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId     | Eine Benutzer-ID, die den aktuellen Benutzer im Kontext Ihrer Dienste angibt. Dies ist der gleiche Wert, den Sie beim Erstellen des Schlüssels an den optionalen *publisherUserId*-Parameter der [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674)-Methode oder [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675)-Methode übergeben. |
| http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri | Der URI, mit dem Sie den Schlüssel verlängern können.                                                                                                                                                                                                                                                                                                                                              |

 

Hier ein Beispiel für einen decodierten Windows Store-ID-Schlüsselheader.

```
{ 
    "typ":"JWT", 
    "alg":"RS256", 
    "x5t":"agA_pgJ7Twx_Ex2_rEeQ2o5fZ5g" 
} 
```

Hier ein Beispiel für einen decodierten Satz von Windows Store-ID-Schlüsselansprüchen.

```
{ 
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId": "1d5773695a3b44928227393bfef1e13d", 
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload": "ZdcOq0/N2rjytCRzCHSqnfczv3f0343wfSydx7hghfu0snWzMqyoAGy5DSJ5rMSsKoQFAccs1iNlwlGrX+/eIwh/VlUhLrncyP8c18mNAzAGK+lTAd2oiMQWRRAZxPwGrJrwiq2fTq5NOVDnQS9Za6/GdRjeiQrv6c0x+WNKxSQ7LV/uH1x+IEhYVtDu53GiXIwekltwaV6EkQGphYy7tbNsW2GqxgcoLLMUVOsQjI+FYBA3MdQpalV/aFN4UrJDkMWJBnmz3vrxBNGEApLWTS4Bd3cMswXsV9m+VhOEfnv+6PrL2jq8OZFoF3FUUpY8Fet2DfFr6xjZs3CBS1095J2yyNFWKBZxAXXNjn+zkvqqiVRjjkjNajhuaNKJk4MGHfk2rZiMy/aosyaEpCyncdisHVSx/S4JwIuxTnfnlY24vS0OXy7mFiZjjB8qL03cLsBXM4utCyXSIggb90GAx0+EFlVoJD7+ZKlm1M90xO/QSMDlrzFyuqcXXDBOnt7rPynPTrOZLVF+ODI5HhWEqArkVnc5MYnrZD06YEwClmTDkHQcxCvU+XUEvTbEk69qR2sfnuXV4cJRRWseUTfYoGyuxkQ2eWAAI1BXGxYECIaAnWF0W6ThweL5ZZDdadW9Ug5U3fZd4WxiDlB/EZ3aTy8kYXTW4Uo0adTkCmdLibw=", 
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId": "infusQMLaYCrgtC0d/SZWoPB4FqLEwHXgZFuMJ6TuTY=", 
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri": "https://collections.mp.microsoft.com/v6.0/b2b/keys/renew", 
    "iat": 1442395542, 
    "iss": "https://collections.mp.microsoft.com/v6.0/keys", 
    "aud": "https://collections.mp.microsoft.com/v6.0/keys", 
    "exp": 1450171541, 
    "nbf": 1442391941 
} 
```

## Verwandte Themen

* [Produktabfrage](query-for-products.md)
* [Melden von Verbrauchsprodukten als erfüllt](report-consumable-products-as-fulfilled.md)
* [Gewähren kostenloser Produkte](grant-free-products.md)
* [Verlängern eines Windows Store-ID-Schlüssels](renew-a-windows-store-id-key.md)
* [Integrieren von Anwendungen in Azure Active Directory](http://go.microsoft.com/fwlink/?LinkId=722502)
* [Grundlegendes zum Azure Active Directory-Anwendungsmanifest]( http://go.microsoft.com/fwlink/?LinkId=722500)
* [Unterstützte Token- und Anspruchstypen](http://go.microsoft.com/fwlink/?LinkId=722501)
 

 





<!--HONumber=Mar16_HO1-->


