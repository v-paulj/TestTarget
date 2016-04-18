---
ms.assetid: DC235C16-8DAF-4078-9365-6612A10F3EC3
title: Erstellen der App „Hello World“ in C++ (Windows 10)
description: In Microsoft Visual Studio 2015 können Sie mithilfe von C++ eine App entwickeln, die unter Windows 10 sowie auf Smartphones mit Windows 10 ausgeführt werden kann. Die Benutzeroberfläche dieser App ist in XAML (Extensible Application Markup Language) definiert.
---

# Erstellen der App „Hello World“ in C++ (Windows 10)

In Microsoft Visual Studio 2015 können Sie mithilfe von C++ eine App entwickeln, die unter Windows 10 sowie auf Smartphones mit Windows 10 ausgeführt werden kann. Die Benutzeroberfläche dieser App ist in XAML (Extensible Application Markup Language) definiert.

Verwenden Sie zum Entwickeln einer App, die unter Windows 8.1 und Windows Phone 8.1 ausgeführt wird, Microsoft Visual Studio 2013 Update 3 oder höher, und halten Sie sich an die [hier](https://msdn.microsoft.com/library/windows/apps/Dn263168) aufgeführten Schritte. Der Hauptunterschied besteht darin, dass Sie für Windows 8.1 und Windows Phone 8.1 eine Lösung mit drei Projekten verwenden: eins für den Desktop (oder für einen Tablet PC), eins für das Smartphone und eins für gemeinsam genutzten Code. Bei der Entwicklung für Windows 10 verwendet der gesamte Code dasselbe Projekt.

Informationen zu Lernprogrammen in anderen Programmiersprachen finden Sie unter:

-   [Erstellen Ihrer ersten Windows Store-App mit JavaScript](https://msdn.microsoft.com/library/windows/apps/BR211385)

-   [Erstellen Ihrer ersten Windows Store-App mit C# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/Hh974581)

## Vorbereitung

-   Für dieses Lernprogramm benötigen Sie mindestens Visual Studio 2015 Community oder eine der Community-fremden Versionen von Visual Studio 2015 sowie einen Computer mit Windows 10 oder Windows 8.1. Informationen zum Herunterladen finden Sie unter [Herunterladen der Tools](http://go.microsoft.com/fwlink/p/?LinkId=532666).
-   Installieren Sie das entsprechende [SDK](http://go.microsoft.com/fwlink/?LinkId=533049) für die Entwicklung für die Universelle Windows-Plattform.
-   Außerdem benötigen Sie eine Entwicklerlizenz. Entsprechende Anweisungen finden Sie unter [Anfordern einer Entwicklerlizenz](https://msdn.microsoft.com/library/windows/apps/Hh974578).
-   Es wird vorausgesetzt, dass Sie über grundlegende Kenntnisse in standardmäßigem C++, XAML und den in der [XAML-Übersicht](https://msdn.microsoft.com/library/windows/apps/Mt185595) erläuterten Konzepten verfügen.
-   Wir gehen davon aus, dass Sie das Standardfensterlayout in Visual Studio verwenden. Um das Layout auf das Standardlayout zurückzusetzen, klicken Sie auf der Menüleiste auf **Fenster** > **Fensterlayout zurücksetzen**.
-   Beachten Sie, dass es ein bekanntes Problem bei Visual Studio 2015 gibt, das beim Laden des XAML-Designers zu einer Ausnahme vom Typ „NullReferenceException“ führen kann. Dieses Problem blockiert einige Schritte dieses Lernprogramms, sofern Sie die Problemumgehung nicht anwenden. Weitere Informationen zu diesem Problem und der Problemumgehung finden Sie in [diesem MSDN-Forumsbeitrag](http://go.microsoft.com/fwlink/p/?LinkId=624036) .

## Vergleich zwischen C++-Desktop-Apps und Windows-Apps

Wenn Sie bereits Windows-Desktop-Apps mit C++ programmiert haben, werden Ihnen einige Aspekte der Programmierung von Windows Store- und Windows Phone-Apps bekannt sein, einiges aber auch nicht.

### Gemeinsamkeiten

-   Sie können die STL-, CRT- (mit ein paar Ausnahmen) und jede andere C++-Bibliothek verwenden, solange der Code nicht versucht, Windows-Funktionen aufzurufen, die in der Windows-Runtime-Umgebung nicht zur Verfügung stehen.

-   Wenn Sie es gewohnt sind, visuelle Designer zu verwenden, können Sie immer noch den in Microsoft Visual Studio integrierten Designer verwenden, oder Sie können das Tool Blend für Visual Studio nutzen, das einen umfassenderen Umfang an Features bietet. Wenn Sie es gewohnt sind, UI manuell zu codieren, können Sie Ihren XAML-Code manuell programmieren.

-   Sie erstellen wie gehabt Apps, die Windows-Betriebssystemtypen und Ihre eigenen benutzerdefinierten Typen verwenden.

-   Sie verwenden weiterhin Debugger, Profiler und andere Entwicklungstools von Visual Studio.

-   Sie erstellen weiterhin Apps, die mit dem Visual C++-Compiler in systemeigenem Computercode kompiliert werden. Windows Store-Apps in C++ können in einer verwalteten Laufzeitumgebung nicht ausgeführt werden.

### Das ist neu:

-   Die Designprinzipien für Windows Store- und universelle Windows-Apps unterscheiden sich erheblich von denen für Desktop-Apps. Der Schwerpunkt liegt nicht mehr auf Fensterrahmen, Bezeichnungen, Dialogfeldern usw. Der Inhalt steht im Vordergrund. Eindrucksvolle universelle Windows-Apps folgen diesen Prinzipien schon mit Beginn der Planungsphase.

-   Die gesamte UI wird in XAML definiert. Die Trennung zwischen UI und Kernprogrammlogik ist bei Universal Windows-Apps viel eindeutiger als bei einer MFC- oder Win32-App. Designer können in der XAML-Datei am Erscheinungsbild der UI feilen, während Sie sich mit dem Verhalten in der Codedatei beschäftigen.

-   Sie programmieren in erster Linie für die Windows-Runtime – eine neue, navigationsfreundliche, objektorientierte API. Auf Windows-Geräten ist für einige Funktionen aber auch weiterhin Win32 verfügbar.

-   Sie verwenden C++/CX zum Verwenden oder Erstellen von Windows-Runtime-Objekten. C++/CX ermöglicht die Behandlung von C++-Ausnahmen, die Verwendung von Delegaten und Ereignissen sowie die automatische Verweiszählung dynamisch erstellter Objekte. Bei Verwendung von C++/CX bleibt die zugrunde liegende COM- und Windows-Architektur vor dem Code der App verborgen. Weitere Informationen finden Sie in der [Programmiersprachenreferenz für C++/CX](https://msdn.microsoft.com/en-us/library/windows/apps/hh699871.aspx).

-   Ihre App wird zu einem Paket kompiliert, das auch Metadaten zu den in der App enthaltenen Typen, den verwendeten Ressourcen und den benötigten Funktionen (Datei-, Internet- und Kamerazugriff usw.) enthält.

-   Im Windows Store und dem Windows Phone Store wird die Sicherheit Ihrer App anhand eines Zertifizierungsprozesses geprüft, und die App kann von Millionen potenzieller Kunden entdeckt werden.

## Store-App „Hello, world“ in C++

Unsere erste App ist „Hello World“. Sie veranschaulicht einige grundlegende Interaktivitätsfunktionen, Layouts und Stile. Wir erstellen eine App auf der Grundlage der Projektvorlage für universelle Windows-Apps. Wenn Sie bereits Apps für Windows 8.1 und Windows Phone 8.1 entwickelt haben, erinnern Sie sich wahrscheinlich daran, dass Sie drei Projekte in Visual Studio verwendet haben: eins für die Windows-App, eins für die Phone-App und ein weiteres mit gemeinsam genutztem Code. Die universelle Windows-Plattform (UWP) von Windows 10 ermöglicht die Verwendung eines einzelnen Projekts, das auf allen Geräten (Desktop- und Laptop-PCs mit Windows 10, Tablets, Smartphones usw.) ausgeführt werden kann.

Wir beginnen mit den Grundlagen:

-   Erstellen eines universellen Windows-Projekts in Visual Studio 2015 oder höher

-   Kennenlernen der erstellten Projekte und Dateien

-   Kennenlernen der Erweiterungen in Visual C++-Komponentenerweiterungen (C++/CX) und ihrer Verwendungsmöglichkeiten

**Erstellen einer Lösung in Visual Studio**

1.  Klicken Sie in Visual Studio auf der Menüleiste auf **Datei** > **Neu** > **Projekt**.

2.  Erweitern Sie im Dialogfeld **Neues Projekt** im linken Bereich **Installiert** > **Visual C++** > **Windows** > **Universal**.

3.  Wählen Sie im mittleren Bereich **Leere App (universelle Windows-App)** aus.

4.  Geben Sie einen Namen für das Projekt ein. Wir nennen unser Projekt „HelloWorld“.

 ![C++-Projektvorlagen im Dialogfeld „Neues Projekt“ ](images/vs2015-newuniversalproject-cpp.png)

5.  Klicken Sie auf die Schaltfläche **OK**.

   Wenn dies das erste UWP-Projekt ist, das Sie erstellt haben, und der Entwicklermodus noch nicht auf dem Computer aktiviert ist, wird das Dialogfeld „Entwicklermodus aktivieren“ angezeigt. Klicken Sie auf den Link, um die Seite „Einstellungen“ aufzurufen, auf der Sie den Entwicklermodus aktivieren können. Mit dem Entwicklermodus können Apps bereitgestellt und lokal ausgeführt werden.

   Ihre Projektdateien werden erstellt.

Werfen wir einen Blick darauf, was sich in der Lösung befindet, bevor wir fortfahren.

![Projektmappe der universellen App mit reduzierten Knoten](images/vs2015-solutionexploreruniversal-0-cpp.png)

### Informationen zu Projektdateien

Jede XAML-Datei in einem Projektordner verfügt über eine zugehörige XAML.H- und eine XAML.CPP-Datei im selben Ordner und eine G- und eine G.HPP-Datei im Ordner „Generierte Dateien“, der auf dem Datenträger vorhanden ist, jedoch nicht zum Projekt gehört. Sie können die XAML-Dateien modifizieren, um Benutzeroberflächenelemente zu erstellen und sie mit Datenquellen zu verbinden (DataBinding). Sie können die „.h“- und „.cpp“-Dateien modifizieren, um benutzerdefinierte Logik für Ereignishandler hinzuzufügen. Die automatisch erstellten Dateien stellen die Umwandlung von XAML-Markup in C++ dar. Verändern Sie diese Dateien nicht, sehen Sie sich die Dateien jedoch genauer an, um den CodeBehind besser zu verstehen. Im Grunde genommen enthält die generierte Datei eine partielle Klassendefinition für ein XAML-Stammelement. Diese Klasse ist die gleiche Klasse, die Sie in den XAML.H- und CPP-Dateien bearbeiten. Die generierten Dateien deklarieren die untergeordneten XAML-UI-Elemente als Klassenmember, sodass Sie in Ihrem Code auf sie verweisen können. Beim Erstellen des Builds werden der generierte Code und Ihr Code zu einer vollständigen Klassendefinition zusammengeführt und anschließend kompiliert.

Befassen wir uns zuerst mit den Projektdateien.

-   **App.xaml, App.xaml.h, App.xaml.cpp:** Stellen das Application-Objekt dar, das als Einstiegspunkt einer App fungiert. „App.xaml“ enthält kein seitenspezifisches UI-Markup, Sie können jedoch UI-Formate und andere Elemente hinzufügen, die auf allen Seiten verfügbar sein sollen. Die CodeBehind-Dateien enthalten Handler für die Ereignisse **OnLaunched** und **OnSuspending**. In der Regel können Sie hier benutzerdefinierten Code hinzufügen, um Ihre App zu initialisieren, wenn sie gestartet wird, und eine Bereinigung durchzuführen, wenn sie unterbrochen oder beendet wird.
-   **MainPage.xaml, MainPage.xaml.h, MainPage.xaml.cpp:** Enthalten das XAML-Markup und den CodeBehind für die standardmäßige Startseite in einer App. Sie bietet keine Unterstützung für Navigation oder integrierte Steuerelemente.
-   **pch.h, pch.cpp:** Eine vorkompilierte Headerdatei und die Datei, die sie in Ihr Projekt einfügt. In „pch.h“ können Sie alle Header einfügen, die sich nur selten ändern und sich in anderen Dateien in der Lösung befinden.
-   **Package.appxmanifest:** Eine XML-Datei, die die von Ihrer App benötigten Gerätefunktionen und die App-Versionsinformationen und andere Metadaten beschreibt. Doppelklicken Sie auf die Datei, um sie im Manifest-Designer**** zu öffnen.
-   **HelloWorld\_TemporaryKey.pfx:** Ein Schlüssel von Visual Studio, der die Bereitstellung der App auf diesem Gerät ermöglicht.

## Ein erster Blick auf den Code

Wenn Sie den Code in den Dateien „App.Xaml.h“ und „App.Xaml.cpp“ im freigegebenen Projekt untersuchen, werden Sie feststellen, dass es sich meistens um C++-Code handelt. Einige Syntaxelemente kennen Sie jedoch möglicherweise nicht, wenn Sie noch nicht mit Windows-Runtime-Apps vertraut sind oder wenn Sie mit C++/CLI gearbeitet haben. Die häufigsten nicht standardmäßigen Syntaxelemente, die in C++/CX auftauchen, sind die folgenden:

-   **Referenzklassen**

Nahezu alle Windows-Runtime-Klassen, zu denen alle Typen in der Windows-API zählen (XAML-Steuerelemente, die Seiten in Ihrer App, die App-Klasse selbst, alle Geräte- und Netzwerkobjekte sowie alle Containertypen), werden als **ref class** deklariert. (Einige Windows-Typen werden als **value class** oder **value struct** deklariert.) Eine Referenzklasse (ref class) kann von beliebigen Programmiersprachen verwendet werden. In C++ wird der Lebenszyklus dieser Typen von der automatischen Verweiszählung (nicht Garbage Collection) bestimmt, sodass Sie diese Objekte niemals explizit löschen. Sie können auch Ihre eigenen Referenzklassen erstellen.

```cpp
    namespace HelloWorld
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public ref class MainPage sealed
        {
        public:
            MainPage();

        };
    }
```    

Alle Windows-Runtime-Typen müssen innerhalb eines Namespace deklariert sein, und anders als bei ISO C++ haben die Typen selbst einen Zugriffsmodifikator. Der Modifikator **public** macht die Klasse für Komponenten für Windows-Runtime außerhalb des Namespace sichtbar. Das Schlüsselwort **sealed** bedeutet, dass die Klasse nicht als Basisklasse dienen kann. Nahezu alle Referenzklassen sind „sealed“. Klassenvererbung wird häufig nicht verwendet, da JavaScript sie nicht versteht.

-   **ref new** und **^ (Hütchen)**

 Sie können eine Variable einer Referenzklasse deklarieren, indem Sie den Operator ^ (Hütchen) verwenden, und Sie können das Objekt mit dem Schlüsselwort „ref new“ instanziieren. Danach greifen Sie auf die Instanzmethoden des Objekts mit dem Operator -> zu, wie bei einem C++-Zeiger. Auf statische Methoden kann mit dem Operator :: zugegriffen werden, genau wie in ISO C++.

 Im folgenden Code verwenden wir den vollständig qualifizierten Namen, um ein Objekt zu instanziieren, und den Operator „->“, um eine Instanzmethode aufzurufen.

 ```cpp
    Windows::UI::Xaml::Media::Imaging::BitmapImage^ bitmapImage =
        ref new Windows::UI::Xaml::Media::Imaging::BitmapImage();
      
    bitmapImage->SetSource(fileStream);
    ```

   Typically, in a .cpp file we would add a `using namespace  Windows::UI::Xaml::Media::Imaging` directive and the auto keyword, so that the same code would look like this:

```cpp
    auto bitmapImage = ref new BitmapImage();
    bitmapImage->SetSource(fileStream);
```

-   **Eigenschaften**

   Eine Referenzklasse kann Eigenschaften besitzen, die (ebenso wie bei verwalteten Sprachen) spezielle Memberfunktionen darstellen, die für verwendenden Code als Felder erscheinen.

```cpp
    public ref class SaveStateEventArgs sealed
            {
            public:

                // Declare the property
                property Windows::Foundation::Collections::IMap<Platform::String^, Platform::Object^>^ PageState
                {
                    Windows::Foundation::Collections::IMap<Platform::String^, Platform::Object^>^ get();
                }
    ...
    };

    ...
    // consume the property like a public field
    void PhotoPage::SaveState(Object^ sender, Common::SaveStateEventArgs^ e)
    {    
        if (mruToken != nullptr && !mruToken->IsEmpty())
        {
            e->PageState->Insert("mruToken", mruToken);
        }
    }
```

-   **Delegaten**

   Ebenso wie bei verwalteten Sprachen stellt ein Delegat einen Referenztyp dar, der eine Funktion mit einer bestimmten Signatur umschließt. Sie kommen meistens mit Ereignissen und Ereignishandlern zum Einsatz.

```cpp
    // Delegate declaration (within namespace scope)
    public delegate void LoadStateEventHandler(Platform::Object^ sender, LoadStateEventArgs^ e);

    // Event declaration (class scope)
    public ref class NavigationHelper sealed
    {
      public:
        event LoadStateEventHandler^ LoadState;
    };

    // Create the event handler in consuming class
    MainPage::MainPage()
    {
        auto navigationHelper = ref new Common::NavigationHelper(this);
        navigationHelper->LoadState += ref new Common::LoadStateEventHandler(this, &MainPage::LoadState);
    }
```

## Hinzufügen von Inhalt zur App

Lassen Sie uns der App einige Inhalte hinzufügen.

**Schritt 1: Anpassen der Startseite**

1.  Öffnen Sie im Projektmappen-Explorer ****die Datei „MainPage.xaml.cs“.
2.  Erstellen Sie Steuerelemente für die Benutzeroberfläche, indem Sie den folgenden XAML-Code direkt vor dem schließenden Tag zum [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)-Stammelement hinzufügen. Er enthält ein [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635) mit einem [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652), in dem der Benutzer zur Eingabe seines Namens aufgefordert wird, ein [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683)-Element, in das der Name eingegeben wird, sowie ein [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)- und ein weiteres **TextBlock**-Element.

```xml
    <StackPanel x:Name="contentPanel" Margin="120,30,0,0">
        <TextBlock HorizontalAlignment="Left" Text="Hello World" FontSize="36"/>
        <TextBlock Text="What's your name?"/>
        <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
            <TextBox x:Name="nameInput" Width="300" HorizontalAlignment="Left"/>
            <Button x:Name="inputButton" Content="Say \"Hello\""/>
        </StackPanel>
        <TextBlock x:Name="greetingOutput"/>
    </StackPanel>
```

Auf das XAML-Layout gehen wir im Artikel [Navigation, Layout und Ansichten](https://msdn.microsoft.com/library/windows/apps/Dn263172) noch genauer ein.

3.  Sie haben nun eine sehr einfache universelle Windows-App erstellt. Falls Sie wissen möchten, wie die UWP-App aussieht, drücken Sie F5, um die App zu erstellen, bereitzustellen und im Debugmodus auszuführen.

Zunächst erscheint der standardmäßige Begrüßungsbildschirm. Er setzt sich aus einem Bild (Assets\\SplashScreen.scale-100.png) und einer Hintergrundfarbe zusammen, die in der Manifestdatei der App angegeben sind. Weitere Informationen zur Anpassung des Begrüßungsbildschirms finden Sie unter [Hinzufügen eines Begrüßungsbildschirms](https://msdn.microsoft.com/library/windows/apps/Hh465332).

Wenn der Begrüßungsbildschirm verschwindet, wird Ihre App angezeigt. Die Hauptseite der App angezeigt.

Drücken Sie die WINDOWS-TASTE, oder klicken Sie auf die Schaltfläche „Start“, um zum Startmenü zu wechseln. Beachten Sie, dass durch die Bereitstellung der App sie in der Liste installierter Apps im Startmenü hinzugefügt wird. Außerdem wird sie angezeigt, wenn Sie neben der Schaltfläche „Alle“ auf den Link „Neu“ klicken. Tippen Sie oder klicken Sie zum erneuten Ausführen der App auf die Kachel, oder drücken Sie wie gewohnt F5 oder STRG+F5 in Visual Studio.

 ![Windows Store-App-Bildschirm mit Steuerelementen](images/xaml-hw-app2.png)

   Viel zu bieten hat sie zwar noch nicht, aber trotzdem: Herzlichen Glückwunsch! Sie haben Ihre erste UWP-App erstellt!

   Kehren Sie zu Visual Studio zurück, und drücken Sie Umschalt+F5, um den Debugmodus zu beenden und die App zu schließen.

   Weitere Informationen finden Sie unter [Ausführen einer Store-App über Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=619619).

   In der App können Sie etwas in das [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683)-Element eingeben, das Klicken auf [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) hat jedoch keinerlei Auswirkung. In späteren Schritten erstellen wir daher einen Ereignishandler für das [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737)-Ereignis der Schaltfläche, um eine personalisierte Begrüßung einzublenden.

## Starten der App in einem Emulator für mobile Geräte


Ihre App kann auf jedem beliebigen Windows 10-Gerät ausgeführt werden. Im Anschluss sehen wir uns an, wie sie auf einem Windows Phone dargestellt wird. In diesem Abschnitt wird ein Windows Phone mit Windows 10 oder Zugriff auf einen Windows Phone-Emulator benötigt. Außerdem muss Visual Studio auf einem physischen Computer (nicht auf einem virtuellen Computer) ausgeführt werden, auf dem Hyper-V unterstützt und aktiviert ist.

Zusätzlich zu den Optionen zum Debuggen auf einem Desktopgerät enthält Visual Studio Optionen zum Bereitstellen und Debuggen Ihrer App auf einem physischen mobilen Gerät, das an den Computer angeschlossen ist, oder in einem Emulator für mobile Geräte. Sie können zwischen Emulatoren für Geräte mit unterschiedlichen Arbeitsspeicher- und Bildschirmkonfigurationen wählen.

-   **Gerät**
-   **Emulator 10.0.0.0 WVGA 4 Zoll 512 MB**
-   Verschiedene Emulatoren mit anderen Konfigurationen

Es empfiehlt sich, Ihre App auf einem Gerät mit kleinem Bildschirm und begrenztem Arbeitsspeicher zu testen. Wählen Sie also die Option **Emulator 10.0.0.0 WVGA 4 Zoll 512 MB** aus.
**Tipp:** Weitere Informationen zur Verwendung des Phone-Emulators finden Sie unter [Ausführen von Windows Phone-Apps im Emulator](http://go.microsoft.com/fwlink/p/?LinkId=394233).

 

Um die App für ein physisches Gerät zu debuggen, benötigen Sie ein für die Entwicklung registriertes Gerät. Weitere Informationen erhalten Sie unter [Registrieren Ihres Windows Phone-Geräts](https://msdn.microsoft.com/library/windows/apps/Dn614128).

**So beginnen Sie mit dem Debuggen in einem Emulator für mobile Geräte**

1.  Wählen Sie auf der Standardsymbolleiste**** im Menü mit den Zielgeräten (![Menü „Debuggen starten“](images/startdebug-full.png)) die Option **Emulator 10.0.0.0 WVGA 4 Zoll 512 MB** aus.
2.  Klicken Sie auf der Symbolleiste auf die Schaltfläche **Debuggen starten** (![Schaltfläche „Debuggen starten“](images/startdebug-sm.png)).

   oder

   Klicken Sie im Menü **Debuggen** auf **Debuggen starten**.

   oder

   Drücken Sie F5.

Im Emulator für mobile Geräte sieht die App wie folgt aus:

![Erster App-Bildschirm auf dem mobilen Gerät](images/hw10-screen1-mob.png)

Visual Studio startet den ausgewählten Emulator und stellt die App bereit und startet sie. Als Erstes wird Ihnen auffallen, dass der linke Rand mit einer Breite von 120 Pixel, der auf dem lokalen Computer gut aussieht, Ihren Inhalt auf dem mobilen Gerät aufgrund des kleineren Bildschirms aus dem sichtbaren Bereich verschiebt. Später in diesem Lernprogramm erfahren Sie, wie Sie die UI an unterschiedliche Bildschirmgrößen anpassen, damit die App immer gut aussieht.

## Schritt 2: Erstellen eines Ereignishandlers

1.  Wählen Sie in „MainPage.xaml“ entweder in der XAML- oder in der Entwurfsansicht das [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)-Element „Say Hello“ aus dem zuvor hinzugefügten [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635)-Element aus.
2.  Öffnen Sie durch Drücken von ALT-EINGABETASTE das Eigenschaftenfenster****, und wählen Sie anschließend die Ereignisschaltfläche (![Ereignisschaltfläche](IMAGES/EVENTSBUTTON.png)) aus.
3.  Suchen Sie das [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737)-Ereignis. Geben Sie im Textfeld den Namen der Funktion ein, die das **Click**-Ereignis behandelt. Geben Sie für dieses Beispiel „Button\_Click“ ein.

![Eigenschaftenfenster, Ereignisansicht](images/xaml-hw-event.png)

4.  Drücken Sie die EINGABETASTE. Die Ereignishandlermethode wird in „MainPage.xaml.cpp“ erstellt und im Code-Editor geöffnet, sodass Sie den Code hinzufügen können, der beim Auftreten des Ereignisses ausgeführt werden soll.

   Gleichzeitig wird in „MainPage.xaml“ der XAML-Code für das [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)-Element aktualisiert, um den [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737)-Ereignishandler wie folgt zu deklarieren:

```xml
    <Button Content="Say \"Hello\"" Click="Button_Click"/>
```

Sie können dies auch einfach manuell zum XAML-Code hinzufügen, was vor allem dann hilfreich ist, wenn der Designer nicht geladen wird. Wenn Sie dies manuell eingeben, geben Sie „Click“ ein, und warten Sie, bis IntelliSense die Option zum Hinzufügen eines neuen Ereignishandlers anzeigt. Auf diese Weise erstellt Visual Studio die erforderliche Methodendeklaration und den erforderlichen Stub.

Im Designer tritt ein Fehler beim Laden auf, wenn eine unbehandelte Ausnahme während des Renderns auftritt. Für das Rendern im Designer muss eine Entwurfszeitversion der Seite ausgeführt werden. Es ist möglicherweise hilfreich, den ausgeführten Benutzercode zu deaktivieren. Ändern Sie dazu die Einstellung im Dialogfeld **Tools, Optionen**. Deaktivieren Sie unter **XAML-Designer** die Option **Projektcode im XAML-Designer ausführen (falls unterstützt)**.

5.  Fügen Sie dem gerade erstellten **Button\_Click**-Ereignishandler in „MainPage.xaml.cpp“ den folgenden Code hinzu. Dieser Code ruft den Namen des Benutzers aus dem [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683)-Steuerelement `nameInput` ab und erstellt damit eine Begrüßung. Das [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)-Element `greetingOutput` zeigt das Ergebnis an.

```cpp
    void HelloWorld::MainPage::Button_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
    {
        greetingOutput->Text = "Hello, " + nameInput->Text + "!";
    }
```

6.  Legen Sie das Projekt als Startprojekt fest, und drücken Sie anschließend F5, um die App zu erstellen und auszuführen. Wenn Sie einen Namen in das Textfeld eingeben und anschließend auf die Schaltfläche klicken, zeigt die App eine personalisierte Begrüßung an.

![App-Bildschirm mit angezeigter Meldung](images/xaml-hw-app4.png)

## Schritt 3: Gestalten der Startseite

### Auswählen eines Designs

Das Erscheinungsbild Ihrer App lässt sich ganz einfach anpassen. Standardmäßig verwendet die App Ressourcen mit hellem Design. Die Systemressourcen enthalten auch ein helles Design. Probieren wir doch einmal aus, wie das aussieht.

**So wechseln Sie zum dunklen Design**

1.  Öffnen Sie „App.xaml“.
2.  Bearbeiten Sie im öffnenden [**Application**](https://msdn.microsoft.com/library/windows/apps/BR242324)-Tag die [**RequestedTheme**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.requestedtheme)-Eigenschaft, und legen Sie ihren Wert auf **Dark** fest:

```xml
   RequestedTheme="Light"
```

Hier sehen Sie das gesamte [**Application**](https://msdn.microsoft.com/library/windows/apps/BR242324)-Tag mit dunklem Design:

```xml 
        <Application
        x:Class="HelloWorld.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:HelloWorld" 
        RequestedTheme="Dark">
```

3.  Drücken Sie F5, um die App zu erstellen und auszuführen. Wie Sie sehen, wird nun das dunkle Design verwendet.

![App-Bildschirm mit dunklem Design](images/xaml-hw-app3.png)

Welches Design sollten Sie verwenden? Das bleibt ganz Ihnen überlassen. Bei Apps, die hauptsächlich Bilder oder Videos anzeigen, sollten Sie das dunkle Design verwenden, während sich bei Apps mit viel Text die Verwendung des hellen Designs empfiehlt. Falls Sie ein benutzerdefiniertes Farbschema verwenden, wählen Sie das Design, das am besten zum Erscheinungsbild Ihrer App passt. Im verbleibenden Teil dieses Lernprogramms verwenden wir in Screenshots das helle Design.

**Hinweis:** Das Design wird beim Aktivieren der App angewendet und kann nicht geändert werden, während die App ausgeführt wird.

### Verwenden von Systemstilen

Momentan ist der Text in der Windows-App ziemlich klein und nur schwer lesbar. Lassen Sie uns dies ändern, indem wir einen Systemstil anwenden.

**So ändern Sie den Stil eines Elements**

1.  Öffnen Sie „MainPage.xaml“ im Windows-Projekt.
2.  Wählen Sie in der XAML- oder Entwurfsansicht das von Ihnen hinzugefügte [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)-Element „What’s your name?“ aus.
3.  Wählen Sie im Dialogfeld **Eigenschaften** (F4****) rechts oben die Schaltfläche „Eigenschaften“ (![Schaltfläche „Eigenschaften“](IMAGES/PROPERTIESBUTTON.png)) aus.
4.  Erweitern Sie die Gruppe **Text** , und legen Sie den Schriftgrad auf „18 px“ fest.
5.  Erweitern Sie die Gruppe **Sonstiges**, und suchen Sie dort nach der Eigenschaft **Style**.
6.  Klicken Sie auf den Eigenschaftenmarker (das grüne Feld rechts neben der Eigenschaft **Style**), und wählen Sie anschließend im Menü **Systemressource** > **BaseTextBlockStyle** aus.

 **BaseTextBlockStyle** ist eine im [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794)-Element definierte Ressource. Dieses Element befindet sich an folgendem Speicherort: <root>\\Programme\\Windows Kits\\10\\Include\\winrt\\xaml\\design\\generic.xaml.

![Eigenschaftenfenster, Eigenschaftenansicht](images/xaml-hw-style-cpp.png)

 Auf der XAML-Entwurfsoberfläche ändert sich die Textdarstellung. Im XAML-Editor wird der XAML-Code für [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) aktualisiert:

```xml
   <TextBlock Text="What's your name?" Style="{StaticResource BasicTextStyle}"/><
```

7.  Wiederholen Sie den Vorgang, um den Schriftgrad festzulegen und **BaseTextBlockStyle** dem [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)-Element `greetingOutput` zuzuweisen.

  **Tipp:** Obwohl sich in diesem [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)-Element kein Text befindet, zeigt eine blaue Umrandung seine Position an, wenn Sie den Mauszeiger über die XAML-Entwurfsoberfläche bewegen, sodass Sie ihn auswählen können.  

  Ihr XAML-Code sieht nun so aus:

```xml
    <StackPanel x:Name="contentPanel" Margin="120,30,0,0">
        <TextBlock Style="{ThemeResource BaseTextBlockStyle}" FontSize="16" Text="What's your name?"/>
        <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
            <TextBox x:Name="nameInput" Width="300" HorizontalAlignment="Left"/>
            <Button x:Name="inputButton" Content="Say \"Hello\"" Click="Button_Click"/>
        </StackPanel>
        <TextBlock Style="{ThemeResource BaseTextBlockStyle}" FontSize="16" x:Name="greetingOutput"/>
    </StackPanel>
```

8.  Drücken Sie F5, um die App zu erstellen und auszuführen. Sie sieht jetzt so aus:

 ![App-Bildschirm mit größerem Text](images/xaml-hw-app5.png)

### Schritt 4: Anpassen der UI an verschiedene Fenstergrößen

Als Nächstes sorgen wir dafür, dass sich die UI verschiedenen Bildschirmgrößen anpasst, damit sie auch auf mobilen Geräten gut aussieht. Dazu fügen Sie ein [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021)-Element hinzu und legen Eigenschaften fest, die für die unterschiedlichen Ansichtszustände angewendet werden.

**So passen Sie das UI-Layout an**

1.  Fügen Sie im XAML-Editor nach dem Starttag des [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)-Stammelements den folgenden XAML-Block hinzu:

```xml
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="wideState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="641" />
                </VisualState.StateTriggers>
            </VisualState>
            <VisualState x:Name="narrowState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="0" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="contentPanel.Margin" Value="20,30,0,0"/>
                    <Setter Target="inputPanel.Orientation" Value="Vertical"/>
                    <Setter Target="inputButton.Margin" Value="0,4,0,0"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
```

2.  Debuggen Sie die App auf dem lokalen Computer. Sie sehen, dass die UI genauso aussieht wie vorher, es sei denn, die Fensterbreite beträgt weniger als 641 DIP (geräteunabhängige Pixel).
3.  Debuggen Sie die App auf dem Emulator für mobile Geräte. Beachten Sie, dass für die UI die Eigenschaften verwendet werden, die Sie unter `narrowState` definiert haben, und dass die Benutzeroberfläche auf dem kleinen Bildschirm korrekt angezeigt wird.

![Bildschirm der mobilen App mit Textdesign](images/hw10-screen2-mob.png)

Wenn Sie bereits in früheren Versionen von XAML ein [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021)-Element genutzt haben, fällt Ihnen vielleicht auf, dass hier für den XAML-Code eine vereinfachte Syntax verwendet wird.

Das [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007)-Element mit dem Namen `wideState` verfügt über ein [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn890382)-Element, für das die [**MinWindowWidth**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.adaptivetrigger.minwindowwidth)-Eigenschaft auf den Wert 641 festgelegt ist. Das bedeutet, dass der Zustand nur angewendet wird, wenn die Fensterbreite nicht geringer als die Mindestgröße von 641 DIP ist. Da Sie keine [**Setter**](https://msdn.microsoft.com/library/windows/apps/BR208817)-Objekte für diesen Zustand definieren, werden die Layouteigenschaften verwendet, die Sie im XAML-Code für den Seiteninhalt festgelegt haben.

Das zweite [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007)-Element `narrowState` verfügt über ein [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn890382)-Element, für das die [**MinWindowWidth**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.adaptivetrigger.minwindowwidth)-Eigenschaft auf 0 festgelegt ist. Dieser Zustand wird angewendet, wenn die Fensterbreite größer 0, aber kleiner als 641 DIP ist. (Bei 641 DIP wird `wideState` angewendet.) In diesem Zustand definieren Sie einige [**Setter**](https://msdn.microsoft.com/library/windows/apps/BR208817)-Objekte, um die Layouteigenschaften der Steuerelemente in der UI zu ändern:

-   Verringern Sie den linken Rand des `contentPanel`-Elements von 120 auf 20.
-   Ändern Sie das [**Orientation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.orientation)-Element des `inputPanel`-Elements von **Horizontal** in **Vertical**.
-   Fügen Sie dem `inputButton`-Element einen oberen Rand mit einer Breite von 4 DIP hinzu.

### Zusammenfassung

Herzlichen Glückwunsch! Sie haben das erste Lernprogramm abgeschlossen. Darin haben Sie gelernt, wie Sie Inhalte zu universellen Windows-Apps hinzufügen, wie Sie sie interaktiv machen und wie Sie ihr Erscheinungsbild ändern.

## Nächste Schritte

Wenn Sie ein Projekt für universelle Windows-Apps für Windows 8.1 und/oder Windows Phone 8.1 besitzen, können Sie es zu Windows 10 portieren. Es gibt keinen automatischen Prozess dafür, Sie können das Projekt jedoch ohne großen Aufwand manuell portieren. Beginnen Sie mit einem neuen universellen Windows-Projekt, um die aktuelle Projektsystemstruktur und Manifestdateien abzurufen. Kopieren Sie dann die Codedateien in die Verzeichnisstruktur des Projekts, fügen Sie Ihrem Projekt Elemente hinzu, und schreiben Sie Ihren XAML-Code mithilfe von [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021) gemäß den Anweisungen in diesem Thema um. Weitere Informationen finden Sie unter [Portieren eines Windows-Runtime 8-Projekts zu einem UWP-Projekt (Universelle Windows-Plattform)](https://msdn.microsoft.com/library/windows/apps/Mt188203) sowie unter [Portieren zur Universellen Windows-Plattform (C++)](http://go.microsoft.com/fwlink/p/?LinkId=619525).

Wenn Sie über C++-Code verfügen, den Sie mit einer UWP-App integrieren möchten, um beispielsweise eine neue UWP-Benutzeroberfläche für eine vorhandene Anwendung zu erstellen, finden Sie unter [Gewusst wie: Verwenden von vorhandenem C++-Code in einem universellen Windows-Projekt](http://go.microsoft.com/fwlink/p/?LinkId=619623) entsprechende Anweisungen dazu.



<!--HONumber=Mar16_HO1-->


