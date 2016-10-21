---
author: mcleblanc
title: Erste Schritte mit der Navigation
description: Erste Schritte mit der Navigation
ms.assetid: F4DF5C5F-C886-4483-BBDA-498C4E2C1BAF
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: daf56299b3a5e81bb756bc9fcb8112da044b00f3

---

# Erste Schritte: Navigation

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## Hinzufügen von Navigation

iOS bietet die **UINavigationController**-Klasse als In-App-Navigationshilfe: Sie erstellen eine Hierarchie von **UIViewControllers**, die Ihre App definieren, durch das Drücken und Aufklappen von Ansichten.

Eine Windows 10-App mit mehreren Ansichten holt dagegen mehr aus einem Website-orientierten Ansatz für die Navigation heraus. Sie können sich vorstellen, wie Benutzer von Seite zu Seite wechseln, indem Sie auf Steuerelemente klicken, um durch die App zu navigieren. Weitere Informationen finden Sie unter [Navigationsdesigngrundlagen](https://msdn.microsoft.com/library/windows/apps/dn958438).

Bei einer Windows 10-App ist die Verwendung der [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)-Klasse eine der Methoden zum Verwalten dieser Navigation. Die folgende exemplarische Vorgehensweise veranschaulicht, wie Sie dies ausprobieren können.

Fahren Sie mit der Lösung aus früheren Schritten fort, öffnen Sie die **MainPage.xaml**-Datei, und fügen Sie eine Schaltfläche in der Ansicht **Entwurf** hinzu. Ändern Sie die **Content**-Eigenschaft der Schaltfläche von „Button“ in „Go To Page“. Erstellen Sie anschließend einen Handler für das **Click**-Ereignis der Schaltfläche, wie in der folgenden Abbildung dargestellt. Wenn Sie nicht mehr wissen, wie das geht, schlagen Sie unter der exemplarischen Vorgehensweise im vorherigen Abschnitt nach (Hinweis: Doppelklicken Sie auf die Schaltfläche in der Ansicht **Entwurf**).

![Hinzufügen einer Schaltfläche und des zugehörigen Click-Ereignisses in Visual Studio](images/ios-to-uwp/vs-go-to-page.png)

Als Nächstes fügen wir eine neue Seite hinzu. Tippen Sie in der Ansicht **Lösung** auf das Menü **Projekt**, und tippen Sie auf **Neues Element hinzufügen**. Tippen Sie wie in der folgenden Abbildung dargestellt auf **Leere Seite** und dann auf **Hinzufügen**.

![Hinzufügen einer neuen Seite in Visual Studio](images/ios-to-uwp/vs-add-new-page.png)

Fügen Sie als Nächstes der Datei „BlankPage.xaml“ eine Schaltfläche hinzu. Nun wird das AppBarButton-Steuerelement verwendet und soll ein Bild mit einem Zurück-Pfeil erhalten: Fügen Sie hierzu in der **XAML**-Ansicht ` <AppBarButton Icon="Back"/>` zwischen den `<Grid> </Grid>`-Elementen hinzu.

Nun fügen wir der Schaltfläche einen Ereignishandler hinzu: Doppelklicken Sie in der Ansicht **Entwurf** auf das Steuerelement, und Microsoft Visual Studio fügt dem Feld **Click** wie in der folgenden Abbildung dargestellt den Text „AppBarButton\_Click“ hinzu. Anschließend wird der entsprechende Ereignishandler der Datei „BlankPage.xaml.cs“ hinzugefügt und angezeigt.

![Hinzufügen einer Zurück-Schaltfläche und des zugehörigen Click-Ereignisses in Visual Studio](images/ios-to-uwp/vs-add-back-button.png)

Wenn Sie zur **XAML**-Ansicht der Datei „BlankPage.xaml“ zurückkehren, sollte der Extensible Application Markup Language (XAML)-Code des `<AppBarButton>`-Elements nun wie folgt lauten:

` <AppBarButton Icon="Back" Click="AppBarButton_Click"/>`

Kehren Sie zur Datei „BlankPage.xaml.cs“ zurück, und fügen Sie diesen Code hinzu, damit der Benutzer zur vorherigen Seite zurückkehrt, nachdem er auf die Schaltfläche getippt hat.

```csharp
private void AppBarButton_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.    
    Frame.GoBack();
}
```

Öffnen Sie zum Schluss die Datei „MainPage.xaml.cs“, und fügen Sie diesen Code hinzu. Mithilfe dieses Codes wird BlankPage geöffnet, nachdem der Benutzer auf die Schaltfläche getippt hat.

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.
    Frame.Navigate(typeof(BlankPage1));
}
```

Führen Sie nun das Programm aus. Tippen Sie auf die Schaltfläche „Go To Page“, um zu der anderen Seite zu wechseln, und tippen Sie dann auf die Schaltfläche mit dem Zurück-Pfeil, um zur vorherigen Seite zurückzukehren.

Die Seitennavigation wird mithilfe der [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)-Klasse verwaltet. Ähnlich wie die **UINavigationController**-Klasse in iOS, die die Methoden **pushViewController** und **popViewController** verwendet, stellt die **Frame**-Klasse für Windows Store-Apps die Methoden [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) und [**GoBack**](https://msdn.microsoft.com/library/windows/apps/dn996568) bereit. Die **Frame**-Klasse verfügt außerdem über eine Methode mit dem Namen [**GoForward**](https://msdn.microsoft.com/library/windows/apps/br242693), die genau das tut, was Sie vermutlich erwartet haben.

Bei dieser exemplarischen Vorgehensweise wird immer dann eine neue Instanz von BlankPage erstellt, wenn Sie zu dieser Seite navigieren. (Die vorherige Instanz wird automatisch *freigegeben*.) Wenn nicht jedes Mal eine neue Instanz erstellt werden soll, fügen Sie dem Konstruktor der BlankPage-Klasse in der Datei „BlankPage.xaml.cs“ den folgenden Code hinzu. Dadurch wird das [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506)-Verhalten aktiviert.

```csharp
public BlankPage()
{
    this.InitializeComponent();
    // Add the following line of code.
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

Sie können auch die [**CacheSize**](https://msdn.microsoft.com/library/windows/apps/br242683)-Eigenschaft der **Frame**-Klasse abrufen oder festlegen, um zu definieren, wie viele Seiten im Navigationsverlauf zwischengespeichert werden können.

Weitere Informationen zur Navigation finden Sie unter [Navigation](https://msdn.microsoft.com/library/windows/apps/mt187344) und unter [XAML-Beispiel für Charakteranimationen](http://go.microsoft.com/fwlink/p/?LinkID=242401).

**Hinweis**  Informationen zur Navigation für WindowsStore-Apps mit JavaScript und HTML finden Sie unter [Schnellstart: Verwenden der Einzelseitennavigation](https://msdn.microsoft.com/library/windows/apps/hh452768).
 
### Nächster Schritt

[Erste Schritte: Animation](getting-started-animation.md)




<!--HONumber=Aug16_HO3-->


