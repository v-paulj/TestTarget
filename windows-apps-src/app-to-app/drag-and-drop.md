---
description: In diesem Artikel erfahren Sie, wie Sie Ihrer UWP-App (Universelle Windows-Plattform) Drag & Drop hinzufügen.
title: Drag & Drop
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
author: awkoren
---
# Drag & Drop

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


In diesem Artikel erfahren Sie, wie Sie Ihrer Universellen Windows-Plattform (UWP)-App Drag & Drop hinzufügen. Drag & Drop ist ein klassisches, natürliches Interaktionsmodell für Inhalte wie Bilder und Dateien. Nach der Implementierung stehen Drag & Drop-Vorgänge für sämtliche Richtungen (App zu App, App zu Desktop und Desktop zu App) zur Verfügung.

## Festlegen gültiger Bereiche

Verwenden Sie die Eigenschaften [**AllowDrop**][AllowDrop] und [**CanDrag**][CanDrag], um die Bereiche der App festzulegen, die für Drag & Drop infrage kommen.

Das folgende Markup veranschaulicht, wie Sie mithilfe von [**AllowDrop**][AllowDrop] in XAML einen bestimmten Bereich der App als gültig für Dropvorgänge festlegen. Wenn ein Benutzer versucht, Elemente an anderer Stelle abzulegen, wird dies vom System nicht zugelassen. Wenn Benutzer die Möglichkeit haben sollen, Elemente an einer beliebigen Stelle Ihrer App abzulegen, legen Sie den gesamten Hintergrund als Dropziel fest.

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDropArea)]

Beim Ziehen sollten Sie in der Regel genau festlegen, was gezogen werden kann. Benutzer möchten meist nur bestimmte Elemente ziehen, z. B. Bilder, jedoch nicht alle Elemente in Ihrer App. Nachfolgend sehen Sie, wie Sie [**CanDrag**][CanDrag] mithilfe von XAML festlegen.

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDragArea)]

Sie müssen keine weiteren Aktionen durchführen, um das Ziehen zu ermöglichen; es sei denn, Sie möchten die Benutzeroberfläche anpassen (was später in diesem Artikel behandelt wird). Das Ablegen erfordert einige weitere Schritte.

## Verarbeiten des DragOver-Ereignisses

Das [**DragOver**][DragOver]-Ereignis wird ausgelöst, wenn ein Benutzer ein Element über Ihre App gezogen, aber noch nicht abgelegt hat. In diesem Handler müssen Sie mithilfe der [**DragEventArgs.AcceptedOperation**][AcceptedOperation]-Eigenschaft angeben, welche Art von Vorgängen Ihre App unterstützt. Kopieren ist der häufigste Vorgang.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOver)]

## Verarbeiten des Dropereignisses

Das [**Drop**][Drop]-Ereignis tritt auf, wenn der Benutzer Elemente in einem gültigen Dropbereich ablegt. Verarbeiten Sie dieses Ereignis mit der [**DragEventArgs.DataView**][DataView]-Eigenschaft.

Der Einfachheit halber wird im folgenden Beispiel angenommen, dass der Benutzer ein einzelnes Foto abgelegt hat und auf dieses zugreift. In der Praxis können Benutzer mehrere Elemente unterschiedlicher Formate gleichzeitig ablegen. Ihre App sollte diese Möglichkeit behandeln, indem sie überprüft, welche Dateitypen abgelegt wurden, sie entsprechend verarbeiten und den Benutzer benachrichtigen, wenn er versucht, eine Aktion auszuführen, die von der App nicht unterstützt wird.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_Drop)]

## Anpassen der Benutzeroberfläche

Das System bietet eine Standardbenutzeroberfläche für Drag & Drop. Sie können jedoch auch verschiedene Teile der Benutzeroberfläche anpassen, indem Sie benutzerdefinierte Beschriftungen und Symbole festlegen oder angeben, dass keine Benutzeroberfläche angezeigt werden soll. Verwenden Sie zum Anpassen der Benutzeroberfläche die Eigenschaft [**DragUIOverride**][DragUiOverride] im Ereignishandler [**DragOver**][DragOver].

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOverCustom)]

 <!-- LINKS -->
[AllowDrop]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx
[CanDrag]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx
[DragOver]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx
[AcceptedOperation]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.drageventargs.acceptedoperation.aspx
[DataView]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.drageventargs.dataview.aspx
[DragUiOverride]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.drageventargs.draguioverride.aspx
[Drop]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx 



<!--HONumber=Mar16_HO5-->


