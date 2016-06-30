---
author: DBirtolo
ms.assetid: 15BAB25C-DA8C-4F13-9B8F-EA9E4270BCE9
title: Verwenden des Lichtsensors
description: "Hier erfahren Sie, wie Sie mithilfe des Umgebungslichtsensors veränderte Lichtverhältnisse erkennen."
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 289d50ff4f45147c46bd66c526cf109d8fdf6d32

---
# Verwenden des Lichtsensors

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** Wichtige APIs **

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**LightSensor**](https://msdn.microsoft.com/library/windows/apps/BR225790)

Hier erfahren Sie, wie Sie mithilfe des Umgebungslichtsensors veränderte Lichtverhältnisse erkennen.

Der Umgebungslichtsensor ist einer von vielen Sensoren, mit denen Apps auf Veränderungen in der Umgebung des Benutzers reagieren können.

## Voraussetzungen

Sie sollten mit XAML (Extensible Application Markup Language), Microsoft Visual C# und Ereignissen vertraut sein.

Das verwendete Gerät oder der Emulator muss einen Umgebungslichtsensor unterstützen.

## Erstellen einer einfachen Lichtsensor-App

Dieser Abschnitt ist in zwei Unterabschnitte unterteilt: Der erste Unterabschnitt enthält die Schritte zum Erstellen einer einfachen Lichtsensoranwendung. Im zweiten Unterabschnitt wird die erstellte App dann näher erläutert.

###  Anweisungen

-   Erstellen Sie ein neues Projekt. Wählen Sie dabei unter den Projektvorlagen für **Visual C#** die Option **Leere App (Universelle Windows-App)** aus.

-   Öffnen Sie die Datei "BlankPage.xaml.cs" des Projekts, und ersetzen Sie den vorhandenen Code durch den folgenden.

```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Windows.Foundation;
    using Windows.Foundation.Collections;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Controls.Primitives;
    using Windows.UI.Xaml.Data;
    using Windows.UI.Xaml.Input;
    using Windows.UI.Xaml.Media;
    using Windows.UI.Xaml.Navigation;

    using Windows.UI.Core; // Required to access the core dispatcher object
    using Windows.Devices.Sensors; // Required to access the sensor platform and the ALS

    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/p/?linkid=234238

    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class BlankPage : Page
        {
            private LightSensor _lightsensor; // Our app' s lightsensor object
           
            // This event handler writes the current light-sensor reading to 
            // the textbox named "txtLUX" on the app' s main page.

            private void ReadingChanged(object sender, LightSensorReadingChangedEventArgs e)
            {
                Dispatcher.RunAsync(CoreDispatcherPriority.Normal, (s, a) =>
                {
                    LightSensorReading reading = (a.Context as LightSensorReadingChangedEventArgs).Reading;
                    txtLuxValue.Text = String.Format("{0,5:0.00}", reading.IlluminanceInLux);
                });
            }

            public BlankPage()
            {
                InitializeComponent();
                _lightsensor = LightSensor.GetDefault(); // Get the default light sensor object

                // Assign an event handler for the ALS reading-changed event
                if (_lightsensor != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _lightsensor.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _lightsensor.ReportInterval = reportInterval;

                    // Establish the even thandler
                    _lightsensor.ReadingChanged += new TypedEventHandler<LightSensor, LightSensorReadingChangedEventArgs>(ReadingChanged);
                }

            }

        }
    }
```

Sie müssen den Namespace im vorhergehenden Codeausschnitt durch den Namen ersetzen, den Sie für Ihr Projekt angegeben haben. Wenn Sie z. B. ein Projekt mit dem Namen **LightingCS** erstellt haben, ersetzen Sie `namespace App1` durch `namespace LightingCS`.

-   Öffnen Sie die Datei „MainPage.xaml“, und ersetzen Sie den ursprünglichen Inhalt durch den folgenden XML-Code.

```xml
    <Page
        x:Class="App1.BlankPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:App1"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid x:Name="LayoutRoot" Background="Black">
            <TextBlock HorizontalAlignment="Left" Height="44" Margin="52,38,0,0" TextWrapping="Wrap" Text="LUX Reading" VerticalAlignment="Top" Width="150"/>
            <TextBlock x:Name="txtLuxValue" HorizontalAlignment="Left" Height="44" Margin="224,38,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="217"/>


        </Grid>

    </Page>
```

Sie müssen den ersten Teil des Klassennamens im vorhergehenden Codeausschnitt durch den Namespace Ihrer App ersetzen. Wenn Sie etwa ein Projekt mit dem Namen **LightingCS** erstellt haben, ersetzen Sie `x:Class="App1.MainPage"` durch `x:Class="LightingCS.MainPage"`. Ersetzen Sie außerdem `xmlns:local="using:App1"` durch `xmlns:local="using:LightingCS"`.

-   Drücken Sie F5 oder wählen Sie **Debuggen** > **Debugging starten** aus, um die App zu erstellen, bereitzustellen und auszuführen.

Sobald die App ausgeführt wird, können Sie die Lichtsensorwerte ändern, indem das für den Sensor verfügbare Licht verändern oder die Emulatortools verwenden.

-   Beenden Sie die App, indem Sie zu Visual Studio zurückkehren und UMSCHALT+F5 drücken oder **Debuggen** > **Debugging beenden** auswählen.

###  Erläuterung

Das vorherige Beispiel zeigt, wie wenig Code Sie schreiben müssen, um Werte des Umgebungslichtsensors in Ihre App zu integrieren.

Die App stellt eine Verbindung mit dem Standardsensor in der **BlankPage**-Methode her.

```csharp
_lightsensor = LightSensor.GetDefault(); // Get the default light sensor object
```

Die App legt das Berichtsintervall in der **BlankPage**-Methode fest. Mit diesem Code wird das vom Gerät unterstützte Mindestintervall abgerufen und mit einem angeforderten Intervall von 16 Millisekunden verglichen (entspricht etwa einer Aktualisierungsrate von 60 Hz). Wenn das unterstützte Mindestintervall größer als das angeforderte Intervall ist, legt der Code den Wert auf das Minimum fest. Andernfalls wird der Wert auf das angeforderte Intervall festgelegt.

```csharp
uint minReportInterval = _lightsensor.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_lightsensor.ReportInterval = reportInterval;
```
Die neuen Lichtsensordaten werden in der **ReadingChanged**-Methode erfasst. Wenn der Sensortreiber neue Daten vom Sensor empfängt, übergibt er den Wert mithilfe dieses Ereignishandlers an Ihre App. Die App registriert diesen Ereignishandler in der folgenden Zeile.

```csharp
_lightsensor.ReadingChanged += new TypedEventHandler<LightSensor, 
LightSensorReadingChangedEventArgs>(ReadingChanged);
```

Diese neuen Werte werden in ein TextBlock-Element des XAML-Projektcodes geschrieben.

```xml
<TextBlock HorizontalAlignment="Left" Height="44" Margin="52,38,0,0" TextWrapping="Wrap" Text="LUX Reading" VerticalAlignment="Top" Width="150"/>
 <TextBlock x:Name="txtLuxValue" HorizontalAlignment="Left" Height="44" Margin="224,38,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="217"/>
```

## Verwandte Themen

* [Lichtsensorbeispiel](http://go.microsoft.com/fwlink/p/?linkid=241381)
 




<!--HONumber=Jun16_HO4-->


