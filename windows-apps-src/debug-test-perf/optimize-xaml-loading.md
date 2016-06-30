---
author: mcleblanc
ms.assetid: 569E8C27-FA01-41D8-80B9-1E3E637D5B99
title: Optimieren Ihres XAML-Markups
description: "Die Analyse von XAML-Markup zum Erstellen von Objekten im Arbeitsspeicher kann für eine komplexe Benutzeroberfläche viel Zeit in Anspruch nehmen. Hier finden Sie einige Punkte, die Sie zur Optimierung der XAML-Markupanalyse, Ladezeit und Effizienz des Arbeitsspeichers für Ihre App vornehmen können."
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c2131b084d8bb989f1f7767f54db697e1cdd8dcf

---
# Optimieren Ihres XAML-Markups

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Die Analyse von XAML-Markup zum Erstellen von Objekten im Arbeitsspeicher kann für eine komplexe Benutzeroberfläche viel Zeit in Anspruch nehmen. Hier finden Sie einige Punkte, die Sie zur Optimierung der XAML-Markupanalyse, Ladezeit und Effizienz des Arbeitsspeichers für Ihre App vornehmen können.

Beschränken Sie beim Start der App das zu ladende XAML-Markup auf die Elemente, die für die anfängliche Benutzeroberfläche erforderlich sind. Überprüfen Sie das Markup auf der ersten Seite, und stellen Sie sicher, dass es keine Elemente enthält, die nicht erforderlich sind. Falls eine Seite auf ein Steuerelement oder auf eine Ressource in einer anderen Datei verweist, wird auch diese Datei vom Framework analysiert.

Da „InitialPage.xaml“ in diesem Beispiel eine Ressource von „ExampleResourceDictionary.xaml“ verwendet, muss die gesamte Datei „ExampleResourceDictionary.xaml“ beim Start analysiert werden.

**InitialPage.xaml**

```xml
<Page x:Class="ExampleNamespace.InitialPage" ...> 
    <UserControl.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="ExampleResourceDictionary.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </UserControl.Resources>

    <Grid>
        <TextBox Foreground="{StaticResource TextBrush}"/>
    </Grid>
</Page>
```

**ExampleResourceDictionary.xaml**

```xml
<ResourceDictionary>
    <SolidColorBrush x:Key="TextBrush" Color="#FF3F42CC"/>

    <!--This ResourceDictionary contains many other resources that
    used in the app, but not during startup.-->
</ResourceDictionary>
```

Wenn Sie eine Ressource in der gesamten App auf vielen Seiten verwenden, ist es eine bewährte Methode, diese in der Datei „App.xaml“ zu speichern, um eine Duplizierung zu vermeiden. Aber „App.xaml“ wird beim Starten der App analysiert, daher sollte jede Ressource, die nur auf einer Seite verwendet wird (es sei denn, diese Seite ist die Ausgangsseite), unter den lokalen Ressourcen der Seite gespeichert werden. In diesem Gegenbeispiel wird die Datei „App.xaml“ veranschaulicht, die Ressourcen enthält, die nur von einer Seite verwendet werden (bei der es sich nicht um die Ausgangsseite handelt). Dadurch wird die Startzeit der App unnötigerweise erhöht.

**InitialPage.xaml**

```xml
<Page ...>  <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->   
    <StackPanel>  
        <TextBox Foreground="{StaticResource InitialPageTextBrush}"/> 
    </StackPanel> 
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**SecondPage.xaml**

```xml
<Page ...>  <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <StackPanel> 
        <Button Content="Submit" Foreground="{StaticResource SecondPageTextBrush}" /> 
    </StackPanel> 
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**App.xaml**

```xml
<Application ...> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
     <Application.Resources>  
        <SolidColorBrush x:Key="DefaultAppTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="InitialPageTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="SecondPageTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="ThirdPageTextBrush" Color="#FF3F42CC"/> 
    </Application.Resources> 
</Application> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

Die Möglichkeit, das obige Gegenbeispiel effizienter zu gestalten, besteht darin, `SecondPageTextBrush` in „SecondPage.xaml“ und `ThirdPageTextBrush` in „ThirdPage.xaml“ zu verschieben. `InitialPageTextBrush` kann in „App.xaml“ verbleiben, da Anwendungsressourcen in jedem Fall beim Start der App analysiert werden müssen.

## Minimieren der Elementanzahl

Obwohl die XAML-Plattform eine große Anzahl von Elementen anzeigen kann, können Sie das Layout und die Darstellung Ihrer App beschleunigen, indem Sie die geringste Anzahl von Elementen verwenden, um das gewünschte Erscheinungsbild zu erreichen.

-   Layoutpanel verfügen über eine [**Background**](https://msdn.microsoft.com/library/windows/apps/BR227512)-Eigenschaft, sodass es nicht erforderlich ist, ein [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) vor einem Panel zu platzieren, um ihm Farbe zu verleihen.

**Ineffizient**

```xml
<Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
        <Rectangle Fill="Black"/> 
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**Effizient**

```xml
<Grid Background="Black"/>
```

-   Wenn Sie das gleiche vektorbasierte Element häufig genug wiederverwenden, ist es effizienter, stattdessen ein [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752)-Element zu verwenden. Vektorbasierte Elemente können aufwendiger sein, da die CPU jedes einzelne Element separat erstellen muss. Die Bilddatei muss hingegen nur einmal decodiert werden.

## Zusammenfassen mehrerer gleichartiger Pinsel in einer Ressource

Die XAML-Plattform speichert allgemein verwendete Objekte zwischen, um eine möglichst häufige Wiederverwendung zu ermöglichen. XAML kann jedoch nicht ohne Weiteres feststellen, ob es sich bei einem Pinsel, der in zwei unterschiedlichen Markupkomponenten deklariert ist, um denselben Pinsel handelt. Dieses Beispiel verwendet [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) zur Veranschaulichung, aber die Wahrscheinlichkeit und Bedeutung von [**GradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210068) ist höher.

**Ineffizient**

```xml
<Page ... > <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <StackPanel>  
        <TextBlock>  
            <TextBlock.Foreground>  
                <SolidColorBrush Color="#FFFFA500"/> 
            </TextBlock.Foreground> 
        </TextBox> 
        <Button Content="Submit"> 
            <Button.Foreground> 
                <SolidColorBrush Color="#FFFFA500"/> 
            </Button.Foreground> 
        </Button> 
    </StackPanel> 
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

Prüfen Sie auch auf Pinsel, die vordefinierte Farben verwenden: `"Orange"` und `"#FFFFA500"` geben dieselbe Farbe an. Definieren Sie den Pinsel als Ressource, um die Duplizierung zu beheben. Wenn Steuerelemente auf anderen Seiten denselben Pinsel verwenden, verschieben Sie diese in die Datei „App.xaml“.

**Effizient**

```xml
<Page ... >
    <Page.Resources>
        <SolidColorBrush x:Key="BrandBrush" Color="#FFFFA500"/>
    </Page.Resources>
    
    <StackPanel>
        <TextBlock Foreground="{StaticResource BrandBrush}" />
        <Button Content="Submit" Foreground="{StaticResource BrandBrush}" />
    </StackPanel>
</Page>
```

## Minimieren der Überzeichnung

Eine Überzeichnung tritt dort auf, wo mehrere Objekte dieselben Bildschirmpixel beanspruchen. Beachten Sie, dass Sie manchmal einen Kompromiss zwischen dieser Richtlinie und dem Wunsch zur Minimierung der Elementanzahl eingehen müssen.

-   Wenn ein Element nicht angezeigt wird, da es transparent ist oder von anderen Elementen verdeckt wird, löschen Sie es einfach, wenn es nicht zum Layout beiträgt. Wenn das Element im anfänglichen visuellen Zustand nicht sichtbar ist, aber in anderen visuellen Zuständen angezeigt wird, legen Sie für [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) den Wert **Collapsed** für das Element selbst fest, und ändern Sie den Wert für die entsprechenden Zustände in **Visible**. Es gibt Ausnahmen von dieser Heuristik: In der Regel wird der Wert, den eine Eigenschaft in den meisten visuellen Zuständen aufweist, am besten lokal für das Element festgelegt.
-   Verwenden Sie ein Verbundelement, anstatt mehrere Elemente übereinander zu stapeln, um einen Effekt zu erzielen. In diesem Beispiel ist das Ergebnis eine zweifarbige Form, wobei die obere Hälfte schwarz (vom Hintergrund von [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)) und die untere Hälfte grau ist (vom halbtransparenten weißen [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371)-Objekt, das mit einer Alpha-Überblendung über den schwarzen Hintergrund von **Grid** gelegt wird). Hier werden 150 % der Pixel gefüllt, die erforderlich sind, um das Ergebnis zu erzielen.

**Ineffizient**
    
```xml
    <Grid Background="Black"> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
        <Grid.RowDefinitions> 
            <RowDefinition Height="*"/> 
            <RowDefinition Height="*"/> 
        </Grid.RowDefinitions> 
        <Rectangle Grid.Row="1" Fill="White" Opacity=".5"/> 
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**Effizient**

```xml
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <Rectangle Fill="Black"/>
        <Rectangle Grid.Row="1" Fill="#FF7F7F7F"/>
    </Grid>
```

-   Ein Layoutpanel kann zwei Zwecke verfolgen: einem Bereich eine Farbe zuzuweisen und untergeordnete Elemente darzustellen. Wenn ein Element, das sich in der Z-Ordnung weiter hinten befindet, einem Bereich bereits eine Farbe zuweist, dann muss ein darüber befindliches Layoutpanel diesem Bereich keine Farbe zuweisen. Stattdessen kann es sich darauf konzentrieren, seine untergeordneten Elemente zu gestalten. Im Folgenden finden Sie ein Beispiel hierfür.

**Ineffizient**

```xml
    <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <GridView Background="Blue">  
        <GridView.ItemTemplate> 
            <DataTemplate> 
                <Grid Background="Blue"/> 
            </DataTemplate> 
        </GridView.ItemTemplate> 
    </GridView> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**Effizient**

```xml
    <GridView Background="Blue">  
        <GridView.ItemTemplate> 
            <DataTemplate> 
                <Grid/> 
            </DataTemplate>
        </GridView.ItemTemplate>
    </GridView> 
```

Wenn für das [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)-Objekt Treffertests durchgeführt werden müssen, legen Sie für den Hintergrund einen transparenten Wert fest.

-   Verwenden Sie ein [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209253)-Element, um einen Rahmen um ein Objekt zu zeichnen. In diesem Beispiel wird ein [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)-Objekt als provisorischer Rahmen für ein [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683)-Steuerelement verwendet. Allerdings werden alle Pixel in der Mitte der Zelle überzeichnet.

**Ineffizient**

```xml
    <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <Grid Background="Blue" Width="300" Height="45"> 
        <Grid.RowDefinitions> 
            <RowDefinition Height="5"/> 
            <RowDefinition/> 
            <RowDefinition Height="5"/> 
        </Grid.RowDefinitions> 
        <Grid.ColumnDefinitions> 
            <ColumnDefinition Width="5"/> 
            <ColumnDefinition/> 
            <ColumnDefinition Width="5"/> 
        </Grid.ColumnDefinitions> 
        <TextBox Grid.Row="1" Grid.Column="1"></TextBox> 
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**Effizient**

```xml
    <Border BorderBrush="Blue" BorderThickness="5" Width="300" Height="45">
        <TextBox/>
    </Border>
```

-   Achten Sie auf die Ränder. Zwei benachbarte Elemente überlappen sich (möglicherweise versehentlich), wenn negative Ränder in die Rendergrenzen hineinreichen und eine Überzeichnung verursachen.

Verwenden Sie [**DebugSettings.IsOverdrawHeatMapEnabled**](https://msdn.microsoft.com/library/windows/apps/Hh701823) als visuelle Diagnose. Möglicherweise erkennen Sie, dass Objekte dargestellt werden, von denen Sie sich nicht bewusst waren, dass sie in der Szene auftreten.

## Zwischenspeichern statischer Inhalte

Eine andere Quelle für die Überzeichnung ist eine Form, die aus vielen überlappenden Elementen besteht. Wenn Sie [**CacheMode**](https://msdn.microsoft.com/library/windows/apps/BR228084) für die [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911)-Klasse mit der Verbundform auf **BitmapCache** festlegen, rendert die Plattform das Element für eine Bitmap einmalig und verwendet dann diese Bitmap für jeden Frame (anstelle der Überzeichnung).

**Ineffizient**

```xml
<Canvas Background="White">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

![Venn-Diagramm mit drei farbigen Kreisen](images/solidvenn.png)

Das obige Bild ist das Ergebnis, aber hier folgt eine Karte der überzeichneten Bereiche. Ein dunkleres Rot deutet auf ein höheres Maß an Überzeichnung hin.

![Venn-Diagramm mit Überschneidungsbereichen](images/translucentvenn.png)

**Effizient**

```xml
<Canvas Background="White" CacheMode="BitmapCache">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

Beachten Sie die Verwendung von [**CacheMode**](https://msdn.microsoft.com/library/windows/apps/BR228084). Verwenden Sie dieses Verfahren nicht, wenn eine der Teilformen animiert ist, da der Bitmapcache wahrscheinlich bei jedem Frame neu generiert werden muss, was dem Zweck widerspricht.

## Ressourcenwörterbücher

Ressourcenwörterbücher dienen im Allgemeinen zum Speichern von Ressourcen auf einer Art globalen Ebene. Ressourcen, auf die von Ihrer App an mehreren Orten verwiesen werden soll. Beispielsweise Stile, Pinsel, Vorlagen usw. Ressourcenwörterbücher wurden grundsätzlich dahingehend optimiert, dass nur angeforderte Ressourcen instanziiert werden. Es gibt jedoch einige wenige Gelegenheiten, bei denen Vorsicht geboten ist.

**Ressource mit „x:Name“**. Ressourcen mit „x:Name“ profitieren nicht von der Plattformoptimierung, sondern werden sofort bei der Erstellung von ResourceDictionary instanziiert. Der Grund: „x:Name“ teilt der Plattform mit, dass Ihre App Feldzugriff auf diese Ressource benötigt. Daher muss die Plattform etwas erstellen, für das ein Verweis erstellt werden kann.

**ResourceDictionaries in einem UserControl**. Innerhalb eines Benutzersteuerelements definierte Ressourcenwörterbücher führen zu Leistungseinbußen. Die Plattform erstellt für jede Instanz von UserControl eine Kopie von ResourceDictionary. Bei einem häufig verwendeten Benutzersteuerelement empfiehlt es sich daher, ResourceDictionary aus UserControl zu entfernen und auf der Seitenebene zu platzieren.

## Verwendung von XBF2

Bei XBF2 handelt es sich um eine binäre Darstellung des XAML-Markups, die sämtliche Nachteile der Textanalyse zur Laufzeit vermeidet. Darüber hinaus optimiert sie den Binärcode für das Laden und die Strukturerstellung und ermöglicht die Verwendung von Fast-Path für XAML-Typen, um die Effizienz bei der Heap- und Objekterstellung (VSM, ResourceDictionary, Stile usw.) zu verbessern. Dank vollständiger Abbildung im Speicher entsteht beim Laden und Lesen einer XAML-Seite keinerlei Heap-Bedarf. Des Weiteren verringert sich der Datenträgerbedarf gespeicherter XAML-Seiten in einem APPX-Element. XBF2 zeichnet sich durch eine kompaktere Darstellung aus und kann den Datenträgerbedarf im Vergleich zu XAML/XBF1-Dateien um bis zu 50 Prozent reduzieren. So wurde bei der integrierten Foto-App etwa eine Reduzierung um rund 60 Prozent erreicht – von ehemals rund 1 MB (XBF1-Ressourcen) auf ca. 400 KB (XBF2-Ressourcen). Darüber hinaus wurden für Apps auch Verbesserungen bei CPU (15 bis 20 Prozent) und Win32-Heap (10 bis 15 Prozent) verzeichnet.

Integrierte XAML-Steuerelemente und vom Framework bereitgestellte Wörterbücher sind bereits vollständig XBF2-fähig. Achten Sie bei Ihrer eigenen App darauf, dass Ihre Projektdatei mindestens „TargetPlatformVersion 8.2“ deklariert.

Wenn Sie wissen möchten, ob Sie über XBF2 verfügen, öffnen Sie Ihre App in einem Binär-Editor. Falls das 12. und 13. Byte „00 02“ ist, haben Sie XBF2.




<!--HONumber=Jun16_HO4-->


