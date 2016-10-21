---
author: Jwmsft
Description: "Erfahren Sie, wie Sie in einer einfachen App für die Universelle Windows-Plattform (UWP) mit zwei Seiten navigieren."
title: Navigieren zwischen zwei Seiten
ms.assetid: 0A364C8B-715F-4407-9426-92267E8FB525
label: Navigate between two pages
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 0cf689ae9d74735f33baa85b3a8791a68a4467d4
ms.openlocfilehash: d5ff0dfcb10c6977bb93f28cbf8631b9b5dfb83f

---

# Navigieren zwischen zwei Seiten

Erfahren Sie, wie Sie in einer einfachen App für die Universelle Windows-Plattform (UWP) mit zwei Seiten navigieren.

![Navigationsbeispiel mit zwei Seiten](images/nav-peertopeer-2page.png)


**Wichtige APIs**

-   [**Windows.UI.Xaml.Controls.Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)
-   [**Windows.UI.Xaml.Controls.Page**](https://msdn.microsoft.com/library/windows/apps/br227503)
-   [**Windows.UI.Xaml.Navigation**](https://msdn.microsoft.com/library/windows/apps/br243300)


## Erstellen der leeren App


1.  Klicken Sie im Microsoft Visual Studio-Menü auf **Datei &gt; Neues Projekt**.
2.  Erweitern Sie im linken Bereich des Dialogfelds **Neues Projekt** den Knoten **Visual C# -&gt; Windows -&gt; Universell** oder **Visual C++ -&gt; Windows -&gt; Universell**.
3.  Wählen Sie im mittleren Bereich die Option **Leere App** aus.
4.  Geben Sie in das Feld **Name** den Wert **NavApp1** ein, und klicken Sie anschließend auf **OK**.

    Die Projektmappe wird erstellt, und die Projektdateien werden im **Projektmappen-Explorer** angezeigt.

5.  Klicken Sie zum Ausführen des Programms im Menü auf **Debuggen** &gt; **Debugging starten**, oder drücken Sie F5.

    Es wird eine leere Seite angezeigt.

6.  Drücken Sie UMSCHALT+F5, um das Debuggen zu beenden und zu Visual Studio zurückzukehren.

## Hinzufügen von Standardseiten

Fügen Sie im nächsten Schritt zwei Inhaltsseiten zum Projekt hinzu.

Führen Sie die folgenden Schritte zweimal aus, um zwei Seiten zum Navigieren hinzuzufügen.

1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten **BlankApp**, um das Kontextmenü zu öffnen.
2.  Wählen Sie im Kontextmenü **Hinzufügen** &gt; **Neues Element** aus.
3.  Wählen Sie im Dialogfeld **Neues Element hinzufügen** im mittleren Bereich die Option **Leere Seite** aus.
4.  Geben Sie in das Feld **Name** den Wert **Page1** (oder **Page2**) ein, und wählen Sie anschließend **Hinzufügen**.

Diese Dateien sollten nun als Teil des Projekts „NavApp1“ aufgeführt werden.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
<th align="left">C++</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><ul>
<li>Page1.xaml</li>
<li>Page1.xaml.cs</li>
<li>Page2.xaml</li>
<li>Page2.xaml.cs</li>
</ul></td>
<td align="left"><ul>
<li>Page1.xaml</li>
<li>Page1.xaml.cpp</li>
<li>Page1.xaml.h</li>
<li>Page2.xaml</li>
<li>Page2.xaml.cpp</li>
<li>Page2.xaml.h
<div class="alert">
<strong>Hinweis</strong>  
<p>Funktionen sind in der Headerdatei (.h) deklariert und in der CodeBehind-Datei (.cpp) implementiert.</p>
</div>
<div>
 
</div></li>
</ul></td>
</tr>
</tbody>
</table>

 

Fügen Sie folgenden Inhalt zur UI von „Page1.xaml“ hinzu.

-   Fügen Sie ein [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)-Element mit der Bezeichnung `pageTitle` als untergeordnetes Element des [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)-Stammelements hinzu. Ändern Sie die [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676)-Eigenschaft in `Page 1`.

```xaml
<TextBlock x:Name="pageTitle" Text="Page 1" />
```

-   Fügen Sie das folgende [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739)-Element als untergeordnetes Element des [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)-Stammelements und nach dem `pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)-Element hinzu.


```xaml
<HyperlinkButton Content="Click to go to page 2"
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

Fügen Sie den folgenden Code zur `Page1`-Klasse in der CodeBehind-Datei „Page1.xaml“ hinzu, um das `Click`-Ereignis des zuvor hinzugefügten [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739)-Elements zu behandeln. Hier navigieren wir zu „Page2.xaml“.

> [!div class="tabbedCodeSnippets"]
```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2));
}
```
```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid));
}
```

Nehmen Sie die folgenden Änderungen an der UI von „Page2.xaml“ vor.

-   Fügen Sie ein [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)-Element mit der Bezeichnung `pageTitle` als untergeordnetes Element des [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)-Stammelements hinzu. Ändern Sie den Wert der [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676)-Eigenschaft in `Page 2`:

```xaml
<TextBlock x:Name="pageTitle" Text="Page 2" />
```

-   Fügen Sie das folgende [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739)-Element als untergeordnetes Element des [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)-Stammelements und nach dem `pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)-Element hinzu.

```xaml
<HyperlinkButton Content="Click to go to page 1"
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

Fügen Sie den folgenden Code zur `Page2`-Klasse in der CodeBehind-Datei „Page2.xaml“ hinzu, um das `Click`-Ereignis des zuvor hinzugefügten [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739)-Elements zu behandeln. Hier navigieren wir zu „Page1.xaml“.

**Hinweis**  
Bei C++-Projekten müssen Sie in der Headerdatei jeder Seite, die auf eine andere Seite verweist, eine `#include`-Direktive hinzufügen. Für das hier gezeigte Beispiel für die seitenübergreifende Navigation enthält die Datei „page1.xaml.h“ `#include "Page2.xaml.h"`, und die Datei „page2.xaml.h“ wiederum enthält `#include "Page1.xaml.h"`.

> [!div class="tabbedCodeSnippets"]
```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page1));
}
```
```cpp
void Page2::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid));
}
```

Da wir die Inhaltsseiten nun vorbereitet haben, müssen wir festlegen, dass „Page1.xaml“ beim Starten der App angezeigt wird.

Öffnen Sie die CodeBehind-Datei „app.xaml“, und ändern Sie den `OnLaunched`-Handler.

Hier geben wir `Page1` im Aufruf von [**Frame.Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) anstelle von `MainPage` an.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> protected override void OnLaunched(LaunchActivatedEventArgs e)
> {
>     Frame rootFrame = Window.Current.Content as Frame;
>
>     // Do not repeat app initialization when the Window already has content,
>     // just ensure that the window is active
>     if (rootFrame == null)
>     {
>         // Create a Frame to act as the navigation context and navigate to the first page
>         rootFrame = new Frame();
>
>         rootFrame.NavigationFailed += OnNavigationFailed;
>
>         if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
>         {
>             //TODO: Load state from previously suspended application
>         }
>
>         // Place the frame in the current Window
>         Window.Current.Content = rootFrame;
>     }
>
>     if (rootFrame.Content == null)
>     {
>         // When the navigation stack isn&#39;t restored navigate to the first page,
>         // configuring the new page by passing required information as a navigation
>         // parameter
>         rootFrame.Navigate(typeof(Page1), e.Arguments);
>     }
>     // Ensure the current window is active
>     Window.Current.Activate();
> }
> ```
> ```cpp
> void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ e)
> {
>     auto rootFrame = dynamic_cast<Frame^>(Window::Current->Content);
>
>     // Do not repeat app initialization when the Window already has content,
>     // just ensure that the window is active
>     if (rootFrame == nullptr)
>     {
>         // Create a Frame to act as the navigation context and associate it with
>         // a SuspensionManager key
>         rootFrame = ref new Frame();
>
>         rootFrame->NavigationFailed +=
>             ref new Windows::UI::Xaml::Navigation::NavigationFailedEventHandler(
>                 this, &amp;App::OnNavigationFailed);
>
>         if (e->PreviousExecutionState == ApplicationExecutionState::Terminated)
>         {
>             // TODO: Load state from previously suspended application
>         }
>         
>         // Place the frame in the current Window
>         Window::Current->Content = rootFrame;
>     }
>
>     if (rootFrame->Content == nullptr)
>     {
>         // When the navigation stack isn&#39;t restored navigate to the first page,
>         // configuring the new page by passing required information as a navigation
>         // parameter
>         rootFrame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid), e->Arguments);
>     }
>
>     // Ensure the current window is active
>     Window::Current->Activate();
> }
> ```

**Hinweis**  In diesem Beispielcode wird der Rückgabewert von [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) verwendet, um eine App-Ausnahme auszulösen, wenn die Navigation zum anfänglichen Fensterframe der App einen Fehler verursacht. Wenn **Navigate** den Wert **true** zurückgibt, findet die Navigation statt.

Erstellen Sie nun die App, und führen Sie sie aus. Klicken Sie auf den Link „Click to go to page 2“. Die zweite Seite mit der Bezeichnung „Seite 2“ wird geladen und im Frame angezeigt.

## Frame- und Page-Klassen

Bevor wir der App weitere Funktionen hinzufügen, betrachten wir zunächst, inwiefern die hinzugefügten Seiten Navigationsunterstützung für die App bereitstellen.

Zuerst wird ein [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) (`rootFrame`) für die App in der `App.OnLaunched`-Methode der CodeBehind-Datei „App.xaml“ erstellt. Die [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)-Klasse unterstützt verschiedene Navigationsmethoden, z.B. [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694), [**GoBack**](https://msdn.microsoft.com/library/windows/apps/dn996568) oder [**GoForward**](https://msdn.microsoft.com/library/windows/apps/br242693) und Eigenschaften wie [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543), [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547) oder [**BackStackDepth**](https://msdn.microsoft.com/library/windows/apps/hh967995). Die [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694)-Methode wird zum Anzeigen von Inhalt im **Frame** verwendet.
 
In unserem Beispiel wird `Page1` an die [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694)-Methode übergeben. Mit dieser Methode wird der Inhalt des aktuellen Fensters der App auf [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) festgelegt und der Inhalt der angegebenen Seite in **Frame** (in unserem Beispiel „Page1.xaml“ oder standardmäßig „MainPage.xaml“) geladen.

`Page1` ist eine Unterklasse der [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503)-Klasse. Die **Page**-Klasse hat eine schreibgeschützte [**Frame**](https://msdn.microsoft.com/library/windows/apps/br227504)-Eigenschaft, die den [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) mit der **Page**-Klasse abruft. Wenn der [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737)-Ereignishandler der [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739)-Klasse ` Frame.Navigate(typeof(Page2))` aufruft, wird im **Frame** des App-Fensters der Inhalt von „Page2.xaml“ angezeigt.

Wenn eine Seite in den Frame geladen wird, wird diese Seite als [**PageStackEntry**](https://msdn.microsoft.com/library/windows/apps/dn298572)-Klasse der [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543)- oder [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547)-Eigenschaft der [**Frame**](https://msdn.microsoft.com/library/windows/apps/br227504)-Klasse hinzugefügt.

## Übergeben von Informationen zwischen Seiten

Unsere App navigiert zwischen zwei Seiten, sie bietet jedoch noch keine interessanten Funktionen. Bei vielen Apps mit mehreren Seiten müssen die Seiten Informationen freigeben. Übergeben wir also einige Informationen der ersten Seite an die zweite Seite.

Ersetzen Sie in „Page1.xaml“ das zuvor hinzugefügte [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739)-Element mit der folgenden [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)-Klasse.

Hier werden eine [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)-Bezeichnung und ein [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)-Element (`name`) zum Eingeben einer Textzeichenfolge hinzugefügt.

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Text="Enter your name"/>
    <TextBox HorizontalAlignment="Center" Width="200" Name="name"/>
    <HyperlinkButton Content="Click to go to page 2"
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

Fügen Sie im `HyperlinkButton_Click`-Ereignishandler der CodeBehind-Datei „Page1.xaml“ einen Parameter hinzu, der die `Text`-Eigenschaft von `name` [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) auf die `Navigate`-Methode verweist.

> [!div class="tabbedCodeSnippets"]
```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2), name.Text);
}
```
```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid), name->Text);
}
```

Ersetzen Sie in „Page2.xaml“ das zuvor hinzugefügte [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739)-Element mit der folgenden [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)-Klasse.

Hier fügen wir einen [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) hinzu, der die bereitgestellten Texteingaben aus [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) auf Page1 anzeigt.

```XAML
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Name="greeting"/>
    <HyperlinkButton HorizontalAlignment="Center" Content="Click to go to page 1" Click="HyperlinkButton_Click"/>
</StackPanel>
```

Überschreiben Sie in der CodeBehind-Datei „Page2.xaml“ die `OnNavigatedTo`-Methode durch Folgendes:

> [!div class="tabbedCodeSnippets"]
```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    if (e.Parameter is string)
    {
        greeting.Text = "Hi, " + e.Parameter.ToString();
    }
    else
    {
        greeting.Text = "Hi!";
    }
    base.OnNavigatedTo(e);
}
```
```cpp
void Page2::OnNavigatedTo(NavigationEventArgs^ e)
{
    if (dynamic_cast<Platform::String^>(e->Parameter) != nullptr)
    {
        greeting->Text = "Hi," + e->Parameter->ToString();
    }
    else
    {
        greeting->Text = "Hi!";
    }
    ::Windows::UI::Xaml::Controls::Page::OnNavigatedTo(e);
}
```

Führen Sie die App aus, geben Sie Ihren Namen in das Textfeld ein, und klicken Sie auf den Link **Click to go to page 2**. Beim Aufruf von `this.Frame.Navigate(typeof(Page2), tb1.Text)` im [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737)-Ereignis des [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739)-Elements wird die `name.Text`-Eigenschaft an `Page2` übergeben. Der Wert der Ereignisdaten wird zum Anzeigen der Nachricht auf der Seite verwendet.

## Zwischenspeichern einer Seite

Inhalt und Zustand einer Seite werden nicht standardmäßig zwischengespeichert, sondern müssen auf jeder Seite der App aktiviert werden.

In unserem einfachen Beispiel gibt es keine Zurück-Schaltfläche (Informationen zur Rückwärtsnavigation finden Sie unter [Navigation per Schaltfläche „Zurück“](navigation-history-and-backwards-navigation.md)). Würden Sie jedoch auf `Page2` auf eine Zurück-Schaltfläche klicken, würde das [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)-Element (und alle anderen Felder) auf `Page1` auf den Standardzustand zurückgesetzt werden. Eine Möglichkeit zur Umgehung dieses Problems ist die Verwendung der [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506)-Eigenschaft, um anzugeben, dass eine Seite zum Seitencache des Frames hinzugefügt werden soll.

Legen Sie im Konstruktor von `Page1` die [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506)-Eigenschaft auf [**Enabled**](https://msdn.microsoft.com/library/windows/apps/br243284) fest. Hierdurch werden alle Inhalts- und-Zustandswerte für die Seite zurückbehalten, bis der Höchstwert für den Seitencache des Frames überschritten wird.

Legen Sie [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) auf [**Required**](https://msdn.microsoft.com/library/windows/apps/br243284) fest, wenn Cachegrößenbeschränkungen für den Frame ignoriert werden sollen. Cachegrößenbeschränkungen können je nach Arbeitsspeicherlimit eines Geräts äußerst wichtig sein.

**Hinweis**  Die [**CacheSize**](https://msdn.microsoft.com/library/windows/apps/br242683)-Eigenschaft gibt die Anzahl der Seiten im Navigationsverlauf an, die für den Frame zwischengespeichert werden können.

> [!div class="tabbedCodeSnippets"]
```csharp
public Page1()
{
    this.InitializeComponent();
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```
```cpp
Page1::Page1()
{
    this->InitializeComponent();
    this->NavigationCacheMode = Windows::UI::Xaml::Navigation::NavigationCacheMode::Enabled;
}
```

## Verwandte Artikel

* [Navigationsdesigngrundlagen für UWP-Apps](https://msdn.microsoft.com/library/windows/apps/dn958438)
* [Richtlinien für Registerkarten und Pivots](https://msdn.microsoft.com/library/windows/apps/dn997788)
* [Richtlinien für Navigationsbereiche](https://msdn.microsoft.com/library/windows/apps/dn997766)
 

 



<!--HONumber=Sep16_HO2-->


