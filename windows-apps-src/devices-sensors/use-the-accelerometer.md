---
author: DBirtolo
ms.assetid: F90686F5-641A-42D9-BC44-EC6CA11B8A42
title: Verwenden des Beschleunigungsmessers
description: "Hier erfahren Sie, wie Sie mithilfe des Beschleunigungsmessers auf Benutzerbewegungen reagieren können."
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 8ce3baf2b030096ae5cfc56f31b97ec58e138a44

---
# Verwenden des Beschleunigungsmessers

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** Wichtige APIs **

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Beschleunigungsmesser**](https://msdn.microsoft.com/library/windows/apps/BR225687)

\[Einige Informationen beziehen sich auf die Vorabversion, die vor der kommerziellen Freigabe möglicherweise wesentlichen Änderungen unterliegt. Microsoft übernimmt keine Garantie, weder ausdrücklicher noch impliziter Art, für die hier bereitgestellten Informationen.\]

Hier erfahren Sie, wie Sie mithilfe des Beschleunigungsmessers auf Benutzerbewegungen reagieren können.

Eine einfache Spiele-App verwendet als Eingabegerät einen einzigen Sensor: den Beschleunigungsmesser. Diese Apps verwenden für die Eingabe in der Regel nur eine oder zwei Achsen. Als weitere Eingabequelle kann aber auch das Schüttelereignis verwendet werden.

## Voraussetzungen

Sie sollten mit XAML (Extensible Application Markup Language), Microsoft VisualC# und Ereignissen vertraut sein.

Das verwendete Gerät oder der Emulator muss einen Beschleunigungsmesser unterstützen.

## Erstellen einer einfachen Beschleunigungsmesser-App

Dieser Abschnitt ist in zwei Unterabschnitte unterteilt: Der erste Unterabschnitt enthält die Schritte zum Erstellen einer einfachen Beschleunigungsmesseranwendung. Im zweiten Unterabschnitt wird die erstellte App dann näher erläutert.

### Anweisungen

-   Erstellen Sie ein neues Projekt. Wählen Sie dabei unter den Projektvorlagen für **VisualC#** die Option **Leere App (Universelle Windows-App)** aus.

-   Öffnen Sie die Projektdatei „MainPage.xaml.cs“, und ersetzen Sie den vorhandenen Code durch den folgenden Code.

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

    // Required to support the core dispatcher and the accelerometer

    using Windows.UI.Core;
    using Windows.Devices.Sensors;

    namespace App1
    {

        public sealed partial class MainPage : Page
        {
            // Sensor and dispatcher variables
            private Accelerometer _accelerometer;

            // This event handler writes the current accelerometer reading to 
            // the three acceleration text blocks on the app' s main page.

            private async void ReadingChanged(object sender, AccelerometerReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    AccelerometerReading reading = e.Reading;
                    txtXAxis.Text = String.Format("{0,5:0.00}", reading.AccelerationX);
                    txtYAxis.Text = String.Format("{0,5:0.00}", reading.AccelerationY);
                    txtZAxis.Text = String.Format("{0,5:0.00}", reading.AccelerationZ);

                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _accelerometer = Accelerometer.GetDefault();

                if (_accelerometer != null)
                {
                    // Establish the report interval
                    uint minReportInterval = _accelerometer.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _accelerometer.ReportInterval = reportInterval;

                    // Assign an event handler for the reading-changed event
                    _accelerometer.ReadingChanged += new TypedEventHandler<Accelerometer, AccelerometerReadingChangedEventArgs>(ReadingChanged);
                }
            }
        }
    }
```

Sie müssen den Namespace im vorhergehenden Codeausschnitt durch den Namen ersetzen, den Sie für Ihr Projekt angegeben haben. Wenn Sie z.B. ein Projekt mit dem Namen **AccelerometerCS** erstellt haben, ersetzen Sie `namespace App1` durch `namespace AccelerometerCS`.

-   Öffnen Sie die Datei „MainPage.xaml“, und ersetzen Sie den ursprünglichen Inhalt durch den folgenden XML-Code.

```xml
        <Page
        x:Class="App1.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:App1"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid x:Name="LayoutRoot" Background="#FF0C0C0C">
            <TextBlock HorizontalAlignment="Left" Height="25" Margin="8,20,0,0" TextWrapping="Wrap" Text="X-axis:" VerticalAlignment="Top" Width="62" Foreground="#FFEDE6E6"/>
            <TextBlock HorizontalAlignment="Left" Height="27" Margin="8,49,0,0" TextWrapping="Wrap" Text="Y-axis:" VerticalAlignment="Top" Width="62" Foreground="#FFF5F2F2"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="8,80,0,0" TextWrapping="Wrap" Text="Z-axis:" VerticalAlignment="Top" Width="62" Foreground="#FFF6F0F0"/>
            <TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="15" Margin="70,16,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="61" Foreground="#FFF2F2F2"/>
            <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="15" Margin="70,49,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53" Foreground="#FFF2EEEE"/>
            <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="15" Margin="70,80,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53" Foreground="#FFFFF8F8"/>

        </Grid>
    </Page>
```

Sie müssen den ersten Teil des Klassennamens im vorhergehenden Codeausschnitt durch den Namespace Ihrer App ersetzen. Wenn Sie etwa ein Projekt mit dem Namen **AccelerometerCS**erstellt haben, ersetzen Sie `x:Class="App1.MainPage"` durch `x:Class="AccelerometerCS.MainPage"`. Ersetzen Sie außerdem `xmlns:local="using:App1"` durch `xmlns:local="using:AccelerometerCS"`.

-   Drücken Sie F5, oder wählen Sie **Debuggen** &gt; **Debugging starten** aus, um die App zu erstellen, bereitzustellen und auszuführen.

Wenn die App ausgeführt wird, können Sie die Beschleunigungsmesserwerte ändern, indem Sie das Gerät bewegen oder die Emulatortools verwenden.

-   Beenden Sie die App, indem Sie zu Visual Studio zurückkehren und UMSCHALT+F5 drücken oder **Debuggen** &gt; **Debugging beenden** auswählen.

### Erläuterung

Das vorherige Beispiel zeigt, wie wenig Code Sie schreiben müssen, um Beschleunigungsmesserwerte in Ihre App zu integrieren.

Die App stellt eine Verbindung mit dem Standardbeschleunigungsmesser in der **MainPage**-Methode her.

```csharp
_accelerometer = Accelerometer.GetDefault();
```

Die App legt das Berichtsintervall in der **MainPage**-Methode fest. Mit diesem Code wird das vom Gerät unterstützte Mindestintervall abgerufen und mit einem angeforderten Intervall von 16 Millisekunden verglichen (entspricht etwa einer Aktualisierungsrate von 60Hz). Wenn das unterstützte Mindestintervall größer als das angeforderte Intervall ist, legt der Code den Wert auf das Minimum fest. Andernfalls wird der Wert auf das angeforderte Intervall festgelegt.

```csharp
uint minReportInterval = _accelerometer.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_accelerometer.ReportInterval = reportInterval;
```

Die neuen Beschleunigungsmesserdaten werden in der **ReadingChanged**-Methode erfasst. Wenn der Sensortreiber neue Daten vom Sensor empfängt, übergibt er die Werte mithilfe dieses Ereignishandlers an Ihre App. Die App registriert diesen Ereignishandler in der folgenden Zeile.

```csharp
_accelerometer.ReadingChanged += new TypedEventHandler<Accelerometer, 
AccelerometerReadingChangedEventArgs>(ReadingChanged);
```

Die neuen Werte werden in die TextBlock-Elemente des XAML-Projektcodes geschrieben.

```xml
<TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="15" Margin="70,16,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="61" Foreground="#FFF2F2F2"/>
 <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="15" Margin="70,49,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53" Foreground="#FFF2EEEE"/>
 <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="15" Margin="70,80,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53" Foreground="#FFFFF8F8"/>
```
## Verwandte Themen

* [Beschleunigungsmesserbeispiel](http://go.microsoft.com/fwlink/p/?linkid=241377)




<!--HONumber=Aug16_HO3-->


