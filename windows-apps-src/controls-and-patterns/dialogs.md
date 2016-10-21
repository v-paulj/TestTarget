---
author: mijacobs
Description: "Dialogfelder und Flyouts zeigen vorübergehende UI-Elemente an, die angezeigt werden, wenn der Benutzer sie anfordert oder eine Aktion erfolgt, die eine Benachrichtigung oder Genehmigung erfordert."
title: Dialogfelder und Flyouts
label: Dialogs
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: ff9940c06276165dc139e120c4e9cdeb005ff125

---
# Dialogfelder und Flyouts

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Dialogfelder und Flyouts sind vorübergehende UI-Elemente, die angezeigt werden, wenn Ereignisse Benachrichtigungen, Genehmigungen oder zusätzliche Informationen vom Benutzer erfordern.

<div class="important-apis" >
<b>Wichtige APIs</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx">ContentDialog-Klasse</a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/dn279496">Flyout-Klasse</a></li>
</ul>

</div>
</div>



<!--
<table>
<tr>
<th>Dialogs</th><th>Flyouts</th>
</tr>
<tr>
<td>![Example of a full-button dialog](images/controls_dialog_twobutton.png)</td>
<td>![Example of a flyout](images/flyout-example.png)</td>
</tr>
<tr>
<td>Dialogs are modal UI overlays that provide contextual app information. Dialogs block interactions with the app window until being explicitly dismissed. They often request some kind of action from the user.  </td>
<td>A flyout is a lightweight contextual popup that displays UI related to what the user is doing. It includes placement and sizing logic, and can be used to reveal a hidden control, show more detail about an item, or ask the user to confirm an action. Flyouts can be quickly dismissed by tapping or clicking somewhere outside the flyout, pressing the Escape key or Back button, resizing the app window, or changing the device's orientation.
</td>
</tr>
</table>
-->

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>Dialogfelder</b> <br/><br/>
   ![Beispiel für ein Dialogfeld mit einer Schaltfläche](images/controls_dialog_twobutton.png)</p>
<p>Dialogfelder sind modale Benutzeroberflächenüberlagerungen, die kontextbezogene App-Informationen enthalten. Dialogfelder blockieren Interaktionen mit dem App-Fenster, bis sie explizit geschlossen werden. Sie verlangen häufig eine Aktion vom Benutzer.   
</p><br/>

  </div>
  <div class="side-by-side-content-right">
   <p><b>Flyouts</b> <br/><br/>
   ![Flyout-Beispiel](images/flyout-example.png)</p>
<p>Ein Flyout ist ein einfaches kontextbezogenes Popupmenü zum Anzeigen der Benutzeroberfläche, die im Zusammenhang mit den Aktionen des Benutzers steht. Es umfasst die Platzierung und Größe und kann ein ausgeblendetes Steuerelement oder weitere Details zu einem Element anzeigen oder den Benutzer zum Bestätigen einer Aktion auffordern. 
</p><p>Im Gegensatz zu einem Dialogfeld kann ein Flyout durch Tippen oder Klicken auf eine Stelle außerhalb des Flyouts schnell geschlossen werden. Das Drücken der ESC- oder Zurück-Taste sowie das Ändern der Größe des App-Fensters oder der Ausrichtung des Geräts haben denselben Effekt.
</p><br/>

  </div>
</div>
</div>

## Ist dies das richtige Steuerelement?

* Verwenden Sie Dialogfelder und Flyouts, um Benutzern wichtige Informationen zu melden oder deren Bestätigung oder zusätzliche Informationen anzufordern, bevor eine Aktion abgeschlossen werden kann. 
* Verwenden Sie keine Flyouts anstelle von [QuickInfos](tooltips.md) oder [Kontextmenüs](menus.md). Verwenden Sie QuickInfos, um kurze Beschreibungen anzuzeigen, die nach einer festgelegten Zeit ausgeblendet werden. Verwenden Sie ein Kontextmenü für kontextbezogene Aktionen im Zusammenhang mit UI-Elementen (beispielsweise Kopieren und Einfügen).  


Dialogfelder und Flyouts stellen sicher, dass den Benutzern wichtige Informationen bekannt sind, sie stellen jedoch auch eine Unterbrechung dar. Da Dialogfelder modal (gesperrt) sind, unterbrechen sie die Benutzer und verhindern, dass diese bis zur Interaktion mit dem Dialogfeld andere Schritte durchführen können. Flyouts sind weniger störend, das Anzeigen zu vieler Flyouts kann jedoch eine ablenkende Wirkung haben. 

Berücksichtigen Sie die Bedeutung der zu vermittelnden Informationen: Sind sie wichtig genug, um den Benutzer zu unterbrechen? Berücksichtigen Sie zudem, wie häufig die Informationen angezeigt werden müssen. Wenn ein Dialogfeld oder eine Benachrichtigung alle paar Minuten angezeigt wird, sollten Sie diese Informationen stattdessen in die primäre UI einbinden. So können Sie z.B. in einem Chat-Client anstatt eines Flyouts, der jedes Mal angezeigt wird, wenn sich ein Freund anmeldet, eine Liste der Freunde anzeigen, die derzeit online sind, und diejenigen Freunde hervorheben, die sich gerade anmelden. 

Flyouts und Dialogfelder werden häufig zum Bestätigen einer Aktion vor deren Ausführung verwendet (z.B. vor dem Löschen einer Datei). Wenn Sie davon ausgehen, dass die Benutzer häufig eine bestimmte Aktion ausführen, sollten Sie eine Möglichkeit bereitstellen, versehentliche Aktionen rückgängig zu machen, anstatt jedes Mal die Bestätigung der Aktion zu erzwingen. 



## Dialogfelder im Vergleich zu Flyouts

Wenn ein Dialogfeld oder Flyout eingesetzt werden soll, müssen Sie sich für eine der beiden Optionen entscheiden. 

<!--
Dialogs are modal, which means they block all interaction with the app until the user selects a dialog button. To visually reinforce their modal behavior, dialogs draw an overlay layer which partially obscures the temporarily unreachable app UI.

A flyout is a light dismiss control, meaning that users can choose from a variety of actions to quickly dismiss it. These interactions are intended to be lightweight and non-blocking. Light dismiss actions include

* Clicking or tap outside the transient UI
* Pressing the Escape key
* Pressing the Back button
* Resizing the app window
* Changing device orientation

-->

Angesichts der Tatsache, dass Dialogfelder im Gegensatz zu Flyouts Interaktionen blockieren, sollten Dialogfelder auf Situationen beschränkt bleiben, in denen der Benutzer unterbrochen werden soll, um sich auf eine bestimmte Information oder die Beantwortung einer Frage zu konzentrieren. Flyouts hingegen können verwendet werden, wenn Sie auf etwas aufmerksam machen möchten, das der Benutzer jedoch ignorieren kann. 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>Fälle, in denen ein Dialogfeld verwendet werden sollte:</b> <br/>
<ul>
<li>Für wichtige Informationen, die der Benutzer vor dem Fortsetzen lesen und bestätigen **muss**. Beispiele:
<ul>
  <li>Die Sicherheit des Benutzers ist möglicherweise gefährdet.</li>
  <li>Der Benutzer möchte eine wertvolle Ressource endgültig ändern.</li>
  <li>Der Benutzer möchte eine wertvolle Ressource löschen.</li>
  <li>Bestätigen eines In-App-Einkaufs</li>
</ul>

</li>
<li>Fehlermeldungen, die sich auf den Gesamtkontext der App beziehen, z. B. Verbindungsfehler.</li>
<li>Fragen, wenn dem Benutzer eine blockierende Frage gestellt werden muss, z. B. wenn die App nicht im Auftrag des Benutzers eine Auswahl treffen kann. Eine blockierende Frage kann nicht ignoriert oder verschoben werden und sollte dem Benutzer klar definierte Auswahlmöglichkeiten bieten.</li>
</ul> 
</p>
  </div>
  <div class="side-by-side-content-right">
   <p><b>Fälle, in denen ein Flyout verwendet werden sollte:</b> <br/>
<ul>
<li>Erfassen zusätzlicher Informationen, die erforderlich sind, bevor eine Aktion abgeschlossen werden kann.</li>
<li>Anzeigen von Informationen, die nur vorübergehend relevant sind. So können Sie z.B. in einer Fotogalerie-App ein Flyout einsetzen, damit eine große Version des Bilds angezeigt wird, wenn der Benutzer auf eine Miniaturansicht klickt.</li>
<li>Warnungen und Bestätigungen, z. B. im Zusammenhang mit möglicherweise schädlichen Aktionen.</li>
<li>Anzeigen weiterer Informationen, z. B. von Details oder ausführlicheren Beschreibungen eines Elements auf der Seite.</li>
</ul></p>
  </div>
</div>
</div>



## Nutzungsrichtlinien zur Verwendung von Dialogfeldern

-   Sie sollten das Problem oder das Ziel direkt in der ersten Zeile des Dialogfeldtexts deutlich benennen.
-   Der Dialogfeldtitel ist die Hauptanweisung und optional.
    -   Verwenden Sie einen kurzen Titel, um dem Benutzer die erforderlichen Aktionen im Dialogfeld zu erklären. Lange Titel werden nicht umgebrochen und daher abgeschnitten.
    -   Wenn Sie mit dem Dialogfeld eine einfache Meldung, einen Fehler oder eine Frage anzeigen möchten, können Sie den Titel optional auslassen. Vermitteln Sie dann im Inhalt die wichtigsten Informationen.
    -   Stellen Sie sicher, dass sich der Titel direkt auf die Auswahlmöglichkeiten der Schaltflächen bezieht.
-   Der Inhalt des Dialogfelds enthält den beschreibenden Text und ist erforderlich.
    -   Stellen Sie die Meldung, den Fehler oder die blockierende Frage so einfach wie möglich dar.
    -   Stellen Sie bei Verwendung eines Dialogfeldtitels mithilfe des Inhaltsbereichs weitere Details bereit, oder definieren Sie Terminologie. Wiederholen Sie nicht den Titel mit anderen Worten.
-   Mindestens eine Dialogfeldschaltfläche muss angezeigt werden.
    -   Schaltflächen sind die einzigen Mechanismen, mit denen Benutzer das Dialogfeld schließen können.
    -   Verwenden Sie Schaltflächen mit Text, die eine konkrete Reaktion auf die Hauptanweisung oder den Inhalt repräsentieren. Beispiel: "Möchten Sie AppName den Zugriff auf Ihren Standort erlauben?", gefolgt von den Schaltflächen "Zulassen" und "Blockieren". Klare Antworten erleichtern das Verständnis und damit die schnelle Entscheidungsfindung.
-   Bei Fehlerdialogfeldern wird die Fehlermeldung im Dialogfeld zusammen mit allen relevanten Informationen angezeigt. Die einzige in einem Fehlerdialogfeld verwendete Schaltfläche sollte „Schließen“ oder eine ähnliche Aktion sein.
-   Kontextbezogene Fehler, die sich auf eine bestimmte Stelle auf der Seite beziehen, beispielsweise Validierungsfehler (wie in Kennwortfeldern), verwenden die Canvas der App selbst zum Anzeigen von Inlinefehlern.

## Erstellen eines Dialogfelds
Um ein Dialogfeld zu erstellen, verwenden Sie die [ContentDialog-Klasse](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx). Sie können ein Dialogfeld im Code oder Markup erstellen. Obwohl es in der Regel leichter ist, UI-Elemente in XAML zu definieren, ist es bei einem einfachen Dialogfeld unkomplizierter, Code zu verwenden. In diesem Beispiel wird ein Dialogfeld erstellt, um dem Benutzer mitzuteilen, dass keine WLAN-Verbindung vorhanden ist. Für die Anzeige wird die Methode [ShowAsync](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialog.showasync.aspx) verwendet.

```csharp
private async void displayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog()
    {
        Title = "No wifi connection",
        Content = "Check connection and try again",
        PrimaryButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

Wenn der Benutzer auf eine Dialogfeldschaltfläche klickt, gibt die Methode [ShowAsync](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialog.showasync.aspx) ein [ContentDialogResult](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialogresult.aspx) zurück, damit Sie wissen, auf welche Schaltfläche der Benutzer klickt. 

Das Dialogfeld in diesem Beispiel stellt eine Frage und verwendet das zurückgegebene [ContentDialogResult](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialogresult.aspx), um die Antwort des Benutzers zu ermitteln. 

```csharp
private async void displayDeleteFileDialog()
{
    ContentDialog deleteFileDialog = new ContentDialog()
    {
        Title = "Delete file permanently?",
        Content = "If you delete this file, you won't be able to recover it. Do you want to delete it?",
        PrimaryButtonText = "Cancel",
        SecondaryButtonText = "Delete file permanently"
    };

    ContentDialogResult result = await deleteFileDialog.ShowAsync();
    
    // Delete the file if the user clicked the second button. 
    /// Otherwise, do nothing. 
    if (result == ContentDialogResult.Secondary)
    {
        // Delete the file. 
    }
}
```


##  Erstellen eines Flyouts

Ein Flyout ist ein offener Container, der beliebige UI als Inhalt anzeigen kann.  

Flyouts sind an bestimmte Steuerelemente angefügt. Sie können mit der [Placement](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.placement.aspx)-Eigenschaft angeben, wo das Flyout angezeigt wird: oben, links, unten, rechts oder als Vollbild. Wenn Sie den [vollständigen Platzierungsmodus](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutplacementmode.aspx) auswählen, streckt die App das Flyout und zentriert es innerhalb des App-Fensters. Wenn sie sichtbar sind, sollten sie am aufrufenden Objekt verankert sein und ihre bevorzugte relative Position zum Objekt angeben: oben, links, unten oder rechts. Flyout verfügt außerdem über einen vollständigen Platzierungsmodus, der versucht, das Flyout zu strecken und innerhalb des App-Fensters zu zentrieren. Einige Steuerelemente wie z.B. [Schaltflächen](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx) verfügen über die Eigenschaft [Flyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx), mit der Sie ein Flyout zuordnen können. 

In diesem Beispiel wird ein einfaches Flyout erstellt, das Text angezeigt, wenn die Schaltfläche gedrückt wird. 
````xaml
<Button Content="Click me">
  <Button.Flyout>
     <Flyout>
        <TextBlock Text="This is a flyout!"/>
     </Flyout>
  </Button.Flyout>
</Button>
````

Wenn das Steuerelement nicht über eine Flyout-Eigenschaft verfügt, können Sie stattdessen die angefügte Eigenschaft [FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx) verwenden. In diesem Fall müssen Sie zudem die [FlyoutBase.ShowAttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout)-Methode aufrufen, um das Flyout anzuzeigen. 

In diesem Beispiel wird einem Bild ein einfaches Flyout hinzugefügt. Wenn der Benutzer auf das Bild tippt, zeigt die App das Flyout an. 

````xaml
<Image Source="Assets/cliff.jpg" Width="50" Height="50" 
  Margin="10" Tapped="Image_Tapped">
  <FlyoutBase.AttachedFlyout>
    <Flyout>
      <TextBlock TextWrapping="Wrap" Text="This is some text in a flyout."  />
    </Flyout>        
  </FlyoutBase.AttachedFlyout>
</Image>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}
````

In den vorherigen Beispielen wurden die Flyouts inline definiert. Sie können ein Flyout auch als statische Ressource definieren und sie dann mit mehreren Elementen verwenden. In diesem Beispiel wird ein komplizierteres Flyout erstellt, mit dem eine größere Version eines Bilds angezeigt wird, wenn auf die Miniaturansicht getippt wird. 

````xaml
<!-- Declare the shared flyout as a resource. -->
<Page.Resources>
    <Flyout x:Key="ImagePreviewFlyout" Placement="Right">
        <!-- The flyout's DataContext must be the Image Source 
             of the image the flyout is attached to. -->
        <Image Source="{Binding Path=Source}" 
            MaxHeight="400" MaxWidth="400" Stretch="Uniform"/>
        <Flyout.FlyoutPresenterStyle>
            <Style TargetType="FlyoutPresenter">
                <Setter Property="ScrollViewer.ZoomMode" Value="Enabled"/>
                <Setter Property="Background" Value="Black"/>
                <Setter Property="BorderBrush" Value="Gray"/>
                <Setter Property="BorderThickness" Value="5"/>
                <Setter Property="MinHeight" Value="300"/>
                <Setter Property="MinWidth" Value="300"/>
            </Style>
        </Flyout.FlyoutPresenterStyle>
    </Flyout>
</Page.Resources>
````

````xaml
<!-- Assign the flyout to each element that shares it. -->
<StackPanel>
    <Image Source="Assets/cliff.jpg" Width="50" Height="50" 
           Margin="10" Tapped="Image_Tapped" 
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/grapes.jpg" Width="50" Height="50" 
           Margin="10" Tapped="Image_Tapped" 
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/rainier.jpg" Width="50" Height="50" 
           Margin="10" Tapped="Image_Tapped" 
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
</StackPanel>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);  
}
````

## Gestalten eines Flyouts
Um ein Flyout zu formatieren, ändern Sie den [FlyoutPresenterStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flyout.flyoutpresenterstyle.aspx). In diesem Beispiel wird ein Absatz mit Textumbruch dargestellt. Zudem wird der Textblock für ein Bildschirmleseprogramm zugänglich gemacht.

````xaml
<Flyout>
  <Flyout.FlyoutPresenterStyle>
    <Style TargetType="FlyoutPresenter">
      <Setter Property="ScrollViewer.HorizontalScrollMode" 
          Value="Disabled"/>
      <Setter Property="ScrollViewer.HorizontalScrollBarVisibility" Value="Disabled"/>
      <Setter Property="IsTabStop" Value="True"/>
      <Setter Property="TabNavigation" Value="Cycle"/>
    </Style>
  </Flyout.FlyoutPresenterStyle>
  <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."/>
</Flyout>
````

## Beispiele herunterladen
*   [XAML-UI-Grundlagen](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)<br/>
    Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## Verwandte Artikel
- [QuickInfos](tooltips.md)
- [Menüs und Kontextmenü](menus.md)
- [**Flyout-Klasse**](https://msdn.microsoft.com/library/windows/apps/dn279496)
- [**ContentDialog-Klasse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)



<!--HONumber=Aug16_HO3-->


