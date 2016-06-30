---
author: mcleanbyron
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: "Erfahren Sie mehr über die Verwendung von interstitiellen Anzeigen in Windows 10-, Windows 8.1- oder Windows Phone 8.1-Apps mithilfe der Microsoft Advertising-Bibliotheken im Microsoft Store Engagement and Monetization SDK."
title: Interstitielle Anzeigen
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 0f159409bb584aacaf66550efe8d147cd8fddd50

---

# Interstitielle Anzeigen


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

In dieser Schritt-für-Schritt-Einführung wird die Verwendung von interstitiellen Anzeigen in Windows 10-, Windows 8.1- oder Windows Phone 8.1-Apps mithilfe der Microsoft Advertising-Bibliotheken im Microsoft Store Engagement and Monetization SDK gezeigt.

Vollständige Beispielprojekte, die das Hinzufügen von interstitiellen Anzeigen zu JavaScript-/HTML- und XAML-Apps unter Verwendung von C# und C++ zeigen, finden Sie in den [Anzeigenbeispielen auf GitHub](http://aka.ms/githubads).

<span id="whatareinterstitialads10"/>
## Was sind interstitielle Anzeigen?

Im Gegensatz zu Banneranzeigen werden interstitielle Anzeigen (oder *Interstitialanzeigen*) auf dem gesamten Bildschirm der App angezeigt. In Spielen werden häufig zwei grundlegende Formen verwendet.

* Bei *Paywall*-Anzeigen muss der Benutzer eine Anzeige in regelmäßigen Intervallen ansehen. Dies findet beispielsweise zwischen Spiellevels statt:

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* Bei *prämienbasierten* Anzeigen möchte der Benutzer ausdrücklich einen Vorteil nutzen, z. B. einen Hinweis oder zusätzliche Zeit zum Abschließen eines Levels erhalten, und initialisiert daher über die Benutzeroberfläche der App die Videoanzeige.

    Es ist wichtig, zu beachten, dass dieses SDK keine Benutzeroberfläche behandelt, ausgenommen zum Zeitpunkt der Videowiedergabe. Informieren Sie sich in den [bewährten Methoden für Interstitialanzeigen](ui-and-user-experience-guidelines.md#interstitialbestpractices10) über Richtlinien für das, was Sie tun und was Sie vermeiden sollten, wenn Sie sich überlegen, wie Sie interstitielle Anzeigen in Ihre App integrieren können.

## Erstellen einer App mit interstitiellen Anzeigen


### Voraussetzungen

1.  Installieren Sie das [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk) mit Visual Studio 2015 oder Visual Studio 2013.

2.  Öffnen Sie in Visual Studio Ihr Projekt, oder erstellen Sie ein neues Projekt.

### Codeentwicklung

* [Schritte für eine XAML/.NET-App](#interstitialadsxaml10)

* [Schritte für HTML/JavaScript](#interstitialadshtml10)

* [Schritte für C++ (DirectX Interop)](#interstitialadsdirectx10)

<span id="interstitialadsxaml10"/>
### Interstitialanzeigen (XAML/.NET)

> **Hinweis**   Dieser Abschnitt enthält Beispiele für C#-. Visual Basic und C++ werden jedoch ebenfalls unterstützt.
 
1. Öffnen Sie das Projekt in Visual Studio.
2. Wählen Sie im **Verweis-Manager** je nach Projekttyp einen der folgenden Verweise aus:

    -   Für ein Projekt für die Universelle Windows-Plattform (UWP): Erweitern Sie **Universelles Windows**, klicken Sie auf **Erweiterungen** und aktivieren Sie anschließend das Kontrollkästchen neben **Microsoft Advertising SDK für XAML** (Version 10.0).

    -   Für ein Windows 8.1-Projekt: Erweitern Sie **Windows 8.1**, klicken Sie auf **Erweiterungen**, und aktivieren Sie anschließend das Kontrollkästchen neben **Ad Mediator SDK für Windows 8.1 XAML**. Mit dieser Option werden die Microsoft Advertising- und Ad Mediator-Bibliotheken dem Projekt hinzugefügt. Die Ad Mediator-Bibliotheken können Sie jedoch außer Acht lassen.

    -   Für ein Windows Phone 8.1-Projekt: Erweitern Sie **Windows Phone 8.1**, klicken Sie auf **Erweiterungen** und aktivieren Sie anschließend das Kontrollkästchen neben **Ad Mediator SDK für Windows Phone 8.1 XAML**. Mit dieser Option werden die Microsoft Advertising- und Ad Mediator-Bibliotheken dem Projekt hinzugefügt. Die Ad Mediator-Bibliotheken können Sie jedoch außer Acht lassen.

3.  Integrieren Sie den folgenden Namespace-Verweis in den App-Code.

    ``` syntax
    using Microsoft.Advertising.WinRT.UI;
    ```

4.  Deklarieren Sie Ihre `MyAppId`- und `MyAdUnitId`-Eigenschaften.

    ``` syntax
    var MyAppId = "<your app id for windows>";
    var MyAdUnitId = "<your adunit for windows";

    // if your code is in a universal solution and resides in the shared project
    // you can opt to use #if WINDOWS_APP or WINDOWS_PHONE_APP to override with different
    // identifiers for each
#if WINDOWS_PHONE_APP
    MyAppId = "<your app id for phone>";
    MyAdUnitId = "<your adunit for phone>";
#endif
    ```

    > **Hinweis**   Ersetzen Sie die Testwerte mit Livewerten, bevor Sie Ihre App übermitteln.

5.  Instanziieren Sie ein [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx), verbinden Sie alle Ereignishandler, und fordern Sie eine Anzeige an.

    ``` syntax
    // instantiate an InterstitialAd
    InterstitialAd MyVideoAd = new InterstitialAd();

    // wire up all 4 events, see below for function templates
    MyVideoAd.AdReady += MyVideoAd_AdReady;
    MyVideoAd.ErrorOccurred += MyVideoAd_ErrorOccurred;
    MyVideoAd.Completed += MyVideoAd_Completed;
    MyVideoAd.Cancelled += MyVideoAd_Cancelled;

    // pre-fetch an ad 30-60 seconds before you need it
    MyVideoAd.RequestAd(AdType.Video, MyAppId, MyAdUnitId);
    ```

6.  Achten Sie darauf, dass die Anzeige an der Stelle im Code, an der sie angezeigt werden soll, bereit ist, und zeigen Sie die Anzeige an.

    ``` syntax
    if ((InterstitialAdState.Ready) == (MyVideoAd.State))
    {
      MyVideoAd.Show();
    }
    ```

7.  Definieren und codieren Sie die Ereignisse.

    ``` syntax
    void MyVideoAd_AdReady(object sender, object e)
    {
      // code
    }

    void MyVideoAd_ErrorOccurred(object sender, AdErrorEventArgs e)
    {
      // code
    }

    void MyVideoAd_Completed(object sender, object e)
    {  
      // code
    }

    void MyVideoAd_Cancelled(object sender, object e)
    {
      // code
    }
    ```

8.  Weisen Sie die `MyAppId`-Eigenschaft dem Testwert zu, der in [Testmoduswerte](test-mode-values.md) angegeben wird. Dieser Wert wird nur zu Testzwecken verwendet. Sie ersetzen ihn vor der Veröffentlichung Ihrer App mit einem Livewert.

    ``` syntax
    var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

9.  Weisen Sie die `MyAdUnitId`-Eigenschaft dem Testwert zu, der in [Testmoduswerte](test-mode-values.md) angegeben wird. Dieser Wert wird nur zu Testzwecken verwendet. Sie ersetzen ihn vor der Veröffentlichung Ihrer App mit einem Livewert.

    ``` syntax
    var MyAdUnitId = "11389925";
    ```

10.  Erstellen und testen Sie Ihre App, um zu überprüfen, ob sie Testanzeigen anzeigt.

<span id="interstitialadshtml10"/>
### Interstitielle Anzeigen (HTML/JavaScript)

In diesem Beispiel wird angenommen, dass Sie ein Universal App-Projekt für JavaScript in Visual Studio 2015 erstellt haben und dieses für eine bestimmte CPU bestimmt ist.

1. Öffnen Sie das Projekt in Visual Studio.
2.  Wählen Sie im **Verweis-Manager** je nach Projekttyp einen der folgenden Verweise aus:

    -   Für ein Projekt für die Universelle Windows-Plattform (UWP): Erweitern Sie **Universelles Windows**, klicken Sie auf **Erweiterungen** und aktivieren Sie anschließend das Kontrollkästchen neben **Microsoft Advertising SDK für JavaScript** (Version 10.0).

    -   Für ein Windows 8.1-Projekt: Erweitern Sie **Windows 8.1**, klicken Sie auf **Erweiterungen** und aktivieren Sie anschließend das Kontrollkästchen neben **Microsoft Advertising SDK für Windows 8.1 Native (JS)**.

    -   Für ein Windows 8.1-Projekt: Erweitern Sie **Windows 8.1**, klicken Sie auf **Erweiterungen** und aktivieren Sie anschließend das Kontrollkästchen neben **Microsoft Advertising SDK for Windows Phone 8.1 Native (JS)**.

3.  Fügen Sie den folgenden Skriptverweis in den HTML-Code ein.

    ``` syntax
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

4.  Deklarieren Sie Ihre `myAppId`- und `myAdUnitId`-Eigenschaften.

    ``` syntax
    <script>
      var myAppId = "<your app id>";
      var myAdUnitId = "<your adunit id>";
    </script>
    ```

5.  Instanziieren Sie ein **InterstitialAd**, verbinden Sie alle Ereignishandler, und fordern Sie eine Anzeige an.

    ``` syntax
    // instantiate an InterstitialAd
    window.interstitialAd = new MicrosoftNSJS.Advertising.InterstitialAd();

    // wire up all 4 events, see below for function templates
    window.interstitialAd.onAdReady = readyHandler;
    window.interstitialAd.onErrorOccurred = errorHandler;
    window.interstitialAd.onCompleted = completeHandler;
    window.interstitialAd.onCancelled = cancelHandler;

    // pre-fetch an ad 30-60 seconds before you need it
    var myAdType = MicrosoftNSJS.Advertising.InterstitialAdType.video;
    window.interstitialAd.requestAd(myAdType, myAppId, myAdUnitId);
    ```

6.  Achten Sie darauf, dass die Anzeige an der Stelle im Code, an der sie angezeigt werden soll, bereit ist, und zeigen Sie die Anzeige an.

    ``` syntax
    if ((MicrosoftNSJS.Advertising.InterstitialAdState.ready) == (window.interstitialAd.state)) {
             window.interstitialAd.show();
    }
    ```

7.  Definieren und codieren Sie die Ereignisse.

    ``` syntax
    function readyHandler(sender) {
      // code
    }

    function errorHandler(sender, args) {
      // code
    }

    function completeHandler(sender) {
      // code
    }

    function cancelHandler(sender) {
      // code
    }
    ```

7.  Weisen Sie die `MyAppId`-Eigenschaft dem Testwert zu, der in [Testmoduswerte](test-mode-values.md) angegeben wird. Dieser Wert wird nur zu Testzwecken verwendet. Sie ersetzen ihn vor der Veröffentlichung Ihrer App mit einem Livewert.

    ``` syntax
    var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

8.  Weisen Sie die `MyAdUnitId`-Eigenschaft dem Testwert zu, der in [Testmoduswerte](test-mode-values.md) angegeben wird. Dieser Wert wird nur zu Testzwecken verwendet. Sie ersetzen ihn vor der Veröffentlichung Ihrer App mit einem Livewert.

    ``` syntax
    var MyAdUnitId = "11389925";
    ```

9.  Erstellen und testen Sie Ihre App, um zu überprüfen, ob sie Testanzeigen anzeigt.

<span id="interstitialadsdirectx10"/>
### Interstitielle Anzeigen (C++ und DirectX mit XAML-Interoperabilität)

In diesem Beispiel wird angenommen, dass Sie ein Universal App-Projekt für XAML in Visual Studio 2015 erstellt haben, das für eine bestimmte CPU-Architektur bestimmt ist.

> **Wichtig**   Dieser Code wird in C++ geschrieben, wie für DirectX erforderlich.

 
1. Öffnen Sie das Projekt in Visual Studio.
1.  Wählen Sie im **Verweis-Manager** je nach Projekttyp einen der folgenden Verweise aus:

    -   Für ein Projekt für die Universelle Windows-Plattform (UWP): Erweitern Sie **Universelles Windows**, klicken Sie auf **Erweiterungen** und aktivieren Sie anschließend das Kontrollkästchen neben **Microsoft Advertising SDK für XAML** (Version 10.0).

    -   Für ein Windows 8.1-Projekt: Erweitern Sie **Windows 8.1**, klicken Sie auf **Erweiterungen**, und aktivieren Sie anschließend das Kontrollkästchen neben **Ad Mediator SDK für Windows 8.1 XAML**. Mit dieser Option werden die Microsoft Advertising- und Ad Mediator-Bibliotheken dem Projekt hinzugefügt. Die Ad Mediator-Bibliotheken können Sie jedoch außer Acht lassen.

    -   Für ein Windows Phone 8.1-Projekt: Erweitern Sie **Windows Phone 8.1**, klicken Sie auf **Erweiterungen** und aktivieren Sie anschließend das Kontrollkästchen neben **Ad Mediator SDK für Windows Phone 8.1 XAML**. Mit dieser Option werden die Microsoft Advertising- und Ad Mediator-Bibliotheken dem Projekt hinzugefügt. Die Ad Mediator-Bibliotheken können Sie jedoch außer Acht lassen.

2.  Deklarieren Sie in der entsprechenden Headerdatei für Ihre App das Interstitialanzeigenobjekt und die zugehörigen Eigenschaften/Methoden.

    ``` syntax
    Microsoft::Advertising::WinRT::UI::InterstitialAd^ m_ia;
    void OnAdReady(Object^ sender, Object^ args);
    void OnAdCompleted(Object^ sender, Object^ args);
    void OnAdCancelled(Object^ sender, Object^ args);
    void OnAdError (Object^ sender,  Microsoft::Advertising::WinRT::UI::AdErrorEventArgs^ args);
    ```

3.  Deklarieren Sie Ihre `AppId`- und `AdUnitId`-Eigenschaften.

    ``` syntax
    #if WINDOWS_PHONE_APP
    static Platform::String^ IA_APPID = L"<your app id for phone>";
    static Platform::String^ IA_ADUNITID = L"<your ad unit for phone>";
    #endif

    #if WINDOWS_APP
    static Platform::String^ IA_APPID = L"<your app id for windows>";
    static Platform::String^ IA_ADUNITID = L"<your ad unit for windows>";
    #endif
    ```

4.  Fügen Sie in der CPP-Datei einen Namespaceverweis hinzu.

    ``` syntax
    using namespace Microsoft::Advertising::WinRT::UI;
    ```

5.  Instanziieren Sie ein **InterstitialAd**, verbinden Sie alle Ereignishandler, und fordern Sie eine Anzeige an.

    ``` syntax
    // Instantiate an InterstitialAd.
    m_ia = ref new InterstitialAd();

    // Wire up all 4 events, see below for function templates.            
    m_ia->AdReady += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdReady);
    m_ia->Completed += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdCompleted);
    m_ia->Cancelled += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdCancelled);
    m_ia->ErrorOccurred += ref new
        Windows::Foundation::EventHandler<Microsoft::Advertising::WinRT::UI::AdErrorEventArgs ^>
            (this, &Simple3DGameXaml::DirectXPage::OnAdError);

    // Pre-fetch an ad 30-60 seconds before you need it.
    m_ia->RequestAd(AdType::Video, IA_APPID, IA_ADUNITID);
    ```

6.  Achten Sie darauf, dass die Anzeige an der Stelle im Code, an der sie angezeigt werden soll, bereit ist, und zeigen Sie die Anzeige an.

    ``` syntax
    if ((InterstitialAdState::Ready == m_ia->State))
    {
        m_ia->Show();
    }
    ```

7.  Definieren und codieren Sie die Ereignisse.

    ``` syntax
    void DirectXPage::OnAdReady(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdCompleted(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdCancelled(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdError
      (Object^ sender, Microsoft::Advertising::WinRT::UI::AdErrorEventArgs^ args)
    {
      // code
    }
    ```

8.  Weisen Sie die `AppId`-Eigenschaft dem Testwert zu, der in [Testmoduswerte](test-mode-values.md) angegeben wird. Dieser Wert wird nur zu Testzwecken verwendet. Sie ersetzen ihn vor der Veröffentlichung Ihrer App mit einem Livewert.

    ``` syntax
    static Platform::String^ IA_APPID = L"d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

9.  Weisen Sie die `AdUnitId`-Eigenschaft dem Testwert zu, der in [Testmoduswerte](test-mode-values.md) angegeben wird. Dieser Wert wird nur zu Testzwecken verwendet. Sie ersetzen ihn vor der Veröffentlichung Ihrer App mit einem Livewert.

    ``` syntax
    static Platform::String^ IA_ADUNITID = L"11389925";
    ```

10. Erstellen und testen Sie Ihre App, um zu überprüfen, ob sie Testanzeigen anzeigt.

### Veröffentlichen von Apps mit Liveanzeigen mithilfe von Windows Dev Center

1.  Rufen Sie für Ihre App im Dev Center-Dashboard die Seite **Monetisierung**&gt;**Gewinnbringende Nutzung mit Anzeigen** auf, und [erstellen Sie eine eigenständige Microsoft Advertising-Einheit](../publish/monetize-with-ads.md). Geben Sie für den Anzeigeneinheitentyp **Video-Interstitialanzeige** an. Notieren Sie sich die Anzeigeeinheits-ID und die Anwendungs-ID.

2.  Ersetzen Sie in Ihrem Code die Testanzeigen-Einheitenwerte mit den Livewerten, die Sie in Dev Center generiert haben.

3.  [Übermitteln Sie Ihre App](../publish/app-submissions.md) mithilfe des Windows Dev Center-Dashboards an den Store.

4.  Überprüfen Sie die [Anzeigenvermittlungsberichte](../publish/advertising-performance-report.md) im Dev Center-Dashboard.

<span id="interstitialbestpractices10"/>
## Bewährte Methoden für Interstitialanzeigen


Weitere Informationen zur effektiven Verwendung von Interstitialanzeigen finden Sie unter [Richtlinien für Benutzeroberfläche und Benutzerumgebung](ui-and-user-experience-guidelines.md).

<span id="targetplatform10"/>
## Entfernen von Verweisfehlern: Ausrichtung auf eine bestimmte CPU-Plattform (XAML und HTML)


Wenn Sie die Microsoft Advertising-Bibliotheken verwenden, können Sie in Ihrem Projekt als Ziel nicht **Any CPU** angeben. Sollte in Ihrem Projekt die Zielplattform **Any CPU** definiert sein, wird in Ihrem Projekt eine Warnung ausgegeben, sobald Sie einen Verweis auf die Microsoft Advertising-Bibliotheken hinzufügen. Um diese Warnung zu entfernen, müssen Sie eine architekturspezifische Buildausgabe verwenden (beispielsweise **x86**) und das Projekt entsprechend aktualisieren. Weitere Informationen finden Sie unter [Bekannte Probleme](known-issues-for-the-advertising-libraries.md).

## Verwandte Themen


* [Beispielcode für interstitielle Anzeigen in C##](interstitial-ad-sample-code-in-c.md)
* [Beispielcode für interstitielle Anzeigen in JavaScript](interstitial-ad-sample-code-in-javascript.md)
* [Anzeigenbeispiele auf GitHub](http://aka.ms/githubads)

 

 



<!--HONumber=Jun16_HO4-->


