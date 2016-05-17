---
author: martinekuan
ms.assetid: CFB3601D-3459-465F-80E2-520F57B88F62
title: Erstellen der App Hello, world (JS)
description: In diesem Lernprogramm erfahren Sie, wie Sie JavaScript und HTML zum Erstellen einer einfachen Hello, world-App für die Universelle Windows-Plattform (UWP) unter Windows 10 verwenden.
---
# Erstellen der App „Hello, world“ (JS)

In diesem Lernprogramm erfahren Sie, wie Sie JavaScript und HTML zum Erstellen einer einfachen „Hello, World“-App für die universelle Windows-Plattform (UWP) unter Windows 10 verwenden. Mit nur einem Projekt in Microsoft Visual Studio können Sie eine App erstellen, die auf allen Geräten mit Windows 10 ausgeführt werden kann. In diesem Thema geht es um die Erstellung einer App, die für Desktops und mobile Geräte gleichermaßen gut geeignet ist.

**Wichtig**   Dieses Lernprogramm ist für Microsoft Visual Studio 2015 und Windows 10 konzipiert. Die korrekte Funktionsweise mit früheren Versionen ist nicht sichergestellt.

Hier erfahren Sie Folgendes:

-   Erstellen eines neuen Projekts
-   Hinzufügen von HTML-Inhalt zu Ihrer Startseite
-   Behandeln von Touch-, Stift- und Mauseingaben
-   Ausführen des Projekts auf dem lokalen Desktop und auf dem Smartphone-Emulator in Visual Studio
-   Erstellen eigener Stile
-   Verwenden einer Windows-Bibliothek für JavaScript-Steuerelemente

##Vorbereitung


-   Wir beginnen direkt mit den Schritten zum Erstellen einer einfachen universellen App. Daher empfehlen wir Ihnen dringend, sich die Informationen unter [Neues in Windows 10](https://dev.windows.com/whats-new-windows-10-dev-preview) und [Was ist eine universelle Windows-App](whats-a-uwp.md) gründlich durchzulesen, bevor Sie sich diesem Lernprogramm widmen.
-   Zum Durcharbeiten dieses Lernprogramms benötigen Sie Windows 10 und Visual Studio 2015. Weitere Informationen finden Sie unter [Vorbereiten](get-set-up.md).
-   Außerdem wird davon ausgegangen, dass Sie das Standardfensterlayout in Visual Studio verwenden. Wenn Sie das Standardlayout ändern, können Sie es im Menü **Fenster** mit dem Befehl **Fensterlayout zurücksetzen** wiederherstellen.

##Schritt 1: Erstellen eines neuen Projekts in Visual Studio


Zunächst erstellen wir eine neue App namens `HelloWorld`. Gehen Sie wie folgt vor:

1.  Starten Sie Visual Studio 2015.

    Der Startbildschirm von Visual Studio 2015 wird angezeigt.

    (Hinweis: Im weiteren Verlauf wird Visual Studio 2015 kurz als Visual Studio bezeichnet.)

2.  Klicken Sie im Menü **Datei** auf **Neu** > **Projekt**.

    Das Dialogfeld **Neues Projekt** wird geöffnet. Im linken Bereich des Dialogfelds können Sie die Art der anzuzeigenden Vorlagen auswählen.

3.  Erweitern Sie im linken Bereich die Option **Installiert > Vorlagen > JavaScript > Windows**, und wählen Sie die Vorlagengruppe **Universal** aus. Im mittleren Bereich des Dialogfelds sehen Sie eine Liste mit Projektvorlagen für Apps der universellen Windows-Plattform (UWP).

    ![Das Fenster „Neues Projekt“ ](images/js-tut-newproject.png)

    In diesem Lernprogramm verwenden wir die Vorlage **Leere App** . Mit dieser Vorlage wird eine minimale UWP-App erstellt, die kompiliert und ausgeführt wird, aber keine Steuerelemente oder Daten für die Benutzeroberfläche enthält. Die App wird später in diesem Lernprogramm noch mit Steuerelementen und Daten versehen.

4.  Wählen Sie im mittleren Bereich die Projektvorlage **Leere App (universelle Windows-App)** aus.

    Die Vorlage **Leere App** stellt eine minimale UWP-App bereit, die kompiliert und ausgeführt wird, aber keine Steuerelemente oder Daten für die Benutzeroberfläche enthält. Die App wird später in diesem Lernprogramm noch mit Steuerelementen versehen.

5.  Geben Sie im Textfeld **Name** den Namen „HelloWorld“ ein.
6.  Klicken Sie auf **OK**, um das Projekt zu erstellen.

    Visual Studio erstellt Ihr Projekt und zeigt es im **Projektmappen-Explorer**an.

    ![Visual Studio-Projektmappen-Explorer für das „HelloWorld“-Projekt](images/js-tut-helloworld.png)

**Leere App** ist zwar nur eine Minimalvorlage, umfasst aber trotzdem eine Reihe von Dateien:

-   Eine Manifestdatei (package.appxmanifest), die Ihre App beschreibt (Name, Beschreibung, Kachel, Startseite, Begrüßungsbildschirm usw.) und die in der App enthaltenen Dateien aufführt
-   Einen Satz mit Logobildern („images/Square150x150Logo.scale-200.png“, „images/Square44x44Logo.scale-200.png“ und „images/Wide310x150Logo.scale-200.png“), die im Startmenü angezeigt werden
-   Ein Bild der App für den Windows Store (images/StoreLogo.png)
-   Einen Begrüßungsbildschirm (images/SplashScreen.scale-200.png), der beim Start der App angezeigt wird
-   Eine Startseite (default.html) und eine entsprechende JavaScript-Datei (default.js), die beim Start der App ausgeführt werden

Doppelklicken Sie zum Anzeigen und Bearbeiten der Dateien im **Projektmappen-Explorer** auf die gewünschte Datei.

Diese Dateien werden für alle UWP-Apps mit JavaScript benötigt. Sie sind Teil jedes Projekts, das Sie mit Visual Studio erstellen.

##Schritt 2: Starten der App


Sie haben nun eine sehr einfache App erstellt. Dies ist eine guter Zeitpunkt zum Erstellen, Bereitstellen und Starten Ihrer App, um sie in Aktion zu sehen. Sie können Ihre App auf dem lokalen Computer, in einem Simulator oder Emulator oder auf einem Remotegerät debuggen. Dies ist das Zielgerätmenü in Visual Studio.

![Dropdownliste mit Zielgeräten zum Debuggen Ihrer App](images/uap-debug.png)

### Starten der App auf einem Desktop-Gerät

Standardmäßig wird die App auf dem lokalen Computer ausgeführt. Das Menü mit den Zielgeräten enthält mehrere Optionen zum Debuggen Ihrer App auf Geräten der Desktopfamilie.

-   **Simulator**
-   **Lokaler Computer**
-   **Remotecomputer**

**So beginnen Sie mit dem Debuggen auf dem lokalen Computer**

1.  Stellen Sie sicher, dass auf der **Standardsymbolleiste** im Menü mit den Zielgeräten (![Menü „Debuggen starten“](images/startdebug-full.png)) die Option **Lokaler Computer** ausgewählt ist. (Dies ist die Standardeinstellung.)
2.  Klicken Sie auf der Symbolleiste auf die Schaltfläche **Debuggen starten** (![Schaltfläche „Debuggen starten“](images/startdebug-sm.png)).

   oder

   Klicken Sie im Menü **Debuggen** auf **Debuggen starten**.

   oder

   Drücken Sie F5.

Die App wird in einem Fenster geöffnet. Zuerst wird ein standardmäßiger Begrüßungsbildschirm angezeigt. Der Begrüßungsbildschirm setzt sich aus einem Bild (SplashScreen.png) und einer Hintergrundfarbe (in der Manifestdatei der App angegeben) zusammen.

Nach dem Ausblenden des Begrüßungsbildschirms wird Ihre App angezeigt. Sie enthält einen schwarzen Bildschirm mit dem Text „Hier Inhalt einfügen“.

![Die App „HelloWorld“ auf einem PC](images/helloworld-1-js.png)

Drücken Sie die WINDOWS-TASTE, um das Menü **Start** zu öffnen, und zeigen Sie alle Apps an. Beachten Sie, dass beim lokalen Bereitstellen der App dem Menü **Start** die dazugehörige Kachel hinzugefügt wird. Wenn Sie die App erneut ausführen möchten (nicht im Debugmodus), tippen oder klicken Sie im Menü **Start** auf die Kachel.

Viel zu bieten hat die App zwar noch nicht, aber trotzdem: Herzlichen Glückwunsch! Sie haben Ihre erste UWP-App erstellt!

**So beenden Sie das Debuggen**

-   Klicken Sie auf der Symbolleiste auf die Schaltfläche **Debuggen beenden** (![Schaltfläche „Debuggen beenden“](images/stopdebug.png)).

   oder

   Klicken Sie im Menü **Debuggen** auf **Debuggen beenden**.

   oder

   Schließen Sie das App-Fenster.

### Starten der App in einem Emulator für mobile Geräte

Ihre App kann auf jedem Windows 10-Gerät ausgeführt werden. Wir sehen uns nun an, wie sie auf einem Windows Phone dargestellt wird.

Zusätzlich zu den Optionen zum Debuggen auf einem Desktopgerät enthält Visual Studio Optionen zum Bereitstellen und Debuggen Ihrer App auf einem physischen mobilen Gerät, das an den Computer angeschlossen ist, oder in einem Emulator für mobile Geräte. Sie können zwischen Emulatoren für Geräte mit unterschiedlichen Arbeitsspeicher- und Bildschirmkonfigurationen wählen.

-   **Gerät**
-   **Emulator <SDK version> WVGA 4 Zoll 512 MB**
-   **Emulator <SDK version> WVGA 4 Zoll 1 GB**
-   usw. (verschiedene Emulatoren mit anderen Konfigurationen)

Es ist ratsam, Ihre App auf einem Gerät mit kleinem Bildschirm und begrenztem Arbeitsspeicher zu testen. Wählen Sie also die Option **Emulator 10.0.10240.0 WVGA 4 inch 512MB**.
**So beginnen Sie mit dem Debuggen in einem Emulator für mobile Geräte**

1.  Wählen Sie im Menü (![Debuggen starten](images/startdebug-full.png)) des Zielgeräts in der Symbolleiste **Standard** die Option **Emulator 10.0.10240.0 WVGA 4 inch 512MB**.
2.  Klicken Sie auf der Symbolleiste auf die Schaltfläche **Debuggen starten** (![Schaltfläche „Debuggen starten“](images/startdebug-sm.png)).

   oder

   Klicken Sie im Menü **Debuggen** auf **Debuggen starten**.

   
Visual Studio startet den ausgewählten Emulator, stellt die App bereit und startet sie. Im Emulator für mobile Geräte sieht die App wie folgt aus.

![Erster App-Bildschirm auf dem mobilen Gerät](images/helloworld-1-js-phone.png)

## Schritt 3: Anpassen der Startseite

Eine der Dateien, die Visual Studio für Sie erstellt hat, ist „default.html“ (die Startseite Ihrer App). Wenn die App ausgeführt wird, zeigt sie den Inhalt der Startseite an. Die Startseite enthält auch Verweise auf die Codedateien und Stylesheets der App. Hier sehen Sie die Startseite, die Visual Studio für Sie erstellt hat:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>HelloWorld</title>

    <!-- WinJS references -->
    <link href="WinJS/css/ui-dark.css" rel="stylesheet" />
    <script src="WinJS/js/base.js"></script>
    <script src="WinJS/js/ui.js"></script>

    <!-- HelloWorld references -->
    <link href="/css/default.css" rel="stylesheet" />
    <script src="/js/default.js"></script>
</head>
<body class="win-type-body">
    <p>Content goes here</p>
</body>
</html>
```

Fügen wir der Datei „default.html“ doch ein paar neue Inhalte hinzu. Gehen Sie beim Hinzufügen genauso vor wie bei anderen HTML-Dateien, und fügen Sie die Inhalte in das [**body**](https://msdn.microsoft.com/library/windows/apps/Hh453011)-Element ein. Sie können Ihre App unter Verwendung von HTML5-Elementen erstellen. Hierbei gelten allerdings [einige wenige Ausnahmen](https://msdn.microsoft.com/library/windows/apps/Hh465380). Sie können also HTML5-Elemente wie [**h1**](https://msdn.microsoft.com/library/windows/apps/Hh441078), [**p**](https://msdn.microsoft.com/library/windows/apps/Hh453431), [**button**](https://msdn.microsoft.com/library/windows/apps/Hh453017), [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) und [**img**](https://msdn.microsoft.com/library/windows/apps/Hh466114) verwenden.

**So passen Sie die Startseite an**

1.  Ersetzen Sie den vorhandenen Inhalt im [**body**](https://msdn.microsoft.com/library/windows/apps/Hh453011)-Element durch eine Überschrift erster Ebene mit dem Text „Hello, world!“, einen Text zum Anfordern des Benutzernamens, ein [**input**](https://msdn.microsoft.com/library/windows/apps/Hh453271)-Element zum Akzeptieren des Benutzernamens, ein [**button**](https://msdn.microsoft.com/library/windows/apps/Hh453017)- Element und ein [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133)-Element. Weisen Sie IDs für **input**, **button** und **div** zu.

 ```html
    <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What' s your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
    </body>
 ```

2.  Führen Sie die App auf dem lokalen Computer aus. Sie sieht ungefähr wie folgt aus:

![Die App „HelloWorld“ mit neuem Inhalt](images/helloworld-2-js.png)

   Sie können zwar im [**input**](https://msdn.microsoft.com/library/windows/apps/Hh453271)-Element schreiben, doch derzeit passiert nach dem Klicken auf das [**button**](https://msdn.microsoft.com/library/windows/apps/Hh453017)-Element noch nichts. Einige Elemente, z. B. **button**, können Meldungen senden, wenn bestimmte Ereignisse eintreten. Dank dieser Ereignismeldungen können Sie mit einer Aktion auf das Ereignis reagieren. Sie fügen den Code zum Reagieren auf das Ereignis in eine Ereignishandlermethode ein.

   In den nächsten Schritten erstellen wir daher einen Ereignishandler für das [**button**](https://msdn.microsoft.com/library/windows/apps/Hh453017)-Element, der eine personalisierte Begrüßung anzeigt. Den Ereignishandlercode fügen wir der Datei „default.js“ hinzu.

##Schritt 4: Erstellen eines Ereignishandlers

Bei der Erstellung unseres neuen Projekts hat Visual Studio die Datei „/js/default.js“ erstellt. Die Datei enthält Code zum Behandeln des Lebenszyklus der App. In dieser Datei schreiben Sie außerdem zusätzlichen Code, um Interaktivität für die Datei „default.html“ zu ermöglichen.

Öffnen Sie die Datei „default.js“.

Vor dem Hinzufügen unseres eigenen Codes sehen wir uns zunächst die ersten und die letzten Codezeilen in der Datei an:

```javascript
(function () {
    "use strict";

     // Omitted code 

 })(); 
```

Falls Sie sich jetzt fragen, was hier vor sich geht: Diese Codezeilen umschließen den Rest des Codes von „default.js“ in einer selbstausführenden anonymen Funktion. Eine selbstausführende anonyme Funktion erleichtert Ihnen das Vermeiden von Namenskonflikten oder Situationen, in denen Sie versehentlich einen Wert ändern, der nicht geändert werden sollte. Außerdem sparen Sie so überflüssige IDs im globalen Namespace, was wiederum der Leistung zugute kommt. Es sieht vielleicht ein bisschen merkwürdig aus, ist aber eine empfehlenswerte Programmiermethode.

Die nächste Codezeile aktiviert den [Strict-Modus](https://msdn.microsoft.com/en-us/library/windows/apps/br230269.aspx) für Ihren JavaScript-Code. Der Strict-Modus bietet eine zusätzliche Fehlerprüfung für Ihren Code. So verhindert er beispielsweise die Verwendung implizit deklarierter Variablen und die Zuweisung eines Werts zu einer schreibgeschützten Eigenschaft.

Sehen Sie sich den restlichen Code in „default.js“ an. Er behandelt das [**activated**](https://msdn.microsoft.com/library/windows/apps/BR212679)- und das [**checkpoint**](https://msdn.microsoft.com/library/windows/apps/BR229839)-Ereignis Ihrer App. Mit diesen Ereignissen beschäftigen wir uns später noch ausführlicher. Für den Moment genügt es, wenn Sie wissen, dass das **activated**-Ereignis beim Start Ihrer App ausgelöst wird.

```javascript
   (function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;

    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                // TODO: This application has been newly launched. Initialize your application here.
            } else {
                // TODO: This application was suspended and then terminated.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
            }
            args.setPromise(WinJS.UI.processAll());
        }
    };

    app.oncheckpoint = function (args) {
        // TODO: This application is about to be suspended. Save any state that needs to persist across suspensions here.
        // You might use the WinJS.Application.sessionState object, which is automatically saved and restored across suspension.
        // If you need to complete an asynchronous operation before your application is suspended, call args.setPromise().
    };

    app.start();
})();
```

Definieren wir nun einen Ereignishandler für Ihr [**button**](https://msdn.microsoft.com/library/windows/apps/Hh453017)-Element. Unser neuer Ereignishandler ruft den Namen des Benutzers aus dem `nameInput` [**input**](https://msdn.microsoft.com/library/windows/apps/Hh453271)-Steuerelement ab und verwendet ihn, um eine Begrüßung an das `greetingOutput` [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133)-Element auszugeben, das Sie im letzten Abschnitt erstellt haben.

### Verwenden von Ereignissen für Touch-, Maus- und Stifteingaben

In einer UWP-App müssen Sie sich keine Gedanken über die Unterschiede zwischen Toucheingaben, Mauseingaben und anderen Zeigereingabearten machen. Sie können ganz einfach gewohnte Ereignisse wie [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312) verwenden, da diese für alle Eingabearten funktionieren.

**Tipp**   Ihre App kann auch die neuen Ereignisse *MSPointer\** und *MSGesture\** verwenden. Diese funktionieren für Touch-, Maus- und Stifteingaben und stellen zusätzliche Informationen über das Gerät bereit, mit dem das Ereignis ausgelöst wurde. Weitere Informationen finden Sie unter [Reagieren auf Benutzerinteraktionen](https://msdn.microsoft.com/library/windows/apps/Hh700412) sowie unter [Gesten, Manipulationen und Interaktionen](https://msdn.microsoft.com/library/windows/apps/Hh761498).

Im nächsten Schritt erstellen wir den Ereignishandler.

**So erstellen Sie den Ereignishandler**

1.  Erstellen Sie in „default.js“ zwischen dem [**app.oncheckpoint**](https://msdn.microsoft.com/library/windows/apps/BR229839)-Ereignishandler und dem Aufruf von [**app.start**](https://msdn.microsoft.com/library/windows/apps/BR229705) eine [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312)-Ereignishandlerfunktion namens `buttonClickHandler`, die einen einzelnen Parameter übernimmt.
```javascript
    function buttonClickHandler(eventInfo) {
     
        }
```

2.  Rufen Sie innerhalb des Ereignishandlers den Namen des Benutzers aus dem `nameInput` [**input**](https://msdn.microsoft.com/library/windows/apps/Hh453271)-Steuerelement ab, und erstellen Sie damit eine Begrüßung. Zeigen Sie das Ergebnis mithilfe des `greetingOutput` [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133)-Elements an.
```javascript
    function buttonClickHandler(eventInfo) {
            var userName = document.getElementById("nameInput").value;
            var greetingString = "Hello, " + userName + "!";
            document.getElementById("greetingOutput").innerText = greetingString; 
        }
 ```

Sie haben „default.js“ den Ereignishandler hinzugefügt. Nun müssen Sie ihn registrieren.

## Schritt 5: Registrieren des Ereignishandlers beim App-Start


Jetzt müssen wir nur noch den Ereignishandler bei der Schaltfläche registrieren. Die empfohlene Vorgehensweise für die Registrierung eines Ereignishandlers ist der Aufruf von [**addEventListener**](https://msdn.microsoft.com/library/windows/apps/Hh441145) im Code. Ein guter Zeitpunkt für die Registrierung des Ereignishandlers ist die Aktivierung der App. Praktischerweise hat Visual Studio in der Datei „default.js“ in Form des [**app.onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679)-Ereignishandlers bereits Code für die Behandlung der App-Aktivierung generiert. Diesen Code sehen wir uns einmal genauer an.

```javascript
    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;

    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                // TODO: This application has been newly launched. Initialize your application here.
            } else {
                // TODO: This application was suspended and then terminated.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
            }
            args.setPromise(WinJS.UI.processAll());
        }
    };
```

Der Code prüft innerhalb des [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679)-Handlers, welche Aktivierungsart vorliegt. Es gibt viele verschiedene Arten von Aktivierungen. Die App wird beispielsweise aktiviert, wenn sie vom Benutzer gestartet wird und wenn der Benutzer eine Datei öffnen möchte, die mit der App verknüpft ist. (Weitere Informationen finden Sie unter [App-Lebenszyklus](https://msdn.microsoft.com/library/windows/apps/Mt243287).)

Wir interessieren uns für die [**launch**](https://msdn.microsoft.com/library/windows/apps/BR224693)-Aktivierung. Eine App wird *gestartet*, wenn sie nicht ausgeführt und dann von einem Benutzer aktiviert wird.

```javascript
    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
```

Handelt es sich bei der Aktivierung um eine Startaktivierung, prüft der Code, wie die App bei der letzten Ausführung beendet wurde.

```javascript
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                // TODO: This application has been newly launched. Initialize your application here.
            } else {
                // TODO: This application was suspended and then terminated.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
            }
```

Anschließend wird [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) aufgerufen.

```javascript
            args.setPromise(WinJS.UI.processAll());
        }
    };
```    

[
            **WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) wird unabhängig davon aufgerufen, ob die App schon einmal beendet wurde oder ob sie gerade zum ersten Mal gestartet wird. Die **WinJS.UI.processAll**-Funktion ist in einen Aufruf der [**setPromise**](https://msdn.microsoft.com/library/windows/apps/JJ215609)-Methode eingeschlossen, mit dessen Hilfe sichergestellt wird, dass der Begrüßungsbildschirm erst dann ausgeblendet wird, wenn die App-Seite bereit ist.

**Tipp**   Die [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975)-Funktion sucht in der Datei „default.html“ nach WinJS-Steuerelementen und initialisiert sie. Bislang haben wir noch keines dieser Steuerelemente hinzugefügt. Es empfiehlt sich aber, diesen Code zu behalten, falls Sie später noch welche hinzufügen möchten.

Ein gute Position für die Registrierung von Ereignishandlern für Steuerelemente, die keine WinJS-Steuerelemente sind, ist direkt nach dem Aufruf von [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975).

**So registrieren Sie Ihren Ereignishandler**

-   Rufen Sie `helloButton` im [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679)-Ereignishandler in „default.js“ ab, und registrieren Sie unseren Ereignishandler für das [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312)-Ereignis mit [**addEventListener**](https://msdn.microsoft.com/library/windows/apps/Hh441145). Fügen Sie diesen Code nach dem Aufruf von [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) hinzu.

```javascript
   app.onactivated = function (args) {
            if (args.detail.kind === activation.ActivationKind.launch) {
                if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                    // TODO: This application has been newly launched. Initialize your application here.
                } else {
                    // TODO: This application was suspended and then terminated.
                    // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
                }
                args.setPromise(WinJS.UI.processAll());

             // Retrieve the button and register our event handler. 
                var helloButton = document.getElementById("helloButton");
                helloButton.addEventListener("click", buttonClickHandler, false);
            }
        };
```    

Hier ist der vollständige Code für die aktualisierte Datei „default.js“:

```javascript
   (function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;

    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                // TODO: This application has been newly launched. Initialize your application here.
            } else {
                // TODO: This application was suspended and then terminated.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
            }
            args.setPromise(WinJS.UI.processAll());

            // Retrieve the button and register our event handler. 
            var helloButton = document.getElementById("helloButton");
            helloButton.addEventListener("click", buttonClickHandler, false);
        }
    };

    app.oncheckpoint = function (args) {
        // TODO: This application is about to be suspended. Save any state that needs to persist across suspensions here.
        // You might use the WinJS.Application.sessionState object, which is automatically saved and restored across suspension.
        // If you need to complete an asynchronous operation before your application is suspended, call args.setPromise().
    };

    function buttonClickHandler(eventInfo) {
        var userName = document.getElementById("nameInput").value;
        var greetingString = "Hello, " + userName + "!";
        document.getElementById("greetingOutput").innerText = greetingString;
    }

    app.start();
})();
```

Führen Sie die App aus. Wenn Sie Ihren Namen in das Textfeld eingeben und anschließend auf die Schaltfläche klicken, zeigt die App eine personalisierte Begrüßung an. Diese Begrüßung sieht auf dem lokalen Computer und im Emulator wie folgt aus.

![Eine personalisierte Begrüßung der App „HelloWorld“](images/helloworld-3-js.png)

![Eine personalisierte Begrüßung der App „HelloWorld“](images/helloworld-3-js-phone.png)

**Hinweis**   Eine ausführliche Erklärung dafür, warum wir unser Ereignis im Code mittels [**addEventListener**](https://msdn.microsoft.com/library/windows/apps/Hh441145) registrieren, anstatt das [**onclick**](https://msdn.microsoft.com/library/windows/apps/Hh441312)-Ereignis in HTML festzulegen, finden Sie unter [Codieren einfacher Apps](https://msdn.microsoft.com/library/windows/apps/Hh780660).

## Schritt 6: Hinzufügen eines Steuerelements aus der Windows-Bibliothek für JavaScript


Neben Standard-HTML-Steuerelementen können Sie in Ihrer App alle Steuerelemente aus der Windows-Bibliothek für JavaScript verwenden, z. B. die Steuerelemente [**WinJS.UI.DatePicker**](https://msdn.microsoft.com/library/windows/apps/BR211681), [**WinJS.UI.FlipView**](https://msdn.microsoft.com/library/windows/apps/BR211711), [**WinjS.UI.ListView**](https://msdn.microsoft.com/library/windows/apps/BR211837) und [**WinJS.UI.Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895).

Im Gegensatz zu HTML-Steuerelementen besitzen WinJS-Steuerelemente keine dedizierten Markupelemente: Sie können ein [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895)-Steuerelement erstellen, indem Sie z. B. ein `<rating />`-Element hinzufügen. Zum Hinzufügen eines WinJS-Steuerelements erstellen Sie ein [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133)-Element und geben mit dem [**data-win-control**](https://msdn.microsoft.com/library/windows/apps/Hh440969)-Attribut den gewünschten Steuerelementtyp an. Zum Hinzufügen eines **Rating**-Steuerelements legen Sie das Attribut auf „WinJS.UI.Rating“ fest.

Nun fügen wir Ihrer App ein [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895)-Steuerelement hinzu.

1.  Fügen Sie in der Datei „default.html“ hinter dem `greetingOutput` [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133)-Element ein [**label**](https://msdn.microsoft.com/library/windows/apps/Hh453321)- und ein [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895)-Steuerelement hinzu.

    ```html
    <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What' s your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
        <label for="ratingControlDiv">
            Rate this greeting:
        </label>
        <div id="ratingControlDiv" data-win-control="WinJS.UI.Rating">
        </div>
    </body> 
    ```

2.  Führen Sie die App auf dem lokalen Computer aus. Beachten Sie das neue [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895)-Steuerelement.

   ![Die App „Hello, World“ mit einem Steuerelement der Windows-Bibliothek für JavaScript](images/helloworld-4-js.png)

> Damit das [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895)-Steuerelement geladen werden kann, muss die Seite [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) aufrufen. Da in der App eine der Visual Studio-Vorlagen verwendet wird, enthält „default.js“ bereits einen Aufruf von **WinJS.UI.processAll**, wie bereits erläutert wurde. Sie müssen daher keinen Code schreiben.

Jetzt ändert sich beim Klicken auf das [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895)-Steuerelement die Bewertung, aber ansonsten geschieht nichts weiter. Wir wollen nun einen Ereignishandler verwenden, der etwas tut, wenn der Benutzer die Bewertung ändert.

## Schritt 7: Registrieren eines Ereignishandlers für ein Steuerelement der Windows-Bibliothek für JavaScript


Das Registrieren eines Ereignishandlers für ein WinJS-Steuerelement unterscheidet sich etwas vom Registrieren eines Ereignishandlers für ein Standard-HTML-Steuerelement. Weiter oben wurde erwähnt, dass der [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679)-Ereignishandler die [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975)-Methode zur Initialisierung von WinJS in Ihrem Markup aufruft. **WinJS.UI.processAll** ist in einen Aufruf der [**setPromise**](https://msdn.microsoft.com/library/windows/apps/JJ215609)-Methode eingeschlossen.

```javascript
            args.setPromise(WinJS.UI.processAll());           
```

Wenn [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) ein Standard-HTML-Steuerelement wäre, könnten Sie nach diesem Aufruf von [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) Ihren Ereignishandler hinzufügen. Bei einem WinJS-Steuerelement wie dem hier verwendeten **Rating**-Element ist es jedoch etwas komplizierter. Da **WinJS.UI.processAll** das **Rating**-Steuerelement für uns erstellt, können wir den Ereignishandler für **Rating** erst dann hinzufügen, wenn **WinJS.UI.processAll** die Verarbeitung beendet hat.

Wäre [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) eine normale Methode, könnten wir den [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895)-Ereignishandler direkt nach dem Aufruf registrieren. Die **WinJS.UI.processAll**-Methode ist aber asynchron, sodass nachfolgender Code unter Umständen bereits ausgeführt wird, bevor **WinJS.UI.processAll** abgeschlossen wurde. Wie gehen wir nun vor? Wir lassen uns von einem [**Promise**](https://msdn.microsoft.com/library/windows/apps/BR211867)-Objekt über den Abschluss von **WinJS.UI.processAll** benachrichtigen.

Wie alle asynchronen WinJS-Methoden gibt [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) ein [**Promise**](https://msdn.microsoft.com/library/windows/apps/BR211867)-Objekt zurück. Ein **Promise**-Objekt ist eine Zusage, dass in Zukunft etwas geschieht. Wenn dies der Fall ist, sagen wir, dass das **Promise**-Objekt (also die Zusage) erfüllt wurde.

[
              **Promise**
            ](https://msdn.microsoft.com/library/windows/apps/BR211867) -Objekte verfügen über eine [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728)-Methode, die als Parameter eine completed-Funktion übernimmt. Das **Promise**-Objekt ruft diese Funktion auf, nachdem es abgeschlossen wurde.

Durch Einfügen des Codes in eine completed-Funktion und deren Übergabe an die [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728)-Methode des [**Promise**](https://msdn.microsoft.com/library/windows/apps/BR211867)-Objekts können Sie sicher sein, dass Ihr Code nach dem Abschluss von [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) ausgeführt wird.

1.  Wir geben nun den entsprechenden Wert aus, wenn der Benutzer eine Bewertung auswählt. Erstellen Sie in der Datei „default.html“ ein [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133)-Element, das die Bewertung anzeigt, und weisen Sie ihm die ID ****„ratingOutput“ zu.
```html
        <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What' s your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
        <label for="ratingControlDiv">
            Rate this greeting:
        </label>
        <div id="ratingControlDiv" data-win-control="WinJS.UI.Rating">
        </div>
        <div id="ratingOutput"></div>
    </body>
```

2.  Erstellen Sie in der Datei „default.js“ den Ereignishandler `ratingChanged` für das Ereignis [**change**](https://msdn.microsoft.com/library/windows/apps/BR211891) des Steuerelements [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895). Der [**eventInfo**](https://msdn.microsoft.com/library/windows/apps/Hh465776)-Parameter enthält eine **detail.tentativeRating**-Eigenschaft, welche die neue Benutzerbewertung liefert. Rufen Sie diesen Wert ab, und zeigen Sie ihn im [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133)-Element für die Ausgabe an.

```javascript
        function ratingChanged(eventInfo) {

            var ratingOutput = document.getElementById("ratingOutput");
            ratingOutput.innerText = eventInfo.detail.tentativeRating; 
        }
    ```

3.  Update the code in the [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) event handler that calls [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) by adding a call to the [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728) method and passing it a `completed` function. In the `completed` function, retrieve the `ratingControlDiv` element that hosts the [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) control. Then use the [**winControl**](https://msdn.microsoft.com/library/windows/apps/Hh770814) property to retrieve the actual **Rating** control. (This example defines the `completed` function inline.)

```javascript
           args.setPromise(WinJS.UI.processAll().then(function completed() {

                    // Retrieve the div that hosts the Rating control.
                    var ratingControlDiv = document.getElementById("ratingControlDiv");

                    // Retrieve the actual Rating control.
                    var ratingControl = ratingControlDiv.winControl;

                    // Register the event handler. 
                    ratingControl.addEventListener("change", ratingChanged, false);

                }));
    ```

4.  While it's fine to register event handlers for HTML controls after the call to [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975), it's also OK to register them inside your `completed` function. For simplicity, let's go ahead and move all your event handler registrations inside the [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728) event handler.

Here's the updated [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) event handler:

```javascript
       app.onactivated = function (args) {
            if (args.detail.kind === activation.ActivationKind.launch) {
                if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                    // TODO: This application has been newly launched. Initialize your application here.
                } else {
                    // TODO: This application was suspended and then terminated.
                    // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
                }
                args.setPromise(WinJS.UI.processAll().then(function completed() {

                    // Retrieve the div that hosts the Rating control.
                    var ratingControlDiv = document.getElementById("ratingControlDiv");

                    // Retrieve the actual Rating control.
                    var ratingControl = ratingControlDiv.winControl;

                    // Register the event handler. 
                    ratingControl.addEventListener("change", ratingChanged, false);

                    // Retrieve the button and register our event handler. 
                    var helloButton = document.getElementById("helloButton");
                    helloButton.addEventListener("click", buttonClickHandler, false);

                }));

            }
        };
```        

5.  Führen Sie die App aus. Wenn Sie eine Bewertung auswählen, wird der numerische Wert unter dem [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895)-Steuerelement ausgegeben.

![Die fertig gestellte App „Hello World“ auf einem PC](images/helloworld-5-js.png)

![Die fertig gestellte App „Hello World“ auf einem Smartphone](images/helloworld-5-js-phone.png)

## Zusammenfassung

Herzlichen Glückwunsch, Sie haben Ihre erste App für Windows 10 und die universelle Windows-Plattform mit JavaScript und HTML erstellt!



<!--HONumber=May16_HO2-->


