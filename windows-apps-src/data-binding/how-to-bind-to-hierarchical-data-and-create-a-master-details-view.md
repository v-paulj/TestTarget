---
author: mcleblanc
ms.assetid: 0C69521B-47E0-421F-857B-851B0E9605F2
title: Binden von hierarchischen Daten und Erstellen einer Master/Details-Ansicht
description: "Sie können eine Master/Detailansicht mit mehreren Ebenen (auch bekannt als Listendetailansicht) hierarchischer Daten erstellen, indem Sie Elementsteuerelemente an CollectionViewSource-Instanzen binden, die in einer Kette verbunden sind."
translationtype: Human Translation
ms.sourcegitcommit: afb508fcbc2d4ab75188a2d4f705ea0bee385ed6
ms.openlocfilehash: 2ff66a1d6a80bb085f54dec8e35371ba0c9e6b27

---
# Binden von hierarchischen Daten und Erstellen einer Master/Details-Ansicht

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


> **Hinweis**  Weitere Informationen finden Sie im [Master/Details-Beispiel](http://go.microsoft.com/fwlink/p/?linkid=619991).

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
        public IEnumerable<Team> Teams { get; set; }
    }

    public class League
    {
        public string Name { get; set; }
        public IEnumerable<Division> Divisions { get; set; }
    }

    public class LeagueList : List<League>
    {
        public LeagueList()
        {
            this.AddRange(GetLeague().ToList());
        }

        public IEnumerable<League> GetLeague()
        {
            return from x in Enumerable.Range(1, 2)
                   select new League
                   {
                       Name = "League " + x,
                       Divisions = GetDivisions(x).ToList()
                   };
        }

        public IEnumerable<Division> GetDivisions(int x)
        {
            return from y in Enumerable.Range(1, 3)
                   select new Division
                   {
                       Name = String.Format("Division {0}-{1}", x, y),
                       Teams = GetTeams(x, y).ToList()
                   };
        }

        public IEnumerable<Team> GetTeams(int x, int y)
        {
            return from z in Enumerable.Range(1, 4)
                   select new Team
                   {
                       Name = String.Format("Team {0}-{1}-{2}", x, y, z),
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
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
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

```xml
<Page
    x:Class="MasterDetailsBinding.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MasterDetailsBinding"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Page.Resources>
        <CollectionViewSource x:Name="Leagues"
            Source="{x:Bind ViewModel}"/>
        <CollectionViewSource x:Name="Divisions"
            Source="{Binding Divisions, Source={StaticResource Leagues}}"/>
        <CollectionViewSource x:Name="Teams"
            Source="{Binding Teams, Source={StaticResource Divisions}}"/>

        <Style TargetType="TextBlock">
            <Setter Property="FontSize" Value="15"/>
            <Setter Property="FontWeight" Value="Bold"/>
        </Style>

        <Style TargetType="ListBox">
            <Setter Property="FontSize" Value="15"/>
        </Style>

        <Style TargetType="ContentControl">
            <Setter Property="FontSize" Value="15"/>
        </Style>

    </Page.Resources>

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        <StackPanel Orientation="Horizontal">

            <!-- All Leagues view -->

            <StackPanel Margin="5">
                <TextBlock Text="All Leagues"/>
                <ListBox ItemsSource="{Binding Source={StaticResource Leagues}}" 
                    DisplayMemberPath="Name"/>
            </StackPanel>

            <!-- League/Divisions view -->

            <StackPanel Margin="5">
                <TextBlock Text="{Binding Name, Source={StaticResource Leagues}}"/>
                <ListBox ItemsSource="{Binding Source={StaticResource Divisions}}" 
                    DisplayMemberPath="Name"/>
            </StackPanel>

            <!-- Division/Teams view -->

            <StackPanel Margin="5">
                <TextBlock Text="{Binding Name, Source={StaticResource Divisions}}"/>
                <ListBox ItemsSource="{Binding Source={StaticResource Teams}}" 
                    DisplayMemberPath="Name"/>
            </StackPanel>

            <!-- Team view -->

            <ContentControl Content="{Binding Source={StaticResource Teams}}">
                <ContentControl.ContentTemplate>
                    <DataTemplate>
                        <StackPanel Margin="5">
                            <TextBlock Text="{Binding Name}" 
                                FontSize="15" FontWeight="Bold"/>
                            <StackPanel Orientation="Horizontal" Margin="10,10">
                                <TextBlock Text="Wins:" Margin="0,0,5,0"/>
                                <TextBlock Text="{Binding Wins}"/>
                            </StackPanel>
                            <StackPanel Orientation="Horizontal" Margin="10,0">
                                <TextBlock Text="Losses:" Margin="0,0,5,0"/>
                                <TextBlock Text="{Binding Losses}"/>
                            </StackPanel>
                        </StackPanel>
                    </DataTemplate>
                </ContentControl.ContentTemplate>
            </ContentControl>

        </StackPanel>

    </Grid>
</Page>
```

Beachten Sie, dass Sie durch die direkte Bindung an die [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833)-Instanz implizieren, dass Sie in Bindungen, in denen der Pfad in der Sammlung selbst nicht gefunden werden kann, an das aktuelle Element binden möchten. Die **CurrentItem**-Eigenschaft muss nicht als Pfad für die Bindung angegeben werden, obwohl dies im Zweifelsfall möglich ist. Beispielsweise wird die [**Content**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content)-Eigenschaft der [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365)-Klasse, welche die Teamansicht darstellt, an `Teams`**CollectionViewSource** gebunden. Die Steuerelemente in der [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348) werden jedoch an Eigenschaften der `Team`-Klasse gebunden, da die **CollectionViewSource** bei Bedarf automatisch das aktuell aus der Teamliste ausgewählte Team liefert.

 

 




<!--HONumber=Jun16_HO4-->


