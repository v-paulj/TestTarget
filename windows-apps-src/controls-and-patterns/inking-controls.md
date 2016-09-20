---
author: Karl-Bridge-Microsoft
Description: Beschreibung der Freihandtools
title: "Steuerelemente für Freihandeingaben"
label: Inking Controls
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 7198f4084df8ce484cdc5f6b3231a4bdb02f18b5

---
# Steuerelemente für Freihandeingaben

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Es gibt zwei verschiedene Steuerelemente, die die Freihandeingabe in Apps in der universellen Windows-Plattform (UWP) ermöglichen: [**InkCanvas**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) und [**InkToolbar**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

Das InkCanvas-Steuerelement rendert Stifteingaben entweder als Freihandstrich (mit Standardeinstellungen für Farbe und Breite) oder als Radierstrich. Dieses Steuerelement ist eine transparente Überlagerung, die keine integrierte Benutzeroberfläche zum Ändern der Standardeigenschaften von Freihandstrichen enthält.

>**Hinweis**&nbsp;&nbsp;Das InkCanvas-Steuerelement kann zur Unterstützung ähnlicher Funktionen für Maus- und Toucheingabe konfiguriert werden.

Da das InkCanvas-Steuerelement keine Unterstützung für das Ändern der Standardeinstellungen von Freihandstrichen enthält, kann es mit einem InkToolbar-Steuerelement kombiniert werden. Das InkToolbar-Steuerelement enthält eine anpassbare und erweiterbare Sammlung von Schaltflächen, die Features für Freihandeingaben in einem verknüpften InkCanvas-Steuerelement aktivieren.

Das InkToolbar-Steuerelement enthält standardmäßig Schaltflächen zum Zeichnen, Löschen, Hervorheben sowie zum Anzeigen eines Lineals. Abhängig vom Feature werden in einem Flyout weitere Einstellungen und Befehle bereitgestellt, beispielsweise für Freihandfarbe, Strichstärke und das Löschen aller Freihandeingaben.

>**Hinweis**&nbsp;&nbsp;Das InkToolbar-Steuerelement unterstützt Stift- und Mauseingaben und kann zur Erkennung von Toucheingaben konfiguriert werden.

<img src="images/ink-tools-invoked-toolbar.png" width="300">

<div class="important-apis" >
<b>Wichtige APIs</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx"><strong>InkCanvas-Klasse</strong></a></li>
<li><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx"><strong>InkToolbar-Klasse</strong></a></li>
<li><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx"><strong>InkPresenter-Klasse</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/br208524"><strong>Windows.UI.Input.Inking</strong></a></li>
</ul>

</div>
</div>




## Ist dies das richtige Steuerelement?

Verwenden Sie das InkCanvas-Steuerelement, wenn Sie in Ihrer App einfache Freihandfeatures ohne Freihandeinstellungen für den Benutzer bereitstellen müssen.

Standardmäßig werden Striche als Freihandeingabe gerendert, wenn die Stiftspitze (ein schwarzer Kugelschreiber mit einer Stärke von 2 Pixeln) verwendet wird, und als Radierer, wenn die Radiergummispitze verwendet wird. Falls keine Radiergummispitze vorhanden ist, kann das InkCanvas-Steuerelement so konfiguriert werden, dass Eingaben mit der Stiftspitze wie Radierstriche behandelt werden.

Kombinieren Sie das InkCanvas-Steuerelement mit einem InkToolbar-Steuerelement, um eine Benutzeroberfläche zum Aktivieren von Freihandfeatures und Festlegen grundlegender Freihandeigenschaften wie Strichgröße, Farbe und Form der Stiftspitze bereitzustellen.

>**Hinweis**&nbsp;&nbsp;Verwenden Sie das zugrunde liegende [**InkPresenter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx)-Objekt, wenn Sie umfassendere Anpassungen am Rendering von Freihandeingaben für ein InkCanvas-Steuerelement vornehmen möchten.

## Beispiele

**Microsoft Edge**

Der Edge-Browser verwendet „InkCanvas” und „InkToolbar” für **Webseitennotizen**.  
![Das InkCanvas-Steuerelement wird für Freihandeingaben in Microsoft Edge verwendet.](images/ink-tools-edge.png)

**Windows Ink-Arbeitsbereich**

Die InkCanvas- und InkToolbar-Steuerelemente werden auch für den **Skizzenblock** und **Bildschirmskizzen** im **Windows Ink-Arbeitsbereich** verwendet.  
![InkToolbar-Steuerelement im Windows Ink-Arbeitsbereich](images/ink-tools-ink-workspace.png)

## Erstellen eines InkCanvas- und InkToolbar-Steuerelements

Zum Hinzufügen eines InkCanvas-Steuerelements zu Ihrer App ist nur eine Markupzeile erforderlich:

```xaml
<InkCanvas x:Name=“myInkCanvas”/>
```

>**Hinweis**&nbsp;&nbsp;Ausführliche Informationen zur Anpassung von „InkCanvas” mit „InkPresenter” finden Sie im Artikel [Zeichen- und Eingabestiftinteraktionen in UWP-Apps](http://windowsstyleguide/input-and-devices/pen-and-stylus-interactions/).

Das InkToolbar-Steuerelement muss in Verbindung mit einem InkCanvas-Steuerelement verwendet werden. Zum Einbinden eines InkToolbar-Steuerelements (mit allen integrierten Tools) in Ihre App ist nur eine zusätzliche Markupzeile erforderlich:

 ```xaml
<InkToolbar TargetInkCanvas=“{x:Bind myInkCanvas}”/>
 ```

Dadurch wird das folgende InkToolbar-Steuerelement angezeigt:
<img src="images/ink-tools-uninvoked-toolbar.png" width="250">

### Integrierte Schaltflächen

Das InkToolbar-Steuerelement enthält die folgenden integrierten Schaltflächen:

**Stifte**

- Kugelschreiber – zeichnet mit einer runden Stiftspitze einen durchgehenden, undurchsichtigen Strich. Die Strichgröße hängt vom erkannten Stiftdruck ab.
- Bleistift – zeichnet mit einer runden Stiftspitze einen auslaufenden halbtransparenten Strich mit Textur (nützlich für überlagerte Schattierungseffekte). Die Strichfarbe (Dunkelheit) hängt vom erkannten Stiftdruck ab.
- Textmarker – zeichnet mit einer rechteckigen Stiftspitze einen halbtransparenten Strich.

Sie können sowohl die Farbpalette als auch die Größenattribute (Mindest-, Höchst- und Standardwerte) im Flyout für jeden Stift anpassen.

**Tool**

- Radierer – löscht alle Freihandstriche, die berührt werden. Beachten Sie, dass der gesamte Freihandstrich gelöscht wird, nicht nur der Teil unter dem Radiererstrich.

**Ein-/Ausschalten**

- Lineal – blendet das Lineal ein oder aus. Beim Zeichnen in der Nähe des Linealrands wird der Freihandstrich am Lineal ausgerichtet.  
 ![Dem InkToolbar-Steuerelement zugeordnetes visuelles Linealelement](images/inking-tools-ruler.png)

Obwohl dies die Standardkonfiguration ist, haben Sie die vollständige Kontrolle über die integrierten Schaltflächen, die im InkToolbar-Steuerelement für Ihre App enthalten sind.

### Benutzerdefinierte Schaltflächen

Das InkToolbar-Steuerelement besteht aus zwei unterschiedlichen Gruppen von Schaltflächentypen:

1. Eine Gruppe von „Toolschaltflächen“ mit den integrierten Schaltflächen zum Zeichnen, Radieren und Hervorheben. Hier werden benutzerdefinierte Stifte und Tools hinzugefügt.
> **Hinweis**&nbsp;&nbsp;Die ausgewählten Features schließen sich gegenseitig aus.

2. Eine Gruppe von „Umschaltflächen“ mit der integrierten Linealschaltfläche. Hier werden benutzerdefinierte Umschaltflächen hinzugefügt.
> **Hinweis**&nbsp;&nbsp;Die Features schließen sich nicht gegenseitig aus und können gleichzeitig mit anderen aktiven Tools verwendet werden.

Je nach Anwendung und erforderlicher Freihandfunktion können Sie dem InkToolbar-Steuerelement eine der folgenden Schaltflächen (die an die benutzerdefinierten Freihandfunktionen gebunden sind) hinzufügen:

- Benutzerdefinierter Stift: Ein Stift, für den die Farbpaletten- und Stiftspitzeneigenschaften der Freihandeingabe wie Form, Drehung und Größe von der Host-App definiert werden.
- Benutzerdefiniertes Tool: Ein Tool ohne Stift, das von der Host-App definiert wird.
- Benutzerdefiniertes Umschalten: Legt den Zustand eines durch die App definierten Features auf „aktiviert“ oder „deaktiviert“ fest. Wenn die Schaltfläche aktiviert ist, funktioniert das Feature in Verbindung mit dem aktiven Tool.

> **Hinweis**&nbsp;&nbsp;Die Anzeigereihenfolge der integrierten Schaltflächen kann nicht geändert werden. Die standardmäßige Anzeigereihenfolge lautet wie folgt: Kugelschreiber, Stift, Textmarker, Radierer und Lineal. Benutzerdefinierte Stifte werden an den letzten Standardstift angefügt, benutzerdefinierte Tool-Schaltflächen werden zwischen der letzten Stiftschaltfläche und der Radiererschaltfläche hinzugefügt, und benutzerdefinierte Umschaltflächen werden nach der Linealschaltfläche hinzugefügt. (Benutzerdefinierte Schaltflächen werden in der Reihenfolge hinzugefügt, in der sie angegeben werden.)

Obwohl das InkToolbar-Steuerelement ein Element auf oberster Ebene sein kann, wird es in der Regel über eine Schaltfläche oder einen Befehl für die Freihandeingabe verfügbar gemacht. Wir empfehlen, die Glyphe EE56 aus der Schriftart „Segoe MLD2 Assets“ als Symbol auf oberster Ebene zu verwenden.

## InkToolbar-Interaktion

Alle integrierten Stift- und Toolschaltflächen enthalten ein Flyoutmenü, in dem Freihandeigenschaften sowie die Form und Größe der Stiftspitze festgelegt werden können. Eine „Erweiterungsglyphe” ![InkToolbar-Glyphe](images/ink-tools-glyph.png) wird auf der Schaltfläche angezeigt, um auf das Vorhandensein des Flyouts hinzuweisen.

Das Flyout wird angezeigt, wenn die Schaltfläche eines aktiven Tools erneut ausgewählt wird. Wenn die Farbe oder Größe geändert wird, wird das Flyout automatisch geschlossen, und die Freihandeingabe kann fortgesetzt werden. Für benutzerdefinierte Stifte und Tools kann das Standardflyoutmenü oder ein benutzerdefiniertes Flyoutmenü verwendet werden.

Der Radierer verfügt ebenfalls über ein Flyout mit dem Befehl **Freihand vollständig löschen**.  
![InkToolbar-Steuerelement mit aufgerufenem Radierer-Flyout](images/ink-tools-erase-all-ink.png)

 Informationen zur Anpassung und Erweiterbarkeit finden Sie im [einfachen Freihandbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk).

## Empfohlene und nicht empfohlene Vorgehensweisen

- Das InkCanvas-Steuerelement und die Freihandeingabe im Allgemeinen bieten bei der Verwendung eines aktiven Stifts die beste Benutzerfreundlichkeit. Es wird jedoch empfohlen, die Freihandeingabe mit Maus- und Toucheingabe (einschließlich des passiven Stifts) zu unterstützen, wenn dies für Ihre App erforderlich ist.
- Verwenden Sie ein InkToolbar-Steuerelement mit dem InkCanvas-Steuerelement, um grundlegende Freihandfeatures und -einstellungen bereitzustellen. Sowohl das InkCanvas- als auch das InkToolbar-Steuerelement können programmgesteuert angepasst werden.
- Das InkToolbar-Steuerelement und die Freihandeingabe im Allgemeinen bieten bei der Verwendung eines aktiven Stifts die beste Benutzerfreundlichkeit. Die Freihandeingabe mit Maus- und Toucheingabe kann aber unterstützt werden, wenn dies für Ihre App erforderlich ist.
- Wenn Sie die Freihandeingabe per Toucheingabe unterstützen, wird empfohlen, das Symbol ED5F aus der Schriftart „Segoe MLD2 Assets” für die Umschaltfläche mit einer QuickInfo „Schreiben durch Berühren” zu verwenden.
- Wenn Sie mehrere InkCanvas-Steuerelemente verwenden, empfehlen wir die Verwendung eines einzelnen InkToolbar-Steuerelements zum Steuern der Freihandeingabe auf allen Canvas.
- Für eine optimale Leistung empfehlen wir, das Standardflyout zu ändern, anstatt ein benutzerdefiniertes Flyoutmenü für standardmäßige und benutzerdefinierte Tools zu erstellen.

## Beispiele herunterladen

Im [einfachen Freihandbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk) werden acht Szenarien im Zusammenhang mit den Anpassungs- und Erweiterbarkeitsfunktionen der InkCanvas- und InkToolbar-Steuerelemente erläutert. Jedes Szenario bietet einen allgemeinen Überblick über gängige Situationen bei der Freihandeingabe und Steuerelementimplementierungen.

Erweiterte Szenarien finden Sie im [komplexen Freihandbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk).

## Verwandte Artikel

- [Zeichen- und Eingabestiftinteraktionen in UWP-Apps](http://windowsstyleguide/input-and-devices/pen-and-stylus-interactions/)
- [Erkennen von Freihandstrichen](http://windowsstyleguide/input-and-devices/convert-ink-to-text/)
- [Speichern und Abrufen von Freihandstrichen](http://windowsstyleguide/input-and-devices/save-and-load-ink/)



<!--HONumber=Aug16_HO3-->


