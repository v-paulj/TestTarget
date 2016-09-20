---
author: mcleanbyron
ms.assetid: 7a61c328-77be-4614-b117-a32a592c9efe
description: "Erfahren Sie mehr über Lösungen für allgemeine Entwicklungsprobleme mit den Microsoft Advertising-Bibliotheken in JavaScript/HTML-Apps."
title: "Handbuch zur Problembehandlung für HTML und JavaScript"
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: af4ea6f3360ea85d1c70ec9b757db65ec23c88af


---

# Handbuch zur Problembehandlung für HTML und JavaScript


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Dieses Thema enthält Lösungen für allgemeine Entwicklungsprobleme mit den Microsoft Advertising-Bibliotheken in JavaScript/HTML-Apps.

-   [HTML](#html)

    -   [AdControl wird nicht angezeigt](#html-notappearing)

    -   [Blackbox blinkt und wird ausgeblendet](#html-blackboxblinksdisappears)

    -   [Anzeigen werden nicht aktualisiert](#html-adsnotrefreshing)

-   [JavaScript](#js)

    -   [AdControl wird nicht angezeigt](#js-adcontrolnotappearing)

    -   [Blackbox blinkt und wird ausgeblendet](#js-blackboxblinksdisappears)

    -   [Anzeigen werden nicht aktualisiert](#js-adsnotrefreshing)

## HTML

<span id="html-notappearing"/>
### AdControl wird nicht angezeigt

1.  Stellen Sie sicher, dass die **Internet (Client)**-Funktion in „Package.appxmanifest“ ausgewählt ist.

2.  Stellen Sie sicher, dass der JavaScript-Verweis vorhanden ist. Ohne den Verweis ad.js im Abschnitt &lt;head&gt; (nach dem Verweis default.js) kann **AdControl** nicht angezeigt werden, und während der Erstellung tritt ein Fehler auf.

    Windows 10:

    ``` syntax
    <head>
        …
        <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
        …
    </head>
    ```

    Windows 8.x:

    ``` syntax
    <head>
        …
        <script src="//Microsoft.Advertising.JavaScript/ads/ad.js"></script>
        …
    </head>
    ```

3.  Überprüfen Sie die ID der Anwendung und der Anzeigeneinheit. Diese IDs müssen mit der Anwendungs-ID und Anzeigeneinheits-ID übereinstimmen, die Sie im Windows Dev Center erhalten haben. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in Ihrer App](set-up-ad-units-in-your-app.md).

    ``` syntax
    <div id="myAd" style="position: absolute; top: 50px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

4.  Überprüfen Sie die Eigenschaften **height** und **width**. Diese müssen auf eine der [unterstützten Anzeigengrößen für Werbebanner](supported-ad-sizes-for-banner-ads.md) festgelegt werden.

    ``` syntax
    <div id="myAd" style="position: absolute; top: 50px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

5.  Überprüfen Sie die Elementpositionierung. [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) muss sich im sichtbaren Bereich befinden.

6.  Überprüfen Sie die Eigenschaft **visibility**. Diese Eigenschaft darf nicht auf reduziert oder ausgeblendet festgelegt sein. Diese Eigenschaft kann als Inlineeigenschaft (wie unten dargestellt) oder in einem externen Stylesheet festgelegt werden.

    ``` syntax
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

7.  Überprüfen Sie die Eigenschaft **position**. Die Eigenschaft „position“ muss auf einen geeigneten Wert abhängig von den anderen Eigenschaften des Elements festgelegt werden (beispielsweise Rändern im übergeordneten Element und z-index). Diese Eigenschaft kann als Inlineeigenschaft (wie unten dargestellt) oder in einem externen Stylesheet festgelegt werden.

    ``` syntax
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

8.  Überprüfen Sie die Eigenschaft **z-index**. Die Eigenschaft **z-index** muss hoch genug festgelegt werden, sodass **AdControl** stets oberhalb anderer Elemente angezeigt wird. Diese Eigenschaft kann als Inlineeigenschaft (wie unten dargestellt) oder in einem externen Stylesheet festgelegt werden.

    ``` syntax
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

9.  Überprüfen Sie die externen Stylesheets. Wenn im Element **AdControl** Eigenschaften über ein externes Stylesheet festgelegt werden, müssen alle oben genannten Eigenschaften ordnungsgemäß festgelegt worden sein.

    ``` syntax
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

10. Überprüfen Sie das übergeordnete Element von **AdControl**. Wenn sich **AdControl** in einem übergeordneten Element befindet, muss das übergeordnete Element aktiv und sichtbar sein.

    ``` syntax
    <div style="position: absolute; width: 500px; height: 500px;">
        <div id="myAd" style="position: relative; top: 0px; left: 100px;
                              width: 250px; height: 250px; z-index: 1"
             data-win-control="MicrosoftNSJS.Advertising.AdControl"
             data-win-options="{applicationId: 'ApplicationID',
                                adUnitId: 'AdUnitID'}">
        </div>
    </div>
    ```

11. Stellen Sie sicher, dass **AdControl** im Viewport nicht ausgeblendet ist. **AdControl** muss sichtbar sein, damit Anzeigen ordnungsgemäß dargestellt werden.

12. Livewerte für [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) und [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) sollten nicht im Emulator getestet werden. Um sicherzustellen, dass **AdControl** erwartungsgemäß funktioniert, verwenden Sie sowohl für **ApplicationId** als auch für **AdUnitId** die Test-IDs in [Testmoduswerte](test-mode-values.md).

<span id="html-blackboxblinksdisappears"/>
### Blackbox blinkt und wird ausgeblendet

1.  Überprüfen Sie noch einmal alle Schritte im vorherigen Abschnitt [AdControl wird nicht angezeigt](#html-notappearing).

2.  Behandeln Sie das Ereignis **onErrorOccurred**, und bestimmen Sie anhand der an den Ereignishandler übergebenen Meldung, ob ein Fehler aufgetreten ist und welche Art von Fehler ausgelöst wurde. Weitere Informationen finden Sie in [Exemplarische Vorgehensweise zur Fehlerbehandlung in JavaScript](error-handling-in-javascript-walkthrough.md).

    ``` syntax
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 728px; height: 90px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger}">
    </div>

    <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

    Eine Blackbox wird am häufigsten dadurch verursacht, dass keine Anzeige verfügbar ist. Dieser Fehler bedeutet, dass durch die Anforderung keine Anzeige zurückgegeben werden kann.

3.  **AdControl** verhält sich normal. **AdControl** wird standardmäßig reduziert, wenn keine Anzeige dargestellt werden kann. Wenn andere Elemente demselben übergeordneten Element untergeordnet sind, können sie verschoben werden, um den freien Platz des reduzierten **AdControl**-Elements zu füllen, und bei der nächsten Anforderung erweitert werden.

<span id="html-adsnotrefreshing"/>
### Anzeigen werden nicht aktualisiert

1.  Überprüfen Sie die Eigenschaft **isAutoRefreshEnabled**. Diese optionale Eigenschaft ist standardmäßig auf true festgelegt. Wenn sie auf false festgelegt ist, muss die Methode **refresh** verwendet werden, um eine weitere Anzeige abzurufen.

    ``` syntax
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{ applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger,
                            isAutoRefreshEnabled: true}">
    </div>
    ```

2.  Überprüfen Sie Aufrufe der Methode **refresh**. Bei Verwendung der automatischen Aktualisierung kann **refresh** nicht verwendet werden, um eine weitere Anzeige abzurufen. Bei Verwendung der manuellen Aktualisierung sollte **refresh** abhängig von der aktuellen Datenverbindung des Geräts erst nach mindestens 30 bis 60 Sekunden aufgerufen werden.

    In diesem Beispiel wird die Verwendung der Methode **refresh** gezeigt. Der folgende HTML-Code zeigt ein Beispiel für die Instanziierung von **AdControl**, wenn **isAutoRefreshEnabled** auf false festgelegt ist.

    ``` syntax
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{ applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger,
                            isAutoRefreshEnabled: false}">
    </div>
    ```

    Dieses Beispiel zeigt die Verwendung der Funktion **refresh**.

    ``` syntax
    args.setPromise(WinJS.UI.processAll()
        .then(function (args) {
            window.setInterval(function()
            {
                document.getElementById("myAd").winControl.refresh();
            }, 60000)
        })
    );
    ```

3.  **AdControl** verhält sich normal. In einigen Fällen wird dieselbe Anzeige mehrmals in Folge angezeigt, wodurch der Eindruck entsteht, dass Anzeigen nicht aktualisiert werden.

<span id="js"/>
## JavaScript

<span id="js-adcontrolnotappearing"/>
### AdControl wird nicht angezeigt

1.  Stellen Sie sicher, dass die **Internet (Client)**-Funktion in „Package.appxmanifest“ ausgewählt ist.

2.  Stellen Sie sicher, dass **AdControl** instanziiert ist. Wenn **AdControl** nicht instanziiert ist, ist es nicht verfügbar.

    Die folgenden Codeausschnitte zeigen ein Beispiel für die Instanziierung von **AdControl**. Dieser HTML-Code zeigt ein Beispiel für das Einrichten der Benutzeroberfläche für **AdControl**.

    ``` syntax
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl">
    </div>
    ```

    Der folgende JavaScript-Code zeigt ein Beispiel für die Instanziierung von **AdControl**.

    ``` syntax
    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !==
                    activation.ApplicationExecutionState.terminated)
            {
                var adDiv = document.getElementById("myAd");
                var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
                {
                    applicationId: "{ApplicationID}",
                    adUnitId: "{AdUnitID}"
                 });                
                 myAdControl.onErrorOccurred = myAdError;
            } else {
                …
            }
        }
    }
    ```

3.  Überprüfen Sie das übergeordnete Element. Das übergeordnete Element **&lt;div&gt;** muss richtig zugewiesen, aktiv und sichtbar sein.

    ``` syntax
    var adDiv = document.getElementById("myAd");
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv, {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });  
    ```

4.  Überprüfen Sie die ID der Anwendung und der Anzeigeneinheit. Diese IDs müssen mit der Anwendungs-ID und Anzeigeneinheits-ID übereinstimmen, die Sie im Windows Dev Center erhalten haben. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in Ihrer App](set-up-ad-units-in-your-app.md).

    ``` syntax
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv, {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });  
    ```

5.  Überprüfen Sie das übergeordnete Element von **AdControl**. Das übergeordnete Element muss aktiv und sichtbar sein.

6.  Echte Werte für **ApplicationId** und **AdUnitId** sollten nicht im Emulator getestet werden. Um sicherzustellen, dass **AdControl** erwartungsgemäß funktioniert, verwenden Sie sowohl für **ApplicationId** als auch für **AdUnitId** die Test-IDs in [Testmoduswerte](test-mode-values.md).

<span id="js-blackboxblinksdisappears"/>
### Blackbox blinkt und wird ausgeblendet

1.  Überprüfen Sie noch einmal alle Schritte im Abschnitt [AdControl wird nicht angezeigt](#js-adcontrolnotappearing).

2.  Behandeln Sie das Ereignis **onErrorOccurred**, und bestimmen Sie anhand der an den Ereignishandler übergebenen Meldung, ob ein Fehler aufgetreten ist und welche Art von Fehler ausgelöst wurde. Weitere Informationen finden Sie in [Exemplarische Vorgehensweise zur Fehlerbehandlung in JavaScript](error-handling-in-javascript-walkthrough.md).

    In diesem Beispiel wird die Implementierung eines Fehlerhandlers gezeigt, der Fehlermeldungen meldet. Der folgende HTML-Codeausschnitt enthält ein Beispiel für das Einrichten der Benutzeroberfläche zum Anzeigen von Fehlermeldungen.

    ``` syntax
    <div style="position:absolute; width:100%; height:130px; top:300px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

    In diesem Beispiel wird die Instanziierung von **AdControl** gezeigt. Diese Funktion würde in der Datei app.onactivated eingefügt werden.

    ``` syntax
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });                
    myAdControl.onErrorOccurred = myAdError;
    ```

    In diesem Beispiel wird die Meldung von Fehlern gezeigt. Diese Funktion würde unterhalb der selbstausführenden Funktion in der Datei default.js eingefügt werden.

    ``` syntax
    WinJS.Utilities.markSupportedForProcessing
    (
        window.errorLogger = function (sender, evt)
        {
            adEvents.innerHTML = (new Date()).toLocaleTimeString() + ": " +
            sender.element.id + " error: " + evt.errorMessage + " error code: " +
            evt.errorCode + "<br>" + adEvents.innerHTML;
        }
    );
    ```

    Eine Blackbox wird am häufigsten dadurch verursacht, dass keine Anzeige verfügbar ist. Dieser Fehler bedeutet, dass durch die Anforderung keine Anzeige zurückgegeben werden kann.

3.  **AdControl** verhält sich normal. In einigen Fällen wird dieselbe Anzeige mehrmals in Folge angezeigt, wodurch der Eindruck entsteht, dass Anzeigen nicht aktualisiert werden.

<span id="js-adsnotrefreshing"/>
### Anzeigen werden nicht aktualisiert

1.  Überprüfen Sie die Eigenschaft **isAutoRefreshEnabled**. Diese optionale Eigenschaft ist standardmäßig auf **true** festgelegt. Wenn sie auf **false** festgelegt ist, muss die Methode **refresh** verwendet werden, um eine weitere Anzeige abzurufen.

    Im folgenden Beispiel wird die Verwendung der Eigenschaft **isAutoRefreshEnabled** gezeigt.

    ``` syntax
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
      applicationId: "{ApplicationID}",
      adUnitId: "{AdUnitID}",
      isAutoRefreshEnabled: true
    });  
    ```

2.  Überprüfen Sie Aufrufe der Methode **refresh**. Bei Verwendung der automatischen Aktualisierung kann **refresh** nicht verwendet werden, um eine weitere Anzeige abzurufen. Bei Verwendung der manuellen Aktualisierung sollte **refresh** abhängig von der aktuellen Datenverbindung des Geräts erst nach mindestens 30 bis 60 Sekunden aufgerufen werden.

    Dieses Beispiel zeigt das Erstellen von **div** für **AdControl**.

    ``` syntax
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl">
    </div>
    ```

    In diesem Beispiel wird die Verwendung der Funktion **refresh** gezeigt.

    ``` syntax
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
      applicationId: "{ApplicationID}",
      adUnitId: "{AdUnitID}",
      isAutoRefreshEnabled: false
    });
    …
    args.setPromise(WinJS.UI.processAll()
        .then(function (args) {
            window.setInterval(function()
            {
                document.getElementById("myAd").winControl.refresh();
            }, 60000)
        })
    );
    ```

3.  **AdControl** verhält sich normal. In einigen Fällen wird dieselbe Anzeige mehrmals in Folge angezeigt, wodurch der Eindruck entsteht, dass Anzeigen nicht aktualisiert werden.

 

 



<!--HONumber=Jun16_HO4-->


