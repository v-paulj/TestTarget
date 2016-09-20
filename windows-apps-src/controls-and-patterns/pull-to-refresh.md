---
author: Jwmsft
Description: "Verwenden Sie das Muster „Aktualisierung durch Ziehen“ mit einer Listenansicht."
title: Aktualisierung durch Ziehen
label: Pull-to-refresh
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 508a09e0c12006c00dbdf7675516b41119eab8a6
ms.openlocfilehash: ef5773f9885a5286ac7ca7c256e6a83167316389

---
# Aktualisierung durch Ziehen

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Dank des Musters „Aktualisierung durch Ziehen“ können Benutzer aktuelle Daten in einer Liste durch das Ausführen einer Ziehbewegung von oben nach unten auf der Liste abrufen. Die Aktualisierung durch Ziehen wird häufig in mobilen Apps verwendet, eignet sich jedoch für alle Geräte mit Touchscreen. Durch die Behandlung von [Manipulationsereignissen](../input-and-devices/touch-interactions.md#manipulation-events) können Sie die Aktualisierung durch Ziehen in eine App implementieren.

Im [Beispiel für die Aktualisierung durch Ziehen](http://go.microsoft.com/fwlink/p/?LinkId=620635) wird die Erweiterung eines [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)-Steuerelements zur Unterstützung dieses Musters dargestellt. In diesem Artikel werden mithilfe dieses Beispiels die wichtigsten Aspekte der Implementierung des „Aktualisierung durch Ziehen“-Musters erläutert.

![Beispiel für die Aktualisierung durch Ziehen](images/ptr-phone-1.png)

## Für welche Szenarien eignet sich dieses Muster?

Das „Aktualisierung durch Ziehen“-Muster eignet sich für Datenlisten oder -raster, die vom Benutzer regelmäßig aktualisiert werden, vor allem, wenn die App hauptsächlich auf mobilen Geräten mit Touchscreen ausgeführt werden soll.

## Implementieren von Aktualisierung durch Ziehen

Für die Implementierung der Aktualisierung durch Ziehen ist die Behandlung von Manipulationsereignissen erforderlich, um zu ermitteln, wann der Benutzer die Liste nach unten zieht, visuelles Feedback bereitzustellen und die Daten zu aktualisieren. Wie dies funktioniert, wird in diesem [Beispiel für die Aktualisierung durch Ziehen](http://go.microsoft.com/fwlink/p/?LinkId=620635) gezeigt. Für eine vollständige Darstellung des Codes laden Sie das Beispiel herunter, oder zeigen Sie den Code auf GitHub an.

Im Beispiel für die Aktualisierung durch Ziehen wird ein benutzerdefiniertes Steuerelement namens `RefreshableListView` als Erweiterung des **ListView**-Steuerelements erstellt. Dieses Steuerelement behandelt die Manipulationsereignisse in der internen Bildlaufanzeige der Listenansicht und liefert visuelles Feedback mithilfe einer Aktualisierungsanzeige. Durch zwei zusätzliche Ereignisse werden Sie zudem informiert, wenn die Liste gezogen wird und die Daten aktualisiert werden müssen. RefreshableListView bietet lediglich die Benachrichtigung, dass Daten zu aktualisieren sind. Die Aktualisierung der Daten erfordert eine Behandlung des Ereignisses in Ihrer App. Der entsprechende Code hierfür unterscheidet sich je nach App.

RefreshableListView verfügt über einen automatischen Aktualisierungsmodus, mit dem der Zeitpunkt der Aktualisierungsanforderung und die Ausblendung der Aktualisierungsanzeige bestimmt werden können. Die automatische Aktualisierung kann aktiviert oder deaktiviert werden.
- Deaktiviert: Eine Aktualisierung wird nur dann angefordert, wenn die Liste bei Überschreitung von `PullThreshold` losgelassen wird. Wenn der Benutzer den Scroller loslässt, wird die Anzeige ausgeblendet. Die Statusleisten-Anzeige wird bei Verfügbarkeit angezeigt (gilt für Smartphone).
- Aktiviert: Die Aktualisierung wird, unabhängig vom Loslassen, bei Überschreitung von `PullThreshold` angefordert. Die Anzeige wird erst nach abgeschlossenem Abruf der Daten ausgeblendet. Zur Benachrichtigung der App über den abgeschlossenen Abruf der Daten wird eine **Verzögerung** eingesetzt.

> **Hinweis**&nbsp;&nbsp;Der im Beispiel dargestellte Code lässt sich auch auf [**GridView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx) anwenden. Zum Ändern von GridView leiten Sie die benutzerdefinierte Klasse von GridView anstelle von ListView ab und nehmen Änderungen an der GridView-Standardvorlage vor.

## Hinzufügen einer Aktualisierungsanzeige

Es ist wichtig, den Benutzern durch visuelles Feedback mitzuteilen, dass Ihre App die Aktualisierung durch Ziehen unterstützt. RefreshableListView verfügt über eine `RefreshIndicatorContent` -Eigenschaft, mit der Sie die Ansicht der Anzeige in Ihrer XAML festlegen können. RefreshableListView beinhaltet auch eine Standardtextanzeige für den Fall, dass Sie die `RefreshIndicatorContent`-Eigenschaft nicht festlegen.

Im Folgenden finden Sie empfohlene Richtlinien für die Aktualisierungsanzeige.

![Rote Markierungen der Aktualisierungsanzeige](images/ptr-redlines-1.png)

**Ändern der Vorlage für die Listenansicht**

Im Beispiel für die Aktualisierung durch Ziehen erweitert die `RefreshableListView`-Steuerelementvorlage die standardmäßige **ListView**-Vorlage um eine Aktualisierungsanzeige. Die Aktualisierungsanzeige befindet sich in einem [**Raster**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.grid.aspx) oberhalb des [**ItemsPresenter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemspresenter.aspx), der die Listenelemente anzeigt.

> **Hinweis**&nbsp;&nbsp;Das `DefaultRefreshIndicatorContent`-Textfeld stellt eine Standardtextanzeige bereit, die nur bei fehlender Festlegung der `RefreshIndicatorContent` -Eigenschaft angezeigt wird.

Im Folgenden finden Sie den Teil der Steuerelementvorlage, die auf Grundlage der ListView-Standardvorlage erstellt wird:

**XAML**
```xaml
<!-- Styles/Styles.xaml -->
<Grid x:Name="ScrollerContent" VerticalAlignment="Top">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <Border x:Name="RefreshIndicator" VerticalAlignment="Top" Grid.Row="1">
        <Grid>
            <TextBlock x:Name="DefaultRefreshIndicatorContent" HorizontalAlignment="Center" 
                       Foreground="White" FontSize="20" Margin="20, 35, 20, 20"/>
            <ContentPresenter Content="{TemplateBinding RefreshIndicatorContent}"></ContentPresenter>
        </Grid>
    </Border>
    <ItemsPresenter FooterTransitions="{TemplateBinding FooterTransitions}" 
                    FooterTemplate="{TemplateBinding FooterTemplate}" 
                    Footer="{TemplateBinding Footer}" 
                    HeaderTemplate="{TemplateBinding HeaderTemplate}" 
                    Header="{TemplateBinding Header}" 
                    HeaderTransitions="{TemplateBinding HeaderTransitions}" 
                    Padding="{TemplateBinding Padding}"
                    Grid.Row="1"
                    x:Name="ItemsPresenter"/>
</Grid>
```

**Inhalt in XAML festlegen**

Der Inhalt der Aktualisierungsanzeige wird in der XAML für Ihre Listenansicht festgelegt. Die von Ihnen festgelegten XAML-Inhalte werden über den [ContentPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentpresenter.aspx) (`<ContentPresenter Content="{TemplateBinding RefreshIndicatorContent}">`) der Aktualisierungsanzeige angezeigt. Wenn Sie diese Inhalte nicht festlegen, wird stattdessen die Standardtextanzeige eingeblendet.

**XAML**
```xaml
<!-- MainPage.xaml -->
<c:RefreshableListView
    <!-- ... See sample for removed code. -->
    AutoRefresh="{x:Bind Path=UseAutoRefresh, Mode=OneWay}"
    ItemsSource="{x:Bind Items}"
    PullProgressChanged="listView_PullProgressChanged"
    RefreshRequested="listView_RefreshRequested">

    <c:RefreshableListView.RefreshIndicatorContent>
        <Grid Height="100" Background="Transparent">
            <FontIcon
                Margin="0,0,0,30"
                HorizontalAlignment="Center"
                VerticalAlignment="Bottom"
                FontFamily="Segoe MDL2 Assets"
                FontSize="20"
                Glyph="&#xE72C;"
                RenderTransformOrigin="0.5,0.5">
                <FontIcon.RenderTransform>
                    <RotateTransform x:Name="SpinnerTransform" Angle="0" />
                </FontIcon.RenderTransform>
            </FontIcon>
        </Grid>
    </c:RefreshableListView.RefreshIndicatorContent>
    
    <!-- ... See sample for removed code. -->

</c:RefreshableListView>
```

**Animieren des Drehfelds**

Wird die Liste nach unten gezogen, wird das `PullProgressChanged` -Ereignis von RefreshableListView ausgelöst. Sie können dieses Ereignis in Ihrer App behandeln, um die Aktualisierungsanzeige zu steuern. Im Beispiel wird durch dieses Storyboard das [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.rotatetransform.aspx) -Element animiert und die Aktualisierungsanzeige gedreht. 

**XAML**
```xaml
<!-- MainPage.xaml -->
<Storyboard x:Name="SpinnerStoryboard">
    <DoubleAnimation
        Duration="00:00:00.5"
        FillBehavior="HoldEnd"
        From="0"
        RepeatBehavior="Forever"
        Storyboard.TargetName="SpinnerTransform"
        Storyboard.TargetProperty="Angle"
        To="360" />
</Storyboard>
```

## Behandeln von Manipulationsereignissen der Bildlaufanzeige

Die Steuerelementvorlage der Listenansicht enthält einen integrierten [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx), mit dem der Benutzer durch die Listenelemente scrollen kann. Zum Implementieren der Aktualisierung durch Ziehen müssen sowohl die Manipulationsereignisse in der integrierten Bildlaufanzeige als auch mehrere verwandte Ereignisse behandelt werden. Weitere Informationen zu Manipulationsereignissen finden Sie unter [Toucheingabe-Interaktionen](../input-and-devices/touch-interactions.md).

** OnApplyTemplate**

Um auf die Bildlaufanzeige und andere Bereiche der Vorlage zuzugreifen sowie Ereignishandler hinzufügen und später im Code aufrufen zu können, müssen Sie die [**OnApplyTemplate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.onapplytemplate.aspx)-Methode überschreiben. In OnApplyTemplate rufen Sie [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.gettemplatechild.aspx) auf, um einen Verweis auf einen benannten Teil der Steuerelementvorlage zu erhalten, den Sie speichern und später im Code verwenden können.

Im Beispiel werden die zum Speichern der Vorlagenteile verwendeten Variablen im Bereich „Private Variables“ deklariert. Nachdem sie in der OnApplyTemplate-Methode abgerufen wurden, werden Handler für die Ereignisse [**DirectManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.directmanipulationstarted.aspx), [**DirectManipulationCompleted**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.directmanipulationcompleted.aspx), [**ViewChanged**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.viewchanged.aspx), und [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx) hinzugefügt.

**DirectManipulationStarted**

Zum Initiieren einer „Aktualisierung durch Ziehen“-Aktion muss der Inhalt an das obere Ende der Bildlaufanzeige gescrollt sein, wenn der Benutzer den Finger auf dem Bildschirm nach unten zieht. Andernfalls wird davon ausgegangen, dass der Benutzer über den Touchscreen auf der Liste nach oben scrollt. Der Code in diesem Handler bestimmt, ob die Manipulation am oberen Rand der Bildlaufanzeige beginnt, und kann die Aktualisierung der Liste auslösen. Der Status der Aktualisierbarkeit des Steuerelements wird entsprechend festgelegt. 

Wenn eine Aktualisierung des Steuerelements möglich ist, werden die Ereignishandler für Animationen ebenfalls hinzugefügt.

**DirectManipulationCompleted**

Sobald der Benutzer das Ziehen der Liste nach unten beendet, prüft der Code dieses Handlers, ob während der Manipulation eine Aktualisierung aktiviert wurde. Ist dies der Fall, wird das `RefreshRequested` -Ereignis ausgelöst und der `RefreshCommand` Befehl ausgeführt.

Die Ereignishandler für Animationen werden ebenfalls entfernt.

Basierend auf dem Wert der `AutoRefresh` -Eigenschaft kann die Liste per Animation entweder sofort oder erst nach abgeschlossener Aktualisierung an ihren Anfang zurückgeblättert werden. Zur Kennzeichnung der abgeschlossenen Aktualisierung wird ein [**Verzögerungs**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.aspx)-Objekt verwendet. An diesem Punkt ist die Aktualisierungsanzeige ausgeblendet.

Dieser Teil des „DirectManipulationCompleted“-Ereignishandlers löst das `RefreshRequested`-Ereignis aus und ruft bei Bedarf die Verzögerung ab.

**C#**
```csharp
if (this.RefreshRequested != null)
{
    RefreshRequestedEventArgs refreshRequestedEventArgs = new RefreshRequestedEventArgs(
        this.AutoRefresh ? new DeferralCompletedHandler(RefreshCompleted) : null);
    this.RefreshRequested(this, refreshRequestedEventArgs);
    if (this.AutoRefresh)
    {
        m_scrollerContent.ManipulationMode = ManipulationModes.None;
        if (!refreshRequestedEventArgs.WasDeferralRetrieved)
        {
            // The Deferral object was not retrieved in the event handler.
            // Animate the content up right away.
            this.RefreshCompleted();
        }
    }
}
```

**ViewChanged**

Im „ViewChanged“-Ereignishandler werden zwei Fälle behandelt.

Erstens wird im Fall einer durch die Zoombildlaufanzeige geänderten Ansicht der Status der Aktualisierbarkeit des Steuerelements widerrufen.

Zweitens werden, sobald sich der Inhalt am Ende einer automatischen Aktualisierung wieder nach oben bewegt hat, die Rechtecke für den Abstand ausgeblendet, Touchinteraktionen mit der Bildlaufanzeige wieder aktiviert und der [VerticalOffset](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.verticaloffset.aspx) auf 0 festgelegt.

**PointerPressed**

Die Aktualisierung durch Ziehen wird nur dann ausgelöst, wenn die Liste durch eine Touchmanipulation nach unten gezogen wird. Mit dem Code des „PointerPressed“-Ereignishandlers wird überprüft, welche Art von Zeiger das Ereignis ausgelöst hat und eine Variable (`m_pointerPressed`) zur Indikation eines Touchzeigers festgelegt. Diese Variable wird im „DirectManipulationStarted“-Handler verwendet. Wenn der Zeiger kein Touchzeiger ist, wird der „DirectManipulationStarted“-Handler ohne Ausführung einer Aktion zurückgegeben.

## Hinzufügen von „Aktualisierung durch Ziehen“-Ereignissen

„RefreshableListView“ fügt zwei Ereignisse hinzu, die Sie in Ihrer App behandeln können. Mit diesen lassen sich die Daten aktualisieren und die Aktualisierungsanzeige verwalten.

Weitere Informationen zu Ereignissen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](https://msdn.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview).

**RefreshRequested**

Das „RefreshRequested“-Ereignis benachrichtigt Ihre App, wenn der Benutzer die Liste für eine Aktualisierung nach unten zieht. Sie behandeln dieses Ereignis zum Abrufen neuer Daten und zum Aktualisieren der Liste.

Hier sehen Sie den Ereignishandler aus dem Beispiel. Entscheidend ist, dass die `AutoRefresh`-Eigenschaft der Listenansicht überprüft und eine Verzögerung abgerufen wird, wenn die Eigenschaft auf **true** festgelegt ist. Durch eine Verzögerung wird die Aktualisierungsanzeige weiterhin angezeigt, bis die Aktualisierung abgeschlossen ist.

**C#**
```csharp
private async void listView_RefreshRequested(object sender, RefreshableListView.RefreshRequestedEventArgs e)
{
    using (Deferral deferral = listView.AutoRefresh ? e.GetDeferral() : null)
    {
        await FetchAndInsertItemsAsync(_rand.Next(1, 5));

        if (SpinnerStoryboard.GetCurrentState() != Windows.UI.Xaml.Media.Animation.ClockState.Stopped)
        {
            SpinnerStoryboard.Stop();
        }
    }
}
```

**PullProgressChanged**

In diesem Beispiel werden Inhalte für die Aktualisierungsanzeige bereitgestellt und durch die App gesteuert. Das „PullProgressChanged“-Ereignis benachrichtigt Ihre App, wenn der Benutzer die Liste herunterzieht. So kann die Aktualisierungsanzeige gestartet, beendet und zurückgesetzt werden. 

## Kompositionsanimationen

Standardmäßig wird die Scrollbewegung des Inhalts in einer Bildlaufanzeige beim Erreichen des oberen Endes der Scrollleiste beendet. Um dem Benutzer das Herunterziehen der Liste zu ermöglichen, muss auf die visuelle Ebene zugegriffen und der Listeninhalt animiert werden. Im Beispiel werden hierfür [Kompositionsanimationen](https://msdn.microsoft.com/windows/uwp/graphics/composition-animation), genauer [Ausdrucksanimationen](https://msdn.microsoft.com/windows/uwp/graphics/composition-animation#expression-animations), verwendet.

Diese Vorgänge werden im Beispiel hauptsächlich mit dem `CompositionTarget_Rendering` -Ereignishandler und der `UpdateCompositionAnimations` -Methode ausgeführt.

## Verwandte Artikel

- [Formatieren von Steuerelementen](styling-controls.md)
- [Toucheingabe-Interaktionen](../input-and-devices/touch-interactions.md)
- [Listenansicht und Rasteransicht](listview-and-gridview.md)
- [Vorlagen für Listenansichtselemente](listview-item-templates.md)
- [Ausdrucksanimationen](https://msdn.microsoft.com/windows/uwp/graphics/composition-animation#expression-animations)


<!--HONumber=Aug16_HO3-->


