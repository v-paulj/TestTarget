---
author: PatrickFarley
title: Abrufen der Position eines Benutzers
description: "Ermitteln Sie den Standort des Benutzers, und reagieren Sie auf Änderungen des Standorts. Der Zugriff auf die Position eines Benutzers wird über die Datenschutzeinstellungen in der Einstellungs-App verwaltet. In diesem Thema wird auch gezeigt, wie Sie überprüfen, ob Ihre App über die Berechtigung zum Zugriff auf den Benutzerstandort verfügt."
ms.assetid: 24DC9A41-8CC1-48B0-BC6D-24BF571AFCC8
translationtype: Human Translation
ms.sourcegitcommit: bdb6cbd0b0ccb7b6aa04cf6ba98bb154af325515
ms.openlocfilehash: 1172aae67169295ac6f2446c839a1cce5a84fa36

---

# Abrufen der Position eines Benutzers


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Ermitteln Sie den Standort des Benutzers, und reagieren Sie auf Änderungen des Standorts. Der Zugriff auf die Position eines Benutzers wird über die Datenschutzeinstellungen in der Einstellungs-App verwaltet. In diesem Thema wird auch gezeigt, wie Sie überprüfen, ob Ihre App über die Berechtigung zum Zugriff auf den Benutzerstandort verfügt.

**Tipp** Um mehr über das Zugreifen auf den Benutzerstandort in Ihrer App zu erfahren, laden Sie das folgende Beispiel aus den [API-Beispielen für die Universelle Windows-Plattform](http://go.microsoft.com/fwlink/p/?LinkId=619979) auf GitHub herunter.

-   [Kartenbeispiel für die Universelle Windows-Plattform (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## Aktivieren der Positionsfunktion


1.  Doppelklicken Sie im **Projektmappen-Explorer** auf **package.appxmanifest**, und wählen Sie die Registerkarte **Funktionen** aus.
2.  Wählen Sie in der Liste **Funktionen** die Registerkarte **Funktionen** aus. Dadurch wird der Paketmanifestdatei die `Location`-Gerätefunktion hinzugefügt.

```XML
  <Capabilities>
    <!-- DeviceCapability elements must follow Capability elements (if present) -->
    <DeviceCapability Name="location"/>
  </Capabilities>
```

## Abrufen der aktuellen Position


In diesem Abschnitt erfahren Sie, wie Sie den geografische Standort eines Benutzers über APIs im [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603)-Namespace feststellen.

### Schritt1: Anfordern des Zugriffs auf die Position des Benutzers

Wenn Ihre App nicht über die Consentless Location-Funktion (siehe Hinweis) verfügt, müssen Sie mit der [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152)-Methode Zugriff auf Positionsdaten des Benutzers anfordern, bevor Sie versuchen, auf die Position zuzugreifen. Sie müssen die **RequestAccessAsync**-Methode aus dem UI-Thread aufrufen, und die App muss sich im Vordergrund ausgeführt werden. Ihre App kann erst auf Positionsdaten des Benutzers zugreifen, nachdem der Benutzer der App den Zugriff gewährt hat.\*

```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();
```



Die [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152)-Methode fordert den Benutzer auf, den Zugriff auf seinen Standort zu genehmigen. Der Benutzer wird nur einmal (pro App) aufgefordert. Nachdem die Berechtigung erstmalig gewährt oder verweigert wurde, fordert die Methode keine Berechtigung mehr vom Benutzer an. Um das Ändern von Standortberechtigungen nach der Aufforderung für den Benutzer zu vereinfachen, sollten Sie einen Link zu den Standorteinstellungen bereitstellen (s. wie weiter unten in diesem Thema).

>Hinweis: Mit der Funktion „Consentless Location“ kann die App eine absichtlich verborgene (ungenaue) Position ohne ausdrückliche Erlaubnis des Benutzers abrufen. (Die systemweite Einstellung muss jedoch weiterhin auf **Ein** festgelegt sein). Informationen zur Verwendung von Consentless Location in Ihrer App finden Sie in der [**AllowFallbackToConsentlessPositions**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Geolocation.Geolocator.AllowFallbackToConsentlessPositions)-Methode in der [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geolocator.aspx)-Klasse.

### Schritt2: Abrufen des Benutzerstandorts und Registrieren für Änderungen von Standortberechtigungen

Die [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536)-Methode liest einmalig den aktuellen Standort. Hier wird eine **switch**-Anweisung mit **accessStatus** (aus dem vorherigen Beispiel) verwendet, die nur aktiv ist, wenn der Zugriff auf den Standort des Benutzers zugelassen wird. Wenn der Zugriff auf die Position des Benutzers zugelassen wurde, erstellt der Code ein [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534)-Objekt, führt eine Registrierung für Änderungen von Standortberechtigungen aus und fordert den Standort des Benutzers an.

```csharp
switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        _rootPage.NotifyUser("Waiting for update...", NotifyType.StatusMessage);

        // If DesiredAccuracy or DesiredAccuracyInMeters are not set (or value is 0), DesiredAccuracy.Default is used.
        Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = _desireAccuracyInMetersValue };

        // Subscribe to the StatusChanged event to get updates of location status changes.
        _geolocator.StatusChanged += OnStatusChanged;
                        
        // Carry out the operation.
        Geoposition pos = await geolocator.GetGeopositionAsync();

        UpdateLocationData(pos);
        _rootPage.NotifyUser("Location updated.", NotifyType.StatusMessage);
        break;

    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Visible;
        UpdateLocationData(null);
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecified error.", NotifyType.ErrorMessage);
        UpdateLocationData(null);
        break;
}
```

### Schritt 3: Behandeln von Änderungen von Standortberechtigungen

Das [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534)-Objekt löst das [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542)-Ereignis aus, um anzugeben, dass sich die Standorteinstellungen des Benutzers geändert haben. Das Ereignis übergibt den entsprechenden Status über die Eigenschaft **Status** des Arguments (des Typs [**PositionStatus**](https://msdn.microsoft.com/library/windows/apps/br225599). Beachten Sie, dass diese Methode nicht vom UI-Thread aufgerufen wird und das [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)-Objekt die UI-Änderungen aufruft.

```csharp
using Windows.UI.Core;
...
async private void OnStatusChanged(Geolocator sender, StatusChangedEventArgs e)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        // Show the location setting message only if status is disabled.
        LocationDisabledMessage.Visibility = Visibility.Collapsed;

        switch (e.Status)
        {
            case PositionStatus.Ready:
                // Location platform is providing valid data.
                ScenarioOutput_Status.Text = "Ready";
                _rootPage.NotifyUser("Location platform is ready.", NotifyType.StatusMessage);
                break;

            case PositionStatus.Initializing:
                // Location platform is attempting to acquire a fix. 
                ScenarioOutput_Status.Text = "Initializing";
                _rootPage.NotifyUser("Location platform is attempting to obtain a position.", NotifyType.StatusMessage);
                break;

            case PositionStatus.NoData:
                // Location platform could not obtain location data.
                ScenarioOutput_Status.Text = "No data";
                _rootPage.NotifyUser("Not able to determine the location.", NotifyType.ErrorMessage);
                break;

            case PositionStatus.Disabled:
                // The permission to access location data is denied by the user or other policies.
                ScenarioOutput_Status.Text = "Disabled";
                _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);

                // Show message to the user to go to location settings.
                LocationDisabledMessage.Visibility = Visibility.Visible;

                // Clear any cached location data.
                UpdateLocationData(null);
                break;

            case PositionStatus.NotInitialized:
                // The location platform is not initialized. This indicates that the application 
                // has not made a request for location data.
                ScenarioOutput_Status.Text = "Not initialized";
                _rootPage.NotifyUser("No request for location is made yet.", NotifyType.StatusMessage);
                break;

            case PositionStatus.NotAvailable:
                // The location platform is not available on this version of the OS.
                ScenarioOutput_Status.Text = "Not available";
                _rootPage.NotifyUser("Location is not available on this version of the OS.", NotifyType.ErrorMessage);
                break;

            default:
                ScenarioOutput_Status.Text = "Unknown";
                _rootPage.NotifyUser(string.Empty, NotifyType.StatusMessage);
                break;
        }
    });
}
```

## Reagieren auf Standortupdates


In diesem Abschnitt wird beschrieben, wie Sie mit dem [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540)-Ereignis Standortupdates des Benutzers über einen bestimmten Zeitraum empfangen. Da der Benutzer den Zugriff auf den Standort jederzeit widerrufen kann, ist es wichtig, [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) aufzurufen und das [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542)-Ereignis zu verwenden wie im vorherigen Abschnitt dargestellt.

In diesem Abschnitt wird vorausgesetzt, dass Sie die Standortfunktion bereits aktiviert und [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) im UI-Thread der Vordergrund-App aufgerufen haben.

### Schritt1: Definieren des Berichtsintervalls und Registrieren für Standortupdates

In diesem Beispiel wird eine **switch**-Anweisung mit **accessStatus** (aus dem vorherigen Beispiel) verwendet, die nur aktiv ist, wenn der Zugriff auf den Standort eines Benutzers zugelassen wird. Wenn der Zugriff auf den Standort des Benutzers zugelassen wurde, erstellt der Code ein [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534)-Objekt, gibt den Nachverfolgungstyp an und führt eine Registrierung für Standortupdates aus.

Das [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534)-Objekt kann das [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540)-Ereignis basierend auf einer Standortänderung (entfernungsbasierte Nachverfolgung) oder Zeitänderung (zeitraumbasierte Nachverfolgung) auslösen.

-   Für die entfernungsbasierte Nachverfolgung legen Sie die [**MovementThreshold**](https://msdn.microsoft.com/library/windows/apps/br225539)-Eigenschaft fest.
-   Für die zeitraumbasierte Nachverfolgung legen Sie die [**ReportInterval**](https://msdn.microsoft.com/library/windows/apps/br225541)-Eigenschaft fest.

Wenn keine der beiden Eigenschaften festgelegt wird, wird jede 1 Sekunde ein Standort zurückgegeben (entsprechend `ReportInterval = 1000`). Hier wird ein Berichtsintervall von 2 Sekunden (`ReportInterval = 2000`) verwendet.
```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();

switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        // Create Geolocator and define perodic-based tracking (2 second interval).
        _geolocator = new Geolocator { ReportInterval = 2000 };

        // Subscribe to the PositionChanged event to get location updates.
        _geolocator.PositionChanged += OnPositionChanged;

        // Subscribe to StatusChanged event to get updates of location status changes.
        _geolocator.StatusChanged += OnStatusChanged;
                    
        _rootPage.NotifyUser("Waiting for update...", NotifyType.StatusMessage);
        LocationDisabledMessage.Visibility = Visibility.Collapsed;
        StartTrackingButton.IsEnabled = false;
        StopTrackingButton.IsEnabled = true;
        break;

    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Visible;
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecificed error!", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Collapsed;
        break;
}
```

### Schritt2: Behandeln von Standortupdates

Das [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534)-Objekt löst das [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540)-Ereignis aus, um anzugeben, dass sich der Benutzerstandort geändert hat bzw. dass Zeit vergangen ist, je nachdem, welche Eigenschaft Sie konfiguriert haben. Dieses Ereignis übergibt den entsprechende Standort über die Eigenschaft **Position** des Arguments (des Typs [**Geoposition**](https://msdn.microsoft.com/library/windows/apps/br225543). In diesem Beispiel wird die Methode nicht vom UI-Thread aufgerufen. Die UI-Änderungen werden durch das [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)-Objekt aufgerufen.

```csharp
using Windows.UI.Core;
...
async private void OnPositionChanged(Geolocator sender, PositionChangedEventArgs e)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        _rootPage.NotifyUser("Location updated.", NotifyType.StatusMessage);
        UpdateLocationData(e.Position);
    });
}
```

## Ändern der Datenschutzeinstellungen für den Standort


Wenn Ihre App gemäß den Standortdatenschutzeinstellungen nicht auf den Standort des Benutzers zugreifen darf, sollten Sie einen praktischen Link zu den **Standortdatenschutzeinstellungen** in der **Einstellungs**-App bereitstellen. In diesem Beispiel wird ein Hyperlink-Steuerelement verwendet, um zum `ms-settings:privacy-location`-URI zu navigieren.

```xml
<!--Set Visibility to Visible when access to location is denied -->  
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic" 
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access Location. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-location">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the location privacy settings."/>
</TextBlock>
```

Alternativ kann Ihre App die [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)-Methode aufrufen, um die **Einstellungs**-App per Code zu starten. Weitere Informationen finden Sie unter [Starten der Einstellungs-App von Windows](https://msdn.microsoft.com/library/windows/apps/mt228342).

```csharp
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-location"));
```

## Behandeln von App-Problemen


Bevor Ihre App auf die Position des Benutzers zugreifen kann, muss **Position** auf dem Gerät aktiviert sein. Vergewissern Sie sich in der **Einstellungs**-App, dass die folgenden **Datenschutzeinstellungen für den Standort** aktiviert sind:

-   **Position dieses Geräts** ist **aktiviert** (gilt nicht für Windows 10 Mobile)
-   Die Einstellung **Position** der Positionsdienste ist **aktiviert**.
-   Ihre App hat unter **Wählen Sie Apps aus, die Ihre Position verwenden dürfen** die Einstellung **Ein**.

## Verwandte Themen

* [UWP-Geolocation-Beispiel](http://go.microsoft.com/fwlink/p/?linkid=533278)
* [Entwurfsrichtlinien für Geofencing](https://msdn.microsoft.com/library/windows/apps/dn631756)
* [Entwurfsrichtlinien für Apps mit Positionsbestimmung](https://msdn.microsoft.com/library/windows/apps/hh465148)





<!--HONumber=Aug16_HO3-->


