---
author: Karl-Bridge-Microsoft
Description: Reagieren Sie in Ihren Apps auf Tastaturaktionen von Hardware- oder Softwaretastaturen, indem Sie sowohl Tastatur- als auch Klassen-Ereignishandler verwenden.
title: Tastaturinteraktionen
ms.assetid: FF819BAC-67C0-4EC9-8921-F087BE188138
label: Keyboard interactions
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: f9c475a90c270270217999c5a7289e29e7fef208
ms.openlocfilehash: a1d97c5a66db1b799ccc16769ff18130155743b8

---

# Tastaturinteraktionen


Die Tastatureingabe ist ein wichtiger Teil der Erfahrung, die Benutzer bei der Interaktion mit Apps machen. Die Tastatur ist unentbehrlich für Personen mit bestimmten körperlichen Beeinträchtigungen oder für Benutzer, die die Tastatur als effizienteste Methode ansehen, um mit einer App zu interagieren. Benutzer sollten beispielsweise in der Lage sein, mit der TAB-Taste und den Pfeiltasten in der App zu navigieren, mit der LEERTASTE und der EINGABETASTE UI-Elemente zu aktivieren und mit Tastenkombinationen auf Befehle zuzugreifen.  

![Tastaturfavoritenbild](images/input-patterns/input-keyboard-small.jpg)

**Wichtige APIs**

-   [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941)
-   [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)
-   [**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072)


Eine gut durchdachte Tastatur-UI ist ein wichtiger Aspekt für die Barrierefreiheit von Software. Sie ermöglicht es Benutzern mit einer Sehbeeinträchtigung oder mit bestimmten motorischen Einschränkungen, in einer App zu navigieren und mit deren Features zu interagieren. Diese Benutzer können u.U. keine Maus bedienen und sind auf verschiedene Hilfstechnologien wie etwa Tastaturerweiterungstools, Bildschirmtastaturen, Bildschirmlupen, Bildschirmleseprogramme oder die Möglichkeit der Spracheingabe angewiesen.

Benutzer können mit universellen Apps über eine Hardwaretastatur und zwei Softwaretastaturen (Bildschirmtastatur oder Touch-Bildschirmtastatur) interagieren.

Bildschirmtastatur  
Bei der Bildschirmtastatur handelt es sich um eine visuelle Softwaretastatur, die Sie anstelle der physischen Tastatur zum Eingeben von Daten per Touch, Maus, Zeichen-/Eingabestift oder mit einem anderen Zeigegerät verwenden können. Ein Touchscreen ist nicht erforderlich. Die Bildschirmtastatur ist für Systeme ohne physische Tastatur oder für Benutzer vorgesehen, deren Mobilitätseinschränkungen die Verwendung herkömmlicher physischer Eingabegeräte verhindern. Die Bildschirmtastatur emuliert nahezu alle Funktionen der Hardwaretastatur.

Die Bildschirmtastatur kann auf der Seite „Tastatur“ unter „Einstellungen &gt; Erleichterte Bedienung“ aktiviert werden.

**Hinweis**: Die Bildschirmtastatur hat Priorität gegenüber der Touch-Bildschirmtastatur. Diese wird nicht angezeigt, wenn die Bildschirmtastatur angezeigt wird.

 

![Bildschirmtastatur](images/input-patterns/osk.png)

<sup>Bildschirmtastatur</sup>

Touch-Bildschirmtastatur  
Bei der Touch-Bildschirmtastatur handelt es sich um eine visuelle Softwaretastatur für die Texteingabe per Touchscreen. Sie ist kein Ersatz für die Bildschirmtastatur, da sie nur für Texteingaben verwendet wird (keine Emulation der Hardwaretastatur).

Abhängig vom Gerät wird die Touch-Bildschirmtastatur angezeigt, wenn ein Textfeld oder ein anderes bearbeitbares Textsteuerelement im Fokus steht, oder wenn der Benutzer sie über das **Benachrichtigungs-Center**manuell aktiviert.

![Symbol der Touch-Bildschirmtastatur im Benachrichtigungs-Center](images/input-patterns/touch-keyboard-notificationcenter.png)

**Hinweis**: Möglicherweise muss der Benutzer zum Bildschirm **Tablet-Modus** unter „Einstellungen &gt; System“ wechseln und „Durch die Verwendung des Geräts als Tablet wird die Toucheingabe in Windows verbessert“ aktivieren, um die automatische Anzeige der Touch-Bildschirmtastatur zu aktivieren.

 

Wenn Ihre App den Fokus programmgesteuert auf ein Texteingabesteuerelement festlegt, wird die Touch-Bildschirmtastatur nicht aufgerufen. Dadurch wird unerwartetes, nicht direkt vom Benutzer ausgelöstes Verhalten verhindert. Allerdings wird die Tastatur automatisch ausgeblendet, wenn der Fokus programmgesteuert auf ein nicht textuelles Eingabesteuerelement bewegt wird.

Die Bildschirmtastatur bleibt in der Regel sichtbar, während der Benutzer zwischen Steuerelementen in einem Formular navigiert. Dieses Verhalten kann je nach den anderen Steuerelementtypen innerhalb des Formulars variieren.

Im Folgenden finden Sie eine Liste der nicht bearbeitbaren Steuerelemente, die in einer Texteingabesitzung mit der Bildschirmtastatur den Fokus erhalten können, ohne dass die Tastatur ausgeblendet wird. Statt die Benutzeroberfläche unnötigerweise zu ändern und den Benutzer möglicherweise zu verwirren, bleibt die Bildschirmtastatur angezeigt, da der Benutzer wahrscheinlich zwischen diesen Steuerelementen und der Texteingabe über die Bildschirmtastatur hin und her wechselt.

-   Kontrollkästchen
-   Kombinationsfeld
-   Optionsfeld
-   Bildlaufleiste
-   Struktur
-   Strukturelement
-   Menü
-   Menüleiste
-   Menüelement
-   Symbolleiste
-   Liste
-   Listenelement

Hier finden Sie einige Beispiele für verschiedene Modi der Touch-Bildschirmtastatur. Das erste Bild zeigt das Standardlayout, das zweite das Daumenlayout. (Letzteres ist unter Umständen nicht in allen Sprachen verfügbar.)

Hier finden Sie einige Beispiele für verschiedene Modi der Touch-Bildschirmtastatur. Das erste Bild zeigt das Standardlayout, das zweite das Daumenlayout. (Letzteres ist unter Umständen nicht in allen Sprachen verfügbar.)
<table>
<tr>
    <td>**Touch-Bildschirmtastatur mit Standardlayout  **</td>
    <td>![Touch-Bildschirmtastatur mit Standardlayout](images/touchkeyboard-standard.png)</td>
</tr>
<tr>
    <td>**Touch-Bildschirmtastatur mit erweitertem Layout  **</td>
    <td>![Touch-Bildschirmtastatur mit erweitertem Layout](images/touchkeyboard-expanded.png)</td>
</tr>
<tr>
    <td>**Touch-Bildschirmtastatur im standardmäßigen Daumenlayoutmodus:  **</td>
    <td>![Touch-Bildschirmtastatur mit Daumenlayout](images/touchkeyboard-thumb.png)</td>
</tr>
<tr>
    <td>**Touch-Bildschirmtastatur mit numerischem Daumenlayout  **</td>
    <td>![Touch-Bildschirmtastatur mit numerischem Daumenlayout](images/touchkeyboard-numeric-thumb.png)</td>
</tr>
</table>


Erfolgreiche Tastaturinteraktionen ermöglichen es Benutzern, einfache App-Szenarien nur über die Tastatur auszuführen; Benutzer können demnach über die Tastatur alle interaktiven Elemente erreichen und Standardfunktionen aktivieren. Eine Reihe von Faktoren kann den Erfolg beeinflussen, z. B. Tastaturnavigation, Tastenkombinationen für die Barrierefreiheit sowie Tastenkombinationen für erfahrene Benutzer.

**Hinweis**  Die Touch-Bildschirmtastatur unterstützt keine Umschaltung und nur die wenigsten Systembefehle (siehe [Muster](#keyboard_command_patterns)).

## Navigation


Damit ein Steuerelement (einschließlich der Navigationselemente) über die Tastatur verwendet werden kann, muss auf dem Steuerelement der Fokus liegen. Eine Möglichkeit, einem Steuerelement den Tastaturfokus zuzuweisen, besteht darin, es per TAB-Navigation zugänglich zu machen. Ein gut durchdachtes Tastaturnavigationsmodell enthält eine logische und vorhersehbare Aktivierreihenfolge, die es dem Benutzer ermöglicht, die App schnell und effizient zu erkunden und zu verwenden.

Alle interaktiven Steuerelemente (die nicht in einer Gruppe enthalten sind) sollten über Tabstopps verfügen, nicht interaktive Steuerelemente, z. B. Beschriftungen, dagegen nicht.

Eine Gruppe verwandter Steuerelemente kann in einer Steuerelementgruppe zusammengefasst und einem einzelnen Tabstopp zugeordnet werden. Steuerelementgruppen werden für Steuerelemente verwendet, die sich wie ein einzelnes Steuerelement verhalten, z. B. Optionsfelder. Sie können auch verwendet werden, wenn es zu viele Steuerelemente für eine effiziente Navigation mit der TAB-TASTE allein gibt. Der Pfeiltasten, POS1, ENDE, BILD-AUF und BILD-AB verschieben den Eingabefokus zwischen den Steuerelementen in einer Gruppe (es ist nicht möglich, mit diesen Tasten aus einer Steuerelementgruppe heraus zu navigieren).

Sie sollten den anfänglichen Fokus Ihrer Tastatur auf das Element festlegen, mit dem Benutzer beim Starten der App intuitiv (oder mit der größten Wahrscheinlichkeit) als Erstes interagieren. Häufig handelt es sich dabei um die Hauptansicht der App. So können Benutzer sofort damit beginnen, mithilfe der Pfeiltasten durch den Inhalt der App zu scrollen.

Legen Sie den anfänglichen Tastaturfokus nicht auf ein Element mit potenziell negativen oder verhängnisvollen Ergebnissen fest. Dadurch kann der Verlust von Daten oder des Systemzugriffs verhindert werden.

Versuchen Sie, die wichtigsten Befehle, Steuerelemente und Inhalte zuerst in der Aktivierreihenfolge und der Anzeigereihenfolge (oder visuellen Hierarchie) einzustufen und darzustellen. Die tatsächliche Anzeigeposition kann jedoch vom übergeordneten Layoutcontainer und bestimmten Eigenschaften der untergeordneten Elemente abhängen, die das Layout beeinflussen. Insbesondere kann sich die Leserichtung von Layouts mit einer Raster- oder Tabellenmetapher erheblich von der Aktivierreihenfolge unterscheiden. Dies ist nicht immer ein Problem, aber Sie sollten die Funktionen der App sowohl als UI für die Toucheingabe als auch als UI für die Verwendung mit der Tastatur testen.

Die Aktivierreihenfolge sollte möglichst der Leserichtung folgen. Dies ist weniger verwirrend für den Benutzer und abhängig vom Gebietsschema und der Sprache.

Weisen Sie Tastaturschaltflächen in der Benutzeroberfläche Ihrer App entsprechende Funktionen zu (Zurück- und Vorwärts-Schaltflächen).

Versuchen Sie, das Navigieren zurück zum Startbildschirm der App und zwischen wichtigen Inhalten so unkompliziert wie möglich zu machen.

Verwenden Sie die Pfeiltasten als Tastenkombinationen für die ordnungsgemäße interne Navigation zwischen untergeordneten Elementen zusammengesetzter Elemente. Wenn Knoten der Strukturansicht separate untergeordnete Elemente für die Verarbeitung der Erweitern/Reduzieren-Funktion und der Knotenaktivierung aufweisen, sollten Sie die NACH-LINKS- und die NACH-RECHTS-Taste verwenden, um die Erweitern/Reduzieren-Funktion über die Tastatur bereitzustellen. Dies entspricht den Plattformsteuerelementen.

Da die Touch-Bildschirmtastatur einen großen Teil des Bildschirms verdeckt, stellt die Universelle Windows-Plattform (UWP) sicher, dass das fokussierte Eingabefeld eingeblendet wird, wenn ein Benutzer durch die Steuerelemente des Formulars navigiert, einschließlich der Steuerelemente, die derzeit nicht sichtbar sind. Benutzerdefinierte Steuerelemente sollten dieses Verhalten emulieren.

![Formular mit und ohne angezeigte Touch-Bildschirmtastatur](images/input-patterns/touch-keyboard-pan1.png)

Manchmal müssen bestimmte UI-Elemente dauerhaft auf dem Bildschirm zu sehen sein. Gestalten Sie die UI so, dass sich die Formularsteuerelemente in einer verschiebbaren Region befinden und die wichtigen UI-Elemente statisch sind. Beispiel:

![Formular mit Bereichen, die immer sichtbar sein sollen](images/input-patterns/touch-keyboard-pan2.png)
## Aktivierung


Ein Steuerelement kann auf unterschiedliche Arten aktiviert werden, je nachdem, ob es derzeit den Fokus hat oder nicht.

LEERTASTE, EINGABETASTE und ESC-TASTE  
Die LEERTASTE sollte das Steuerelement mit dem Eingabefokus aktivieren. Die EINGABETASTE sollte ein Standardsteuerelement oder das Steuerelement mit dem Eingabefokus aktivieren. Ein Standardsteuerelement ist das Steuerelement mit dem anfänglichen Fokus oder eines, das ausschließlich auf die EINGABETASTE reagiert (in der Regel ändert sich dies mit dem Eingabefokus). Zudem sollte die ESC-Taste vorübergehende UI, z. B. Menüs und Dialogfelder, schließen oder beenden.

Die hier gezeigte Rechner-App verwendet die LEERTASTE zum Aktivieren der Schaltfläche mit dem Fokus und legt die EINGABETASTE auf die Schaltfläche „=“ und die ESC-Taste auf die Schaltfläche „C“ fest.

![Rechner-App](images/input-patterns/calculator.png)

Umschalttasten der Tastatur  
Die Umschalttasten der Tastatur fallen in die folgenden Kategorien:


| Kategorie | Beschreibung |
|----------|-------------|
| Tastenkombination | Führen Sie eine allgemeine Aktion ohne UI durch, z. B. „STRG+S“ für **Speichern**. Implementieren Sie Tastenkombinationen für wichtige Funktionen der App. Nicht jeder Befehl hat oder erfordert eine Tastenkombination. |   
| Zugriffstaste/Abkürzungstaste | Zugewiesen zu jedem sichtbaren Steuerelement der obersten Ebene wie „ALT+F“ für das Menü **Datei**. Eine Zugriffstaste ruft einen Befehl nicht auf und aktiviert ihn nicht. |
| Tastenkombination | Führen Sie standardmäßige systemdefinierte oder App-definierte Befehle wie „ALT+Druck“ für die Bildschirmaufzeichnung, „ALT+TAB“ zum Wechseln von Apps oder „F1“ für Hilfe aus. Ein mit einer Tastenkombination verknüpfter Befehl muss nicht unbedingt ein Menüelement sein. |
| Anwendungstaste/Menütaste | Zeigen Sie das Kontextmenü an. |
| Fenstertaste/Befehlstaste | Aktivieren Sie Systembefehle wie **Systemmenü**, **Sperrbildschirm** oder **Desktop anzeigen**. |

Zugriffstasten und Tastenkombinationen unterstützen die direkte Interaktion mit den Steuerelementen. Eine Navigation mit der TAB-Taste ist nicht erforderlich.
> Während einige Steuerelemente über systeminterne Beschriftungen verfügen, beispielsweise Befehlsschaltflächen, Kontrollkästchen und Optionsfelder, weisen andere Steuerelemente wie Listenansichten externe Beschriftungen auf. Bei Steuerelementen mit externen Beschriftungen wird die Zugriffstaste einer Beschriftung zugewiesen, die, nachdem sie aufgerufen wurde, den Fokus auf ein Element oder einen Wert im zugehörigen Steuerelement setzt.


Im vorliegenden Beispiel werden die Zugriffstasten für die Registerkarte **Seitenlayout** in **Word** gezeigt.

![Zugriffstasten für die Registerkarte „Seitenlayout“ in Word](images/input-patterns/accesskeys-show.png)

Hier wird der Textfeldwert „Einzug links“ hervorgehoben, nachdem die in der zugehörigen Beschriftung identifizierte Zugriffstaste eingegeben wurde.

![Der Textfeldwert „Einzug links“ wird hervorgehoben, nachdem die in der zugehörigen Beschriftung identifizierte Zugriffstaste eingegeben wurde.](images/input-patterns/accesskeys-entered.png)

## Benutzerfreundlichkeit und Barrierefreiheit


Eine gut durchdachte Tastaturinteraktionserfahrung ist ein wichtiger Aspekt für die Barrierefreiheit von Software. Sie ermöglicht es Benutzern mit einer Sehbeeinträchtigung oder mit bestimmten motorischen Einschränkungen, in einer App zu navigieren und mit deren Features zu interagieren. Diese Benutzer können u.U. keine Maus bedienen und sind auf verschiedene Hilfstechnologien wie Tastaturerweiterungstools, Bildschirmtastaturen, Bildschirmlupen, Bildschirmleseprogramme oder die Möglichkeit der Spracheingabe angewiesen. Für diese Benutzer ist Vollständigkeit wichtiger als Konsistenz.

Erfahrene Benutzer haben oftmals eine starke Vorliebe für die Verwendung der Tastatur, da tastaturbasierte Befehle viel schneller eingegeben werden können. Zudem ist es dafür nicht erforderlich, die Hände von der Tastatur wegzubewegen. Für diese Benutzer sind Effizienz und Konsistenz entscheidend. Die Vollständigkeit hingegen ist nur für am häufigsten verwendeten Befehle wichtig.

Es gibt geringfügige Unterschiede beim Entwerfen von Elementen für die Benutzerfreundlichkeit und Barrierefreiheit. Daher werden zwei unterschiedliche Tastaturzugriffsmechanismen unterstützt.

Zugriffstasten weisen die folgenden Merkmale auf:

-   Zugriffstasten ermöglichen den schnellen Zugriff auf UI-Elemente in Ihrer App.
-   Sie verwenden ALT und eine alphanumerische Taste.
-   Sie dienen in erster Linie der Barrierefreiheit.
-   Sie sind allen Menüs und den meisten Dialogfeldsteuerelementen zugewiesen.
-   Ihre Speicherung ist nicht vorgesehen, daher werden sie direkt in der UI dokumentiert, indem das entsprechende Steuerelement-Beschriftungszeichen unterstrichen wird.
-   Sie wirken sich nur auf das aktuelle Fenster aus und navigieren zum entsprechenden Menüelement oder Steuerelement.
-   Sie werden nicht konsistent zugewiesen, da dies nicht immer möglich ist. Zugriffstasten sollten jedoch für die am häufigsten verwendeten Befehle, insbesondere für Schaltflächen für den Commit, konsistent zugewiesen werden.
-   Sie sind lokalisiert.

Da die Speicherung von Zugriffstasten nicht vorgesehen ist, werden sie einem Zeichen zugewiesen, das sich am Anfang der Beschriftung befindet, um es einfacher finden zu können, und zwar selbst dann, wenn später in der Beschriftung ein Schlüsselwort vorhanden ist.

Demgegenüber weisen Tastenkombinationen die folgenden Merkmale auf:

-   Eine Tastenkombination ist eine Verknüpfung zu einem App-Befehl.
-   Sie verwenden primär die STRG- und Funktionstastensequenzen (Windows-Systemtastenkombinationen verwenden ebenfalls ALT in Verbindung mit nicht alphanumerischen Tasten und der Windows-Logo-Taste).
-   Sie dienen in erster Linie erweiterten Benutzer hinsichtlich Effizienz.
-   Sie werden nur den am häufigsten verwendeten Befehlen zugewiesen.
-   Ihre Speicherung ist vorgesehen, und sie werden nur in Menüs, QuickInfos und in der Hilfe dokumentiert.
-   Sie wirken sich auf das gesamte Programm aus, sie haben jedoch keine Auswirkung, wenn sie nicht angewendet werden.
-   Sie müssen konsistent zugewiesen werden, da sie gespeichert und nicht direkt dokumentiert werden.
-   Sie sind nicht lokalisiert.

Da die Speicherung von Tastenkombinationen vorgesehen ist, verwenden die am häufigsten verwendeten Tastenkombinationen idealerweise Buchstaben vom ersten oder die einprägsamsten Zeichen des Schlüsselworts im Befehl, beispielsweise STRG+C für Kopiervorgänge und STRG+Q für Anforderungen.

Die Benutzer müssen in der Lage sein, alle von der App unterstützten Funktionen nur mithilfe der Hardwaretastatur oder der Bildschirmtastatur auszuführen.

Benutzern, die auf Sprachausgabe und andere Hilfstechnologien angewiesen sind, sollte eine einfache Möglichkeit geboten werden, die Tastenkombinationen der App zu erkennen. Weisen Sie mithilfe von QuickInfos, Namen und Beschreibungen zur Verwendung durch Bildschirmleseprogramme oder anderen Hinweisen auf dem Bildschirm auf Tastenkombinationen hin. Zumindest sollten die Zugriffstasten und Tastenkombinationen im Hilfeinhalt Ihrer App gut dokumentiert sein.

Weisen Sie keine bekannten oder standardmäßigen Tastenkombinationen zu anderen Funktionen zu. Beispielsweise wird STRG+F in der Regel für die Suche verwendet.

Versuchen Sie nicht, allen interaktiven Steuerelementen in einer dichten UI zuzuweisen. Stellen Sie hingegen einfach sicher, dass die wichtigsten und am häufigsten verwendeten Befehle Zugriffstasten aufweisen, oder verwenden Sie Steuerelementgruppen, und weisen Sie der Steuerelement-Gruppenbeschriftung eine Zugriffstaste zu.

Ändern Sie Befehle nicht mithilfe von Tastaturmodifizierern. Dies ist nicht auffindbar und kann Verwirrung stiften.

Deaktivieren Sie ein Steuerelement nicht, wenn es den Eingabefokus besitzt. Dies kann die Tastatureingabe stören.

Zum Gewährleisten von erfolgreichen Tastaturinteraktionserfahrungen ist es von entscheidender Bedeutung, die App sorgfältig und exklusiv mit der Tastatur zu testen.

## Texteingabe


Fragen Sie immer die Gerätefunktionen ab, wenn Sie sich auf die Tastatureingabe verlassen. Auf einigen Geräten (wie Telefonen) kann die Touch-Bildschirmtastatur nur für Texteingaben verwendet werden, da sie viele der auf der Hardwaretastatur (wie ALT, die Funktionstasten oder die Windows-Logo-Taste) vorhandenen Tastenkombinationen oder Befehlstasten nicht bereitstellt.

Geben Sie Benutzer nicht die Möglichkeit, die Touch-Bildschirmtastatur zum Navigieren in der App zu verwenden. In Abhängigkeit des Steuerelements, das den Fokus erhält, wird die Touch-Bildschirmtastatur möglicherweise geschlossen.

Versuchen Sie, die Tastatur während der gesamten Interaktion mit dem Formular anzuzeigen. Dies beseitigt Benutzeroberflächenirritationen, die Benutzer in der Mitte eines Formulars oder eines Texteingabeflusses verwirren könnten.

Stellen Sie sicher, dass Benutzer das Eingabefeld, in das sie Text eingeben, immer sehen können. Die Touch-Bildschirmtastatur verdeckt die Hälfte des Bildschirms. Daher sollte das Eingabefeld mit dem Fokus angezeigt werden, wenn der Benutzer das Formular durchläuft.

Eine Standardhardwaretastatur oder OSK besteht aus sieben Tastentypen, wobei jeder Typ eine eindeutige Funktion unterstützt:

-   Zeichentaste: Sendet ein literales Zeichen an das Fenster mit dem Eingabefokus.
-   Zusatztaste: Ändert beim gleichzeitigen Drücken die Funktion einer primären Taste, beispielsweise STRG, ALT, UMSCHALT und die Windows-Logo-Taste.
-   Navigationstaste: Verschiebt den Eingabefokus oder die Texteingabeposition, beispielsweise TAB, POS1, ENDE, BILD-AUF, BILD-AB und direktionale Pfeiltasten.
-   Bearbeitungstaste: Ändert Text, beispielsweise die Tasten UMSCHALT, TAB, EINGABETASTE, EINFG, RÜCKTASTE und ENTF.
-   Funktionstaste: Führt eine spezielle Funktion aus, beispielsweise die Tasten F1–F12.
-   Umschalttaste: Versetzt das System in einen Modus, beispielsweise die Tasten FESTSTELLTASTE, ROLLEN und NUM-TASTE.
-   Befehlstaste: Führt eine Systemaufgabe oder Befehlsaktivierung aus, beispielsweise die Tasten LEERTASTE, EINGABETASTE, ESC, PAUSE/UNTBR und DRUCKEN.

Zusätzlich zu diesen Kategorien ist eine sekundäre Klasse von Tasten und Tastenkombinationen vorhanden, die als Verknüpfungen zu einer App-Funktion verwendet werden können.

-   Zugriffstaste: Stellt Steuerelemente oder Menüelemente zur Verfügung, indem die ALT-TASTE zusammen mit einer Zeichentaste gedrückt wird. Auf sie wird durch die Unterstreichung der Zugriffstasten-Zeichenzuweisung in einem Menü oder das Anzeigen des bzw. der Zugriffstastenzeichen(s) in einer Überlagerung hingewiesen.
-   Tastenkombination: Stellt App-Befehle zur Verfügung, indem eine Funktionstaste oder die STRG-TASTE mit einer Zeichentaste verwendet wird. Ihr App kann UI-Elemente aufweisen, die dem Befehl entsprechen.

Eine weitere Klasse von Tastenkombinationen, die so genannte SAS (Secure Attention Sequence), kann nicht von einer App abgefangen werden. Dieses Sicherheitsfeature dient zum Schutz des Systems des Benutzers bei der Anmeldung und enthält STRG-ALT-ENTF und WINDOWS-L.

Die Editor-App wird hier mit dem erweiterten Dateimenü gezeigt, das Zugriffstasten und Tastenkombinationen enthält.

![Die Editor-App mit einem erweiterten Dateimenü, das Zugriffstasten und Tastenkombinationen enthält.](images/input-patterns/notepad.png)

## Tastaturbefehle


Im Folgenden finden Sie eine vollständige Liste der Tastaturinteraktionen, die über verschiedene Geräte hinweg bereitgestellt werden, die die Tastatureingabe unterstützen. Für einige Geräte und Plattformen sind systemeigene Tastaturanschläge und Interaktionen erforderlich, wobei diese nicht notiert sind.

Verwenden Sie beim Entwerfen von benutzerdefinierten Steuerelementen und Interaktionen diese Tastatursprache konsistent, damit Ihre App intuitiv, verlässlich und einfach zu erlernen ist.

Definieren Sie nicht die Standardtastaturverknüpfungen.

In den folgenden Tabellen sind häufig verwendete Tastaturbefehle aufgeführt. Eine vollständige Liste der Tastaturbefehle erhalten Sie unter [Tastenkombinationen in Windows](http://go.microsoft.com/fwlink/p/?linkid=325424).

**Navigationsbefehle**

| Aktion                               | Tastenkombination                                      |
|--------------------------------------|--------------------------------------------------|
| Zurück                                 | ALT+NACH-LINKS-TASTE oder die RÜCKTASTE auf speziellen Tastaturen |
| Vorwärts                              | ALT+NACH-RECHTS-TASTE                                        |
| Nach oben                                   | ALT+NACH-OBEN-TASTE                                           |
| Abbrechen oder Modus beenden   | ESC                                              |
| Durch die Elemente in einer Liste navigieren         | Pfeiltaste (NACH-LINKS, NACH-RECHTS, NACH-OBEN, NACH-UNTEN)                |
| Zur nächsten Elementliste springen           | STRG+NACH-LINKS-TASTE                                        |
| Semantischer Zoom                        | STRG+PLUS-TASTE oder STRG+MINUS-TASTE                                 |
| Zu einem benannten Element in einer Sammlung springen | Einen Elementnamen eingeben                           |
| Nächste Seite                            | BILD-AUF, BILD-AB oder LEERTASTE                   |
| Nächste Registerkarte                             | STRG+TAB                                         |
| Vorherige Registerkarte                         | STRG+UMSCHALT+TAB                                   |
| App-Leiste öffnen                         | Windows+Z                                        |
| Ein Element aktivieren oder in ein Element navigieren    | EINGABETASTE                                            |
| Auswählen                               | LEERTASTE                                         |
| Fortlaufend auswählen                  | UMSCHALT+Pfeiltaste                                  |
| Alle auswählen                           | STRG+A                                           |

 

**Gebräuchliche Befehle**

| Aktion                                                 | Tastenkombination     |
|--------------------------------------------------------|-----------------|
| Ein Element anheften                                            | STRG+UMSCHALT+1    |
| Speichern                                                   | STRG+S          |
| Suchen                                                   | STRG+F          |
| Drucken                                                  | STRG+P          |
| Kopieren                                                   | STRG+C          |
| Ausschneiden                                                    | STRG+X          |
| Neues Element                                               | STRG+N          |
| Einfügen                                                  | STRG+V          |
| Öffnen                                                   | STRG+O          |
| Adresse öffnen (z.B. eine URL in Internet Explorer) | STRG+L oder ALT+D |

 

**Navigationsbefehle für Medien**

| Aktion       | Tastenkombination |
|--------------|-------------|
| Wiedergabe/Anhalten   | STRG+P      |
| Nächstes Element    | STRG+F      |
| Vorschau des Elements anzeigen | STRG+B      |

 

Hinweis: Die Tastenbefehle für die Mediennavigation wie "Wiedergeben"/"Anhalten" oder "Nächstes Element" entsprechen den jeweiligen Tastenkombinationen für "Drucken" und "Suchen". Häufig verwendete Befehle sollten Priorität gegenüber Navigationsbefehlen für Medien haben. Wenn eine App z.B. sowohl das Wiedergeben von Medien als auch das Drucken unterstützt, sollte die Tastenkombination STRG+P den Druckvorgang starten.
## Visuelles Feedback


Verwenden Sie Fokusrechtecke nur bei Tastaturinteraktionen. Wenn der Benutzer eine Touch-Interaktion auslöst, blenden Sie die für Tastaturinteraktionen spezifische UI schrittweise aus. Somit bleibt die Benutzeroberfläche sauber und aufgeräumt.

Zeigen Sie kein visuelles Feedback an, wenn ein Element keine Interaktionen unterstützt (z.B. statischer Text). Beachten Sie, dass die Benutzeroberfläche dadurch sauber und aufgeräumt bleibt.

Versuchen Sie, für alle Elemente, die das gleiche Eingabeziel darstellen, das gleiche visuelle Feedback anzuzeigen.

Versuchen Sie, Schaltflächen (z.B. „+“ und „-“) bereitzustellen, um Hinweise für touchbasierte Manipulationen wie etwa Schwenken, Drehen, Zoomen usw. zu emulieren.

Allgemeine Informationen zum visuellen Feedback finden Sie unter [Richtlinien für visuelles Feedback](guidelines-for-visualfeedback.md).


## Tastaturereignisse und Fokus


Die folgenden Tastaturereignisse können sowohl für Hardware- als auch für Touch-Bildschirmtastaturen eintreten.

| Ereignis                                      | Beschreibung                    |
|--------------------------------------------|--------------------------------|
| [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) | Tritt auf, wenn eine Taste gedrückt wird.  |
| [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)     | Tritt auf, wenn eine Taste losgelassen wird. |


**Wichtig**  
Einige Windows-Runtime-Steuerelemente verarbeiten Eingabeereignisse intern. In diesen Fällen tritt ein Eingabeereignis möglicherweise nicht ein, weil der Ereignislistener den zugehörigen Handler nicht aufruft. Normalerweise wird diese Tastenteilmenge vom Klassenhandler verarbeitet, um eine integrierte Unterstützung für die Barrierefreiheit des einfachen Tastaturzugriffs bereitzustellen. Die Klasse [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) setzt beispielsweise die [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982)-Ereignisse für die LEERTASTE und die EINGABETASTE (sowie für [**OnPointerPressed**](https://msdn.microsoft.com/library/windows/apps/hh967989)) außer Kraft und leitet sie an das Ereignis [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) des Steuerelements weiter. Wenn von der Steuerelementklasse ein Tastendruck verarbeitet wird, werden die Ereignisse [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) und [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) nicht ausgelöst.

Dadurch wird ein integriertes Tastaturäquivalent zum Aufrufen der Schaltfläche bereitgestellt, das dem Tippen mit dem Finger oder Klicken mit einer Maus ähnelt. Andere Tasten als die LEER- oder EINGABETASTE lösen die Ereignisse [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) und [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) trotzdem aus. Weitere Informationen zur Funktionsweise der klassenbasierten Verarbeitung von Ereignissen (insbesondere den Abschnitt „Eingabe-Ereignishandler in Steuerelementen“) finden Sie unter [Übersicht über Ereignisse und Routingereignisse](https://msdn.microsoft.com/library/windows/apps/mt185584).


Die Steuerelemente der UI generieren nur dann Tastaturereignisse, wenn sie den Eingabefokus aufweisen. Ein einzelnes Steuerelement steht im Fokus, wenn Benutzer im Layout auf das Steuerelement klicken oder tippen oder mit der TAB-Taste im Inhaltsbereich eine Aktivierreihenfolge durchlaufen.

Sie können auch die [**Focus**](https://msdn.microsoft.com/library/windows/apps/hh702161)-Methode eines Steuerelements aufrufen, um den Fokus zu erzwingen. Dies ist notwendig, wenn Sie Tastenkombinationen implementieren, da der Tastaturfokus beim Laden der UI nicht standardmäßig festgelegt wird. Weitere Informationen finden Sie weiter unten in diesem Thema unter [Beispiel für Tastenkombinationen](#shortcut_keys_example).

Damit ein Steuerelement den Eingabefokus erhält, muss es aktiviert und sichtbar sein. Außerdem müssen die Eigenschaften [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/br209422) und [**HitTestVisible**](https://msdn.microsoft.com/library/windows/apps/br208933) den Wert **true** haben. Dies ist der Standardzustand der meisten Steuerelemente. Wenn ein Steuerelement den Eingabefokus aufweist, kann es Tastatureingabeereignisse auslösen und auf diese reagieren. Dies wird weiter unten in diesem Thema beschrieben. Mit den Ereignissen [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/br208927) und [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/br208943) können Sie außerdem reagieren, wenn ein Steuerelement den Fokus erhält oder verliert.

Standardmäßig entspricht die Aktivierreihenfolge von Steuerelementen der Reihenfolge, in der sie in der Extensible Application Markup Language (XAML) angezeigt werden. Mit der Eigenschaft [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/br209461) können Sie diese Reihenfolge jedoch ändern. Weitere Informationen finden Sie unter [Implementieren von Barrierefreiheit für den Tastaturzugriff](https://msdn.microsoft.com/library/windows/apps/hh868161).

## Tastaturereignishandler


Ein Eingabe-Ereignishandler implementiert einen Delegaten, der die folgenden Informationen bereitstellt:

-   Den Sender des Ereignisses. Der Sender meldet das Objekt, dem der Ereignishandler angefügt ist.
-   Ereignisdaten. Bei Tastaturereignissen sind diese Daten eine Instanz von [**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072). [**KeyEventHandler**](https://msdn.microsoft.com/library/windows/apps/br227904) ist der Delegat für Handler. Die wichtigsten Eigenschaften von **KeyRoutedEventArgs** für die meisten Handler-Szenarien sind [**Key**](https://msdn.microsoft.com/library/windows/apps/hh943074) und möglicherweise [**KeyStatus**](https://msdn.microsoft.com/library/windows/apps/hh943075).
-   [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810). Da es sich bei Tastaturereignissen um Routingereignisse handelt, stellen die Ereignisdaten **OriginalSource** bereit. Wenn Sie bewusst das Bubbling von Ereignissen in der Objektstruktur zulassen, ist **OriginalSource** manchmal eher das fragliche Objekt als der Sender. Das hängt jedoch vom Design ab. Weitere Informationen dazu, wie Sie **OriginalSource** anstelle des Senders verwenden können, finden Sie im Abschnitt „Routingereignisse der Tastatur“ in diesem Thema oder unter [Übersicht über Ereignisse und Routingereignisse](https://msdn.microsoft.com/library/windows/apps/mt185584).

### Anfügen eines Tastaturereignishandlers

Sie können Funktionen von Tastatur-Ereignishandlern für jedes Objekt anfügen, das das Ereignis als Member einschließt. Dazu gehört jede von [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) abgeleitete Klasse. Das folgende XAML-Beispiel zeigt, wie Sie Handler für das Ereignis [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) für [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) anfügen.

```XAML
<Grid KeyUp="Grid_KeyUp">
  ...
</Grid>
```

Sie können einen Ereignishandler auch manuell im Code anfügen. Weitere Informationen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](https://msdn.microsoft.com/library/windows/apps/mt185584).

### Definieren eines Tastaturereignishandlers

Das folgende Beispiel zeigt eine unvollständige Ereignishandler-Definition für den im vorherigen Beispiel hinzugefügten Ereignishandler [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942).

```CSharp
void Grid_KeyUp(object sender, KeyRoutedEventArgs e)
{
    //handling code here
}
```

```VisualBasic
Private Sub Grid_KeyUp(ByVal sender As Object, ByVal e As KeyRoutedEventArgs)
    &#39;handling code here
End Sub
```

```ManagedCPlusPlus
void MyProject::MainPage::Grid_KeyUp(
  Platform::Object^ sender,
  Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{//handling code here}
```

### Verwenden von KeyRoutedEventArgs

Alle Tastaturereignisse verwenden [**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072) für Ereignisdaten. **KeyRoutedEventArgs** enthält die folgenden Eigenschaften:

-   [**Key**](https://msdn.microsoft.com/library/windows/apps/hh943074)
-   [**KeyStatus**](https://msdn.microsoft.com/library/windows/apps/hh943075)
-   [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073)
-   [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810) (geerbt von [**RoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br208809))

### Key

Das Ereignis [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) wird ausgelöst, wenn eine Taste gedrückt wird. Entsprechend wird das Ereignis [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) ausgelöst, wenn eine Taste losgelassen wird. In der Regel lauschen Sie auf Ereignisse, um einen bestimmten Tastenwert zu verarbeiten. Überprüfen Sie den Wert [**Key**](https://msdn.microsoft.com/library/windows/apps/hh943074) in den Ereignisdaten, um die gedrückte oder losgelassene Taste zu ermitteln. **Key** gibt einen [**VirtualKey**](https://msdn.microsoft.com/library/windows/apps/br241812)-Wert zurück. Die **VirtualKey**-Enumeration umfasst alle unterstützten Tasten.

### Zusatztasten

Zusatztasten sind Tasten wie beispielsweise STRG oder UMSCHALT, die Benutzer normalerweise in Kombination mit anderen Tasten drücken. Ihre App kann diese Kombinationen als Tastenkombinationen zum Aufrufen von App-Befehlen nutzen.

Sie erkennen Tastenkombinationen mithilfe des Codes in den Ereignishandlern [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) und [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942). Sie können dann den Zustand "Gedrückt" der Zusatztasten überwachen, die für Sie von Interesse sind. Wenn für eine Nichtzusatztaste ein Tastaturereignis auftritt, können Sie überprüfen, ob gleichzeitig eine Zusatztaste gedrückt wurde.

**Hinweis**: Die ALT-Taste wird durch den Wert **VirtualKey.Menu** dargestellt.

 

## Beispiel für Tastenkombinationen


Im folgenden Beispiel wird die Implementierung von Tastenkombinationen gezeigt. In diesem Beispiel können Benutzer die Medienwiedergabe mit den Schaltflächen „Play“, „Pause“ und „Stop“ oder mit den Tastenkombinationen STRG+P, STRG+A und STRG+S steuern. Das Schaltflächen-XAML zeigt die Tastenkombinationen in Form von QuickInfos und [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/br209081)-Eigenschaften in den Schaltflächenbeschriftungen. Diese Selbstdokumentation ist wichtig, um die Benutzerfreundlichkeit und Barrierefreiheit der App zu erhöhen. Weitere Informationen finden Sie unter [Barrierefreiheit für den Tastaturzugriff](https://msdn.microsoft.com/library/windows/apps/mt244347).

Beachten Sie auch, dass die Seite den Eingabefokus auf sich selbst festlegt, wenn sie geladen wird. Ohne diesen Schritt hat kein Steuerelement anfangs den Eingabefokus, und die App löst erst Eingabeereignisse aus, wenn Benutzer den Eingabefokus manuell festlegen (beispielsweise durch Drücken der TAB-Taste oder Klicken auf ein Steuerelement).

```XAML
<Grid KeyDown="Grid_KeyDown">

  <Grid.RowDefinitions>
    <RowDefinition Height="Auto" />
    <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>

  <MediaElement x:Name="DemoMovie" Source="xbox.wmv"
    Width="500" Height="500" Margin="20" HorizontalAlignment="Center" />

  <StackPanel Grid.Row="1" Margin="10"
    Orientation="Horizontal" HorizontalAlignment="Center">

    <Button x:Name="PlayButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+P"
      AutomationProperties.AcceleratorKey="Control P">
      <TextBlock>Play</TextBlock>
    </Button>

    <Button x:Name="PauseButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+A"
      AutomationProperties.AcceleratorKey="Control A">
      <TextBlock>Pause</TextBlock>
    </Button>

    <Button x:Name="StopButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+S"
      AutomationProperties.AcceleratorKey="Control S">
      <TextBlock>Stop</TextBlock>
    </Button>

  </StackPanel>

</Grid>
```

```ManagedCPlusPlus
//showing implementations but not header definitions
void MainPage::OnNavigatedTo(NavigationEventArgs^ e)
{
    (void) e;    // Unused parameter
    this->Loaded+=ref new RoutedEventHandler(this,&amp;MainPage::ProgrammaticFocus);
}
void MainPage::ProgrammaticFocus(Object^ sender, RoutedEventArgs^ e) {
    this->Focus(Windows::UI::Xaml::FocusState::Programmatic);
}

void KeyboardSupport::MainPage::MediaButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    FrameworkElement^ fe = safe_cast<FrameworkElement^>(sender);
    if (fe->Name == "PlayButton") {DemoMovie->Play();}
    if (fe->Name == "PauseButton") {DemoMovie->Pause();}
    if (fe->Name == "StopButton") {DemoMovie->Stop();}
}


void KeyboardSupport::MainPage::Grid_KeyDown(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (e->Key == VirtualKey::Control) isCtrlKeyPressed = true;
}


void KeyboardSupport::MainPage::Grid_KeyUp(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (e->Key == VirtualKey::Control) isCtrlKeyPressed = true;
    else if (isCtrlKeyPressed) {
        if (e->Key==VirtualKey::P) {
            DemoMovie->Play();
        }
        if (e->Key==VirtualKey::A) {DemoMovie->Pause();}
        if (e->Key==VirtualKey::S) {DemoMovie->Stop();}
    }
}
```

```CSharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Set the input focus to ensure that keyboard events are raised.
    this.Loaded += delegate { this.Focus(FocusState.Programmatic); };
}

private void Grid_KeyUp(object sender, KeyRoutedEventArgs e)
{
    if (e.Key == VirtualKey.Control) isCtrlKeyPressed = false;
}

private void Grid_KeyDown(object sender, KeyRoutedEventArgs e)
{
    if (e.Key == VirtualKey.Control) isCtrlKeyPressed = true;
    else if (isCtrlKeyPressed)
    {
        switch (e.Key)
        {
            case VirtualKey.P: DemoMovie.Play(); break;
            case VirtualKey.A: DemoMovie.Pause(); break;
            case VirtualKey.S: DemoMovie.Stop(); break;
        }
    }
}

private void MediaButton_Click(object sender, RoutedEventArgs e)
{
    switch ((sender as Button).Name)
    {
        case "PlayButton": DemoMovie.Play(); break;
        case "PauseButton": DemoMovie.Pause(); break;
        case "StopButton": DemoMovie.Stop(); break;
    }
}
```

```VisualBasic
Private isCtrlKeyPressed As Boolean
Protected Overrides Sub OnNavigatedTo(e As Navigation.NavigationEventArgs)

End Sub

Private Sub Grid_KeyUp(sender As Object, e As KeyRoutedEventArgs)
    If e.Key = Windows.System.VirtualKey.Control Then
        isCtrlKeyPressed = False
    End If
End Sub

Private Sub Grid_KeyDown(sender As Object, e As KeyRoutedEventArgs)
    If e.Key = Windows.System.VirtualKey.Control Then isCtrlKeyPressed = True
    If isCtrlKeyPressed Then
        Select Case e.Key
            Case Windows.System.VirtualKey.P
                DemoMovie.Play()
            Case Windows.System.VirtualKey.A
                DemoMovie.Pause()
            Case Windows.System.VirtualKey.S
                DemoMovie.Stop()
        End Select
    End If
End Sub

Private Sub MediaButton_Click(sender As Object, e As RoutedEventArgs)
    Dim fe As FrameworkElement = CType(sender, FrameworkElement)
    Select Case fe.Name
        Case "PlayButton"
            DemoMovie.Play()
        Case "PauseButton"
            DemoMovie.Pause()
        Case "StopButton"
            DemoMovie.Stop()
    End Select
End Sub
```

**Hinweis**: Durch das Festlegen der Eigenschaften [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/hh759762) oder [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/hh759763) in XAML werden Zeichenfolgeninformationen zum Dokumentieren der Tastenkombination zum Auslösen der jeweiligen Aktion bereitgestellt. Die Informationen werden von Microsoft-Benutzeroberflächenautomatisierungsclients wie der Sprachausgabe erfasst und normalerweise direkt für den Benutzer bereitgestellt. Mit dem Festlegen der Eigenschaft **AutomationProperties.AcceleratorKey** oder **AutomationProperties.AccessKey** ist keine eigene Aktion verknüpft. Sie müssen weiterhin Handler für [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941)- oder [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)-Ereignisse anhängen, um das Verhalten der Tastenkombination tatsächlich in die App zu implementieren. Außerdem wird der Unterstrichzusatz für eine Zugriffstaste nicht automatisch bereitgestellt. Sie müssen den Text für die jeweilige Taste in Ihrem mnemonischen Zeichen explizit als [**Underline**](https://msdn.microsoft.com/library/windows/apps/br209982)-Formatierung unterstreichen, wenn in der UI unterstrichener Text angezeigt werden soll.

 

## Routingereignisse der Tastatur


Bestimmte Ereignisse gelten als Routingereignisse, wie z.B. [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) und [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942). Routingereignisse verwenden die Bubbling-Routingstrategie. Bei der Bubbling-Routingstrategie geht ein Ereignis von einem untergeordneten Objekt aus und wird jeweils an die übergeordneten Objekte in der Struktur weitergeleitet. Dadurch ergibt sich eine weitere Möglichkeit, dasselbe Ereignis zu behandeln und mit denselben Ereignisdaten zu interagieren.

Im folgenden XAML-Beispiel werden [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)-Ereignisse für ein [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)-Objekt und zwei [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)-Objekte definiert. Wenn Sie in diesem Fall eine Taste loslassen, während der Fokus auf einem der **Button**-Objekte liegt, wird das Ereignis **KeyUp** ausgelöst. Das Ereignis wird per Bubbling an die übergeordnete **Canvas** weitergeleitet.

```XAML
<StackPanel KeyUp="StackPanel_KeyUp">
  <Button Name="ButtonA" Content="Button A"/>
  <Button Name="ButtonB" Content="Button B"/>
  <TextBlock Name="statusTextBlock"/>
</StackPanel>
```

Das folgende Beispiel zeigt, wie der Ereignishandler [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) für den entsprechenden XAML-Inhalt im vorherigen Beispiel implementiert wird.

```CSharp
void StackPanel_KeyUp(object sender, KeyRoutedEventArgs e)
{
    statusTextBlock.Text = String.Format(
        "The key {0} was pressed while focus was on {1}",
        e.Key.ToString(), (e.OriginalSource as FrameworkElement).Name);
}
```

Beachten Sie die Verwendung der Eigenschaft [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810) im vorherigen Handler. Hier meldet **OriginalSource** das Objekt, das das Ereignis ausgelöst hat. Bei dem Objekt kann es sich nicht um [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635) handeln, da **StackPanel** kein Steuerelement ist und nicht im Fokus stehen kann. Nur eine der beiden Schaltflächen in **StackPanel** kann das Ereignis ausgelöst haben. Die Frage ist, welche? Mit **OriginalSource** können Sie das tatsächliche Quellobjekt des Ereignisses unterscheiden, wenn Sie das Ereignis für ein übergeordnetes Objekt verarbeiten.

### Die Handled-Eigenschaft in Ereignisdaten

Je nach Ihrer Strategie für die Ereignisverarbeitung empfiehlt sich vielleicht nur ein Ereignishandler, der auf ein Bubbling-Ereignis reagiert. Wenn beispielsweise ein bestimmter [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)-Handler an eines der [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)-Steuerelemente angefügt ist, hat dieses zuerst Gelegenheit, das Ereignis zu verarbeiten. In diesem Fall ist es nicht sinnvoll, dass auch das übergeordnete Panel das Ereignis behandelt. Verwenden Sie in diesem Szenario die Eigenschaft [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) in den Ereignisdaten.

Der Zweck der Eigenschaft [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) in der Datenklasse eines Routingereignisses besteht darin, zu melden, dass ein anderer Handler, den Sie an früherer Stelle in der Ereignisroute registriert haben, bereits tätig war. Dies beeinflusst das Verhalten des Routingereignissystems. Wenn Sie **Handled** in einem Ereignishandler auf **true** festlegen, ist das Routing eines Ereignisses beendet, und das Ereignis wird nicht mehr an nachfolgende übergeordnete Elemente gesendet.

### "AddHandler " und bereits behandelte Tastaturereignisse

Sie können eine spezielle Technik verwenden, um Handler anzufügen, die auf bereits als verarbeitet markierte Ereignisse reagieren können. Bei dieser Technik wird mit der Methode [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399) ein Handler registriert, und es werden keine XAML-Attribute und keine sprachspezifische Syntax verwendet, die Handler wie „+=“ in C\# hinzufügen. Ein Nachteil dieser Technik ist im Allgemeinen, dass die API **AddHandler** einen Parameter vom Typ [**RoutedEvent**](https://msdn.microsoft.com/library/windows/apps/br208808) verwendet, der das fragliche Routingereignis identifiziert. Nicht alle Routingereignisse bieten einen **RoutedEvent**-Bezeichner. Dieser Umstand wirkt sich somit darauf aus, welche Routingereignisse im Fall [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) noch verarbeitet werden können. Die Ereignisse [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) und [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) haben Routingereignisbezeichner ([**KeyDownEvent**](https://msdn.microsoft.com/library/windows/apps/hh702416) und [**KeyUpEvent**](https://msdn.microsoft.com/library/windows/apps/hh702418)) in [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911). Andere Texteingabe-Ereignisse, wie [**TextBox.TextChanged**](https://msdn.microsoft.com/library/windows/apps/br209706), besitzen jedoch keine Routingereignisbezeichner und können deshalb für die Technik **AddHandler** nicht verwendet werden.

## Steuerung


Eine geringe Anzahl von UI-Elementen verfügt über integrierte Unterstützung für die Steuerung. Die Steuerung verwendet eingabebezogene Routingereignisse in der zugrundeliegenden Implementierung. Sie ermöglicht die Verarbeitung verwandter UI-Eingaben (eine bestimmte Zeigeraktion oder Zugriffstaste) durch das Aufrufen eines einzelnen Befehlshandlers.

Wenn die Steuerung für ein UI-Element verfügbar ist, sollten Sie dessen Steuerungs-APIs anstelle einzelner Eingabeereignisse verwenden. Weitere Informationen finden Sie unter [**ButtonBase.Command**](https://msdn.microsoft.com/library/windows/apps/br227740).

Sie können auch [**ICommand**](https://msdn.microsoft.com/library/windows/apps/br227885) implementieren, um die Befehlsfunktionalität zu kapseln, die Sie über normale Ereignishandler aufrufen. Auf diese Weise können Sie die Steuerung auch verwenden, wenn keine **Command**-Eigenschaft verfügbar ist.

## Texteingabe und Steuerelemente


Bestimmte Steuerelemente reagieren mit einer eigenen Verarbeitung auf Tastaturereignisse. So ist beispielsweise das Steuerelement [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) dazu gedacht, über die Tastatur eingegebenen Text zu erfassen und visuell darzustellen. Es verwendet in seiner eigenen Logik [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) und [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) zum Erfassen von Tastaturanschlägen und löst dann auch sein eigenes [**TextChanged**](https://msdn.microsoft.com/library/windows/apps/br209706)-Ereignis aus, wenn der Text tatsächlich geändert wurde.

Trotzdem können Sie generell einer [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br208942) oder einem verwandten Steuerelement, das der Verarbeitung von Texteingaben dient, Handler für [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208941) und [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br209683) hinzufügen. Jedoch reagiert ein Steuerelement möglicherweise im Rahmen seines Designs nicht auf alle Tastenwerte, die es über Tastaturereignisse empfängt. Das Verhalten hängt ganz vom jeweiligen Steuerelement ab.

Beispielsweise verarbeitet [**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/br227736) (die Basisklasse von [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)) [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) so, dass eine Überprüfung für die LEERTASTE oder für die EINGABETASTE durchgeführt werden kann. **ButtonBase** verarbeitet **KeyUp** analog zu einem Ereignis für das Drücken der linken Maustaste, um ein [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737)-Ereignis auszulösen. Diese Verarbeitung des Ereignisses wird beim Überschreiben der virtuellen Methode [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/hh967983) durch **ButtonBase** erreicht. In der Implementierung wird [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) auf **true** festgelegt. Als Ergebnis empfängt kein übergeordnetes Element einer Schaltfläche, die bei der LEERTASTE auf ein Tastaturereignis lauscht, das bereits verarbeitete Element für seine eigenen Handler.

[**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) ist ein weiteres Beispiel. Einige Tasten, wie die Pfeiltasten, werden von **TextBox** nicht als Text angesehen und gelten stattdessen als spezifisch für das UI-Steuerelementverhalten **TextBox** markiert diese Ereignisse als verarbeitet.

Benutzerdefinierte Steuerelemente können ein ähnliches eigenes Überschreibungsverhalten für Tastaturereignisse implementieren und [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982) / [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/hh967983) überschreiben. Wenn Ihr benutzerdefiniertes Steuerelement bestimmte Zugriffstasten verarbeitet oder ein Steuerelement- oder Fokusverhalten besitzt, das mit dem für [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) geschilderten Szenario vergleichbar ist, sollten Sie diese Logik in Ihre eigenen **OnKeyDown** / **OnKeyUp**-Überschreibungen aufnehmen.

## Die Touch-Bildschirmtastatur


Steuerelemente für die Texteingabe bieten automatische Unterstützung für die Touch-Bildschirmtastatur. Wenn Benutzer den Eingabefokus per Fingereingabe auf ein Textsteuerelement festlegen, wird automatisch die Bildschirmtastatur angezeigt. Wenn sich der Eingabefokus nicht auf einem Textsteuerelement befindet, ist die Bildschirmtastatur ausgeblendet.

Wenn die Bildschirmtastatur angezeigt wird, wird Ihre UI automatisch umpositioniert, um sicherzustellen, dass das Element mit dem Fokus sichtbar bleibt. Dies kann dazu führen, dass andere wichtige Bereiche der UI nicht mehr auf dem Bildschirm angezeigt werden. Sie können das Standardverhalten jedoch deaktivieren und eigene UI-Anpassungen vornehmen, wenn die Bildschirmtastatur eingeblendet wird. Weitere Informationen finden Sie im [Beispiel zur Reaktion auf die Anzeige der Bildschirmtastatur](http://go.microsoft.com/fwlink/p/?linkid=231633).

Wenn Sie ein benutzerdefiniertes Steuerelement erstellen, das zwar Texteingabe erfordert, aber nicht von einem Standardsteuerelement für die Texteingabe abgeleitet ist, können Sie Unterstützung für die Touch-Bildschirmtastatur hinzufügen, indem Sie die richtigen Steuerelementmuster der Benutzeroberflächenautomatisierung implementieren. Weitere Informationen finden Sie unter [Reagieren auf das Vorhandensein der Touch-Bildschirmtastatur](respond-to-the-presence-of-the-touch-keyboard.md) und im [Beispiel für die Touch-Bildschirmtastatur](http://go.microsoft.com/fwlink/p/?linkid=246019).

Durch Drücken von Tasten auf der Touch-Bildschirmtastatur werden genau wie beim Drücken von Tasten auf Hardwaretastaturen die Ereignisse [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) und [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) ausgelöst. Die Touch-Bildschirmtastatur löst jedoch keine Eingabeereignisse für STRG+A, STRG+Z, STRG+X, STRG+C und STRG+V aus, da diese Tastenkombinationen für Textänderungen im Eingabesteuerelement reserviert sind.

Benutzer können Daten in Ihre App schneller und einfacher eingeben, indem Sie den Eingabeumfang des Textsteuerelements an die Art der Daten anpassen, die der Benutzer vermutlich eingeben wird. Der Eingabeumfang bietet einen Hinweis auf die Art von Text, die vermutlich über das Steuerelement eingegeben wird. Auf diese Weise kann das System ein spezielles Bildschirmtastaturlayout für den Eingabetyp bereitstellen. Wird ein Textfeld beispielsweise nur verwendet, um eine vierstellige PIN einzugeben, legen Sie die Eigenschaft [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) auf [**Number**](https://msdn.microsoft.com/library/windows/apps/hh702028) fest. Dies weist das System an, das Layout der Zehnertastatur anzuzeigen, das dem Benutzer die Eingabe der PIN erleichtert. Weitere Informationen finden Sie unter [Verwenden des Eingabeumfangs zum Ändern der Bildschirmtastatur](https://msdn.microsoft.com/library/windows/apps/mt280229).


## Weitere Artikel in diesem Abschnitt
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Reagieren auf das Vorhandensein der Bildschirmtastatur](respond-to-the-presence-of-the-touch-keyboard.md)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie die UI Ihrer App beim Ein- oder Ausblenden der Touch-Bildschirmtastatur anpassen können.</p></td>
</tr>
</tbody>
</table>

 


## Verwandte Artikel


**Entwickler**
* [Identifizieren von Eingabegeräten](identify-input-devices.md)
* [Reagieren auf das Vorhandensein der Bildschirmtastatur](respond-to-the-presence-of-the-touch-keyboard.md)

**Designer**
* [Tastaturentwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/hh972345)

**Beispiele**
* [Beispiel für grundlegende Eingabe](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Beispiel für Eingabe mit niedriger Latenz](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Beispiel für visuelle Fokuselemente](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Archivbeispiele**
* [Eingabebeispiel](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Eingabe: Beispiel für Gerätefunktionen](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Eingabe: Beispiel für die Bildschirmtastatur](http://go.microsoft.com/fwlink/p/?linkid=246019)
* [Beispiel für die Reaktion auf die Anzeige der Bildschirmtastatur](http://go.microsoft.com/fwlink/p/?linkid=231633)
* [Beispiel für die XAML-Textbearbeitung](http://go.microsoft.com/fwlink/p/?LinkID=251417)
 

 



<!--HONumber=Aug16_HO3-->


