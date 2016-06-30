---
author: Xansky
Description: "In diesem Thema werden die bewährten Methoden für barrierefreien Text in Apps beschrieben. Damit stellen Sie sicher, dass der Kontrast zwischen Farben und Hintergründen ausreichend hoch ist."
ms.assetid: BA689C76-FE68-4B5B-9E8D-1E7697F737E6
title: "Anforderungen für barrierefreien Text"
label: Accessible text requirements
template: detail.hbs
ms.sourcegitcommit: 50c37d71d3455fc2417d70f04e08a9daff2e881e
ms.openlocfilehash: 1307b4f70cf7ffed300f4254a7d92b67b5afd085

---

# Anforderungen für barrierefreien Text  




In diesem Thema werden die bewährten Methoden für barrierefreien Text in Apps beschrieben. Damit stellen Sie sicher, dass der Kontrast zwischen Farben und Hintergründen ausreichend hoch ist. Außerdem werden in diesem Thema die Rollen der Microsoft-Benutzeroberflächenautomatisierung, die für Textelemente in einer App der universellen Windows-Plattform (UWP) verwendet werden können, sowie bewährte Methoden für Text in Grafiken erläutert.

<span id="contrast_rations"/>
<span id="CONTRAST_RATIONS"/>
## Kontrastverhältnisse  
Obwohl Benutzer immer die Möglichkeit haben, die Anzeige auf einen Modus mit hohem Kontrast umzuschalten, sollten Sie diese Option in Ihrem App-Design für Text möglichst nicht verwenden. Stellen Sie stattdessen sicher, dass der App-Text bestimmte Richtlinien für die Kontraststufe zwischen Text und seinem Hintergrund erfüllt. Die Bewertung der Kontraststufe basiert auf deterministischen Techniken, bei denen der Farbton nicht berücksichtigt wird. Wenn Sie z. B. roten Text auf grünem Hintergrund haben, ist dieser Text für einen farbenblinden Benutzer möglicherweise nicht lesbar. Durch eine Überprüfung und Korrektur des Kontrastverhältnisses können Sie solche Probleme bezüglich der Barrierefreiheit vermeiden.

Die hier dokumentierten Empfehlungen für den Textkontrast basieren auf einem Standard für Barrierefreiheit im Internet, [G18: Sicherstellen eines Kontrastverhältnisses von mindestens 4,5:1 zwischen Text (und Textbildern) und dem Texthintergrund](http://go.microsoft.com/fwlink/p/?linkid=221823). Dieser Leitfaden ist in der *W3C-Spezifikation für die Techniken für WCAG 2.0* enthalten.

Damit sichtbarer Text als barrierefreier Text gilt, muss er ein Leuchtdichte-Kontrastverhältnis von mindestens 4,5:1 bezogen auf den Hintergrund aufweisen. Ausnahmen sind Logos und nebensächlicher Text (z. B. Text, der Teil einer inaktiven Benutzeroberflächenkomponente ist).

Für dekorativen Text, der keine Informationen liefert, gilt diese Anforderung nicht. Wenn beispielsweise zufällige Wörter zum Erstellen eines Hintergrunds verwendet werden und die Wörter ohne Bedeutungsänderung neu angeordnet oder ersetzt werden können, gelten die Wörter als dekorative Elemente und müssen dieses Kriterium nicht erfüllen.

Überprüfen Sie mit den Farbkontrasttools, ob das Kontrastverhältnis von sichtbarem Text in Ordnung ist. Tools zum Testen des Kontrastverhältnisses finden Sie unter [Techniken für WCAG 2.0 G18 (Abschnitt Ressourcen)](http://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources).

> [!NOTE]
> Einige der unter den Techniken für WCAG 2.0 G18 aufgeführten Tools können bei einer UWP-App nicht interaktiv verwendet werden. Sie müssen möglicherweise die Werte für Vordergrund- und Hintergrundfarben manuell in das Tool eingeben oder Bildschirmaufnahmen der App-UI erstellen und anschließend das Kontrastverhältnistool für das aufgenommene Bild ausführen.

<span id="Text_element_roles"/>
<span id="text_element_roles"/>
<span id="TEXT_ELEMENT_ROLES"/>
## Textelementrollen  
In einer UWP-App können die folgenden Standardelemente (meist als *Textelemente* oder *textedit-Steuerelemente* bezeichnet) verwendet werden:

* [
              **TextBlock**
            ](https://msdn.microsoft.com/library/windows/apps/BR209652): Rolle ist [**Text**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [
              **TextBox**
            ](https://msdn.microsoft.com/library/windows/apps/BR209683): Rolle ist [**Edit**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [
              **RichTextBlock**
            ](https://msdn.microsoft.com/library/windows/apps/BR227565) (und Überlaufklasse [**RichTextBlockOverflow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.richtextblockoverflow)): Rolle ist [**Text**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [
              **RichEditBox**
            ](https://msdn.microsoft.com/library/windows/apps/BR227548): Rolle ist [**Edit**](https://msdn.microsoft.com/library/windows/apps/BR209182)

Wenn ein Steuerelement meldet, dass ihm die Rolle [**Edit**](https://msdn.microsoft.com/library/windows/apps/BR209182) zugewiesen ist, gehen Hilfstechnologien davon aus, dass Benutzer die Werte ändern können. Falls Sie also statischen Text in einem [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) platzieren, wird die Rolle falsch gemeldet, wodurch auch die Struktur der App falsch für einen Benutzer gemeldet wird, der Barrierefreiheitsfeatures verwendet.

In den Textmodellen für XAML sind zwei Elemente enthalten, die hauptsächlich für statischen Text, [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)-Elemente und [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565)-Elemente verwendet werden. Bei keinem dieser Elemente handelt es sich um eine [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390)-Unterklasse, sodass sie auch nicht den Tastaturfokus erhalten oder in der Aktivierreihenfolge erscheinen können. Dies bedeutet jedoch nicht, dass sie von Hilfstechnologien nicht gelesen werden bzw. gelesen werden können. Sprachausgaben sind in der Regel für die Unterstützung mehrerer Modi zum Ausgeben der Inhalte einer App ausgelegt und verfügen beispielsweise über einen dedizierten Lesemodus oder Navigationsmuster, die über den Fokus und die Aktivierreihenfolge hinausgehen, z. B. einen „virtuellen Cursor“. Fügen Sie Ihren statischen Text also nicht nur deswegen in Container ein, die den Fokus erhalten können, damit Benutzer über die Aktivierreihenfolge darauf zugreifen können. Benutzer von Hilfstechnologien erwarten, dass über die Aktivierreihenfolge verfügbare Inhalte interaktiv sind. Wenn sie darin auf statischen Text treffen, ist dies eher verwirrend als hilfreich. Testen Sie dies selbst mit der Sprachausgabe, um einen Eindruck der Benutzeroberfläche der App zu gewinnen, wenn die Sprachausgabe zum Untersuchen des statischen Texts einer App verwendet wird.

<span id="Text_in_graphics"/>
<span id="text_in_graphics"/>
<span id="TEXT_IN_GRAPHICS"/>
## Text in Grafiken  
Verwenden Sie nach Möglichkeit keinen Text in Grafiken. Text, den Sie der in der App als [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752)-Element angezeigten Bildquelldatei hinzufügen, ist z. B. nicht automatisch barrierefrei oder für Hilfstechnologien lesbar. Falls Sie Text in Grafiken verwenden müssen, müssen Sie sicherstellen, dass der [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770)-Wert, den Sie als Entsprechung für „alternativen Text“ angeben, diesen Text oder eine Zusammenfassung seiner Bedeutung enthält. Ähnliches gilt auch, wenn Sie Textzeichen anhand von Vektoren als Teil einer [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355)-Klasse oder mit [**Glyphs**](https://msdn.microsoft.com/library/windows/apps/BR209921) erstellen.

<span id="Text_font_size"/>
<span id="text_font_size"/>
<span id="TEXT_FONT_SIZE"/>
## Schriftgrad des Texts  
Viele Benutzer haben Probleme beim Lesen eines Texts in einer App, wenn ein zu kleiner Schriftgrad verwendet wird. Sie können dieses Problem verhindern, indem Sie von vornherein den Text auf der Benutzeroberfläche der App groß genug machen. Unter Windows sind auch Hilfstechnologien verfügbar, mit denen Benutzer die Anzeigegröße von Apps oder die allgemeine Anzeige ändern können.

* Manche Benutzer verwenden die Barrierefreiheitsfunktion zum Ändern der DPI (Dots per Inch)-Werte für die Hauptanzeige. Diese Option steht unter **Erleichterte Bedienung** als **Elementgröße ändern** zur Verfügung. Sie führt in der **Systemsteuerung** zum Punkt **Darstellung und Anpassung**/ / **Anzeige**. Die Optionen für die Größenanpassung können variieren, da sich dies nach den Funktionen des Anzeigegeräts richtet.
* Mit der Bildschirmlupe kann ein ausgewählter Bereich der UI vergrößert werden. Es ist aber nicht einfach, die Bildschirmlupe zum Lesen von Text zu verwenden.

<span id="Text_scale_factor"/>
<span id="text_scale_factor"/>
<span id="TEXT_SCALE_FACTOR"/>
## Textskalierungsfaktor  
Verschiedene Textelemente und Steuerelemente verfügen über eine [**IsTextScaleFactorEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.istextscalefactorenabled)-Eigenschaft. Diese Eigenschaft weist standardmäßig den Wert **true** auf. Wenn sie den Wert **true** besitzt, bewirkt die Einstellung **Textskalierung** auf dem Smartphone (**Einstellungen &gt; Erleichterte Bedienung**) die Skalierung der Textgröße in diesem Element. Die Skalierung wirkt sich auf Text mit kleiner **FontSize** deutlicher aus als auf Text mit großer **FontSize**. Sie können diese automatische Vergrößerung jedoch deaktivieren, indem Sie die **IsTextScaleFactorEnabled**-Eigenschaft eines Elements auf **false** festlegen. Probieren Sie dieses Markup aus. Passen Sie auf dem Smartphone die Einstellung für die **Textgröße** an, und verfolgen Sie, was mit den **TextBlock**-Elementen geschieht:

XAML
```xml
<TextBlock Text="In this case, IsTextScaleFactorEnabled has been left set to its default value of true."
    Style="{StaticResource BodyTextBlockStyle}"/>

<TextBlock Text="In this case, IsTextScaleFactorEnabled has been set to false."
    Style="{StaticResource BodyTextBlockStyle}" IsTextScaleFactorEnabled="False"/>
```  

Es ist jedoch nicht ratsam, die automatische Vergrößerung routinemäßig zu deaktivieren, da das Skalieren von UI-Text für alle Apps eine wichtige Barrierefreiheitsfunktion ist. Benutzer erwarten, dass sie auch in Ihrer App funktioniert.

Sie können auch das [**TextScaleFactorChanged**](https://msdn.microsoft.com/library/windows/apps/Dn633867)-Ereignis und die [**TextScaleFactor**](https://msdn.microsoft.com/library/windows/apps/Dn633866)-Eigenschaft verwenden, um Informationen zu Änderungen der Einstellung **Textgröße** auf dem Smartphone zu erhalten. Gehen Sie wie folgt vor:

C#
```csharp
{
    ...
    var uiSettings = new Windows.UI.ViewManagement.UISettings();
    uiSettings.TextScaleFactorChanged += UISettings_TextScaleFactorChanged;
    ...
}

private async void UISettings_TextScaleFactorChanged(Windows.UI.ViewManagement.UISettings sender, object args)
{
    var messageDialog = new Windows.UI.Popups.MessageDialog(string.Format("It's now {0}", sender.TextScaleFactor), "The text scale factor has changed");
    await messageDialog.ShowAsync();
}
```

Der Wert von **TextScaleFactor** ist ein Double im Bereich \[1,2\]. Der kleinste Text wird um diesen Wert vergrößert. Unter Umständen können Sie den Wert verwenden, um beispielsweise Grafiken passend zum Text zu skalieren. Denken Sie jedoch daran, dass nicht der gesamte Text um denselben Faktor skaliert wird. Allgemein gilt: Je größer der Text, desto geringer sind die Auswirkungen der Skalierung.

Diese Typen verfügen über eine **IsTextScaleFactorEnabled**-Eigenschaft:  
* [**ContentPresenter**](https://msdn.microsoft.com/library/windows/apps/BR209378)
* [
              **Control**
            ](https://msdn.microsoft.com/library/windows/apps/BR209390) und abgeleitete Klassen
* [**FontIcon**](https://msdn.microsoft.com/library/windows/apps/Dn279514)
* [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565)
* [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)
* [
              **TextElement**
            ](https://msdn.microsoft.com/library/windows/apps/BR209967) und abgeleitete Klassen

<span id="related_topics"/>
## Verwandte Themen  
* [Barrierefreiheit](accessibility.md)
* [Grundlegende Barrierefreiheitsinformationen](basic-accessibility-information.md)
* [Beispiel für die XAML-Textanzeige](http://go.microsoft.com/fwlink/p/?linkid=238579)
* [Beispiel für die XAML-Textbearbeitung](http://go.microsoft.com/fwlink/p/?linkid=251417)
* [XAML-Beispiel für Barrierefreiheit](http://go.microsoft.com/fwlink/p/?linkid=238570)



<!--HONumber=Jun16_HO4-->


