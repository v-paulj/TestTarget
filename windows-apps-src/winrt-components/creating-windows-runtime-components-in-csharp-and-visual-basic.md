---
author: msatranjr
title: "Erstellen von Komponenten für Windows-Runtime in C# und Visual Basic"
description: "Ab .NET Framework 4.5 können Sie mit verwaltetem Code eigene Windows-Runtime-Typen erstellen, die in einer Komponente für Windows-Runtime gepackt sind."
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.sourcegitcommit: 4c32b134c704fa0e4534bc4ba8d045e671c89442
ms.openlocfilehash: e8fd48b99d6a05af57e67e503c7bd3058b07569c

---

# Erstellen von Komponenten für Windows-Runtime in C# und Visual Basic


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \].

Ab .NET Framework4.5 können Sie mit verwaltetem Code eigene Windows-Runtime-Typen erstellen, die in einer Komponente für Windows-Runtime gepackt sind. Diese Komponente können Sie in UWP-Apps (Universelle Windows-Plattform) mit C++, JavaScript, Visual Basic oder C# verwenden. In diesem Artikel werden die Regeln zum Erstellen einer Komponente und einige Aspekte der .NET Framework-Unterstützung für die Windows-Runtime erläutert. Im Allgemeinen ist diese Unterstützung allen .NET Framework-Programmierern klar. Wenn Sie aber eine Komponente erstellen, die mit JavaScript oder C++ verwendet werden soll, müssen Sie auf die Unterschiede bei der Unterstützung der Windows-Runtime durch diese Sprachen achten.

Wenn Sie eine Komponente nur für die Verwendung in UWP-Apps mit Visual Basic oder C# erstellen und die Komponente keine UWP-Steuerelemente enthält, sollten Sie die Verwendung der Vorlage **Klassenbibliothek** anstelle der Vorlage **Komponente für Windows-Runtime** in Betracht ziehen. Eine einfache Klassenbibliothek weist weniger Einschränkungen auf.

Dieser Artikel enthält folgende Abschnitte:

## Deklarieren von Typen in Komponenten für Windows-Runtime


Intern können die Windows-Runtime-Typen in Ihrer Komponente alle .NET Framework-Funktionen verwenden, die in einer universellen Windows-App zulässig sind. (Weitere Informationen finden Sie unter [.NET für UWP-Apps](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx).) Extern können die Member Ihrer Typen nur Windows-Runtime-Typen für ihre Parameter und Rückgabewerte verfügbar machen. In der folgenden Liste sind die Einschränkungen für .NET Framework-Typen aufgeführt, die von Komponenten für Windows-Runtime verfügbar gemacht werden.

-   Die Felder, Parameter und Rückgabewerte aller öffentlichen Typen und Member in der Komponente müssen Windows-Runtime-Typen sein.

    Diese Einschränkung betrifft die von Ihnen erstellten Windows-Runtime-Typen und die Typen, die von der Windows-Runtime bereitgestellt werden. Außerdem trifft sie auf einige .NET Framework-Typen zu. Die Aufnahme dieser Typen ist Teil der Unterstützung, die das .NET Framework bietet, um die natürliche Verwendung der Windows-Runtime in verwaltetem Code zu ermöglichen: Es scheint, als verwende der Code bekannte .NET Framework-Typen anstelle der zugrunde liegenden Windows-Runtime-Typen. Beispielsweise können Sie primitive .NET Framework-Typen wie Int32 und Double, bestimmte grundlegende Typen wie DateTimeOffset und Uri sowie einige häufig verwendete generische Schnittstellentypen wie IEnumerable&lt;T&gt; (IEnumerable(Of T) in Visual Basic) und IDictionary&lt;TKey,TValue&gt; verwenden. (Beachten Sie, dass die Typargumente dieser generischen Typen Windows-Runtime-Typen sein müssen.) Dies wird in den Abschnitten „Übergeben von Windows-Runtime-Typen an verwalteten Code” und „Übergeben von verwalteten Typen an die Windows-Runtime” weiter unten in diesem Artikel erläutert.

-   Öffentliche Klassen und Schnittstellen können Methoden, Eigenschaften und Ereignisse enthalten. Sie können Delegaten für Ereignisse deklarieren oder den EventHandler&lt;T&gt;-Delegaten verwenden. Eine öffentliche Klasse oder Schnittstelle kann nicht:

    -   generisch sein.
    -   eine Schnittstelle implementieren, die keine Windows-Runtime-Schnittstelle ist. (Sie können aber eigene Windows-Runtime-Schnittstellen erstellen und implementieren.)
    -   von Typen abgeleitet sein, die nicht in der Windows-Runtime vorkommen, wie System.Exception und System.EventArgs.
-   Alle öffentliche Typen müssen über einen Stammnamespace verfügen, der mit dem Assemblynamen übereinstimmt, und der Assemblyname darf nicht mit „Windows” beginnen.

    > 
            **Tipp**  Standardmäßig entsprechen die Namespacenamen in Visual Studio-Projekten den Assemblynamen. In Visual Basic wird die Namespace-Anweisung für diesen Standardnamespace nicht im Code angezeigt.

-   Öffentliche Strukturen können nur öffentliche Felder als Member enthalten, und diese Felder müssen Werttypen oder Zeichenfolgen sein.
-   Öffentliche Klassen müssen **versiegelt** (**NotInheritable** in Visual Basic) sein. Wenn das Programmiermodell Polymorphie erfordert, können Sie eine öffentliche Schnittstelle erstellen und diese in den Klassen implementieren, die polymorph sein müssen.

## Debuggen der Komponente


Wenn Ihre universelle Windows-App und Ihre Komponente mit verwaltetem Code erstellt wurden, können Sie sie gleichzeitig debuggen.

Wenn Sie Ihre Komponente als Teil einer universellen Windows-App mit C++ testen, können Sie verwalteten und systemeigenen Code gleichzeitig debuggen. Die Vorgabe ist nur systemeigener Code.

## **So debuggen Sie systemeigenen C++-Code und verwalteten Code**

1.  Öffnen Sie das Kontextmenü für das Visual C++-Projekt, und wählen Sie **Eigenschaften** aus.
2.  Wählen Sie auf den Eigenschaftenseiten unter **Konfigurationseigenschaften** die Option **Debuggen** aus.
3.  Wählen Sie **Debuggertyp** aus, und ändern Sie dann in der Dropdownliste **Nur systemeigen** in **Gemischt (verwaltet und systemeigen)**. Klicken Sie auf **OK**.
4.  Legen Sie Haltepunkte im systemeigenen und verwalteten Code fest.

Wenn Sie die Komponenten als Teil einer universellen Windows-App mit JavaScript testen, befindet sich die Projektmappe standardmäßig im JavaScript-Debugmodus. In Visual Studio ist das gleichzeitige Debuggen von JavaScript und verwaltetem Code nicht möglich.

## **So debuggen Sie verwalteten Code anstelle von JavaScript**

1.  Öffnen Sie das Kontextmenü für das JavaScript-Projekt, und wählen Sie **Eigenschaften** aus.
2.  Wählen Sie auf den Eigenschaftenseiten unter **Konfigurationseigenschaften** die Option **Debuggen** aus.
3.  Wählen Sie **Debuggertyp** aus, und ändern Sie in der Dropdownliste **Nur Skript** in **Nur verwaltet**. Klicken Sie auf **OK**.
4.  Legen Sie Haltepunkte im verwaltetem Code fest, und debuggen Sie wie gewohnt.

## Übergeben von Windows-Runtime Typen an verwalteten Code


Wie bereits im Abschnitt „Deklarieren von Typen in Komponenten für Windows-Runtime” erwähnt, können bestimmte .NET Framework-Typen in den Signaturen von Membern öffentlicher Klassen erscheinen. Dies ist Teil der Unterstützung, die .NET Framework bietet, um die natürliche Verwendung der Windows-Runtime in verwaltetem Code zu ermöglichen. Darin sind primitive Typen und einige Klassen und Schnittstellen enthalten. Wenn eine Komponente von JavaScript oder C++-Code verwendet wird, müssen Sie wissen, wie die .NET Framework-Typen für den Aufrufer angezeigt werden. Beispiele mit JavaScript finden Sie unter [Exemplarische Vorgehensweise: Erstellen einer einfachen Komponente in C# oder Visual Basic und Aufrufen dieser Komponente über JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md). In diesem Abschnitt werden häufig verwendete Typen beschrieben.

Im .NET Framework haben primitive Typen wie die Int32-Struktur viele nützliche Eigenschaften und Methoden, z.B. die TryParse-Methode. Im Gegensatz dazu haben primitive Typen und Strukturen in der Windows-Runtime nur Felder. Wenn Sie diese Typen an verwalteten Code übergeben, verhalten sie sich wie .NET Framework-Typen, und Sie können die Eigenschaften und Methoden der .NET Framework-Typen wie gewohnt verwenden. Die folgende Liste fasst die Ersetzungen zusammen, die automatisch in der IDE vorgenommen werden:

-   Verwenden Sie für die primitiven Windows-Runtime Int32, Int64, Single, Double, Boolean, String (eine unveränderliche Sammlung von Unicode-Zeichen), Enum, UInt32, UInt64 und Guid den Typ mit dem gleichen Namen im System-Namespace.
-   Verwenden Sie für UInt8 den Typ System.Byte.
-   Verwenden Sie für Char16 den Typ System.Char.
-   Verwenden Sie für die IInspectable-Schnittstelle System.Object.

Wenn C# oder Visual Basic ein Schlüsselwort für einen dieser Typen bereitstellt, können Sie stattdessen das Schlüsselwort verwenden.

Zusätzlich zu primitiven Typen erscheinen einige grundlegende, häufig verwendete Windows-Runtime-Typen in verwaltetem Code als ihre .NET Framework-Entsprechung. Wenn Ihr JavaScript-Code beispielsweise die Windows.Foundation.Uri-Klasse verwendet und Sie diese einer C#- oder Visual Basic-Methode übergeben möchten, entspricht der Typ im verwalteten Code der .NET Framework-Klasse System.Uri und das ist der Typ für den Methodenparameter. Sie können feststellen, wann ein Windows-Runtime-Typ als .NET Framework-Typ erscheint, da IntelliSense in Visual Studio den Windows-Runtime-Typ ausblendet, wenn Sie verwalteten Code schreiben und den entsprechenden .NET Framework-Typ anzeigt. (In der Regel haben beiden Typen den gleichen Namen. Beachten Sie jedoch, dass die Windows.Foundation.DateTime-Struktur in verwaltetem Code als System.DateTimeOffset und nicht als System.DateTime angezeigt wird.)

Für einige häufig verwendete Sammlungstypen erfolgt die Zuordnung zwischen den Schnittstellen, die von einem Windows-Runtime-Typ implementiert werden, und den Schnittstellen, die vom entsprechenden .NET Framework-Typ implementiert werden. Wie bei den bereits erwähnten Typen deklarieren Sie Parametertypen, indem Sie den .NET Framework-Typ verwenden. Dadurch werden bestimmte Unterschiede zwischen den Typen ausgeblendet, und das Schreiben von .NET Framework-Code wirkt natürlicher. In der folgenden Tabelle sind die häufigsten dieser generischen Schnittstellentypen zusammen mit anderen allgemeinen Klassen- und Schnittstellenzuordnungen aufgeführt. Eine vollständige Liste der Windows-Runtime-Typen, die das .NET Framework zuordnet, finden Sie unter „.NET Framework-Zuordnungen von Windows-Runtime-Typen”.

| Windows-Runtime                                  | .NET Framework                                    |
|--------------------------------------------------|---------------------------------------------------|
| IIterable&lt;T&gt;                               | IEnumerable&lt;T&gt;                              |
| IVector&lt;T&gt;                                 | IList&lt;T&gt;                                    |
| IVectorView&lt;T&gt;                             | IReadOnlyList&lt;T&gt;                            |
| IMap&lt;K, V&gt;                                 | IDictionary&lt;TKey, TValue&gt;                   |
| IMapView&lt;K, V&gt;                             | IReadOnlyDictionary&lt;TKey, TValue&gt;           |
| IKeyValuePair&lt;K, V&gt;                        | KeyValuePair&lt;TKey, TValue&gt;                  |
| IBindableIterable                                | IEnumerable                                       |
| IBindableVecinr                                  | IList                                             |
| Windows.UI.Xaml.Data.INotifyPropertyChanged      | System.ComponentModel.INotifyPropertyChanged      |
| Windows.UI.Xaml.Data.PropertyChangedEventHandler | System.ComponentModel.PropertyChangedEventHandler |
| Windows.UI.Xaml.Data.PropertyChangedEventArgs    | System.ComponentModel.PropertyChangedEventArgs    |

 

Wenn ein Typ mehrere Schnittstellen implementiert, können Sie jede Schnittstelle verwenden, die als Parametertyp oder Rückgabetyp eines Members implementiert wird. Beispielsweise können Sie Dictionary&lt;int, string&gt; (Dictionary(Of Integer, String) in Visual Basic) als IDictionary&lt;int, string&gt;, IReadOnlyDictionary&lt;int, string&gt; oder IEnumerable&lt;System.Collections.Generic.KeyValuePair&lt;TKey, TValue&gt;&gt; übergeben oder zurückgeben.


            **Wichtig**  JavaScript verwendet die Schnittstelle, die zuerst in der Liste der Schnittstellen angezeigt wird, die ein verwalteter Typ implementiert. Wenn Sie beispielsweise Dictionary&lt;int, string&gt; an JavaScript-Code zurückgeben, wird IDictionary&lt;int, string&gt; angezeigt, unabhängig davon, welche Schnittstelle Sie als Rückgabetyp angeben. Das bedeutet, dass die erste Schnittstelle Member enthalten muss, die in den nächsten Schnittstellen erscheinen, damit diese Member für JavaScript sichtbar sind.

In der Windows-Runtime werden IMap&lt;K, V&gt; und IMapView&lt;K, V&gt; mit IKeyValuePair durchlaufen. Wenn Sie diese Schnittstellen an verwalteten Code übergeben, werden sie als IDictionary&lt;TKey, TValue&gt; und IReadOnlyDictionary&lt;TKey, TValue&gt; angezeigt. Daher können Sie sie mit System.Collections.Generic.KeyValuePair&lt;TKey, TValue&gt; auflisten.

Die Darstellungsweise von Schnittstellen in verwaltetem Code wirkt sich auf die Darstellungsweise der Typen aus, die diese Schnittstellen implementieren. Die PropertySet-Klasse implementiert z.B. den Typ IMap&lt;K, V&gt;, der in verwaltetem Code als IDictionary&lt;TKey, TValue&gt; erscheint. PropertySet wird angezeigt, als ob es IDictionary&lt;TKey, TValue&gt; anstelle von IMap&lt;K, V&gt; implementiert. In verwaltetem Code ist scheinbar eine Add-Methode vorhanden, die sich wie eine Add-Methode in .NET Framework-Wörterbüchern verhält. Eine Insert-Methode ist scheinbar nicht vorhanden. Sie finden dieses Beispiel im Artikel [Exemplarische Vorgehensweise: Erstellen einer einfachen Komponente in C# oder Visual Basic und Aufrufen dieser Komponente über JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## Übergeben von verwalteten Typen an die Windows-Runtime


Wie bereits im vorherigen Abschnitt erwähnt, können einige Windows-Runtime-Typen als .NET Framework-Typen in den Signaturen von Komponentenmembern oder in den Signaturen von Windows-Runtime-Membern erscheinen, wenn Sie sie in der IDE verwenden. Wenn Sie .NET Framework-Typen an diese Member übergeben oder als Rückgabewerte von Komponentenmembern verwenden, werden sie dem Code auf der anderen Seite als der entsprechende Windows-Runtime-Typ dargestellt. Sie finden einige Beispiele für die Auswirkungen beim Aufruf Ihrer Komponente in JavaScript im Abschnitt „Zurückgeben verwalteter Typen aus der Komponente” unter [Exemplarische Vorgehensweise: Erstellen einer einfachen Komponente in C# oder Visual Basic und Aufrufen dieser Komponente über JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## Überladene Methoden


In der Windows-Runtime können Methoden überladen werden. Wenn Sie aber mehrere Überladungen mit der gleichen Anzahl von Parametern deklarieren, müssen Sie das Attribut [Windows.Foundation.Metadata.DefaultOverloadAttribute](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.defaultoverloadattribute.aspx) nur auf eine dieser Überladungen anwenden. Nur diese Überladung kann aus JavaScript aufgerufen werden. Im folgenden Code ist beispielsweise die Überladung, die **int** übernimmt (**Integer** in Visual Basic), die Standardüberladung.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public string OverloadExample(string s)
> {
>     return s;
> }
> [Windows.Foundation.Metadata.DefaultOverload()]
> public int OverloadExample(int x)
> {
>     return x;
> }
> ```
> ```vb
> Public Function OverloadExample(ByVal s As String) As String
>     Return s
> End Function
> <Windows.Foundation.Metadata.DefaultOverload> _
> Public Function OverloadExample(ByVal x As Integer) As Integer
>     Return x
> End Function
> ```

 [!div class="tabbedCodeSnippets"] 
            **Achtung**  JavaScript ermöglicht die Übergabe beliebiger Werte an „OverloadExample“ und wandelt den Wert in den Typ um, der für den Parameter erforderlich ist. Sie können OverloadExample mit „zweiundvierzig”, „42” oder „42,3” aufrufen. Alle diese Werte werden an die Standardüberladung übergeben.

Die Standardüberladung im vorherigen Beispiel gibt 0, 42 bzw. 42 zurück. Das DefaultOverloadAttribute-Attribut können Sie nicht für Konstruktoren verwenden.

## Alle Konstruktoren in einer Klasse müssen eine unterschiedliche Anzahl von Parametern aufweisen.


Implementieren von IStringable Ab Windows8.1 enthält die Windows-Runtime eine IStringable-Schnittstelle, deren einzige Methode, IStringable.ToString, eine grundlegende Formatierungsunterstützung bietet, die mit der von Object.ToString vergleichbar ist.

-   Wenn Sie IStringable in einem öffentlichen, verwalteten Typ implementieren möchten, der in eine Komponente für Windows-Runtime exportiert wird, gelten folgende Einschränkungen:

    ```cs
    public class NewClass : IStringable
    ```

    Sie können die IStringable-Schnittstelle nur in einer „class implements”-Beziehung definieren, also in C#-Code:

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   Oder in Visual Basic-Code:
-   Sie können IStringable nicht in einer Schnittstelle implementieren.
-   Sie können einen Parameter nicht als IStringable-Typ deklarieren.
-   IStringable kann nicht der Rückgabetyp einer Methode, einer Eigenschaft oder eines Felds sein.

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    Sie können die IStringable-Implementierung nicht vor Basisklassen verbergen, indem Sie eine Methodendefinition wie die folgende verwenden: Stattdessen muss die IStringable.ToString-Implementierung immer die Implementierung der Basisklasse überschreiben.

Sie können eine ToString-Implementierung nur verbergen, indem Sie sie für eine stark typisierte Klasseninstanz aufrufen.

## Unter bestimmten Bedingungen können Aufrufe eines verwalteten Typs, der IStringable implementiert oder seine ToString-Implementierung verbirgt, aus systemeigenem Code zu unerwartetem Verhalten führen.


Asynchrone Vorgänge

Um eine asynchrone Methode in Ihrer Komponente zu implementieren, fügen Sie am Ende des Methodennamens „Async“ hinzu und geben eine der Windows-Runtime-Schnittstellen zurück, die asynchrone Aktionen oder Vorgänge repräsentieren: IAsyncAction, IAsyncActionWithProgress&lt;TProgress&gt;, IAsyncOperation&lt;TResult&gt; oder IAsyncOperationWithProgress&lt;TResult, TProgress&gt;. Sie können mit .NET Framework-Aufgaben (die [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)-Klasse und die generische [Task&lt;TResult&gt;](https://msdn.microsoft.com/library/dd321424.aspx)-Klasse) eine asynchrone Methode implementieren. Sie müssen eine Aufgabe zurückgeben, die einen laufenden Vorgang darstellt, wie z.B. eine Aufgabe, die von einer asynchronen Methode zurückgegeben wird, die in C# oder Visual Basic geschrieben wurde, oder eine Aufgabe, die von der [Task.Run](https://msdn.microsoft.com/library/system.threading.tasks.task.run.aspx)-Methode zurückgegeben wird.

Wenn Sie für die Aufgabenerstellung einen Konstruktor verwenden, müssen Sie seine [Task.Start](https://msdn.microsoft.com/library/system.threading.tasks.task.start.aspx)-Methode vor der Rückgabe aufrufen. Eine Methode, die „await” verwendet („Await” in Visual Basic) benötigt das Schlüsselwort **async** (**Async** in Visual Basic).

Wenn Sie eine solche Methode aus einer Komponente für Windows-Runtime verfügbar machen, müssen Sie das Schlüsselwort **async** für den Delegaten verwenden, den Sie an die Run-Methode übergeben. Für asynchrone Aktionen und Vorgänge, die weder die Abbruch- noch die Fortschrittsberichterstattung unterstützen, können Sie mit der Erweiterungsmethode [WindowsRuntimeSystemExtensions.AsAsyncAction](https://msdn.microsoft.com/library/system.windowsruntimesystemextensions.asasyncaction.aspx) oder [AsAsyncOperation&lt;TResult&gt;](https://msdn.microsoft.com/library/hh779745.aspx) die Aufgabe in die entsprechende Schnittstelle umschließen. Der folgende Code implementiert z.B. eine asynchrone Methode, indem mit der Task.Run&lt;TResult&gt;-Methode eine Aufgabe gestartet wird.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
> {
>     return Task.Run<IList<string>>(async () =>
>     {
>         var data = await DownloadDataAsync(id);
>         return ExtractStrings(data);
>     }).AsAsyncOperation();
> }
> ```
> ```vb
> Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
>      As IAsyncOperation(Of IList(Of String))
>
>     Return Task.Run(Of IList(Of String))(
>         Async Function()
>             Dim data = Await DownloadDataAsync(id)
>             Return ExtractStrings(data)
>         End Function).AsAsyncOperation()
> End Function
> ```

Die Erweiterungsmethode „AsAsyncOperation&lt;TResult&gt;“ gibt die Aufgabe als asynchronen Windows-Runtime-Vorgang zurück. [!div class="tabbedCodeSnippets"] Im folgenden JavaScript-Code wird gezeigt, wie die Methode mit einem [WinJS.Promise](https://msdn.microsoft.com/library/windows/apps/br211867.aspx)-Objekt aufgerufen werden kann.

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

Die an die then-Methode übergebene Funktion wird ausgeführt, wenn der asynchrone Aufruf abgeschlossen ist. Der StringList-Parameter enthält die Liste der Zeichenfolgen, die von der DownloadAsStringAsync-Methode zurückgegeben wird. Die Funktion übernimmt die gewünschte Verarbeitung.

Verwenden Sie für asynchrone Aktionen und Vorgänge, die die Abbruch- oder die Fortschrittsberichterstattung unterstützen, die [AsyncInfo](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.asyncinfo.aspx)-Klasse, um eine gestartete Aufgabe zu generieren und die Abbruch- und Fortschrittsberichterstattungsfunktionen der Aufgabe mit den Abbruch- und Fortschrittsberichterstattungsfunktionen der entsprechenden Windows-Runtime-Schnittstelle zu verknüpfen. Ein Beispiel, das sowohl die Abbruch- als auch die Fortschrittsberichterstattung unterstützt, finden Sie unter [Exemplarische Vorgehensweise: Erstellen einer einfachen Komponente in C# oder Visual Basic und Aufrufen dieser Komponente über JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md). Sie können die Methoden der AsyncInfo-Klasse auch verwenden, wenn Ihre asynchrone Methode die Abbruch- oder Fortschrittsberichterstattung nicht unterstützt. Geben Sie keine Parameter für das Token und die [IProgress&lt;T&gt;](https://msdn.microsoft.com/library/hh138298.aspx)-Schnittstelle an, wenn Sie eine Visual Basic-Lambda-Funktion oder eine anonyme C#-Methode verwenden.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
> {
>     return AsyncInfo.Run<IList<string>>(async (token) =>
>     {
>         var data = await DownloadDataAsync(id);
>         return ExtractStrings(data);
>     });
> }
> ```
> ```vb
> Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
>     As IAsyncOperation(Of IList(Of String))
>
>     Return AsyncInfo.Run(Of IList(Of String))(
>         Async Function()
>             Dim data = Await DownloadDataAsync(id)
>             Return ExtractStrings(data)
>         End Function)
> End Function
> ```

Wenn Sie eine C#-Lambda-Funktion verwenden, geben Sie einen Tokenparameter an, aber ignorieren Sie ihn.

## Wenn Sie für das vorherige Beispiel, in dem die AsAsyncOperation&lt;TResult&gt;-Methode verwendet wird, die  [AsyncInfo.Run&lt;TResult&gt;(Func&lt;CancellationToken, Task&lt;TResult&gt;&gt;](https://msdn.microsoft.com/library/hh779740.aspx))-Überladungsmethode verwenden, sieht es wie folgt aus:


[!div class="tabbedCodeSnippets"] Wenn Sie eine asynchrone Methode erstellen, die optional die Abbruch- oder Fortschrittsberichterstattung unterstützt, sollten Sie das Hinzufügen von Überladungen in Betracht ziehen, die keine Parameter für ein Abbruchtoken oder die IProgress&lt;T&gt;-Schnittstelle haben.

Auslösen von Ausnahmen Sie können jeden Ausnahmetyp auslösen, der in .NET für Windows-Apps enthalten ist.

-   Sie können keine eigenen öffentlichen Ausnahmetypen in einer Komponente für Windows-Runtime deklarieren, aber Sie können nicht öffentliche Typen deklarieren und auslösen. Wenn die Komponente die Ausnahme nicht behandelt, wird eine entsprechende Ausnahme im Code ausgelöst, der die Komponente aufgerufen hat. Die Unterstützung der Windows-Runtime durch die aufrufende Sprache bestimmt, wie die Ausnahme dem Aufrufer dargestellt wird.

    > In JavaScript erscheint die Ausnahme als Objekt, in dem die Ausnahmemeldung durch eine Stapelüberwachung ersetzt ist. Wenn Sie Ihre App in Visual Studio debuggen, wird der Originaltext der Meldung im Ausnahmedialogfeld des Debuggers unter „WinRT Information" angezeigt.

-   Sie können mit JavaScript-Code nicht auf den Originaltext der Meldung zugreifen. 
            **Tipp**  Momentan enthält die Stapelüberwachung zwar den verwalteten Ausnahmetyp, aber es ist nicht empfehlenswert, diese zu analysieren, um den Ausnahmetyp zu ermitteln. Verwenden Sie stattdessen einen HRESULT-Wert, wie weiter unten in diesem Abschnitt beschrieben. In C++ erscheint die Ausnahme als Plattformausnahme. Wenn die HResult-Eigenschaft der verwalteten Ausnahme dem HRESULT einer bestimmten Plattformausnahme zugeordnet werden kann, wird die jeweilige Ausnahme verwendet. Andernfalls wird eine [Platform::COMException](https://msdn.microsoft.com/library/windows/apps/xaml/hh710414.aspx)-Ausnahme ausgelöst.
-   Der Meldungstext der verwalteten Ausnahme ist für C++-Code nicht verfügbar.

Wenn eine bestimmte Plattformausnahme ausgelöst wurde, erscheint der Meldungstext für diesen Ausnahmetyp. Andernfalls wird kein Meldungstext ausgegeben. Siehe [Ausnahmen (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699896.aspx).

> In C# oder Visual Basic ist die Ausnahme eine normale verwaltete Ausnahme. Wenn Sie in Ihrer Komponente eine Ausnahme auslösen, sollten Sie einen nicht öffentlichen Ausnahmetyp verwenden, dessen HResult-Eigenschaftswert speziell für Ihre Komponente gilt, damit die Ausnahme leichter von einem JavaScript- oder C++-Aufrufer verwaltet werden kann.

## HRESULT ist für einen JavaScript-Aufrufer über die Eigenschaft „number” des Ausnahmeobjekts verfügbar und für einen C++-Aufrufer über die Eigenschaft [COMException::HResult](https://msdn.microsoft.com/library/windows/apps/xaml/hh710415.aspx).


            **Hinweis**  Verwenden Sie einen negativen Wert für HRESULT. Ein positiver Wert wird als Erfolg interpretiert und im JavaScript- oder C++-Aufrufer wird keine Ausnahme ausgelöst. Deklarieren und Auslösen von Ereignissen

Wenn Sie einen Typ deklarieren, um die Daten für das Ereignis aufzunehmen, leiten Sie diesen von „Object“ und nicht von „EventArgs“ ab, da „EventArgs“ kein Windows-Runtime-Typ ist. Verwenden Sie [EventHandler&lt;TEventArgs&gt;](https://msdn.microsoft.com/library/db0etb8x.aspx) als Typ des Ereignisses, und verwenden Sie den Ereignisargumenttyp als generisches Typargument. Lösen Sie das Ereignis genauso wie in einer .NET Framework-Anwendung aus.

Wenn Ihre Komponente für Windows-Runtime von JavaScript oder C++ verwendet wird, folgt das Ereignis dem Windows-Runtime-Ereignismuster, das diese Sprachen erwarten. Wenn Sie die Komponente in C# oder Visual Basic verwenden, erscheint das Ereignis als normales .NET Framework-Ereignis. Ein Beispiel finden Sie unter [Exemplarische Vorgehensweise: Erstellen einer einfachen Komponente in C# oder Visual Basic und Aufrufen dieser Komponente über JavaScript]().

## Wenn Sie benutzerdefinierte Ereignisaccessoren implementieren (in Visual Basic ein Ereignis mit dem Schlüsselwort **Custom** deklarieren), müssen Sie in der Implementierung das Windows-Runtime-Ereignismuster verwenden.


Siehe [Benutzerdefinierte Ereignisse und Ereignisaccessoren in Komponenten für Windows-Runtime](custom-events-and-event-accessors-in-windows-runtime-components.md). Beachten Sie, dass das Ereignis auch dann als einfaches .NET Framework-Ereignis erscheint, wenn Sie C#- oder Visual Basic-Code verwenden. Nächste Schritte

Nachdem Sie eine Komponente für Windows-Runtime für eigene Zwecke erstellt haben, stellen Sie möglicherweise fest, dass die Funktionen, die diese kapselt, für andere Entwickler nützlich sind.

## Sie haben zwei Optionen, um eine Komponente für die Verteilung an andere Entwickler zu packen.

* [Siehe [Verteilen einer verwalteten Komponente für Windows-Runtime](https://msdn.microsoft.com/library/jj614475.aspx).](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx)
* [Weitere Informationen zu Visual Basic- und C#-Sprachfunktionen und zur .NET Framework-Unterstützung für die Windows-Runtime finden Sie unter [Visual Basic- und C#-Programmiersprachenreferenz](https://msdn.microsoft.com/library/windows/apps/xaml/br212458.aspx).](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx)
* [Verwandte Themen](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)



<!--HONumber=Jun16_HO5-->


