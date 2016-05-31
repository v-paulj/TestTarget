---
author: martinekuan
title: Auslösen von Ereignissen in Komponenten für Windows-Runtime
ms.assetid: 3F7744E8-8A3C-4203-A1CE-B18584E89000
description: 
---

# Auslösen von Ereignissen in Komponenten für Windows-Runtime


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


\[Einige Informationen beziehen sich auf die Vorabversion, die vor der kommerziellen Freigabe möglicherweise wesentlichen Änderungen unterliegt. Microsoft übernimmt keine Garantie, weder ausdrücklicher noch impliziter Art, für die hier bereitgestellten Informationen.\]

Wenn die Komponente für Windows-Runtime ein Ereignis eines benutzerdefinierten Delegattyps in einem Hintergrundthread (Arbeitsthread) auslöst und JavaScript in der Lage sein soll, das Ereignis zu empfangen, können Sie es auf eine der folgenden Weisen implementieren und/oder auslösen:

-   (Option 1) Lösen Sie das Ereignis mit dem [Windows.UI.Core.CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx) aus, um das Ereignis an den JavaScript-Threadkontext zu marshallen. Dies ist in der Regel zwar die beste Option, erzielt in manchen Szenarien aber möglicherweise nicht die schnellste Leistung.
-   (Option 2) Verwenden Sie [Windows.Foundation.EventHandler](https://msdn.microsoft.com/library/windows/apps/br206577.aspx)&lt;Object&gt;, allerdings mit Verlust von Ereignistypinformationen. Falls Option 1 nicht möglich oder die Leistung nicht ausreichend ist, bietet diese Option eine gute Alternative, wenn der Verlust von Typinformationen akzeptabel ist.
-   (Option 3) Erstellen Sie einen eigenen Proxy und Stub für die Komponente. Diese Option ist am schwierigsten zu implementieren, behält aber die Typinformationen bei und erzielt in anspruchsvollen Szenarien unter Umständen eine bessere Leistung als Option 1.

Wenn Sie nur ein Ereignis in einem Hintergrundthread ohne eine dieser Optionen auslösen, wird das Ereignis von einem JavaScript-Client nicht empfangen.

## Hintergrund


Bei allen Komponenten und Apps für Windows-Runtime handelt es sich im Grunde um COM-Objekte, unabhängig davon, welche Sprache Sie bei ihrer Erstellung verwenden. In der Windows-API sind die meisten Komponenten agile COM-Objekte, die mit Objekten im Hintergrundthread und im UI-Thread gleichermaßen gut kommunizieren können. Wenn ein COM-Objekt nicht agil angelegt werden kann, sind Hilfsobjekte (auch als Proxys bzw. Stubs bezeichnet) erforderlich, um mit anderen COM-Objekten über die Threadgrenze des UI-Threadhintergrunds hinweg zu kommunizieren. (In der COM-Terminologie wird dies als Kommunikation zwischen Threadapartments bezeichnet.)

Die meisten Objekte in der Windows-API sind entweder agil oder verfügen über integrierte Proxys und Stubs. Allerdings können Proxys und Stubs nicht für generische Typen wie Windows.Foundation.[TypedEventHandler&lt;TSender, TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br225997.aspx) erstellt werden, da sie erst vollständige Typen sind, wenn das Typargument bereitgestellt ist. Fehlende Proxys oder Stubs stellen nur bei JavaScript-Clients ein Problem dar. Wenn die Komponente aber sowohl in JavaScript als auch in C++ oder einer .NET-Sprache verwendet werden soll, müssen Sie eine der drei folgenden Optionen verwenden.

## Option 1) Auslösen des Ereignisses mit dem CoreDispatcher-Ereignis


Sie können Ereignisse eines benutzerdefinierten Delegattyps mit [Windows.UI.Core.CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx) senden, und JavaScript kann diese Ereignisse empfangen. Wenn Sie nicht sicher sind, welche Option Sie verwenden sollen, versuchen Sie es zunächst mit dieser ersten Option. Wenn die Wartezeit zwischen dem Auslösen von Ereignissen und der Ereignisbehandlung ein Problem darstellt, versuchen Sie es mit einer der anderen Optionen.

Im folgenden Beispiel wird gezeigt, wie der CoreDispatcher verwendet wird, um ein stark typisiertes Ereignis auszulösen. Beachten Sie, dass es sich bei dem Typargument um „Toast“, und nicht um „Object“ handelt.

```csharp
public event EventHandler<Toast> ToastCompletedEvent;
private void OnToastCompleted(Toast args)
{
    var completedEvent = ToastCompletedEvent;
    if (completedEvent != null)
    {
        completedEvent(this, args);
    }
}

public void MakeToastWithDispatcher(string message)
{
    Toast toast = new Toast(message);
    // Assume you have a CoreDispatcher at class scope.
    // Initialize it here, then use it from the background thread.
    var window = Windows.UI.Core.CoreWindow.GetForCurrentThread();
    m_dispatcher = window.Dispatcher;

    Task.Run( () =>
    {
        if (ToastCompletedEvent != null)
        {
            m_dispatcher.RunAsync(CoreDispatcherPriority.Normal,
            new DispatchedHandler(() =>
            {
                this.OnToastCompleted(toast);
            })); // end m_dispatcher.RunAsync
         }
     }); // end Task.Run
}
```

## (Option 2) Verwenden von EventHandler&lt;Object&gt;, allerdings mit Verlust von Typinformationen


Eine weitere Möglichkeit, ein Ereignis aus einem Hintergrundthread zu senden, ist die Verwendung von [Windows.Foundation.EventHandler](https://msdn.microsoft.com/library/windows/apps/br206577.aspx)&lt;Object&gt; als Typ des Ereignisses. Windows instanziiert den generischen Typ konkret und stellt dafür einen Proxy und einen Stub bereit. Der Nachteil besteht darin, dass die Typinformationen der Ereignisargumente und des Senders verloren gehen. C++- und .NET-Clients müssen aus der Dokumentation entnehmen können, welcher Typ in den ursprünglichen Typ umgewandelt werden muss, wenn das Ereignis empfangen wird. JavaScript-Clients benötigt keine Informationen über die ursprünglichen Typen. Sie finden die Argumenteigenschaften anhand ihrer Namen in den Metadaten.

In diesem Beispiel wird gezeigt, wie Windows.Foundation.EventHandler&lt;Object&gt; in C# verwendet wird:

```csharp
public sealed Class1
{
// Declare the event
public event EventHandler<Object> ToastCompletedEvent;

    // Raise the event
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message);
        // Fire the event from a background thread to allow this thread to continue 
        Task.Run(() =>
        {
            if (ToastCompletedEvent != null)
            {
                OnToastCompleted(toast);
            }
        });
    }

    private void OnToastCompleted(Toast args)
    {
        var completedEvent = ToastCompletedEvent;
        if (completedEvent != null)
        {
            completedEvent(this, args);
        }
    }
}
```

Sie verwenden dieses Ereignis auf der JavaScript-Seite wie folgt:

```javascript
toastCompletedEventHandler: function (event) {
   var toastType = event.toast.toastType;
   document.getElementById("toasterOutput").innerHTML = "<p>Made " + toastType + " toast</p>";
}
```

## (Option 3) Erstellen eines eigenen Proxys und Stubs


Um bei benutzerdefinierten Ereignistypen mit vollständig beibehaltenen Typinformationen potenzielle Leistungszuwächse zu erreichen, müssen Sie eigene Proxy- und Stub-Objekte erstellen und diese in das App-Paket einbetten. In der Regel wird diese Option nur selten verwendet und zwar, wenn keine der beiden anderen Optionen geeignet ist. Außerdem ist nicht gewährleistet, dass mit dieser Option eine bessere Leistung erzielt wird als mit den anderen beiden Optionen. Die tatsächliche Leistung hängt von vielen Faktoren ab. Verwenden Sie den Visual Studio-Profiler oder andere Profilerstellungstools, um die tatsächliche Leistung der Anwendung zu messen und um festzustellen, ob das Ereignis tatsächlich einen Engpass darstellt.

Im weiteren Verlauf dieses Artikels wird gezeigt, wie eine grundlegende Komponente für Windows-Runtime in C# und anschließend eine DLL für Proxy und Stub in C++ erstellt werden, sodass JavaScript ein Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt;-Ereignis nutzen kann, das von der Komponente in einem asynchronen Vorgang ausgelöst wird. (Sie können die Komponente auch in C++ oder Visual Basic erstellen. Die Schritte zum Erstellen der Proxys und Stubs sind identisch.) Diese exemplarische Vorgehensweise basiert auf dem Artikel „Erstellen einer prozessinternen Komponente für Windows-Runtime (C++/CX) – Beispiel”, dessen Zielsetzungen näher erläutert werden.

Diese exemplarische Vorgehensweise besteht aus folgenden Teilen:

-   Hier erstellen Sie zwei Windows-Runtime-Basisklassen. Die eine Klasse macht ein Ereignis vom Typ [Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br225997.aspx) verfügbar, und die andere Klasse ist der Typ, der als Argument für TValue an JavaScript zurückgegeben wird. Diese Klassen können erst mit JavaScript kommunizieren, wenn Sie die nachfolgenden Schritten ausgeführt haben.
-   Diese App aktiviert das Hauptklassenobjekt, ruft eine Methode auf und behandelt ein Ereignis, das von der Komponente für Windows-Runtime ausgelöst wird.
-   Diese werden von den Tools benötigt, die die Proxy- und Stubklassen generieren.
-   Dann wird mithilfe der IDL-Datei der C-Quellcode für den Proxy und den Stub generiert.
-   Registrieren Sie die Proxy-Stub-Objekte, damit die COM-Laufzeitumgebung sie finden kann, und verweisen Sie im App-Projekt auf die Proxy-Stub-DLL-Datei.

## So erstellen Sie die Komponente für Windows-Runtime

1.  Wählen Sie in Visual Studio in der Menüleiste **Datei &gt; Neues Projekt** aus. Erweitern Sie im Dialogfeld **Neues Projekt** die Option **JavaScript &gt; Universal Windows**, und wählen Sie dann **Leere App** aus. Geben Sie als Projektnamen „ToasterApplication“ ein, und klicken Sie dann auf die Schaltfläche **OK**.
2.  Fügen Sie der Projektmappe eine C#-Komponente für Windows-Runtime hinzu: Öffnen Sie im Projektmappen-Explorer das Kontextmenü für die Projektmappe, und wählen Sie dann **Hinzufügen &gt; Neues Projekt** aus. Erweitern Sie **Visual C# &gt; Windows Store**, und wählen Sie dann **Komponente für Windows-Runtime** aus. Geben Sie als Projektnamen „ToasterComponent“ ein, und klicken Sie dann auf die Schaltfläche **OK**. ToasterComponent ist der Stammnamespace für die Komponenten, die Sie in späteren Schritten erstellen.

    Öffnen Sie im Projektmappen-Explorer das Kontextmenü für die Projektmappe, und wählen Sie dann **Eigenschaften** aus. Wählen Sie im Dialogfeld **Eigenschaftenseiten** im linken Bereich **Konfigurationseigenschaften** aus, und legen Sie dann oben im Dialogfeld die **Konfiguration** auf **Debuggen** und die **Plattform** auf „x86”, „x64” oder „ARM” fest. Klicken Sie auf die Schaltfläche **OK**.

    > **Wichtig**  „Plattform = Any CPU“ funktioniert nicht, da dies für die Win32-DLL in systemeigenem Code, die Sie später der Projektmappe hinzufügen, ungültig ist.

3.  Benennen Sie im Projektmappen-Explorer die Datei „class1.cs” in „ToasterComponent.cs” um, sodass sie dem Namen des Projekts entspricht. Visual Studio benennt die Klasse in der Datei entsprechend dem neuen Dateinamen automatisch um.
4.  Fügen Sie der CS-Datei eine using-Direktive für den Namespace Windows.Foundation hinzu, um TypedEventHandler in den Gültigkeitsbereich einzubinden.
5.  Wenn Sie Proxys und Stubs benötigen, muss die Komponente mit Schnittstellen ihre öffentlichen Member verfügbar machen. Definieren Sie in „ToasterComponent.cs” eine Schnittstelle für den Toaster und eine andere für den „Toast” (Popup), den der Toaster erzeugt.

    > **Hinweis**  In C# können Sie diesen Schritt überspringen. Erstellen Sie stattdessen zuerst eine Klasse, öffnen Sie das Kontextmenü, und wählen Sie **Umgestalten &gt; Schnittstelle extrahieren** aus. Ordnen Sie den Schnittstellen im generierten Code manuell öffentlichen Zugriff zu.

    ```csharp
    public interface IToaster
        {
            void MakeToast(String message);
            event TypedEventHandler<Toaster, Toast> ToastCompletedEvent;

        }
        public interface IToast
        {
            String ToastType { get; }
        }
    ```

    Die IToast-Schnittstelle hat eine Zeichenfolge, die abgerufen werden kann, um den Typ des Popups zu beschreiben. Die IToaster-Schnittstelle verfügt eine Methode, um das Popup zu erstellen, und über ein Ereignis, das angibt, dass das Popup erstellt wird. Da dieses Ereignis einen bestimmten Teil (d. h. den Typ) des Popups zurückgibt, wird es als typisiertes Ereignis bezeichnet.

6.  Als Nächstes müssen Klassen angelegt werden, die diese Schnittstellen implementieren. Diese Klassen müssen öffentlich und versiegelt sein, damit die JavaScript-App, die Sie später programmieren, darauf zugreifen kann.

    ```csharp
    public sealed class Toast : IToast
        {
            private string _toastType;

            public string ToastType
            {
                get
                {
                    return _toastType;
                }
            }
            internal Toast(String toastType)
            {
                _toastType = toastType;
            }

        }
        public sealed class Toaster : IToaster
        {
            public event TypedEventHandler<Toaster, Toast> ToastCompletedEvent;

            private void OnToastCompleted(Toast args)
            {
                var completedEvent = ToastCompletedEvent;
                if (completedEvent != null)
                {
                    completedEvent(this, args);
                }
            }

            public void MakeToast(string message)
            {
                Toast toast = new Toast(message);
                // Fire the event from a thread-pool thread to enable this thread to continue 
                Windows.System.Threading.ThreadPool.RunAsync(
                (IAsyncAction action) =>
                {
                    if (ToastCompletedEvent != null)
                    {
                        OnToastCompleted(toast);
                    }
                });
           }
        }
    ``` 

    Im vorhergehenden Code wird das Popup erstellt und eine Arbeitsaufgabe im Threadpool ausgeführt, um die Benachrichtigung auszulösen. Auch wenn die IDE vorschlagen sollte, das „await“-Schlüsselwort dem asynchronen Aufruf zuzuweisen, ist dies in diesem Fall nicht nötig, da die Methode keine Aktionen ausführt, die von den Ergebnissen des Vorgangs abhängig sind.

    > **Hinweis**  Der asynchrone Aufruf im vorhergehenden Code verwendet ThreadPool.RunAsync nur, um auf einfache Weise zu veranschaulichen, dass das Ereignis in einem Hintergrundthread ausgelöst wird. Sie könnten diese spezielle Methode auch schreiben wie im folgenden Beispiel gezeigt. Dies funktioniert, da der .NET-Taskplaner automatisch „async/await“-Aufrufe zurück an den UI-Thread marshallt.
  
    ````csharp
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message)
        await Task.Delay(new Random().Next(1000));
        OnToastCompleted(toast);
    }
    ```

    If you build the project now, it should build cleanly.

## To program the JavaScript app

1.  Now we can add a button to the JavaScript app to cause it to use the class we just defined to make toast. Before we do this, we must add a reference to the ToasterComponent project we just created. In Solution Explorer, open the shortcut menu for the ToasterApplication project, choose **Add &gt; References**, and then choose the **Add New Reference** button. In the Add Reference dialog box, in the left pane under Solution, select the component project, and then in the middle pane, select ToasterComponent. Choose the **OK** button.
2.  In Solution Explorer, open the shortcut menu for the ToasterApplication project and then choose **Set as Startup Project**.
3.  At the end of the default.js file, add a namespace to contain the functions to call the component and be called back by it. The namespace will have two functions, one to make toast and one to handle the toast-complete event. The implementation of makeToast creates a Toaster object, registers the event handler, and makes the toast. So far, the event handler doesn’t do much, as shown here:

    ```javascript
    WinJS.Namespace.define("ToasterApplication"), {
       makeToast: function () {

          var toaster = new ToasterComponent.Toaster();
          //toaster.addEventListener("ontoastcompletedevent", ToasterApplication.toastCompletedEventHandler);
          toaster.ontoastcompletedevent = ToasterApplication.toastCompletedEventHandler;
          toaster.makeToast("Peanut Butter");
       },

       toastCompletedEventHandler: function(event) {
           // The sender of the event (the delegate's first type parameter)
           // is mapped to event.target. The second argument of the delegate
           // is contained in event, which means in this case event is a 
           // Toast class, with a toastType string.
           var toastType = event.toastType;

           document.getElementById('toastOutput').innerHTML = "<p>Made " + toastType + " toast</p>";
        },
    });
    ```

4.  The makeToast function must be hooked up to a button. Update default.html to include a button and some space to output the result of making toast:

    ```html
    <body>
        <h1>Click the button to make toast</h1>
        <button onclick="ToasterApplication.makeToast()">Make Toast!</button>
        <div id="toasterOutput">
            <p>No Toast Yet...</p>
        </div>
    </body>
    ```

    If we weren’t using a TypedEventHandler, we would now be able to run the app on the local machine and click the button to make toast. But in our app, nothing happens. To find out why, let’s debug the managed code that fires the ToastCompletedEvent. Stop the project, and then on the menu bar, choose **Debug &gt; Toaster Application properties**. Change **Debugger Type** to **Managed Only**. Again on the menu bar, choose **Debug &gt; Exceptions**, and then select **Common Language Runtime Exceptions**.

    Now run the app and click the make-toast button. The debugger catches an invalid cast exception. Although it’s not obvious from its message, this exception is occurring because proxies are missing for that interface.

    ![missing proxy](./images/debuggererrormissingproxy.png)

    The first step in creating a proxy and stub for a component is to add a unique ID or GUID to the interfaces. However, the GUID format to use differs depending on whether you're coding in C#, Visual Basic, or another .NET language, or in C++.

## To generate GUIDs for the component's interfaces (C# and other .NET languages)

1.  On the menu bar, choose Tools &gt; Create GUID. In the dialog box, select 5. \[Guid(“xxxxxxxx-xxxx...xxxx)\]. Choose the New GUID button and then choose the Copy button.

    ![guid generator tool](./images/guidgeneratortool.png)

2.  Go back to the interface definition, and then paste the new GUID just before the IToaster interface, as shown in the following example. (Don't use the GUID in the example. Every unique interface should have its own GUID.)

    ```cpp
    [Guid("FC198F74-A808-4E2A-9255-264746965B9F")]
        public interface IToaster...
    ```

3.  Add a using directive for the System.Runtime.InteropServices namespace.
4.  Repeat these steps for the IToast interface.

## To generate GUIDs for the component's interfaces (C++)

1.  On the menu bar, choose Tools &gt; Create GUID. In the dialog box, select 3. static const struct GUID = {...}. Choose the New GUID button and then choose the Copy button.

2.  Paste the GUID just before the IToaster interface definition. After you paste, the GUID should resemble the following example. (Don't use the GUID in the example. Every unique interface should have its own GUID.)

    ```cpp 
      <Extensions> <!--Use your own GUIDs!!!-->
        <Extension Category="windows.activatableClass.proxyStub">
          <ProxyStub ClassId="1ecafeff-1ee1-504a-9af5-a68c6fb2b47d">
            <Path>Proxies.dll</Path>
            <Interface Name="IToast" InterfaceId="F8D30778-9EAF-409C-BCCD-C8B24442B09B"/>
            <Interface Name="IToaster"  InterfaceId="E976784C-AADE-4EA4-A4C0-B0C2FD1307C3"/>  
            <Interface Name="ITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast" InterfaceId="1ecafeff-1ee1-504a-9af5-a68c6fb2b47d"/>
          </ProxyStub>      
        </Extension>
      </Extensions>
    ```

    Paste the Extensions XML node as a direct child of the Package node, and a peer of, for example, the Resources node.

2.  Before moving on, it’s important to ensure that:

    -   The ProxyStub ClassId is set to the first GUID in the ToasterComponent\_i.c file. Use the first GUID that's defined in this file for the classId. (This might be the same as the GUID for ITypedEventHandler2.)
    -   The Path is the package relative path of the proxy binary. (In this walkthrough, proxies.dll is in the same folder as ToasterApplication.winmd.)
    -   The GUIDs are in the correct format. (This is easy to get wrong.)
    -   The interface IDs in the manifest match the IIDs in ToasterComponent\_i.c file.
    -   The interface names are unique in the manifest. Because these are not used by the system, you can choose the values. It is a good practice to choose interface names that clearly match interfaces that you have defined. For generated interfaces, the names should be indicative of the generated interfaces. You can use the ToasterComponent\_i.c file to help you generate interface names.

3.  If you try to run the solution now, you will get an error that proxies.dll is not part of the payload. Open the shortcut menu for the **References** folder in the ToasterApplication project and then choose **Add Reference**. Select the check box next to the Proxies project. Also, make sure that the check box next to ToasterComponent is also selected. Choose the **OK** button.

    The project should now build. Run the project and verify that you can make toast.

## Related topics

* [Creating Windows Runtime Components in C++](creating-windows-runtime-components-in-cpp.md)



<!--HONumber=May16_HO2-->


