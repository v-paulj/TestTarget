---
ms.assetid: 3C03FDD8-FA61-4E7B-BDCA-3C29DFEA20E4
description: Befolgen Sie nach der Installation des Microsoft Store Engagement and Monetization SDK die Anweisungen in diesem Thema, um das Ad Mediator-Steuerelement in Ihrer App zu verwenden.
title: Hinzufügen und Verwenden des Ad Mediator-Steuerelements
---

# Hinzufügen und Verwenden des Ad Mediator-Steuerelements


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Befolgen Sie nach der [Installation des Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk) die Anweisungen in diesem Thema, um das Ad Mediator-Steuerelement in Ihrer App zu verwenden. Eine Liste der Anzeigennetzwerke und Projekttypen, die derzeit von der Anzeigenvermittlung unterstützt werden, finden Sie unter [Auswählen und Verwalten der Anzeigennetzwerke](select-and-manage-your-ad-networks.md).

## Hinzufügen des Ad Mediator-Steuerelements zum Projekt


So fügen Sie Ihrem Projekt eine Instanz des Ad Mediator-Steuerelements hinzu:

1.  Öffnen Sie das Projekt in Visual Studio.
2.  Wenn Sie die Anzeigenvermittlung einer App hinzufügen, die Sie bereits mit einem der von der Anzeigenvermittlung unterstützten [Anzeigennetzwerke](select-and-manage-your-ad-networks.md) vermarkten, entfernen Sie die vorhandene Anzeigenimplementierung und alle zugehörigen Verweise, bevor Sie den Vorgang fortsetzen.
3.  Suchen Sie im **Projektmappen-Explorer** die Seite Ihrer App, auf der Anzeigen dargestellt werden sollen, und doppelklicken Sie dann auf die Seite, um sie im Designer zu öffnen.
4.  Ziehen Sie aus der **Toolbox** ein neues **AdMediatorControl**-Steuerelement in den Designer (achten Sie darauf, das Steuerelement in den Designer und nicht in den XAML-Code zu ziehen). Positionieren Sie das Steuerelement an der Stelle, an Sie Ihre Anzeigen anzeigen möchten. Sie können mehrere Steuerelemente hinzufügen, wenn Sie Anzeigen in mehreren Bereichen Ihrer App anzeigen möchten.

    Das **AdMediatorControl**-Element befindet sich an folgenden **Toolbox**-Speicherorten:

    -   Verwenden Sie in einem UWP (Universelle Windows-Plattform)-Projekt das **AdMediatorControl**-Element unter dem Abschnitt **AdMediator Universal**.
    -   In einem Windows 8.1- oder Windows Phone 8.1-Projekt mit C# oder Visual Basic und XAML verwenden Sie das **AdMediatorControl**-Element unter dem Abschnitt **AdMediator**.
    -   In einem Windows Phone Silverlight-Projekt verwenden Sie das **AdMediatorControl**-Element unter dem Abschnitt **Alle Windows Phone-Steuerelemente**.

    **Hinweis**  Beim ersten Ziehen des **AdMediatorControl**-Steuerelement in den Designer in einem UWP-, Windows 8.1- oder Windows Phone 8.1-Projekt mit C# oder Visual Basic und XAML fügt Visual Studio Ihrem Projekt den erforderlichen Ad Mediator-Assemblyverweis hinzu, das Steuerelement wird dem Designer jedoch noch nicht hinzugefügt. Klicken Sie zum Hinzufügen des Steuerelements in der von Visual Studio angezeigten Meldung auf „OK“, warten dann einige Sekunden auf die Aktualisierung des Designers und ziehen das Steuerelement anschließend erneut in den Designer. Wenn Sie dem Designer das Steuerelement weiterhin nicht hinzufügen können, stellen Sie sicher, dass Ihr Projekt die geeignete Prozessorarchitektur für Ihre App aufweist (z. B. **X 86**) statt einer **beliebigen CPU**. Das Steuerelement kann dem Designer nicht hinzugefügt werden, wenn das Projekt für die Buildplattform auf eine **beliebige CPU** ausgerichtet ist.

5.  Visual Studio fügt einen Assemblyverweis für Ad Mediator zu Ihrem Projekt hinzu und fügt XAML für das Ad Mediator-Steuerelement auf der aktuellen Seite ein, u. a. eine eindeutige ID und einen Namen für das Steuerelement. Der Assemblyverweis und die XAML variieren je nach Zielplattform. Bei einer UWP (Universelle Windows-Plattform)-App lautet der Name der Assembly z. B. **Microsoft.AdMediator.Universal**, und die generierte XAML ähnelt dem folgenden Beispiel.

    ```xml
    // Code that gets added to the XAML page header
        xmlns:Universal="using:Microsoft.AdMediator.Universal"

        // Code that gets added for the ad mediator control
        <Universal:AdMediatorControl x:Name="AdMediator_3D4884"
         Grid.ColumnSpan="2" HorizontalAlignment="Left" Height="250"
         Id="AdMediator-Id-D1FDFDA7-EABB-474C-940C-ECA7FBCFF143" Margin="121,175,0,0"
         VerticalAlignment="Top" Width="300"/>
    ```

    Das **Name**-Element vereinfacht die Identifizierung des spezifischen Steuerelements in Ihrer App beim Konfigurieren der Anzeigenvermittlung. Sie können dieses beliebig ändern. Achten Sie jedoch darauf, das **Id**-Element nicht zu ändern oder zu duplizieren. Diese **Id** muss für jedes Steuerelement in Ihrer App eindeutig sein.

6.  Passen Sie die Größe und Position des Steuerelements nach Bedarf an. Weitere Informationen finden Sie unter [Anpassen der Größe und Position](#adjust-size-and-position).

## Konfigurieren von Anzeigennetzwerken

Nachdem Sie die gewünschten Steuerelemente hinzugefügt haben, können Sie die Anzeigennetzwerke über „Verbundene Dienste“ konfigurieren.

**Wichtig**  Wenn Sie später ein zusätzliches AdMediatorControl-Element hinzufügen, müssen Sie es erneut über „Verbundene Dienste“ konfigurieren. Andernfalls kann das neue Steuerelement die Anzeigenvermittlung nicht verwenden.

So konfigurieren Sie die Anzeigennetzwerke

1.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Namen des Projekts, klicken Sie auf **Hinzufügen** und anschließend auf **Verbundener Dienst…**, um das Fenster **Verbundenen Dienst hinzufügen** (Visual Studio 2015) oder **Dienst-Manager** (Visual Studio 2013) zu öffnen.
2.  Wenn Sie Visual Studio 2015 verwenden, klicken Sie auf **Ad Mediator** und dann auf **Konfigurieren**, um das Fenster **Ad Mediator** zu öffnen. Wenn Sie Visual Studio 2013 verwenden, klicken Sie einfach auf **Ad Mediator** (im linken Bereich des **Dienste-Managers**).

    Die Datei „AdMediator.config“ wird dem Projekt hinzugefügt. In dieser Datei werden die anfänglichen Konfigurationseinstellungen für das Anzeigennetzwerk lokal in Ihrem Projekt gespeichert.

3.  Klicken Sie im Fenster **Ad Mediator** (Visual Studio 2015) oder **Dienste-Manager** (Visual Studio 2013) auf **Anzeigennetzwerke auswählen**, wählen Sie dann die zu verwendenden Anzeigennetzwerke aus, und klicken Sie im Fenster **Anzeigennetzwerke auswählen** auf **OK**.

    **Tipp**  Fügen Sie am besten alle Netzwerke hinzu, bei denen Sie angemeldet sind, auch wenn Sie nicht alle sofort in Ihrer App verwenden möchten. Nachdem die App veröffentlicht wurde, können Sie konfigurieren, wie oft jedes Netzwerk in Dev Center verwendet wird (oder ein Netzwerk festlegen, das Sie zuvor noch nicht genutzt haben), ohne Codeänderungen vorzunehmen und die App erneut einzureichen.

    Visual Studio ruft die erforderlichen Assemblys für die ausgewählten Anzeigennetzwerke ab und fügt diese Assemblyverweise zum Projekt hinzu. Nachdem dieser Vorgang abgeschlossen ist, klicken Sie im Dialogfenster zum **Abrufen des Status** auf **OK**.

4.  Wählen Sie im Fenster **Ad Mediator** (Visual Studio 2015) oder **Dienste-Manager** (Visual Studio 2013) optional die einzelnen Netzwerke aus, und klicken Sie auf **Konfigurieren**, um die Konfigurationsinformationen für die einzelnen Netzwerke einzugeben, die beim Testen der App verwendet werden. Diese Informationen werden in der Datei „AdMediator.config“ in Ihrem Projekt gespeichert. Sie können diese Informationen ändern, wenn Sie im Windows Dev Center-Dashboard das Verhalten des Anzeigennetzwerks konfigurieren. Weitere Informationen finden Sie unter [Übermitteln der App und Konfigurieren der Anzeigenvermittlung](submit-your-app-and-configure-ad-mediation.md).
    **Hinweis**  Wenn Sie in diesem Schritt keine Konfigurationsinformationen eingeben, verwendet die Anzeigenvermittlung automatisch Testkonfigurationswerte, wenn Sie Ihre App auf dem Entwicklungscomputer (für UWP-Apps und XAML-Apps für Windows 8.1) oder im Emulator bzw. auf einem Gerät (für Windows Phone-Apps) ausführen.

5.  Überprüfen Sie im Fenster **Ad Mediator** (Visual Studio 2015) oder **Dienste-Manager** (Visual Studio 2013), ob für jedes von Ihnen ausgewählte Anzeigennetzwerk **Abgerufen** angezeigt wird. Klicken Sie auf **OK**, um die Änderungen an Ihr Projekt zu übermitteln.

**Hinweis** Wenn Sie später auf eine neuere Version des Microsoft Store Engagement and Monetization SDK upgraden, müssen Sie **Verbundene Dienste** neu starten, damit automatisch abgerufene DLL-Dateien des Anzeigennetzwerks ordnungsgemäß aktualisiert werden.

### Deklarieren der erforderlichen Funktionen

Jedes Anzeigennetzwerk kann bestimmte App-Funktionen benötigen. Diese werden vom jeweiligen Anbieter im Fenster **Ad Mediator** (für Visual Studio 2015) oder **Dienst-Manager** (für Visual Studio 2013) angezeigt. Achten Sie darauf, alle erforderlichen Funktionen im App-Manifest zu deklarieren, damit die Anzeigen ordnungsgemäß angezeigt werden.

Der folgende Screenshot zeigt die erforderlichen Funktionen für verschiedene Anzeigennetzwerke in einer XAML-App für Windows 8.1 oder Windows Phone 8.1.

![Dienst-Manager zeigt an, dass alle Verweise abgerufen wurden](images/ad-med-8.jpg)

Der folgende Screenshot zeigt die erforderlichen Funktionen für verschiedene Anzeigennetzwerke in einer Silverlight-App für Windows Phone 8.1.

![Dienst-Manager zeigt an, dass alle Verweise abgerufen wurden](images/ad-med-6.jpg)
### Manuelles Abrufen der DLL-Dateien von Anzeigennetzwerken

Unter Umständen sehen Sie, dass bestimmte DLL-Dateien nicht abgerufen wurden. In diesem Fall müssen Sie diese manuell hinzufügen. Links zum Herunterladen einzelner Assemblys finden Sie unter [Auswählen und Verwalten von Anzeigennetzwerken](select-and-manage-your-ad-networks.md).

**Hinweis**  Wenn Sie DLL-Dateien manuell hinzufügen, erhalten Sie möglicherweise die Fehlermeldung „Dem Projekt kann kein Verweis auf eine höhere Version oder inkompatible Assembly hinzugefügt werden." Klicken Sie zum Beheben dieses Fehlers mit der rechten Maustaste auf die DLL-Datei im Explorer, und wählen Sie dann **Eigenschaften**. Klicken Sie im Abschnitt „Sicherheit“ auf **Aufheben**.

![Schaltfläche „Aufheben“ zum Beheben der Fehlermeldung](images/ad-med-4.png)
## Anpassen der Größe und Position

Sie können die Größe und die Position des Ad Mediator-Steuerelements im Designer oder im XAML-Code konfigurieren. Achten Sie darauf, dass diese Größe für alle Anzeigen ausreicht, die Sie aus Ihren Anzeigennetzwerken einblenden möchten. Einige Werbenetzwerke schalten die Werbung ggf. nicht, wenn sie feststellen, dass die Canvas-Größe nicht für die Anzeige im Vollbildmodus ausreicht. Wenn Ihre Anzeigeneinheiten größer als die Standardgröße sind, können Sie die Canvasgröße an Ihre größte Anzeige anpassen.

Wenn Sie das Steuerelement in den Designer ziehen, werden folgende Standardsteuerelementgrößen angewendet:

-   UWP und Windows 8.1 XAML: 300 breit x 250 hoch.
-   Windows Phone 8.1 XAML: 400 breit x 67 hoch.
-   Windows Phone 8 und Windows Phone 8.1 Silverlight: 480 breit x 80 hoch.

Sie können die Standardanzeigengrößen mit den optionalen Parametern **Breite** und **Höhe** überschreiben, wie unten gezeigt.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 400;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 80;
```

Sie können auch angeben, wie jedes Steuerelement positioniert wird, um eine Vielzahl von Größen und Platzierungen je nach den Anforderungen Ihrer App zu berücksichtigen. Bei einigen Anzeigennetzwerken können Sie auch Anpassungen durch optionale Parameter vornehmen. Wenn Sie beispielsweise Anzeigen von Microsoft Advertising unten links ausrichten möchten:

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["HorizontalAlignment"] = HorizontalAlignment.Left;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["VerticalAlignment"] = VerticalAlignment.Bottom;
```

### Unterstützte Anzeigengrößen für Microsoft Advertising

Microsoft Advertising unterstützt nur Anzeigen in den folgenden Standardgrößen. Diese werden vom Interactive Advertising Bureau (IAB) für Apps empfohlen, die auf den Plattformen ausgeführt werden.

-   Windows 10 und Windows 8.1:
    -   160 x 600
    -   250 x 250 (Beachten Sie, dass nur sehr wenige Anzeigen in dieser Größe verfügbar sind. Sie sollten andere Größen verwenden, um die Füllrate und eCPM zu maximieren.)
    -   300 x 250
    -   300 x 600
    -   728 x 90
-   Windows 10 Mobile, Windows Phone 8.1 und Windows Phone 8:
    -   300 x 50
    -   320 x 50
    -   480 x 80
    -   640 x 100

Möglicherweise möchten Sie eine Ad Mediator-Steuerelementgröße angeben, die keiner der von Microsoft Advertising unterstützten Anzeigengrößen entspricht (etwa, wenn eine andere Größe besser zur Benutzeroberfläche Ihrer App passt oder wenn das Steuerelement auch in anderen Anzeigennetzwerken verwendet werden soll, die andere Anzeigengrößen unterstützen). Hierzu geben Sie die genaue gewünschte Steuerelementgröße im Designer oder im XAML-Code an und weisen dann die optionalen Parameter **Breite** und **Höhe** für Microsoft Advertising der unterstützten Größe zu, die am ehesten in die Grenzen des Steuerelements passt. Das Steuerelement wird im Designer mit der angegebenen genauen Größe angezeigt, aber die von Microsoft Advertising geschalteten Anzeigen entsprechen der angegebenen Größe mit den optionalen Parametern **Breite** und **Höhe**.

Beispiel: Wenn bei einer UWP-App das Ad Mediator-Steuerelement mit einer Größe von 300 x 300 angezeigt werden soll, legen Sie das Steuerelement im Designer oder im XAML-Code auf 300 x 300 fest. Weisen Sie anschließend den optionalen Parameter **Breite** mit 300 und den optionalen Parameter **Höhe** mit 250 für Microsoft Advertising zu, wie im folgenden Code dargestellt.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Width"] = 300;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Height"] = 250;
```

Um sicherzustellen, dass die Größeneinstellungen mit Microsoft Advertising kompatibel sind, [testen Sie die Implementierung der Anzeigenvermittlung](test-your-ad-mediation-implementation.md), und vergewissern Sie sich, dass Testanzeigen von Microsoft Advertising angezeigt werden.

## Anhalten, Fortsetzen und Deaktivieren der Anzeigenvermittlung

Wenden Sie die AdMediatorControl.Pause()-Methode an, wenn Sie die Anzeigenvermittlung während der Laufzeit der App für einen bestimmten Zeitraum anhalten möchten. Beachten Sie, dass dabei die aktuellste Werbung weiterhin angezeigt wird, bis Sie den Ad Mediator durch Aufruf der AdMediatorControl.Resume()-Methode erneut aufnehmen.

Wenden Sie die AdMediatorControl.Disable()-Methode an, um den Ad Mediator vollständig zu deaktivieren. Dadurch entfernen Sie sämtliche Werbung und reduzieren den Speicherbedarf des Ad Mediators. Rufen Sie AdMediatorControl.Resume() auf, um den Ad Mediator fortzusetzen. Beachten Sie jedoch, dass dieser nach der Deaktivierung langsamer startet.

## Festlegen von Timeouts

Sie können die Anzahl der Sekunden (von 2 bis 60) angeben, für die die Anzeigenvermittlung nach dem Anfordern einer Anzeige aus diesem Anzeigennetzwerk warten soll, bevor die Anfrage abgebrochen und eine Anfrage bei einem anderen Netzwerk gestellt wird. Standardmäßig beträgt das Timeout für alle Werbenetzwerke 15 Sekunden.

Der folgende Code veranschaulicht, wie eine Timeoutdauer für Microsoft Advertising festgelegt wird. Sie können die Dauer und Netzwerke bei Bedarf ändern.

```CSharp
myAdMediatorControl.AdSdkTimeouts[AdSdkNames.MicrosoftAdvertising] = TimeSpan.FromSeconds(10);
```

**Hinweis**  Alternativ können Sie den Timeoutwert auf der Seite **Gewinnbringende Nutzung mit Anzeigen** im Dev Center-Dashboard festlegen. Wenn Sie das Timeout im Code und im Dashboard festgelegt haben, überschreibt der im Code angegebene Wert den Dashboardwert.

## Ereignishandling

Das Hinzufügen von Code zum Protokollieren von Ereignissen und Aufzeichnen von Fehlern beim Ad Mediator können bei der Problembehandlung hilfreich sein. Im folgenden Codebeispiel werden Ereignishandler für bestimmte Ereignisse von einem Steuerelement hinzugefügt.

```CSharp
// add this during initialization of your app

    AdMediator_Bottom.AdSdkError += AdMediator_Bottom_AdError;
    AdMediator_Bottom.AdMediatorFilled += AdMediator_Bottom_AdFilled;
    AdMediator_Bottom.AdMediatorError += AdMediator_Bottom_AdMediatorError;
    AdMediator_Bottom.AdSdkEvent += AdMediator_Bottom_AdSdkEvent;

// and then add these functions

void AdMediator_Bottom_AdSdkEvent(object sender, Microsoft.AdMediator.Core.Events.AdSdkEventArgs e)
{
    Debug.WriteLine("AdSdk event {0} by {1}", e.EventName, e.Name);}

void AdMediator_Bottom_AdMediatorError(object sender, Microsoft.AdMediator.Core.Events.AdMediatorFailedEventArgs e)
{
    Debug.WriteLine("AdMediatorError:" + e.Error + " " + e.ErrorCode );
    // if (e.ErrorCode == AdMediatorErrorCode.NoAdAvailable)
    // AdMediator will not show an ad for this mediation cycle
}

void AdMediator_Bottom_AdFilled(object sender, Microsoft.AdMediator.Core.Events.AdSdkEventArgs e)
{
    Debug.WriteLine("AdFilled:" + e.Name);
}

void AdMediator_Bottom_AdError(object sender, Microsoft.AdMediator.Core.Events.AdFailedEventArgs e)
{
    Debug.WriteLine("AdSdkError by {0} ErrorCode: {1} ErrorDescription: {2} Error: {3}", e.Name, e.ErrorCode, e.ErrorDescription, e.Error);
}
```

## Behandeln von unbehandelten Ausnahmen von Anzeigennetzwerken

**Hinweis**  Beim Testen haben wir eine Reihe von unbehandelten Ausnahmen von bestimmten Anzeigennetzwerken identifiziert, die innerhalb der App gelöst werden müssen, um App-Abstürze zu vermeiden. Wir empfehlen dringend, das folgende Codebeispiel zu kopieren und in die Datei „App.Xaml.cs“ einzufügen.

Code für eine UWP-, Windows 8.1- oder Windows Phone-App mit C# und XAML

```CSharp
// In App.xaml.cs file, register with the UnhandledException event handler.
UnhandledException += App_UnhandledException;

void App_UnhandledException(object sender, UnhandledExceptionEventArgs e)
   {
      if (e != null)
      {
         Exception exception = e.Exception;
         if (exception is NullReferenceException &amp;&amp; exception.ToString().ToUpper().Contains("SOMA"))
         {
            Debug.WriteLine("Handled Smaato null reference exception {0}", exception);
            e.Handled = true;
            return;
         }
      }
// APP SPECIFIC HANDLING HERE

   if (Debugger.IsAttached)
      {
         // An unhandled exception has occurred; break into the debugger
         Debugger.Break();
      }
   }
```

Code für eine Windows Phone Silverlight-App

```CSharp
// In App.xaml.cs file, register with the UnhandledException event handler.
UnhandledException += Application_UnhandledException;

// Code to execute on unhandled exceptions
private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
{
    if (e != null)
   {
       Exception exception = e.ExceptionObject;
       if ((exception is XmlException || exception is NullReferenceException) &amp;&amp; exception.ToString().ToUpper().Contains("INNERACTIVE"))
       {
           Debug.WriteLine("Handled Inneractive exception {0}", exception);
           e.Handled = true;
           return;
       }
       else if (exception is NullReferenceException &amp;&amp; exception.ToString().ToUpper().Contains("SOMA"))
       {
           Debug.WriteLine("Handled Smaato null reference exception {0}", exception);
           e.Handled = true;
           return;
       }
       else if ((exception is System.IO.IOException || exception is NullReferenceException) &amp;&amp; exception.ToString().ToUpper().Contains("GOOGLE"))
      {
          Debug.WriteLine("Handled Google exception {0}", exception);
          e.Handled = true;
          return;
       }
       else if ((exception is NullReferenceException || exception is XamlParseException) &amp;&amp; exception.ToString().ToUpper().Contains("MICROSOFT.ADVERTISING"))
       {
           Debug.WriteLine("Handled Microsoft.Advertising exception {0}", exception);
           e.Handled = true;
           return;
       }

   }
// APP SPECIFIC HANDLING HERE

if (Debugger.IsAttached)
   {
       // An unhandled exception has occurred; break into the debugger
       Debugger.Break();
   }
   //e.Handled = true;
}
```

## Verwandte Themen

* [Auswählen und Verwalten der Anzeigennetzwerke](select-and-manage-your-ad-networks.md)
* [Testen der Implementierung der Anzeigenvermittlung](test-your-ad-mediation-implementation.md)
* [Übermitteln der App und Konfigurieren der Anzeigenvermittlung](submit-your-app-and-configure-ad-mediation.md)
* [Problembehandlung bei der Anzeigenvermittlung](troubleshoot-ad-mediation.md)
 

 


<!--HONumber=Mar16_HO5-->


