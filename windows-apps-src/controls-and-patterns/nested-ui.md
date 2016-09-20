---
author: Jwmsft
Description: "Benutze eine geschachtelte UI, um mehrere Aktionen über Listenelementen zu ermöglichen"
title: Geschachtelte UI bei Listenelementen
label: Nested UI in list items
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 37f47db0d7085bf61836ed3fc9ccdc03470f58da

---
# Geschachtelte UI bei Listenelementen

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Eine geschachtelte UI ist eine Benutzeroberfläche (User Interface, UI) mit geschachtelten Steuerelementen, die in einem Container eingeschlossen sind und auch unabhängig den Fokus erhalten kann.

Sie können dem Benutzer mit geschachtelten UIs weitere Optionen zur Verfügung stellen, mit denen sie wichtige Aktionen schneller ausführen können. Bedenken Sie jedoch, dass die Benutzeroberfläche komplizierter wird, je mehr Aktionen Sie anbieten. Wenn Sie diese Art von Benutzeroberfläche verwenden, sollten Sie besonders vorsichtig vorgehen. Dieser Artikel enthält Richtlinien, um die beste Vorgehensweise für Ihre UI zu ermitteln.

In diesem Artikel erläutern wir die Erstellung von geschachtelten UIs in [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)- und in [GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)-Elementen. In diesem Abschnitt werden andere Formen von geschachtelten UIs nicht berücksichtigt, aber die hier gegebenen Konzepte lassen sich auf andere UIs übertragen. Bevor Sie beginnen, sollten Sie mit der allgemeinen Richtlinie für die Verwendung von ListView- und GridView-Steuerelementen in der Benutzeroberfläche vertraut sein, die in den Artikeln [Listen](lists.md) und [Listenansicht und Rasteransicht](listview-and-gridview.md) beschrieben wird.

In diesem Artikel verwenden wir die Begriffe *Liste*, *Listenelement* und *geschachtelte UI* entsprechend den folgenden Definitionen:
- *Liste* bezeichnet eine Sammlung von Elementen in einer Listenansicht oder in einer Rasteransicht.
- *Listenelement* bezeichnet ein einzelnes Element in einer Liste, für das der Benutzer bestimmte Aktionen ausführen kann.
- *Geschachtelte UI* bezeichnet UI-Elemente in einem Listenelement, für die der Benutzer bestimmte Aktionen ausführen kann, die sich nicht auf das Listenelement selbst auswirken.

![Komponenten von geschachtelten UIs](images/nested-ui-example-1.png)

> Hinweis:&nbsp;&nbsp; ListView und GridView sind beide von der Klasse [ListViewBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.aspx) abgeleitet, sodass sie dieselbe Funktionalität haben, sie zeigen die Daten jedoch unterschiedlich an. In diesem Artikel beziehen sich Aussagen zu Listen sowohl auf die ListView- als auch die GridView-Steuerelemente, wenn nicht anders angegeben.

## Primäre und sekundäre Aktionen

Beim Erstellen einer UI mit einer Liste sollten Sie berücksichtigen, welche Aktionen Benutzer über den Elementen dieser Liste ausführen könnten.  

- Kann der Benutzer auf das Element klicken, um eine Aktion auszuführen?
    - In der Regel wird durch das Klicken auf ein Listenelement eine Aktion eingeleitet, dies ist jedoch nicht notwendigerweise so.
- Gibt es mehr als eine Aktion, die der Benutzer ausführen kann?
    - Beispiel: Wenn Sie in einer Liste von E-Mail-Nachrichten auf eine E-Mail tippen, wird diese geöffnet. Es kann jedoch noch andere Aktionen geben, die der Benutzer möglicherweise ausführen möchte, ohne die E-Mail vorher zu öffnen, beispielsweise die E-Mail löschen. Es wäre für den Benutzer hilfreich, auf diese Aktion direkt in der Liste zugreifen zu können.
- Wie sollten die Aktionen für den Benutzer verfügbar gemacht werden?
    - Berücksichtigen Sie alle Arten von Eingabemöglichkeiten. Manche Formen von geschachtelten UIs eignen sich hervorragend für eine Eingabemethode, funktionieren jedoch möglicherweise nicht mit anderen Methoden.  

Die *primäre Aktion* ist das, was der Benutzer beim Klicken auf ein Element der Liste erwartet.

*Sekundäre Aktionen* sind in der Regel Abkürzungen für Aktionen über Listenelementen. Diese Abkürzungen können zur Verwaltung der Liste dienen oder Aktionen in Zusammenhang mit dem Listenelement aufrufen.

## Optionen für sekundäre Aktionen

Wenn Sie eine Listen-UI erstellen, müssen Sie zunächst sicherstellen, dass Sie alle Eingabemethoden berücksichtigen, die UWP unterstützt. Weitere Informationen zu verschiedenen Arten von Eingaben finden Sie unter [Einführung in Eingaben](../input-and-devices/input-primer.md).

Nachdem Sie sichergestellt haben, dass Ihre App alle Eingaben unterstützt, die UWP unterstützt, sollten Sie entscheiden, ob die sekundären Aktionen Ihrer App wichtig genug sind, um sie als Abkürzungen in der Hauptliste verfügbar zu machen. Bedenken Sie, dass Ihre Benutzeroberfläche mit jeder weiteren Aktion, die Sie zur Verfügung stellen, komplizierter wird. Möchten Sie die sekundären Aktionen wirklich in der Hauptliste der Benutzeroberfläche verfügbar machen, oder können Sie sie an einer anderen Stelle platzieren?

Wenn Sie weitere Aktionen in die Hauptlisten-UI aufnehmen möchten, sollten dies Aktionen sein, die jederzeit für alle Eingaben zugänglich sein müssen.

Wenn Sie entscheiden, dass es nicht erforderlich ist, in der Hauptlisten-UI sekundäre Aktionen aufzunehmen, gibt es verschiedene andere Methoden, über die Sie diese für den Benutzer verfügbar machen können. Nachfolgend finden Sie einige Möglichkeiten, sekundäre Aktionen an anderen Stellen zu platzieren.

### Platzieren der sekundären Aktionen auf der Detailseite

Platzieren Sie die sekundären Aktionen auf der Seite, zu der das Listenelement navigiert, wenn es ausgewählt wird. Wenn Sie das Master/Details-Muster verwenden, ist die Detailseite häufig eine gute Stelle für sekundäre Aktionen.

Weitere Informationen finden Sie unter [Master/Details-Muster](master-details.md).

### Platzieren der sekundären Aktionen in einem Kontextmenü

Platzieren Sie die sekundären Aktionen in einem Kontextmenü, auf das der Benutzer mit der rechten Maustaste oder durch Drücken und Halten zugreifen kann. Dies bietet den Vorteil, dass der Benutzer eine Aktion ausführen kann, ohne die Detailseite laden zu müssen, z. B. eine E-Mail löschen. Es ist eine empfohlene Vorgehensweise, diese Optionen außerdem auch auf der Detailseite zur Verfügung zu stellen, da Kontextmenüs als Abkürzung gedacht sind, für die die primäre UI tatsächlich an anderer Stelle vorhanden ist.

Um sekundäre Aktionen verfügbar zu machen, wenn die Eingabe über ein Gamepad oder eine Fernbedienung erfolgt, empfehlen wir, ein Kontextmenü zu verwenden.

Weitere Informationen finden Sie in den [Kontextmenüs und Flyouts](menus.md).

### Optimiertes Platzieren der sekundären Aktionen für die Zeigereingabe in Hover-UIs

Wenn Ihre App benutzerfreundlich für Zeigereingaben wie Maus und Stift sein soll, und Sie sekundäre Aktionen nur für diese Eingabemethoden verfügbar machen möchten, können Sie die sekundären Aktionen durch Zeigeeffekte anzeigen. Diese Abkürzung wird nur angezeigt, wenn eine Zeigereingabe verwendet wird. Sie müssen daher sicherstellen, dass für andere Eingabetypen ebenfalls eine Möglichkeit verfügbar ist.

![Geschachtelte UI, die beim Draufzeigen angezeigt wird](images/nested-ui-hover.png)


Weitere Informationen finden Sie unter [Mausinteraktionen](../input-and-devices/mouse-interactions.md).

## Platzierung der UI für primäre und sekundäre Aktionen

Wenn Sie entscheiden, dass sekundäre Aktionen über die Hauptlisten-UI verfügbar gemacht werden sollen, empfehlen wir die folgenden Richtlinien.

Beim Erstellen eines Listenelements mit primären und sekundären Aktionen platzieren Sie die primäre Aktion auf der linken und sekundäre Aktionen auf der rechten Seite. In Kulturen mit Leserichtung von links nach rechts identifizieren Benutzer Aktionen auf der linken Seite des Listenelements eher als primäre Aktion.

In der Listen-UI in diesen Beispielen ist der Fluss der Elemente tendenziell horizontal (die UI ist eher breit als hoch). Es besteht dennoch die Möglichkeit, dass einzelne Listenelemente in ihrer Form eher quadratisch sind (höher als breit). Solche Elemente werden in der Regel in einem Raster verwendet. Wenn die Liste bei diesen Elementen keinen vertikalen Bildlauf bietet, können Sie sekundäre Aktionen statt rechts neben dem Element darunter platzieren.

## Berücksichtigen aller Eingaben

Bei der Entscheidung, eine geschachtelte UI zu verwenden, sollten Sie auch die Benutzerfreundlichkeit bei allen Eingabetypen evaluieren. Wie bereits erwähnt eignen sich geschachtelte UIs gut für bestimmte Eingabetypen. Leider gibt es andere Eingabetypen, für die sie nicht so gut geeignet sind. Insbesondere problematisch ist der Zugriff auf geschachtelte UI-Elemente über Tastatur, Controller und bei Eingabe über eine Fernbedienung. Folge unbedingt den Richtlinien unten, um sicherzustellen, dass Ihre UWP-App mit allen Eingabetypen verwendet werden kann.

## Behandeln von geschachtelten UIs

Wenn mehr als eine Aktion im Listenelement geschachtelt ist, empfehlen wir diese Richtlinien, um die Navigation über Tastatur, Gamepad, Fernbedienung und andere nicht zeigerorientierte Eingabetypen zu behandeln.

### Geschachtelte UI, deren Elemente eine Aktion ausführen

Wenn Ihre Listen-UI mit geschachtelten Elementen Aktionen unterstützt wie Aufrufen, Auswählen (Einzel- oder Mehrfachauswahl) oder Ziehen und Ablegen, empfehlen wir die folgenden pfeilorientierten Verfahren zum Navigieren in geschachtelten UI-Elementen.

![Komponenten von geschachtelten UIs](images/nested-ui-navigation.png)

**Gamepad**

Leiten Sie den Benutzer bei der Eingabe über ein Gamepad wie folgt:

- Von **A** aus übergibt die Richtungstaste nach rechts den Fokus an **B**.
- Von **B** aus übergibt die Richtungstaste nach rechts den Fokus an **C**.
- Von **C** aus übergibt die Richtungstaste nach rechts den Fokus an ein fokussierbares UI-Element rechts davon, falls ein solches Element vorhanden ist, ansonsten ist der Taste keine Aktion zugeordnet.
- Von **C** aus übergibt die Richtungstaste nach links den Fokus an **B**.
- Von **B** aus übergibt die Richtungstaste nach links den Fokus an **A**.
- Von **A** aus übergibt die Richtungstaste nach links den Fokus an ein fokussierbares UI-Element rechts davon, falls ein solches Element vorhanden ist, ansonsten ist der Taste keine Aktion zugeordnet.
- Von **A**, **B** oder **C** aus übergibt die Richtungstaste nach unten den Fokus an **D**.
- Von einem UI-Element links neben dem Listenelement aus übergibt die Richtungstaste nach rechts den Fokus an **A**.
- Von einem UI-Element rechts neben dem Listenelement übergibt die Richtungstaste nach links den Fokus an **A**.

**Tastatur**

Bei Eingabe über eine Tastatur sollte der Benutzer wie folgt geleitet werden:

- Von **A** aus übergibt die Tabulatortaste den Fokus an **B**.
- Von **B** aus übergibt die Tabulatortaste den Fokus an **C**.
- Von **C** aus übergibt die Tabulatortaste den Fokus an das nächste fokussierbare UI-Element in der Aktivierreihenfolge.
- Von **C** aus übergibt die Tastenkombination Umschalt+Tabulatortaste den Fokus an **B**.
- Von **B** aus übergeben die Tastenkombination Umschalt+Tabulatortaste und die Pfeiltaste nach links den Fokus an **A**.
- Von **A** aus übergibt die Tastenkombination Umschalt+Tabulatortaste den Fokus an das nächste fokussierbare UI-Element in der umgekehrten Aktivierreihenfolge.
- Von **A**, **B** oder **C** aus übergibt die Pfeiltaste nach unten den Fokus an **D**.
- Von einem UI-Element links neben dem Listenelement aus übergibt die Tabulatortaste den Fokus an **A**.
- Von UI-Element rechts neben dem Listenelement aus übergibt die Tastenkombination Umschalt+Tabulatortaste den Fokus an **C**.

Um diese UI so zur Verfügung zu stellen, legen Sie [IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx) für Ihre Liste auf **true** fest. [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx) kann ein beliebiger Wert sein.

Der Code zur Implementierung dieses Verhaltens finden Sie im Abschnitt [Beispiel](#example) dieses Artikels.

### Geschachtelte UI, deren Listenelemente keine Aktion ausführen

Sie können eine Listenansicht auch verwenden, weil sie die Inhalte visuell optimiert darstellt und ein bestimmtes Bildlaufverhalten bietet, ohne dass den Listenelementen eine Aktion zugeordnet ist. Diese Benutzeroberflächen verwenden die Listendarstellung, um Elemente zu gruppieren und sicherzustellen, dass alle Listenelemente beim Bildlauf gemeinsam bewegt werden.

Diese Art von UI ist sehr viel komplizierter als die vorherigen Beispiele, es sind zahlreiche geschachtelte Elemente vorhanden, über die der Benutzer Aktionen ausführen kann.

![Komponenten von geschachtelten UIs](images/nested-ui-grouping.png)


Um diese UI so zur Verfügung zu stellen, legen Sie die folgenden Eigenschaften in der Liste fest:
- [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx) auf **None**.
- [IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx) auf **false**.
- [IsFocusEngagementEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isfocusengagementenabled.aspx) auf **true**.

```xaml
<ListView SelectionMode="None" IsItemClickEnabled="False" >
    <ListView.ItemContainerStyle>
         <Style TargetType="ListViewItem">
             <Setter Property="IsFocusEngagementEnabled" Value="True"/>
         </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

Wenn die Listenelemente keine Aktion ausführen, empfehlen wir die folgenden Richtlinien, um die Navigation mit einem Gamepad oder Tastatur zu behandeln.

**Gamepad**

Leiten Sie den Benutzer bei der Eingabe über ein Gamepad wie folgt:

- Von einem Listenelement aus übergibt die Richtungstaste nach unten den Fokus an das nächste Listenelement.
- Von einem Listenelement aus übergibt die Richtungstaste nach links bzw. rechts den Fokus an ein fokussierbares UI-Element rechts bzw. links davon, falls ein solches Element vorhanden ist, ansonsten ist der Taste keine Aktion zugeordnet.
- Von einem Listenelement aus übergibt die Taste „A“ den Fokus an das geschachtelte Listenelement in der Reihenfolge oben/unten und links/rechts.
- Folgen Sie in der geschachtelten UI dem XY-Fokusnavigationsmodell.  Der Fokus kann die geschachtelte UI in dem aktuellen Listenelement erst verlassen, wenn der Benutzer die Taste „B“ drückt, die den Fokus an das Listenelement zurückgibt.

**Tastatur**

Bei Eingabe über eine Tastatur sollte der Benutzer wie folgt geleitet werden:

- Von einem Listenelement aus übergibt die Pfeiltaste nach unten den Fokus an das nächste Listenelement.
- In dem Listenelement ist der Pfeiltaste nach links bzw. rechts keine Aktion zugeordnet.
- Von einem Listenelement aus übergibt die Tabulatortaste den Fokus an das nächste fokussierbare Element in dem geschachtelten UI-Element.
- Von einem der geschachtelten UI-Elemente aus durchläuft der Fokus beim Drücken der Tabulatortaste die geschachtelte UI in der Aktivierreihenfolge.  Nachdem alle Elemente der geschachtelten UI durchlaufen wurden, erhält den Fokus das nächste Steuerelement, das in der Aktivierreihenfolge auf ListView folgt.
- Bei der Tastenkombination Umschalt+Tabulatortaste wird das Aktivierungsverhalten umgekehrt.

## Beispiel

Dieses Beispiel zeigt, wie Sie eine [geschachtelte UI implementieren, in denen Listenelemente eine Aktion ausführen](#nested-ui-where-list-items-perform-an-action).

```xaml
<ListView SelectionMode="None" IsItemClickEnabled="True"
          ChoosingItemContainer="listview1_ChoosingItemContainer"/>
```

```csharp
private void OnListViewItemKeyDown(object sender, KeyRoutedEventArgs e)
{
    // Code to handle going in/out of nested UI with gamepad and remote only.
    if (e.Handled == true)
    {
        return;
    }

    var focusedElementAsListViewItem = FocusManager.GetFocusedElement() as ListViewItem;
    if (focusedElementAsListViewItem != null)
    {
        // Focus is on the ListViewItem.
        // Go in with Right arrow.
        Control candidate = null;

        switch (e.OriginalKey)
        {
            case Windows.System.VirtualKey.GamepadDPadRight:
            case Windows.System.VirtualKey.GamepadLeftThumbstickRight:
                var rawPixelsPerViewPixel = DisplayInformation.GetForCurrentView().RawPixelsPerViewPixel;
                GeneralTransform generalTransform = focusedElementAsListViewItem.TransformToVisual(null);
                Point startPoint = generalTransform.TransformPoint(new Point(0, 0));
                Rect hintRect = new Rect(startPoint.X * rawPixelsPerViewPixel, startPoint.Y * rawPixelsPerViewPixel, 1, focusedElementAsListViewItem.ActualHeight * rawPixelsPerViewPixel);
                candidate = FocusManager.FindNextFocusableElement(FocusNavigationDirection.Right, hintRect) as Control;
                break;
        }

        if (candidate != null)
        {
            candidate.Focus(FocusState.Keyboard);
            e.Handled = true;
        }
    }
    else
    {
        // Focus is inside the ListViewItem.
        FocusNavigationDirection direction = FocusNavigationDirection.None;
        switch (e.OriginalKey)
        {
            case Windows.System.VirtualKey.GamepadDPadUp:
            case Windows.System.VirtualKey.GamepadLeftThumbstickUp:
                direction = FocusNavigationDirection.Up;
                break;
            case Windows.System.VirtualKey.GamepadDPadDown:
            case Windows.System.VirtualKey.GamepadLeftThumbstickDown:
                direction = FocusNavigationDirection.Down;
                break;
            case Windows.System.VirtualKey.GamepadDPadLeft:
            case Windows.System.VirtualKey.GamepadLeftThumbstickLeft:
                direction = FocusNavigationDirection.Left;
                break;
            case Windows.System.VirtualKey.GamepadDPadRight:
            case Windows.System.VirtualKey.GamepadLeftThumbstickRight:
                direction = FocusNavigationDirection.Right;
                break;
            default:
                break;
        }

        if (direction != FocusNavigationDirection.None)
        {
            Control candidate = FocusManager.FindNextFocusableElement(direction) as Control;
            if (candidate != null)
            {
                ListViewItem listViewItem = sender as ListViewItem;

                // If the next focusable candidate to the left is outside of ListViewItem,
                // put the focus on ListViewItem.
                if (direction == FocusNavigationDirection.Left &&
                    !listViewItem.IsAncestorOf(candidate))
                {
                    listViewItem.Focus(FocusState.Keyboard);
                }
                else
                {
                    candidate.Focus(FocusState.Keyboard);
                }
            }

            e.Handled = true;
        }
    }
}

private void listview1_ChoosingItemContainer(ListViewBase sender, ChoosingItemContainerEventArgs args)
{
    if (args.ItemContainer == null)
    {
        args.ItemContainer = new ListViewItem();
        args.ItemContainer.KeyDown += OnListViewItemKeyDown;
    }
}
```

```csharp
// DependencyObjectExtensions.cs definition.
public static class DependencyObjectExtensions
{
    public static bool IsAncestorOf(this DependencyObject parent, DependencyObject child)
    {
        DependencyObject current = child;
        bool isAncestor = false;

        while (current != null && !isAncestor)
        {
            if (current == parent)
            {
                isAncestor = true;
            }

            current = VisualTreeHelper.GetParent(current);
        }

        return isAncestor;
    }
}
```



<!--HONumber=Aug16_HO3-->


