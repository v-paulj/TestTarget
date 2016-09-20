---
author: mcleanbyron
Description: "Im Anschluss finden Sie einige Lösungen für verschiedene allgemeine Entwicklungsprobleme im Zusammenhang mit der Anzeigenvermittlung."
title: Problembehandlung bei der Anzeigenvermittlung
ms.assetid: 8728DE4F-E050-4217-93D3-588DD3280A3A
ms.sourcegitcommit: 10dcf3c2b8ea530b94e9c17ada80aaa98e9418fe
ms.openlocfilehash: f32dc28c9b199c11a1932639f49ab4c29d3e1e8f

---

# Problembehandlung bei der Anzeigenvermittlung


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Im Anschluss finden Sie einige Lösungen für verschiedene allgemeine Entwicklungsprobleme im Zusammenhang mit der Anzeigenvermittlung.

**Der Entwurfsoberfläche kann kein AdMediatorControl-Element hinzugefügt werden.**  
Beim ersten Ziehen des **AdMediatorControl**-Steuerelements in den Designer in einem Projekt (für die universelle Windows-Plattform [UWP], Windows8.1 oder WindowsPhone8.1) mit C# oder VisualBasic und XAML fügt VisualStudio Ihrem Projekt den erforderlichen AdMediator-Assemblyverweis hinzu, das Steuerelement wird dem Designer jedoch noch nicht hinzugefügt. Klicken Sie zum Hinzufügen des Steuerelements in der von VisualStudio angezeigten Meldung auf „OK“, warten dann einige Sekunden auf die Aktualisierung des Designers und ziehen das Steuerelement anschließend erneut in den Designer.

Wenn Sie dem Designer das Steuerelement weiterhin nicht hinzufügen können, stellen Sie sicher, dass Ihr Projekt die geeignete Prozessorarchitektur für Ihre App aufweist (z.B. **X 86**) statt einer **beliebigen CPU**. Das Steuerelement kann dem Designer nicht hinzugefügt werden, wenn das Projekt für die Buildplattform auf eine **beliebige CPU** ausgerichtet ist.


            *
              *Das AdMediatorControl-Element zeigt zur Laufzeit die Fehlermeldung „&lt;*width*
            &gt; x &lt;*height*&gt; nicht unterstützt“ an, wenn Anzeigen von Microsoft angezeigt werden.** Microsoft Advertising unterstützt nur [Anzeigen in bestimmten Größen, die vom Interactive Advertising Bureau (IAB) empfohlen wurden](add-and-use-the-ad-mediator-control.md#supported-ad-sizes-for-microsoft-advertising). In einigen Fällen verhindern Skalierungs- und Abrundungsprobleme das Einblenden einer Anzeige durch das Anzeigenvermittlungs-Framework u.U. auch dann, wenn Sie die Höhe und Breite des Ad Mediator-Steuerelements im Designer oder in Ihrer XAML auf eine dieser unterstützten Größen festgelegt haben. Um dieses Problem zu vermeiden, weisen Sie in Ihrem Code einer der unterstützten Anzeigengrößen die optionalen Parameter **Width** und **Height** für Microsoft Advertising zu.

Das folgende Codebeispiel zeigt, wie Sie die optionalen Parameter **Width** und **Height** für MicrosoftAdvertising auf 728x90 festlegen.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Width"] = 728;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Height"] = 90;
```

**Die Anzeigenvermittlung umfasst keine Standortangaben (Breiten-/Längengrad) für das Anzeigennetzwerk.**  
Wenn Sie die Standortfunktion in Ihrer App aktivieren, ruft das Ad Mediator-Steuerelement automatisch die Koordinaten für Breiten-/Längengrad ab und stellt diese für die Anzeigennetzwerke bereit, von denen sie unterstützt werden.

**Das Smaato-Anzeigensteuerelement wird nicht ordnungsgemäß ausgerichtet.**  
Verwenden Sie die optionalen Parameter zum Festlegen von Werten für die SDK-Steuerelemente:

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Margin"] = new Thickness(0, -20, 0, 0);
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 50d;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 320d;
```

**Das AdDuplex-Anzeigensteuerelement wird nicht im richtigen Format angezeigt (sondern im Format 250 x 250).**  
Die Anzeigenvermittlung legt keinen Wert für die Größe fest, daher können Sie es mithilfe des optionalen **Größen**parameters ändern. Beispiel:

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.AdDuplex]["Size"] = "160x600";
```

**Sie erhalten die Fehlermeldung, dass das Anzeigensteuerelement verdeckt ist.**  
Ad Duplex zeigt immer einem Fehler an, wenn die Anzeige in Ihrer App in irgendeiner Weise verdeckt ist. 
            [Lesen Sie die entsprechende Lösung](http://blog.adduplex.com/2014/01/solving-something-is-covering-ad.mdl) zu diesem Fehler.

**Sie erhalten die Fehlermeldung, dass zwischen zwei Dateien ein Konflikt besteht.**  
Sie haben in Ihrer App an einer anderen Stelle auf die MicrosoftAdvertising-Assemblys verwiesen. Die Anzeigenvermittlung ist ausschließlich für Ihre App konzipiert und funktioniert nicht, wenn andere Verweise auf die MicrosoftAdvertising-Assemblys verwendet werden. Entfernen Sie die Verweise auf MicrosoftAdvertising manuell, und installieren Sie das Microsoft Store Engagement and Monetization SDK neu, um den Fehler zu beheben.

**Sie stoßen auf unerwartetes Verhalten, nachdem Sie den Wert für die Aktualisierungsrate in der Datei AdMediator.config geändert haben**

Nachdem Sie die Anzeigennetzwerke durch Ausführen der **Ad Mediator**-Komponente im Dialogfeld **Verbundenen Dienst hinzufügen** in Visual Studio konfiguriert haben, werden die standardmäßigen Konfigurationsinformationen in der Datei AdMediator.config in Ihrem Projekt gespeichert. Es wird von Ihnen nicht erwartet, diese Datei direkt zu ändern. Stattdessen können Sie diese Informationen (einschließlich der Aktualisierungsrate für neue Anzeigen) ändern, wenn Sie [die Einstellungen der Anzeigenvermittlung für Ihre App konfigurieren,](submit-your-app-and-configure-ad-mediation.md) nachdem Sie das App-Paket in das Windows Dev Center-Dashboard hochgeladen haben.

Wenn Sie den Wert für die **"Aktualisierungsrate"** in der Datei AdMediator.config ändern, beachten Sie, dass dieser Wert eine ganze Zahl zwischen 30 und 120 enthalten muss, die die Aktualisierungsrate in Sekunden darstellt. Wenn Sie für diesen Wert eine ganze Zahl kleiner als 30 oder größer als 120 festlegen, verwendet das Anzeigenvermittlungs-Framework automatisch eine Aktualisierungsrate von 60 Sekunden.

## Verwandte Themen

* [Auswählen und Verwalten der Anzeigennetzwerke](select-and-manage-your-ad-networks.md)
* [Hinzufügen und Verwenden des Steuerelements für die Anzeigenvermittlung](add-and-use-the-ad-mediator-control.md)
* [Testen der Implementierung der Anzeigenvermittlung](test-your-ad-mediation-implementation.md)
* [Übermitteln der App und Konfigurieren der Anzeigenvermittlung](submit-your-app-and-configure-ad-mediation.md)
 

 



<!--HONumber=Jun16_HO4-->


