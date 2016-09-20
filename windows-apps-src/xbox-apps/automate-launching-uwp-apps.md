---
title: "Automatisieren des Starts für Apps der universellen Windows Plattform (UWP) in Windows10"
description: "Mit der Protokoll- und Startaktivierung können Entwickler ihre UWP-Apps oder Spiele für automatisierte Tests automatisch starten."
author: listurm
ms.sourcegitcommit: adf2d16f9c208631f91fbcad19d1ea8087cd9cb5
ms.openlocfilehash: ae2f80a915f4aed90c269c37a11d01a2f6c9849e

---

# Automatisieren des Starts von UWP-Apps unter Windows 10

## Einführung

Entwickler haben mehrere Möglichkeiten, einen automatisierten Start von UWP-Apps zu realisieren. Dieses Dokument beschreibt Methoden zum Starten einer App über die Protokoll- und Startaktivierung.


             Die *Protokollaktivierung* ermöglicht der App eine Registrierung als Handler für ein bestimmtes Protokoll. 


             Die *Startaktivierung* stellt den normalen Start einer App dar, beispielsweise das Starten über eine App-Kachel.

Bei jeder Aktivierungsmethode können Sie entweder die Befehlszeile oder eine Startanwendung nutzen. Wird die App gerade ausgeführt, wird sie bei allen Aktivierungsmethoden in den Vordergrund geschaltet, wodurch sie erneut aktiviert wird. Dabei werden die neuen Aktivierungsargumente bereitgestellt. Somit besteht die Möglichkeit, Aktivierungsbefehle zu verwenden, um neue Nachrichten für die App bereitzustellen. Beachten Sie unbedingt, dass das Projekt kompiliert und für die Aktivierungsmethode bereitgestellt werden muss, damit die aktualisierte App ausgeführt wird. 

## Protokollaktivierung

Führen Sie diese Schritte zum Einrichten der Protokollaktivierung für Apps aus: 

1. Öffnen Sie die Datei **Package.appxmanifest** in Visual Studio.
2. Klicken Sie auf die Registerkarte **Deklarationen**.
3. Wählen Sie unter **Verfügbare Deklarationen** die Option **Protokoll** und anschließend **Hinzufügen** aus.
4. Geben Sie unter **Eigenschaften** im Feld **Name** einen eindeutigen Namen zum Starten der App ein. 

    ![Protokollaktivierung](images/automate-uwp-apps-1.png)

5. Speichern Sie die Datei, und stellen Sie das Projekt bereit. 
6. Nach der Bereitstellung des Projekts sollte die Protokollaktivierung festgelegt werden. 
7. Wechseln Sie zu **Systemsteuerung\Alle Systemsteuerungselemente\Standardprogramme**, und wählen Sie **Dateityp oder Protokoll einem Programm zuordnen** aus. Führen Sie einen Bildlauf zum Abschnitt **Protokolle** aus, um festzustellen, ob das Protokoll aufgeführt ist. 

Nach der Einrichtung der Protokollaktivierung haben Sie zwei Möglichkeiten (Befehlszeile oder Startanwendung), die App über das Protokoll zu aktivieren. 

### Befehlszeile

Die App wird per Protokoll aktiviert, indem Sie die Befehlszeile mit dem Startbefehl gefolgt vom vorher festgelegten Protokoll, einem Doppelpunkt („:“) und beliebigen Parametern verwenden. Die Parameter können eine beliebige Zeichenfolge sein. Um aber die Vorteile der Uniform Resource Identifier (URI)-Funktionen nutzen zu können, sollten Sie das standardmäßige URI-Format verwenden: 

  ```
  scheme://username:password@host:port/path.extension?query#fragment
  ```

Das Uri-Objekt verfügt über Methoden zum Analysieren einer URI-Zeichenfolge in diesem Format. Weitere Informationen finden Sie unter [Uri-Klasse (MSDN)](https://msdn.microsoft.com/en-us/library/windows/apps/windows.foundation.uri.aspx). 

Beispiele:

  ```
  >start bingnews:
  >start myapplication:protocol-parameter
  >start myapplication://single-player/level3?godmode=1&ammo=200
  ```

Die Befehlszeilenaktivierung per Protokoll unterstützt Unicode-Zeichen bis zu maximal 2038 Zeichen auf dem unformatierten URI. 

### Startanwendung

Erstellen Sie zum Starten eine separate Anwendung, die die WinRT-API unterstützt. Im folgenden Beispiel wird der C++-Code für einen Start per Protokollaktivierung in einem Startprogramm gezeigt. Dabei ist **PackageURI** der URI für die Anwendung mit beliebigen Argumenten, beispielsweise `myapplication:` oder `myapplication:protocol activation arguments`.

```
bool ProtocolLaunchURI(Platform::String^ URI)
{
       IAsyncOperation<bool>^ protocolLaunchAsyncOp;
       try
       {
              protocolLaunchAsyncOp = Windows::System::Launcher::LaunchUriAsync(ref new 
Uri(URI));
       }
       catch (Platform::Exception^ e)
       {
              Platform::String^ dbgStr = "ProtocolLaunchURI Exception Thrown: " 
+ e->ToString() + "\n";
              OutputDebugString(dbgStr->Data());
              return false;
       }

       concurrency::create_task(protocolLaunchAsyncOp).wait();

       if (protocolLaunchAsyncOp->Status == AsyncStatus::Completed)
       {
              bool LaunchResult = protocolLaunchAsyncOp->GetResults();
              Platform::String^ dbgStr = "ProtocolLaunchURI " + URI 
+ " completed. Launch result " + LaunchResult + "\n";
              OutputDebugString(dbgStr->Data());
              return LaunchResult;
       }
       else
       {
              Platform::String^ dbgStr = "ProtocolLaunchURI " + URI + " failed. Status:" 
+ protocolLaunchAsyncOp->Status.ToString() + " ErrorCode:" 
+ protocolLaunchAsyncOp->ErrorCode.ToString() + "\n";
              OutputDebugString(dbgStr->Data());
              return false;
       }
}
```
Für die Protokollaktivierung mit der Startanwendung gelten dieselben Einschränkungen für Argumente wie für die Protokollaktivierung mit der Befehlszeile. Beide unterstützen Unicode-Zeichen bis maximal 2038 Zeichen auf dem unformatierten URI. 

## Startaktivierung

Sie können die App auch mit der Startaktivierung starten. Es ist kein Setup erforderlich, dafür aber die Anwendungsbenutzermodell-ID (Application User Model ID, AUMID) der UWP-App. Die AUMID ist der Paketfamilienname gefolgt von einem Ausrufezeichen und der Anwendungs-ID. 

Die beste Möglichkeit zum Abrufen des Paketfamiliennamens besteht in der Durchführung der folgenden Schritte:

1. Öffnen Sie die Datei **Package.appxmanifest**.
2. Geben Sie auf der Registerkarte **Verpacken** den **Paketnamen** ein.

    ![Startaktivierung](images/automate-uwp-apps-2.png)

3. Wenn der **Paketfamilienname** nicht aufgeführt ist, öffnen Sie PowerShell und führen `>get-appxpackage MyPackageName` aus, um **PackageFamilyName** zu finden.

Sie finden die Anwendungs-ID in der Datei **Package.appxmanifest** (geöffnet in der XML-Ansicht) unter dem `<Applications>`-Element.

### Befehlszeile

Ein Tool für die Durchführung einer Startaktivierung einer UWP-App ist im Windows 10-SDK enthalten. Es kann über die Befehlszeile ausgeführt werden und benötigt die AUMID der App, damit ein Start als Argument möglich ist.

```
C:\Program Files (x86)\Windows Kits\10\App Certification Kit\microsoft.windows.softwarelogo.appxlauncher.exe <AUMID>
```

Es sieht etwa wie folgt aus:

```
"C:\Program Files (x86)\Windows Kits\10\App Certification Kit\microsoft.windows.softwarelogo.appxlauncher.exe" MyPackageName_ph1m9x8skttmg!AppId
```

Diese Option unterstützt keine Befehlszeilenargumente. 

### Startanwendung

Sie können eine separate Anwendung erstellen, die den Einsatz von COM zum Starten unterstützt. Das folgende Beispiel zeigt den C++-Code für den Start mit Startaktivierung in einem Startprogramm. Mit diesem Code erstellen Sie ein **ApplicationActivationManager**-Objekt und rufen **ActivateApplication** auf, indem Sie die zuvor gefundene AUMID und beliebige Argumente übergeben. Weitere Informationen zu den anderen Parametern finden Sie unter [IApplicationActivationManager::ActivateApplication-Methode (MSDN)](https://msdn.microsoft.com/en-us/library/windows/desktop/hh706903(v=vs.85).aspx).

```
#include <ShObjIdl.h>
#include <atlbase.h>

HRESULT LaunchApp(LPCWSTR AUMID)
{
     HRESULT hr = CoInitializeEx(nullptr, COINIT_APARTMENTTHREADED);
     if (FAILED(hr))
     {
            wprintf(L"LaunchApp %s: Failed to init COM. hr = 0x%08lx \n", AUMID, hr);
     }
     {
            CComPtr<IApplicationActivationManager> AppActivationMgr = nullptr;
            if (SUCCEEDED(hr))
            {
                   hr = CoCreateInstance(CLSID_ApplicationActivationManager, nullptr,  
CLSCTX_LOCAL_SERVER, IID_PPV_ARGS(&AppActivationMgr));
                   if (FAILED(hr))
                   {
                         wprintf(L"LaunchApp %s: Failed to create Application Activation 
Manager. hr = 0x%08lx \n", AUMID, hr);
                   }
            }
            if (SUCCEEDED(hr))
            {
                   DWORD pid = 0;
                   hr = AppActivationMgr->ActivateApplication(AUMID, nullptr, AO_NONE, 
&pid);
                   if (FAILED(hr))
                   {
                         wprintf(L"LaunchApp %s: Failed to Activate App. hr = 0x%08lx 
\n", AUMID, hr);
                   }
            }
     }
     CoUninitialize();
     return hr;
}
```

Dabei ist zu beachten, dass diese Methode im Gegensatz zur vorherigen Methode für den Start (also die Befehlszeilenmethode) übergebene Argumente unterstützt.

## Akzeptieren von Argumenten

Damit Argumente, die bei der Aktivierung der UWP-App übergeben werden, akzeptiert werden, müssen Sie der App einen Code hinzufügen. Zur Feststellung, ob eine Protokoll- oder Startaktivierung stattgefunden hat, überschreiben Sie das **OnActivated**-Ereignis, überprüfen den Typ des Arguments und rufen dann die unformatierte Zeichenfolge oder die vorab analysierten Werte des Uri-Objekts ab. 

Dieses Beispiel zeigt, wie Sie die unformatierte Zeichenfolge abrufen.

```
void OnActivated(IActivatedEventArgs^ args)
{
        // Check for launch activation
        if (args->Kind == ActivationKind::Launch)
        {
            auto launchArgs = static_cast<LaunchActivatedEventArgs^>(args); 
Platform::String^ argval = launchArgs->Arguments;
            // Manipulate arguments …
        }

        // Check for protocol activation
        if (args->Kind == ActivationKind::Protocol)
        {
            auto protocolArgs = static_cast< ProtocolActivatedEventArgs^>(args);
            Platform::String^ argval = protocolArgs->Uri->ToString();
            // Manipulate arguments …
        }
    }
```

## Zusammenfassung
Zusammenfassend lässt sich sagen, dass für den Start der UWP-App verschiedene Methoden zur Verfügung stehen. Je nach Anforderungen und Anwendungsfällen können manche Methoden besser geeignet sein als andere. 



<!--HONumber=Jun16_HO4-->


