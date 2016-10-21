---
author: mcleanbyron
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: "Erfahren Sie, wie Sie mithilfe von C# eine interstitielle Anzeige veröffentlichen."
title: "Beispielcode für interstitielle Anzeigen in C#"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: 5969bfacd34bcfab5f1bebd2cfbade4fd16c5a39


---

# Beispielcode für interstitielle Anzeigen in C\# #  




In diesem Thema wird die Veröffentlichung interstitieller Anzeigen mithilfe von C# beschrieben. Schritt-für-Schritt-Anweisungen für die Konfigurierung Ihres Projekts zur Verwendung dieses Codes finden Sie unter [Interstitielle Anzeigen](interstitial-ads.md). Ein vollständiges Beispielprojekt, das die Hinzufügung von Videointerstitialanzeigen zu einer XAML-App mithilfe von C# zeigt, finden Sie unter den [Anzeigenbeispielen auf GitHub](http://aka.ms/githubads).


## Codebeispiel

Dieses Codebeispiel zeigt eine funktionierende MainPage.xaml.cs-Codedatei, die eine Interstitialanzeige implementiert. Dieser Code geht davon aus, dass die Datei MainPage.xaml eine funktionierende Schaltfläche mit einem **Click**-Ereignis besitzt, das von einer Methode namens **button_Click** ausgelöst und behandelt wird. Dieser Code startet die Interstitialanzeige, wenn das **Click**-Ereignis für die Schaltfläche ausgelöst wird.

Ersetzen Sie vor der Übermittlung Ihrer App an den Store den Text in den **AppID**- und **AdUnitId**-Variablen mit Livewerten. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in Ihrer App](set-up-ad-units-in-your-app.md).

``` syntax
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
using Microsoft.Advertising.WinRT.UI;


namespace BasicCSharpInterstitialUWP
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        InterstitialAd MyVideoAd = new InterstitialAd();

        public MainPage()
        {
            this.InitializeComponent();
        }

        private void button_Click(object sender, RoutedEventArgs e)
        {
            // Define ApplicationId and AdUnitId.
            // Test values are shown here. Replace the test values with live values before submitting the app to the Store.
            var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
            var MyAdUnitId = "11389925";

            MyVideoAd.AdReady += MyVideoAd_AdReady;
            MyVideoAd.ErrorOccurred += MyVideoAd_ErrorOccurred;
            MyVideoAd.Completed += MyVideoAd_Completed;
            MyVideoAd.Cancelled += MyVideoAd_Cancelled;

            // Pre-fetch an ad 30-60 seconds before you need it.
            MyVideoAd.RequestAd(AdType.Video, MyAppId, MyAdUnitId);
        }

        void MyVideoAd_AdReady(object sender, object e)
        {
            if ((InterstitialAdState.Ready) == (MyVideoAd.State))
            {
                MyVideoAd.Show();
            }
        }

        void MyVideoAd_ErrorOccurred(object sender, AdErrorEventArgs e)
        {
            // Add your code here.
        }

        void MyVideoAd_Completed(object sender, object e)
        {
            // Add your code here.
        }

        void MyVideoAd_Cancelled(object sender, object e)
        {
            // Add your code here.  
        }
    }
}
```

 
## Verwandte Themen

* [Anzeigenbeispiele auf GitHub](http://aka.ms/githubads)
 



<!--HONumber=Aug16_HO3-->


