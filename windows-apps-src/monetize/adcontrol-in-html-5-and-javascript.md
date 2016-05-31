---
author: mcleanbyron
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: Hier erfahren Sie, wie Sie die AdControl-Klasse nutzen können, um Werbebanner in einer JavaScript/HTML-App für Windows 10 (UWP), Windows 8.1 oder Windows Phone 8.1 anzuzeigen.
title: AdControl in HTML 5 und Javascript
---

# AdControl in HTML 5 und Javascript


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie die [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)-Klasse nutzen können, um Werbebanner in einer JavaScript/HTML-App für Windows 10 (UWP), Windows 8.1 oder Windows Phone 8.1 anzuzeigen. In dieser exemplarischen Vorgehensweise wird die **AdMediatorControl** oder die Anzeigenvermittlung nicht verwendet.

Ein vollständiges Beispielprojekt, das veranschaulicht, wie Sie mithilfe von C# und C++ Werbebanner zu einer JavaScript/HTML-App hinzufügen, finden Sie unter den [Anzeigenbeispielen auf GitHub](http://aka.ms/githubads).

## Voraussetzungen


* Installieren Sie das [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk) mit Visual Studio 2015 oder Visual Studio 2013.

> **Hinweis**  Wenn Sie Windows 10 Anniversary SDK Preview Build 14295 oder später mit Visual Studio 2015 installiert haben, müssen Sie außerdem die WinJS-Bibliothek installieren. Diese Bibliothek war in den früheren Versionen von Windows SDK für Windows 10 enthalten, aber ab Windows 10 Anniversary SDK Preview Build 14295 muss diese Bibliothek separat installiert werden. Informationen zur Installation von WinJS finden Sie unter [WinJS herunterladen](http://try.buildwinjs.com/download/GetWinJS/).

## Codeentwicklung

1. Öffnen Sie in Visual Studio Ihr Projekt oder erstellen Sie ein neues Projekt.

2. Sollte in Ihrem Projekt die Zielplattform **ANYCPU** definiert sein, müssen Sie eine architekturspezifische Buildausgabe verwenden (z. B. **X86**) und das Projekt entsprechend aktualisieren. Sollte in Ihrem Projekt die Zielplattform **ANYCPU** definiert sein, können Sie bei den folgenden Schritten keinen Verweis auf die Microsoft Advertising-Bibliotheken hinzufügen. Weitere Informationen finden Sie unter [durch „Any CPU“ verursachte Verweisfehler im Projekt](known-issues-for-the-advertising-libraries.md#reference_errors).

3.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Verweise** und wählen Sie **Verweis hinzufügen...** aus.

4.  Wählen Sie im **Verweis-Manager** je nach Projekttyp einen der folgenden Verweise aus:

    -   Für ein Projekt für die Universelle Windows-Plattform (UWP): Erweitern Sie **Universelles Windows**, klicken Sie auf **Erweiterungen** und aktivieren Sie dann das Kontrollkästchen neben **Microsoft Advertising SDK für JavaScript** (Version 10.0).

    -   Für ein Windows 8.1-Projekt: Erweitern Sie **Windows 8.1**, klicken Sie auf **Erweiterungen** und aktivieren Sie dann das Kontrollkästchen neben **Microsoft Advertising SDK für Windows 8.1 Systemeigen (JS)**.

    -   Für ein Windows 8.1-Projekt: Erweitern Sie **Windows 8.1**, klicken Sie auf **Erweiterungen** und aktivieren Sie dann das Kontrollkästchen neben **Microsoft Advertising SDK for Windows Phone 8.1 Systemeigen (JS)**.

    ![javascriptaddreference](images/13-f7f6d6a6-161e-4f17-995d-1236d0b5d9f2.png)

    > **Hinweis**  Diese Abbildung bezieht sich auf die Erstellung eines UWP-Projekts für Windows 10 mit Visual Studio 2015. Wenn Sie eine Windows 8.1- oder Windows Phone 8.1-App erstellen oder Visual Studio 2013 verwenden, sieht Ihr Bildschirm anders aus.

5.  Klicken Sie im **Verweis-Manager** auf OK.

6.  Öffnen Sie die Datei „default.HTML“ (oder je nach Projekt eine andere html-Datei).

7.  Fügen Sie im Abschnitt **&lt;Head&gt;** nach den JavaScript-Verweisen auf default.css und default.js des Projekts den Verweis auf ad.js ein.

    Fügen Sie im UWP-Projekt den folgenden Code hinzu.

    ``` syntax
    <!-- Microsoft advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    Fügen Sie bei einem Windows 8.1- oder Windows Phone 8.1-Projekt den folgenden Code hinzu.

    ``` syntax
    <!-- Microsoft advertising required references -->
    <script src="/MSAdvertisingJS/ads/ad.js"></script>
    ```

    > **Hinweis**  Diese Zeile muss im Bereich **&lt;Head&gt;** nach der default.js platziert werden. Andernfalls tritt ein Fehler auf, wenn Sie Ihr Projekt erstellen.

8.  Ändern Sie den Bereich **&lt;Body&gt;** in der Datei „default.HTML“ (oder je nach Projekt einer anderen html-Datei), sodass sie das DIV-Element für die **AdControl** enthält. Weisen Sie den Eigenschaften **ApplicationId** und **AdUnitId** Eigenschaften in der **AdControl** die unter [Testmoduswerte](test-mode-values.md) bereitgestellten Testwerte zu. Passen Sie Höhe und Breite des Steuerelements an, damit es einer der [unterstützten Anzeigengrößen für Werbebanner](supported-ad-sizes-for-banner-ads.md) entspricht.

    > **Hinweis**  
    Ersetzen Sie die Testwerte **applicationId** und **adUnitId** mit Live-Werten, bevor Sie Ihre App übermitteln.

    ``` syntax
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
          data-win-control="MicrosoftNSJS.Advertising.AdControl"
          data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270'}">
    </div>
    ```

9.  Kompilieren und Ausführen der App, um sie mit einer Anzeige zu sehen

## Veröffentlichen der App mit Live-Werbung mithilfe von Windows Dev Center


1.  Rufen Sie für Ihre App im Dev Center-Dashboard die Seite **Monetisierung**&gt;**Gewinnbringende Nutzung mit Anzeigen** auf und [erstellen Sie eine eigenständige Microsoft Advertising-Einheit](../publish/monetize-with-ads.md). Geben Sie als Einheitentyp **Banner** an. Notieren Sie sich die Anzeigeeinheits-ID und die Anwendungs-ID.

2.  Ersetzen Sie in Ihrem Code die Testwerte der Anzeigenheit (**applicationId** und **adUnitId**) mit den Live-Werten, die Sie in Dev Center generiert haben.

3.  [Übermitteln Sie Ihre App](../publish/app-submissions.md) mithilfe des Dev Center-Dashboards an den Store.

4.  Überprüfen Sie die [Anzeigenvermittlungsberichte](../publish/advertising-performance-report.md) auf dem Dev Center-Dashboard.

## Vollständige Default.html für ein Beispielprojekt für UWP


``` syntax
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>My_Windows_10_Ad_Funded_JavaScript_App</title>

    <!-- WinJS references -->
    <link href="//Microsoft.WinJS.2.0.Preview/css/ui-dark.css" rel="stylesheet" />
    <script src="//Microsoft.WinJS.2.0.Preview/js/base.js"></script>
    <script src="//Microsoft.WinJS.2.0.Preview/js/ui.js"></script>

    <!-- My_Windows_10_Ad_Funded_JavaScript_App references -->
    <link href="/css/default.css" rel="stylesheet" />
    <script src="/js/default.js"></script>

    <!-- Microsoft advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
</head>
<body>
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270'}">
    </div>
    <p>Content goes here</p>
</body>
</html>
```

## Verwandte Themen

* [Anzeigenbeispiele bei GitHub](http://aka.ms/githubads)
 

 


<!--HONumber=May16_HO2-->


