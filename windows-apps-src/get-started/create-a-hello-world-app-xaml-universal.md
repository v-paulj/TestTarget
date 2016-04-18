---
ms.assetid: 03A74239-D4B6-4E41-B2FA-6C04F225B844
title: Erstellen der App „Hello, world“ (XAML)
description: In diesem Lernprogramm erfahren Sie, wie Sie XAML (Extensible Application Markup Language) mit C# zum Erstellen einer einfachen „Hello, World“-App verwenden, die auf die universelle Windows Plattform (UWP) unter Windows 10 abzielt.
---

# Erstellen der App „Hello, world“ (XAML)

In diesem Lernprogramm erfahren Sie, wie Sie XAML (Extensible Application Markup Language) mit C# zum Erstellen einer einfachen „Hello, World“-App verwenden, die auf die universelle Windows Plattform (UWP) unter Windows 10 abzielt. Mit nur einem Projekt in Microsoft Visual Studio können Sie eine App erstellen, die auf allen Geräten mit Windows 10 ausgeführt werden kann. In diesem Thema geht es um die Erstellung einer App, die für Desktops und mobile Geräte gleichermaßen gut geeignet ist.

**Wichtig** Dieses Lernprogramm ist für Microsoft Visual Studio 2015 und Windows 10 konzipiert. Die korrekte Funktionsweise mit früheren Versionen ist nicht sichergestellt.

Hier erfahren Sie Folgendes:

-   Erstellen Sie ein neues Visual Studio-Projekt für Windows 10 und die UWP.
-   Hinzufügen von XAML-Inhalt zu Ihrer Startseite
-   Behandeln von Touch-, Stift- und Mauseingaben
-   Ausführen des Projekts auf dem lokalen Desktop und auf dem Smartphone-Emulator in Visual Studio
-   Anpassen der UI an unterschiedliche Bildschirmgrößen

## Vorbereitung


-   Wir beginnen direkt mit den Schritten zum Erstellen einer einfachen universellen App. Daher empfehlen wir Ihnen dringend, sich die Informationen unter [Neues in Windows 10](https://dev.windows.com/whats-new-windows-10-dev-preview) und [Was ist eine universelle Windows-App](whats-a-uwp.md) gründlich durchzulesen, bevor Sie sich diesem Lernprogramm widmen.
-   Zum Durcharbeiten dieses Lernprogramms benötigen Sie Windows 10 und Visual Studio 2015. Weitere Informationen finden Sie unter [Vorbereiten](get-set-up.md) .
-   Es wird davon ausgegangen, dass Sie über Grundkenntnisse in XAML verfügen und mit den Konzepten in der [XAML-Übersicht](https://msdn.microsoft.com/library/windows/apps/Mt185595) vertraut sind.
-   Außerdem wird davon ausgegangen, dass Sie das Standardfensterlayout in Visual Studio verwenden. Wenn Sie das Standardlayout ändern, können Sie es im Menü **Fenster** mit dem Befehl **Fensterlayout zurücksetzen** wiederherstellen.

##  Schritt 1: Erstellen eines neuen Projekts in Visual Studio


1.  Starten Sie Visual Studio 2015.

   Der Visual Studio 2015-Startbildschirm wird angezeigt. (Hinweis: Im weiteren Verlauf wird Visual Studio 2015 kurz als Visual Studio bezeichnet.)

2.  Klicken Sie im Menü **Datei** auf **Neu** und dann auf **Projekt**.

   Das Dialogfeld **Neues Projekt** wird geöffnet. Im linken Bereich des Dialogfelds können Sie den Typ der anzuzeigenden Vorlagen auswählen.

3.  Erweitern Sie im linken Bereich die Option **Installiert > Vorlagen > Visual C# > Windows**, und wählen Sie anschließend die Vorlagengruppe **Universal**. Im mittleren Bereich des Dialogfelds sehen Sie eine Liste mit Projektvorlagen für Apps der universellen Windows-Plattform (UWP).

   ![Das Fenster „Neues Projekt“ ](images/newproject-cs.png)

4.  Wählen Sie im mittleren Bereich die Projektvorlage **Leere App (universelle Windows-App)** aus.

   Die Vorlage **Leere App** stellt eine minimale UWP-App bereit, die kompiliert und ausgeführt wird, aber keine Steuerelemente oder Daten für die Benutzeroberfläche enthält. Die App wird später in diesem Lernprogramm noch mit Steuerelementen versehen.

5.  Geben Sie im Textfeld **Name** den Namen „HelloWorld“ ein.
6.  Klicken Sie auf **OK**, um das Projekt zu erstellen.

   Visual Studio erstellt Ihr Projekt und zeigt es im **Projektmappen-Explorer**an.

   ![Visual Studio-Projektmappen-Explorer für das „HelloWorld“-Projekt](images/solutionexplorer-cs.png)

**Leere App** ist zwar nur eine Minimalvorlage, umfasst aber trotzdem eine Reihe von Dateien:

-   Eine Manifestdatei (Package.appxmanifest), die Ihre App beschreibt (Name, Beschreibung, Kachel, Startseite, Begrüßungsbildschirm usw.) und die in der App enthaltenen Dateien aufführt
-   Einen Satz mit Logobildern („Assets/Square150x150Logo.scale-200.png“, „Assets/Square44x44Logo.scale-200.png“ und „Assets/Wide310x150Logo.scale-200.png“), die im Startmenü angezeigt werden
-   Ein Bild der App für den Windows Store (Assets/StoreLogo.png)
-   Einen Begrüßungsbildschirm (Assets/SplashScreen.scale-200.png), der beim Start der App angezeigt wird
-   XAML- und Codedateien für die App („App.xaml“ und „App.Xaml.cs“)
-   Eine Startseite (MainPage.xaml) und eine entsprechende Codedatei (MainPage.xaml.cs), die beim Start der App ausgeführt werden

Diese Dateien werden für alle UWP-Apps mit C# benötigt. Sie sind Teil jedes Projekts, das Sie mit Visual Studio erstellen.

## Schritt 2: Anpassen der Startseite


### Inhalt der Dateien

Doppelklicken Sie zum Anzeigen und Bearbeiten einer Datei im Projekt im **Projektmappen-Explorer** auf die gewünschte Datei. Um die zugeordnete Codedatei anzuzeigen, können Sie eine XAML-Datei standardmäßig genauso wie einen Ordner erweitern. XAML-Dateien werden in einer geteilten Ansicht geöffnet, die sowohl die Entwurfsoberfläche als auch den XAML-Editor enthält.

In diesem Lernprogramm verwenden Sie lediglich einige der zuvor aufgeführten Dateien: „App.xaml“, „MainPage.xaml“ und „MainPage.xaml.cs“.

### „App.xaml“ und „App.xaml.cs“

In „App.xaml“ deklarieren Sie Ressourcen, die in der gesamten App zur Anwendung kommen. „App.xaml.cs“ ist die CodeBehind-Datei für „App.xaml“. CodeBehind ist der Code, der mit der partiellen Klassen der XAML-Seite verknüpft wird. Zusammen bilden XAML und CodeBehind eine vollständige Klasse. „App.xaml.cs“ ist der Einstiegspunkt für Ihre App. Sie enthält wie alle CodeBehind-Seiten einen Konstruktor, der die `InitializeComponent`-Methode aufruft. Die `InitializeComponent`-Methode wird nicht von Ihnen geschrieben. Sie wird von Visual Studio generiert und dient in erster Linie dazu, die in der XAML-Datei deklarierten Elemente zu initialisieren. „App.xaml.cs“ enthält außerdem Methoden zum Behandeln der Aktivierung und Unterbrechung der App.

### MainPage.xaml

In der Datei „MainPage.xaml“ definieren Sie die Benutzeroberfläche für Ihre App. Sie können Elemente direkt per XAML-Markup hinzufügen oder die Designtools von Visual Studio verwenden. „MainPage.xaml.cs“ ist die CodeBehind-Seite für „MainPage.xaml“. Hier fügen Sie Ihre App-Logik und Ereignishandler hinzu.

Zusammen definieren diese beiden Dateien im `HelloWorld`-Namespace eine neue Klasse mit dem Namen `MainPage`, die von [**Page**](https://msdn.microsoft.com/library/windows/apps/BR227503) erbt.

MainPage.xaml

```xml
    <Page
    x:Class="HelloWorld.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloWorld"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    </Grid>
</Page>
```

MainPage.xaml.cs

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

namespace HelloWorld
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
        }
    }
}
```

### Anpassen der Startseite

Lassen Sie uns nun der App einige Inhalte hinzufügen.

**So passen Sie die Startseite an**

1.  Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei „MainPage.xaml“, um sie zu öffnen.
2.  Fügen Sie im XAML-Editor die Steuerelemente für die UI hinzu.

   Fügen Sie den folgenden XAML-Code im [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)-Stammelement hinzu. Darin sind ein [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635)-Element mit [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) als Titel, ein **TextBlock**-Element, das den Namen des Benutzers anfordert, ein [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683)-Element zum Akzeptieren des Benutzernamens, ein [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)-Element sowie ein weiteres **TextBlock**-Element zum Anzeigen einer Begrüßung enthalten. Einige dieser Steuerelemente haben Namen, damit Sie später im Code darauf verweisen können.

```xml    
    <StackPanel x:Name="contentPanel" Margin="8,32,0,0">
        <TextBlock Text="Hello, world!" Margin="0,0,0,40"/>
        <TextBlock Text="What' s your name?"/>
        <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
            <TextBox x:Name="nameInput" Width="280" HorizontalAlignment="Left"/>
            <Button x:Name="inputButton" Content="Say &amp;quot;Hello&amp;quot;"/>
        </StackPanel>
        <TextBlock x:Name="greetingOutput"/>
    </StackPanel>
```    

    The controls that you added in the XAML editor show up in the design view.

## Schritt 3: Starten der App


Sie haben nun eine sehr einfache App erstellt. Dies ist eine guter Zeitpunkt zum Erstellen, Bereitstellen und Starten Ihrer App, um sie in Aktion zu sehen. Sie können Ihre App auf dem lokalen Computer, in einem Simulator oder Emulator oder auf einem Remotegerät debuggen. Dies ist das Zielgerätmenü in Visual Studio.

![Dropdownliste mit Zielgeräten zum Debuggen Ihrer App](images/uap-debug.png)

### Starten der App auf einem Desktop-Gerät

Standardmäßig wird die App auf dem lokalen Computer ausgeführt. Das Menü mit den Zielgeräten enthält mehrere Optionen zum Debuggen Ihrer App auf Geräten der Desktopfamilie.

-   **Simulator**
-   **Lokaler Computer**
-   **Remotecomputer**

**So beginnen Sie mit dem Debuggen auf dem lokalen Computer**

1.  Stellen Sie sicher, dass auf der **Standardsymbolleiste** im Menü mit den Zielgeräten (![Menü „Debuggen starten“](images/startdebug-full.png)) die Option **Lokaler Computer** ausgewählt ist. (Dies ist die Standardeinstellung.)
2.  Klicken Sie auf der Symbolleiste auf die Schaltfläche **Debuggen starten** (![Schaltfläche „Debuggen starten“](images/startdebug-sm.png)).

   oder

   Klicken Sie im Menü **Debuggen** auf **Debuggen starten**.

   oder

   Drücken Sie F5.

Die App wird in einem Fenster geöffnet, und zuerst wird ein standardmäßiger Begrüßungsbildschirm angezeigt. Der Begrüßungsbildschirm setzt sich aus einem Bild (SplashScreen.png) und einer Hintergrundfarbe (in der Manifestdatei der App angegeben) zusammen.

Nach dem Ausblenden des Begrüßungsbildschirms wird Ihre App angezeigt. Sie sieht ungefähr wie folgt aus:

![Erster App-Bildschirm](images/helloworld-1-cs.png)

Drücken Sie die WINDOWS-TASTE, um das Menü **Start** zu öffnen, und zeigen Sie alle Apps an. Beachten Sie, dass beim lokalen Bereitstellen der App dem Menü **Start** die dazugehörige Kachel hinzugefügt wird. Wenn Sie die App erneut ausführen möchten (nicht im Debugmodus), tippen oder klicken Sie im Menü **Start** auf die Kachel.

Viel zu bieten hat die App zwar noch nicht, aber trotzdem: Herzlichen Glückwunsch! Sie haben Ihre erste UWP-App erstellt!

**So beenden Sie das Debuggen**

-   Klicken Sie auf der Symbolleiste auf die Schaltfläche **Debuggen beenden** (![Schaltfläche „Debuggen beenden“](images/stopdebug.png)).

   oder

   Klicken Sie im Menü **Debuggen** auf **Debuggen beenden**.

   oder

   Schließen Sie das App-Fenster.

### Starten der App in einem Emulator für mobile Geräte

Ihre App kann auf jedem Windows 10-Gerät ausgeführt werden. Wir sehen uns nun an, wie sie auf einem Windows Phone dargestellt wird.

Zusätzlich zu den Optionen zum Debuggen auf einem Desktopgerät enthält Visual Studio Optionen zum Bereitstellen und Debuggen Ihrer App auf einem physischen mobilen Gerät, das an den Computer angeschlossen ist, oder in einem Emulator für mobile Geräte. Sie können zwischen Emulatoren für Geräte mit unterschiedlichen Arbeitsspeicher- und Bildschirmkonfigurationen wählen.

-   **Gerät**
-   **Emulator <SDK version> WVGA 4 Zoll 512 MB**
-   **Emulator <SDK version> WVGA 4 Zoll 1 GB**
-   usw. (verschiedene Emulatoren mit anderen Konfigurationen)

Es ist ratsam, Ihre App auf einem Gerät mit kleinem Bildschirm und begrenztem Arbeitsspeicher zu testen. Wählen Sie also die Option **Emulator 10.0.10240.0 WVGA 4 inch 512MB**.
**So beginnen Sie mit dem Debuggen auf einem Emulator für mobile Geräte**

1.  Wählen Sie auf der **Standardsymbolleiste** im Menü mit den Zielgeräten (![Menü „Debuggen starten“](images/startdebug-full.png)) die Option **Emulator 10.0.10240.0 WVGA 4 inch 512MB**.
2.  Klicken Sie auf der Symbolleiste auf die Schaltfläche **Debuggen starten** (![Schaltfläche „Debuggen starten“](images/startdebug-sm.png)).

   oder

   Klicken Sie im Menü **Debuggen** auf **Debuggen starten**.

   oder

   Drücken Sie F5.

Visual Studio startet den ausgewählten Emulator und stellt die App bereit und startet sie. Im Emulator für mobile Geräte sieht die App wie folgt aus.

![Erster App-Bildschirm auf dem mobilen Gerät](images/helloworld-1-cs-phone.png)

Als Erstes Ihnen auffallen, dass die Schaltfläche vom kleineren Bildschirms eines mobilen Geräts geschoben wird. Später in diesem Lernprogramm erfahren Sie, wie Sie die UI an unterschiedliche Bildschirmgrößen anpassen, damit die App immer gut aussieht.

Sie merken vielleicht auch, dass Sie Text im [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683)-Element eingeben können. Wenn Sie jetzt jedoch auf [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) klicken oder tippen, passiert noch nichts. In den nächsten Schritten erstellen Sie einen Ereignishandler für das [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737)-Ereignis der Schaltfläche, um eine personalisierte Begrüßung einzublenden. Sie fügen den Code für den Ereignishandler der Datei „MainPage.xaml.cs“ hinzu.

## Schritt 4: Erstellen eines Ereignishandlers


XAML-Elemente können Meldungen senden, wenn bestimmte Ereignisse eintreten. Dank dieser Ereignismeldungen können Sie mit einer Aktion auf das Ereignis reagieren. Sie fügen Ihren Code zum Reagieren auf das Ereignis in eine Ereignishandlermethode ein. Eines der am häufigsten auftretenden Ereignisse in vielen Apps ist das Klicken eines Benutzers auf ein [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)-Element.

Wir definieren nun einen Ereignishandler für das [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737)-Ereignis Ihrer Schaltfläche. Der Ereignishandler ruft den Namen des Benutzers aus dem [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683)-Steuerelement `nameInput` ab und gibt unter Verwendung dieser Informationen eine Begrüßung an das [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)-Element `greetingOutput` aus.

### Verwenden von Ereignissen für Touch-, Maus- und Stifteingaben

Welche Ereignisse sollten behandelt werden? Windows Store-Apps können auf unterschiedlichsten Geräten ausgeführt werden. Berücksichtigen Sie bei der App-Gestaltung also auch die Nutzung der Toucheingabe. Ihre App muss zudem mit Maus- und Stifteingaben zurechtkommen. Praktischerweise sind Ereignisse wie [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) und [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/BR208922) aber geräteunabhängig. Wenn Sie mit der Programmierung für Microsoft .NET vertraut sind, sind Ihnen möglicherweise separate Ereignisse für Maus-, Touch- und Stifteingaben (wie **MouseMove**, **TouchMove** und **StylusMove**) geläufig. Bei Windows Store-Apps werden diese separaten Ereignisse durch ein einzelnes [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/BR208970)-Ereignis ersetzt, das gleichzeitig für Touch-, Maus- und Stifteingaben verwendet werden kann.

**So fügen Sie einen Ereignishandler hinzu**

1.  Wählen Sie in der XAML- oder Designansicht das [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)-Element „Say Hello“ aus, das Sie „MainPage.xaml“ hinzugefügt haben.
2.  Klicken Sie im **Eigenschaftenfenster** auf die Ereignisschaltfläche (![Ereignisschaltfläche](images/eventsbutton.png)).
3.  Suchen Sie am Anfang der Ereignisliste nach dem [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737)-Ereignis. Geben Sie im Textfeld für das Ereignis den Namen der Funktion ein, die das **Click**-Ereignis behandelt. Geben Sie für dieses Beispiel „Button\_Click“ ein.

   ![Ereignisliste im Eigenschaftenfenster](images/xaml-hw-event.png)

4.  Drücken Sie die EINGABETASTE. Die Ereignishandlermethode wird erstellt und im Code-Editor geöffnet, damit Sie den Code hinzufügen können, der beim Auftreten des Ereignisses ausgeführt werden soll.

    Im XAML-Editor wird der XAML-Code für das [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)-Element aktualisiert, sodass der [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737)-Ereignishandler wie folgt deklariert wird:

```xml   
   <Button x:Name="inputButton" Content="Say &amp;quot;Hello&amp;quot;" Click="Button_Click"/>
```    

5.  Fügen Sie dem Ereignishandler, den Sie auf der CodeBehind-Seite erstellt haben, Code hinzu. Rufen Sie im Ereignishandler den Namen des Benutzers aus dem [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683)-Steuerelement `nameInput` ab, und erstellen Sie damit eine Begrüßung. Verwenden Sie das [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)-Element `greetingOutput` zum Anzeigen des Ergebnisses.
    
```csharp    
    private void Button_Click(object sender, RoutedEventArgs e)
    {
        greetingOutput.Text = "Hello, " + nameInput.Text + "!";
    }
```    

6.  Debuggen Sie die App auf dem lokalen Computer. Wenn Sie Ihren Namen in das Textfeld eingeben und anschließend auf die Schaltfläche klicken, zeigt die App eine personalisierte Begrüßung an.

## Schritt 5: Anpassen der UI an verschiedene Fenstergrößen


Als Nächstes sorgen wir dafür, dass sich die UI verschiedenen Bildschirmgrößen anpasst, damit sie auch auf mobilen Geräten gut aussieht. Dazu fügen Sie ein [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021)-Element hinzu und legen Eigenschaften fest, die für die unterschiedlichen Ansichtszustände angewendet werden.

**So passen Sie das UI-Layout an**

1.  Fügen Sie im XAML-Editor nach dem Starttag des [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)-Stammelements den folgenden XAML-Block hinzu:

```xml    
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="wideState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="641" />
                </VisualState.StateTriggers>
            </VisualState>
            <VisualState x:Name="narrowState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="0" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="inputPanel.Orientation" Value="Vertical"/>
                    <Setter Target="inputButton.Margin" Value="0,4,0,0"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
```    

2.  Debuggen Sie die App auf dem lokalen Computer. Sie sehen, dass die UI genauso wie vorher aussieht, es sei denn, die Fensterbreite sinkt unter 641 Pixel.
3.  Debuggen Sie die App auf dem Emulator für mobile Geräte. Beachten Sie, dass für die UI die Eigenschaften verwendet werden, die Sie unter `narrowState` definiert haben, und dass die Benutzeroberfläche auf dem kleinen Bildschirm korrekt angezeigt wird.

![Bildschirm der mobilen App](images/helloworld-2-cs-phone.png)

Wenn Sie bereits in früheren Versionen von XAML ein [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021)-Element genutzt haben, fällt Ihnen vielleicht auf, dass für den XAML-Code hier eine vereinfachte Syntax verwendet wird.

Das [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007)-Element mit dem Namen `wideState` verfügt über ein [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn890382)-Element, für das die [**MinWindowWidth**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.adaptivetrigger.minwindowwidth)-Eigenschaft auf den Wert 641 festgelegt ist. Dies bedeutet, dass der Zustand nur angewendet wird, wenn die Fensterbreite nicht kleiner als die Mindestgröße von 641 Pixeln ist. Da Sie keine [**Setter**](https://msdn.microsoft.com/library/windows/apps/BR208817)-Objekte für diesen Zustand definieren, werden die Layouteigenschaften verwendet, die Sie im XAML-Code für den Seiteninhalt festgelegt haben.

Das zweite [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007)-Element `narrowState` verfügt über ein [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn890382)-Element, für das die [**MinWindowWidth**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.adaptivetrigger.minwindowwidth)-Eigenschaft auf 0 festgelegt ist. Dieser Zustand wird angewendet, wenn die Fensterbreite größer als 0 und kleiner als 641 Pixel ist. (Bei 641 Pixeln wird `wideState` angewendet.) In diesem Zustand definieren Sie einige [**Setter**](https://msdn.microsoft.com/library/windows/apps/BR208817)-Objekte, um die Layouteigenschaften der Steuerelemente in der UI zu ändern:

-   Ändern Sie das [**Orientation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.orientation)-Element des `inputPanel`-Elements von **Horizontal** in **Vertical**.
-   Sie fügen dem `inputButton`-Element einen oberen Rand mit einer Breite von 4 DIP hinzu.

## Zusammenfassung


Herzlichen Glückwunsch, Sie haben Ihre erste App für Windows 10 und die UWP erstellt!


<!--HONumber=Mar16_HO1-->


