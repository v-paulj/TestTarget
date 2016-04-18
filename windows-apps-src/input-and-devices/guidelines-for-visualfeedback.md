---
Description: Zeigen Sie Benutzern mit visuellem Feedback, wenn ihre Interaktionen mit einer Windows Store-App ermittelt, interpretiert und behandelt werden.
title: Visuelles Feedback
ms.assetid: bf2f3672-95f0-4c8c-9a72-0934f2d3b767
label: Visual feedback
template: detail.hbs
---

# Richtlinien für visuelles Feedback


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Zeigen Sie Benutzern mit visuellem Feedback, wenn ihre Interaktionen ermittelt, interpretiert und behandelt werden. Visuelles Feedback ist hilfreich für Benutzer und kann sie zur Interaktion ermutigen. Es weist auf erfolgreiche Interaktionen hin, was für den Benutzer das Gefühl der Kontrolle verstärkt. Darüber hinaus informiert es über den Systemstatus und verringert Fehler.

**Wichtige APIs**

-   [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)
-   [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)


## <span id="Dos_and_don_ts"></span><span id="dos_and_don_ts"></span><span id="DOS_AND_DON_TS"></span>Empfohlene und nicht empfohlene Vorgehensweisen


-   Stellen Sie unabhängig von der Kürze der Berührung immer ein visuelles Feedback bereit. So kann der Benutzer:
    -   Sicher sein, dass der Touchscreen funktioniert.
    -   Feststellen, welches Ziel die Fingereingabe unterstützt oder reagiert.
    -   Sehen, ob er das gewünschte Ziel verfehlt hat.
-   Zeigen Sie für alle Interaktionsereignisse Feedback sofort an.
-   Stellen Sie Feedback bereit, das unaufdringliche, intuitive Hinweise liefert, ohne den Benutzer abzulenken.
-   Stellen Sie sicher, dass Fingereingabeziele während jeder Manipulation an der Fingerspitze "haften bleiben".
-   Ermöglichen Sie die Auswahl von Elementen mit der Streifbewegung, wenn das Verschieben auf eine Richtung beschränkt ist.
-   Verwenden Sie keine Fingereingabevisualisierungen, wenn diese die Verwendung der App behindern können. Weitere Informationen finden Sie unter [**ShowGestureFeedback**](https://msdn.microsoft.com/library/windows/apps/br241969).
-   Zeigen Sie nur dann Feedback an, wenn dies absolut notwendig ist. Sorgen Sie dafür, dass die Benutzeroberfläche übersichtlich bleibt. Zeigen Sie nur dann visuelles Feedback an, wenn die darin enthaltenen Informationen sonst nirgends verfügbar sind. Zeigen Sie niemals QuickInfos an, die bereits sichtbaren Text wiederholen. QuickInfos sollten bestimmten Situationen vorbehalten sein, z. B. abgeschnittener Text (Text mit Auslassungspunkten), der nicht angezeigt wird, wenn das Element ausgewählt ist, oder Fälle, in denen zusätzliche Informationen für das Verständnis oder die Verwendung Ihrer App erforderlich sind.
-   Verwenden Sie die Gedrückthaltebewegung nur für die Informationsbenutzeroberfläche.
    **Wichtig**  Gedrückthalten kann für die Auswahl verwendet werden, wenn sowohl horizontales als auch vertikales Verschieben aktiviert ist.

     

-   Passen Sie das visuelle Feedbackverhalten der integrierten Windows 8-Gesten nicht an. Eine uneinheitliche Handhabung kann die Benutzerfreundlichkeit beeinträchtigen.
-   Zeigen Sie beim Verschieben oder Ziehen kein visuelles Feedback an. Die tatsächliche Verschiebung des Objekts auf dem Bildschirm ist ausreichend. Wenn der Inhaltsbereich jedoch nicht verschoben oder kein Bildlauf durchgeführt werden kann, sollten Sie mit Visualisierungen darauf hinweisen. Weitere Informationen finden Sie unter [Richtlinien für Verschiebung](guidelines-for-panning.md).
-   Zeigen Sie kein Feedback für ein Steuerelement an, das nicht als Ziel ausgewählt wurde. Visuelles Feedback ist wichtig, wenn die Fingereingabe für Aktivitäten verwendet wird, bei denen Positionsgenauigkeit gefragt ist. Wenn Sie bei jeder erkannten Toucheingabe Feedback anzeigen, können Benutzer die spezielle Auswahlheuristik Ihrer App und die zugehörigen Steuerelemente besser verstehen.
-   Verwenden Sie das für einen Eingabetyp vorgesehene Feedbackverhalten nicht für einen anderen Eingabetyp. Beispiel: Tastaturfokusrechtecke sollten nur für die Tastatur- und nicht für die Toucheingabe verwendet werden.

## <span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>Weitere Hinweise zur Verwendung


Kontaktvisualisierungen sind gerade für Interaktionen per Fingereingabe wichtig, bei denen Genauigkeit und Präzision gefragt sind. Ihre App sollte eine Position, auf die getippt wurde, z. B. genau anzeigen, damit der Benutzer erkennen kann, ob er das Ziel verfehlt hat, wie weit er danebenliegt und welche Korrekturen erforderlich sind.

Mit den Plattformsteuerelementen der Sprachframeworks für Windows Store-Apps (Windows Store-Apps mit JavaScript und Windows Store-Apps mit C++, C# oder Visual Basic) können Sie Windows 8-Visualisierungen kostenlos nutzen. Falls Ihre App benutzerdefinierte Interaktionen beinhaltet, die angepasstes Feedback erfordern, müssen Sie sicherstellen, dass sich das Feedback für die jeweiligen Zwecke eignet, für alle unterstützten Eingabegeräte verfügbar ist und den Benutzer nicht von seiner eigentlichen Arbeit ablenkt. Dies kann insbesondere bei Spiel- oder Zeichnungs-Apps ein Problem sein, bei denen das visuelle Feedback wichtige UI-Elemente behindern oder verdecken kann.

**Wichtig**  
Das Interaktionsverhalten der integrierten Gesten sollte nicht geändert werden.

 

**Feedback-UI**

Die Feedback-UI hängt im Allgemeinen vom Eingabegerät ab (Toucheingabe, Touchpad, Maus, Zeichen-/Eingabestift, Tastatur usw.). Das integrierte Feedback für die Maus z. B. beinhaltet normalerweise eine Bewegung und Änderung des Cursors, während für Touch- und Stifteingabe Berührungsvisualisierungen erforderlich sind und für die Eingabe und Navigation per Tastatur Fokusrechtecke und Hervorhebung verwendet werden.

Verwenden Sie [**ShowGestureFeedback**](https://msdn.microsoft.com/library/windows/apps/br241969), um das Feedbackverhalten für die Plattformgesten festzulegen.

Wenn Sie Anpassungen an der Feedback-UI vornehmen, muss das Feedback alle Eingabemodi unterstützen und für alle Modi geeignet sein.

Im Folgenden finden Sie einige Beispiele für integrierte Kontaktvisualisierungen in Windows 8.

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">
<img src="images/feedback-touch-cursor.png" alt="Screenshot showing a touch visualization." />
<p>Fingereingabevisualisierung</p></td>
<td align="left"><img src="images/feedback-mouse-cursor2.png" alt="Screenshot showing a mouse visualization." />
<p>Maus-/Touchpadvisualisierung</p></td>
<td align="left"><img src="images/feedback-pen-cursor3.png" alt="Screenshot showing a pen visualization." />
<p>Stiftvisualisierung</p></td>
<td align="left"><img src="images/feedback-keyboard-cursor.png" alt="Screenshot showing a keyboard visualization." />
<p>Tastaturvisualisierung</p></td>
</tr>
</tbody>
</table>

 

### <span id="informationalui"></span><span id="INFORMATIONALUI"></span>

**Informations-UI (Popups)**

Eine der wichtigsten Formen des visuellen Feedbacks ist die Informations-UI (oder Mehrdeutigkeitsvermeidungs-UI). Die Informations-UI enthält Informationen zu einem Objekt, eine Beschreibung seiner Funktion, Anweisungen für den Zugriff auf das Objekt und ggf. weitere Anleitungen.

Dies sind die unterschiedlichen Informations-UI-Typen, die von Windows Store-Apps unterstützt werden.

-   QuickInfos
-   Umfangreiche QuickInfos
-   Menüs
-   Meldungsdialogfelder
-   Flyouts

Mit Informations-UI können Sie verhindern, dass UI-Elemente durch Finger verdeckt (behindert) werden, und Toucheingabeinteraktionen mit Ihrer App verbessern. Sie verfügt sogar über eine integrierte Geste: Gedrückthalten.

Das Gedrückthalten ist eine zeitgesteuerte Interaktion – ein Interaktionstyp, von dem in Windows 8 normalerweise abgeraten wird. Eine zeitgesteuerte Interaktion ist in diesem Fall akzeptabel, da sie für ein Tool dient, mit dem der Benutzer Zugriff auf weitere Informationen erhält. Die empfohlene Dauer hängt vom Typ der Informationsbenutzeroberfläche ab. Die folgende Tabelle enthält die empfohlenen Zeitschwellenwerte.

Informations-UI-Typ
Zeitliche Steuerung
Aktivierung
Verwendung
Okklusions-QuickInfo (für Scrubbing und kleine Ziele)
0 ms
Ja
Für die schnelle Erläuterung von Aktionen. Wird in der Regel für Befehle verwendet.
Okklusions-QuickInfo (für Aktionen)
200 ms
Ja
Umfangreiche QuickInfo
~2000 ms
Nein
Für langsameres, bewussteres Anzeigen zusätzlicher Informationen. Wird in der Regel für Auflistungselemente verwendet.
Interaktion mit automatischem Einblenden
~2000 ms
Nein
Kontextmenü
~2000 ms
Nein
Macht einen begrenzten Satz von Befehlen im Zusammenhang mit dem ausgewählten Objekt verfügbar.
Flyouts
~2000 ms
Nein
Macht einen begrenzten Satz von Befehlen im Zusammenhang mit dem ausgewählten Objekt verfügbar.
 

Weitere Informationen zum Bereitstellen von Informations-UI finden Sie unter [Gestalten der Benutzeroberfläche](https://msdn.microsoft.com/library/windows/apps/hh465304) und [Anzeigen von Popups](https://msdn.microsoft.com/library/windows/apps/hh738362).

**QuickInfos**

Verwenden Sie QuickInfos, um weitere Informationen zu einem Steuerelement anzuzeigen, bevor der Benutzer zum Ausführen einer Aktion aufgefordert wird.

QuickInfos ([**Tooltip**](https://msdn.microsoft.com/library/windows/apps/br229763)) werden automatisch angezeigt, wenn der Benutzer eine Gedrückthaltebewegung für ein Steuerelement oder Objekt ausführt (oder Daraufzeigen erkannt wird). Die QuickInfo verschwindet, wenn der Kontakt gelöst oder der Cursor vom Steuerelement bzw. Objekt wegbewegt wird. Eine QuickInfo kann Text und Bilder enthalten, ist aber nicht interaktiv.

**Okklusions-QuickInfos für kleine Ziele**

Okklusions-QuickInfos beschreiben das verdeckte Ziel. Diese QuickInfos sind beim Auswählen und Aktivieren von Zielen hilfreich, die kleiner als die Standardgröße für Fingereingabeziele sind, z. B. Hyperlinks auf einer Webseite.

Sie können diese QuickInfos nach Verstreichen eines bestimmten Zeitschwellenwerts durch ein Informationspopup ersetzen. Verwenden Sie z. B. eine Okklusions-QuickInfo, um den verdeckten Text des Links anzuzeigen, und ersetzen Sie die QuickInfo dann durch ein Popup mit der URL.

**Okklusions-QuickInfos für Aktionen und Befehle**

Diese QuickInfos beschreiben die Aktion oder den Befehl, die bzw. der ausgeführt wird, wenn der Benutzer den Finger von einem Element nimmt. Diese QuickInfos sind beim Auswählen und Aktivieren von Schaltflächen oder ähnlichen Steuerelementen hilfreich.

Nach einer QuickInfo für ein kleines Ziel kann nach einem bestimmten Zeitschwellenwert eine Aktions-QuickInfo angezeigt. In diesem Fall sollte die QuickInfo für das kleine Ziel so erweitert werden, dass sie die zusätzlichen Informationen aus der Aktions-QuickInfo enthält.

**Umfangreiche QuickInfo**

Diese QuickInfos zeigen sekundäre Informationen zu einem Element an. Bei einer umfangreichen QuickInfo kann es sich z. B. um eine Textbeschreibung eines Bilds, den vollständigen Text eines abgeschnittenen Titels oder andere für das Ziel relevante Infos handeln.

Umfange QuickInfos enthalten normalerweise Informationen, die nicht sofort verfügbar gemacht werden müssen und in manchen Fällen ablenken können, wenn sie zu schnell angezeigt werden. Bei einem längeren Zeitschwellenwert rufen Benutzer die Informationen bewusster ab.

Nach der Anzeige einer umfangreichen QuickInfo ist das Objekt nicht mehr aktiviert, wenn der Benutzer den Finger abhebt. Der Grund hierfür ist, dass die der QuickInfo entnommenen Informationen den Benutzer veranlassen können, dass Element nicht zu aktivieren.

Umfangreiche QuickInfos sollten sich sowohl hinsichtlich des Designs als auch des Informationsumfangs deutlich von Standard-QuickInfos unterscheiden.

**Kontextmenü**

Das Kontextmenü ([**PopupMenu**](https://msdn.microsoft.com/library/windows/apps/br208693)) ist ein kompaktes Menü, über das Benutzer direkt auf Aktionen (z. B. Zwischenablagebefehle) für Text oder UI-Objekte in Windows Store-Apps zugreifen können.

Das für Toucheingabe optimierte Kontextmenü besteht aus zwei Teilen. Als Ergebnis einer Halteinteraktion wird ein visueller Hinweis angezeigt. Das Kontextmenü selbst wird dann angezeigt, nachdem der Hinweis ausgeblendet und der Finger abgehoben wurde.

Die folgenden Abbildungen zeigen, wie Sie das Standardkontextmenü aufrufen, indem Sie in eine Auswahl oder auf ein Ziehelement tippen (Gedrückthalten kann auch verwendet werden).

![Tippen (oder drücken und halten) Sie in die Auswahl oder auf ein Ziehelement, um das Kontextmenü aufzurufen.](images/textselection-show-context.png)

Siehe [Hinzufügen von Kontextmenüs](https://msdn.microsoft.com/library/windows/apps/hh465300).

**Meldungsdialogfeld**

Verwenden Sie Meldungsdialogfelder ([**MessageDialog**](https://msdn.microsoft.com/library/windows/apps/br208674)), um den Benutzer basierend auf der Benutzeraktion oder dem App-Status zu einer Eingabe aufzufordern, bevor der jeweilige Vorgang fortgesetzt wird. Es ist eine explizite Benutzerinteraktion erforderlich, und Eingaben in der App sind blockiert, bis der Benutzer reagiert.

![Meldungsdialogfeld für eine Fehlermeldung](images/messagedialog.png)

Die folgende Liste enthält einige typische Gründe für die Verwendung eines Meldungsdialogfelds.

-   Anzeigen wichtiger Informationen
-   Stellen einer Frage vor dem Fortsetzen der Ausführung
-   Fehlermeldungen anzeigen

Siehe [Hinzufügen von Meldungsdialogfeldern](https://msdn.microsoft.com/library/windows/apps/hh738361).

**Flyout**

Ein Flyout ([**Flyout**](https://msdn.microsoft.com/library/windows/apps/br211726)) ist ein kleines UI-Panel, das durch Tippen, Klicken oder eine andere Aktivierung aufgerufen wird, um Informationen, Fragen oder ein spezifisches Optionsmenü für die aktuelle Aktivität des Benutzers anzuzeigen. Es kann leicht ausgeblendet werden, d. h. es verschwindet, wenn der Benutzer an einer Stelle außerhalb des Flyoutpanels tippt oder klickt oder die ESC-Taste drückt. Mit anderen Worten: Ein Flyout kann ignoriert werden.

Anders als QuickInfos unterstützen Flyouts Eingaben. Anders als bei einem Meldungsdialogfeld ist die App noch immer aktiv und nimmt Eingaben an.

![Flyout mit Bestätigung](images/flyout.png)

Siehe [Hinzufügen von Flyouts und Menüs](https://msdn.microsoft.com/library/windows/apps/hh465325).

### <span id="selfreveal"></span><span id="SELFREVEAL"></span>

**Automatisch eingeblendete UI**

Eine Interaktion mit automatischem Einblenden ist ein informativer visueller Hinweis oder eine Animation, der bzw. die die Ausführung einer Aktion für ein Zielobjekt veranschaulicht und eine Vorschau des Ergebnisses der Aktion bereitstellt.

Die folgenden Abbildungen zeigen die Interaktion mit automatischem Einblenden für eine Auswahl durch Querziehen auf der Startseite. Wenn der Benutzer eine App-Kachel berührt (ohne die Kachel zu ziehen), gleitet die Kachel nach unten (wie beim Ziehen), um das Auswahlhäkchen einzublenden, das angezeigt würde, wenn die App tatsächlich ausgewählt wird.

![Screenshot eines Zustands ohne Auswahl](images/crossslide-selfreveal1.png)

*Durch Drücken mit dem Finger auf ein Element wird die Interaktion mit automatischem Einblenden für die Auswahl gestartet. Die Interaktion mit automatischem Einblenden zeigt, welche Aktion für das Element ausgeführt wird.*

![Screenshot der Animation für die Auswahl](images/crossslide-selfreveal2.png)

*Durch eine Streifbewegung ohne Abheben des Fingers wird das Element tatsächlich ausgewählt.*

![Screenshot der Animation für Drag & Drop](images/crossslide-selfreveal3.png)

*Wenn der Benutzer die Streifbewegung fortführt, ändert sich die Visualisierung mit automatischem Einblenden, um anzuzeigen, dass das Objekt jetzt verschoben werden kann.*

Nach der Anzeige einer Interaktion mit automatischem Einblenden ist das Objekt nicht mehr aktiviert, wenn der Benutzer den Finger hebt.

## <span id="related_topics"></span>Verwandte Artikel


**Für Designer**
* [Richtlinien für Verschiebung](guidelines-for-panning.md)
**Für Entwickler**
* [Benutzerdefinierte Benutzerinteraktionen](https://msdn.microsoft.com/library/windows/apps/mt185599)
**Beispiele**
* [Einfaches Eingabebeispiel](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Eingabebeispiel mit geringer Latenz](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Beispiel für den Benutzerinteraktionsmodus](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Beispiel für visuelle Fokuselemente](http://go.microsoft.com/fwlink/p/?LinkID=619895)
**Archivbeispiele**
* [Eingabe: Beispiel XAML-Benutzereingabeereignisse](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Eingabe: Beispiel für Gerätefunktionen](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Eingabe: Beispiel für Fingereingabe-Treffertests](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [Beispiel für XAML-Bildlauf, -Verschiebung und -Zoomen](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Eingabe: vereinfachtes Freihandbeispiel](http://go.microsoft.com/fwlink/p/?linkid=246570)
* [Eingabe: Beispiel für Windows 8-Bewegungen](http://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Eingabe: Beispiel für Manipulationen und Gesten (C++)](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [Beispiel für die DirectX-Fingereingabe](http://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 






<!--HONumber=Apr16_HO3-->


