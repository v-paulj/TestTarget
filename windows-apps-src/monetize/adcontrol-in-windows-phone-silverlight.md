---
author: mcleanbyron
ms.assetid: c0450f7b-5c81-4d8c-92ef-2b1190d18af7
description: "Hier erfahren Sie, wie Sie die AdControl-Klasse nutzen können, um Werbebanner in einer Silverlight-App für Windows Phone 8.1 oder Windows Phone 8.0 anzuzeigen."
title: AdControl in Windows Phone Silverlight
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 5a12badfb11cfd43c0833522d996da7df73b3d55


---

# AdControl in Windows Phone Silverlight


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie die [AdControl](https://msdn.microsoft.com/library/windows/apps/hh524191.aspx)-Klasse nutzen können, um Werbebanner in einer Silverlight-App für Windows Phone 8.1 oder Windows Phone 8.0 anzuzeigen.

## Voraussetzungen

*  Installieren Sie das [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk) mit Visual Studio 2015 oder Visual Studio 2013.


## Advertising-Assembly-Verweise hinzufügen

Die Microsoft Advertising-Assemblys für Windows Phone Silverlight-Projekte sind bei der Microsoft Store Engagement and Monetization SDK nicht lokal installiert. Bevor Sie mit dem Aktualisieren des Codes beginnen können, müssen Sie zuerst **Verbundene Dienste** mit der Unterstützung der Anzeigenvermittlung in der Microsoft Store Engagement and Monetization SDK verwenden, um diese Assemblys herunterzuladen und in Ihrem Projekt darauf zu verweisen.

1.  Klicken Sie in Visual Studio auf **Projekt** und **Verbundenen Dienst hinzufügen**.

2.  Klicken Sie im Dialogfeld **Verbundenen Dienst hinzufügen** auf **Anzeigenvermittlung** und anschließend auf **Konfigurieren**.

3.  Klicken Sie auf **Werbenetzwerke auswählen** und wählen Sie nur **Microsoft Advertising** aus.

    An diesem Punkt werden alle erforderlichen Microsoft Advertising-Assemblys für Silverlight für Ihr lokales Projekt als NuGet-Pakete heruntergeladen. Verweise auf diese Assemblys werden dem Projekt automatisch hinzugefügt. Außerdem wird Ihrem Projekt ein Verweis auf die Anzeigenvermittlungs-Assembly hinzugefügt. Den Verweis auf die Anzeigenvermittlungs-Assembly werden Sie in einem späteren Schritt entfernen, da er für dieses Szenario nicht erforderlich ist.

4.  Klicken Sie im Dialogfeld **Werbenetzwerke auswählen** auf **OK**. Klicken Sie auf der folgenden Bestätigungsseite **Abrufen des Status** auf **OK** und schließlich auf **Hinzufügen**. Daraufhin wird das Dialogfeld **Ad Mediator** geschlossen.

5.  Erweitern Sie im **Projektmappen-Explorer** den Knoten **Verweise**. Klicken Sie mit der rechten Maustaste auf **Microsoft.AdMediator.WindowsPhone81SL.MicrosoftAdvertising** und dann auf **Entfernen**. Dieser Assemblyverweis ist nicht erforderlich für dieses Szenario.

## Ihr App-Code


1.  Füge Sie in Ihrer Datei „WMAppManifest.xml“ die folgenden Funktionen zum Knoten **Funktionen** hinzu.

    ``` syntax
    <Capability Name="ID_CAP_IDENTITY_USER"/>
    <Capability Name="ID_CAP_MEDIALIB_PHOTO"/>
    <Capability Name="ID_CAP_PHONEDIALER"/>
    ```

    In diesem Beispiel sieht Ihr Knoten **Funktionen** wie folgt aus:

    ``` syntax
        <Capabilities>
          <Capability Name="ID_CAP_NETWORKING"/>
          <Capability Name="ID_CAP_MEDIALIB_AUDIO"/>
          <Capability Name="ID_CAP_MEDIALIB_PLAYBACK"/>
          <Capability Name="ID_CAP_SENSORS"/>
          <Capability Name="ID_CAP_WEBBROWSERCOMPONENT"/>
          <Capability Name="ID_CAP_IDENTITY_USER"/>
          <Capability Name="ID_CAP_MEDIALIB_PHOTO"/>
          <Capability Name="ID_CAP_PHONEDIALER"/>
        </Capabilities>
    ```

2.  Optional: Speichern Sie Ihr Projekt. Klicken Sie auf das Symbol **Alles speichern** oder klicken Sie im Menü **Datei** auf **Alles speichern**.

3.  Fügen Sie die Funktion **Internet (Client und Server)**zur Package.appxmanifest-Datei in Ihrem Projekt hinzu. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei **appxmanifest**.

    ![wp81silverlightmarkup\-Solutionexplorer\-packageappxmanifest](images/13-b98c2a1a-69c3-4018-be0a-6ce010e703e7.jpg)

    Klicken Sie im **Editor** auf die Registerkarte **Funktionen**. Überprüfen Sie das Feld **Internet (Client und Server)**.

4.  Optional: Speichern Sie Ihr Projekt. Klicken Sie auf das Symbol **Alles speichern** oder klicken Sie im Menü **Datei** auf **Alles speichern**.

5.  Ändern Sie das Silverlight-Markup in der Datei „MainPage.xaml“, sodass es den **Microsoft.Advertising.Mobile.UI**- Namespace enthält.

    ``` syntax
    xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
    ```

    Die Kopfzeile Ihrer Seite enthält den folgenden Code:

    ``` syntax
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
        x:Class="PhoneApp1.MainPage"
    ```

6.  Fügen Sie im **Raster**-Tag den folgenden Code für die **AdControl** ein. Weisen Sie den Eigenschaften **ApplicationId** und **AdUnitId** die unter [Testmoduswerte](test-mode-values.md) bereitgestellten Testwerte zu und passen Sie die Eigenschaften **Höhe** und **Breite** auf eines der [unterstützten Anzeigengrößen für Werbebanner](supported-ad-sizes-for-banner-ads.md) an.

    > **Hinweis**  
    Ersetzen Sie die Testwerte **ApplicationId** und **AdUnitId** mit Live-Werten, bevor Sie Ihre App übermitteln.

    ``` syntax
    <Grid x:Name="ContentPanel" Grid.Row="1">

      <UI:AdControl
             ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
             AdUnitId="10865270"
             HorizontalAlignment="Left"
             Height="80"
             VerticalAlignment="Top"
             Width="480"/>

    </Grid>
    ```

7.  Erstellen und Ausführen des Projekts Stellen Sie sicher, dass Ihre App eine Anzeige zeigt, die in etwa wie folgt aussieht:

    ![wp81silverlight\-emulatorwithad](images/13-8db1492f-ae1d-439b-9b78-bed8e22fe996.jpg)

## Veröffentlichen der App mit Live-Werbung mithilfe von Dev Center


1.  Rufen Sie für Ihre App im Dev Center-Dashboard die Seite **Monetisierung**&gt;**Gewinnbringende Nutzung mit Anzeigen** auf und [erstellen Sie eine eigenständige Microsoft Advertising-Einheit](../publish/monetize-with-ads.md). Geben Sie als Einheitentyp **Banner** an. Notieren Sie sich die Anzeigeeinheits-ID und die Anwendungs-ID.

2.  Ersetzen Sie in Ihrem Code die Testwerte der Anzeigenheit (**applicationId** und **adUnitId**) mit den Live-Werten, die Sie in Dev Center generiert haben.

3.  [Übermitteln Sie Ihre App](../publish/app-submissions.md) mithilfe des Dev Center-Dashboards an den Store.

4.  Überprüfen Sie die [Anzeigenvermittlungsberichte](../publish/advertising-performance-report.md) auf dem Dev Center-Dashboard.


 



<!--HONumber=Jun16_HO4-->


