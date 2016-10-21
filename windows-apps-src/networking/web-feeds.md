---
author: DelfCo
description: "Mithilfe von Fremdanbieterfeeds, die entsprechend den RSS- und Atom-Standards mit Features im Windows.Web.Syndication-Namespace generiert werden, können Sie die neuesten und beliebtesten Webinhalte abrufen oder erstellen."
title: RSS/Atom-Feeds
ms.assetid: B196E19B-4610-4EFA-8FDF-AF9B10D78843
translationtype: Human Translation
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: b20eb8a241d3cb7800904c26331ac39da93f4d44

---

# RSS/Atom-Feeds

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

**Wichtige APIs**

-   [**Windows.Data.Xml.Dom**](https://msdn.microsoft.com/library/windows/apps/br240819)
-   [**Windows.Web.AtomPub**](https://msdn.microsoft.com/library/windows/apps/br210609)
-   [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632)

Mithilfe von Fremdanbieterfeeds, die entsprechend den RSS- und Atom-Standards mit Features im [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632)-Namespace generiert werden, können Sie die aktuellsten und beliebtesten Webinhalte abrufen oder erstellen.

## Was ist ein Feed?

Ein Webfeed ist ein Dokument, das eine Reihe von Einzeleinträgen enthält, die sich aus Text, Links und Bildern zusammensetzen. Aktualisierungen von Feeds werden in Form neuer Einträge vorgenommen, mit denen aktuelle Inhalte im Web verbreitet werden. Nutzer von Inhalten können mithilfe einer Feedleser-App Feeds einer beliebigen Anzahl einzelner Inhaltsautoren zusammenfassen und verfolgen. Sie erhalten damit schnell und bequem Zugang zu den jeweils aktuellen Inhalten.

## Welche Feed-Formatstandards werden unterstützt?

Die universelle Windows-Plattform (UWP) unterstützt das Abrufen von Feeds für RSS-Formatstandards von 0.91 bis RSS 2.0 sowie für Atom-Standards von 0.3 bis 1.0. Klassen im [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632)-Namespace können Feeds und Feedelemente definieren, die sowohl RSS- als auch Atom-Elemente darstellen können.

Zudem können Feeddokumente in den Atom1.0- und RSS2.0-Formaten Elemente oder Attribute enthalten, die nicht in den offiziellen Spezifikationen definiert sind. Mit der Zeit haben sich diese benutzerdefinierten Elemente und Attribute zu einer Möglichkeit entwickelt, domänenspezifische Informationen zu definieren, die von anderen Webdienst-Datenformaten wie GData und OData genutzt werden. Zur Unterstützung dieses zusätzlichen Features stellt die [**SyndicationNode**](https://msdn.microsoft.com/library/windows/apps/br243585)-Klasse generische XML-Elemente dar. Durch die Verwendung von **SyndicationNode** mit Klassen im [**Windows.Data.Xml.Dom**](https://msdn.microsoft.com/library/windows/apps/br240819)-Namespace können Apps auf Attribute, Erweiterungen und beliebige verwendbare Inhalte zugreifen.

Beachten Sie, dass die UWP-Implementierung des Atom Publication Protocol ([**Windows.Web.AtomPub**](https://msdn.microsoft.com/library/windows/apps/br210609)) für die Veröffentlichung der Inhalte von Fremdanbietern lediglich Feedinhaltsvorgänge gemäß den Standards Atom und Atom Publication unterstützt.

## Verwenden von Fremdanbieterinhalt mit der Netzwerkisolation

Das Netzwerkisolationsfeature der UWP ermöglicht es Entwicklern, den Netzwerkzugriff einer UWP-App zu steuern und zu begrenzen. Nicht jede App muss Zugriff auf das Netzwerk haben. Ist dies für eine App jedoch erforderlich, bietet die UWP verschiedene Ebenen des Netzwerkzugriffs, die durch Auswahl der entsprechenden Funktionen aktiviert werden können.

Mit der Netzwerkisolation kann ein Entwickler für jede App den Umfang des erforderlichen Netzwerkzugriffs definieren. Eine App, für die der geeignete Umfang nicht definiert wurde, wird am Zugriff auf den angegebenen Netzwerktyp und an bestimmten Netzwerkanforderungstypen (ausgehende, vom Client initiierte Anforderungen oder sowohl eingehende, unaufgeforderte Anforderungen als auch ausgehende, vom Client initiierte Anforderungen) gehindert. Durch die Möglichkeit, die Netzwerkisolation festzulegen und zu erzwingen, wird sichergestellt, dass eine App im Falle ihres Missbrauchs nur auf Netzwerke zugreifen kann, für die der App explizit der Zugriff gestattet wurde. Hierdurch wird das Ausmaß der Auswirkungen auf andere Anwendungen und auf Windows erheblich reduziert.

Die Netzwerkisolation wirkt sich auf alle Klassenelemente in den [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632)-Namespace und im [**Windows.Web.AtomPub**](https://msdn.microsoft.com/library/windows/apps/br210609)-Namespace aus, die versuchen, auf das Netzwerk zuzugreifen. Die Netzwerkisolation wird unter Windows aktiv erzwungen. Ein Aufruf eines Klassenelements im **Windows.Web.Syndication**-Namespace oder **Windows.Web.AtomPub**-Namespace, der zu einem Netzwerkzugriff führt, kann aufgrund der Netzwerkisolation einen Fehler verursachen, falls die geeignete Netzwerkfunktion nicht aktiviert wurde.

Die Netzwerkfunktionen für eine App werden beim Erstellen der App im App-Manifest konfiguriert. Netzwerkfunktionen werden normalerweise bei der Entwicklung einer App mithilfe von Microsoft Visual Studio 2015 hinzugefügt. Sie können aber auch manuell mit einem Texteditor in der App-Manifestdatei festgelegt werden.

Ausführlichere Informationen zu Netzwerkisolation und Netzwerkfunktionen finden Sie im Abschnitt „Funktionen“ des Themas [Grundlagen zum Netzwerk](networking-basics.md).

## So wird's gemacht: Zugreifen auf einen Webfeed

In diesem Abschnitt wird beschrieben, wie Sie einen Webfeed mithilfe von Klassen im [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632)-Namespace der in C# oder Javascript geschriebenen UWP-App abrufen und anzeigen.

**Voraussetzungen**

Damit die UWP-App im Netzwerk verwendet werden kann, müssen Sie alle erforderlichen Netzwerkfunktionen in der Projektdatei **Package.appxmanifest** festlegen. Wenn Ihre App als Client eine Verbindung mit Remotediensten im Internet herstellen muss, ist die Funktion **internetClient** erforderlich. Weitere Informationen finden Sie im Abschnitt „Funktionen“ des Themas [Grundlagen zum Netzwerk](networking-basics.md).

**Abrufen von Fremdinhalten aus einem Webfeed**

Nun sehen wir uns etwas Code an, der demonstriert, wie ein Feed abgerufen werden kann und anschließend die einzelnen Elemente des Feeds angezeigt werden können. Bevor wir die Anforderung konfigurieren und senden können, definieren wir einige Variablen, die wir während des Vorgangs verwenden werden, und initialisieren eine Instanz von [**SyndicationClient**](https://msdn.microsoft.com/library/windows/apps/br243456), die die Methoden und Eigenschaften definiert, die wir zum Abrufen und Anzeigen des Feeds verwenden.

Der [**Uri**](https://msdn.microsoft.com/library/windows/apps/br226017)-Konstruktor löst eine Ausnahme aus, wenn das an den Konstruktor übergebene *uriString*-Element kein gültiger URI ist. Daher überprüfen wir das *uriString*-Element mithilfe eines try/catch-Blocks.

> [!div class="tabbedCodeSnippets"]
```csharp
Windows.Web.Syndication.SyndicationClient client = new Windows.Web.Syndication.SyndicationClient();
Windows.Web.Syndication.SyndicationFeed feed;

// The URI is validated by catching exceptions thrown by the Uri constructor.
Uri uri = null;

// Use your own uriString for the feed you are connecting to.
string uriString = "";

try
{
    uri = new Uri(uriString);
}
catch (Exception ex)
{
    // Handle the invalid URI here.
}
```
```javascript
var currentFeed = null;
var currentItemIndex = 0;
        
var client = new Windows.Web.Syndication.SyndicationClient();

// The URI is validated by catching exceptions thrown by the Uri constructor.
var uri = null;
try {
    uri = new Windows.Foundation.Uri(uriString);
} catch (error) {
    WinJS.log && WinJS.log("Error: Invalid URI");
    return;
}
```

Anschließend konfigurieren wir die Anforderung, indem wir alle erforderlichen Serveranmeldeinformationen (die [**ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br243461)-Eigenschaft), Proxyanmeldeinformationen (die [**ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br243459)-Eigenschaft) und HTTP-Header (die [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br243462)-Methode) festlegen. Da die grundlegenden Anforderungsparameter nun konfiguriert sind, haben wir ein gültiges, mit einer von der App bereitgestellten Feed-URI-Zeichenfolge erstelltes [**Uri**](https://msdn.microsoft.com/library/windows/apps/br226017)-Objekt. Das **Uri**-Objekt wird anschließend an die [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/br243460)-Funktion übergeben, um den Feed anzufordern.

In der Annahme, dass die gewünschten Feedinhalte zurückgegeben wurden, durchläuft der Beispielcode jedes Feedelement, wobei er **displayCurrentItem** aufruft (was wir als Nächstes definieren), um Elemente und ihre Inhalte über die UI als Liste anzuzeigen.

Beim Aufrufen der meisten asynchronen Netzwerkmethoden müssen Sie Code zum Behandeln von Ausnahmen schreiben. Ihr Ausnahmehandler kann detailliertere Informationen zur Ursache abrufen, um die Ausnahme besser verstehen und entsprechende Entscheidungen treffen zu können.

Die [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/br243460)-Methode löst eine Ausnahme aus, wenn keine Verbindung mit dem HTTP-Server hergestellt werden kann oder wenn das [**Uri**](https://msdn.microsoft.com/library/windows/apps/br226017)-Objekt nicht auf einen gültigen AtomPub- oder RSS-Feed verweist. Im JavaScript-Beispielcode wird eine **onError**-Funktion verwendet, um etwaige Ausnahmen abzufangen und ausführlichere Informationen zur Ausnahme auszugeben, wenn ein Fehler auftritt.

> [!div class="tabbedCodeSnippets"]
```csharp
try
{
    // Although most HTTP servers do not require User-Agent header, 
    // others will reject the request or return a different response if this header is missing.
    // Use the setRequestHeader() method to add custom headers.
    client.SetRequestHeader("User-Agent", "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)");

    feed = await client.RetrieveFeedAsync(uri);

    // Retrieve the title of the feed and store it in a string.
    string title = feed.Title.Text;

    // Iterate through each feed item.
    foreach (Windows.Web.Syndication.SyndicationItem item in feed.Items)
    {
        displayCurrentItem(item);
    }
}
catch (Exception ex)
{
    // Handle the exception here.
}
```
```javascript
function onError(err) {
    WinJS.log && WinJS.log(err, "sample", "error");

    // Match error number with a ErrorStatus value.
    // Use Windows.Web.WebErrorStatus.getStatus() to retrieve HTTP error status codes.
    var errorStatus = Windows.Web.Syndication.SyndicationError.getStatus(err.number);
    if (errorStatus === Windows.Web.Syndication.SyndicationErrorStatus.invalidXml) {
        displayLog("An invalid XML exception was thrown. Please make sure to use a URI that points to a RSS or Atom feed.");
    }
}

// Retrieve and display feed at given feed address.
function retreiveFeed(uri) {

    // Although most HTTP servers do not require User-Agent header, 
    // others will reject the request or return a different response if this header is missing.
    // Use the setRequestHeader() method to add custom headers.
    client.setRequestHeader("User-Agent", "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)");

    client.retrieveFeedAsync(uri).done(function (feed) {
        currentFeed = feed;

        WinJS.log && WinJS.log("Feed download complete.", "sample", "status");

        var title = "(no title)";
        if (currentFeed.title) {
            title = currentFeed.title.text;
        }
        document.getElementById("CurrentFeedTitle").innerText = title;

        currentItemIndex = 0;
        if (currentFeed.items.size > 0) {
            displayCurrentItem();
        }

        // List the items.
        displayLog("Items: " + currentFeed.items.size);
     }, onError);
}
```

Im vorherigen Schritt hat [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/br243460) die angeforderten Feedinhalte zurückgegeben, und der Beispielcode hat damit begonnen, die verfügbaren Feedelemente zu durchlaufen. Jedes dieser Elemente wird mithilfe eines [**SyndicationItem**](https://msdn.microsoft.com/library/windows/apps/br243533)-Objekts dargestellt, das alle vom jeweiligen Veröffentlichungsstandard (RSS oder Atom) bereitgestellten Eigenschaften und Inhalte des Elements enthält. Im folgenden Beispiel beobachten wir die **displayCurrentItem**-Funktion dabei, wie sie die einzelnen Elemente durchgeht und ihre Inhalte über verschiedene benannte UI-Elemente ausgibt.

> [!div class="tabbedCodeSnippets"]
```csharp
private void displayCurrentItem(Windows.Web.Syndication.SyndicationItem item)
{
    string itemTitle = item.Title == null ? "No title" : item.Title.Text;
    string itemLink = item.Links == null ? "No link" : item.Links.FirstOrDefault().ToString();
    string itemContent = item.Content == null ? "No content" : item.Content.Text;
    //displayCurrentItem is continued below.

```
```javascript
function displayCurrentItem() {
    var item = currentFeed.items[currentItemIndex];

    // Display item number.
    document.getElementById("Index").innerText = (currentItemIndex + 1) + " of " + currentFeed.items.size;

    // Display title.
    var title = "(no title)";
    if (item.title) {
        title = item.title.text;
    }
    document.getElementById("ItemTitle").innerText = title;

    // Display the main link.
    var link = "";
    if (item.links.size > 0) {
        link = item.links[0].uri.absoluteUri;
    }

    var link = document.getElementById("Link");
    link.innerText = link;
    link.href = link;

    // Display the body as HTML.
    var content = "(no content)";
    if (item.content) {
        content = item.content.text;
    }
    else if (item.summary) {
        content = item.summary.text;
    }
    document.getElementById("WebView").innerHTML = window.toStaticHTML(content);
                //displayCurrentItem is continued below.
```

Wie bereits angesprochen, unterscheidet sich der Inhaltstyp, der durch ein [**SyndicationItem**](https://msdn.microsoft.com/library/windows/apps/br243533)-Objekt dargestellt wird, in Abhängigkeit vom Feedstandard (RSS oder Atom), der für die Veröffentlichung des Feeds genutzt wird. Im Gegensatz zu einem RSS-Feed kann ein Atom-Feed beispielsweise eine Liste mit [**Contributors**](https://msdn.microsoft.com/library/windows/apps/br243540)-Elementen bereitstellen. Auf Erweiterungselemente in einem Feedelement, die von keinem der Standards unterstützt werden (etwa Dublin Core-Erweiterungselemente), kann jedoch mithilfe der [**SyndicationItem.ElementExtensions**](https://msdn.microsoft.com/library/windows/apps/br243543)-Eigenschaft zugegriffen werden, um sie anschließend wie im folgenden Beispielcode dargestellt anzuzeigen:

> [!div class="tabbedCodeSnippets"]
```csharp
    //displayCurrentItem continued.
    string extensions = "";

    foreach (Windows.Web.Syndication.SyndicationNode node in item.ElementExtensions)
    {
        string nodeName = node.NodeName;
        string nodeNamespace = node.NodeNamespace;
        string nodeValue = node.NodeValue;

        extensions += nodeName + "\n" + nodeNamespace + "\n" + nodeValue + "\n";
    }

    this.listView.Items.Add(itemTitle + "\n" + itemLink + "\n" + itemContent + "\n" + extensions);
}
```
```javascript
    // displayCurrentItem function continued.
    var bindableNodes = [];
    for (var i = 0; i < item.elementExtensions.size; i++) {
        var bindableNode = {
            nodeName: item.elementExtensions[i].nodeName,
             nodeNamespace: item.elementExtensions[i].nodeNamespace,
             nodeValue: item.elementExtensions[i].nodeValue,
        };
        bindableNodes.push(bindableNode);
    }

    var dataList = new WinJS.Binding.List(bindableNodes);
    var listView = document.getElementById("extensionsListView").winControl;
    WinJS.UI.setOptions(listView, {
        itemDataSource: dataList.dataSource

    });
}
```




<!--HONumber=Aug16_HO3-->


