---
author: mcleanbyron
ms.assetid: 48a1ef86-8514-4af8-9c93-81e869d36de7
description: Hier erfahren Sie, wie Sie programmgesteuert ein **AdControl** (Anzeigensteuerelement) mit JavaScript erstellen.
title: Erstellen eines Anzeigensteuerelements in JavaScript

---

# Erstellen eines Anzeigensteuerelements in JavaScript


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Dieses Beispiel zeigt, wie Sie programmgesteuert ein [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)-Element mit JavaScript erstellen.

## HTML-div-Element für AdControl

Das **AdControl** benötigt ein **div**-Element auf der HTML-Seite, auf der die Werbung angezeigt wird. Der folgende Code ist ein Beispiel für so ein **div**-Element.

``` syntax
<div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
    data-win-control="MicrosoftNSJS.Advertising.AdControl">
</div>
```

## JavaScript zum Erstellen eines AdControl

Im folgenden Beispiel wird davon ausgegangen, dass Sie in Ihrem HTML-Code ein vorhandenes **div**-Element mit der ID **MyAd** verwenden.

Instanziieren Sie das **AdControl** in der **app.onactivated**-Funktion.

``` syntax
// TODO: This application has been newly launched. Initialize
// your application here.
var adDiv = document.getElementById("myAd");
var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
        applicationId: "3f83fe91-d6be-434d-a0ae-7351c5a997f1",
        adUnitId: "10865270"
    });
myAdControl.isAutoRefreshEnabled = false;
myAdControl.onErrorOccurred = myAdError;
myAdControl.onAdRefreshed = myAdRefreshed;
myAdControl.onEngagedChanged = myAdEngagedChanged;
```

Die Werte dienen als Beispiele. In Ihrem Code legen Sie die Werte dieser Funktionen und Eigenschaften entsprechend für Ihre App fest.

Wenn Sie diesen Code verwenden und keine Anzeigen angezeigt werden, können Sie versuchen, ein **position:relativ**-Attribut im **div**-Element einzufügen, das das **AdControl** enthält. Dadurch wird die Standardeinstellung von **IFrame** überschrieben. Anzeigen werden ordnungsgemäß angezeigt, sofern sie nicht aufgrund des Werts dieses Attributs nicht angezeigt werden. Beachten Sie, dass neue Anzeigeeinheiten unter Umständen bis zu 30 Minuten nicht verfügbar sind.

## Verwandte Themen

* [Anzeigenbeispiele bei GitHub](http://aka.ms/githubads)

 

 


<!--HONumber=May16_HO2-->


