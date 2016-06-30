---
author: mcleanbyron
ms.assetid: 141900dd-f1d3-4432-ac8b-b98eaa0b0da2
description: "Hier finden Sie Lösungen für allgemeine Entwicklungsprobleme mit den Microsoft Advertising-Bibliotheken in XAML-Apps."
title: XAML- und C#-Handbuch zur Problembehandlung
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: ef9ad8f8056b17793d7ad8230e410e014edf2c95

---

# XAML- und C#-Handbuch zur Problembehandlung

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Dieses Thema enthält Lösungen für allgemeine Entwicklungsprobleme mit den Microsoft Advertising-Bibliotheken in XAML-Apps.

-   [XAML](#xaml)

    -   [AdControl wird nicht angezeigt](#xaml-notappearing)

    -   [Blackbox blinkt und wird ausgeblendet](#xaml-blackboxblinksdisappears)

    -   [Anzeigen werden nicht aktualisiert](#xaml-adsnotrefreshing)

-   [C#](#csharp)

    -   [AdControl wird nicht angezeigt](#csharp-adcontrolnotappearing)

    -   [Blackbox blinkt und wird ausgeblendet](#csharp-blackboxblinksdisappears)

    -   [Anzeigen werden nicht aktualisiert](#csharp-adsnotrefreshing)

<span id="xaml"/>
## XAML

<span id="xaml-notappearing"/>
### AdControl wird nicht angezeigt

1.  Stellen Sie sicher, dass die **Internet (Client)**-Funktion in „Package.appxmanifest“ ausgewählt ist.

2.  Überprüfen Sie die ID der Anwendung und der Anzeigeneinheit. Diese IDs müssen mit der Anwendungs-ID und Anzeigeneinheits-ID übereinstimmen, die Sie im Windows Dev Center erhalten haben. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in Ihrer App](set-up-ad-units-in-your-app.md).

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}" ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

3.  Überprüfen Sie die **Height**-Eigenschaft und **Width**-Eigenschaft. Diese müssen auf eine der [unterstützten Anzeigengrößen für Werbebanner](supported-ad-sizes-for-banner-ads.md) festgelegt werden.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

4.  Überprüfen Sie die Elementposition. [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) muss sich im sichtbaren Bereich befinden.

5.  Überprüfen Sie die **Visibility**-Eigenschaft. Die optionale **Visibility**-Eigenschaft darf nicht auf „collapsed“ oder „hidden“ festgelegt werden. Diese Eigenschaft kann als Inlineeigenschaft (wie unten dargestellt) oder in einem externen Stylesheet festgelegt werden.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Visibility="Visible"
                  Width="728" Height="90" />
    ```

6.  Überprüfen Sie die **IsEnabled**-Eigenschaft. Die optionale `IsEnabled`-Eigenschaft muss auf `True` festgelegt sein.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  IsEnabled="True"
                  Width="728" Height="90" />
    ```

7.  Überprüfen Sie das übergeordnete Element von **AdControl**. Wenn sich das **AdControl**-Element in einem übergeordneten Element befindet, muss das übergeordnete Element aktiv und sichtbar sein.

    ``` syntax
    <StackPanel>
        <UI:AdControl AdUnitId="{AdUnitID}"
                      ApplicationId="{ApplicationID}"
                      Width="728" Height="90" />
    </StackPanel>
    ```

8.  Stellen Sie sicher, dass **AdControl** im Viewport nicht ausgeblendet ist. **AdControl** muss sichtbar sein, damit Anzeigen ordnungsgemäß dargestellt werden.

9.  Echte Werte für **ApplicationId** und **AdUnitId** sollten nicht im Emulator getestet werden. Um sicherzustellen, dass **AdControl** erwartungsgemäß funktioniert, verwenden Sie sowohl für **ApplicationId** als auch für **AdUnitId** die Test-IDs in [Testmoduswerte](test-mode-values.md).

<span id="xaml-blackboxblinksdisappears"/>
### Blackbox blinkt und wird ausgeblendet

1.  Überprüfen Sie noch einmal alle Schritte im vorherigen Abschnitt [AdControl wird nicht angezeigt](#xaml-notappearing).

2.  Behandeln Sie das **ErrorOccurred**-Ereignis, und bestimmen Sie anhand der an den Ereignishandler übergebenen Meldung, ob ein Fehler aufgetreten ist und welche Art von Fehler ausgelöst wurde. Weitere Informationen finden Sie unter [Fehlerbehandlung in XAML/Exemplarische Vorgehensweise für C#](error-handling-in-xamlc-walkthrough.md).

    Dieses Beispiel veranschaulicht einen **ErrorOccurred**-Ereignishandler. Der erste Ausschnitt ist das XAML-UI-Markup.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  ErrorOccurred="adControl_ErrorOccurred" />

    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    Dieses Beispiel veranschaulicht den entsprechenden Code.

    ``` syntax
    private void adControl_ErrorOccurred(object sender,               
        Microsoft.Advertising.WinRT.UI.AdErrorEventArgs e)
    {
        TextBlock1.Text = e.Error.Message;
    }
    ```

    Eine Blackbox wird am häufigsten dadurch verursacht, dass keine Anzeige verfügbar ist. Dieser Fehler bedeutet, dass durch die Anforderung keine Anzeige zurückgegeben werden kann.

3.  [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) verhält sich normal.

    **AdControl** wird standardmäßig reduziert, wenn keine Anzeige dargestellt werden kann. Wenn andere Elemente demselben übergeordneten Element untergeordnet sind, können sie verschoben werden, um den freien Platz des reduzierten **AdControl**-Elements zu füllen, und bei der nächsten Anforderung erweitert werden.

<span id="xaml-adsnotrefreshing"/>
### Anzeigen werden nicht aktualisiert

1.  Überprüfen Sie die [IsAutoRefreshEnabled](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled.aspx)-Eigenschaft. Diese optionale Eigenschaft ist standardmäßig auf **True** festgelegt. Beim Wert **False** muss die [Refresh](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.refresh.aspx)-Methode verwendet werden, um eine weitere Anzeige abzurufen.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="True" />
    ```

2.  Überprüfen Sie Aufrufe der **Refresh**-Methode. Bei Verwendung der automatischen Aktualisierung kann **Refresh** nicht verwendet werden, um eine weitere Anzeige abzurufen. Bei Verwendung der manuellen Aktualisierung sollte **Refresh** abhängig von der aktuellen Datenverbindung des Geräts erst nach mindestens 30 bis 60 Sekunden aufgerufen werden.

    In den folgenden Codeausschnitten wird die Verwendung der **Refresh**-Methode veranschaulicht. Der erste Ausschnitt ist das XAML-UI-Markup.

    ``` syntax
    <UI:AdControl x:Name="adControl1"
                  AdUnitId="{AdUnit_ID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="False" />
    ```

    Dieser Codeausschnitt zeigt ein Beispiel für den C#-CodeBehind-Code des UI-Markups.

    ``` syntax
    public Ads()
    {
        var timer = new DispatcherTimer() { Interval = TimeSpan.FromSeconds(60) };
        timer.Tick += (s, e) => adControl1.Refresh();
        timer.Start();
    }
    ```

3.  **AdControl** verhält sich normal. In einigen Fällen wird dieselbe Anzeige mehrmals in Folge angezeigt, wodurch der Eindruck entsteht, dass Anzeigen nicht aktualisiert werden.

<span id="csharp"/>
## C\# #

<span id="csharp-adcontrolnotappearing"/>
### AdControl wird nicht angezeigt

1.  Stellen Sie sicher, dass die **Internet (Client)**-Funktion in „Package.appxmanifest“ ausgewählt ist.

2.  Stellen Sie sicher, dass **AdControl** instanziiert ist. Wenn **AdControl** nicht instanziiert wird, ist es nicht verfügbar.

    ``` syntax
    using Microsoft.Advertising.WinRT.UI;

    namespace App1
    {
        public sealed partial class MainPage : Page
        {
            AdControl adControl;

            public MainPage()
            {
                this.InitializeComponent();

                adControl = new AdControl()
                {
                    ApplicationId = "{ApplicationID}",
                    AdUnitId = "{AdUnitID}",
                    Height = 90,
                    Width = 728
                };
            }
        }
    }
    ```

3.  Überprüfen Sie die ID der Anwendung und der Anzeigeneinheit. Diese IDs müssen mit der Anwendungs-ID und Anzeigeneinheits-ID übereinstimmen, die Sie im Windows Dev Center erhalten haben. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in Ihrer App](set-up-ad-units-in-your-app.md).

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    ```

4.  Überprüfen Sie den **Height**-Parameter und **Width**-Parameter. Diese müssen auf eine der [unterstützten Anzeigengrößen für Werbebanner](supported-ad-sizes-for-banner-ads.md) festgelegt werden.

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;adControl.Width = 728;
    ```

5.  Stellen Sie sicher, dass **AdControl** einem übergeordneten Element hinzugefügt wurde. Damit **AdControl** angezeigt wird, muss es einem übergeordneten Steuerelement (z. B. **StackPanel** oder **Grid**) als untergeordnetes Element hinzugefügt werden.

    ``` syntax
    ContentPanel.Children.Add(adControl);
    ```

6.  Überprüfen Sie den **Margin**-Parameter. **AdControl** muss sich im sichtbaren Bereich befinden.

7.  Überprüfen Sie die **Visibility**-Eigenschaft. Die optionale **Visibility**-Eigenschaft muss auf **Visible** festgelegt sein.

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.Visibility = System.Windows.Visibility.Visible;
    ```

8.  Überprüfen Sie die **IsEnabled**-Eigenschaft. Die optionale **IsEnabled**-Eigenschaft muss auf **True** festgelegt sein.

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.IsEnabled = True;
    ```

9.  Überprüfen Sie das übergeordnete Element von **AdControl**. Das übergeordnete Element muss aktiv und sichtbar sein.

10. Echte Werte für **ApplicationId** und **AdUnitId** sollten nicht im Emulator getestet werden. Um sicherzustellen, dass **AdControl** erwartungsgemäß funktioniert, verwenden Sie sowohl für **ApplicationId** als auch für **AdUnitId** die Test-IDs in [Testmoduswerte](test-mode-values.md).

<span id="csharp-blackboxblinksdisappears"/>
### Blackbox blinkt und wird ausgeblendet

1.  Überprüfen Sie noch einmal alle Schritte im Abschnitt oben [AdControl wird nicht angezeigt](#csharp-adcontrolnotappearing).

2.  Behandeln Sie das **ErrorOccurred**-Ereignis, und bestimmen Sie anhand der an den Ereignishandler übergebenen Meldung, ob ein Fehler aufgetreten ist und welche Art von Fehler ausgelöst wurde. Weitere Informationen finden Sie unter [Fehlerbehandlung in XAML/Exemplarische Vorgehensweise für C#](error-handling-in-xamlc-walkthrough.md).

    Die folgenden Beispiele veranschaulichen den grundlegenden Code zum Implementieren eines Fehleraufrufs. Durch diesen XAML-Code wird ein **TextBlock**-Element definiert, mit dem die Fehlermeldung angezeigt wird.

    ``` syntax
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    Durch diesen C#-Code wird die Fehlermeldung abgerufen und in **TextBlock** angezeigt.

    ``` syntax
    using Microsoft.Advertising.WinRT.UI;

    namespace App1
    {
        public partial class MainPage : Page
        {
            AdControl adControl;

            public MainPage()
            {
                this.InitializeComponent();

                adControl = new AdControl();
                adControl.ApplicationId = "{ApplicationID}";
                adControl.AdUnitId = "{AdUnitID}";
                adControl.Height = 90;
                adControl.Width = 728;
                adControl.ErrorOccurred += (s,e) =>
                {
                    TextBlock1.Text = e.Error.Message;
                };
            }
        }
    }
    ```

    Eine Blackbox wird am häufigsten dadurch verursacht, dass keine Anzeige verfügbar ist. Dieser Fehler bedeutet, dass durch die Anforderung keine Anzeige zurückgegeben werden kann.

3.  **AdControl** verhält sich normal. In einigen Fällen wird dieselbe Anzeige mehrmals in Folge angezeigt, wodurch der Eindruck entsteht, dass Anzeigen nicht aktualisiert werden.

<span id="csharp-adsnotrefreshing"/>
### Anzeigen werden nicht aktualisiert

1.  Überprüfen Sie die **IsAutoRefreshEnabled**-Eigenschaft. Diese optionale Eigenschaft ist standardmäßig auf **True** festgelegt. Beim Wert **False** muss die **Refresh**-Methode verwendet werden, um eine weitere Anzeige abzurufen.

    Im folgenden Beispiel wird die Verwendung der **IsAutoRefreshEnabled**-Eigenschaft veranschaulicht.

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.IsAutoRefreshEnabled = true;
    ```

2.  Überprüfen Sie Aufrufe der **Refresh**-Methode. Bei Verwendung der automatischen Aktualisierung kann **Refresh** nicht verwendet werden, um eine weitere Anzeige abzurufen. Bei Verwendung der manuellen Aktualisierung sollte **Refresh** abhängig von der aktuellen Datenverbindung des Geräts erst nach mindestens 30 bis 60 Sekunden aufgerufen werden.

    Das folgende Beispiel veranschaulicht, wie die **Refresh**-Methode aufgerufen wird.

    ``` syntax
    public MainPage()
    {
        InitializeComponent();

        adControl = new AdControl();
        adControl.ApplicationId = "{ApplicationID}";
        adControl.AdUnitId = "{AdUnitID}";
        adControl.Height = 90;
        adControl.Width = 728;
        adControl.IsAutoRefreshEnabled = false;

        ContentPanel.Children.Add(adControl);

        var timer = new DispatcherTimer() { Interval = TimeSpan.FromSeconds(60) };
        timer.Tick += (s, e) => adControl.Refresh();
        timer.Start();
    }
    ```

3.  **AdControl** verhält sich normal. In einigen Fällen wird dieselbe Anzeige mehrmals in Folge angezeigt, wodurch der Eindruck entsteht, dass Anzeigen nicht aktualisiert werden.

 

 



<!--HONumber=Jun16_HO4-->


