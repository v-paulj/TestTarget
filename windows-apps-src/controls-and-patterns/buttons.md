---
author: Jwmsft
label: Buttons
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: c183f7390c5b4f99cf0f31426c1431066e1bc96d
ms.openlocfilehash: 91399060e129df18acd38e18d98cad848667a5ad

---
# Schaltflächen
Eine Schaltfläche ermöglicht dem Benutzer das unmittelbare Auslösen einer Aktion.

![Beispiel für Schaltflächen](images/controls/button.png)


<span class="sidebar_heading" style="font-weight: bold;">Wichtige APIs</span>

-   [**Button-Klasse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)
-   [**RepeatButton-Klasse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx)
-   [**Click-Ereignis**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx)

## Ist dies das richtige Steuerelement?

Mit einer Schaltfläche kann ein Benutzer unmittelbar eine Aktion auslösen. Beispiel: Absenden eines Formulars.

Schaltflächen sollten nicht verwendet werden, um zu anderen Seiten zu navigieren. Links sind für diesen Zweck besser geeignet. Weitere Informationen finden Sie unter [Hyperlinks](hyperlinks.md).
    
> Ausnahme: Für die Navigation in einem Assistenten können die Schaltflächen „Zurück“ und „Weiter“ verwendet werden. Für andere Arten der Rückwärtsnavigation oder der Navigation zu einer übergeordneten Ebene sollte stattdessen eine Zurück-Schaltfläche verwendet werden.

## Beispiel

In diesem Beispiel werden zwei Schaltflächen („Alle schließen“ und „Abbrechen“) in einem Dialogfeld des Microsoft Edge-Browsers verwendet. 

![Beispiel für Schaltflächen in einem Dialogfeld](images/control-examples/buttons-edge.png)

## Erstellen einer Schaltfläche

Dieses Beispiel zeigt eine Schaltfläche, die auf einen Mausklick reagiert. 

Erstellen Sie die Schaltfläche in XAML.

```xaml
<Button Content="Submit" Click="SubmitButton_Click"/>
```

Sie können die Schaltfläche auch in Code erstellen.

```csharp
Button submitButton = new Button();
submitButton.Content = "Submit";
submitButton.Click += SubmitButton_Click;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(submitButton);
```

Behandeln Sie das Click-Ereignis.

```csharp
private async void SubmitButton_Click(object sender, RoutedEventArgs e)
{
    // Call app specific code to submit form. For example:
    // form.Submit();
    Windows.UI.Popups.MessageDialog messageDialog = 
        new Windows.UI.Popups.MessageDialog("Thank you for your submission.");
    await messageDialog.ShowAsync();
}
```

### Interaktion mit Schaltflächen

Wenn Sie mit einem Finger oder Stift auf eine Schaltfläche tippen oder mit der linken Maustaste darauf klicken, löst die Schaltfläche das [**Click**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx)-Ereignis aus. Bei einer Schaltfläche mit Tastaturfokus wird das Click-Ereignis auch durch Drücken der Eingabe- oder Leertaste ausgelöst.

Sie können für Schaltflächen generell keine [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx)-Ereignisse auf niedriger Ebene verarbeiten, da diese stattdessen mit dem Click-Verhalten konfiguriert sind. Weitere Informationen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](https://msdn.microsoft.com/en-us/library/windows/apps/mt185584.aspx).

Sie können durch Ändern der [**ClickMode**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.clickmode.aspx)-Eigenschaft festlegen, wie eine Schaltfläche das Click-Ereignis auslöst. Der ClickMode-Standardwert lautet **Release**. Wenn als ClickMode-Wert **Hover** festgelegt ist, kann das Click-Event nicht über die Tastatur oder durch Berührung ausgelöst werden. 


### Inhalt von Schaltflächen

„Button“ ist ein [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.aspx)-Steuerelement. Die XAML-Inhaltseigenschaft ist [**Content**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx). Damit ist für XAML folgende Syntax möglich: `<Button>A button's content</Button>`. Sie können jedes Objekt als Inhalt der Schaltfläche festlegen. Wenn der Inhalt ein [UIElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx) ist, wird er in der Schaltfläche gerendert. Wenn es sich beim Inhalt um einen anderen Objekttyp handelt, wird die entsprechende Zeichenfolgendarstellung in der Schaltfläche angezeigt.

Im folgenden Beispiel wird ein **StackPanel**-Element, das das Bild einer Banane und Text enthält, als Inhalt einer Schaltfläche festgelegt.

```xaml
<Button Click="Button_Click" 
        Background="#FF0D6AA3" 
        Height="100" Width="80">
    <StackPanel>
        <Image Source="Assets/Slices.png" Height="62"/>
        <TextBlock Text="Orange"  Foreground="White"
                   HorizontalAlignment="Center"/>
    </StackPanel>
</Button>
```

Die Schaltfläche sieht wie folgt aus.

![Eine Schaltfläche mit Bild- und Textinhalt](images/button-orange.png)

## Erstellen einer Wiederholungsschaltfläche

[
            **RepeatButton**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx) ist eine Schaltfläche, die [**Click**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx)-Ereignisse auslöst, die andauern, solange die Schaltfläche betätigt wird. Legen Sie mit der [**Delay**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.delay.aspx)-Eigenschaft fest, wie lange „RepeatButton“ nach dem Betätigen der Schaltfläche wartet, bis die Klickaktion wiederholt wird. Legen Sie mit der [**Interval**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.interval.aspx)-Eigenschaft das Intervall zwischen Wiederholungen der Klickaktion fest. Die Zeiten beider Eigenschaften werden in Millisekunden angegeben.

Das folgende Beispiel zeigt zwei „RepeatButton“-Steuerelemente. Ihre jeweiligen Click-Ereignisse dienen dazu, den Wert in einem Textblock zu erhöhen oder zu verringern.

```xaml
<StackPanel>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Increase_Click">Increase</RepeatButton>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Decrease_Click">Decrease</RepeatButton>
    <TextBlock x:Name="clickTextBlock" Text="Number of Clicks:" />
</StackPanel>
```

```csharp
private static int _clicks = 0;
private void Increase_Click(object sender, RoutedEventArgs e)
{
    _clicks += 1;
    clickTextBlock.Text = "Number of Clicks: " + _clicks;
}

private void Decrease_Click(object sender, RoutedEventArgs e)
{
    if(_clicks > 0)
    {
        _clicks -= 1;
        clickTextBlock.Text = "Number of Clicks: " + _clicks;
    }
}
```

## Empfehlungen

-   Der Zweck und Status einer Schaltfläche müssen für den Benutzer eindeutig sein.
-   Verwenden Sie kurze, spezifische und selbsterklärende Texte, aus denen die Funktion einer Schaltfläche eindeutig hervorgeht. In der Regel umfasst der Schaltflächen-Textinhalt ein einzelnes Wort: ein Verb.
-   Wenn der Textinhalt der Schaltfläche dynamisch ist, beispielsweise wenn er lokalisiert ist, ziehen Sie die Größenänderung der Schaltfläche und was mit den Steuerelementen herum passiert in Betracht.
-   Verwenden Sie für Befehlsschaltflächen mit Textinhalt eine Schaltflächen-Mindestbreite.
-   Vermeiden Sie es, für Textinhalt schmale, kurze oder hohe Befehlsschaltflächen zu verwenden.
-   Verwenden Sie die standardmäßige Schriftart, es sei denn, Sie müssen entsprechend Ihren Markenrichtlinien eine andere verwenden.
-   Für eine Aktion, die auf mehreren Seiten in Ihrer App verfügbar sein muss, sollten Sie die Verwendung einer [unteren App-Leiste](app-bars.md) in Erwägung ziehen, anstatt eine Schaltfläche auf mehreren Seiten zu duplizieren.
-   Stellen Sie dem Benutzer jeweils nur zwei Schaltflächen zur Verfügung, beispielsweise „Akzeptieren“ und „Abbrechen“. Wenn Sie dem Benutzer mehr Aktionen zur Verfügung stellen möchten, sollten Sie [Kontrollkästchen](checkbox.md) oder [Optionsfelder](radio-button.md) verwenden, über die der Benutzer Aktionen mit einer einzelnen Befehlsschaltfläche auslösen kann.
-   Verwenden Sie die standardmäßige Befehlsschaltfläche, um die häufigste oder empfohlene Aktion anzuzeigen.
-   Ziehen Sie in Erwägung, Ihre Schaltflächen anzupassen. Schaltfläche sind standardmäßig rechteckig. Sie können aber die visuellen Effekte anpassen, die das Erscheinungsbild der Schaltfläche ausmachen. Der Inhalt einer Schaltfläche ist für gewöhnlich Text (beispielsweise „Akzeptieren“ oder „Abbrechen“). Sie könnten den Text aber auch durch ein Symbol ersetzen oder ein Symbol und Text kombinieren.
-   Stellen Sie sicher, dass sich, sobald der Benutzer eine Schaltfläche betätigt, der Status und das Erscheinungsbild der Schaltfläche ändern, um dem Benutzer ein Rückmeldung zu geben. „Normal“, „pressed“ und „disabled“ sind Beispiele von Schaltflächenstatus.
-   Lösen Sie die Aktion der Schaltfläche aus, wenn der Benutzer auf die Schaltfläche tippt oder drückt. Die Aktion wird für gewöhnlich ausgelöst, wenn der Benutzer die Schaltfläche loslässt. Sie können aber auch festlegen, dass die Aktion einer Schaltfläche durch Berühren mit dem Finger ausgelöst wird.
-   Verwenden Sie keine Befehlsschaltfläche zum Festlegen des Status.
-   Ändern Sie den Schaltflächentext nicht, während die App ausgeführt wird (z. B. den Text einer Schaltfläche „Weiter” in „Fortsetzen”).
-   Tauschen Sie nicht die standardmäßigen Stile „submit“, „reset“ und „button“.
-   Überfrachten Sie eine Schaltfläche nicht mit Inhalt. Inhalte von Steuerelementen sollten kurz und prägnant sein (nicht mehr als ein Bild und ein kurzer Text).

## Zurück-Schaltflächen
Die Zurück-Schaltfläche ist ein durch das System bereitgestelltes UI-Angebot, das die Rückwärtsnavigation über den Back-Stapel oder den Navigationsverlauf des Benutzers ermöglicht.

Der Bereich des Navigationsverlaufs (in der App oder global) ist vom Gerät und vom Gerätemodus abhängig.

## <span id="examples"></span><span id="EXAMPLES"></span>Beispiel


Die Benutzeroberfläche für die Zurück-Schaltfläche des Systems ist für jedes Gerät und jeden Eingabetyp optimiert. Die Navigationsfunktion hingegen ist auf allen Geräten und UWP-Apps (Universelle Windows-Plattform) global und einheitlich gültig. Zu diesen Unterschieden zählen:

Geräte Smartphone ![Zurück-Funktion des Systems auf einem Smartphone](images/nav-back-phone.png)
-   Immer vorhanden.
-   Eine Software- oder Hardwareschaltfläche am unteren Rand des Geräts.
-   Globale Rückwärtsnavigation innerhalb der App und zwischen Apps.

<span id="Tablet"></span><span id="tablet"></span><span id="TABLET"></span>Tablet ![Zurück-Funktion des Systems auf einem Tablet (im Tablet-Modus)](images/nav-back-tablet.png)
-   Im Tablet-Modus immer vorhanden.

    In Desktopmodus nicht verfügbar. Stattdessen kann die Zurück-Schaltfläche in der Titelleiste aktiviert werden. Weitere Infos finden Sie unter [PC, Laptop, Tablet](#PC).

    Benutzer können zwischen dem Tablet- und dem Desktopmodus wechseln, indem sie unter **Einstellungen &gt; System &gt; Tablet-Modus** die Option **Durch die Verwendung des Geräts als Tablet wird die Toucheingabe in Windows verbessert** aktivieren.

-   Eine Softwareschaltfläche in der Navigationsleiste am unteren Rand des Geräts.
-   Globale Rückwärtsnavigation innerhalb der App und zwischen Apps.

<span id="PC"></span><span id="pc"></span>PC, Laptop, Tablet ![Zurück-Funktion des Systems auf einem PC oder Laptop](images/nav-back-pc.png)
-   Optional im Desktopmodus.

    Im Tablet-Modus nicht verfügbar. Weitere Infos finden Sie unter [Tablet](#Tablet).

    Standardmäßig deaktiviert. Muss zur Verwendung aktiviert werden.

    Benutzer können zwischen dem Tablet- und dem Desktopmodus wechseln, indem sie unter **Einstellungen &gt; System &gt; Tablet-Modus** die Option **Durch die Verwendung des Geräts als Tablet wird die Toucheingabe in Windows verbessert** aktivieren.

-   Eine Softwareschaltfläche in der Titelleiste der App.
-   Ermöglicht die Rückwärtsnavigation nur innerhalb der App. Unterstützt keine App-zu-App-Navigation.

Surface Hub ![Zurück-Funktion des Systems auf einem Surface Hub](images/nav-back-surfacehub.png)
-   Immer vorhanden.
-   Eine Softwareschaltfläche am unteren Rand des Geräts.
-   Ermöglicht die Rückwärtsnavigation innerhalb der App und zwischen Apps.

 

## Empfohlene und nicht empfohlene Vorgehensweisen


-   Ermöglichen Sie eine Rückwärtsnavigation.

    Ohne Rückwärtsnavigation ist die App zwar im globalen Back-Stapel enthalten, der In-App-Seitennavigationsverlauf wird jedoch nicht gespeichert.

-   Aktivieren Sie die Zurück-Schaltfläche der Titelleiste im Desktopmodus.

    Der In-App-Seitennavigationsverlauf wird beibehalten, die Rückwärtsnavigation von App zu App wird nicht unterstützt.

    **Hinweis**  Im Tablet-Modus wird die Titelleiste angezeigt, wenn ein Benutzer vom oberen Rand des Geräts nach unten wischt oder den Mauszeiger an den oberen Rand des Geräts bewegt. Um eine Duplizierung und Missverständnisse zu vermeiden, wird die Zurück-Schaltfläche für die Titelleiste im Tablet-Modus nicht angezeigt.

     

-   Blenden Sie die Zurück-Schaltfläche für die Titelleiste im Desktop-Modus aus, oder deaktivieren Sie sie, wenn ein In-App-Navigationsverlauf leer oder nicht verfügbar ist.

    Teilt dem Benutzer klar und deutlich mit, dass es nicht möglich ist, weiter zurück zu navigieren.

-   Jeder Zurück-Befehl sollte im Back-Stapel eine Seite nach hinten oder zu der unmittelbar vorangehenden App zurück navigieren, wenn nicht der Desktopmodus aktiv ist.

    Wenn die Rückwärtsnavigation nicht intuitiv, konsistent und vorhersagbar ist, könnte dies beim Benutzer zu Verwirrungen führen.

## Verwandte Artikel

- [Optionsfelder](radio-button.md)
- [Umschalter](toggles.md)
- [Kontrollkästchen](checkbox.md)

**Für Entwickler (XAML)**
- [**Button-Klasse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)





<!--HONumber=Jun16_HO4-->


