---
author: Jwmsft
Description: Befolgen Sie bei der Auswahl von Schriftarten und der Angabe von Schriftgraden und Schriftfarben die folgenden Richtlinien.
title: Schriftarten
ms.assetid: 1B8B90AD-CDC4-4997-ACDE-871C1E94A929
label: Fonts
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 7db364240dd98f59a4a4d1d0c23cee1195682de2
ms.openlocfilehash: 52de4d9517c7f3064ad9e589a95e6f96400524cc

---

# Richtlinien für Schriftarten

**Wichtige APIs**

-   [**FontFamily-Eigenschaft**](https://msdn.microsoft.com/library/windows/apps/br209655)

Die richtige Verwendung von Schriftgraden, -breiten, -farben, Laufweite und Abstand kann Ihrer UWP-App (Universelle Windows-Plattform) ein klares und übersichtliches Erscheinungsbild verleihen und damit ihre Verwendung erleichtern. Befolgen Sie bei der Auswahl von Schriftarten und der Angabe von Schriftgraden und Schriftfarben die folgenden Richtlinien.

Eine Liste der Segoe UI Symbol-Symbole finden Sie unter [**Richtlinien für Segoe UI Symbol-Symbole**](segoe-ui-symbol-font.md).

## Der Typenverlauf von Windows 10


Die Typhierarchie stellt eine wichtige gestalterische Beziehung zwischen Überschrift und Textkörper her und schafft damit eine klare und verständliche Hierarchie zwischen den verschiedenen Ebenen. Benutzer verstehen sofort, wo sie Informationen finden und wie Sie die Seite analysieren können.

Hier ist die Typhierarchie, die wir für UWP-Apps empfehlen:

| Textstil | Schriftart | Breite    | Größe (Epx) | Zeilenabstand (Epx) | Wortabstand | Laufweite (1/1000 Geviert) | XAML-Stilschlüssel          |
|------------|----------|-----------|------------|--------------------|--------------|----------------------|-------------------------|
| Header     | Segoe UI | Light     | 46         | 56                 | 100%         | 0                    | HeaderTextBlockStyle    |
| Unterüberschrift  | Segoe UI | Light     | 34         | 40                 | 100%         | 0                    | SubheaderTextBlockStyle |
| Überschrift      | Segoe UI | Semilight | 24         | 28                 | 100%         | 0                    | TitleTextBlockStyle     |
| Untertitel   | Segoe UI | Regular   | 20         | 24                 | 100%         | 0                    | SubtitleTextBlockStyle  |
| Basis       | Segoe UI | SemiBold  | 15         | 20                 | 100%         | 0                    | BaseTextBlockStyle      |
| Textkörper       | Segoe UI | Regular   | 15         | 20                 | 100%         | 0                    | BodyTextBlockStyle      |
| Aufnahme    | Segoe UI | Regular   | 12         | 14                 | 100%         | 0                    | CaptionTextBlockStyle   |

 

## Empfohlene Schriftarten


Sie müssen die Segoe UI-Schriftart nicht für alles verwenden. Sie können andere Schriftarten für bestimmte Szenarien verwenden, z. B. zum Lesen oder wenn Sie Text in anderen Sprachen anzeigen.

Dies ist die Liste der Schriftarten, die garantiert in allen Editionen von Windows10 verfügbar sind, die UWP-Apps unterstützen.

**Hinweis**  Wenn Sie eine Schriftart verwenden, die nicht in dieser Liste enthalten ist, löst Ihre App möglicherweise einen automatischen Download der Schriftartdaten von einem Microsoft-Dienst aus. Dies kann die Leistung und andere Faktoren beeinträchtigen, die insbesondere für mobile Geräte möglicherweise ein Problem darstellen. Beachten Sie zudem, dass dabei möglicherweise der mobile Datentarif eines Benutzers in Anspruch genommen wird oder Kosten für die Nutzung mobiler Daten anfallen. UWP-Apps, die auf mobilen Geräten verfügbar sind, sollten nie andere Schriftarten für UI-Content als in dieser Liste verwenden.

 

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
<th align="left">Kommentar</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Arial</td>
<td align="left">Normal, kursiv, fett, fett kursiv, schwarz</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Calibri</td>
<td align="left">Normal, kursiv, fett, fett kursiv, dünn, dünn kursiv</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Cambria</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Cambria Math</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Comic Sans MS</td>
<td align="left">Normal, kursiv, fett, fett kursiv</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Courier New</td>
<td align="left">Normal, kursiv, fett, fett kursiv</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Ebrima</td>
<td align="left">Normal, fett</td>
<td align="left">Benutzeroberflächen-Schriftart für afrikanische Schriften (Äthiopisch, N'Ko, Osmanisch, Tifinagh, Vai)</td>
</tr>
<tr class="even">
<td align="left">Gadugi</td>
<td align="left">Regular</td>
<td align="left">Benutzeroberflächen-Schriftart für nordamerikanische Schriften (Kanadische Silbenschrift, Cherokee)</td>
</tr>
<tr class="odd">
<td align="left">Georgia</td>
<td align="left">Normal, kursiv, fett, fett kursiv</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Javanischer Text Normal Fallbackschriftart für javanische Schrift</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für javanische Schrift</td>
</tr>
<tr class="odd">
<td align="left">LeelawadeeUI</td>
<td align="left">Normal, Semilight, fett</td>
<td align="left">Benutzeroberflächen-Schriftart für südostasiatische Schriften (Buginesisch, Laotisch, Khmer, Thailändisch)</td>
</tr>
<tr class="even">
<td align="left">Lucida Console</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Malgun Gothic</td>
<td align="left">Regular</td>
<td align="left">Benutzeroberflächen-Schriftart für Koreanisch</td>
</tr>
<tr class="even">
<td align="left">Microsoft Himalaya</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für tibetische Schrift</td>
</tr>
<tr class="odd">
<td align="left">Microsoft JhengHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Microsoft JhengHei UI</td>
<td align="left">Regular</td>
<td align="left">Benutzeroberflächen-Schriftart für Chinesisch (traditionell)</td>
</tr>
<tr class="odd">
<td align="left">Microsoft Neu-Tai-Lue</td>
<td align="left">Regular</td>
<td align="left">Fallback-Schriftart für Neu-Tai-Lue-Schrift</td>
</tr>
<tr class="even">
<td align="left">Microsoft PhagsPa</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für Phags-pa-Schrift</td>
</tr>
<tr class="odd">
<td align="left">Microsoft Tai Le</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart Tai Le-Schrift</td>
</tr>
<tr class="even">
<td align="left">Microsoft YaHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Microsoft YaHei UI</td>
<td align="left">Regular</td>
<td align="left">Benutzeroberflächen-Schriftart für Chinesisch (vereinfacht)</td>
</tr>
<tr class="even">
<td align="left">Microsoft Yi Baiti</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für Yi-Schrift</td>
</tr>
<tr class="odd">
<td align="left">Mongolisches Baiti</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für mongolische Schrift</td>
</tr>
<tr class="even">
<td align="left">MV Boli</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für Thaana-Schrift</td>
</tr>
<tr class="odd">
<td align="left">Myanmar Text</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für Myanmar-Schrift</td>
</tr>
<tr class="even">
<td align="left">Nirmala UI</td>
<td align="left">Normal, Semilight, fett</td>
<td align="left">Benutzeroberflächen-Schriftart für südasiatische Schriften (Bangla, Devanagari, Gujarati, Gurmukhi, Kannada, Malayalam, Odia, Ol Chiki, Singhalesisch, Sora Sompeng, Tamil, Telugu)</td>
</tr>
<tr class="odd">
<td align="left">Segoe MDL2-Ressourcen</td>
<td align="left">Regular</td>
<td align="left">Benutzeroberflächen-Schriftart für App-Symbole</td>
</tr>
<tr class="even">
<td align="left">Segoe-Druck</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Segoe UI</td>
<td align="left">Normal, kursiv, fett, fett kursiv, dünn, Semilight, Semibold, schwarz</td>
<td align="left">Benutzeroberflächen-Schriftart für europäische und nahöstliche Schriften (Arabisch, Armenisch, Kyrillisch, Georgisch, Griechisch, Hebräisch, Lateinisch) und auch Lisu-Schrift</td>
</tr>
<tr class="even">
<td align="left">Segoe UI-Emoji</td>
<td align="left">Regular</td>
<td align="left">In der in Windows Phone enthaltenen Version ist jedes Emoticon mit einem weißen Rand versehen, um sicherzustellen, dass es auf allen Hintergrundfarben angezeigt werden kann. Sie ist metrisch kompatibel mit der in Windows enthaltenen Version.</td>
</tr>
<tr class="odd">
<td align="left">Segoe UI historisch</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für historische Schriften</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für Symbole</td>
</tr>
<tr class="odd">
<td align="left">SimSun</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Times New Roman</td>
<td align="left">Normal, kursiv, fett, fett kursiv</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Trebuchet MS</td>
<td align="left">Normal, kursiv, fett, fett kursiv</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Verdana</td>
<td align="left">Normal, kursiv, fett, fett kursiv</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Webdings</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Wingdings</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Yu Gothic</td>
<td align="left">Medium</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Yu Gothic UI</td>
<td align="left">Regular</td>
<td align="left">Benutzeroberflächen-Schriftart für Japanisch</td>
</tr>
</tbody>
</table>

 

## Verwandte Themen

**Für Designer**
* [Beschriftung (oder Textblock)](../controls-and-patterns/labels.md)
* [Segoe UI Symbol-Symbole](segoe-ui-symbol-font.md)
**Für Entwickler (XAML)**
* [XAML-Designressourcen](https://msdn.microsoft.com/library/windows/apps/mt187274)
* [Gestalten einer App-Seite](https://msdn.microsoft.com/library/windows/apps/hh872191)
* [Segoe UI Symbol-Symbole](segoe-ui-symbol-font.md)
* [**TextBlock.FontFamily-Eigenschaft**](https://msdn.microsoft.com/library/windows/apps/br209655)

**Beispiele**
* [Beispiel für die XAML-Textanzeige](http://go.microsoft.com/fwlink/p/?linkid=238578)
* [CSS-Formatvorlagen: Beispiel für das Branding Ihrer App](http://go.microsoft.com/fwlink/p/?linkid=231641)
* [Beispiel für Sprachschriftartzuordnung](http://go.microsoft.com/fwlink/p/?linkid=231603)
 

 







<!--HONumber=Jul16_HO2-->


