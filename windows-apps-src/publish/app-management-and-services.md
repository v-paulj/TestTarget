---
Description: Im Windows Dev Center-Dashboard können Sie Details zu einzelnen Apps anzeigen und verwalten sowie Dienste wie Pushbenachrichtigungen und Karten konfigurieren.
title: App-Verwaltung und -Dienste
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
---

# App-Verwaltung und -Dienste

Im Windows Dev Center-Dashboard können Sie Details zu einzelnen Apps anzeigen und verwalten sowie Dienste wie Pushbenachrichtigungen und Karten konfigurieren.

Wenn Sie in Ihrem Dashboard mit einer App arbeiten, sehen Sie im linken Navigationsmenü Abschnitte für die Dienst- und App-Verwaltung. Sie können diese Abschnitte erweitern, um auf die unten beschriebenen Funktionen zuzugreifen.

## Dienste

Im Abschnitt **Dienste** können Sie verschiedene Dienste für Ihre Apps verwalten.

### Pushbenachrichtigungen

Abhängig vom Pakettyp Ihrer App und den jeweiligen Anforderungen können Sie eine der folgenden Optionen für Pushbenachrichtigungen verwenden:

-   **Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS)** ermöglichen es, Popup-, Kachel- und Badgeupdates sowie unformatierte Updates von ihren eigenen Clouddiensten aus zu senden. Weitere Informationen finden Sie unter [Übersicht über den Windows-Pushbenachrichtigungsdienst (Windows Push Notification Service, WNS)](https://msdn.microsoft.com/library/windows/apps/mt187203).

-   **Microsoft Azure Mobile Apps** ermöglichen das Senden von Pushbenachrichtigungen, die Authentifizierung und Verwaltung von App-Benutzern und das Speichern von App-Daten in der Cloud. Weitere Informationen finden Sie in der [Mobile Apps-Dokumentation](http://go.microsoft.com/fwlink/p/?LinkId=221116).

-   Der **Microsoft-Pushbenachrichtigungsdienst (MPNS)** kann mit Ihren XAP-Paketen für Windows Phone verwendet werden. Sie können eine begrenzte Anzahl nicht authentifizierter Benachrichtigungen senden, ohne hier eine Konfiguration vorzunehmen. Zur Vermeidung von Drosselungslimits wird jedoch empfohlen, authentifizierte Benachrichtigungen zu verwenden. Wenn Sie diesen Dienst verwenden, müssen Sie ein Zertifikat in das auf der Seite **Pushbenachrichtigungen** bereitgestellte Feld hochladen. Weitere Informationen finden Sie unter [Einrichten eines authentifizierten Webdiensts zum Senden von Pushbenachrichtigungen für Windows Phone 8](http://go.microsoft.com/fwlink/p/?LinkId=528736).

### Experimentation

Auf der Seite **Experimentation** können Sie Experimente mit A/B-Tests für Ihre Apps für die universelle Windows-Plattform (UWP) erstellen und ausführen. Bei einem A/B-Test wird die Effektivität von Featureänderungen (oder Varianten) in Ihrer App für einige Kunden zu ermittelt, bevor die Änderungen für die Allgemeinheit aktiviert werden.

Weitere Informationen finden Sie unter [Ausführen von App-Experimenten mit A/B-Tests](../monetize/run-app-experiments-with-a-b-testing.md).

### Karten

Wenn Sie Kartendienste in Apps unter Windows Phone 8.1 und früheren Versionen verwenden möchten, müssen Sie eine Kartendienst-Anwendungs-ID und ein Token in Ihren App-Code einfügen. Sie finden das Token auf der Seite **Karten** im Abschnitt **Dienste**.

> **Hinweis**  Um Kartendienste in Apps zu verwenden, die auf andere Betriebssysteme ausgerichtet sind, besuchen Sie das [Bing Maps Dev Center](http://go.microsoft.com/fwlink/p/?LinkId=614880). Weitere Informationen finden Sie unter [Anfordern eines Kartenauthentifizierungsschlüssels](https://msdn.microsoft.com/library/windows/apps/mt219694).

Weitere Informationen finden Sie unter [Verwenden von Kartendiensten](use-map-services.md).

### Produktsammlungen und Einkäufe

Zur Verwendung der Windows Store-Sammlungs-API und der Windows Store-Einkaufs-API für den Zugriff auf Besitzerinformationen für Apps und IAPs müssen Sie die hier die zugehörigen Azure AD-Client-IDs eingeben. Beachten Sie, dass es bis zu 16 Stunden dauern kann, bis diese Änderungen wirksam werden.

Weitere Informationen finden Sie unter [Anzeigen von Produkten und Gewähren von Produktansprüchen aus einem Dienst](https://msdn.microsoft.com/library/windows/apps/mt609002).

## App-Verwaltung

Im Abschnitt **App-Verwaltung** können Sie Identitäts- und Paketdetails anzeigen sowie die Namen Ihrer App verwalten.

### App-Identität

Diese Seite enthält die Details zur eindeutigen App-Identität im Store, einschließlich den URL(s) zur Verknüpfung der App mit dem App-Eintrag.

Weitere Informationen finden Sie unter [Anzeigen von Details zur App-Identität](view-app-identity-details.md).

### Verwalten von App-Namen

Auf dieser Seite können Sie alle für Ihre App reservierten Namen anzeigen. Sie können hier auch zusätzliche Namen reservieren oder nicht mehr verwendete Namen löschen.

Weitere Informationen finden Sie unter [Verwalten von App-Namen](manage-app-names.md).

### Aktuelle Pakete

Auf dieser Seite können Sie Details zu allen veröffentlichten Paketen anzeigen.

> **Hinweis**  Hier werden erst Informationen angezeigt, nachdem Ihre App veröffentlicht wurde.

Der Name, die Version und die Architektur der einzelnen Pakete werden angezeigt. Klicken Sie auf **Details**, um zusätzliche Informationen wie z. B. unterstützte Sprache, App-Funktionen und Dateigrößen anzuzeigen.

Welche Informationen jeweils für ein Paket angezeigt werden, hängt vom Zielbetriebssystem und anderen Faktoren ab. Falls Sie Ihrem Paket beispielsweise die [Windows-Anzeigenvermittlung](https://msdn.microsoft.com/library/windows/apps/mt219691) hinzugefügt haben, sehen Sie hier einen Link zum Konfigurieren der Vermittlung für dieses Paket.

Entwickler mit OEM-Berechtigungen können auf der Seite **Aktuelle Pakete** außerdem [Vorinstallationspakete generieren](generate-preinstall-packages-for-oems.md).

 

 


<!--HONumber=Mar16_HO5-->


