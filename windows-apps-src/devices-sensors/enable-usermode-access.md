---
author: JordanRh1
title: Benutzermoduszugriff auf Windows 10 IoT Core aktivieren
description: In diesem Lernprogramm wird der Benutzermoduszugriff auf GPIO, I2C, SPI und UART auf Windows 10 IoT Core beschrieben.
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: eddb2ca0aaa4bdbc19b2c3015ec8d599e0ef5584

---
# Benutzermoduszugriff auf Windows 10 IoT Core aktivieren

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Windows 10 IoT Core enthält neue APIs für den Zugriff auf GPIO, I2C, SPI und UART direkt aus Benutzermodus. Platinen für die Entwicklung, z.B. Raspberry Pi 2, zeigen eine Teilmenge dieser Verbindungen, mit denen Benutzer ein grundlegendes Berechnungsmodell mit benutzerdefiniertem Schaltkreis auf eine bestimmte Anwendung erweitern können. Diese Low-Level Busse werden in der Regel mit nur einer Teilmenge der GPIO-Pins und -Busse in den Headern für andere wichtige integrierte Funktionen freigegeben. Um die Systemstabilität zu erhalten, ist es erforderlich, anzugeben, welche Pins und Busse zur Änderung durch Benutzermodusanwendungen sicher sind. 

In diesem Dokument wird beschrieben, wie diese Konfiguration in ACPI angegeben wird, und es enthält Tools, um zu überprüfen, dass die Konfiguration richtig angegeben wurde. 

> [!IMPORTANT]
> Die Zielgruppe für dieses Dokument sind UEFI- und ACPI-Entwickler. Grundkenntnisse mit ACPI, dem Erstellen von ASL und SpbCx/GpioClx werden angenommen.

Benutzermoduszugriff auf Low-Level-Busse unter Windows wird über die vorhandenen `GpioClx`- und `SpbCx`-Frameworks konfiguriert. Ein neuer Treiber namens *RhProxy*, nur für Windows 10 IoT Core verfügbar, macht die `GpioClx`- und `SpbCx`-Ressourcen für den Benutzermodus verfügbar. Um die APIs zu aktivieren, muss ein Geräteknoten für rhproxy in Ihren ACPI-Tabellen für die einzelnen GPIO- und SPB-Ressourcen deklariert werden, die für den Benutzermodus verfügbar gemacht werden soll. Dieses Dokument führt Sie durch die Erstellung und Überprüfung von ASL. 


## ASL anhand eines Beispiels

Betrachten wir die rhproxy-Geräteknotendeklaration auf Raspberry Pi 2. Erstellen Sie zunächst die ACPI-Gerätedeklaration im Bereich \\_SB.  

```cpp
Device(RHPX) 
{ 
    Name(_HID, "MSFT8000") 
    Name(_CID, "MSFT8000") 
    Name(_UID, 1) 
    
```

* _HID – Hardware-ID. Legen Sie hier eine herstellerspezifische Hardware-ID fest. 
* _CID – Kompatible ID. Muss „MSFT8000“ sein.  
* _UID – Eindeutige ID. Legen Sie den Wert auf 1 fest.  

Als Nächstes deklarieren wir alle GPIO- und SPB-Ressourcen, die für den Benutzermodus verfügbar gemacht werden soll. Die Reihenfolge, in der Ressourcen deklariert werden, ist wichtig, da Ressourcenindizes verwendet werden, um Ressourcen Eigenschaften zuordnen. Sind mehrere I2C- oder SPI-Busse verfügbar gemacht, gilt der erste deklarierte Bus als „Standard“-Bus für diesen Typ und ist die Instanz, die durch die `GetDefaultAsync()`-Methoden der [Windows.Devices.I2c.I2cController](https://msdn.microsoft.com/library/windows/apps/windows.devices.i2c.i2ccontroller.aspx) und [Windows.Devices.Spi.SpiController](https://msdn.microsoft.com/library/windows/apps/windows.devices.spi.spicontroller.aspx) zurückgegeben wird. 

### SPI 

Raspberry Pi verfügt über zwei verfügbar gemachte SPI-Busse. SPI0 hat zwei Hardware-Chip-Auswahlzeilen und SPI1 verfügt über eine Hardware-Chip-Auswahlzeile. Eine SPISerialBus()-Ressourcendeklaration ist für jede Chip-Auswahlzeile für jeden Bus erforderlich. Die folgenden zwei SPISerialBus-Ressourcendeklarationen beziehen sich auf die zwei Chip-Auswahlzeilen in SPI0. Das DeviceSelection-Feld enthält einen eindeutigen Wert, das der Treiber als Hardware-Chip-Auswahlzeilenidentifikator interpretiert. Der genaue Wert, den Sie im DeviceSelection-Feld eingeben, hängt davon ab, wie Ihr Treiber dieses Feld des ACPI-Verbindungsdeskriptors interpretiert.  

```cpp
// Index 0 
SPISerialBus(              // SCKL - GPIO 11 - Pin 23 
                           // MOSI - GPIO 10 - Pin 19 
                           // MISO - GPIO 9  - Pin 21 
                           // CE0  - GPIO 8  - Pin 24 
    0,                     // Device selection (CE0) 
    PolarityLow,           // Device selection polarity 
    FourWireMode,          // wiremode 
    0,                     // databit len: placeholder 
    ControllerInitiated,   // slave mode 
    0,                     // connection speed: placeholder 
    ClockPolarityLow,      // clock polarity: placeholder 
    ClockPhaseFirst,       // clock phase: placeholder 
    "\\_SB.SPI0",          // ResourceSource: SPI bus controller name 
    0,                     // ResourceSourceIndex 
                           // Resource usage 
    )                      // Vendor Data 

// Index 1 
SPISerialBus(              // SCKL - GPIO 11 - Pin 23 
                           // MOSI - GPIO 10 - Pin 19 
                           // MISO - GPIO 9  - Pin 21 
                           // CE1  - GPIO 7  - Pin 26 
    1,                     // Device selection (CE1) 
    PolarityLow,           // Device selection polarity 
    FourWireMode,          // wiremode 
    0,                     // databit len: placeholder 
    ControllerInitiated,   // slave mode 
    0,                     // connection speed: placeholder 
    ClockPolarityLow,      // clock polarity: placeholder 
    ClockPhaseFirst,       // clock phase: placeholder 
    "\\_SB.SPI0",          // ResourceSource: SPI bus controller name 
    0,                     // ResourceSourceIndex 
                           // Resource usage 
    )                      // Vendor Data 

```

Woher weiß Software, dass diese beiden Ressourcen demselben Bus zugeordnet werden sollen? Die Zuordnung zwischen Bus-Anzeigenamen und Ressourcenindex wird in der DSD angegeben:  

```cpp
Package(2) { "bus-SPI-SPI0", Package() { 0, 1 }}, 
```

Dadurch wird einen Bus mit dem Namen „SPI0“ und zwei Chip-Auswahlzeilen erstellt – Ressourcenindizes 0 und 1. Einige weitere Eigenschaften sind erforderlich, um die SPI-Bus-Funktionen zu deklarieren.  

```cpp
Package(2) { "SPI0-MinClockInHz", 7629 }, 
Package(2) { "SPI0-MaxClockInHz", 125000000 },
```

Die **MinClockInHz**- und **MaxClockInHz**-Eigenschaften geben die minimalen und maximalen Taktfrequenzen an, die vom Controller unterstützt werden. Die API verhindert, dass Benutzer Werte außerhalb dieses Bereichs angeben. Die Taktfrequenz wird an Ihren SPB-Treiber im _SPE-Feld der Verbindungsbeschreibung (ACPI-Abschnitt 6.4.3.8.2.2) übergeben.  

```cpp
Package(2) { "SPI0-SupportedDataBitLengths", Package() { 8 }}, 
```

Die **SupportedDataBitLengths**-Eigenschaft listet die Datenbitlängen auf, die vom Controller unterstützt wird. Mehrere Werte können in einer kommagetrennten Liste angegeben werden. Die API verhindert, dass Benutzer Werte außerhalb dieser Liste angeben. Die Datenbitlänge wird an Ihren SPB-Treiber im _LEN-Feld der Verbindungsbeschreibung (ACPI Abschnitt 6.4.3.8.2.2) übergeben.  

Sie können sich diese Ressourcedeklarationen als „Vorlagen“ vorstellen. Einige der Felder werden beim Systemstart behoben, während andere dynamisch zur Laufzeit angegeben werden. Die folgenden Felder der SPISerialBus-Beschreibung werden korrigiert: 

* DeviceSelection 
* DeviceSelectionPolarity 
* WireMode 
* SlaveMode 
* ResourceSource 

Die folgenden Felder sind Platzhalter für Werte, die vom Benutzer zur Laufzeit angegeben werden: 

* DataBitLength 
* ConnectionSpeed 
* ClockPolarity 
* ClockPhase 

Da SPI1 nur ein Chip-Auswahlzeile enthält, wird eine einzelne `SPISerialBus()`-Ressource deklariert: 

```cpp
// Index 2 

SPISerialBus(              // SCKL - GPIO 21 - Pin 40 
                           // MOSI - GPIO 20 - Pin 38 
                           // MISO - GPIO 19 - Pin 35 
                           // CE1  - GPIO 17 - Pin 11 
    1,                     // Device selection (CE1) 
    PolarityLow,           // Device selection polarity 
    FourWireMode,          // wiremode 
    0,                     // databit len: placeholder 
    ControllerInitiated,   // slave mode 
    0,                     // connection speed: placeholder 
    ClockPolarityLow,      // clock polarity: placeholder 
    ClockPhaseFirst,       // clock phase: placeholder 
    "\\_SB.SPI1",          // ResourceSource: SPI bus controller name 
    0,                     // ResourceSourceIndex 
                           // Resource usage 
    )                      // Vendor Data 

```

Die zugehörigen Anzeigenamendeklaration – die erforderlich ist – wird in der DSD angegeben und verweist auf den Index dieser Ressourcendeklaration. 

```cpp
Package(2) { "bus-SPI-SPI1", Package() { 2 }}, 
```

Dadurch wird ein Bus mit dem Namen „SPI1“ erstellt und dem Ressourcenindex 2 zugeordnet.  

#### SPI-Treiberanforderungen 

* Muss `SpbCx` verwenden oder SpbCx-kompatibel sein 
* Muss die [MITT SPI-Tests](https://msdn.microsoft.com/library/windows/hardware/dn919873.aspx) bestanden haben
* Muss eine Taktfrequenz von 4Mhz unterstützen 
* Muss 8-Bit-Datenlänge unterstützen 
* Muss alle SPI-Modi unterstützen: 0, 1, 2, 3 

### I2C 

Als Nächstes deklarieren wir die I2C-Ressourcen. Raspberry Pi macht einen einzelnen I2C-Bus auf den Pins 3 und 5 verfügbar. 

```cpp
// Index 3 
I2CSerialBus(              // Pin 3 (GPIO2, SDA1), 5 (GPIO3, SCL1) 
    0xFFFF,                // SlaveAddress: placeholder 
    ,                      // SlaveMode: default to ControllerInitiated 
    0,                     // ConnectionSpeed: placeholder 
    ,                      // Addressing Mode: placeholder 
    "\\_SB.I2C1",          // ResourceSource: I2C bus controller name 
    , 
    , 
    )                      // VendorData 

```

Die zugehörige Anzeigenamendeklaration – die erforderlich ist – wird in der DSD angegeben: 

```cpp
Package(2) { "bus-I2C-I2C1", Package() { 3 }}, 
```

Dadurch wird ein I2C-Bus mit Anzeigenamen „I2C1“ deklariert, der sich auf Ressourcenindex 3 bezieht, der der Index der I2CSerialBus()-Ressource ist, die wir soeben deklariert haben. 

Die folgenden Felder der I2CSerialBus()-Beschreibung werden korrigiert: 

* SlaveMode 
* ResourceSource 

Die folgenden Felder sind Platzhalter für Werte, die vom Benutzer zur Laufzeit angegeben werden. 

* SlaveAddress 
* ConnectionSpeed 
* AddressingMode 

#### I2C-Treiberanforderungen 

* Muss SpbCx verwenden oder SpbCx-kompatibel sein 
* Muss die [MITT I2C-Tests](https://msdn.microsoft.com/library/windows/hardware/dn919852.aspx) bestanden haben 
* Muss 7-Bit-Adressierung unterstützen 
* Muss 100-kHz-Taktfrequenz unterstützen 
* Muss 400-kHz-Taktfrequenz unterstützen 

### GPIO 

Als Nächstes deklarieren wir die GPIO-Pins, die für den Benutzermodus verfügbar gemacht werden. Wir bieten die folgenden Richtlinien für die Entscheidung an, welche Pins verfügbar gemacht werden: 

* Deklarieren Sie alle Pins in verfügbar gemachten Headern. 
* Deklarieren Sie Pins, die mit integrierten Funktionen wie Schaltflächen und LEDs verbunden sind. 
* Deklarieren Sie keine Pins, die für Systemfunktionen reserviert sind oder gar nicht verbunden sind. 

Im folgenden ASL-Block werden zwei Pins deklariert – GPIO4 und GPIO5. Die anderen Pins werden aus Platzgründen nicht hier aufgeführt. AnhangC enthält ein Beispiel für das Powershell-Skript, das verwendet werden kann, um die GPIO-Ressourcen zu generieren. 

```cpp
// Index 4 – GPIO 4 
GpioIO(Shared, PullUp, , , , “\\_SB.GPI0”, , , , ) { 4 } 
GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, “\\_SB.GPI0”,) { 4 } 

// Index 6 – GPIO 5 
GpioIO(Shared, PullUp, , , , “\\_SB.GPI0”, , , , ) { 5 } 
GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, “\\_SB.GPI0”,) { 5 } 
```

Bei der Deklaration von GPIO-Pins müssen die folgenden Anforderungen beachtet werden: 

* Nur im Speicher abgebildete GPIO-Controller werden unterstützt. Über I2C/SPIGPIO verbundene Controller werden nicht unterstützt. Der Treiber für den Controller ist ein im Speicher abgebildeter Controller, wenn mit ihm das [MemoryMappedController](https://msdn.microsoft.com/library/windows/hardware/hh439449.aspx)-Flag in der Struktur [CLIENT_CONTROLLER_BASIC_INFORMATION](https://msdn.microsoft.com/library/windows/hardware/hh439358.aspx) als Antwort auf den [CLIENT_QueryControllerBasicInformation](https://msdn.microsoft.com/library/windows/hardware/hh439399.aspx)-Rückruf festgelegt wird. 
* Jeder Pin erfordert eine GpioIO- und eine GpioInt-Ressource. Die GpioInt-Ressource muss direkt auf die GpioIO-Ressource folgen und muss auf dieselbe Pinnummer verweisen. 
* GPIO-Ressourcen müssen nach aufsteigender Pinnummer geordnet werden. 
* Jede GpioIO- und GpioInt-Ressource muss genau eine Pinnummer in der Pinliste enthalten. 
* Das ShareType-Feld der beiden Beschreibungen muss „Shared“ sein 
* Das EdgeLevel-Feld der GpioInt-Beschreibung muss „Edge“ sein 
* Das ActiveLevel-Feld der GpioInt-Beschreibung muss „ActiveBoth“ sein 
* Das PinConfig-Feld 
  * Muss für die GpioIO- und GpioInt-Beschreibungen identisch sein 
  * Muss PullUp, PullDown oder PullNone sein. Es kann nicht PullDefault sein.
  * Die Pull-Konfiguration muss mit dem Einschaltzustand des Pins übereinstimmen. Wenn der Pin in den angegebenen Pull-Modus des Einschaltzustand versetzt wird, darf sich der Zustand des Pins nicht ändern. Wenn in der Tabelle angegeben ist, dass der Pin mit einem Pull-Up eingeblendet wird, legen Sie PinConfig als PullUp fest.  

Firmware, UEFI und Treiberinitialisierungscode sollten den Zustand eines Pins nicht entsprechend des Einschaltzustands während des Starts ändern. Nur der Benutzer weiß, was mit einer Pin verbunden ist und welche Zustandsübergänge daher sicher sind. Der Einschaltzustand der einzelnen Pins muss dokumentiert werden, damit Benutzer Hardware entwerfen können, die ordnungsgemäß mit einem Pin verbunden ist. Ein Pin darf den Zustand nicht unerwartet während des Starts ändern. 

Wenn ein verfügbar gemachter Pin über mehrere alternative Funktionen verfügt, liegt es in der Verantwortung der Firmware, den Pin in der richtigen Mux-Konfiguration für die nachfolgende Verwendung durch das Betriebssystem zu initialisieren. Dynamisches Ändern der Funktion eines Pins („Muxing“) wird derzeit von Windows nicht unterstützt. 

#### Unterstützte Laufwerkmodi 

Wenn Ihr GPIO-Controller integrierte Pull-Up- und Pull-Down-Widerstände zusätzlich zur hohen Impedanzeingabe und CMOS-Ausgabe unterstützt, müssen Sie dies mit der optionalen SupportedDriveModes-Eigenschaft angeben. 

```cpp
Package (2) { “GPIO-SupportedDriveModes”, 0xf }, 
```

Die SupportedDriveModes-Eigenschaft gibt an, welche Laufwerkmodi vom GPIO-Controller unterstützt werden. Im obigen Beispiel werden alle der folgenden Laufwerkmodi unterstützt. Die Eigenschaft ist eine Bitmaske der folgenden Werte: 

| Flagwert | Laufwerkmodus | Beschreibung |
|------------|------------|-------------|
| 0 x 1        | InputHighImpedance | Der Pin unterstützt eine hohe Impedanzeingabe, die dem Wert „PullNone“ in ACPI entspricht. |
| 0 x 2        | InputPullUp | Der Pin unterstützt einen integrierten Pull-Up-Widerstand, der dem Wert „PullUp“ in ACPI entspricht. |
| 0 x 4        | InputPullDown | Der Pin unterstützt einen integrierten Pull-Down-Widerstand, der dem Wert „PullDown“ in ACPI entspricht. |
| 0 x 8        | OutputCmos | Der Pin unterstützt sowohl die Generierung von starken Höhen als auch die von starken Tiefen (im Gegensatz zu einem offenen Ausgleich). |

InputHighImpedance und OutputCmos werden von fast allen GPIO-Controllern unterstützt. Wenn die SupportedDriveModes-Eigenschaft nicht angegeben wird, ist dies die Standardeinstellung. 

Wenn ein GPIO-Signal einen Levelshifter durchläuft, bevor es den verfügbar gemachten Header erreicht, deklarieren Sie die Laufwerkmodi, die vom SOC unterstützt werden, selbst, wenn der Laufwerkmodus nicht auf dem externen Header auszumachen wäre. Wenn ein Pin beispielsweise einen bidirektionalen Levelshifter durchläuft, der aus dem Pin einen offenen Ausgleich mit widerstandsfähigem Pull-Up macht, dann werden Sie niemals einen Zustand hoher Impendanz am verfügbar gemachten Header feststellen, selbst wenn der Pin als Eingabe mit hoher Impendanz konfiguriert ist. Sie sollten noch immer deklarieren, dass der Pin eine Eingabe mit hoher Impendanz unterstützt. 

#### Pinnummerierung 

Windows unterstützt zwei Schemas für die Pinnummerierung: 

* Sequenzielle Pinnummerierung – Benutzer sehen Zahlen wie 0, 1, 2 ... bis zur Anzahl der verfügbar gemachten Pins. 0 ist die erste in ASL deklarierte GpioIo-Ressource, 1 ist die zweite in ASL deklarierte GpioIo-Ressource usw. 
* Native Pinnummerierung – Benutzer sehen die Pinnummern in GpioIo-Beschreibungen, z.B. 4, 5, 12, 13 ... .  

```cpp
Package (2) { “GPIO-UseDescriptorPinNumbers”, 1 }, 
```

Die **UseDescriptorPinNumbers**-Eigenschaft weist Windows an, die native Pinnummerierung anstelle der sequenziellen Pinnummerierung zu verwenden. Wenn die UseDescriptorPinNumbers-Eigenschaft nicht angegeben wurde oder der Wert 0 (null) ist, verwendet Windows standardmäßig die sequenzielle Pinnummerierung. 

Wenn native Pinnummerierung verwendet wird, müssen Sie auch die **PinCount**-Eigenschaft angeben. 

```cpp
Package (2) { “GPIO-PinCount”, 54 }, 
```

Die **PinCount**-Eigenschaft sollte dem durch die **TotalPins**-Eigenschaft beim [CLIENT_QueryControllerBasicInformation](https://msdn.microsoft.com/library/windows/hardware/hh439399.aspx)-Rückruf des `GpioClx`-Treibers zurückgegebenen Wert entsprechen. 

Wählen Sie das Schema für die Nummerierung, das am kompatibelsten mit der veröffentlichten Dokumentation zu Ihrer Platine ist. Beispielsweise verwendet Raspberry Pi eine native Pinnummerierung, da viele vorhandene Pinout-Diagramme die BCM2835-Pinnummern verwenden. MinnowBoardMax verwendet eine sequenzielle Pinnummerierung, da es einige vorhandene Pinout-Diagramme gibt und die sequenzielle Pinnummerierung Entwicklern die Arbeit erleichtert, da nur 10 von über 200 Pins verfügbar gemacht werden. Die Entscheidung zur Verwendung einer sequenziellen oder nativen Pinnummerierung sollte darauf abzielen, Verwechslungen durch Entwickler zu vermeiden. 

#### GPIO-Treiberanforderungen 

* Müssen verwenden: `GpioClx`
* Müssen auf SOC-Speicher zugeordnet werden 
* Müssen emulierte ActiveBoth-Interruptbehandlung verwenden 

### UART 

UART wird von Raspberry Pi zum aktuellen Zeitpunkt nicht unterstützt. Die folgende UART-Deklaration stammt deshalb von MinnowBoardMax. 

```cpp
// Index 2 
UARTSerialBus(           // Pin 17, 19 of JP1, for SIO_UART2 
    115200,                // InitialBaudRate: in bits ber second 
    ,                      // BitsPerByte: default to 8 bits 
    ,                      // StopBits: Defaults to one bit 
    0xfc,                  // LinesInUse: 8 1-bit flags to declare line enabled 
    ,                      // IsBigEndian: default to LittleEndian 
    ,                      // Parity: Defaults to no parity 
    ,                      // FlowControl: Defaults to no flow control 
    32,                    // ReceiveBufferSize 
    32,                    // TransmitBufferSize 
    "\\_SB.URT2",          // ResourceSource: UART bus controller name 
    , 
    , 
    , 
    )
```

Nur das ResourceSource-Feld ist festgelegt, alle anderen Felder hingegen sind Platzhalter für Werte, die zur Laufzeit vom Benutzer angegebenen werden. 

Die zugehörigen Anzeigenamendeklaration ist: 

```cpp
Package(2) { "bus-UART-UART2", Package() { 2 }}, 
```

Dies weist dem Controller den Anzeigenamen „UART2“ zu. Dabei handelt es sich um den Bezeichner, den Benutzer verwenden, um im Benutzermodus auf den Bus zuzugreifen.  

## Runtime-Pin-Muxing 

Pin-Muxing bezeichnet die Möglichkeit, denselben physischen Pin für unterschiedliche Funktionen zu verwenden. Mehrere verschiedene integrierte Peripheriegeräte, z.B. I2C-Controller, SPI-Controller und GPIO-Controller können an der gleichen physischen Pin auf einen SOC geleitet werden. Der Mux-Block steuert, welche Funktion zu einem bestimmten Zeitpunkt auf dem Pin aktiv ist. Normalerweise ist Firmware für die Funktionszuweisung beim Start verantwortlich ist und diese Zuweisung bleibt während des Startvorgangs statisch. Runtime-Pin-Muxing bietet die Möglichkeit, Pinfunktionszuweisungen zur Laufzeit neu zu konfigurieren. Wenn Benutzer die Pinfunktion zur Laufzeit auswählen können, wird die Entwicklung beschleunigt, da Benutzer die Pins einer Platine schnell neu konfigurieren können und da die Hardware mehr Anwendungen unterstützt, als sie es bei einer statischen Konfiguration tun würde. 

Benutzer können Muxing-Unterstützung für GPIO, I2C, SPI und UART nutzen, ohne zusätzlichen Code zu schreiben. Wenn ein Benutzer eine GPIO oder Bus mit [OpenPin()](https://msdn.microsoft.com/library/dn960157.aspx) oder [FromIdAsync()](https://msdn.microsoft.com/windows.devices.i2c.i2cdevice.fromidasync) öffnet, werden die zugrunde liegenden physische Pins automatisch an die angeforderte Funktion gemuxt. Wenn die Pins bereits von einer anderen Funktion verwendet werden, schlägt der OpenPin()- oder FromIdAsync()-Aufruf fehl. Wenn der Benutzer das Gerät schließt, indem er das Objekt [GpioPin](https://msdn.microsoft.com/library/windows/apps/windows.devices.gpio.gpiopin.aspx), [I2cDevice](https://msdn.microsoft.com/library/windows/apps/windows.devices.i2c.i2cdevice.aspx), [SpiDevice](https://msdn.microsoft.com/library/windows/apps/windows.devices.spi.spidevice.aspx) oder [SerialDevice](https://msdn.microsoft.com/library/windows/apps/windows.devices.serialcommunication.serialdevice.aspx) löscht, werden die Pins freigegeben, sodass sie später für eine andere Funktion geöffnet werden können. 

Windows enthält eine integrierte Unterstützung für Pin-Muxing in den [GpioClx](https://msdn.microsoft.com/library/windows/hardware/hh439515.aspx)-, [SpbCx](https://msdn.microsoft.com/library/windows/hardware/hh406203.aspx)- und [SerCx](https://msdn.microsoft.com/library/windows/hardware/dn265349.aspx)-Frameworks. Diese Frameworks arbeiten zusammen, um einen Pin automatisch an die richtige Funktion zu schalten, wenn auf einen GPIO-Pin oder Bus zugegriffen wird. Der Zugriff auf die Pins wird vermittelt, um Konflikte zwischen mehreren Clients zu vermeiden. Zusätzlich zu dieser integrierten Unterstützung sind die Schnittstellen und Protokolle für das Pin-Muxing universell einsetzbar und können erweitert werden, um zusätzliche Geräte und Szenarien zu unterstützen. 

In diesem Dokument werden zunächst die zugrunde liegenden Schnittstellen und Protokolle des Pin-Muxing beschrieben und anschließend wird beschrieben, wie Sie Unterstützung für Pin-Muxing GpioClx-, SpbCx- und SerCx-Controllertreiber hinzufügen. 

### Pin-Muxing-Architektur 

In diesem Abschnitt werden die zugrunde liegenden Schnittstellen und Protokolle des Pin-Muxing beschrieben. Eine Kenntnis der zugrunde liegenden Protokolle ist nicht zwingend erforderlich, um Pin-Muxing mit SpbCx-/GpioClx-/SerCx-Treibern zu unterstützen. Ausführliche Informationen zur Unterstützung von Pin-Muxing mit SpbCx-/GpioCls-/SerCx-Treibern finden Sie unter [Implementieren von Pin-Muxing-Unterstützung in GpioClx-Clienttreibern](#supporting-muxing-support-in-GpioClx-client-drivers) und [Verwendung von Muxing-Unterstützung in SpbCx- und SerCx-Controllertreibern](#supporting-muxing-in-SpbCx-and-SerCx-controller-drivers). 

Pin-Muxing erfolgt durch Zusammenarbeit mehrerer Komponenten. 

* Pin-Muxing-Server – Hierbei handelt es sich um Treiber, die den Pin-Muxing-Steuerblock steuern. Pin-Muxing-Server erhalten Pin-Muxing-Anfragen von Clients über Anfragen zur Reservierung von Muxing-Ressourcen (über *IRP_MJ_CREATE*) und Anfragen zum Wechsel einer Pin-Funktion (über *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS*). Der Pin-Muxing-Server ist in der Regel der GPIO-Treiber, da der Muxing-Block manchmal Teil des GPIO-Blocks ist. Auch, wenn der Muxing-Block ein separates Peripheriegerät ist, ist der GPIO-Treiber ein logischer Ort für die Muxing-Funktion. 
* Pin-Muxing-Clients – Treiber, die Pin-Muxing beanspruchen. Pin-Muxing-Clients erhalten Pin-Muxing-Ressourcen von der ACPI-Firmware. Pin-Muxing-Ressourcen sind eine Art der Verbindungsressource und werden vom Ressourcenhub verwaltet. Pin-Muxing-Clients reservieren Pin-Muxing-Ressourcen, indem sie ein Handle zur Ressource öffnen. Damit eine Änderung an der Hardware wirksam ist, müssen Clients die Konfiguration durch Senden der Anforderung *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* übernehmen. Clients geben Pin-Muxing-Ressourcen frei, indem Sie den Handel schließen. Die Muxing-Konfiguration wird dann wieder auf Standardeinstellungen zurückgesetzt. 
* ACPI-Firmware – gibt die Muxing-Konfiguration mit `MsftFunctionConfig()`-Ressourcen an. MsftFunctionConfig-Ressourcen geben an, welche Pins in welcher Muxing-Konfiguration von einem Client benötigt werden. MsftFunctionConfig-Ressourcen enthalten Funktionsnummer, Pull-Konfiguration und eine Liste der Pinnummern. MsftFunctionConfig-Ressourcen werden Pin-Muxing-Clients wie Hardwareressourcen bereitgestellt, die von Treibern beim PrepareHardware-Rückruf entsprechend GPIO- und SPB-Verbindungsressourcen empfangen werden. Clients empfangen eine Ressourcen-Hub-ID, die verwendet werden kann, um ein Handle für die Ressource zu öffnen. 

> Sie müssen die Befehlszeilenoption `/MsftInternal` an `asl.exe` übergeben, um ASL-Dateien mit `MsftFunctionConfig()`-Beschreibungen zu kompilieren, da diese Deskriptoren derzeit von der ACPI-Arbeitsgruppe überprüft werden. Beispiel: `asl.exe /MsftInternal dsdt.asl`

Nachfolgend finden Sie die Reihenfolge der Vorgänge des Pin-Muxing. 

![Pin-Muxing-Client-Server-Interaktion](images/usermode-access-diagram-1.png)

1.  Der Client empfängt MsftFunctionConfig-Ressourcen von der ACPI-Firmware bei seinem [EvtDevicePrepareHardware()](https://msdn.microsoft.com/library/windows/hardware/ff540880.aspx)-Rückruf.
2.  Der Client verwendet die Ressourcen-Hub-Hilfsfunktion, `RESOURCE_HUB_CREATE_PATH_FROM_ID()`um einen Pfad über die Ressourcen-ID zu erstellen. Anschließend öffnet er ein Handle zum Pfad (mit [ZwCreateFile()](https://msdn.microsoft.com/library/windows/hardware/ff566424.aspx), [IoGetDeviceObjectPointer()](https://msdn.microsoft.com/library/windows/hardware/ff549198.aspx) oder [WdfIoTargetOpen()](https://msdn.microsoft.com/library/windows/hardware/ff548634.aspx)).
3.  Der Server extrahiert die Ressourcen-Hub-ID aus dem Dateipfad mit der Ressourcen-Hub-Hilfsfunktionen `RESOURCE_HUB_ID_FROM_FILE_NAME()` und fragt dann beim Ressourcen-Hub die Beschreibung der Ressourcen ab.
4.  Der Server führt eine Freigabevermittlung für jeden Pin in der Beschreibung durch und schließt die Anforderung IRP_MJ_CREATE ab.
5.  Der Client sendet eine *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS*-Anforderung an das empfangene Handle.
6.  In Reaktion auf *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* führt der Server das Hardware-Muxing durch, indem er die angegebene Funktion auf jedem Pin aktiviert.
7.  Der Client setzt dann die Vorgänge fort, die von der Konfiguration des angeforderten Pin-Muxing abhängen.
8.  Wenn der Client das Muxing der Pins nicht länger benötigt, wird das Handle geschlossen.
9.  In Reaktion auf das geschlossene Handle setzt der Server die Pins zurück in ihren Ausgangszustand.

### Protokollbeschreibung für Pin-Muxing-Clients

In diesem Abschnitt wird beschrieben, wie ein Client die Pin-Muxing-Funktionalität verwendet. Dies gilt nicht für die Controllertreiber `SerCx` und `SpbCx`, da die Frameworks dieses Protokoll anstelle der Controllertreiber implementieren.

####    Analysieren von Ressourcen

Ein WDF-Treiber empfängt `MsftFunctionConfig()`-Ressourcen in seiner [EvtDevicePrepareHardware()](https://msdn.microsoft.com/library/windows/hardware/ff540880.aspx)-Routine. MsftFunctionConfig-Ressourcen können durch die folgenden Felder identifiziert werden:

```cpp
CM_PARTIAL_RESOURCE_DESCRIPTOR::Type = CmResourceTypeConnection
CM_PARTIAL_RESOURCE_DESCRIPTOR::u.Connection.Class = CM_RESOURCE_CONNECTION_CLASS_FUNCTION_CONFIG
CM_PARTIAL_RESOURCE_DESCRIPTOR::u.Connection.Type = CM_RESOURCE_CONNECTION_TYPE_FUNCTION_CONFIG
```

Ein `EvtDevicePrepareHardware()`-Routine extrahiert MsftFunctionConfig-Ressourcen möglicherweise wie folgt:

```cpp
EVT_WDF_DEVICE_PREPARE_HARDWARE evtDevicePrepareHardware;

_Use_decl_annotations_
NTSTATUS
evtDevicePrepareHardware (
    WDFDEVICE WdfDevice,
    WDFCMRESLIST ResourcesTranslated
    )
{
    PAGED_CODE();

    LARGE_INTEGER connectionId;
    ULONG functionConfigCount = 0;

    const ULONG resourceCount = WdfCmResourceListGetCount(ResourcesTranslated);
    for (ULONG index = 0; index < resourceCount; ++index) {
        const CM_PARTIAL_RESOURCE_DESCRIPTOR* resDescPtr =
            WdfCmResourceListGetDescriptor(ResourcesTranslated, index);

        switch (resDescPtr->Type) {
        case CmResourceTypeConnection:
            switch (resDescPtr->u.Connection.Class) {
            case CM_RESOURCE_CONNECTION_CLASS_FUNCTION_CONFIG:
                switch (resDescPtr->u.Connection.Type) {
                case CM_RESOURCE_CONNECTION_TYPE_FUNCTION_CONFIG:                    
                    switch (functionConfigCount) {
                    case 0:
                        // save the connection ID
                        connectionId.LowPart = resDescPtr->u.Connection.IdLowPart;
                        connectionId.HighPart = resDescPtr->u.Connection.IdHighPart;
                        break;
                    } // switch (functionConfigCount)
                    ++functionConfigCount;
                    break; // CM_RESOURCE_CONNECTION_TYPE_FUNCTION_CONFIG

                } // switch (resDescPtr->u.Connection.Type)
                break; // CM_RESOURCE_CONNECTION_CLASS_FUNCTION_CONFIG
            } // switch (resDescPtr->u.Connection.Class)
            break;
        } // switch
    } // for (resource list)

    if (functionConfigCount < 1) {
        return STATUS_INVALID_DEVICE_CONFIGURATION;
    }
    // TODO: save connectionId in the device context for later use

    return STATUS_SUCCESS;
}
```

####    Reservieren und Übernehmen von Ressourcen

Wenn ein Client Mux-Pins möchte, reserviert und übernimmt er die MsftFunctionConfig-Ressource. Das folgende Beispiel zeigt, wie ein Client MsftFunctionConfig-Ressourcen reservieren und übernehmen kann.

```cpp
_IRQL_requires_max_(PASSIVE_LEVEL)
NTSTATUS AcquireFunctionConfigResource (
    WDFDEVICE WdfDevice,
    LARGE_INTEGER ConnectionId,
    _Out_ WDFIOTARGET* ResourceHandlePtr
    )
{
    PAGED_CODE();

    //
    // Form the resource path from the connection ID
    //
    DECLARE_UNICODE_STRING_SIZE(resourcePath, RESOURCE_HUB_PATH_CHARS);
    NTSTATUS status = RESOURCE_HUB_CREATE_PATH_FROM_ID(
            &resourcePath,
            ConnectionId.LowPart,
            ConnectionId.HighPart);
    if (!NT_SUCCESS(status)) {
        return status;
    }
    
    //
    // Create a WDFIOTARGET
    //
    WDFIOTARGET resourceHandle;
    status = WdfIoTargetCreate(WdfDevice, WDF_NO_ATTRIBUTES, &resourceHandle);
    if (!NT_SUCCESS(status)) {
        return status;
    }

    //
    // Reserve the resource by opening a WDFIOTARGET to the resource
    //
    WDF_IO_TARGET_OPEN_PARAMS openParams;
    WDF_IO_TARGET_OPEN_PARAMS_INIT_OPEN_BY_NAME(
        &openParams,
        &resourcePath,
        FILE_GENERIC_READ | FILE_GENERIC_WRITE);

    status = WdfIoTargetOpen(resourceHandle, &openParams);
    if (!NT_SUCCESS(status)) {
        return status;
    }
    //
    // Commit the resource
    //
    status = WdfIoTargetSendIoctlSynchronously(
            resourceHandle,
            WDF_NO_HANDLE,      // WdfRequest
            IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS,
            nullptr,            // InputBuffer
            nullptr,            // OutputBuffer
            nullptr,            // RequestOptions
            nullptr);           // BytesReturned
    
    if (!NT_SUCCESS(status)) {
        WdfIoTargetClose(resourceHandle);
        return status;
    }

    //
    // Pins were successfully muxed, return the handle to the caller
    //
    *ResourceHandlePtr = resourceHandle;
    return STATUS_SUCCESS;
}
```

Der Treiber sollte das WDFIOTARGET in einem seiner Kontextbereiche speichern, damit dieses später geschlossen werden kann. Wenn der Treiber für die Veröffentlichung der Muxing-Konfiguration bereit ist, sollte das Ressourcenhandle durch Aufrufen von [WdfObjectDelete()](https://msdn.microsoft.com/library/windows/hardware/ff548734.aspx) oder [WdfIoTargetClose()](https://msdn.microsoft.com/library/windows/hardware/ff548586.aspx) geschlossen werden, wenn Sie WDFIOTARGET wiederverwenden möchten.

```cpp
    WdfObjectDelete(resourceHandle);
```

Wenn der Client das Ressourcenhandle schließt, werden die Pins auf ihren ursprünglichen Zustand zurück gemuxt und können jetzt von einem anderen Client verwendet werden.

### Protokollbeschreibung für Pin-Muxing-Server

In diesem Abschnitt wird erläutert, wie ein Pin-Muxing-Server seine Funktionalität für Clients bereitstellt. Dies gilt nicht für `GpioClx`-Miniporttreiber, da das Framework dieses Protokoll anstelle des Clienttreibers implementiert. Ausführliche Informationen zur Unterstützung von Pin-Muxing bei `GpioClx`-Clienttreibern finden Sie unter [Implementieren von Muxing-Unterstützung bei GpioClx Clienttreibern](#supporting-muxing-support-in-GpioClx-client-drivers).

####    Behandeln von IRP_MJ_CREATE-Anforderungen

Clients öffnen ein Handle für eine Ressource, wenn sie eine Pin-Muxing-Ressource reservieren möchten. Ein Pin-Muxing-Server empfängt *IRP_MJ_CREATE*-Anfragen über einen Analysevorgang von der Hub-Ressource. Die nachfolgende Pfadkomponente der *IRP_MJ_CREATE*-Anforderung enthält die Ressourcen-Hub-ID, eine 64-Bit-Ganzzahl im Hexadezimalformat. Der Server sollte die Ressourcen-Hub-ID aus dem Dateinamen mit `RESOURCE_HUB_ID_FROM_FILE_NAME()` aus reshub.h extrahieren und *IOCTL_RH_QUERY_CONNECTION_PROPERTIES* an den Ressourcen-Hub zum Abrufen der `MsftFunctionConfig()`-Beschreibung senden.

Der Server sollte die Beschreibung überprüfen und den Freigabemodus und die Pinliste aus der Beschreibung extrahieren. Anschließend sollte er eine Freigabevermittlung für die Pins durchführen. Wenn diese erfolgreich war, sollte er die Pins als reserviert markieren, bevor er die Abfrage abschließt.

Die Freigabevermittlung war insgesamt erfolgreich, wenn die Freigabevermittlung für jeden Pin in der Pinliste erfolgreich war. Jede Pin sollte wie folgt vermittelt werden:

*   Wenn der Pin nicht bereits reserviert ist, verläuft die Freigabevermittlung erfolgreich.
*   Ist der Pin bereits exklusiv reserviert, schlägt die Freigabevermittlung fehl.
*   Wenn der Pin bereits als freigegeben reserviert ist,
  * und die eingehende Anforderung wird freigegeben, dann verläuft die Freigabevermittlung erfolgreich.
  * und die eingehende Anforderung exklusiv ist, dann schlägt die Freigabevermittlung fehl.

Wenn die Freigabevermittlung fehlschlägt, sollte die Anforderung mit *STATUS_GPIO_INCOMPATIBLE_CONNECT_MODE* abgeschlossen werden. Wenn Vermittlung Freigabe erfolgreich ist, sollte die Anforderung mit *STATUS_SUCCESS* abgeschlossen werden.

Beachten Sie, dass der Freigabemodus der eingehenden Anforderung aus der MsftFunctionConfig-Beschreibung entnommen werden soll, nicht aus [IrpSp -> Parameters.Create.ShareAccess](https://msdn.microsoft.com/library/windows/hardware/ff548630.aspx).

####    Behandeln von IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS-Anfragen

Nachdem der Client erfolgreich eine MsftFunctionConfig-Ressource durch Öffnen eines Handle reserviert hat, kann er *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* senden, um anzufordern, dass der Server den Hardware-Muxing-Vorgang selbst durchführt. Wenn der Server für jeden Pin in der Pinliste *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* empfängt, sollte er 

*   den Pull-Modus, der im PinConfiguration-Element der Struktur PNP_FUNCTION_CONFIG_DESCRIPTOR in der Hardware festgelegt ist, angeben.
*   den Pin zur Funktion muxen, die im FunctionNumber-Element der Struktur PNP_FUNCTION_CONFIG_DESCRIPTOR angegeben ist.

Der Server sollte dann die Anforderung mit *STATUS_SUCCESS* abschließen.

Die Bedeutung von FunctionNumber wird vom Server definiert, und es wird davon ausgegangen, dass die MsftFunctionConfig-Beschreibung mit Kenntnissen darüber erstellt wurde, wie der Servers dieses Feld interpretiert.

Denken Sie daran, dass, wenn das Handle geschlossen ist, der Server die Pins in der Konfiguration wiederherstellen muss, in der sie eingegangen sind, als IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS empfangen wurde. Der Server muss den Zustand der Pins also möglicherweise speichern, bevor diese geändert werden können.

####    Behandeln von IRP_MJ_CLOSE-Anfragen

Wenn ein Client eine Muxing-Ressource nicht länger benötigt, wird das Handle geschlossen. Wenn ein Server eine *IRP_MJ_CLOSE*-Anfrage empfängt, sollte er die Pins auf den Zustand wiederherstellen, als *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* empfangen wurde. Wenn der Client nie ein *IOCTL_GPIO_COMMIT_FUNCTION_CONFIG_PINS* gesendet hat, ist keine Aktion erforderlich. Der Server sollte die Pins dann nach Verfügbarkeit und im Hinblick auf die Freigabevermittlung markieren und die Anforderung mit *STATUS_SUCCESS* durchführen. Achten Sie darauf, dass Sie die *IRP_MJ_CLOSE*-Handhabung ordnungsgemäß mit der *IRP_MJ_CREATE*-Handhabung synchronisieren.

### Richtlinien für das Erstellen von ACPI-Tabellen

In diesem Abschnitt wird beschrieben, wie Muxing-Ressourcen Clienttreibern bereitgestellt werden. Beachten Sie, dass Sie Microsoft ASL Compiler Build 14327 oder höher zum Kompilieren von Tabellen mit `MsftFunctionConfig()`-Ressourcen benötigen. `MsftFunctionConfig()` Ressourcen werden den Pin-Muxing-Clients als Hardwareressourcen bereitgestellt. `MsftFunctionConfig()` Ressourcen sollten Treibern bereitgestellt werden, die Pin-Muxing-Änderungen erfordern. Dies sind in der Regel SPB- und serielle Controllertreiber; sie sollten jedoch nicht SPB- und seriellen peripheren Gerätetreibern bereitgestellt werden, da die Controllertreiber für die Muxing-Konfiguration verantwortlich sind.
Das `MsftFunctionConfig()`-ACPI-Makro wird wie folgt definiert:

```cpp
  MsftFunctionConfig(Shared/Exclusive
                PinPullConfig,
                FunctionNumber,
                ResourceSource,
                ResourceSourceIndex,
                ResourceConsumer/ResourceProducer,
                VendorData) { Pin List }

```

* Freigegeben/exklusiv – Bei „exklusiv“ kann der Pin nur von einem einzigen Client abgerufen werden. Bei „freigegeben“ können mehrere freigegebene Clients die Ressource abrufen. Legen Sie diesen Wert immer auf „exklusiv“ fest, da mehrere unkoordinierte Clients mit Zugriff auf eine änderbare Ressource zu Datenrennen und damit zu unvorhersehbaren Ergebnissen führen können. 
* PinPullConfig – Einer davon 
  * PullDefault – Verwenden der SOC-definierten Pull-Standardkonfiguration beim Einschalten 
  * PullUp – Pull-Up-Widerstand aktivieren 
  * PullDown – Pull-Down-Widerstand aktivieren 
  * PullNone – Alle Pull-Widerstände deaktivieren 
* FunctionNumber – Die in den Mux zu programmierende Funktionsnummer. 
* ResourceSource – Der ACPI-Namespace-Pfad des Pin-Muxing-Servers 
* ResourceSourceIndex – Legen Sie diesen Wert auf 0 fest 
* ResourceConsumer/ResourceProducer – Legen Sie diesen Wert auf ResourceConsumer fest 
* VendorData – Optional Binärdaten, deren Bedeutung vom Pin-Muxing-Server definiert wird. Dies Wert sollte in der Regel leer bleiben
* Pinliste – eine kommagetrennte Liste der Pinnummern für die die Konfiguration gilt. Wenn der Pin-Muxing-Server ein GpioClx-Treiber ist, sind dies GPIO-Pinnummern. Sie haben dieselbe Bedeutung wie Pinnummern in einer GpioIo-Beschreibung. 

Das folgende Beispiel zeigt, wie eine MsftFunctionConfig()-Ressource an einem I2C-Controllertreiber bereitgestellt werden kann. 

```cpp
Device(I2C1) 
{ 
    Name(_HID, "BCM2841") 
    Name(_CID, "BCMI2C") 
    Name(_UID, 0x1) 
    Method(_STA) 
    { 
        Return(0xf) 
    } 
    Method(_CRS, 0x0, NotSerialized) 
    { 
        Name(RBUF, ResourceTemplate() 
        { 
            Memory32Fixed(ReadWrite, 0x3F804000, 0x20) 
            Interrupt(ResourceConsumer, Level, ActiveHigh, Shared) { 0x55 } 
            MsftFunctionConfig(Exclusive, PullUp, 4, "\\_SB.GPI0", 0, ResourceConsumer, ) { 2, 3 } 
        }) 
        Return(RBUF) 
    } 
} 
```

Zusätzlich zum Arbeitsspeicher und den Interruptressourcen, die normalerweise von einem Controllertreiber benötigt werden, wird eine `MsftFunctionConfig()`-Ressource ebenfalls angegeben. Diese Ressource ermöglicht es dem I2C-Controllertreiber, die Pins 2 und 3 – vom Geräteknoten am \\_SB.GPIO0 verwaltet – in Funktion 4 mit aktiviertem Pull-Up-Widerstand zu verwenden. 

### Unterstützen der Muxing-Unterstützung im GpioClx-Clienttreiber 

`GpioClx` verfügt über integrierte Unterstützung für Pin-Muxing. GpioClx-Miniporttreiber (auch als „GpioClx-Clienttreiber“ bezeichnet) steuern die GPIO-Controller-Hardware. Ab Windows 10 Build 14327 können GpioClx-Miniporttreiber durch die Implementierung von zwei neuen DDIs Unterstützung für Pin-Muxing hinzufügen: 

* CLIENT_ConnectFunctionConfigPins – Aufgerufen von `GpioClx`, damit der Miniporttreiber die angegebene Muxing-Konfiguration anwendet. 
* CLIENT_ConnectFunctionConfigPins – Aufgerufen von `GpioClx`, damit der Miniporttreiber die angegebene Muxing-Konfiguration rückgängig macht. 

Unter [GpioClx-Ereignisrückruffunktionen](https://msdn.microsoft.com/library/windows/hardware/hh439464.aspx) finden Sie eine Beschreibung dieser Routinen.

Zusätzlich zu diesen beiden neuen DDIs, sollten bestehende DDIs auf Pin-Muxing-Kompatibilität überprüft werden: 

* CLIENT_ConnectIoPins/CLIENT_ConnectInterrupt – CLIENT_ConnectIoPins wird von GpioClx aufgerufen, damit der Miniporttreiber einen Satz Pins für die GPIO-Eingabe oder -Ausgabe konfiguriert. GPIO und MsftFunctionConfig schließen sich gegenseitig aus, d.h. ein Pin wird nie mit GPIO und MsftFunctionConfig gleichzeitig verbunden sein. Da eine standardmäßige Pinfunktion von GPIO nicht benötigt wird, muss ein Pin nicht unbedingt an ein GPIO gemuxt werden, wenn ConnectIoPins aufgerufen wird. ConnectIoPins ist erforderlich, um alle Vorgänge auszuführen, die erforderlich sind, um den Pin bereit für GPIO zu machen, einschließlich des Muxing-Vorgangs. 
              *CLIENT_ConnectInterrupt* sollte sich ähnlich verhalten, da Interrupts als Sonderfall von GPIO-Eingaben angesehen werden können. 
* CLIENT_DisconnectIoPins/CLIENT_DisconnectInterrupt – Diese Routine sollte Pins in den Zustand zurückversetzen, in dem sie waren, als CLIENT_ConnectIoPins/CLIENT_ConnectInterrupt aufgerufen wurde, es sei denn, das PreserveConfiguration-Flag ist angegeben. Zusätzlich zum Wiederherstellen der Richtung des Pins auf ihren Standardzustand, sollte der Miniport auch jeden Pin-Muxing-Zustand in den Zustand zurückversetzen, in dem er war, als die Routine _Connect aufgerufen wurde. 

Nehmen wir beispielsweise an, dass die Muxing-Standardkonfiguration eines Pins UART ist und dass der Pin auch als GPIO verwendet werden kann. Wenn CLIENT_ConnectIoPins aufgerufen wird, um den Pin für GPIO zu verbinden, sollte der Pin auf GPIO gemuxt werden; bei CLIENT_DisconnectIoPins sollte der Pin zurück auf UART gemuxt werden. Im Allgemeinen sollten die _Disconnect-Routinen Vorgänge widerrufen, die von _Connect Routinen durchgeführt wurden. 

### Unterstützen von Muxing bei SpbCx- und SerCx-Controllertreibern 

Ab Windows 10 Build 14327 enthalten die `SpbCx`- und `SerCx`-Frameworks integrierte Unterstützung für Pin-Muxing, die die `SpbCx`- und `SerCx`-Controllertreiber zu Pin-Muxing-Clients macht, ohne Codeänderungen an den Controllertreibern selbst. Durch die Erweiterung löst jeder periphere SpbCx-/SerCx-Treibern, der mit einem Muxing-aktivierten SpbCx-/SerCx-Controllertreiber verbunden ist, eine Pin-Muxing-Aktivität aus. 

Das folgende Diagramm zeigt die Abhängigkeiten zwischen den einzelnen Komponenten. Wie Sie sehen können, eröffnet das Pin-Muxing eine Abhängigkeit der SerCx- und SpbCx-Controllertreiber vom GPIO-Treiber, der in der Regel für Muxing zuständig ist. 

![Pin-Muxing-Abhängigkeit](images/usermode-access-diagram-2.png)

Bei Gerätinitialisierung analysieren die `SpbCx`- und `SerCx`-Frameworks alle `MsftFunctionConfig()`-Ressourcen, die dem Gerät als Hardwareressourcen bereitgestellt werden. SpbCx/SerCx erwerben dann Pin-Muxing-Ressourcen bzw. geben diese bei Bedarf frei.

`SpbCx` wendet die Pin-Muxing-Konfiguration in seinem *IRP_MJ_CREATE*-Handler direkt vor Aufruf des [EvtSpbTargetConnect()](https://msdn.microsoft.com/library/windows/hardware/hh450818.aspx)-Rückrufs des Clienttreibers an. Wenn die Muxing-Konfiguration nicht angewendet werden kann, wird der `EvtSpbTargetConnect()`-Rückruf des Controllertreibers nicht aufgerufen. Daher kann ein SPB-Controllertreiber davon ausgehen, dass Pins an die SPB-Funktion gemuxt werden, wenn `EvtSpbTargetConnect()` aufgerufen wird.

`SpbCx` kehrt die Pin-Muxing-Konfiguration in seinem *IRP_MJ_CLOSE*-Handler direkt nach Aufrufen des [EvtSpbTargetDisconnect()](https://msdn.microsoft.com/library/windows/hardware/hh450820.aspx)-Rückruf des Controllertreibers um. Das Ergebnis ist, dass Pins an die SPB-Funktion gemuxt werden, sobald ein peripherer Treiber einen Handle für den SPB-Controllertreiber öffnet. Das Muxing wird entfernt, wenn der periphere Treiber seinen Handle schließt.

`SerCx` verhält sich ähnlich. `SerCx` erwirbt alle `MsftFunctionConfig()`-Ressourcen in seinem *IRP_MJ_CREATE*-Handler direkt vor Aufruf des [EvtSerCx2FileOpen()](https://msdn.microsoft.com/library/windows/hardware/dn265209.aspx)-Rückruf des Controllertreibers, und gibt alle Ressourcen im IRP_MJ_CLOSE-Handler nach dem [EvtSerCx2FileClose](https://msdn.microsoft.com/library/windows/hardware/dn265208.aspx)-Rückruf des Controllertreibers frei.

Die Auswirkung des dynamischen Pin-Muxing für `SerCx` und `SpbCx`-Controllertreiber besteht darin, dass sie Pins tolerieren müssen, bei denen das Muxing von der SPB-/UART-Funktion zu bestimmten Zeiten entfernt wird. Controllertreiber müssen wird davon ausgehen, dass Pins nicht gemuxt werden, bis `EvtSpbTargetConnect()` oder `EvtSerCx2FileOpen()` aufgerufen wird. Pins werden nicht zwangsläufig während der folgenden Rückrufe an eine SPB-/UART-Funktion gemuxt. Folgende Liste ist nicht vollständig, sie stellt jedoch die am häufigsten verwendeten PNP-Routinen dar, die von Controllertreibern implementiert werden.

* DriverEntry 
* EvtDriverDeviceAdd 
* EvtDevicePrepareHardware/EvtDeviceReleaseHardware 
* EvtDeviceD0Entry/EvtDeviceD0Exit 

## Überprüfung 

Wenn Sie die Erstellung Ihrer ASL ausgeführt haben, sollten Sie die [Hardware Lab Kit (HLK)](https://msdn.microsoft.com/library/windows/hardware/dn930814.aspx)-Tests durchführen, um sicherzustellen, dass alle Ressourcen korrekt angezeigt werden und dass die zugrunde liegenden Busse dem Funktionsvertrag der API entsprechen. In den folgenden Abschnitten wird beschrieben, wie der rhproxy-Geräteknoten zum Testen geladen werden kann, ohne die Kompilierung der Firmware zu korrigieren, und wie HLK-Tests ausführt werden. 

### Kompilieren und Laden von ASL mit ACPITABL.dat 

Der erste Schritt ist das Kompilieren und Laden der ASL-Datei auf Ihrem System Under Test. Wir empfehlen die Verwendung von ACPITABL.dat während der Entwicklung und Validierung, da zum Testen von ASL-Änderungen keine vollständige UEFI-Wiederherstellung erforderlich ist. 

1. Erstellen Sie eine Datei mit dem Namen yourboard.asl, und fügen Sie den RHPX-Geräteknoten innerhalb eines DefinitionBlock ein: 
```
DefinitionBlock ("ACPITABL.dat", "SSDT", 1, "MSFT", "RHPROXY", 1)
{
    Scope (\_SB)
    {
        Device(RHPX)
        {
        ...
        }
    }
}
```
2.  Laden Sie das WDK herunter und suchen Sie nach der Datei asl.exe
3.  Führen Sie den folgenden Befehl aus, um ACPITABL.dat zu generieren:
```
asl.exe yourboard.asl
```
4.  Kopieren Sie die Ausgabedatei ACPITABL.dat nach c:\windows\system32 auf Ihrem System Under Test.
5.  Aktivieren Sie Testsigning auf Ihrem System Under Test:
```
bcdedit /set testsigning on
```
6.  Starten Sie das System Under Test neu. Das System fügt die ACPI-Tabellen, die in ACPITABL.dat definiert sind, zu den System-Firmware-Tabellen hinzu. 
7.  Stellen Sie sicher, dass der RHPX-Geräteknoten zum System hinzugefügt wurde:
```
devcon status *msft8000
```
Die Ausgabe von devcon sollte anzeigen, dass das Gerät vorhanden ist, auch wenn das Initialisieren durch den Treiber fehlgeschlagen sein könnte, wenn es Fehler in der ASL gibt, die behoben werden müssen.

### Führen Sie die HLK-Tests aus

Bei der Auswahl des rhproxy-Geräteknotens im HLK-Manager werden die entsprechenden Tests automatisch ausgewählt.

Wählen Sie im HLK-Manager „Ressourcen-Hub-Proxygerät“:

![HLK-Manager-Screenshot](images/usermode-hlk-1.png)

Klicken Sie dann auf der Registerkarte „Tests“ und wählen Sie I2C WinRT-, Gpio WinRT- und Spi WinRT-Tests.

![HLK-Manager-Screenshot](images/usermode-hlk-2.png)

Klicken Sie auf „Ausgewählte ausführen“. Weitere Dokumentation zu jedem Test steht zur Verfügung, wenn Sie mit der rechten Maustaste auf den Test klicken und anschließend auf „Testbeschreibung“ klicken.

### Weitere Testressourcen

Einfache Befehlszeilentools für Gpio, I2c, Spi und Serial stehen im ms-iot github-Beispielrepository (https://github.com/ms-iot/samples) zur Verfügung. Diese Tools können zum manuellen Debuggen hilfreich sein.

| Tool | Link |
|------|------|
| GpioTestTool | https://developer.microsoft.com/windows/iot/win10/samples/GPIOTestTool |
| I2cTestTool   | https://developer.microsoft.com/windows/iot/win10/samples/I2cTestTool | 
| SpiTestTool | https://developer.microsoft.com/windows/iot/win10/samples/spitesttool |
| MinComm (seriell) |    https://github.com/ms-iot/samples/tree/develop/MinComm |

## Ressourcen

| Ziel | Link |
|-------------|------|
| ACPI 5.0-Spezifikation | http://acpi.info/spec.htm |
| Asl.exe (Microsoft ASL Compiler) | https://msdn.microsoft.com/library/windows/hardware/dn551195.aspx |
| Windows.Devices.Gpio  | https://msdn.microsoft.com/library/windows/apps/windows.devices.gpio.aspx | 
| Windows.Devices.I2c | https://msdn.microsoft.com/library/windows/apps/windows.devices.i2c.aspx |
| Windows.Devices.Spi | https://msdn.microsoft.com/library/windows/apps/windows.devices.spi.aspx |
| Windows.Devices.SerialCommunication | https://msdn.microsoft.com/library/windows/apps/windows.devices.serialcommunication.aspx |
| Test Authoring and Execution Framework (TAEF) | https://msdn.microsoft.com/library/windows/hardware/hh439725.aspx |
| SpbCx | https://msdn.microsoft.com/library/windows/hardware/hh450906.aspx |
| GpioClx   | https://msdn.microsoft.com/library/windows/hardware/hh439508.aspx |
| SerCx | https://msdn.microsoft.com/library/windows/hardware/ff546939.aspx |
| MITT I2C-Tests | https://msdn.microsoft.com/library/windows/hardware/dn919852.aspx |
| GpioTestTool | https://developer.microsoft.com/windows/iot/win10/samples/GPIOTestTool |
| I2cTestTool   | https://developer.microsoft.com/windows/iot/win10/samples/I2cTestTool | 
| SpiTestTool | https://developer.microsoft.com/windows/iot/win10/samples/spitesttool |
| MinComm (seriell) |    https://github.com/ms-iot/samples/tree/develop/MinComm |
| Hardware Lab Kit (HLK) | https://msdn.microsoft.com/library/windows/hardware/dn930814.aspx |

## Anhang

### AnhangA – Raspberry Pi-ASL-Verzeichnis

Header-Pinout: https://developer.microsoft.com/windows/iot/win10/samples/PinMappingsRPi2

```
DefinitionBlock ("ACPITABL.dat", "SSDT", 1, "MSFT", "RHPROXY", 1)
{

    Scope (\_SB)
    {
        //
        // RHProxy Device Node to enable WinRT API
        //
        Device(RHPX)
        {
            Name(_HID, "MSFT8000")
            Name(_CID, "MSFT8000")
            Name(_UID, 1)

            Name(_CRS, ResourceTemplate()
            {
                // Index 0
                SPISerialBus(              // SCKL - GPIO 11 - Pin 23
                                           // MOSI - GPIO 10 - Pin 19
                                           // MISO - GPIO 9  - Pin 21
                                           // CE0  - GPIO 8  - Pin 24
                    0,                     // Device selection (CE0)
                    PolarityLow,           // Device selection polarity
                    FourWireMode,          // wiremode
                    0,                     // databit len: placeholder
                    ControllerInitiated,   // slave mode
                    0,                     // connection speed: placeholder
                    ClockPolarityLow,      // clock polarity: placeholder
                    ClockPhaseFirst,       // clock phase: placeholder
                    "\\_SB.SPI0",          // ResourceSource: SPI bus controller name
                    0,                     // ResourceSourceIndex
                                           // Resource usage
                    )                      // Vendor Data

                // Index 1
                SPISerialBus(              // SCKL - GPIO 11 - Pin 23
                                           // MOSI - GPIO 10 - Pin 19
                                           // MISO - GPIO 9  - Pin 21
                                           // CE1  - GPIO 7  - Pin 26
                    1,                     // Device selection (CE1)
                    PolarityLow,           // Device selection polarity
                    FourWireMode,          // wiremode
                    0,                     // databit len: placeholder
                    ControllerInitiated,   // slave mode
                    0,                     // connection speed: placeholder
                    ClockPolarityLow,      // clock polarity: placeholder
                    ClockPhaseFirst,       // clock phase: placeholder
                    "\\_SB.SPI0",          // ResourceSource: SPI bus controller name
                    0,                     // ResourceSourceIndex
                                           // Resource usage
                    )                      // Vendor Data

                // Index 2
                SPISerialBus(              // SCKL - GPIO 21 - Pin 40
                                           // MOSI - GPIO 20 - Pin 38
                                           // MISO - GPIO 19 - Pin 35
                                           // CE1  - GPIO 17 - Pin 11
                    1,                     // Device selection (CE1)
                    PolarityLow,           // Device selection polarity
                    FourWireMode,          // wiremode
                    0,                     // databit len: placeholder
                    ControllerInitiated,   // slave mode
                    0,                     // connection speed: placeholder
                    ClockPolarityLow,      // clock polarity: placeholder
                    ClockPhaseFirst,       // clock phase: placeholder
                    "\\_SB.SPI1",          // ResourceSource: SPI bus controller name
                    0,                     // ResourceSourceIndex
                                           // Resource usage
                    )                      // Vendor Data
                // Index 3
                I2CSerialBus(              // Pin 3 (GPIO2, SDA1), 5 (GPIO3, SCL1)
                    0xFFFF,                // SlaveAddress: placeholder
                    ,                      // SlaveMode: default to ControllerInitiated
                    0,                     // ConnectionSpeed: placeholder
                    ,                      // Addressing Mode: placeholder
                    "\\_SB.I2C1",          // ResourceSource: I2C bus controller name
                    ,
                    ,
                    )                      // VendorData

                // Index 4 - GPIO 4 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 4 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 4 }
                // Index 6 - GPIO 5 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 5 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 5 }
                // Index 8 - GPIO 6 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 6 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 6 }
                // Index 10 - GPIO 12 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 12 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 12 }
                // Index 12 - GPIO 13 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 13 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 13 }
                // Index 14 - GPIO 16 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 16 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 16 }
                // Index 16 - GPIO 18 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 18 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 18 }
                // Index 18 - GPIO 22 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 22 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 22 }
                // Index 20 - GPIO 23 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 23 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 23 }
                // Index 22 - GPIO 24 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 24 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 24 }
                // Index 24 - GPIO 25 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 25 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 25 }
                // Index 26 - GPIO 26 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 26 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 26 }
                // Index 28 - GPIO 27 -
                GpioIO(Shared, PullDown, , , , "\\_SB.GPI0", , , , ) { 27 }
                GpioInt(Edge, ActiveBoth, Shared, PullDown, 0, "\\_SB.GPI0",) { 27 }
                // Index 30 - GPIO 35 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 35 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 35 }
                // Index 32 - GPIO 47 -
                GpioIO(Shared, PullUp, , , , "\\_SB.GPI0", , , , ) { 47 }
                GpioInt(Edge, ActiveBoth, Shared, PullUp, 0, "\\_SB.GPI0",) { 47 }
            })

            Name(_DSD, Package()
            {
                ToUUID("daffd814-6eba-4d8c-8a91-bc9bbf4aa301"),
                Package()
                {
                    // Reference http://www.raspberrypi.org/documentation/hardware/raspberrypi/spi/README.md
                    // SPI 0
                    Package(2) { "bus-SPI-SPI0", Package() { 0, 1 }},                       // Index 0 & 1
                    Package(2) { "SPI0-MinClockInHz", 7629 },                               // 7629 Hz
                    Package(2) { "SPI0-MaxClockInHz", 125000000 },                          // 125 MHz
                    Package(2) { "SPI0-SupportedDataBitLengths", Package() { 8 }},          // Data Bit Length
                    // SPI 1
                    Package(2) { "bus-SPI-SPI1", Package() { 2 }},                          // Index 2
                    Package(2) { "SPI1-MinClockInHz", 30518 },                              // 30518 Hz
                    Package(2) { "SPI1-MaxClockInHz", 125000000 },                          // 125 MHz
                    Package(2) { "SPI1-SupportedDataBitLengths", Package() { 8 }},          // Data Bit Length
                    // I2C1
                    Package(2) { "bus-I2C-I2C1", Package() { 3 }},
                    // GPIO Pin Count and supported drive modes
                    Package (2) { "GPIO-PinCount", 54 },
                    Package (2) { "GPIO-UseDescriptorPinNumbers", 1 },
                    Package (2) { "GPIO-SupportedDriveModes", 0xf },                        // InputHighImpedance, InputPullUp, InputPullDown, OutputCmos
                }
            })
        }
    }
}

```

### AnhangB – MinnowBoardMax-ASL-Verzeichnis

Header-Pinout: https://developer.microsoft.com/windows/iot/win10/samples/PinMappingsMBM

```
DefinitionBlock ("ACPITABL.dat", "SSDT", 1, "MSFT", "RHPROXY", 1)
{
    Scope (\_SB)
    {
        Device(RHPX)
        {
            Name(_HID, "MSFT8000")
            Name(_CID, "MSFT8000")
            Name(_UID, 1)

            Name(_CRS, ResourceTemplate() 
            {  
                // Index 0 
                SPISerialBus(            // Pin 5, 7, 9 , 11 of JP1 for SIO_SPI
                    1,                     // Device selection
                    PolarityLow,           // Device selection polarity
                    FourWireMode,          // wiremode
                    8,                     // databit len
                    ControllerInitiated,   // slave mode
                    8000000,               // Connection speed
                    ClockPolarityLow,      // Clock polarity
                    ClockPhaseSecond,      // clock phase
                    "\\_SB.SPI1",          // ResourceSource: SPI bus controller name
                    0,                     // ResourceSourceIndex
                    ResourceConsumer,      // Resource usage
                    JSPI,                  // DescriptorName: creates name for offset of resource descriptor
                    )                      // Vendor Data  
    
                // Index 1     
                I2CSerialBus(            // Pin 13, 15 of JP1, for SIO_I2C5 (signal)
                    0xFF,                  // SlaveAddress: bus address (TBD)
                    ,                      // SlaveMode: default to ControllerInitiated
                    400000,                // ConnectionSpeed: in Hz
                    ,                      // Addressing Mode: default to 7 bit
                    "\\_SB.I2C6",          // ResourceSource: I2C bus controller name (For MinnowBoard Max, hardware I2C5(0-based) is reported as ACPI I2C6(1-based))
                    ,
                    ,
                    JI2C,                  // Descriptor Name: creates name for offset of resource descriptor
                    )                      // VendorData
    
                // Index 2
                UARTSerialBus(           // Pin 17, 19 of JP1, for SIO_UART2
                    115200,                // InitialBaudRate: in bits ber second
                    ,                      // BitsPerByte: default to 8 bits
                    ,                      // StopBits: Defaults to one bit
                    0xfc,                  // LinesInUse: 8 1-bit flags to declare line enabled
                    ,                      // IsBigEndian: default to LittleEndian
                    ,                      // Parity: Defaults to no parity
                    ,                      // FlowControl: Defaults to no flow control
                    32,                    // ReceiveBufferSize
                    32,                    // TransmitBufferSize
                    "\\_SB.URT2",          // ResourceSource: UART bus controller name
                    ,
                    ,
                    UAR2,                  // DescriptorName: creates name for offset of resource descriptor
                    )                      
    
                // Index 3
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO2",) {0}  // Pin 21 of JP1 (GPIO_S5[00])
                // Index 4
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO2",) {0} 
    
                // Index 5
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO2",) {1}  // Pin 23 of JP1 (GPIO_S5[01])
                // Index 6
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO2",) {1}
    
                // Index 7
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO2",) {2}  // Pin 25 of JP1 (GPIO_S5[02])
                // Index 8
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO2",) {2} 
    
                // Index 9
                UARTSerialBus(           // Pin 6, 8, 10, 12 of JP1, for SIO_UART1
                    115200,                // InitialBaudRate: in bits ber second
                    ,                      // BitsPerByte: default to 8 bits
                    ,                      // StopBits: Defaults to one bit
                    0xfc,                  // LinesInUse: 8 1-bit flags to declare line enabled
                    ,                      // IsBigEndian: default to LittleEndian
                    ,                      // Parity: Defaults to no parity
                    FlowControlHardware,   // FlowControl: Defaults to no flow control
                    32,                    // ReceiveBufferSize
                    32,                    // TransmitBufferSize
                    "\\_SB.URT1",          // ResourceSource: UART bus controller name
                    ,
                    ,
                    UAR1,              // DescriptorName: creates name for offset of resource descriptor
                    )  
    
                // Index 10
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {62}  // Pin 14 of JP1 (GPIO_SC[62])
                // Index 11
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {62} 

                // Index 12
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {63}  // Pin 16 of JP1 (GPIO_SC[63])
                // Index 13
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {63} 
    
                // Index 14
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {65}  // Pin 18 of JP1 (GPIO_SC[65])
                // Index 15
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {65} 
    
                // Index 16
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {64}  // Pin 20 of JP1 (GPIO_SC[64])
                // Index 17
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {64} 
    
                // Index 18
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {94}  // Pin 22 of JP1 (GPIO_SC[94])
                // Index 19
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {94} 
    
                // Index 20
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {95}  // Pin 24 of JP1 (GPIO_SC[95])
                // Index 21
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {95} 
    
                // Index 22
                GpioIo (Shared, PullNone, 0, 0, IoRestrictionNone, "\\_SB.GPO0",) {54}  // Pin 26 of JP1 (GPIO_SC[54])
                // Index 23
                GpioInt(Edge, ActiveBoth, SharedAndWake, PullNone, 0,"\\_SB.GPO0",) {54}
            })
    
            Name(_DSD, Package() 
            {
                ToUUID("daffd814-6eba-4d8c-8a91-bc9bbf4aa301"),
                Package() 
                {
                    // SPI Mapping
                    Package(2) { "bus-SPI-SPI0", Package() { 0 }},

                    Package(2) { "SPI0-MinClockInHz", 100000 },
                    Package(2) { "SPI0-MaxClockInHz", 15000000 },
                    // SupportedDataBitLengths takes a list of support data bit length
                    // Example : Package(2) { "SPI0-SupportedDataBitLengths", Package() { 8, 7, 16 }},
                    Package(2) { "SPI0-SupportedDataBitLengths", Package() { 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32 }},
                     // I2C Mapping
                    Package(2) { "bus-I2C-I2C5", Package() { 1 }},
                    // UART Mapping
                    Package(2) { "bus-UART-UART2", Package() { 2 }},
                    Package(2) { "bus-UART-UART1", Package() { 9 }},
                }
            })
        }
    }
}
```

### AnhangC - Beispiel-Powershell-Skript zum Generieren von GPIO-Ressourcen

Das folgende Skript kann zum Generieren der GPIO-Ressourcendeklarationen für Raspberry Pi verwendet werden:

```
$pins = @(
    @{PinNumber=4;PullConfig='PullUp'},
    @{PinNumber=5;PullConfig='PullUp'},
    @{PinNumber=6;PullConfig='PullUp'},
    @{PinNumber=12;PullConfig='PullDown'},
    @{PinNumber=13;PullConfig='PullDown'},
    @{PinNumber=16;PullConfig='PullDown'},
    @{PinNumber=18;PullConfig='PullDown'},
    @{PinNumber=22;PullConfig='PullDown'},
    @{PinNumber=23;PullConfig='PullDown'},
    @{PinNumber=24;PullConfig='PullDown'},
    @{PinNumber=25;PullConfig='PullDown'},
    @{PinNumber=26;PullConfig='PullDown'},
    @{PinNumber=27;PullConfig='PullDown'},
    @{PinNumber=35;PullConfig='PullUp'},
    @{PinNumber=47;PullConfig='PullUp'})
    
# generate the resources
$FIRST_RESOURCE_INDEX = 4
$resourceIndex = $FIRST_RESOURCE_INDEX
$pins | % {
    $a = @"
// Index $resourceIndex - GPIO $($_.PinNumber) - $($_.Name)
GpioIO(Shared, $($_.PullConfig), , , , "\\_SB.GPI0", , , , ) { $($_.PinNumber) }
GpioInt(Edge, ActiveBoth, Shared, $($_.PullConfig), 0, "\\_SB.GPI0",) { $($_.PinNumber) }
"@    
    Write-Host $a
    $resourceIndex += 2;
}
```



<!--HONumber=Jul16_HO2-->


