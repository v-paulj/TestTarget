---
author: Jwmsft
Description: "Befolgen Sie bei der Auswahl von Schriftarten und der Angabe von Schriftgraden und Schriftfarben für UWP-Apps die folgenden Richtlinien."
title: "Schriftarten für UWP-Apps"
ms.assetid: 1B8B90AD-CDC4-4997-ACDE-871C1E94A929
label: Fonts
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: d7236006f2c620a4ff0de4e0f413f32a2eaf5687
ms.openlocfilehash: b79a6f3ee32494f04fa472c0531c06aa0a60098b

---

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

# Schriftarten für UWP-Apps

In diesem Artikel sind die empfohlenen Schriftarten für UWP-Apps aufgeführt. Diese Schriftarten sind garantiert in allen Editionen von Windows10 verfügbar, die UWP-Apps unterstützen.

<div class="important-apis" >
<b>Wichtige APIs</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/br209655"><strong>FontFamily-Eigenschaft</strong></a></li>
</ul>

</div>
</div>



Der [UWP-Typografieleitfaden](typography.md) empfiehlt für Apps die Schriftart „SegoeUI“. Zwar eignet sich SegoeUI für zahlreiche Apps, muss jedoch nicht überall verwendet werden. Sie können andere Schriftarten für bestimmte Szenarien verwenden, z.B. zum Lesen oder wenn Sie Text in bestimmten Sprachen anzeigen. 



 
## Serifenlose Schriftarten

Serifenlose Schriftarten eignen sich für Überschriften und UI-Elemente. 

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Schriftfamilie</th>
<th align="left">Stile</th>
<th align="left">Hinweise</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left" style="font-family: Arial;">Arial</td>
<td align="left">Normal, kursiv, fett, fett kursiv, schwarz</td>
<td align="left">Unterstützung für europäische und nahöstliche Schriften (Lateinisch, Griechisch, Kyrillisch, Arabisch, Armenisch und Hebräisch). Die Schriftbreite „Black“ unterstützt nur europäische Schriften.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Calibri;">Calibri</td>
<td align="left">Normal, kursiv, fett, fett kursiv, dünn, dünn kursiv</td>
<td align="left">Unterstützung für europäische und nahöstliche Schriften (Lateinisch, Griechisch, Kyrillisch, Arabisch und Hebräisch). Arabisch nur in gerader Schrift verfügbar.</td>
</tr>
<td style="font-family: Consolas;">Consolas</td>
<td>Normal, kursiv, fett, fett kursiv</td>
<td>Schriftart mit fester Breite mit Unterstützung für europäische Schriften (Lateinisch, Griechisch und Kyrillisch).</td>
</tr>

<tr>
<td style="font-family: Segoe UI;">Segoe UI</td>
<td>Normal, kursiv, Light kursiv, Black kursiv, fett, fett kursiv, Light, Semilight, Semibold, Black</td>
<td>Benutzeroberflächen-Schriftart für europäische und nahöstliche Schriften (Arabisch, Armenisch, Kyrillisch, Georgisch, Griechisch, Hebräisch, Lateinisch) und auch Lisu-Schrift.</td>
</tr>

<tr class="odd">
<td>Segoe UI historisch</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für historische Schriften</td>
</tr>

<tr class="even">
<td style="font-family: Selawik;">Selawik</td>
<td align="left">Normal, Semilight, Light, fett, Semibold</td>
<td align="left">Open-Source-Schriftart, die metrisch kompatibel mit SegoeUI ist. Vorgesehen für Apps auf anderen Plattformen, auf denen SegoeUI nicht verfügbar ist. [Laden Sie Selawik über GitHub herunter.](https://github.com/Microsoft/Selawik)</td>
</tr>

<tr class="even">
<td style="font-family: Verdana;">Verdana</td>
<td align="left">Normal, kursiv, fett, fett kursiv</td>
<td align="left">Unterstützung für europäische Schriften (Lateinisch, Griechisch, Kyrillisch und Armenisch).</td>
</tr>

</tbody>
</table>


## Serifenschriftarten

Mit Serifenschriftarten lassen sich größere Textmengen gut darstellen. 

<table>
<thead>
<tr class="header">
<th align="left">Schriftfamilie</th>
<th align="left">Stile</th>
<th align="left">Hinweise</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Cambria;">Cambria</td>
<td align="left">Regular</td>
<td align="left">Serifenschriftart mit Unterstützung für europäischen Schriften (Lateinisch, Griechisch, Kyrillisch).</td>
</tr>
<tr class="even">
<td style="font-family: Courier New;">Courier New</td>
<td align="left">Normal, kursiv, fett, fett kursiv</td>
<td align="left">Serifenschriftart mit fester Breite und Unterstützung für europäische und nahöstliche Schriften (Lateinisch, Griechisch, Kyrillisch, Arabisch, Armenisch und Hebräisch).</td>
</tr>
<tr class="odd">
<td style="font-family: Georgia;">Georgia</td>
<td align="left">Normal, kursiv, fett, fett kursiv</td>
<td align="left">Unterstützung für europäische Schriften (Lateinisch, Griechisch und Kyrillisch).</td>
</tr>


<tr class="even">
<td style="font-family: Times New Roman;">Times New Roman</td>
<td align="left">Normal, kursiv, fett, fett kursiv</td>
<td align="left">Ältere Schriftart mit Unterstützung für europäische Schriften (Lateinisch, Griechisch, Kyrillisch, Arabisch, Armenisch, Hebräisch).</td>
</tr>

</tbody>
</table>

## Symbole


<table>
<thead>
<tr class="header">
<th align="left">Schriftfamilie</th>
<th align="left">Stile</th>
<th align="left">Hinweise</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Segoe MDL2-Ressourcen</td>
<td align="left">Regular</td>
<td align="left">Benutzeroberflächen-Schriftart für App-Symbole. Weitere Informationen finden Sie im Artikel [Segoe MDL2 Assets](segoe-ui-symbol-font.md).</td>
</tr>
<tr class="even">
<td align="left">Segoe UI-Emoji</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für Symbole</td>
</tr>
</tbody>
</table>



## Schriftarten für nicht lateinische Sprachen

Obwohl viele dieser Schriftarten auch lateinische Zeichen anbieten.

<table>
<thead>
<tr class="header">
<th align="left">Schriftfamilie</th>
<th align="left">Stile</th>
<th align="left">Hinweise</th>
</tr>
</thead>
<tbody>

<tr class="odd">
<td style="font-family: Embrima;">Ebrima</td>
<td align="left">Normal, fett</td>
<td align="left">Benutzeroberflächen-Schriftart für afrikanische Schriften (Äthiopisch, N'Ko, Osmanya, Tifinagh, Vai).</td>
</tr>
<tr class="even">
<td style="font-family: Gadugi;">Gadugi</td>
<td align="left">Normal, fett</td>
<td align="left">Benutzeroberflächen-Schriftart für nordamerikanische Schriften (Kanadische Silbenschrift, Cherokee).</td>
</tr>
<tr class="even">
<td align="left">Javanischer Text Normal Fallbackschriftart für javanische Schrift</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für javanische Schrift</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Leelawadee UI;">LeelawadeeUI</td>
<td align="left">Normal, Semilight, fett</td>
<td align="left">Benutzeroberflächen-Schriftart für südostasiatische Schriften (Buginesisch, Laotisch, Khmer, Thailändisch).</td>
</tr>

<tr class="odd">
<td align="left" style="font-family: Malgun Gothic;">Malgun Gothic</td>
<td align="left">Regular</td>
<td align="left">Benutzeroberflächen-Schriftart für Koreanisch.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft Himalaya;">Microsoft Himalaya</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für die tibetische Schrift.</td>
</tr>
<!--
<tr class="odd">
<td align="left" style="font-family: Microsoft JhengHei;">Microsoft JhengHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
-->
<tr class="even">
<td align="left" style="font-family: Microsoft JhengHei UI;">Microsoft JhengHei UI</td>
<td align="left">Normal, fett, Light</td>
<td align="left">Benutzeroberflächen-Schriftart für Chinesisch (traditionell).</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft New Tai Lue;">Microsoft Neu-Tai-Lue</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für die Neu-Tai-Lue-Schrift.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft PhagsPa;">Microsoft PhagsPa</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für die Phags-pa-Schrift.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft Tai Le;">Microsoft Tai Le</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für die Tai Le-Schrift.</td>
</tr>
<!--
<tr class="even">
<td align="left" style="font-family: Microsoft YaHei;">Microsoft YaHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
-->
<tr class="odd">
<td align="left" style="font-family: Microsoft YaHei UI;">Microsoft YaHei UI</td>
<td align="left">Normal, fett, Light</td>
<td align="left">Benutzeroberflächen-Schriftart für Chinesisch (vereinfacht).</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft Yi Baiti;">Microsoft Yi Baiti</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für die Yi-Schrift.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Mongolian Baiti;">Mongolisches Baiti</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für die mongolische Schrift.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: MV Boli;">MV Boli</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für die Thaana-Schrift.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Myanmar Text;">Myanmar Text</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für die Myanmar-Schrift.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Nirmala UI;">Nirmala UI</td>
<td align="left">Normal, Semilight, fett</td>
<td align="left">Benutzeroberflächen-Schriftart für südasiatische Schriften (Bangla, Devanagari, Gujarati, Gurmukhi, Kannada, Malayalam, Odia, Ol Chiki, Singhalesisch, Sora Sompeng, Tamil, Telugu)</td>
</tr>

<tr class="odd">
<td align="left" style="font-family: SimSun;">SimSun</td>
<td align="left">Regular</td>
<td align="left">Eine ältere chinesische UI-Schriftart. </td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Yu Gothic;">Yu Gothic</td>
<td align="left">Mittel</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left" style="font-family: Yu Gothic UI;">Yu Gothic UI</td>
<td align="left">Regular</td>
<td align="left">Benutzeroberflächen-Schriftart für Japanisch.</td>
</tr>
</tbody>
</table>


## Globalisierung/Lokalisierung von Schriftarten
Verwenden Sie die [LanguageFont-Schriftartenersetzungs-APIs](https://msdn.microsoft.com/library/windows/apps/br206864) für den programmgesteuerten Zugriff auf die Empfohlenen Einstellungen für Familie, Grad, Breite und Schnitt der Schriftart für eine spezielle Sprache. Das LanguageFont-Objekt ermöglicht den Zugriff auf die richtigen Schriftartinformationen für verschiedene Inhaltskategorien: UI-Kopfzeilen, Benachrichtigungen, Textkörper und Schriftarten für den Textkörper, die vom Benutzer bearbeitet werden können. Weitere Informationen finden Sie unter [Anpassen von Layout und Schriftarten zur Globalisierungsunterstützung](https://msdn.microsoft.com/windows/uwp/globalizing/adjust-layout-and-fonts--and-support-rtl).

<!--
## Triggering a font download
If you use a font that's not listed in this article, your app might trigger an automatic download of the font data from a Microsoft service. This can have performance and other impacts that may be a concern, particularly for mobile devices. In particular, note that this might consume some of a user's mobile data plan or result in mobile data usage costs. UWP apps that will available on mobile devices should never use fonts for UI content other than fonts in this list.
-->

## Beispiele herunterladen

* [Beispiel: Herunterladbare Schriftarten](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlCloudFontIntegration)
* [Beispiel: UI-Grundlagen](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)
* [Zeilenabstand mit DirectWrite-Beispiel](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/DWriteLineSpacingModes) 

## Verwandte Artikel

* [Anpassen von Layout und Schriftarten zur Globalisierungsunterstützung](https://msdn.microsoft.com/windows/uwp/globalizing/adjust-layout-and-fonts--and-support-rtl)
* [Segoe MDL2](segoe-ui-symbol-font.md)
* [Textsteuerelemente](../controls-and-patterns/text-controls.md)
* [XAML-Designressourcen](https://msdn.microsoft.com/library/windows/apps/mt187274)

 

 







<!--HONumber=Aug16_HO3-->


