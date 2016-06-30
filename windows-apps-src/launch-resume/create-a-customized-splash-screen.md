---
author: TylerMSFT
title: "Längere Anzeige des Begrüßungsbildschirms"
description: "Verlängern Sie die Anzeige eines Begrüßungsbildschirms, indem Sie für die App einen erweiterten Begrüßungsbildschirm erstellen. Mit diesem erweiterten Bildschirm wird der beim Starten der App angezeigte Begrüßungsbildschirm imitiert. Er kann aber angepasst werden."
ms.assetid: CD3053EB-7F86-4D74-9C5A-950303791AE3
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 6ad43ec4fc865c5aae439693e0cd5b37a5d2c5dc

---

# Längere Anzeige des Begrüßungsbildschirms


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**Wichtige APIs**

-   [**SplashScreen-Klasse**](https://msdn.microsoft.com/library/windows/apps/br224763)
-   [**Window.SizeChanged-Ereignis**](https://msdn.microsoft.com/library/windows/apps/br209055)
-   [**Application.OnLaunched-Methode**](https://msdn.microsoft.com/library/windows/apps/br242335)

Verlängern Sie die Anzeige eines Begrüßungsbildschirms, indem Sie für die App einen erweiterten Begrüßungsbildschirm erstellen. Mit diesem erweiterten Bildschirm wird der beim Starten der App angezeigte Begrüßungsbildschirm imitiert. Er kann aber angepasst werden. Mit einem erweiterten Begrüßungsbildschirm können Sie das Startverhalten unabhängig davon definieren, ob Sie Echtzeitinformationen zum Ladevorgang anzeigen oder der App lediglich zusätzliche Zeit zum Vorbereiten der UI-Anfangselemente geben möchten.

> **Hinweis**  Die Bezeichnung „erweiterter Begrüßungsbildschirm“ in diesem Thema bezieht sich auf einen Begrüßungsbildschirm, der für längere Zeit auf dem Bildschirm angezeigt wird. Sie bezieht sich nicht auf eine Unterklasse, die von der [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763)-Klasse abgeleitet ist.

 

Stellen Sie sicher, dass der erweiterte Begrüßungsbildschirm den standardmäßigen Begrüßungsbildschirm genau imitiert, indem Sie sich an die folgenden Empfehlungen halten:

-   Sie sollten für die Seite mit dem erweiterten Begrüßungsbildschirm ein Bild mit 620 x 300 Pixeln verwenden. Es sollte zudem mit dem Bild übereinstimmen, das im App-Manifest für den Begrüßungsbildschirm angegeben ist (dem Bild des App-Begrüßungsbildschirms). In Microsoft Visual Studio 2015 werden die Einstellungen für den Begrüßungsbildschirm im App-Manifest (Datei „Package.appxmanifest“) auf der Registerkarte **Visuelle Anlagen** im Abschnitt **Begrüßungsbildschirm** gespeichert.
-   Sie sollten für den erweiterten Begrüßungsbildschirm eine Hintergrundfarbe verwenden, die mit der in Ihrem App-Manifest für Ihren Begrüßungsbildschirm angegebenen Hintergrundfarbe konsistent ist (dem Hintergrund des Begrüßungsbildschirms Ihrer App).
-   Sie müssen im Code die [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763)-Klasse verwenden, um das Bild des App-Begrüßungsbildschirms an den gleichen Koordinaten zu positionieren, an denen der standardmäßige Begrüßungsbildschirms positioniert wird.
-   Der Code sollte mithilfe der [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763)-Klasse auf Ereignisse zur Änderung der Fenstergröße (beispielsweise beim Drehen des Bildschirms oder Verschieben der App neben eine andere App auf dem Bildschirm) reagieren, um die Elemente auf dem erweiterten Begrüßungsbildschirm neu anzuordnen.

Erstellen Sie mit den folgenden Schritten einen erweiterten Begrüßungsbildschirm, der den standardmäßigen Begrüßungsbildschirm wirkungsvoll imitiert.

## Hinzufügen des Elements **Leere Seite** zur vorhandenen App


In diesem Thema wird davon ausgegangen, dass Sie einem vorhandenen App-Projekt für die universelle Windows-Plattform (UWP) mit C#, Visual Basic oder C++ einen erweiterten Begrüßungsbildschirm hinzufügen möchten.

-   Öffnen Sie die App in Visual Studio 2015.
-   Öffnen Sie in der Menüleiste **Projekt**, und klicken Sie auf **Neues Element hinzufügen**. Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
-   Fügen Sie der App in diesem Dialogfeld ein neues Element **Leere Seite** hinzu. In diesem Thema erhält die Seite mit dem erweiterten Begrüßungsbildschirm den Namen „ExtendedSplash“.

Beim Hinzufügen des Elements **Leere Seite** werden zwei Dateien erstellt: eine für Markup (ExtendedSplash.xaml) und eine für Code (ExtendedSplash.xaml.cs).

## Grundlegender XAML-Code für einen erweiterten Begrüßungsbildschirm


Führen Sie diese Schritte aus, um dem erweiterten Begrüßungsbildschirm ein Bild und ein Statussteuerelement hinzuzufügen.

In der Datei „ExtendedSplash.xaml“:

-   Ändern Sie die [**Background**](https://msdn.microsoft.com/library/windows/apps/br209396)-Eigenschaft des [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)-Standardelements, um sie an die Hintergrundfarbe anzupassen, die Sie im App-Manifest für den Begrüßungsbildschirm der App festgelegt haben (im Abschnitt **Visuelle Anlagen** der Datei „Package.appxmanifest“). Die Standardfarbe des Begrüßungsbildschirms ist Hellgrau (Hexadezimalwert \#464646). Beachten Sie, dass dieses **Grid**-Element standardmäßig bereitgestellt wird, wenn Sie eine neues Element **Leere Seite** erstellen. Sie müssen nicht zwingend ein **Grid**-Element verwenden. Es handelt sich dabei lediglich um eine gut geeignete Basis zum Erstellen eines erweiterten Begrüßungsbildschirms.
-   Fügen Sie dem [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)-Element ein [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)-Element hinzu. Sie verwenden dieses **Canvas**-Element zum Positionieren des Bilds für den erweiterten Begrüßungsbildschirm.
-   Fügen Sie dem [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)-Element ein [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752)-Element hinzu. Verwenden Sie für den erweiterten Begrüßungsbildschirm das gleiche Bild mit einer Größe von 600 x 320 Pixeln, das Sie für den standardmäßigen Begrüßungsbildschirm gewählt haben.
-   (Optional) Fügen Sie ein Statussteuerelement hinzu, um Benutzern anzuzeigen, dass die App geladen wird. In diesem Thema wird anstelle eines bestimmten oder unbestimmten [**ProgressBar**](https://msdn.microsoft.com/library/windows/apps/br227529)-Elements ein [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538)-Element hinzugefügt.

Fügen Sie in „ExtendedSplash.xaml“ folgenden Code hinzu, um die Elemente [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) und [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752) sowie ein [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538)-Steuerelement zu definieren:

```xml
    <Grid Background="#464646">
        <Canvas>
            <Image x:Name="extendedSplashImage" Source="Assets/SplashScreen.png"/>
            <ProgressRing Name="splashProgressRing" IsActive="True" Width="20" HorizontalAlignment="Center"></ProgressRing>
        </Canvas>
    </Grid>
```

**Hinweis**  Mit diesem Code wird die Breite des [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538)-Elements auf 20 Pixel festgelegt. Sie können die Breite manuell auf einen Wert festlegen, der für Ihre App geeignet ist. Das Steuerelement wird jedoch nicht für Breiten unterhalb von 20 Pixel gerendert.

 

## Grundlegender Code für die Klasse eines erweiterten Begrüßungsbildschirms


Der erweiterte Begrüßungsbildschirm muss entsprechend reagieren, wenn sich die Fenstergröße (nur Windows) oder die Ausrichtung ändert. Die Position des verwendeten Bilds muss aktualisiert werden, damit der erweiterte Begrüßungsbildschirm unabhängig von der Änderung des Fensters gut aussieht.

Führen Sie die folgenden Schritte aus, um Methoden zu definieren, damit der erweiterte Begrüßungsbildschirm richtig dargestellt wird.

1.  **Hinzufügen von erforderlichen Namespaces**

    Sie müssen „ExtendedSplash.xaml.cs“ die folgenden Namespaces hinzufügen, um auf die [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763)-Klasse und [**Window.SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br209055)-Ereignisse zugreifen zu können.

    ```cs
    using Windows.ApplicationModel.Activation;
    using Windows.UI.Core;
    ```

2.  **Erstellen Sie eine partielle Klasse, und deklarieren Sie Klassenvariablen.**

    Fügen Sie den folgenden Code in die Datei „ExtendedSplash.xaml.cs“ ein, um eine partielle Klasse zum Darstellen eines erweiterten Begrüßungsbildschirms zu erstellen.

    ```cs
    partial class ExtendedSplash : Page
    {
        internal Rect splashImageRect; // Rect to store splash screen image coordinates.
        private SplashScreen splash; // Variable to hold the splash screen object.
        internal bool dismissed = false; // Variable to track splash screen dismissal status.
        internal Frame rootFrame;

       // Define methods and constructor
    }
    ```

    Diese Klassenvariablen werden von verschiedenen Methoden verwendet. Die `splashImageRect`-Variable speichert die Koordinaten der Position, an der das System das Begrüßungsbildschirmbild für die App angezeigt hat. Die `splash`-Variable speichert ein [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763)-Objekt, und die `dismissed`-Variable verfolgt, ob der vom System angezeigte Begrüßungsbildschirm geschlossen wurde.

3.  **Definieren eines Konstruktors für die Klasse, der das Bild richtig positioniert**

    Mit dem folgenden Code wird ein Konstruktor für die Klasse des erweiterten Begrüßungsbildschirms definiert, der eine Überprüfung auf Ereignisse zum Ändern der Fenstergröße durchführt, das Bild und (optional) das Statussteuerelement auf dem erweiterten Begrüßungsbildschirm positioniert, einen Rahmen für die Navigation erstellt und eine asynchrone Methode zum Wiederherstellen eines gespeicherten Sitzungszustands aufruft.

    ```cs
    public ExtendedSplash(SplashScreen splashscreen, bool loadState)
    {
        InitializeComponent();

        // Listen for window resize events to reposition the extended splash screen image accordingly.
        // This ensures that the extended splash screen formats properly in response to window resizing.
        Window.Current.SizeChanged += new WindowSizeChangedEventHandler(ExtendedSplash_OnResize);

        splash = splashscreen;
        if (splash != null)
        {
            // Register an event handler to be executed when the splash screen has been dismissed.
            splash.Dismissed += new TypedEventHandler<SplashScreen, Object>(DismissedEventHandler);

            // Retrieve the window coordinates of the splash screen image.
            splashImageRect = splash.ImageLocation;
            PositionImage();

            // If applicable, include a method for positioning a progress control.
            PositionRing();
        }

        // Create a Frame to act as the navigation context
        rootFrame = new Frame();            
    }
    ```

    Registrieren Sie den [**Window.SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br209055)-Handler (in diesem Beispiel `ExtendedSplash_OnResize`) im Klassenkonstruktor, damit das Bild von der App auf dem erweiterten Begrüßungsbildschirm richtig positioniert wird.

4.  **Definieren Sie eine Klassenmethode, um das Bild auf dem erweiterten Begrüßungsbildschirm zu positionieren**

    Mit diesem Code wird veranschaulicht, wie das Bild mithilfe der `splashImageRect`-Klassenvariablen auf dem erweiterten Begrüßungsbildschirm positioniert wird.

    ```cs
    void PositionImage()
    {
        extendedSplashImage.SetValue(Canvas.LeftProperty, splashImageRect.X);
        extendedSplashImage.SetValue(Canvas.TopProperty, splashImageRect.Y);
        extendedSplashImage.Height = splashImageRect.Height;
        extendedSplashImage.Width = splashImageRect.Width;
    }
    ```

5.  **(Optional) Definieren einer Klassenmethode, um ein Statussteuerelement auf dem erweiterten Begrüßungsbildschirm zu positionieren**

    Wenn Sie sich für das Hinzufügen eines [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538)-Elements zum erweiterten Begrüßungsbildschirm entscheiden, sollten Sie es relativ zum Bild des Begrüßungsbildschirms positionieren. Fügen Sie der Datei „ExtendedSplash.xaml.cs“ den folgenden Code hinzu, um das **ProgressRing**-Element 32 Pixel unter dem Bild zu zentrieren.

    ```cs
    void PositionRing()
    {
        splashProgressRing.SetValue(Canvas.LeftProperty, splashImageRect.X + (splashImageRect.Width*0.5) - (splashProgressRing.Width*0.5));
        splashProgressRing.SetValue(Canvas.TopProperty, (splashImageRect.Y + splashImageRect.Height + splashImageRect.Height*0.1));
    }
    ```

6.  **Definieren Sie innerhalb der Klasse einen Handler für das Dismissed-Ereignis.**

    Legen Sie in der Datei „ExtendedSplash.xaml.cs“ die `dismissed`-Klassenvariable als Reaktion auf das [**SplashScreen.Dismissed**](https://msdn.microsoft.com/library/windows/apps/br224764)-Ereignis auf „true“ fest. Falls Ihre App über Setupvorgänge verfügt, fügen Sie sie diesem Ereignishandler hinzu.

    ```cs
    // Include code to be executed when the system has transitioned from the splash screen to the extended splash screen (application's first view).
    void DismissedEventHandler(SplashScreen sender, object e)
    {
        dismissed = true;

        // Complete app setup operations here...
    }
    ```

    Führen Sie nach Abschluss des App-Setups eine Navigation weg vom erweiterten Begrüßungsbildschirm durch. Mit dem folgenden Code wird eine Methode mit dem Namen `DismissExtendedSplash` definiert. Diese dient der Navigation zur `MainPage`, die in der Datei „MainPage.xaml“ der App definiert ist.

    ```cs
    void DismissExtendedSplash()
    {
        // Navigate to mainpage
        rootFrame.Navigate(typeof(MainPage));
        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }
    ```

7.  **Definieren Sie innerhalb der Klasse einen Handler für Windows.SizeChanged-Ereignisse.**

    Bereiten Sie den erweiterten Begrüßungsbildschirm so vor, dass die Elemente neu angeordnet werden, wenn Benutzer die Größe des Fensters ändern. Mit diesem Code wird beim Auftreten eines [**Window.SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br209055)-Ereignisses reagiert, indem die neuen Koordinaten erfasst werden und das Bild neu positioniert wird. Wenn Sie dem erweiterten Begrüßungsbildschirm ein Statussteuerelement hinzugefügt haben, sollten Sie es in diesem Ereignishandler ebenfalls neu positionieren.

    ```cs
    void ExtendedSplash_OnResize(Object sender, WindowSizeChangedEventArgs e)
    {
        // Safely update the extended splash screen image coordinates. This function will be executed when a user resizes the window.
        if (splash != null)
        {
            // Update the coordinates of the splash screen image.
            splashImageRect = splash.ImageLocation;
            PositionImage();

            // If applicable, include a method for positioning a progress control.
            // PositionRing();
        }
    }
    ```

    **Hinweis**  Bevor Sie versuchen, die Bildposition abzurufen, müssen Sie wie im Beispiel gezeigt sicherstellen, dass die Klassenvariable (`splash`) ein gültiges [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763)-Objekt enthält (siehe Beispiel).

     

8.  **(Optional) Hinzufügen einer Klassenmethode zum Wiederherstellen eines gespeicherten Sitzungszustands**

    Der Code, den Sie der [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)-Methode unter „Schritt 4: [Ändern Sie den Startaktivierungshandler](#modify-the-launch-activation-handler)“ hinzugefügt haben, führt dazu, dass die App beim Starten einen erweiterten Begrüßungsbildschirm anzeigt. Sie können das Hinzufügen der folgenden asynchronen Methode zur Datei „ExtendedSplash.xaml.cs“ erwägen, um alle Methoden für das Starten der App unter der Klasse für den erweiterten Begrüßungsbildschirm zusammenzufassen, damit der Zustand der App wiederhergestellt wird.

    ```cs
    async void RestoreStateAsync(bool loadState)
    {
        if (loadState)
        {
             // code to load your app's state here
        }
    }
    ```

    Beim Ändern des Startaktivierungshandlers in „App.xaml.cs“ sollten Sie zusätzlich `loadstate` auf „true“ festlegen, wenn das vorherige [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694)-Element der App **Terminated** lautete. Ist dies der Fall, stellt die `RestoreStateAsync`-Methode den vorherigen Zustand der App wieder her. Eine Übersicht über das Starten, Anhalten und Beenden einer App finden Sie unter [App-Lebenszyklus](app-lifecycle.md).

## Ändern Sie den Startaktivierungshandler


Beim Starten Ihrer App übergibt das System Begrüßungsbildschirminformationen an den Handler für das Startaktivierungsereignis. Sie können diese Informationen verwenden, um das Bild auf der Seite mit dem erweiterten Begrüßungsbildschirm richtig zu positionieren. Sie können diese Begrüßungsbildschirminformationen aus den Argumenten des Aktivierungsereignisses abrufen, die an den [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)-Handler der App übergeben werden (siehe `args`-Variable im folgenden Code).

Wenn Sie den [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)-Handler für Ihre App noch nicht außer Kraft gesetzt haben, finden Sie unter [App-Lebenszyklus](app-lifecycle.md) Informationen zum Behandeln von Aktivierungsereignissen.

Fügen Sie in der Datei „App.xaml.cs“ den folgenden Code zum Erstellen und Anzeigen eines erweiterten Begrüßungsbildschirms hinzu.

```cs
protected override void OnLaunched(LaunchActivatedEventArgs args)
{
    if (args.PreviousExecutionState != ApplicationExecutionState.Running)
    {
        bool loadState = (args.PreviousExecutionState == ApplicationExecutionState.Terminated);
        ExtendedSplash extendedSplash = new ExtendedSplash(args.SplashScreen, loadState);
        Window.Current.Content = extendedSplash;
    }

    Window.Current.Activate();
}
```

## Vollständiger Code


> **Hinweis**  Der folgende Code unterscheidet sich leicht von den Codeausschnitten der vorherigen Schritte.
-   „ExtendedSplash.xaml“ enthält eine `DismissSplash`-Schaltfläche. Beim Klicken auf diese Schaltfläche wird mit dem `DismissSplashButton_Click`-Ereignishandler die `DismissExtendedSplash`-Methode aufgerufen. Rufen Sie in der App `DismissExtendedSplash` auf, wenn das Laden von Ressourcen oder Initialisieren der UI in der App abgeschlossen ist.
-   Für diese App wird auch eine Projektvorlage für eine UWP-App verwendet, bei der die [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)-Navigation zum Einsatz kommt. Der Startaktivierungshandler ([**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)) definiert in „App.xaml.cs“ dann ein `rootFrame`-Element und verwendet es zum Festlegen des Inhalts für das App-Fenster.

ExtendedSplash.xaml: Dieses Beispiel enthält eine `DismissSplash`-Schaltfläche, da keine App-Ressourcen geladen werden müssen. Blenden Sie den erweiterten Begrüßungsbildschirm in der App automatisch aus, wenn das Laden von Ressourcen oder Vorbereiten der UI-Anfangselemente in der App abgeschlossen ist.

```xml
<Page
    x:Class="SplashScreenExample.ExtendedSplash"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:SplashScreenExample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="#464646">
        <Canvas>
            <Image x:Name="extendedSplashImage" Source="Assets/SplashScreen.png"/>
            <ProgressRing Name="splashProgressRing" IsActive="True" Width="20" HorizontalAlignment="Center"/>
        </Canvas>
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Bottom">
            <Button x:Name="DismissSplash" Content="Dismiss extended splash screen" HorizontalAlignment="Center" Click="DismissSplashButton_Click" />
        </StackPanel>
    </Grid>
</Page>
```

ExtendedSplash.xaml.cs: Beachten Sie, dass die `DismissExtendedSplash`-Methode vom Click-Ereignishandler für die `DismissSplash`-Schaltfläche aufgerufen wird. In der App benötigen Sie keine `DismissSplash`-Schaltfläche. Rufen Sie stattdessen `DismissExtendedSplash` auf, wenn das Laden der Ressourcen in der App abgeschlossen ist und Sie die Navigation zur Hauptseite durchführen möchten.

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;

using Windows.ApplicationModel.Activation;
using SplashScreenExample.Common;
using Windows.UI.Core;

// The Blank Page item template is documented at http://go.microsoft.com/fwlink/p/?LinkID=234238

namespace SplashScreenExample
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    partial class ExtendedSplash : Page
    {
        internal Rect splashImageRect; // Rect to store splash screen image coordinates.
        private SplashScreen splash; // Variable to hold the splash screen object.
        internal bool dismissed = false; // Variable to track splash screen dismissal status.
        internal Frame rootFrame;

        public ExtendedSplash(SplashScreen splashscreen, bool loadState)
        {
            InitializeComponent();

            // Listen for window resize events to reposition the extended splash screen image accordingly.
            // This is important to ensure that the extended splash screen is formatted properly in response to snapping, unsnapping, rotation, etc...
            Window.Current.SizeChanged += new WindowSizeChangedEventHandler(ExtendedSplash_OnResize);

            splash = splashscreen;

            if (splash != null)
            {
                // Register an event handler to be executed when the splash screen has been dismissed.
                splash.Dismissed += new TypedEventHandler<SplashScreen, Object>(DismissedEventHandler);

                // Retrieve the window coordinates of the splash screen image.
                splashImageRect = splash.ImageLocation;
                PositionImage();

                // Optional: Add a progress ring to your splash screen to show users that content is loading
                PositionRing();
            }

            // Create a Frame to act as the navigation context
            rootFrame = new Frame();

            // Restore the saved session state if necessary
            await RestoreStateAsync(loadState);
        }

        async void RestoreStateAsync(bool loadState)
        {
            if (loadState)
            {
                // TODO: write code to load state
            }
        }

        // Position the extended splash screen image in the same location as the system splash screen image.
        void PositionImage()
        {
            extendedSplashImage.SetValue(Canvas.LeftProperty, splashImageRect.X);
            extendedSplashImage.SetValue(Canvas.TopProperty, splashImageRect.Y);
            extendedSplashImage.Height = splashImageRect.Height;
            extendedSplashImage.Width = splashImageRect.Width;

        }

        void PositionRing()
        {
            splashProgressRing.SetValue(Canvas.LeftProperty, splashImageRect.X + (splashImageRect.Width*0.5) - (splashProgressRing.Width*0.5));
            splashProgressRing.SetValue(Canvas.TopProperty, (splashImageRect.Y + splashImageRect.Height + splashImageRect.Height*0.1));
        }

        void ExtendedSplash_OnResize(Object sender, WindowSizeChangedEventArgs e)
        {
            // Safely update the extended splash screen image coordinates. This function will be fired in response to snapping, unsnapping, rotation, etc...
            if (splash != null)
            {
                // Update the coordinates of the splash screen image.
                splashImageRect = splash.ImageLocation;
                PositionImage();
                PositionRing();
            }
        }

        // Include code to be executed when the system has transitioned from the splash screen to the extended splash screen (application's first view).
        void DismissedEventHandler(SplashScreen sender, object e)
        {
            dismissed = true;

            // Complete app setup operations here...
        }

        void DismissExtendedSplash()
        {
            // Navigate to mainpage
            rootFrame.Navigate(typeof(MainPage));
            // Place the frame in the current Window
            Window.Current.Content = rootFrame;
        }

        void DismissSplashButton_Click(object sender, RoutedEventArgs e)
        {
            DismissExtendedSplash();
        }
    }
}
```

App.xaml.cs: Dieses Projekt wurde mithilfe der UWP-App-Projektvorlage **Leere App (XAML)** in Visual Studio 2015 erstellt. Die Ereignishandler `OnNavigationFailed` und `OnSuspending` werden automatisch erstellt und müssen nicht geändert werden, um einen erweiterten Begrüßungsbildschirm zu implementieren. In diesem Thema wird nur `OnLaunched` geändert.

Falls Sie für Ihre App keine Projektvorlage verwendet haben, finden Sie unter „Schritt 4: [Ändern Sie den Startaktivierungshandler](#modify_the_launch_activation_handler) ein Beispiel für die Änderung von `OnLaunched` ohne [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)-Navigation.

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.ApplicationModel;
using Windows.ApplicationModel.Activation;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;

// The Blank Application template is documented at http://go.microsoft.com/fwlink/p/?LinkID=234227

namespace SplashScreenExample
{
    /// <summary>
    /// Provides application-specific behavior to supplement the default Application class.
    /// </summary>
    sealed partial class App : Application
    {
        /// <summary>
        /// Initializes the singleton application object.  This is the first line of authored code
        /// executed, and as such is the logical equivalent of main() or WinMain().
        /// </summary>
        public App()
        {
            Microsoft.ApplicationInsights.WindowsAppInitializer.InitializeAsync(
            Microsoft.ApplicationInsights.WindowsCollectors.Metadata |
            Microsoft.ApplicationInsights.WindowsCollectors.Session);
            this.InitializeComponent();
            this.Suspending += OnSuspending;
        }

        /// <summary>
        /// Invoked when the application is launched normally by the end user.  Other entry points
        /// will be used such as when the application is launched to open a specific file.
        /// </summary>
        /// <param name="e">Details about the launch request and process.</param>
        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
#if DEBUG
            if (System.Diagnostics.Debugger.IsAttached)
            {
                this.DebugSettings.EnableFrameRateCounter = true;
            }
#endif

            Frame rootFrame = Window.Current.Content as Frame;

            // Do not repeat app initialization when the Window already has content,
            // just ensure that the window is active
            if (rootFrame == null)
            {
                // Create a Frame to act as the navigation context and navigate to the first page
                rootFrame = new Frame();
                // Set the default language
                rootFrame.Language = Windows.Globalization.ApplicationLanguages.Languages[0];

                rootFrame.NavigationFailed += OnNavigationFailed;

                //  Display an extended splash screen if app was not previously running.
                if (e.PreviousExecutionState != ApplicationExecutionState.Running)
                {
                    bool loadState = (e.PreviousExecutionState == ApplicationExecutionState.Terminated);
                    ExtendedSplash extendedSplash = new ExtendedSplash(e.SplashScreen, loadState);
                    rootFrame.Content = extendedSplash;
                    Window.Current.Content = rootFrame;
                }
            }

            if (rootFrame.Content == null)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(typeof(MainPage), e.Arguments);
            }
            // Ensure the current window is active
            Window.Current.Activate();
        }

        /// <summary>
        /// Invoked when Navigation to a certain page fails
        /// </summary>
        /// <param name="sender">The Frame which failed navigation</param>
        /// <param name="e">Details about the navigation failure</param>
        void OnNavigationFailed(object sender, NavigationFailedEventArgs e)
        {
            throw new Exception("Failed to load Page " + e.SourcePageType.FullName);
        }

        /// <summary>
        /// Invoked when application execution is being suspended.  Application state is saved
        /// without knowing whether the application will be terminated or resumed with the contents
        /// of memory still intact.
        /// </summary>
        /// <param name="sender">The source of the suspend request.</param>
        /// <param name="e">Details about the suspend request.</param>
        private void OnSuspending(object sender, SuspendingEventArgs e)
        {
            var deferral = e.SuspendingOperation.GetDeferral();
            // TODO: Save applicaiton state and stop any background activity
            deferral.Complete();
        }
    }
}
```

## Verwandte Themen


* [App-Lebenszyklus](app-lifecycle.md)

**Referenz**

* [**Windows.ApplicationModel.Activation-Namespace**](https://msdn.microsoft.com/library/windows/apps/br224766)
* [**Windows.ApplicationModel.Activation.SplashScreen-Klasse**](https://msdn.microsoft.com/library/windows/apps/br224763)
* [**Windows.ApplicationModel.Activation.SplashScreen.ImageLocation-Eigenschaft**](https://msdn.microsoft.com/library/windows/apps/br224765)
* [**Windows.ApplicationModel.Core.CoreApplicationView.Activated-Ereignis**](https://msdn.microsoft.com/library/windows/apps/br225018)

 

 



<!--HONumber=Jun16_HO4-->


