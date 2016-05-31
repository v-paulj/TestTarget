---
author: jnHs
Description: Sie können für Ihre Apps detaillierte Analysen im Windows Dev Center-Dashboard anzeigen.
title: Analyse
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
---

# Analyse

Sie können für Ihre Apps detaillierte Analysen im Windows Dev Center-Dashboard anzeigen. Statistiken und Diagramme geben Aufschluss über den Erfolg Ihrer Apps, z. B. wie viele Kunden Sie erreichen, wie die Kunden Ihre App einsetzen und was die Kunden über die App denken. Außerdem finden Sie dort Informationen zur App-Integrität, Anzeigennutzung und vieles mehr. Zeigen Sie die Berichte im Dashboard an, oder [laden Sie die erforderlichen Berichte herunter](download-analytic-reports.md), um Ihre Daten offline zu analysieren. Wir stellen Ihnen auch verschiedene Möglichkeiten für den [Zugriff auf Analysedaten ohne das Dashboard](#no-dashboard) zur Verfügung.

> **Hinweis**
            &nbsp;&nbsp;Zusätzlich zu den Dashboardberichten können Sie auf einige Analysedaten auch programmgesteuert mithilfe der [Windows Store-REST-API für Analysen](../monetize/access-analytics-data-using-windows-store-services.md) zugreifen.

## Analysen für Ihre gesamten Apps


Ihre Seite „Dashboardübersicht“ enthält auch eine Rollupansicht zum Durchblättern aller Details zu Ihren Apps. Welche Statistikdaten auf der Übersichtsseite angezeigt werden, hängt von Ihren Apps ab.

Wenn Sie [Analyseberichte herunterladen](download-analytic-reports.md), haben Sie auch die Möglichkeit, Berichte über Ihre gesamten Apps herunterzuladen. Beachten Sie, dass Sie auf die Seite **Berichte herunterladen** im Abschnitt **Analysen** einer Ihrer Apps zugreifen müssen. Dies bedeutet jedoch nicht, dass Sie nur für diese bestimmte App Daten herunterladen können.

## Verfügbare Berichte für die einzelnen Apps


In diesem Abschnitt erhalten Sie Details zu den Informationen, die in den folgenden Berichten enthalten sind:

-   [Bericht „Käufe“](acquisitions-report.md)
-   [Bericht „Integrität“](health-report.md)
-   [Bericht „Bewertungen“](ratings-report.md)
-   [Bericht „Rezensionen“](reviews-report.md)
-   [Feedbackbericht](feedback-report.md)
-   [Bericht „Nutzung“](usage-report.md)
-   [Bericht „IAP-Käufe“](iap-acquisitions-report.md)
-   [Bericht „Anzeigenvermittlung“](ad-mediation-report.md)
-   [Bericht zur Anzeigenleistung](advertising-performance-report.md)
-   [Bericht „Anzeigen für die App-Installation“](app-install-ads-reports.md)
-   [Bericht zu Kanälen und Konvertierungen](channels-and-conversions-report.md)

> **Hinweis**
            &nbsp;&nbsp;Abhängig von den spezifischen Features und der Implementierung Ihrer App enthalten einige dieser Berichte möglicherweise keine Daten.

## Seitenfilter und Abschnittsfilter

Jeder Bericht enthält Filter, mit denen Sie einen Drilldown auf Ihre Daten ausführen können. Am oberen Seitenrand sehen Sie **Filter anwenden**. Mithilfe dieser Filter können Sie den Umfang aller auf der Seite angezeigten Diagramme und Informationen einschränken oder erweitern.

Innerhalb eines bestimmten Diagramms können auch einzelne Abschnittsfilter angezeigt werden. Durch diese werden die Daten eingeschränkt, die nur für dieses bestimmte Diagramm angezeigt werden.

Die spezifischen Filter variieren je nach Bericht. Die Themen in diesem Abschnitt erläutern, welche Filter verfügbar sind, und beschreiben weitere Daten, die Sie auf der Seite finden.

<span id="no-dashboard"/>
## Zugreifen auf Analysedaten ohne das Dev Center-Dashboard

Neben den Analyseberichten im Dashboard gibt es weitere Möglichkeiten für den Zugriff auf Analysedaten.

### Windows Store-Analyse-API

Verwenden Sie die [Windows Store-Analyse-API](../monetize/access-analytics-data-using-windows-store-services.md), um programmgesteuert Analysedaten für Ihre Apps abzurufen. Mit dieser REST-API können Sie Daten für App- und IAP-Käufe, Fehler sowie App-Bewertungen und -Rezensionen abrufen. Diese API verwendet Azure Active Directory (Azure AD), um die Aufrufe von Ihrer App oder Ihrem Dienst zu authentifizieren.

### Windows Dev Center-Inhaltspaket für Power BI

Nutzen Sie das [Windows Dev Center-Inhaltspaket für Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/), um mehr über Ihre Dev Center-Analysedaten in Power BI zu erfahren und diese zu überwachen. Power BI ist ein cloudbasierter Geschäftsanalysedienst, der Ihnen Ihre Geschäftsdaten in einer einzelnen Ansicht präsentiert.

Verwenden Sie die folgenden Ressourcen, um mit Power BI auf Ihre Analysedaten zuzugreifen.

* [Registrieren für Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Informationen zum Verwenden von Power BI](https://powerbi.microsoft.com/guided-learning/)
* [Informationen zum Einsetzen des Windows Dev Center-Inhaltspakets für Power BI, um eine Verbindung zu Ihren Analysedaten herzustellen](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> **Hinweis**
            &nbsp;&nbsp;Für die Verbindung zum Windows Dev Center-Inhaltspaket für Power BI sollten Sie Anmeldeinformationen aus einem Azure AD-Verzeichnis angeben, das mit Ihrem Dev Center-Konto verknüpft ist. Wenn Sie die Anmeldeinformationen Ihres Microsoft-Kontos verwenden, werden Ihre Analysedaten in Power BI nicht automatisch aktualisiert. Für eine Aktualisierung müssen Sie sich dann in Power BI anmelden. Wenn in Ihrer Organisation bereits mit Office 365 oder anderen Unternehmensdiensten von Microsoft gearbeitet wird, verfügen Sie bereits über Azure AD. Andernfalls können Sie es [kostenlos abrufen](http://go.microsoft.com/fwlink/p/?LinkId=703757). Weitere Informationen zur Verknüpfung Ihres Dev Center-Kontos mit Azure AD finden Sie unter [Kontobenutzer verwalten](manage-account-users.md).

### Dev Center-App

Installieren Sie die [Dev Center](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws)-App, um schnell Details über den Zustand und die Leistung Ihrer Apps auf Windows 10-Geräten anzuzeigen. 


<!--HONumber=May16_HO2-->


