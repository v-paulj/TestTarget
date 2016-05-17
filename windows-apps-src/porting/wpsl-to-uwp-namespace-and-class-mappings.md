---
author: mcleblanc
description: Dieses Thema enthält eine umfassende Zuordnung der Windows Phone Silverlight-APIs zu ihren Entsprechungen in der universellen Windows-Plattform (UWP).
title: Windows Phone Silverlight zu UWP – Namespace- und Klassenzuordnungen
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
---

# Windows Phone Silverlight zu UWP – Namespace- und Klassenzuordnungen

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Dieses Thema enthält eine umfassende Zuordnung der Windows Phone Silverlight-APIs zu den UWP (Universelle Windows-Plattform)-Entsprechungen. Im Allgemeinen erfolgt keine 1: 1-Zuordnung von Funktionen, jedoch gilt: Jede Plattform kann ggf. mehr oder weniger Funktionalität bieten als ihr Gegenstück in einem Namespace oder einer Klasse.

Die Zuordnungstabelle ist hilfreich, wenn Sie in einem UWP-Projekt arbeiten und Quellcode aus einem Windows Phone Silverlight-Projekt erneut verwenden. Zwischen den beiden Plattformen gibt es Unterschiede bei den Namen von Namespaces und Klassen (einschließlich UI-Steuerelemente). In vielen Fällen ist es einfach: Sie ändern z. B. einen Namespacenamen, und der Code wird kompiliert Manchmal wird neben dem Namespacenamen auch der Name einer Klasse oder API geändert In anderen Fällen ist die Zuordnung etwas schwieriger. In seltenen Fällen muss der Ansatz geändert werden.

**So verwenden Sie die Tabelle:** Suchen Sie zunächst nach dem Namen der Klasse, die Sie verwenden. Klassen werden aufgelistet, wenn es sich um eine kompliziertere Zuordnung als eine Änderung des Namespacenamens handelt. Wenn Ihre Klasse nicht aufgeführt ist, handelt es sich bei der Zuordnung nur um eine Namespaceänderung. Wenn Sie den Namespacenamen Ihrer Klasse finden, finden Sie auch den entsprechenden Namen des UWP-Namespaces. Ihre Klasse ist in diesem Namespace enthalten. Wenn der Namespace nicht aufgeführt ist, hat sich dessen Name nicht geändert.

**Hinweis**  Windows 10 unterstützt einen deutlich größeren Teil von .NET Framework als Windows Phone Store-Apps. Windows 10 verfügt beispielsweise über mehrere System.ServiceModel.\*-Namespaces sowie über System.Net, System.Net.NetworkInformation und System.Net.Sockets.
Außerdem profitieren Sie in einer Windows 10-App von .NET Native. Dabei handelt es sich um eine fortschrittliche Kompilierungstechnologie, mit der MSIL-Code in Computercode für die systemeigene Ausführung konvertiert wird. .NET Native-Apps starten schneller, verbrauchen weniger Arbeitsspeicher und benötigen weniger Akkuenergie als ihre MSIL-Gegenstücke.

| Windows Phone Silverlight | Windows-Runtime |
|---------------------------|-----------------|
| Werbung | |
| **Microsoft.Advertising.Mobile.UI.AdControl**-Klasse | [AdControl](http://msdn.microsoft.com/library/advertising-windows-sdk-api-reference-adcontrol.aspx)-Klasse |
| Alarme, Erinnerungen und Hintergrund-Agents | |
| **Microsoft.Phone.BackgroundAgent**-Klasse | [
              **BackgroundTaskBuilder**
            ](https://msdn.microsoft.com/library/windows/apps/br224768)-Klasse |
| **Microsoft.Phone.Scheduler**-Namespace | [
              **Windows.ApplicationModel.Background**
            ](https://msdn.microsoft.com/library/windows/apps/br224847)-Namespace |
| **Microsoft.Phone.Scheduler.Alarm**-Klasse | [
              **BackgroundTaskBuilder**
            ](https://msdn.microsoft.com/library/windows/apps/br224768)- und [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642)-Klassen |
| **Microsoft.Phone.Scheduler.PeriodicTask**-, **ScheduledAction**-, **ScheduledActionService**-, **ScheduledTask**- und **ScheduledTaskAgent**-Klassen | [
              **BackgroundTaskBuilder**
            ](https://msdn.microsoft.com/library/windows/apps/br224768)-Klasse |
| **Microsoft.Phone.Scheduler.Reminder**-Klasse | [
              **BackgroundTaskBuilder**
            ](https://msdn.microsoft.com/library/windows/apps/br224768)- und [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642)-Klassen |
| **Microsoft.Phone.PictureDecoder**-Klasse | [
              **BitmapDecoder**
            ](https://msdn.microsoft.com/library/windows/apps/br226176)-Klasse |
| **Microsoft.Phone.BackgroundAudio**-Namespace | [
              **Windows.Media.Playback**
            ](https://msdn.microsoft.com/library/windows/apps/dn640562)-Namespace |
| **Microsoft.Phone.BackgroundTransfer**-Namespace | [
              **Windows.Networking.BackgroundTransfer**
            ](https://msdn.microsoft.com/library/windows/apps/br207242)-Namespace |
| App-Modell und -Umgebung | |
| **System.AppDomain**-Klasse | Keine direkte Entsprechung Siehe [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324), [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016), Klassen |
| **System.Environment**-Klasse | Keine direkte Entsprechung |
| **System.ComponentModel.Annotations**-Klasse  | Keine direkte Entsprechung |
| **System.ComponentModel.BackgroundWorker**-Klasse | [
              **ThreadPool**
            ](https://msdn.microsoft.com/library/windows/apps/br229621)-Klasse |
| **System.ComponentModel.DesignerProperties**-Klasse | [
              **DesignMode**
            ](https://msdn.microsoft.com/library/windows/apps/br224664)-Klasse |
| **System.Threading.Thread**-, **System.Threading.ThreadPool**-Klassen | [
              **ThreadPool**
            ](https://msdn.microsoft.com/library/windows/apps/br229621)-Klasse |
| (ST = **System.Threading** <br/> **ST.Thread.MemoryBarrier**-Methode | (ST = **System.Threading** <br/> **ST.Interlocked.MemoryBarrier**-Methode |
| (ST = **System.Threading** <br/> **ST.Thread.ManagedThreadId**-Eigenschaft | (S = **System** <br/> **S.Environment.ManagedThreadId**-Eigenschaft |
| **System.Threading.Timer**-Klasse | [
              **ThreadPoolTimer**
            ](https://msdn.microsoft.com/library/windows/apps/br230587)-Klasse |
| (SWT = **System.Windows.Threading** <br/> **SWT.Dispatcher**-Klasse | [
              **CoreDispatcher**
            ](https://msdn.microsoft.com/library/windows/apps/br208211)-Klasse |
| (SWT = **System.Windows.Threading** <br/> **SWT.DispatcherTimer**-Klasse | [
              **DispatcherTimer**
            ](https://msdn.microsoft.com/library/windows/apps/br244250)-Klasse |
| Blend für Visual Studio | |
| (MEDC = **Microsoft.Expression.Drawing.Core** <br/> **MEDC.GeometryHelper**-Klasse | Keine direkte Entsprechung |
| **Microsoft.Expression.Interactivity**-Namespace | [Microsoft.Xaml.Interactivity](http://go.microsoft.com/fwlink/p/?LinkId=328776)-Namespace |
| **Microsoft.Expression.Interactivity.Core**-Namespace | [Microsoft.Xaml.Interactions.Core](http://go.microsoft.com/fwlink/p/?LinkId=328773)-Namespace |
| (MEIC = **Microsoft.Expression.Interactivity.Core** <br/> **MEIC.ExtendedVisualStateManager**-Klasse | Keine direkte Entsprechung |
| **Microsoft.Expression.Interactivity.Input**-Namespace | Keine direkte Entsprechung |
| **Microsoft.Expression.Interactivity.Media**-Namespace | [Microsoft.Xaml.Interactions.Media](http://go.microsoft.com/fwlink/p/?LinkId=328775)-Namespace |
| **Microsoft.Expression.Shapes**-Namespace | Keine direkte Entsprechung |
| (MI = **Microsoft.Internal** <br/> **MI.IManagedFrameworkInternalHelper**-Schnittstelle | Keine direkte Entsprechung |
| Kontakt- und Kalenderdaten | |
| **Microsoft.Phone.UserData**-Namespace | [
              **Windows.ApplicationModel.Contacts**
            ](https://msdn.microsoft.com/library/windows/apps/br225002)-, [**Windows.ApplicationModel.Appointments**](https://msdn.microsoft.com/library/windows/apps/dn263359)-Namespaces |
| (MPU = **Microsoft.Phone.UserData** <br/> **MPU.Account**-, **ContactAddress**-, **ContactCompanyInformation**-, **ContactEmailAddress**-, **ContactPhoneNumber**-Klassen | [
              **Contact**
            ](https://msdn.microsoft.com/library/windows/apps/br224849)-Klasse |
| (MPU = **Microsoft.Phone.UserData** <br/> **MPU.Appointments**-Klasse | [
              **AppointmentCalendar**
            ](https://msdn.microsoft.com/library/windows/apps/dn596134)-Klasse |
| (MPU = **Microsoft.Phone.UserData** <br/> **MPU.Contacts**-Klasse | [
              **ContactStore**
            ](https://msdn.microsoft.com/library/windows/apps/dn624859)-Klasse |
| Steuerelemente und UI-Infrastruktur | |
| **ControlTiltEffect.TiltEffect**-Klasse | Animationen aus der Windows-Runtime-Animationsbibliothek sind in die Standardstile der allgemeinen Steuerelemente integriert. Weitere Informationen finden Sie unter [Animation](wpsl-to-uwp-porting-xaml-and-ui.md#animation). |
| **Microsoft.Phone.Controls**-Namespace | [
              **Windows.UI.Xaml.Controls**
            ](https://msdn.microsoft.com/library/windows/apps/br227716)-Namespace |
| (MPC = **Microsoft.Phone.Controls** <br/> **MPC.ContextMenu**-Klasse | [
              **PopupMenu**
            ](https://msdn.microsoft.com/library/windows/apps/br208693)-Klasse |
| (MPC = **Microsoft.Phone.Controls** <br/>**MPC.DatePickerPage**-Klasse | [
              **DatePickerFlyout**
            ](https://msdn.microsoft.com/library/windows/apps/dn625013)-Klasse |
| (MPC = **Microsoft.Phone.Controls** <br/>**MPC.GestureListener**-Klasse | [
              **GestureRecognizer**
            ](https://msdn.microsoft.com/library/windows/apps/br241937)-Klasse |
| (MPC = **Microsoft.Phone.Controls** <br/>**MPC.LongListSelector**-Klasse | [
              **SemanticZoom**
            ](https://msdn.microsoft.com/library/windows/apps/hh702601)-Klasse |
| (MPC = **Microsoft.Phone.Controls** <br/>**MPC.ObscuredEventArgs**-Klasse | [
              **SystemProtection**
            ](https://msdn.microsoft.com/library/windows/apps/jj585394)-, [**WindowActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br208377)-Klassen | 
| (MPC = **Microsoft.Phone.Controls** <br/>**MPC.Panorama**-Klasse | [
              **Hub**
            ](https://msdn.microsoft.com/library/windows/apps/dn251843)-Klasse | 
| (MPC = **Microsoft.Phone.Controls** <br/>**MPC.PhoneApplicationFrame**-,<br/>(SWN = **System.Windows.Navigation** <br/>**SWN.NavigationService**-Klassen | [
              **Frame**
            ](https://msdn.microsoft.com/library/windows/apps/br242682)-Klasse |
| (MPC = **Microsoft.Phone.Controls** <br/>**MPC.PhoneApplicationPage**-Klasse | [
              **Page**
            ](https://msdn.microsoft.com/library/windows/apps/br227503)-Klasse|
| (MPC = **Microsoft.Phone.Controls** <br/>**MPC.TiltEffect**-Klasse | [
              **PointerDownThemeAnimation**
            ](https://msdn.microsoft.com/library/windows/apps/hh969164)-Klasse | 
| (MPC = **Microsoft.Phone.Controls** <br/>**MPC.TimePickerPage**-Klasse | [
              **TimePickerFlyout**
            ](https://msdn.microsoft.com/library/windows/apps/dn608313)-Klasse |
| (MPC = **Microsoft.Phone.Controls** <br/>**MPC.WebBrowser**-Klasse | [
              **WebView**
            ](https://msdn.microsoft.com/library/windows/apps/br227702)-Klasse | 
| (MPC = **Microsoft.Phone.Controls** <br/>**MPC.WebBrowserExtensions**-Klasse | Keine direkte Entsprechung | 
| (MPC = **Microsoft.Phone.Controls** <br/>**MPC.WrapPanel**-Klasse | Keine direkte Entsprechung für das allgemeine Layout. [
              **ItemsWrapGrid**
            ](https://msdn.microsoft.com/library/windows/apps/dn298849) und [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/br227717) können in der ItemsPanel-Vorlage eines Elementsteuerelements verwendet werden. | 
| (MPD = **Microsoft.Phone.Data** <br/>**MPD.Linq**-Namespace | Keine direkte Entsprechung | 
| (MPD = **Microsoft.Phone.Data** <br/>**MPD.Linq.Mapping**-Namespace | Keine direkte Entsprechung |
| **Microsoft.Phone.Globalization**-Namespace | Keine direkte Entsprechung | 
| (MPI = **Microsoft.Phone.Info** <br/>**MPI.DeviceExtendedProperties**-, **DeviceStatus**-Klassen | [
              **EasClientDeviceInformation**
            ](https://msdn.microsoft.com/library/windows/apps/hh701390)-, [**MemoryManager**](https://msdn.microsoft.com/library/windows/apps/dn633831)-Klassen. Weitere Informationen finden Sie unter [Gerätestatus](wpsl-to-uwp-input-and-sensors.md#device-status). | 
| (MPI = **Microsoft.Phone.Info** <br/>**MPI.MediaCapabilities**-Klasse | Keine direkte Entsprechung | 
| (MPI = **Microsoft.Phone.Info** <br/>**MPI.UserExtendedProperties**-Klasse | [
              **AdvertisingManager**
            ](https://msdn.microsoft.com/library/windows/apps/dn363391)-Klasse | 
| **System.Windows**-Namespace | [
              **Windows.UI.Xaml**
            ](https://msdn.microsoft.com/library/windows/apps/br209045)-Namespace | 
| **System.Windows.Automation**-Namespace | [
              **Windows.UI.Xaml.Automation**
            ](https://msdn.microsoft.com/library/windows/apps/br209179)-Namespace | 
| **System.Windows.Controls**-, **System.Windows.Input**-Namespaces | [
              **Windows.UI.Core**
            ](https://msdn.microsoft.com/library/windows/apps/br208383)-, [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)-, [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716)-Namespaces | 
| **System.Windows.Controls.DrawingSurface**-, **DrawingSurfaceBackgroundGrid**-Klassen | [
              **SwapChainPanel**
            ](https://msdn.microsoft.com/library/windows/apps/dn252834)-Klasse | 
| **System.Windows.Controls.RichTextBox**-Klasse | [
              **RichEditBox**
            ](https://msdn.microsoft.com/library/windows/apps/br227548)-Klasse | 
| **System.Windows.Controls.WrapPanel**-Klasse | Keine direkte Entsprechung für das allgemeine Layout. [
              **ItemsWrapGrid**
            ](https://msdn.microsoft.com/library/windows/apps/dn298849) und [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/br227717) können in der ItemsPanel-Vorlage eines Elementsteuerelements verwendet werden. | 
| **System.Windows.Controls.Primitives**-Namespace | [
              **Windows.UI.Xaml.Controls.Primitives**
            ](https://msdn.microsoft.com/library/windows/apps/br209818)-Namespace |
| **System.Windows.Controls.Shapes**-Namespace | [
              **Windows.UI.Xaml.Controls.Shapes**
            ](https://msdn.microsoft.com/library/windows/apps/br243401)-Namespace | 
| **System.Windows.Data**-Namespace | [
              **Windows.UI.Xaml.Data**
            ](https://msdn.microsoft.com/library/windows/apps/br209917)-Namespace | 
| **System.Windows.Documents**-Namespace | [
              **Windows.UI.Xaml.Documents**
            ](https://msdn.microsoft.com/library/windows/apps/br209984)-Namespace | 
| **System.Windows.Ink**-Namespace | Keine direkte Entsprechung |
| **System.Windows.Markup**-Namespace | [
              **Windows.UI.Xaml.Markup**
            ](https://msdn.microsoft.com/library/windows/apps/br228046)-Namespace | 
| **System.Windows.Navigation**-Namespace | [
              **Windows.UI.Xaml.Navigation**
            ](https://msdn.microsoft.com/library/windows/apps/br243300)-Namespace |
| **System.Windows.UIElement.Tap**-Ereignis, **EventHandler&lt;GestureEventArgs&gt;**-Delegat | [
              **Tapped**
            ](https://msdn.microsoft.com/library/windows/apps/br208985) Ereignis, [**TappedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br227993)-Delegat | 
| Daten und Dienste |  | 
| **System.Data.Linq.DataContext**-Klasse | Keine direkte Entsprechung | 
| **System.Data.Linq.Mapping.ColumnAttribute**-Klasse | Keine direkte Entsprechung | 
| **System.Data.Linq.SqlClient.SqlHelpers**-Klasse | Keine direkte Entsprechung | 
| Geräte | |Anwend
| **Microsoft.Devices**-, **Microsoft.Devices.Sensors**-Namespaces | [
              **Windows.Devices.Enumeration**
            ](https://msdn.microsoft.com/library/windows/apps/br225459)-, [**Windows.Devices.Enumeration.Pnp**](https://msdn.microsoft.com/library/windows/apps/br225517)-, [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)-, [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/br206408)-Namespaces |
| **Microsoft.Devices.Camera**-, **Microsoft.Devices.PhotoCamera**-Klassen | [
              **MediaCapture**
            ](https://msdn.microsoft.com/library/windows/apps/br241124)-Klasse. Auch [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030)-Klasse (nur Windows). |
| **Microsoft.Devices.CameraButtons**-Klasse | [
              **HardwareButtons**
            ](https://msdn.microsoft.com/library/windows/apps/jj207557)-Klasse |
| **Microsoft.Devices.CameraVideoBrushExtensions**-Klasse | [
              **CaptureElement**
            ](https://msdn.microsoft.com/library/windows/apps/br209278)-Klasse |
| **Microsoft.Devices.Environment**-Klasse | Keine direkte Entsprechung Um dieses Problem zu umgehen, verwenden Sie die bedingte Kompilierung und definieren Sie ein benutzerdefiniertes Symbol. Unter Umständen können Sie das Problem auch mit der [IsAttached](http://msdn.microsoft.com/library/e299w87h.aspx) Eigenschaft umgehen. |
| **Microsoft.Devices.MediaHistory**-Klasse | Keine direkte Entsprechung | 
| **Microsoft.Devices.VibrateController**-Klasse | [
              **VibrationDevice**
            ](https://msdn.microsoft.com/library/windows/apps/jj207230)-Klasse |
| **Microsoft.Devices.Radio.FMRadio**-Klasse | Keine direkte Entsprechung | 
| **Microsoft.Devices.Sensors.Accelerometer**-, **Compass**-Klassen | Im [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/br206408)-Namespace |
| **Microsoft.Devices.Sensors.Gyroscope**-Klasse | [
              **Gyrometer**
            ](https://msdn.microsoft.com/library/windows/apps/br225718)-Klasse |
| **Microsoft.Devices.Sensors.Motion**-Klasse | [
              **Inclinometer**
            ](https://msdn.microsoft.com/library/windows/apps/br225766)-Klasse |
| Globalization | |
| **System.Globalization**-Namespace | [
              **Windows.Globalization**
            ](https://msdn.microsoft.com/library/windows/apps/br206813)-Namespace |
| (ST = **System.Threading** <br/> **ST.Thread.CurrentCulture**-Eigenschaft | (SG = **System.Globalization** <br/> **S.CultureInfo.CurrentCulture**-Eigenschaft |
| (ST = **System.Threading** <br/> **ST.Thread.CurrentUICulture**-Eigenschaft | (SG = **System.Globalization** <br/> **S.CultureInfo.CurrentUICulture**-Eigenschaft |
| Grafiken und Animationen | |
| **Microsoft.Xna.Framework.\***-Namespaces, [XNA Framework Class Library](http://go.microsoft.com/fwlink/p/?LinkId=263769), [Content Pipeline Class Library](http://go.microsoft.com/fwlink/p/?LinkId=263770) | Keine direkte Entsprechung Verwenden Sie im Allgemeinen [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) mit C++. Weitere Informationen finden Sie unter [Entwickeln von Spielen](https://msdn.microsoft.com/library/windows/apps/hh452744) und [Interoperabilität von DirectX und XAML](https://msdn.microsoft.com/library/windows/apps/hh825871). |
| **Microsoft.Xna.Framework.Audio.Microphone**-Klasse | [
              **MediaCapture**
            ](https://msdn.microsoft.com/library/windows/apps/br241124)-Klasse |
| **Microsoft.Xna.Framework.Audio.SoundEffect**-Klasse | [
              **MediaElement**
            ](https://msdn.microsoft.com/library/windows/apps/br242926)-Klasse |
| **Microsoft.Xna.Framework.GamerServices**-Namespace | (WPS = **Windows.Phone.System** <br/> [
              **WPS.UserProfile.GameServices.Core**
            ](https://msdn.microsoft.com/library/windows/apps/jj207609)-Namespace |
| **Microsoft.Xna.Framework.GamerServices.Guide**-Klasse | Keine direkte Entsprechung | 
| **Microsoft.Xna.Framework.Input.GamePad**-Klasse | [
              **HardwareButtons**
            ](https://msdn.microsoft.com/library/windows/apps/jj207557)-Klasse |
| **Microsoft.Xna.Framework.Input.Touch.TouchPanel**-Klasse | [
              **GestureRecognizer**
            ](https://msdn.microsoft.com/library/windows/apps/br241937)-Klasse |
| (MXFM = **Microsoft.Xna.Framework.Media** <br/> **MXFM.MediaLibrary**-, **MXFM.PhoneExtensions.MediaLibraryExtensions**-Klassen | [
              **KnownFolders**
            ](https://msdn.microsoft.com/library/windows/apps/br227151)-Klasse |
| **Microsoft.Xna.Framework.Media.MediaQueue**-Klasse | [
              **SystemMediaTransportControls**
            ](https://msdn.microsoft.com/library/windows/apps/dn278677)-Klasse |
| **Microsoft.Xna.Framework.Media.Playlist**-Klasse | [
              **BackgroundMediaPlayer**
            ](https://msdn.microsoft.com/library/windows/apps/dn652527)-Klasse |
| **System.Windows.Media**-Namespace | [
              **Windows.UI.Xaml.Media**
            ](https://msdn.microsoft.com/library/windows/apps/br243045)-Namespace |
| **System.Windows.Media.RadialGradientBrush**-Klasse | Keine direkte Entsprechung Weitere Informationen finden Sie unter [Medien und Grafiken](wpsl-to-uwp-porting-xaml-and-ui.md#media). |
| **System.Windows.Media.Animation**-Namespace | [
              **Windows.UI.Xaml.Media.Animation**
            ](https://msdn.microsoft.com/library/windows/apps/br243232)-Namespace |
| **System.Windows.Media.Effects**-Namespace | Keine direkte Entsprechung | 
| **System.Windows.Media.Imaging**-Namespace | [
              **Windows.UI.Xaml.Media.Imaging**
            ](https://msdn.microsoft.com/library/windows/apps/br243258)-Namespace |
| **System.Windows.Media.Media3D**-Namespace | [
              **Windows.UI.Xaml.Media.Media3D**
            ](https://msdn.microsoft.com/library/windows/apps/br243274)-Namespace |
| **System.Windows.Shapes**-Namespace | [
              **Windows.UI.Xaml.Shapes**
            ](https://msdn.microsoft.com/library/windows/apps/br243401)-Namespace |
| Launcher und Chooser | |
| **Microsoft.Phone.Tasks.AddressChooserTask**-, **EmailAddressChooserTask**-, **PhoneNumberChooserTask**-Klassen | [
              **ContactPicker**
            ](https://msdn.microsoft.com/library/windows/apps/br224913)-Klasse |
| **Microsoft.Phone.Tasks.AddWalletItemTask**-, **AddWalletItemResult**-Klassen | [
              **Windows.ApplicationModel.Wallet**
            ](https://msdn.microsoft.com/library/windows/apps/dn631399)-Namespace |
| **Microsoft.Phone.Tasks.BingMapsDirectionsTask**-, **BingMapsTask**-Klassen | Keine direkte Entsprechung | 
| **Microsoft.Phone.Tasks.CameraCaptureTask**-Klasse | [
              **MediaCapture**
            ](https://msdn.microsoft.com/library/windows/apps/br241124)-Klasse. Auch [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030)-Klasse (nur Windows). |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | [
              **CurrentApp**
            ](https://msdn.microsoft.com/library/windows/apps/hh779765)-Klasse ([**RequestAppPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/hh967813)-Methode) |
| **Microsoft.Phone.Tasks.ConnectionSettingsTask**-, **MarketplaceHubTask**-, **MarketplaceReviewTask**-, **MarketplaceSearchTask**-, **MediaPlayerLauncher**-, **SearchTask**-, **SmsComposeTask**-, **WebBrowserTask**-Klassen | [
              **Launcher**
            ](https://msdn.microsoft.com/library/windows/apps/br241801)-Klasse |
| **Microsoft.Phone.Tasks.EmailComposeTask**-Klasse | [
              **EmailMessage**
            ](https://msdn.microsoft.com/library/windows/apps/dn631270)-Klasse |
| **Microsoft.Phone.Tasks.GameInviteTask**-Klasse | Keine direkte Entsprechung | 
| **Microsoft.Phone.Tasks.MapDownloaderTask**-, **MapsDirectionsTask**-, **MapsTask**-, **MapUpdaterTask**-Klassen | Keine direkte Entsprechung | 
| **Microsoft.Phone.Tasks.PhoneCallTask**-Klasse | [
              **PhoneCallManager**
            ](https://msdn.microsoft.com/library/windows/apps/dn624832)-Klasse |
| **Microsoft.Phone.Tasks.PhotoChooserTask**-Klasse | [
              **FileOpenPicker**
            ](https://msdn.microsoft.com/library/windows/apps/br207847)-Klasse |
| **Microsoft.Phone.Tasks.SaveAppointmentTask**-Klasse | [
              **AppointmentManager**
            ](https://msdn.microsoft.com/library/windows/apps/dn297254)-Klasse |
| **Microsoft.Phone.Tasks.SaveContactTask**-, **SaveEmailAddressTask**-, **SavePhoneNumberTask**-Klassen | [
              **StoredContact**
            ](https://msdn.microsoft.com/library/windows/apps/jj207727)-Klasse (nur Windows Phone) | 
| **Microsoft.Phone.Tasks.SaveRingtoneTask**-Klasse | Keine direkte Entsprechung | 
| **Microsoft.Phone.Tasks.ShareLinkTask**-, **ShareMediaTask**-, **ShareStatusTask**-Klassen | [
              **DataPackage**
            ](https://msdn.microsoft.com/library/windows/apps/br205873)-Klasse |
| Position | |
| **System.Device.Location**-Namespace | [
              **Windows.Devices.Geolocation**
            ](https://msdn.microsoft.com/library/windows/apps/br225603)-Namespace |
| **System.Device.GeoCoordinateWatcher**-Klasse | [
              **Geolocator**
            ](https://msdn.microsoft.com/library/windows/apps/br225534)-Klasse |
| Karten | |
| **Microsoft.Phone.Maps**-Namespaces | [
              **Windows.Services.Maps**
            ](https://msdn.microsoft.com/library/windows/apps/dn636979)-Namespace |
| **Microsoft.Phone.Maps.Controls**-Namespace | [
              **Windows.UI.Xaml.Controls.Maps**
            ](https://msdn.microsoft.com/library/windows/apps/dn610751)-Namespace |
| **Microsoft.Phone.Maps.Controls.Map**-Klasse | [
              **MapControl**
            ](https://msdn.microsoft.com/library/windows/apps/dn637004)-Klasse |
| **Microsoft.Phone.Maps.Services**-Namespace | [
              **Windows.Services.Maps**
            ](https://msdn.microsoft.com/library/windows/apps/dn636979)-Namespace |
| **Microsoft.Phone.Maps.Services.GeocodeQuery**-, **ReverseGeocodeQuery**-Klassen | [
              **MapLocationFinder**
            ](https://msdn.microsoft.com/library/windows/apps/dn627550)-Klasse |
| **System.Device.Location.GeoCoordinate**-Klasse | [
              **Geopoint**
            ](https://msdn.microsoft.com/library/windows/apps/dn263675)-Klasse |
| **Microsoft.Phone.Maps.Services.Route**-Klasse | [
              **MapRoute**
            ](https://msdn.microsoft.com/library/windows/apps/dn636937)-Klasse |
| **Microsoft.Phone.Maps.Services.RouteQuery**-Klasse | [
              **MapRouteFinder**
            ](https://msdn.microsoft.com/library/windows/apps/dn636938)-Klasse |
| Monetisierung | |
| **Microsoft.Phone.Marketplace**-Namespace | [
              **Windows.ApplicationModel.Store**
            ](https://msdn.microsoft.com/library/windows/apps/br225197)-Namespace |
| Medien | |
| **Microsoft.Phone.Media**-Namespace | [
              **MediaElement**
            ](https://msdn.microsoft.com/library/windows/apps/br242926)-Klasse |
| Netzwerk | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation** <br/> **MPNN.DeviceNetworkInformation**-Klasse | [
              **Hostname**
            ](https://msdn.microsoft.com/library/windows/apps/br207113)-, [**NetworkInformation**](https://msdn.microsoft.com/library/windows/apps/br207293)-Klassen
| (MPNN = **Microsoft.Phone.Net.NetworkInformation** <br/> **MPNN.NetworkInterface**-Klasse | [
              **NetworkInformation**
            ](https://msdn.microsoft.com/library/windows/apps/br207293)-Klasse |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation** <br/> **MPNN.NetworkInterfaceInfo**-Klasse | [
              **ConnectionProfile**
            ](https://msdn.microsoft.com/library/windows/apps/br207249)-Klasse |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation** <br/> **MPNN.NetworkInterfaceList**-Klasse | [
              **NetworkInformation**
            ](https://msdn.microsoft.com/library/windows/apps/br207293)-Klasse |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation** <br/> **MPNN.SocketExtensions**-Klasse | Keine direkte Entsprechung | 
| (MPNN = **Microsoft.Phone.Net.NetworkInformation** <br/> **MPNN.WebRequestExtensions**-Klasse | Keine direkte Entsprechung | 
| **Microsoft.Phone.Networking.Voip**-Namespace | Keine direkte Entsprechung | 
| **System.Net.CookieCollection**-Klasse | Wird noch unterstützt, aber einige Eigenschaften fehlen (z. B. IsReadOnly) |
| **System.Net.DownloadProgressChangedEventArgs**-Klasse und vergleichbare Klassen im Zusammenhang mit **System.Net.WebClient** | [
              **HttpClient**
            ](https://msdn.microsoft.com/library/windows/apps/dn298639)-Klasse (oder [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Ableitung von [System.Net.Http.StreamContent](https://msdn.microsoft.com/library/system.net.http.streamcontent.aspx) zum Messen des Fortschritts |
| **System.Net.DnsEndPoint**-, **IPAddress**-Klassen | Diese Klassen werden zwar noch unterstützt, aber einige Eigenschaften fehlen. Alternativ dazu ist das Portieren zur [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113)-Klasse möglich. |
| **System.Net.HttpUtility**-Klasse | [
              **HtmlFormatHelper**
            ](https://msdn.microsoft.com/library/windows/apps/hh738437)-Klasse |
| **System.Net.HttpWebRequest**-Klasse | Wird teilweise unterstützt, aber die empfohlene fortschrittliche Alternative ist die [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)-Klasse (oder [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Für diese APIs wird [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) verwendet, um eine HTTP-Anforderung darzustellen. |
| **System.Net.HttpWebResponse**-Klasse | Wird weiterhin unterstützt, aber anstelle von „Close()“ wird „Dispose()“ verwendet. Die empfohlene fortschrittliche Alternative ist jedoch die [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)-Klasse (oder [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Für diese APIs wird [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx) verwendet, um eine HTTP-Antwort darzustellen. |
| (SNN = **System.Net.NetworkInformation** <br/> **SNN.NetworkChange**-Klasse | Wird weiterhin unterstützt, mit Ausnahme des Konstruktors |
| **System.Net.OpenReadCompletedEventArgs**-Klasse und vergleichbare Klassen im Zusammenhang mit **System.Net.WebClient** | [
              **HttpClient**
            ](https://msdn.microsoft.com/library/windows/apps/dn298639)-Klasse (oder [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx) |
| **System.Net.Sockets.Socket**-Klasse | Wird weiterhin unterstützt, aber anstelle von „Close()“ wird „Dispose()“ verwendet. Alternativ dazu ist das Portieren zur[**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)-Klasse möglich. |
| **System.Net.Sockets.SocketException**-Klasse | Wird zwar noch unterstützt, aber anstelle von ErrorCode wird die SocketErrorCode-Eigenschaft verwendet.
| **System.Net.Sockets.UdpAnySourceMulticastClient**-, **UdpSingleSourceMulticastClient**-Klassen | [
              **DatagramSocket**
            ](https://msdn.microsoft.com/library/windows/apps/br241319)-Klasse | 
| **System.Net.UploadProgressChangedEventArgs**-Klasse und vergleichbare Klassen im Zusammenhang mit **System.Net.WebClient** | [
              **HttpClient**
            ](https://msdn.microsoft.com/library/windows/apps/dn298639)-Klasse (oder [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)
| **System.Net.WebClient**-Klasse | [
              **HttpClient**
            ](https://msdn.microsoft.com/library/windows/apps/dn298639)-Klasse (oder [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)
| **System.Net.WebRequest**-Klasse | Wird teilweise unterstützt (anderer Eigenschaftensatz), die empfohlene modernere Alternative ist jedoch die [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)-Klasse (oder [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Für diese APIs wird [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) verwendet, um eine HTTP-Anforderung darzustellen.
| **System.Net.WebResponse**-Klasse | Wird weiterhin unterstützt, aber anstelle von „Close()“ wird „Dispose()“ verwendet. Die empfohlene fortschrittliche Alternative ist jedoch die [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)-Klasse (oder [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Für diese APIs wird [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx) verwendet, um eine HTTP-Antwort darzustellen.
| (SN = **System.Net** <br/> **SN.WriteStreamClosedEventArgs**-Klasse | [
              **HttpClient**
            ](https://msdn.microsoft.com/library/windows/apps/dn298639)-Klasse (oder [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)
| (SN = **System.Net** <br/> **SN.WriteStreamClosedEventHandler**-Klasse | [
              **HttpClient**
            ](https://msdn.microsoft.com/library/windows/apps/dn298639)-Klasse (oder [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)
| **System.UriFormatException**-Klasse | **System.FormatException**-Klasse |
| Benachrichtigungen | |
| MPN = **Microsoft.Phone.Notification**-Namespace | [
              **Windows.UI.Notifications**
            ](https://msdn.microsoft.com/library/windows/apps/br208661)-, [**Windows.Networking.PushNotifications**](https://msdn.microsoft.com/library/windows/apps/br241307)-Namespaces |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotification**-Klasse | [
              **TileNotification**
            ](https://msdn.microsoft.com/library/windows/apps/br208616)-Klasse |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotificationChannel**-Klasse | [
              **PushNotificationChannel**
            ](https://msdn.microsoft.com/library/windows/apps/br241283)-Klasse |
| Programmierung | |
| **System**-Namespace | [
              **Windows.Foundation**
            ](https://msdn.microsoft.com/library/windows/apps/br226021)-Namespace |
| **System.Diagnostics.StackFrame**-, **StackTrace**-Klassen | Keine direkte Entsprechung | 
| **System.Diagnostics**-Namespace | [
              **Windows.Foundation.Diagnostics**
            ](https://msdn.microsoft.com/library/windows/apps/br206677)-Namespace |
| **System.ICloneable**-Schnittstelle | Eine benutzerdefinierte Methode, mit der der passende Typ zurückgegeben wird. |
| **System.Reflection.Emit.ILGenerator**-Klasse | Keine direkte Entsprechung | 
| Reaktive Erweiterungen | |
| **Microsoft.Phone.Reactive**-Namespace | Keine direkte Entsprechung | 
| Reflektion | |
| **System.Type**-Klasse | **System.Reflection.TypeInfo**-Klasse. Weitere Informationen finden Sie unter [Reflection in the .NET Framework for Windows Store Apps](https://msdn.microsoft.com/library/hh535795.aspx). |
| Ressourcen | |
| **System.Resources.ResourceManager**-Klasse | (WA = **Windows.ApplicationModel**<br/>[
              **WA.Resources.Core**
            ](https://msdn.microsoft.com/library/windows/apps/br225039)- und [**WA.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022)-Namespaces, [**ResourceManager**](https://msdn.microsoft.com/library/windows/apps/br206078)-Klasse. Weitere Informationen finden Sie unter [Erstellen und Abrufen von Ressourcen in Windows-Runtime-Apps](https://msdn.microsoft.com/library/windows/apps/xaml/hh694557.aspx). |
| Sicheres Element | |
| (MPS = **Microsoft.Phone.SecureElement** <br/> **MPS.SecureElementChannel**-, **MPS.SecureElementSession**-Klassen | [
              **SmartCardConnection**
            ](https://msdn.microsoft.com/library/windows/apps/dn608002)-Klasse |
| (MPS = **Microsoft.Phone.SecureElement** <br/> **MPS.SecureElementReader**-Klasse | [
              **SmartCardReader**
            ](https://msdn.microsoft.com/library/windows/apps/dn263857)-Klasse |
| Sicherheit | |
| (SSC = **System.Security.Cryptography** <br/> **SSC.Aes**-, **SSC.RSA**-Klassen | [
              **CryptographicEngine**
            ](https://msdn.microsoft.com/library/windows/apps/br241490)-Klasse |
| (SSC = **System.Security.Cryptography** <br/> **SSC.HMACSHA256**-, **SSC.SHA256**-Klassen | [
              **HashAlgorithmProvider**
            ](https://msdn.microsoft.com/library/windows/apps/br241511)-Klasse |
| (SSC = **System.Security.Cryptography** <br/> **SSC.ProtectedData**-Klasse | [
              **DataProtectionProvider**
            ](https://msdn.microsoft.com/library/windows/apps/br241559)-Klasse |
| (SSC = **System.Security.Cryptography** <br/> **SSC.RandomNumberGenerator**-Klasse | [
              **CryptographicBuffer**
            ](https://msdn.microsoft.com/library/windows/apps/br227092)-Klasse |
| (SSC = **System.Security.Cryptography** <br/> **SSC.X509Certificates.X509Certificate**-Klasse | [
              **CertificateEnrollmentManager**
            ](https://msdn.microsoft.com/library/windows/apps/br212075)-Klasse |
| Shell | |
| (MPSh = **Microsoft.Phone.Shell** <br/> **MPSh.ApplicationBar**-Klasse | [
              **CommandBar**
            ](https://msdn.microsoft.com/library/windows/apps/dn279427)-Klasse |
| (MPSh = **Microsoft.Phone.Shell** <br/> **MPSh.ApplicationBarIconButton**-Klasse | [
              **AppBarButton**
            ](https://msdn.microsoft.com/library/windows/apps/dn279244)-Klasse (bei Verwendung innerhalb der Eigenschaft [**PrimaryCommands**](https://msdn.microsoft.com/library/windows/apps/dn279430))
| (MPSh = **Microsoft.Phone.Shell** <br/> **MPSh.ApplicationBarMenuItem**-Klasse | [
              **AppBarButton**
            ](https://msdn.microsoft.com/library/windows/apps/dn279244)-Klasse (bei Verwendung innerhalb der Eigenschaft [**SecondaryCommands**](https://msdn.microsoft.com/library/windows/apps/dn279434))
| (MPSh = **Microsoft.Phone.Shell** <br/> **MPSh.CycleTileData**-, **MPSh.FlipTileData**-, **MPSh.IconicTileData**-, **MPSh.ShellTileData**-, **MPSh.StandardTileData**-Klassen | [
              **TileTemplateType**
            ](https://msdn.microsoft.com/library/windows/apps/br208621)-Klasse |
| (MPSh = **Microsoft.Phone.Shell** <br/> **MPSh.PhoneApplicationService**-Klasse | [
              **CoreApplication**
            ](https://msdn.microsoft.com/library/windows/apps/br225016)-, [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816)-Klassen
| (MPSh = **Microsoft.Phone.Shell** <br/> **MPSh.ProgressIndicator**-Klasse | [
              **StatusBarProgressIndicator**
            ](https://msdn.microsoft.com/library/windows/apps/dn633865)-Klasse |
| (MPSh = **Microsoft.Phone.Shell** <br/> **MPSh.ShellTile**-Klasse | [
              **SecondaryTile**
            ](https://msdn.microsoft.com/library/windows/apps/br242183)-Klasse |
| (MPSh = **Microsoft.Phone.Shell** <br/> **MPSh.ShellTileSchedule**-Klasse | [
              **TileUpdater**
            ](https://msdn.microsoft.com/library/windows/apps/br208628)-Klasse |
| (MPSh = **Microsoft.Phone.Shell** <br/> **MPSh.ShellToast**-Klasse | [
              **ToastNotificationManager**
            ](https://msdn.microsoft.com/library/windows/apps/br208642)-Klasse |
| (MPSh = **Microsoft.Phone.Shell** <br/> **MPSh.SystemTray**-Klasse | [
              **StatusBar**
            ](https://msdn.microsoft.com/library/windows/apps/dn633864)-Klasse |
| Speicher und E/A | |
| **Microsoft.Phone.Storage.ExternalStorage**-, **ExternalStorageDevice**-, **ExternalStorageFile**-, **ExternalStorageFolder**-Klassen | [
              **KnownFolders**
            ](https://msdn.microsoft.com/library/windows/apps/br227151)-Klasse |
| **System.IO**-Namespace | [
              **Windows.Storage**
            ](https://msdn.microsoft.com/library/windows/apps/br227346)-, [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791)-Namespaces
| **System.IO.Directory**-Klasse | [
              **StorageFolder**
            ](https://msdn.microsoft.com/library/windows/apps/br227230)-Klasse |
| **System.IO.File**-Klasse | [
              **StorageFile**
            ](https://msdn.microsoft.com/library/windows/apps/br227171)- und [**PathIO**](https://msdn.microsoft.com/library/windows/apps/hh701663)-Klassen
| (SII = **System.IO.IsolatedStorage** <br/> **SII.IsolatedStorageFile**-Klasse | [
              **ApplicationData.LocalFolder**
            ](https://msdn.microsoft.com/library/windows/apps/br241621)-Eigenschaft |
| (SII = **System.IO.IsolatedStorage** <br/> **SII.IsolatedStorageSettings**-Klasse | [
              **ApplicationData.LocalSettings**
            ](https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.localsettings.aspx)-Eigenschaft |
| **System.IO.Stream**-Klasse | Wird noch unterstützt, aber anstelle von BeginRead()/EndRead() und BeginWrite()/EndWrite() wird ReadAsync() und WriteAsync() verwendet. |
| Brieftasche | |
| **Microsoft.Phone.Wallet**-Namespace | [
              **Windows.ApplicationModel.Wallet**
            ](https://msdn.microsoft.com/library/windows/apps/dn631399)-Namespace |
| Xml | |
| (SX = **System.Xml** | **SX.XmlConvert.ToDateTime**-Methode |
| (SX = **System.Xml** | **SX.XmlConvert.ToDateTimeOffset**-Methode |
 

Das nächste Thema ist [Portieren des Projekts](wpsl-to-uwp-porting-to-a-uwp-project.md)



<!--HONumber=May16_HO2-->


