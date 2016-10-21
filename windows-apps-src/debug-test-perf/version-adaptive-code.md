---
author: jwmsft
title: Versionsadaptiver Code
description: "Erfahren Sie, wie Sie neue APIs nutzen und gleichzeitig die Kompatibilität mit früheren Versionen gewährleisten."
translationtype: Human Translation
ms.sourcegitcommit: 24a62c9331d4f651937f3f795fb1e7c9704af2ca
ms.openlocfilehash: 7656018c61688bddbf23f889a82af4fd6d58c3ea

---

# Versionsadaptiver Code: Verwenden neuer APIs bei gleichzeitiger Gewährleistung der Kompatibilität mit früheren Versionen

Mit jeder Version des Windows10SDKs kommen spannende neue Funktionen hinzu, die Sie nicht ungenutzt lassen sollten. Da allerdings nicht alle Kunden ihre Geräte gleichzeitig auf die neueste Version von Windows10 aktualisieren, müssen Sie sicherstellen, dass Ihre App auf einem möglichst breiten Gerätespektrum verwendet werden kann. Hier zeigen wir Ihnen, wie Sie Ihre App so gestalten, dass sie unter früheren Versionen von Windows10 ausgeführt werden kann, aber auch neue Features nutzt, wenn sie auf einem Gerät ausgeführt wird, auf dem das neueste Update installiert ist.

Mit den beiden folgenden Schritten können Sie sicherstellen, dass Ihre App auf einem möglichst breiten Spektrum von Windows10-Geräten verwendet werden kann: Erstens: Konfigurieren Sie Ihr VisualStudio-Projekt mit den neuesten APIs. Dies beeinflusst die Kompilierung der App. Zweitens: Führen Sie Laufzeitprüfungen durch, um sicherzustellen, dass nur APIs aufgerufen werden, die auf dem Gerät vorhanden sind, auf dem die App ausgeführt wird.

## Konfigurieren des VisualStudio-Projekts

Im ersten Schritt zur Unterstützung mehrerer Versionen von Windows10 müssen im VisualStudio-Projekt die unterstützten *Ziel*- und *Mindestversionen* des Betriebssystems/SDKs angegeben werden.
- *Ziel*: Die SDK-Version, für die Visual Studio den App-Code kompiliert und alle Tools ausführt. Alle APIs und Ressourcen in dieser SDK-Version stehen zur Kompilierzeit im App-Code zur Verfügung.
- *Minimum*: Die SDK-Version, die die niedrigste Betriebssystemversion unterstützt, unter der Ihre App ausgeführt werden kann (und vom Store bereitgestellt wird), und die Version, für die Visual Studio den Markupcode der App kompiliert. 

Zur Laufzeit wird Ihre App für die Betriebssystemversion ausgeführt, für die sie bereitgestellt wurde. Wenn Sie also Ressourcen verwenden oder APIs aufrufen, die in dieser Version nicht zur Verfügung stehen, löst Ihre App Ausnahmen aus. Weiter unten in diesem Artikel erfahren Sie, wie Sie mithilfe von Laufzeitprüfungen die passenden APIs aufrufen.

Die Einstellungen für Ziel- und Mindestversion geben jeweils das Ende eines Bereichs von Betriebssystem-/SDK-Versionen an. Wenn Sie Ihre App allerdings unter der Mindestversion testen, können Sie sicher sein, dass sie unter jeder beliebigen Version zwischen der Mindest- und Zielversion verwendet werden kann.

> [!TIP]
> Visual Studio gibt im Zusammenhang mit der API-Kompatibilität keine Warnungen aus. Daher müssen Sie selbst testen und sicherstellen, dass Ihre App unter allen Betriebssystemversionen zwischen Mindest- und Zielversion (jeweils einschließlich) erwartungsgemäß funktioniert.

Wenn Sie ein neues Projekt in Visual Studio2015 (Update2 oder höher) erstellen, werden Sie aufgefordert, die von Ihrer App unterstützte Ziel- und Mindestversion festzulegen. Die Zielversion ist standardmäßig die höchste installierte SDK-Version, die Mindestversion die niedrigste installierte SDK-Version. Als Ziel- und Mindestversion stehen nur SDK-Versionen zur Auswahl, die auf Ihrem Computer installiert sind. 

![Festlegen des Ziel-SDKs in Visual Studio](images/vs-target-sdk-1.png)

In der Regel empfiehlt es sich, die Standardeinstellungen beizubehalten. Falls Sie jedoch eine Vorschauversion des SDKs installiert haben und Produktionscode schreiben, empfiehlt es sich, die Zielversion zu ändern und auf die neueste offizielle SDK-Version festzulegen. 

Navigieren Sie zum Ändern der Mindest- und Zielversion für ein bereits in Visual Studio erstelltes Projekt zu „Projekt“> „Eigenschaften“> Registerkarte „Anwendung“> „Ziel“.

![Ändern des Ziel-SDKs in Visual Studio](images/vs-target-sdk-2.png) 

Im Anschluss finden Sie die Buildnummern für die einzelnen SDKs:
- Windows10-Version1506: SDK-Version10240
- Windows10-Version1511 (November-Updates): SDK-Version10586
- Windows 10, Version 1607 (Anniversary Update): SDK Version 14393.

Sie können jede beliebige veröffentlichte Version des SDKs aus dem [WindowsSDK- und Emulator-Archiv](https://developer.microsoft.com/downloads/sdk-archive) herunterladen. Das neueste Windows Insider Preview SDK können Sie im Entwicklerabschnitt der [Windows Insider-Website](https://insider.windows.com/) herunterladen.

## Schreiben von adaptivem Code

Das Schreiben von adaptivem Code ist vergleichbar mit dem [Erstellen einer adaptiven UI](https://msdn.microsoft.com/windows/uwp/layout/layouts-with-xaml). Dabei können Sie etwa Ihre grundlegende Benutzeroberfläche für den kleinsten Bildschirm entwerfen und anschließend Elemente verschieben oder hinzufügen, wenn erkannt wird, dass die App auf einem größeren Bildschirm ausgeführt wird. Bei adaptivem Code schreiben Sie Ihren Basiscode für die niedrigste Betriebssystemversion und können anschließend ausgewählte Features hinzufügen, wenn erkannt wird, dass Ihre App unter einer höheren Version ausgeführt wird, die über das neue Feature verfügt.

### API-Laufzeitprüfungen

Verwenden Sie die [Windows.Foundation.Metadata.ApiInformation](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.aspx)-Klasse in einer Bedingung in Ihrem Code, um zu testen, ob die aufzurufende API vorhanden ist. Diese Bedingung wird immer ausgewertet – ganz gleich, wo Ihre App ausgeführt wird. Das Ergebnis ist aber nur dann **True**, wenn sie auf einem Gerät ausgeführt wird, auf dem die API vorhanden ist und somit aufgerufen werden kann. So können Sie Apps mit versionsadaptivem Code erstellen, die APIs verwenden, welche nur unter bestimmten Betriebssystemversionen verfügbar sind.

Hier gehen wir anhand von spezifischen Beispielen auf die Nutzung neuer Features in Windows Insider Preview ein. Eine allgemeine Übersicht über die Verwendung von **ApiInformation** finden Sie in der [Anleitung für UWP-Apps](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide) sowie im Blogbeitrag [Dynamically detecting features with API contracts](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/) (Dynamisches Erkennen von Features mithilfe von API-Verträgen).

> [!TIP]
> Eine hohe Anzahl von API-Laufzeitprüfungen kann die Leistung Ihrer App beeinträchtigen. Die Prüfungen werden in diesen Beispielen inline veranschaulicht. In Produktionscode empfiehlt es sich, die Prüfung nur einmal durchzuführen, das Ergebnis zwischenzuspeichern und es anschließend in der gesamten App zu verwenden. 

### Nicht unterstützte Szenarien

In den meisten Fällen können Sie die auf die SDK-Version10240 festgelegte Mindestversion Ihrer App beibehalten und neue APIs auf der Grundlage von Laufzeitprüfungen aktivieren, wenn die App unter einer höheren Version ausgeführt wird. In bestimmten Fällen müssen Sie allerdings die Mindestversion Ihrer App erhöhen, um neue Features verwenden zu können.

Die Mindestversion der App muss erhöht werden, wenn Sie Folgendes verwenden:
- Eine neue API, die eine Funktion erfordert, die in einer früheren Version nicht verfügbar ist. Die unterstützte Mindestversion muss auf eine höhere Version festgelegt werden, die über diese Funktion verfügt. Weitere Informationen finden Sie unter [Deklaration der App-Funktionen](../packaging/app-capability-declarations.md).
- Einen neuen Ressourcenschlüssel, der zu „generic.xaml“ hinzugefügt wurde und in einer früheren Version nicht verfügbar war. Die zur Laufzeit verwendete Version von „generic.xaml“ wird durch die auf dem Gerät ausgeführte Betriebssystemversion bestimmt. Mit API-Laufzeitprüfungen lässt sich nicht ermitteln, ob XAML-Ressourcen vorhanden sind. Daher dürfen nur Ressourcenschlüssel verwendet werden, die in der von der App unterstützten Mindestversion zur Verfügung stehen. Andernfalls bringt eine [XAMLParseException](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.markup.xamlparseexception.aspx) Ihre App zur Laufzeit zum Absturz.

### Optionen für adaptiven Code

Adaptiver Code kann auf zwei Arten erstellt werden. In den meisten Fällen schreiben Sie den Markupcode Ihrer App für die Mindestversion und verwenden dann Ihren App-Code, um ggf. die Nutzung neuerer Betriebssystemfeatures zu ermöglichen. Wenn Sie allerdings eine Eigenschaft in einem visuellen Zustand aktualisieren müssen und sich zwischen Betriebssystemversionen lediglich ein Eigenschafts- oder Enumerationswert ändert, können Sie einen erweiterbaren Zustandsauslöser erstellen, dessen Aktivierung davon abhängt, ob eine API vorhanden ist.

Im Anschluss finden Sie eine Gegenüberstellung dieser Optionen:

**App-Code**

Verwendung
- Empfohlen für alle Szenarien mit adaptivem Code mit Ausnahme bestimmter Fälle, die weiter unten für erweiterbare Auslöser definiert sind.

Vorteile:
- Weniger Aufwand/Komplexität für Entwickler beim Einbinden von API-Unterschieden in das Markup

Nachteile:
- Keine Designerunterstützung

**Zustandsauslöser**

Verwendung
- Verwenden Sie diese Option, wenn sich zwischen Betriebssystemversionen nur eine Eigenschaft oder Enumeration geändert hat, keine Änderung der Logik erforderlich ist und eine Verbindung mit einem visuellen Zustand besteht.

Vorteile:
- Ermöglicht die Erstellung spezifischer visueller Zustände, die abhängig davon ausgelöst werden, ob eine API vorhanden ist.
- Eingeschränkte Designerunterstützung verfügbar.

Nachteile:
- Die Verwendung von benutzerdefinierten Auslösern ist auf visuelle Zustände beschränkt, weshalb sich die Option nicht für komplizierte adaptive Layouts eignet.
- Wertänderungen müssen mithilfe von Settern angegeben werden, sodass nur einfache Änderungen möglich sind.
- Die Einrichtung und Verwendung benutzerdefinierter Zustandsauslöser ist relativ aufwendig.

## Beispiele für adaptiven Code

In diesem Abschnitt zeigen wir verschiedene Beispiele für adaptiven Code, in denen neue APIs aus der Windows10-Version1607 (Windows Insider Preview) verwendet werden.

### Beispiel1: Neuer Enumerationswert

In der Windows10-Version1607 wird die [InputScopeNameValue](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.inputscopenamevalue.aspx)-Enumeration um einen neuen Wert erweitert: **ChatWithoutEmoji**. Dieser neue Eingabeumfang besitzt das gleiche Eingabeverhalten wie der Eingabeumfang **Chat** (Rechtschreibprüfung, AutoVervollständigen, automatische Großschreibung), wird aber einer Bildschirmtastatur ohne Emoji-Schaltfläche zugeordnet. Dies ist hilfreich, wenn Sie Ihre eigene Emoji-Auswahl erstellen und die integrierte Emoji-Schaltfläche auf der Bildschirmtastatur deaktivieren möchten. 

Dieses Beispiel zeigt, wie Sie überprüfen, ob der **ChatWithoutEmoji**-Enumerationswert vorhanden ist, und die [InputScope](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox.inputscope.aspx)-Eigenschaft eines **TextBox**-Elements festlegen, falls dies der Fall ist. Ist der Wert in dem System, auf dem die App ausgeführt wird, nicht vorhanden, wird **InputScope** stattdessen auf **Chat** festgelegt. Der hier gezeigte Code kann in einem Page-Konstruktor oder in einem Page.Loaded-Ereignishandler platziert werden.

> [!TIP]
> Verlassen Sie sich beim Prüfen einer API nicht auf .NET-Sprachfeatures, sondern verwenden Sie statische Zeichenfolgen. Andernfalls versucht Ihre App unter Umständen, auf einen nicht definierten Typ zuzugreifen, was einen Absturz zur Laufzeit zur Folge hat.

**C#**
```csharp
// Create a TextBox control for sending messages 
// and initialize an InputScope object.
TextBox messageBox = new TextBox();
messageBox.AcceptsReturn = true;
messageBox.TextWrapping = TextWrapping.Wrap;
InputScope scope = new InputScope();
InputScopeName scopeName = new InputScopeName();

// Check that the ChatWithEmoji value is present.
// (It's present starting with Windows 10, version 1607,
//  the Target version for the app. This check returns false on earlier versions.)         
if (ApiInformation.IsEnumNamedValuePresent("Windows.UI.Xaml.Input.InputScopeNameValue", "ChatWithoutEmoji"))
{
    // Set new ChatWithoutEmoji InputScope if present.
    scopeName.NameValue = InputScopeNameValue.ChatWithoutEmoji;
}
else
{
    // Fall back to Chat InputScope.
    scopeName.NameValue = InputScopeNameValue.Chat;
}

// Set InputScope on messaging TextBox.
scope.Names.Add(scopeName);
messageBox.InputScope = scope;

// For this example, set the TextBox text to show the selected InputScope.
messageBox.Text = messageBox.InputScope.Names[0].NameValue.ToString();

// Add the TextBox to the XAML visual tree (rootGrid is defined in XAML).
rootGrid.Children.Add(messageBox);
```

Im vorherigen Beispiel wird das TextBox-Element erstellt, und alle Eigenschaften werden im Code festgelegt. Wenn Sie allerdings bereits über XAML verfügen und lediglich die InputScope-Eigenschaft für Systeme ändern möchten, auf denen der neue Wert unterstützt wird, können Sie dies wie hier zu sehen ohne XAML-Änderung implementieren. Der Standardwert wird in XAML auf **Chat** festgelegt und im Code überschrieben, falls der **ChatWithoutEmoji**-Wert vorhanden ist.

**XAML**
```xaml
<TextBox x:Name="messageBox"
         AcceptsReturn="True" TextWrapping="Wrap"
         InputScope="Chat"
         Loaded="messageBox_Loaded"/>
```

**C#**
```csharp
private void messageBox_Loaded(object sender, RoutedEventArgs e)
{
    if (ApiInformation.IsEnumNamedValuePresent("Windows.UI.Xaml.Input.InputScopeNameValue", "ChatWithoutEmoji"))
    {
        // Check that the ChatWithEmoji value is present.
        // (It's present starting with Windows 10, version 1607,
        //  the Target version for the app. This code is skipped on earlier versions.)
        InputScope scope = new InputScope();
        InputScopeName scopeName = new InputScopeName();
        scopeName.NameValue = InputScopeNameValue.ChatWithoutEmoji;
        // Set InputScope on messaging TextBox.
        scope.Names.Add(scopeName);
        messageBox.InputScope = scope;
    }

    // For this example, set the TextBox text to show the selected InputScope.
    // This is outside of the API check, so it will happen on all OS versions.
    messageBox.Text = messageBox.InputScope.Names[0].NameValue.ToString();
}
```

Nachdem wir nun über ein konkretes Beispiel verfügen, sehen wir uns einmal an, wie in diesem Fall die Einstellungen für Mindest- und Zielversion angewendet werden.

In diesen Beispielen können Sie den Chat-Enumerationswert in XAML oder in Code ohne Überprüfung verwenden, da der Wert in der unterstützten Mindestversion des Betriebssystems vorhanden ist. 

Wenn Sie den ChatWithoutEmoji-Wert in XAML oder in Code ohne Überprüfung verwenden, wird er ohne Fehler kompiliert, da der Wert in der Zielversion des Betriebssystems vorhanden ist. Auch die Ausführung auf einem System mit der Zielversion des Betriebssystems ist ohne Fehler möglich. Wenn die App allerdings auf einem System mit der Mindestversion des Betriebssystems ausgeführt wird, stürzt sie zur Laufzeit ab, da der ChatWithoutEmoji-Enumerationswert nicht vorhanden ist. Daher dürfen Sie diesen Wert nur in Code verwenden und müssen ihn mit einer API-Laufzeitprüfung umschließen, sodass er nur aufgerufen wird, wenn dies auf dem aktuellen System unterstützt wird.

### Beispiel2: Neues Steuerelement

Eine neue Version von Windows verfügt üblicherweise über neue Steuerelemente für die UWP-API-Oberfläche, die den Funktionsumfang der Plattform erweitern. Das Vorhandensein eines neuen Steuerelements kann mithilfe der [ApiInformation.IsTypePresent](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.istypepresent.aspx)-Methode ermittelt werden.

In der Windows10-Version1607 wird ein neues Mediensteuerelement namens [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) eingeführt. Dieses Steuerelement basiert auf der [MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.aspx)-Klasse, stellt beispielweise Features wie die problemlose Einbindung von Hintergrundaudio bereit und profitiert von Verbesserungen bei der Architektur des Medienstapels.

Wenn die App allerdings auf einem Gerät mit einer älteren Windows10-Version als1607 ausgeführt wird, muss anstelle des neuen **MediaPlayerElement**-Steuerelements das [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaelement.aspx)-Steuerelement verwendet werden. Sie können mithilfe der [**ApiInformation.IsTypePresent**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.istypepresent.aspx)-Methode zur Laufzeit überprüfen, ob das MediaPlayerElement-Steuerelement vorhanden ist, und dann das geeignete Steuerelement für das System laden, auf dem die App ausgeführt wird.

Dieses Beispiel zeigt, wie Sie eine App erstellen, die abhängig davon, ob der MediaPlayerElement-Typ vorhanden ist, entweder das neue MediaPlayerElement-Steuerelement oder das alte MediaElement-Steuerelement verwendet. In diesem Code werden die Steuerelemente samt dazugehöriger Benutzeroberfläche und entsprechendem Code mithilfe der [UserControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol.aspx)-Klasse in Komponenten zerlegt, sodass sie abhängig von der Betriebssystemversion einbezogen werden können. Alternativ können Sie ein benutzerdefiniertes Steuerelement verwendet. Dieses bietet mehr Funktionen und benutzerdefiniertes Verhalten als in diesem einfachen Beispiel erforderlich.
 
**MediaPlayerUserControl** 

`MediaPlayerUserControl` kapselt ein **MediaPlayerElement** und mehrere Schaltflächen für eine framebasierte Mediennavigation. „UserControl“ ermöglicht die Behandlung dieser Steuerelemente als einzelne Entität und erleichtert den Wechsel zu „MediaElement“ bei älteren Systemen. Dieses Benutzersteuerelement sollte nur für Systeme verwendet werden, die über „MediaPlayerElement“ verfügen, sodass im Code innerhalb dieses Benutzersteuerelements keine ApiInformation-Prüfungen ausgeführt werden.

> [!NOTE]
> Die Schaltflächen für die Frameschritte werden außerhalb des Medienplayers platziert, um dieses Beispiel möglichst einfach und prägnant zu halten. Zur Verbesserung der Benutzerfreundlichkeit sollten Sie „MediaTransportControls“ mit Ihren benutzerdefinierten Schaltflächen anpassen. Weitere Informationen finden Sie unter [Benutzerdefinierte Transportsteuerelemente](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/custom-transport-controls). 

**XAML**
```xaml
<UserControl
    x:Class="MediaApp.MediaPlayerUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MediaApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="300"
    d:DesignWidth="400">

    <Grid x:Name="MPE_grid>
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <StackPanel Orientation="Horizontal" 
                    HorizontalAlignment="Center" Grid.Row="1">
            <RepeatButton Click="StepBack_Click" Content="Step Back"/>
            <RepeatButton Click="StepForward_Click" Content="Step Forward"/>
        </StackPanel>
    </Grid>
</UserControl>
```

**C#**
```csharp
using System;
using Windows.Media.Core;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

namespace MediaApp
{
    public sealed partial class MediaPlayerUserControl : UserControl
    {
        public MediaPlayerUserControl()
        {
            this.InitializeComponent();
            
            // The markup code compiler runs against the Minimum OS version so MediaPlayerElement must be created in app code
            MPE = new MediaPlayerElement();
            Uri videoSource = new Uri("ms-appx:///Assets/UWPDesign.mp4");
            MPE.Source = MediaSource.CreateFromUri(videoSource);
            MPE.AreTransportControlsEnabled = true;
            MPE.MediaPlayer.AutoPlay = true;

            // Add MediaPlayerElement to the Grid
            MPE_grid.Children.Add(MPE);

        }

        private void StepForward_Click(object sender, RoutedEventArgs e)
        {
            // Step forward one frame, only available using MediaPlayerElement.
            MPE.MediaPlayer.StepForwardOneFrame();
        }

        private void StepBack_Click(object sender, RoutedEventArgs e)
        {
            // Step forward one frame, only available using MediaPlayerElement.
            MPE.MediaPlayer.StepForwardOneFrame();
        }
    }
}
```

**MediaElementUserControl**
 
`MediaElementUserControl` kapselt ein **MediaElement**-Steuerelement.

**XAML**
```xaml
<UserControl
    x:Class="MediaApp.MediaElementUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MediaApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="300"
    d:DesignWidth="400">

    <Grid>
        <MediaElement AreTransportControlsEnabled="True" 
                      Source="Assets/UWPDesign.mp4"/>
    </Grid>
</UserControl>
```

> [!NOTE]
> Die Codepage für `MediaElementUserControl` enthält nur generierten Code und wird daher nicht gezeigt.

**Initialisieren eines Steuerelements auf der Grundlage von „IsTypePresent“**

Rufen Sie zur Laufzeit **ApiInformation.IsTypePresent** auf, um zu prüfen, ob „MediaPlayerElement“ vorhanden ist. Falls ja, wird `MediaPlayerUserControl` geladen. Andernfalls wird `MediaElementUserControl` geladen.

**C#**
```csharp
public MainPage()
{
    this.InitializeComponent();

    UserControl mediaControl;

    // Check for presence of type MediaPlayerElement.
    if (ApiInformation.IsTypePresent("Windows.UI.Xaml.Controls.MediaPlayerElement"))
    {
        mediaControl = new MediaPlayerUserControl();
    }
    else
    {
        mediaControl = new MediaElementUserControl();
    }

    // Add mediaControl to XAML visual tree (rootGrid is defined in XAML).
    rootGrid.Children.Add(mediaControl);
}
```

> [!IMPORTANT]
> Zur Erinnerung: Diese Überprüfung legt nur das `mediaControl`-Objekt auf `MediaPlayerUserControl` oder `MediaElementUserControl` fest. Diese bedingten Überprüfungen müssen überall im Code durchgeführt werden, wo Sie ermitteln müssen, ob die MediaPlayerElement- oder die MediaElement-API verwendet werden soll. Es empfiehlt sich, die Prüfung nur einmal durchzuführen, das Ergebnis zwischenzuspeichern und es anschließend in der gesamten App zu verwenden.

## Beispiele für Zustandsauslöser

Mithilfe erweiterbarer Zustandsauslöser können Sie Markup und Code zusammen verwenden, um Änderungen des visuellen Zustands auf der Grundlage einer im Code überprüften Bedingung auszulösen (in diesem Fall: das Vorhandensein einer bestimmten API). In allgemeinen Szenarien mit adaptivem Code wird die Verwendung von Zustandsauslösern aufgrund des zusätzlichen Aufwands und der Beschränkung auf visuelle Zustände nicht empfohlen. 

Verwenden Sie Zustandsauslöser nur dann für adaptiven Code, wenn lediglich geringfügige Benutzeroberflächenänderungen zwischen Betriebssystemversionen vorliegen, die sich nicht auf die restliche Benutzeroberfläche auswirken (etwa die Änderung einer Eigenschaft oder eines Enumerationswerts für ein Steuerelement).

### Beispiel1: Neue Eigenschaft

Der erste Schritt bei der Einrichtung eines erweiterbaren Zustandsauslösers besteht in der Verwendung von Unterklassen für die [StateTriggerBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.statetriggerbase.aspx)-Klasse zur Erstellung eines benutzerdefinierten Auslösers, dessen Aktivierung davon abhängt, ob eine API vorhanden ist. Dieses Beispiel zeigt einen Auslöser, der aktiviert wird, wenn das Vorhandensein der Eigenschaft der in XAML festgelegten `_isPresent`-Variablen entspricht.

**C#**
```csharp
class IsPropertyPresentTrigger : StateTriggerBase
{
    public string TypeName { get; set; }
    public string PropertyName { get; set; }

    private Boolean _isPresent;
    private bool? _isPropertyPresent = null;

    public Boolean IsPresent
    {
        get { return _isPresent; }
        set
        {
            _isPresent = value;
            if (_isPropertyPresent == null)
            {
                // Call into ApiInformation method to determine if property is present.
                _isPropertyPresent =
                ApiInformation.IsPropertyPresent(TypeName, PropertyName);
            }

            // If the property presence matches _isPresent then the trigger will be activated;
            SetActive(_isPresent == _isPropertyPresent);
        }
    }
}
```

Im nächsten Schritt wird der visuelle Zustandsauslöser in XAML eingerichtet, um abhängig vom Vorhandensein der API zwei unterschiedliche visuelle Zustände zu erhalten. 

In der Windows10-Version1607 wird für die [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx)-Klasse eine neue Eigenschaft namens [AllowFocusOnInteraction](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.allowfocusoninteraction.aspx) eingeführt, die bestimmt, ob ein Steuerelement den Fokus erhält, wenn ein Benutzer damit interagiert. Dies ist hilfreich, falls ein Textfeld für die Dateneingabe den Fokus behalten (und die Bildschirmtastatur weiter angezeigt werden) soll, wenn der Benutzer auf eine Schaltfläche klickt.

Der Auslöser in diesem Beispiel überprüft, ob die Eigenschaft vorhanden ist. Ist die Eigenschaft vorhanden, wird die **AllowFocusOnInteraction**-Eigenschaft für eine Schaltfläche auf **False** festgelegt. Ist die Eigenschaft nicht vorhanden, wird der ursprüngliche Zustand der Schaltfläche beibehalten. Das TextBox-Element dient zur Veranschaulichung der Auswirkung dieser Eigenschaft, wenn Sie den Code ausführen.

**XAML**
```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel>
        <TextBox Width="300" Height="36"/>
        <!-- Button to set the new property on. -->
        <Button x:Name="testButton" Content="Test" Margin="12"/>
    </StackPanel>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="propertyPresentStateGroup">
            <VisualState>
                <VisualState.StateTriggers>
                    <!--Trigger will activate if the AllowFocusOnInteraction property is present-->
                    <local:IsPropertyPresentTrigger 
                        TypeName="Windows.UI.Xaml.FrameworkElement" 
                        PropertyName="AllowFocusOnInteraction" IsPresent="True"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="testButton.AllowFocusOnInteraction" 
                            Value="False"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

### Beispiel2: Neuer Enumerationswert

Dieses Beispiel zeigt, wie Sie abhängig davon, ob ein Wert vorhanden ist, unterschiedliche Enumerationswerte festlegen. Hierbei kommt ein benutzerdefinierter Zustandsauslöser zum Einsatz, um das gleiche Ergebnis zu erhalten wie im vorherigen Chatbeispiel. In diesem Beispiel wird der neue ChatWithoutEmoji-Eingabeumfang verwendet, wenn auf dem Gerät die Windows10-Version1607 ausgeführt wird. Andernfalls wird der Eingabeumfang **Chat** verwendet. Die visuellen Zustände, die diesen Auslöser verwenden, werden im *if-else*-Stil eingerichtet, wobei die Wahl des Eingabeumfangs davon abhängt, ob der neue Enumerationswert vorhanden ist.

**C#**
```csharp
class IsEnumPresentTrigger : StateTriggerBase
{
    public string EnumTypeName { get; set; }
    public string EnumValueName { get; set; }

    private Boolean _isPresent;
    private bool? _isEnumValuePresent = null;

    public Boolean IsPresent
    {
        get { return _isPresent; }
        set
        {
            _isPresent = value;

            if (_isEnumValuePresent == null)
            {
                // Call into ApiInformation method to determine if value is present.
                _isEnumValuePresent =
                ApiInformation.IsEnumNamedValuePresent(EnumTypeName, EnumValueName);
            }

            // If the property presence matches _isPresent then the trigger will be activated;
            SetActive(_isPresent == _isEnumValuePresent);
        }
    }
}
```

**XAML**
```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <TextBox x:Name="messageBox"
     AcceptsReturn="True" TextWrapping="Wrap"/>


    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="EnumPresentStates">
            <!--if-->
            <VisualState x:Name="isPresent">
                <VisualState.StateTriggers>
                    <local:IsEnumPresentTrigger 
                        EnumTypeName="Windows.UI.Xaml.Input.InputScopeNameValue" 
                        EnumValueName="ChatWithoutEmoji" IsPresent="True"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="messageBox.InputScope" Value="ChatWithoutEmoji"/>
                </VisualState.Setters>
            </VisualState>
            <!--else-->
            <VisualState x:Name="isNotPresent">
                <VisualState.StateTriggers>
                    <local:IsEnumPresentTrigger 
                        EnumTypeName="Windows.UI.Xaml.Input.InputScopeNameValue" 
                        EnumValueName="ChatWithoutEmoji" IsPresent="False"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="messageBox.InputScope" Value="Chat"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```
## Verwandte Artikel

- [Anleitung für UWP-Apps](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
- [Dynamically detecting features with API contracts (Dynamisches Erkennen von Features mithilfe von API-Verträgen)](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)



<!--HONumber=Aug16_HO3-->


