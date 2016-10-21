---
author: mcleanbyron
Description: "Durch einen kostenlose, zeitlich begrenzte Testversion Ihrer App mit eingeschränkten Features können Sie Ihre Kunden motivieren, auf die Vollversion Ihrer App zu aktualisieren."
title: "Ausschließen oder Einschränken von Features in einer Testversion"
ms.assetid: 1B62318F-9EF5-432A-8593-F3E095CA7056
keywords: kostenlose Testversion, Beispielcode
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: fdca95a6e925ca2238fdcd8791ade2ed4ea5a310

---

# Ausschließen oder Einschränken von Features in einer Testversion




>**Hinweis**&nbsp;&nbsp;In diesem Artikel wird die Verwendung von Mitgliedern des [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)-Namespace erläutert. Wenn Ihre App auf Windows10, Version1607 oder höher ausgerichtet ist, wird empfohlen, den [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)-Namespace zu verwenden, statt des **Windows.ApplicationModel.Store**-Namespace, um eine Testversion zu installieren. Weitere Informationen finden Sie unter [Implementieren einer Testversion der App](implement-a-trial-version-of-your-app.md).

Durch einen kostenlose, zeitlich begrenzte Testversion Ihrer App mit eingeschränkten Features können Sie Ihre Kunden motivieren, auf die Vollversion Ihrer App zu aktualisieren. Bestimmen Sie die einzuschränkenden Features, bevor Sie mit dem Codieren beginnen, und stellen Sie dann sicher, dass diese nur beim Erwerb einer Lizenz für die Vollversion der App verfügbar sind. Außerdem können Sie Features wie Banner oder Wasserzeichen aktivieren, die nur in der Testversion angezeigt werden, bevor ein Kunde Ihre App kauft.

Hier finden Sie Informationen dazu, wie Sie dies Ihrer App hinzufügen.

## Voraussetzungen

Eine Windows-App zum Hinzufügen von Features zum Kauf für Kunden.

## Schritt 1: Suchen Sie die Features aus, die während des Testzeitraums aktiviert bzw. deaktiviert werden sollen.

Der aktuelle Lizenzstatus Ihrer App wird als Eigenschaften der [**LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/br225157)-Klasse gespeichert. In der Regel nehmen Sie die vom Lizenzstatus abhängigen Funktionen in einen Bedingungsblock auf, wie im nächsten Schritt beschrieben. Stellen Sie beim Auswählen dieser Features sicher, dass sie auf eine Weise implementiert werden können, dass sie in jedem Lizenzstatus funktionieren.

Überlegen Sie auch, wie Änderungen an der App-Lizenz während der Ausführung der App verarbeitet werden sollen. Ihre Test-App kann alle Features, jedoch zusätzlich In-App-Anzeigenbanner enthalten, die die kostenpflichtige Version nicht enthält. Oder in der Test-App sind bestimmte Features deaktiviert, oder es werden regelmäßig Aufforderungen zum Kauf angezeigt.

Überlegen Sie, welche Art App Sie erstellen und welche Test- oder Ablaufstrategie sich gut für sie eignet. Bei einer Testversion für ein Spiel hat es sich bewährt, den Umfang des Spielinhalts, den ein Anwender nutzen kann, zu beschränken. Bei der Testversion eines Hilfsprogramms möchten Sie vielleicht ein Ablaufdatum festlegen oder die Features einschränken, die ein potenzieller Kunde verwenden kann.

Bei den meisten Apps, die keine Spiele sind, ist das Festlegen eines Ablaufdatums eine gute Methode, da Benutzer ein gutes Verständnis für die vollständige App entwickeln. Im Folgenden sind häufige Ablaufszenarien und Ihre Optionen für ihre Handhabung aufgeführt.

-   **Ablauf der Testlizenz während der App-Ausführung**

    Falls die Testversion abläuft, während die App ausgeführt wird, kann Ihre App:

    -   keine Aktion ausführen.
    -   dem Kunden eine Meldung anzeigen.
    -   geschlossen werden.
    -   den Kunden auffordern, die App zu kaufen.

    Die beste Vorgehensweise besteht darin, eine Meldung mit der Aufforderung zum Kauf der App anzuzeigen und nach dem Kauf die App mit allen aktivierten Features fortzusetzen. Wenn der Kunde die App nicht kauft, wird die App geschlossen, oder der Kunde wird in regelmäßigen Abständen daran erinnert, die App zu kaufen.

-   **Ablauf der Testlizenz vor dem Starten der App**

    Falls die Testversion abläuft, bevor der Benutzer die App startet, wird Ihre App nicht gestartet. Stattdessen sehen Benutzer ein Dialogfeld, das ihnen die Möglichkeit bietet, die App im Windows Store zu kaufen.

-   **Kauf der App durch den Kunden während der App-Ausführung**

    Wenn der Kunde Ihre App während der Ausführung erwirbt, kann die App:

    -   keine Aktion ausführen und die Fortsetzung im Testmodus bis zum Neustart der App zulassen.
    -   dem Kunden für den Kauf danken oder eine andere Meldung anzeigen.
    -   die Features aktivieren, die mit einer Volllizenz verfügbar sind (oder die Meldungen, dass es sich um eine Testversion handelt, deaktivieren).

Falls Sie die Lizenzänderung ermitteln und eine Aktion in der App ausführen möchten, müssen Sie einen Ereignishandler hinzufügen, wie im nächsten Schritt beschrieben.
## Schritt 2: Initialisieren Sie die Lizenzinformationen.

Rufen Sie beim Initialisieren der App das [**LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/br225157)-Objekt für die App ab wie im folgenden Beispiel beschrieben. Es wird davon ausgegangen, dass **licenseInformation** eine globale Variable oder ein Feld vom Typ **LicenseInformation** ist.

Initialisieren Sie [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) oder [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766), um auf die Lizenzinformationen zur App zuzugreifen.

```CSharp
void initializeLicense()
{
    // Initialize the license info for use in the app that is uploaded to the Store.
    // uncomment for release
    //   licenseInformation = CurrentApp.LicenseInformation;

    // Initialize the license info for testing.
    // comment the next line for release
    licenseInformation = CurrentAppSimulator.LicenseInformation;

}
```

Fügen Sie einen Ereignishandler hinzu, um Benachrichtigungen zu Lizenzänderungen während der Ausführung der App zu erhalten. Die App-Lizenz kann sich zum Beispiel ändern, wenn der Testzeitraum abläuft oder der Kunde die App in einem Store kauft.

```CSharp
void InitializeLicense()
{
    // Initialize the license info for use in the app that is uploaded to the Store.
    // uncomment for release
    //   licenseInformation = CurrentApp.LicenseInformation;

    // Initialize the license info for testing.
    // comment the next line for release
    licenseInformation = CurrentAppSimulator.LicenseInformation;

    // Register for the license state change event.
     licenseInformation.LicenseChanged += new LicenseChangedEventHandler(licenseChangedEventHandler);

}

// ...

void licenseChangedEventHandler()
{
    ReloadLicense(); // code is in next steps
}
```

## Schritt 3: Schreiben Sie den Code für die Features in Bedingungsblöcke.

Beim Auslösen des Lizenzänderungsereignisses muss die App über einen Aufruf der Lizenz-API ermitteln, ob sich der Teststatus geändert hat. Der Code in diesem Schritt veranschaulicht, wie der Handler für dieses Ereignis strukturiert werden muss. Falls ein Benutzer die App gekauft hat, wird empfohlen, den Benutzer zu diesem Zeitpunkt über den geänderten Lizenzstatus zu informieren. Gegebenenfalls müssen Sie den Benutzer zum Neustarten der App auffordern, falls Ihre Programmierung dies erfordert. Versuchen Sie jedoch, diesen Übergang so nahtlos und unmerklich wie möglich zu machen.

Dieses Beispiel zeigt, wie der Lizenzstatus einer App ermittelt wird, um ein Feature Ihrer App zu aktivieren oder zu deaktivieren.

```CSharp
void ReloadLicense()
{
    if (licenseInformation.IsActive)
    {
         if (licenseInformation.IsTrial)
         {
             // Show the features that are available during trial only.
         }
         else
         {
             // Show the features that are available only with a full license.
         }
     }
     else
     {
         // A license is inactive only when there' s an error.
     }
}
```

## Schritt 4: Rufen Sie das Ablaufdatums der Testversion einer App ab.

Nehmen Sie Code mit auf, um das Ablaufdatum der Testversion einer App abzurufen.

Der Code in diesem Beispiel legt eine Funktion fest, mit der das Ablaufdatum der Testversion einer App abgerufen wird. Ist die Lizenz noch gültig, wird das Ablaufdatum zusammen mit den verbleibenden Tagen bis zum Ablauf des Testzeitraums angezeigt.

```CSharp
void DisplayTrialVersionExpirationTime()
{
    if (licenseInformation.IsActive)
    {
        if (licenseInformation.IsTrial)
        {
            var longDateFormat = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longdate");

            // Display the expiration date using the DateTimeFormatter.
            // For example, longDateFormat.Format(licenseInformation.ExpirationDate)

            var daysRemaining = (licenseInformation.ExpirationDate - DateTime.Now).Days;

            // Let the user know the number of days remaining before the feature expires
        }
        else
        {
            // ...
        }
    }
    else
    {
       // ...
    }
}
```

## Schritt 5: Testen Sie die Features mithilfe simulierter Aufrufe an die Lizenz-API.

Testen Sie nun die App mithilfe simulierter Aufrufe an den Lizenzserver. Ersetzen Sie in JavaScript, C#, Visual Basic oder Visual C++ im Initialisierungscode der App Verweise auf [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) durch [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766).

[**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) ruft testspezifische Lizenzierungsinformationen aus einer XML-Datei mit dem Namen "WindowsStoreProxy.xml" ab, die sich in "%userprofile%\\AppData\\local\\packages\\&lt;package name&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData" befindet. Sind dieser Pfad und diese Datei nicht vorhanden, müssen Sie diese bei der Installation oder während der Laufzeit erstellen. Wenn die Datei WindowsStoreProxy.xml nicht am angegebenen Speicherort vorhanden ist, und Sie versuchen, auf die [**CurrentAppSimulator.LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/hh779768)-Eigenschaft zuzugreifen, erhalten Sie eine Fehlermeldung.

Dieses Beispiel veranschaulicht das Hinzufügen von Code, um die App in unterschiedlichen Lizenzierungszuständen zu testen.

```CSharp
void appInit()
{
    // some app initialization functions

    // Initialize the license info for use in the app that is uploaded to the Store.
    // uncomment for release
    //   licenseInformation = CurrentApp.LicenseInformation;

    // Initialize the license info for testing.
    // comment the next line for release
    licenseInformation = CurrentAppSimulator.LicenseInformation;

    // other app initialization functions
}
```

Sie können "WindowsStoreProxy.xml" bearbeiten, um die simulierten Ablaufdaten für die App und die Features zu ändern. Testen Sie alle möglichen Ablauf- und Lizenzkonfigurationen, damit alles wunschgemäß funktioniert.

## Schritt 6: Ersetzen Sie die simulierten Lizenz-API-Methoden durch die tatsächliche API.

Nachdem Sie die App mit dem simulierten Lizenzserver getestet haben, ersetzen Sie [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) durch [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765), bevor Sie die App zur Zertifizierung an einen Store übermitteln (siehe nächstes Codebeispiel).

**Wichtig**  Ihre App muss das [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)-Objekt verwenden, wenn Sie sie an einen Store übermitteln, da ansonsten ein Zertifizierungsfehler auftritt.

```CSharp
void appInit()
{
    // some app initialization functions

    // Initialize the license info for use in the app that is uploaded to the Store.
    // uncomment for release
    licenseInformation = CurrentApp.LicenseInformation;

    // Initialize the license info for testing.
    // comment the next line for release
    //   licenseInformation = CurrentAppSimulator.LicenseInformation;

    // other app initialization functions
}
```

## Schritt 7: Beschreiben Sie für Ihre Kunden, wie die kostenlose Testversion funktioniert.

Erläutern Sie, wie sich Ihre App während und nach dem kostenlosen Testzeitraum verhält, sodass Ihre Kunden vom Verhalten Ihrer App nicht überrascht werden.

Weitere Informationen zum Beschreiben Ihrer App finden Sie unter [Erstellen von App-Beschreibungen](https://msdn.microsoft.com/library/windows/apps/mt148529).

## Verwandte Themen

* [Store-Beispiel (zeigt Testversionen und In-App-Käufe)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Festlegen von Preisen und Verfügbarkeit von Apps](https://msdn.microsoft.com/library/windows/apps/mt148548)
* [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)
* [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)
 

 



<!--HONumber=Aug16_HO5-->


