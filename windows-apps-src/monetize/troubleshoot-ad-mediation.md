---
Description: Im Anschluss finden Sie einige Lösungen für verschiedene allgemeine Entwicklungsprobleme im Zusammenhang mit der Anzeigenvermittlung.
title: Problembehandlung bei der Anzeigenvermittlung
ms.assetid: 8728DE4F-E050-4217-93D3-588DD3280A3A
---

# Problembehandlung bei der Anzeigenvermittlung


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Im Anschluss finden Sie einige Lösungen für verschiedene allgemeine Entwicklungsprobleme im Zusammenhang mit der Anzeigenvermittlung.

**Der Entwurfsoberfläche kann kein AdMediatorControl-Element hinzugefügt werden.**  
Beim ersten Ziehen des **AdMediatorControl**-Steuerelements in den Designer in einem Projekt (für die universelle Windows-Plattform [UWP], Windows 8.1 oder Windows Phone 8.1) mit C# oder Visual Basic und XAML fügt Visual Studio Ihrem Projekt den erforderlichen Ad Mediator-Assemblyverweis hinzu, das Steuerelement wird dem Designer jedoch noch nicht hinzugefügt. Klicken Sie zum Hinzufügen des Steuerelements in der von Visual Studio angezeigten Meldung auf „OK“, warten dann einige Sekunden auf die Aktualisierung des Designers und ziehen das Steuerelement anschließend erneut in den Designer.

Wenn Sie dem Designer das Steuerelement weiterhin nicht hinzufügen können, stellen Sie sicher, dass Ihr Projekt die geeignete Prozessorarchitektur für Ihre App aufweist (z. B. **X 86**) statt einer **beliebigen CPU**. Das Steuerelement kann dem Designer nicht hinzugefügt werden, wenn das Projekt für die Buildplattform auf eine beliebige CPU**** ausgerichtet ist.

**Das AdMediatorControl-Element zeigt zur Laufzeit die Fehlermeldung „“&lt;*Breite*&gt; x &lt;*Höhe*&gt; nicht unterstützt“ an, wenn Anzeigen von Microsoft** angezeigt werden.  
Microsoft Advertising unterstützt nur [Anzeigen in bestimmten Größen, die vom Interactive Advertising Bureau (IAB) empfohlen wurden](add-and-use-the-ad-mediator-control.md#supported-ad-sizes-for-microsoft-advertising). In einigen Fällen verhindern Skalierungs- und Abrundungsprobleme das Einblenden einer Anzeige durch das Anzeigenvermittlungs-Framework u. U. auch dann, wenn Sie die Höhe und Breite des Ad Mediator-Steuerelements im Designer oder in Ihrer XAML auf eine dieser unterstützten Größen festgelegt haben. Um dieses Problem zu vermeiden, weisen Sie in Ihrem Code einer der unterstützten Anzeigengrößen die optionalen Parameter **Width** und **Height** für Microsoft Advertising zu.

Das folgende Codebeispiel zeigt, wie Sie die optionalen Parameter **Width** und **Height** für Microsoft Advertising auf 728 x 90 festlegen.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Width"] = 728;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Height"] = 90;
```

**Die Anzeigenvermittlung umfasst keine Standortangaben (Breiten-/Längengrad) für das Anzeigennetzwerk.**  
Wenn Sie die Standortfunktion in Ihrer App aktivieren, ruft das Ad Mediator-Steuerelement automatisch die Koordinaten für Breiten-/Längengrad ab und stellt diese für die Anzeigennetzwerke bereit, von denen sie unterstützt werden.

**Das Smaato-Anzeigensteuerelement wird nicht ordnungsgemäß ausgerichtet.**  
Verwenden Sie die optionalen Parameter zum Festlegen von Werten für die SDK-Steuerelemente:

```
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato][“Margin”] = new Thickness(0, -20, 0, 0);
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato][“Width”] = 50d;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato][“Height”] = 320d;
```

**Das AdDuplex-Anzeigensteuerelement wird nicht im richtigen Format angezeigt (sondern im Format 250 x 250).**  
Die Anzeigenvermittlung legt keinen Wert für die Größe fest, daher können Sie es mithilfe des optionalen Größenparameters ändern. Beispiel:

```
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.AdDuplex][“Size”] = “160×600″;
```

**Sie erhalten die Fehlermeldung, dass das Anzeigensteuerelement verdeckt ist.**  
Ad Duplex zeigt immer einem Fehler an, wenn die Anzeige in Ihrer App in irgendeiner Weise verdeckt ist. [Lesen Sie die entsprechende Lösung](http://blog.adduplex.com/2014/01/solving-something-is-covering-ad.mdl) zu diesem Fehler.

**Sie erhalten die Fehlermeldung, dass zwischen zwei Dateien ein Konflikt besteht.**  
Sie haben in Ihrer App an einer anderen Stelle auf die Microsoft Advertising-Assemblys verwiesen. Die Anzeigenvermittlung ist ausschließlich für Ihre App konzipiert und funktioniert nicht, wenn andere Verweise auf die Microsoft Advertising-Assemblys verwendet werden. Entfernen Sie die Verweise auf Microsoft Advertising manuell, und installieren Sie das Microsoft Store Engagement and Monetization SDK neu, um den Fehler zu beheben.

## Verwandte Themen

* [Auswählen und Verwalten der Anzeigennetzwerke](select-and-manage-your-ad-networks.md)
* [Hinzufügen und Verwenden des Steuerelements für die Anzeigenvermittlung](add-and-use-the-ad-mediator-control.md)
* [Testen der Implementierung der Anzeigenvermittlung](test-your-ad-mediation-implementation.md)
* [Übermitteln der App und Konfigurieren der Anzeigenvermittlung](submit-your-app-and-configure-ad-mediation.md)
 

 


<!--HONumber=Mar16_HO5-->


