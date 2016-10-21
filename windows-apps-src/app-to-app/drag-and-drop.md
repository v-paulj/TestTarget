---
description: "In diesem Artikel erfahren Sie, wie Sie Ihrer UWP-App (Universelle Windows-Plattform) Drag & Drop hinzufügen."
title: Drag & Drop
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: f2133ca15e30f7451a61f78b48e883db1a5687a6
ms.openlocfilehash: ee3d0c40effc12382f6fd31154016953f172be70

---
# Drag & Drop

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


In diesem Artikel erfahren Sie, wie Sie Ihrer Universellen Windows-Plattform (UWP)-App Drag&Drop hinzufügen. Drag&Drop ist ein klassisches, natürliches Interaktionsmodell für Inhalte wie Bilder und Dateien. Nach der Implementierung stehen Drag & Drop-Vorgänge für sämtliche Richtungen (App zu App, App zu Desktop und Desktop zu App) zur Verfügung.

## Festlegen gültiger Bereiche

Verwenden Sie die Eigenschaften [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) und [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag), um die Bereiche der App festzulegen, die für Drag & Drop infrage kommen.

Das folgende Markup veranschaulicht, wie Sie mithilfe von [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) in XAML einen bestimmten Bereich der App als gültig für Dropvorgänge festlegen. Wenn ein Benutzer versucht, Elemente an anderer Stelle abzulegen, wird dies vom System nicht zugelassen. Wenn Benutzer die Möglichkeit haben sollen, Elemente an einer beliebigen Stelle Ihrer App abzulegen, legen Sie den gesamten Hintergrund als Dropziel fest.

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDropArea)]

Beim Ziehen sollten Sie in der Regel genau festlegen, was gezogen werden kann. Benutzer möchten meist nicht alle Elemente Ihrer App ziehen, sondern nur bestimmte Elemente wie beispielsweise Bilder. Nachfolgend sehen Sie, wie Sie [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag) mithilfe von XAML festlegen.

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDragArea)]

Sie müssen keine weiteren Aktionen durchführen, um das Ziehen zu ermöglichen, es sei denn, Sie möchten die Benutzeroberfläche anpassen (dies wird später in diesem Artikel behandelt). Das Ablegen erfordert einige weitere Schritte.

## Verarbeiten des DragOver-Ereignisses

Das [**DragOver**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DragOver)-Ereignis wird ausgelöst, wenn ein Benutzer ein Element über Ihre App gezogen, aber noch nicht abgelegt hat. In diesem Handler müssen Sie mithilfe der [**AcceptedOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.AcceptedOperation)-Eigenschaft angeben, welche Art von Vorgängen Ihre App unterstützt. Kopieren ist der häufigste Vorgang.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOver)]

## Verarbeiten des Dropereignisses

Das [**Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop)-Ereignis tritt auf, wenn der Benutzer Elemente in einem gültigen Dropbereich ablegt. Verarbeiten Sie dieses Ereignis mit der [**DataView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DataView)-Eigenschaft.

Der Einfachheit halber wird im folgenden Beispiel angenommen, dass der Benutzer ein einzelnes Foto abgelegt hat und auf dieses zugreift. In der Praxis können Benutzer mehrere Elemente unterschiedlicher Formate gleichzeitig ablegen. Ihre App sollte diese Möglichkeit behandeln, indem sie überprüft, welche Dateitypen abgelegt wurden, sie entsprechend verarbeitet und den Benutzer benachrichtigt, wenn er versucht, eine von der App nicht unterstützte Aktion auszuführen.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_Drop)]

## Anpassen der Benutzeroberfläche

Das System bietet eine Standardbenutzeroberfläche für Drag&Drop. Sie können jedoch auch verschiedene Teile der Benutzeroberfläche anpassen, indem Sie benutzerdefinierte Beschriftungen und Symbole festlegen oder angeben, dass keine Benutzeroberfläche angezeigt werden soll. Verwenden Sie zum Anpassen der Benutzeroberfläche die [**DragEventArgs.DragUIOverride**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DragUIOverride)-Eigenschaft.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOverCustom)]

## Öffnen eines Kontextmenüs für ein Element, das per Toucheingabe gezogen werden kann

Bei der Toucheingabe erfordern das Ziehen eines [**UI-Elements**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement) und das Öffnen des Kontextmenüs ähnliche Touchgesten. Beide beginnen mit Drücken und Halten. Hier erfahren Sie, wie das System zwischen den beiden Aktionen für Elemente unterscheidet, die beide unterstützen: 

* Wenn der Benutzer ein Element drückt und hält und es innerhalb von 500Millisekunden zu ziehen beginnt, wird es gezogen. Das Kontextmenü wird nicht angezeigt. 
* Wenn der Benutzer drückt und hält, jedoch nicht innerhalb von 500Millisekunden zieht, wird das Kontextmenü geöffnet. 
* Wenn der Benutzer bei geöffnetem Kontextmenü versucht, das Element zu ziehen (ohne den Finger anzuheben), wird das Kontextmenü geschlossen und das Ziehen gestartet.

## Festlegen eines Elements in einer ListView oder GridView als Ordner

Sie können ein [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ListViewItem) oder [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.GridViewItem) als Ordner festlegen. Dies ist besonders hilfreich für TreeView- und Datei-Explorer-Szenarien. Setzen Sie hierzu für das Element die [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop)-Eigenschaft auf **True**. 

Das System zeigt automatisch die entsprechenden Animationen zum Ablegen in einen Ordner anstelle eines Nicht-Ordner-Elements an. Der App-Code muss für das Ordnerelement (sowie das Nicht-Ordner-Element) auch weiterhin das Ereignis [**Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop) beinhalten, damit die Datenquelle aktualisiert und das abgelegte Element im Zielordner hinzugefügt wird.

## Weitere Informationen

* [App-zu-App-Kommunikation](index.md)
* [AllowDrop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx)
* [CanDrag](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx)
* [DragOver](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx)
* [AcceptedOperation](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.acceptedoperation.aspx)
* [DataView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.dataview.aspx)
* [DragUIOverride](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.draguioverride.aspx)
* [Drop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx)
* [IsDragSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isdragsource.aspx)



<!--HONumber=Aug16_HO3-->


