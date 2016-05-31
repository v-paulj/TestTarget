---
author: Jwmsft
Description: In diesem Artikel finden Sie eine Liste der in der Schriftart „Segoe MDL2 Assets“ enthaltenen Glyphen und Hinweise zu deren Verwendung.
Search.Refinement.TopicID: 184
title: Richtlinien für Segoe MDL2-Symbole
ms.assetid: DFB215C2-8A61-4957-B662-3B1991AC9BE1
label: Segoe MDL2 icons
template: detail.hbs
---

# Richtlinien für Segoe MDL2-Symbole

In diesem Artikel finden Sie eine Liste der in der Schriftart „Segoe MDL2 Assets“ enthaltenen Glyphen und Hinweise zu deren Verwendung. Sie erhalten die Schriftart durch die Installation von Windows 10.

**Wichtige APIs**

-   [**Symbol-Enumeration (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn252842)
-   [**AppBarIcon-Enumeration (HTML)**](https://msdn.microsoft.com/library/windows/apps/hh770557)



## <span id="Recommendations"></span><span id="recommendations"></span><span id="RECOMMENDATIONS"></span>Empfehlungen


-   Verwenden Sie diese Glyphen nur, wenn Sie die Schriftart **Segoe MDL2-Ressourcen** explizit angeben können.

## <span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>Weitere Hinweise zur Verwendung


Die Symbolschriftart **Segoe UI Symbol** für Windows 8/8.1 wurde durch **Segoe MDL2 Assets** ersetzt, die ab der Veröffentlichung von Windows 10 gilt. Sie kann im Prinzip auf ähnliche Weise wie die ältere Schriftart verwendet werden. Es wurden jedoch viele Glyphen mit den festgelegten Metriken der Schriftart in den Windows 10-Symbolstil umgezeichnet, sodass Symbole im em-Quadrat der Schriftart ausgerichtet werden und nicht anhand einer typografischen Grundlinie.

**Hinweis**  Ein **Em** ist eine Maßeinheit in der Schriftart. Ein em in der Schriftart entspricht 100 % des angegebenen Punktwerts bei 72 ppi. Beispielsweise entspricht 16 pt 16 px bei 72 ppi (wird auch als 100 %-Plateau bezeichnet). Die neuen MDL2-Schriftarten wurden entwickelt, damit der Platzbedarf des Symbolbereichs einem em-Quadrat entspricht. Wenn Sie also 16 px für die Breite und Höhe im Code angeben, erhalten Sie ein 16 px x 16 px großes Symbol. Dies bedeutet jedoch nicht immer, dass das Symbol die vollständige Abmessung beansprucht.

 

**Segoe UI Symbol** steht jedoch weiterhin als „veraltete“ Ressource zur Verfügung. Es wird jedoch empfohlen, dass alle Anwendungen für die Verwendung der neuen **Segoe MDL2-Ressourcen**.

Die meisten in der **Segoe MDL2-Ressourcen** -Schriftart enthaltenen Symbole und UI-Steuerelemente sind dem PUA (Private Use Area) von Unicode zugeordnet. Mithilfe des PUA können Entwickler Glyphen, die keinen vorhandenen Codepunkten zugeordnet sind, private Unicode-Werte zuweisen. Dies ist hilfreich bei der Erstellung einer Symbolschriftart, führt jedoch auch zu einem Interoperabilitätsproblem. Ist die Schriftart nicht verfügbar, werden die Glyphen nicht angezeigt. Verwenden Sie die Glyphen nur, wenn Sie die Schriftart **Segoe MDL2-Ressourcen** explizit angeben können.

Bei Verwendung von Kacheln können Sie diese Glyphen nicht nutzen, da Sie die Kachelschriftart nicht angeben können und PUA-Glyphen nicht per Schriftartfallback verfügbar sind.

Anders als bei **Segoe UI Symbol** sind die Symbole in der Schriftart **Segoe MDL2 Assets** nicht für die Inlineverwendung mit Text vorgesehen. Demnach gelten einige ältere „Tricks“ wie die Pfeile für schrittweise Anzeige nicht mehr. Da alle neuen Symbole gleichermaßen größentechnisch angepasst und positioniert werden, müssen sie entsprechend nicht mit einer Nullbreite erstellt werden. Wir haben hingegen sichergestellt, dass sie einfach als ein Satz funktionieren. Im Idealfall können Sie zwei Symbole überlagern, die als ein Satz konzipiert wurden, und sie nehmen Gestalt an. Wir können dies vornehmen, um die Farbgebung im Code zu erlauben. Beispielsweise wurden U+EA3A und U+EA3B für den Badgestatus der Startkachel erstellt. Da diese bereits zentriert sind, kann die Kreisfüllung für unterschiedliche Status gefärbt werden.

Segoe UI Symbol beruhte zudem auf Glyphen mit einer „Nullbreite“ für die Überlagerung und Farbgebung. In diesem Beispiel könnte eine schwarze Umrandung (U+E006) auf einem roten Herz mit Nullbreite (U+E00B) gezeichnet werden.

![Verwenden von Glyphen mit der Breite 0](images/segoe-ui-symbol-layering.png)

Alle Glyphen in den **Segoe MDL2-Ressourcen** haben dieselbe feste Breite mit einer konsistenten Höhe und einem linken Ursprungspunkt. So können Überlagerungs- und Farbgebungseffekte erzielt werden, indem Glyphen direkt übereinander gezeichnet werden.

Viele der Symbole verfügen zudem über gespiegelte Formen, die in Sprachen verwendet werden können, in denen die Rechts-nach-Links-Ausrichtung verwendet wird, beispielsweise Arabisch, Farsi und Hebräisch.

Wenn Sie eine App in C#/VB/C++ und XAML entwickeln, können Sie mithilfe von [**Symbol enumeration**](https://msdn.microsoft.com/library/windows/apps/dn252842) anstelle der Unicode-ID Glyphen der Schriftart „Segoe MDL2 Assets“ angeben. Wenn Sie eine App in JavaScript und HTML entwickeln, können Sie mittels [**AppBarIcon enumeration**](https://msdn.microsoft.com/library/windows/apps/hh770557) anstelle der Unicode-ID Glyphen der Schriftart **Segoe MDL2 Assets** angeben.

Beachten Sie zudem, dass die Schriftart **Segoe MDL2-Ressourcen** viel mehr Symbole enthält, als hier gezeigt werden kann. Viele davon dienen speziellen Zwecken und werden für gewöhnlich nicht an anderer Stelle verwendet.

## <span id="Hearts"></span><span id="hearts"></span><span id="HEARTS"></span>Herzen


<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Code</th>
<th align="left">Symbol</th>
<th align="left">Enumeration</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>U+E006</p></td>
<td align="left"><img src="images/heartlegacy.png" alt="HeartLegacy" /></td>
<td align="left"><p>HeartLegacy</p></td>
<td align="left"><p>Umrandetes Herz</p></td>
</tr>
<tr class="even">
<td align="left"><p>U+E0A5</p></td>
<td align="left"><img src="images/heartfilllegacy.png" alt="HeartFillLegacy" /></td>
<td align="left"><p>HeartFillLegacy</p></td>
<td align="left"><p>Einfarbiges Herz</p></td>
</tr>
<tr class="odd">
<td align="left"><p>U+E007</p></td>
<td align="left"><img src="images/heartbrokenlegacy.png" alt="HeartBrokenLegacy" /></td>
<td align="left"><p>HeartBrokenLegacy</p></td>
<td align="left"><p>Gebrochenes Herz</p></td>
</tr>
<tr class="even">
<td align="left"><p>U+E00B</p></td>
<td align="left"><img src="images/heartfillzerowidthlegacy.png" alt="HeartFillZeroWidthLegacy" /></td>
<td align="left"><p>HeartFillZeroWidthLegacy</p></td>
<td align="left"><p>Einfarbiges Herz (Nullbreite)</p></td>
</tr>
<tr class="odd">
<td align="left"><p>U+E00C</p></td>
<td align="left"><img src="images/heartbrokenzerowidthlegacy.png" alt="HeartBrokenZeroWidthLegacy" /></td>
<td align="left"><p>HeartBrokenZeroWidthLegacy</p></td>
<td align="left"><p>Gebrochenes Herz (Nullbreite)</p></td>
</tr>
</tbody>
</table>

 

## <span id="Rating_stars"></span><span id="rating_stars"></span><span id="RATING_STARS"></span>Bewertungssterne


<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Code</th>
<th align="left">Symbol</th>
<th align="left">Enumeration</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">U+E224</td>
<td align="left"><img src="images/ratingstarlegacy.png" alt="RatingStarLegacy" /></td>
<td align="left">RatingStarLegacy</td>
<td align="left">Umrandeter Stern</td>
</tr>
<tr class="even">
<td align="left">U+E0B4</td>
<td align="left"><img src="images/ratingstarfilllegacy.png" alt="RatingStarFillLegacy" /></td>
<td align="left">RatingStarFillLegacy</td>
<td align="left">Einfarbiger Stern</td>
</tr>
<tr class="odd">
<td align="left">U+E00A</td>
<td align="left"><img src="images/ratingstarfillzerowidthlegacy.png" alt="RatingStarFillZeroWidthLegacy" /></td>
<td align="left">RatingStarFillZeroWidthLegacy</td>
<td align="left">Einfarbiger Stern (Nullbreite)</td>
</tr>
<tr class="even">
<td align="left">U+E082</td>
<td align="left"><img src="images/ratingstarfillreducedpaddinghtmllegacy.png" alt="RatingStarFillReducedPaddingHTMLLegacy" /></td>
<td align="left">RatingStarFillReducedPaddingHTMLLegacy</td>
<td align="left">Einfarbiger Stern (reduzierter Abstand für die Verwendung in HTML)</td>
</tr>
<tr class="odd">
<td align="left">U+E0B5</td>
<td align="left"><img src="images/ratingstarfillsmalllegacy.png" alt="RatingStarFillSmallLegacy" /></td>
<td align="left">RatingStarFillSmallLegacy</td>
<td align="left">Kleiner Stern</td>
</tr>
<tr class="even">
<td align="left">U+E7C6</td>
<td align="left"><img src="images/halfstarleft.png" alt="HalfStarLeft" /></td>
<td align="left">HalfStarLeft</td>
<td align="left">Halber Stern, linke Seite</td>
</tr>
<tr class="odd">
<td align="left"><p>U+E7C7</p></td>
<td align="left"><img src="images/halfstarright.png" alt="HalfStarRight" /></td>
<td align="left">HalfStarRight</td>
<td align="left">Halber Stern, rechte Seite</td>
</tr>
</tbody>
</table>

 

## <span id="Checkbox_components"></span><span id="checkbox_components"></span><span id="CHECKBOX_COMPONENTS"></span>Kontrollkästchenkomponenten


|        |                                                                                |                                 |                         |
|--------|--------------------------------------------------------------------------------|---------------------------------|-------------------------|
| Code   | Symbol                                                                         | Enumeration                            | Beschreibung             |
| U+E001 | ![checkmarklegacy](images/checkmarklegacy.png)                                 | CheckMarkLegacy                 | Häkchen              |
| U+E002 | ![checkboxfilllegacy](images/checkboxfilllegacy.png)                           | CheckboxFillLegacy              | Ausgefülltes Kontrollkästchen         |
| U+E003 | ![checkboxlegacy](images/checkboxlegacy.png)                                   | CheckboxLegacy                  | Kontrollkästchen                |
| U+E004 | ![checkboxindeterminatelegacy](images/checkboxindeterminatelegacy.png)         | CheckboxIndeterminateLegacy     | Unbestimmter Status     |
| U+E005 | ![checkboxcompositereversedlegacy](images/checkboxcompositereversedlegacy.png) | CheckboxCompositeReversedLegacy | Reserviert                |
| U+E008 | ![checkmarkzerowidthlegacy](images/checkmarkzerowidthlegacy.png)               | CheckMarkZeroWidthLegacy        | Häkchen (Nullbreite) |
| U+E009 | ![checkboxfillzerowidthlegacy](images/checkboxfillzerowidthlegacy.png)         | CheckboxFillZeroWidthLegacy     | Ausfüllen (Nullbreite)       |
| U+E0A2 | ![checkboxcompositelegacy](images/checkboxcompositelegacy.png)                 | CheckboxCompositeLegacy         | Komposition               |
| U+E739 | ![checkbox](images/checkbox.png)                                               | Checkbox                        | Kontrollkästchen                |
| U+E73A | ![checkboxcomposite](images/checkboxcomposite.png)                             | CheckboxComposite               | Verbundkontrollkästchen      |
| U+E73B | ![checkboxfill](images/checkboxfill.png)                                       | CheckboxFill                    | Ausgefülltes Kontrollkästchen         |
| U+E73C | ![checkboxindeterminate](images/checkboxindeterminate.png)                     | CheckboxIndeterminate           | Unbestimmter Status     |
| U+E73D | ![checkboxcompositereversed](images/checkboxcompositereversed.png)             | CheckboxCompositeReversed       | Umgekehrter Verbund      |
| U+E73E | ![checkmark](images/checkmark.png)                                             | CheckMark                       | Häkchen              |

 

## <span id="Miscellaneous"></span><span id="miscellaneous"></span><span id="MISCELLANEOUS"></span>Sonstiges


<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Code</th>
<th align="left">Symbol</th>
<th align="left">Enumeration</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>U+E134</p></td>
<td align="left"><img src="images/commentlegacy.png" alt="CommentLegacy" /></td>
<td align="left">CommentLegacy</td>
<td align="left">Kommentar</td>
</tr>
<tr class="even">
<td align="left"><p>U+E113</p></td>
<td align="left"><img src="images/favoritelegacy.png" alt="FavoriteLegacy" /></td>
<td align="left">FavoriteLegacy</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left"><p>U+E195</p></td>
<td align="left"><img src="images/unfavoritelegacy.png" alt="UnfavoriteLegacy" /></td>
<td align="left">Markierung als Favorit aufhebenLegacy</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>U+E734</p></td>
<td align="left"><img src="images/favoritestar.png" alt="FavoriteStar" /></td>
<td align="left">FavoriteStar</td>
<td align="left">Umrandeter Favorit</td>
</tr>
<tr class="odd">
<td align="left"><p>U+E735</p></td>
<td align="left"><img src="images/favoritestarfill.png" alt="FavoriteStarFill" /></td>
<td align="left">FavoriteStarFill</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>U+E8D9</p></td>
<td align="left"><img src="images/unfavorite.png" alt="Unfavorite" /></td>
<td align="left">Markierung als Favorit aufheben</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left"><p>U+E19F</p></td>
<td align="left"><img src="images/likelegacy.png" alt="LikeLegacy" /></td>
<td align="left">Gefällt mirLegacy</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>U+E19E</p></td>
<td align="left"><img src="images/dislikelegacy.png" alt="DislikeLegacy" /></td>
<td align="left">Gefällt nichtLegacy</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left"><p>U+E19D</p></td>
<td align="left"><img src="images/likedislikelegacy.png" alt="LikeDislikeLegacy" /></td>
<td align="left">Gefällt mirGefällt nichtLegacy</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>U+E116</p></td>
<td align="left"><img src="images/videolegacy.png" alt="VideoLegacy" /></td>
<td align="left">VideoLegacy</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left"><p>U+E714</p></td>
<td align="left"><img src="images/video.png" alt="Video" /></td>
<td align="left">Video</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>U+E20B</p></td>
<td align="left"><img src="images/mailmessagelegacy.png" alt="MailMessageLegacy" /></td>
<td align="left">MailMeldungLegacy</td>
<td align="left">E-Mail (Legacy)</td>
</tr>
<tr class="odd">
<td align="left"><p>U+E248</p></td>
<td align="left"><img src="images/replylegacy.png" alt="ReplyLegacy" /></td>
<td align="left">AntwortenLegacy</td>
<td align="left">Antworten</td>
</tr>
<tr class="even">
<td align="left"><p>U+E249</p></td>
<td align="left"><img src="images/favorite2legacy.png" alt="Favorite2Legacy" /></td>
<td align="left">Favorite2Legacy</td>
<td align="left">Gefüllter Favorit</td>
</tr>
<tr class="odd">
<td align="left"><p>U+E24E</p></td>
<td align="left"><img src="images/unfavorite2legacy.png" alt="Unfavorite2Legacy" /></td>
<td align="left">Markierung als Favorit aufheben2Legacy</td>
<td align="left">Markierung als Favorit aufheben</td>
</tr>
<tr class="even">
<td align="left"><p>U+E25A</p></td>
<td align="left"><img src="images/mobilecontactlegacy.png" alt="MobileContactLegacy" /></td>
<td align="left">MobileKontaktLegacy</td>
<td align="left">Mobiler Kontakt</td>
</tr>
<tr class="odd">
<td align="left"><p>U+E25B</p></td>
<td align="left"><img src="images/blockedlegacy.png" alt="BlockedLegacy" /></td>
<td align="left">BlockedLegacy</td>
<td align="left">Blockierter Kontakt</td>
</tr>
<tr class="even">
<td align="left"><p>U+E25C</p></td>
<td align="left"><img src="images/typingindicatorlegacy.png" alt="TypingIndicatorLegacy" /></td>
<td align="left">TypingIndicatorLegacy</td>
<td align="left">Eingabeindikator</td>
</tr>
<tr class="odd">
<td align="left"><p>U+E25D</p></td>
<td align="left"><img src="images/presencechickletvideolegacy.png" alt="PresenceChickletVideoLegacy" /></td>
<td align="left">PresenceChickletVideoLegacy</td>
<td align="left">Videopräsenz-Chicklet</td>
</tr>
<tr class="even">
<td align="left"><p>U+E25E</p></td>
<td align="left"><img src="images/presencechickletlegacy.png" alt="PresenceChickletLegacy" /></td>
<td align="left">PresenceChickletLegacy</td>
<td align="left">Präsenz-Chicklet</td>
</tr>
</tbody>
</table>

 

## <span id="Scroll_bar_arrows"></span><span id="scroll_bar_arrows"></span><span id="SCROLL_BAR_ARROWS"></span>Bildlaufleistenpfeile


| Code   | Symbol                                                                   | Enumeration                         |
|--------|--------------------------------------------------------------------------|------------------------------|
| U+E00E | ![scrollchevronleftlegacy](images/scrollchevronleftlegacy.png)           | ScrollChevronLeftLegacy      |
| U+E00F | ![scrollchevronrightlegacy](images/scrollchevronrightlegacy.png)         | ScrollChevronRightLegacy     |
| U+E010 | ![scrollchevronuplegacy](images/scrollchevronuplegacy.png)               | ScrollChevronUpLegacy        |
| U+E011 | ![scrollchevrondownlegacy](images/scrollchevrondownlegacy.png)           | ScrollChevronDownLegacy      |
| U+E016 | ![scrollchevronleftboldlegacy](images/scrollchevronleftboldlegacy.png)   | ScrollChevronLeftBoldLegacy  |
| U+E017 | ![scrollchevronrightboldlegacy](images/scrollchevronrightboldlegacy.png) | ScrollChevronRightBoldLegacy |
| U+E018 | ![scrollchevronupboldlegacy](images/scrollchevronupboldlegacy.png)       | ScrollChevronUpBoldLegacy    |
| U+E019 | ![scrollchevrondownboldlegacy](images/scrollchevrondownboldlegacy.png)   | ScrollChevronDownBoldLegacy  |

 

## <span id="Back_buttons"></span><span id="back_buttons"></span><span id="BACK_BUTTONS"></span>Zurück-Schaltflächen


Ältere Glyphen für „Zurück“-Schaltflächen stehen in zwei unterschiedlichen Größen zur Verfügung, sodass die Gewichtung des äußeren Rings in 20 Pt und 42 Pt einheitlich ist. Außerdem sind zusätzlich zwei neue proportionale Zurück-Schaltflächen verfügbar. Diese Glyphen können geschichtet werden.

| Code   | Symbol                                                                     | Enumeration                          | Beschreibung                               |
|--------|----------------------------------------------------------------------------|-------------------------------|-------------------------------------------|
| U+E0C4 | ![backbttnarrow20legacy](images/backbttnarrow20legacy.png)                 | BackBttnArrow20Legacy         | Zurück-Schaltfläche, Pfeil (20 pt)                   |
| U+E0A6 | ![backbttnarrow42legacy](images/backbttnarrow42legacy.png)                 | BackBttnArrow42Legacy         | Zurück-Schaltfläche, Pfeil (42 pt)                   |
| U+E0AD | ![backbttnmirroredarrow20legacy](images/backbttnmirroredarrow20legacy.png) | BackBttnMirroredArrow20Legacy | Gespiegelte Zurück-Schaltfläche, umgekehrter Pfeil (20 pt) |
| U+E0AB | ![backbttnmirroredarrow42legacy](images/backbttnmirroredarrow42legacy.png) | BackBttnMirroredArrow42Legacy | Gespiegelte Zurück-Schaltfläche, umgekehrter Pfeil (42 pt) |
| U+E830 | ![chromeback](images/chromeback.png)                                       | ChromeBack                    | Zurück-Schaltfläche (Chrome)                        |
| U+EA47 | ![chromebackmirrored](images/chromebackmirrored.png)                       | ChromeBackMirrored            | Gespiegelte Zurück-Schaltfläche (Chrome)               |

 

## <span id="Back_arrows_for_HTML"></span><span id="back_arrows_for_html"></span><span id="BACK_ARROWS_FOR_HTML"></span>Zurück-Pfeile für HTML


Fügen Sie zusätzlichen Code hinzu, um Kreise um diese Glyphen zu erstellen.

| Code   | Symbol                                                         | Enumeration                    | Beschreibung                |
|--------|----------------------------------------------------------------|-------------------------|----------------------------|
| U+E0D5 | ![arrowhtmllegacy](images/arrowhtmllegacy.png)                 | ArrowHTMLLegacy         | Pfeil für die Verwendung von HTML         |
| U+E0AE | ![arrowhtmlmirroredlegacy](images/arrowhtmlmirroredlegacy.png) | ArrowHTMLMirroredLegacy | Gespiegelte Version von U+E0D5 |

 

## <span id="AppBar_glyphs"></span><span id="appbar_glyphs"></span><span id="APPBAR_GLYPHS"></span>AppBar-Glyphen


Verwenden Sie Glyphen der folgenden Liste für eine [**AppBar**](https://msdn.microsoft.com/library/windows/apps/br229670). Üblicherweise werden sie anhand ihrer Enumerationsnamen bezeichnet. Zudem sind sie als 20 px x 20 px große Symbole ohne Kreis entworfen.

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Code</th>
<th align="left">Symbol</th>
<th align="left">Enumeration</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">U+E8FB</td>
<td align="left"><img src="images/accept.png" alt="Accept" /></td>
<td align="left">Akzeptieren</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E910</td>
<td align="left"><img src="images/accounts.png" alt="Accounts" /></td>
<td align="left">Konten</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E710</td>
<td align="left"><img src="images/add.png" alt="Add" /></td>
<td align="left">Hinzufügen</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8FA</td>
<td align="left"><img src="images/addfriend.png" alt="Addfriend" /></td>
<td align="left">AddFriend</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E7EF</td>
<td align="left"><img src="images/admin.png" alt="Admin" /></td>
<td align="left">Administrator</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8E3</td>
<td align="left"><img src="images/aligncenter.png" alt="Aligncenter" /></td>
<td align="left">AlignCenter</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8E4</td>
<td align="left"><img src="images/alignleft.png" alt="Alignleft" /></td>
<td align="left">AlignLeft</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8E2</td>
<td align="left"><img src="images/alignright.png" alt="Alignright" /></td>
<td align="left">Alignright</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E71D</td>
<td align="left"><img src="images/allapps.png" alt="Allapps" /></td>
<td align="left">AllApps</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E723</td>
<td align="left"><img src="images/attach.png" alt="Attach" /></td>
<td align="left">Anfügen</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A2</td>
<td align="left"><img src="images/attachcamera.png" alt="Attachcamera" /></td>
<td align="left">AttachCamera</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8D6</td>
<td align="left"><img src="images/audio.png" alt="Audio" /></td>
<td align="left">Audio</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E72B</td>
<td align="left"><img src="images/back.png" alt="Back" /></td>
<td align="left">Zurück</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E73F</td>
<td align="left"><img src="images/backtowindow.png" alt="Backtowindow" /></td>
<td align="left">BackToWindow</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8F8</td>
<td align="left"><img src="images/blockcontact.png" alt="Blockcontact" /></td>
<td align="left">BlockContact</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8DD</td>
<td align="left"><img src="images/bold.png" alt="Bold" /></td>
<td align="left">Fett</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A4</td>
<td align="left"><img src="images/bookmarks.png" alt="Bookmarks" /></td>
<td align="left">Lesezeichen</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7C5</td>
<td align="left"><img src="images/browsephotos.png" alt="Browsephotos" /></td>
<td align="left">BrowsePhotos</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8FD</td>
<td align="left"><img src="images/bulletedlist.png" alt="Bulletedlist" /></td>
<td align="left">BulletedList</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8EF</td>
<td align="left"><img src="images/calculator.png" alt="Calculator" /></td>
<td align="left">Rechner</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E787</td>
<td align="left"><img src="images/calendar.png" alt="Calendar" /></td>
<td align="left">Kalender</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8BF</td>
<td align="left"><img src="images/calendarday.png" alt="Calendarday" /></td>
<td align="left">KalenderDay</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8F5</td>
<td align="left"><img src="images/calendarreply.png" alt="Calendarreply" /></td>
<td align="left">KalenderAntworten</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C0</td>
<td align="left"><img src="images/calendarweek.png" alt="Calendarweek" /></td>
<td align="left">KalenderWeek</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E722</td>
<td align="left"><img src="images/camera.png" alt="Camera" /></td>
<td align="left">Kamera</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E711</td>
<td align="left"><img src="images/cancel.png" alt="Cancel" /></td>
<td align="left">Abbrechen</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8BA</td>
<td align="left"><img src="images/caption.png" alt="Caption" /></td>
<td align="left">Aufnahme</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7F0</td>
<td align="left"><img src="images/cc.png" alt="CC" /></td>
<td align="left">CC</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8EA</td>
<td align="left"><img src="images/cellphone.png" alt="Cellphone" /></td>
<td align="left">Mobiltelefon</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C1</td>
<td align="left"><img src="images/characters.png" alt="Characters" /></td>
<td align="left">Zeichen</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E894</td>
<td align="left"><img src="images/clear.png" alt="Clear" /></td>
<td align="left">Deutlichkeit</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8E6</td>
<td align="left"><img src="images/clearselection.png" alt="Clearselection" /></td>
<td align="left">ClearSelection</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E89F</td>
<td align="left"><img src="images/closepane.png" alt="Closepane" /></td>
<td align="left">ClosePane</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E753</td>
<td align="left"><img src="images/cloud.png" alt="Cloud" /></td>
<td align="left">Cloud</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E90A</td>
<td align="left"><img src="images/comment.png" alt="Comment" /></td>
<td align="left">Kommentar</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E77B</td>
<td align="left"><img src="images/contact.png" alt="Contact" /></td>
<td align="left">Kontakt</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8D4</td>
<td align="left"><img src="images/contact2.png" alt="Contact2" /></td>
<td align="left">Contact2</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E779</td>
<td align="left"><img src="images/contactinfo.png" alt="Contactinfo" /></td>
<td align="left">KontaktInfo</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8CF</td>
<td align="left"><img src="images/contactpresence.png" alt="Contactpresence" /></td>
<td align="left">KontaktPresence</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C8</td>
<td align="left"><img src="images/copy.png" alt="Copy" /></td>
<td align="left">Kopieren</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E7A8</td>
<td align="left"><img src="images/crop.png" alt="Crop" /></td>
<td align="left">Zuschneiden</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C6</td>
<td align="left"><img src="images/cut.png" alt="Cut" /></td>
<td align="left">Ausschneiden</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E74D</td>
<td align="left"><img src="images/delete.png" alt="Delete" /></td>
<td align="left">Löschen</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8F0</td>
<td align="left"><img src="images/directions.png" alt="Directions" /></td>
<td align="left">Richtungen</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8D8</td>
<td align="left"><img src="images/disableupdates.png" alt="Disableupdates" /></td>
<td align="left">DisableUpdates</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8CD</td>
<td align="left"><img src="images/disconnectdrive.png" alt="Disconnectdrive" /></td>
<td align="left">DisconnectDrive</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8E0</td>
<td align="left"><img src="images/dislike.png" alt="Dislike" /></td>
<td align="left">Gefällt nicht</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E90E</td>
<td align="left"><img src="images/dockbottom.png" alt="Dockbottom" /></td>
<td align="left">DockBottom</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E90C</td>
<td align="left"><img src="images/dockleft.png" alt="Dockleft" /></td>
<td align="left">DockLeft</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E90D</td>
<td align="left"><img src="images/dockright.png" alt="Dockright" /></td>
<td align="left">DockRight</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A5</td>
<td align="left"><img src="images/document.png" alt="Document" /></td>
<td align="left">Document</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E896</td>
<td align="left"><img src="images/download.png" alt="Download" /></td>
<td align="left">Download</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E70F</td>
<td align="left"><img src="images/edit.png" alt="Edit" /></td>
<td align="left">Edit</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E899</td>
<td align="left"><img src="images/emoji.png" alt="Emoji" /></td>
<td align="left">Emoji</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E76E</td>
<td align="left"><img src="images/emoji2.png" alt="Emoji2" /></td>
<td align="left">Emoji2</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E728</td>
<td align="left"><img src="images/favoritelist.png" alt="Favoritelist" /></td>
<td align="left">FavoriteListe</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E734</td>
<td align="left"><img src="images/favoritestar.png" alt="Favoritestar" /></td>
<td align="left">FavoriteStar</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E735</td>
<td align="left"><img src="images/favoritestarfill.png" alt="Favoritestarfill" /></td>
<td align="left">FavoriteStarFill</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E71C</td>
<td align="left"><img src="images/filter.png" alt="Filter" /></td>
<td align="left">Filtern</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E11A</td>
<td align="left"><img src="images/findlegacy.png" alt="Findlegacy" /></td>
<td align="left">FindLegacy</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E7C1</td>
<td align="left"><img src="images/flag.png" alt="Flag" /></td>
<td align="left">Kennzeichen</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8B7</td>
<td align="left"><img src="images/folder.png" alt="Folder" /></td>
<td align="left">Ordner</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8D2</td>
<td align="left"><img src="images/font.png" alt="Font" /></td>
<td align="left">Schriftart</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8D3</td>
<td align="left"><img src="images/fontcolor.png" alt="Fontcolor" /></td>
<td align="left">Schriftartfarbe</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8E7</td>
<td align="left"><img src="images/fontdecrease.png" alt="Fontdecrease" /></td>
<td align="left">SchriftartDecrease</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8E8</td>
<td align="left"><img src="images/fontincrease.png" alt="Fontincrease" /></td>
<td align="left">FontIncrease</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8E9</td>
<td align="left"><img src="images/fontsize.png" alt="Fontsize" /></td>
<td align="left">FontSize</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E72A</td>
<td align="left"><img src="images/forward.png" alt="Forward" /></td>
<td align="left">Vorwärts</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E908</td>
<td align="left"><img src="images/fourbars.png" alt="Fourbars" /></td>
<td align="left">FourBars</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E740</td>
<td align="left"><img src="images/fullscreen.png" alt="Fullscreen" /></td>
<td align="left">FullScreen</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E774</td>
<td align="left"><img src="images/globe.png" alt="Globe" /></td>
<td align="left">Globe</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8AD</td>
<td align="left"><img src="images/go.png" alt="Go" /></td>
<td align="left">Los</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8FC</td>
<td align="left"><img src="images/gotostart.png" alt="Gotostart" /></td>
<td align="left">GoToStart</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8D1</td>
<td align="left"><img src="images/gototoday.png" alt="Gototoday" /></td>
<td align="left">GoToToday</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E778</td>
<td align="left"><img src="images/hangup.png" alt="Hangup" /></td>
<td align="left">Auflegen</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E897</td>
<td align="left"><img src="images/help.png" alt="Help" /></td>
<td align="left">Hilfe</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8C5</td>
<td align="left"><img src="images/hidebcc.png" alt="Hidebcc" /></td>
<td align="left">HideBCC</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7E6</td>
<td align="left"><img src="images/highlight.png" alt="Highlight" /></td>
<td align="left">Hervorheben</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E80F</td>
<td align="left"><img src="images/home.png" alt="Home" /></td>
<td align="left">Startseite</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8B5</td>
<td align="left"><img src="images/import.png" alt="Import" /></td>
<td align="left">Importieren</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8B6</td>
<td align="left"><img src="images/importall.png" alt="Importall" /></td>
<td align="left">ImportAll</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C9</td>
<td align="left"><img src="images/important.png" alt="Important" /></td>
<td align="left">Wichtig</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8DB</td>
<td align="left"><img src="images/italic.png" alt="Italic" /></td>
<td align="left">Kursiv</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E765</td>
<td align="left"><img src="images/keyboardclassic.png" alt="Keyboardclassic" /></td>
<td align="left">KeyboardClassic</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E89B</td>
<td align="left"><img src="images/leavechat.png" alt="Leavechat" /></td>
<td align="left">LeaveChat</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8F1</td>
<td align="left"><img src="images/library.png" alt="Library" /></td>
<td align="left">Bibliothek</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8E1</td>
<td align="left"><img src="images/like.png" alt="Like" /></td>
<td align="left">Like</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8DF</td>
<td align="left"><img src="images/likedislike.png" alt="Likedislike" /></td>
<td align="left">LikeDislike</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E71B</td>
<td align="left"><img src="images/link.png" alt="Link" /></td>
<td align="left">Link</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+EA37</td>
<td align="left"><img src="images/list.png" alt="List" /></td>
<td align="left">Liste</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E81D</td>
<td align="left"><img src="images/location.png" alt="Location" /></td>
<td align="left">Ort</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E715</td>
<td align="left"><img src="images/mail.png" alt="Mail" /></td>
<td align="left">Mail</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A8</td>
<td align="left"><img src="images/mailfill.png" alt="Mailfill" /></td>
<td align="left">MailFill</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E89C</td>
<td align="left"><img src="images/mailforward.png" alt="Mailforward" /></td>
<td align="left">MailVorwärts</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8CA</td>
<td align="left"><img src="images/mailreply.png" alt="Mailreply" /></td>
<td align="left">MailReply</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C2</td>
<td align="left"><img src="images/mailreplyall.png" alt="Mailreplyall" /></td>
<td align="left">MailReplyAll</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E912</td>
<td align="left"><img src="images/manage.png" alt="Manage" /></td>
<td align="left">Verwalten</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8CE</td>
<td align="left"><img src="images/mapdrive.png" alt="Mapdrive" /></td>
<td align="left">MapDrive</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E707</td>
<td align="left"><img src="images/mappin.png" alt="Mappin" /></td>
<td align="left">Zuordnen</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E77C</td>
<td align="left"><img src="images/memo.png" alt="Memo" /></td>
<td align="left">Memo</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8BD</td>
<td align="left"><img src="images/message.png" alt="Message" /></td>
<td align="left">Message</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E720</td>
<td align="left"><img src="images/microphone.png" alt="Microphone" /></td>
<td align="left">Mikrofon</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E712</td>
<td align="left"><img src="images/more.png" alt="More" /></td>
<td align="left">Mehr</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8DE</td>
<td align="left"><img src="images/movetofolder.png" alt="Movetofolder" /></td>
<td align="left">MoveToFolder</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E90B</td>
<td align="left"><img src="images/musicinfo.png" alt="Musicinfo" /></td>
<td align="left">MusicInfo</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E74F</td>
<td align="left"><img src="images/mute.png" alt="Mute" /></td>
<td align="left">Mute</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8F4</td>
<td align="left"><img src="images/newfolder.png" alt="Newfolder" /></td>
<td align="left">NewFolder</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E78B</td>
<td align="left"><img src="images/newwindow.png" alt="Newwindow" /></td>
<td align="left">NewWindow</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E893</td>
<td align="left"><img src="images/next.png" alt="Next" /></td>
<td align="left">Next</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E905</td>
<td align="left"><img src="images/onebar.png" alt="Onebar" /></td>
<td align="left">OneBar</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8E5</td>
<td align="left"><img src="images/openfile.png" alt="Openfile" /></td>
<td align="left">OpenFile</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8DA</td>
<td align="left"><img src="images/openlocal.png" alt="Openlocal" /></td>
<td align="left">OpenLocal</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A0</td>
<td align="left"><img src="images/openpane.png" alt="Openpane" /></td>
<td align="left">OpenPane</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7AC</td>
<td align="left"><img src="images/openwith.png" alt="Openwith" /></td>
<td align="left">OpenWith</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8B4</td>
<td align="left"><img src="images/orientation.png" alt="Orientation" /></td>
<td align="left">Ausrichtung</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7EE</td>
<td align="left"><img src="images/otheruser.png" alt="Otheruser" /></td>
<td align="left">OtherUser</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E1CE</td>
<td align="left"><img src="images/outlinestarlegacy.png" alt="Outlinestarlegacy" /></td>
<td align="left">OutlineStarLegacy</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7C3</td>
<td align="left"><img src="images/page.png" alt="Page" /></td>
<td align="left">Page</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E77F</td>
<td align="left"><img src="images/paste.png" alt="Paste" /></td>
<td align="left">Einfügen</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E769</td>
<td align="left"><img src="images/pause.png" alt="Pause" /></td>
<td align="left">Pause</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E716</td>
<td align="left"><img src="images/people.png" alt="People" /></td>
<td align="left">Kontakte</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8D7</td>
<td align="left"><img src="images/permissions.png" alt="Permissions" /></td>
<td align="left">Berechtigungen</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E717</td>
<td align="left"><img src="images/phone.png" alt="Phone" /></td>
<td align="left">Telefon</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E780</td>
<td align="left"><img src="images/phonebook.png" alt="Phonebook" /></td>
<td align="left">TelefonBook</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E718</td>
<td align="left"><img src="images/pin.png" alt="Pin" /></td>
<td align="left">Anheften</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E768</td>
<td align="left"><img src="images/play.png" alt="Play" /></td>
<td align="left">Wiedergeben</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8F3</td>
<td align="left"><img src="images/postupdate.png" alt="Postupdate" /></td>
<td align="left">PostNach obendate</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8FF</td>
<td align="left"><img src="images/preview.png" alt="Preview" /></td>
<td align="left">Vorschau</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A1</td>
<td align="left"><img src="images/previewlink.png" alt="Previewlink" /></td>
<td align="left">VorschauLink</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E892</td>
<td align="left"><img src="images/previous.png" alt="Previous" /></td>
<td align="left">Zurück</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8D0</td>
<td align="left"><img src="images/priority.png" alt="Priority" /></td>
<td align="left">Priorität</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8A6</td>
<td align="left"><img src="images/protecteddocument.png" alt="Protecteddocument" /></td>
<td align="left">ProtectedDokument</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8C3</td>
<td align="left"><img src="images/read.png" alt="Read" /></td>
<td align="left">Lesen</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7A6</td>
<td align="left"><img src="images/redo.png" alt="Redo" /></td>
<td align="left">Wiederholen</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E72C</td>
<td align="left"><img src="images/refresh.png" alt="Refresh" /></td>
<td align="left">Aktualisieren</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8AF</td>
<td align="left"><img src="images/remote.png" alt="Remote" /></td>
<td align="left">Remote</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E738</td>
<td align="left"><img src="images/remove.png" alt="Remove" /></td>
<td align="left">Entfernen</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8AC</td>
<td align="left"><img src="images/rename.png" alt="Rename" /></td>
<td align="left">Umbenennen</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E90F</td>
<td align="left"><img src="images/repair.png" alt="Repair" /></td>
<td align="left">Reparatur</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8EE</td>
<td align="left"><img src="images/repeatall.png" alt="Repeatall" /></td>
<td align="left">RepeatAll</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8ED</td>
<td align="left"><img src="images/repeatone.png" alt="Repeatone" /></td>
<td align="left">RepeatOne</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E730</td>
<td align="left"><img src="images/reporthacked.png" alt="Reporthacked" /></td>
<td align="left">ReportHacked</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8EB</td>
<td align="left"><img src="images/reshare.png" alt="Reshare" /></td>
<td align="left">Erneut freigeben</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7AD</td>
<td align="left"><img src="images/rotate.png" alt="Rotate" /></td>
<td align="left">Drehen</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E89E</td>
<td align="left"><img src="images/rotatecamera.png" alt="Rotatecamera" /></td>
<td align="left">RotateCamera</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E74E</td>
<td align="left"><img src="images/save.png" alt="Save" /></td>
<td align="left">Speichern</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E78C</td>
<td align="left"><img src="images/savelocal.png" alt="Savelocal" /></td>
<td align="left">SaveLocal</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8FE</td>
<td align="left"><img src="images/scan.png" alt="Scan" /></td>
<td align="left">Scan</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8B3</td>
<td align="left"><img src="images/selectall.png" alt="Selectall" /></td>
<td align="left">SelectAll</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E724</td>
<td align="left"><img src="images/send.png" alt="Send" /></td>
<td align="left">Senden</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E7B5</td>
<td align="left"><img src="images/setlockscreen.png" alt="Setlockscreen" /></td>
<td align="left">SetLockScreen</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E97B</td>
<td align="left"><img src="images/settile.png" alt="Settile" /></td>
<td align="left">SetTile</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E713</td>
<td align="left"><img src="images/settings.png" alt="Settings" /></td>
<td align="left">Einstellungen</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E72D</td>
<td align="left"><img src="images/share.png" alt="Share" /></td>
<td align="left">Share</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E719</td>
<td align="left"><img src="images/shop.png" alt="Shop" /></td>
<td align="left">Shop</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C4</td>
<td align="left"><img src="images/showbcc.png" alt="Showbcc" /></td>
<td align="left">ShowBCC</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8BC</td>
<td align="left"><img src="images/showresults.png" alt="Showresults" /></td>
<td align="left">ShowResults</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8B1</td>
<td align="left"><img src="images/shuffle.png" alt="Shuffle" /></td>
<td align="left">Zufällig anordnen</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E786</td>
<td align="left"><img src="images/slideshow.png" alt="Slideshow" /></td>
<td align="left">Diashow</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E1CF</td>
<td align="left"><img src="images/solidstarlegacy.png" alt="Solidstarlegacy" /></td>
<td align="left">SolidStarLegacy</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8CB</td>
<td align="left"><img src="images/sort.png" alt="Sort" /></td>
<td align="left">Sortieren</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E71A</td>
<td align="left"><img src="images/stop.png" alt="Stop" /></td>
<td align="left">Stop</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E913</td>
<td align="left"><img src="images/street.png" alt="Street" /></td>
<td align="left">Straße</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8AB</td>
<td align="left"><img src="images/switch.png" alt="Switch" /></td>
<td align="left">Switch</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8F9</td>
<td align="left"><img src="images/switchapps.png" alt="Switchapps" /></td>
<td align="left">SwitchApps</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E895</td>
<td align="left"><img src="images/sync.png" alt="Sync" /></td>
<td align="left">Synchronisieren</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8F7</td>
<td align="left"><img src="images/syncfolder.png" alt="Syncfolder" /></td>
<td align="left">SyncFolder</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8EC</td>
<td align="left"><img src="images/tag.png" alt="Tag" /></td>
<td align="left">Tag</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E907</td>
<td align="left"><img src="images/threebars.png" alt="Threebars" /></td>
<td align="left">ThreeBars</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7C9</td>
<td align="left"><img src="images/touchpointer.png" alt="Touchpointer" /></td>
<td align="left">TouchPointer</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E78A</td>
<td align="left"><img src="images/trim.png" alt="Trim" /></td>
<td align="left">Kürzen</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E906</td>
<td align="left"><img src="images/twobars.png" alt="Twobars" /></td>
<td align="left">TwoBars</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E89A</td>
<td align="left"><img src="images/twopage.png" alt="Twopage" /></td>
<td align="left">TwoPage</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8DC</td>
<td align="left"><img src="images/underline.png" alt="Underline" /></td>
<td align="left">Unterstreichen</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E7A7</td>
<td align="left"><img src="images/undo.png" alt="Undo" /></td>
<td align="left">Rückgängig machen</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8D9</td>
<td align="left"><img src="images/unfavorite.png" alt="Unfavorite" /></td>
<td align="left">UnFavorite</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E77A</td>
<td align="left"><img src="images/unpin.png" alt="Unpin" /></td>
<td align="left">UnPin</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8F6</td>
<td align="left"><img src="images/unsyncfolder.png" alt="Unsyncfolder" /></td>
<td align="left">UnSyncFolder</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E74A</td>
<td align="left"><img src="images/up.png" alt="Up" /></td>
<td align="left">Nach oben</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E898</td>
<td align="left"><img src="images/upload.png" alt="Upload" /></td>
<td align="left">Hochladen</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8AA</td>
<td align="left"><img src="images/videochat.png" alt="Videochat" /></td>
<td align="left">VideoChat</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E890</td>
<td align="left"><img src="images/view.png" alt="View" /></td>
<td align="left">Ansicht</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A9</td>
<td align="left"><img src="images/viewall.png" alt="Viewall" /></td>
<td align="left">ViewAll</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E767</td>
<td align="left"><img src="images/volume.png" alt="Volume" /></td>
<td align="left">Volume</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8B8</td>
<td align="left"><img src="images/webcam.png" alt="Webcam" /></td>
<td align="left">Webcam</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E909</td>
<td align="left"><img src="images/world.png" alt="World" /></td>
<td align="left">Welt</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E904</td>
<td align="left"><img src="images/zerobars.png" alt="Zerobars" /></td>
<td align="left">ZeroBars</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E71E</td>
<td align="left"><img src="images/zoom.png" alt="Zoom" /></td>
<td align="left">Zoom</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A3</td>
<td align="left"><img src="images/zoomin.png" alt="Zoomin" /></td>
<td align="left">ZoomIn</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E71F</td>
<td align="left"><img src="images/zoomout.png" alt="Zoomout" /></td>
<td align="left">ZoomOut</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

## <span id="Battery_icons"></span><span id="battery_icons"></span><span id="BATTERY_ICONS"></span>Akkusymbole


| Code | Symbol                                             | Enumeration              | Code | Symbol                                                  | Enumeration                |
|------|----------------------------------------------------|-------------------|------|---------------------------------------------------------|---------------------|
| E996 | ![batteryunknown](images/batteryunknown.png)       | BatteryUnknown    | EC02 | ![mobbatteryunknown](images/mobbatteryunknown.png)      | MobBatteryUnknown   |
| E850 | ![battery0](images/battery0.png)                   | Battery0          | EBA0 | ![mobbattery0](images/mobbattery0.png)                  | MobBattery0         |
| E851 | ![battery1](images/battery1.png)                   | Battery1          | EBA1 | ![mobbattery1](images/mobbattery1.png)                  | MobBattery1         |
| E852 | ![battery2](images/battery2.png)                   | Battery2          | EBA2 | ![mobbattery2](images/mobbattery2.png)                  | MobBattery2         |
| E853 | ![battery3](images/battery3.png)                   | Battery3          | EBA3 | ![mobbattery3](images/mobbattery3.png)                  | MobBattery3         |
| E854 | ![battery4](images/battery4.png)                   | Battery4          | EBA4 | ![mobbattery4](images/mobbattery4.png)                  | MobBattery4         |
| E855 | ![battery5](images/battery5.png)                   | Battery5          | EBA5 | ![mobbattery5](images/mobbattery5.png)                  | MobBattery5         |
| E856 | ![battery6](images/battery6.png)                   | Battery6          | EBA6 | ![mobbattery6](images/mobbattery6.png)                  | MobBattery6         |
| E857 | ![battery7](images/battery7.png)                   | Battery7          | EBA7 | ![mobbattery7](images/mobbattery7.png)                  | MobBattery7         |
| E858 | ![battery8](images/battery8.png)                   | Battery8          | EBA8 | ![mobbattery8](images/mobbattery8.png)                  | MobBattery8         |
| E859 | ![battery9](images/battery9.png)                   | Battery9          | EBA9 | ![mobbattery9](images/mobbattery9.png)                  | MobBattery9         |
| E83F | ![battery10](images/battery10.png)                 | Battery10         | EBA0 | ![mobbatter10](images/mobbattery10.png)                 | MobBatter10         |
| E85A | ![batterycharging0](images/batterycharging0.png)   | BatteryCharging0  | EBAB | ![mobbatterycharging0](images/mobbatterycharging0.png)  | MobBatteryCharging0 |
| E85B | ![batterycharging1](images/batterycharging1.png)   | BatteryCharging1  | EBAC | ![mobbatterycharging1](images/mobbatterycharging1.png)  | MobBatteryCharging1 |
| E85C | ![batterycharging2](images/batterycharging2.png)   | BatteryCharging2  | EBAD | ![mobbatterycharging2](images/mobbatterycharging2.png)  | MobBatteryCharging2 |
| E85D | ![batterycharging3](images/batterycharging3.png)   | BatteryCharging3  | EBAE | ![mobbatterycharging3](images/mobbatterycharging3.png)  | MobBatteryCharging3 |
| E85E | ![batterycharging4](images/batterycharging4.png)   | BatteryCharging4  | EBAF | ![mobbatterycharging4](images/mobbatterycharging4.png)  | MobBatteryCharging4 |
| E85F | ![batterycharging5](images/batterycharging5.png)   | BatteryCharging5  | EBB0 | ![mobbatterycharging5](images/mobbatterycharging5.png)  | MobBatteryCharging5 |
| E860 | ![batterycharging6](images/batterycharging6.png)   | BatteryCharging6  | EBB1 | ![mobbatterycharging6](images/mobbatterycharging6.png)  | MobBatteryCharging6 |
| E861 | ![batterycharging7](images/batterycharging7.png)   | BatteryCharging7  | EBB2 | ![mobbatterycharging7](images/mobbatterycharging7.png)  | MobBatteryCharging7 |
| E862 | ![batterycharging8](images/batterycharging8.png)   | BatteryCharging8  | EBB3 | ![mobbatterycharging8](images/mobbatterycharging8.png)  | MobBatteryCharging8 |
| E83E | ![batterycharging9](images/batterycharging9.png)   | BatteryCharging9  | EBB4 | ![mobbatterycharging9](images/mobbatterycharging9.png)  | MobBatteryCharging9 |
| ea93 | ![batterycharging10](images/batterycharging10.png) | BatteryCharging10 | EBB5 | ![mobbatterychargin10](images/mobbatterycharging10.png) | MobBatteryChargin10 |
| E863 | ![batterysaver0](images/batterysaver0.png)         | BatterySaver0     | EBB6 | ![mobbatterysaver0](images/mobbatterysaver0.png)        | MobBatterySaver0    |
| E864 | ![batterysaver1](images/batterysaver1.png)         | BatterySaver1     | EBB7 | ![mobbatterysaver1](images/mobbatterysaver1.png)        | MobBatterySaver1    |
| E865 | ![batterysaver2](images/batterysaver2.png)         | BatterySaver2     | EBB8 | ![mobbatterysaver2](images/mobbatterysaver2.png)        | MobBatterySaver2    |
| E866 | ![batterysaver3](images/batterysaver3.png)         | BatterySaver3     | EBB9 | ![mobbatterysaver3](images/mobbatterysaver3.png)        | MobBatterySaver3    |
| E867 | ![batterysaver4](images/batterysaver4.png)         | BatterySaver4     | EBBA | ![mobbatterysaver4](images/mobbatterysaver4.png)        | MobBatterySaver4    |
| E868 | ![batterysaver5](images/batterysaver5.png)         | BatterySaver5     | EBBB | ![mobbatterysaver5](images/mobbatterysaver5.png)        | MobBatterySaver5    |
| E869 | ![batterysaver6](images/batterysaver6.png)         | BatterySaver6     | EBBC | ![mobbatterysaver6](images/mobbatterysaver6.png)        | MobBatterySaver6    |
| E86A | ![batterysaver7](images/batterysaver7.png)         | BatterySaver7     | EBBD | ![mobbatterysaver7](images/mobbatterysaver7.png)        | MobBatterySaver7    |
| E86B | ![batterysaver8](images/batterysaver8.png)         | BatterySaver8     | EBBE | ![mobbatterysaver8](images/mobbatterysaver8.png)        | MobBatterySaver8    |
| EA94 | ![batterysaver9](images/batterysaver9.png)         | BatterySaver9     | EBBF | ![mobbatterysaver9](images/mobbatterysaver9.png)        | MobBatterySaver9    |
| EA95 | ![batterysaver10](images/batterysaver10.png)       | BatterySaver10    | EBC0 | ![mobbatterysaver10](images/mobbatterysaver10.png)      | MobBatterySaver10   |

 

## <span id="related_topics"></span>Verwandte Themen


**Für Designer**
* [Richtlinien für Schriftarten](fonts.md)
* [W3C für Sprachen, die von rechts nach links (RTL) geschrieben werden?](http://www.i18nguy.com/temp/rtl.mdl)
            
          
            **Für Entwickler (XAML)**
* [**Symbol-Enumeration**](https://msdn.microsoft.com/library/windows/apps/dn252842)


 






<!--HONumber=May16_HO2-->


