---
author: Xansky
Description: "Enthält eine Liste mit den Steuerelementmustern für die Microsoft-Benutzeroberflächenautomatisierung sowie mit den Klassen, die Clients für den Zugriff verwenden, und den Schnittstellen, die Anbieter zur Implementierung verwenden."
ms.assetid: 2091883C-5D0C-44ED-936A-709022926A42
title: Steuerelementmuster und Schnittstellen
label: Control patterns and interfaces
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 74e5af4c3eb5a2e17c95afce156474b613e966c5
ms.openlocfilehash: d2ae98f95538c014ef256f5d4a400aabb36c3118

---

# Steuerelementmuster und Schnittstellen  



Enthält eine Liste mit den Steuerelementmustern für die Microsoft-Benutzeroberflächenautomatisierung sowie mit den Klassen, die Clients für den Zugriff verwenden, und den Schnittstellen, die Anbieter zur Implementierung verwenden.

In der Tabelle in diesem Thema werden die Steuerelementmuster der Microsoft-Benutzeroberflächenautomatisierung erläutert. Die Tabelle enthält außerdem die Klassen, die von Benutzeroberflächenautomatisierungs-Clients für den Zugriff auf die Steuerelementmuster verwendet werden, sowie die Schnittstellen, die Benutzeroberflächenautomatisierungs-Anbieter zu deren Implementierung nutzen. In der Spalte **Steuerelementmuster** wird der Mustername aus Sicht des Benutzeroberflächenautomatisierungs-Clients als Konstantenwert angezeigt, der in [**Eigenschaftsbezeichnern für die Verfügbarkeit von Steuerelementmustern**](https://msdn.microsoft.com/library/windows/desktop/Ee671199) aufgeführt wird. Aus Sicht des Benutzeroberflächenautomatisierungs-Anbieters ist jedes dieser Muster ein [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496)-Konstantenname. In der Spalte **Schnittstelle für Klassenanbieter** wird der Name der Windows-Runtime-Schnittstelle angezeigt, die von Anbietern implementiert wird, um dieses Muster für ein benutzerdefiniertes XAML-Steuerelement bereitzustellen.

Weitere Informationen zum Implementieren benutzerdefinierter Automatisierungspeers, von denen Steuerelementmuster verfügbar gemacht und die Schnittstellen implementiert werden, finden Sie unter [Benutzerdefinierte Automatisierungspeers](custom-automation-peers.md).

Beim Implementieren eines Steuerelementmusters sollten Sie auch die Dokumentation zum Benutzeroberflächenautomatisierungs-Anbieter heranziehen. Darin werden einige Erwartungen beschrieben, die Clients unabhängig davon, welches Benutzeroberflächenframework zum Implementieren verwendet wird, an ein Steuerelementmuster stellen. Einige Informationen, die in der allgemeinen Dokumentation zum Benutzeroberflächenautomatisierungs-Anbieter aufgeführt sind, haben Einfluss darauf, wie Sie Ihre Peers implementieren und das Muster richtig unterstützen. Lesen Sie sich die Informationen unter [Implementieren von Steuerelementmustern für die Benutzeroberflächenautomatisierung](https://msdn.microsoft.com/library/windows/desktop/Ee671292) durch, und zeigen Sie die Seite an, auf der das gewünschte Muster dokumentiert ist.

| Steuerelementmuster | Schnittstelle für Klassenanbieter | Beschreibung |
|-----------------|--------------------------|-------------|
| **Annotation** | [**IAnnotationProvider**](https://msdn.microsoft.com/library/windows/apps/Hh738493) | Damit werden die Eigenschaften einer Anmerkung in einem Dokument verfügbar gemacht. |
| **Dock** | [**IDockProvider**](https://msdn.microsoft.com/library/windows/apps/BR242565) | Wird für Steuerelemente verwendet, die in einem Andockcontainer angedockt werden können. Beispiele: Symbolleisten oder Toolpaletten. |
| **Drag** | [**IDragProvider**](https://msdn.microsoft.com/library/windows/apps/Hh750322) | Wird zum Unterstützen von ziehbaren Steuerelementen bzw. Steuerelementen mit ziehbaren Elementen verwendet. |
| **DropTarget** | [**IDropTargetProvider**](https://msdn.microsoft.com/library/windows/apps/Hh750327) | Wird zum Unterstützen von Steuerelementen verwendet, die Ziel eines Drag&Drop-Vorgangs sein können. |
| **ExpandCollapse** | [**IExpandCollapseProvider**](https://msdn.microsoft.com/library/windows/apps/BR242568) | Wird zum Unterstützen von Steuerelementen verwendet, die visuell erweitert oder reduziert werden, um mehr bzw. weniger Inhalt anzuzeigen. |
| **Grid** | [**IGridProvider**](https://msdn.microsoft.com/library/windows/apps/BR242578) | Wird für Steuerelemente verwendet, von denen Rasterfunktionen wie etwa Größenanpassung und Verschieben in eine angegebene Zelle unterstützt werden. Beachten Sie, dass dieses Muster nicht vom Raster selbst implementiert wird. Dies liegt daran, dass das Raster zwar ein Layout bereitstellt, aber kein Steuerelement ist. |
| **GridItem** | [**IGridItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242572) | Wird für Steuerelemente verwendet, die innerhalb von Rastern über Zellen verfügen. |
| **Invoke** | [**IInvokeProvider**](https://msdn.microsoft.com/library/windows/apps/BR242582) | Wird für Steuerelemente verwendet, die aufgerufen werden können, beispielsweise ein [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)-Steuerelement. |
| **ItemContainer** | [**IItemContainerProvider**](https://msdn.microsoft.com/library/windows/apps/BR242583) | Ermöglicht es Anwendungen, Elemente in einem Container (z.B. in einer virtualisierten Liste) zu finden. |
| **MultipleView** | [**IMultipleViewProvider**](https://msdn.microsoft.com/library/windows/apps/BR242585) | Wird für Steuerelemente verwendet, in denen zwischen verschiedenen Darstellungen des gleichen Informationssatzes, Datensatzes oder Satzes von untergeordneten Elementen umgeschaltet werden kann. |
| **ObjectModel** | [**IObjectModelProvider**](https://msdn.microsoft.com/library/windows/apps/Dn251815) | Wird verwendet, um für das zugrunde liegende Objektmodell eines Dokuments einen Zeiger verfügbar zu machen. |
| **RangeValue** | [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590) | Wird für Steuerelemente verwendet, die über einen Wertebereich verfügen, der auf das Steuerelement angewendet werden kann. So verfügt ein Drehfeld, das Jahre enthält, beispielsweise über einen Bereich von 1900 bis zum aktuellen Jahr. Ein anderes Drehfeld, das für Monate steht, würde jedoch über einen Bereich von1 bis12 verfügen. |
| **Scroll** | [**IScrollProvider**](https://msdn.microsoft.com/library/windows/apps/BR242601) | Wird für Steuerelemente verwendet, für die ein Bildlauf durchgeführt werden kann. Beispiel: ein Steuerelement mit Bildlaufleisten, die aktiv sind, wenn mehr Informationen vorhanden sind, als im Anzeigebereich des Steuerelements angezeigt werden können. |
| **ScrollItem** | [**IScrollItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242599) | Wird für Steuerelemente verwendet, die über einzelne Elemente in einer Liste mit Bildlauf verfügen. Beispiel: ein Steuerelement für eine Liste, das über einzelne Elemente in der Bildlaufliste verfügt, wie etwa ein Steuerelement für ein Kombinationsfeld. |
| **Selection** | [**ISelectionProvider**](https://msdn.microsoft.com/library/windows/apps/BR242616) | Wird für Steuerelemente für Auswahlcontainer verwendet. Beispiel: [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) und [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/BR209348). |
| **SelectionItem** | [**ISelectionItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242610) | Wird für einzelne Elemente in Steuerelementen für Auswahlcontainer verwendet, z.B. Listen- und Kombinationsfelder. |
| **Spreadsheet** | [**ISpreadsheetProvider**](https://msdn.microsoft.com/library/windows/apps/Dn251821) | Wird verwendet, um den Inhalt einer Tabellenkalkulation oder eines anderen rasterbasierten Dokuments verfügbar zu machen. |
| **SpreadsheetItem** | [**ISpreadsheetItemProvider**](https://msdn.microsoft.com/library/windows/apps/Dn251817) | Wird verwendet, um die Eigenschaften einer Zelle in einer Tabellenkalkulation oder einem anderen rasterbasierten Dokument verfügbar zu machen. |
| **Styles** | [**IStylesProvider**](https://msdn.microsoft.com/library/windows/apps/Dn251823) | Wird verwendet, um ein UI-Element zu beschreiben, das in Bezug auf Stil, Füllfarbe, Füllmuster oder Form über bestimmte Einstellungen verfügt. |
| **SynchronizedInput** | [**ISynchronizedInputProvider**](https://msdn.microsoft.com/library/windows/apps/Dn279198) | Ermöglicht Benutzeroberflächenautomatisierungs-Client-Apps das Lenken der Maus- oder Tastatureingabe auf ein bestimmtes UI-Element. |
| **Table** | [**ITableProvider**](https://msdn.microsoft.com/library/windows/apps/BR242623) | Wird für Steuerelemente verwendet, die sowohl über ein Raster als auch über Kopfzeileninformationen verfügen. Beispiel: ein Steuerelement für einen tabellarischen Kalender. |
| **TableItem** | [**ITableItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242620) | Wird für Elemente in einer Tabelle verwendet. |
| **Text** | [**ITextProvider**](https://msdn.microsoft.com/library/windows/apps/BR242627) | Wird für Bearbeitungssteuerelemente und für Dokumente verwendet, in denen Informationen in Textform verfügbar gemacht werden. Siehe auch [**ITextRangeProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextrangeprovider) und [**ITextProvider2**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextprovider2). |
| **TextChild** | [**ITextChildProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextchildprovider) | Wird für den Zugriff auf den nächstgelegenen Vorgänger eines Elements verwendet, der das **Text**-Steuerelementmuster unterstützt. |
| **TextEdit** | Keine verwaltete Klasse verfügbar | Gewährt Zugriff auf ein Steuerelement, mit dem Text geändert wird. Dies kann beispielsweise ein Steuerelement sein, mit dem die Autokorrektur durchgeführt oder mithilfe eines Input Method Editors (IME) die Komposition der Eingabe ermöglicht wird. |
| **TextRange** | [**ITextRangeProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextrangeprovider) | Bietet Zugriff auf einen Bereich mit fortlaufendem Text in einem Textcontainer, von dem [**ITextProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextprovider) implementiert wird. Siehe auch [**ITextRangeProvider2**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextrangeprovider2). |
| **Toggle** | [**IToggleProvider**](https://msdn.microsoft.com/library/windows/apps/BR242653) | Wird für Steuerelemente verwendet, deren Status geändert werden kann. Beispiel: aktivierbare [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/BR209316)-Steuerelemente und Menüelemente. |
| **Transform** | [**ITransformProvider**](https://msdn.microsoft.com/library/windows/apps/BR242656) | Wird für Steuerelemente verwendet, deren Größe geändert werden kann und die verschoben und gedreht werden können. Gewöhnlich wird das Steuerelementmuster für Transformation in Designern, Formen, Grafikeditoren und Zeichenanwendungen verwendet. |
| **Value** | [**IValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242663) | Ermöglicht es Clients, einen Wert von Steuerelementen abzurufen, die keinen Wertebereich unterstützen, oder einen Wert dafür festzulegen. |
| **VirtualizedItem** | [**IVirtualizedItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242668) | Macht Elemente in Containern verfügbar, die virtualisiert sind und vollständig als Benutzeroberflächenautomatisierungs-Elemente zur Verfügung stehen müssen. |
| **Window** | [**IWindowProvider**](https://msdn.microsoft.com/library/windows/apps/BR242670) | Macht für bestimmte Fenster spezifische Informationen verfügbar. Hierbei handelt es sich um ein grundlegendes Konzept des MicrosoftWindows-Betriebssystems. Beispiele für Steuerelemente als Fenster: untergeordnete Fenster und Dialogfelder. |

> [!NOTE]
> Implementierungen all dieser Muster sind in vorhandenen XAML-Steuerelementen nicht immer enthalten. Einige Muster verfügen nur über Schnittstellen, um die Parität mit der allgemeinen Benutzeroberflächenautomatierungs-Frameworkdefinition für Muster sowie Automatisierungspeerszenarien zu unterstützen, die für die Unterstützung dieses Musters eine rein benutzerdefinierte Implementierung benötigen.

> [!NOTE]
> Windows Phone Store-Apps unterstützen nicht alle hier aufgeführten Steuerelementmuster der Benutzeroberflächenautomatisierung. Zu den nicht unterstützten Mustern zählen beispielsweise **Annotation**, **Dock**, **Drag**, **DropTarget** und **ObjectModel**.

<span id="related_topics"/>
## Verwandte Themen  
* [Benutzerdefinierte Automatisierungspeers](custom-automation-peers.md)
* [Barrierefreiheit](accessibility.md) 



<!--HONumber=Aug16_HO3-->


