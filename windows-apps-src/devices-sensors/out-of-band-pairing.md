---
author: IvorB
ms.assetid: E9ADC88F-BD4F-4721-8893-0E19EA94C8BA
title: Out-of-Band-Kopplung
description: Mithilfe der Out-of-Band-Kopplung können Apps ohne Geräteermittlung eine Verbindung mit einem POS-Peripheriegerät (Point-of-Service) herstellen.
---
# Out-of-Band-Kopplung

Mithilfe der Out-of-Band-Kopplung können Apps ohne Geräteermittlung eine Verbindung mit einem POS-Peripheriegerät (Point-of-Service) herstellen. Apps müssen den [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/windows.devices.pointofservice.aspx)-Namespace verwenden und eine speziell formatierte Zeichenfolge (Out-of-Band-Blob) an die entsprechende **FromIdAsync**-Methode für das gewünschte Peripheriegerät übergeben. Bei Ausführung von **FromIdAsync** wird das Hostgerät mit dem Peripheriegerät gekoppelt und verbunden, bevor der Vorgang an den Aufrufer zurückgegeben wird.

## Out-of-Band-Blob-Format

```json
    "connectionKind":"Network",
    "physicalAddress":"AA:BB:CC:DD:EE:FF",
    "connectionString":"192.168.1.1:9001",
    "peripheralKinds":"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}",
    "providerId":"{02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2}",
    "providerName":"PrinterProtocolProvider.dll"
```

**connectionKind** – die Art der Verbindung. Gültige Werte sind „Network“ und „Bluetooth“.

**physicalAddress** – die MAC-Adresse des Peripheriegeräts. Bei einem Netzwerkdrucker wäre dies z. B. die MAC-Adresse, die vom Testblatt des Druckers im Format AA:BB:CC:DD:EE:FF bereitgestellt wird.

**connectionString** – die Verbindungszeichenfolge des Peripheriegeräts. Bei einem Netzwerkdrucker wäre dies z. B. die auf dem Testblatt im Format 192.168.1.1:9001 ausgegebene IP-Adresse. Dieses Feld wird bei allen Bluetooth-Peripheriegeräten weggelassen.

**peripheralKinds** – Die GUID für den Gerätetyp. Gültige Werte sind:

| Gerätetyp | GUID |
| ---- | ---- |
| *POS-Drucker* | C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33 |
| *Strichcodescanner* | C243FFBD-3AFC-45E9-B3D3-2BA18BC7EBC5 |
| *Kassenschublade* | 772E18F2-8925-4229-A5AC-6453CB482FDA |


**providerId** – die GUID für die Protokollanbieterklasse. Gültige Werte sind:

| Protokollanbieterklasse | GUID |
| ---- | ---- |
| *Generische ESC/POS-Netzwerkdrucker* | 02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2 |
| *Generische ESC/POS-Bluetooth-Drucker* | CCD5B810-95B9-4320-BA7E-78C223CAF418 |
| *Epson-Bluetooth-Drucker* | 94917594-544F-4AF8-B53B-EC6D9F8A4464 |
| *Epson-Netzwerkdrucker* | 9F0F8BE3-4E59-4520-BFBA-AF77614A31CE |
| *Star-Netzwerkdrucker* | 1E3A32C2-F411-4B8C-AC91-CC2C5FD21996 |
| *Socket-Bluetooth-Scanner* | 6E7C8178-A006-405E-85C3-084244885AD2 |
| *APG-Netzwerkschublade* | E619E2FE-9489-4C74-BF57-70AED670B9B0 |
| *APG-Bluetooth-Schublade* | 332E6550-2E01-42EB-9401-C6A112D80185 |


**providerName** – die DLL mit dem Namen des Anbieters. Die Standardanbieter sind:

| Anbieter | DLL-Name |
| ---- | ---- |
| Drucker | PrinterProtocolProvider.dll |
| Kassenschublade | CashDrawerProtocolProvider.dll |
| Scanner | BarcodeScannerProtocolProvider.dll |

## Verwendungsbeispiel: Netzwerkdrucker

```csharp
String oobBlobNetworkPrinter =
  "{\"connectionKind\":\"Network\"," +
  "\"physicalAddress\":\"AA:BB:CC:DD:EE:FF\"," +
  "\"connectionString\":\"192.168.1.1:9001\"," +
  "\"peripheralKinds\":\"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}\"," +
  "\"providerId\":\"{02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2}\"," +
  "\"providerName\":\"PrinterProtocolProvider.dll\"}";

printer = await PosPrinter.FromIdAsync(oobBlobNetworkPrinter);
```

## Verwendungsbeispiel: Bluetooth-Drucker

```csharp
string oobBlobBTPrinter =
    "{\"connectionKind\":\"Bluetooth\"," +
    "\"physicalAddress\":\"AA:BB:CC:DD:EE:FF\"," +
    "\"peripheralKinds\":\"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}\"," +
    "\"providerId\":\"{CCD5B810-95B9-4320-BA7E-78C223CAF418}\"," +
    "\"providerName\":\"PrinterProtocolProvider.dll\"}";

printer = await PosPrinter.FromIdAsync(oobBlobBTPrinter);

```


<!--HONumber=May16_HO2-->


