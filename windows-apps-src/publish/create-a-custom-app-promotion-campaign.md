---
author: jnHs
Description: "Neben dem Erstellen einer Anzeigenkampagne für Ihre App, die in Windows-Apps ausgeführt wird, können Sie Ihre App auch über andere Kanäle bewerben."
title: "Erstellen einer benutzerdefinierten Werbekampagne für Apps"
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.sourcegitcommit: 98b9bca2528c041d2fdfc6a0adead321737932b4
ms.openlocfilehash: b6fa63727ac636d37c52e2f3247c52efb1f8b763

---

# Erstellen einer benutzerdefinierten Werbekampagne für Apps



Neben dem Erstellen einer [Anzeigenkampagne für Ihre App](create-an-ad-campaign-for-your-app.md), die in Windows-Apps ausgeführt wird, können Sie Ihre App auch über andere Kanäle bewerben. Beispielsweise können Sie Ihre App mit einem Drittanbieter für App-Marketing bewerben oder Links zu Ihrer App in sozialen Netzwerken bereitstellen. Diese Aktivitäten werden als *benutzerdefinierte Kampagnen* bezeichnet.

Wenn Sie benutzerdefinierte Kampagnen für Ihre App ausführen, können Sie die relative Leistung der einzelnen Kampagnen nachverfolgen, indem Sie für jede benutzerdefinierte Kampagne eine andere Windows Store-App-URL mit jeweils unterschiedlichen Kampagnen-IDs erstellen. Wenn ein Kunde, der Windows10 ausführt, auf eine URL mit einer Kampagnen-ID klickt, ordnet Microsoft den Klick der entsprechenden benutzerdefinierten Kampagne zu und stellt Ihnen diese Daten zur Verfügung.

Es gibt im Wesentlichen zwei Typen von Daten zu benutzerdefinierten Kampagnen: Seitenaufrufe für Ihre App und *Konvertierungen*. Als Konvertierung wird eine App-Installation bezeichnet, die daraus resultiert, dass ein Kunde von einer über eine benutzerdefinierte Kampagne beworbenen URL aus auf die Windows Store-Seite für Ihre App klickt. Weitere Informationen zu Konvertierungen finden Sie unter [Wann gelten App-Installationen als Konvertierungen?](#understanding-how-app-installs-qualify-as-conversions) in diesem Thema.

Sie können Leistungsdaten einer benutzerdefinierten Kampagne für Ihre App auf folgende Arten abrufen:

-   Wenn es sich bei Ihrer App um eine App für die universelle Windows-Plattform (UWP) handelt, kann sie mit der [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445)-Methode programmgesteuert die benutzerdefinierte Kampagnen-ID abrufen, die zu einer Konvertierung geführt hat.
-   Sie können Seitenaufrufe und Konvertierungen für Ihre App oder Ihr IAP aus dem Bericht [Kanäle und Konvertierungen](channels-and-conversions-report.md) im Dev Center-Dashboard abrufen.

> 
            **Wichtig**  Diese Daten werden nur für Kunden nachverfolgt, die Windows10 ausführen. Kunden, die andere Betriebssysteme verwenden, können trotzdem den Link zu Ihrem App-Eintrag aufrufen, es werden jedoch keine Daten über die Aktivitäten dieser Kunden eingeschlossen.

 

## Beispielszenario für eine benutzerdefinierte Kampagne


Stellen Sie sich vor, ein Spieleentwickler hat die Entwicklung eines neuen Spiels abgeschlossen und möchte dafür bei Spielern seiner bereits vorhandenen Spiele werben. Er veröffentlicht die Ankündigung des neuen Spiels auf seiner Facebook-Seite, zusammen mit einem Link zur WindowsStore-Seite für das Spiel. Viele Spieler folgen ihm auch auf Twitter, darum twittert er die Ankündigung mit dem Link zur Windows Store-Seite des Spiels auch dort.

Um den Erfolg der einzelnen Werbekanäle nachzuverfolgen, erstellt der Entwickler zwei Varianten der URL für die Windows Store-Seite des Spiels:

-   Die URL, die er auf seiner Facebook-Seite postet, enthält die benutzerdefinierte Kampagnen-ID `my-facebook-campaign`.
-   Die URL, die er auf Twitter postet, enthält die benutzerdefinierte Kampagnen-ID `my-twitter-campaign`.

Wenn seine Facebook- und Twitter-Follower auf die URLs klicken, verfolgt Microsoft jeden Klick und ordnet ihn der entsprechenden benutzerdefinierten Kampagne zu. Nachfolgende qualifizierende Käufe des Spiels und Käufe von In-App-Produkten (IAP) werden der benutzerdefinierten Kampagne zugeordnet und als Konvertierungen gemeldet.

## Wann gelten App-Installationen als Konvertierungen?


Als *Konvertierung* wird eine App-Installation bezeichnet, die daraus resultiert, dass ein Kunde von einer über eine benutzerdefinierte Kampagne beworbenen URL aus auf die Windows Store-Seite für Ihre App klickt. Für die Qualifizierung als Konvertierung für den Bericht [Kanäle und Konvertierungen](channels-and-conversions-report.md) im Dev Center-Dashboard und für die Qualifizierung als Konvertierung für [programmgesteuertes Abrufen der Kampagnen-ID](#programmatically) gelten verschiedene Bedingungen.

Um als Konvertierung für den Bericht [Kanäle und Konvertierungen](channels-and-conversions-report.md) zu gelten, müssen die folgenden Bedingungen erfüllt sein:

-   Ein Kunde mit einem bekannten Microsoft-Konto klickt auf eine App-URL, die eine benutzerdefinierte Kampagnen-ID enthält, und wird auf die Windows Store-Seite für die App weitergeleitet.
-   Der gleiche Kunde (identifiziert mit demselben Microsoft-Konto) installiert die App innerhalb von 24 Stunden, nachdem er auf die Windows Store-URL mit der benutzerdefinierten Kampagnen-ID geklickt hat. Dies gilt als eine Konvertierung, auch wenn der Kunde die App auf einem anderen Computer oder Gerät als auf dem installiert, auf dem auf die Windows Store-URL mit der benutzerdefinierten Kampagnen-ID geklickt wurde.
    > 
            **Hinweis**  Für App-Installationen, die als Konvertierungen für eine benutzerdefinierte Kampagne gezählt werden, werden IAP-Einkäufe in der App ebenfalls als Konvertierungen für dieselbe benutzerdefinierte Kampagne gezählt.

     

Damit eine App-Installation beim programmgesteuerten Abrufen der Kampagnen-ID der App als Konvertierung gilt, müssen die folgenden Bedingungen erfüllt sein:

-   Ein Kunde mit einem bekannten Microsoft-Konto klickt auf eine App-URL, die eine benutzerdefinierte Kampagnen-ID enthält, und wird auf die Windows Store-Seite für die App weitergeleitet.
-   Der Kunde installiert die App während desselben Aufrufs der Windows Store-Seite, nachdem er auf die URL geklickt hat. Wenn der Benutzer die Seite verlässt und dann innerhalb von 24 Stunden (entweder auf demselben Computer oder Gerät oder auf einem anderen Computer oder Gerät) zu der Seite zurückkehrt und die App installiert, zählt dies für den Bericht [Kanäle und Konvertierung](channels-and-conversions-report.md) als Konvertierung, aber nicht beim programmgesteuerten Abruf der Kampagnen-ID.

## Einbetten einer benutzerdefinierten Kampagnen-ID in die URL der Windows Store-Seite Ihrer App


So erstellen Sie eine Windows Store-Seiten-URL für Ihre App mit einer benutzerdefinierten Kampagnen-ID

1.  Erstellen Sie eine ID-Zeichenfolge für Ihre benutzerdefinierte Kampagne. Diese Zeichenfolge kann bis zu 100 Zeichen enthalten. Es wird jedoch empfohlen, kurze, leicht erkennbare Kampagnen-IDs zu definieren.
2.  Rufen Sie die Windows Store-Seiten-URL für Ihre App im HTML- oder Protokollformat ab. Die HTML-Format-URL ist auf der Seite [**App-Identität** im Dev Center-Dashboard](link-to-your-app.md) verfügbar.
    -   Verwenden Sie das HTTP-Format, wenn Sie möchten, dass Kunden in einem Browser auf die Windows Store-Seite Ihrer App navigieren (diese URL startet auch die Windows Store-App mit Ihrem App-Eintrag, wenn die Windows Store-App installiert ist). Diese URL hat das Format **`https://www.microsoft.com/store/apps/*your app name*/*your app ID*`**. Die HTTP-URL für Skype lautet beispielsweise `https://www.microsoft.com/store/apps/skype/9wzdncrfj364`.
        > 
            **Hinweis**  URLs im HTTP-Format können zum Navigieren zum Windows Store in einem Browser auf Computern und Tablets unter Windows 7 und neueren Versionen sowie auf Smartphones mit mindestens Windows Phone 8 verwendet werden.
-   Verwenden Sie das Protokollformat, wenn Sie Ihre App in anderen Windows-Apps bewerben, die auf einem Gerät oder Computer mit installierter WindowsStore-App ausgeführt werden, und Sie möchten, dass Kunden die Seite Ihrer App in der WindowsStore-App öffnen. Diese URL hat das Format **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**. Die Protokoll-URL für Skype lautet beispielsweise `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`.
3.  Fügen Sie am Ende der URL für Ihre App die folgende Zeichenfolge an:
    -   Fügen Sie an eine HTTP-Format-URL **`?cid=*my custom campaign ID*`** an. Wenn Skype beispielsweise eine Kampagnen-ID mit dem Wert **custom\_campaign** einführt, würde die neue HTTP-URL einschließlich der Kampagnen-ID folgendermaßen lauten: `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`.
    -   Fügen Sie für eine URL im Protokoll-Format **`&cid=*my custom campaign ID*`** an. Wenn Skype beispielsweise eine Kampagnen-ID mit dem Wert **custom\_campaign** einführt, würde die neue Protokoll-URL einschließlich der Kampagnen-ID folgendermaßen lauten: `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`.

## Programmgesteuertes Abrufen der benutzerdefinierten Kampagnen-ID für eine App


Wenn es sich bei Ihrer App um eine UWP-App handelt, können Sie die Ihrer App zugeordnete benutzerdefinierte Kampagnen-ID programmgesteuert mit der [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445)-Methode abrufen. Diese Methode ermöglicht viele Analysen und Monetarisierungsszenarien. Beispielsweise können Sie feststellen, ob der aktuelle Benutzer Ihre App erworben hat, nachdem er sie über Ihre Facebook-Kampagne entdeckt hat. Dann können Sie die App-Oberfläche entsprechend anpassen. Oder wenn Sie einen Drittanbieter für App-Marketing verwenden, können Sie Daten zurück an den Anbieter senden.

> 
            **Hinweis** Die [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445)-Methode gibt nur dann eine Kampagnen-ID-Zeichenfolge zurück, wenn der Kunde auf die URL mit der eingebetteten Kampagnen-ID klickt, auf die WindowsStore-Seite für Ihre App weitergeleitet wird und dann die App ohne Verlassen der Seite installiert. Wenn der Benutzer die Seite verlässt und dann später zurückkehrt und die App installiert, gilt dies bei der Verwendung von **GetAppPurchaseCampaignIdAsync** nicht als Konvertierung. Weitere Informationen finden Sie unter [Grundlegendes zu Konvertierungen](#conversions) in diesem Thema.

 

Im folgenden Beispiel wird veranschaulicht, wie mit der [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445)-Methode die benutzerdefinierte Kampagnen-ID für die App abgerufen wird. Wenn die App nicht im Rahmen einer erfolgreichen Konvertierung installiert wurde, gibt diese Methode eine leere Zeichenfolge zurück.

``` csharp
string campaignId = await CurrentApp.GetAppPurchaseCampaignIdAsync();
```

``` cpp
HString campaignId;
HRESULT hr = CurrentApp::GetAppPurchaseCampaignIdAsync(campaignId.GetAddressOf());
```

Die [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445)-Methode greift auf Daten aus dem Windows Store zu. Befolgen Sie bei Verwendung dieser Methode folgende Richtlinien:

-   Umschließen Sie diesen Methodenaufruf mit einem asynchronen Vorgang, um den Abschluss des Aufrufs zu ermöglichen.
-   Wenn Ihre App noch nicht im WindowsStore veröffentlicht wurde und Sie Ihre benutzerdefinierte Kampagne testen, verwenden Sie die [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034)-Methode der [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)-Klasse anstelle der [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)-Klasse. Halten Sie sich dann an folgende Richtlinien:
    -   Fügen Sie der Datei „WindowsStoreProxy.xml“ ein **AppPurchaseCampaignId**-Element hinzu, wie im folgenden Beispiel gezeigt. Weisen Sie der benutzerdefinierten Kampagnen-ID den Elementwert hinzu. Wenn Sie die App ausführen, gibt die [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034)-Methode immer diesen Wert zurück. Weitere Informationen zur Datei „WindowsStoreProxy.xml“ finden Sie in der Dokumentation zu [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766).

    ```        XML
    <CurrentApp>
        ....
        <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
    </CurrentApp>
    ```
    
    -   Um bequem zwischen der Verwendung von [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) und [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) zu wechseln, wird empfohlen, dem Code folgende Anweisungen hinzuzufügen, um einen **Store**-Alias zu definieren, und anschließend Ihre [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034)-Aufrufe mit dem **Store**-Alias zu qualifizieren.

    ```        CSharp
    #if DEBUG
            using Store = Windows.ApplicationMode.Store.CurrentAppSimulator;
            #else
            using Store = Windows.ApplicationMode.Store.CurrentApp;
            #endif   
    ```

## Testen der benutzerdefinierten Kampagne


Bevor Sie eine URL für eine benutzerdefinierte Kampagnen bewerben, empfehlen wir, Ihre benutzerdefinierte Kampagne folgendermaßen zu testen:

1.  Melden Sie sich bei Ihrem Microsoft-Konto auf dem Computer oder Gerät an, das Sie zum Testen verwenden.
2.  Klicken Sie auf die URL für Ihre benutzerdefinierte Kampagne. Stellen Sie sicher, dass Ihre App-Seite korrekt im Windows Store geladen wird, und schließen Sie dann die Windows Store-App bzw. die Browserseite.
3.  Klicken Sie noch einige weitere Male auf die URL, und schließen Sie nach jedem Besuch der App-Seite die Windows Store-App oder die Browserseite. Installieren Sie bei einem der Besuche Ihrer App-Seite die App, um eine Konvertierung zu generieren. Zählen Sie, wie oft Sie insgesamt auf die URL geklickt haben.
4.  Wenn Sie die URL für die benutzerdefinierte Kampagne in Ihrer App programmgesteuert abrufen, testen Sie diesen Vorgang mit der [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034)-Methode der [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)-Klasse anstelle der [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)-Klasse.

 

 







<!--HONumber=Jun16_HO4-->


