---
author: Jwmsft
label: App bars/command bars
template: detail.hbs
ms.sourcegitcommit: 7d438080e2e8533f1148c07e27143d4d1fcacf5d
ms.openlocfilehash: 01cd10c72745ff4bd8204a9adaa8eebf5a892efe

---

# App-Leiste und Befehlsleiste

Über Befehlsleisten (auch als „App-Leisten“ bezeichnet) können Benutzer komfortabel auf häufig verwendete Befehle in Ihrer App zugreifen und Befehle oder Optionen verwenden, die mit dem Kontext des Benutzers zusammenhängen (wie etwa Fotoauswahl oder Zeichnungsmodus). Sie können auch für die Navigation zwischen Seiten oder Abschnitten der App genutzt werden. Befehlsleisten können mit jedem Navigationsmuster verwendet werden.

![Beispiel für eine Befehlsleiste mit Symbolen](images/controls_appbar_icons.png)



-   [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx)
-   [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarbutton.aspx)
-   [**AppBarToggleButton**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbartogglebutton.aspx)
-   [**AppBarSeparator**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarseparator.aspx)

## Ist dies das richtige Steuerelement?

Das CommandBar-Steuerelement ist ein allgemeines, flexibles und kleines Steuerelement, mit dem komplexe Inhalte wie Bilder oder Textblöcke sowie einfache Befehle wie [AppBarButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarbutton.aspx)-, [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbartogglebutton.aspx)- und [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarseparator.aspx)-Steuerelemente angezeigt werden können.

XAML stellt das AppBar-Steuerelement und das CommandBar-Steuerelement bereit. Sie sollten AppBar nur verwenden, wenn Sie eine universelle Windows 8-App aktualisieren, in der AppBar verwendet wird, und möglichst wenige Änderungen vornehmen möchten. Für neue Apps in Windows 10 sollten Sie stattdessen das CommandBar-Steuerelement verwenden. In diesem Dokument wird davon ausgegangen, dass Sie das CommandBar-Steuerelement verwenden.

## Beispiele
Eine erweiterte Befehlsleiste in der Microsoft Fotos-App:

![Befehlsleiste in der Microsoft Fotos-App](images/control-examples/command-bar-photos.png)

Eine Befehlsleiste im Outlook-Kalender auf einem Windows Phone:

![Befehlsleiste in der Outlook-Kalender-App](images/control-examples/command-bar-calendar-phone.png)

## Aufbau

Standardmäßig werden auf der Befehlsleiste eine Reihe von Symbolschaltflächen und eine optionale, durch drei Punkte (\[•••\]) dargestellte Schaltfläche für weitere Optionen angezeigt. Dies ist die Befehlsleiste, die mit dem weiter unten gezeigten Beispielcode erstellt wird. Hier ist der geschlossene, kompakte Zustand zu sehen.

![Geschlossene Befehlsleiste](images/command-bar-compact.png)

Die Befehlsleiste kann auch im geschlossenen, minimierten Zustand angezeigt werden, die wie folgt aussieht. Weitere Informationen finden Sie im Abschnitt [Geöffneter und geschlossener Zustand](#open-and-closed-states).

![Geschlossene Befehlsleiste](images/command-bar-minimal.png)

Dies ist die gleiche Befehlsleiste im geöffneten Zustand. Die Beschriftungen zeigen die wichtigsten Elemente des Steuerelements.

![Geschlossene Befehlsleiste](images/commandbar_anatomy_open.png)

Die Befehlsleiste ist in 4 Hauptbereiche unterteilt:
- Die Schaltfläche für weitere Optionen (\[•••\]) wird rechts auf der Leiste angezeigt. Das Auswählen der Schaltfläche für weitere Optionen (\[•••\]) hat 2 Folgen: Die Beschriftungen auf den primären Befehlsschaltflächen werden eingeblendet, und das Überlaufmenü wird geöffnet, wenn sekundäre Befehle vorhanden sind. Im neuesten SDK wird die Schaltfläche nicht angezeigt, wenn keine sekundären Befehle oder ausgeblendeten Beschriftungen vorhanden sind. [
              **OverflowButtonVisibility**
            ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.overflowbuttonvisibility.aspx) ermöglicht Apps, das Standardverhalten „Automatisch im Hintergrund“ zu ändern
- Der Inhaltsbereich wird an der linken Seite der Leiste ausgerichtet. Er wird angezeigt, wenn die [**Content**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx)-Eigenschaft gefüllt wird.
- Der Bereich für primäre Befehle wird an der rechten Seite der Leiste ausgerichtet, neben der Schaltfläche für weitere Optionen (\[•••\]). Er wird angezeigt, wenn die [**PrimaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx)-Eigenschaft gefüllt wird.  
- Das Überlaufmenü wird nur angezeigt, wenn die Befehlsleiste geöffnet ist und die [**SecondaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx)-Eigenschaft gefüllt ist. Das neue dynamische Überlaufverhalten verschiebt primäre Befehle automatisch in den SecondaryCommands-Bereich, wenn der Platz begrenzt ist.

Das Layout wird umgekehrt, wenn [FlowDirection](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.flowdirection.aspx) auf **RightToLeft** festgelegt wurde.

## Erstellen einer Befehlsleiste
Mit folgendem Beispiel wird die oben gezeigte Befehlsleiste erstellt.

```xaml
<CommandBar>
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle" Click="AppBarButton_Click" />
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat" Click="AppBarButton_Click"/>
    <AppBarSeparator/>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Forward" Label="Forward" Click="AppBarButton_Click"/>

    <CommandBar.SecondaryCommands>
        <AppBarButton Icon="Like" Label="Like" Click="AppBarButton_Click"/>
        <AppBarButton Icon="Dislike" Label="Dislike" Click="AppBarButton_Click"/>
    </CommandBar.SecondaryCommands>

    <CommandBar.Content>
        <TextBlock Text="Now playing..." Margin="12,14"/>
    </CommandBar.Content>
</CommandBar>
```

## Befehle und Inhalt
Das CommandBar-Steuerelement verfügt über drei Eigenschaften, mit denen Sie Befehle und Inhalte hinzufügen können: [**PrimaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx), [**SecondaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx) und [**Content**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx).


### Primäre Aktionen und Überlauf

Standardmäßig werden Elemente, die Sie der Befehlsleiste hinzufügen, der **PrimaryCommands**-Sammlung hinzugefügt. Diese Befehle werden links von der Schaltfläche für weitere Optionen (\[•••\]) im sogenannten Aktionsbereich angezeigt. Platzieren Sie die wichtigsten Befehle (also die, die auf der Leiste sichtbar bleiben sollen) im Aktionsbereich. Auf sehr kleinen Bildschirmen (Breite: 320 epx) passen maximal vier Elemente in den Aktionsbereich der Befehlsleiste.

Sie können der **SecondaryCommands**-Sammlung auch Befehle hinzufügen. Diese Elemente werden im Überlaufbereich angezeigt. Platzieren Sie weniger wichtige Befehle im Überlaufbereich.

Der Standardüberlaufbereich ist so konzipiert, dass er von der Leiste abgegrenzt ist. Sie können das Format anpassen, indem Sie die [**CommandBarOverflowPresenterStyle**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.commandbaroverflowpresenterstyle.aspx)-Eigenschaft auf einen [Style](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.aspx)-Wert für [**CommandBarOverflowPresenter**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbaroverflowpresenter.aspx) festlegen.

Befehle können programmgesteuert nach Bedarf zwischen „PrimaryCommands“ und „SecondaryCommands“ verschoben werden. {{> interner Inhalt = "Befehle können auch automatisch in den oder aus dem Überlauf verschoben werden, wenn die Breite der Befehlsleiste geändert wird, beispielsweise wenn Benutzer die Größe des App-Fensters ändern. Der dynamische Überlauf ist standardmäßig aktiviert, Apps können dieses Verhalten jedoch deaktivieren, indem der Wert der `IsDynamicOverflowEnabled`-Eigenschaft geändert wird."}}

### App-Leistenschaltflächen

„PrimaryCommands“ und „SecondaryCommands“ können nur mit Befehlselementen vom Typ [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx), [**AppBarToggleButton**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx) und [**AppBarSeparator**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) gefüllt werden. Diese Steuerelemente sind für die Verwendung in Befehlsleisten optimiert, und ihr Erscheinungsbild verändert sich abhängig davon, ob das Steuerelement im Aktionsbereich oder im Überlaufbereich verwendet wird.

Die Steuerelemente für die App-Leistenschaltfläche zeichnen sich durch ein Symbol und eine zugeordnete Beschriftung aus. Es gibt zwei Größen: normal und kompakt. Standardmäßig wird die Beschriftung angezeigt. Wenn die [**IsCompact**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.iscompact.aspx)-Eigenschaft auf **true** festgelegt wird, wird die Beschriftung ausgeblendet. Bei Verwendung in einem CommandBar-Steuerelement überschreibt die Befehlsleiste automatisch die IsCompact-Eigenschaft der Schaltfläche, wenn die Befehlsleiste geöffnet und geschlossen wird.

Damit Beschriftungen von App-Leistenschaltflächen rechts neben den entsprechenden Symbolen angezeigt werden, können Apps die neue [**DefaultLabelPosition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.defaultlabelposition.aspx)-Eigenschaft von CommandBar nutzen.

```xaml
<CommandBar DefaultLabelPosition="Right">
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle"/>
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat"/>
</CommandBar>
```

Hier sehen Sie, wie der oben gezeigte Codeausschnitt aussieht, wenn er von einer App gezeichnet wird.

![Befehlsleiste mit Beschriftungen auf der rechten Seite](images/app-bar-labels-on-right.png)

Bei einzelnen App-Leistenschaltflächen kann die Position der Beschriftung nicht geändert werden. Dies muss über die Befehlsleiste erfolgen. App-Leistenschaltflächen können angeben, dass ihre Beschriftungen nie angezeigt werden, indem die neue [**LabelPosition**](https://msdn.microsoft.com/library/windows/apps/mt710920.aspx)-Eigenschaft auf **Collapsed** festgelegt wird. Wir empfehlen die Verwendung dieser Einstellung auf universell erkennbare Symbole zu begrenzen, z. B. „+“.

Wenn Sie eine App-Leistenschaltfläche im Überlaufmenü (SecondaryCommands) platzieren, wird nur Text angezeigt. Die **LabelPosition** der App-Leistenschaltflächen im Überlauf wird ignoriert. Dies ist der gleiche Schalter in der App-Leistenschaltfläche im Aktionsbereich als primärer Befehl (oben) und im Überlaufbereich als sekundärer Befehl (unten).

![App-Leistenschaltfläche als primärer und sekundärer Befehl](images/app-bar-toggle-button-two-modes.png)

- *Ein Befehl, der für mehrere Seiten einheitlich angezeigt wird, sollte immer an der gleichen Stelle platziert werden.*
- *Es wird empfohlen, die Befehle „Akzeptieren“, „Ja“ und „OK“ links von den Befehlen „Ablehnen“, „Nein“ und „Abbrechen“ zu platzieren. Konsistenz trägt dazu bei, dass Benutzern die App vertraut ist und sie ihre App-Navigationskenntnisse von einer App auf andere Apps übertragen können.*

### Schaltflächenbeschriftungen

Bei App-Leistenschaltflächen empfiehlt sich die Verwendung kurzer Beschriftungen (vorzugsweise ein einzelnes Wort). Wenn längere Beschriftungen unter dem Symbol einer App-Leistenschaltfläche platziert werden, müssen diese auf mehrere Zeilen umgebrochen werden, wodurch die geöffnete Befehlsleiste insgesamt höher wird. Sie können im Text für eine Beschriftung einen bedingten Trennstrich (0x00AD) einfügen, um die Stelle anzugeben, an der der Zeilenumbruch erfolgen soll. In XAML können Sie dies wie folgt mit einer Escapesequenz ausdrücken:

```xaml
<AppBarButton Icon="Back" Label="Areally&#x00AD;longlabel"/>
```

Wenn die Beschriftung an der angegebenen Stelle umgebrochen wird, sieht dies wie folgt aus:

![App-Leistenschaltfläche mit umgebrochener Beschriftung](images/app-bar-button-label-wrap.png)

### Andere Inhalte

Sie können dem Inhaltsbereich beliebige XAML-Elemente hinzufügen, indem Sie die **Content**-Eigenschaft festlegen. Wenn Sie mehrere Elemente hinzufügen möchten, müssen Sie sie in einem Panel-Container platzieren und das Panel als einziges untergeordnetes Element der Content-Eigenschaft festlegen.

Wenn primäre Befehle und Inhalte vorhanden sind, haben die primären Befehle Vorrang. Dies kann dazu führen, dass der Inhalt abgeschnitten wird. {{> Interner Inhalt = "Inhalt wird nicht abgeschnitten, wenn der dynamische Überlauf aktiviert ist, da die primären Befehle in das Überlaufmenü verschoben würden, um Platz für Inhalte zu schaffen."}}

Wenn [**ClosedDisplayMode**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closeddisplaymode.aspx) auf **Compact** festgelegt wurde, kann der Inhalt abgeschnitten werden, wenn er größer als die kompakte Größe der Befehlsleiste ist. Behandeln Sie das [**Opening**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opening.aspx)-Ereignis und das [**Closed**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closed.aspx)-Ereignis, um Teile der Benutzeroberfläche im Inhaltsbereich anzuzeigen oder auszublenden, damit sie nicht beschnitten werden. Weitere Informationen finden Sie im Abschnitt [Geöffneter und geschlossener Zustand](#open-and-closed-states).

## Geöffneter und geschlossener Zustand

Die Befehlsleiste kann geöffnet oder geschlossen sein. Der Benutzer kann mit der Schaltfläche für weitere Optionen (\[•••\]) zwischen diesen Zuständen wechseln. Sie können programmgesteuert zwischen den Zuständen wechseln, indem Sie die [**IsOpen**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.isopen.aspx)-Eigenschaft festlegen. Im geöffneten Zustand werden die primären Befehlsschaltflächen mit Beschriftungen angezeigt, und das Überlaufmenü ist geöffnet, wenn sekundäre Befehle vorhanden sind, wie oben dargestellt.

Sie können mit den Ereignissen [**Opening**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opening.aspx), [**Opened**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opened.aspx), [**Closing**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closing.aspx) und [**Closed**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closed.aspx) auf das Öffnen und Schließen der Befehlsleiste reagieren.  
- Das Opening-Ereignis und das Closing-Ereignis treten vor Beginn der Übergangsanimation ein.
- Das Opened-Ereignis und das Closed-Ereignis treten nach Abschluss des Übergangs ein.

In diesem Beispiel wird mit dem Opening-Ereignis und dem Closing-Ereignis die Deckkraft der Befehlsleiste geändert. Im geschlossenen Zustand ist die Befehlsleiste halbtransparent, sodass der Hintergrund der App durchscheint. Wenn die Befehlsleiste geöffnet wird, wird sie undurchsichtig, damit der Benutzer sich auf die Befehle konzentrieren kann.

```xaml
<CommandBar Opening="CommandBar_Opening"
            Closing="CommandBar_Closing">
    <AppBarButton Icon="Accept" Label="Accept"/>
    <AppBarButton Icon="Edit" Label="Edit"/>
    <AppBarButton Icon="Save" Label="Save"/>
    <AppBarButton Icon="Cancel" Label="Cancel"/>
</CommandBar>
```

```csharp
private void CommandBar_Opening(object sender, object e)
{
    CommandBar cb = sender as CommandBar;
    if (cb != null) cb.Background.Opacity = 1.0;
}

private void CommandBar_Closing(object sender, object e)
{
    CommandBar cb = sender as CommandBar;
    if (cb != null) cb.Background.Opacity = 0.5;
}

```

### ClosedDisplayMode

Sie können steuern, wie die Befehlsleiste im geschlossenen Zustand angezeigt wird, indem Sie die [**ClosedDisplayMode**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closeddisplaymode.aspx)-Eigenschaft festlegen. Sie können aus drei Anzeigemodi für den geschlossenen Zustand auswählen:
- **Kompakt**: Standardmodus. Hiermit werden der Inhalt, primäre Befehlssymbole ohne Beschriftungen und die Schaltfläche für weitere Optionen (\[•••\]) angezeigt.
- **Minimal**: Hiermit wird nur eine dünne Leiste angezeigt, die als Schaltfläche für weitere Optionen (\[•••\]) fungiert. Der Benutzer kann auf eine beliebige Stelle auf der Leiste tippen, um sie zu öffnen.
- **Ausgeblendet**: Die Befehlsleiste wird nicht angezeigt, wenn sie geschlossen ist. Dies kann hilfreich beim Anzeigen von Kontextbefehlen mit einer Inlinebefehlsleiste sein. In diesem Fall müssen Sie die Befehlsleiste programmgesteuert öffnen, indem Sie die **IsOpen**-Eigenschaft festlegen oder ClosedDisplayMode auf **Minimal** oder **Compact** festlegen.

Hier enthält eine Befehlsleiste einfache Formatierungsbefehle für eine [RichEditBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx). Wenn das Bearbeitungsfeld nicht den Fokus besitzt, können die Formatierungsbefehle störend sein, daher werden sie ausgeblendet. Wenn das Bearbeitungsfeld verwendet wird, wird ClosedDisplayMode für die Befehlsleiste in „Compact“ geändert, sodass die Formatierungsbefehle angezeigt werden.

```xaml
<StackPanel Width="300"
            GotFocus="EditStackPanel_GotFocus"
            LostFocus="EditStackPanel_LostFocus">
    <CommandBar x:Name="FormattingCommandBar" ClosedDisplayMode="Hidden">
        <AppBarButton Icon="Bold" Label="Bold" ToolTipService.ToolTip="Bold"/>
        <AppBarButton Icon="Italic" Label="Italic" ToolTipService.ToolTip="Italic"/>
        <AppBarButton Icon="Underline" Label="Underline" ToolTipService.ToolTip="Underline"/>
    </CommandBar>
    <RichEditBox Height="200"/>
</StackPanel>
```

```csharp
private void EditStackPanel_GotFocus(object sender, RoutedEventArgs e)
{
    FormattingCommandBar.ClosedDisplayMode = AppBarClosedDisplayMode.Compact;
}

private void EditStackPanel_LostFocus(object sender, RoutedEventArgs e)
{
    FormattingCommandBar.ClosedDisplayMode = AppBarClosedDisplayMode.Hidden;
}
```

>**Hinweis**
            &nbsp;&nbsp;Die Implementierung von Bearbeitungsbefehlen sprengt den Umfang dieses Beispiels. Weitere Informationen finden Sie im Artikel zu [RichEditBox](rich-edit-box.md).

Auch wenn die Modi „Minimal“ und „Hidden“ in einigen Situationen nützlich sind, beachten Sie, dass es für die Benutzer verwirrend sein kann, wenn alle Aktionen ausgeblendet werden.

Das Ändern von ClosedDisplayMode, um mehr oder weniger Informationen für die Benutzer bereitzustellen, hat Auswirkungen auf das Layout der umgebenden Elemente. Das Wechseln zwischen dem geschlossenen und geöffneten Zustand von CommandBar hat hingegen keine Auswirkungen auf das Layout von anderen Elementen.

### IsSticky

Wenn der Benutzer nach dem Öffnen der Befehlsleiste außerhalb des Steuerelements mit der App interagiert, wird standardmäßig das Überlaufmenü geschlossen, und die Beschriftungen werden ausgeblendet. Das Schließen auf diese Weise wird als *einfaches Ausblenden* bezeichnet. Sie können steuern, wie die Leiste geschlossen wird, indem Sie die [**IsSticky**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.issticky.aspx)-Eigenschaft festlegen. Wenn die Leiste eingerastet ist (`IsSticky="true"`), wird sie durch eine Geste zum einfachen Ausblenden nicht geschlossen. Die Leiste bleibt geöffnet, bis der Benutzer auf die Schaltfläche für weitere Optionen (([•••\]) klickt oder ein Menüelement im Überlaufmenü auswählt. Es wird empfohlen, eingerastete Befehlsleisten zu vermeiden, da sie nicht den Benutzererwartungen an das einfache Ausblenden entsprechen.

## Empfohlene und nicht empfohlene Vorgehensweisen

### Platzierung

Befehlsleisten können am oberen Rand des App-Fensters, am unteren Rand des App-Fensters und inline platziert werden.

![Beispiel 1 für die Platzierung der App-Leiste](images/AppbarGuidelines_Placement1.png)

-   Bei kleinen Handheld-Geräten empfiehlt es sich, Befehlsleisten am unteren Bildschirmrand zu platzieren, da sie dort besser erreichbar sind.
-   Wenn Sie bei Geräten mit größerem Bildschirm nur eine Befehlsleiste verwenden, wird empfohlen, sie in der Nähe des oberen Fensterrands zu platzieren.
Die Größe des physischen Bildschirms können Sie mithilfe der [**DiagonalSizeInInches**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.display.displayinformation.diagonalsizeininches.aspx)-API ermitteln.

Befehlsleisten können auf Bildschirmen mit einzelner Ansicht (linkes Beispiel) und auf Bildschirmen mit mehreren Ansichten (rechtes Beispiel) in folgenden Bildschirmbereichen platziert werden: Inlinebefehlsleisten können überall im Aktionsbereich platziert werden.

![Beispiel 2 für die Platzierung der App-Leiste](images/AppbarGuidelines_Placement2.png)

>**Fingereingabegeräte**: Wenn die Befehlsleiste für den Benutzer sichtbar bleiben muss, wenn die Bildschirmtastatur (oder ein Soft Input Panel, SIP) angezeigt wird, können Sie die Befehlsleiste der [BottomAppBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.bottomappbar.aspx)-Eigenschaft einer Seite zuordnen. Sie wird dann verschoben und bleibt sichtbar, wenn die Bildschirmtastatur eingeblendet ist. Andernfalls müssen Sie die Befehlsleiste inline relativ zum App-Inhalt platzieren.

### Aktionen

Priorisieren Sie die auf der Befehlsleiste angezeigten Aktionen auf der Grundlage ihrer Sichtbarkeit.

-   Platzieren Sie die wichtigsten Befehle, also die, die auf der Leiste sichtbar bleiben sollen, an erster Stelle im Aktionsbereich. Auf sehr kleinen Bildschirmen (Breite: 320 Epx) passen abhängig von anderen UI-Elementen 2 bis 4 Elemente in den Aktionsbereich der Befehlsleiste.
-   Platzieren Sie weniger wichtige Befehle weiter hinten im Aktionsbereich der Leiste oder an erster Stelle im Überlaufbereich. Diese Befehle werden angezeigt, wenn auf der Leiste ausreichend Platz verfügbar ist. Sie werden jedoch in das Dropdownmenü des Überlaufbereichs verschoben, wenn nicht ausreichend Platz zur Verfügung steht.
-   Platzieren Sie die am wenigsten wichtigen Befehle im Überlaufbereich. Diese Befehle werden immer im Dropdownmenü angezeigt.

Ein Befehl, der einheitlich über mehrere Seiten hinweg angezeigt wird, muss immer an der gleichen Stelle platziert werden. Es wird empfohlen, die Befehle „Akzeptieren“, „Ja“ und „OK“ links von den Befehlen „Ablehnen“, „Nein“ und „Abbrechen“ zu platzieren. Konsistenz trägt dazu bei, dass Benutzern die App vertraut ist und sie ihre Kenntnisse der App-Navigation von einer App auf andere Apps übertragen können.

Sie können zwar alle Aktionen im Überlaufbereich platzieren, sodass auf der Befehlsleiste lediglich die Schaltfläche für weitere Optionen (\[•••\]) sichtbar ist, beachten Sie dabei jedoch, dass das Ausblenden aller Aktionen für den Benutzer verwirrend sein kann.

### Flyouts auf einer Befehlsleiste

Ziehen Sie logische Gruppierungen für die Befehle in Erwägung. Platzieren Sie z. B. die Befehle „Antworten“, „Allen antworten“ und „Weiterleiten“ im Menü „Antworten“. Mit einer App-Leistenschaltfläche wird zwar üblicherweise ein einzelner Befehl aktiviert, sie kann jedoch auch zum Anzeigen eines [**MenuFlyout**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.menuflyout.aspx)- oder [**Flyout**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flyout.aspx)-Elements mit benutzerdefiniertem Inhalt verwendet werden.

![Beispiel für Flyouts auf einer Befehlsleiste](images/AppbarGuidelines_Flyouts.png)

### Überlaufmenü

![Beispiel für die Befehlsleiste mit Bereich für weitere Optionen](images/AppbarGuidelines_Illustration.png)

-   Das Überlaufmenü wird durch die Schaltfläche für weitere Optionen (\[•••\]) dargestellt. Diese ist der sichtbare Einstiegspunkt für das Menü. Er befindet sich ganz rechts auf der Symbolleiste neben den primären Aktionen.
-   Der Platz für den Überlaufbereich wird für Aktionen verwendet, die nicht so häufig verwendet werden.
-   Aktionen können an Haltepunkten in den Bereich für primäre Aktionen und das Überlaufmenü aufgenommen bzw. daraus entfernt werden. Sie können auch festlegen, dass Aktionen unabhängig von der Größe des Bildschirms oder des App-Fensters immer im primären Aktionsbereich verbleiben.
-   Selten verwendete Aktionen können im Überlaufmenü bleiben, selbst wenn die App-Leiste auf größeren Bildschirmen erweitert wird.

## Anpassungsfähigkeit

-   Im Hoch- und im Querformat sollte die gleiche Anzahl von Aktionen auf der App-Leiste sichtbar sein. Dadurch wird der Benutzer kognitiv entlastet. Die Anzahl von verfügbaren Aktionen muss durch die Breite des Geräts im Hochformat bestimmt werden.
-   Auf kleinen Bildschirmen, die meistens einhändig bedient werden, müssen die App-Leisten im unteren Bildschirmbereich positioniert werden.
-   Auf größeren Bildschirmen hat es sich bewährt, App-Leisten am oberen Fensterrand zu platzieren.
-   Mithilfe von Haltepunkten können Sie bei einer Änderung der Fenstergröße Aktionen in das Menü bzw. aus dem Menü verschieben.
-   Mithilfe der Bildschirmdiagonale können Sie die Position der App-Leiste auf der Grundlage der Bildschirmgröße ändern.
-   Zur Verbesserung der Lesbarkeit können Sie Beschriftungen rechts neben die Symbole der App-Leistenschaltflächen verschieben. Bei Beschriftungen, die sich unten befinden, müssen Benutzer die Befehlsleiste öffnen, um die Beschriftungen anzuzeigen. Beschriftungen auf der rechten Seite werden auch angezeigt, wenn die Befehlsleiste geschlossen ist. Diese Optimierung funktioniert gut bei größeren Fenstern.

## Verwandte Artikel

**Für Designer**
            
          
            [Befehlsdesigngrundlagen für UWP-Apps](../layout/commanding-basics.md)

**Für Entwickler (XAML)**
            
          
            [
              **CommandBar**
            ](https://msdn.microsoft.com/library/windows/apps/dn279427)



<!--HONumber=Jun16_HO4-->


