---
author: Karl-Bridge-Microsoft
Description: "In diesem Thema wird die neue Windows-Benutzeroberfläche zum Auswählen und Bearbeiten von Text, Bildern und Steuerelementen beschrieben. Außerdem finden Sie hier Richtlinien für die Benutzerumgebung, die berücksichtigt werden sollten, wenn Sie diese neuen Auswahl- und Manipulationsmechanismen in Ihrer Windows Store-App verwenden."
title: "Auswählen von Text und Bildern"
ms.assetid: d973ffd8-602e-47b5-ab0b-4b2a964ec53d
label: Selecting text and images
template: detail.hbs
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: 6533e76981d815c2d62008c17e2320fc770dfcc9

---

# Auswählen von Text und Bildern

In diesem Artikel wird das Auswählen und Bearbeiten von Text, Bildern und Steuerelementen beschrieben. Außerdem enthält er Richtlinien für die Benutzeroberfläche, die bei Verwendung dieser Mechanismen in Ihren Apps berücksichtigt werden sollten.




**Wichtige APIs**

-   [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)
-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)


## Empfohlene und nicht empfohlene Vorgehensweisen


-   Verwenden Sie Schriftartglyphen, wenn Sie Ihre eigene Ziehelement-UI implementieren. Das Ziehelement ist eine Kombination aus zwei systemweit verfügbaren Segoe UI-Schriftarten. Die Verwendung von Schriftressourcen vereinfacht das Rendering bei unterschiedlichen DPI-Einstellungen und funktioniert gut mit den verschiedenen UI-Skalierungsebenen. Ihre Ziehelemente sollten alle die folgenden Benutzeroberflächenmerkmale aufweisen:

    -   Kreisform
    -   Sichtbar auf jedem Hintergrund
    -   Einheitliche Größe
-   Fügen Sie um auswählbaren Inhalt einen Rand für die Ziehelement-UI hinzu. Falls Ihre App die Textauswahl in einem Bereich ohne Verschiebung/Bildlauf ermöglicht, sollten Sie einen Rand lassen, der an der linken und rechten Seite halb so groß ist wie das Ziehelement und an der oberen und unteren Seite so groß wie die Höhe des Zielelements (siehe folgende Abbildungen). Dadurch wird sichergestellt, dass die gesamte Ziehelement-UI für den Benutzer sichtbar ist, und unbeabsichtigte Interaktionen mit anderen randbasierten Benutzeroberflächen werden minimiert.

    ![Ränder des Textauswahlziehelements](images/textselection-gripper-margins.png)

-   Blenden Sie die Ziehelement-UI während der Interaktion aus. So können Sie verhindern, dass die Interaktion durch die Ziehelemente behindert wird. Dies ist hilfreich, wenn ein Ziehelement nicht vollständig vom Finger verdeckt wird oder mehrere Ziehelemente für die Textauswahl vorhanden sind. Dadurch werden visuelle Fehler beim Anzeigen untergeordneter Fenster verhindert.

-   Die Auswahl von UI-Elementen wie Steuerelementen, Beschriftungen, Bildern, geschützten Inhalten usw. sollte nicht möglich sein. In Windows-Apps ist die Auswahl normalerweise nur in bestimmten Steuerelementen möglich. Steuerelemente wie Schaltflächen, Beschriftungen und Logos sind nicht auswählbar. Beurteilen Sie, ob die Auswahl ein Problem für die App darstellt, und identifizieren Sie gegebenenfalls die Bereiche der UI, in denen keine Auswahl möglich sein sollte. 

## Weitere Hinweise zur Verwendung


Für die Auswahl und Manipulation von Text ergeben sich durch Fingereingabeinteraktionen besonders leicht Probleme für die Benutzeroberfläche. Eingaben über Maus, Zeichen-/Tablettstift und Tastatur sind sehr präzise: Ein Mausklick oder ein Kontakt mit dem Zeichen- bzw. Tablettstift ist in der Regel einem einzigen Pixel zugeordnet, und eine Taste wird gedrückt oder nicht gedrückt. Die Fingereingabe ist weniger präzise. Es ist schwierig, die gesamte Oberfläche einer Fingerspitze einer bestimmten x-y-Position auf dem Bildschirm zuzuordnen, um ein Caretzeichen exakt im Text zu platzieren.

**Überlegungen und Empfehlungen**

Verwenden Sie die integrierten Steuerelemente der Sprachframeworks in Windows, um Apps mit sämtlichen Benutzerinteraktionsfunktionen der Plattform – einschließlich Auswahl- und Manipulationsverhalten – zu erstellen. Die Interaktionsfunktionen der integrierten Steuerelemente sollten daher für die meisten UWP-Apps ausreichen.

Bei Verwendung der standardmäßigen UWP-Textsteuerelemente können die in diesem Thema beschriebenen Auswahlverhalten und visuellen Elemente nicht angepasst werden.

**Textauswahl**

Falls Ihre App eine benutzerdefinierte Benutzeroberfläche erfordert, die die Textauswahl unterstützt, empfehlen wir, die hier beschriebenen Auswahlverhalten von Windows anzuwenden.

**Bearbeitbare und nicht bearbeitbare Inhalte**


Bei der Fingereingabe werden Auswahlinteraktionen hauptsächlich durch Bewegungen ausgeführt, z. B. Tippen, um einen Einfügecursor zu setzen oder ein Wort auszuwählen, und Ziehen, um eine Auswahl zu ändern. Wie bei anderen Fingereingabeinteraktionen in Windows sind zeitgesteuerte Interaktionen zum Anzeigen einer Informationsbenutzeroberfläche auf die Gedrückthaltebewegung beschränkt. Weitere Informationen finden Sie unter [Richtlinien für visuelles Feedback](guidelines-for-visualfeedback.md).

Windows erkennt zwei mögliche Zustände für Auswahlinteraktionen (bearbeitbar und nicht bearbeitbar) und passt Auswahl-UI, Feedback und Funktionalität entsprechend an.

**Bearbeitbare Inhalte**

Durch Tippen in der linken Hälfte eines Worts wird der Cursor links neben dem Wort platziert, durch Tippen in der rechten Hälfte wird er rechts neben dem Wort platziert.

Im folgenden Bild wird veranschaulicht, wie Sie einen anfänglichen Einfügecursor mit Ziehelement platzieren, indem Sie in die Nähe des Anfangs oder Endes eines Worts tippen.

![Tippen (oder drücken und halten) Sie links neben ein Wort, um ein Caretzeichen und ein Ziehelement am Anfang des Worts zu platzieren. Tippen (oder drücken und halten) Sie rechts neben ein Wort, um ein Caretzeichen und ein Ziehelement am Ende des Worts zu platzieren.](images/textselection-place-caret.png)

Im folgenden Bild wird veranschaulicht, wie Sie eine Auswahl durch Ziehen des Ziehelements anpassen.

![Ziehen Sie das Ziehelement in eine beliebige Richtung, um die Auswahl anzupassen (das erste Ziehelement bleibt verankert, und ein zweites Ziehelement wird angezeigt). Ziehen Sie eines der Ziehelemente, um weitere Anpassungen vorzunehmen.](images/adjust-selection.png)

Im folgenden Bild wird veranschaulicht, wie Sie das Kontextmenü aufrufen, indem Sie in die Auswahl oder auf ein Ziehelement tippen (Sie können auch Drücken und Halten verwenden).

![Tippen (oder drücken und halten) Sie in die Auswahl oder auf ein Ziehelement, um das Kontextmenü aufzurufen.](images/textselection-show-context.png)

**Hinweis**  Bei einem falsch geschriebenen Wort weichen diese Interaktionen etwas ab. Wenn Sie auf ein Wort tippen, das als falsch geschrieben gekennzeichnet ist, wird das gesamte Wort hervorgehoben. Außerdem wird das Kontextmenü mit der vorgeschlagenen Schreibweise aufgerufen.

 

**Nicht bearbeitbare Inhalte**

Im folgenden Bild wird veranschaulicht, wie Sie ein Wort auswählen, indem Sie in das Wort tippen (die anfängliche Auswahl enthält keine Leerzeichen).

![Tippen Sie in ein Wort, um es auszuwählen (die anfängliche Auswahl enthält keine Leerzeichen).](images/select-word.png)

Gehen Sie für bearbeitbaren Test genauso vor, um die Auswahl anzupassen und das Kontextmenü anzuzeigen.

**Objektmanipulation**

Verwenden Sie beim Implementieren einer benutzerdefinierten Objektmanipulation in einer UWP-App nach Möglichkeit die gleichen (oder ähnliche) Ziehelementressourcen wie für die Textauswahl. Auf diese Weise kann ein einheitliches Interaktionsverhalten für die gesamte Plattform bereitgestellt werden.

Zielelemente können wie in den folgenden Abbildungen dargestellt z. B. auch in Bildverarbeitungs-Apps verwendet werden, die Größenänderungen und Zuschneiden unterstützen, oder in Media-Player-Apps mit anpassbaren Statusanzeigen.

![Media-Player mit Statusziehelement](images/gripper-mediaplayer.png)

*Media-Player mit anpassbarer Statusanzeige*

![Bild mit Ziehelementen zum Zuschneiden](images/gripper-imagemanip.png)

*Bild-Editor mit Ziehelementen zum Zuschneiden*

## Verwandte Artikel



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
 

 







<!--HONumber=Jun16_HO4-->


