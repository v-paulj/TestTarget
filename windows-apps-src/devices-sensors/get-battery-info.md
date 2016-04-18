---
ms.assetid: 90BB59FC-90FE-453E-A8DE-9315E29EB98C
Titel: Abrufen von Akkuinformationen
Beschreibung: Erfahren Sie, wie Sie mithilfe von APIs im Windows.Devices.Power-Namespace ausführliche Akkuinformationen erhalten.
---
# Abrufen von Akkuinformationen

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** Wichtige APIs **

-   [**Windows.Devices.Power**](https://msdn.microsoft.com/library/windows/apps/Dn895017)
-   [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/BR225432)

Erfahren Sie, wie Sie mithilfe von APIs im [**Windows.Devices.Power**](https://msdn.microsoft.com/library/windows/apps/Dn895017)-Namespace ausführliche Akkuinformationen erhalten In einem *Akkubericht* ([**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005)) werden der Ladezustand, die Kapazität und der Status eines Akkus oder Akkuaggregats beschrieben. In diesem Thema wird veranschaulicht, wie Ihre App Akkuberichte abrufen und über Veränderungen informiert werden kann. Die Codebeispiele stammen aus der einfachen Akku-App, die am Ende dieses Themas angegeben wird.

## Abrufen eines zusammengefassten Akkuberichts


Einige Geräte verfügen über mehr als einen Akku, und es ist nicht immer eindeutig, in welcher Form die einzelnen Akkus zur gesamten Energiekapazität des Geräts beitragen. An dieser Stelle wird die [**AggregateBattery**](https://msdn.microsoft.com/library/windows/apps/Dn895011)-Klasse verwendet. *AggregateBattery* repräsentiert alle Akkucontroller, die mit dem Gerät verbunden sind. Damit kann ein zusammengefasstes [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005)-Objekt bereitgestellt werden.

**Hinweis**  Eine [**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004)-Klasse entspricht einem Akkucontroller. Je nach Gerät kann der Controller auch am physischen Akku angeschlossen sein, und es kommt auch vor, dass er am Gehäuse des Geräts angeschlossen ist. So kann auch dann ein Akkuobjekt erstellt werden, wenn keine Akkus vorhanden sind. In anderen Fällen kann für das Akkuobjekt **null** gelten.

Wenn Sie über ein zusammengefasstes Akkuobjekt verfügen, rufen Sie [**GetReport**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.getreport) auf, um den entsprechenden [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) abzurufen.

```csharp
private void RequestAggregateBatteryReport()
{
    // Create aggregate battery object
    var aggBattery = Battery.AggregateBattery;

    // Get report
    var report = aggBattery.GetReport();

    // Update UI
    AddReportUI(BatteryReportPanel, report, aggBattery.DeviceId);
}
```

## Abrufen von individuellen Akkuberichten

Sie können auch ein [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005)-Objekt für individuelle Akkus erstellen. Verwenden Sie [**GetDeviceSelector**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.power.battery.getdeviceselector.aspx) mit der [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/BR225432)-Methode zum Abrufen einer Sammlung mit [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)-Objekten, die alle mit dem Gerät verbundenen Akkucontroller repräsentieren. Erstellen Sie anschließend mit der **Id**-Eigenschaft des gewünschten **DeviceInformation**-Objekts ein entsprechendes [**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004)-Element mit der Methode [**FromIdAsync**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.power.battery.fromidasync.aspx). Rufen Sie zuletzt [**GetReport**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.getreport) auf, um den individuellen Akkubericht abzurufen.

Dieses Beispiel zeigt, wie Sie einen Akkubericht für alle Akkus erstellen, die mit dem Gerät verbunden sind.

```csharp
async private void RequestIndividualBatteryReports()
{
    // Find batteries 
    var deviceInfo = await DeviceInformation.FindAllAsync(Battery.GetDeviceSelector());
    foreach(DeviceInformation device in deviceInfo)
    {
        try
        {
        // Create battery object
        var battery = await Battery.FromIdAsync(device.Id);

        // Get report
        var report = battery.GetReport();

        // Update UI
        AddReportUI(BatteryReportPanel, report, battery.DeviceId);
        }
        catch { /* Add error handling, as applicable */ }
    }
}
```

## Zugreifen auf Berichtdetails

Das [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005)-Objekt liefert zahlreiche Akkuinformationen. Weitere Informationen finden Sie in der API-Referenz für die Eigenschaften: **Status** (eine [**BatteryStatus**](https://msdn.microsoft.com/library/windows/apps/Dn818458)-Enumerierung), [**ChargeRateInMilliwatts**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.power.batteryreport.chargerateinmilliwatts.aspx), [**DesignCapacityInMilliwattHours**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.power.batteryreport.designcapacityinmilliwatthours.aspx), [**FullChargeCapacityInMilliwattHours**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.power.batteryreport.fullchargecapacityinmilliwatthours.aspx) und [**RemainingCapacityInMilliwattHours**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.batteryreport.remainingcapacityinmilliwatthours). Dieses Beispiel enthält einige Eigenschaften des Akkuberichts, die von der einfachen Akku-App verwendet werden. Die App wird weiter unten in diesem Thema bereitgestellt.

```csharp
...
TextBlock txt3 = new TextBlock { Text = "Charge rate (mW): " + report.ChargeRateInMilliwatts.ToString() };
TextBlock txt4 = new TextBlock { Text = "Design energy capacity (mWh): " + report.DesignCapacityInMilliwattHours.ToString() };
TextBlock txt5 = new TextBlock { Text = "Fully-charged energy capacity (mWh): " + report.FullChargeCapacityInMilliwattHours.ToString() };
TextBlock txt6 = new TextBlock { Text = "Remaining energy capacity (mWh): " + report.RemainingCapacityInMilliwattHours.ToString() };
...
...
```

## Anfordern von Berichtsaktualisierungen

Das [**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004)-Objekt löst das [**ReportUpdated**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.reportupdated)-Ereignis aus, wenn sich Ladezustand, Kapazität oder Status des Akkus ändern. Dies geschieht für Statusänderungen in der Regel sofort, und für alle anderen Änderungen in regelmäßigen Abständen. Dieses Beispiel zeigt, wie Sie die Registrierung für Aktualisierungen des Akkuberichts durchführen.

```csharp
...
Battery.AggregateBattery.ReportUpdated += AggregateBattery_ReportUpdated;
...
```

## Behandeln von Berichtsaktualisierungen

Bei einer Aktualisierung des Akkuberichts übergibt das [**ReportUpdated**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.reportupdated)-Ereignis das entsprechende [**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004)-Objekt an die Ereignishandlermethode. Dieser Ereignishandler wird jedoch nicht über den UI-Thread aufgerufen. Sie müssen das [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211)-Objekt verwenden, um UI-Änderungen aufzurufen. Dies wird im folgenden Beispiel gezeigt.

```csharp
async private void AggregateBattery_ReportUpdated(Battery sender, object args)
{
    if (reportRequested)
    {

        await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            // Clear UI
            BatteryReportPanel.Children.Clear();


            if (AggregateButton.IsChecked == true)
            {
                // Request aggregate battery report
                RequestAggregateBatteryReport();
            }
            else
            {
                // Request individual battery report
                RequestIndividualBatteryReports();
            }
        });
    }
}
```

## Beispiel: einfache Akku-App

Testen Sie diese APIs, indem Sie die folgende einfache Akku-App in Microsoft Visual Studio erstellen. Klicken Sie auf der Startseite von Visual Studio auf **Neues Projekt**, und erstellen Sie dann im Bereich mit den Vorlagen unter **Visual C# &gt; Windows &gt; Universal** eine neue App, indem Sie die Vorlage **Leere App** verwenden.

Öffnen Sie als Nächstes die Datei **MainPage.xaml**, und kopieren Sie den folgenden XML-Code in diese Datei, sodass der Originalinhalt ersetzt wird.

```xml
<Page
    x:Class="App1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" >
        <StackPanel VerticalAlignment="Center" Margin="15,30,0,0" >
            <RadioButton x:Name="AggregateButton" Content="Aggregate results" GroupName="Type" IsChecked="True" />
            <RadioButton x:Name="IndividualButton" Content="Individual results" GroupName="Type" IsChecked="False" />
        </StackPanel>
        <StackPanel Orientation="Horizontal">
        <Button x:Name="GetBatteryReportButton" 
                Content="Get battery report" 
                Margin="15,15,0,0" 
                Click="GetBatteryReport"/>
        </StackPanel>
        <StackPanel x:Name="BatteryReportPanel" Margin="15,15,0,0"/>
    </StackPanel>
</Page>
```

Wenn Ihre App nicht den Namen **App1** hat, müssen Sie den ersten Teil des Klassennamens im vorhergehenden Codeausschnitt durch den Namespace Ihrer App ersetzen. Wenn Sie z. B. ein Projekt mit dem Namen **BasicBatteryApp** erstellt haben, ersetzen Sie `x:Class="App1.MainPage"` durch `x:Class="BasicBatteryApp.MainPage"`. Außerdem sollten Sie `xmlns:local="using:App1"` durch `xmlns:local="using:BasicBatteryApp"` ersetzen.

Öffnen Sie als Nächstes die Datei **MainPage.xaml.cs** des Projekts, und ersetzen Sie den vorhandenen Code durch Folgendes.

```csharp
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
using Windows.Devices.Enumeration;
using Windows.Devices.Power;
using Windows.UI.Core;

namespace App1
{
    public sealed partial class MainPage : Page
    {
        bool reportRequested = false;
        public MainPage()
        {
            this.InitializeComponent();
            Battery.AggregateBattery.ReportUpdated += AggregateBattery_ReportUpdated;
        }


        private void GetBatteryReport(object sender, RoutedEventArgs e)
        {
            // Clear UI
            BatteryReportPanel.Children.Clear();


            if (AggregateButton.IsChecked == true)
            {
                // Request aggregate battery report
                RequestAggregateBatteryReport();
            }
            else
            {
                // Request individual battery report
                RequestIndividualBatteryReports();
            }

            // Note request
            reportRequested = true;
        }

        private void RequestAggregateBatteryReport()
        {
            // Create aggregate battery object
            var aggBattery = Battery.AggregateBattery;

            // Get report
            var report = aggBattery.GetReport();

            // Update UI
            AddReportUI(BatteryReportPanel, report, aggBattery.DeviceId);
        }

        async private void RequestIndividualBatteryReports()
        {
            // Find batteries 
            var deviceInfo = await DeviceInformation.FindAllAsync(Battery.GetDeviceSelector());
            foreach(DeviceInformation device in deviceInfo)
            {
                try
                {
                // Create battery object
                var battery = await Battery.FromIdAsync(device.Id);

                // Get report
                var report = battery.GetReport();

                // Update UI
                AddReportUI(BatteryReportPanel, report, battery.DeviceId);
                }
                catch { /* Add error handling, as applicable */ }
            }
        }


        private void AddReportUI(StackPanel sp, BatteryReport report, string DeviceID)
        {
            // Create battery report UI
            TextBlock txt1 = new TextBlock { Text = "Device ID: " + DeviceID };
            txt1.FontSize = 15;
            txt1.Margin = new Thickness(0, 15, 0, 0);
            txt1.TextWrapping = TextWrapping.WrapWholeWords;

            TextBlock txt2 = new TextBlock { Text = "Battery status: " + report.Status.ToString() };
            txt2.FontStyle = Windows.UI.Text.FontStyle.Italic;
            txt2.Margin = new Thickness(0, 0, 0, 15);

            TextBlock txt3 = new TextBlock { Text = "Charge rate (mW): " + report.ChargeRateInMilliwatts.ToString() };
            TextBlock txt4 = new TextBlock { Text = "Design energy capacity (mWh): " + report.DesignCapacityInMilliwattHours.ToString() };
            TextBlock txt5 = new TextBlock { Text = "Fully-charged energy capacity (mWh): " + report.FullChargeCapacityInMilliwattHours.ToString() };
            TextBlock txt6 = new TextBlock { Text = "Remaining energy capacity (mWh): " + report.RemainingCapacityInMilliwattHours.ToString() };

            // Create energy capacity progress bar &amp; labels
            TextBlock pbLabel = new TextBlock { Text = "Percent remaining energy capacity" };
            pbLabel.Margin = new Thickness(0,10, 0, 5);
            pbLabel.FontFamily = new FontFamily("Segoe UI");
            pbLabel.FontSize = 11;

            ProgressBar pb = new ProgressBar();
            pb.Margin = new Thickness(0, 5, 0, 0);
            pb.Width = 200;
            pb.Height = 10;
            pb.IsIndeterminate = false;
            pb.HorizontalAlignment = HorizontalAlignment.Left;

            TextBlock pbPercent = new TextBlock();
            pbPercent.Margin = new Thickness(0, 5, 0, 10);
            pbPercent.FontFamily = new FontFamily("Segoe UI");
            pbLabel.FontSize = 11;

            // Disable progress bar if values are null
            if ((report.FullChargeCapacityInMilliwattHours == null)||
                (report.RemainingCapacityInMilliwattHours == null))
            {
                pb.IsEnabled = false;
                pbPercent.Text = "N/A";
            }
            else
            {
                pb.IsEnabled = true;
                pb.Maximum = Convert.ToDouble(report.FullChargeCapacityInMilliwattHours);
                pb.Value = Convert.ToDouble(report.RemainingCapacityInMilliwattHours);
                pbPercent.Text = ((pb.Value / pb.Maximum) * 100).ToString("F2") + "%";
            }

            // Add controls to stackpanel
            sp.Children.Add(txt1);
            sp.Children.Add(txt2);
            sp.Children.Add(txt3);
            sp.Children.Add(txt4);
            sp.Children.Add(txt5);
            sp.Children.Add(txt6);
            sp.Children.Add(pbLabel);
            sp.Children.Add(pb);
            sp.Children.Add(pbPercent);
        }

        async private void AggregateBattery_ReportUpdated(Battery sender, object args)
        {
            if (reportRequested)
            {

                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    // Clear UI
                    BatteryReportPanel.Children.Clear();


                    if (AggregateButton.IsChecked == true)
                    {
                        // Request aggregate battery report
                        RequestAggregateBatteryReport();
                    }
                    else
                    {
                        // Request individual battery report
                        RequestIndividualBatteryReports();
                    }
                });
            }
        }
    }
}
```

Wenn Ihre App nicht den Namen **App1** hat, müssen Sie den Namespace im vorherigen Beispiel durch den Namen ersetzen, den Sie für Ihr Projekt vergeben haben. Wenn Sie z. B. ein Projekt mit dem Namen **BasicBatteryApp** erstellt haben, ersetzen Sie den Namespace `App1` durch den Namespace `BasicBatteryApp`.

Führen Sie schließlich Folgendes aus, um diese einfache Akku-App auszuführen: Klicken Sie im Menü **Debuggen** auf **Debuggen starten**, um die Projektmappe zu testen.

**Tipp**  Um numerische Werte über das [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005)-Objekt zu erhalten, debuggen Sie Ihre App auf dem **lokalen Computer** oder einem externen **Gerät** (z. B. einem Windows Phone). Beim Debuggen mit einem Geräteemulator gibt das **BatteryReport**-Objekt für die Kapazitäts- und Rateneigenschaften **null** zurück.

 



<!--HONumber=Mar16_HO1-->


