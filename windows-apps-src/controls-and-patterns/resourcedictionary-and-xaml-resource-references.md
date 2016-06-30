---
author: Jwmsft
Description: "Erläutert die Definition von ResourceDictionary-Elementen und Schlüsselressourcen sowie den Bezug von XAML-Ressourcen zu anderen Ressourcen, die Sie als Teil Ihrer App oder Ihres App-Pakets definieren."
MS-HAID: dev\_ctrl\_layout\_txt.resourcedictionary\_and\_xaml\_resource\_references
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: ResourceDictionary- und XAML-Ressourcenreferenzen
ms.assetid: E3CBFA3D-6AF5-44E1-B9F9-C3D3EA8A25CE
label: ResourceDictionary and XAML resource references
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: fddd345507aace54aca66fe1caa2d9f89a74a299

---

# ResourceDictionary- und XAML-Ressourcenreferenzen



Sie können die UI oder Ressourcen für Ihre App mit XAML definieren. Bei Ressourcen handelt es sich meist um Definitionen von Objekten, die Sie voraussichtlich mehrmals verwenden. Um später auf eine XAML-Ressource verweisen zu können, geben Sie einen Ressourcenschlüssel an, der wie ein Name fungiert. Auf eine Ressource kann überall in einer App sowie auf jeder enthaltenen XAML-Seite verwiesen werden. Ressourcen können mithilfe eines [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)-Elements aus der Windows-Runtime-XAML definiert werden. Anschließend können Sie auf die Ressourcen mithilfe einer [StaticResource-Markuperweiterung](../xaml-platform/staticresource-markup-extension.md) oder [ThemeResource-Markuperweiterung](../xaml-platform/themeresource-markup-extension.md) verweisen.

Zu den XAML-Elementen, die besonders häufig als XAML-Ressourcen deklariert werden, zählen [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849), [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391), Animationskomponenten und [**Brush**](https://msdn.microsoft.com/library/windows/apps/br228076)-Unterklassen. Hier erläutern wir die Definition von [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)-Elementen und Schlüsselressourcen sowie den Bezug von XAML-Ressourcen zu anderen Ressourcen, die Sie als Teil Ihrer App oder Ihres App-Pakets definieren. Außerdem werden die erweiterten Features des Ressourcenverzeichnisses wie etwa [**MergedDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208801) und [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208807) beschrieben.

**Voraussetzungen**

Es wird vorausgesetzt, dass Sie über Kenntnisse im Bereich XAML-Markup verfügen und die [Übersicht über XAML](https://msdn.microsoft.com/library/windows/apps/mt185595) gelesen haben.

## Definieren und Verwenden von XAML-Ressourcen

XAML-Ressourcen sind Objekte, auf die mehrmals im Markup verwiesen wird. Ressourcen werden üblicherweise in einer separaten Datei oder oben auf der Markupseite in einem [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)-Element definiert:

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <x:String x:Key="greeting">Hello world</x:String>
        <x:String x:Key="goodbye">Goodbye world</x:String>
    </Page.Resources>

    <TextBlock Text="{StaticResource greeting}" Foreground="Gray" VerticalAlignment="Center"/>
</Page>
```

Für dieses Beispiel gilt Folgendes:

-   `<Page.Resources>…</Page.Resources>` – Definiert das Ressourcenverzeichnis
-   `<x:String>` – Definiert die Ressource mit dem Schlüssel „greeting“
-   `{StaticResource greeting}` – Sucht nach der Ressource mit dem Schlüssel „greeting“, die der [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676)-Eigenschaft von [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) zugeordnet ist.

> **Hinweis**
            &nbsp;&nbsp;Die Konzepte für [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) dürfen nicht mit dem Buildvorgang **Resource**, mit Ressourcendateien (.resw) oder mit anderen Ressourcen verwechselt werden, die im Zusammenhang mit der Strukturierung des Codeprojekts zur Generierung Ihres App-Pakets erwähnt werden.

Ressourcen sind nicht zwingend Zeichenfolgen, sondern können beliebige freigebbare Objekte sein (etwa Stile, Vorlagen, Pinsel und Farben). Steuerelemente, Formen und andere [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)-Elemente können allerdings nicht freigegeben und somit auch nicht als wiederverwendbare Ressourcen deklariert werden. Weitere Informationen zum Freigeben finden Sie weiter unten in diesem Thema im Abschnitt [XAML-Ressourcen müssen freigabefähig sein](#xaml_resources_must_be_sharable).

Hierbei werden sowohl ein Pinsel als auch eine Zeichenfolge als Ressourcen deklariert und von den Steuerelementen einer Seite verwendet.

```XAML
<Page
    x:Class="SpiderMSDN.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <SolidColorBrush x:Key="myFavoriteColor" Color="green"/>
        <x:String x:Key="greeting">Hello world</x:String>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource myFavoriteColor}" Text="{StaticResource greeting}" VerticalAlignment="Top"/>
    <Button Foreground="{StaticResource myFavoriteColor}" Content="{StaticResource greeting}" VerticalAlignment="Center"/>
</Page>
```

Alle Ressourcen müssen über einen Schlüssel verfügen. Normalerweise ist dieser Schlüssel eine Zeichenfolge, die wie folgt definiert ist: `x:Key=”myString”`. Es gibt aber auch einige andere Möglichkeiten, einen Schlüssel anzugeben:

-   [
              **Style**
            ](https://msdn.microsoft.com/library/windows/apps/br208849) und [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) erfordern ein **TargetType**-Element. **TargetType** wird als Schlüssel verwendet, wenn [x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787) nicht angegeben ist. In diesem Fall ist der Schlüssel das eigentliche Typobjekt, anstatt einer Zeichenfolge. (Siehe Beispiele unten.)
-   [
              Für **DataTemplate**
            ](https://msdn.microsoft.com/library/windows/apps/br242348)-Ressourcen mit **TargetType** wird das **TargetType**-Element als Schlüssel verwendet, wenn [x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787) nicht angegeben ist. In diesem Fall ist der Schlüssel das eigentliche Typobjekt, anstatt einer Zeichenfolge.
-   [x:Name](https://msdn.microsoft.com/library/windows/apps/mt204788) kann anstelle von [x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787) verwendet werden. Bei x:Name wird aber auch ein CodeBehind-Feld für die Ressource generiert. x:Name ist also weniger effizient als x:Key, da dieses Feld beim Laden der Seite initialisiert werden muss.

Mit der [StaticResource-Markuperweiterung](../xaml-platform/staticresource-markup-extension.md) können Ressourcen nur per Zeichenfolgenname ([x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787) oder [x:Name](https://msdn.microsoft.com/library/windows/apps/mt204788)) abgerufen werden. Das XAML-Framework sucht aber auch nach impliziten Stilressourcen (für die **TargetType** verwendet wird, anstatt x:Key oder x:Name), wenn entschieden wird, welcher Stil und welche Vorlage für ein Steuerelement verwendet werden sollen, für das die Eigenschaften [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743) und [**ContentTemplate**](https://msdn.microsoft.com/library/windows/apps/br209369) oder [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/br242830) nicht festgelegt wurden.

Hier verfügt [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) über den impliziten Schlüssel **typeof(Button)**. Da für das [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)-Steuerelement unten auf der Seite keine [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743)-Eigenschaft angegeben ist, wird nach einem Stil mit dem Schlüssel **typeof(Button)** gesucht:

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <Style TargetType="Button">
              <Setter Property="Background" Value="red"/>
        </Style>
    </Page.Resources> 
       <!-- This button will have a red background. -->
       <Button Content="Button" Height="100" VerticalAlignment="Center" Width="100"/>
</Page>
```

Weitere Informationen zu impliziten Stilen und zu ihrer Funktionsweise finden Sie unter [Formatieren von Steuerelementen](styling-controls.md) sowie unter [Steuerelementvorlagen](control-templates.md).

## Suchen nach Ressourcen im Code

Der Zugriff auf Member des Ressourcenverzeichnisses erfolgt auf die gleiche Weise wie bei allen anderen Verzeichnissen.

> **Achtung**
            &nbsp;&nbsp;Wenn Sie im Code eine Ressourcensuche durchführen, werden nur die Ressourcen im `Page.Resources`-Verzeichnis berücksichtigt. Im Gegensatz zur [StaticResource-Markuperweiterung](../xaml-platform/staticresource-markup-extension.md) erfolgt kein Fallback auf das `Application.Resources`-Verzeichnis, falls die Ressourcen im ersten Verzeichnis nicht gefunden werden.

 

In diesem Beispiel wird veranschaulicht, wie Sie die `redButtonStyle`-Ressource aus dem Ressourcenverzeichnis einer Seite abrufen:

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <Style TargetType="Button" x:Key="redButtonStyle">
            <Setter Property="Background" Value="red"/>
        </Style>
    </Page.Resources>
</Page>
```

```CSharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            Style redButtonStyle = (Style)this.Resources["redButtonStyle"];
        }
    }
```

Verwenden Sie zum Suchen nach App-weiten Ressourcen im Code das **Application.Current.Resources**-Element, um das Ressourcenverzeichnis der App wie hier dargestellt abzurufen.

```XAML
<Application
    x:Class="MSDNSample.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:SpiderMSDN">
    <Application.Resources>
        <Style TargetType="Button" x:Key="appButtonStyle">
            <Setter Property="Background" Value="red"/>
        </Style>
    </Application.Resources>

</Application>
```

```CSharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            Style appButtonStyle = (Style)Application.Current.Resources["appButtonStyle"];
        }
    }
```

Sie können eine Anwendungsressource auch im Code hinzufügen.

Hierbei sind zwei Dinge zu beachten.

-   Erstens müssen Sie die Ressourcen hinzufügen, bevor eine beliebige Seite versucht, die Ressourcen zu verwenden.
-   Zweitens können Sie keine Ressourcen im Konstruktor der App hinzufügen.

Sie können beide Probleme vermeiden, wenn Sie die Ressource wie folgt in der [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)-Methode hinzufügen.

```CSharp
// App.xaml.cs
    
sealed partial class App : Application
{
    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;
        if (rootFrame == null)
        {
            SolidColorBrush brush = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 0, 255, 0)); // green
            this.Resources["brush"] = brush;
            // … Other code that VS generates for you …
        }
    }
}
```

## Jedes FrameworkElement kann über ein ResourceDictionary verfügen

[
              **FrameworkElement**
            ](https://msdn.microsoft.com/library/windows/apps/br208706) ist eine Basisklasse zum Steuern der Vererbung, und sie verfügt über eine [**Resources**](https://msdn.microsoft.com/library/windows/apps/br208740)-Eigenschaft. Sie können also jedem **FrameworkElement**-Element ein lokales Ressourcenverzeichnis hinzufügen.

Hier wird ein Ressourcenverzeichnis einem Seitenelement hinzugefügt.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <x:String x:Key="greeting">Hello world</x:String>
    </Page.Resources>

    <Border>
        <Border.Resources>
            <x:String x:Key="greeting">Hola mundo</x:String>
        </Border.Resources>
        <TextBlock Text="{StaticResource greeting}" Foreground="Gray" VerticalAlignment="Center"/>
    </Border>
</Page>

```

Hier verfügen sowohl das [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503)-Element als auch das [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250)-Element über Ressourcenverzeichnisse, und beide Elemente weisen eine Ressource mit dem Namen „greeting“ auf. Das [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)-Element ist im **Border**-Element enthalten. Die dazugehörige Ressource sucht also zuerst nach den Ressourcen des **Border**-Elements, anschließend nach den Ressourcen des **Page**-Elements und zuletzt nach den Ressourcen des [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324)-Elements. Das **TextBlock**-Element enthält „Hola mundo“.

Verwenden Sie die [**Resources**](https://msdn.microsoft.com/library/windows/apps/br208740)-Eigenschaft dieses Elements, um über den Code auf die Ressourcen dieses Elements zuzugreifen. Beim Zugreifen auf die Ressourcen eines [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)-Elements im Code, anstatt per XAML, wird nur im entsprechenden Verzeichnis gesucht, nicht in den Verzeichnissen des übergeordneten Elements.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <x:String x:Key="greeting">Hello world</x:String>
    </Page.Resources>

    <Border x:Name="border">
        <Border.Resources>
            <x:String x:Key="greeting">Hola mundo</x:String>
        </Border.Resources>
    </Border>
</Page>

```

```CSharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            string str = (string)border.Resources["greeting"];
        }
    }
```

## Zusammengeführte Ressourcenverzeichnisse

Bei einem *zusammengeführten Ressourcenverzeichnis* wird ein Ressourcenverzeichnis mit einem anderen kombiniert, meist in einer anderen Datei.

> **Tipp**
            &nbsp;&nbsp;In Microsoft Visual Studio können Sie eine Ressourcenverzeichnisdatei erstellen, indem Sie im Menü **Projekt** die Option **Hinzufügen &gt; Neues Element… &gt; Ressourcenverzeichnis** verwenden.

Hier definieren Sie ein Ressourcenverzeichnis in einer separaten XAML-Datei mit dem Namen „Dictionary1.xaml“.

```XAML
<!-- Dictionary1.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="Red"/>

</ResourceDictionary>

```

Wenn Sie dieses Verzeichnis verwenden möchten, führen Sie es mit dem Verzeichnis Ihrer Seite zusammen:

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml"/>
            </ResourceDictionary.MergedDictionaries>

            <x:String x:Key="greeting">Hello world</x:String>

        </ResourceDictionary>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource brush}" Text="{StaticResource greeting}" VerticalAlignment="Center"/>
</Page>
```

In diesem Beispiel passiert Folgendes: In `<Page.Resources>` deklarieren Sie `<ResourceDictionary>`. Das XAML-Framework erstellt für Sie implizit ein Ressourcenverzeichnis, wenn Sie Ressourcen zu `<Page.Resources>` hinzufügen. In diesem Fall möchten Sie aber kein beliebiges Verzeichnis erhalten, sondern eines mit zusammengeführten Verzeichnissen.

Sie deklarieren also `<ResourceDictionary>` und fügen der dazugehörigen `<ResourceDictionary.MergedDictionaries>`-Sammlung dann Elemente hinzu. Jeder dieser Einträge hat das Format `<ResourceDictionary Source="Dictionary1.xaml"/>`. Fügen Sie zum Hinzufügen mehrerer Verzeichnisse einfach nach dem ersten Eintrag einen Eintrag vom Typ `<ResourceDictionary Source="Dictionary2.xaml"/>` hinzu.

Nach `<ResourceDictionary.MergedDictionaries>…</ResourceDictionary.MergedDictionaries>` können Sie Ihrem Hauptverzeichnis optional weitere Ressourcen hinzufügen. Sie verwenden Ressourcen aus einem zusammengeführten Verzeichnis genauso wie bei einem normalen Verzeichnis. Im obigen Beispiel findet `{StaticResource brush}` die Ressource im untergeordneten/zusammengeführten Verzeichnis (Dictionary1.xaml), während `{StaticResource greeting}` seine Ressource im Verzeichnis der Hauptseite findet.

In der Ressourcensuchsequenz wird ein [**MergedDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208801)-Verzeichnis nur nach erfolgter Überprüfung aller anderen Schlüsselressourcen des [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) überprüft. Nach der Suche auf dieser Ebene werden die zusammengeführten Verzeichnisse erreicht und jedes Element in **MergedDictionaries** überprüft. Wenn mehrere zusammengeführte Verzeichnisse vorhanden sind, werden diese Verzeichnisse in entgegengesetzter Richtung ihrer Deklarierung in der **MergedDictionaries**-Eigenschaft überprüft. Wenn im folgenden Beispiel sowohl Dictionary2.xaml als auch Dictionary1.xaml denselben Schlüssel deklarieren, wird zuerst der Schlüssel aus Dictionary2.xaml verwendet, weil er der letzte im **MergedDictionaries**-Satz ist.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml"/>
                <ResourceDictionary Source="Dictionary2.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource brush}" Text="greetings!" VerticalAlignment="Center"/>
</Page>
```

Innerhalb des Bereichs eines [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) wird das Verzeichnis auf die Eindeutigkeit der Schlüssel überprüft. Dieser Bereich erstreckt sich jedoch nicht über verschiedene Elemente in [**MergedDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208801)-Dateien.

Sie können die Kombination aus Suchsequenz und fehlender Erzwingung eindeutiger Schlüssel für Bereiche mit zusammengeführten Verzeichnissen verwenden, um eine Fallbackwertesequenz von [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)-Ressourcen zu erstellen. Sie können beispielsweise Benutzereinstellungen für eine bestimmte Pinselfarbe im letzten zusammengeführten Ressourcenverzeichnis in der Sequenz speichern, indem Sie ein Ressourcenverzeichnis verwenden, das mit Ihrem App-Status und den Benutzereinstellungsdaten synchronisiert wird. Wenn jedoch noch keine Benutzereinstellungen vorhanden sind, können Sie dieselbe Schlüsselzeichenfolge für eine **ResourceDictionary**-Ressource in der ursprünglichen [**MergedDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208801)-Datei definieren. Diese kann als Fallbackwert verwendet werden. Beachten Sie, dass die von Ihnen in einem primären Ressourcenverzeichnis bereitgestellten Werte immer geprüft werden, bevor eine Überprüfung der zusammengeführten Verzeichnisse erfolgt. Wenn Sie die Fallbacktechnik verwenden möchten, definieren Sie daher die Ressource nicht in einem primären Ressourcenverzeichnis.

## Designressourcen und Designverzeichnisse

Eine [ThemeResource](../xaml-platform/themeresource-markup-extension.md) ähnelt einer [StaticResource](../xaml-platform/staticresource-markup-extension.md), aber die Ressourcensuche wird neu ausgewertet, wenn sich das Design ändert.

In diesem Beispiel legen Sie den Vordergrund eines [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)-Elements auf einen Wert aus dem aktuellen Design fest.

```XAML
<TextBlock Text="hello world" Foreground="{ThemeResource FocusVisualWhiteStrokeThemeBrush}" VerticalAlignment="Center"/>
```

Bei einem Designverzeichnis handelt es sich um einen speziellen Typ zusammengeführter Verzeichnisse. Darin sind die Ressourcen gespeichert, die in Abhängigkeit davon variieren, welches Design ein Benutzer momentan auf seinem Gerät verwendet. Beispielsweise kann für das Design „Hell“ ein Pinsel mit weißer Farbe verwendet werden, während für das Design „Dunkel“ ein Pinsel mit dunkler Farbe verwendet wird. Der Pinsel ändert die Ressource, in der er aufgelöst wird, aber die Komposition eines Steuerelements, für das der Pinsel als Ressource verwendet wird, kann identisch sein. Nur die Designressource ändert sich. Um das Designwechselverhalten in Ihren eigenen Vorlagen und Stilen zu reproduzieren, müssen Sie, anstatt diese Elemente mit [**MergedDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208801) als Eigenschaft in den Hauptverzeichnissen zusammenzuführen, die [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208807)-Eigenschaft verwenden.

Hierbei muss jedes [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)-Element in [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208807) über einen [x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787)-Wert verfügen. Dieser Wert ist eine Zeichenfolge zur Benennung des relevanten Design – etwa „Default“, „Dark“, „Light“ oder „HighContrast“. Mit `Dictionary1` und `Dictionary2` werden üblicherweise Ressourcen mit gleichem Namen und unterschiedlichen Werten definiert.

Hier wird roter Text für das helle Design und blauer Text für das dunkle Design verwendet.

```XAML
<!-- Dictionary1.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="Red"/>

</ResourceDictionary>

<!—Dictionary2.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="blue"/>

</ResourceDictionary>
```

In diesem Beispiel legen Sie den Vordergrund eines [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)-Elements auf einen Wert aus dem aktuellen Design fest.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.ThemeDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml" x:Key="Light"/>
                <ResourceDictionary Source="Dictionary2.xaml" x:Key="Dark"/>
            </ResourceDictionary.ThemeDictionaries>
        </ResourceDictionary>
    </Page.Resources>
    <TextBlock Foreground="{StaticResource brush}" Text="hello world" VerticalAlignment="Center"/>
</Page>
```

Bei Designverzeichnissen ändert sich das für die Ressourcensuche zu verwendende aktive Verzeichnis dynamisch, wenn mit einer [ThemeResource-Markuperweiterung](../xaml-platform/themeresource-markup-extension.md) ein Verweis erstellt wird und das System eine Designänderung erkennt. Das Suchverhalten des Systems basiert auf der Zuordnung des aktiven Designs zum [x:Key](https://msdn.microsoft.com/library/windows/apps/mt204787)-Element eines spezifischen Designverzeichnisses.

Eine Untersuchung der Strukturierung der Designverzeichnisse in den XAML-Standarddesignressourcen kann sehr hilfreich sein. Die XAML-Standarddesignressourcen entsprechen den Vorlagen, die die Windows-Runtime standardmäßig für ihre Steuerelemente verwendet. Öffnen Sie die XAML-Dateien in „\\(Programme)\\Windows Kits\\10\\DesignTime\\CommonConfiguration\\Neutral\\UAP\\&lt;SDK-Version&gt;\\Generic“ mit einem Text-Editor oder Ihrer IDE. Beachten Sie, dass die Designverzeichnisse zuerst in „generic.xaml“ definiert und von den einzelnen Designverzeichnissen jeweils die gleichen Schlüssel definiert werden. Alle diese Schlüssel werden anschließend von Kompositionselementen in den verschiedenen Schlüsselelementen referenziert, die sich außerhalb der Designverzeichnisse befinden und später im XAML definiert werden. Außerdem gibt es eine separate Datei „themeresources.xaml“ für Designs, die nur die Designressourcen und zusätzlichen Vorlagen enthält, aber nicht die standardmäßigen Steuerelementvorlagen. Die Designbereiche sind Duplikate der Bereiche in „generic.xaml“.

Wenn Sie Kopien von Stilen oder Vorlagen mit XAML-Designtools bearbeiten, extrahieren die Designtools Abschnitte der XAML-Designressourcenverzeichnisse und platzieren diese als lokale Kopien von XAML-Verzeichniselementen, die Teil Ihrer App und Ihres Projekts sind.

Weitere Informationen und eine Liste mit den verfügbaren designspezifischen Ressourcen sowie Systemressourcen für Ihre App finden Sie unter [XAML-Designressourcen](xaml-theme-resources.md).

## Suchverhalten für XAML-Ressourcenverweise

Der Begriff *Suchverhalten* beschreibt die Vorgehensweise des XAML-Ressourcensystems bei der Suche nach einer XAML-Ressource. Die Suche wird durchgeführt, wenn ein Schlüssel im XAML-Code der App als XAML-Ressourcenverweis referenziert wird. Aufgrund des Verhaltens des Ressourcensystems ist vorhersehbar, wo auf der Grundlage des Bereichs nach einer Ressource gesucht wird. Wenn im Anfangsbereich keine Ressource gefunden wird, wird der Bereich erweitert. Das Suchverhalten wird für alle Positionen und Bereiche beibehalten, an bzw. in denen eine XAML-Ressource durch eine App oder das System definiert sein kann. Falls keine der möglichen Ressourcensuchen erfolgreich ist, tritt häufig ein Fehler auf. In der Regel können diese Fehler während des Entwicklungsprozesses behoben werden.

Das Suchverhalten für XAML-Ressourcenverweise beginnt mit dem Objekt, bei dem die tatsächliche Verwendung angewendet wird, und mit dessen eigener [**Resources**](https://msdn.microsoft.com/library/windows/apps/br208740)-Eigenschaft. Wenn dort ein [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) vorhanden ist, wird dieses **ResourceDictionary** auf ein Element überprüft, das den angeforderten Schlüssel enthält. Diese erste Suchebene ist nur selten relevant, weil für dasselbe Objekt normalerweise keine Ressource definiert und anschließend referenziert wird. Vielmehr ist an dieser Stelle oft keine **Resources**-Eigenschaft vorhanden. Sie können XAML-Ressourcenverweise nahezu überall in XAML erstellen. Dabei sind Sie nicht auf Eigenschaften von [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)-Unterklassen beschränkt.

Anschließend überprüft die Suchsequenz das nächste übergeordnete Objekt in der Laufzeitobjektstruktur der App. Wenn eine [**FrameworkElement.Resources**](https://msdn.microsoft.com/library/windows/apps/br208740)-Eigenschaft vorhanden ist und ein [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) enthält, wird das Verzeichniselement mit der angegebenen Schlüsselzeichenfolge angefordert. Wenn die Ressource gefunden wird, hält die Suchsequenz an, und das Objekt wird an dem Speicherort angegeben, an dem die Referenz erstellt wurde. Andernfalls wechselt das Suchverhalten zur nächsten übergeordneten Ebene in Richtung des Objektstrukturstamms. Die Suche wird bis zum Erreichen des XAML-Stammelements rekursiv nach oben fortgesetzt, wobei die Suche an allen möglichen Speicherorten für direkte Ressourcen erfolgt.

> **Hinweis**
            &nbsp;&nbsp;Es ist üblich, alle direkten Ressourcen auf Stammebene einer Seite zu definieren, um sowohl die Vorteile dieses Ressourcensuchverhaltens zu nutzen als auch eine Konvention des XAML-Markupstils einzuhalten.

 

Wenn die angeforderte Ressource in den direkten Ressourcen nicht gefunden werden kann, besteht der nächste Schritt bei der Suche in der Überprüfung der [**Application.Resources**](https://msdn.microsoft.com/library/windows/apps/br242338)-Eigenschaft. Die **Application.Resources**-Eigenschaft eignet sich optimal für die Positionierung von App-spezifischen Ressourcen, auf die in der Navigationsstruktur Ihrer App durch mehrere Seiten verwiesen wird.

Für Steuerelementvorlagen ist in der Referenzsuche ein weiterer möglicher Speicherort vorhanden: Designverzeichnisse. Bei einem Designverzeichnis handelt es sich um eine einzelne XAML-Datei mit einem [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)-Element als Stamm. Bei einem Designverzeichnis kann es sich um ein zusammengeführtes Verzeichnis aus [**Application.Resources**](https://msdn.microsoft.com/library/windows/apps/br242338) handeln. Das Designverzeichnis kann auch das steuerelementspezifische Designverzeichnis für ein benutzerdefiniertes Steuerelement mit Vorlagen sein.

Schlussendlich ist eine Ressourcensuche für Plattformressourcen vorhanden. Plattformressourcen enthalten die Steuerelementvorlagen, die für die einzelnen Designs der System-UI definiert werden und mit deren Hilfe die Standarddarstellung aller Steuerelemente definiert wird, die Sie für die UI in einer Windows-Runtime-App verwenden. Zu den Plattformressourcen zählt auch ein Satz benannter Ressourcen im Zusammenhang mit systemweiter Darstellung und Designs. Diese Ressourcen sind aus technischer Sicht ein [**MergedDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208801)-Element und daher nach dem Laden der App für die XAML- oder Codesuche verfügbar. Die Systemdesignressourcen enthalten z. B. eine Ressource mit dem Namen „SystemColorWindowTextColor“, die eine [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723)-Definition zum Abgleichen einer App-Textfarbe mit der Textfarbe eines Systemfensters bereitstellt, die vom Betriebssystem und den Benutzereinstellungen stammt. Andere XAML-Stile für Ihre App können auf diesen Stil verweisen, oder Sie können im Code einen Ressourcensuchwert abrufen (und im Beispielfall in **Color** umwandeln).

Weitere Informationen und eine Liste der verfügbaren designspezifischen Ressourcen sowie Systemressourcen für eine Windows Store-App, die XAML verwendet, finden Sie unter [XAML-Designressourcen](xaml-theme-resources.md).

Wenn der angeforderte Schlüssel an diesen Speicherorten immer noch nicht gefunden werden kann, tritt für die XAML-Analyse ein Fehler oder eine Ausnahme auf. Unter bestimmten Umständen kann es sich bei der XAML-Analyseausnahme um eine Laufzeitausnahme handeln, die weder von einer XAML-Markupkompilierung, noch von einer XAML-Designumgebung erkannt wird.

Aufgrund des mehrstufigen Suchverhaltens für Ressourcenverzeichnisse können Sie bewusst mehrere Ressourcenelemente definieren, die jeweils denselben Zeichenfolgenwert wie der Schlüssel haben. Die einzelnen Ressourcen müssen jedoch auf unterschiedlichen Ebenen definiert sein. Anders ausgedrückt: Auch wenn die Schlüssel in einem bestimmten [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) eindeutig sein müssen, gilt die Anforderung an die Eindeutigkeit jedoch nicht für die gesamte Sequenz des Suchverhaltens. Während der Suche wird für den XAML-Ressourcenverweis nur das erste erfolgreich abgerufene Objekt dieses Typs verwendet. Anschließend wird die Suche beendet. Sie können dieses Verhalten verwenden, um eine XAML-Ressource per Schlüssel an verschiedenen Positionen im XAML-Code Ihrer App je nach Bereich, von dem der XAML-Ressourcenverweis ausgegangen ist, und nach dem jeweiligen Verhalten der Suche anzufordern.

##  Vorwärtsverweise in einem ResourceDictionary


XAML-Ressourcenverweise in einem bestimmten Ressourcenverzeichnis müssen auf eine Ressource verweisen, die bereits mit einem Schlüssel definiert wurde. Diese Ressource muss lexikalisch vor dem Ressourcenverweis angezeigt werden. Vorwärtsverweise können nicht durch einen XAML-Ressourcenverweis aufgelöst werden. Aus diesem Grund müssen Sie bei Verwendung von XAML-Ressourcenverweisen in einer anderen Ressource die Ressourcenverzeichnisstruktur so konzipieren, dass die von anderen Ressourcen verwendeten Ressourcen in einem Ressourcenverzeichnis zuerst definiert werden.

Auf App-Ebene definierte Ressourcen können nicht auf direkte Ressourcen verweisen. Dies ist gleichbedeutend mit einem Vorwärtsverweis, da die App-Ressourcen tatsächlich zuerst verarbeitet werden (wenn die App zuerst gestartet wird, noch vor dem Laden von Navigationsseiteninhalten). Direkte Ressourcen können jedoch auf eine App-Ressource verweisen. Diese Tatsache kann eine hilfreiche Technik zum Vermeiden von Situationen mit Vorwärtsverweisen sein.

## XAML-Ressourcen müssen freigabefähig sein


Damit ein Objekt in [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) vorhanden sein kann, muss das Objekt *freigabefähig* sein.

Die Freigabefähigkeit ist aus folgendem Grund erforderlich: Wenn die Objektstruktur einer App zur Laufzeit erstellt und verwendet wird, können Objekte in der Struktur nicht an mehreren Stellen gleichzeitig vorhanden sein. Das Ressourcensystem erstellt intern Kopien von Ressourcenwerten, die im Objektgraphen Ihrer App verwendet werden, wenn die einzelnen XAML-Ressourcen angefordert werden.

Ein [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) und Windows-Runtime-XAML unterstützen die folgenden Objekte in der Regel für die freigabefähige Verwendung:

-   Stile und Vorlagen ([**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) und von [**FrameworkTemplate**](https://msdn.microsoft.com/library/windows/apps/br208753) abgeleitete Klassen)
-   Pinsel und Farben (von [**Brush**](https://msdn.microsoft.com/library/windows/apps/br228076) abgeleitete Klassen und [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723)-Werte)
-   Animationstypen, einschließlich [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490)
-   Transformationen (von [**GeneralTransform**](https://msdn.microsoft.com/library/windows/apps/br210034) abgeleitete Klassen)
-   [
              **Matrix**
            ](https://msdn.microsoft.com/library/windows/apps/br210127) und [**Matrix3D**](https://msdn.microsoft.com/library/windows/apps/br243266)
-   [
              **Point**
            ](https://msdn.microsoft.com/library/windows/apps/br225870)-Werte
-   Bestimmte andere UI-bezogene Strukturen wie [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864) und [**CornerRadius**](https://msdn.microsoft.com/library/windows/apps/br242343)
-   [Systeminterne XAML-Datentypen](https://msdn.microsoft.com/library/windows/apps/mt186448)

Sie können auch benutzerdefinierte Typen als freigabefähige Ressource verwenden, wenn Sie die erforderlichen Implementierungsmuster einhalten. Solche Klassen werden im zugrunde liegenden Code (oder in einbezogene Laufzeitkomponenten) definiert und dann in XAML als Ressource instantiiert. Beispiele dafür sind Objektdatenquellen und [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/br209903)-Implementierungen für die Datenbindung.

Benutzerdefinierte Typen müssen einen Standardkonstruktor besitzen, da dieser von einem XAML-Parser für die Instanziierung einer Klasse verwendet wird. Benutzerdefinierte Typen, die als Ressourcen verwendet werden, können in ihrer Vererbung die [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)-Klasse nicht besitzen, weil ein **UIElement** niemals freigabefähig sein kann (sie stellt immer genau ein UI-Element dar, das an einer Position im Objektgraphen Ihrer Laufzeit-App vorhanden ist).

## UserControl-Verwendungsbereich


Ein [**UserControl**](https://msdn.microsoft.com/library/windows/apps/br227647)-Element umfasst eine spezielle Situation für das Ressourcensuchverhalten, da es die vererbten Konzepte eines Definitions- und Verwendungsbereichs enthält. Ein **UserControl**-Element mit einem XAML-Ressourcenverweis aus dem zugehörigen Definitionsbereich muss die Suche der jeweiligen Ressource innerhalb der eigenen Suchsequenz für den Definitionsbereich unterstützen können. Das heißt, es kann nicht auf App-Ressourcen zugreifen. In einem **UserControl**-Verwendungsbereich wird ein Ressourcenverweis so behandelt, als würde er sich innerhalb der Suchsequenz in Richtung des zugehörigen Seitenstamms befinden (wie eine andere Ressourcenreferenz, die von einem Objekt in einer geladenen Objektstruktur erstellt wird) und als könnte sie auf App-Ressourcen zugreifen.

## ResourceDictionary und XamlReader.Load

Sie können ein [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) als Stamm oder als Teil der XAML-Eingabe für die [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048)-Methode verwenden. Sie können in diesen XAML-Code auch XAML-Ressourcenverweise einfügen, wenn all diese Referenzen im XAML-Code, der zum Laden übermittelt wird, vollständig eigenständig sind. **XamlReader.Load** analysiert den XAML-Code in einem Kontext, in dem keine anderen **ResourceDictionary**-Objekte beachtet werden, nicht einmal [**Application.Resources**](https://msdn.microsoft.com/library/windows/apps/br242338). Außerdem sollten Sie `{ThemeResource}` nicht in XAML-Code verwenden, der an **XamlReader.Load** übermittelt wird.

## Verwenden eines ResourceDictionary aus Code

Die meisten Szenarien für ein [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) werden ausschließlich im XAML-Code behandelt. Sie deklarieren den **ResourceDictionary**-Container und die darin enthaltenen Ressourcen als XAML-Datei oder Gruppe von XAML-Knoten in einer UI-Definitionsdatei. Anschließend nutzen Sie XAML-Ressourcenverweise, um diese Ressourcen aus anderen Teilen des XAML-Codes anzufordern. Es gibt trotzdem noch bestimmte Fälle, in denen die App den Inhalt eines **ResourceDictionary**-Elements mithilfe von Code anpassen sollte, der bei laufender App ausgeführt wird, oder in denen wenigstens der Inhalt eines **ResourceDictionary**-Elements daraufhin abgefragt werden sollte, ob eine Ressource bereits definiert ist. Diese Codeaufrufe werden für eine **ResourceDictionary**-Instanz erstellt. Daher müssen Sie eine Instanz abrufen, und zwar entweder ein direktes **ResourceDictionary** in der Objektstruktur durch Abrufen von [**FrameworkElement.Resources**](https://msdn.microsoft.com/library/windows/apps/br208740) oder `Application.Current.Resources`.

In C\# - oder Microsoft Visual Basic-Code können Sie in einem bestimmten [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) mithilfe des Indexers ([**Item**](https://msdn.microsoft.com/library/windows/apps/jj603134)) auf eine Ressource verweisen. Bei einem **ResourceDictionary** handelt es sich um ein Verzeichnis mit Zeichenfolgenschlüsseln, sodass vom Indexer anstelle eines Ganzzahlindex der Zeichenfolgenschlüssel verwendet wird. Für Visual C++-Komponentenerweiterungscode (C++/CX) verwenden Sie [**Lookup**](https://msdn.microsoft.com/library/windows/apps/br208800).

Bei der Verwendung von Code zum Untersuchen oder Ändern eines [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)-Elements wechselt das Verhalten für APIs wie [**Lookup**](https://msdn.microsoft.com/library/windows/apps/br208800) oder [**Item**](https://msdn.microsoft.com/library/windows/apps/jj603134) nicht von direkten Ressourcen zu App-Ressourcen. Dies ist das Verhalten eines XAML-Parsers, das nur beim Laden von XAML-Seiten auftritt. Zur Laufzeit gilt der Bereich für Schlüssel eigenständig für die **ResourceDictionary**-Instanz, die Sie gerade verwenden. Der Bereich wird jedoch auf [**MergedDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208801) erweitert.

Wenn Sie zudem einen Schlüssel anfordern, der im [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) nicht vorhanden ist, tritt ggf. kein Fehler auf. Für den Rückgabewert kann einfach **null** angegeben werden. Unter Umständen erhalten Sie dennoch einen Fehler, wenn Sie versuchen, das zurückgebende Ergebnis **null** als Wert zu verwenden. Der Fehler würde durch den Setter für die Eigenschaft ausgelöst, nicht durch Ihren **ResourceDictionary**-Aufruf. Ein Fehler kann nur dann vermieden werden, wenn die Eigenschaft **null** als gültigen Wert akzeptiert. Beachten Sie, wie sich dieses Verhalten vom XAML-Suchverhalten zur XAML-Analysezeit unterscheidet. Wenn der bereitgestellte Schlüssel aus dem XAML-Code zur Analysezeit nicht aufgelöst werden kann, tritt auch dann ein XAML-Analysefehler auf, wenn **null** von der Eigenschaft akzeptiert worden wäre.

Zusammengeführte Ressourcenverzeichnisse werden in den Indexbereich des primären Ressourcenverzeichnisses einbezogen, das zur Laufzeit auf das zusammengeführte Verzeichnis verweist. Das heißt, dass Sie **Item** oder [**Lookup**](https://msdn.microsoft.com/library/windows/apps/br208800) des primären Verzeichnisses zum Suchen von Objekten verwenden können, die tatsächlich im zusammengeführten Verzeichnis definiert wurden. In diesem Fall ähnelt das Suchverhalten dem XAML-Suchverhalten zur Analysezeit: Wenn zusammengeführte Verzeichnisse mit dem gleichen Schlüssel mehrere Objekte enthalten, wird das Objekt des zuletzt hinzugefügten Verzeichnisses zurückgegeben.

Sie können einem vorhandenen [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)-Element Elemente hinzufügen, indem Sie **Add** (C\# oder Visual Basic) oder [**Insert**](https://msdn.microsoft.com/library/windows/apps/br208799) (C++/CX) aufrufen. Die Elemente können sowohl direkten Ressourcen als auch App-Ressourcen hinzugefügt werden. Für jeden dieser API-Aufrufe ist ein Schlüssel erforderlich, um die Anforderung zu erfüllen, dass jedes Element in einem **ResourceDictionary**-Element über einen Schlüssel verfügen muss. Elemente, die Sie einem **ResourceDictionary**-Element zur Laufzeit hinzufügen, sind für XAML-Ressourcenverweise allerdings nicht relevant. Die erforderliche Suche nach XAML-Ressourcenverweisen wird durchgeführt, wenn der XAML-Code beim Laden der App (oder bei Ermittlung einer Designänderung) zum ersten Mal analysiert wird. Ressourcen, die Sammlungen zur Laufzeit hinzugefügt wurden, waren zu diesem Zeitpunkt noch nicht verfügbar. Eine Änderung des **ResourceDictionary**-Elements führt nicht dazu, dass eine daraus bereits abgerufene Ressource auch dann nicht ungültig wird, wenn Sie den Wert dieser Ressource ändern.

Sie haben auch die Möglichkeit, zur Laufzeit Elemente aus einem [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) zu entfernen, Kopien von einigen oder allen Elementen zu erstellen und andere Vorgänge auszuführen. Die Elementliste für **ResourceDictionary** gibt an, welche APIs verfügbar sind. Da das **ResourceDictionary** eine projizierte API zur Unterstützung der zugrunde liegenden Sammlungsschnittstelle aufweist, gilt es zu beachten, dass Ihre API-Optionen je nach Verwendung von C\# oder Visual Basic anstelle von C++/CX variieren.

## ResourceDictionary und Lokalisierung


Ein XAML-[**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) enthält anfangs unter Umständen Zeichenfolgen, die lokalisiert werden müssen. Wenn dies der Fall ist, speichern Sie diese Zeichenfolgen nicht in einem **ResourceDictionary**, sondern als Projektressourcen. Entfernen Sie die Zeichenfolgen aus dem XAML, und weisen Sie dem besitzenden Element einen Wert für die [x:Uid-Direktive](https://msdn.microsoft.com/library/windows/apps/mt204791) zu. Definieren Sie anschließend eine Ressource in einer Ressourcendatei. Geben Sie einen Ressourcennamen im Format *XUIDValue*.*PropertyName* und einen Ressourcenwert für die zu lokalisierende Zeichenfolge an.

## Benutzerdefinierte Ressourcensuche

Für erweiterte Szenarien können Sie eine Klasse implementieren, die von dem hier beschriebenen Suchverhalten für XAML-Ressourcenverweise abweicht. Implementieren Sie dazu die [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327)-Klasse. Sie können dann mit der [CustomResource-Markuperweiterung](https://msdn.microsoft.com/library/windows/apps/mt185580) statt über [StaticResource](../xaml-platform/staticresource-markup-extension.md) oder [ThemeResource](../xaml-platform/themeresource-markup-extension.md) auf dieses Verhalten zugreifen. Die meisten Apps verfügen nicht über Szenarien, für die dies erforderlich ist. Weitere Informationen finden Sie unter [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327).

 
## Verwandte Themen

[**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)

[Übersicht über XAML](https://msdn.microsoft.com/library/windows/apps/mt185595)

[StaticResource-Markuperweiterung](../xaml-platform/staticresource-markup-extension.md)

[ThemeResource-Markuperweiterung](../xaml-platform/themeresource-markup-extension.md)

[XAML-Designressourcen](xaml-theme-resources.md)

[Formatieren von Steuerelementen](styling-controls.md)

[x:Key-Attribut](https://msdn.microsoft.com/library/windows/apps/mt204787)

 

 






<!--HONumber=Jun16_HO4-->


