---
author: DelfCo
Description: Entwickeln Sie Ihre App so, dass die Layouts und Schriftarten mehrerer Sprachen unterstützt werden – also beispielsweise auch die Flussrichtung von rechts nach links (right-to-left, RTL).
title: Anpassen von Layout und Schriftarten und Unterstützen von „Von rechts nach links“
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
---

# Anpassen von Layout und Schriftarten und Unterstützen von „Von rechts nach links“





Entwickeln Sie Ihre App so, dass die Layouts und Schriftarten mehrerer Sprachen unterstützt werden – also beispielsweise auch die Flussrichtung von rechts nach links (right-to-left, RTL).

## <span id="Layout_guidelines"></span><span id="layout_guidelines"></span><span id="LAYOUT_GUIDELINES"></span>Layoutrichtlinien


Einige Sprachen, darunter Deutsch und Finnisch, benötigen für den Text mehr Raum als Englisch. Für die Schriftarten einiger Sprachen, z. B. Japanisch, wird eine größere Höhe benötigt. Und bei Sprachen wie Arabisch und Hebräisch muss die Leserichtung des Text- und des App-Layouts von rechts nach links verlaufen.

Verwenden Sie flexible Layoutmechanismen anstelle einer absoluten Positionierung oder fester Breiten und Höhen. Bestimmte UI-Elemente können abhängig von der Sprache geändert werden.

### <span id="XAML"></span><span id="xaml"></span>XAML

Geben Sie eine **Uid** für ein Element an:

```XML
<TextBlock x:Uid="Block1">
```

Stellen Sie sicher, dass die „ResW“-Datei der App eine Ressource für „Block1.Width“ besitzt, die Sie für jede zu lokalisierende Sprache festlegen können.

Verwenden Sie bei Windows Store-Apps mit C++, C\# oder Visual Basic die [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716)-Eigenschaft mit symmetrischen Abständen und Rändern, um die Lokalisierung für andere Layoutrichtungen zu ermöglichen.

Bei XAML-Layoutsteuerelementen wie [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) erfolgt das Skalieren und Kippen automatisch mit der [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716)-Eigenschaft. Machen Sie Ihre eigene **FlowDirection**-Eigenschaft in Ihrer App als Ressource für Lokalisierer verfügbar.

Geben Sie eine **Uid** für die Hauptseite der App an:

```XML
<Page x:Uid="MainPage">
```

Stellen Sie sicher, dass die **ResW**-Datei der App eine Ressource für MainPage.FlowDirection besitzt, die Sie für jede zu lokalisierende Sprache festlegen können.

### <span id="HTML"></span><span id="html"></span>HTML

Verwenden Sie für Windows Store-Apps mit JavaScript [Cascading Style Sheets (CSS)](https://msdn.microsoft.com/library/ms531209)-Layoutmechanismen wie [-ms-grid](https://msdn.microsoft.com/en-us/library/windows/apps/hh465453.aspx#g_section) und [-ms-box](https://msdn.microsoft.com/en-us/library/windows/apps/hh465453.aspx#f_section). Verwenden Sie symmetrische Abstände und Ränder, um die Lokalisierung für verschiedene Layoutrichtungen zu ermöglichen.

Sie können auch den Pseudoklassenselektor [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) verwenden, um CSS-Eigenschaften wie die Breite für bestimmte Elemente basierend auf der Sprache der App anzupassen. Dazu muss der App-Host das **lang**-Attribut des Stammelements auf die App-Sprache festlegen.

**CSS**
```CSS
.item:-ms-lang(de, fi) { width: 350px; }
```

Bei Windows Store-Apps mit JavaScript, die das Stylesheet „ui-light.css“ oder „ui-dark.css“ verwenden, wird die Richtung des Textkörperlayouts automatisch je nach App-Sprache festgelegt. Das folgende CSS entspricht „ui-light.css“ und „ui-dark.css“. Sie müssen es also nicht selbst schreiben.

**CSS**
```CSS
body:-ms-lang(ar,he…) { direction: rtl;}
```

Die meisten App-Layouts werden somit richtig dargestellt, wenn das System eine Sprache von rechts nach links verwendet.

Wie bei [WinJS.UI](https://msdn.microsoft.com/library/windows/apps/br229782)-Steuerelementen kann die App den Pseudoklassenselektor [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) verwenden, um physische CSS-Eigenschaften anzupassen, z. B. **margin** und **padding**. Sie müssen keine logischen CSS-Eigenschaften anpassen, die Schlüsselwörter wie **after** und **before** einsetzen.

Verwenden Sie keine **align**-Eigenschaft oder ein entsprechendes Attribut in HTML. Steuern Sie stattdessen mit der **direction**-Eigenschaft die Ausrichtung bestimmter Komponenten.

Unterstützen Sie mit der [**writing-mode**](https://msdn.microsoft.com/library/ms531187)-Eigenschaft vertikale Textlayouts in CSS.

## <span id="Mirroring_images"></span><span id="mirroring_images"></span><span id="MIRRORING_IMAGES"></span>Spiegeln von Bildern


### <span id="XAML"></span><span id="xaml"></span>XAML

Wenn Ihre App Bilder enthält, die für die Leserichtung von rechts nach links gespiegelt werden müssen (Umkehrung desselben Bilds), können Sie die [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716)-Eigenschaft anwenden:

```XML
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```

### <span id="HTML"></span><span id="html"></span>HTML

Wenn Ihre App Bilder enthält, die für Rechts-nach-links-Sprachen gespiegelt werden müssen (Umkehrung desselben Bilds), können Sie die Bilder beim Rendern mithilfe von CSS-Transformationen spiegeln. Dazu fügen Sie den Elementen eine .mirrorable-Klasse hinzu. Fügen Sie außerdem die folgende CSS-Klasse hinzu:

```CSS
.mirrorable { transform: scaleX(-1); }
```

**Für XAML und HTML:** Wenn die App ein anderes Bild benötigt, damit das Bild richtig umgedreht werden kann, können Sie das Ressourcenverwaltungssystem mit dem [layoutdir-Qualifizierer](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324) verwenden. Das System wählt ein Bild mit dem Namen „file.layoutdir-rtl.png“ aus, wenn die [App-Sprache](manage-language-and-region.md) auf eine Rechts-nach-links-Sprache festgelegt wird. Diese Vorgehensweise ist möglicherweise nötig, wenn nicht alle Teile des Bilds umgedreht werden.

## <span id="Fonts"></span><span id="fonts"></span><span id="FONTS"></span>Schriftarten


**Für XAML und HTML:** Verwenden Sie die [**LanguageFont**](https://msdn.microsoft.com/library/windows/apps/br206864)-Schriftartenersetzungs-APIs für den programmgesteuerten Zugriff auf die empfohlene Familie sowie den Grad, die Breite und den Schnitt der Schriftart für eine spezielle Sprache. Das **LanguageFont**-Objekt ermöglicht den Zugriff auf die richtigen Schriftartinformationen für verschiedene Inhaltskategorien: UI-Kopfzeilen, Benachrichtigungen, Textkörper und Schriftarten für den Textkörper, die vom Benutzer bearbeitet werden können.

### <span id="HTML"></span><span id="html"></span>HTML

Bei Windows Store-Apps mit JavaScript, die das Stylesheet „ui-light.css“ oder „ui-dark.css“ verwenden, wird die Schriftart automatisch je nach App-Sprache auf die am besten geeignete Schriftart festgelegt. Der App-Host legt das **lang**-Attribut des Stammelements auf die App-Sprache fest.

Bei Apps, die mehrere Sprachen auf einer einzigen Seite anzeigen, sollte das **lang**-Attribut für den Abschnitt in jeder Sprache festgelegt werden. Der Pseudoklassenselektor [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) wählt für jeden Seitenabschnitt die richtige Schriftart aus.

 

 





<!--HONumber=May16_HO2-->


