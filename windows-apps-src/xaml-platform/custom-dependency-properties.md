---
author: jwmsft
description: "Hier wird erläutert, wie Sie benutzerdefinierte Abhängigkeitseigenschaften für eine Windows-Runtime-App mit C++, C# oder Visual Basic definieren und implementieren können."
title: "Benutzerdefinierte Abhängigkeitseigenschaften"
ms.assetid: 5ADF7935-F2CF-4BB6-B1A5-F535C2ED8EF8
translationtype: Human Translation
ms.sourcegitcommit: 5efe261bf504d0d77518b7a5393927d168234907
ms.openlocfilehash: 09bf5fdb76bcc3210d822b769061b900b51a9cb2

---

# Benutzerdefinierte Abhängigkeitseigenschaften

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Hier wird erläutert, wie Sie eigene Abhängigkeitseigenschaften für eine Windows-Runtime-App mit C++, C# oder Visual Basic verwenden können. Wir zählen mögliche Gründen für die Erstellung benutzerdefinierter Abhängigkeitseigenschaften durch App-Entwickler und Komponentenautoren auf. Des Weiteren beschreiben wir die Implementierungsschritte für eine benutzerdefinierte Abhängigkeitseigenschaft sowie einige bewährte Methoden, die die Leistung, Benutzerfreundlichkeit und Vielseitigkeit der Abhängigkeitseigenschaft verbessern.

## Voraussetzungen


Wir gehen davon aus, dass Sie die [Übersicht über Abhängigkeitseigenschaften](dependency-properties-overview.md) bereits gelesen haben und dass Sie Abhängigkeitseigenschaften aus der Perspektive eines Konsumenten von bestehenden Abhängigkeitseigenschaften verstehen. Für ein besseres Verständnis der in diesem Thema aufgeführten Beispiele sollten Sie XAML verstehen und wissen, wie eine einfache Windows-Runtime-App mit C++, C# oder Visual Basic geschrieben wird.

## Was ist eine Abhängigkeitseigenschaft?


Um Formate, Datenbindung, Animationen und Standardwerte für eine Eigenschaft zu unterstützen, sollte sie als Abhängigkeitseigenschaft implementiert werden. Werte der Abhängigkeitseigenschaft werden nicht als Felder in der Klasse gespeichert, sondern sie werden vom XAML-Framework gespeichert und mit einem Schlüssel verwiesen, der abgerufen wird, wenn die Eigenschaft durch Aufruf der [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)-Methode im Windows-Runtime-Eigenschaftensystem registriert wird.   Abhängigkeitseigenschaften können nur von Typen verwendet werden, die von [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) abgeleitet sind. Das **DependencyObject** befindet sich jedoch ziemlich weit oben in der Klassenhierarchie, sodass die Mehrzahl der Klassen, die für die UI- und Darstellungsunterstützung bestimmt sind, Abhängigkeitseigenschaften unterstützen können. Weitere Informationen zu Abhängigkeitseigenschaften und für die in dieser Dokumentation verwendeten Begriffe und Konventionen finden Sie unter [Übersicht über Abhängigkeitseigenschaften](dependency-properties-overview.md).

Beispiele für Abhängigkeitseigenschaften in der Windows-Runtime sind: [**Control.Background**](https://msdn.microsoft.com/library/windows/apps/br209395), [**FrameworkElement.Width**](https://msdn.microsoft.com/library/windows/apps/br208751), [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/br209702) und viele weitere.

Gemäß der Konvention besitzt jede durch eine Klasse verfügbar gemachte Abhängigkeitseigenschaft eine entsprechende **public static readonly**-Eigenschaft des Typs [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362), die für die gleiche Klasse verfügbar gemacht wird, die den Bezeichner für die Abhängigkeitseigenschaft bereitstellt. Für die Benennung des Bezeichners wird folgende Konvention verwendet: der Name der Abhängigkeitseigenschaft mit der am Namensende angehängten Zeichenfolge „Property“. Beispielsweise ist der **DependencyProperty**-Bezeichner für die Eigenschaft **Control.Background** [**Control.BackgroundProperty**](https://msdn.microsoft.com/library/windows/apps/br209396). Der Bezeichner speichert die Informationen zur Abhängigkeitseigenschaft, die bei der Registrierung gelten, und kann anschließend für andere Vorgänge verwendet werden, welche die Abhängigkeitseigenschaft betreffen, wie z. B. der Aufruf von [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361).

##  Eigenschaftenwrapper

Abhängigkeitseigenschaften haben für gewöhnlich eine Wrapper-Implementierung. Ohne den Wrapper können die Eigenschaften nur durch die Verwendung der Methoden der Abhängigkeitseigenschafts-Hilfsprogramme [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) und [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) sowie durch die Nutzung des Bezeichners als Parameter abgerufen oder festgelegt werden. Dies handelt es sich um eine ziemlich ungewöhnliche Verwendung für eine offensichtliche Eigenschaft. Mit dem Wrapper können Ihr Code und jegliche anderen Codes, die auf die Abhängigkeitseigenschaft verweisen, eine unkomplizierte, für die von Ihnen verwendete Sprache natürliche Syntax für Objekteigenschaften verwenden.

Wenn Sie eine benutzerdefinierte Abhängigkeitseigenschaft selbst implementieren und diese veröffentlichen und leicht auffindbar machen möchten, definieren Sie auch die Eigenschaftenwrapper. Eigenschaftenwrapper sind des Weiteren nützlich für die Berichterstattung über grundlegende Informationen der Abhängigkeitseigenschaft für Betrachtungen und statische Analysen. Der Wrapper befindet sich insbesondere dort, wo Sie Attribute platzieren, beispielsweise [**ContentPropertyAttribute**](https://msdn.microsoft.com/library/windows/apps/br228011).

## Wann sollten Sie eine Eigenschaft als Abhängigkeitseigenschaft implementieren?

Wenn Sie eine öffentliche Lese-/Schreibeigenschaft in eine Klasse implementieren und die Klasse von [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) abgeleitet ist, können Sie Ihre Eigenschaft als Abhängigkeitseigenschaft nutzen. In einigen Fällen ist die typische Methode, Ihre Eigenschaft mit einem privaten Feld abzusichern, angemessen. Das Definieren Ihrer benutzerdefinierten Eigenschaft als Abhängigkeitseigenschaft ist nicht immer notwendig oder angemessen. Die Entscheidung hängt von den Szenarien ab, die Ihre Eigenschaft unterstützen soll.

Sie können Ihre Eigenschaft als Abhängigkeitseigenschaft implementieren, wenn diese mindestens eine der folgenden Eigenschaften der Windows-Runtime oder von Windows-Runtime-Apps unterstützen soll:

-   Festlegen der Eigenschaft über einen [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849)
-   Funktion als gültige Zieleigenschaft für Datenbindung mit [**{Binding}**](binding-markup-extension.md)
-   Unterstützung animierter Werte durch ein [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490)
-   Melden von Änderungen des Eigenschaftswerts durch:
    -   Vom Eigenschaftensystem selbst durchgeführte Aktionen
    -   Die Umgebung
    -   Benutzeraktionen
    -   Lese- und Schreibstile

## Prüfliste für die Definition einer Abhängigkeitseigenschaft

Das Definieren einer Abhängigkeitseigenschaft umfasst mehrere Konzepte. Bei diesen Begriffen handelt es sich nicht unbedingt um einzelne Schritte, da einige Schritte bei der Implementierung im Code in einzelnen Codezeilen zusammengefasst werden: Diese Liste verschafft Ihnen einen schnellen Überblick.. Wir werden später noch genauer auf jeden Begriff eingehen und Ihnen Beispielcode in verschiedenen Sprachen zeigen.

-   Registrieren Sie den Eigenschaftennamen im Eigenschaftensystem (durch Aufruf von [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)), indem Sie einen Besitzertyp und den Typ des Eigenschaftswerts angeben. 
    -  Es gibt auch einen erforderlichen Parameter für [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829), der Eigenschaftenmetadaten benötigt. Geben Sie **null** dafür an, oder wenn Sie PropertyChanged-Verhalten oder einen metadatenbasierten Standardwert haben möchten, der durch den Aufruf von [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357) wiederhergestellt werden kann, geben Sie eine Instanz von [**PropertyMetadata**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.propertymetadata) an.
-   Definieren Sie einen [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362)-Bezeichner als ein **public static readonly**-Eigenschaftenmember des Besitzertyps.
-   Definieren Sie eine Wrappereigenschaft nach dem Accessor-Modell für Eigenschaften, das in der von Ihnen implementierten Sprache verwendet wird. Der Name der Wrappereigenschaft muss mit der Zeichenfolge *string* übereinstimmen, die Sie in [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) verwendet haben. Implementieren Sie die **get**- und **set**-Accessoren, um den Wrapper durch den Aufruf von [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) und [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) und die Übergabe des Bezeichners Ihrer Eigenschaft als Parameter mit der von diesem umschlossenen Abhängigkeitseigenschaft zu verbinden.
-   (Optional) Platzieren Sie Attribute wie [**ContentPropertyAttribute**](https://msdn.microsoft.com/library/windows/apps/br228011) auf dem Wrapper.

**Hinweis**  Wenn Sie eine benutzerdefinierte angefügte Eigenschaft definieren, wird der Wrapper im Allgemeinen ausgelassen. Stattdessen verwenden Sie einen anderen Accessor-Stil, den ein XAML-Prozessor verwenden kann. Siehe [Benutzerdefinierte angefügte Eigenschaften](custom-attached-properties.md). 

## Registrieren der Eigenschaft

Damit Ihre Eigenschaft zu einer Abhängigkeitseigenschaft wird, müssen Sie die Eigenschaft in einem Eigenschaftenspeicher registrieren, der vom Windows-Runtime-Eigenschaftensystem verwaltet wird.  Rufen Sie für die Registrierung der Eigenschaft die [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)-Methode auf.

Im Fall von Microsoft .NET-Sprachen (C# und Microsoft Visual Basic) rufen Sie [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) im Text Ihrer Klasse auf (innerhalb der Klasse, jedoch außerhalb der Memberdefinitionen). Der Bezeichner wird vom [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)-Methodenaufruf als Rückgabewert bereitgestellt. Der [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)-Aufruf erfolgt in der Regel als statischer Konstruktor oder als Teil der Initialisierung einer **public static readonly**-Eigenschaft des Typs [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) als Teil der Klasse. Diese Eigenschaft macht den Bezeichner für Ihre Abhängigkeitseigenschaft verfügbar. Im Anschluss finden Sie Beispiele für den [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)-Aufruf.

> [!div class="tabbedCodeSnippets"]
```csharp
public static readonly DependencyProperty LabelProperty = DependencyProperty.Register(
  "Label",
  typeof(String),
  typeof(ImageWithLabelControl),
  new PropertyMetadata(null)
);
```
```vb
Public Shared ReadOnly LabelProperty As DependencyProperty = 
    DependencyProperty.Register("Label", 
      GetType(String), 
      GetType(ImageWithLabelControl), 
      New PropertyMetadata(Nothing))
```

**Hinweis** Das Registrieren der Abhängigkeitseigenschaft als Teil der Bezeichnereigenschaftendefinition ist gängigste Art der Implementierung. Sie können eine Abhängigkeitseigenschaft jedoch auch im statischen Konstruktor der Klasse registrieren. Dieser Ansatz eignet sich ggf., wenn Sie mehr als eine Codezeile benötigen, um die Abhängigkeitseigenschaft zu initialisieren.

Im Fall von C++ haben Sie verschiedene Möglichkeiten für die Aufteilung der Implementierung auf die Kopfzeile und die Codedatei. In der Regel wird der Bezeichner selbst als **public static**-Eigenschaft in der Kopfzeile mit einer **get**-Implementierung, jedoch ohne **set**-Implementierung deklariert. Die **get**-Implementierung bezieht sich auf ein privates Feld, das eine nicht initialisierte [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362)-Instanz ist. Sie können darüber hinaus die Wrapper und die **get**- und **set**-Implementierungen des Wrappers deklarieren. In diesem Fall enthält der Wrapper eine minimale Implementierung. Wenn der Wrapper eine Zuordnung zur Windows-Runtime benötigt, führen Sie die Zuordnung auch in der Kopfzeile durch. Platzieren Sie den [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)-Aufruf in die Codedatei innerhalb einer Hilfsfunktion, die nur bei der ersten Initialisierung der App ausgeführt wird. Verwenden Sie den Rückgabewert von **Register**, um die statischen, jedoch nicht initialisierten Bezeichner zu füllen, die Sie in der Kopfzeile deklariert haben und zunächst im Stammbereich der Implementierungsdatei auf **nullptr** festgelegt haben.

```cpp
//.h file
//using namespace Windows::UI::Xaml::Controls;
//using namespace Windows::UI::Xaml::Interop;
//using namespace Windows::UI::Xaml;
//using namespace Platform;

public ref class ImageWithLabelControl sealed : public Control
{  
private:
    static DependencyProperty^ _LabelProperty;
...
public:
    static void RegisterDependencyProperties(); 
    static property DependencyProperty^ LabelProperty
    {
        DependencyProperty^ get() {return _LabelProperty;}
    }
...
};
```

```cpp
//.cpp file
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml.Interop;

DependencyProperty^ ImageWithLabelControl::_LabelProperty = nullptr;

// This function is called from the App constructor in App.xaml.cpp 
// to register the properties
void ImageWithLabelControl::RegisterDependencyProperties() 
{ 
    if (_LabelProperty == nullptr) 
    { 
        _LabelProperty = DependencyProperty::Register(
          "Label", Platform::String::typeid, ImageWithLabelControl::typeid, nullptr); 
    } 
}
```

**Hinweis**Im Fall von C++-Code sind ein privates Feld und eine öffentliche schreibgeschützte Eigenschaft vorhanden, die [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) einblendet, sodass andere Aufrufer, die Ihre Abhängigkeitseigenschaft verwenden, auch Eigenschaftensystem-Hilfsprogramm-APIs nutzen können, für die der Bezeichner öffentlich sein muss. Wenn Sie den Bezeichner nicht offenlegen, können Benutzer diese Hilfsprogramm-APIs nicht verwenden. Beispiele für eine API und Szenarien dieser Art sind [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359), [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361), [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357), [**GetAnimationBaseValue**](https://msdn.microsoft.com/library/windows/apps/br242358), [**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257) und [**Setter.Property**](https://msdn.microsoft.com/library/windows/apps/br208836). Ein öffentliches Feld kann dazu nicht verwendet werden, da Windows-Runtime-Metadatenregeln keine öffentlichen Felder zulassen.

## Namenskonventionen für Abhängigkeitseigenschaften

Für Abhängigkeitseigenschaften gelten Namenskonventionen, die Sie bis auf bestimmte Ausnahmefälle befolgen müssen. Die Abhängigkeitseigenschaft selbst hat einen Basisnamen („Label“ im vorherigen Beispiel), der als erster Parameter von [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) angegeben wird. Dieser Name muss innerhalb jedes Registrierungstyps eindeutig sein. Diese Eindeutigkeit muss auch von vererbten Membern eingehalten werden. Abhängigkeitseigenschaften, die über Basistypen geerbt werden, werden als Teil des Registrierungstyps betrachtet. Die Namen geerbter Eigenschaften können nicht erneut registriert werden.

**Achtung** Obwohl der von Ihnen angegebene Name jeder Zeichenfolgebezeichner sein kann, der bei der Programmierung in der von Ihnen ausgewählten Sprache gültig ist, sollten Sie in der Regel Ihre Abhängigkeitseigenschaft auch in XAML festlegen können. Für die Verwendung in XAML muss der von Ihnen gewählte Eigenschaftenname ein gültiger XAML-Name sein. Weitere Informationen finden Sie in der [XAML-Übersicht](xaml-overview.md).

Kombinieren Sie beim Erstellen der Bezeichnereigenschaft den von Ihnen registrierten Eigenschaftennamen mit dem Suffix „Property“ (beispielsweise „LabelProperty“). Diese Eigenschaft ist Ihr Bezeichner für die Abhängigkeitseigenschaft und wird als Eingabe für die [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361)- und [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359)-Aufrufe verwendet, die Sie in Ihren eigenen Eigenschaftenwrappern ausführen. Sie wird auch vom Eigenschaftensystem und anderen XAML-Prozessoren, wie z. B. [**{x:Bind}**](x-bind-markup-extension.md), verwendet.

## Implementieren des Wrappers

Ihr Eigenschaftenwrapper sollte [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) in der **get**-Implementierung und [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) in der **set**-Implementierung aufrufen.

**Achtung** Von Ausnahmefällen abgesehen, sollten Ihre Wrapperimplementierungen nur die Operationen [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) und [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) ausführen. Andernfalls erhalten Sie ein anderes Verhalten, wenn Ihre Eigenschaft über XAML anstelle über Code festgelegt wird. Aus Effizienzgründen umgeht der XAML-Parser Wrapper beim Festlegen von Abhängigkeitseigenschaften und kommuniziert mit dem Sicherungsspeicher über **SetValue**.

> [!div class="tabbedCodeSnippets"]
```csharp
public String Label
{
    get { return (String)GetValue(LabelProperty); }
    set { SetValue(LabelProperty, value); }
}
```
```vb
Public Property Label() As String 
    Get 
        Return DirectCast(GetValue(LabelProperty), String) 
    End Get 
    Set(ByVal value As String) 
        SetValue(LabelProperty, value) 
    End Set 
End Property
```
```cpp
//using namespace Platform;
public:
...
  property String^ Label
  {
    String^ get() {
      return (String^)GetValue(LabelProperty);
    }
    void set(String^ value) {
      SetValue(LabelProperty, value); 
    }
  }
```

## Eigenschaftenmetadaten für eine benutzerdefinierte Abhängigkeitseigenschaft

Wenn Eigenschaftenmetadaten einer Abhängigkeitseigenschaft zugewiesen werden, gelten die gleichen Metadaten für diese Eigenschaft für jede Instanz des Eigenschaftenbesitzertyps oder dessen Unterklassen. Sie können zwei Verhalten in Eigenschaftenmetadaten festlegen:

-   Einen Standardwert, den das Eigenschaftensystem allen Anfragen der Eigenschaft zuweist.
-   Eine statische Rückrufmethode, die automatisch innerhalb des Eigenschaftensystems aufgerufen wird, sobald eine Eigenschaftswertänderung erkannt wird.

### Aufrufen von Register mit Eigenschaftenmetadaten

In den bisherigen Beispielen für den Aufruf von [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) wurde für den Parameter *propertyMetadata* ein Nullwert übergeben. Damit eine Abhängigkeitseigenschaft einen Standardwert bereitstellt oder einen Rückruf mit geänderter Eigenschaft verwendet, müssen Sie eine [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771)-Instanz definieren, die mindestens eine dieser beiden Funktionen bereitstellt.

In der Regel geben Sie einen [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) als inline erstellte Instanz innerhalb der Parameter für [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) an.

**Hinweis**  Wenn Sie eine [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812)-Implementierung verwenden, müssen Sie die Hilfsmethode [**PropertyMetadata.Create**](https://msdn.microsoft.com/library/windows/apps/hh702099) aufrufen und nicht einen [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771)-Konstruktor, um die **PropertyMetadata**-Instanz zu definieren.

Im nächsten Beispiel werden die zuvor gezeigten Beispiele für [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) modifiziert, indem auf eine [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771)-Instanz mit einem [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770)-Wert verwiesen wird. Die Implementierung des OnLabelChanged-Rückrufs wird später in diesem Abschnitt gezeigt.

> [!div class="tabbedCodeSnippets"]
```csharp
public static readonly DependencyProperty LabelProperty = DependencyProperty.Register(
  "Label",
  typeof(String),
  typeof(ImageWithLabelControl),
  new PropertyMetadata(null,new PropertyChangedCallback(OnLabelChanged))
);
```
```vb
Public Shared ReadOnly LabelProperty As DependencyProperty = 
    DependencyProperty.Register("Label", 
      GetType(String), 
      GetType(ImageWithLabelControl), 
      New PropertyMetadata(
        Nothing, new PropertyChangedCallback(AddressOf OnLabelChanged)))
```
```cpp
DependencyProperty^ ImageWithLabelControl::_LabelProperty = 
    DependencyProperty::Register("Label", 
    Platform::String::typeid,
    ImageWithLabelControl::typeid, 
    ref new PropertyMetadata(nullptr,
      ref new PropertyChangedCallback(&ImageWithLabelControl::OnLabelChanged))
    );
```

### Standardwert

Sie können einen Standardwert für eine Abhängigkeitseigenschaft festlegen, damit diese immer einen bestimmten Standardwert zurückgibt, wenn ihre Festlegung aufgehoben wird. Dieser Wert kann vom vererbten Standardwert für den Typ dieser Eigenschaft abweichen.

Wenn kein Standardwert festgelegt ist, ist der Standardwert einer Abhängigkeitseigenschaft für einen Verweistyp null oder entspricht der Standardeinstellung des Werttyps oder des Sprachengrundtyps (zum Beispiel 0 für einen Integer oder eine leere Zeichenfolge für eine Zeichenfolge). Ein Standardwert wird vor allem erstellt, damit dessen Wert wiederhergestellt wird, wenn [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357) in der Eigenschaft aufgerufen wird. Das Erstellen eines Standardwerts pro Eigenschaft kann vorteilhafter als das Erstellen von Standardwerten in Konstruktoren sein, insbesondere für Werttypen. Achten Sie jedoch bei Verweistypen darauf, dass beim Erstellen eines Standardwerts kein unbeabsichtigtes Singleton-Muster entsteht. Weitere Informationen finden Sie weiter unten in diesem Thema unter [Bewährte Methoden](#best-practices).

**Hinweis**  Führen Sie keine Registrierung mit einem Standardwert von [**UnsetValue**](https://msdn.microsoft.com/library/windows/apps/br242371) durch. Diese Handlung verwirrt Eigenschaftennutzer und führt zu unbeabsichtigten Folgen innerhalb des Eigenschaftensystems.

### CreateDefaultValueCallback

In einigen Szenarien werden Abhängigkeitseigenschaften für Objekte definiert, die in mehreren UI-Threads verwendet werden. Dies ist beispielsweise der Fall, wenn ein Datenobjekt oder ein Steuerelement für mehrere Apps definiert wird. Sie können den Austausch des Objekts zwischen verschiedenen Benutzeroberflächenthreads ermöglichen, indem Sie anstelle einer Standardwertinstanz eine [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812)-Implementierung bereitstellen, die an den Thread gebunden ist, der die Eigenschaft registriert hat. Grundsätzlich definiert ein [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) eine Zuordnungsinstanz für Standardwerte. Der Wert, der von **CreateDefaultValueCallback** zurückgegeben wird, ist stets mit dem aktuellen Benutzeroberflächenthread von **CreateDefaultValueCallback** verbunden, von dem das Objekt verwendet wird.

Um Metadaten zu definieren, die einen [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) angeben, müssen Sie [**PropertyMetadata.Create**](https://msdn.microsoft.com/library/windows/apps/hh702115) aufrufen, um eine Metadateninstanz zurückzugeben. Die [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771)-Konstruktoren besitzen keine Signatur, die einen **CreateDefaultValueCallback**-Parameter enthält.

Das typische Implementierungsmuster für einen [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) besteht im Erstellen einer neuen [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)-Klasse, dem Festlegen der spezifischen Eigenschaftswerte der einzelnen Eigenschaften von **DependencyObject** auf die beabsichtigen Standardwerte und der anschließenden Rückgabe der neuen Klasse als **Object**-Verweis über den Rückgabewert der Methode **CreateDefaultValueCallback**.

### Rückrufmethode mit geänderter Eigenschaft

Sie können eine PropertyChanged-Rückrufmethode verwenden, um die Interaktionen Ihrer Eigenschaft mit anderen Abhängigkeitseigenschaften zu definieren oder eine interne Eigenschaft bzw. einen Status Ihres Objekts zu aktualisieren, sobald die Eigenschaft geändert wird. Wenn Ihr Rückruf aufgerufen wurde, hat das Eigenschaftensystem festgestellt, dass es eine effektive Änderung des Eigenschaftswerts gibt. Da die Rückrufmethode statisch ist, ist der *d*-Parameter des Rückrufs wichtig, da dieser anzeigt, welche Instanz der Klasse eine Änderung gemeldet hat. In einer typischen Implementierung wird die [**NewValue**](https://msdn.microsoft.com/library/windows/apps/br242364)-Eigenschaft des Ereignisdatums verwendet und dieser Wert auf bestimmte Weise verarbeitet, in der Regel durch Ausführung einer anderen Änderung für das Objekt, das als *d* übergeben wurde. Weitere Reaktionen auf eine Eigenschaftenänderung sind die Zurückweisung des von **NewValue** gemeldeten Werts, die Wiederherstellung von [**OldValue**](https://msdn.microsoft.com/library/windows/apps/br242365) oder die Festlegung des Werts auf eine programmgesteuerte Einschränkung für den **NewValue**.

Das nächste Beispiel zeigt eine [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770)-Implementierung. Hier wird die Methode implementiert, auf die in den vorigen [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)-Beispielen als Teil des Konstruktionsarguments für die [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) Bezug genommen wurde. In diesem Rückrufszenario besitzt die Klasse auch eine berechnete schreibgeschützte Eigenschaft „HasLabelValue“ (Implementierung nicht gezeigt). Diese Rückrufmethode wird jedes Mal aufgerufen, wenn die Label-Eigenschaft neu ausgewertet wird. Der Rückruf ermöglicht die kontinuierliche Synchronisierung des abhängigen berechneten Werts mit den Änderungen der Abhängigkeitseigenschaft.

> [!div class="tabbedCodeSnippets"]
```csharp
private static void OnLabelChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    ImageWithLabelControl iwlc = d as ImageWithLabelControl; //null checks omitted
    String s = e.NewValue as String; //null checks omitted
    if (s == String.Empty)
    {
        iwlc.HasLabelValue = false;s
    } else {
        iwlc.HasLabelValue = true;
    }
}
```
```vb
    Private Shared Sub OnLabelChanged(d As DependencyObject, e As DependencyPropertyChangedEventArgs)
        Dim iwlc As ImageWithLabelControl = CType(d, ImageWithLabelControl) ' null checks omitted
        Dim s As String = CType(e.NewValue,String) ' null checks omitted
        If s Is String.Empty Then
            iwlc.HasLabelValue = False
        Else
            iwlc.HasLabelValue = True
        End If
    End Sub
```
```cpp
static void OnLabelChanged(DependencyObject^ d, DependencyPropertyChangedEventArgs^ e)
{
    ImageWithLabelControl^ iwlc = (ImageWithLabelControl^)d;
    Platform::String^ s = (Platform::String^)(e->NewValue);
    if (s->IsEmpty()) {
        iwlc->HasLabelValue=false;
    }
}
```

### Verhalten bei geänderter Eigenschaft für Strukturen und Enumerationen

Wenn der Typ einer [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) eine Enumeration oder Struktur ist, kann der Rückruf auch aufgerufen werden, wenn die internen Werte der Struktur oder der Enumerationswert nicht geändert wurden. Dieses Verhalten weicht gegenüber einem Systemgrundtyp wie einer Zeichenfolge ab. Hier erfolgt der Aufruf nur bei einem geänderten Wert. Dies ist der Nebeneffekt eines internen Boxing- und Unboxing-Vorgangs für diese Werte. Wenn Sie über eine [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770)-Methode für eine Eigenschaft verfügen, deren Wert eine Enumeration oder Struktur ist, müssen Sie [**OldValue**](https://msdn.microsoft.com/library/windows/apps/br242365) und [**NewValue**](https://msdn.microsoft.com/library/windows/apps/br242364) vergleichen. Dazu wandeln Sie die Werte selbst um und verwenden die überladenen Vergleichsoperatoren, die für die nun umgewandelten Werte verfügbar sind. Wenn kein entsprechender Operator verfügbar ist (beispielsweise weil es sich um eine benutzerdefinierte Struktur handelt), müssen Sie möglicherweise die einzelnen Werte vergleichen. Wenn Sie zu dem Ergebnis kommen, dass sich die Werte nicht geändert haben, ist normalerweise keine Aktion erforderlich.

> [!div class="tabbedCodeSnippets"]
```csharp
private static void OnVisibilityValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    if ((Visibility)e.NewValue != (Visibility)e.OldValue)
    {
        //value really changed, invoke your changed logic here
    } // else this was invoked because of boxing, do nothing
}
```
```vb
Private Shared Sub OnVisibilityValueChanged(d As DependencyObject, e As DependencyPropertyChangedEventArgs)
    If CType(e.NewValue,Visibility) != CType(e.OldValue,Visibility) Then
        '  value really changed, invoke your changed logic here
    End If
    '  else this was invoked because of boxing, do nothing
End Sub
```
```cpp
static void OnVisibilityValueChanged(DependencyObject^ d, DependencyPropertyChangedEventArgs^ e)
{
    if ((Visibility)e->NewValue != (Visibility)e->OldValue)
    {
        //value really changed, invoke your changed logic here
    } 
    // else this was invoked because of boxing, do nothing
    }
}
```

## Bewährte Methoden

Berücksichtigen Sie die folgenden Überlegungen als bewährte Methoden, wenn Sie Ihre benutzerdefinierte Abhängigkeitseigenschaft festlegen.

### DependencyObject und Threading

Alle [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)-Instanzen müssen in dem Benutzeroberflächenthread erstellt werden, der mit dem aktuellen [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) verknüpft ist, das von einer Windows-Runtime-App angezeigt wird. Obwohl jedes **DependencyObject** im zentralen Benutzeroberflächenthread erstellt werden muss, kann durch Aufrufen von [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br230616) mithilfe eines Verteilerverweises von anderen Threads aus auf die Objekte zugegriffen werden.

Die Threadingmerkmale von [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) sind relevant, da dies in der Regel bedeutet, dass nur im Benutzeroberflächenthread ausgeführter Code den Wert einer Abhängigkeitseigenschaft ändern oder auch nur lesen kann. Threadingprobleme können in typischem Benutzeroberflächencode in der Regel vermieden werden, wenn **async**-Muster und Hintergrundworkerthreads richtig verwendet werden. Threadingprobleme im Zusammenhang mit **DependencyObject** entstehen in der Regel nur, wenn Sie eigene **DependencyObject**-Typen definieren und versuchen, diese als Datenquellen oder für andere Szenarien zu verwenden, für die ein **DependencyObject** nicht notwendigerweise geeignet ist.

### Vermeiden unbeabsichtigter Singletons

Ein unbeabsichtigter Singleton kann auftreten, wenn eine Abhängigkeitseigenschaft deklariert wird, die einen Verweistyp annimmt, und Sie einen Konstruktor für diesen Verweistyp als Teil des Codes aufrufen, der [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) erstellt. In diesem Fall teilen sich alle Anwendungen der Abhängigkeitseigenschaft eine einzige Instanz von **PropertyMetadata** und versuchen daher, den einzelnen von Ihnen erstellten Verweistyp gemeinsam zu nutzen. Alle untergeordneten Eigenschaften dieses Werttyps, die Sie durch Ihre Abhängigkeitseigenschaft festlegen, werden anschließend auf andere Objekte übertragen, und zwar auf eine Weise, die Sie möglicherweise nicht beabsichtigt haben.

Sie können Klassenkonstruktoren verwenden, um erste Werte für eine Abhängigkeitseigenschaft vom Typ "Verweis" festzulegen, wenn Sie einen Nicht-null-Wert haben möchten. Beachten Sie jedoch, dass dies als lokaler Wert für die [Übersicht über Abhängigkeitseigenschaften](dependency-properties-overview.md) betrachtet werden würde. Es ist unter Umständen angemessener, eine Vorlage– falls diese von Ihrer Klasse unterstützt wird– für diesen Zweck zu verwenden. Eine andere Möglichkeit, um Singleton-Muster zu vermeiden, aber dennoch einen nützlichen Standardwert bereitzustellen, besteht darin, eine statische Eigenschaft für den Verweistyp, der einen passenden Standardwert für die Werte dieser Klasse liefert, verfügbar zu machen.

### Abhängigkeitseigenschaften des Sammlungstyps

Bei Abhängigkeitseigenschaften vom Typ "Sammlung" sind einige zusätzliche Implementierungsaspekte zu beachten.

Abhängigkeitseigenschaften des Sammlungstyps sind in der Windows-Runtime-API vergleichsweise selten. In den meisten Fällen können Sie Sammlungen verwenden, deren Elemente eine [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)-Unterklasse sind. Die Sammlungseigenschaft selbst wird jedoch als herkömmliche CLR- oder eine C++-Eigenschaft implementiert. Das ist darauf zurückzuführen, dass sich Sammlungen nicht unbedingt für herkömmliche Szenarien eignen, bei denen Abhängigkeitseigenschaften involviert sind. Beispiel:

-   Sie animieren für gewöhnlich keine Sammlung.
-   Sie füllen die Elemente einer Sammlung für gewöhnlich nicht vorher mit Stilen oder einer Vorlage aus.
-   Obwohl das Binden an Sammlungen ein wichtiges Szenario ist, muss die Sammlung keine Abhängigkeitseigenschaft sein, um eine Bindungsquelle darzustellen. Im Fall von Bindungszielen werden in der Regel Unterklassen von [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/br242803) oder [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348) verwendet, um Sammlungselemente zu unterstützen oder Modellanzeigemuster zu verwenden. Weitere Informationen zur Bindung zu und von Sammlungen finden Sie unter [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946).
-   Benachrichtigungen zu Sammlungsänderungen sollten besser durch Schnittstellen wie **INotifyPropertyChanged** oder **INotifyCollectionChanged** oder durch Ableiten des Sammlungstyps von [**ObservableCollection&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/ms668604.aspx) behandelt werden.

Dennoch gibt es Szenarien für Abhängigkeitseigenschaften des Sammlungstyps. In den nächsten drei Abschnitten finden Sie Informationen zur Implementierung einer Abhängigkeitseigenschaft vom Typ "Sammlung".

### Initialisieren der Sammlung

Wenn Sie eine Abhängigkeitseigenschaft erstellen, geben Sie den Standardwert über die Metadaten für die Abhängigkeitseigenschaft an. Achten Sie darauf, keine einzelne statische Sammlung als Standardwert zu verwenden. Stattdessen müssen Sie sicherstellen, dass Sie den Sammlungswert bewusst auf eine eindeutige (Instanz-)Sammlung im Rahmen der Klassenkonstruktorlogik für die Besitzerklasse der Sammlungseigenschaft festlegen.

### Änderungsbenachrichtigungen

Durch die Definition der Sammlung als Abhängigkeitseigenschaft werden nicht automatisch Änderungsbenachrichtigungen für die Elemente in der Sammlung bereitgestellt, indem das Eigenschaftensytem die Rückrufmethode „PropertyChanged“ aufruft. Wenn Sie Benachrichtigungen für Sammlungen oder Sammlungselemente einrichten möchten, beispielsweise für ein Datenbindungsszenario, implementieren Sie die **INotifyPropertyChanged**- oder **INotifyCollectionChanged**-Schnittstelle. Weitere Informationen finden Sie unter [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946).

### Sicherheitsüberlegungen in Bezug auf Abhängigkeitseigenschaften

Deklarieren Sie Abhängigkeitseigenschaften als öffentliche Eigenschaften. Bezeichner von Abhängigkeitseigenschaften sollten als **public static readonly**-Member deklariert werden. Auch wenn Sie versuchen, andere durch eine Sprache zugelassene Zugriffsebenen (beispielsweise **protected**) zu deklarieren, können Sie stets auf eine Abhängigkeitseigenschaft zugreifen, indem Sie den Bezeichner in Verbindung mit den APIs des Eigenschaftensystems verwenden. Das Deklarieren des Bezeichners der Abhängigkeitseigenschaft als intern oder privat wird nicht funktionieren, da das Eigenschaftensystem in diesem Fall nicht ordnungsgemäß arbeitet.

Wrappereigenschaften sind nur als Vereinfachung gedacht. Die Sicherheitsmechanismen, die für die Wrapper angewendet werden, können durch das Aufrufen von [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) oder [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) umgangen werden. Wrappereigenschaften sollten daher öffentlich bleiben. Andernfalls machen Sie die Nutzung Ihrer Eigenschaft für legitime Aufrufer schwieriger, ohne einen tatsächlichen Vorteil in Bezug auf die Sicherheit zu erhalten.

Die Windows-Runtime bietet keine Möglichkeit, eine benutzerdefinierte Abhängigkeitseigenschaft als schreibgeschützt zu registrieren.

### Abhängigkeitseigenschaften und Klassenkonstruktoren

Hier gilt der allgemeine Grundsatz, dass Klassenkonstruktoren keine virtuellen Methoden aufrufen sollten. Der Grund hierfür ist, dass Konstruktoren als Basisinitialisierung eines abgeleiteten Klassenkonstruktors aufgerufen werden können, und das Eingeben der virtuellen Methode über den Konstruktor könnte zu einem Zeitpunkt erfolgen, zu dem das erstellte Objekt einen unvollständigen Initialisierungszustand aufweist. Wenn Sie eine Ableitung von einer Klasse durchführen, die bereits von [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) abgeleitet ist, sollten Sie daran denken, dass das Eigenschaftensystem selbst als Teil seiner Dienste virtuelle Methoden intern aufruft und offenlegt. Um mögliche Probleme mit der Laufzeitinitialisierung zu vermeiden, sollten Sie in Konstruktoren von Klassen keine Werte von Abhängigkeitseigenschaften festlegen.

### Registrieren der Abhängigkeitseigenschaften für C++/CX-Apps

Die Implementierung für das Registrieren einer Eigenschaft in C++/CX ist schwieriger als für C#, zum einen aufgrund der Aufteilung in Kopfzeile und Implementierungsdatei und zum anderen, weil die Initialisierung im Stammbereich der Implementierungsdatei nicht empfohlen wird. (Visual C++-Komponentenerweiterungen (C++/CX) platzieren statischen Initialisierungscode aus dem Stammbereich direkt in **DllMain**, während C#-Compiler die statischen Initialisierer Klassen zuweisen und so **DllMain**-Ladeprobleme vermeiden). Die bewährte Methode besteht hier im Deklarieren einer Hilfsfunktion, die die gesamte Registrierung von Abhängigkeitseigenschaften für eine Klasse durchführt, d.h. eine Funktion pro Klasse. Für jede benutzerdefinierte Klasse, die Ihre App nutzt, müssen Sie dann auf die Hilfsregistrierungsfunktion verweisen, die von den einzelnen benutzerdefinierten Klassen jeweils verfügbar gemacht wird, die Sie verwenden möchten. Rufen Sie jede Hilfsregistrierungsfunktion einmalig als Teil des [**Application constructor**](https://msdn.microsoft.com/library/windows/apps/br242325) (`App::App()`) vor `InitializeComponent` auf. Dieser Konstruktor wird nur ausgeführt, wenn wirklich zum ersten Mal auf die App verwiesen wir. Er wird nicht erneut ausgeführt, wenn beispielsweise eine angehaltene App fortgesetzt wird. Wie im vorherigen C++-Registrierungsbeispiel gezeigt, ist auch die **nullptr**-Überprüfung jedes [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)-Aufrufs wichtig, da hierdurch sichergestellt wird, dass kein Aufrufer der Funktion die Eigenschaft zweimal registrieren kann. Bei einem zweiten Registrierungsaufruf würde Ihre App ohne eine solche Überprüfung wahrscheinlich abstürzen, da der Eigenschaftsname doppelt vorhanden wäre. Dieses Implementierungsmuster können Sie im [XAML-Beispiel für Benutzer und benutzerdefinierte Steuerelemente](http://go.microsoft.com/fwlink/p/?linkid=238581) im Code für die C++/CX-Version des Beispiels sehen.

## Verwandte Themen

* [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)
* [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)
* [Übersicht über Abhängigkeitseigenschaften](dependency-properties-overview.md)
* [XAML-Beispiel für Benutzer und benutzerdefinierte Steuerelemente](http://go.microsoft.com/fwlink/p/?linkid=238581)
 




<!--HONumber=Aug16_HO3-->


