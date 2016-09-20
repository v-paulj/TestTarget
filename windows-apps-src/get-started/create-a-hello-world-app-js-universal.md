---
author: GrantMeStrength
ms.assetid: CFB3601D-3459-465F-80E2-520F57B88F62
title: Erstellen der App Hello, world (JS)
description: "In diesem Lernprogramm erfahren Sie, wie Sie JavaScript und HTML zum Erstellen einer einfachen Hello, world-App für die Universelle Windows-Plattform (UWP) unter Windows 10 verwenden."
translationtype: Human Translation
ms.sourcegitcommit: 2e0965f964f6f2e10b895d99244b66458eb15903
ms.openlocfilehash: 6c81b24f7fa9abe036d4ccd22ee8fa24c011fe77

---
# Erstellen der App „Hello, world“ (JS)

In diesem Lernprogramm erfahren Sie, wie Sie JavaScript und HTML zum Erstellen einer einfachen „Hello, World“-App für die universelle Windows-Plattform (UWP) unter Windows 10 verwenden. Mit nur einem Projekt in Microsoft Visual Studio können Sie eine App erstellen, die auf allen Geräten mit Windows10 ausgeführt werden kann.

Hier erfahren Sie Folgendes:

-   Erstellen Sie ein neues **Visual Studio 2015**-Projekt für **Windows 10** und die **UWP**.
-   Hinzufügen von HTML-Inhalt zu Ihrer Startseite
-   Behandeln von Touch-, Stift- und Mauseingaben
-   Ausführen des Projekts auf dem lokalen Desktop und dem Phone Emulator in Visual Studio
-   Verwenden einer Windows-Bibliothek für JavaScript-Steuerelemente

## Vorbereitung

-   [Was ist eine universelle Windows-App](whats-a-uwp.md)?
-   [Neuigkeiten in Windows10](https://dev.windows.com/whats-new-windows-10-dev-preview)?
-   Zum Durcharbeiten dieses Lernprogramms benötigen Sie Windows 10 und Visual Studio 2015. [Vorbereiten](get-set-up.md).
-   Außerdem wird davon ausgegangen, dass Sie das Standardfensterlayout in Visual Studio verwenden. Wenn Sie das Standardlayout ändern, können Sie es im Menü **Fenster** mit dem Befehl **Fensterlayout zurücksetzen** wiederherstellen.

## Schritt1: Erstellen eines neuen Projekts in Visual Studio

Zunächst erstellen wir eine neue App namens `HelloWorld`. Gehen Sie dazu wie folgt vor:
1.  Starten Sie Visual Studio 2015.

2.  Wählen Sie im Menü **Datei** die Befehle **Neu > Projekt...** aus, um das Dialogfeld *Neues Projekt* anzuzeigen.

3.  Erweitern Sie in der Liste der Vorlagen auf der linken Seite **Installiert > Vorlagen > JavaScript > Windows**, und wählen Sie dann **Universell** aus, um eine Liste der UWP-Projektvorlagen anzuzeigen. Wählen Sie **WinJS-App (Universelles Windows)** aus.

    ![Das Fenster „Neues Projekt“ ](images/winjs-tut-newproject.png)

    In diesem Lernprogramm verwenden wir die Vorlage **WinJS-App** . Mit dieser Vorlage wird eine minimale UWP-App erstellt, die kompiliert und ausgeführt wird, aber keine Steuerelemente oder Daten für die Benutzeroberfläche enthält. Die App wird im weiteren Verlauf dieses Lernprogramms noch mit Steuerelementen und Daten versehen.

   (Sollten diese Optionen nicht angezeigt werden, vergewissern Sie sich, dass die Entwicklungstools für universelle Windows-Apps installiert sind. Weitere Informationen finden Sie unter [Vorbereiten](get-set-up.md).)

4.  Geben Sie im Textfeld **Name** den Namen „HelloWorld“ ein.
5.  Klicken Sie auf **OK**, um das Projekt zu erstellen.
6.  Sie werden aufgefordert, eine **Zielversion** und eine **mindestens erforderliche Version** von Windows auszuwählen, die unterstützt werden sollen. Die Standardeinstellungen sind in Ordnung, klicken Sie daher auf **OK**.

    Visual Studio erstellt Ihr Projekt und zeigt es im **Projektmappen-Explorer**an.

    ![Visual Studio-Projektmappen-Explorer für das „HelloWorld“-Projekt](images/winjs-tut-helloworld.png)

**WinJS App** ist zwar nur eine Minimalvorlage, umfasst aber trotzdem eine Reihe von Dateien:

-   Eine Manifestdatei (package.appxmanifest), die Ihre App beschreibt (Name, Beschreibung, Kachel, Startseite, Begrüßungsbildschirm usw.) und die in der App enthaltenen Dateien aufführt
-   Einen Satz mit Logobildern („images/Square150x150Logo.scale-200.png“, „images/Square44x44Logo.scale-200.png“ und „images/Wide310x150Logo.scale-200.png“), die im Startmenü angezeigt werden
-   Ein Bild der App für den Windows Store (images/StoreLogo.png)
-   Einen Begrüßungsbildschirm (images/SplashScreen.scale-200.png), der beim Start der App angezeigt wird
-   Eine Startseite (index.html) und eine entsprechende JavaScript-Datei (main.js), die beim Start der App ausgeführt werden

Doppelklicken Sie zum Anzeigen und Bearbeiten der Dateien auf die gewünschte Datei im **Projektmappen-Explorer**.

Diese Dateien werden für alle UWP-Apps mit JavaScript benötigt. Sie sind Teil jedes Projekts, das Sie mit Visual Studio erstellen.

## Schritt2: Starten der App


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

Die App wird in einem Fenster geöffnet, und zuerst wird ein standardmäßiger Begrüßungsbildschirm angezeigt. Der Begrüßungsbildschirm setzt sich aus einem Bild (SplashScreen.png) und einer Hintergrundfarbe (in der Manifestdatei der App angegeben) zusammen.

Nach dem Ausblenden des Begrüßungsbildschirms wird Ihre App angezeigt. Sie enthält einen schwarzen Bildschirm mit dem Text „Hier Inhalt einfügen“.

![Die App „HelloWorld“ auf einem PC](images/helloworld-1-winjs.png)

Drücken Sie die WINDOWS-TASTE, um das Menü **Start** zu öffnen, und zeigen Sie alle Apps an. Beachten Sie, dass beim lokalen Bereitstellen der App dem Menü **Start** die dazugehörige Kachel hinzugefügt wird. Wenn Sie die App erneut ausführen möchten (nicht im Debugmodus), tippen oder klicken Sie im Menü **Start** auf die Kachel.

Viel zu bieten hat die App zwar noch nicht, aber trotzdem: Herzlichen Glückwunsch! Sie haben Ihre erste UWP-App erstellt!

**So beenden Sie das Debuggen**

-   Klicken Sie auf der Symbolleiste auf die Schaltfläche **Debuggen beenden** (![Schaltfläche „Debuggen beenden“](images/stopdebug.png)).

   oder

   Klicken Sie im Menü **Debuggen** auf **Debuggen beenden**.

   oder

   Schließen Sie das App-Fenster.

### Starten der App in einem Emulator für mobile Geräte

Ihre App kann auf jedem Windows10-Gerät ausgeführt werden. Wir sehen uns nun an, wie sie auf einem Windows Phone dargestellt wird.

Zusätzlich zu den Optionen zum Debuggen auf einem Desktopgerät enthält Visual Studio Optionen zum Bereitstellen und Debuggen Ihrer App auf einem physischen mobilen Gerät, das an den Computer angeschlossen ist, oder in einem Emulator für mobile Geräte. Sie können zwischen Emulatoren für Geräte mit unterschiedlichen Arbeitsspeicher- und Bildschirmkonfigurationen wählen.

-   **Gerät**
-   **Emulator <SDK version> WVGA 4Zoll 512MB**
-   **Emulator <SDK version> WVGA 4Zoll 1GB**
-   usw. (verschiedene Emulatoren mit anderen Konfigurationen)

(Sollten diese Emulatoren nicht angezeigt werden, vergewissern Sie sich, dass die Entwicklungstools für universelle Windows-Apps installiert sind. Weitere Informationen finden Sie unter [Vorbereiten](get-set-up.md).)

Es empfiehlt sich, Ihre App auf einem Gerät mit kleinem Bildschirm und begrenztem Arbeitsspeicher zu testen. Wählen Sie daher die Option **Emulator 10.0.14393.0 WVGA 4 inch 512MB**.

**So beginnen Sie mit dem Debuggen in einem Emulator für mobile Geräte**

1.  Wählen Sie auf der **Standardsymbolleiste** im Menü mit den Zielgeräten (![Menü „Debuggen starten“](images/startdebug-full.png)) die Option **Emulator 10.0.14393.0 WVGA 4 inch 512MB**.
2.  Klicken Sie in der Symbolleiste auf die Schaltfläche **Debuggen starten** (![Schaltfläche „Debuggen starten“](images/startdebug-sm.png)).

   oder

   Klicken Sie im Menü **Debuggen** auf **Debuggen starten**.


Visual Studio startet den ausgewählten Emulator, stellt dann die App bereit und startet sie. Beim ersten Start benötigt der Emulator möglicherweise etwas Zeit zum Starten. Falls Fehler in Zusammenhang mit HyperV angezeigt werden, klicken Sie auf **Wiederholen**, um sie zu beheben. Im Emulator für mobile Geräte sieht die App wie folgt aus.

![Erster App-Bildschirm auf dem mobilen Gerät](images/helloworld-1-winjs-phone.png)

## Schritt3: Anpassen der Startseite

Eine der Dateien, die Visual Studio für Sie erstellt hat, ist **index.html** (die Startseite Ihrer App). Wenn die App ausgeführt wird, zeigt sie den Inhalt der Startseite an. Die Startseite enthält auch Verweise auf die Codedateien und Stylesheets der App. Hier sehen Sie die Startseite, die Visual Studio für Sie erstellt hat:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>HelloWorld</title>

    <!-- WinJS references -->
    <link href="lib/winjs-4.0.1/css/ui-light.css" rel="stylesheet" />
    <script src="lib/winjs-4.0.1/js/base.js"></script>
    <script src="lib/winjs-4.0.1/js/ui.js"></script>

    <!-- HelloWorld references -->
    <link href="/css/default.css" rel="stylesheet" />
    <script src="/js/main.js"></script>
</head>
<body class="win-type-body">
    <div>Content goes here!</div>
</body>
</html>
```

Fügen wir der Datei „default.html“ doch ein paar neue Inhalte hinzu. Gehen Sie beim Hinzufügen von Inhalten genauso vor wie bei anderen HTML-Dateien, und fügen Sie die Inhalte in das [body](https://msdn.microsoft.com/library/windows/apps/Hh453011)-Element ein. Sie können Ihre App unter Verwendung von HTML5-Elementen erstellen. Hierbei gelten allerdings [einige wenige Ausnahmen](https://msdn.microsoft.com/library/windows/apps/Hh465380). Sie können also HTML5-Elemente wie [h1](https://msdn.microsoft.com/library/windows/apps/Hh441078), [p](https://msdn.microsoft.com/library/windows/apps/Hh453431), [button](https://msdn.microsoft.com/library/windows/apps/Hh453017), [div](https://msdn.microsoft.com/library/windows/apps/Hh453133) und [img](https://msdn.microsoft.com/library/windows/apps/Hh466114) verwenden.

**Bearbeiten Sie Ihre Startseite**

1.  Ersetzen Sie den vorhandenen Inhalt im **body**-Element durch eine Überschrift erster Ebene mit dem Text „Hello, world!“, einen Text zum Anfordern des Benutzernamens, ein **input**-Element zum Akzeptieren des Benutzernamens, ein **button**- Element und ein **div**-Element. Weisen Sie IDs für **input**, **button** und **div** zu.

 ```html
    <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What's your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
    </body>
 ```

2.  Führen Sie die App auf dem lokalen Computer aus. Sie sieht ungefähr wie folgt aus:

![Die App „HelloWorld“ mit neuem Inhalt](images/helloworld-2-winjs.png)

   Sie können zwar in das **input**-Element schreiben, jedoch geschieht derzeit nichts, wenn Sie auf das **button**-Element klicken. Einige Elemente, z.B. **button**, können Meldungen senden, wenn bestimmte Ereignisse eintreten. Dank dieser Ereignismeldungen können Sie mit einer Aktion auf das Ereignis reagieren. Sie fügen den Code zum Reagieren auf das Ereignis in eine Ereignishandlermethode ein.

   In den nächsten Schritten erstellen wir einen Ereignishandler für das **button**-Element, der eine personalisierte Begrüßung anzeigt. Den Ereignishandlercode fügen wir der Datei „main.js“ hinzu.

## Schritt4: Erstellen eines Ereignishandlers

Bei der Erstellung unseres neuen Projekts wurde von Visual Studio die Datei „/js/main.js“ erstellt. Die Datei enthält Code zum Behandeln des Lebenszyklus der App. In diese Datei schreiben Sie außerdem zusätzlichen Code, um für die Datei „index.html“ Interaktivität zu ermöglichen.

Öffnen Sie die Datei „main.js“.

Vor dem Hinzufügen unseres eigenen Codes sehen wir uns zunächst die ersten und die letzten Codezeilen in der Datei an:

```javascript
(function () {
    "use strict";

     // Omitted code

 })();
```

Sie fragen sich vielleicht, worum es hier geht: Diese Codezeilen umschließen den Rest des Codes von „main.js“ in einer selbstausführenden anonymen Funktion. Eine selbstausführende anonyme Funktion erleichtert es Ihnen, Namenskonflikte oder Situationen zu vermeiden, in denen Sie versehentlich einen Wert ändern, der nicht geändert werden sollte. Außerdem sparen Sie so überflüssige IDs im globalen Namespace, was wiederum der Leistung zugute kommt. Es sieht vielleicht ein bisschen merkwürdig aus, ist aber eine empfehlenswerte Programmiermethode.

Die nächste Codezeile aktiviert den [Strict-Modus](https://msdn.microsoft.com/library/windows/apps/br230269.aspx) für Ihren JavaScript-Code. Der Strict-Modus bietet eine zusätzliche Fehlerprüfung für Ihren Code. So verhindert er beispielsweise die Verwendung implizit deklarierter Variablen und die Zuweisung eines Werts zu einer schreibgeschützten Eigenschaft.

Sehen Sie sich den restlichen Code in „main.js“ an. Er behandelt die [activated](https://msdn.microsoft.com/library/windows/apps/BR212679)- und [checkpoint](https://msdn.microsoft.com/library/windows/apps/BR229839)-Ereignisse Ihrer App. Mit diesen Ereignissen beschäftigen wir uns später noch ausführlicher. Für den Moment genügt es, wenn Sie wissen, dass das **activated**-Ereignis beim Start Ihrer App ausgelöst wird.

```javascript
   (function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;
    var isFirstActivation = true;

    app.onactivated = function (args) {
          if (args.detail.kind === activation.ActivationKind.voiceCommand) {
            // TODO: Handle relevant ActivationKinds. For example, if your app can be started by voice commands,
            // this is a good place to decide whether to populate an input field or choose a different initial view.
        }
          else if (args.detail.kind === activation.ActivationKind.launch) {
            // A Launch activation happens when the user launches your app via the tile
            // or invokes a toast notification by clicking or tapping on the body.
              if (args.detail.arguments) {
                // TODO: If the app supports toasts, use this value from the toast payload to determine where in the app
                // to take the user in response to them invoking a toast notification.
              }
              else if (args.detail.previousExecutionState === activation.ApplicationExecutionState.terminated) {
                // TODO: This application had been suspended and was then terminated to reclaim memory.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
                // Note: You may want to record the time when the app was last suspended and only restore state if they've returned after a short period.
            }
        }

        if (!args.detail.prelaunchActivated) {
            // TODO: If prelaunchActivated were true, it would mean the app was prelaunched in the background as an optimization.
            // In that case it would be suspended shortly thereafter.
            // Any long-running operations (like expensive network or disk I/O) or changes to user state which occur at launch
            // should be done here (to avoid doing them in the prelaunch case).
            // Alternatively, this work can be done in a resume or visibilitychanged handler.
        }

        if (isFirstActivation) {
            // TODO: The app was activated and had not been running. Do general startup initialization here.
            document.addEventListener("visibilitychange", onVisibilityChanged);
            args.setPromise(WinJS.UI.processAll());
        }

        isFirstActivation = false;
    };
```

Definieren wir nun einen Ereignishandler für Ihr [button](https://msdn.microsoft.com/library/windows/apps/Hh453017)-Element. Unser neuer Ereignishandler ruft den Namen des Benutzers aus dem `nameInput` [input](https://msdn.microsoft.com/library/windows/apps/Hh453271)-Steuerelement ab und verwendet die Informationen, um eine Begrüßung an das `greetingOutput` **div**-Element auszugeben, das Sie im letzten Abschnitt erstellt haben.

### Verwenden von Ereignissen für Touch-, Maus- und Stifteingaben

In einer UWP-App müssen Sie sich keine Gedanken über die Unterschiede zwischen Toucheingaben, Mauseingaben und anderen Zeigereingabearten machen. Sie können ganz einfach gewohnte Ereignisse wie [click](https://msdn.microsoft.com/library/windows/apps/Hh441312) verwenden, da diese für alle Eingabearten funktionieren.

**Tipp:** Ihre App kann auch die neuen Ereignisse *MSPointer\** und *MSGesture\** verwenden. Diese funktionieren für Touch-, Maus- und Stifteingaben und stellen zusätzliche Informationen zu dem Gerät bereit, mit dem das Ereignis ausgelöst wurde. Weitere Informationen finden Sie unter [Reagieren auf Benutzerinteraktionen](https://msdn.microsoft.com/library/windows/apps/Hh700412) sowie unter [Gesten, Manipulationen und Interaktionen](https://msdn.microsoft.com/library/windows/apps/Hh761498).

Im nächsten Schritt erstellen wir den Ereignishandler.

**Erstellen des Ereignishandlers**

1.  Erstellen Sie in main.js nach dem [**app.oncheckpoint**](https://msdn.microsoft.com/library/windows/apps/BR229839)-Ereignishandler und vor dem Aufruf von [**app.start**](https://msdn.microsoft.com/library/windows/apps/BR229705) eine [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312)-Ereignishandlerfunktion namens `buttonClickHandler`, die einen Einzelparameter namens `eventInfo` übernimmt.
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

Sie haben den Ereignishandler zu main.js hinzugefügt. Nun müssen Sie ihn registrieren.

## Schritt5: Registrieren des Ereignishandlers beim App-Start


Jetzt müssen wir nur noch den Ereignishandler bei der Schaltfläche registrieren. Die empfohlene Vorgehensweise für die Registrierung eines Ereignishandlers besteht darin, den [addEventListener](https://msdn.microsoft.com/library/windows/apps/Hh441145) in Ihrem Code aufzurufen. Ein guter Zeitpunkt für die Registrierung des Ereignishandlers ist die Aktivierung der App. Wie Sie sehen, hat Visual Studio praktischerweise bereits Code in der Datei main.js für die Behandlung der App-Aktivierung generiert.


Der Code prüft innerhalb des [onactivated](https://msdn.microsoft.com/library/windows/apps/BR212679)-Handlers, welche Aktivierungsart vorliegt. Es gibt viele verschiedene Arten von Aktivierungen. Die App wird beispielsweise aktiviert, wenn sie vom Benutzer gestartet wird und wenn der Benutzer eine Datei öffnen möchte, die mit der App verknüpft ist. (Weitere Informationen finden Sie unter [App-Lebenszyklus](https://msdn.microsoft.com/library/windows/apps/Mt243287).)

Wir interessieren uns für die [launch](https://msdn.microsoft.com/library/windows/apps/BR224693)-Aktivierung. Eine App wird *gestartet*, wenn sie nicht ausgeführt und dann von einem Benutzer aktiviert wird. [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/Hh440975) wird unabhängig davon aufgerufen, ob die App schon einmal beendet wurde oder ob sie gerade zum ersten Mal gestartet wird. Die **WinJS.UI.processAll**-Funktion ist in einen Aufruf der [setPromise](https://msdn.microsoft.com/library/windows/apps/JJ215609)-Methode eingeschlossen, mit dessen Hilfe sichergestellt wird, dass der Begrüßungsbildschirm erst dann ausgeblendet wird, wenn die App-Seite bereit ist.

**Tipp** Die **WinJS.UI.processAll**-Funktion sucht in der Datei „default.html“ nach WinJS-Steuerelementen und initialisiert sie. Bislang haben wir noch keines dieser Steuerelemente hinzugefügt. Es empfiehlt sich aber, diesen Code zu behalten, falls Sie später noch welche hinzufügen möchten.

Ein guter Zeitpunkt für die Registrierung von Ereignishandlern für Steuerelemente, die keine WinJS-Steuerelemente sind, ist direkt nach dem Aufruf von **WinJS.UI.processAll**.

**Registrieren Ihres Ereignishandlers**

-   Rufen Sie im [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679)-Ereignishhandler in „main.js“ `helloButton` ab, und verwenden Sie den [**addEventListener**](https://msdn.microsoft.com/library/windows/apps/Hh441145), um unseren Ereignishandler für das [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312)-Ereignis zu registrieren. Fügen Sie diesen Code nach dem Aufruf von [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) hinzu.

```javascript
   app.onactivated = function (args) {
           // Omitted code
           if (isFirstActivation) {
              document.addEventListener("visibilitychange", onVisibilityChanged);
              args.setPromise(WinJS.UI.processAll());

              // Add your code to retrieve the button and register the event handler.
              var helloButton = document.getElementById("helloButton");
              helloButton.addEventListener("click", buttonClickHandler, false);
            }

```    



Führen Sie die App aus. Wenn Sie Ihren Namen in das Textfeld eingeben und anschließend auf die Schaltfläche klicken, zeigt die App eine personalisierte Begrüßung an.

**Hinweis** Eine ausführliche Erklärung dafür, warum wir unser Ereignis im Code mithilfe von [addEventListener](https://msdn.microsoft.com/library/windows/apps/Hh441145) registrieren, anstatt das [onclick](https://msdn.microsoft.com/library/windows/apps/Hh441312)-Ereignis im HTML festzulegen, finden Sie unter [Codieren einfacher Apps](https://msdn.microsoft.com/library/windows/apps/Hh780660).

## Schritt 6: Hinzufügen eines Steuerelements aus der Windows-Bibliothek für JavaScript


Neben Standard-HTML-Steuerelementen können Sie in Ihrer App alle Steuerelemente in der [Windows-Bibliothek für JavaScript](https://msdn.microsoft.com/library/windows/apps/BR229782) verwenden, wie z. B. die Steuerelemente [WinJS.UI.DatePicker](https://msdn.microsoft.com/library/windows/apps/BR211681), [WinJS.UI.FlipView](https://msdn.microsoft.com/library/windows/apps/BR211711), [WinjS.UI.ListView](https://msdn.microsoft.com/library/windows/apps/BR211837) und [WinJS.UI.Rating](https://msdn.microsoft.com/library/windows/apps/BR211895).

Im Gegensatz zu HTML-Steuerelementen besitzen WinJS-Steuerelemente keine dedizierten Markupelemente: So können Sie z. B. kein [Rating](https://msdn.microsoft.com/library/windows/apps/BR211895)-Steuerelement erstellen, indem Sie ein `<rating />`-Element hinzufügen. Zum Hinzufügen eines WinJS-Steuerelements erstellen Sie ein **div**-Element und verwenden das [data-win-control](https://msdn.microsoft.com/library/windows/apps/Hh440969)-Attribut, um den gewünschten Steuerelementtyp anzugeben. Zum Hinzufügen eines **Rating**-Steuerelements legen Sie das Attribut auf „WinJS.UI.Rating“ fest.

**Fügen Sie Ihrer App-UI ein Rating-Steuerelement hinzu.**

1.  Fügen Sie in der Datei „index.html“ ein [label](https://msdn.microsoft.com/library/windows/apps/Hh453321)- und ein [Rating](https://msdn.microsoft.com/library/windows/apps/BR211895)-Steuerelement nach dem `greetingOutput` **div**-Element hinzu.

    ```html
    <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What's your name?</p>
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

   ![Die App „Hello, World“ mit einem Steuerelement der Windows-Bibliothek für JavaScript](images/helloworld-4-winjs.png)

> Damit das **Rating**-Steuerelement geladen werden kann, muss die Seite [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/Hh440975) aufrufen. Da in der App eine der Visual Studio-Vorlagen verwendet wird, enthält „main.js“ bereits einen Aufruf von **WinJS.UI.processAll**, wie bereits erläutert wurde. Sie müssen daher keinen Code hinzufügen.

Jetzt ändert sich beim Klicken auf das **Rating**-Steuerelement die Bewertung, aber sonst geschieht nichts weiter. Wir wollen nun einen Ereignishandler verwenden, der etwas tut, wenn der Benutzer die Bewertung ändert.

## Schritt 7: Registrieren eines Ereignishandlers für ein Steuerelement der Windows-Bibliothek für JavaScript


Das Registrieren eines Ereignishandlers für ein WinJS-Steuerelement unterscheidet sich etwas vom Registrieren eines Ereignishandlers für ein Standard-HTML-Steuerelement. Weiter oben wurde erwähnt, dass der **onactivated**-Ereignishandler die **WinJS.UI.processAll**-Methode aufruft, um WinJS in Ihrem Markup zu initialisieren. Der **WinJS.UI.processAll**-Aufruf ist in einen Aufruf der **setPromise**-Methode eingeschlossen, wie z. B.:

```javascript
            args.setPromise(WinJS.UI.processAll());           
```

Wenn **Rating** ein Standard-HTML-Steuerelement wäre, könnten Sie nach diesem Aufruf von **WinJS.UI.processAll** Ihren Ereignishandler hinzufügen. Bei einem WinJS-Steuerelement wie dem hier verwendeten **Rating**-Element ist es jedoch etwas komplizierter. Da **WinJS.UI.processAll** das **Rating**-Steuerelement für uns erstellt, können wir den Ereignishandler für **Rating** erst dann hinzufügen, wenn **WinJS.UI.processAll** die Verarbeitung beendet hat.

Wenn **WinJS.UI.processAll** eine normale Methode wäre, könnten wir den **Rating**-Ereignishandler direkt nach dem Aufruf registrieren. Die **WinJS.UI.processAll**-Methode ist aber asynchron, sodass nachfolgender Code unter Umständen bereits ausgeführt wird, bevor **WinJS.UI.processAll** abgeschlossen wurde. Wie gehen wir nun vor? Wir verwenden ein [Promise](https://msdn.microsoft.com/library/windows/apps/BR211867)-Objekt, um benachrichtigt zu werden, wenn **WinJS.UI.processAll** abgeschlossen ist.

Wie alle asynchronen WinJS-Methoden gibt **WinJS.UI.processAll** ein **Promise**-Objekt zurück. Ein **Promise**-Objekt ist eine Zusage, dass in Zukunft etwas geschieht. Wenn dies der Fall ist, sagen wir, dass das **Promise**-Objekt (also die Zusage) erfüllt wurde.

[Promise](https://msdn.microsoft.com/library/windows/apps/BR211867)-Objekte verfügen über eine [then](https://msdn.microsoft.com/library/windows/apps/BR229728)-Methode, die als Parameter eine completed-Funktion übernimmt. Das **Promise**-Objekt ruft diese Funktion auf, nachdem es abgeschlossen wurde.

Durch Einfügen Ihres Codes in eine completed-Funktion und deren Übergabe an die **then**-Methode des **Promise**-Objekts können Sie sicher sein, dass Ihr Code nach dem Abschluss von **WinJS.UI.processAll** ausgeführt wird.

**Ausgabe des Bewertungswerts, den der Benutzer auswählt**

1.  Erstellen Sie in der Datei „index.html“ ein [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133)-Element, das den Bewertungswert anzeigt, und weisen Sie ihm die **ID** „ratingOutput“ zu.

    ```html
        <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What's your name?</p>
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

2.  Erstellen Sie in der Datei „main.js“ einen Ereignishandler für das [change](https://msdn.microsoft.com/library/windows/apps/BR211891)-Ereignis des **Rating**-Steuerelements namens `ratingChanged`. Der [eventInfo](https://msdn.microsoft.com/library/windows/apps/Hh465776)-Parameter enthält eine **detail.tentativeRating**-Eigenschaft, welche die neue Benutzerbewertung liefert. Rufen Sie diesen Wert ab, und zeigen Sie ihn im **div**-Element der Ausgabe an.

    ```javascript
        function ratingChanged(eventInfo) {

            var ratingOutput = document.getElementById("ratingOutput");
            ratingOutput.innerText = eventInfo.detail.tentativeRating;
        }
```

3.  Aktualisieren Sie den Code im [onactivated](https://msdn.microsoft.com/library/windows/apps/BR212679)-Ereignishhandler, der [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/Hh440975) aufruft, indem Sie einen Aufruf der [then](https://msdn.microsoft.com/library/windows/apps/BR229728)-Methode hinzufügen und ihr eine `completed`-Funktion übergeben. Rufen Sie in der `completed`-Funktion das `ratingControlDiv`-Element ab, in dem sich das [Rating](https://msdn.microsoft.com/library/windows/apps/BR211895)-Steuerelement befindet. Rufen Sie dann mithilfe der [winControl](https://msdn.microsoft.com/library/windows/apps/Hh770814)-Eigenschaft das eigentliche **Rating**-Steuerelement ab. (In diesem Beispiel wird die `completed` -Funktion inline definiert.)

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

4.  Es ist zwar eleganter, Ereignishandler für HTML-Steuerelemente nach dem Aufruf von [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) zu registrieren, sie können jedoch auch innerhalb der `completed`-Funktion registriert werden. Der Einfachheit halber fahren wir fort und verlegen alle Registrierungen von Ereignishandlern ins Innere des [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728)-Ereignishandlers.

    Hier ist der aktualisierte [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679)-Ereignishandler:

    ```javascript
    (function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;
    var isFirstActivation = true;

    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.voiceCommand) {
            // TODO: Handle relevant ActivationKinds. For example, if your app can be started by voice commands,
            // this is a good place to decide whether to populate an input field or choose a different initial view.
        }
        else if (args.detail.kind === activation.ActivationKind.launch) {
            // A Launch activation happens when the user launches your app via the tile
            // or invokes a toast notification by clicking or tapping on the body.
            if (args.detail.arguments) {
                // TODO: If the app supports toasts, use this value from the toast payload to determine where in the app
                // to take the user in response to them invoking a toast notification.
            }
            else if (args.detail.previousExecutionState === activation.ApplicationExecutionState.terminated) {
                // TODO: This application had been suspended and was then terminated to reclaim memory.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
                // Note: You may want to record the time when the app was last suspended and only restore state if they've returned after a short period.
            }
        }

        if (!args.detail.prelaunchActivated) {
            // TODO: If prelaunchActivated were true, it would mean the app was prelaunched in the background as an optimization.
            // In that case it would be suspended shortly thereafter.
            // Any long-running operations (like expensive network or disk I/O) or changes to user state which occur at launch
            // should be done here (to avoid doing them in the prelaunch case).
            // Alternatively, this work can be done in a resume or visibilitychanged handler.
        }

        if (isFirstActivation) {
            // TODO: The app was activated and had not been running. Do general startup initialization here.
            document.addEventListener("visibilitychange", onVisibilityChanged);

            args.setPromise(WinJS.UI.processAll().then(function completed() {
                var ratingControlDiv = document.getElementById("ratingControlDiv");
                var ratingControl = ratingControlDiv.winControl;
                ratingControl.addEventListener("change",ratingChanged, false);
            }));

            var helloButton = document.getElementById("helloButton");
            helloButton.addEventListener("click", buttonClickHandler, false);

        }

        isFirstActivation = false;
    };

    ```        

    Führen Sie die App aus. Wenn Sie einen Bewertungswert auswählen, wird der numerische Wert unter dem [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895)-Steuerelement ausgegeben.

![Die fertig gestellte App „Hello World“ auf einem PC](images/helloworld-5-winjs.png)

## Zusammenfassung

Herzlichen Glückwunsch, Sie haben Ihre erste App für Windows 10 und die universelle Windows-Plattform mit JavaScript und HTML erstellt!



<!--HONumber=Aug16_HO3-->


