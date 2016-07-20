---
author: DBirtolo
ms.assetid: B4A550E7-1639-4C9A-A229-31E22B1415E7
title: Sensorausrichtung
description: "Sensordaten der Klassen Accelerometer, Gyrometer, Compass, Inclinometer und OrientationSensor sind durch ihre Referenzachsen definiert. Diese Achsen werden durch das Querformat des Geräts bestimmt und drehen sich mit dem Gerät, wenn es vom Benutzer gedreht wird."
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 0f1123d3be66973d5b56a4789b1ff6e171f94900

---
# Sensorausrichtung

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** Wichtige APIs **

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Windows.Devices.Sensors.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn895032)

Sensordaten der Klassen [**Accelerometer**](https://msdn.microsoft.com/library/windows/apps/BR225687), [**Gyrometer**](https://msdn.microsoft.com/library/windows/apps/BR225718), [**Compass**](https://msdn.microsoft.com/library/windows/apps/BR225705), [**Inclinometer**](https://msdn.microsoft.com/library/windows/apps/BR225766) und [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) sind durch ihre Referenzachsen definiert. Diese Achsen werden durch das Querformat des Geräts bestimmt und drehen sich mit dem Gerät, wenn es vom Benutzer gedreht wird. Falls Ihre App die automatische Drehung unterstützt und sie sich selbst entsprechend der Drehung des Geräts neu ausrichtet, müssen Sie die Sensordaten für die Drehung anpassen, bevor Sie sie verwenden.

## Bildschirmausrichtung und Geräteausrichtung

Um die Referenzachse für Sensoren begreifen zu können, muss zwischen Bildschirmausrichtung und Geräteausrichtung unterschieden werden. Bei der Bildschirmausrichtung handelt es sich um die Richtung, in der Text und Bilder auf dem Bildschirm angezeigt werden, wohingegen es sich bei der Geräteausrichtung um die physische Position des Geräts handelt. Im folgenden Bild sind sowohl Bildschirm- als auch Geräteausrichtung im **Landscape**-Format.

![Bildschirm- und Geräteausrichtung im Querformat](images/accelerometer-axis-orientation-landscape-with-text.png)

Im folgenden Bild sind sowohl Bildschirm- als auch Geräteausrichtung im **LandscapeFlipped**-Format.

![Bildschirm- und Geräteausrichtung im LandscapeFlipped-Format](images/accelerometer-axis-orientation-landscape-180-with-text.png)

Das nächste Bild zeigt die Anzeigeausrichtung im Querformat und die Geräteausrichtung im LandscapeFlipped-Format.

![Bildschirmausrichtung im Querformat und Geräteausrichtung im LandscapeFlipped-Format](images/accelerometer-axis-orientation-landscape-180-with-text-inverted.png)

Sie können die Ausrichtungswerte mithilfe der [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/Dn264258)-Klasse abfragen, indem Sie die [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.display.displayinformation.getforcurrentview.aspx)-Methode mit der [**CurrentOrientation**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.display.displayinformation.currentorientation.aspx)-Eigenschaft verwenden. Anschließend können Sie durch einen Vergleich mit der [**DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/BR226142)-Enumeration eine Logik erstellen. Bedenken Sie, dass Sie für jede unterstützte Ausrichtung eine Konvertierung der Referenzachsen in die jeweilige Ausrichtung unterstützen müssen.

## Für Querformat und für Hochformat ausgelegte Geräte

Hersteller bieten Geräte an, die sowohl für das Quer- als auch das Hochformat ausgelegt sind. Wenn Hersteller Komponenten in Geräte integrieren, erfolgt dies auf einheitliche und konsistente Weise, damit alle Geräte innerhalb desselben Referenzrahmens betrieben werden. Die folgende Tabelle enthält die Gerätesensorachsen (Hoch- und Querformat).

| Ausrichtung | Für Querformat ausgelegt | Für Hochformat ausgelegt |
|-------------|-----------------|----------------|
| **Querformat** | ![Querformatgerät im Querformat](images/accelerometer-axis-orientation-landscape.png) | ![Hochformatgerät im Querformat](images/accelerometer-axis-orientation-portrait-270.png) |
| **Hochformat** | ![Querformatgerät im Hochformat](images/accelerometer-axis-orientation-landscape-90.png) | ![Hochformatgerät im Hochformat](images/accelerometer-axis-orientation-portrait.png) |
| **LandscapeFlipped ** | ![Querformatgerät in LandscapeFlipped-Ausrichtung](images/accelerometer-axis-orientation-landscape-180.png) | ![Hochformatgerät in LandscapeFlipped-Ausrichtung](images/accelerometer-axis-orientation-portrait-90.png) | 
| **PortraitFlipped** | ![Querformatgerät in PortraitFlipped-Ausrichtung](images/accelerometer-axis-orientation-landscape-270.png)| ![Hochformatgerät in PortraitFlipped-Ausrichtung](images/accelerometer-axis-orientation-portrait-180.png) |

## Geräte, die die Anzeige übertragen, und monitorlose Geräte

Manche Geräte können die Anzeige auf ein anderes Gerät übertragen. Sie können z.B. ein Tablet verwenden und die Anzeige auf einen Projektor im Querformat übertragen. In diesem Szenario müssen Sie bedenken, dass die Geräteausrichtung auf dem ursprünglichen Gerät basiert, und nicht auf dem Gerät, das die Anzeige darstellt. Ein Beschleunigungsmesser würde daher Daten für das Tablet melden.

Außerdem verfügen einige Geräte nicht über eine Anzeige. Die Standardausrichtung für diese Geräte ist das Hochformat.

## Bildschirmausrichtung und Kompassrichtung


Die Kompassrichtung hängt von den Referenzachsen ab und ändert sich daher mit der Geräteausrichtung. Sie kompensieren dies entsprechend den Angaben in der folgenden Tabelle (unter der Annahme, dass der Benutzer nach Norden ausgerichtet ist).

| Bildschirmausrichtung | Referenzachse für Kompassrichtung | API-Kompassrichtung bei Ausrichtung nach Norden | Kompensation der Kompassrichtung | 
|---------------------|------------------------------------|---------------------------------------|------------------------------|
| Querformat           | -Z | 0   | Richtung               |
| Hochformat            |  Y | 90  | (Richtung + 270) % 360 | 
| LandscapeFlipped    |  Z | 180 | (Richtung + 180) % 360 |
| PortraitFlipped     |  Y | 270 | (Richtung + 90) % 360  |

Ändern Sie die Kompassrichtung entsprechend den Angaben in der Tabelle, um die Richtung korrekt anzuzeigen. Der folgende Codeausschnitt veranschaulicht die Vorgehensweise.

```csharp
private void ReadingChanged(object sender, CompassReadingChangedEventArgs e)
{
    double heading = e.Reading.HeadingMagneticNorth;        
    double displayOffset;
    
    // Calculate the compass heading offset based on
    // the current display orientation.
    DisplayInformation displayInfo = DisplayInformation.GetForCurrentView();
    
    switch (displayInfo.CurrentOrientation) 
    { 
        case DisplayOrientations.Landscape: 
            displayOffset = 0; 
            break;
        case DisplayOrientations.Portrait: 
            displayOffset = 270; 
            break; 
        case DisplayOrientations.LandscapeFlipped: 
            displayOffset = 180; 
            break; 
        case DisplayOrientations.PortraitFlipped: 
            displayOffset = 90; 
            break; 
     } 
    

    double displayCompensatedHeading = (heading + displayOffset) % 360;
    
    // Update the UI...
}
```

## Bildschirmausrichtung mit Beschleunigungsmesser und Gyrometer

Die folgende Tabelle zeigt, wie Beschleunigungsmesser- und Gyrometerdaten für die Bildschirmausrichtung konvertiert werden.

| Referenzachsen        |  X |  Y | Z |
|-----------------------|----|----|---|
| **Querformat**         |  X |  Y | Z |
| **Hochformat**          |  Y | -X | Z |
| **LandscapeFlipped**  | -X | -Y | Z |
| **PortraitFlipped**   | -Y |  X | Z |

Das folgende Codebeispiel wendet diese Konvertierungen auf das Gyrometer an.

```csharp
private void ReadingChanged(object sender, GyrometerReadingChangedEventArgs e)
{
    double x_Axis;
    double y_Axis;
    double z_Axis;

    GyrometerReading reading = e.Reading;  
    
    // Calculate the gyrometer axes based on
    // the current display orientation.
    DisplayInformation displayInfo = DisplayInformation.GetForCurrentView();
    switch (displayInfo.CurrentOrientation) 
    { 
        case DisplayOrientations.Landscape: 
            x_Axis = reading.AngularVelocityX;
            y_Axis = reading.AngularVelocityY;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.Portrait: 
            x_Axis = reading.AngularVelocityY;
            y_Axis = -1 * reading.AngularVelocityX;
            z_Axis = reading.AngularVelocityZ;
            break; 
        case DisplayOrientations.LandscapeFlipped: 
            x_Axis = -1 * reading.AngularVelocityX;
            y_Axis = -1 * reading.AngularVelocityY;
            z_Axis = reading.AngularVelocityZ;
            break; 
        case DisplayOrientations.PortraitFlipped: 
            x_Axis = -1 * reading.AngularVelocityY;
            y_Axis = reading.AngularVelocityX;
            z_Axis = reading.AngularVelocityZ;
            break; 
     } 
    
    
    // Update the UI...
}
```

## Bildschirmausrichtung und Geräteausrichtung

Die [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371)-Daten müssen auf andere Weise geändert werden. Stellen Sie sich diese unterschiedlichen Ausrichtungen als Drehungen um die Z-Achse entgegen dem Uhrzeigersinn vor. Folglich müssen wir die Drehung umkehren, um wieder die Ausrichtung des Benutzers zu erhalten. Für Quaterniondaten können wir anhand der eulerschen Formel eine Drehung mit einer Referenzquaternion definieren, und außerdem können wir eine Referenzdrehungsmatrix verwenden.

![Eulerformel](images/eulers-formula.png) Um die gewünschte relative Ausrichtung zu erhalten, multiplizieren Sie das Referenzobjekt mit dem absoluten Objekt. Beachten Sie, dass diese Berechnung nicht kommutativ ist.

![Multiplizieren des Referenzobjekts mit dem absoluten Objekt](images/orientation-formula.png) Im vorangehenden Ausdruck wird das absolute Objekt von den Sensordaten zurückgegeben.

| Bildschirmausrichtung  | Drehung gegen den Uhrzeigersinn um Z | Referenzquaternion (Drehung in umgekehrter Richtung) | Referenzdrehungsmatrix (Drehung in umgekehrter Richtung) | 
|----------------------|------------------------------------|-----------------------------------------|----------------------------------------------|
| **Querformat**        | 0                                  | 1 + 0i + 0j + 0k                        | \[1 0 0<br/> 0 1 0<br/> 0 0 1\]               |
| **Hochformat**         | 90                                 | cos(-45⁰) + (i + j + k)*sin(-45⁰)       | \[0 1 0<br/>-1 0 0<br/>0 0 1]              |
| **LandscapeFlipped** | 180                                | 0 - i - j - k                           | \[1 0 0<br/> 0 1 0<br/> 0 0 1]               |
| **PortraitFlipped**  | 270                                | cos(-135⁰) + (i + j + k)*sin(-135⁰)     | \[0 -1 0<br/> 1 0 0<br/> 0 0 1]             |




<!--HONumber=Jul16_HO2-->


