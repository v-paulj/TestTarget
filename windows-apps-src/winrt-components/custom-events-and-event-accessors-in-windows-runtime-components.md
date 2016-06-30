---
author: msatranjr
title: "Benutzerdefinierte Ereignisse und Ereignisaccessoren in Komponenten für Windows-Runtime"
description: "Die .NET Framework-Unterstützung für Komponenten für Windows-Runtime erleichtert die Deklaration von Ereigniskomponenten, indem die Unterschiede zwischen dem Ereignismuster der universellen Windows-Plattform (UWP) und dem Ereignismuster von .NET Framework verborgen werden."
ms.assetid: 6A66D80A-5481-47F8-9499-42AC8FDA0EB4
ms.sourcegitcommit: 4c32b134c704fa0e4534bc4ba8d045e671c89442
ms.openlocfilehash: 1308989c8d1c6959560458dd4d87119b4bfa74b0

---

# Benutzerdefinierte Ereignisse und Ereignisaccessoren in Komponenten für Windows-Runtime


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \].

Die .NET Framework-Unterstützung für Komponenten für Windows-Runtime erleichtert die Deklaration von Ereigniskomponenten, indem die Unterschiede zwischen dem Ereignismuster der universellen Windows-Plattform (UWP) und dem Ereignismuster von .NET Framework verborgen werden. Wenn Sie jedoch benutzerdefinierte Ereignisaccessoren in einer Komponente für Windows-Runtime deklarieren, müssen Sie dem Muster folgen das in der UWP verwendet wird.

## Registrieren von Ereignissen


Wenn Sie die Registrierung für die Behandlung eines Ereignisses in der UWP durchführen, gibt der Add-Accessor ein Token zurück. Um die Registrierung aufzuheben, übergeben Sie dieses Token an den Remove-Accessor. Dies bedeutet, dass sich die Signaturen der Add- und Remove-Accessoren für UWP-Ereignisse von den üblichen Accessoren unterscheiden.

Glücklicherweise vereinfachen die Visual Basic- und C#-Compiler diesen Prozess: Wenn Sie ein Ereignis mit benutzerdefinierten Accessoren in einer Komponente für Windows-Runtime deklarieren, verwenden die Compiler automatisch das UWP-Muster. Beispielsweise erhalten Sie einen Compilerfehler, wenn Ihr Add-Accessor kein Token zurückgibt. Das .NET Framework stellt zwei Typen zur Unterstützung der Implementierung bereit:

-   Die [EventRegistrationToken](https://msdn.microsoft.com/library/windows/apps/windows.foundation.eventregistrationtoken.aspx)-Struktur stellt das Token dar.
-   Die [EventRegistrationTokenTable&lt;T&gt;](https://msdn.microsoft.com/library/hh138412.aspx)-Klasse erstellt Token und verwaltet eine Zuordnung zwischen Token und Ereignishandlern. Das generische Typargument ist der Ereignisargumenttyp. Sie erstellen eine Instanz dieser Klasse für jedes Ereignis, wenn ein Ereignishandler zum ersten Mal für dieses Ereignis registriert wird.

Der folgende Code für das Ereignis „NumberChanged” zeigt das grundlegende Muster für UWP-Ereignisse. In diesem Beispiel übernimmt der Konstruktor für das Ereignisargumentobjekt „NumberChangedEventArgs” einen einzelnen ganzzahligen Parameter, der den geänderten numerischen Wert darstellt.

> **Hinweis**  Dies ist das gleiche Muster, das die Compiler für gewöhnliche Ereignisse verwenden, die Sie in einer Komponente für Windows-Runtime deklarieren.

 
> [!div class="tabbedCodeSnippets"]
> ```csharp
> private EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>     m_NumberChangedTokenTable = null;
>
> public event EventHandler<NumberChangedEventArgs> NumberChanged
> {
>     add
>     {
>         return EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>             .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>             .AddEventHandler(value);
>     }
>     remove
>     {
>         EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>             .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>             .RemoveEventHandler(value);
>     }
> }
>
> internal void OnNumberChanged(int newValue)
> {
>     EventHandler<NumberChangedEventArgs> temp =
>         EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>         .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>         .InvocationList;
>     if (temp != null)
>     {
>         temp(this, new NumberChangedEventArgs(newValue));
>     }
> }
> ```
> ```vb
> Private m_NumberChangedTokenTable As  _
>     EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs))
>
> Public Custom Event NumberChanged As EventHandler(Of NumberChangedEventArgs)
>
>     AddHandler(ByVal handler As EventHandler(Of NumberChangedEventArgs))
>         Return EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             AddEventHandler(handler)
>     End AddHandler
>
>     RemoveHandler(ByVal token As EventRegistrationToken)
>         EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             RemoveEventHandler(token)
>     End RemoveHandler
>
>     RaiseEvent(ByVal sender As Class1, ByVal args As NumberChangedEventArgs)
>         Dim temp As EventHandler(Of NumberChangedEventArgs) = _
>             EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             InvocationList
>         If temp IsNot Nothing Then
>             temp(sender, args)
>         End If
>     End RaiseEvent
> End Event
> ```

Die statische („Shared” in Visual Basic) Methode „GetOrCreateEventRegistrationTokenTable” erstellt die Instanz des Ereignisses des EventRegistrationTokenTable&lt;T&gt;-Objekts verzögert. Übergeben Sie das Feld auf Klassenebene, das die Tokentabelleninstanz enthalten soll, an diese Methode. Wenn das Feld leer ist, erstellt die Methode die Tabelle, speichert einen Verweis auf die Tabelle in dem Feld und gibt einen Verweis auf die Tabelle zurück. Wenn das Feld bereits einen Verweis auf die Tokentabelle enthält, gibt die Methode einfach diesen Verweis zurück.

> **Wichtig**  Um Threadsicherheit zu gewährleisten, muss das Feld, das die Instanz des Ereignisses „EventRegistrationTokenTable&lt;T&gt;” enthält, ein Feld auf Klassenebene sein. Wenn dies ein Feld auf Klassenebene ist, stellt die Methode „GetOrCreateEventRegistrationTokenTable” sicher, dass alle Threads dieselbe Instanz der Tabelle abrufen, wenn mehrere Threads versuchen, die Tokentabelle zu erstellen. Für ein bestimmtes Ereignis müssen alle Aufrufe der Methode „GetOrCreateEventRegistrationTokenTable” dasselbe Feld auf Klassenebene verwenden.

Durch Aufrufen der Methode „GetOrCreateEventRegistrationTokenTable” im Remove-Accessor und in der Methode [RaiseEvent](https://msdn.microsoft.com/library/fwd3bwed.aspx) („OnRaiseEvent”-Methode in C#) wird sichergestellt, dass keine Ausnahmen ausgelöst werden, wenn diese Methoden aufgerufen werden, bevor Ereignishandlerdelegaten hinzugefügt wurden.

Zu den anderen Membern der EventRegistrationTokenTable&lt;T&gt;-Klasse, die im UWP-Ereignismuster verwendet werden, zählen:

-   Die [AddEventHandler](https://msdn.microsoft.com/library/hh138458.aspx)-Methode generiert ein Token für den Ereignishandlerdelegaten, speichert den Delegaten in der Tabelle, fügt ihn der Aufrufliste hinzu und gibt das Token zurück.
-   Die [RemoveEventHandler(EventRegistrationToken)](https://msdn.microsoft.com/library/hh138425.aspx)-Methodenüberladung entfernt den Delegaten aus der Tabelle und der Aufrufliste.

    >**Hinweis**  Die Methoden „AddEventHandler” und „RemoveEventHandler(EventRegistrationToken)” sperren die Tabelle, um die Threadsicherheit zu gewährleisten.

-   Die [InvocationList](https://msdn.microsoft.com/library/hh138465.aspx)-Eigenschaft gibt einen Delegaten zurück, der alle Ereignishandler enthält, die derzeit zum Behandeln des Ereignisses registriert sind. Verwenden Sie diesen Delegaten zum Auslösen des Ereignisses, oder verwenden Sie die Methoden der Delegate-Klasse, um die Handler einzeln aufzurufen.

    >**Hinweis**  Wir empfehlen, dass Sie dem Muster weiter des oben in diesem Artikel gezeigten Beispiels folgen und den Delegaten in eine temporäre Variable kopieren, bevor Sie ihn aufrufen. Dadurch wird eine Racebedingung verhindert, in der ein Thread den letzten Ereignishandler entfernt, indem der Delegat auf null festlegt wird, kurz bevor ein anderer Thread versucht, den Delegaten aufzurufen. Delegaten sind unveränderlich, sodass die Kopie weiterhin gültig ist.

Nehmen Sie eigenen Code nach Bedarf in die Accessoren auf. Wenn Threadsicherheit wichtig ist, müssen Sie eigene Sperren für Ihren Code vorsehen.

C#-Benutzer: Wenn Sie benutzerdefinierte Ereignisaccessoren im UWP-Ereignismuster schreiben, stellt der Compiler nicht die üblichen syntaktischen Verknüpfungen bereit. Es werden Fehler generiert, wenn Sie den Namen des Ereignisses im Code verwenden.

Visual Basic-Benutzer: Im .NET Framework ist ein Ereignis einfach ein Multicastdelegat, der alle registrierten Ereignishandler darstellt. Das Auslösen des Ereignisses bedeutet nur, dass der Delegat aufgerufen wird. Visual Basic-Syntax blendet im Allgemeinen die Interaktionen mit dem Delegaten aus, und der Compiler kopiert den Delegaten, bevor er aufgerufen wird, wie im Hinweis zu Threadsicherheit beschrieben. Wenn Sie ein benutzerdefiniertes Ereignis in einer Komponente für Windows-Runtime erstellen, müssen Sie sich direkt mit dem Delegaten befassen. Dies bedeutet auch, dass Sie beispielsweise mit der [MulticastDelegate.GetInvocationList](https://msdn.microsoft.com/library/system.multicastdelegate.getinvocationlist.aspx)-Methode ein Array mit einem separaten Delegaten für jeden Ereignishandler abrufen können, wenn Sie die Handler einzeln aufrufen möchten.

## Verwandte Themen

* [Ereignisse (Visual Basic)](https://msdn.microsoft.com/library/ms172877.aspx)
* [Ereignisse (C#-Programmierhandbuch)](https://msdn.microsoft.com/library/awbftdfh.aspx)
* [Übersicht über .NET für Windows Store-Apps](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx)
* [.NET für UWP-Apps](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx)
* [Exemplarische Vorgehensweise: Erstellen einer einfachen Komponente für Windows-Runtime und Aufrufen der Komponente über JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)



<!--HONumber=Jun16_HO4-->


