---
ms.assetid: 0C69521B-47E0-421F-857B-851B0E9605F2
title: Binden von hierarchischen Daten und Erstellen einer Master/Details-Ansicht
description: Sie können eine Master/Details-Ansicht mit mehreren Ebenen (auch bekannt als Listen-Details-Ansicht) von hierarchischen Daten erstellen, indem Sie Elementsteuerelemente an CollectionViewSource-Instanzen binden, die in einer Kette verbunden sind.
---
# Binden von hierarchischen Daten und Erstellen einer Master/Details-Ansicht

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


> **Hinweis** Siehe auch das [Master/Details-Beispiel](http://go.microsoft.com/fwlink/p/?linkid=619991).

Sie können eine Master/Details-Ansicht mit mehreren Ebenen (auch bekannt als Listen-Details-Ansicht) von hierarchischen Daten erstellen, indem Sie Elementsteuerelemente an [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833)-Instanzen binden, die in einer Kette verbunden sind. In diesem Thema verwenden wir nach Möglichkeit die [{x:Bind}-Markuperweiterung](https://msdn.microsoft.com/library/windows/apps/Mt204783) und die flexiblere (aber weniger leistungsfähige) [{Binding}-Markuperweiterung](https://msdn.microsoft.com/library/windows/apps/Mt204782), wenn nötig.

Eine häufig verwendete Struktur für UWP-Apps (Universelle Windows-Plattform) ist die Navigation zu unterschiedlichen Detailseiten, wenn der Benutzer in einer Masterliste eine Auswahl trifft. Dies ist hilfreich, wenn Sie eine anschauliche visuelle Darstellung jedes Elements auf allen Ebenen in einer Hierarchie erzielen möchten. Eine weitere Möglichkeit ist die Anzeige von mehreren Datenebenen auf einer einzelnen Seite. Dies ist nützlich, wenn Sie einige einfache Listen anzeigen möchten, in denen der Benutzer schnell Detailinformationen für ein bestimmtes Element anzeigen kann. In diesem Thema wird die Implementierung dieser Interaktion beschrieben. Die [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833)-Instanzen verfolgen die aktuelle Auswahl auf jeder Hierarchieebene nach.

Wir erstellen eine Ansicht einer Sportmannschaftshierarchie, die in Listen für Ligen, Divisionen und Mannschaften unterteilt ist und eine Detailansicht für Mannschaften enthält. Wenn Sie ein Element in einer Liste auswählen, werden die nachfolgenden Ansichten automatisch aktualisiert.

![Master/Details-Ansicht einer Sporthierarchie](images/xaml-masterdetails.png)

## Voraussetzungen

In diesem Thema wird vorausgesetzt, dass Sie eine UWP-App erstellen können. Anweisungen zum Erstellen Ihrer ersten UWP-App finden Sie unter [Erstellen Ihrer ersten UWP-App mit C# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/Hh974581).

## Erstellen des Projekts

Erstellen Sie ein neues Projekt vom Typ **Leere Anwendung (Windows Universal)**. Weisen Sie ihm den Namen „MasterDetailsBinding“ zu.

## Erstellen des Datenmodells

Fügen Sie Ihrem Projekt eine neue Klasse hinzu, nennen Sie sie „ViewModel.cs“, und fügen Sie ihr diesen Code hinzu. Dies ist die Bindungsquellklasse.

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace MasterDetailsBinding
{
    public class Team
    {
        public string Name { get; set; }
        public int Wins { get; set; }
        public int Losses { get; set; }
    }

    public class Division
    {
        public string Name { get; set; }
        public IEnumerable&lt;Team&gt; Teams { get; set; }
    }

    public class League
    {
        public string Name { get; set; }
        public IEnumerable&lt;Division&gt; Divisions { get; set; }
    }

    public class LeagueList : List&lt;League&gt;
    {
        public LeagueList()
        {
            this.AddRange(GetLeague().ToList());
        }

        public IEnumerable&lt;League&gt; GetLeague()
        {
            return from x in Enumerable.Range(1, 2)
                   select new League
                   {
                       Name = &quot;League &quot; + x,
                       Divisions = GetDivisions(x).ToList()
                   };
        }

        public IEnumerable&lt;Division&gt; GetDivisions(int x)
        {
            return from y in Enumerable.Range(1, 3)
                   select new Division
                   {
                       Name = String.Format(&quot;Division {0}-{1}&quot;, x, y),
                       Teams = GetTeams(x, y).ToList()
                   };
        }

        public IEnumerable&lt;Team&gt; GetTeams(int x, int y)
        {
            return from z in Enumerable.Range(1, 4)
                   select new Team
                   {
                       Name = String.Format(&quot;Team {0}-{1}-{2}&quot;, x, y, z),
                       Wins = 25 - (x * y * z),
                       Losses = x * y * z
                   };
        }
    }
}
```

## Erstellen der Ansicht

Machen Sie als Nächstes die Bindungsquellklasse aus der Klasse verfügbar, die die Markupseite darstellt. Zu diesem Zweck fügen Sie eine Eigenschaft vom Typ **LeagueList** zu **MainPage** hinzu.

```cs
namespace MasterDetailsBinding
{
    /// &lt;summary&gt;
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// &lt;/summary&gt;
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new LeagueList();
        }
        public LeagueList ViewModel { get; set; }
    }
}
```

Schließlich ersetzen Sie den Inhalt der Datei „MainPage.xaml“ durch das folgende Markup, mit dem drei [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833)-Instanzen deklariert und zu einer Kette verbunden werden. Die nachfolgenden Steuerelemente können dann abhängig von der Hierarchieebene an die entsprechende **CollectionViewSource**-Instanz gebunden werden.

```xaml
<Page
    x:Class=&quot;MasterDetailsBinding.MainPage&quot;
    xmlns=&quot;http://schemas.microsoft.com/winfx/2006/xaml/presentation&quot;
    xmlns:x=&quot;http://schemas.microsoft.com/winfx/2006/xaml&quot;
    xmlns:local=&quot;using:MasterDetailsBinding&quot;
    xmlns:d=&quot;http://schemas.microsoft.com/expression/blend/2008&quot;
    xmlns:mc=&quot;http://schemas.openxmlformats.org/markup-compatibility/2006&quot;
    mc:Ignorable=&quot;d&quot;>

    <Page.Resources>
        <CollectionViewSource x:Name=&quot;Leagues&quot;
            Source=&quot;{x:Bind ViewModel}&quot;/>
        <CollectionViewSource x:Name=&quot;Divisions&quot;
            Source=&quot;{Binding Divisions, Source={StaticResource Leagues}}&quot;/>
        <CollectionViewSource x:Name=&quot;Teams&quot;
            Source=&quot;{Binding Teams, Source={StaticResource Divisions}}&quot;/>

        <Style TargetType=&quot;TextBlock&quot;>
            <Setter Property=&quot;FontSize&quot; Value=&quot;15&quot;/>
            <Setter Property=&quot;FontWeight&quot; Value=&quot;Bold&quot;/>
        </Style>

        <Style TargetType=&quot;ListBox&quot;>
            <Setter Property=&quot;FontSize&quot; Value=&quot;15&quot;/>
        </Style>

        <Style TargetType=&quot;ContentControl&quot;>
            <Setter Property=&quot;FontSize&quot; Value=&quot;15&quot;/>
        </Style>

    </Page.Resources>

    <Grid Background=&quot;{ThemeResource ApplicationPageBackgroundThemeBrush}&quot;>

        <StackPanel Orientation=&quot;Horizontal&quot;>

            <!-- All Leagues view -->

            <StackPanel Margin=&quot;5&quot;>
                <TextBlock Text=&quot;All Leagues&quot;/>
                <ListBox ItemsSource=&quot;{Binding Source={StaticResource Leagues}}&quot; 
                    DisplayMemberPath=&quot;Name&quot;/>
            </StackPanel>

            <!-- League/Divisions view -->

            <StackPanel Margin=&quot;5&quot;>
                <TextBlock Text=&quot;{Binding Name, Source={StaticResource Leagues}}&quot;/>
                <ListBox ItemsSource=&quot;{Binding Source={StaticResource Divisions}}&quot; 
                    DisplayMemberPath=&quot;Name&quot;/>
            </StackPanel>

            <!-- Division/Teams view -->

            <StackPanel Margin=&quot;5&quot;>
                <TextBlock Text=&quot;{Binding Name, Source={StaticResource Divisions}}&quot;/>
                <ListBox ItemsSource=&quot;{Binding Source={StaticResource Teams}}&quot; 
                    DisplayMemberPath=&quot;Name&quot;/>
            </StackPanel>

            <!-- Team view -->

            <ContentControl Content=&quot;{Binding Source={StaticResource Teams}}&quot;>
                <ContentControl.ContentTemplate>
                    <DataTemplate>
                        <StackPanel Margin=&quot;5&quot;>
                            <TextBlock Text=&quot;{Binding Name}&quot; 
                                FontSize=&quot;15&quot; FontWeight=&quot;Bold&quot;/>
                            <StackPanel Orientation=&quot;Horizontal&quot; Margin=&quot;10,10&quot;>
                                <TextBlock Text=&quot;Wins:&quot; Margin=&quot;0,0,5,0&quot;/>
                                <TextBlock Text=&quot;{Binding Wins}&quot;/>
                            </StackPanel>
                            <StackPanel Orientation=&quot;Horizontal&quot; Margin=&quot;10,0&quot;>
                                <TextBlock Text=&quot;Losses:&quot; Margin=&quot;0,0,5,0&quot;/>
                                <TextBlock Text=&quot;{Binding Losses}&quot;/>
                            </StackPanel>
                        </StackPanel>
                    </DataTemplate>
                </ContentControl.ContentTemplate>
            </ContentControl>

        </StackPanel>

    </Grid>
</Page>
```

Beachten Sie, dass Sie durch die direkte Bindung an die [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833)-Instanz implizieren, dass Sie in Bindungen, in denen der Pfad in der Sammlung selbst nicht gefunden werden kann, an das aktuelle Element binden möchten. Die **CurrentItem**-Eigenschaft muss nicht als Pfad für die Bindung angegeben werden, obwohl dies im Zweifelsfall möglich ist. Zum Beispiel wird die [**Content**](https://msdn.microsoft.com/library/windows/apps/BR209365-content)-Eigenschaft der [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365)-Klasse, welche die Teamansicht darstellt, an `Teams`**CollectionViewSource** gebunden. Die Steuerelemente in der [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348) werden jedoch an Eigenschaften der `Team`-Klasse gebunden, da die **CollectionViewSource** bei Bedarf automatisch das aktuell aus der Teamliste ausgewählte Team liefert.

 

 



<!--HONumber=Mar16_HO1-->


