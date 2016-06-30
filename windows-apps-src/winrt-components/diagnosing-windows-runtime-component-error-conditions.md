---
author: msatranjr
title: "Diagnostizieren von Fehlerbedingungen für Komponenten für Windows-Runtime"
description: "Dieser Artikel enthält zusätzliche Informationen zu Einschränkungen bei Komponenten für Windows-Runtime, die mit verwaltetem Code geschrieben wurden."
ms.assetid: CD0D0E11-E68A-411D-B92E-E9DECFDC9599
ms.sourcegitcommit: 4c32b134c704fa0e4534bc4ba8d045e671c89442
ms.openlocfilehash: 29199b7c94c4fecd173fb96f0d8fb43692d72464

---

# Diagnostizieren von Fehlerbedingungen für Komponenten für Windows-Runtime


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \].


Dieser Artikel enthält zusätzliche Informationen zu Einschränkungen bei Komponenten für Windows-Runtime, die mit verwaltetem Code geschrieben wurden. Der Artikel beinhaltet Details zu den Fehlermeldungen von [Winmdexp.exe (Windows Runtime Metadata Export Tool)](https://msdn.microsoft.com/library/hh925576.aspx) und ergänzt die unter [Erstellen von Komponenten für Windows-Runtime in C# und Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) aufgeführten Informationen zu Einschränkungen.

In diesem Artikel werden aber nicht alle Fehler abgedeckt. Die hier beschriebenen Fehler sind in allgemeine Kategorien eingeteilt, und jede Kategorie enthält eine Tabelle mit den zugehörigen Fehlermeldungen. Suchen Sie nach dem Meldungstext (ohne bestimmte Werte für Platzhalter) oder der Meldungsnummer. Falls Sie die benötigten Informationen hier nicht finden, können Sie mithilfe der Feedback-Schaltfläche am Ende dieses Artikels zur Verbesserung der Dokumentation beitragen. Geben Sie dabei die Fehlermeldung an. Alternativ können Sie einen Fehler auf der Microsoft Connect-Website melden.

## Fehlermeldung beim Implementieren einer asynchronen Schnittstelle stellt den falschen Typ bereit


Verwaltete Komponenten für Windows-Runtime können die UWP-Schnittstellen (Universelle Windows-Plattform) nicht implementieren, die asynchrone Aktionen oder Vorgänge darstellen ([IAsyncAction](https://msdn.microsoft.com/library/br205781.aspx), [IAsyncActionWithProgress&lt;TProgress&gt;](https://msdn.microsoft.com/library/br205784.aspx), [IAsyncOperation&lt;TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br206598.aspx) oder [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](https://msdn.microsoft.com/library/windows/apps/br206594.aspx)). Stattdessen stellt das .NET Framework die [AsyncInfo](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.asyncinfo.aspx) -Klasse zum Generieren von asynchronen Vorgängen in Komponenten für Windows-Runtime bereit. Die Fehlermeldung, die Winmdexp.exe bei dem Versuch anzeigt, eine asynchrone Schnittstelle zu implementieren, verweist auf diese Klasse mit ihrem früheren Namen, AsyncInfoFactory. Das .NET Framework enthält die AsyncInfoFactory-Klasse nicht mehr.

| Fehlernummer | Meldungstext                                                                                                                                                                                                                                                          |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1084      | Der Typ '{0}' implementiert die Async-Schnittstelle '{1}' für Windows-Runtime. Windows-Runtime-Typen dürfen keine Async-Schnittstellen implementieren. Verwenden Sie die System.Runtime.InteropServices.WindowsRuntime.AsyncInfo-Klasse, um asynchrone Vorgänge für den Export in die Windows-Runtime zu erstellen. |

 

> **Hinweis** In Fehlermeldungen, die auf die Windows-Runtime verweisen, wird ein alte Terminologie verwendet. Dies wird nun als die universelle Windows-Plattform (UWP) bezeichnet. Zum Beispiel werden Windows-Runtime-Typen jetzt als UWP-Typen bezeichnet.

 

## Fehlende Verweise auf mscorlib.dll oder System.Runtime.dll


Dieses Problem tritt nur auf, wenn Sie Winmdexp.exe aus der Befehlszeile ausführen. Wir empfehlen die Verwendung der Option „/reference”, um Verweise auf mscorlib.dll und System.Runtime.dll von den .NET Framework-Kernverweisassemblys einzuschließen, die sich in „%ProgramFiles(x86)%\\Reference Assemblies\\Microsoft\\Framework\\.NETCore\\v4.5" ("%ProgramFiles%\\...” auf einem 32-Bit-Computer) befinden.

| Fehlernummer | Meldungstext                                                                                                                                     |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1009      | Es wurde kein Verweis auf "mscorlib.dll" festgelegt. Für den ordnungsgemäßen Export ist ein Verweis auf diese Metadatendatei erforderlich.                               |
| WME1090      | Die Kernverweisassembly konnte nicht ermittelt werden. Stellen Sie sicher, dass mithilfe des /reference-Schalters auf "mscorlib.dll" und "System.Runtime.dll" verwiesen wird. |

 

## Operatorüberladung ist nicht zulässig


In einer Komponente für Windows-Runtime, die in verwaltetem Code geschrieben wurde, können Sie keine überladenen Operatoren für öffentliche Typen verfügbar machen.

> **Hinweis** In der Fehlermeldung wird der Operator über seinen Metadatennamen identifiziert, zum Beispiel op\_Addition, op\_Multiply, op\_ExclusiveOr, op\_Implicit (implizite Konvertierung) usw.

 

| Fehlernummer | Meldungstext                                                                                          |
|--------------|-------------------------------------------------------------------------------------------------------|
| WME1087      | '{0}' ist eine Überladung des Operators. Verwaltete Typen können in der Windows-Runtime keine überladenen Operatoren verfügbar machen. |

 

## Konstruktoren einer Klasse haben die gleiche Anzahl von Parametern


In der UWP kann eine Klasse nur einen Konstruktor mit einer bestimmten Anzahl von Parametern haben. Sie können z. B. keinen Konstruktor verwenden, der einen einzelnen Parameter vom Typ **String** und einen anderen einzelnen Parameter vom Typ **int** (**Integer** in Visual Basic) aufweist. Das Problem kann nur umgangen werden, indem Sie für jeden Konstruktor eine andere Anzahl von Parametern verwenden.

| Fehlernummer | Meldungstext                                                                                                                                            |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1099      | Typ '{0}' enthält mehrere Konstruktoren mit '{1}' Argument(en). Windows-Runtime-Typen dürfen nicht mehrere Konstruktoren mit derselben Anzahl von Argumenten enthalten. |

 

## Ein Standard für Überladungen mit derselben Anzahl von Parametern muss festgelegt werden


In der UWP können überladene Methoden nur dann über dieselbe Anzahl von Parametern verfügen, wenn eine Überladung als Standardüberladung angegeben wird. Weitere Informationen finden Sie unter [Erstellen von Komponenten für Windows-Runtime in C# und Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md).

| Fehlernummer | Meldungstext                                                                                                                                                                      |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1059      | Mehrere {0}-Parameterüberladungen von '{1}.{2}' sind mit Windows.Foundation.Metadata.DefaultOverloadAttribute versehen.                                                            |
| WME1085      | Für die {0}-Parameterüberladungen von {1}.{2} muss genau eine Methode als Standardüberladung angegeben werden, indem diese mit Windows.Foundation.Metadata.DefaultOverloadAttribute versehen wird. |

 

## Namespacefehler und ungültige Namen für die Ausgabedatei


In der universellen Windows-Plattform müssen sich alle öffentlichen Typen in einer Windows-Metadatendatei (WINMD) in einem Namespace mit demselben Namen wie die WINMD-Datei oder in Subnamespaces des Dateinamens befinden. Wenn Ihr Visual Studio-Projekt beispielsweise A.B heißt (d. h. die Komponente für Windows-Runtime ist A.B.WINMD), kann es die öffentlichen Klassen A.B.Class1 und A.B.C.Class2 enthalten, aber nicht A.Class3 (WME0006) oder D.Class4 (WME1044).

> **Hinweis**  Diese Einschränkungen gelten nur für öffentliche Typen, nicht für private Typen, die in der Implementierung verwendet werden.

 

Für A.Class3 können Sie Class3 in einen anderen Namespace verschieben oder den Namen der Komponente für Windows-Runtime in A.WINMD ändern. Obwohl WME0006 eine Warnung ist, sollten Sie sie als Fehler behandeln. Im vorherigen Beispiel kann A.Class3 nicht vom Code, der A.B.WINMD aufruft, gefunden werden.

Im Fall von D.Class4 kann kein Dateiname beides, D.Class4 und Klassen im A.B-Namespace, enthalten. Das Ändern des Namens der Komponente für Windows-Runtime ist daher keine Lösung. Sie können D.Class4 entweder in einen anderen Namespace verschieben oder in eine andere Komponente für Windows-Runtime einfügen.

Das Dateisystem kann nicht zwischen Groß- und Kleinbuchstaben unterscheiden. Namespaces mit unterschiedlicher Groß-/Kleinschreibung sind daher nicht zulässig (WME1067).

Die Komponente muss mindestens einen **public sealed**-Typ (**Public NotInheritable** in Visual Basic) enthalten. Andernfalls erhalten Sie die Fehlermeldung WME1042 oder WME1043, je nachdem, ob die Komponente private Typen enthält.

Ein Typ in einer Komponente für Windows-Runtime darf nicht wie ein Namespace benannt werden (WME1068).

> **Achtung**  Wenn Sie Winmdexp.exe direkt aufrufen und für die Benennung der Komponente für Windows-Runtime nicht die Option „/out” verwenden, versucht Winmdexp.exe, einen Namen zu generieren, der alle Namespaces in der Komponente enthält. Die Umbenennung von Namespaces kann zur Änderung des Komponentennamens führen.

 

| Fehlernummer | Meldungstext                                                                                                                                                                                                                                                                                                                                             |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME0006      | '{0}' ist kein gültiger WinMD-Dateiname für diese Assembly. Alle Typen in einer Windows-Metadatendatei müssen sich in einem Subnamespace des im Dateinamen enthaltenen Namespace befinden. Typen, die nicht in solch einem Subnamespace vorhanden sind, werden zur Laufzeit nicht gefunden. In dieser Assembly lautet der kleinste gemeinsame Namespace, der als Dateiname verwendet werden könnte, '{1}'. |
| WME1042      | Das Eingabemodul muss mindestens einen öffentlichen Typ enthalten, der sich in einem Namespace befindet.                                                                                                                                                                                                                                                                   |
| WME1043      | Das Eingabemodul muss mindestens einen öffentlichen Typ enthalten, der sich in einem Namespace befindet. In den Namespaces wurden nur private Typen gefunden.                                                                                                                                                                                                               |
| WME1044      | Ein öffentlicher Typ hat einen Namespace ('{1}'), der kein gemeinsames Präfix mit anderen Namespaces ('{0}') aufweist. Alle Typen in einer Windows-Metadatendatei müssen sich in einem Subnamespace des im Dateinamen enthaltenen Namespace befinden.                                                                                                                              |
| WME1067      | Namespace-Namen dürfen sich nicht nur in der Groß-/Kleinschreibung unterscheiden: '{0}', '{1}'.                                                                                                                                                                                                                                                                                                |
| WME1068      | Der Typ '{0}' darf nicht denselben Namen wie der Namespace '{1}' aufweisen.                                                                                                                                                                                                                                                                                                 |

 

## Exportieren von Typen, die keine gültigen universellen Windows-Plattform-Typen sind


Die öffentliche Schnittstelle der Komponente darf nur UWP-Typen verfügbar machen. Das .NET Framework stellt aber auch Zuordnungen für einige häufig verwendete Typen bereit, die im .NET Framework und in der UWP Unterschiede aufweisen. Somit können .NET Framework-Entwickler mit bekannten Typen arbeiten, anstatt sich erst mit neuen Typen vertraut machen zu müssen. Sie können diese zugeordneten .NET Framework-Typen in der öffentliche Schnittstelle der Komponente verwenden. Weitere Informationen hierzu finden Sie in „Deklarieren von Typen in Komponenten für Windows-Runtime” und „Übergeben von universellen Windows-Plattform-Typen an verwalteten Code” unter [Erstellen von Komponenten für Windows-Runtime in C# und Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)sowie unter [.NET Framework-Zuordnungen von Windows-Runtime-Typen](net-framework-mappings-of-windows-runtime-types.md).

Viele dieser Zuordnungen sind Schnittstellen. Zum Beispiel wird [IList&lt;T&gt;](https://msdn.microsoft.com/library/5y536ey6.aspx) der UWP-Schnittstelle [IVector&lt;T&gt;](https://msdn.microsoft.com/library/windows/apps/br206631.aspx) zugeordnet. Wenn Sie List&lt;string&gt; (`List(Of String)` in Visual Basic) anstelle von IList&lt;string&gt; als Parametertyp verwenden, stellt Winmdexp.exe eine Liste von Alternativen zur Verfügung, die alle von List&lt;T&gt; implementierten, zugeordneten Schnittstellen enthält. Bei geschachtelten Typen wie List&lt;Dictionary&lt;int, string&gt;&gt; (List(Of Dictionary(Of Integer, String)) in Visual Basic) bietet Winmdexp.exe verschiedene Optionen für jede Schachtelungsebene an. Diese Listen können recht umfangreich sein.

Im Allgemeinen sollte die Schnittstelle ausgewählt werden, die dem Typ am nächsten ist. Für Dictionary&lt;int, string&gt; ist beispielsweise IDictionary&lt;int, string&gt; am besten geeignet.

> **Wichtig**  JavaScript verwendet die Schnittstelle, die zuerst in der Liste der Schnittstellen angezeigt wird, die ein verwalteter Typ implementiert. Wenn Sie beispielsweise Dictionary&lt;int, string&gt; an JavaScript-Code zurückgeben, wird IDictionary&lt;int, string&gt; angezeigt, unabhängig davon, welche Schnittstelle Sie als Rückgabetyp angeben. Das bedeutet, dass die erste Schnittstelle Member enthalten muss, die in den nächsten Schnittstellen erscheinen, damit diese Member für JavaScript sichtbar sind.

> **Achtung**  Vermeiden Sie die nicht generischen Schnittstellen [IList](https://msdn.microsoft.com/library/system.collections.ilist.aspx) und [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx), wenn die Komponente von JavaScript verwendet wird. Diese Schnittstellen werden [IBindableVector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.interop.ibindablevector.aspx) und [IBindableIterator](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.interop.ibindableiterator.aspx) zugeordnet. Sie unterstützen die Bindung für XAML-Steuerelemente und sind für JavaScript nicht sichtbar. JavaScript generiert die Laufzeitfehlermeldung „Die Funktion '%s' kann aufgrund einer ungültigen Signatur nicht aufgerufen werden.”

 

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Fehlernummer</th>
<th align="left">Meldungstext</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">WME1033</td>
<td align="left">Die Methode '{0}' weist den Parameter '{1}' vom Typ '{1}' auf. '{2}' ist kein gültiger Windows-Runtime-Parametertyp.</td>
</tr>
<tr class="even">
<td align="left">WME1038</td>
<td align="left">Die Methode '{0}' weist in der Signatur einen Parameter vom Typ '{1}' auf. Obwohl dieser Typ kein gültiger Windows-Runtime-Typ ist, implementiert er Schnittstellen, bei denen es sich um gültige Windows-Runtime-Typen handelt. Ändern Sie die Methodensignatur stattdessen eventuell für die Verwendung eines der folgenden Typen: '{2}'.</td>
</tr>
<tr class="odd">
<td align="left">WME1039</td>
<td align="left"><p>Die Methode '{0}' weist in der Signatur einen Parameter vom Typ '{1}' auf. Obwohl es sich bei diesem generischen Typ nicht um einen gültigen Windows-Runtime-Typ handelt, werden von diesem Typ oder von dessen generischen Parametern Schnittstellen implementiert, die gültige Windows-Runtime-Typen sind. {2}</p>
> **Hinweis**  Für {2} fügt Winmdexp.exe eine Liste von Alternativen an, wie „Ändern Sie eventuell den Typ 'System.Collections.Generic.List&lt;T&gt;' in der Methodensignatur in einen der folgenden Typen: 'System.Collections.Generic.IList&lt;T&gt;, System.Collections.Generic.IReadOnlyList&lt;T&gt;, System.Collections.Generic.IEnumerable&lt;T&gt;'.”
</td>
</tr>
<tr class="even">
<td align="left">WME1040</td>
<td align="left">Die Methode '{0}' weist in der Signatur einen Parameter vom Typ '{1}' auf. Verwenden Sie anstelle eines verwalteten Aufgabentyps Windows.Foundation.IAsyncAction, Windows.Foundation.IAsyncOperation oder eine der anderen Async-Schnittstellen für Windows-Runtime. Das Standardmuster für .NET-Await wird auch auf diese Schnittstellen angewendet. Weitere Informationen zum Konvertieren von verwalteten Task-Objekten in Async-Schnittstellen für Windows-Runtime finden Sie unter „ System.Runtime.InteropServices.WindowsRuntime.AsyncInfo”.</td>
</tr>
</tbody>
</table>

 

## Strukturen, die Felder mit unzulässigen Typen enthalten


In der UWP kann eine Struktur nur Felder enthalten, und nur Strukturen können Felder enthalten. Diese Felder müssen öffentlich sein. Zu den gültigen Feldtypen zählen Strukturen, Enumerationen und primitive Typen.

| Fehlernummer | Meldungstext                                                                                                                                                                                                                                                            |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1060      | Die Struktur '{0}' weist das Feld '{1}' vom Typ '{1}' auf. '{2}' ist kein gültiger Windows-Runtime-Feldtyp. Die Felder in einer Windows-Runtime-Struktur müssen vom Typ UInt8, Int16, UInt16, Int32, UInt32, Int64, UInt64, Single, Double, Boolean, String, Enum sein oder selbst eine Struktur darstellen. |

 

## Einschränkungen für Arrays in Membersignaturen


In der UWP müssen Arrays in Membersignaturen eindimensional sein und eine Untergrenze von 0 (null) aufweisen. Geschachtelte Arraytypen wie `myArray[][]` (`myArray()()` in Visual Basic) sind nicht zulässig.

> **Hinweis** Diese Einschränkung gilt nicht für Arrays, die Sie intern in der Implementierung verwenden.

 

| Fehlernummer | Meldungstext                                                                                                                                                     |
|--------------|--------------------|
| WME1034      | Die Methode '{0}' weist ein Array des Typs '{1}' auf, dessen untere Grenze in der Signatur ungleich null ist. Arrays in Windows-Runtime-Methodensignaturen müssen eine Untergrenze von NULL aufweisen. |
| WME1035      | Die Methode '{0}' weist in der Signatur ein mehrdimensionales Array des Typs '{1}' auf. Arrays in Signaturen von Windows-Runtime-Methoden müssen eindimensional sein.                  |
| WME1036      | Die Methode '{0}' weist in der Signatur ein geschachteltes Array des Typs '{1}' auf. Arrays dürfen in Windows-Runtime-Methodensignaturen nicht geschachtelt sein.                                    |

 

## Arrayparameter müssen angeben, ob Arrayinhalt lesbar oder schreibbar sind


In der UWP müssen Parameter schreibgeschützt oder lesegeschützt sein. Parameter können nicht mit **ref** (**ByRef** ohne das [OutAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.outattribute.aspx)-Attribut in Visual Basic) gekennzeichnet werden. Dies gilt für den Inhalt von Arrays. Daher müssen Arrayparameter angeben, ob der Arrayinhalt schreibgeschützt oder lesegeschützt ist. Die Richtung ist für **out**-Parameter klar (**ByRef**-Parameter mit dem OutAttribute-Attribut in Visual Basic), aber Arrayparameter, die per Wert übergeben werden (ByVal in Visual Basic), müssen gekennzeichnet werden. Weitere Informationen hierzu finden Sie unter [Übergeben von Arrays an eine Komponente für Windows-Runtime](passing-arrays-to-a-windows-runtime-component.md).

| Fehlernummer | Meldungstext         |
|--------------|----------------------|
| WME1101      | Die Methode '{0}' weist den Parameter '{1}' auf, bei dem es sich um ein Array handelt, und der {2} und {3} enthält. Die Inhalte von Arrayparametern müssen in der Windows-Runtime entweder lesbar oder schreibbar sein. Entfernen Sie eines der Attribute aus '{1}'.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| WME1102      | Die Methode '{0}' weist den Ausgabeparameter '{1}' auf, bei dem es sich zwar um ein Array handelt, der jedoch {2} enthält. Die Inhalte von Ausgabearrays sind in der Windows-Runtime schreibbar. Entfernen Sie das Attribut aus '{1}'.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| WME1103      | Die Methode '{0}' weist den Parameter '{1}' auf. Hierbei handelt es sich um ein Array, das entweder ein System.Runtime.InteropServices.InAttribute oder ein System.Runtime.InteropServices.OutAttribute enthält. Arrayparameter müssen für Windows-Runtime entweder {2} oder {3} enthalten. Entfernen Sie diese Attribute, oder ersetzen Sie sie bei Bedarf durch das entsprechende Windows-Runtime-Attribut.                                                                                                                                                                                                                                                                                                                                                                                          |
| WME1104      | Die Methode '{0}' weist den Parameter '{1}' auf, bei dem es sich um ein Array handelt, und der entweder einen {2} oder einen {3} enthält. Das Markieren von Nicht-Arrayparametern mit {2} oder {3} wird von Windows-Runtime nicht unterstützt.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| WME1105      | Die Methode '{0}' enthält den Parameter '{1}' mit einem System.Runtime.InteropServices.InAttribute oder System.Runtime.InteropServices.OutAttribute. Das Markieren von Parametern mit System.Runtime.InteropServices.InAttribute oder System.Runtime.InteropServices.OutAttribute wird von Windows-Runtime nicht unterstützt. Entfernen Sie eventuell System.Runtime.InteropServices.InAttribute, und ersetzen Sie System.Runtime.InteropServices.OutAttribute stattdessen durch den 'out'-Modifizierer. Die Methode '{0}' enthält den Parameter '{1}' mit einem System.Runtime.InteropServices.InAttribute oder System.Runtime.InteropServices.OutAttribute. Windows-Runtime unterstützt nur das Markieren von ByRef-Parametern mit System.Runtime.InteropServices.OutAttribute. Eine andere Verwendung dieser Attribute ist nicht möglich. |
| WME1106      | Die Methode '{0}' weist den Parameter '{1}' auf, bei dem es sich um ein Array handelt. Die Inhalte von Array-Parametern müssen in der Windows-Runtime entweder lesbar oder schreibbar sein. Wenden Sie entweder {2} oder {3} auf '{1}' an.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |


## Member mit einem Parameter mit dem Namen „Value"


In der UWP werden Rückgabewerte als Ausgabeparameter betrachtet, und die Namen der Parameter müssen eindeutig sein. Standardmäßig gibt Winmdexp.exe dem Rückgabewert den Namen „Value". Wenn die Methode einen Parameter mit dem Namen „Value" hat, erhalten Sie die Fehlermeldung WME1092. Es gibt zwei Korrekturmöglichkeiten:

-   Nennen Sie den Parameter nicht „Value" (in den Eigenschaftenaccessoren sollte der Parametername nicht „returnValue" lauten).
-   Verwenden Sie das ReturnValueNameAttribute-Attribut, um den Namen des Rückgabewerts zu ändern, wie hier gezeigt:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > using System.Runtime.InteropServices;
    > using System.Runtime.InteropServices.WindowsRuntime;
    >
    > [return: ReturnValueName("average")]
    > public int GetAverage(out int lowValue, out int highValue)
    > ```
    > ```vb
    > Imports System.Runtime.InteropServices
    > Imports System.Runtime.InteropServices.WindowsRuntime
    >
    > Public Function GetAverage(<Out> ByRef lowValue As Integer, _
    > <Out> ByRef highValue As Integer) As <ReturnValueName("average")> String
    > ```

> **Hinweis**  Wenn Sie den Namen des Rückgabewerts ändern und der neue Namen mit dem Namen eines anderen Parameters in Konflikt steht, erhalten Sie die Fehlermeldung WME1091.

JavaScript-Code kann auf die Ausgabeparameter einer Methode, einschließlich des Rückgabewerts, über den Namen zugreifen. Ein Beispiel finden Sie unter [ReturnValueNameAttribute](https://msdn.microsoft.com/library/windows/apps/system.runtime.interopservices.windowsruntime.returnvaluenameattribute.aspx).

| Fehlernummer | Meldungstext |
|---------------|------------|
| WME1091 | Die Methode '\{0}' weist den Rückgabewert mit dem Namen '\{1}' auf, bei dem es sich um denselben Namen wie der Parametername handelt. Methodenparameter und Rückgabewerte müssen für Windows-Runtime eindeutige Namen aufweisen. |
| WME1092 | Die Methode '\{0}' weist einen Parameter mit dem Namen '\{1}' auf, der mit dem Standardnamen des Rückgabewerts identisch ist. Verwenden Sie ggf. einen anderen Namen für den Parameter, oder verwenden Sie das System.Runtime.InteropServices.WindowsRuntime.ReturnValueNameAttribute, um den Namen des Rückgabewerts explizit anzugeben.<br/>**Hinweis**  Der Standardname lautet „returnValue" für Eigenschaftenaccessoren und „Value" für alle anderen Methoden. |
 

## Verwandte Themen

* [Erstellen von Komponenten für Windows-Runtime in C# und Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)
* [Winmdexp.exe (Windows Runtime Metadata Export Tool)](https://msdn.microsoft.com/library/hh925576.aspx)



<!--HONumber=Jun16_HO4-->


