---
author: seksenov
title: "Gehostete Web Apps – Zugriff auf Features der Universellen Windows-Plattform (UWP) und Runtime-APIs"
description: "Greifen Sie auf systemeigene Features der Universellen Windows-Plattform (UWP) sowie Windows10-Runtime-APIs zu, einschließlich Cortana-Sprachbefehlen, Live-Kacheln, ACURs für die Sicherheit, OpenID und OAuth – alles über Remote-JavaScript."
kw: Hosted Web Apps, Accessing Windows 10 features from remote JavaScript, Building a Win10 Web Application, Windows JavaScript Apps, Microsoft Web Apps, HTML5 app for PC, ACUR URI Rules for Windows App, Call Live Tiles with web app, Use Cortana with web app, Access Cortana from website, msapplication-cortanavcd
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: a7f7dccb9c7461e482bd43c8f370a2a7244eb735

---

# Zugriff auf Features der Universellen Windows-Plattform (UWP)

Ihre Webanwendung kann über vollständigen Zugriff auf die Universelle Windows Plattform (UWP) verfügen. So kann sie auf Windows-Geräten systemeigene Features aktivieren, [von der Windows-Sicherheit profitieren](#keep-your-app-secure-setting-application-content-uri-rules-acurs), [Windows-Runtime-APIs](#call-windows-runtime-apis) direkt über ein auf einem Server gehostetes Skript aufrufen, die [Cortana-Integration](#integrate-cortana-voice-commands) nutzen und einen [Onlineauthentifizierungsanbieter](#web-authentication-broker) verwenden. [Hybrid-Apps](#create-hybrid-apps-packaged-web-apps-vs-hosted-web-apps) werden ebenfalls unterstützt, da Sie lokalen Code einfügen können, der aus dem gehosteten Skript aufgerufen werden soll, und die App-Navigation zwischen Remote- und lokalen Seiten verwalten können.

## Schützen Sie Ihre App durch das Einrichten von Regeln für den Anwendungsinhalt-URI (Application Content URI Rules, ACURs)

Durch ACURs, auch als URL-Zulassungsliste bekannt, können Sie Remote-URLs über Remote-HTML, -CSS und -JavaScript direkten Zugriff auf universelle Windows-APIs gewähren. Auf der Ebene des Windows-Betriebssystems wurden entsprechende Richtliniengrenzen festgelegt, damit auf dem Webserver gehosteter Code Plattform-APIs direkt aufrufen kann. Sie definieren diese Grenzen im Manifest des App-Pakets, wenn Sie den Satz von URLs platzieren, aus dem Ihre gehostete Web-App in den Regeln für den Anwendungsinhalt-URI (Application Content URI Rules, ACURs) besteht. Ihre Regeln müssen die Startseite Ihrer App und sämtliche weiteren Seiten enthalten, die als App-Seiten enthalten sein sollen. Optional können Sie auch bestimmte URLs ausschließen.

Es gibt verschiedene Möglichkeiten, eine URL-Übereinstimmung in Ihren Regeln anzugeben:

- Ein exakter Hostname
- Ein Hostname, für den ein URI mit einer Unterdomäne dieses Hostnamens einbezogen oder ausgeschlossen wird
- Ein exakter URI
- Ein exakter URI, der eine Abfrageeigenschaft enthalten kann
- Ein Teilpfad und ein Platzhalter für die Angabe einer bestimmten Dateierweiterung für eine Einbeziehungsregel
- Relative Pfade für Ausschlussregeln

Wenn der Benutzer zu einer URL navigiert, die nicht in Ihren Regeln enthalten ist, öffnet Windows die Ziel-URL in einem Browser.

Hier finden Sie einige Beispiele für ACURs:

```HTML
<Application
Id="App"
StartPage="http://contoso.com/home">
<uap:ApplicationContentUriRules>
    <uap:Rule Type="include" Match="https://contoso.com/" WindowsRuntimeAccess="all" />
    <uap:Rule Type="include" Match="https://*.contoso.com/" WindowsRuntimeAccess="all" />
    <uap:Rule Type="exclude" Match="https://contoso.com/excludethispage.aspx" />
</uap:ApplicationContentUriRules>
```

## Windows-Runtime-APIs aufrufen

Wenn eine URL innerhalb der App-Grenzen (ACURs) definiert ist, kann sie mithilfe des Attributs „WindowsRuntimeAccess“ Windows-Runtime-APIs mit JavaScript aufrufen. Der Windows-Namespace wird eingefügt und ist im Skriptmodul vorhanden, wenn eine URL mit dem entsprechenden Zugriff im App-Host geladen wird. Dadurch werden universelle Windows-APIs für den direkten Aufruf durch die App-Skripts verfügbar. Als Entwickler müssen Sie nur die Entdeckung der Windows-API ermöglichen, die aufgerufen werden soll, und Plattformfeatures hervorheben, wenn verfügbar.

Geben Sie dazu das Attribut `(WindowsRuntimeAccess="<<level>>")` in den ACURs mit einem dieser Werte an:

- **all**: Remote-JavaScript-Code hat Zugriff auf alle UWP-APIs und alle lokal verpackten Komponenten.
- **allowForWeb**: Remote-JavaScript-Cpde hat nur Zugriff auf benutzerdefinierte Komponenten im Paketcode. Lokaler Zugriff auf benutzerdefinierte C++/C#-Komponenten.
- **none**: Standard. Die angegebene URL hat keinen Plattformzugriff.

Dies ist ein Beispiel für einen Regeltyp:

```HTML
<uap:ApplicationContentUriRules>
    <uap:Rule Type="include" Match="http://contoso.com/" WindowsRuntimeAccess="all"  />
</uap:ApplicationContentUriRules>
```

Dieser ermöglicht dem Skript, das unter http://contoso.com/ ausgeführt wird, Zugriff auf Windows-Runtime-Namespaces und benutzerdefinierte verpackte Komponenten im Paket. Popupbenachrichtigungen finden Sie im Beispiel [Windows.UI.Notifications.js](https://gist.github.com/Gr8Gatsby/3d471150e5b317eb1813#file-windows-ui-notifications-js) auf GitHub.

Dies ist ein Beispiel für das Implementieren einer Live-Kachel und ihre Aktualisierung über Remote-JavaScript:

```Javascript
function updateTile(message, imgUrl, imgAlt) {
    // Namespace: Windows.UI.Notifications

    if (typeof Windows !== 'undefined'&&
            typeof Windows.UI !== 'undefined' &&
            typeof Windows.UI.Notifications !== 'undefined') {  
        var notifications = Windows.UI.Notifications,
        tile = notifications.TileTemplateType.tileSquare150x150PeekImageAndText01,
        tileContent = notifications.TileUpdateManager.getTemplateContent(tile),
        tileText = tileContent.getElementsByTagName('text'),
        tileImage = tileContent.getElementsByTagName('image');  
        tileText[0].appendChild(tileContent.createTextNode(message || 'Demo Message'));
        tileImage[0].setAttribute('src', imgUrl || 'https://unsplash.it/150/150/?random');
        tileImage[0].setAttribute('alt', imgAlt || 'Random demo image');    
        var tileNotification = new notifications.TileNotification(tileContent);
        var currentTime = new Date();
        tileNotification.expirationTime = new Date(currentTime.getTime() + 600 * 1000);
        notifications.TileUpdateManager.createTileUpdaterForApplication().update(tileNotification);
    } else {
        //Alternative behavior

    }
}
```

Dieser Code erzeugt eine Kachel, die ungefähr folgendermaßen aussieht:

![Aufrufen einer Live-Kachel durch Windows10](images/hwa-to-uwp/hwa_livetile.png)

Rufen Sie Windows-Runtime APIs mit der Umgebung oder Technik auf, mit der Sie am meisten vertraut sind, indem Sie Ihre Ressourcen in ein Serverfeature einfügen, das nach Windows-Funktionen sucht, bevor diese aufgerufen werden. Wenn keine Plattformfunktionen verfügbar sind, und die Web-App in einem anderen Host ausgeführt wird, können Sie dem Benutzer eine Standardumgebung bereitstellen, die im Browser funktioniert.

## Integrieren von Cortana-Sprachbefehlen

Sie können die Cortana-Integration nutzen und eine Sprachbefehlsdefinitionsdatei (Voice Command Definition-Datei, VCD-Datei) auf Ihrer HTML-Seite angeben. Die VCD-Datei ist eine XML-Datei, die bestimmten Formulierungen Befehle zuordnet. So kann beispielsweise ein Benutzer auf die Starttaste tippen und „Contoso Books, Bestseller anzeigen“ sagen, um die ContosoBooks-App zu starten und zur Seite mit den Bestsellern zu navigieren.

Beim Hinzufügen eines `<meta>`-Elementtags, das den Speicherort der VCD-Datei aufführt, lädt Windows die Datei mit Sprachbefehlsdefinitionen automatisch herunter und registriert sie.

Dies ist ein Beispiel für die Verwendung des Meta-Tags auf einer HTML-Seite in einer gehosteten Web-App:

```HTML
<meta name="msapplication-cortanavcd" content="http:// contoso.com/vcd.xml"/>
```

Weitere Informationen zur Integration von Cortana und VCDs finden Sie unter „Cortana-Interaktionen und Voice Command Definition (VCD) Elements and Attributes v1.2“.

## Erstellen Sie Hybrid-Apps – verpackte Web-Apps statt gehosteter Web-Apps

Sie haben verschiedene Optionen zum Erstellen Ihrer UWP-App. Die App kann so entworfen sein, dass sie aus dem Windows Store heruntergeladen und vollständig auf dem lokalen Client gehostet wird, häufig als **verpackte Web-App** bezeichnet. Auf diese Weise können Sie Ihre App offline auf jeder kompatiblen Plattform ausführen. Die App kann auch eine vollständig gehostete Web-App sein, die auf einem Remotewebserver ausgeführt wird, in der Regel als **gehostete Web-App** bezeichnet. Es gibt jedoch noch eine dritte Option: Die App kann teilweise auf dem lokalen Client und teilweise auf einem Remotewebserver gehostet werden. Diese dritte Option wird als **Hybrid-App** bezeichnet. In der Regel verwendet sie die **WebView**-Komponente, um Remoteinhalte wie lokale Inhalte erscheinen zu lassen. Hybrid-Apps können Ihren HTML5-, CSS- und JavaScript-Code enthalten, der als Paket im lokalen App-Client ausgeführt wird, um dafür zu sorgen, dass die Interaktion mit Remoteinhalten möglich ist.

## Webauthentifizierungsbroker

Sie können den Webauthentifizierungsbroker für die Verarbeitung des Anmeldungsablaufs Ihrer Benutzer verwenden, wenn Sie über einen Onlineidentitätsanbieter verfügen, der Internetauthentifizierung und Autorisierungsprotokolle wie OpenID oder OAuth verwendet. Sie geben den Start- und End-URI in einem `<meta>`-Tag auf einer HTML-Seite in Ihrer App an. Windows erkennt diese URIs und leitet sie an den Webauthentifizierungsbroker weiter, um den Ablauf bei der Anmeldung abzuschließen. Der Start-URI besteht aus der Adresse, unter der Sie die Authentifizierungsanforderung an den Onlineanbieter senden, und angefügten anderen benötigten Informationen, z.B. eine App-ID, ein Umleitungs-URI, zu dem der Benutzer nach Abschluss der Authentifizierung umgeleitet wird, und der erwartete Antworttyp. Informationen zu den erforderlichen Parametern erhalten Sie von Ihrem Anbieter. Dies ist ein Beispiel für die Verwendung des `<meta>`-Tags:

```HTML
<meta name="ms-webauth-uris" content="https://<providerstartpoint>?client_id=<clientid>&response_type=token, https://<appendpoint>"/>
```

Weitere Informationen finden Sie unter [Überlegungen zum Webauthentifizierungsbroker für Onlineanbieter](https://msdn.microsoft.com/library/windows/apps/dn448956.aspx).

## Deklarationen der App-Funktionen

Wenn Ihre App programmgesteuerten Zugriff auf Benutzerressourcen wie Bilder oder Musik oder auf Geräte wie eine Kamera oder ein Mikrofon benötigt, müssen Sie die entsprechende Funktion deklarieren. Es gibt drei Kategorien von App Funktionsdeklarationen: 

- [Funktionen zur allgemeinen Verwendung](https://msdn.microsoft.com/library/windows/apps/Mt270968.aspx#General-use_capabilities), die auf die meisten allgemeinen App-Szenarien zutreffen. 
- [Gerätefunktionen](https://msdn.microsoft.com/library/windows/apps/Mt270968.aspx#Device_capabilities), die Ihrer App den Zugriff auf Peripheriegeräte und interne Geräte ermöglichen. 
- [Sonderfunktionen](https://msdn.microsoft.com/library/windows/apps/Mt270968.aspx#Special_and_restricted_capabilities) die ein spezielles Unternehmenskonto für die Einreichung beim Store erfordern. 

Weitere Informationen zu Unternehmenskonten finden Sie unter [Kontotypen, Standorte und Gebühren](https://msdn.microsoft.com/library/windows/apps/jj863494.aspx).

> [!NOTE]
> Es ist wichtig, zu wissen, dass Kunden über alle von der App deklarierten Funktionen informiert werden, wenn sie Ihre App über den Windows Store beziehen. Verwenden Sie daher keine Funktionen, die Ihre App nicht benötigt.

Zum Anfordern des Zugriffs werden Funktionen im [Paketmanifest](https://msdn.microsoft.com/library/windows/apps/br211474.aspx) Ihrer App deklariert. Allgemeine Funktionen können mithilfe des [Manifest-Designers](https://msdn.microsoft.com/library/windows/apps/xaml/hh454036(v=vs.140).aspx#Configure) in Microsoft Visual Studio deklariert werden. Alternativ können Sie diese manuell hinzufügen. Weitere Informationen finden Sie unter [Angeben von Funktionen in einem Paketmanifest](https://msdn.microsoft.com/library/windows/apps/br211477.aspx).

Einige Funktionen bieten Apps Zugriff auf eine sicherheitssensible Ressource. Diese Ressourcen gelten als sicherheitssensibel, da sie auf persönliche Daten des Benutzers zugreifen oder ihm Kosten verursachen können. Mit Datenschutzeinstellungen, die von der Einstellungs-App verwaltet werden, können Benutzer den Zugriff auf sicherheitssensible Ressourcen dynamisch steuern. Daher ist es wichtig, dass Ihre App nicht davon ausgeht, dass eine sicherheitssensible Ressource stets verfügbar ist. Weitere Informationen zum Zugriff auf sicherheitssensible Ressourcen finden Sie unter [Richtlinien für Apps mit Berücksichtigung von Datenschutz](https://msdn.microsoft.com/library/windows/apps/hh768223.aspx).

## manifoldjs und das App-Manifest

Eine einfache Möglichkeit, Ihre Website in eine UWP-App zu verwandeln, ist die Verwendung eines **App-Manifests** und von **manifoldjs**. Das App-Manifest ist eine XML-Datei, die Metadaten zur App enthält. In dieser Datei werden Dinge wie der App-Name, Links zu Ressourcen, der Anzeigemodus, URLs und andere Daten angegeben, mit denen beschrieben wird, wie die Bereitstellung und Ausführung der App erfolgen soll. Mit manifoldjs wird dies sehr einfach. Dies gilt auch für Systeme, die keine Web-Apps unterstützen. Weitere Informationen zur Funktionsweise finden Sie unter [manifoldjs.com](http://www.manifoldjs.com/). Sie können sich im Rahmen dieser [Präsentation zu Windows10-Web-Apps](http://channel9.msdn.com/Events/WebPlatformSummit/2015/Hosted-web-apps-and-web-platform-innovations?wt.mc_id=relatedsession) auch eine Demonstration von manifoldjs ansehen.

## Verwandte Themen
- [Windows-Runtime-API: JavaScript-Code-Beispiele](http://rjs.azurewebsites.net/)
- [Codepen: Sandkasten für den Aufruf der Windows-Runtime-APIs](http://codepen.io/seksenov/pen/wBbVyb/)
- [Cortana-Interaktionen](https://msdn.microsoft.com/library/windows/apps/dn974231.aspx)
- [Voice Command Definition (VCD) Elements and Attributes v1.2](https://msdn.microsoft.com/library/windows/apps/dn954977.aspx)
- [Überlegungen zum Webauthentifizierungsbroker für Onlineanbieter](https://msdn.microsoft.com/library/windows/apps/dn448956.aspx)
- [Deklarationen der App-Funktionen](https://msdn.microsoft.com/ibrary/windows/apps/hh464936.aspx)


<!--HONumber=Jul16_HO2-->


