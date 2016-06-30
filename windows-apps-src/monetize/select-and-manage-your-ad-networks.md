---
author: mcleanbyron
ms.assetid: 86D9D3CF-8FDC-4B67-881B-DF33A1BEE8BF
description: "Vor dem Verwenden der Anzeigenvermittlung müssen Sie Konten bei jedem Anzeigennetzwerk einrichten, das Sie in Ihren Apps verwenden möchten."
title: "Auswählen und Verwalten der Anzeigennetzwerke"
translationtype: Human Translation
ms.sourcegitcommit: ec7ce299545de8e5c167e1934fb9a0b4f4370948
ms.openlocfilehash: 49c9b8e60da9239c948265fb22563013019da259

---

# Auswählen und Verwalten der Anzeigennetzwerke


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Vor dem Verwenden der Anzeigenvermittlung müssen Sie Konten bei jedem Anzeigennetzwerk einrichten, das Sie in Ihren Apps verwenden möchten. Wir empfehlen, diese einzurichten, bevor Sie Ihrem Microsoft Visual Studio-Projekt das Ad Mediator-Steuerelement hinzufügen. Für maximale Flexibilität beim Optimieren der Leistung der Anzeigenvermittlung empfehlen wir außerdem, Konten bei möglichst vielen Anzeigennetzwerken einzurichten.

## Unterstützte Anzeigennetzwerke und Projekttypen

Die Anzeigenvermittlung wird derzeit für die folgenden Anzeigennetzwerke und Projekttypen unterstützt.

|  Anzeigennetzwerk    | UWP-App (Universelle Windows-Plattform) unter Verwendung von C# oder Visual Basic mit XAML | Windows 8.1-App unter Verwendung von C# oder Visual Basic mit XAML | Windows Phone 8.1-App unter Verwendung von C# oder Visual Basic mit XAML | Windows Phone 8.x Silverlight-App |               
|-------------------------------------------------------------------------|----------------------------------------------------|----------------------------------------------------------|-----------------------------------|---------------|
| [Microsoft](#microsoft)                         | **Unterstützt**                                      | **Unterstützt**                                            | **Unterstützt**                     | **Unterstützt** |
| [AdDuplex](#adduplex)                                                   | **Unterstützt**                                      | **Unterstützt**                                            | **Unterstützt**                     | **Unterstützt** |
| [Smaato](#smaato)                                                       | Nicht unterstützt                                      | **Unterstützt**                                            | **Unterstützt**                     | **Unterstützt** |
| [AdMob (Google)](#admob)                                                | Nicht unterstützt                                      | Nicht unterstützt                                            | Nicht unterstützt                     | **Unterstützt** |
| [Inneractive](#inneractive)                                             | Nicht unterstützt                                      | Nicht unterstützt                                            | Nicht unterstützt                     | **Unterstützt** |
| [Vserv VMAX](#vserv)                                                    | Nicht unterstützt                                      | Nicht unterstützt                                            | **Unterstützt**                     | Nicht unterstützt | 

Einige Projekttypen, z. B. C++ mit XAML und JavaScript mit HTML, werden von der Anzeigenvermittlung derzeit nicht unterstützt. Für diese Projekttypen können Sie Anzeigen von Microsoft anzeigen, ohne die Anzeigenvermittlung zu verwenden. Weitere Informationen finden Sie unter [AdControl in XAML und .NET](https://msdn.microsoft.com/library/mt313186.aspx) und [AdControl in HTML 5 und JavaScript](https://msdn.microsoft.com/library/mt313130.aspx).

## Bestimmte Parameter und Details für die einzelnen Netzwerke


Hier finden Sie alle erforderlichen Infos für die einzelnen Anzeigennetzwerke, einschließlich Infos zum Einrichten Ihres Kontos und Integrieren einer App. Zudem nennen wir die erforderlichen Parameter, die Sie beim [Übermitteln Ihrer App und Konfigurieren der Anzeigenvermittlung](submit-your-app-and-configure-ad-mediation.md) bereitstellen müssen (und/oder für Verbundene Dienste zum [Testen der Implementierung der Anzeigenvermittlung](test-your-ad-mediation-implementation.md)). Weitere Infos zum Verwenden eines bestimmten Anzeigennetzwerks finden Sie auf dessen Website.

Neben den erforderlichen Parametern verfügt jedes Anzeigennetzwerk auch über zusätzliche optionale Parameter, die Sie über Code in Ihrer App festlegen können. Beispiele finden Sie unter [Optionale Parameter für Anzeigennetzwerke](#optionalparameters) weiter unten in diesem Thema. Die vollständige Liste der optionalen Parameter finden Sie in der Dokumentation zu den einzelnen Anzeigennetzwerken. Einige dieser Parameter werden durch die Anzeigenvermittlung ignoriert oder überschrieben. Sie werden in den folgenden Abschnitten aufgeführt. Beispielsweise werden Parameter zur aktuellen Position in der Regel mit Informationen über den von der Positionsfunktion in der App ermittelten Standort des Kunden überschrieben.

Hinweis: Wenn Sie das [Ad Mediator-Steuerelement hinzufügen](add-and-use-the-ad-mediator-control.md) und angeben, welche Anzeigennetzwerke verwendet werden sollen, versucht Visual Studio, die erforderlichen Assemblys für einige Anzeigennetzwerke programmgesteuert abzurufen. Können Assemblys nicht automatisch abgerufen werden, können Sie sie mithilfe der unten aufgeführten Links für die einzelnen Anzeigennetzwerke installieren.

### Microsoft

| Website                        | Verwenden Sie zum Konfigurieren von Parametern für Anzeigennetzwerke die Seite [Monetisierung durch Werbeanzeigen](https://msdn.microsoft.com/library/windows/apps/mt170658) im [Windows Dev Center-Dashboard](https://dev.windows.com/overview).   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SDK-Speicherort                   | [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk).                                                                                                                                                                                                                         |
| Integrieren einer App              | Fügen Sie das Steuerelement der Anzeigenvermittlung zu Ihrer App hinzu, und senden Sie Ihre App an das Windows Dev Center-Dashboard.                                                                                                                                                                                                            |
| Erforderliche Parameter            | ApplicationId und AdUnitId: Diese Parameter werden für Sie beim Einreichen Ihres App-Pakets auf Grundlage des Inhalts Ihrer App automatisch ausgefüllt. Sie können diese Parameter jedoch optional bearbeiten, wenn Sie [Ihre App einreichen und die Anzeigenvermittlung konfigurieren](submit-your-app-and-configure-ad-mediation.md). <br> <br> Height und Width (nur für Windows Phone 8 Silverlight und Windows Phone 8.1 Silverlight erforderlich).                                                                                                                                                                                                           |
| Überschriebene/ignorierte Parameter | Latitude (überschrieben)  <br><br> Longitude (überschrieben) <br><br> AutoRefreshIntervalInSeconds (ignoriert) <br><br> IsAutoRefreshEnabled (ignoriert) <br><br> IsAutoCollapsedEnabled (ignoriert) <br><br> IsEngaged (ignoriert) <br><br> IsSuspended (ignoriert) |

 

### AdDuplex

| Website                        | [http://adduplex.com](http://go.microsoft.com/fwlink/p/?LinkId=518028)      |
|--------------------------------|-----------------------------------------------------------------------------|
| SDK-Speicherort                   | Versuchen Sie zunächst, die Assemblys vom Ad Mediator-Steuerelement über Verbundene Dienste abrufen zu lassen, wie unter [Hinzufügen und Verwenden des Ad Mediator-Steuerelements](add-and-use-the-ad-mediator-control.md) beschrieben. Wenn Sie diese manuell herunterladen müssen, wechseln Sie zum Abschnitt [Getting Started](http://go.microsoft.com/fwlink/p/?LinkId=518029) (Erste Schritte) auf der AdDuplex-Website. |
| Integrieren einer App              | Erstellen Sie eine neue App im AdDuplex-Portal.                                                                                                                                                                                                                                                                                                        |
| Erforderliche Parameter            | AppKey <br><br> AdUnitId <br><br> Size (nur für Windows 8.1 XAML erforderlich)  |
| Überschriebene/ignorierte Parameter | Latitude (überschrieben) <br><br> Longitude (überschrieben) <br><br> AutoRefreshIntervalInSeconds (ignoriert) <br><br> IsAutoRefreshEnabled (ignoriert) <br><br> IsAutoCollapsedEnabled (ignoriert) <br><br> IsEngaged (ignoriert) <br><br> IsSuspended (ignoriert) |
 

### Smaato

| Website                        | [http://www.smaato.com/](http://go.microsoft.com/fwlink/p/?LinkId=518030)                                                                                                                                                                                                        |
|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SDK-Speicherort                   | Versuchen Sie zunächst, die Assemblys vom Ad Mediator-Steuerelement über Verbundene Dienste abrufen zu lassen, wie unter [Hinzufügen und Verwenden des Ad Mediator-Steuerelements](add-and-use-the-ad-mediator-control.md) beschrieben. Wenn Sie diese manuell herunterladen müssen, rufen Sie die Registerkarte Downloads im Smaato-Portal auf. |
| Integrieren einer App              | Rufen Sie im Smaato-Portal My Spaces (Meine Adspaces) auf, und generieren Sie einen neuen Adspace.                                                                                                                                                                                                                |
| Erforderliche Parameter            | Pub <br> <br> Adspace <br> <br> Height und Width (nur für Windows 8.1 XAML erforderlich)  |
| Überschriebene/ignorierte Parameter | Gps (überschrieben)                                                                                                                                                                                                                                                                |

 

### AdMob (Google)

| Website                        | [http://apps.admob.com](http://go.microsoft.com/fwlink/p/?LinkId=518031)                                               |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------|
| SDK-Speicherort                   | Rufen Sie das Windows Phone 8 SDK von der [Google Mobile Ads SDK-Website](http://go.microsoft.com/fwlink/p/?LinkId=518032) ab. |
| Integrieren einer App              | Klicken Sie im AdMob-Portal auf Neue App monetarisieren.                                                                          |
| Erforderliche Parameter            | AdUnitID <br> <br> Format                                                                                              |
| Überschriebene/ignorierte Parameter | Location (überschrieben)  <br><br> ForceTesting (ignoriert) <br><br> Refresh Rate (ignoriert)                                |
 

### Inneractive

| Website             | [http://inner-active.com](http://go.microsoft.com/fwlink/p/?LinkId=518035)                                                                                                                                                                                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SDK-Speicherort        | Versuchen Sie zunächst, die Assemblys vom Ad Mediator-Steuerelement über Verbundene Dienste abrufen zu lassen, wie unter [Hinzufügen und Verwenden des Ad Mediator-Steuerelements](add-and-use-the-ad-mediator-control.md) beschrieben. Wenn Sie diese manuell herunterladen müssen, melden Sie sich bei Ihrem Konto an, und wechseln Sie im Dashboard zur Registerkarte „SDK“, um das Windows Phone 8 SDK herunterzuladen. |
| Integrieren einer App   | Erstellen Sie eine neue App im Inneractive-Portal.                                                                                                                                                                                                                                                                                           |
| Erforderliche Parameter | AppID <br> <br> AdType (IaAdType_Banner oder IaAdType_Text)                                                                               |
 

### Vserv VMAX

| Website             | [http://www.vmax.com](http://www.vmax.com)                                                                                                                                                                                                                                                                         |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SDK-Speicherort        | Versuchen Sie zunächst, die Assemblys vom Ad Mediator-Steuerelement über „Verbundene Dienste“ abrufen zu lassen, wie unter [Hinzufügen und Verwenden des Ad Mediator-Steuerelements](add-and-use-the-ad-mediator-control.md) beschrieben. Wenn Sie diese manuell herunterladen müssen, wechseln Sie zum Abschnitt [SDK](http://go.microsoft.com/fwlink/?LinkId=627078) auf der VMAX-Website. |
| Integrieren einer App   | Ermitteln Sie die Adspot-ID für Ihre App auf der VMAX-Website (die Adspot-ID wird in VMAX-APIs „ZoneId“ genannt).                                                                                                                                                                                                                     |
| Erforderliche Parameter | ZoneId                                                                                                                                                                                                                                                                                                                         |12345

 

## Optionale Parameter für Anzeigennetzwerke


Neben den erforderlichen Parametern verfügt jedes Anzeigennetzwerk auch über zusätzliche optionale Parameter, die Sie über Code in Ihrer App festlegen können. Die vollständige Liste der optionalen Parameter finden Sie in der Dokumentation zu den einzelnen Anzeigennetzwerken. Verwenden Sie zum Festlegen dieser optionalen Parameter im Code die **AdSdkOptionalParameters**-Eigenschaft Ihres **AdMediatorControl**-Objekts.

Das folgende Beispiel veranschaulicht das Festlegen der [CountryOrRegion](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.countryorregion.aspx)-Eigenschaft für Microsoft Advertising auf den aus zwei Buchstaben bestehenden Code für das Land oder die Region des Benutzers.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["CountryOrRegion"] = "IN";
```

Das folgende Beispiel veranschaulicht, wie der **Width**-Parameter und **Height**-Parameter für Smaato festgelegt werden.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 300;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 250;
```

## Verwandte Themen

* [Hinzufügen und Verwenden des Steuerelements für die Anzeigenvermittlung](add-and-use-the-ad-mediator-control.md)
* [Testen der Implementierung der Anzeigenvermittlung](test-your-ad-mediation-implementation.md)
* [Übermitteln der App und Konfigurieren der Anzeigenvermittlung](submit-your-app-and-configure-ad-mediation.md)
* [Problembehandlung bei der Anzeigenvermittlung](troubleshoot-ad-mediation.md)
 

 



<!--HONumber=Jun16_HO4-->


