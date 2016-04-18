---
Description: Wenn Ihre App keine barrierefreie Bedienung mit der Tastatur ermöglicht, können Benutzer, die blind oder in ihrer Beweglichkeit eingeschränkt sind, Schwierigkeiten bei der Verwendung Ihrer App haben oder Ihre App möglicherweise überhaupt nicht nutzen.
title: Barrierefreiheit der Tastaturnavigation
ms.assetid: DDAE8C4B-7907-49FE-9645-F105F8DFAD8B
label: Keyboard accessibility
template: detail.hbs
---

Barrierefreiheit der Tastaturnavigation
=================================================================================

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Wenn Ihre App keine barrierefreie Bedienung mit der Tastatur ermöglicht, können Benutzer, die blind oder in ihrer Beweglichkeit eingeschränkt sind, Schwierigkeiten bei der Verwendung Ihrer App haben oder Ihre App möglicherweise überhaupt nicht nutzen.

<span id="keyboard_navigation_among_UI_elements"></span><span id="keyboard_navigation_among_ui_elements"></span><span id="KEYBOARD_NAVIGATION_AMONG_UI_ELEMENTS"></span>Navigation zwischen UI-Elementen mithilfe der Tastatur
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Um die Tastatur für ein Steuerelement verwenden zu können, muss das Steuerelement den Fokus haben, und damit es den Fokus (ohne Verwendung eines Zeigers) erhalten kann, muss es in einem Benutzeroberflächenentwurf über die TAB-Navigation erreichbar sein. Standardmäßig entspricht die Aktivierreihenfolge von Steuerelementen der Reihenfolge, in der die Steuerelemente der Entwurfsoberfläche hinzugefügt, in XAML aufgelistet oder programmgesteuert einem Container hinzugefügt werden.

In den meisten Fällen ist die auf Ihrer Definition der Steuerelemente in XAML basierende Standardreihenfolge die beste Reihenfolge, insbesondere, weil die Steuerelemente in dieser Reihenfolge auch von Bildschirmleseprogrammen gelesen werden. Die Standardreihenfolge ist jedoch nicht notwendigerweise mit der visuellen Reihenfolge identisch. Die tatsächliche Anzeigeposition kann vom übergeordneten Layoutcontainer und bestimmten Eigenschaften abhängen, die Sie für die untergeordneten Elemente festlegen können, um das Layout zu beeinflussen. Testen Sie dieses Verhalten selbst, um sicherzustellen dass die Aktivierreihenfolge Ihrer App gut ist. Insbesondere bei einer Raster- oder Tabellenmetapher für Ihr Layout lesen die Benutzer am Ende unter Umständen in einer anderen Reihenfolge als der Aktivierreihenfolge. Das ist an sich nicht unbedingt ein Problem. Achten Sie aber darauf, die Funktionen der App sowohl als UI für die Toucheingabe als auch als UI für die Verwendung mit der Tastatur zu testen. Vergewissern Sie sich, dass die UI bei beiden Methoden sinnvoll ist.

Sie können eine der visuellen Reihenfolge entsprechende Aktivierreihenfolge verwenden oder das XAML anpassen. Sie können auch die standardmäßige Aktivierreihenfolge überschreiben, indem Sie die Eigenschaft [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) festlegen. Das folgende Beispiel zeigt dies für ein [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)-Layout mit Tab-Navigation, bei dem zuerst Spalten aktiviert werden.

<span codelanguage="XAML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XAML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;!--Custom tab order.--&gt; 
&lt;Grid&gt;
  &lt;Grid.RowDefinitions&gt;...&lt;/Grid.RowDefinitions&gt;
  &lt;Grid.ColumnDefinitions&gt;...&lt;/Grid.ColumnDefinitions&gt;

  &lt;TextBlock Grid.Column=&quot;1&quot; HorizontalAlignment=&quot;Center&quot;&gt;Groom&lt;/TextBlock&gt;
  &lt;TextBlock Grid.Column=&quot;2&quot; HorizontalAlignment=&quot;Center&quot;&gt;Bride&lt;/TextBlock&gt;

  &lt;TextBlock Grid.Row=&quot;1&quot;&gt;First name&lt;/TextBlock&gt;
  &lt;TextBox x:Name=&quot;GroomFirstName&quot; Grid.Row=&quot;1&quot; Grid.Column=&quot;1&quot; TabIndex=&quot;1&quot;/&gt;
  &lt;TextBox x:Name=&quot;BrideFirstName&quot; Grid.Row=&quot;1&quot; Grid.Column=&quot;2&quot; TabIndex=&quot;3&quot;/&gt;

  &lt;TextBlock Grid.Row=&quot;2&quot;&gt;Last name&lt;/TextBlock&gt;
  &lt;TextBox x:Name=&quot;GroomLastName&quot; Grid.Row=&quot;2&quot; Grid.Column=&quot;1&quot; TabIndex=&quot;2&quot;/&gt;
  &lt;TextBox x:Name=&quot;BrideLastName&quot; Grid.Row=&quot;2&quot; Grid.Column=&quot;2&quot; TabIndex=&quot;4&quot;/&gt;
&lt;/Grid&gt;</code></pre></td>
</tr>
</tbody>
</table>

Möglicherweise möchten Sie ein Steuerelement von der Aktivierreihenfolge ausschließen. In der Regel legen Sie das Steuerelement dazu nur als nicht interaktiv fest, indem Sie z. B. seine [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/BR209419)-Eigenschaft auf **false** setzen. Ein deaktiviertes Steuerelement wird automatisch von der Aktivierreihenfolge ausgeschlossen. Mitunter ist es aber erforderlich, ein nicht deaktiviertes Steuerelement von der Aktivierreihenfolge auszuschließen. In diesem Fall können Sie die [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/BR209422)-Eigenschaft auf **false** festlegen.

Alle Elemente, die im Fokus stehen können, sind normalerweise standardmäßig in der Aktivierreihenfolge enthalten. Eine Ausnahme sind bestimmte Textanzeigetypen wie [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565), die im Fokus stehen können, damit die Zwischenablage zur Textauswahl auf sie zugreifen kann. Diese Elemente sind aber nicht in der Aktivierreihenfolge enthalten, weil dies bei statischen Textelementen nicht zu erwarten ist. Sie sind nicht im herkömmlichen Sinn interaktiv (sie können nicht aufgerufen werden und erfordern keine Texteingabe, unterstützen aber das [Steuerelementmuster für Text](https://msdn.microsoft.com/library/windows/desktop/Ee671194), das das Suchen und Anpassen von Auswahlpunkten in Text unterstützt). Text sollte nicht darauf hindeuten, dass er eine mögliche Aktion auslöst, wenn er den Fokus erhält. Textelemente werden von Hilfstechnologien weiterhin erkannt und von Sprachausgaben laut vorgelesen. Dies basiert jedoch auf anderen Techniken als dem Suchen dieser Elemente in der praktischen Aktivierreihenfolge.

Unabhängig davon, ob Sie [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461)-Werte anpassen oder die Standardreihenfolge verwenden, gelten folgende Regeln:

-   UI-Elemente, bei denen [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) gleich 0 ist, werden der Aktivierreihenfolge basierend auf der Deklarierungsreihenfolge in XAML-Collections oder untergeordneten Collections hinzugefügt.
-   UI-Elemente, bei denen [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) größer als 0 ist, werden der Aktivierreihenfolge basierend auf dem Wert **TabIndex** hinzugefügt.
-   UI-Elemente, bei denen [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) kleiner als 0 ist, werden der Aktivierreihenfolge hinzugefügt und werden vor allen Elementen mit NULL-Werten angezeigt. Dies ist unter Umständen anders als bei der Verarbeitung des Attributs **tabindex** in HTML. (Ein negativer **tabindex** wurde in älteren HTML-Spezifikationen nicht unterstützt.)

<span id="keyboard_navigation_within_a_UI_element"></span><span id="keyboard_navigation_within_a_ui_element"></span><span id="KEYBOARD_NAVIGATION_WITHIN_A_UI_ELEMENT"></span>Navigation innerhalb eines UI-Elements mithilfe der Tastatur
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Bei zusammengesetzten Elementen ist es wichtig, eine korrekte interne Navigation zwischen den enthaltenen Elementen sicherzustellen. Ein zusammengesetztes Element kann seine derzeit aktiven untergeordneten Elemente verwalten, wodurch der Mehraufwand reduziert wird, der entsteht, wenn alle untergeordneten Elemente fokussierbar sein müssen. Ein solches zusammengesetztes Element ist in der Aktivierreihenfolge enthalten und behandelt Tastaturnavigationsereignisse selbst. Viele der zusammengesetzten Steuerelemente verfügen bereits über eine in ihre Ereignisbehandlung integrierte Navigationslogik. Das Durchlaufen von Elementen mit den Pfeiltasten ist z. B. standardmäßig für die Elemente eines [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)-, [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242704view)-, [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868)- und [**FlipView**](https://msdn.microsoft.com/library/windows/apps/BR242678)-Steuerelements aktiviert.

<span id="keyboard_activation"></span><span id="KEYBOARD_ACTIVATION"></span>Tastaturalternativen für Zeigeraktionen und Ereignisse für bestimmte Steuerelemente
-------------------------------------------------------------------------------------------------------------------------------------------------------------

Stellen Sie sicher, dass UI-Elemente, auf die geklickt werden kann, auch über die Tastatur aufgerufen werden können. Um die Tastatur für ein UI-Element verwenden zu können, muss das Element im Fokus stehen. Nur von [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) abgeleitete Klassen unterstützen den Fokus und die TAB-Navigation.

Implementieren Sie für UI-Elemente, die aufgerufen werden können, Tastatur-Ereignishandler für die LEERTASTE und die EINGABETASTE. So machen Sie die Unterstützung für die Barrierefreiheit des Tastaturzugriffs komplett und ermöglichen es Benutzern, einfache App-Szenarien nur über die Tastatur auszuführen – d. h. Benutzer können über die Tastatur alle interaktiven UI-Elemente erreichen und die Standardfunktionen aktivieren.

Wenn ein Element, das Sie in der UI verwenden möchten, nicht im Fokus stehen kann, können Sie ein eigenes benutzerdefiniertes Steuerelement erstellen. Sie müssen die Eigenschaft [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/BR209422) auf **true** festlegen, damit das Element im Fokus stehen kann, und Sie müssen eine visuelle Anzeige des fokussierten Zustands bereitstellen, indem Sie einen visuellen Zustand erstellen, der für eine Fokusanzeige auf der UI sorgt. Oft ist es allerdings einfacher, die Steuerelementkomposition zu verwenden, damit die Unterstützung für TAB-Navigation, Fokus und Microsoft-Benutzeroberflächenautomatisierungs-Peers und -Muster von dem Steuerelement behandelt werden kann, in dem Sie den Inhalt zusammensetzen.

Anstatt z. B. das Ereignis „PointerPressed“ für ein [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) zu verarbeiten, können Sie das Element in einem [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)-Objekt umschließen, um die Zeiger-, Tastatur- und Fokusunterstützung bereitzustellen.

<span codelanguage="XAML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XAML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;!--Don&#39;t do this.--&gt;
&lt;Image Source=&quot;sample.jpg&quot; PointerPressed=&quot;Image_PointerPressed&quot;/&gt;

&lt;!--Do this instead.--&gt;
&lt;Button Click=&quot;Button_Click&quot;&gt;&lt;Image Source=&quot;sample.jpg&quot;/&gt;&lt;/Button&gt;</code></pre></td>
</tr>
</tbody>
</table>

<span id="keyboard_shortcuts"></span><span id="KEYBOARD_SHORTCUTS"></span>Tastenkombinationen
--------------------------------------------------------------------------------------------

Zusätzlich zur Navigation und Aktivierung über die Tastatur empfiehlt es sich, Tastenkombinationen für die Funktionen Ihrer App zu implementieren. Die TAB-Navigation ist eine gute, einfache Form der Tastaturunterstützung. Bei komplexeren Formularen kann es aber sinnvoll sein, auch Unterstützung für Tastenkombinationen hinzuzufügen. Hierdurch kann Ihre App effizienter genutzt werden – auch von Menschen, die sowohl Tastatur als auch Zeigegeräte nutzen.

*Tastenkombinationen* sind eine effiziente Methode für den Zugriff auf App-Funktionen und verbessern daher die Produktivität der Benutzer. Es gibt zwei Arten von Tastenkombinationen:

-   *Tastenkombinationen mit der ALT-TASTE* ermöglichen den schnellen Zugriff auf UI-Elemente Ihrer App. Hierbei handelt es sich um eine Kombination aus der ALT-TASTE und einer Buchstabentaste.
-   *Tastenkombinationen mit der STRG-Taste* ermöglichen den schnellen Zugriff auf App-Befehle. Ihr App kann UI-Elemente aufweisen, die exakt dem Befehl entsprechen. Hierbei handelt es sich um eine Kombination aus der STRG-TASTE und einer Buchstabentaste.

Es ist unbedingt notwendig, dass Benutzer, die auf Bildschirmleseprogramme und andere Hilfstechnologien angewiesen sind, die Tastenkombinationen der App einfach erkennen können. Weisen Sie mithilfe von QuickInfos, Namen und Beschreibungen zur Verwendung durch Bildschirmleseprogramme oder anderen Hinweisen auf dem Bildschirm auf Tastenkombinationen hin. Zumindest sollten die Tastenkombinationen im Hilfeinhalt Ihrer App gut dokumentiert sein.

Sie können Zugriffstasten über Bildschirmleseprogramme dokumentieren, indem Sie die angefügte Eigenschaft [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/Hh759763) auf eine Zeichenfolge festlegen, die die Tastenkombination beschreibt. Darüber hinaus gibt es die angefügte Eigenschaft [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/Hh759762), um nicht-mnemonische Tastenkombinationen zu dokumentieren. Beide Eigenschaften werden von Bildschirmleseprogrammen jedoch normalerweise gleich behandelt. Sie sollten versuchen, Tastenkombinationen auf mehrere Arten zu dokumentieren – mithilfe von QuickInfos, Automatisierungseigenschaften und schriftlicher Hilfedokumentation.

Das folgende Beispiel zeigt das Dokumentieren von Tastenkombinationen für Schaltflächen für die Medienwiedergabe: „Play“, „Pause“ und „Stop“.

<span codelanguage="XAML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XAML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Grid KeyDown=&quot;Grid_KeyDown&quot;&gt;

  &lt;Grid.RowDefinitions&gt;
    &lt;RowDefinition Height=&quot;Auto&quot; /&gt;
    &lt;RowDefinition Height=&quot;Auto&quot; /&gt;
  &lt;/Grid.RowDefinitions&gt;

  &lt;MediaElement x:Name=&quot;DemoMovie&quot; Source=&quot;xbox.wmv&quot; 
    Width=&quot;500&quot; Height=&quot;500&quot; Margin=&quot;20&quot; HorizontalAlignment=&quot;Center&quot; /&gt;

  &lt;StackPanel Grid.Row=&quot;1&quot; Margin=&quot;10&quot;
    Orientation=&quot;Horizontal&quot; HorizontalAlignment=&quot;Center&quot;&gt;

    &lt;Button x:Name=&quot;PlayButton&quot; Click=&quot;MediaButton_Click&quot;
      ToolTipService.ToolTip=&quot;Shortcut key: Ctrl+P&quot;
      AutomationProperties.AcceleratorKey=&quot;Control P&quot;&gt;
      &lt;TextBlock&gt;Play&lt;/TextBlock&gt;
    &lt;/Button&gt;

    &lt;Button x:Name=&quot;PauseButton&quot; Click=&quot;MediaButton_Click&quot;
      ToolTipService.ToolTip=&quot;Shortcut key: Ctrl+A&quot; 
      AutomationProperties.AcceleratorKey=&quot;Control A&quot;&gt;
      &lt;TextBlock&gt;Pause&lt;/TextBlock&gt;
    &lt;/Button&gt;

    &lt;Button x:Name=&quot;StopButton&quot; Click=&quot;MediaButton_Click&quot;
      ToolTipService.ToolTip=&quot;Shortcut key: Ctrl+S&quot; 
      AutomationProperties.AcceleratorKey=&quot;Control S&quot;&gt;
      &lt;TextBlock&gt;Stop&lt;/TextBlock&gt;
    &lt;/Button&gt;

  &lt;/StackPanel&gt;

&lt;/Grid&gt;</code></pre></td>
</tr>
</tbody>
</table>

**Wichtig** Durch die Einstellung [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/Hh759762) oder [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/Hh759763) werden keine Tastaturfunktionen aktiviert. Es wird lediglich an das Benutzeroberflächenautomatisierungs-Framework weitergegeben, welche Tasten verwendet werden sollen, damit diese Informationen über Hilfstechnologien an Benutzer weitergeleitet werden können. Die Implementierung für die Tastenverarbeitung muss dennoch im Code erfolgen, nicht in XAML. Sie müssen weiterhin Handler für [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/BR208941)- oder [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/BR208942)-Ereignisse im entsprechenden Steuerelement anhängen, um das Verhalten der Tastenkombination tatsächlich in die App zu implementieren. Außerdem wird der Unterstrichzusatz für eine Zugriffstaste nicht automatisch bereitgestellt. Sie müssen den Text für die jeweilige Taste in Ihrem mnemonischen Zeichen explizit als [**Underline**](https://msdn.microsoft.com/library/windows/apps/BR209982)-Formatierung unterstreichen, wenn in der UI unterstrichener Text angezeigt werden soll.

 

Aus Gründen der Einfachheit werden im obigen Beispiel Ressourcen für Zeichenfolgen wie „STRG+A“ weggelassen. Sie müssen die Tastenkombinationen aber auch während der Lokalisierung berücksichtigen. Die Lokalisierung von Tastenkombinationen ist wichtig, da die Auswahl einer Taste als Tastenkombination in der Regel von der sichtbaren Textbeschriftung des Elements abhängt.

Weitere Unterstützung bei der Implementierung von Tastenkombinationen erhalten Sie unter [Tastenkombinationen](http://go.microsoft.com/fwlink/p/?linkid=221825) in den Richtlinien zur benutzerfreundlichen Interaktion bei Windows.

### <span id="Implementing_a_key_event_handler"></span><span id="implementing_a_key_event_handler"></span><span id="IMPLEMENTING_A_KEY_EVENT_HANDLER"></span>Implementieren eines Tasten-Ereignishandlers

Für Eingabeereignisse wie Tastenereignisse wird ein als *Routingereignisse* bezeichnetes Ereigniskonzept verwendet. Ein Routingereignis kann durch die untergeordneten Elemente eines zusammengesetzten Steuerelements per Bubbling weitergeleitet werden, sodass ein gemeinsames übergeordnetes Steuerelement Ereignisse für mehrere untergeordnete Elemente behandeln kann. Dieses Ereignismodell eignet sich zum Definieren von Tastenkombinationsaktionen für ein Steuerelement mit mehreren zusammengesetzten Teilen, die entwurfsbedingt nicht den Fokus haben oder Teil der Aktivierreihenfolge sein können.

Beispielcode für das Erstellen eines Tastenereignishandlers, der eine Überprüfung auf Zusatztasten wie die STRG-TASTE enthält, finden Sie unter [Tastaturinteraktionen](https://msdn.microsoft.com/library/windows/apps/Mt185607).

<span id="Keyboard_navigation_for_custom_controls"></span><span id="keyboard_navigation_for_custom_controls"></span><span id="KEYBOARD_NAVIGATION_FOR_CUSTOM_CONTROLS"></span>Tastaturnavigation für benutzerdefinierte Steuerelemente
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Wenn zwischen untergeordneten Elementen eine räumliche Beziehung besteht, empfehlen wir, die Pfeiltasten als Tastenkombinationen für die Navigation zwischen den Elementen zu verwenden. Wenn Knoten der Strukturansicht separate untergeordnete Elemente für die Verarbeitung der Erweitern/Reduzieren- und Knoten-Aktivierung aufweisen, sollten Sie die Nach-links- und die Nach-rechts-Taste verwenden, um die Erweitern/Reduzieren-Funktion über die Tastatur bereitzustellen. Für ein ausgerichtetes Steuerelement, dessen Inhalt in einer bestimmten Richtung durchlaufen werden kann, verwenden Sie die entsprechenden Pfeiltasten.

Im Allgemeinen implementieren Sie die benutzerdefinierte Tastenverarbeitung für benutzerdefinierte Steuerelemente, indem Sie in der Klassenlogik eine Überschreibung der Methoden [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/BR209390_onkeydown) und [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/BR209390_onkeyup) hinzufügen.

<span id="An_example_of_a_visual_state_for_a_focus_indicator"></span><span id="an_example_of_a_visual_state_for_a_focus_indicator"></span><span id="AN_EXAMPLE_OF_A_VISUAL_STATE_FOR_A_FOCUS_INDICATOR"></span>Beispiel für einen Ansichtszustand für eine Fokusanzeige
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Wie bereits erwählt sollte jedes benutzerdefinierte Steuerelement, mit dem Benutzer fokussieren können, über eine visuelle Fokusanzeige verfügen. In der Regel kann diese Fokusanzeige einfach durch Zeichnen eines Rechtecks direkt um das normale umgebende Rechteck des Steuerelements erzeugt werden. Das [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371)-Element für visuellen Fokus ist ein Peerelement der übrigen Zusammenstellung des Steuerelements in einer Steuerelementvorlage, wird jedoch anfänglich mit dem [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992)-Wert **Collapsed** festgelegt, da das Steuerelement noch nicht fokussiert ist. Wenn das Steuerelement im Fokus steht, wird ein visueller Zustand aufgerufen, der die **Visibility** des visuellen Fokus speziell auf **Visible** festlegt. Sobald der Fokus verschoben wird, wird ein anderer visueller Zustand auf gerufen, und die **Visibility** wird zu **Collapsed**.

Alle standardmäßigen XAML-Steuerelemente weisen eine entsprechende visuelle Fokusanzeige auf, wenn sie im Fokus stehen (sofern dies möglich ist). Es gibt auch potenziell unterschiedliche Designs je nach ausgewähltem Design des Benutzers (insbesondere wenn der Benutzer einen hohen Kontrast verwendet). Wenn Sie die XAML-Steuerelemente in Ihrer UI verwenden und nicht die Steuerelementvorlagen ersetzen, sind für visuelle Fokusanzeigen für Steuerelemente, die korrekt funktionieren und angezeigt werden, keine weiteren Schritte erforderlich sind. Wenn Sie aber eine neue Vorlage für ein Steuerelement verwenden möchten oder sich fragen, wie XAML-Steuerelemente ihre Fokusanzeigen bereitstellen, wird im restlichen Teil dieses Abschnitts erläutert, wie Sie dies in XAML und in der Steuerelementlogik erreichen.

Im Folgenden finden Sie eine Beispiel-XAML, die aus der standardmäßigen XAML-Vorlage für [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) stammt.

<span codelanguage="XAML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XAML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;ControlTemplate TargetType=&quot;Button&quot;&gt;
... 
    &lt;Rectangle 
      x:Name=&quot;FocusVisualWhite&quot; 
      IsHitTestVisible=&quot;False&quot; 
      Stroke=&quot;{ThemeResource FocusVisualWhiteStrokeThemeBrush}&quot; 
      StrokeEndLineCap=&quot;Square&quot; 
      StrokeDashArray=&quot;1,1&quot; 
      Opacity=&quot;0&quot; 
      StrokeDashOffset=&quot;1.5&quot;/&gt;
    &lt;Rectangle 
      x:Name=&quot;FocusVisualBlack&quot; 
      IsHitTestVisible=&quot;False&quot; 
      Stroke=&quot;{ThemeResource FocusVisualBlackStrokeThemeBrush}&quot; 
      StrokeEndLineCap=&quot;Square&quot; 
      StrokeDashArray=&quot;1,1&quot; 
      Opacity=&quot;0&quot; 
      StrokeDashOffset=&quot;0.5&quot;/&gt;
...
&lt;/ControlTemplate&gt;</code></pre></td>
</tr>
</tbody>
</table>

Bislang ist nur die Zusammensetzung vorhanden. Zum Steuern der Sichtbarkeit der Fokusanzeige definieren Sie visuelle Zustände zum Umschalten der Eigenschaft [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992). Dazu verwenden Sie die angefügte Eigenschaft [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/Hh738505) wie bei der Anwendung auf das Stammelement, das die Zusammensetzung definiert.

<span codelanguage="XAML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XAML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;ControlTemplate TargetType=&quot;Button&quot;&gt;
  &lt;Grid&gt;
    &lt;VisualStateManager.VisualStateGroups&gt; 
       &lt;!--other visual state groups here--&gt;
       &lt;VisualStateGroup x:Name=&quot;FocusStates&quot;&gt; 
         &lt;VisualState x:Name=&quot;Focused&quot;&gt; 
           &lt;Storyboard&gt; 
             &lt;DoubleAnimation 
               Storyboard.TargetName=&quot;FocusVisualWhite&quot; 
               Storyboard.TargetProperty=&quot;Opacity&quot; 
               To=&quot;1&quot; Duration=&quot;0&quot;/&gt;
             &lt;DoubleAnimation 
               Storyboard.TargetName=&quot;FocusVisualBlack&quot; 
               Storyboard.TargetProperty=&quot;Opacity&quot; 
               To=&quot;1&quot; Duration=&quot;0&quot;/&gt;
         &lt;/VisualState&gt; 
         &lt;VisualState x:Name=&quot;Unfocused&quot; /&gt; 
         &lt;VisualState x:Name=&quot;PointerFocused&quot; /&gt; 
       &lt;/VisualStateGroup&gt;
     &lt;VisualStateManager.VisualStateGroups&gt;
&lt;!--composition is here--&gt;
   &lt;/Grid&gt;
&lt;/ControlTemplate&gt;</code></pre></td>
</tr>
</tbody>
</table>

Nur einer der benannten Zustände passt [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) direkt an, während die anderen scheinbar leer sind. Die Funktionsweise visueller Zustände besteht darin, dass bei Verwendung eines anderen Zustands aus derselben [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/BR209014) durch das Steuerelement alle im vorhergehenden Zustand angewendeten Animationen sofort abgebrochen werden. Dies bedeutet, dass das Rechteck nicht angezeigt wird, da die Standard-**Visibility** aus der Zusammensetzung **Collapsed** ist. Die Logik des Steuerelements steuert dies durch Überwachen von Fokusereignissen wie z. B. [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/BR208927) und Ändern der Status mit [**GoToState**](https://msdn.microsoft.com/library/windows/apps/BR209025). Bei Verwendung eines Standardsteuerelements oder bei einer Anpassung, die auf einem Steuerelement mit diesem Verhalten basiert, wird dieser Schritt oftmals automatisch für Sie erledigt.

<span id="_Keyboard_accessibility_and_Windows_Phone"></span><span id="_keyboard_accessibility_and_windows_phone"></span><span id="_KEYBOARD_ACCESSIBILITY_AND_WINDOWS_PHONE"></span> Barrierefreiheit für den Tastaturzugriff und Windows Phone
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Ein Windows Phone-Gerät verfügt in der Regel nicht über eine dedizierte Hardwaretastatur. Ein Soft Input Panel (SIP) kann jedoch mehrere Szenarien der Barrierefreiheit für den Tastaturzugriff unterstützen. Sprachausgaben können Texteingaben vom **Text**-SIP lesen, darunter Löschankündigungen. Benutzer wissen, wo sich ihre Finger befinden, da die Sprachausgabe erkennt, dass der Benutzer Tasten auswählt, und den Namen der ausgewählten Taste laut vorliest. Einige der tastaturbezogenen Konzepte für die Barrierefreiheit können auch zugehörigen Hilfstechnologieverhalten zugeordnet werden, die überhaupt keine Tastatur verwenden. Das SIP verfügt zwar nicht über eine TAB-Taste, die Sprachausgabe unterstützt jedoch eine Touch-Bewegung, die dem Drücken der TAB-Taste entspricht. Daher ist eine geeignete Aktivierreihenfolge über die Steuerelemente in einer UI weiterhin ein wichtiges Barrierefreiheitsprinzip. Pfeiltasten, wie sie für die Navigation innerhalb von komplexen Steuerelementen verwendet werden, werden auch über die Toucheingabe der Sprachausgabe unterstützt. Sobald der Fokus auf einem Steuerelement liegt, das nicht zur Texteingabe dient, unterstützt die Sprachausgabe eine Geste zum Aufrufen der Aktion dieses Steuerelements.

Tastenkombinationen sind für Windows Phone-Apps in der Regel nicht relevant, da ein SIP nicht über STRG- und ALT-Tasten verfügt.

<span id="related_topics"></span>Verwandte Themen
-----------------------------------------------

* [Barrierefreiheit](accessibility.md)
* [Tastaturinteraktionen](https://msdn.microsoft.com/library/windows/apps/Mt185607)
* [Eingabe: Beispiel für die Touch-Bildschirmtastatur](http://go.microsoft.com/fwlink/p/?linkid=246019)
* [Beispiel für die Reaktion auf die Anzeige der Bildschirmtastatur](http://go.microsoft.com/fwlink/p/?linkid=231633)
* [XAML-Beispiel für Barrierefreiheit](http://go.microsoft.com/fwlink/p/?linkid=238570)
 

 





<!--HONumber=Mar16_HO3-->


