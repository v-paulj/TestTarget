---
author: Karl-Bridge-Microsoft
Description: "Identifizieren Sie die Eingabegeräte, die mit einem Gerät für die universelle Windows-Plattform (UWP) verbunden sind, sowie deren Funktionen und Attribute."
title: "Identifizieren von Eingabegeräten"
ms.assetid: B2E93FBF-C508-44D9-BA46-ECFDAA8746F4
label: Identify input devices
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: ee5935a79b10b6c4c084457049cbb518e264be0d

---

# Identifizieren von Eingabegeräten

Identifizieren Sie die Eingabegeräte, die mit einem Gerät für die universelle Windows-Plattform (UWP) verbunden sind, sowie deren Funktionen und Attribute.

**Wichtige APIs**

-   [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br208383)
-   [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)


## <span id="Retrieve_mouse_properties"></span><span id="retrieve_mouse_properties"></span><span id="RETRIEVE_MOUSE_PROPERTIES"></span>Abrufen von Mauseigenschaften


Der [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)-Namespace enthält die [**MouseCapabilities**](https://msdn.microsoft.com/library/windows/apps/br225626)-Klasse, mit der Sie die Eigenschaften abrufen können, die von einer oder mehreren angeschlossenen Mäusen bereitgestellt werden. Erstellen Sie einfach ein neues **MouseCapabilities**-Objekt, und rufen Sie die benötigten Eigenschaften ab.

**Hinweis**  Die von den hier beschriebenen Eigenschaften zurückgegebenen Werte basieren auf allen ermittelten Mäusen: Boolesche Eigenschaften geben Werte ungleich 0 zurück, wenn mindestens eine Maus eine bestimmte Funktion unterstützt, während numerische Eigenschaften den größten Wert einer der Mäuse zurückgeben.

 

Der folgende Code verwendet eine Reihe von [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)-Elementen, um die einzelnen Mauseigenschaften und -werte anzuzeigen.

```CSharp
private void GetMouseProperties()
{
    MouseCapabilities mouseCapabilities = new Windows.Devices.Input.MouseCapabilities();
    MousePresent.Text = mouseCapabilities.MousePresent != 0 ? "Yes" : "No";
    VertWheel.Text = mouseCapabilities.VerticalWheelPresent != 0 ? "Yes" : "No";
    HorzWheel.Text = mouseCapabilities.HorizontalWheelPresent != 0 ? "Yes" : "No";
    SwappedButtons.Text = mouseCapabilities.SwapButtons != 0 ? "Yes" : "No";
    NumButtons.Text = mouseCapabilities.NumberOfButtons.ToString();
}
```

## <span id="Retrieve_keyboard_properties"></span><span id="retrieve_keyboard_properties"></span><span id="RETRIEVE_KEYBOARD_PROPERTIES"></span>Abrufen von Tastatureigenschaften


Der [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)-Namespace enthält die [**KeyboardCapabilities**](https://msdn.microsoft.com/library/windows/apps/br225623)-Klasse, mit der Sie ermitteln können, ob eine Tastatur angeschlossen ist. Erstellen Sie einfach ein neues **KeyboardCapabilities**-Objekt, und rufen Sie die [**KeyboardPresent**](https://msdn.microsoft.com/library/windows/apps/br225625)-Eigenschaft ab.

Der folgende Code verwendet ein [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)-Element, um die Tastatureigenschaft und ihren Wert anzuzeigen.

```CSharp
private void GetKeyboardProperties()
{
    KeyboardCapabilities keyboardCapabilities = new Windows.Devices.Input.KeyboardCapabilities();
    KeyboardPresent.Text = keyboardCapabilities.KeyboardPresent != 0 ? "Yes" : "No";
}
```

## <span id="Retrieve_touch_properties"></span><span id="retrieve_touch_properties"></span><span id="RETRIEVE_TOUCH_PROPERTIES"></span>Abrufen von Berührungseigenschaften


Der [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)-Namespace enthält die [**TouchCapabilities**](https://msdn.microsoft.com/library/windows/apps/br225644)-Klasse, mit der Sie ermitteln können, ob Touchdigitalisierungsgeräte angeschlossen sind. Erstellen Sie einfach ein neues **TouchCapabilities**-Objekt, und rufen Sie die benötigten Eigenschaften ab.

**Hinweis**  Die von den hier beschriebenen Eigenschaften zurückgegebenen Werte basieren auf allen ermittelten Touchdigitalisierungsgeräten: Boolesche Eigenschaften geben Werte ungleich 0 zurück, wenn mindestens ein Digitalisierungsgerät eine bestimmte Funktion unterstützt, während numerische Eigenschaften den größten Wert eines der Digitalisierungsgeräte zurückgeben.

 

Der folgende Code verwendet eine Reihe von [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)-Elementen, um die Eigenschaften und Werte der einzelnen Touchdigitalisierer anzuzeigen.

```CSharp
private void GetTouchProperties()
{
    TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();
    TouchPresent.Text = touchCapabilities.TouchPresent != 0 ? "Yes" : "No";
    Contacts.Text = touchCapabilities.Contacts.ToString();
}
```

## <span id="Retrieve_pointer_properties"></span><span id="retrieve_pointer_properties"></span><span id="RETRIEVE_POINTER_PROPERTIES"></span>Abrufen von Zeigereigenschaften


Der [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)-Namespace enthält die [**PointerDevice**](https://msdn.microsoft.com/library/windows/apps/br225633)-Klasse, mit der Sie abrufen können, ob eines der erkannten Geräte Zeigereingaben (Toucheingabe, Stift oder Maus) unterstützt. Erstellen Sie einfach ein neues **PointerDevice**-Objekt, und rufen Sie die benötigten Eigenschaften ab.

**Hinweis**  Die von den hier beschriebenen Eigenschaften zurückgegebenen Werte basieren auf allen ermittelten Zeigegeräten: Boolesche Eigenschaften geben Werte ungleich 0 zurück, wenn mindestens ein Zeigegerät eine bestimmte Funktion unterstützt, während numerische Eigenschaften den größten Wert eines der Zeigegeräte zurückgeben.

 

Der folgende Code zeigt in einer Tabelle die Eigenschaften und Werte der einzelnen Zeigergeräte an.

```CSharp
private void GetPointerDevices()
{
    IReadOnlyList<PointerDevice> pointerDevices = Windows.Devices.Input.PointerDevice.GetPointerDevices();
    int gridRow = 0;
    int gridColumn = 0;

    for (int i = 0; i < pointerDevices.Count; i++)
    {
        // Pointer device type.
        TextBlock textBlock1 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock1);
        textBlock1.Text = (i + 1).ToString() + " Pointer Device Type:";
        Grid.SetRow(textBlock1, gridRow);
        Grid.SetColumn(textBlock1, gridColumn);

        TextBlock textBlock2 = new TextBlock();
        textBlock2.Text = pointerDevices[i].PointerDeviceType.ToString();
        Grid_PointerProps.Children.Add(textBlock2);
        Grid.SetRow(textBlock2, gridRow++);
        Grid.SetColumn(textBlock2, gridColumn + 1);

        // Is external?
        TextBlock textBlock3 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock3);
        textBlock3.Text = (i + 1).ToString() + " Is External?";
        Grid.SetRow(textBlock3, gridRow);
        Grid.SetColumn(textBlock3, gridColumn);

        TextBlock textBlock4 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock4);
        textBlock4.Text = pointerDevices[i].IsIntegrated.ToString();
        Grid.SetRow(textBlock4, gridRow++);
        Grid.SetColumn(textBlock4, gridColumn + 1);

        // Maximum contacts.
        TextBlock textBlock5 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock5);
        textBlock5.Text = (i + 1).ToString() + " Max Contacts:";
        Grid.SetRow(textBlock5, gridRow);
        Grid.SetColumn(textBlock5, gridColumn);

        TextBlock textBlock6 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock6);
        textBlock6.Text = pointerDevices[i].MaxContacts.ToString();
        Grid.SetRow(textBlock6, gridRow++);
        Grid.SetColumn(textBlock6, gridColumn + 1);

        // Physical device rectangle.
        TextBlock textBlock7 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock7);
        textBlock7.Text = (i + 1).ToString() + " Physical Device Rect:";
        Grid.SetRow(textBlock7, gridRow);
        Grid.SetColumn(textBlock7, gridColumn);

        TextBlock textBlock8 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock8);
        textBlock8.Text = pointerDevices[i].PhysicalDeviceRect.X.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Y.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Width.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Height.ToString();
        Grid.SetRow(textBlock8, gridRow++);
        Grid.SetColumn(textBlock8, gridColumn + 1);

        // Screen rectangle.
        TextBlock textBlock9 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock9);
        textBlock9.Text = (i + 1).ToString() + " Screen Rect:";
        Grid.SetRow(textBlock9, gridRow);
        Grid.SetColumn(textBlock9, gridColumn);

        TextBlock textBlock10 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock10);
        textBlock10.Text = pointerDevices[i].ScreenRect.X.ToString() + "," +
            pointerDevices[i].ScreenRect.Y.ToString() + "," +
            pointerDevices[i].ScreenRect.Width.ToString() + "," +
            pointerDevices[i].ScreenRect.Height.ToString();
        Grid.SetRow(textBlock10, gridRow++);
        Grid.SetColumn(textBlock10, gridColumn + 1);

        gridColumn += 2;
        gridRow = 0;
    }
```

## <span id="related_topics"></span>Verwandte Artikel


**Beispiele**
* [Einfaches Eingabebeispiel](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Eingabebeispiel mit geringer Latenz](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Beispiel für den Benutzerinteraktionsmodus](http://go.microsoft.com/fwlink/p/?LinkID=619894)

**Archivbeispiele**
* [Eingabe: Beispiel für Gerätefunktionen](http://go.microsoft.com/fwlink/p/?linkid=231530)
 

 







<!--HONumber=Jun16_HO3-->


