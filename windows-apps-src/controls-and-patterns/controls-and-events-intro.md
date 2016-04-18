---
Description: Sie erstellen die Benutzeroberfläche für Ihre App mit Steuerelementen wie Schaltflächen, Textfeldern und Kombinationsfeldern, um Daten anzuzeigen und Benutzereingaben zu erhalten. Hier zeigen wir Ihnen, wie Sie Ihrer App Steuerelemente hinzufügen.
title: Hinzufügen von Steuerelementen und Verarbeiten von Ereignissen
ms.assetid: 64740BF2-CAA1-419E-85D1-42EE7E15F1A5
label: Einführung in Steuerelemente und Ereignisse
template: detail.hbs
---
# Einführung in Steuerelemente und Ereignisse

Sie erstellen die Benutzeroberfläche für Ihre App mit Steuerelementen wie Schaltflächen, Textfeldern und Kombinationsfeldern, um Daten anzuzeigen und Benutzereingaben zu erhalten. Hier zeigen wir Ihnen, wie Sie Ihrer App Steuerelemente hinzufügen. Es gibt drei wichtige Schritte zum Hinzufügen von Steuerelementen zur App: 

- Fügen Sie Ihrer App-UI ein Steuerelement hinzu. 
- Legen Sie Eigenschaften für das Steuerelement fest, z. B. Breite, Höhe und Vordergrundfarbe. 
- Fügen Sie den Ereignishandlern des Steuerelements Code hinzu, damit sie eine Funktion haben. 

## Hinzufügen eines Steuerelements
Es gibt mehrere Möglichkeiten, einer App ein Steuerelement hinzuzufügen:
 
- Verwenden Sie ein Entwicklungstool wie Blend für Visual Studio oder Microsoft Visual Studio Extensible Application Markup Language (XAML)-Designer. 
- Fügen Sie das Steuerelement dem XAML-Markup im XAML-Editor in Visual Studio hinzu. 
- Fügen Sie das Steuerelement im Code hinzu. Im Code hinzugefügte Steuerelemente sind bei der Ausführung der App, jedoch nicht im Visual Studio-XAML-Designer sichtbar.

Wenn Sie in Visual Studio Steuerelemente in Ihrer App hinzufügen und bearbeiten, können Sie viele Features des Programms wie die Toolbox, den XAML-Designer, den XAML-Editor und das Eigenschaftenfenster nutzen. 

In der Toolbox von Visual Studio werden viele Steuerelemente angezeigt, die Sie in Ihrer App verwenden können. Wenn Sie der App ein Steuerelement hinzufügen möchten, doppelklicken Sie in der Toolbox auf das gewünschte Steuerelement. Wenn Sie beispielsweise auf das „TextBox“-Steuerelement doppelklicken, wird der XAML-Ansicht folgender XAML-Code hinzugefügt. 

```xaml
<TextBox HorizontalAlignment="Left" Text="TextBox" VerticalAlignment="Top"/>
```

Sie können das Steuerelement auch aus der Toolbox in den XAML-Designer ziehen.

## Festlegen des Namens eines Steuerelements 

Wenn Sie mit einem Steuerelement im Code arbeiten möchten, legen Sie das Attribut [x:Name](../xaml-platform/x-name-attribute.md) fest und verweisen im Code anhand des Namens darauf. Sie können den Namen im Eigenschaftenfenster von Visual Studio oder in XAML festlegen. Hier wird veranschaulicht, wie Sie den Namen des derzeit ausgewählten Steuerelements über das Textfeld Name am oberen Rand des Eigenschaftenfensters festlegen. 

So benennen Sie ein Steuerelement
1. Wählen Sie das zu benennende Element aus.
2. Geben Sie im Eigenschaftenpanel einen Namen in das Textfeld Name ein.
3. Drücken Sie die EINGABETASTE, um den Namen zu übernehmen.

![Name-Eigenschaft im Visual Studio-Designer](images/add-controls-control-name-designer.png)

Hier wird erläutert, wie Sie den Namen eines Steuerelements im XAML-Editor festlegen, indem Sie das x:Name-Attribut hinzufügen.

```xaml
<Button x:Name="Button1" Content="Button"/>
```

## Festlegen von Steuerelementeigenschaften 

Mithilfe von Eigenschaften geben Sie das Erscheinungsbild, den Inhalt sowie weitere Attribute von Steuerelementen an. Wenn Sie ein Steuerelement mit einem Entwicklungstool hinzufügen, werden manche Eigenschaften, die Größe, Position und Inhalt steuern, möglicherweise von Visual Studio für Sie festgelegt. Sie können einige Eigenschaften wie etwa die Breite, die Höhe oder den Rand ändern, indem Sie das Steuerelement in der Entwurfsansicht auswählen und bearbeiten. In der Abbildung werden einige der in der Entwurfsansicht verfügbaren Größenanpassungstools veranschaulicht. 

![Größenanpassungstools im Visual Studio-Designer](images/add-controls-resizing-designer.png)

Sie können die Größe und die Position des Steuerelements auch automatisch festlegen lassen. In diesem Fall können Sie die von Visual Studio festgelegten Eigenschaften für Größe und Position zurücksetzen.

Zurücksetzen einer Eigenschaft
1. Klicken Sie im Bereich Eigenschaften auf den Eigenschaftsmarker neben dem Eigenschaftswert. Das Eigenschaftsmenü wird geöffnet.
2. Klicken Sie im Eigenschaftsmenü auf „Zurücksetzen“.

![Option „Zurücksetzen“ im Visual Studio-Menü „Eigenschaften“](images/add-controls-property-reset.png)

Sie können Steuerelementeigenschaften im Eigenschaftenfenster, in XAML oder im Code festlegen. Wenn Sie beispielsweise die Vordergrundfarbe einer Schaltfläche ändern möchten, legen Sie die „Foreground“-Eigenschaft des Steuerelements fest. In dieser Abbildung wird veranschaulicht, wie die „Foreground“-Eigenschaft über die Farbauswahl im Eigenschaftenfenster festgelegt wird. 

![Farbauswahl im Visual Studio-Designer](images/add-controls-foreground-designer.png)

Hier wird beschrieben, wie Sie die „Foreground“-Eigenschaft im XAML-Editor festlegen. Beachten Sie das Visual Studio IntelliSense-Fenster, in dem hilfreiche Informationen zur Syntax angezeigt werden. 

![IntelliSense in XAML Teil 1](images/add-controls-foreground-xaml.png)

![IntelliSense in XAML Teil 2](images/add-controls-foreground-xaml-2.png)

Im Folgenden finden Sie den XAML-Code, der nach dem Festlegen der „Foreground“-Eigenschaft vorliegt. 

```xaml
<Button x:Name="Button1" Content="Button" 
        HorizontalAlignment="Left" VerticalAlignment="Top"
        Foreground="Beige"/>
```

An dieser Stelle wird beschrieben, wie die „Foreground“-Eigenschaft im Code festgelegt wird. 

```csharp
Button1.Foreground = new SolidColorBrush(Windows.UI.Colors.Beige);
```

## Erstellen eines Ereignishandlers 

Jedes Steuerelement verfügt über Ereignisse, mit denen Sie auf Aktionen seitens des Benutzers oder sonstige Änderungen in der App reagieren können. Das „Button“-Steuerelement stellt beispielsweise ein Click-Ereignis bereit, das ausgelöst wird, wenn ein Benutzer auf die Schaltfläche klickt. Sie erstellen eine als Ereignishandler bezeichnete Methode, mit der das Ereignis behandelt wird. Sie können das Ereignis eines Steuerelements einer Ereignishandlermethode im Eigenschaftenfenster, in XAML oder im Code zuordnen. Weitere Informationen zu Ereignissen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](../xaml-platform/events-and-routed-events-overview.md).

Wählen Sie zum Erstellen eines Ereignishandlers das Steuerelement aus, und klicken Sie oben im Eigenschaftenfenster auf die Registerkarte „Ereignisse“. Im Eigenschaftenfenster werden alle für das betreffende Steuerelement verfügbaren Ereignisse aufgelistet. Im Folgenden werden einige Ereignisse für eine Schaltfläche aufgelistet.

![Ereignisliste in Visual Studio](images/add-controls-add-event-designer.png)

Wenn Sie einen Ereignishandler mit dem Standardnamen erstellen möchten, doppelklicken Sie im Eigenschaftenfenster auf das Textfeld neben dem Ereignisnamen. Wenn Sie einen Ereignishandler mit einem benutzerdefinierten Namen erstellen möchten, geben Sie im Textfeld den gewünschten Namen ein, und drücken Sie die EINGABETASTE. Der Ereignishandler wird erstellt, und die CodeBehind-Datei wird im Code-Editor geöffnet. Die Ereignishandlermethode enthält zwei Parameter. Der erste Parameter ist `sender`. Dabei handelt es sich um einen Verweis auf das Objekt, dem der Handler angefügt ist. Der `sender`-Parameter ist ein **Object**-Typ. In der Regel wandeln Sie `sender` in einen genaueren Typ um, wenn Sie den Zustand direkt für das `sender`-Objekt überprüfen oder ändern möchten. Abhängig davon, wo der Handler angefügt wird, handelt es sich dabei basierend auf Ihrem eigenen App-Design voraussichtlich um einen Typ, in den `sender` sicher umgewandelt werden kann. Den zweiten Wert stellen Ereignisdaten dar. In der Regel sind diese in Signaturen als `e`- oder `args`-Parameter enthalten.

Hier finden Sie den Code, der das Click-Ereignis einer Schaltfläche namens `Button1` behandelt. Wenn Sie auf die Schaltfläche klicken, wird die „Foreground“-Eigenschaft der Schaltfläche auf Blau gesetzt. 

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    Button b = (Button)sender;
    b.Foreground = new SolidColorBrush(Windows.UI.Colors.Blue);
}
```

Sie können einen Ereignishandler auch in XAML zuordnen. Geben Sie im XAML-Editor den Namen des Ereignisses ein, das behandelt werden soll. In Visual Studio wird ein IntelliSense-Fenster geöffnet, sobald Sie mit der Eingabe beginnen. Nach dem Angeben des Ereignisses können Sie im IntelliSense-Fenster auf `<New Event Handler>` doppelklicken, um einen neuen Ereignishandler mit dem Standardnamen zu erstellen oder einen vorhandenen Ereignishandler aus der Liste auszuwählen. 

Das folgende IntelliSense-Fenster wird angezeigt. Sie können darin einen neuen Ereignishandler erstellen oder einen vorhandenen Ereignishandler auswählen.

![IntelliSense für das Click-Ereignis](images/add-controls-add-event-xaml.png)

In diesem Beispiel wird veranschaulicht, wie in XAML ein Click-Ereignis einem Ereignishandler namens „Button_Click“ zugeordnet wird. 

```xaml
<Button Name="Button1" Content="Button" Click="Button_Click"/>
```

Sie können ein Ereignis auch dem zugehörigen Ereignishandler im „CodeBehind“ zuordnen. Im Folgenden wird beschrieben, wie ein Ereignishandler im Code zugeordnet wird.

```csharp
Button1.Click += new RoutedEventHandler(Button_Click);
```

\[Dieser Artikel enthält spezielle Informationen zu Apps für die universelle Windows-Plattform (UWP) und Windows 10. Laden Sie für Windows 8.1 die [PDF-Datei mit Windows 8.1-Richtlinien](https://go.microsoft.com/fwlink/p/?linkid=258743) herunter.\]

## Verwandte Themen

-   [**Befehlsleisten**](app-bars.md)
-   [Suchen](search.md)
-   [Flyouts](dialogs-popups-menus.md)


<!--HONumber=Mar16_HO1-->


