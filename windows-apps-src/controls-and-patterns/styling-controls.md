---
author: Jwmsft
Description: "Sie können mit Stilen die Steuerelementeigenschaften festlegen und diese Einstellungen dann für andere Steuerelemente übernehmen, um so für ein einheitliches Erscheinungsbild zu sorgen."
MS-HAID: dev\_ctrl\_layout\_txt.styling\_controls
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Formatieren von Steuerelementen
ms.assetid: AB469A46-FAF5-42D0-9340-948D0EDF4150
label: Styling controls
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 2386ccbc89cad5514c0b1a4879af6d0df328263e

---

# Formatieren von Steuerelementen



Das XAML-Framework bietet zahlreiche Anpassungsmöglichkeiten für die App-Darstellung. Sie können mit Stilen die Steuerelementeigenschaften festlegen und diese Einstellungen dann für andere Steuerelemente übernehmen, um so für ein einheitliches Erscheinungsbild zu sorgen.

## Grundlagen zu Stilen


Verwenden Sie Stile, um visuelle Eigenschaften in wiederverwendbare Ressourcen auszulagern. Dieses Beispiel zeigt drei Schaltflächen mit einem Stil, der die Eigenschaften [**BorderBrush**](https://msdn.microsoft.com/library/windows/apps/br209397), [**BorderThickness**](https://msdn.microsoft.com/library/windows/apps/br209399) und [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414) festlegt. Durch Anwenden eines Stils können Sie eine einheitliche Darstellung der Steuerelemente erreichen, ohne diese Eigenschaften separat für jedes Steuerelement festlegen zu müssen.

![Formatierte Schaltflächen](images/styles-rainbow-buttons.png)

Sie können einen Stil inline im XAML-Code für ein Steuerelement oder als wiederverwendbare Ressource definieren. Sie können Ressourcen in der XAML-Datei einer bestimmten Seite, in der Datei App.xaml oder in einer separaten XAML-Datei mit Ressourcenverzeichnis definieren. Eine XAML-Datei mit einem Ressourcenverzeichnis kann App-übergreifend genutzt werden. Außerdem können in einer einzelnen App mehrere Ressourcenverzeichnisse zusammengeführt werden. Der Ort, an dem die Ressource definiert wird, bestimmt den Bereich, in dem sie verwendet werden kann. Ressourcen auf Seitenebene sind nur auf der Seite verfügbar, für die sie definiert sind. Sind Ressourcen mit demselben Schlüssel sowohl in „App.xaml“ als auch auf einer Seite definiert, haben die Ressourcen auf der Seite Vorrang vor den Ressourcen in App.xaml. Wenn eine Ressource in einer separaten Ressourcenwörterbuchdatei definiert ist, wird Ihr Bereich dadurch bestimmt, von wo auf das Ressourcenwörterbuch verwiesen wird.

In der [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849)-Definition benötigen wir ein [**TargetType**](https://msdn.microsoft.com/library/windows/apps/br208857)-Attribut und eine Auflistung eines oder mehrerer [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817)-Elemente. Das **TargetType**-Attribut ist eine Zeichenfolge, die einen [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)-Typ angibt, auf den der Stil angewendet wird. Der **TargetType**-Wert muss einen von **FrameworkElement** abgeleiteten Typ angeben, der durch die Windows-Runtime definiert ist, oder einen benutzerdefinierten Typ, der in einer referenzierten Assembly verfügbar ist. Wenn Sie versuchen, einen Stil auf ein Steuerelement anzuwenden, das nicht zum **TargetType**-Attribut des Stils passt, der angewendet werden soll, wird eine Ausnahme ausgelöst.

Jedes [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817)-Element benötigt eine [**Property**](https://msdn.microsoft.com/library/windows/apps/br208836) und einen [**Value**](https://msdn.microsoft.com/library/windows/apps/br208838). Diese Eigenschaftseinstellungen geben die Steuerelementeigenschaft an, auf die die Einstellung angewendet wird, und den Wert, der für diese Eigenschaft festgelegt wird. Sie können den **Setter.Value** entweder mit Attribut- oder Eigenschaftselementsyntax angeben. Dieses XAML-Codebeispiel zeigt den Stil, der auf die zuvor abgebildeten Schaltflächen angewendet wurde. In diesem XAML-Code verwenden die ersten beiden **Setter**-Elemente Attributsyntax, aber der letzte **Setter** (für die [**BorderBrush**](https://msdn.microsoft.com/library/windows/apps/br209397)-Eigenschaft) verwendet Eigenschaftselementsyntax. Bei diesem Beispiel wird das [x:Key-Attribut](../xaml-platform/x-key-attribute.md) nicht verwendet, sodass der Stil implizit auf die Schaltflächen angewendet wird. Das implizite oder explizite Anwenden von Stilen wird im nächsten Abschnitt beschrieben.

```XAML
<Page.Resources>
    <Style TargetType="Button">
        <Setter Property="BorderThickness" Value="5" />
        <Setter Property="Foreground" Value="Blue" />
        <Setter Property="BorderBrush" >
            <Setter.Value>
                <LinearGradientBrush StartPoint="0.5,0" EndPoint="0.5,1">
                    <GradientStop Color="Yellow" Offset="0.0" />
                    <GradientStop Color="Red" Offset="0.25" />
                    <GradientStop Color="Blue" Offset="0.75" />
                    <GradientStop Color="LimeGreen" Offset="1.0" />
                </LinearGradientBrush>
            </Setter.Value>
        </Setter>
    </Style>
</Page.Resources>

<StackPanel Orientation="Horizontal">
    <Button Content="Button"/>
    <Button Content="Button"/>
    <Button Content="Button"/>
</StackPanel>
```

## Anwenden impliziter oder expliziter Stile

Wenn Sie einen Stil als Ressource definieren, können Sie ihn auf zwei Arten auf Ihre Steuerelemente anwenden:

-   Implizit, indem Sie nur einen [**TargetType**](https://msdn.microsoft.com/library/windows/apps/br208857) für den [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) festlegen.
-   Explizit, indem Sie einen [**TargetType**](https://msdn.microsoft.com/library/windows/apps/br208857) und ein [x:Key-Attribut](../xaml-platform/x-key-attribute.md) für den [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) angeben und die [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743)-Eigenschaft des Zielsteuerelements mit einem Verweis auf die [{StaticResource}-Markuperweiterung](https://msdn.microsoft.com/library/windows/apps/mt185588) festlegen, die den expliziten Schlüssel verwendet.

Wenn ein Stil ein [x:Key-Attribut](../xaml-platform/x-key-attribute.md) enthält, können Sie ihn nur auf Steuerelemente anwenden, indem Sie die [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743)-Eigenschaft des Steuerelements auf den Stilschlüssel festlegen. Im Gegensatz dazu wird ein Stil ohne x:Key-Attribut automatisch auf jedes Steuerelement mit dem Zieltyp angewendet, das ansonsten über keine explizite Stileinstellung verfügt.

Diese beiden Schaltflächen veranschaulichen implizite und explizite Stile.

![Schaltflächen mit impliziten und expliziten Stilen](images/styles-buttons-implicit-explicit.png)

In diesem Beispiel besitzt der erste Stil ein [x:Key-Attribut](../xaml-platform/x-key-attribute.md), und der Zieltyp ist [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265). Die [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743)-Eigenschaft der ersten Schaltfläche wird auf diesen Schlüssel festgelegt, sodass der Stil explizit angewendet wird. Der zweite Stil wird implizit auf die zweite Schaltfläche angewendet, da deren Zieltyp **Button** ist und der Stil kein x:Key-Attribut besitzt.

```XAML
<Page.Resources>
    <Style x:Key="PurpleStyle" TargetType="Button">
        <Setter Property="FontFamily" Value="Lucida Sans Unicode"/>
        <Setter Property="FontStyle" Value="Italic"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Foreground" Value="MediumOrchid"/>
    </Style>

    <Style TargetType="Button">
        <Setter Property="FontFamily" Value="Lucida Sans Unicode"/>
        <Setter Property="FontStyle" Value="Italic"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="RenderTransform">
            <Setter.Value>
                <RotateTransform Angle="25"/>
            </Setter.Value>
        </Setter>
        <Setter Property="BorderBrush" Value="Orange"/>
        <Setter Property="BorderThickness" Value="2"/>
        <Setter Property="Foreground" Value="Orange"/>
    </Style>
</Page.Resources>

<Grid x:Name="LayoutRoot">
    <Button Content="Button" Style="{StaticResource PurpleStyle}"/>
    <Button Content="Button" />
</Grid>
```

## Verwenden abgeleiteter Stile

Um die Verwaltung von Stilen zu vereinfachen und die Wiederverwendung zu optimieren, können Sie Stile erstellen, die von anderen Stilen erben. Verwenden Sie die [**BasedOn**](https://msdn.microsoft.com/library/windows/apps/br208852)-Eigenschaft, um abgeleitete Stile zu erstellen. Stile, die von anderen Stilen erben, müssen als Ziel denselben Steuerelementtyp haben oder ein Steuerelement, das von dem Typ abgeleitet wird, auf den der Basisstil verweist. Wenn das Ziel des Basisstils z.B. [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/br209365) ist, können die von diesem abgeleiteten Stile **ContentControl** als Ziel haben oder Typen, die von **ContentControl** abgeleitet sind, z.B. [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) und [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527). Wenn ein Wert im abgeleiteten Stil nicht festgelegt wurde, wird er vom Basisstil vererbt. Möchten Sie den Wert vom Basisstil ändern, können Sie ihn im abgeleiteten Stil überschreiben. Das nächste Beispiel zeigt einen **Button** und ein [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) mit Stilen, die von demselben Basisstil abgeleitet sind.

![Formatierte Schaltflächen mit abgeleiteten Stilen](images/styles-buttons-based-on.png)

Der Basisstil hat als Ziel [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/br209365). Er legt die [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718)-Eigenschaft und die [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751)-Eigenschaft fest. Die von diesem Stil abgeleiteten Stile haben als Ziel [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) und [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265), die von **ContentControl** abgeleitet sind. Die abgeleiteten Stile legen andere Farben für die [**BorderBrush**](https://msdn.microsoft.com/library/windows/apps/br209397)-Eigenschaft und die [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414)-Eigenschaft fest. (Normalerweise wird eine **CheckBox** nicht mit einem Rahmen versehen. Hier dient dies zur Veranschaulichung der Auswirkungen des Stils.)

```XAML
<Page.Resources>
    <Style x:Key="BasicStyle" TargetType="ContentControl">
        <Setter Property="Width" Value="130" />
        <Setter Property="Height" Value="30" />
    </Style>

    <Style x:Key="ButtonStyle" TargetType="Button" 
           BasedOn="{StaticResource BasicStyle}">
        <Setter Property="BorderBrush" Value="Orange" />
        <Setter Property="BorderThickness" Value="2" />
        <Setter Property="Foreground" Value="Red" />
    </Style>

    <Style x:Key="CheckBoxStyle" TargetType="CheckBox" 
           BasedOn="{StaticResource BasicStyle}">
        <Setter Property="BorderBrush" Value="Blue" />
        <Setter Property="BorderThickness" Value="2" />
        <Setter Property="Foreground" Value="Green" />
    </Style>
</Page.Resources>

<StackPanel>
    <Button Content="Button" Style="{StaticResource ButtonStyle}" Margin="0,10"/>
    <CheckBox Content="CheckBox" Style="{StaticResource CheckBoxStyle}"/>
</StackPanel>
```

## Verwenden von Tools für die problemlose Arbeit mit Stilen

Wenn Sie schnell einen Stil auf Ihr Steuerelement anwenden möchten, klicken Sie auf der Microsoft Visual Studio XAML-Entwicklungsoberfläche mit der rechten Maustaste auf das Steuerelement, und wählen Sie **Stil bearbeiten** oder **Vorlage bearbeiten** aus (je nach Steuerelement). Anschließend können Sie einen vorhandenen Stil anwenden, indem Sie **Ressource übernehmen** auswählen, oder einen neuen erstellen, indem Sie **Leer erstellen** auswählen. Wenn Sie einen leeren Stil erstellen, haben Sie die Möglichkeit, diesen auf der Seite, in der Datei App.xaml oder in einem eigenen Ressourcenverzeichnis zu definieren.

## Ändern der standardmäßigen Systemstile

Verwenden Sie nach Möglichkeit die Stile der standardmäßigen Windows-Runtime-XAML-Ressourcen. Wenn Sie eigene Stile definieren müssen, leiten Sie Ihre Stile am besten von diesen Standardstilen ab (indem Sie, wie zuvor erläutert, abgeleitete Stile verwenden oder eine Kopie des Originalstandardstils bearbeiten).

## Die Template-Eigenschaft

Für die [**Template**](https://msdn.microsoft.com/library/windows/apps/br209465)-Eigenschaft eines [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390)-Elements kann ein Stilsetter verwendet werden. Dieser erstellt faktisch den größten Teil eines typischen XAML-Stils und der XAML-Ressourcen einer App. Eine ausführlichere Beschreibung finden Sie im Thema [Steuerelementvorlagen](control-templates.md).




<!--HONumber=Aug16_HO3-->


