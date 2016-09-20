---
author: DelfCo
Description: "Nehmen Sie Zeichenfolgenressourcen für Ihre Benutzeroberfläche in Ressourcendateien auf. Sie können dann in Ihrem Code oder Markup auf diese Zeichenfolgen verweisen."
title: Aufnehmen von UI-Zeichenfolgen in Ressourcen
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: Put UI strings into resources
template: detail.hbs
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: b44d9235e34b8d4c75f663029d1dde3f87bd0eb7

---

# Aufnehmen von UI-Zeichenfolgen in Ressourcen





**Wichtige APIs**

-   [**ApplicationModel.Resources.ResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br206014)
-   [**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864)

Nehmen Sie Zeichenfolgenressourcen für Ihre Benutzeroberfläche in Ressourcendateien auf. Sie können dann in Ihrem Code oder Markup auf diese Zeichenfolgen verweisen.

In diesem Thema wird gezeigt, welche Schritte Sie ausführen, um Ihrer universellen Windows-App Zeichenfolgenressourcen für mehrere Sprachen hinzuzufügen und die App kurz zu testen.

## <span id="put_strings_into_resource_files__instead_of_putting_them_directly_in_code_or_markup."></span><span id="PUT_STRINGS_INTO_RESOURCE_FILES__INSTEAD_OF_PUTTING_THEM_DIRECTLY_IN_CODE_OR_MARKUP."></span>Platzieren Sie Zeichenfolgen in Ressourcendateien und nicht direkt im Code oder Markup.


1.  Öffnen Sie Ihre Projektmappe in Visual Studio (oder erstellen Sie eine neue).

2.  Öffnen Sie in Visual Studio „package.appxmanifest“, rufen Sie die Registerkarte **Anwendung** auf, und legen Sie (in diesem Beispiel) die Standardsprache auf „en-US“ fest. Wenn Ihre Projektmappe mehrere Dateien namens „package.appxmanifest“ enthält, führen Sie die Schritte für jede Datei aus.
    <br>
            **Hinweis**  Damit haben Sie die Standardsprache für das Projekt festgelegt. Die Standardsprachressourcen werden verwendet, wenn die bevorzugte Sprache des Benutzers oder die Anzeigesprachen nicht mit den von der Anwendung bereitgestellten Sprachressourcen übereinstimmen.
3.  Erstellen Sie einen Ordner, um dort die Ressourcendateien zu speichern.
    1.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt (das freigegebene Projekt, falls die Projektmappe mehrere Projekte enthält), und wählen Sie **Hinzufügen**&gt;**Neuer Ordner** aus.
    2.  Geben Sie dem neuen Ordner den Namen „Strings“.
    3.  Wenn der neue Ordner nicht im Projektmappen-Explorer sichtbar ist, wählen Sie im Microsoft Visual Studio-Menü **Projekt**&gt;**Alle Dateien anzeigen** aus, während das Projekt noch ausgewählt ist.

4.  Erstellen Sie einen Unterordner und eine Ressourcendatei für Englisch (USA).
    1.  Klicken Sie mit der rechten Maustaste auf den Ordner „Strings“, und fügen Sie darunter einen neuen Ordner hinzu. Geben Sie ihm den Namen „en-US“. Die Ressourcendatei muss in einem Ordner mit dem Sprachtag [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) abgelegt werden. Details zum Sprachqualifizierer und eine Liste häufiger Sprachtags finden Sie unter [Benennen von Ressourcen mit Qualifizierern](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324).
    2.  Klicken Sie mit der rechten Maustaste auf den Ordner „en-US“, und wählen Sie **Hinzufügen**&gt;**Neues Element** aus.
    3.  
                    **XAML:** Wählen Sie „Ressourcendatei (.resw)“ aus.
<br>
            **HTML:** Wählen Sie „Ressourcendatei (.resjson)“ aus.

    4.  Klicken Sie auf **Hinzufügen**. Dadurch wird eine Ressourcendatei mit dem Standardnamen „Resources.resw“ (für **XAML**) oder „resources.resjson“ (für **HTML**) hinzugefügt. Es wird empfohlen, diesen Standarddateinamen zu verwenden. Apps können ihre Ressourcen in andere Dateien partitionieren. Sie müssen jedoch darauf achten, richtig auf die Dateien zu verweisen (siehe [Laden von Zeichenfolgenressourcen](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)).
    5.  
            **Nur XAML:** Wenn Sie RESX-Dateien ausschließlich mit Zeichenfolgenressourcen aus vorherigen .NET-Projekten haben, wählen Sie **Hinzufügen**&gt;**Vorhandenes Element** aus, fügen Sie die RESX-Datei hinzu, und benennen Sie sie in eine RESW-Datei um.
    6.  Öffnen Sie die Datei, und fügen Sie mit dem Editor die folgenden Ressourcen hinzu:

        **XAML:**

        Strings/en-US/Resources.resw ![Ressource hinzufügen, Englisch](images/addresource-en-us.png) In diesem Beispiel identifizieren „Greeting.Text“ und „Farewell“ die Zeichenfolgen, die angezeigt werden sollen. „Greeting.Width“ identifiziert die „Width“-Eigenschaft der „Greeting“-Zeichenfolge. Kommentare eignen sich gut, um ggf. zusätzliche Anweisungen für Übersetzer bereitzustellen, die die Zeichenfolgen in andere Sprachen lokalisieren.

        **HTML:**

        Die neue Datei enthält Standardinhalt. Ersetzen Sie ihn durch folgenden Inhalt (der dem Standardinhalt unter Umständen ähnlich ist):

        Strings/en-US/resources.resjson

        ```        json
        {
                "greeting"              : "Hello",
                "_greeting.comment"     : "A welcome greeting",

                "farewell"              : "Goodbye",
                "_farewell.comment"     : "A goodbye"
        }
        ```

        Hierbei handelt es sich um strikte JSON (JavaScript Object Notation)-Syntax, bei der nach jedem Name/Wert-Paar mit Ausnahme des letzten Paars ein Komma stehen muss. In diesem Beispiel identifizieren „greeting“ und „farewell“ die Zeichenfolgen, die angezeigt werden sollen. Die anderen Paare („\_greeting.comment“ und „\_farewell.comment“) sind Kommentare zur Beschreibung der Zeichenfolgen. Kommentare eignen sich gut, um ggf. zusätzliche Anweisungen für Übersetzer bereitzustellen, die die Zeichenfolgen in andere Sprachen lokalisieren.

## <span id="associate_controls_to_resources."></span><span id="ASSOCIATE_CONTROLS_TO_RESOURCES."></span>Ordnen Sie Steuerelemente zu Ressourcen zu.


**Nur XAML:**

Sie müssen jedes Steuerelement, das lokalisierten Text erfordert, mit der RESW-Datei verknüpfen. Verwenden Sie dazu das **x:Uid**-Attribut in den XAML-Elementen wie folgt:

```XML
<TextBlock x:Uid="Greeting" Text="" />
```

Für den Ressourcennamen geben Sie den **Uid**-Attributwert und die Eigenschaft an, die die übersetzte Zeichenfolge erhält (in diesem Fall die Eigenschaft „Text“). Sie können andere Eigenschaften/Werte für verschiedene Sprachen festlegen, z.B. "Greeting.Width". Sie sollten mit solchen layoutbezogenen Eigenschaften jedoch vorsichtig umgehen. Versuchen Sie stets, ein dynamisches Layout auf Grundlage des Gerätebildschirms für die Steuerelemente zu verwenden.

Beachten Sie weiterhin, dass die verknüpften Eigenschaften unterschiedlich in RESW-Dateien behandelt werden, z.B. "AutomationPeer.Name". Sie müssen den Namespace explizit wie folgt ausschreiben:

```XML
MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name</code></pre></td>
```

## <span id="add_string_resource_identifiers_to_code_and_markup."></span><span id="ADD_STRING_RESOURCE_IDENTIFIERS_TO_CODE_AND_MARKUP."></span>Hinzufügen von Zeichenfolgenressourcen-Bezeichnern im Code und im Markup


**XAML:**

Sie können in Ihrem Code dynamisch auf Zeichenfolgen verweisen:

**C#**
```CSharp
var loader = new Windows.ApplicationModel.Resources.ResourceLoader();
var str = loader.GetString("Farewell");
```

**C++**
```cpp
auto loader = ref new Windows::ApplicationModel::Resources::ResourceLoader();
auto str = loader->GetString("Farewell");
```

**HTML:**

1.  Fügen Sie der HTML-Datei, sofern noch nicht vorhanden, Verweise auf die Windows-Bibliothek für JavaScript hinzu.

    
            **Hinweis**  Das folgende Beispiel zeigt den HTML-Code für die Datei „default.html“ des Windows-Projekts, das generiert wird, wenn Sie in Visual Studio ein neues JavaScript-Projekt vom Typ **Leere App (Universelle Windows-App)** erstellen. Beachten Sie, dass die Datei bereits Verweise auf die WinJS enthält.

    ```    HTML
    <!-- WinJS references -->
    <link href="WinJS/css/ui-dark.css" rel="stylesheet" />
    <script src="WinJS/js/base.js"></script>
    <script src="WinJS/js/ui.js"></script>
    ```

2.  Rufen Sie in dem zur HTML-Datei gehörenden JavaScript-Code die [**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864)-Funktion auf, wenn der HTML-Code geladen wird.

    ```    JavaScript
    WinJS.Application.onloaded = function(){
        WinJS.Resources.processAll();
    }
    ```
    
    Wenn zusätzlicher HTML-Code in ein [**WinJS.UI.Pages.PageControl**](https://msdn.microsoft.com/library/windows/apps/jj126158)-Objekt geladen wird, rufen Sie [**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864)(*element*) in der [**IPageControlMembers.ready**](https://msdn.microsoft.com/library/windows/apps/hh770590)-Methode der Seitensteuerung auf. Dabei entspricht *element* dem zu ladenden HTML-Element (und den untergeordneten Elementen). Dieses Beispiel basiert auf Szenario 6 von [Anwendungsressourcen und Lokalisierung – Beispiel](http://go.microsoft.com/fwlink/p/?linkid=227301):

    ```    JavaScript
    var output;
    
    var page = WinJS.UI.Pages.define("/html/scenario6.html", {
        ready: function (element, options) {
            output = element.querySelector('#output');
            WinJS.Resources.processAll(output); // Refetch string resources
            WinJS.Resources.addEventListener("contextchanged", refresh, false);
        }
        unload: function () { 
            WinJS.Resources.removeEventListener("contextchanged", refresh, false); 
        } 
    });
    ```

3.  Verweisen Sie in der HTML mit den Ressourcenbezeichnern („greeting“ und „farewell“) aus den Ressourcendateien auf die Zeichenfolgenressourcen.
    ```    HTML
    <h2><span data-win-res="{textContent: 'greeting';}"></span></h2>
    <h2><span data-win-res="{textContent: 'farewell'}"></span></h2>
    ```

4.  Verweisen Sie auf die Zeichenfolgenressourcen für Attribute.

    ```    HTML
    <div data-win-res="{attributes: {'aria-label'; : 'String1'}}" >
    ```

    Das allgemeine Muster des Attributs „data-win-res“ für HTML-Ersatz ist „data-win-res = "{*propertyname1*: '*Ressourcen-ID*', *propertyname2*: '*Ressource ID2*'}“.

    
            **Hinweis**  Wenn die Zeichenfolge kein Markup enthält, sollte die Ressource möglichst an die textContent-Eigenschaft und nicht an „innerHTML“ gebunden werden. Die textContent-Eigenschaft ist sehr viel schneller zu ersetzen als „innerHTML“.

5.  Verweisen Sie auf die Zeichenfolgenressourcen in JavaScript.
    <span codelanguage="JavaScript"></span>
    ```    JavaScript
    var el = element.querySelector('#header');
    var res = WinJS.Resources.getString('greeting');
    el.textContent = res.value;
    el.setAttribute('lang', res.lang);
    ```

## <span id="add_folders_and_resource_files_for_two_additional_languages."></span><span id="ADD_FOLDERS_AND_RESOURCE_FILES_FOR_TWO_ADDITIONAL_LANGUAGES."></span>Fügen Sie Ordner und Ressourcendateien für zwei zusätzliche Sprachen hinzu.


1.  Fügen Sie unter dem Ordner „Strings“ einen weiteren Ordner für Deutsch hinzu. Geben Sie dem Ordner den Namen „de-DE“ für Deutsch (Deutschland).
2.  Erstellen Sie eine weitere Ressourcendatei im Ordner „de-DE“, und fügen Sie Folgendes hinzu:

    **XAML:**

    strings/de-DE/Resources.resw

    ![Ressource hinzufügen, Deutsch](images/addresource-de-de.png)

    **HTML:**

    strings/de-DE/resources.resjson

    ```    json
    {
        "greeting"              : "Hallo",
        "_greeting.comment"     : "A welcome greeting.",

        "farewell"              : "Auf Wiedersehen",
        "_farewell.comment"     : "A goodbye."
    }
    ```

3.  Erstellen Sie einen weiteren Ordner mit dem Namen „fr-FR“ für Französisch (Frankreich). Erstellen Sie eine neue Ressourcendatei, und fügen Sie Folgendes hinzu:

    **XAML:**

    strings/fr-FR/Resources.resw ![Ressource hinzufügen, Französisch,](images/addresource-fr-fr.png)
    **HTML:**

    strings/fr-FR/resources.resjson

    ```    json
    {
        "greeting"              : "Bonjour",
        "_greeting.comment"     : "A welcome greeting.",

        "farewell"              : "Au revoir",
        "_farewell.comment"     : "A goodbye."
    }
    ```

## <span id="build_and_run_the_app."></span><span id="BUILD_AND_RUN_THE_APP."></span>Erstellen Sie die App, und führen Sie sie aus.


Testen Sie die App für die Standardanzeigesprache.

1.  Drücken Sie F5, um die App zu erstellen und auszuführen.
2.  Wie Sie sehen, werden Begrüßung und Verabschiedung in der bevorzugten Sprache des Benutzers angezeigt.
3.  Beenden Sie die App.

Testen Sie die App für die anderen Sprachen.

1.  Rufen Sie **Einstellungen** auf Ihrem Gerät auf.
2.  Wählen Sie **Zeit & Sprache** aus.
3.  Wählen Sie **Region & Sprache** (oder auf einem Telefon oder im Phone-Emulator) **Sprache**) aus.
4.  Wie Sie sehen, wird die Sprache, die beim Ausführen der App angezeigt wurde, als erste Sprache aufgeführt, also Englisch, Deutsch oder Französisch. Wenn die zuerst aufgeführte Sprache keine der drei genannten Sprachen ist, greift die App auf die erste Sprache in der Liste zurück, die von der App unterstützt wird.
5.  Wenn nicht alle drei Sprachen auf Ihrem Computer verfügbar sind, fügen Sie die fehlenden Sprache hinzu, indem Sie auf **Sprache hinzufügen** klicken und die Sprachen dann zur Liste hinzufügen.
6.  Um die App mit einer anderen Sprache zu testen, wählen Sie die Sprache in der Liste aus, und klicken Sie auf **Als Standard**. (Auf einem Telefon oder im Phone-Emulator wählen Sie die Sprache mit Tippen und Halten aus der Liste aus und tippen dann auf **Nach oben**, bis sie am Anfang der Liste steht.) Führen Sie die App anschließend aus.

## <span id="related_topics"></span>Verwandte Themen


* [Benennen von Ressourcen mithilfe von Qualifizierern](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)
* [Laden von Zeichenfolgenressourcen](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)
* [Das BCP-47-Sprachtag](http://go.microsoft.com/fwlink/p/?linkid=227302)
 

 






<!--HONumber=Jun16_HO4-->


