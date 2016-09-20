---
author: Xansky
Description: "Wenn Ihre App keine barrierefreie Bedienung mit der Tastatur ermöglicht, können Benutzer, die blind oder in ihrer Beweglichkeit eingeschränkt sind, Schwierigkeiten bei der Verwendung Ihrer App haben oder Ihre App möglicherweise überhaupt nicht nutzen."
ms.assetid: DDAE8C4B-7907-49FE-9645-F105F8DFAD8B
title: Barrierefreiheit der Tastaturnavigation
label: Keyboard accessibility
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 2215aeae041ff975d99d29ea42350f6ce0f2a681
ms.openlocfilehash: 112c7be4cb5418332a19565be9b739e7032ba834

---

# Barrierefreiheit der Tastaturnavigation  



Wenn Ihre App keine barrierefreie Bedienung mit der Tastatur ermöglicht, können Benutzer, die blind oder in ihrer Beweglichkeit eingeschränkt sind, Schwierigkeiten bei der Verwendung Ihrer App haben oder Ihre App möglicherweise überhaupt nicht nutzen.

<span id="keyboard_navigation_among_UI_elements"/>
<span id="keyboard_navigation_among_ui_elements"/>
<span id="KEYBOARD_NAVIGATION_AMONG_UI_ELEMENTS"/>
## Navigation zwischen UI-Elementen mithilfe der Tastatur  
Um die Tastatur für ein Steuerelement verwenden zu können, muss das Steuerelement den Fokus haben, und damit es den Fokus (ohne Verwendung eines Zeigers) erhalten kann, muss es in einem Benutzeroberflächenentwurf über die TAB-Navigation erreichbar sein. Standardmäßig entspricht die Aktivierreihenfolge von Steuerelementen der Reihenfolge, in der die Steuerelemente der Entwurfsoberfläche hinzugefügt, in XAML aufgelistet oder programmgesteuert einem Container hinzugefügt werden.

In den meisten Fällen ist die auf Ihrer Definition der Steuerelemente in XAML basierende Standardreihenfolge die beste Reihenfolge, insbesondere, weil die Steuerelemente in dieser Reihenfolge auch von Bildschirmleseprogrammen gelesen werden. Die Standardreihenfolge ist jedoch nicht notwendigerweise mit der visuellen Reihenfolge identisch. Die tatsächliche Anzeigeposition kann vom übergeordneten Layoutcontainer und bestimmten Eigenschaften abhängen, die Sie für die untergeordneten Elemente festlegen können, um das Layout zu beeinflussen. Testen Sie dieses Verhalten selbst, um sicherzustellen dass die Aktivierreihenfolge Ihrer App gut ist. Insbesondere bei einer Raster- oder Tabellenmetapher für Ihr Layout lesen die Benutzer am Ende unter Umständen in einer anderen Reihenfolge als der Aktivierreihenfolge. Das ist an sich nicht unbedingt ein Problem. Achten Sie aber darauf, die Funktionen der App sowohl als UI für die Toucheingabe als auch als UI für die Verwendung mit der Tastatur zu testen. Vergewissern Sie sich, dass die UI bei beiden Methoden sinnvoll ist.

Sie können eine der visuellen Reihenfolge entsprechende Aktivierreihenfolge verwenden oder das XAML anpassen. Sie können auch die standardmäßige Aktivierreihenfolge überschreiben, indem Sie die Eigenschaft [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) festlegen. Das folgende Beispiel zeigt dies für ein [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)-Layout mit Tab-Navigation, bei dem zuerst Spalten aktiviert werden.

XAML
```xml
<!--Custom tab order.-->
<Grid>
  <Grid.RowDefinitions>...</Grid.RowDefinitions>
  <Grid.ColumnDefinitions>...</Grid.ColumnDefinitions>

  <TextBlock Grid.Column="1" HorizontalAlignment="Center">Groom</TextBlock>
  <TextBlock Grid.Column="2" HorizontalAlignment="Center">Bride</TextBlock>

  <TextBlock Grid.Row="1">First name</TextBlock>
  <TextBox x:Name="GroomFirstName" Grid.Row="1" Grid.Column="1" TabIndex="1"/>
  <TextBox x:Name="BrideFirstName" Grid.Row="1" Grid.Column="2" TabIndex="3"/>

  <TextBlock Grid.Row="2">Last name</TextBlock>
  <TextBox x:Name="GroomLastName" Grid.Row="2" Grid.Column="1" TabIndex="2"/>
  <TextBox x:Name="BrideLastName" Grid.Row="2" Grid.Column="2" TabIndex="4"/>
</Grid>
```

Möglicherweise möchten Sie ein Steuerelement von der Aktivierreihenfolge ausschließen. In der Regel legen Sie das Steuerelement dazu nur als nicht interaktiv fest, indem Sie z. B. seine [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/BR209419)-Eigenschaft auf **false** setzen. Ein deaktiviertes Steuerelement wird automatisch von der Aktivierreihenfolge ausgeschlossen. Mitunter ist es aber erforderlich, ein nicht deaktiviertes Steuerelement von der Aktivierreihenfolge auszuschließen. In diesem Fall können Sie die [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/BR209422)-Eigenschaft auf **false** festlegen.

Alle Elemente, die im Fokus stehen können, sind normalerweise standardmäßig in der Aktivierreihenfolge enthalten. Eine Ausnahme sind bestimmte Textanzeigetypen wie [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565), die im Fokus stehen können, damit die Zwischenablage zur Textauswahl auf sie zugreifen kann. Diese Elemente sind aber nicht in der Aktivierreihenfolge enthalten, weil dies bei statischen Textelementen nicht zu erwarten ist. Sie sind nicht im herkömmlichen Sinn interaktiv (sie können nicht aufgerufen werden und erfordern keine Texteingabe, unterstützen aber das [Steuerelementmuster für Text](https://msdn.microsoft.com/library/windows/desktop/Ee671194), das das Suchen und Anpassen von Auswahlpunkten in Text unterstützt). Text sollte nicht darauf hindeuten, dass er eine mögliche Aktion auslöst, wenn er den Fokus erhält. Textelemente werden von Hilfstechnologien weiterhin erkannt und von Sprachausgaben laut vorgelesen. Dies basiert jedoch auf anderen Techniken als dem Suchen dieser Elemente in der praktischen Aktivierreihenfolge.

Unabhängig davon, ob Sie [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461)-Werte anpassen oder die Standardreihenfolge verwenden, gelten folgende Regeln:

* UI-Elemente, bei denen [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) gleich0 ist, werden der Aktivierreihenfolge basierend auf der Deklarierungsreihenfolge in XAML-Collections oder untergeordneten Collections hinzugefügt.
* UI-Elemente, bei denen [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) größer als0 ist, werden der Aktivierreihenfolge basierend auf dem Wert **TabIndex** hinzugefügt.
* UI-Elemente, bei denen [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) kleiner als0 ist, werden der Aktivierreihenfolge hinzugefügt und werden vor allen Elementen mit NULL-Werten angezeigt. Dies ist unter Umständen anders als bei der Verarbeitung des Attributs **tabindex** in HTML. (Ein negativer **tabindex** wurde in älteren HTML-Spezifikationen nicht unterstützt.)

<span id="keyboard_navigation_within_a_UI_element"/>
<span id="keyboard_navigation_within_a_ui_element"/>
<span id="KEYBOARD_NAVIGATION_WITHIN_A_UI_ELEMENT"/>
## Navigation innerhalb eines Benutzeroberflächenelements mithilfe der Tastatur  
Bei zusammengesetzten Elementen ist es wichtig, eine korrekte interne Navigation zwischen den enthaltenen Elementen sicherzustellen. Ein zusammengesetztes Element kann seine derzeit aktiven untergeordneten Elemente verwalten, wodurch der Mehraufwand reduziert wird, der entsteht, wenn alle untergeordneten Elemente fokussierbar sein müssen. Ein solches zusammengesetztes Element ist in der Aktivierreihenfolge enthalten und behandelt Tastaturnavigationsereignisse selbst. Viele der zusammengesetzten Steuerelemente verfügen bereits über eine in ihre Ereignisbehandlung integrierte Navigationslogik. Das Durchlaufen von Elementen mit den Pfeiltasten ist z.B. standardmäßig für die Elemente eines [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)-, [**GridView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview)-, [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868)- und [**FlipView**](https://msdn.microsoft.com/library/windows/apps/BR242678)-Steuerelements aktiviert.

<span id="keyboard_activation"/>
<span id="KEYBOARD_ACTIVATION"/>
## Tastaturalternativen für Zeigeraktionen und Ereignisse für bestimmte Steuerelemente  
Stellen Sie sicher, dass UI-Elemente, auf die geklickt werden kann, auch über die Tastatur aufgerufen werden können. Um die Tastatur für ein UI-Element verwenden zu können, muss das Element im Fokus stehen. Nur von [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) abgeleitete Klassen unterstützen den Fokus und die TAB-Navigation.

Implementieren Sie für UI-Elemente, die aufgerufen werden können, Tastatur-Ereignishandler für die LEERTASTE und die EINGABETASTE. So machen Sie die Unterstützung für die Barrierefreiheit des Tastaturzugriffs komplett und ermöglichen es Benutzern, einfache App-Szenarien nur über die Tastatur auszuführen – d.h. Benutzer können über die Tastatur alle interaktiven UI-Elemente erreichen und die Standardfunktionen aktivieren.

Wenn ein Element, das Sie in der UI verwenden möchten, nicht im Fokus stehen kann, können Sie ein eigenes benutzerdefiniertes Steuerelement erstellen. Sie müssen die Eigenschaft [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/BR209422) auf **true** festlegen, damit das Element im Fokus stehen kann, und Sie müssen eine visuelle Anzeige des fokussierten Zustands bereitstellen, indem Sie einen visuellen Zustand erstellen, der für eine Fokusanzeige auf der UI sorgt. Oft ist es allerdings einfacher, die Steuerelementkomposition zu verwenden, damit die Unterstützung für TAB-Navigation, Fokus und Microsoft-Benutzeroberflächenautomatisierungs-Peers und -Muster von dem Steuerelement behandelt werden kann, in dem Sie den Inhalt zusammensetzen.

Anstatt z. B. das PointerPressed-Ereignis für ein [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) zu verarbeiten, können Sie das Element in einem [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)-Objekt umschließen, um die Zeiger-, Tastatur- und Fokusunterstützung bereitzustellen.

XAML
```xml
<!--Don't do this.-->
<Image Source="sample.jpg" PointerPressed="Image_PointerPressed"/>

<!--Do this instead.-->
<Button Click="Button_Click"><Image Source="sample.jpg"/></Button>
```

<span id="keyboard_shortcuts"/>
<span id="KEYBOARD_SHORTCUTS"/>
## Tastenkombinationen  
Zusätzlich zur Navigation und Aktivierung über die Tastatur empfiehlt es sich, Tastenkombinationen für die Funktionen Ihrer App zu implementieren. Die TAB-Navigation ist eine gute, einfache Form der Tastaturunterstützung. Bei komplexeren Formularen kann es aber sinnvoll sein, auch Unterstützung für Tastenkombinationen hinzuzufügen. Hierdurch kann Ihre App effizienter genutzt werden – auch von Menschen, die sowohl Tastatur als auch Zeigegeräte nutzen.

*Tastenkombinationen* sind eine effiziente Methode für den Zugriff auf App-Funktionen und verbessern daher die Produktivität der Benutzer. Es gibt zwei Arten von Tastenkombinationen:

* *Tastenkombinationen mit der ALT-TASTE* ermöglichen den schnellen Zugriff auf UI-Elemente Ihrer App. Hierbei handelt es sich um eine Kombination aus der ALT-TASTE und einer Buchstabentaste.
* *Tastenkombinationen mit der STRG-Taste* ermöglichen den schnellen Zugriff auf App-Befehle. Ihr App kann UI-Elemente aufweisen, die exakt dem Befehl entsprechen. Hierbei handelt es sich um eine Kombination aus der STRG-TASTE und einer Buchstabentaste.

Es ist unbedingt notwendig, dass Benutzer, die auf Bildschirmleseprogramme und andere Hilfstechnologien angewiesen sind, die Tastenkombinationen der App einfach erkennen können. Weisen Sie mithilfe von QuickInfos, Namen und Beschreibungen zur Verwendung durch Bildschirmleseprogramme oder anderen Hinweisen auf dem Bildschirm auf Tastenkombinationen hin. Zumindest sollten die Tastenkombinationen im Hilfeinhalt Ihrer App gut dokumentiert sein.

Sie können Zugriffstasten über Bildschirmleseprogramme dokumentieren, indem Sie die angefügte Eigenschaft [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/Hh759763) auf eine Zeichenfolge festlegen, die die Tastenkombination beschreibt. Darüber hinaus gibt es die angefügte Eigenschaft [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/Hh759762), um nicht-mnemonische Tastenkombinationen zu dokumentieren. Beide Eigenschaften werden von Bildschirmleseprogrammen jedoch normalerweise gleich behandelt. Sie sollten versuchen, Tastenkombinationen auf mehrere Arten zu dokumentieren – mithilfe von QuickInfos, Automatisierungseigenschaften und schriftlicher Hilfedokumentation.

Das folgende Beispiel zeigt das Dokumentieren von Tastenkombinationen für Schaltflächen für die Medienwiedergabe: „Play“, „Pause“ und „Stop“.

XAML
```xml
<Grid KeyDown="Grid_KeyDown">

  <Grid.RowDefinitions>
    <RowDefinition Height="Auto" />
    <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>

  <MediaElement x:Name="DemoMovie" Source="xbox.wmv"
    Width="500" Height="500" Margin="20" HorizontalAlignment="Center" />

  <StackPanel Grid.Row="1" Margin="10"
    Orientation="Horizontal" HorizontalAlignment="Center">

    <Button x:Name="PlayButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+P"
      AutomationProperties.AcceleratorKey="Control P">
      <TextBlock>Play</TextBlock>
    </Button>

    <Button x:Name="PauseButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+A"
      AutomationProperties.AcceleratorKey="Control A">
      <TextBlock>Pause</TextBlock>
    </Button>

    <Button x:Name="StopButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+S"
      AutomationProperties.AcceleratorKey="Control S">
      <TextBlock>Stop</TextBlock>
    </Button>
  </StackPanel>
</Grid>
```

> [!IMPORTANT]
> Durch die Einstellung [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/Hh759762) oder [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/Hh759763) werden keine Tastaturfunktionen aktiviert. Es wird lediglich an das Benutzeroberflächenautomatisierungs-Framework weitergegeben, welche Tasten verwendet werden sollen, damit diese Informationen über Hilfstechnologien an Benutzer weitergeleitet werden können. Die Implementierung für die Tastenverarbeitung muss dennoch im Code erfolgen, nicht in XAML. Sie müssen weiterhin Handler für [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/BR208941)- oder [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/BR208942)-Ereignisse im entsprechenden Steuerelement anhängen, um das Verhalten der Tastenkombination tatsächlich in die App zu implementieren. Außerdem wird der Unterstrichzusatz für eine Zugriffstaste nicht automatisch bereitgestellt. Sie müssen den Text für die jeweilige Taste in Ihrem mnemonischen Zeichen explizit als [**Underline**](https://msdn.microsoft.com/library/windows/apps/BR209982)-Formatierung unterstreichen, wenn in der UI unterstrichener Text angezeigt werden soll.

Aus Gründen der Einfachheit werden im obigen Beispiel Ressourcen für Zeichenfolgen wie „STRG+A“ weggelassen. Sie müssen die Tastenkombinationen aber auch während der Lokalisierung berücksichtigen. Die Lokalisierung von Tastenkombinationen ist wichtig, da die Auswahl einer Taste als Tastenkombination in der Regel von der sichtbaren Textbeschriftung des Elements abhängt.

Weitere Unterstützung bei der Implementierung von Tastenkombinationen erhalten Sie unter [Tastenkombinationen](http://go.microsoft.com/fwlink/p/?linkid=221825) in den Richtlinien zur benutzerfreundlichen Interaktion bei Windows.

<span id="Implementing_a_key_event_handler"/>
<span id="implementing_a_key_event_handler"/>
<span id="IMPLEMENTING_A_KEY_EVENT_HANDLER"/>
### Implementieren eines Tastenereignishandlers  
Für Eingabeereignisse wie Tastenereignisse wird ein als *Routingereignisse* bezeichnetes Ereigniskonzept verwendet. Ein Routingereignis kann durch die untergeordneten Elemente eines zusammengesetzten Steuerelements per Bubbling weitergeleitet werden, sodass ein gemeinsames übergeordnetes Steuerelement Ereignisse für mehrere untergeordnete Elemente behandeln kann. Dieses Ereignismodell eignet sich zum Definieren von Tastenkombinationsaktionen für ein Steuerelement mit mehreren zusammengesetzten Teilen, die entwurfsbedingt nicht den Fokus haben oder Teil der Aktivierreihenfolge sein können.

Beispielcode für das Erstellen eines Tastenereignishandlers, der eine Überprüfung auf Zusatztasten wie die STRG-TASTE enthält, finden Sie unter [Tastaturinteraktionen](https://msdn.microsoft.com/library/windows/apps/Mt185607).

<span id="Keyboard_navigation_for_custom_controls"/>
<span id="keyboard_navigation_for_custom_controls"/>
<span id="KEYBOARD_NAVIGATION_FOR_CUSTOM_CONTROLS"/>
## Tastaturnavigation für benutzerdefinierte Steuerelemente  
Wenn zwischen untergeordneten Elementen eine räumliche Beziehung besteht, empfehlen wir, die Pfeiltasten als Tastenkombinationen für die Navigation zwischen den Elementen zu verwenden. Wenn Knoten der Strukturansicht separate untergeordnete Elemente für die Verarbeitung der Erweitern/Reduzieren- und Knoten-Aktivierung aufweisen, sollten Sie die Nach-links- und die Nach-rechts-Taste verwenden, um die Erweitern/Reduzieren-Funktion über die Tastatur bereitzustellen. Für ein ausgerichtetes Steuerelement, dessen Inhalt in einer bestimmten Richtung durchlaufen werden kann, verwenden Sie die entsprechenden Pfeiltasten.

Im Allgemeinen implementieren Sie die benutzerdefinierte Tastenverarbeitung für benutzerdefinierte Steuerelemente, indem Sie in der Klassenlogik eine Überschreibung der Methoden [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982.aspx) und [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/hh967983.aspx) hinzufügen.

<span id="An_example_of_a_visual_state_for_a_focus_indicator"/>
<span id="an_example_of_a_visual_state_for_a_focus_indicator"/>
<span id="AN_EXAMPLE_OF_A_VISUAL_STATE_FOR_A_FOCUS_INDICATOR"/>
## Beispiel für einen Ansichtszustand für eine Fokusanzeige  
Wie bereits erwählt sollte jedes benutzerdefinierte Steuerelement, mit dem Benutzer fokussieren können, über eine visuelle Fokusanzeige verfügen. In der Regel kann diese Fokusanzeige einfach durch Zeichnen eines Rechtecks direkt um das normale umgebende Rechteck des Steuerelements erzeugt werden. Das [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371)-Element für visuellen Fokus ist ein Peerelement der übrigen Zusammenstellung des Steuerelements in einer Steuerelementvorlage, wird jedoch anfänglich mit dem [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992)-Wert **Collapsed** festgelegt, da das Steuerelement noch nicht fokussiert ist. Wenn das Steuerelement im Fokus steht, wird ein visueller Zustand aufgerufen, der die **Visibility** des visuellen Fokus speziell auf **Visible** festlegt. Sobald der Fokus verschoben wird, wird ein anderer visueller Zustand auf gerufen, und die **Visibility** wird zu **Collapsed**.

Alle standardmäßigen XAML-Steuerelemente weisen eine entsprechende visuelle Fokusanzeige auf, wenn sie im Fokus stehen (sofern dies möglich ist). Es gibt auch potenziell unterschiedliche Designs je nach ausgewähltem Design des Benutzers (insbesondere wenn der Benutzer einen hohen Kontrast verwendet). Wenn Sie die XAML-Steuerelemente in Ihrer UI verwenden und nicht die Steuerelementvorlagen ersetzen, sind für visuelle Fokusanzeigen für Steuerelemente, die korrekt funktionieren und angezeigt werden, keine weiteren Schritte erforderlich sind. Wenn Sie aber eine neue Vorlage für ein Steuerelement verwenden möchten oder sich fragen, wie XAML-Steuerelemente ihre Fokusanzeigen bereitstellen, wird im restlichen Teil dieses Abschnitts erläutert, wie Sie dies in XAML und in der Steuerelementlogik erreichen.

Im Folgenden finden Sie eine Beispiel-XAML, die aus der standardmäßigen XAML-Vorlage für [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) stammt.

XAML
```xml
<ControlTemplate TargetType="Button">
...
    <Rectangle
      x:Name="FocusVisualWhite"
      IsHitTestVisible="False"
      Stroke="{ThemeResource FocusVisualWhiteStrokeThemeBrush}"
      StrokeEndLineCap="Square"
      StrokeDashArray="1,1"
      Opacity="0"
      StrokeDashOffset="1.5"/>
    <Rectangle
      x:Name="FocusVisualBlack"
      IsHitTestVisible="False"
      Stroke="{ThemeResource FocusVisualBlackStrokeThemeBrush}"
      StrokeEndLineCap="Square"
      StrokeDashArray="1,1"
      Opacity="0"
      StrokeDashOffset="0.5"/>
...
</ControlTemplate>
```

Bislang ist nur die Zusammensetzung vorhanden. Zum Steuern der Sichtbarkeit der Fokusanzeige definieren Sie visuelle Zustände zum Umschalten der Eigenschaft [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992). Dazu verwenden Sie die angefügte Eigenschaft [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/Hh738505) wie bei der Anwendung auf das Stammelement, das die Zusammensetzung definiert.

XAML
```xml
<ControlTemplate TargetType="Button">
  <Grid>
    <VisualStateManager.VisualStateGroups>
       <!--other visual state groups here-->
       <VisualStateGroup x:Name="FocusStates">
         <VisualState x:Name="Focused">
           <Storyboard>
             <DoubleAnimation
               Storyboard.TargetName="FocusVisualWhite"
               Storyboard.TargetProperty="Opacity"
               To="1" Duration="0"/>
             <DoubleAnimation
               Storyboard.TargetName="FocusVisualBlack"
               Storyboard.TargetProperty="Opacity"
               To="1" Duration="0"/>
         </VisualState>
         <VisualState x:Name="Unfocused" />
         <VisualState x:Name="PointerFocused" />
       </VisualStateGroup>
     <VisualStateManager.VisualStateGroups>
<!--composition is here-->
   </Grid>
</ControlTemplate>
```

Nur einer der benannten Zustände passt [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) direkt an, während die anderen scheinbar leer sind. Die Funktionsweise visueller Zustände besteht darin, dass bei Verwendung eines anderen Zustands aus derselben [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/BR209014) durch das Steuerelement alle im vorhergehenden Zustand angewendeten Animationen sofort abgebrochen werden. Dies bedeutet, dass das Rechteck nicht angezeigt wird, da die Standard-**Visibility** aus der Zusammensetzung **Collapsed** ist. Die Logik des Steuerelements steuert dies durch Überwachen von Fokusereignissen wie z.B. [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/BR208927) und Ändern der Status mit [**GoToState**](https://msdn.microsoft.com/library/windows/apps/BR209025). Bei Verwendung eines Standardsteuerelements oder bei einer Anpassung, die auf einem Steuerelement mit diesem Verhalten basiert, wird dieser Schritt oftmals automatisch für Sie erledigt.

<span id="Keyboard_accessibility_and_Windows_Phone"/>
<span id="keyboard_accessibility_and_windows_phone"/>
<span id="KEYBOARD_ACCESSIBILITY_AND_WINDOWS_PHONE"/>
## Barrierefreiheit für den Tastaturzugriff und Windows Phone
Ein Windows Phone-Gerät verfügt in der Regel nicht über eine dedizierte Hardwaretastatur. Ein Soft Input Panel (SIP) kann jedoch mehrere Szenarien der Barrierefreiheit für den Tastaturzugriff unterstützen. Sprachausgaben können Texteingaben vom **Text**-SIP lesen, darunter Löschankündigungen. Benutzer wissen, wo sich ihre Finger befinden, da die Sprachausgabe erkennt, dass der Benutzer Tasten auswählt, und den Namen der ausgewählten Taste laut vorliest. Einige der tastaturbezogenen Konzepte für die Barrierefreiheit können auch zugehörigen Hilfstechnologieverhalten zugeordnet werden, die überhaupt keine Tastatur verwenden. Das SIP verfügt zwar nicht über eine TAB-Taste, die Sprachausgabe unterstützt jedoch eine Touch-Bewegung, die dem Drücken der TAB-Taste entspricht. Daher ist eine geeignete Aktivierreihenfolge über die Steuerelemente in einer UI weiterhin ein wichtiges Barrierefreiheitsprinzip. Pfeiltasten, wie sie für die Navigation innerhalb von komplexen Steuerelementen verwendet werden, werden auch über die Toucheingabe der Sprachausgabe unterstützt. Sobald der Fokus auf einem Steuerelement liegt, das nicht zur Texteingabe dient, unterstützt die Sprachausgabe eine Geste zum Aufrufen der Aktion dieses Steuerelements.

Tastenkombinationen sind für Windows Phone-Apps in der Regel nicht relevant, da ein SIP nicht über STRG- und ALT-Tasten verfügt.

<span id="related_topics"/>
## Verwandte Themen  
* [Barrierefreiheit](accessibility.md)
* [Tastaturinteraktionen](https://msdn.microsoft.com/library/windows/apps/Mt185607)
* [Eingabe: Beispiel für die Touch-Bildschirmtastatur](http://go.microsoft.com/fwlink/p/?linkid=246019)
* [Beispiel für die Reaktion auf die Anzeige der Bildschirmtastatur](http://go.microsoft.com/fwlink/p/?linkid=231633)
* [XAML-Beispiel für Barrierefreiheit](http://go.microsoft.com/fwlink/p/?linkid=238570)




<!--HONumber=Aug16_HO3-->


