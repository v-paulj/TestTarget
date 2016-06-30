---
author: mijacobs
Description: "Verwenden Sie Querziehen, um Auswahlinteraktionen mit einer Streifbewegung und Ziehinteraktionen (Verschieben) mit einer Ziehbewegung zu unterstützen."
title: "Richtlinien für Querziehen"
ms.assetid: 897555e2-c567-4bbe-b600-553daeb223d5
label: Cross-slide
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 47a16acc4025541b1cc19582c2c7d59755fd2594

---

# Richtlinien für Querziehen


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**CrossSliding**](https://msdn.microsoft.com/library/windows/apps/br241942)
-   [**CrossSlideThresholds**](https://msdn.microsoft.com/library/windows/apps/br241941)
-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)

Verwenden Sie Querziehen, um Auswahlinteraktionen mit einer Streifbewegung und Ziehinteraktionen (Verschieben) mit einer Ziehbewegung zu unterstützen.

## <span id="Dos_and_don_ts"></span><span id="dos_and_don_ts"></span><span id="DOS_AND_DON_TS"></span>Empfohlene und nicht empfohlene Vorgehensweisen


-   Verwenden Sie das Querziehen für Listen oder Auflistungen, bei denen ein Bildlauf nur in eine Richtung möglich ist.
-   Verwenden Sie das Querziehen für die Elementauswahl, wenn die Tippinteraktion für andere Zwecke verwendet wird.
-   Verwenden Sie das Querziehen nicht, um Elemente zu einer Warteschlange hinzuzufügen.

## <span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>Weitere Hinweise zur Verwendung


Auswahl und Ziehen sind nur in Inhaltsbereichen möglich, die in eine Richtung (vertikal oder horizontal) verschoben werden können. Damit die Interaktion funktioniert, muss eine Verschiebungsrichtung arretiert sein, und die Bewegung muss senkrecht zur Verschiebungsrichtung ausgeführt werden.

Im Folgenden wird das Auswählen und Ziehen eines Objekts durch Querziehen veranschaulicht. Die linke Abbildung zeigt, wie ein Element ausgewählt wird, wenn eine Streifbewegung eine Distanzschwelle nicht überschreitet, bevor der Kontakt aufgehoben und das Objekt losgelassen wird. Die rechte Abbildung zeigt eine Ziehbewegung, die eine Distanzschwelle überschreitet und zum Ziehen des Objekts führt.

![Diagramm, das die Prozesse für Auswahl und Drag & Drop veranschaulicht](images/crossslide-mechanism.png)

Das folgende Diagramm zeigt Distanzschwellen, die bei der Interaktion des Querziehens verwendet werden.

![Screenshot der Prozesse für Auswahl und Drag & Drop](images/crossslide-threshold.png)

Damit die Verschiebungsfunktion erhalten bleibt, muss eine niedrige Schwelle von 2,7 mm (ca. 10 Pixel bei Zielauflösung) überschritten werden, bevor eine Auswahl- oder Ziehinteraktion aktiviert wird. Anhand dieser niedrigen Schwelle kann das System zwischen Querziehen und Verschieben unterscheiden. Außerdem kann so eine Tippbewegung von Querziehen und Verschieben unterschieden werden.

Die folgende Abbildung zeigt, wie der Benutzer ein UI-Element berührt, den Finger beim Kontakt aber leicht nach unten bewegt. Ohne Schwelle würde die Interaktion aufgrund der anfänglichen vertikalen Bewegung als Querziehen interpretiert. Dank der Schwelle wird die Bewegung korrekt als horizontales Verschieben interpretiert.

![Screenshot der Mehrdeutigkeitsvermeidungsschwelle für Auswahl und Drag & Drop](images/crossslide-threshold2.png)

Beachten Sie die folgenden Richtlinien, wenn Sie eine Querziehfunktion in Ihrer App bereitstellen.

Verwenden Sie das Querziehen für Listen oder Auflistungen, bei denen ein Bildlauf nur in eine Richtung möglich ist. Weitere Informationen finden Sie unter [Hinzufügen von ListView-Steuerelementen](https://msdn.microsoft.com/library/windows/apps/hh465382).

**Hinweis**  In Fällen, in denen der Inhaltsbereich in zwei Richtungen verschoben werden kann, z. B. in einem Webbrowser oder E-Reader, sollte die zeitlich festgelegte Interaktion des Gedrückthaltens verwendet werden, um das Kontextmenü für Objekte wie Bilder und Links aufzurufen.

 

|                                                                                         |                                                                                         |
|-----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| ![Horizontal verschiebbare zweidimensionale Liste](images/groupedlistview1.png)                | ![Vertikal verschiebbare eindimensionale Liste](images/listviewlistlayout.png)                |
| Hier sehen Sie eine horizontal verschiebbare zweidimensionale Liste. Ziehen Sie vertikal, um ein Element auszuwählen oder zu verschieben. | Hier sehen Sie eine vertikal verschiebbare eindimensionale Liste. Ziehen Sie horizontal, um ein Element auszuwählen oder zu verschieben. |

 

### <span id="selection"></span><span id="SELECTION"></span>

**Auswählen**

Beim Auswählen wird mindestens ein Objekt markiert, ohne es zu starten oder zu aktivieren. Diese Aktion entspricht einem einfachen Mausklick oder einem Mausklick mit gedrückter UMSCHALTTASTE auf mindestens ein Objekt.

Zum Auswählen durch Querziehen wird ein Element berührt und nach einer kurzen Interaktion des Ziehens wieder losgelassen. Bei dieser Methode entfallen sowohl der dedizierte Auswahlmodus als auch die zeitlich festgelegte Interaktion des Gedrückthaltens, die bei anderen Schnittstellen für die Fingereingabe erforderlich sind. Sie steht nicht in Konflikt mit der für die Aktivierung verwendeten Interaktion des Tippens.

Neben der Distanzschwelle gilt für das Auswählen durch Querziehen auch eine Beschränkung auf einen Schwellenbereich von 90°, wie im folgenden Diagramm veranschaulicht. Wenn das Objekt außerhalb dieses Bereichs gezogen wird, wird es nicht ausgewählt.

![Diagramm, das den Schwellenbereich für die Auswahl zeigt](images/crossslide-selection.png)

Die Interaktion des Querziehens wird durch eine zeitlich festgelegte Interaktion des Gedrückthaltens, auch als „Interaktion mit automatischem Einblenden“ bezeichnet, ergänzt. Durch diese zusätzliche Interaktion wird eine Animation aktiviert, die zeigt, welche Aktion für das Objekt ausgeführt werden kann. Weitere Informationen zur Mehrdeutigkeitsvermeidungs-UI finden Sie unter [Richtlinien für visuelles Feedback](guidelines-for-visualfeedback.md).

In den folgenden Screenshots wird die Funktionsweise der Animation mit automatischem Einblenden verdeutlicht.

1.  Halten Sie das Element gedrückt, um die Animation für die Interaktion mit automatischem Einblenden auszulösen. Der ausgewählte Zustand des Elements hat direkten Einfluss auf die durch die Animation eingeblendeten Inhalte: ein Häkchen bei aufgehobener Auswahl und kein Häkchen bei erfolgter Auswahl.

    ![Screenshot eines Zustands ohne Auswahl](images/crossslide-selfreveal1.png)

2.  Wählen Sie das Element mit einer Streifbewegung (nach oben oder unten) aus.

    ![Screenshot der Animation für die Auswahl](images/crossslide-selfreveal2.png)

3.  Das Element ist nun ausgewählt. Setzen Sie das Auswahlverhalten mit der Streifbewegung zum Verschieben des Elements außer Kraft.

    ![Screenshot der Animation für Drag & Drop](images/crossslide-selfreveal3.png)

Verwenden Sie in Apps, in denen das Auswählen die einzige Hauptaktion ist, einfaches Tippen für die Auswahl. Die Querziehanimation mit automatischem Einblenden wird angezeigt, um diese Funktion von der standardmäßigen Tippinteraktion für Aktivierung und Navigation zu unterscheiden.

**Auswahlkorb**

Der Auswahlkorb ist eine visuell unverwechselbare, dynamische Darstellung von Elementen, die aus der primären Liste oder Auflistung in der Anwendung ausgewählt wurden. Dieses Feature ist hilfreich, um den Überblick über ausgewählte Elemente zu behalten, und es sollte in Anwendungen verwendet werden, auf die Folgendes zutrifft:

-   Elemente können an mehreren Orten ausgewählt werden.
-   Es können mehrere Elemente ausgewählt werden.
-   Einer Aktion oder einem Befehl wird die Auswahlliste zugrunde gelegt.

Der Inhalt des Auswahlkorbs bleibt über Aktionen und Befehle hinweg erhalten. Wenn Sie z. B. eine Serie von Fotos aus einer Galerie auswählen, in jedem Foto eine Farbkorrektur durchführen und die Fotos auf irgendeine Weise mit anderen teilen, bleibt die Auswahl der Elemente erhalten.

Wenn in einer Anwendung kein Auswahlkorb verwendet wird, sollte die aktuelle Auswahl nach einer Aktion oder einem Befehl gelöscht werden. Wenn Sie z. B. einen Musiktitel in einer Wiedergabeliste auswählen und bewerten, sollte die Auswahl gelöscht werden.

Auch wenn kein Auswahlkorb verwendet und ein anderes Element in der Liste oder Auflistung aktiviert wird, sollte die aktuelle Auswahl gelöscht werden. Wenn Sie z. B. eine Nachricht im Posteingang auswählen, wird das Vorschaufenster aktualisiert. Wählen Sie anschließend eine zweite Nachricht im Posteingang aus, wird die Auswahl der vorherigen Nachricht aufgehoben und das Vorschaufenster erneut aktualisiert.

**Warteschlangen**

Eine Warteschlange ist nicht mit der Liste im Auswahlkorb gleichzusetzen und sollte auch nicht so behandelt werden. Hier die wichtigsten Unterschiede:

-   Die Liste der Elemente im Auswahlkorb ist nur eine visuelle Darstellung, während die Elemente in einer Warteschlange im Hinblick auf eine bestimmte Aktion zusammengestellt werden.
-   Elemente können im Auswahlkorb nur einmal dargestellt werden, in einer Warteschlange hingegen mehrmals.
-   Die Reihenfolge der Elemente im Auswahlkorb entspricht der Reihenfolge, in der sie ausgewählt wurden. Die Reihenfolge der Elemente in einer Warteschlange hängt unmittelbar mit der Funktion zusammen.

Aus diesen Gründen sollte die Auswahlinteraktion durch Querziehen nicht verwendet werden, um Elemente zu einer Warteschlange hinzuzufügen. Zum Hinzufügen von Elementen zu einer Warteschlange sollte eine Ziehaktion ausgeführt werden.

### <span id="draganddrop"></span><span id="DRAGANDDROP"></span>

**Ziehen**

Verwenden Sie eine Ziehbewegung, um Objekte von einer Position an eine andere zu ziehen.

Wenn mehrere Objekte verschoben werden müssen, geben Sie dem Benutzer die Möglichkeit, mehrere Elemente auszuwählen und anschließend gleichzeitig zu ziehen.

## <span id="related_topics"></span>Verwandte Artikel


**Beispiele**
* [Einfaches Eingabebeispiel](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Eingabebeispiel mit geringer Latenz](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Beispiel für den Benutzerinteraktionsmodus](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Beispiel für visuelle Fokus-Elemente](http://go.microsoft.com/fwlink/p/?LinkID=619895)
            
          
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


