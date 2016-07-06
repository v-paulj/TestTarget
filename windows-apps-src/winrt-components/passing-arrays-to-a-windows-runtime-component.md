---
author: msatranjr
title: "Übergeben von Arrays an eine Komponente für Windows-Runtime"
description: "Parameter in der UWP (Universal Windows-Plattform) sind entweder für die Eingabe oder für die Ausgabe, nie für beide, vorgesehen. Das bedeutet, dass der Inhalt eines Arrays, der an eine Methode übergeben wird, wie auch das Array selbst für die Eingabe oder für die Ausgabe vorgesehen sind."
ms.assetid: 8DE695AC-CEF2-438C-8F94-FB783EE18EB9
ms.sourcegitcommit: 4c32b134c704fa0e4534bc4ba8d045e671c89442
ms.openlocfilehash: 21e4b504b4adc6e2cb9b16d377781aaaab6a4aac

---

# Übergeben von Arrays an eine Komponente für Windows-Runtime


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \].


Parameter in der UWP (Universal Windows-Plattform) sind entweder für die Eingabe oder für die Ausgabe, nie für beides, vorgesehen. Das bedeutet, dass der Inhalt eines Arrays, der an eine Methode übergeben wird, wie auch das Array selbst, für die Eingabe oder für die Ausgabe vorgesehen sind. Wenn der Inhalt des Arrays für die Eingabe vorgesehen ist, liest die Methode aus dem Array, aber schreibt nicht in das Array. Wenn der Inhalt des Arrays für die Ausgabe vorgesehen ist, schreibt die Methode in das Array, aber liest nicht in daraus. Dies stellt ein Problem für Arrayparameter dar, da Arrays im .NET Framework Verweistypen sind, und der Inhalt eines Arrays änderbar ist, auch wenn der Array-Verweis per Wert übergeben wird (**ByVal** in Visual Basic). Für das [Windows-Runtime-Metadatenexport-Tool (Winmdexp.exe)](https://msdn.microsoft.com/library/hh925576.aspx) müssen Sie die beabsichtigte Verwendung des Arrays angeben, falls dies nicht eindeutig aus dem Kontext ersichtlich ist. Fügen Sie dem Parameter zu diesem Zweck das Attribut „ReadOnlyArrayAttribute” oder „WriteOnlyArrayAttribute” hinzu. Die Array-Nutzung wird wie folgt bestimmt:

-   Für den Rückgabewert oder einen Ausgabeparameter (einen **ByRef**-Parameter mit dem [OutAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.outattribute.aspx)-Attribut in Visual Basic) ist das Array immer nur für die Ausgabe vorgesehen. Geben Sie das Attribut „ReadOnlyArrayAttribute” nicht an. Das „WriteOnlyArrayAttribute”-Attribut ist für Ausgabeparameter zulässig, aber redundant.

    > **Achtung**  Der Visual Basic-Compiler erzwingt keine Regeln nur für die Ausgabe. Sie sollten nie aus Ausgabeparametern lesen, sie könnten **Nothing** enthalten. Weisen Sie immer ein neues Array zu.
 
-   Parameter mit dem Modifizierer **ref** (**ByRef** in Visual Basic) sind nicht zulässig. Winmdexp.exe erzeugt einen Fehler.
-   Für einen Parameter, der als Wert übergeben wird, müssen Sie angeben, ob der Arrayinhalt für die Eingabe oder die Ausgabe vorgesehen ist, indem Sie entweder das Attribut [ReadOnlyArrayAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.readonlyarrayattribute.aspx) oder das Attribut [WriteOnlyArrayAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute.aspx) angeben. Die Angabe von beiden Attributen ist ein Fehler.

Wenn eine Methode ein Array für die Eingabe akzeptieren soll, ändern Sie den Arrayinhalt, und geben Sie das Array an den Aufrufer zurück; verwenden Sie einen schreibgeschützten Parameter für die Eingabe und einen lesegeschützten Parameter (oder den Rückgabewert) für die Ausgabe. Der folgende Code zeigt eine Möglichkeit, dieses Muster zu implementieren:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public int[] ChangeArray([ReadOnlyArray()] int[] input)
> {
>     int[] output = input.Clone();
>     // Manipulate the copy.
>     //   ...
>     return output;
> }
> ```
> ```vb
> Public Function ChangeArray(<ReadOnlyArray> input() As Integer) As Integer()
>     Dim output() As Integer = input.Clone()
>     ' Manipulate the copy.
>     '   ...
>     Return output
> End Function
> ```

[!div class="tabbedCodeSnippets"] Wir empfehlen, dass Sie sofort eine Kopie des Eingabearrays anlegen und die Kopie bearbeiten.

## Dadurch wird sichergestellt, dass die Methode sich identisch verhält, unabhängig davon, ob die Komponente von .NET Framework-Code aufgerufen wird oder nicht.


Verwenden von Komponenten aus verwaltetem und nicht verwaltetem Code Parameter mit dem Attribut „ReadOnlyArrayAttribute” oder „WriteOnlyArrayAttribute” verhalten sich anders, je nachdem, ob der Aufrufer in systemeigenem oder verwaltetem Code geschrieben wurde.

-   Wenn der Aufrufer systemeigener Code (JavaScript oder Visual C++-Komponentenerweiterungen) ist, wird der Arrayinhalt wie folgt behandelt: ReadOnlyArrayAttribute: Das Array wird kopiert, wenn der Aufruf die Grenze der binären Anwendungsschnittstelle (ABI) überschreitet. Elemente werden bei Bedarf konvertiert.
-   Daher sind versehentliche Änderungen, die die Methode an einem nur für die Eingabe bestimmten Array vornimmt, nicht für den Aufrufer sichtbar. WriteOnlyArrayAttribute: Die aufgerufene Methode kann keine Annahmen über den Inhalt des ursprünglichen Arrays treffen. Zum Beispiel könnte das Array, das die Methode erhält, möglicherweise nicht initialisiert werden oder Standardwerte enthalten.

Von der Methode wird erwartet, die Werte aller Elemente im Array festzulegen. Ist der Aufrufer in verwaltetem Code geschrieben, ist das ursprüngliche Array für die aufgerufene Methode genauso wie in einem Methodenaufruf in .NET Framework verfügbar. Arrayinhalt ist in .NET Framework-Code änderbar, sodass alle Änderungen, die die Methode an dem Array vornimmt, für den Aufrufer sichtbar sind. Dies ist wichtig, da es Komponententests für eine Komponente für Windows-Runtime betrifft.

## Wenn die Tests in verwaltetem Code geschrieben sind, erscheint der Inhalt eines Arrays während der Testphase als änderbar.

* [Verwandte Themen](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.readonlyarrayattribute.aspx)
* [ReadOnlyArrayAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute.aspx)
* [WriteOnlyArrayAttribute](creating-windows-runtime-components-in-csharp-and-visual-basic.md)



<!--HONumber=Jun16_HO5-->


