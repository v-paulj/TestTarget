---
author: Jwmsft
Description: Verwenden Sie den Beispielcode zur Strukturansicht, um eine ausklappbare Struktur (einen ausklappbaren Baum) zu erzeugen.
title: Strukturansicht
label: Tree view
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: b81ef40954860cb026038447158ba1a9edb07002

---
# Hierarchisches Layout mit Strukturansicht
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 


Eine Strukturansicht ist eine Hierarchieauflistung mit Knoten, die das Aus- und Einblenden von geschachtelten Elementen erlauben. Geschachtelte Elemente können zusätzliche Knoten oder reguläre Listenelemente sein. Sie können eine [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) zum Erstellen einer Strukturansicht benutzen, um eine Ordnerstruktur oder geschachtelte Beziehungen zwischen Elementen in der UI zu veranschaulichen.

Das [Strukturansicht-Beispiel](http://go.microsoft.com/fwlink/?LinkId=785018) ist eine Referenzimplementierung, das mit der **ListView** erstellt wurde. Es ist kein eigenständiges Steuerelement. Für die Strukturansicht im Bereich "Favoriten" des Microsoft Edge Browsers wird diese Referenzimplementierung genutzt.

Das Beispiel unterstützt:
- Die Schachtelung von N Ebenen
- Das Ein-/Ausblenden von Knoten
- Ziehen und Ablegen von Knoten in der Strukturansicht
- Integrierte Eingabehilfen

![Die Strukturansicht im Referenzbeispiel](images/tree-view-sample.png) | ![Die Strukturansicht im Edge-Browser](images/tree-view-edge.png)
-- | --
Referenzbeispiel für die Strukturansicht | Die Strukturansicht im Edge-Browser

## Ist dies das richtige Muster?

- Verwenden Sie eine Strukturansicht, wenn Elemente geschachtelte Elemente haben, und dann, wenn es wichtig ist, die hierarchische Beziehung zwischen Elementen und ihren gleichgestellten Elementen und Knoten zu veranschaulichen.

- Vermeiden Sie Strukturansicht, wenn die geschachtelte Beziehung eines Elements nicht im Vordergrund steht. Für die meisten Drilldown-Szenarios eignet sich eine normale ListView

## Strukturansicht "UI-Struktur"

Sie können Symbole benutzen, um Knoten in einer Strukturansicht zu veranschaulichen. Es kann eine Kombination aus Einzügen und Symbolen verwendet werden, um die geschachtelte Beziehung zwischen Ordnern und übergeordneten Knoten bzw. Nicht-Ordnern und untergeordneten Knoten darzustellen. Im Folgenden eine Anleitung zur Umsetzung.

### Symbole

Benutzen Sie Symbole, um anzuzeigen, dass es sich bei einem Element um einen Knoten handelt, und außerdem, um anzuzeigen, in welchem Zustand sich der Knoten befindet (ein- oder ausgeblendet).

#### Chevron

Aus Gründen der Konsistenz sollten ausgeblendete Knoten ein Chevron haben, welches nach rechts zeigt, während eingeblendete Knoten ein Chevron haben, das nach unten zeigt.

![Verwendung des Chevronsymbols in der Strukturansicht](images/treeview_chevron.png)

#### Ordner

Verwenden Sie ein Ordnersymbol ausschließlich als Darstellung für tatsächliche Ordner.

![Verwendung des Ordnersymbols in der Strukturansicht](images/treeview_folder.png)

#### Chevron und Ordner

Eine Kombination aus einem Chevron und einem Ordner sollte nur dann verwendet werden, wenn die Listenelemente, die selbst keine Knoten sind, in der Strukturansicht auch Symbole haben.

![Verwendung der Chevron- und Ordnersymbole zusammen in einer Strukturansicht.](images/treeview_chevron_folder.png)

#### Redlines für den Einzug von Ordnern und Knoten, die keine Ordner sind

Verwenden Sie Redlines wie in dem Bildschirmfoto unten für den Einzug von Ordnern und Knoten, die keine Ordner sind.

![Redlines für den Einzug von Ordnern und Knoten, die keine Ordner sind](images/treeview_chevron_folder_indent_rl.png)

## Erstellen einer Strukturansicht

Die Strukturansicht hat die folgenden Hauptklassen. Jede dieser Klassen wird definiert und in die Referenzimplementierung eingebunden.

> **Hinweis**&nbsp;&nbsp;Die Struktur ist als [Komponente für Windows-Runtime](https://msdn.microsoft.com/windows/uwp/winrt-components/index) implementiert, geschrieben in C++, d. h. es kann darauf von jeder UWP-App in jeder Sprache Bezug genommen werden. In diesem Beispiel befindet sich der Code der Strukturansicht im Ordner *cpp/Control*. Es gibt keinen entsprechenden *cs/Control*-Ordner für C#.

- Die `TreeNode`-Klasse implementiert das hierarchische Layout für die Strukturansicht. Sie enthält außerdem die Daten, die in der Vorlage an sie gebunden werden.
- Die `TreeView`-Klasse implementiert Ereignisse für ItemClick, das Ausklappen/Zuklappen von Ordnern, und die Initiierung von Zieh-Vorgang.
- Die `TreeViewItem` Klasse implementiert die Ereignisse für den Drop-Vorgang.
- Die `ViewModel`-Klasse fasst die Liste der Objekte in der Strukturansicht zusammen, damit Vorgänge wie Tastaturnavigation und Drag & Drop aus der ListView übernommen werden können.

## Erstellen einer Datenvorlage für Ihre Objekte in der Strukturansicht

Hier sehen Sie einen Abschnitt der XAML, die die Datenvorlage für Ordner und Elemente, die keine Ordner sind, erstellt.
- Um ein Element der ListView als Ordner zu spezifizieren, müssen Sie explizit die [AllowDrop](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.allowdrop.aspx)-Eigenschaft auf **true** für dieses Element setzen. Dieses XAML zeigt Ihnen eine Möglichkeit dafür.
- Wenn Sie ein Element der ListView nicht als Ordner definieren wollen, müssen Sie keine Eigenschaft auf das Element selbst festlegen. Setzen Sie einfach die AllowDrop-Eigenschaft in der ListView auf True.
- Sie können die Symbole für ein- oder ausgeblendete Ordner bzw. Chevron-Zeichen benutzen, um visuell anzuzeigen, ob ein Ordner ein- oder ausgeblendet ist.
- Nutzen Sie die Möglichkeit, in diesem Beispiel verschiedene Symbole zu wählen, die für den ein- und ausgeklappten Zustand benötigt werden.

```xaml
<!-- MainPage.xaml -->
<DataTemplate x:Key="TreeViewItemDataTemplate">
    <StackPanel Orientation="Horizontal" Height="40" Margin="{Binding Depth, Converter={StaticResource IntToIndConverter}}" AllowDrop="{Binding Data.IsFolder}">
        <FontIcon x:Name="expandCollapseChevron"
                  Glyph="{Binding IsExpanded, Converter={StaticResource expandCollapseGlyphConverter}}"
                  Visibility="{Binding Data.IsFolder, Converter={StaticResource booleanToVisibilityConverter}}"                           
                  FontSize="12"
                  Margin="12,8,12,8"
                  FontFamily="Segoe MDL2 Assets"                          
                  />
        <Grid>
            <FontIcon x:Name ="expandCollapseFolder"
                      Glyph="{Binding IsExpanded, Converter={StaticResource folderGlyphConverter}}"
                      Foreground="#FFFFE793"
                      FontSize="16"
                      Margin="0,8,12,8"
                      FontFamily="Segoe MDL2 Assets"
                      Visibility="{Binding Data.IsFolder, Converter={StaticResource booleanToVisibilityConverter}}"
                      />

            <FontIcon x:Name ="nonFolderIcon"
                      Glyph="&#xE160;"
                      Foreground="{ThemeResource SystemControlForegroundBaseLowBrush}"
                      FontSize="12"
                      Margin="20,8,12,8"
                      FontFamily="Segoe MDL2 Assets"
                      Visibility="{Binding Data.IsFolder, Converter={StaticResource inverseBooleanToVisibilityConverter}}"
                      />

            <FontIcon x:Name ="expandCollapseFolderOutline"
                      Glyph="{Binding IsExpanded, Converter={StaticResource folderOutlineGlyphConverter}}"
                      Foreground="#FFECC849"
                      FontSize="16"
                      Margin="0,8,12,8"
                      FontFamily="Segoe MDL2 Assets"
                      Visibility="{Binding Data.IsFolder, Converter={StaticResource booleanToVisibilityConverter}}"/>
        </Grid>

        <TextBlock Text="{Binding Data.Name}"
                   HorizontalAlignment="Stretch"
                   VerticalAlignment="Center"  
                   FontWeight="Medium"
                   FontFamily="Segoe MDL2 Assests"                           
                   Style="{ThemeResource BodyTextBlockStyle}"/>
    </StackPanel>
</DataTemplate>
```

## Richten Sie die Daten in der Strukturansicht ein

Im Folgenden ist der Code, der die Daten in der beispielhaften Strukturansicht liefert.

```csharp
 public MainPage()
 {
     this.InitializeComponent();

     TreeNode workFolder = CreateFolderNode("Work Documents");
     workFolder.Add(CreateFileNode("Feature Functional Spec"));
     workFolder.Add(CreateFileNode("Feature Schedule"));
     workFolder.Add(CreateFileNode("Overall Project Plan"));
     workFolder.Add(CreateFileNode("Feature Resource allocation"));
     sampleTreeView.RootNode.Add(workFolder);

     TreeNode remodelFolder = CreateFolderNode("Home Remodel");
     remodelFolder.IsExpanded = true;
     remodelFolder.Add(CreateFileNode("Contactor Contact Information"));
     remodelFolder.Add(CreateFileNode("Paint Color Scheme"));
     remodelFolder.Add(CreateFileNode("Flooring woodgrain types"));
     remodelFolder.Add(CreateFileNode("Kitchen cabinet styles"));

     TreeNode personalFolder = CreateFolderNode("Personal Documents");
     personalFolder.IsExpanded = true;
     personalFolder.Add(remodelFolder);

     sampleTreeView.RootNode.Add(personalFolder);
 }

 private static TreeNode CreateFileNode(string name)
 {
     return new TreeNode() { Data = new FileSystemData(name) };
 }

 private static TreeNode CreateFolderNode(string name)
 {
     return new TreeNode() { Data = new FileSystemData(name) { IsFolder = true } };
 }
```

Sobald Sie mit den oben genannten Schritten fertig sind, erhalten Sie eine vollständig aufgefüllte Strukturansicht bzw. ein hierarchisches Layout mit n Verschachtelungen, Unterstützung für das Ein-/Ausblenden von Ordnern, die Drag & Drop-Funktion zwischen Ordnern und integrierte Eingabehilfen.

Um dem Benutzer die Möglichkeit zu geben, Elemente in der Strukturansicht hinzuzufügen oder zu entfernen, wird Ihnen empfohlen, ein Kontextmenü einzusetzen, welches dem Benutzer diese Optionen zugänglich macht.


## Verwandte Artikel

- [Beispiel für die Strukturansicht](http://go.microsoft.com/fwlink/?LinkId=785018)
- [**ListView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [ListView und Gitteransicht](listview-and-gridview.md)



<!--HONumber=Aug16_HO3-->


