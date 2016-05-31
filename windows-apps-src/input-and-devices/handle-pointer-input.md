---
author: Karl-Bridge-Microsoft
Description: Empfangen, verarbeiten und verwalten Sie Eingabedaten von Zeigegeräten, z. B. Touch, Maus, Zeichen-/Eingabestift und Touchpad, in Apps für die universelle Windows-Plattform (UWP).
title: Behandeln von Zeigereingaben
ms.assetid: BDBC9E33-4037-4671-9596-471DCF855C82
label: Handle pointer input
template: detail.hbs
---

# Behandeln von Zeigereingaben

Empfangen, verarbeiten und verwalten Sie Eingabedaten von Zeigegeräten, z. B. Touch, Maus, Zeichen-/Eingabestift und Touchpad, in Apps für die universelle Windows-Plattform (UWP).

**Wichtige APIs**

-   [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br208383)
-   [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)


**Wichtig**  
Wenn Sie eine eigene Interaktionsunterstützung implementieren, sollten Sie daran denken, dass die Benutzer eine intuitive Umgebung erwarten, die die direkte Interaktion mit den UI-Elementen der App beinhaltet. Es empfiehlt sich, die benutzerdefinierten Interaktionen auf der Basis der [Liste der Steuerelemente](https://msdn.microsoft.com/library/windows/apps/mt185406) zu modellieren, um auf diese Weise für eine konsistente und intuitive Benutzerumgebung zu sorgen. Die Plattformsteuerelemente bieten umfassende Funktionen für UWP-Benutzerinteraktionen (Universelle Windows-Plattform) wie Standardinteraktionen, animierte Physikeffekte, visuelles Feedback und Barrierefreiheit. Erstellen Sie benutzerdefinierte Interaktionen nur dann, wenn ein eindeutiger, klar umrissener Bedarf besteht und es keine Basisinteraktion gibt, die das gewünschte Szenario unterstützt.


## <span id="Pointers"></span><span id="pointers"></span><span id="POINTERS"></span>Zeiger


Bei vielen Interaktionsfunktionen ist der Benutzer involviert, der das Objekt identifizieren muss, mit dem er interagieren möchte, indem er mithilfe von Eingabegeräten, z. B. Toucheingabe, Maus, Zeichen-/Eingabestift und Touchpad, darauf zeigt. Da die von diesen Eingabegeräten bereitgestellten HID-Rohdaten (Human Interface Device) viele allgemeine Eigenschaften enthalten, werden die Informationen an einen einheitlichen Eingabestapel weitergeleitet und als konsolidierte geräteunabhängige Zeigerdaten verfügbar gemacht. Ihre UWP-Apps können dann diese Daten ohne Rücksicht auf das verwendete Eingabegerät verwenden.

**Hinweis**  Gerätespezifische Informationen werden auch von den HID-Rohdaten weitergeleitet, wenn dies für die App erforderlich ist.

 

Jeder Eingabepunkt (oder Kontakt) in dem Eingabestapel wird durch ein [**Pointer**](https://msdn.microsoft.com/library/windows/apps/br227968)-Objekt dargestellt, das über den [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076)-Parameter verfügbar gemacht wird, der von den verschiedenen Zeigerereignissen bereitgestellt wird. Bei Verwendung mehrerer Stifte oder der Mehrfingereingabe wird jeder Kontakt als separater Eingabezeiger behandelt.

## <span id="Pointer_events"></span><span id="pointer_events"></span><span id="POINTER_EVENTS"></span>Zeigerereignisse


Zeigerereignisse machen grundlegende Informationen wie Erkennungszustand (im Bereich oder bei Kontakt) und Gerätetyp sowie erweiterte Informationen wie Position, Druck und Kontaktgeometrie verfügbar. Darüber hinaus sind auch bestimmte gerätespezifische Eigenschaften verfügbar, z. B. welche Maustaste ein Benutzer gedrückt hat oder ob die Radiergummispitze des Zeichenstifts verwendet wird. Wenn die App zwischen Eingabegeräten und ihren Funktionen unterscheiden muss, finden Sie entsprechende Informationen unter [Erkennen von Eingabegeräten](identify-input-devices.md).

UWP-Apps können die folgenden Zeigerereignisse überwachen:

**Hinweis**  Rufen Sie [**CapturePointer**](https://msdn.microsoft.com/library/windows/apps/br208918) auf, um Zeigereingaben auf ein bestimmtes UI-Element zu beschränken. Wenn ein Zeiger von einem Element erfasst wurde, empfängt nur dieses Objekt die Zeigereingabeereignisse, auch wenn sich der Mauszeiger aus dem Begrenzungsbereich des Objekts heraus bewegt. Normalerweise wird der Zeiger in einem [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971)-Ereignishandler als [**IsInContact**](https://msdn.microsoft.com/library/windows/apps/br227976) erfasst (gedrückte Maustaste, Toucheingabe oder Eingabestift in Kontakt). Damit der Vorgang erfolgreich ist, muss dies für **CapturePointer** „true“ sein.

 

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Ereignis</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><span id="PointerCanceled"></span><span id="pointercanceled"></span><span id="POINTERCANCELED"></span>[<strong>PointerCanceled</strong>](https://msdn.microsoft.com/library/windows/apps/br208964)</p></td>
<td align="left"><p>Tritt auf, wenn ein Zeiger von der Plattform abgebrochen wird.</p>
<ul>
<li>Touchzeiger werden abgebrochen, wenn ein Zeichenstift innerhalb des Bereichs der Eingabeoberfläche erkannt wird.</li>
<li>Für mehr als 100 ms wird kein aktiver Kontakt erkannt.</li>
<li>Monitor/Anzeige wird geändert (Auflösung, Einstellungen, Konfigurationen mit mehreren Bildschirmen).</li>
<li>Der Desktop ist gesperrt, oder der Benutzer hat sich abgemeldet.</li>
<li>Die Anzahl gleichzeitiger Kontakte hat die vom Gerät unterstützte Anzahl überschritten.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><span id="PointerCaptureLost"></span><span id="pointercapturelost"></span><span id="POINTERCAPTURELOST"></span>[<strong>PointerCaptureLost</strong>](https://msdn.microsoft.com/library/windows/apps/br208965)</p></td>
<td align="left"><p>Tritt auf, wenn ein anderes Benutzeroberflächenelement den Zeiger erfasst, der Zeiger freigegeben wurde oder ein anderer Zeiger programmgesteuert erfasst wurde.</p>
<div class="alert">
<strong>Hinweis</strong>  Es gibt kein entsprechendes Zeigererfassungsereignis.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><span id="PointerEntered"></span><span id="pointerentered"></span><span id="POINTERENTERED"></span>[<strong>PointerEntered</strong>](https://msdn.microsoft.com/library/windows/apps/br208968)</p></td>
<td align="left"><p>Tritt auf, wenn der Zeiger in den Begrenzungsbereich eines Elements eintritt. Dies kann geringfügig anders für Touch-, Touchpad-, Maus- und Stifteingaben passieren.</p>
<ul>
<li>Für Toucheingaben ist zur Auslösung dieses Ereignisses eine Fingerkontakt erforderlich, entweder über eine direkte Toucheingabe oder durch Bewegen in den Begrenzungsbereich des Elements.</li>
<li>Maus und Touchpad haben beide einen Cursor auf dem Bildschirm, der immer sichtbar ist und dieses Ereignis auslöst, auch wenn keine Maus- oder Touchpadtaste gedrückt wird.</li>
<li>Wie bei der Toucheingabe löst der Stift das Ereignis über eine direkte Stifteingabe oder durch Bewegen in den Begrenzungsbereich des Elements aus. Der Stift verfügt jedoch auch über einen Hoverzustand ([<strong>IsInRange</strong>](https://msdn.microsoft.com/library/windows/apps/br227977)), der das Ereignis bei „true“ auslöst.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><span id="PointerExited"></span><span id="pointerexited"></span><span id="POINTEREXITED"></span>[<strong>PointerExited</strong>](https://msdn.microsoft.com/library/windows/apps/br208969)</p></td>
<td align="left"><p>Tritt auf, wenn der Zeiger den Begrenzungsbereich eines Elements verlässt. Dies kann geringfügig anders für Touch-, Touchpad-, Maus- und Stifteingaben passieren.</p>
<ul>
<li>Die Toucheingabe erfordert einen Fingerkontakt und löst dieses Ereignis aus, wenn sich der Mauszeiger aus dem Begrenzungsbereich des Elements heraus bewegt.</li>
<li>Maus und Touchpad haben beide einen Cursor auf dem Bildschirm, der immer sichtbar ist und dieses Ereignis auslöst, auch wenn keine Maus- oder Touchpadtaste gedrückt wird.</li>
<li>Wie bei der Toucheingabe löst der Stift dieses Ereignis beim Bewegen aus dem Begrenzungsbereich des Elements heraus aus. Der Stift verfügt jedoch auch über einen Hoverzustand ([<strong>IsInRange</strong>](https://msdn.microsoft.com/library/windows/apps/br227977)), der das Ereignis auslöst, wenn sich der Zustand von „true“ in „false“ ändert.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left"><p><span id="PointerMoved"></span><span id="pointermoved"></span><span id="POINTERMOVED"></span>[<strong>PointerMoved</strong>](https://msdn.microsoft.com/library/windows/apps/br208970)</p></td>
<td align="left"><p>Tritt auf, wenn ein Zeiger Koordinaten, Schaltflächenzustand, Druck, Neigung oder Kontaktgeometrie (z. B. Breite und Höhe) innerhalb des Begrenzungsbereichs eines Elements ändert. Dies kann geringfügig anders für Touch-, Touchpad-, Maus- und Stifteingaben passieren.</p>
<ul>
<li>Die Toucheingabe erfordert einen Fingerkontakt und löst dieses Ereignis nur aus, wenn ein Kontakt innerhalb des Begrenzungsbereichs des Elements besteht.</li>
<li>Maus und Touchpad haben beide einen Cursor auf dem Bildschirm, der immer sichtbar ist und dieses Ereignis auslöst, auch wenn keine Maus- oder Touchpadtaste gedrückt wird.</li>
<li>Wie bei der Toucheingabe löst der Stift dieses Ereignis aus, wenn ein Kontakt innerhalb des Begrenzungsbereichs des Elements besteht. Der Stift verfügt jedoch auch über einen Hoverzustand ([<strong>IsInRange</strong>](https://msdn.microsoft.com/library/windows/apps/br227977)), der dieses Ereignis bei „true“ und innerhalb des Begrenzungsbereichs des Elements auslöst.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><span id="PointerPressed"></span><span id="pointerpressed"></span><span id="POINTERPRESSED"></span>[<strong>PointerPressed</strong>](https://msdn.microsoft.com/library/windows/apps/br208971)</p></td>
<td align="left"><p>Tritt auf, wenn der Zeiger eine Drückaktion (z. B. eine Fingereingabe, gedrückte Maustaste, Stifteingabe oder gedrückte Touchpadtaste) innerhalb des Begrenzungsbereichs eines Elements angibt.</p>
<p>[<strong>CapturePointer</strong>](https://msdn.microsoft.com/library/windows/apps/br208918) must be called from the handler for this event.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><span id="PointerReleased"></span><span id="pointerreleased"></span><span id="POINTERRELEASED"></span>[<strong>PointerReleased</strong>](https://msdn.microsoft.com/library/windows/apps/br208972)</p></td>
<td align="left"><p>Tritt auf, wenn der Zeiger eine Loslass-Aktion (z. B. ein Finger bewegt sich nach oben, Maustaste, Stift oder Touchpadtaste werden losgelassen) innerhalb des Begrenzungsbereichs eines Elements anzeigt, oder wenn der Zeiger außerhalb des Begrenzungsbereichs erfasst wird.</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="PointerWheelChanged"></span><span id="pointerwheelchanged"></span><span id="POINTERWHEELCHANGED"></span>[<strong>PointerWheelChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br208973)</p></td>
<td align="left"><p>Tritt auf, wenn das Mausrad gedreht wird.</p>
<p>Die Mauseingabe wird einem einzelnen Zeiger zugeordnet, der bei der ersten Ermittlung einer Mauseingabe zugewiesen wird. Durch das Klicken auf eine Maustaste (links, Mausrad oder rechts) wird über das [<strong>PointerMoved</strong>](https://msdn.microsoft.com/library/windows/apps/br208970)-Ereignis eine zweite Zuordnung zwischen dem Zeiger und dieser Taste erstellt.</p></td>
</tr>
</tbody>
</table>

 

## <span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Beispiel


Nachfolgend sehen Sie einige Codebeispiele aus einer einfachen Zeigerverfolgungs-App, in denen gezeigt wird, wie Zeigerereignisse überwacht und behandelt und verschiedene Eigenschaften für aktive Zeiger abgerufen werden.

### <span id="Create_the_UI"></span><span id="create_the_ui"></span><span id="CREATE_THE_UI"></span>Erstellen der Benutzeroberfläche

In diesem Beispiel wird ein Rechteck (`targetContainer`) als Zielobjekt für Zeigereingaben verwendet. Die Farbe des Ziels ändert sich, wenn sich der Zeigerstatus ändert.

Details zu jedem Zeiger werden in einem schwebenden Textblock angezeigt, der sich mit dem Mauszeiger bewegt. Die Zeigerereignisse selbst werden links neben dem Rechteck angezeigt (um die Ereignisabfolge zu melden).

Im Folgenden ist der XAML-Code (Extensible Application Markup Language) für dieses Beispiel aufgeführt.

```XAML
<Page
    x:Class="PointerInput.MainPage"
    IsTabStop="false"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:PointerInput"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Name="page">

    <Grid Background="{StaticResource ApplicationForegroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="69.458" />
            <ColumnDefinition Width="80.542"/>
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="320" />
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <Canvas Name="Container" 
                Grid.Column="0"
                Grid.Row="1"
                HorizontalAlignment="Center" 
                VerticalAlignment="Center" 
                Margin="245,0" 
                Height="320"  Width="640">
            <Rectangle Name="Target" 
                       Fill="#FF0000" 
                       Stroke="Black" 
                       StrokeThickness="0"
                       Height="320" Width="640" />
        </Canvas>
        <Button Name="buttonClear"
                Foreground="White"
                Width="100"
                Height="100">
            clear
        </Button>
        <TextBox Name="eventLog" 
                 Grid.Column="1"
                 Grid.Row="0"
                 Grid.RowSpan="3" 
                 Background="#000000" 
                 TextWrapping="Wrap" 
                 Foreground="#FFFFFF" 
                 ScrollViewer.VerticalScrollBarVisibility="Visible" 
                 BorderThickness="0" Grid.ColumnSpan="2"/>
    </Grid>
</Page>
```

### <span id="Listen_for_pointer_events"></span><span id="listen_for_pointer_events"></span><span id="LISTEN_FOR_POINTER_EVENTS"></span>Lauschen auf Zeigerereignisse

In den meisten Fällen wird empfohlen, Zeigerinformationen über die [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) des Ereignishandlers abzurufen.

Sollte das Ereignisargument die erforderlichen Zeigerdetails nicht liefern, können Sie über die Methoden [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/hh943077) und [**GetIntermediatePoints**](https://msdn.microsoft.com/library/windows/apps/hh943078) von [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) auf die von einem [**PointerPoint**](https://msdn.microsoft.com/library/windows/apps/br242038)-Objekt bereitgestellten erweiterten Zeigerdaten zugreifen.

In diesem Beispiel wird ein Rechteck (`targetContainer`) als Zielobjekt für Zeigereingaben verwendet. Die Farbe des Ziels ändert sich, wenn sich der Zeigerstatus ändert.

Der folgende Code richtet das Zielobjekt ein, deklariert globale Variablen und gibt die verschiedenen Zeigerereignislistener für das Ziel an.

```CSharp
        // For this example, we track simultaneous contacts in case the 
        // number of contacts has reached the maximum supported by the device.
        // Depending on the device, additional contacts might be ignored 
        // (PointerPressed not fired). 
        uint numActiveContacts;
        Windows.Devices.Input.TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();

        // Dictionary to maintain information about each active contact. 
        // An entry is added during PointerPressed/PointerEntered events and removed 
        // during PointerReleased/PointerCaptureLost/PointerCanceled/PointerExited events.
        Dictionary<uint, Windows.UI.Xaml.Input.Pointer> contacts;

        public MainPage()
        {
            this.InitializeComponent();
            numActiveContacts = 0;
            // Initialize the dictionary.
            contacts = new Dictionary<uint, Windows.UI.Xaml.Input.Pointer>((int)touchCapabilities.Contacts);
            // Declare the pointer event handlers.
            Target.PointerPressed += new PointerEventHandler(Target_PointerPressed);
            Target.PointerEntered += new PointerEventHandler(Target_PointerEntered);
            Target.PointerReleased += new PointerEventHandler(Target_PointerReleased);
            Target.PointerExited += new PointerEventHandler(Target_PointerExited);
            Target.PointerCanceled += new PointerEventHandler(Target_PointerCanceled);
            Target.PointerCaptureLost += new PointerEventHandler(Target_PointerCaptureLost);
            Target.PointerMoved += new PointerEventHandler(Target_PointerMoved);
            Target.PointerWheelChanged += new PointerEventHandler(Target_PointerWheelChanged);

            buttonClear.Click += new RoutedEventHandler(ButtonClear_Click);
        }

        private void ButtonClear_Click(object sender, RoutedEventArgs e)
        {
            eventLog.Text = "";
        }

```

### <span id="Handle_pointer_events"></span><span id="handle_pointer_events"></span><span id="HANDLE_POINTER_EVENTS"></span>Behandeln von Zeigerereignissen

Im nächsten Schritt wird UI-Feedback verwendet, um die Verwendung einfacher Zeigerereignishandler zu veranschaulichen.

-   Der folgende Handler kontrolliert ein [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971)-Ereignis. Das Ereignis wird zum Ereignisprotokoll hinzugefügt, der Zeiger wird zum Zeigerarray hinzugefügt, das zum Verfolgen der relevanten Zeiger verwendet wird, und die Zeigerdetails werden angezeigt.

    **Hinweis**
            [
              **PointerPressed**
            ](https://msdn.microsoft.com/library/windows/apps/br208971)- und [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972)-Ereignisse treten nicht immer paarweise auf. Die App sollte auf jedes Ereignis lauschen und dieses behandeln, das eine Zeiger-nach-unten-Aktion beenden könnte (beispielsweise [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969), [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964) und [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965)).

     

```    CSharp
        // PointerPressed and PointerReleased events do not always occur in pairs. 
            // Your app should listen for and handle any event that might conclude a pointer down action 
            // (such as PointerExited, PointerCanceled, and PointerCaptureLost).
            // For this example, we track the number of contacts in case the 
            // number of contacts has reached the maximum supported by the device.
            // Depending on the device, additional contacts might be ignored 
            // (PointerPressed not fired). 
            void Target_PointerPressed(object sender, PointerRoutedEventArgs e)
            {
                Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

                // Update event sequence.
                eventLog.Text += "\nDown: " + ptr.PointerId;

                // Change background color of target when pointer contact detected.
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Green);

                // Prevent most handlers along the event route from handling the same event again.
                e.Handled = true;

                // Lock the pointer to the target.
                Target.CapturePointer(e.Pointer);

                // Update event sequence.
                eventLog.Text += "\nPointer captured: " + ptr.PointerId;

                // Check if pointer already exists (for example, enter occurred prior to press).
                if (contacts.ContainsKey(ptr.PointerId))
                {
                    return;
                }
                // Add contact to dictionary.
                contacts[ptr.PointerId] = ptr;
                ++numActiveContacts;

                // Display pointer details.
                createInfoPop(e);
            }
```

-   Der folgende Handler kontrolliert ein [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968)-Ereignis. Das Ereignis wird zum Ereignisprotokoll hinzugefügt, der Zeiger wird zur Zeigerauflistung hinzugefügt, und die Zeigerdetails werden angezeigt.

```    CSharp
        private void Target_PointerEntered(object sender, PointerRoutedEventArgs e)
            {
                Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

                // Update event sequence.
                eventLog.Text += "\nEntered: " + ptr.PointerId;

                if (contacts.Count == 0)
                {
                    // Change background color of target when pointer contact detected.
                    Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
                }

                // Check if pointer already exists (if enter occurred prior to down).
                if (contacts.ContainsKey(ptr.PointerId))
                {
                    return;
                }

                // Add contact to dictionary.
                contacts[ptr.PointerId] = ptr;
                ++numActiveContacts;

                // Prevent most handlers along the event route from handling the same event again.
                e.Handled = true;

                // Display pointer details.
                createInfoPop(e);
            }
```

-   Der folgende Handler kontrolliert ein [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970)-Ereignis. Das Ereignis wird zum Ereignisprotokoll hinzugefügt, und die Zeigerdetails werden aktualisiert.

    **Wichtig**  Die Mauseingabe wird einem einzelnen Zeiger zugeordnet, der bei der ersten Ermittlung einer Mauseingabe zugewiesen wird. Durch das Klicken auf eine Maustaste (links, Mausrad oder rechts) wird über das [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971)-Ereignis eine zweite Zuordnung zwischen dem Zeiger und dieser Taste erstellt. Das [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972)-Ereignis wird nur ausgelöst, wenn dieselbe Maustaste losgelassen wird (dem Zeiger kann erst eine andere Taste zugeordnet werden, wenn dieses Ereignis abgeschlossen ist). Aufgrund dieser exklusiven Zuordnung werden Klicks auf andere Maustasten über das [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970)-Ereignis geleitet.

     

```    CSharp
private void Target_PointerMoved(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Multiple, simultaneous mouse button inputs are processed here.
        // Mouse input is associated with a single pointer assigned when 
        // mouse input is first detected. 
        // Clicking additional mouse buttons (left, wheel, or right) during 
        // the interaction creates secondary associations between those buttons 
        // and the pointer through the pointer pressed event. 
        // The pointer released event is fired only when the last mouse button 
        // associated with the interaction (not necessarily the initial button) 
        // is released. 
        // Because of this exclusive association, other mouse button clicks are 
        // routed through the pointer move event.          
        if (ptr.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse)
        {
            // To get mouse state, we need extended pointer details.
            // We get the pointer info through the getCurrentPoint method
            // of the event argument. 
            Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
            if (ptrPt.Properties.IsLeftButtonPressed)
            {
                eventLog.Text += "\nLeft button: " + ptrPt.PointerId;
            }
            if (ptrPt.Properties.IsMiddleButtonPressed)
            {
                eventLog.Text += "\nWheel button: " + ptrPt.PointerId;
            }
            if (ptrPt.Properties.IsRightButtonPressed)
            {
                eventLog.Text += "\nRight button: " + ptrPt.PointerId;
            }
        }

        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;

        // Display pointer details.
        updateInfoPop(e);
    }
```

-   Der folgende Handler kontrolliert ein [**PointerWheelChanged**](https://msdn.microsoft.com/library/windows/apps/br208973)-Ereignis. Das Ereignis wird zum Ereignisprotokoll hinzugefügt, der Zeiger wird zum Zeigerarray hinzugefügt (sofern erforderlich), und die Zeigerdetails werden angezeigt.

```    CSharp
private void Target_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nMouse wheel: " + ptr.PointerId;

        // Check if pointer already exists (for example, enter occurred prior to wheel).
        if (contacts.ContainsKey(ptr.PointerId))
        {
            return;
        }

        // Add contact to dictionary.
        contacts[ptr.PointerId] = ptr;
        ++numActiveContacts;

        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;

        // Display pointer details.
        createInfoPop(e);
    }
```

-   Der folgende Handler kontrolliert ein [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972)-Ereignis, bei dem der Kontakt mit dem Digitalisierungsgerät beendet wird. Das Ereignis wird zum Ereignisprotokoll hinzugefügt, der Zeiger wird aus der Zeigerauflistung entfernt, und die Zeigerdetails werden aktualisiert.

```    CSharp
void Target_PointerReleased(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nUp: " + ptr.PointerId;

        // If event source is mouse or touchpad and the pointer is still 
        // over the target, retain pointer and pointer details.
        // Return without removing pointer from pointers dictionary.
        // For this example, we assume a maximum of one mouse pointer.
        if (ptr.PointerDeviceType != Windows.Devices.Input.PointerDeviceType.Mouse)
        {
            // Update target UI.
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);

            destroyInfoPop(ptr);

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            // Release the pointer from the target.
            Target.ReleasePointerCapture(e.Pointer);

            // Update event sequence.
            eventLog.Text += "\nPointer released: " + ptr.PointerId;

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }
        else
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
        }

    }
```

-   Der folgende Handler kontrolliert ein [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969)-Ereignis, bei dem der Kontakt mit dem Digitizer beibehalten wird. Das Ereignis wird zum Ereignisprotokoll hinzugefügt, der Zeiger wird aus dem Zeigerarray entfernt, und die Zeigerdetails werden aktualisiert.

```    CSharp
private void Target_PointerExited(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nPointer exited: " + ptr.PointerId;

        // Remove contact from dictionary.
        if (contacts.ContainsKey(ptr.PointerId))
        {
            contacts[ptr.PointerId] = null;
            contacts.Remove(ptr.PointerId);
            --numActiveContacts;
        }

        // Update the UI and pointer details.
        destroyInfoPop(ptr);

        if (contacts.Count == 0)
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);
        }
        
        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;
    }
```

-   Der folgende Handler kontrolliert ein [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964)-Ereignis. Das Ereignis wird zum Ereignisprotokoll hinzugefügt, der Zeiger wird aus dem Zeigerarray entfernt, und die Zeigerdetails werden aktualisiert.

```    CSharp
// Fires for for various reasons, including: 
    //    - Touch contact canceled by pen coming into range of the surface.
    //    - The device doesn&#39;t report an active contact for more than 100ms.
    //    - The desktop is locked or the user logged off. 
    //    - The number of simultaneous contacts exceeded the number supported by the device.
    private void Target_PointerCanceled(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nPointer canceled: " + ptr.PointerId;

        // Remove contact from dictionary.
        if (contacts.ContainsKey(ptr.PointerId))
        {
            contacts[ptr.PointerId] = null;
            contacts.Remove(ptr.PointerId);
            --numActiveContacts;
        }

        destroyInfoPop(ptr);

        if (contacts.Count == 0)
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
        }
        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;
    }
```

-   Der folgende Handler kontrolliert ein [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965)-Ereignis. Das Ereignis wird zum Ereignisprotokoll hinzugefügt, der Zeiger wird aus dem Zeigerarray entfernt, und die Zeigerdetails werden aktualisiert.

    **Hinweis**
            [
              **PointerCaptureLost**
            ](https://msdn.microsoft.com/library/windows/apps/br208965) kann anstelle von [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) eintreten. Die Zeigererfassung kann aus verschiedenen Gründen verloren gehen.

     

```    CSharp
// Fires for for various reasons, including: 
    //    - User interactions
    //    - Programmatic capture of another pointer
    //    - Captured pointer was deliberately released
    // PointerCaptureLost can fire instead of PointerReleased. 
    private void Target_PointerCaptureLost(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nPointer capture lost: " + ptr.PointerId;

        // Remove contact from dictionary.
        if (contacts.ContainsKey(ptr.PointerId))
        {
            contacts[ptr.PointerId] = null;
            contacts.Remove(ptr.PointerId);
            --numActiveContacts;
        }

        destroyInfoPop(ptr);

        if (contacts.Count == 0)
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
        }
        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;
    }
```

### <span id="Get_pointer_properties"></span><span id="get_pointer_properties"></span><span id="GET_POINTER_PROPERTIES"></span>Abrufen von Zeigereigenschaften

Wie bereits erwähnt, müssen Sie die erweiterten Zeigerinformationen von einem [**Windows.UI.Input.PointerPoint**](https://msdn.microsoft.com/library/windows/apps/br242038)-Objekt abrufen, das über die Methoden [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/hh943077) und [**GetIntermediatePoints**](https://msdn.microsoft.com/library/windows/apps/hh943078) von [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) bereitgestellt wird.

-   Zuerst wird ein neues [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)-Objekt für jeden Zeiger erstellt.

```    CSharp
        void createInfoPop(PointerRoutedEventArgs e)
            {
                TextBlock pointerDetails = new TextBlock();
                Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                pointerDetails.Name = ptrPt.PointerId.ToString();
                pointerDetails.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
                pointerDetails.Text = queryPointer(ptrPt);

                TranslateTransform x = new TranslateTransform();
                x.X = ptrPt.Position.X + 20;
                x.Y = ptrPt.Position.Y + 20;
                pointerDetails.RenderTransform = x;

                Container.Children.Add(pointerDetails);
            }
```

-   Anschließend wird Funktionalität bereitgestellt, um die Zeigerinformationen in einem vorhandenen [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)-Objekt zu aktualisieren, das diesem Zeiger zugeordnet ist.

```    CSharp
        void updateInfoPop(PointerRoutedEventArgs e)
            {
                foreach (var pointerDetails in Container.Children)
                {
                    if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
                    {
                        TextBlock _TextBlock = (TextBlock)pointerDetails;
                        if (_TextBlock.Name == e.Pointer.PointerId.ToString())
                        {
                            // To get pointer location details, we need extended pointer info.
                            // We get the pointer info through the getCurrentPoint method
                            // of the event argument. 
                            Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                            TranslateTransform x = new TranslateTransform();
                            x.X = ptrPt.Position.X + 20;
                            x.Y = ptrPt.Position.Y + 20;
                            pointerDetails.RenderTransform = x;
                            _TextBlock.Text = queryPointer(ptrPt);
                        }
                    }
                }
            }
```

-   Abschließend werden verschiedene Zeigereigenschaften abgefragt.

```    CSharp
         String queryPointer(PointerPoint ptrPt)
             {
                 String details = "";

                 switch (ptrPt.PointerDevice.PointerDeviceType)
                 {
                     case Windows.Devices.Input.PointerDeviceType.Mouse:
                         details += "\nPointer type: mouse";
                         break;
                     case Windows.Devices.Input.PointerDeviceType.Pen:
                         details += "\nPointer type: pen";
                         if (ptrPt.IsInContact)
                         {
                             details += "\nPressure: " + ptrPt.Properties.Pressure;
                             details += "\nrotation: " + ptrPt.Properties.Orientation;
                             details += "\nTilt X: " + ptrPt.Properties.XTilt;
                             details += "\nTilt Y: " + ptrPt.Properties.YTilt;
                             details += "\nBarrel button pressed: " + ptrPt.Properties.IsBarrelButtonPressed;
                         }
                         break;
                     case Windows.Devices.Input.PointerDeviceType.Touch:
                         details += "\nPointer type: touch";
                         details += "\nrotation: " + ptrPt.Properties.Orientation;
                         details += "\nTilt X: " + ptrPt.Properties.XTilt;
                         details += "\nTilt Y: " + ptrPt.Properties.YTilt;
                         break;
                     default:
                         details += "\nPointer type: n/a";
                         break;
                 }

                 GeneralTransform gt = Target.TransformToVisual(page);
                 Point screenPoint;

                 screenPoint = gt.TransformPoint(new Point(ptrPt.Position.X, ptrPt.Position.Y));
                 details += "\nPointer Id: " + ptrPt.PointerId.ToString() +
                     "\nPointer location (parent): " + ptrPt.Position.X + ", " + ptrPt.Position.Y +
                     "\nPointer location (screen): " + screenPoint.X + ", " + screenPoint.Y;
                 return details;
             }
```

### <span id="Complete_example"></span><span id="complete_example"></span><span id="COMPLETE_EXAMPLE"></span>Vollständiges Beispiel

Im Folgenden ist der C#-Code für dieses Beispiel aufgeführt. Links zu komplexeren Beispielen finden Sie weiter unten unter „Verwandte Artikel“.

```CSharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Input;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;

// The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=234238

namespace PointerInput
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        // For this example, we track simultaneous contacts in case the 
        // number of contacts has reached the maximum supported by the device.
        // Depending on the device, additional contacts might be ignored 
        // (PointerPressed not fired). 
        uint numActiveContacts;
        Windows.Devices.Input.TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();

        // Dictionary to maintain information about each active contact. 
        // An entry is added during PointerPressed/PointerEntered events and removed 
        // during PointerReleased/PointerCaptureLost/PointerCanceled/PointerExited events.
        Dictionary<uint, Windows.UI.Xaml.Input.Pointer> contacts;

        public MainPage()
        {
            this.InitializeComponent();
            numActiveContacts = 0;
            // Initialize the dictionary.
            contacts = new Dictionary<uint, Windows.UI.Xaml.Input.Pointer>((int)touchCapabilities.Contacts);
            // Declare the pointer event handlers.
            Target.PointerPressed += new PointerEventHandler(Target_PointerPressed);
            Target.PointerEntered += new PointerEventHandler(Target_PointerEntered);
            Target.PointerReleased += new PointerEventHandler(Target_PointerReleased);
            Target.PointerExited += new PointerEventHandler(Target_PointerExited);
            Target.PointerCanceled += new PointerEventHandler(Target_PointerCanceled);
            Target.PointerCaptureLost += new PointerEventHandler(Target_PointerCaptureLost);
            Target.PointerMoved += new PointerEventHandler(Target_PointerMoved);
            Target.PointerWheelChanged += new PointerEventHandler(Target_PointerWheelChanged);

            buttonClear.Click += new RoutedEventHandler(ButtonClear_Click);
        }

        private void ButtonClear_Click(object sender, RoutedEventArgs e)
        {
            eventLog.Text = "";
        }


        // PointerPressed and PointerReleased events do not always occur in pairs. 
        // Your app should listen for and handle any event that might conclude a pointer down action 
        // (such as PointerExited, PointerCanceled, and PointerCaptureLost).
        // For this example, we track the number of contacts in case the 
        // number of contacts has reached the maximum supported by the device.
        // Depending on the device, additional contacts might be ignored 
        // (PointerPressed not fired). 
        void Target_PointerPressed(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nDown: " + ptr.PointerId;

            // Change background color of target when pointer contact detected.
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Green);

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Lock the pointer to the target.
            Target.CapturePointer(e.Pointer);

            // Update event sequence.
            eventLog.Text += "\nPointer captured: " + ptr.PointerId;

            // Check if pointer already exists (for example, enter occurred prior to press).
            if (contacts.ContainsKey(ptr.PointerId))
            {
                return;
            }
            // Add contact to dictionary.
            contacts[ptr.PointerId] = ptr;
            ++numActiveContacts;

            // Display pointer details.
            createInfoPop(e);
        }

        void Target_PointerReleased(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nUp: " + ptr.PointerId;

            // If event source is mouse or touchpad and the pointer is still 
            // over the target, retain pointer and pointer details.
            // Return without removing pointer from pointers dictionary.
            // For this example, we assume a maximum of one mouse pointer.
            if (ptr.PointerDeviceType != Windows.Devices.Input.PointerDeviceType.Mouse)
            {
                // Update target UI.
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);

                destroyInfoPop(ptr);

                // Remove contact from dictionary.
                if (contacts.ContainsKey(ptr.PointerId))
                {
                    contacts[ptr.PointerId] = null;
                    contacts.Remove(ptr.PointerId);
                    --numActiveContacts;
                }

                // Release the pointer from the target.
                Target.ReleasePointerCapture(e.Pointer);

                // Update event sequence.
                eventLog.Text += "\nPointer released: " + ptr.PointerId;

                // Prevent most handlers along the event route from handling the same event again.
                e.Handled = true;
            }
            else
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
            }

        }

        private void Target_PointerMoved(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Multiple, simultaneous mouse button inputs are processed here.
            // Mouse input is associated with a single pointer assigned when 
            // mouse input is first detected. 
            // Clicking additional mouse buttons (left, wheel, or right) during 
            // the interaction creates secondary associations between those buttons 
            // and the pointer through the pointer pressed event. 
            // The pointer released event is fired only when the last mouse button 
            // associated with the interaction (not necessarily the initial button) 
            // is released. 
            // Because of this exclusive association, other mouse button clicks are 
            // routed through the pointer move event.          
            if (ptr.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse)
            {
                // To get mouse state, we need extended pointer details.
                // We get the pointer info through the getCurrentPoint method
                // of the event argument. 
                Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                if (ptrPt.Properties.IsLeftButtonPressed)
                {
                    eventLog.Text += "\nLeft button: " + ptrPt.PointerId;
                }
                if (ptrPt.Properties.IsMiddleButtonPressed)
                {
                    eventLog.Text += "\nWheel button: " + ptrPt.PointerId;
                }
                if (ptrPt.Properties.IsRightButtonPressed)
                {
                    eventLog.Text += "\nRight button: " + ptrPt.PointerId;
                }
            }

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Display pointer details.
            updateInfoPop(e);
        }

        private void Target_PointerEntered(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nEntered: " + ptr.PointerId;

            if (contacts.Count == 0)
            {
                // Change background color of target when pointer contact detected.
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
            }

            // Check if pointer already exists (if enter occurred prior to down).
            if (contacts.ContainsKey(ptr.PointerId))
            {
                return;
            }

            // Add contact to dictionary.
            contacts[ptr.PointerId] = ptr;
            ++numActiveContacts;

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Display pointer details.
            createInfoPop(e);
        }

        private void Target_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nMouse wheel: " + ptr.PointerId;

            // Check if pointer already exists (for example, enter occurred prior to wheel).
            if (contacts.ContainsKey(ptr.PointerId))
            {
                return;
            }

            // Add contact to dictionary.
            contacts[ptr.PointerId] = ptr;
            ++numActiveContacts;

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Display pointer details.
            createInfoPop(e);
        }

        // Fires for for various reasons, including: 
        //    - User interactions
        //    - Programmatic capture of another pointer
        //    - Captured pointer was deliberately released
        // PointerCaptureLost can fire instead of PointerReleased. 
        private void Target_PointerCaptureLost(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nPointer capture lost: " + ptr.PointerId;

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            destroyInfoPop(ptr);

            if (contacts.Count == 0)
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
            }
            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }

        // Fires for for various reasons, including: 
        //    - Touch contact canceled by pen coming into range of the surface.
        //    - The device doesn&#39;t report an active contact for more than 100ms.
        //    - The desktop is locked or the user logged off. 
        //    - The number of simultaneous contacts exceeded the number supported by the device.
        private void Target_PointerCanceled(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nPointer canceled: " + ptr.PointerId;

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            destroyInfoPop(ptr);

            if (contacts.Count == 0)
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
            }
            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }

        private void Target_PointerExited(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nPointer exited: " + ptr.PointerId;

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            // Update the UI and pointer details.
            destroyInfoPop(ptr);

            if (contacts.Count == 0)
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);
            }
            
            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }

        void createInfoPop(PointerRoutedEventArgs e)
        {
            TextBlock pointerDetails = new TextBlock();
            Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
            pointerDetails.Name = ptrPt.PointerId.ToString();
            pointerDetails.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
            pointerDetails.Text = queryPointer(ptrPt);

            TranslateTransform x = new TranslateTransform();
            x.X = ptrPt.Position.X + 20;
            x.Y = ptrPt.Position.Y + 20;
            pointerDetails.RenderTransform = x;

            Container.Children.Add(pointerDetails);
        }

        void destroyInfoPop(Windows.UI.Xaml.Input.Pointer ptr)
        {
            foreach (var pointerDetails in Container.Children)
            {
                if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
                {
                    TextBlock _TextBlock = (TextBlock)pointerDetails;
                    if (_TextBlock.Name == ptr.PointerId.ToString())
                    {
                        Container.Children.Remove(pointerDetails);
                    }
                }
            }
        }

        void updateInfoPop(PointerRoutedEventArgs e)
        {
            foreach (var pointerDetails in Container.Children)
            {
                if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
                {
                    TextBlock _TextBlock = (TextBlock)pointerDetails;
                    if (_TextBlock.Name == e.Pointer.PointerId.ToString())
                    {
                        // To get pointer location details, we need extended pointer info.
                        // We get the pointer info through the getCurrentPoint method
                        // of the event argument. 
                        Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                        TranslateTransform x = new TranslateTransform();
                        x.X = ptrPt.Position.X + 20;
                        x.Y = ptrPt.Position.Y + 20;
                        pointerDetails.RenderTransform = x;
                        _TextBlock.Text = queryPointer(ptrPt);
                    }
                }
            }
        }

         String queryPointer(PointerPoint ptrPt)
         {
             String details = "";

             switch (ptrPt.PointerDevice.PointerDeviceType)
             {
                 case Windows.Devices.Input.PointerDeviceType.Mouse:
                     details += "\nPointer type: mouse";
                     break;
                 case Windows.Devices.Input.PointerDeviceType.Pen:
                     details += "\nPointer type: pen";
                     if (ptrPt.IsInContact)
                     {
                         details += "\nPressure: " + ptrPt.Properties.Pressure;
                         details += "\nrotation: " + ptrPt.Properties.Orientation;
                         details += "\nTilt X: " + ptrPt.Properties.XTilt;
                         details += "\nTilt Y: " + ptrPt.Properties.YTilt;
                         details += "\nBarrel button pressed: " + ptrPt.Properties.IsBarrelButtonPressed;
                     }
                     break;
                 case Windows.Devices.Input.PointerDeviceType.Touch:
                     details += "\nPointer type: touch";
                     details += "\nrotation: " + ptrPt.Properties.Orientation;
                     details += "\nTilt X: " + ptrPt.Properties.XTilt;
                     details += "\nTilt Y: " + ptrPt.Properties.YTilt;
                     break;
                 default:
                     details += "\nPointer type: n/a";
                     break;
             }

             GeneralTransform gt = Target.TransformToVisual(page);
             Point screenPoint;

             screenPoint = gt.TransformPoint(new Point(ptrPt.Position.X, ptrPt.Position.Y));
             details += "\nPointer Id: " + ptrPt.PointerId.ToString() +
                 "\nPointer location (parent): " + ptrPt.Position.X + ", " + ptrPt.Position.Y +
                 "\nPointer location (screen): " + screenPoint.X + ", " + screenPoint.Y;
             return details;
         }
    }
}
```

## <span id="related_topics"></span>Verwandte Artikel


**Beispiele**
* [Einfaches Eingabebeispiel](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Eingabebeispiel mit geringer Latenz](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Beispiel für den Benutzerinteraktionsmodus](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Beispiel für visuelle Fokuselemente](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Archivbeispiele**
* [Eingabe: Beispiel für XAML-Benutzereingabeereignisse](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Eingabe: Beispiel für Gerätefunktionen](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Eingabe: Beispiel für Bearbeitungen und Bewegungen (C++)](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [Eingabe: Beispiel für Fingereingabe-Treffertests](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [Beispiel für XAML-Bildlauf, -Verschiebung und -Zoom](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Eingabe: vereinfachtes Freihandbeispiel](http://go.microsoft.com/fwlink/p/?linkid=246570)
 

 






<!--HONumber=May16_HO2-->


