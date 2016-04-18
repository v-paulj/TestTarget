---
title: Erstellen von Komponenten für Windows-Runtime in C++
description: In diesem Artikel wird beschrieben, wie Sie eine Komponente für Windows-Runtime mit C++ erstellen. Dabei handelt es sich um eine DLL, die aus einer universellen Windows-App aufgerufen werden kann, die mit JavaScript (oder C#, Visual Basic oder C++) entwickelt wurde.
ms.assetid: F7E06AA2-DCEC-427E-BD5D-9CA2A0ED2612
---


# Erstellen von Komponenten für Windows-Runtime in C++


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


\[Einige Informationen beziehen sich auf die Vorabversion, die vor der kommerziellen Freigabe möglicherweise wesentlichen Änderungen unterliegt. Microsoft übernimmt keine Garantie, weder ausdrücklich noch stillschweigend, für die hier bereitgestellten Informationen.\]

In diesem Artikel wird beschrieben, wie Sie eine Komponente für Windows-Runtime mit C++ erstellen. Dabei handelt es sich um eine DLL, die aus einer universellen Windows-App aufgerufen werden kann, die mit JavaScript (oder C#, Visual Basic oder C++) entwickelt wurde.

Im Folgenden finden Sie einige Gründe für die Erstellung einer solchen Komponente:

-   Um die Leistungsvorteile von C++ für komplexe oder rechenintensive Vorgänge zu übernehmen.

-   Um Code wiederzuverwenden, der bereits geschrieben und getestet wurde.

Wenn Sie eine Projektmappe erstellen, die ein JavaScript- oder .NET-Projekt und ein Komponentenprojekt für Windows-Runtime enthält, werden die JavaScript-Projektdateien und die kompilierte DLL in ein Paket zusammengeführt, das Sie lokal im Simulator oder remote auf einem verbundenen Gerät debuggen können. Sie können auch nur das Komponentenprojekt als Erweiterungs-SDK verteilen. Weitere Informationen finden Sie unter [Erstellen eines Software Development Kit](https://msdn.microsoft.com/library/hh768146.aspx).

Wenn Sie die C++-Komponente programmieren, verwenden Sie im Allgemeinen die reguläre C++-Bibliothek und integrierte Typen außer an der abstrakten Grenze der binären Schnittstelle (ABI), an der Sie Daten an und aus Code in einem anderen WINMD-Paket übergeben. Verwenden Sie hier Windows-Runtime-Typen und die spezielle Syntax, die Visual C++ zum Erstellen und Bearbeiten dieser Typen unterstützt. Verwenden Sie in Ihrem Visual C++-Code außerdem Typen wie Delegaten und Ereignisse, um Ereignisse zu implementieren, die von der Komponente ausgelöst und in JavaScript, Visual Basic oder C# bearbeitet werden können. Weitere Informationen zur neuen Visual C++-Syntax finden Sie unter [ Sprachreferenz zu Visual C++ (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx).

## Regeln für die Groß-/Kleinschreibung und die Benennung


### JavaScript

In JavaScript muss die Groß-/Kleinschreibung beachtet werden. Daher müssen Sie die folgenden Konventionen zur Groß-/Kleinschreibung einhalten:

-   Verwenden Sie die C++-Schreibweise, wenn Sie auf C++-Namespaces und -Klassen verweisen.
-   Verwenden Sie beim Aufrufen von Methoden die Kamelschreibweise, auch wenn der Methodenname in C++ großgeschrieben wird. Beispielsweise muss die C++-Methode „GetDate()” aus JavaScript mit „getDate()” aufgerufen werden.
-   Ein aktivierbarer Klassenname und Namespacename darf keine UNICODE-Zeichen enthalten.

### .NET

Die .NET-Sprachen folgen ihren üblichen Regeln für die Groß-und Kleinschreibung.

## Instanziieren des Objekts


Nur Windows-Runtime-Typen können über die ABI-Grenze hinweg übergeben werden. Der Compiler löst einen Fehler aus, wenn die Komponente über einen Typ wie „std::wstring” als Rückgabetyp oder Parameter in einer öffentlichen Methode verfügt. Die in Visual C++-Komponentenerweiterungen (C++/CX) integrierten Typen enthalten die üblichen Skalare, wie int und double sowie deren Typedef-Entsprechungen int32, float64 usw. Weitere Informationen finden Sie unter [Typsystem (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822.aspx).

```cpp
// ref class definition in C++
public ref class SampleRefClass sealed
{
    // Class members...

    // #include <valarray>
public:
    double LogCalc(double input)
    {
        // Use C++ standard library as usual.
        return std::log(input); 
    }

};
```

```javascript
//Instantiation in JavaScript (requires "Add reference > Project reference")
var nativeObject = new CppComponent.SampleRefClass();
```

```csharp
//Call a method and display result in a XAML TextBlock
var num = nativeObject.LogCalc(21.5);
ResultText.Text = num.ToString();
```

## Integrierte C++-Typen, Bibliothekstypen und Windows-Runtime-Typen


Eine aktivierbare Klassen (auch als Verweisklasse bezeichnet) kann in einer anderen Sprache, wie z. B. JavaScript, C# oder Visual Basic, instanziiert werden. Um von einer anderen Sprache verwendet werden zu können, muss eine Komponente mindestens eine aktivierbare Klasse enthalten.

Eine Komponente für Windows-Runtime kann mehrere öffentliche aktivierbare Klassen sowie zusätzliche Klassen enthalten, die der Komponente nur intern bekannt sind. Verwenden Sie das [WebHostHidden](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.webhosthiddenattribute.aspx)-Attribut für C++-Typen, die für JavaScript nicht sichtbar sein sollen.

Alle öffentliche Klassen müssen sich im selben Stammnamespace befinden, der denselben Namen wie die Metadatendatei der Komponente hat. Zum Beispiel kann eine Klasse mit dem Namen A.B.C.MyClass nur instanziiert werden, wenn sie in einer Metadatendatei mit dem Namen A.winmd oder A.B.winmd oder A.B.C.winmd definiert ist. Der Name der DLL muss nicht mit dem Namen der WINMD-Datei übereinstimmen.

Clientcode erstellt eine Instanz der Komponente mit dem Schlüsselwort **new** (**New** in Visual Basic) so wie für jede andere Klasse auch.

Eine aktivierbare Klasse muss als **public ref class sealed** deklariert werden. Das Schlüsselwort **ref class** teilt dem Compiler mit, die Klasse als einen mit der Windows-Runtime kompatiblen Typ zu erstellen, und das Schlüsselwort „sealed” gibt an, dass die Klasse nicht vererbt werden kann. Die Windows-Runtime unterstützt derzeit kein generalisiertes Vererbungsmodell; ein beschränktes Vererbungsmodell unterstützt die Erstellung von benutzerdefinierten XAML-Steuerelementen. Weitere Informationen finden Sie unter [Verweisklassen und Strukturen (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699870.aspx).

In C++ werden alle numerischen Grundtypen im Standardnamespace definiert. Der [Platform](https://msdn.microsoft.com/library/windows/apps/xaml/hh710417.aspx)-Namespace enthält für das Windows-Runtime-Typsystem spezifische C++-Klassen. Dazu gehören die Klassen [Platform:: String](https://msdn.microsoft.com/library/windows/apps/xaml/hh755812.aspx) und [Platform::Object](https://msdn.microsoft.com/library/windows/apps/xaml/hh748265.aspx). Die konkreten Sammlungstypen, wie die Klassen [Platform::Collections::Map](https://msdn.microsoft.com/library/windows/apps/xaml/hh441508.aspx) und [Platform::Collections::Vector](https://msdn.microsoft.com/library/windows/apps/xaml/hh441570.aspx), sind im Namespace [Platform::Collections](https://msdn.microsoft.com/library/windows/apps/xaml/hh710418.aspx) definiert. Die öffentlichen Schnittstellen, die diese Typen implementieren, sind im [Windows::Foundation::Collections-Namespace (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh441496.aspx) definiert. Diese Schnittstellentypen werden von JavaScript, C# und Visual Basic verwendet. Weitere Informationen finden Sie unter [Typsystem (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822.aspx).

## Methode, die einen Wert mit einem integrierten Typ zurückgibt

```cpp
    // #include <valarray>
public:
    double LogCalc(double input)
    {
        // Use C++ standard library as usual.
        return std::log(input); 
    }
```

```javascript
//Call a method
var nativeObject = new CppComponent.SampleRefClass;
var num = nativeObject.logCalc(21.5);
document.getElementById('P2').innerHTML = num;
```

## Methode, die eine benutzerdefinierte Wertstruktur zurückgibt

```cpp
namespace CppComponent
{
    // Custom struct
    public value struct PlayerData
    {
        Platform::String^ Name;
        int Number;
        double ScoringAverage;
    };

    public ref class Player sealed
    {
    private:
        PlayerData m_player;
    public:
        property PlayerData PlayerStats 
        {
            PlayerData get(){ return m_player; }
            void set(PlayerData data) {m_player = data;}
        }
    };
}
```

Um benutzerdefinierte Wertstrukturen über die ABI zu übergeben, definieren Sie ein JavaScript-Objekt, das über die gleichen Member wie die in C++ definierte Wertstruktur verfügt. Sie können dieses Objekt dann als Argument an eine C++-Methode übergeben, damit das Objekt implizit in den C++-Typ konvertiert wird.

```javascript
// Get and set the value struct
function GetAndSetPlayerData() {
    // Create an object to pass to C++
    var myData =
        { name: "Bob Homer", number: 12, scoringAverage: .357 };
    var nativeObject = new CppComponent.Player();
    nativeObject.playerStats = myData;

    // Retrieve C++ value struct into new JavaScript object
    var myData2 = nativeObject.playerStats;
    document.getElementById('P3').innerHTML = myData.name + " , " + myData.number + " , " + myData.scoringAverage.toPrecision(3);
}
```

Ein anderer Ansatz besteht darin, eine Klasse zu definieren, die „IPropertySet” implementiert (nicht dargestellt).

In den .NET-Sprachen erstellen Sie einfach eine Variable des Typs, der in der C++-Komponente definiert ist.

```csharp
private void GetAndSetPlayerData()
{
    // Create a ref class
    var player = new CppComponent.Player();

    // Create a variable of a value struct
    // type that is defined in C++
    CppComponent.PlayerData myPlayer;
    myPlayer.Name = "Babe Ruth";
    myPlayer.Number = 12;
    myPlayer.ScoringAverage = .398;

    // Set the property
    player.PlayerStats = myPlayer;

    // Get the property and store it in a new variable
    CppComponent.PlayerData myPlayer2 = player.PlayerStats;
    ResultText.Text += myPlayer.Name + " , " + myPlayer.Number.ToString() +
        " , " + myPlayer.ScoringAverage.ToString();
}
```

## Überladene Methoden


Eine öffentliche C++-Verweisklasse kann überladene Methoden enthalten, aber JavaScript verfügt nur über begrenzte Möglichkeiten zur Unterscheidung überladener Methoden. Beispielsweise kann JavaScript den Unterschied zwischen diesen Signaturen erkennen:

```cpp
public ref class NumberClass sealed 
{
public:
    int GetNumber(int i);
    int GetNumber(int i, Platform::String^ str);
    double GetNumber(int i, MyData^ d);
};
```

Aber den Unterschied zwischen diesen nicht:

```cpp
int GetNumber(int i);
double GetNumber(double d);
```

In mehrdeutigen Fällen können Sie sicherstellen, dass JavaScript immer eine bestimmte Überladung aufruft, indem Sie der Methodensignatur in der Headerdatei das [Windows::Foundation::Metadata::DefaultOverload](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.defaultoverloadattribute.aspx)-Attribut hinzufügen.

Dieser JavaScript-Code ruft immer die attributierte Überladung auf:

```javascript
var nativeObject = new CppComponent.NumberClass();
var num = nativeObject.getNumber(9);
document.getElementById('P4').innerHTML = num;
```

## .NET


Die .NET-Sprachen erkennen Überladungen in einer C++-Verweisklasse ebenso wie in jeder .NET Framework-Klasse.

## DateTime

In der Windows-Runtime ist ein [Windows::Foundation::DateTime](https://msdn.microsoft.com/library/windows/apps/windows.foundation.datetime.aspx)-Objekt einfach eine 64-Bit-Ganzzahl mit Vorzeichen, die die Anzahl der 100-Nanosekunden-Intervalle vor oder nach dem 1. Januar 1601 darstellt. Es gibt keine Methoden für ein Windows:Foundation::DateTime-Objekt. Jede Sprache stellt stattdessen DateTime in einer für diese Sprache systemeigenen Weise dar: als Date-Objekt in JavaScript und als System.DateTime- und System.DateTimeOffset-Typen in .NET Framework.

```cpp
public  ref class MyDateClass sealed
{
public:
    property Windows::Foundation::DateTime TimeStamp;
    void SetTime(Windows::Foundation::DateTime dt)
    {
        auto cal = ref new Windows::Globalization::Calendar();
        cal->SetDateTime(dt);
        TimeStamp = cal->GetDateTime(); // or TimeStamp = dt;
    }
};
```

Wenn Sie einen DateTime-Wert aus C++ an JavaScript übergeben, akzeptiert JavaScript diesen Wert als Date-Objekt und zeigt ihn standardmäßig als Datumszeichenfolge in Langform an.

```javascript
function SetAndGetDate() {
    var nativeObject = new CppComponent.MyDateClass();

    var myDate = new Date(1956, 4, 21);
    nativeObject.setTime(myDate);

    var myDate2 = nativeObject.timeStamp;

    //prints long form date string
    document.getElementById('P5').innerHTML = myDate2;

}
```

Wenn eine .NET-Sprache einen System.DateTime-Wert für eine C++-Komponente übergibt, akzeptiert die Methode ihn als Windows::Foundation::DateTime. Wenn die Komponente einen Windows::Foundation::DateTime-Wert an eine .NET Framework-Methode übergibt, akzeptiert die Framework-Methode ihn als DateTimeOffset.

```csharp
private void DateTimeExample()
{
    // Pass a System.DateTime to a C++ method
    // that takes a Windows::Foundation::DateTime
    DateTime dt = DateTime.Now;
    var nativeObject = new CppComponent.MyDateClass();
    nativeObject.SetTime(dt);

    // Retrieve a Windows::Foundation::DateTime as a
    // System.DateTimeOffset
    DateTimeOffset myDate = nativeObject.TimeStamp;

    // Print the long-form date string
    ResultText.Text += myDate.ToString();
}
```

## Sammlungen und Arrays


Sammlungen werden immer über die ABI-Grenze hinweg als Handles an Windows-Runtime-Typen wie z. B. Windows::Foundation::Collections::IVector^ und Windows::Foundation::Collections::IMap^ übergeben. Wenn Sie beispielsweise ein Handle für Platform::Collections::Map zurückzugeben, wird er implizit in Windows::Foundation::Collections::IMap^ konvertiert. Die Sammlungsschnittstellen werden in einem Namespace definiert, der von den C++-Klassen getrennt ist, die die konkrete Implementierung bereitstellen. JavaScript und .NET-Sprachen nutzen die Schnittstellen. Weitere Informationen finden Sie unter [Sammlungen (C++/CX)](https://msdn.microsoft.com//library/windows/apps/hh700103.aspx) und [Array und WriteOnlyArray (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh700131.aspx).

## Übergeben von „IVector“


```cpp
// Windows::Foundation::Collections::IVector across the ABI.
//#include <algorithm>
//#include <collection.h>
Windows::Foundation::Collections::IVector<int>^ SortVector(Windows::Foundation::Collections::IVector<int>^ vec)
{
    std::sort(begin(vec), end(vec));
    return vec;
}
```

```javascript
var nativeObject = new CppComponent.CollectionExample();
// Call the method to sort an integer array
var inVector = [14, 12, 45, 89, 23];
var outVector = nativeObject.sortVector(inVector);
var result = "Sorted vector to array:";
for (var i = 0; i < outVector.length; i++)
{
    outVector[i];
    result += outVector[i].toString() + ",";
}
document.getElementById('P6').innerHTML = result;
```

In den .NET-Sprachen wird „IVector&lt;T&gt;” als „IList&lt;T&gt;” betrachtet.

```csharp
private void SortListItems()
{
    IList<int> myList = new List<int>();
    myList.Add(5);
    myList.Add(9);
    myList.Add(17);
    myList.Add(2);

    var nativeObject = new CppComponent.CollectionExample();
    IList<int> mySortedList = nativeObject.SortVector(myList);

    foreach (var item in mySortedList)
    {
        ResultText.Text += " " + item.ToString();
    }
}
```

## Übergeben von „IMap“


```cpp
// #include <map>
//#include <collection.h>
Windows::Foundation::Collections::IMap<int, Platform::String^> ^GetMap(void)
{    
    Windows::Foundation::Collections::IMap<int, Platform::String^> ^ret = 
        ref new Platform::Collections::Map<int, Platform::String^>;
    ret->Insert(1, "One ");
    ret->Insert(2, "Two ");
    ret->Insert(3, "Three ");
    ret->Insert(4, "Four ");
    ret->Insert(5, "Five ");
    return ret;
}
```

```javascript
// Call the method to get the map
var outputMap = nativeObject.getMap();
var mStr = "Map result:" + outputMap.lookup(1) + outputMap.lookup(2)
    + outputMap.lookup(3) + outputMap.lookup(4) + outputMap.lookup(5);
document.getElementById('P7').innerHTML = mStr;
```

In den .NET-Sprachen wird „IMap” als „IDictionary&lt;K, V&gt;” betrachtet.

```csharp
private void GetDictionary()
{
    var nativeObject = new CppComponent.CollectionExample();
    IDictionary<int, string> d = nativeObject.GetMap();
    ResultText.Text += d[2].ToString();
}
```

## Eigenschaften


Eine öffentliche Verweisklasse in Visual C++-Komponentenerweiterungen macht öffentliche Datenmember anhand des Schlüsselworts „property” als Eigenschaften verfügbar. Das Konzept ist identisch mit .NET Framework-Eigenschaften. Eine triviale Eigenschaft ähnelt einem Datenmember, da ihre Funktionalität implizit ist. Eine nicht triviale Eigenschaft verfügt über explizite Get- und Set-Accessoren und über eine benannte private Variable, die der „Sicherungsspeicher" für den Wert ist. In diesem Beispiel ist die private Membervariable „\_propertyAValue” der Sicherungsspeicher für „PropertyA“. Eine Eigenschaft kann ein Ereignis auslösen, wenn sich ihr Wert ändert, und eine Client-App kann sich für den Empfang dieses Ereignisses registrieren.

```cpp
//Properties
public delegate void PropertyChangedHandler(Platform::Object^ sender, int arg);
public ref class PropertyExample  sealed
{
public:
    PropertyExample(){}

    // Event that is fired when PropertyA changes
    event PropertyChangedHandler^ PropertyChangedEvent;

    // Property that has custom setter/getter
    property int PropertyA
    {
        int get() { return m_propertyAValue; }
        void set(int propertyAValue) 
        {
            if (propertyAValue != m_propertyAValue)
            {
                m_propertyAValue = propertyAValue;
                // Fire event. (See event example below.)
                PropertyChangedEvent(this, propertyAValue);
            }
        }
    }

    // Trivial get/set property that has a compiler-generated backing store.
    property Platform::String^ PropertyB;

private:
    // Backing store for propertyA.
    int m_propertyAValue;
};
```

```javascript
var nativeObject = new CppComponent.PropertyExample();
var propValue = nativeObject.propertyA;
document.getElementById('P8').innerHTML = propValue;

//Set the string property
nativeObject.propertyB = "What is the meaning of the universe?";
document.getElementById('P9').innerHTML += nativeObject.propertyB;
```

Die .NET-Sprachen greifen auf Eigenschaften für ein systemeigenes C++-Objekt genauso wie bei einem .NET Framework-Objekt zu.

```csharp
private void GetAProperty()
{
    // Get the value of the integer property
    // Instantiate the C++ object
    var obj = new CppComponent.PropertyExample();

    // Get an integer property
    var propValue = obj.PropertyA;
    ResultText.Text += propValue.ToString();

    // Set a string property
    obj.PropertyB = " What is the meaning of the universe?";
    ResultText.Text += obj.PropertyB;

}
```

## Delegaten und Ereignisse


Ein Delegat ist ein Windows-Runtime-Typ, der ein Funktionsobjekt darstellt. Sie können mit Delegaten in Verbindung mit Ereignissen, Rückrufen und asynchronen Methodenaufrufen eine Aktion festlegen, die zu einem späteren Zeitpunkt ausgeführt werden soll. Wie ein Funktionsobjekt bietet der Delegat Typsicherheit, indem dem Compiler ermöglicht wird, den Rückgabetyp und die Parametertypen der Funktion zu überprüfen. Die Deklaration eines Delegaten ähnelt einer Funktionssignatur, die Implementierung ähnelt einer Klassendefinition und der Aufruf ähnelt einen Funktionsaufruf.

## Hinzufügen eines Ereignislisteners


Mit dem Schlüsselwort „event” können Sie einen öffentlichen Member eines bestimmten Delegattyps deklarieren. Clientcode abonniert das Ereignis anhand der Standardmechanismen, die die jeweilige Sprache bereitstellt.

```cpp
public:
    event SomeHandler^ someEvent;
```

In diesem Beispiel wird der gleiche C++-Code verwendet, wie im vorherigen Abschnitt „Eigenschaften".

```javascript
function Button_Click() {
    var nativeObj = new CppComponent.PropertyExample();
    // Define an event handler method
    var singlecasthandler = function (ev) {
        document.getElementById('P10').innerHTML = "The button was clicked and the value is " + ev;
    };

    // Subscribe to the event
    nativeObj.onpropertychangedevent = singlecasthandler;

    // Set the value of the property and fire the event
    var propValue = 21;
    nativeObj.propertyA = 2 * propValue;

}
```

In den .NET-Sprachen entspricht das Abonnieren eines Ereignisses in einer C++-Komponente dem Abonnieren eines Ereignisses in einer .NET Framework-Klasse:

```csharp
//Subscribe to event and call method that causes it to be fired.
private void TestMethod()
{
    var objWithEvent = new CppComponent.PropertyExample();
    objWithEvent.PropertyChangedEvent += objWithEvent_PropertyChangedEvent;

    objWithEvent.PropertyA = 42;
}

//Event handler method
private void objWithEvent_PropertyChangedEvent(object __param0, int __param1)
{
    ResultText.Text = "the event was fired and the result is " +
         __param1.ToString();
}
```

## Hinzufügen mehrere Ereignislistener für ein Ereignis


JavaScript verfügt über eine addEventListener-Methode, mit der mehrere Handler ein einzelnes Ereignis abonnieren können.

```cpp
public delegate void SomeHandler(Platform::String^ str);

public ref class LangSample sealed
{
public:
    event SomeHandler^ someEvent;
    property Platform::String^ PropertyA;

    // Method that fires an event
    void FireEvent(Platform::String^ str)
    {
        someEvent(Platform::String::Concat(str, PropertyA->ToString()));
    }
    //...
};
```

```javascript
// Add two event handlers
var multicast1 = function (ev) {
    document.getElementById('P11').innerHTML = "Handler 1: " + ev.target;
};
var multicast2 = function (ev) {
    document.getElementById('P12').innerHTML = "Handler 2: " + ev.target;
};

var nativeObject = new CppComponent.LangSample();
//Subscribe to the same event
nativeObject.addEventListener("someevent", multicast1);
nativeObject.addEventListener("someevent", multicast2);

nativeObject.propertyA = "42";

// This method should fire an event
nativeObject.fireEvent("The answer is ");
```

In C# kann eine beliebige Anzahl von Ereignishandlern mit dem Operator += das Ereignis abonnieren, wie im vorherigen Beispiel gezeigt.

## Enumerationen


Eine Windows-Runtime-Enumeration in C++ wird mit „public class enum” deklariert. Sie ähnelt einer bereichsbezogenen Enumeration in Standard-C++.

```cpp
public enum class Direction {North, South, East, West};

public ref class EnumExampleClass sealed
{
public:
    property Direction CurrentDirection
    {
        Direction  get(){return m_direction; }
    }

private:
    Direction m_direction;
};
```

Enumerationswerte werden als ganze Zahlen zwischen C++ und JavaScript übergeben. Optional können Sie ein JavaScript-Objekt deklarieren, das dieselben benannten Werte wie die C++-Enumeration enthält, und es wie folgt verwenden.

```javascript
var Direction = { 0: "North", 1: "South", 2: "East", 3: "West" };
//. . .

var nativeObject = new CppComponent.EnumExampleClass();
var curDirection = nativeObject.currentDirection;
document.getElementById('P13').innerHTML =
Direction[curDirection];
```

C# und Visual Basic bieten Sprachunterstützung für Enumerationen. In diesen Sprachen wird eine öffentliche C++-Enumerationsklasse wie eine .NET Framework-Enumeration betrachtet.

## Asynchrone Methoden


Verwenden Sie die [task-Klasse (Concurrency Runtime)](https://msdn.microsoft.com/library/hh750113.aspx), um asynchrone Methoden zu nutzen, die von anderen Windows-Runtime-Objekten verfügbar gemacht werden. Weitere Informationen finden Sie unter [Aufgabenparallelität (Concurrency Runtime)](https://msdn.microsoft.com/library/dd492427.aspx).

Verwenden Sie die [create\_async](https://msdn.microsoft.com/library/hh750102.aspx)-Funktion, die in „ppltasks.h” definiert ist, um asynchrone Methoden in C++ zu implementieren. Weitere Informationen finden Sie unter [Erstellen von asynchronen Vorgängen in C++ für Windows Store-Apps](https://msdn.microsoft.com/library/vstudio/hh750082.aspx). Ein Beispiel finden Sie unter [Exemplarische Vorgehensweise: Erstellen einer einfachen Komponente für Windows-Runtime in C++ und Aufrufen der Komponente über JavaScript oder C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md). Die .NET-Sprachen verwenden asynchrone C++-Methoden genauso wie eine asynchrone Methode, die im .NET Framework definiert ist.

## Ausnahmen


Sie können jeden Ausnahmetyp auslösen, der von der Windows-Runtime definiert ist. Sie können keine benutzerdefinierten Typen von einem Windows-Runtime-Ausnahmetyp ableiten. Allerdings können Sie eine COMException auslösen und ein benutzerdefiniertes HRESULT bereitstellen, auf das der Code, der die Ausnahme abfängt, zugreifen kann. Es gibt keine Möglichkeit, eine benutzerdefinierte Meldung in einer COMException anzugeben.

## Tipps zum Debuggen


Beim Debuggen einer JavaScript-Lösung mit einer Komponenten-DLL können Sie den Debugger so einstellen, dass er das schrittweise Ausführen von Skripts oder das schrittweise Ausführen von systemeigenem Code in der Komponente, aber nicht beides gleichzeitig ermöglicht. Um die Einstellung zu ändern, wählen Sie im Projektmappen-Explorer den JavaScript-Projektknoten aus und dann „Eigenschaften”, „Debuggen”, „Debuggertyp”.

Achten Sie darauf, entsprechende Funktionen im Paket-Designer auszuwählen. Wenn Sie zum Beispiel eine Bilddatei in der Bildbibliothek des Benutzers mit den Windows-Runtime-APIs öffnen möchten, müssen Sie das Kontrollkästchen „Bildbibliothek” im Bereich „Funktionen” des Manifest-Designers aktivieren.

Wenn der JavaScript-Code die öffentlichen Eigenschaften oder Methoden in der Komponente nicht zu erkennen scheint, stellen Sie sicher, dass Sie in JavaScript die Kamelschreibweise verwenden. Auf die C++-Methode „LogCalc” muss in JavaScript beispielsweise als „logCalc” verwiesen werden.

Wenn Sie ein C++-Komponentenprojekt für Windows-Runtime aus einer Projektmappe entfernen, müssen Sie den Projektverweis auch aus dem JavaScript-Projekt manuell entfernen. Andernfalls werden nachfolgende Debug- oder Buildvorgänge verhindert. Bei Bedarf können Sie dann einen Assemblyverweis zur DLL-Datei hinzufügen.

## Verwandte Themen

* [Exemplarische Vorgehensweise: Erstellen einer einfachen Komponente für Windows-Runtime in C++ und Aufrufen der Komponente über JavaScript oder C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md)



<!--HONumber=Mar16_HO1-->


