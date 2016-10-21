---
author: msatranjr
ms.assetid: 28B30708-FE08-4BE9-AE11-5429F963C330
title: BluetoothGATT
description: "Dieser Artikel enthält eine Übersicht über Bluetooth Generic Attribute Profile (GATT) für Universelle Windows-Plattform (UWP)-Apps zusammen mit Beispielcode für drei häufige GATT-Szenarien."
translationtype: Human Translation
ms.sourcegitcommit: 62e97bdb8feb78981244c54c76a00910a8442532
ms.openlocfilehash: 508acd449c156fa0f5b14298e4a7700748fc65bb

---
# Bluetooth GATT

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** Wichtige APIs **

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)

Dieser Artikel enthält eine Übersicht über Bluetooth Generic Attribute Profile (GATT) für Universelle Windows-Plattform (UWP)-Apps zusammen mit Beispielcode für drei häufige GATT-Szenarien: Abrufen von Bluetooth-Daten, Steuern eines Bluetooth LE-Thermometers und Steuern der Darstellung von Bluetooth LE-Gerätedaten.

## Übersicht

Entwickler können mit den APIs im [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)-Namespace auf Bluetooth LE-Dienste, -Deskriptoren und -Merkmale zugreifen. Bluetooth LE-Geräte machen ihre Funktionen über eine Sammlung folgender Elemente verfügbar:

-   Primäre Dienste
-   Enthaltene Dienste
-   Merkmale
-   Deskriptoren

Primäre Dienste definieren den Funktionsvertrag des LE-Geräts, und sie enthalten eine Sammlung der Merkmale, die den Dienst definieren. Diese Merkmale wiederum enthalten Deskriptoren, von denen die Merkmale beschrieben werden.

Die Bluetooth GATT-APIs machen Objekte und Funktionen verfügbar, statt Zugriff auf den Transport unformatierter Daten zu bieten. Auf der Treiberebene werden primäre Dienste als untergeordnete Geräteknoten des Bluetooth LE-Geräts enumeriert, das die [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459)-APIs verwendet.

Die Bluetooth GATT-APIs ermöglichen Entwicklern zudem das Arbeiten mit Bluetooth LE-Geräten. Dabei sind sie in der Lage, folgende Aufgaben auszuführen:

-   Ermitteln von Diensten/Merkmalen/Deskriptoren
-   Lesen und Schreiben von Werten für Merkmale/Deskriptoren
-   Registrieren eines Rückrufs für das ValueChanged-Ereignis des Merkmals

Die Bluetooth GATT-APIs erleichtern die Entwicklung, da häufige Eigenschaften behandelt und sinnvolle Standardwerte bereitgestellt werden, um das Verwalten und Konfigurieren von Geräten zu unterstützen. Sie bieten Entwicklern eine Möglichkeit, aus einer App heraus auf Funktionen eines Bluetooth LE-Geräts zuzugreifen.

Um eine sinnvolle Implementierung erstellen zu können, muss ein Entwickler über Vorkenntnisse bezüglich der GATT-Dienste und -Merkmale verfügen, die von der Anwendung verwendet werden sollen; zudem müssen die spezifischen Merkmalswerte auf solche Weise verarbeitet werden, dass die von der API bereitgestellten Binärdaten vor der Präsentation für den Benutzer in hilfreiche Daten umgewandelt werden. Die Bluetooth GATT-APIs machen nur die Grundtypen verfügbar, die für die Kommunikation mit einem Bluetooth LE-Gerät erforderlich sind. Zum Interpretieren der Daten muss ein Anwendungsprofil definiert werden, entweder durch ein Bluetooth SIG-Standardprofil oder mit einem benutzerdefinierten Profil, das von einem Geräteanbieter implementiert wird. Ein Profil begründet einen bindenden Vertrag zwischen Anwendung und Gerät darüber, was von den ausgetauschten Daten dargestellt wird und wie sie zu interpretieren sind.

Aus Gründen der Benutzerfreundlichkeit pflegt Bluetooth SIG eine [Liste der öffentlichen Profile](http://go.microsoft.com/fwlink/p/?LinkID=317977), die zur Verfügung stehen.

## Abrufen von Bluetooth-Daten

In diesem Beispiel nutzt die App Temperaturmessungen aus einem Bluetooth-Gerät, das den Bluetooth LE Health Thermometer Service implementiert. Die App gibt an, dass sie benachrichtigt werden soll, wenn eine neue Temperaturmessung verfügbar ist. Durch die Registrierung eines Ereignishandlers für das Ereignis zur Änderung eines Thermometermerkmals erhält die App Ereignisbenachrichtigungen über die Änderung eines Merkmalswerts, während sie im Vordergrund ausgeführt wird.

Beachten Sie Folgendes: Wenn die App unterbrochen wird, muss sie alle Geräteressourcen freigeben. Wenn sie dann wieder fortgesetzt wird, muss sie die Geräteaufzählung und -initialisierung erneut durchführen. Wenn eine Interaktion mit dem Gerät im Hintergrund gewünscht wird, werfen Sie einen Blick auf  [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx) oder [GattCharacteristicNotificationTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.gattcharacteristicnotificationtrigger.aspx). DeviceUseTrigger ist in der Regel besser geeignet für häufiger auftretenden Ereignisse, während GattCharacteristicNotificationTrigger besser für die Behandlung seltener Ereignisse geeignet ist.  

```csharp
double convertTemperatureData(byte[] temperatureData)
{
    // Read temperature data in IEEE 11703 floating point format
    // temperatureData[0] contains flags about optional data - not used
    UInt32 mantissa = ((UInt32)temperatureData[3] << 16) |
        ((UInt32)temperatureData[2] << 8) |
        ((UInt32)temperatureData[1]);

    Int32 exponent = (Int32)temperatureData[4];

    return mantissa * Math.Pow(10.0, exponent);
}

async void Initialize()
{
    var themometerServices = await Windows.Devices.Enumeration
        .DeviceInformation.FindAllAsync(GattDeviceService
            .GetDeviceSelectorFromUuid(
                GattServiceUuids.HealthThermometer),
        null);

    GattDeviceService firstThermometerService = await
        GattDeviceService.FromIdAsync(themometerServices[0].Id);

    serviceNameTextBlock.Text = "Using service: " + 
        themometerServices[0].Name;

    GattCharacteristic thermometerCharacteristic =
        firstThermometerService.GetCharacteristics(
            GattCharacteristicUuids.TemperatureMeasurement)[0];

    thermometerCharacteristic.ValueChanged += temperatureMeasurementChanged;

    await thermometerCharacteristic
        .WriteClientCharacteristicConfigurationDescriptorAsync(
            GattClientCharacteristicConfigurationDescriptorValue.Notify);
}

void temperatureMeasurementChanged(
    GattCharacteristic sender,
    GattValueChangedEventArgs eventArgs)
{
    byte[] temperatureData = new byte[eventArgs.CharacteristicValue.Length];
    Windows.Storage.Streams.DataReader.FromBuffer(
        eventArgs.CharacteristicValue).ReadBytes(temperatureData);

    var temperatureValue = convertTemperatureData(temperatureData);

    temperatureTextBlock.Text = temperatureValue.ToString();
}
```

```cpp
double MainPage::ConvertTemperatureData(
    const Array<unsigned char>^ temperatureData)
{
    unsigned mantissa = ((unsigned)temperatureData[3] << 16) |
        ((unsigned)temperatureData[2] << 8) |
        ((unsigned)temperatureData[1]);

    int exponent = (int)temperatureData[4];

    return mantissa * pow(10.0, (double)exponent);
}

void MainPage::Initialize()
{
    create_task(DeviceInformation::FindAllAsync(
        GattDeviceService::GetDeviceSelectorFromUuid(
            GattServiceUuids::HealthThermometer), 
        nullptr)).then(
            [this] (DeviceInformationCollection^ thermometerServices) 
    {
        create_task(GattDeviceService::FromIdAsync(
            thermometerServices->GetAt(0)->Id))
            .then([this] (GattDeviceService^ firstThermometerService) 
        {
            GattCharacteristic^ thermometerCharacteristic = 
                firstThermometerService->GetCharacteristics(
                    GattCharacteristicUuids::TemperatureMeasurement)
                        ->GetAt(0);

            thermometerCharacteristic->ValueChanged += 
                ref new TypedEventHandler<
                    GattCharacteristic^, 
                    GattValueChangedEventArgs^>(
                        this, &MainPage::TemperatureMeasurementChanged);

            create_task(thermometerCharacteristic->
                WriteClientCharacteristicConfigurationDescriptorAsync(
                GattClientCharacteristicConfigurationDescriptorValue
                    ::Notify));
        });
    });
}


void MainPage::TemperatureMeasurementChanged(
    GattCharacteristic^ sender,
    GattValueChangedEventArgs^ eventArgs)
{
    auto temperatureData =  ref new Array<unsigned char>(
        eventArgs->CharacteristicValue->Length);
    DataReader::FromBuffer(eventArgs->CharacteristicValue)
        ->ReadBytes(temperatureData);

    double temperatureValue = ConvertTemperatureData(temperatureData);
    std::wstringstream str;
    str << temperatureValue;

    temperatureTextBlock->Text = ref new String(str.str().c_str());
}
```

## Steuern eines BluetoothLE-Thermometers

In diesem Beispiel dient eine UWP-App (Universelle Windows-Plattform) als Controller für ein fiktives BluetoothLE-Thermometer. Das Gerät deklariert außerdem ein Formatmerkmal, anhand dessen der Benutzer den Wert in Celsius oder Fahrenheit abrufen kann, zusätzlich zu den Standardmerkmalen des [**HealthThermometer**](https://msdn.microsoft.com/library/windows/apps/Dn297603)-Profils. Die App verwendet zuverlässige Schreibtransaktionen, um sicherzustellen, dass das Format und das Messintervall als einzelner Wert festgelegt werden.

```csharp
// Uuid of the "Format" Characteristic Value
Guid formatCharacteristicUuid = 
    new Guid("{00000000-0000-0000-0000-000000000010}");

// Constant representing a Fahrenheit scale temperature measurement
const byte FahrenheitReading = 1;
async void Initialize()
{
    var themometerServices = await Windows.Devices.Enumeration
        .DeviceInformation.FindAllAsync(GattDeviceService
            .GetDeviceSelectorFromUuid(
                GattServiceUuids.HealthThermometer),
        null);

    GattDeviceService thermometerService = await
        GattDeviceService.FromIdAsync(themometerServices[0].Id);

    serviceNameTextBlock.Text = "Using service: " + 
        themometerServices[0].Name;

    GattCharacteristic intervalCharacteristic = thermometerService
        .GetCharacteristics(GattCharacteristicUuids.MeasurementInterval)[0];

    GattCharacteristic formatCharacteristic = thermometerService
        .GetCharacteristics(formatCharacteristicUuid)[0];

    GattReliableWriteTransaction gattTransaction = 
        new GattReliableWriteTransaction();

    var writer = new Windows.Storage.Streams.DataWriter();

    // Get a temperature reading every 60 seconds
    writer.WriteUInt16(60);

    gattTransaction.WriteValue(
        intervalCharacteristic, 
        writer.DetachBuffer());

    // Get the reading on the Fahrenheit scale
    writer.WriteByte(FahrenheitReading);

    gattTransaction.WriteValue(
        formatCharacteristic, 
        writer.DetachBuffer());

    GattCommunicationStatus status = await gattTransaction.CommitAsync();

    if (GattCommunicationStatus.Unreachable == status)
    {
        statusTextBlock.Text = "Writing to your device failed !";
    }
}
```

```cpp
// Uuid of the "Format" Characteristic Value
Guid formatCharacteristicUuid(0x00000000, 0x0000, 0x0000, 0x00, 0x00, 
                                  0x00, 0x00, 0x00, 0x00, 0x00, 0x10);

// Constant representing a Fahrenheit scale temperature measurement
const unsigned char FAHRENHEIT_READING = 1;

void MainPage::Initialize()
{
    create_task(DeviceInformation::FindAllAsync(
        GattDeviceService::GetDeviceSelectorFromUuid(
            GattServiceUuids::HealthThermometer), 
        nullptr)).then(
            [this] (DeviceInformationCollection^ thermometerServices) 
    {
        create_task(GattDeviceService::FromIdAsync(
            thermometerServices->GetAt(0)->Id)).then([this] (
                GattDeviceService^ thermometerService) 
        {
            GattCharacteristic^ intervalCharacteristic = 
                thermometerService->GetCharacteristics(
                    GattCharacteristicUuids::MeasurementInterval)
                        ->GetAt(0);

            GattCharacteristic^ formatCharacteristic = 
                thermometerService->GetCharacteristics(
                    formatCharacteristicUuid)->GetAt(0);

            GattReliableWriteTransaction^ gattTransaction = 
                ref new GattReliableWriteTransaction();

            DataWriter^ writer = ref new DataWriter();

            // Get a temperature reading every 60 seconds
            writer->WriteUInt16(60);

            gattTransaction->WriteValue(
                intervalCharacteristic, 
                writer->DetachBuffer());

            writer->WriteByte(FAHRENHEIT_READING);

            gattTransaction->WriteValue(
                formatCharacteristic, 
                writer->DetachBuffer());

            create_task(gattTransaction->CommitAsync())
                .then([this] (GattCommunicationStatus status) 
            {
                if (GattCommunicationStatus::Unreachable == status) 
                { 
                    statusTextBlock->Text = 
                        ref new String(L"Writing to your device failed !");
                }
            });
        });
    });

```

## Steuern der Darstellung von BluetoothLE-Gerätedaten

Ein BluetoothLE-Gerät kann einen Akkudienst verfügbar machen, der dem Benutzer den aktuellen Akkustand anzeigt. Der Akkudienst enthält einen optionalen [**PresentationFormats**](https://msdn.microsoft.com/library/windows/apps/Dn263742)-Deskriptor, der eine gewisse Flexibilität bei der Interpretation der Akkustanddaten ermöglicht. Dieses Szenario stellt ein Beispiel für eine App bereit, die mit einem solchen Gerät ausgeführt wird und die **PresentationFormats**-Eigenschaft verwendet, um einen Merkmalswert zu formatieren, bevor er dem Benutzer angezeigt wird.

```csharp
async void Initialize()
{
    var batteryServices = await Windows.Devices.Enumeration
        .DeviceInformation.FindAllAsync(GattDeviceService
            .GetDeviceSelectorFromUuid(GattServiceUuids.Battery),
        null);

    if (batteryServices.Count > 0)
    {
        // Use the first Battery service on the system
        GattDeviceService batteryService = await GattDeviceService
            .FromIdAsync(batteryServices[0].Id);

        // Use the first Characteristic of that Service
        GattCharacteristic batteryLevelCharacteristic =
            batteryService.GetCharacteristics(
                GattCharacteristicUuids.BatteryLevel)[0];

        batteryLevelCharacteristic.ValueChanged += batteryLevelChanged;
    }
    else
    {
        statusTextBlock.Text = "No Battery services found !";
    }
}

void batteryLevelChanged(
    GattCharacteristic sender,
    GattValueChangedEventArgs eventArgs)
{
    byte levelData = Windows.Storage.Streams.DataReader
        .FromBuffer(eventArgs.CharacteristicValue).ReadByte();

    double levelValue;

    if (sender.PresentationFormats.Count > 0)
    {
        levelValue = levelData * 
            Math.Pow(10.0, sender.PresentationFormats[0].Exponent);
    }
    else
    {
        levelValue = (double)levelData;
    }

    batteryLevelTextBlock.Text = levelValue.ToString();
}
```

```cpp
void MainPage::Initialize()
{
    create_task(DeviceInformation::FindAllAsync(
        GattDeviceService::GetDeviceSelectorFromUuid(
            GattServiceUuids::Battery), 
        nullptr)).then([this] (DeviceInformationCollection^ batteryServices) 
    {
        create_task(GattDeviceService::FromIdAsync(
            batteryServices->GetAt(0)->Id)).then([this] (
                GattDeviceService^ batteryService) 
        {
            GattCharacteristic^ batteryLevelCharacteristic = 
                batteryService->GetCharacteristics(
                    GattCharacteristicUuids::BatteryLevel)->GetAt(0);

            batteryLevelCharacteristic->ValueChanged += 
                ref new TypedEventHandler<
                    GattCharacteristic^, 
                    GattValueChangedEventArgs^>
                    (this, &MainPage::BatteryLevelChanged);

            create_task(batteryLevelCharacteristic
                ->WriteClientCharacteristicConfigurationDescriptorAsync(
                GattClientCharacteristicConfigurationDescriptorValue
                    ::Notify));
        });
    });
}

void MainPage::BatteryLevelChanged(
    GattCharacteristic^ sender,
    GattValueChangedEventArgs^ eventArgs)
{
    unsigned char batteryLevelData = DataReader::FromBuffer(
        eventArgs->CharacteristicValue)->ReadByte();

    // if this characteristic has a presentation format
    // use that information to format the value
    double batteryLevelValue;
    if (sender->PresentationFormats->Size > 0)
    {
        batteryLevelValue = batteryLevelData * 
            pow(10.0, sender->PresentationFormats->GetAt(0)->Exponent);
    }
    else
    {
        batteryLevelValue = batteryLevelData;
    }

    std::wstringstream str;
    str << batteryLevelValue;
    batteryLevelTextBlock->Text = 
        ref new String(str.str().c_str());
}
```






<!--HONumber=Aug16_HO3-->


