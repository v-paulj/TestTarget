---
author: Karl-Bridge-Microsoft
Description: "Aktivieren Sie den Tastaturzugriff mit TAB-Navigation und Zugriffstasten, um Benutzern das Navigieren durch UI-Elemente mithilfe der Tastatur zu ermöglichen."
title: Zugriffstasten
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Access keys
template: detail.hbs
keyword: Access keys, keyboard, accessibility
translationtype: Human Translation
ms.sourcegitcommit: ac86012b63646e53dbde492eef504cb8230f2afd
ms.openlocfilehash: d96d507c6ce8537888619ce174e2ff0e5284dcce

---

# Zugriffstasten

Benutzer, die beispielsweise aufgrund motorischer Einschränkungen Schwierigkeiten haben, eine Maus zu bedienen, greifen häufig auf die Tastatur zurück, um in einer App zu navigieren und damit zu interagieren.  Das XAML-Framework bietet Ihnen die Möglichkeit, den Zugriff auf UI-Elemente mithilfe der TAB-Navigation und Zugriffstasten über die Tastatur zu ermöglichen.

- Die TAB-Navigation ist ein grundlegendes tastaturgestütztes Angebot für Barrierefreiheit (und standardmäßig aktiviert). Dabei können Benutzer den Fokus zwischen UI-Elementen mithilfe der TAB-TASTE und der Pfeiltasten auf der Tastatur verschieben.
- Zugriffstasten stellen ein ergänzendes Angebot für Barrierefreiheit dar (das Sie in Ihrer App implementieren). Sie ermöglichen über eine Kombination aus Tastaturmodifizierer (ALT-TASTE) und mindestens einer alphanumerischen Taste (normalerweise einem Buchstaben, der dem Befehl zugeordnet ist) den schnellen Zugriff auf App-Befehle. Häufige Zugriffstasten sind _ALT+F_ zum Öffnen des Menüs „Datei“ und _ALT+AL_ zum linksbündigen ausrichten.  

Weitere Informationen zur Tastaturnavigation und Barrierefreiheit finden Sie unter [Tastaturinteraktionen](https://msdn.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions) und [Barrierefreiheit der Tastaturnavigation](https://msdn.microsoft.com/windows/uwp/accessibility/keyboard-accessibility). In diesem Artikel wird davon ausgegangen, dass Sie mit den in diesen Artikeln erörterten Konzepten vertraut sind.

## Übersicht über Zugriffstasten

Mithilfe von Zugriffstasten können Benutzer Schaltflächen direkt aufrufen oder den Fokus mithilfe der Tastatur festlegen, ohne wiederholt die Pfeiltasten und die TAB-TASTE zu drücken. Zugriffstasten sollten leicht erkennbar sein und daher direkt in der Benutzeroberfläche dokumentiert werden, beispielsweise durch ein unverankertes Badge über dem Steuerelement mit der Zugriffstaste.

![Beispiel für Zugriffstasten und zugehörige Zugriffstasteninfos in Microsoft Word](images/keyboard/accesskeys-keytips.png)

_Abbildung 1: Beispiel für Zugriffstasten und zugehörige Zugriffstasteninfos in Microsoft Word_

Eine Zugriffstaste entspricht mindestens einem alphanumerischen Zeichen, das einem UI-Element zugeordnet ist. In Microsoft Word wird beispielsweise _H_ für die Registerkarte „Start“, _2_ für die Schaltfläche „Rückgängig“ oder _JI_ für die Registerkarte „Zeichnen“ verwendet.

**Zugriffstastenbereich**

Eine Zugriffstaste gehört zu einem bestimmten Bereich. In Abbildung 1 gehören _F_, _H_, _N_ und _JI_ beispielsweise zum Bereich der Seite.  Wenn der Benutzer _H_ drückt, ändert sich der Bereich in den Bereich der Registerkarte „Start“. Daraufhin werden deren Zugriffstasten angezeigt, wie in Abbildung 2 dargestellt. Die Zugriffstasten _V_, _FP_, _FF_ und _FS_ gehören zum Bereich der Registerkarte „Start“.

![Beispiel für Zugriffstasten und zugehörige Zugriffstasteninfos für den Bereich der Registerkarte „Start“ in Microsoft Word](images/keyboard/accesskeys-keytips-hometab.png)

_Abbildung 2: Beispiel für Zugriffstasten und zugehörige Zugriffstasteninfos für den Bereich der Registerkarte „Start“ in Microsoft Word_

Zwei Elemente können über die gleichen Zugriffstasten verfügen, wenn die Elemente zu verschiedenen Bereichen gehören. Beispielsweise ist _2_ im Bereich der Seite die Zugriffstaste für „Rückgängig“ (Abbildung 1) und im Bereich der Registerkarte „Start“ auch die Zugriffstaste für „Kursiv“ (Abbildung 2). Solange kein anderer Bereich angegeben ist, gehören alle Zugriffstasten zum Standardbereich.

**Zugriffstastenabfolge**

Bei einer Kombination von Zugriffstasten wird in der Regel eine Taste nach der anderen gedrückt, um eine Aktion zu erzielen, anstatt die Tasten gleichzeitig zu drücken. (Eine Ausnahme von dieser Regel wird im nächsten Abschnitt erörtert.) Die Abfolge von Tastenanschlägen, die zum Ausführen der Aktion erforderlich sind, wird als _Zugriffstastenabfolge_ bezeichnet. Der Benutzer drückt die ALT-TASTE, um die Zugriffstastenabfolge zu initiieren. Eine Zugriffstaste wird aufgerufen, wenn der Benutzer die letzte Taste in einer Zugriffstastenabfolge drückt. Um z. B. die Registerkarte „Ansicht“ in Word zu öffnen, würde der Benutzer die Zugriffstastenabfolge _ALT, W_ drücken.

Ein Benutzer kann mehrere Zugriffstasten in einer Zugriffstastenabfolge aufrufen. Beispiel: Um „Format übertragen“ in einem Word-Dokument zu öffnen, drückt der Benutzer ALT, um die Abfolge zu starten, dann _H_, um zum Abschnitt „Start“ zu navigieren und den Bereich der Zugriffstaste zu ändern, dann _F_ und zuletzt _P_. _H_ und _FP_ sind die Zugriffstasten für die Registerkarte „Start“ bzw. die Schaltfläche „Format übertragen“.

Durch einige Elemente (wie die Schaltfläche „Format übertragen“) wird eine Zugriffstastenabfolge abgeschlossen, nachdem sie aufgerufen wurden, bei anderen (wie der Registerkarte „Start“) nicht. Das Aufrufen einer Zugriffstaste kann bewirken, dass ein Befehl ausgeführt, der Fokus verschoben, der Zugriffstastenbereich geändert oder eine andere zugeordnete Aktion ausgeführt wird.

## Benutzerinteraktion mit Zugriffstasten

Um die Zugriffstasten-APIs zu verstehen, müssen Sie sich zunächst das Benutzerinteraktionsmodell vergegenwärtigen. Im Folgenden finden Sie eine Übersicht über das Modell für die Benutzerinteraktion mit Zugriffstasten:

- Wenn der Benutzer die ALT-TASTE drückt, wird die Zugriffstastenabfolge selbst dann gestartet, wenn sich der Fokus auf einem Eingabesteuerelement befindet. Anschließend kann der Benutzer die Zugriffstaste drücken, um die zugeordnete Aktion aufzurufen. Diese Benutzerinteraktion erfordert, dass Sie die verfügbaren Zugriffstasten innerhalb der Benutzeroberfläche visuell dokumentieren, beispielsweise durch unverankerte Badges, die eingeblendet werden, wenn die ALT-TASTE gedrückt wird.
- Wenn der Benutzer die ALT-TASTE und die Zugriffstaste gleichzeitig drückt, wird die Zugriffstaste sofort aufgerufen. Dies ist vergleichbar mit einer durch ALT+_Zugriffstaste_ definierten Tastenkombination. In diesem Fall werden keine visuellen Angebote für die Zugriffstaste eingeblendet. Durch das Aufrufen einer Zugriffstaste kann jedoch auch der Zugriffstastenbereich geändert werden. In diesem Fall wird eine Zugriffstastenabfolge initiiert, und gleichzeitig werden visuelle Angebote für den neuen Bereich eingeblendet.
    > [!NOTE]
    > Diese Benutzerinteraktion wird nur von Zugriffstasten mit einem Zeichen unterstützt. Die Kombination aus ALT+_Zugriffstaste_ wird für Zugriffstasten mit mehr als einem Zeichen nicht unterstützt.    
- Wenn Zugriffstasten, die aus mehreren Zeichen bestehen, teilweise dieselben Zeichen verwenden und der Benutzer eines dieser mehrfach verwendeten Zeichen drückt, werden die Zugriffstasten gefiltert. Angenommen, die drei Zugriffstasten _A1_, _A2_ und _C_ werden angezeigt. Wenn der Benutzer _A_ drückt, werden nur die Zugriffstasten _A1_ und _A2_ angezeigt, während das visuelle Angebot für „C“ ausgeblendet wird.
- Durch die ESC-TASTE wird eine Filterebene entfernt. Beispiel: Wenn die Zugriffstasten _B_, _ABC_, _ACD_ und _ABD_ verfügbar sind und der Benutzer _A_ drückt, werden nur _ABC_, _ACD_ und _ABD_ angezeigt. Wenn der Benutzer dann _B_ drückt, werden nur _ABC_ und _ABD_ angezeigt. Wenn der Benutzer ESC drückt, wird eine Filterebene entfernt, und die Zugriffstasten _ABC_, _ACD_ und _ABD_ werden angezeigt. Wenn der Benutzer noch einmal ESC drückt, wird eine weitere Filterebene entfernt, sodass alle Zugriffstasten – _B_, _ABC_, _ACD_ und _ABD_ – aktiviert sind und die zugehörigen visuellen Angebote angezeigt werden.
- Mit der ESC-TASTE navigieren Sie zurück zum vorherigen Bereich. Zugriffstasten können verschiedenen Bereichen angehören, um das Navigieren in Apps zu vereinfachen, die über zahlreiche Befehle verfügen. Die Zugriffstastenabfolge beginnt immer im Hauptbereich. Alle Zugriffstasten gehören zum Hauptbereich. Zugriffstasten, für die ein bestimmtes UI-Element als „Bereichsbesitzer“ angegeben ist, sind hiervon ausgenommen. Wenn der Benutzer die Zugriffstaste eines Elements aufruft, das einen Bereichsbesitzer darstellt, legt das XAML-Framework den Bereich automatisch auf das Element fest und fügt es einem internen Navigationsstapel für Zugriffstasten hinzu. Durch die ESC-TASTE können Sie im Navigationsstapel für Zugriffstasten zurückgehen.
- Es gibt verschiedene Möglichkeiten, die Zugriffstastenabfolge zu schließen:
    - Der Benutzer kann ALT drücken, um eine Zugriffstastenabfolge, die gerade aktiv ist, zu schließen. Denken Sie daran, dass die Zugriffstastenabfolge durch Drücken von ALT auch initiiert wird.
    - Durch die ESC-TASTE wird die Zugriffstastenabfolge geschlossen, wenn sie dem Hauptbereich angehört und nicht gefiltert ist.
        > [!NOTE]
        > Der Anschlag der ESC-TASTE wird an die UI-Ebene übergeben und auch dort verarbeitet.
- Mit der TAB-TASTE schließen Sie die Zugriffstastenabfolge und kehren zur TAB-Navigation zurück.
- Durch die EINGABETASTE wird die Zugriffstastenabfolge geschlossen, und der Tastenanschlag wird an das Element gesendet, das den Fokus hat.
- Durch die Pfeiltasten wird die Zugriffstastenabfolge geschlossen, und der Tastenanschlag wird an das Element gesendet, das den Fokus hat.
- Durch ein Zeiger-nach-unten-Ereignis – z. B. ein Mausklick oder eine Toucheingabe – wird die Zugriffstastenabfolge geschlossen.
- Wenn eine Zugriffstaste aufgerufen wird, wird die Zugriffstastenabfolge standardmäßig geschlossen.  Sie können dieses Verhalten jedoch überschreiben, indem Sie die [ExitDisplayModeOnAccessKeyInvoked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.exitdisplaymodeonaccesskeyinvoked.aspx)-Eigenschaft auf **false** festlegen.
- Zugriffstastenkonflikte treten auf, wenn kein DEA (deterministischer endlicher Automat) möglich ist. Zugriffstastenkonflikte lassen sich nicht immer vermeiden und können bei einer großen Anzahl von Befehlen oder Lokalisierungsproblemen auftreten oder dann, wenn Zugriffstasten während der Laufzeit generiert werden.

 Es gibt zwei Fälle, in denen Konflikte auftreten:
 - Wenn zwei UI-Elemente über denselben Zugriffstastenwert verfügen und demselben Zugriffstastenbereich angehören. Beispiel: Zugriffstaste _A1_ für `button1` und Zugriffstaste _A1_ für `button2`, die dem Standardbereich angehört. In diesem Fall löst das System den Konflikt auf, indem die Zugriffstaste des ersten Elements verarbeitet wird, das der visuellen Struktur hinzugefügt wurde. Der Rest wird ignoriert.
 - Wenn derselbe Zugriffstastenbereich mehrere Berechnungsoptionen umfasst. Beispielsweise _A_ und _A1_. Wenn der Benutzer _A_ drückt, hat das System zwei Möglichkeiten: Zugriffstaste _A_ aufrufen oder das Zeichen A als ersten Teil der Zugriffstaste _A1_ interpretieren und warten. In diesem Fall verarbeitet das System nur den ersten vom Automaten erreichten Zugriffstastenaufruf. Im Beispiel mit _A_ und _A1_ ruft das System nur die Zugriffstaste _A_ auf.
-   Wenn der Benutzer in einer Zugriffstastenabfolge einen ungültigen Zugriffstastenwert drückt, passiert nichts. Zwei Kategorien von Zugriffstasten werden in einer Zugriffstastenabfolge als gültig angesehen:
 - Sondertasten zum Beenden der Zugriffstastenabfolge: ESC, ALT, EINGABE, TAB und die Pfeiltasten.
 - Die den Zugriffstasten zugewiesenen alphanumerischen Zeichen.

## Zugriffstasten-APIs

Das XAML-Framework bietet die hier beschriebenen APIs, um die Benutzerinteraktion mit Zugriffstasten zu unterstützen.

**AccessKeyManager**

[AccessKeyManager](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.aspx) ist eine Hilfsklasse, mit der Sie die Benutzeroberfläche verwalten können, wenn Zugriffstasten angezeigt oder ausgeblendet werden. Das [IsDisplayModeEnabledChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.isdisplaymodeenabledchanged.aspx)-Ereignis wird jedes Mal ausgelöst, wenn die Zugriffstastenabfolge in der App initiiert und beendet wird. Sie können die [IsDisplayModeEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.isdisplaymodeenabled.aspx)-Eigenschaft abfragen, um zu bestimmen, ob visuelle Angebote ein- oder ausgeblendet werden.  Sie können auch [ExitDisplayMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.exitdisplaymode.aspx) aufrufen, um das Schließen einer Zugriffstastenabfolge zu erzwingen.

> [!NOTE]
> Es gibt keine integrierte Implementierung visueller Elemente für Zugriffstasten. Diese müssen von Ihnen bereitgestellt werden.  

**AccessKey**

Mit der [AccessKey](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskey.aspx)-Eigenschaft können Sie eine Zugriffstaste für UIElement oder [TextElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.accesskey.aspx) angeben. Wenn zwei Elemente über dieselbe Zugriffstaste und denselben Bereich verfügen, wird nur das erste Element verarbeitet, das der visuellen Struktur hinzugefügt wurde.

Um sicherzustellen, dass die Zugriffstasten vom XAML-Framework verarbeitet werden, müssen die UI-Elemente in der visuellen Struktur erkannt werden. Wenn die visuelle Struktur keine Elemente mit einer Zugriffstaste enthält, werden keine Zugriffstastenereignisse ausgelöst.

Zugriffstasten-APIs unterstützen keine Zeichen, die mit zwei Tastenanschlägen erzeugt werden müssen. Ein einzelnes Zeichen muss einer Taste im systemeigenen Tastaturlayout der jeweiligen Sprache entsprechen.  

**AccessKeyDisplayRequested/Dismissed**

Das [AccessKeyDisplayRequested](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeydisplayrequested.aspx)-Ereignis und das[AccessKeyDisplayDismissed](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeydisplaydismissed.aspx)-Ereignis werden ausgelöst, wenn ein visuelles Angebot für eine Zugriffstaste angezeigt oder geschlossen werden soll. Für Elemente, deren [Visibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.visibility.aspx)-Eigenschaft auf **Collapsed** festgelegt ist, werden diese Ereignisse nicht ausgelöst. Das AccessKeyDisplayRequested-Ereignis wird während einer Zugriffstastenabfolge jedes Mal ausgelöst, wenn der Benutzer ein von der Zugriffstaste verwendetes Zeichen drückt. Beispiel: Wenn eine Zugriffstaste auf _AB_ festgelegt ist, wird dieses Ereignis einmal ausgelöst, wenn der Benutzer ALT drückt, und ein zweites Mal, wenn er _A_ drückt. Wenn der Benutzer _B_ drückt, wird das AccessKeyDisplayDismissed-Ereignis ausgelöst.

**AccessKeyInvoked**

Das [AccessKeyInvoked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeyinvoked.aspx)-Ereignis wird ausgelöst, wenn ein Benutzer das letzte Zeichen einer Zugriffstaste erreicht. Eine Zugriffstaste kann ein oder mehrere Zeichen umfassen. Beispiel: Für die Zugriffstasten _A_ und _BC_ wird das Ereignis ausgelöst, wenn ein Benutzer _ALT, A_ oder _ALT, B, C_ drückt. Wenn der Benutzer nur _ALT, B_ drückt, wird es jedoch nicht ausgelöst. Dieses Ereignis wird ausgelöst, wenn die Taste gedrückt und nicht, wenn sie losgelassen wird.

**IsAccessKeyScope**

Mit der [IsAccessKeyScope](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.isaccesskeyscope.aspx)-Eigenschaft können Sie angeben, dass ein UIElement das Stammelement eines Zugriffstastenbereichs ist. Das AccessKeyDisplayRequested-Ereignis wird für dieses Element, aber nicht für seine untergeordneten Elemente ausgelöst. Wenn ein Benutzer dieses Element aufruft, ändert das XAML-Framework automatisch den Bereich und löst das AccessKeyDisplayRequested-Ereignis für die untergeordneten Elemente und das AccessKeyDisplayDismissed-Ereignis für andere UI-Elemente (einschließlich des übergeordneten Elements) aus.  Die Zugriffstastenabfolge wird bei einer Änderung des Bereichs nicht beendet.

**AccessKeyScopeOwner**

Wenn ein Element dem Bereich eines anderen Elements (Quelle) angehören soll, das ihm in der visuellen Struktur nicht übergeordnet ist, können Sie die [AccessKeyScopeOwner](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeyscopeowner.aspx)-Eigenschaft festlegen. Bei dem an die AccessKeyScopeOwner-Eigenschaft gebundenen Element muss IsAccessKeyScope auf **true** festgelegt sein. Andernfalls wird eine Ausnahme ausgelöst.

**ExitDisplayModeOnAccessKeyInvoked**

Wenn eine Zugriffstaste aufgerufen wird und das Element kein Bereichsbesitzer ist, wird standardmäßig die Zugriffstastenabfolge abgeschlossen, und das [AccessKeyManager.IsDisplayModeEnabledChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.isdisplaymodeenabledchanged.aspx)-Ereignis wird ausgelöst. Sie können die [ExitDisplayModeOnAccessKeyInvoked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.exitdisplaymodeonaccesskeyinvoked.aspx)-Eigenschaft auf **false** festlegen, um dieses Verhalten zu überschreiben und zu verhindern, dass die Zugriffstastenabfolge nach dem Aufrufen beendet wird. (Diese Eigenschaft gilt sowohl für [UIElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.exitdisplaymodeonaccesskeyinvoked.aspx) als auch für [TextElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.exitdisplaymodeonaccesskeyinvoked.aspx).)

> [!NOTE]
> Wenn das Element ein Bereichsbesitzer (`IsAccessKeyScope="True"`) ist, wechselt die App in einen neuen Zugriffstastenbereich, und das IsDisplayModeEnabledChanged-Ereignis wird nicht ausgelöst.

**Lokalisierung**

Zugriffstasten können in mehrere Sprachen lokalisiert und zur Laufzeit mit den [ResourceLoader](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.resources.resourceloader.aspx)-APIs geladen werden.

## Beim Aufrufen einer Zugriffstaste verwendete Steuerungsmuster

Steuerungsmuster sind Schnittstellenimplementierungen, die allgemeine Steuerungsfunktionen verfügbar machen. Schaltflächen implementieren z. B. das **Invoke**-Steuerungsmuster, das das **Click**-Ereignis auslöst. Wenn eine Zugriffstaste aufgerufen wird, überprüft das XAML-Framework, ob das aufgerufene Element ein Steuerungsmuster implementiert, und führt es ggf. aus. Wenn das Element mehrere Steuerungsmuster aufweist, wird nur eins aufgerufen, und die übrigen werden ignoriert. Steuerungsmuster werden in der folgenden Reihenfolge gesucht:

1.  „Invoke“. Beispielsweise eine Schaltfläche.
2.  „Toggle“. Beispielsweise ein Kontrollkästchen.
3.  „Selection“. Beispielsweise RadioButton.
4.  „Expand/Collapse“. Beispielsweise ComboBox.

Falls ein Steuerungsmuster nicht gefunden wird, wird der Zugriffstastenaufruf als „No-Op“ angegeben. Außerdem wird eine mit der folgenden vergleichbare Debugmeldung aufgezeichnet, um Sie beim Debuggen des Problems zu unterstützen: „Es wurden keine Automatisierungsmuster für diese Komponente gefunden. Implementieren Sie das gewünschte Verhalten im Ereignishandler für AccessKeyInvoked. Wenn Sie „Handled“ im Ereignishandler auf „true“ festlegen, wird diese Meldung unterdrückt.“

> [!NOTE]
> Der Anwendungsprozesstyp des Debuggers muss in den Debugeinstellungen von Visual Studio _Gemischt (verwaltet und systemeigen)_ oder _Systemeigen_ lauten, damit diese Meldung angezeigt wird.

Wenn eine Zugriffstaste nicht ihr standardmäßiges Steuerungsmuster ausführen soll oder wenn das Element kein Steuerungsmuster aufweist, sollten Sie das [AccessKeyInvoked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeyinvoked.aspx)-Ereignis behandeln und das gewünschte Verhalten implementieren.
```csharp
private void OnAccessKeyInvoked(UIElement sender, AccessKeyInvokedEventArgs args)
{
    args.Handled = true;
    //Do something
}
```

Weitere Informationen über Steuerungsmuster finden Sie unter [Übersicht über Steuerungsmuster der Benutzeroberflächenautomatisierung](https://msdn.microsoft.com/library/windows/desktop/ee671194.aspx).

## Zugriffstasten und Sprachausgabe

Windows-Runtime verfügt über Benutzeroberflächenautomatisierungs-Anbieter, die Eigenschaften in Microsoft-Benutzeroberflächenautomatisierungs-Elementen verfügbar machen. Mithilfe dieser Eigenschaften können Benutzeroberflächenautomatisierungs-Clientanwendungen Informationen zu Teilen der Benutzeroberfläche ermitteln. Mithilfe der [AutomationProperties.AccessKey](https://msdn.microsoft.com/library/windows/apps/hh759763)-Eigenschaft können Clients wie die Sprachausgabe die einem Element zugeordnete Zugriffstaste ermitteln. Die Sprachausgabe liest diese Eigenschaft jedes Mal, wenn ein Element den Fokus erhält. Wenn AutomationProperties.AccessKey keinen Wert aufweist, gibt das XAML-Framework den [AccessKey](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskey.aspx)-Eigenschaftswert von UIElement oder TextElement zurück. Sie müssen AutomationProperties.AccessKey nicht einrichten, wenn die AccessKey-Eigenschaft bereits über einen Wert verfügt.

## Beispiel: Zugriffstaste für eine Schaltfläche

Dieses Beispiel veranschaulicht, wie Sie eine Zugriffstaste für eine Schaltfläche erstellen. Dabei werden QuickInfos als visuelle Angebote verwendet, um ein unverankertes Badge zu implementieren, das die Zugriffstaste enthält.

> [!NOTE]
> Der Einfachheit halber werden QuickInfos verwendet, es wird jedoch empfohlen, z. B. mit [Popup](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.popup.aspx) ein eigenes Steuerelement für die Anzeige zu erstellen.

Da der Handler für das Click-Ereignis automatisch vom XAML-Framework aufgerufen wird, müssen Sie das AccessKeyInvoked-Ereignis nicht behandeln. Im Beispiel werden mit der [AccessKeyDisplayRequestedEventArgs.PressedKeys](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeydisplayrequestedeventargs.pressedkeys.aspx)-Eigenschaft visuelle Angebote nur für die Zeichen bereitgestellt, die zum Aufrufen der Zugriffstaste noch gedrückt werden müssen. Wenn beispielsweise die drei Zugriffstasten _A1_, _A2_ und _C_ angezeigt werden und der Benutzer _A_ drückt, werden nur die Zugriffstasten _A1_ und _A2_ nicht herausgefiltert und als _1_ und _2_ anstatt als _A1_ und _A2_ angezeigt.

```xaml
<StackPanel
        VerticalAlignment="Center"
        HorizontalAlignment="Center"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Button Content="Press"
                AccessKey="PB"
                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"
                Click="DoSomething" />
        <TextBlock Text="" x:Name="textBlock" />
    </StackPanel>
```

```csharp
 public sealed partial class ButtonSample : Page
    {
        public ButtonSample()
        {
            this.InitializeComponent();
        }

        private void DoSomething(object sender, RoutedEventArgs args)
        {
            textBlock.Text = "Access Key is working!";
        }

        private void OnAccessKeyDisplayRequested(UIElement sender, AccessKeyDisplayRequestedEventArgs args)
        {
            var tooltip = ToolTipService.GetToolTip(sender) as ToolTip;

            if (tooltip == null)
            {
                tooltip = new ToolTip();
                tooltip.Background = new SolidColorBrush(Windows.UI.Colors.Black);
                tooltip.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
                tooltip.Padding = new Thickness(4, 4, 4, 4);
                tooltip.VerticalOffset = -20;
                tooltip.Placement = PlacementMode.Bottom;
                ToolTipService.SetToolTip(sender, tooltip);
            }

            if (string.IsNullOrEmpty(args.PressedKeys))
            {
                tooltip.Content = sender.AccessKey;
            }
            else
            {
                tooltip.Content = sender.AccessKey.Remove(0, args.PressedKeys.Length);
            }

            tooltip.IsOpen = true;
        }
        private void OnAccessKeyDisplayDismissed(UIElement sender, AccessKeyDisplayDismissedEventArgs args)
        {
            var tooltip = ToolTipService.GetToolTip(sender) as ToolTip;
            if (tooltip != null)
            {
                tooltip.IsOpen = false;
                //Fix to avoid show tooltip with mouse
                ToolTipService.SetToolTip(sender, null);
            }
        }
    }
```

## Beispiel: Bereichsbezogene Zugriffstasten

Dieses Beispiel veranschaulicht das Erstellen bereichsbezogener Zugriffstasten. Die IsAccessKeyScope-Eigenschaft von PivotItem verhindert, dass die Zugriffstasten der untergeordneten Elemente von PivotItem angezeigt werden, wenn der Benutzer die ALT-TASTE drückt. Diese Zugriffstasten werden nur angezeigt, wenn der Benutzer PivotItem aufruft, da der Bereich automatisch vom XAML-Framework gewechselt wird. Das Framework blendet auch die Zugriffstasten der anderen Bereiche aus.

Dieses Beispiel veranschaulicht auch, wie das AccessKeyInvoked-Ereignis behandelt wird. Da PivotItem kein Steuerungsmuster implementiert, ruft das XAML-Framework standardmäßig keine Aktion auf. Diese Implementierung zeigt, wie das mit der Zugriffstaste aufgerufene PivotItem ausgewählt wird.

Abschließend wird im Beispiel das IsDisplayModeChanged-Ereignis gezeigt, in dem Sie eine Aktion ausführen können, wenn sich der Anzeigemodus ändert. In diesem Beispiel wird das Pivot-Steuerelement reduziert, bis der Benutzer ALT drückt. Wenn der Benutzer seine Interaktion mit dem Pivot-Steuerelement beendet hat, wird es wieder reduziert. Mit IsDisplayModeEnabled können Sie überprüfen, ob der Anzeigemodus für Zugriffstasten aktiviert oder deaktiviert ist.

```xaml   
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Pivot x:Name="MyPivot" VerticalAlignment="Center" HorizontalAlignment="Center" >
            <Pivot.Items>
                <PivotItem
                    x:Name="PivotItem1"
                    AccessKey="A"
                    AccessKeyInvoked="OnAccessKeyInvoked"
                    AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                    AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"
                    IsAccessKeyScope="True">
                    <PivotItem.Header>
                        <TextBlock Text="A Options"/>
                    </PivotItem.Header>
                    <StackPanel Orientation="Horizontal" >
                        <Button Content="ButtonAA" AccessKey="A"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested" />
                        <Button Content="ButtonAD1" AccessKey="D1"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"  />
                        <Button Content="ButtonAD2" AccessKey="D2"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"/>
                    </StackPanel>
                </PivotItem>
                <PivotItem
                    x:Name="PivotItem2"
                    AccessKeyInvoked="OnAccessKeyInvoked"
                    AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                    AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"
                    AccessKey="B"
                    IsAccessKeyScope="true">
                    <PivotItem.Header>
                        <TextBlock Text="B Options"/>
                    </PivotItem.Header>
                    <StackPanel Orientation="Horizontal">
                        <Button AccessKey="B" Content="ButtonBB"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"  />
                        <Button AccessKey="F1" Content="ButtonBF1"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"  />
                        <Button AccessKey="F2" Content="ButtonBF2"  
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"/>
                    </StackPanel>
                </PivotItem>
            </Pivot.Items>
        </Pivot>
    </Grid>
```

```csharp
public sealed partial class ScopedAccessKeys : Page
    {
        public ScopedAccessKeys()
        {
            this.InitializeComponent();
            AccessKeyManager.IsDisplayModeEnabledChanged += OnDisplayModeEnabledChanged;
            this.Loaded += OnLoaded;
        }

        void OnLoaded(object sender, object e)
        {
            //To let the framework discover the access keys, the elements should be realized
            //on the visual tree. If there are no elements in the visual
            //tree with access key, the framework won't raise the events.
            //In this sample, if you define the Pivot as collapsed on the constructor, the Pivot
            //will have a lazy loading and the access keys won't be enabled.
            //For this reason, we make it visible when creating the object
            //and we collapse it when we load the page.
            MyPivot.Visibility = Visibility.Collapsed;
        }

        void OnAccessKeyInvoked(UIElement sender, AccessKeyInvokedEventArgs args)
        {
            args.Handled = true;
            MyPivot.SelectedItem = sender as PivotItem;
        }
        void OnDisplayModeEnabledChanged(object sender, object e)
        {
            if (AccessKeyManager.IsDisplayModeEnabled)
            {
                MyPivot.Visibility = Visibility.Visible;
            }
            else
            {
                MyPivot.Visibility = Visibility.Collapsed;

            }
        }

        DependencyObject AdjustTarget(UIElement sender)
        {
            DependencyObject target = sender;
            if (sender is PivotItem)
            {
                PivotItem pivotItem = target as PivotItem;
                target = (sender as PivotItem).Header as TextBlock;
            }
            return target;
        }

        void OnAccessKeyDisplayRequested(UIElement sender, AccessKeyDisplayRequestedEventArgs args)
        {
            DependencyObject target = AdjustTarget(sender);
            var tooltip = ToolTipService.GetToolTip(target) as ToolTip;

            if (tooltip == null)
            {
                tooltip = new ToolTip();
                tooltip.Background = new SolidColorBrush(Windows.UI.Colors.Black);
                tooltip.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
                tooltip.Padding = new Thickness(4, 4, 4, 4);
                tooltip.VerticalOffset = -20;
                tooltip.Placement = PlacementMode.Bottom;
                ToolTipService.SetToolTip(target, tooltip);
            }

            if (string.IsNullOrEmpty(args.PressedKeys))
            {
                tooltip.Content = sender.AccessKey;
            }
            else
            {
                tooltip.Content = sender.AccessKey.Remove(0, args.PressedKeys.Length);
            }

            tooltip.IsOpen = true;
        }
        void OnAccessKeyDisplayDismissed(UIElement sender, AccessKeyDisplayDismissedEventArgs args)
        {
            DependencyObject target = AdjustTarget(sender);

            var tooltip = ToolTipService.GetToolTip(target) as ToolTip;
            if (tooltip != null)
            {
                tooltip.IsOpen = false;
                //Fix to avoid show tooltip with mouse
                ToolTipService.SetToolTip(target, null);
            }
        }
    }
```



<!--HONumber=Aug16_HO3-->


