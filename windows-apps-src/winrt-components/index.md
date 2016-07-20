---
author: msatranjr
title: "Komponenten für Windows-Runtime"
description: "Komponenten für Windows-Runtime sind unabhängige Objekte, die Sie instanziieren und von jeder Sprache aus verwenden können, einschließlich C#, Visual Basic, JavaScript und C++."
ms.assetid: 55887622-828b-4318-87f2-25592268f7c1
translationtype: Human Translation
ms.sourcegitcommit: 4e9f3de68c44cf545ceee2efd99d9db8cab08676
ms.openlocfilehash: ddbfc30f765bdb1dacbf5c6b72255233fe134073

---

# Komponenten für Windows-Runtime


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Komponenten für Windows-Runtime sind unabhängige Objekte, die Sie instanziieren und von jeder Sprache aus verwenden können, einschließlich C#, Visual Basic, JavaScript und C++.

Sie können Visual Studio und C#, Visual Basic oder C++ verwenden, um Komponenten für Windows-Runtime zu erstellen, die in UWP-Apps (Universelle Windows-Plattform) verwendet werden können.

| Thema | Beschreibung |
|-------|-------------|
| [Erstellen von Komponenten für Windows-Runtime in C++](creating-windows-runtime-components-in-cpp.md) | In diesem Artikel wird beschrieben, wie Sie eine Komponente für Windows-Runtime mit C++ erstellen. Dabei handelt es sich um eine DLL, die aus einer Universellen Windows-App aufgerufen werden kann, die mit JavaScript (oder C#, Visual Basic oder C++) entwickelt wurde. |
| [Exemplarische Vorgehensweise: Erstellen einer einfachen Komponente für Windows-Runtime in C++ und Aufrufen der Komponente über JavaScript oder C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md) | In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie eine einfache Komponenten-DLL-Datei für Windows-Runtime erstellen, die von JavaScript, C# oder Visual Basic aufgerufen werden kann. Bevor Sie mit dieser exemplarischen Vorgehensweise beginnen, stellen Sie sicher, dass Sie Konzepte wie die abstrakte binäre Schnittstelle (ABI), Verweisklassen und Visual C++-Komponentenerweiterungen kennen, die das Verwenden von Verweisklassen vereinfachen. Weitere Informationen finden Sie unter [Erstellen von Komponenten für Windows-Runtime in C++](creating-windows-runtime-components-in-cpp.md) und [Visual C++-Programmiersprachenreferenz (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx). |
| [Erstellen von Komponenten für Windows-Runtime in C# und Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) | Ab .NET Framework4.5 können Sie mit verwaltetem Code eigene Windows-Runtime-Typen erstellen, die in einer Komponente für Windows-Runtime gepackt sind. Diese Komponente können Sie in UWP-Apps (Universelle Windows-Plattform) mit C++, JavaScript, Visual Basic oder C# verwenden. In diesem Artikel werden die Regeln zum Erstellen einer Komponente und einige Aspekte der .NET Framework-Unterstützung für die Windows-Runtime erläutert. Im Allgemeinen ist diese Unterstützung allen .NET Framework-Programmierern klar. Wenn Sie aber eine Komponente erstellen, die mit JavaScript oder C++ verwendet werden soll, müssen Sie auf die Unterschiede bei der Unterstützung der Windows-Runtime durch diese Sprachen achten. |
| [Exemplarische Vorgehensweise: Erstellen einer einfachen Komponente für Windows-Runtime und Aufrufen der Komponente über JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) | In dieser exemplarischen Vorgehensweise wird gezeigt, wie Sie das .NET Framework mit Visual Basic oder C# verwenden können, um eigene Windows-Runtime-Typen zu erstellen, die in einer Komponente für Windows-Runtime verpackt sind, und wie Sie die Komponente von Ihrer Universellen Windows-App aus aufrufen, die für Windows mit JavaScript erstellt wurde. |
| [Auslösen von Ereignissen in Komponenten für Windows-Runtime](raising-events-in-windows-runtime-components.md) | Wenn die Windows-Runtime-Komponente ein Ereignis eines benutzerdefinierten Delegattyps in einem Hintergrundthread (Arbeitsthread) auslöst, und JavaScript in der Lage sein soll, das Ereignis zu empfangen, können Sie es auf eine der folgenden Weisen implementieren und/oder auslösen: | 
| [Vermittelte Windows-Runtime-Komponenten für quergeladene Windows Store-Apps](brokered-windows-runtime-components-for-side-loaded-windows-store-apps.md) | In diesem Thema wird ein für Unternehmen bestimmtes Feature beschrieben, das von Windows 10 Update und höher unterstützt wird und .NET-Apps, die Toucheingaben unterstützen, die Verwendung vorhandenen Codes ermöglichen, der für wichtige unternehmenskritische Vorgänge verantwortlich ist.
 

 

 






<!--HONumber=Jul16_HO1-->


