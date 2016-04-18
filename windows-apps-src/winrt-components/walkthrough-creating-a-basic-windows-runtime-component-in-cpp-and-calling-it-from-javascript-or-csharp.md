---
title: Exemplarische Vorgehensweise: Erstellen einer einfachen Komponente für Windows-Runtime in C++ und Aufruf der Komponente über JavaScript oder C#
description: In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie eine einfache Komponenten-DLL-Datei für Windows-Runtime erstellen, die von JavaScript, C# oder Visual Basic aufgerufen werden kann.
ms.assetid: 764CD9C6-3565-4DFF-88D7-D92185C7E452
---

# Exemplarische Vorgehensweise: Erstellen einer einfachen Komponente für Windows-Runtime in C++ und Aufrufen der Komponente über JavaScript oder C#


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


\[Einige Informationen beziehen sich auf die Vorabversion, die vor der kommerziellen Freigabe möglicherweise wesentlichen Änderungen unterliegt. Microsoft übernimmt keine Garantie, weder ausdrücklicher noch impliziter Art, für die hier bereitgestellten Informationen.\]

In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie eine einfache Komponenten-DLL-Datei für Windows-Runtime erstellen, die von JavaScript, C# oder Visual Basic aufgerufen werden kann. Bevor Sie mit dieser exemplarischen Vorgehensweise beginnen, stellen Sie sicher, dass Sie Konzepte wie die abstrakte binäre Schnittstelle (ABI), Verweisklassen und Visual C++-Komponentenerweiterungen kennen, die das Verwenden von Verweisklassen vereinfachen. Weitere Informationen finden Sie unter [Erstellen von Komponenten für Windows-Runtime in C++](creating-windows-runtime-components-in-cpp.md) und [Visual C++-Programmiersprachenreferenz (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx).

## Erstellen der C++-Komponenten-DLL-Datei


In diesem Beispiel erstellen wir zunächst das Komponentenprojekt. Sie können aber auch das JavaScript-Projekt zuerst erstellen. Die Reihenfolge spielt keine Rolle.

Beachten Sie, dass die Hauptklasse der Komponente Beispiele für Eigenschafts- und Methodendefinitionen sowie eine Ereignisdeklaration enthält. Diese werden nur aus Gründen der Anschaulichkeit bereitgestellt. Sie sind nicht erforderlich, und in diesem Beispiel ersetzen wir den gesamten generierten Code durch unseren eigenen Code.

## **So erstellen Sie das C++-Komponentenprojekt**

1.  Klicken Sie in der Visual Studio-Menüleiste auf **Datei, Neu, Projekt**.
2.  Erweitern Sie im Dialogfeld **Neues Projekt** im linken Bereich **Visual C++**, und wählen Sie dann den Knoten für universelle Windows-Apps aus.
3.  Wählen Sie im mittleren Bereich **Komponente für Windows-Runtime** aus, und geben Sie dem Projekt den Namen „WinRT\_CPP“.
4.  Klicken Sie auf die Schaltfläche **OK**.

## **So fügen Sie der Komponente eine aktivierbare Klasse hinzu**

1.  Eine aktivierbare Klasse kann vom Clientcode mithilfe eines **new**-Ausdrucks (**New** in Visual Basic oder **ref new** in C++) erstellt werden. In der Komponente können Sie sie als **public ref class sealed** deklarieren. Die Class1.h- und CPP-Dateien verfügen bereits über eine Verweisklasse. Sie können den Namen ändern, aber in diesem Beispiel verwenden wir den Standardnamen – Class1. Sie können zusätzliche Verweisklassen oder Standardklassen in der Komponente definieren, falls sie benötigt werden. Weitere Informationen zu Verweisklassen finden Sie unter [Typsystem (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822.aspx).

2.  Fügen Sie diese \#include-Direktiven zu „Class1.h“ hinzu:

    ```cpp
             private void PrimesUnOrderedButton_Click_1(object sender, RoutedEventArgs e)
            {
                var nativeObject = new WinRT_CPP.Class1();

                StringBuilder sb = new StringBuilder();
                sb.Append("Primes found (unordered): ");
                PrimesUnOrderedResult.Text = sb.ToString();

                // primeFoundEvent is a user-defined event in nativeObject
                // It passes the results back to this thread as they are produced
                // and the event handler that we define here immediately displays them.
                nativeObject.primeFoundEvent += (n) =>
                {
                    sb.Append(n.ToString()).Append(" ");
                    PrimesUnOrderedResult.Text = sb.ToString();
                };

                // Call the async method.
                var asyncResult = nativeObject.GetPrimesUnordered(2, 100000);

                // Provide a handler for the Progress event that the asyncResult
                // object fires at regular intervals. This handler updates the progress bar.
                asyncResult.Progress += (asyncInfo, progress) =>
                    {
                        PrimesUnOrderedProgress.Value = progress;
                    };
            }

            private void Clear_Button_Click(object sender, RoutedEventArgs e)
            {
                PrimesOrderedProgress.Value = 0;
                PrimesUnOrderedProgress.Value = 0;
                PrimesUnOrderedResult.Text = "";
                PrimesOrderedResult.Text = "";
                Result1.Text = "";
            }
    ```

## Ausführen der App


Wählen Sie das C#-Projekt oder das JavaScript-Projekt als Startprojekt aus, indem Sie das Kontextmenü für den Projektknoten im Projektmappen-Explorer öffnen und **Als Startprojekt festlegen** auswählen. Drücken Sie dann F5 zum Ausführen mit Debugging oder STRG+F5 zum Ausführen ohne Debugging.

## Untersuchen der Komponente im Objektkatalog (optional)


Im Objektkatalog können Sie alle Windows-Runtime-Typen untersuchen, die in WINMD-Dateien definiert werden. Dazu zählen die Typen im Plattformnamespace und Standardnamespace. Da die Typen im „Platform::Collections“-Namespace aber in der Headerdatei „collections.h“ und nicht in einer WINMD-Datei definiert werden, werden sie nicht im Objektkatalog angezeigt.

## **So untersuchen Sie eine Komponente**

1.  Wählen Sie auf der Menüleiste **Ansicht, Objektkatalog** (STRG+ALT+J) aus.
2.  Erweitern Sie im linken Bereich des Objektkatalogs den Knoten „WinRT\_CPP“ zum Anzeigen von Typen und Methoden, die in der Komponente definiert werden.

## Tipps zum Debuggen


Laden Sie für einen besseren Debugvorgang die Debuggingsymbole von den öffentlichen Microsoft-Symbolservern herunter:

## **So laden Sie Debuggingsymbole herunter**

1.  Wählen Sie auf der Menüleiste **Extras, Optionen** aus.
2.  Erweitern Sie im Dialogfeld **Optionen** die Option **Debuggen** , und wählen Sie **Symbole** aus.
3.  Wählen Sie **Microsoft-Symbolserver** und dann die Schaltfläche **OK** aus.

Beim ersten Mal kann das Herunterladen der Symbole einige Zeit dauern. Geben Sie für eine bessere Leistung beim nächsten Drücken von F5 ein lokales Verzeichnis an, in dem Sie die Symbole zwischenspeichern.

Beim Debuggen einer JavaScript-Lösung mit einer Komponenten-DLL können Sie den Debugger so einstellen, dass er das schrittweise Ausführen von Skripts oder das schrittweise Ausführen von systemeigenem Code in der Komponente, aber nicht beides gleichzeitig ermöglicht. Um die Einstellung zu ändern, öffnen Sie im Projektmappen-Explorer das Kontextmenü für den JavaScript-Projektknoten, und wählen Sie **Eigenschaften, Debuggen, Debuggertyp** aus.

Achten Sie darauf, entsprechende Funktionen im Paket-Designer auszuwählen. Sie können den Paket-Designer durch Öffnen der Datei „Package.appxmanifest“ öffnen. Wenn Sie z. B. versuchen, programmgesteuert auf Dateien im Ordner „Bilder“ zuzugreifen, stellen Sie sicher, dass das Kontrollkästchen **Bildbibliothek** im Bereich **Funktionen** des Paket-Designers aktiviert ist.

Wenn der JavaScript-Code die öffentlichen Eigenschaften oder Methoden in der Komponente nicht erkennt, stellen Sie sicher, dass Sie in JavaScript Camel Casing verwenden. Auf die `ComputeResult`-C++-Methode muss beispielsweise in JavaScript als `computeResult` verwiesen werden.

Wenn Sie ein C++-Komponentenprojekt für Windows-Runtime aus einer Projektmappe entfernen, müssen Sie den Projektverweis auch aus dem JavaScript-Projekt manuell entfernen. Andernfalls werden nachfolgende Debug- oder Buildvorgänge verhindert. Bei Bedarf können Sie dann einen Assemblyverweis zur DLL-Datei hinzufügen.

## Verwandte Themen

* [Erstellen von Komponenten für Windows-Runtime in C++](creating-windows-runtime-components-in-cpp.md)



<!--HONumber=Mar16_HO1-->


