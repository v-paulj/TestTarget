---
author: mcleanbyron
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: "Verwenden Sie die Windows Store-Übermittlungs-API, um Übermittlungen für Apps programmgesteuert zu erstellen und zu verwalten, die für Ihr Windows Dev Center-Konto registriert sind."
title: "Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten"
translationtype: Human Translation
ms.sourcegitcommit: 47e0ac11178af98589e75cc562631c6904b40da4
ms.openlocfilehash: 0a566dfee8f7fe08c06ce4963435a70c30b1650d

---

# Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten


Verwenden Sie die *Windows Store-Übermittlungs-API*, um Übermittlungen für Apps, Add-Ons (auch als In-App-Produkte oder IAPs bezeichnet) und Flight-Pakete für Ihr Windows Dev Center-Konto oder das Ihrer Organisation abzufragen und zu erstellen. Diese API ist hilfreich, wenn über Ihr Konto viele Apps oder Add-Ons verwaltet werden und Sie den Übermittlungsprozess für diese Ressourcen automatisieren und optimieren möchten. Diese API verwendet Azure Active Directory (Azure AD), um die Aufrufe von Ihrer App oder Ihrem Dienst zu authentifizieren.

Die folgenden Schritte beschreiben den gesamten Prozess der Verwendung der Windows Store-Übermittlungs-API:

1.  Stellen Sie sicher, dass Sie alle [Voraussetzungen](#prerequisites) erfüllt haben.
3.  Vor dem Aufrufen einer Methode in der Windows Store-Übermittlungs-API müssen Sie [ein AzureAD-Zugriffstoken anfordern](#obtain-an-azure-ad-access-token). Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60 Minuten lang verwenden, bevor es abläuft. Nach dem Ablauf des Tokens können Sie ein neues Token generieren.
4.  [Aufrufen der Windows Store-Übermittlungs-API](#call-the-windows-store-submission-api).



<span id="not_supported" />
>**Wichtig**

> * Diese API kann nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der API erhalten haben. Diese Berechtigung wird für Entwicklerkonten phasenweise aktiviert, und die Berechtigung ist zu diesem Zeitpunkt nicht für alle Konten aktiviert. Um früheren Zugriff anfordern, melden Sie sich beim Dev Center-Dashboard an, klicken Sie am unteren Rand des Dashboards auf **Feedback**, wählen Sie **Übermittlungs-API** für den Feedback-Bereich, und übermitteln Sie Ihre Anforderung. Sie erhalten eine E-Mail, wenn diese Berechtigung für Ihr Konto aktiviert ist.

> * Diese API kann nicht mit Apps oder Add-Ons mit bestimmten Features verwendet werden, die im Dev Center-Dashboard im August2016 eingeführt wurden, einschließlich (aber nicht beschränkt auf) erforderliche App-Updates und vom Store verwaltete Endverbraucher-Add-Ons. Bei Verwendung der Windows Store-Übermittlungs-API mit einer App oder einem Add-On, das eines dieser Features verwendet, gibt die API den Fehlercode409 zurück. In diesem Fall müssen Sie das Dashboard verwenden, um die Übermittlungen für die App bzw. das Add-On zu verwalten.

<span id="prerequisites" />
## Schritt1: Erfüllen der Voraussetzungen für die Verwendung der Windows Store-Übermittlungs-API

Stellen Sie sicher, dass Sie die folgenden Voraussetzungen erfüllt haben, bevor Sie mit dem Schreiben von Code zum Aufrufen der Windows Store-Übermittlungs-API beginnen:

* Sie (bzw. Ihre Organisation) müssen über ein Azure AD-Verzeichnis und die Berechtigung [Globaler Administrator](http://go.microsoft.com/fwlink/?LinkId=746654) für das Verzeichnis verfügen. Wenn Sie bereits mit Office 365oder anderen Unternehmensdiensten von Microsoft arbeiten, verfügen Sie schon über ein Azure AD-Verzeichnis. Andernfalls können Sie [in Dev Center ein neues Azure AD-Instanz](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users) ohne zusätzliche Kosten erstellen.

* Sie müssen [Ihrem Windows Dev Center-Konto eine Azure AD-Anwendung zuordnen](#associate-an-azure-ad-application-with-your-windows-dev-center-account) und Ihre Mandanten-ID, Client-ID und den Schlüssel abrufen. Sie benötigen diese Werte, um ein Azure AD-Zugriffstoken abzurufen, das Sie in Aufrufen von der Windows Store-Übermittlungs-API verwenden.

* Bereiten Sie Ihre App für die Verwendung mit der Windows Store-Übermittlungs-API vor:

  * Wenn Ihre App im Dev Center noch nicht vorhanden ist, [erstellen Sie Ihre App im Dev Center-Dashboard](https://msdn.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name). Sie können die Windows Store-Übermittlungs-API nicht zum Erstellen einer App im Dev Center verwenden. Sie müssen Ihre App mithilfe des Dashboards erstellen, und dann können Sie mithilfe der API auf der App zugreifen und Übermittlungen programmgesteuert erstellen. Sie können jedoch mithilfe der API Add-Ons und Flight-Pakete programmgesteuert erstellen, bevor Sie Übermittlungen für sie erstellen.

  * Bevor Sie eine Übermittlung für eine bestimmte App mit dieser API erstellen können, müssen Sie zunächst [eine Übermittlung für die App im Dev Center-Dashboard erstellen](https://msdn.microsoft.com/windows/uwp/publish/app-submissions), einschließlich der Beantwortung der Fragen zu den [Altersfreigaben](https://msdn.microsoft.com/windows/uwp/publish/age-ratings). Danach können Sie neue Übermittlungen für diese App mithilfe der API programmgesteuert erstellen. Sie müssen keine Add-On-Übermittlung oder Flight-Paketübermittlung vor der Verwendung der API für diese Arten von Übermittlungen erstellen.

  * Wenn Sie eine App-Übermittlung erstellen oder aktualisieren und ein App-Paket angeben müssen, [bereiten Sie das App-Paket vor](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements).

  * Wenn Sie eine App-Übermittlung erstellen oder aktualisieren und Screenshots oder Bilder für den Store-Eintrag angeben müssen, [bereiten Sie die App-Screenshots und -Bilder vor](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images).

  * Wenn Sie eine Add-On-Übermittlung erstellen oder aktualisieren und ein Symbol angeben müssen, [bereiten Sie das Symbol vor](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon).

<span id="associate-an-azure-ad-application-with-your-windows-dev-center-account" />
### Zuordnen einer Azure AD-Anwendung zu Ihrem Windows Dev Center-Konto

Vor der Verwendung der Windows Store-Übermittlungs-API müssen Sie Ihrem Dev Center-Konto eine Azure AD-Anwendung zuordnen, die Mandanten-ID und Client-ID für die Anwendung abrufen und einen Schlüssel generieren. Die Azure AD-Anwendung stellt die App oder den Dienst dar, aus denen Sie die Windows Store-Übermittlungs-API aufrufen möchten. Sie benötigen die Mandanten-ID, die Client-ID und den Schlüssel zum Abrufen eines Azure AD-Zugriffstokens, das Sie an die API übergeben.

>**Hinweis**&nbsp;&nbsp;Sie müssen diesen Schritt nur einmal ausführen. Sobald Sie über die Mandanten-ID, Client-ID und den Schlüssel verfügen, können Sie diese Daten jederzeit wiederverwenden, um ein neues Azure AD-Zugriffstoken zu erstellen.

1.  Rufen Sie in Dev Center die **Kontoeinstellungen** auf, klicken Sie auf **Benutzer verwalten**, und ordnen Sie das Dev Center-Konto Ihrer Organisation dem Azure AD-Verzeichnis Ihrer Organisation zu. Ausführliche Anweisungen finden Sie unter [Verwalten von Kontobenutzern](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users).

2.  Klicken Sie auf der Seite **Benutzer verwalten** auf **Azure AD-Apps hinzufügen**, und fügen Sie die AzureAD-Anwendung hinzu, die die App oder den Dienst darstellt, mit dem Sie auf Übermittlungen für Ihr Dev Center-Konto zugreifen. Weisen Sie ihr anschließend die Rolle **Manager** zu. Wenn diese Anwendung bereits in Ihrem AzureAD-Verzeichnis vorhanden ist, können Sie sie auf der Seite **Azure AD-Apps hinzufügen** auswählen, um sie Ihrem Dev Center-Konto hinzuzufügen. Andernfalls können Sie eine neue Azure AD-Anwendung auf der Seite **Azure AD-Apps hinzufügen** erstellen. Weitere Informationen finden Sie unter [Hinzufügen und Verwalten von Azure AD-Apps](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

3.  Wechseln Sie zurück zur Seite **Benutzer verwalten**, klicken Sie auf den Namen Ihrer Azure AD-Anwendung, um die Anwendungseinstellungen aufzurufen, und kopieren Sie die Werte unter **Mandanten-ID** und **Client-ID**.

4. Klicken Sie auf **Neuen Schlüssel hinzufügen**. Kopieren Sie auf dem folgenden Bildschirm den Wert unter **Schlüssel**. Nach dem Verlassen der Seite können Sie nicht mehr auf diese Informationen zugreifen. Weitere Informationen zum Verwalten von Schlüsseln finden Sie unter [Hinzufügen und Verwalten von Azure AD-Apps](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

<span id="obtain-an-azure-ad-access-token" />
## Schritt 2: Abrufen eines Azure AD-Zugriffstokens

Bevor Sie die Methoden in der Windows Store-Übermittlungs-API aufrufen, müssen Sie zuerst ein Azure AD-Zugriffstoken abrufen, das Sie für jede Methode in der API an den **Authorization**-Header übergeben. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60 Minuten lang verwenden, bevor es abläuft. Nachdem das Token abgelaufen ist, können Sie es aktualisieren, um es in weiteren Aufrufen an die API zu verwenden.

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

Geben Sie für die Parameter *tenant\_id*, *client\_id* und *client\_secret* die Mandanten-ID, die Client-ID und den Schlüssel für Ihre Anwendung an, die Sie im vorherigen Abschnitt aus Dev Center abgerufen haben. Für den Parameter *resource* müssen Sie den URI ```https://manage.devcenter.microsoft.com``` angeben.

Nachdem das Zugriffstoken abgelaufen ist, können Sie es aktualisieren, indem Sie [diese Anleitung](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens) befolgen.

<span id="call-the-windows-store-submission-api">
## Schritt3: Verwenden der Windows Store-Übermittlungs-API

Nachdem Sie ein AzureAD-Zugriffstoken abgerufen haben, können Sie Methoden in der WindowsStore-Übermittlungs-API aufrufen. Die API enthält viele Methoden, die in Szenarien für Apps, Add-Ons und Flight-Pakete gruppiert werden. Zum Erstellen oder Aktualisieren von Übermittlungen rufen Sie in der Regel mehrere Methoden in der Windows Store-Übermittlungs-API in einer bestimmten Reihenfolge auf. Informationen zu jedem Szenario und zur Syntax der einzelnen Methoden finden Sie in den Artikeln in der folgenden Tabelle.

>**Hinweis**&nbsp;&nbsp;Nach dem Abrufen eines Zugriffstokens haben Sie 60Minuten Zeit, um Methoden in der Windows Store-Übermittlungs-API aufzurufen, bevor es abläuft.

| Szenario       | Beschreibung                                                                 |
|---------------|----------------------------------------------------------------------|
| Apps |  Rufen Sie Daten für alle Apps ab, die für Ihr Windows Dev Center-Konto registriert sind, und erstellen Sie Übermittlungen für Apps. Weitere Informationen zu diesen Methoden finden Sie in den folgenden Artikeln: <ul><li>[Abrufen von App-Daten](get-app-data.md)</li><li>[Verwalten von App-Übermittlungen](manage-app-submissions.md)</li></ul> |
| Add-Ons | Rufen Sie Add-Ons für Ihre Apps ab, erstellen oder löschen Sie diese, und rufen Sie dann Übermittlungen für die Add-Ons ab, erstellen oder löschen Sie diese. Weitere Informationen zu diesen Methoden finden Sie in den folgenden Artikeln: <ul><li>[Verwalten von Add-Ons](manage-add-ons.md)</li><li>[Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md)</li></ul> |
| Flight-Pakete | Rufen Sie Flight-Pakete für Ihre Apps ab, erstellen oder löschen Sie diese, und rufen Sie dann Übermittlungen für die Flight-Pakete ab, erstellen oder löschen Sie diese. Weitere Informationen zu diesen Methoden finden Sie in den folgenden Artikeln: <ul><li>[Verwalten von Flight-Paketen](manage-flights.md)</li><li>[Verwalten von Flight-Paketübermittlungen](manage-flight-submissions.md)</li></ul> |

<span />

## Codebeispiele

Die folgenden Artikel enthalten ausführliche Codebeispiele, die zeigen, wie Sie die Windows Store-Übermittlungs-API in verschiedenen Programmiersprachen verwenden können:

* [C#-Codebeispiele](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java-Codebeispiele](java-code-examples-for-the-windows-store-submission-api.md)
* [Python-Codebeispiele](python-code-examples-for-the-windows-store-submission-api.md)

## Problembehandlung

| Problem      | Lösung                                          |
|---------------|---------------------------------------------|
| Nach dem Aufrufen der Windows Store-Übermittlungs-API über die PowerShell sind die Antwortdaten für die API beschädigt, wenn Sie diese aus dem JSON-Format in ein PowerShell-Objekt mit dem [ConvertFrom-Json](https://technet.microsoft.com/en-us/library/hh849898.aspx)-Cmdlet und zurück in das JSON-Format mit dem [ConvertTo-Json](https://technet.microsoft.com/en-us/library/hh849922.aspx)-Cmdlet konvertieren. |  Standardmäßig ist der Parameter *-Depth* für das [ConvertTo-Json](https://technet.microsoft.com/en-us/library/hh849922.aspx)-Cmdlet auf zwei Objektebenen festgelegt. Dies ist zu flach für die meisten JSON-Objekte, die von der Windows Store-Übermittlungs-API zurückgegeben werden. Legen Sie beim Aufrufen des [ConvertTo-Json](https://technet.microsoft.com/en-us/library/hh849922.aspx)-Cmdlets den Parameter *-Depth* auf eine größere Zahl fest, z.B. 20. |

## Zusätzliche Hilfe

Wenn Sie Fragen zur Windows Store-Übermittlungs-API haben oder Hilfe beim Verwalten der Übermittlungen mit dieser API benötigen, verwenden Sie die folgenden Ressourcen:

* Stellen Sie Ihre Fragen in unseren [Foren](https://social.msdn.microsoft.com/Forums/windowsapps/en-us/home?forum=wpsubmit).
* Besuchen Sie unsere [Supportseite](https://developer.microsoft.com/windows/support), und fordern Sie Supportunterstützung für das Dev Center-Dashboard an. Wenn Sie aufgefordert werden, einen Problemtyp und eine Kategorie auszuwählen, wählen Sie **App submission and certification** bzw. **Übermitteln einer App**.  

## Verwandte Themen

* [Abrufen von App-Daten](get-app-data.md)
* [Verwalten von App-Übermittlungen](manage-app-submissions.md)
* [Verwalten von Add-Ons](manage-add-ons.md)
* [Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md)
* [Verwalten von Flight-Paketen](manage-flights.md)
* [Verwalten von Flight-Paketübermittlungen](manage-flight-submissions.md)
 



<!--HONumber=Sep16_HO1-->


