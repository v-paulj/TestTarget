---
author: mcleanbyron
ms.assetid: 9621641A-7462-425D-84CC-101877A738DA
description: Lesen Sie, wie Sie Ihre UWP-Apps von AdMediatorControl auf AdControl umstellen.
title: Migrieren Ihrer UWP-Apps von AdMediatorControl zu AdControl
translationtype: Human Translation
ms.sourcegitcommit: 07baa54990ec31dc0cb9c289f9f0222754da9d7c
ms.openlocfilehash: 3abef943530cc756de117edccc5ab16e5f178604

---

# Migrieren Ihrer UWP-Apps von AdMediatorControl zu AdControl

In den Vorversionen des Advertising SDK von Microsoft konnten UWP- (Universelle Windows-Plattform-)Apps mit der Klasse **AdMediatorControl** Werbebanner anzeigen. Damit konnten Entwickler ihren Anzeigenumsatz optimieren, indem Sie Anzeigen von Werbebannern aus unseren Partnernetzwerken (AOL und AppNexus) sowie von AdDuplex anzeigen. Das [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) unterstützt die Klasse **AdMediatorControl** nicht mehr. Wenn Sie eine App haben, die die Klasse **AdMediatorControl** aus einem früheren SDK verwendet, und Sie diese App in eine UWP-Anwendung umwandeln möchten, die das [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) verwendet, folgen Sie den Anweisungen in diesem Artikel, um Ihren Quellcode so zu ändern, dass die Klasse **AdControl** anstelle von **AdMediatorControl** verwendet. Optional können Sie Ihre App so konfigurieren, das Werbung von AdDuplex eingeblendet wird, wobei ein gewichteter oder Rangfolgeansatz verwendet wird.

>**Note**&nbsp;&nbsp;Die in diesem Artikel verwendeten Codebeispiele dienen nur zur Veranschaulichung. Sie müssen die Codebeispiele möglicherweise anpassen, damit sie in Ihrer App funktionieren.

## Voraussetzungen

* Eine UWP-App, die derzeit AdMediatorControl verwendet und in Windows Store veröffentlicht ist.
* Ein Entwicklungscomputer mit Visual Studio 2015 und installiertem [Microsoft Store Services SDK](http://aka.ms/store-em-sdk).
* Wenn Sie Werbung mit AdDuplex vermitteln möchten, muss auf dem Entwicklungscomputer auch das [AdDuplex-Windows 10-SDK](https://visualstudiogallery.msdn.microsoft.com/6930860a-e64b-4b46-9d72-62d7fddda077) installiert sein.

  >**Hinweis**&nbsp;&nbsp;Sie können den AdDuplex-SDK-Installer über den Link oben ausführen, Sie können jedoch stattdessen auch die AdDuplex-Bibliotheken für Ihr UWP-App-Projekt in Visual Studio 2015 installieren. Stellen Sie sicher, dass Ihr UWP-App-Projekt in Visual Studio 2015 geöffnet ist, klicken Sie dann auf **Projekt** > **NuGet-Pakete verwalten**, suchen Sie nach dem NuGet-Paket mit dem Namen **AdDuplexWin10**, und installieren Sie das Paket.

## Schritt 1: Abrufen Ihrer Anwendungs-IDs und Ihrer Anzeigeneinheits-IDs

Wenn Sie den Code so modifizieren, dass die Klasse **AdControl** verwendet wird, benötigen Sie Ihre Anwendungs-IDs und Anzeigeneinheits-IDs. Die beste Möglichkeit zum Abrufen der neuesten IDs ist, diese aus Ihrer Konfigurationsdatei für die Anzeigenvermittlung auszulesen.

1. Melden Sie sich am Windows Dev Center-Dashboard an, und klicken Sie auf der App, die derzeit noch **AdMediatorControl** verwendet.
2. Klicken Sie auf **Monetarisierung** und **Monetarisierung durch Anzeigen**.
3. Klicken Sie im Abschnitt **Windows-Anzeigenvermittlung** auf den Link **Vermittlungskonfiguration herunterladen**, und öffnen Sie die Datei „AdMediator.config“ in einem Texteditor wie Editor.
4. Suchen Sie in der Datei das Element ```<AdAdapterInfo>``` mit dem untergeordneten Element ```<Name>MicrosoftAdvertising</Name>```. Dieser Abschnitt enthält die Konfiguration für die kostenpflichtige Werbeanzeigen von Microsoft.
5. Suchen Sie in ```<AdAdapterInfo>``` die ```<Property>```-Elemente, die ```<Key>```-Elemente mit den Werten **WApplicationId** bzw. **WAdUnitId** enthalten. Im Beispiel unten sind die Werte der ```<Value>```-Elemente nur als Beispiel zu verstehen.

  ```xml
  <Metadata>
      <Property>
          <Key>WApplicationId</Key>
          <Value>364d4938-c0f5-4c3d-8aae-090206211dc9</Value>
      </Property>
      <Property>
          <Key>WAdUnitId</Key>
          <Value>301568</Value>
      </Property>
  </Metadata>
  ```
6. Kopieren Sie die beiden Werte in diesen ```<Value>```-Elementen zur späteren Verwendung. Diese Werte enthalten die Anwendungs-ID und die Anzeigeneinheits-ID für die nicht-mobile Anzeigeneinheit für kostenpflichtige Werbeanzeigen von Microsoft.
5. Suchen Sie, ebenfalls im Element ```<AdAdapterInfo>```, die ```<Property>```-Elemente, die ```<Key>```-Elemente mit den Werten **MApplicationId** bzw. **MAdUnitId** enthalten. Im Beispiel unten sind die Werte der ```<Value>```-Elemente nur als Beispiel zu verstehen.

  ```xml
  <Metadata>
      <Property>
          <Key>MApplicationId</Key>
          <Value>e2cf8388-7018-4a11-8ab0de90f2a7a401</Value>
      </Property>
      <Property>
          <Key>MAdUnitId</Key>
          <Value>301056</Value>
      </Property>
  </Metadata>
  ```
6. Kopieren Sie die beiden Werte in den ```<Value>```-Elementen zur späteren Verwendung. Diese Werte enthalten die Anwendungs-ID und die Anzeigeneinheits-ID für die mobile Anzeigeneinheit für kostenpflichtige Werbeanzeigen von Microsoft.
7. Wenn Sie [Eigenwerbung](../publish/about-house-ads.md) nutzen, suchen Sie das Element ```<AdAdapterInfo>``` mit dem untergeordneten Element ```<Name>MicrosoftAdvertisingHouse</Name>```. Suchen Sie in diesem Element nach ```<Key>```-Elementen mit den Werten **MAdUnitId** bzw. **WAdUnitId**, und speichern Sie die Werte der entsprechenden ```<Value>```-Elemente zur späteren Verwendung. Dies sind die mobile und die nicht-mobile Anzeigeneinheits-ID für Microsoft-Eigenwerbung.
8. Wenn Sie AdDuplex verwenden, suchen Sie das ```<AdAdapterInfo>```-Element mit dem untergeordneten Element ```<Name>AdDuplex</Name>```. Suchen Sie in diesem Element nach den ```<Key>```-Elementen mit den Werten **AppKey** bzw. **AdUnitId**, und speichern Sie die Werte der entsprechenden ```<Value>```-Elemente zur späteren Verwendung. Dies sind Ihr AdDuplex-App-Schlüssel und die Anzeigeneinheits-ID.

## Schritt 2: Aktualisieren des App-Codes

Nun, da Sie Ihre Anwendungs-IDs und die Anzeigeneinheits-IDs haben, sind Sie bereit, den in Ihrer App verwenden Code so zu modifizieren, dass **AdControl** anstelle von **AdMediatorControl** verwendet wird.

### Nur kostenpflichtige Werbeanzeigen von Microsoft

Wenn Sie in Ihrer Konfiguration der Anzeigenvermittlung nur kostenpflichtige Werbeanzeigen von Microsoft verwenden, gehen Sie wie folgt vor:

  >**Hinweis**&nbsp;&nbsp;Bei den folgenden Anweisungen wird davon ausgegangen, dass die App-Seite, auf der Werbeanzeigen angezeigt werden sollen, ein leeres Raster mit dem Namen **myAdGrid** enthält. Beispiel: ```<Grid x:Name="myAdGrid"/>```. In den folgenden Schritten erstellen und konfigurieren Sie die Steuerelemente für die Werbeanzeigen im Quellcode, und nicht in XAML.

1. Öffnen Sie das UWP-App-Projekt in Visual Studio.
2.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen** aus.
Erweitern Sie im **Verweis-Manager** den Knoten **Universal Windows**, klicken Sie auf **Erweiterungen**, und wählen Sie dann das Kontrollkästchen neben **Microsoft Advertising SDK für XAML** (Version 10.0).
3. Klicken Sie im **Verweis-Manager** auf „OK“.
4. Entfernen Sie die **AdMediatorControl**-Deklaration aus dem XAML-Code, und entfernen Sie weiter jeden Code, der dieses **AdMediatorControl**-Objekt verwendet, einschließlich der zugehörigen Ereignishandler.
5. Öffnen Sie die Codedatei für die App-**Seite**, auf der die Werbeanzeigen angezeigt werden sollen.
6. Fügen Sie die folgende Anweisung am Anfang der Codedatei ein, falls sie nicht bereits vorhanden ist.

  ```csharp
  using Microsoft.Advertising.WinRT.UI;
  using Windows.System.Profile;
  ```
7. Fügen Sie Ihrer **Page**-Klasse die folgenden Konstantendeklarationen hinzu.

  ```csharp
  private const int AD_WIDTH = <tbd>;
  private const int AD_HEIGHT = <tbd>;
  private const string WAPPLICATIONID = "<tbd>";
  private const string WADUNITID = "<tbd>";
  private const string MAPPLICATIONID = "<tbd>";
  private const string MADUNITID = "<tbd>";
  ```
7. Ersetzen Sie bei diesen Konstantendeklarationen die ```<tbd>```-Werte wie folgt:
  * **AD_WIDTH** und **AD_HEIGHT**: Weisen Sie diesen Konstanten eine der [unterstützten Anzeigengrößen für Werbebanner]( https://msdn.microsoft.com/windows/uwp/monetize/supported-ad-sizes-for-banner-ads) zu.
  * **WAPPLICATIONID** und **WADUNITID**: Weisen Sie diesen Konstanten die **WApplicationId**- bzw. **WAdUnitId**-Werte für kostenpflichtige Werbeanzeigen von Microsoft zu, die Sie zuvor aus der Konfigurationsdatei für die Anzeigenvermittlung ausgelesen haben (dies sind die Werte für die nicht-mobile Anzeigeneinheit für kostenpflichtige Werbeanzeigen).
  * **MAPPLICATIONID** und **MADUNITID**: Weisen Sie diesen Konstanten die **MApplicationId**- bzw. **MAdUnitId**-Werte für kostenpflichtige Werbeanzeigen von Microsoft zu, die Sie zuvor aus der Konfigurationsdatei für die Anzeigenvermittlung ausgelesen haben (dies sind die Werte für die mobile Anzeigeneinheit für kostenpflichtige Werbeanzeigen).

8. Fügen Sie Ihrer **Page**-Klasse die folgenden Variablendeklarationen hinzu.

  ```csharp
  // Declare an AdControl.
  private AdControl myAdControl = null;

  // Application ID and ad unit ID values for Microsoft advertising. By default,
  // assign these to non-mobile ad unit info.
  private string myAppId = WAPPLICATIONID;
  private string myAdUnitId = WADUNITID;
  ```
5. Fügen Sie in den Konstruktor der Klasse **Page** nach dem Aufruf der Methode **InitializeComponent()** folgenden Code ein.

  ```csharp
  myAdGrid.Width = AD_WIDTH;
  myAdGrid.Height = AD_HEIGHT;

  // For mobile device families, use the mobile ad unit info.
  if ("Windows.Mobile" == AnalyticsInfo.VersionInfo.DeviceFamily)
  {
      myAppId = MAPPLICATIONID;
      myAdUnitId = MADUNITID;
  }

  // Initialize the AdControl.
  myAdControl = new AdControl();
  myAdControl.ApplicationId = myAppId;
  myAdControl.AdUnitId = myAdUnitId;
  myAdControl.Width = AD_WIDTH;
  myAdControl.Height = AD_HEIGHT;
  myAdControl.IsAutoRefreshEnabled = true;

  myAdGrid.Children.Add(myAdControl);
  ```  

### Kostenpflichtige Werbeanzeigen von Microsoft, Eigenwerbung und AdDuplex

Wenn Sie Microsoft Eigenwerbung oder AdDuplex sowie kostenpflichtige Werbeanzeigen von Microsoft verwenden, aber weiterhin Werbung mit AdDuplex vermitteln möchten, folgen Sie den Anweisungen in diesem Abschnitt. Die Codebeispiele unterstützen sowohl AdDuplex als auch Microsoft-Eigenwerbung. Falls Sie AdDuplex, aber keine Microsoft-Eigenwerbung verwenden oder umgekehrt, entfernen Sie den Code, der für Ihr Szenario nicht zutrifft.

  >**Hinweis**&nbsp;&nbsp;Bei den folgenden Anweisungen wird davon ausgegangen, dass die App-Seite, auf der Werbeanzeigen angezeigt werden sollen, ein leeres Raster mit dem Namen **myAdGrid** enthält. Beispiel: ```<Grid x:Name="myAdGrid"/>```. In den folgenden Schritten erstellen und konfigurieren Sie die Steuerelemente für die Werbeanzeigen im Quellcode, und nicht in XAML.

1. Öffnen Sie das UWP-App-Projekt in Visual Studio.
2.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen** aus.
Erweitern Sie im **Verweis-Manager** den Knoten **Universal Windows**, klicken Sie auf **Erweiterungen**, und wählen Sie dann das Kontrollkästchen neben **Microsoft Advertising SDK für XAML** (Version 10.0).
3. Klicken Sie im **Verweis-Manager** auf „OK“.
4. Entfernen Sie die **AdMediatorControl**-Deklaration aus dem XAML-Code, und entfernen Sie weiter jeden Code, der dieses **AdMediatorControl**-Objekt verwendet, einschließlich der zugehörigen Ereignishandler.
5. Öffnen Sie die Codedatei für die App-**Seite**, auf der die Werbeanzeigen angezeigt werden sollen.
6. Fügen Sie die folgenden Anweisungen am Anfang der Codedatei ein, falls sie nicht bereits vorhanden sind.

  ```csharp
  using Windows.System.UserProfile;
  using Microsoft.Advertising.WinRT.UI;
  using Windows.System.Profile;
  ```
7. Fügen Sie Ihrer **Page**-Klasse die folgenden Konstantendeklarationen hinzu.

  ```csharp
  private const int AD_WIDTH = <tbd>;
  private const int AD_HEIGHT = <tbd>;
  private const int HOUSE_AD_WEIGHT = <tbd>;
  private const int AD_REFRESH_SECONDS = 35;
  private const int MAX_ERRORS_PER_REFRESH = 3;
  private const string WAPPLICATIONID = "<tbd>";
  private const string WADUNITID_PAID = "<tbd>";
  private const string WADUNITID_HOUSE = "<tbd>";
  private const string MAPPLICATIONID = "<tbd>";
  private const string MADUNITID_PAID = "<tbd>";
  private const string MADUNITID_HOUSE = "<tbd>";
  private const string ADDUPLEX_APPKEY = "<tbd>";
  private const string ADDUPLEX_ADUNIT = "<tbd>";
  ```
4. Ersetzen Sie bei den Konstantendeklarationen, denen ```<tbd>``` zugewiesen ist, ```<tbd>``` durch den richtigen Wert für Ihr Szenario.
  * **AD_WIDTH** und **AD_HEIGHT**: Weisen Sie diesen Konstanten eine der [unterstützten Anzeigengrößen für Werbebanner]( https://msdn.microsoft.com/windows/uwp/monetize/supported-ad-sizes-for-banner-ads) zu.
  * **HOUSE_AD_WEIGHT**: Weisen Sie dieser Konstante einen Integerwert zwischen 0 und 100 zu, der die Gewichtung für Microsoft-Eigenwerbung im Verhältnis zu den kostenpflichtigen Werbeanzeigen von Microsoft angibt (bei 0 wird keine Eigenwerbung angezeigt, bei 100 ausschließlich Eigenwerbung).
  * **WAPPLICATIONID** und **WADUNITID_PAID**: Weisen Sie diesen Konstanten die **WApplicationId**- bzw. **WAdUnitId**-Werte für kostenpflichtige Werbeanzeigen von Microsoft zu, die Sie zuvor aus der Konfigurationsdatei für die Anzeigenvermittlung ausgelesen haben (dies sind die Werte für die nicht-mobile Anzeigeneinheit für kostenpflichtige Werbeanzeigen).
  * **WADUNITID_HOUSE**: Weisen Sie dieser Konstante den **WAdUnitId**-Wert für Eigenwerbung zu, die Sie zuvor aus der Konfigurationsdatei für die Anzeigenvermittlung ausgelesen haben (dieser Wert ist für die nicht-mobile Anzeigeneinheit für Eigenwerbung).
  * **MAPPLICATIONID** und **MADUNITID_PAID**: Weisen Sie diesen Konstanten die **MApplicationId**- bzw. **MAdUnitId**-Werte für kostenpflichtige Werbeanzeigen von Microsoft zu, die Sie zuvor aus der Konfigurationsdatei für die Anzeigenvermittlung ausgelesen haben (dies sind die Werte für die mobile Anzeigeneinheit für kostenpflichtige Werbeanzeigen).
  * **MADUNITID_HOUSE**: Weisen Sie dieser Konstante den **MAdUnitId**-Wert für Eigenwerbung zu, die Sie zuvor aus der Konfigurationsdatei für die Anzeigenvermittlung ausgelesen haben (dieser Wert ist für die mobile Anzeigeneinheit für Eigenwerbung).
  * **ADDUPLEX_APPKEY** and **ADDUPLEX_ADUNIT**: Weisen Sie diesen Konstante den AdDuplex-App-Schlüssel bzw. den Wert der Anzeigeneinheit zu, die Sie zuvor aus der Konfigurationsdatei für die Anzeigenvermittlung ausgelesen haben.
5. Fügen Sie Ihrer **Page**-Klasse die folgenden Variablendeklarationen hinzu.
  ```csharp
  // Dispatch timer to fire at each ad refresh interval.
  private DispatcherTimer myAdRefreshTimer = new DispatcherTimer();

  // Global variables used for mediation decisions.
  private Random randomGenerator = new Random();
  private int errorCountCurrentRefresh = 0;  // Prevents infinite redirects.
  private int adDuplexWeight = 0;            // Will be set by GetAdDuplexWeight().

  // Microsoft and AdDuplex controls for banner ads.
  private AdControl myMicrosoftBanner = null;
  private AdDuplex.AdControl myAdDuplexBanner = null;

  // Application ID and ad unit ID values for Microsoft advertising. By default,
  // assign these to non-mobile ad unit info.
  private string myMicrosoftAppId = WAPPLICATIONID;
  private string myMicrosoftPaidUnitId = WADUNITID_PAID;
  private string myMicrosoftHouseUnitId = WADUNITID_HOUSE;
  ```
5. Fügen Sie in den Konstruktor der Klasse **Page** nach dem Aufruf der Methode **InitializeComponent()** folgenden Code ein.
  ```csharp
  myAdGrid.Width = AD_WIDTH;
  myAdGrid.Height = AD_HEIGHT;
  adDuplexWeight = GetAdDuplexWeight();
  RefreshBanner();

  // Start the timer to refresh the banner at the desired interval.
  myAdRefreshTimer.Interval = new TimeSpan(0, 0, AD_REFRESH_SECONDS);
  myAdRefreshTimer.Tick += myAdRefreshTimer_Tick;
  myAdRefreshTimer.Start();

  // For mobile device families, use the mobile ad unit info.
  if ("Windows.Mobile" == AnalyticsInfo.VersionInfo.DeviceFamily)
  {
      myMicrosoftAppId = MAPPLICATIONID;
      myMicrosoftPaidUnitId = MADUNITID_PAID;
      myMicrosoftHouseUnitId = MADUNITID_HOUSE;
  }
  ```
6. Erweitern Sie die Klasse **Page** um die folgenden Methoden. Diese Methoden instanziieren das Microsoft-**AdControl**- und das AdDuplex-**AdControl**-Objekt. Die Methoden verwenden einen Zufallszahlengenerator sowie die angegebenen Gewichtungswerte, um Werbebanner in diesen Steuerelemente in regelmäßigen, von einem Zeitgeber vorgegebenen Intervallen zu aktualisieren.
  ```csharp
  private int GetAdDuplexWeight()
  {
      // TODO: Change this logic to fit your needs.
      // This example uses Microsoft ads first in Canada and Mexico, then
      // AdDuplex as fallback. In France, AdDuplex is first. In other regions,
      // this example uses a weighted average approach, with 50% to AdDuplex.

      int returnValue = 0;
      switch (GlobalizationPreferences.HomeGeographicRegion)
      {
          case "CA":
          case "MX":
              returnValue = 0;
              break;
          case "FR":
              returnValue = 100;
              break;
          default:
              returnValue = 50;
              break;
      }
      return returnValue;
  }

  private void ActivateMicrosoftBanner()
  {
      // Return if you hit the error limit for this refresh interval.
      if (errorCountCurrentRefresh >= MAX_ERRORS_PER_REFRESH)
      {
          myAdGrid.Visibility = Visibility.Collapsed;
          return;
      }

      // Use random number generator and house ads weight to determine whether
      // to use paid ads or house ads. Paid is the default. You could alternatively
      // write a method similar to GetAdDuplexWeight and override by region.
      string myAdUnit = myMicrosoftPaidUnitId;
      int houseWeight = HOUSE_AD_WEIGHT;
      int randomInt = randomGenerator.Next(0, 100);
      if (randomInt < houseWeight)
      {
          myAdUnit = myMicrosoftHouseUnitId;
      }

      // Hide the AdDuplex control if it is showing.
      if (null != myAdDuplexBanner)
      {
          myAdDuplexBanner.Visibility = Visibility.Collapsed;
      }

      // Initialize or display the Microsoft control.
      if (null == myMicrosoftBanner)
      {
          myMicrosoftBanner = new AdControl();
          myMicrosoftBanner.ApplicationId = myMicrosoftAppId;
          myMicrosoftBanner.AdUnitId = myAdUnit;
          myMicrosoftBanner.Width = AD_WIDTH;
          myMicrosoftBanner.Height = AD_HEIGHT;
          myMicrosoftBanner.IsAutoRefreshEnabled = false;

          myMicrosoftBanner.AdRefreshed += myMicrosoftBanner_AdRefreshed;
          myMicrosoftBanner.ErrorOccurred += myMicrosoftBanner_ErrorOccurred;

          myAdGrid.Children.Add(myMicrosoftBanner);
      }
      else
      {
          myMicrosoftBanner.AdUnitId = myAdUnit;
          myMicrosoftBanner.Visibility = Visibility.Visible;
          myMicrosoftBanner.Refresh();
      }
  }

  private void ActivateAdDuplexBanner()
  {
      // Return if you hit the error limit for this refresh interval.
      if (errorCountCurrentRefresh >= MAX_ERRORS_PER_REFRESH)
      {
          myAdGrid.Visibility = Visibility.Collapsed;
          return;
      }

      // Hide the Microsoft control if it is showing.
      if (null != myMicrosoftBanner)
      {
          myMicrosoftBanner.Visibility = Visibility.Collapsed;
      }

      // Initialize or display the AdDuplex control.
      if (null == myAdDuplexBanner)
      {
          myAdDuplexBanner = new AdDuplex.AdControl();
          myAdDuplexBanner.AppKey = ADDUPLEX_APPKEY;
          myAdDuplexBanner.AdUnitId = ADDUPLEX_ADUNIT;
          myAdDuplexBanner.Width = AD_WIDTH;
          myAdDuplexBanner.Height = AD_HEIGHT;
          myAdDuplexBanner.RefreshInterval = AD_REFRESH_SECONDS;

          myAdDuplexBanner.AdLoaded += myAdDuplexBanner_AdLoaded;
          myAdDuplexBanner.AdCovered += myAdDuplexBanner_AdCovered;
          myAdDuplexBanner.AdLoadingError += myAdDuplexBanner_AdLoadingError;
          myAdDuplexBanner.NoAd += myAdDuplexBanner_NoAd;

          myAdGrid.Children.Add(myAdDuplexBanner);
      }
      myAdDuplexBanner.Visibility = Visibility.Visible;
  }

  private void myAdRefreshTimer_Tick(object sender, object e)
  {
      RefreshBanner();
  }

  private void RefreshBanner()
  {
      // Reset the error counter for this refresh interval and
      // make sure the ad grid is visible.
      errorCountCurrentRefresh = 0;
      myAdGrid.Visibility = Visibility.Visible;

      // Display ad from AdDuplex.
      if (100 == adDuplexWeight)
      {
          ActivateAdDuplexBanner();
      }
      // Display Microsoft ad.
      else if (0 == adDuplexWeight)
      {
          ActivateMicrosoftBanner();
      }
      // Use weighted approach.
      else
      {
          int randomInt = randomGenerator.Next(0, 100);
          if (randomInt < adDuplexWeight) ActivateAdDuplexBanner();
          else ActivateMicrosoftBanner();
      }
  }

  private void myMicrosoftBanner_AdRefreshed(object sender, RoutedEventArgs e)
  {
      // Add your code here as necessary.
  }

  private void myMicrosoftBanner_ErrorOccurred(object sender, AdErrorEventArgs e)
  {
      errorCountCurrentRefresh++;
      ActivateAdDuplexBanner();
  }

  private void myAdDuplexBanner_AdLoaded(object sender, AdDuplex.Banners.Models.BannerAdLoadedEventArgs e)
  {
      // Add your code here as necessary.
  }

  private void myAdDuplexBanner_NoAd(object sender, AdDuplex.Common.Models.NoAdEventArgs e)
  {
      errorCountCurrentRefresh++;
      ActivateMicrosoftBanner();
  }

  private void myAdDuplexBanner_AdLoadingError(object sender, AdDuplex.Common.Models.AdLoadingErrorEventArgs e)
  {
      errorCountCurrentRefresh++;
      ActivateMicrosoftBanner();
  }

  private void myAdDuplexBanner_AdCovered(object sender,   AdDuplex.Banners.Core.AdCoveredEventArgs e)
  {
      errorCountCurrentRefresh++;
      ActivateMicrosoftBanner();
  }
  ```



<!--HONumber=Sep16_HO1-->


