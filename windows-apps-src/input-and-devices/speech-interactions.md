---
Description: Description: Verwenden Sie zum Integrieren von Sprache in Ihre Apps Cortana-Sprachbefehle, Spracherkennung und Sprachsynthese.
title: title: Sprachinteraktionen
ms.assetid: 646DB3CE-FA81-4727-8C21-936C81079439
label: Speech interactions
template: detail.hbs
---

# ms.assetid: 646DB3CE-FA81-4727-8C21-936C81079439
label: Sprachinteraktionen template: detail.hbs

Sprachinteraktionen


**\[ Aktualisiert für UWP-Apps unter Windows 10.**

-   Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Integrieren Sie Spracherkennung und Text-zu-Sprache (auch bezeichnet als TTS oder Sprachsynthese) direkt in die Benutzeroberfläche Ihrer App. Weitere Sprachkomponenten

Falls Sie App-Funktionen in der **Cortana**-UI verfügbar machen, finden Sie in den [Cortana-Entwurfsrichtlinien](cortana-interactions.md) weitere Informationen. **Spracherkennung:  **Konvertiert vom Benutzer gesprochene Wörter in Text für die Formulareingabe, für die Diktierfunktion, zum Angeben einer Aktion oder eines Befehls und zum Ausführen von Aufgaben. Unterstützt werden sowohl vordefinierte Grammatiken für das Freitext-Diktieren und die Websuche als auch benutzerdefinierte Grammatiken, die mithilfe von Speech Recognition Grammar Specification (SRGS) Version 1.0 erstellt werden.

**TTS:  **Verwendet ein Sprachsynthesemodul (Sprache), um eine Textzeichenfolge in gesprochene Wörter zu konvertieren. Bei der Eingabezeichenfolge kann es sich um einfachen, schlichten Text oder komplexere Speech Synthesis Markup Language (SSML) handeln.
SSML stellt eine Standardmethode zum Steuern der Eigenschaften der Sprachausgabe bereit, z. B. Aussprache, Lautstärke, Stimmlage, Rate bzw. Geschwindigkeit und Betonung.

 

**Hinweis**  Mit **Cortana** und angepassten Sprachbefehlen kann Ihre App im Vordergrund gestartet (die App erhält wie beim Starten über das Menü „Start“ den Fokus) oder als Hintergrunddienst aktiviert werden (der Fokus bleibt auf **Cortana**, es werden aber Ergebnisse aus der App angezeigt).

## Befehle, die zusätzlichen Kontext oder Benutzereingaben erfordern (z. B. das Senden einer Nachricht an einen bestimmten Kontakt), lassen sich am besten in einer Vordergrund-App verarbeiten, während einfache Befehle in **Cortana** durch eine Hintergrund-App behandelt werden können.


Falls Sie Funktionen als Hintergrunddienst über Sprachbefehle in der **Cortana**-UI verfügbar machen, finden Sie in den [Cortana-Entwurfsrichtlinien](cortana-design-guidelines.md) weitere Informationen.

Wenn die Sprachkomponenten sorgfältig entworfen und implementiert werden, können sie den Benutzern eine stabile und angenehme Möglichkeit zur Interaktion mit Ihrer App bieten und zudem die Interaktion per Tastatur, Maus, Toucheingabe und Gesten ergänzen oder sogar ersetzen.

-   <span id="Speech_interaction_design"></span><span id="speech_interaction_design"></span><span id="SPEECH_INTERACTION_DESIGN"></span>Entwicklung von Spracheingabeinteraktionen Diese Richtlinien und Empfehlungen beschreiben, wie Sie Spracherkennung und TTS am besten in die Interaktionsfunktion Ihrer App integrieren.
-   Beachten Sie Folgendes, wenn Sie Sprachinteraktionen in Ihrer App unterstützen möchten:
-   Welche Aktionen können über die Spracheingabe ausgeführt werden?
-   Kann der Benutzer zwischen Seiten navigieren, Befehle aufrufen oder Daten als Textfelder, kurze Notizen oder lange Nachrichten eingeben?
-   Eignet sich die Spracheingabe zum Ausführen einer Aufgabe? Woher weiß der Benutzer, dass die Spracheingabe verfügbar ist?
-   Ist die App immer im Spracherkennungsmodus oder muss der Benutzer eine Aktion durchführen, damit sie in den Spracherkennungsmodus wechselt?
-   Durch welche Ausdrücke wird eine Aktion oder ein Verhalten initiiert?
-   Müssen die Ausdrücke und Aktionen auf dem Bildschirm aufgelistet werden?
-   Sind Bildschirme zur Eingabeaufforderung, Bestätigung und Begriffsklärung oder TTS erforderlich?

## Wie sieht der Interaktionsdialog zwischen App und Benutzer aus?


Ist für den Kontext Ihrer App benutzerdefiniertes oder eingeschränktes Vokabular erforderlich (z. B. medizinisches, wissenschaftliches oder gebietsspezifisches Vokabular)? Ist Netzwerkkonnektivität erforderlich? <span id="Text_input"></span><span id="text_input"></span><span id="TEXT_INPUT"></span>Texteingabe

Bei der Sprache für die Texteingabe kann es sich um Kurzformen (einzelne Wörter oder Ausdrücke) oder Langformen (fortlaufendes Diktat) handeln. Die Eingabe in Kurzform darf maximal zehn Sekunden dauern, während eine Sitzung zur Eingabe in Langform bis zu zwei Minuten lang sein kann.

(Die Eingabe in Langform kann ohne Eingreifen des Benutzers neu gestartet werden, damit der Eindruck eines fortlaufenden Diktats entsteht.)

Informieren Sie den Benutzer durch einen visuellen Hinweis darüber, dass die Spracherkennung unterstützt wird und ihm zur Verfügung steht, und geben Sie an, ob die Funktion vom Benutzer aktiviert werden muss.

Sie können z. B. eine Befehlsleistenschaltfläche mit einer Mikrofonglyphe (siehe [Befehlsleisten](../controls-and-patterns/app-bars.md)) verwenden, um die Verfügbarkeit und den Status der Funktion anzuzeigen. Stellen Sie laufend Erkennungsfeedback bereit, damit nicht der Eindruck entsteht, die Spracherkennung würde nicht reagieren.

Bieten Sie Benutzern die Möglichkeit, den Erkennungstext per Tastatureingabe zu überarbeiten, und stellen Sie Eingabeaufforderungen zur Begriffsklärung, Vorschläge oder eine zusätzliche Spracherkennung bereit. Beenden Sie die Erkennung, wenn eine Eingabe von einem anderen Gerät als der Spracherkennung erkannt wird, z. B. Touch- oder Tastatureingabe.

Dies weist in der Regel darauf hin, dass der Benutzer sich mit einer anderen Aufgabe beschäftigt, z. B. der Korrektur des Erkennungstexts oder der Interaktion mit anderen Formularfeldern. Legen Sie die Dauer fest, nach der das Ausbleiben einer Spracheingabe als Beendigung der Spracherkennung interpretiert wird.

## Starten Sie die Erkennung nach dieser Dauer nicht automatisch neu. Meist weist das Ausbleiben einer Spracheingabe darauf hin, dass der Benutzer die Interaktion mit Ihrer App beendet hat.


Deaktivieren Sie die gesamte fortlaufende Erkennungs-UI, und beenden Sie die Erkennungssitzung, wenn keine Netzwerkverbindung verfügbar ist.

Die fortlaufende Erkennung erfordert eine Netzwerkverbindung. <span id="Commanding"></span><span id="commanding"></span><span id="COMMANDING"></span>Befehle

Durch Spracheingabe können Aktionen initiiert, Befehle aufgerufen und Aufgaben ausgeführt werden. Wenn genügend Platz vorhanden ist, können Sie ggf. die unterstützten Antworten für den aktuellen App-Kontext mit Beispielen für gültige Eingaben anzeigen. Dies reduziert die Anzahl möglicher Antworten, die Ihre App verarbeiten muss, und beseitigt Unklarheiten für den Benutzer. Versuchen Sie, Ihre Fragen so zu formulieren, dass sie eine möglichst spezifische Antwort erfordern. Zum Beispiel: "Was möchten Sie heute tun?" ist z. B. eine sehr offene Frage, die aufgrund der vielen Antwortmöglichkeiten eine sehr umfassende Grammatikdefinition notwendig macht.

"Möchten Sie ein Spiel spielen oder Musik hören?" würde die Antwort dagegen auf eine oder zwei gültige Antworten mit einer entsprechend kleinen Grammatikdefinition beschränken.

Eine kleine Grammatik ist sehr viel einfacher zu erstellen und führt zu deutlich genaueren Erkennungsergebnissen. Fordern Sie die Bestätigung des Benutzers an, wenn die Treffgenauigkeit der Spracherkennung gering ist.

Wenn die Absichten des Benutzers unklar sind, ist es besser, seine Wünsche zu klären, als eine unbeabsichtigte Aktion zu initiieren.

Informieren Sie den Benutzer durch einen visuellen Hinweis darüber, dass die Spracherkennung unterstützt wird und ihm zur Verfügung steht, und geben Sie an, ob die Funktion vom Benutzer aktiviert werden muss. Sie können z. B. eine Befehlsleistenschaltfläche mit einer Mikrofonglyphe (siehe [Richtlinien für Befehlsleisten](../controls-and-patterns/app-bars.md)) verwenden, um die Verfügbarkeit und den Status der Funktion anzuzeigen.

Wenn die Spracherkennung normalerweise nicht sichtbar ist, können Sie ggf. im Inhaltsbereich der App eine Statusanzeige hinzufügen.

-   Wenn die Erkennung vom Benutzer initiiert wird, empfiehlt sich aus Gründen der Konsistenz die Verwendung der integrierten Sprechererkennungsfunktion.

    -   Die integrierte Funktion enthält anpassbare Bildschirme mit Eingabeaufforderungen, Beispielen, Begriffsklärungen, Bestätigungen und Fehlern.
    -   Die Bildschirme variieren je nach angegebenen Einschränkungen:
    -   Vordefinierte Grammatik (Diktat oder Websuche)
-   Der Bildschirm **Spracherkennung**

    -   Der Bildschirm **Verarbeitung**
    -   Der Bildschirm **Gehört** oder der Fehlerbildschirm
    -   Liste von Wörtern oder Ausdrücken oder SRGS-Grammatikdatei

Der Bildschirm **Spracherkennung**

-   Der Bildschirm **Sagten Sie** , falls die vom Benutzer ausgesprochenen Wörter als mehrere potenzielle Ergebnisse interpretiert werden können.
-   Der Bildschirm **Gehört** oder der Fehlerbildschirm
-   Auf dem Bildschirm **Spracherkennung** können Sie:
-   Den Text der Überschrift anpassen.

Beispieltext für Benutzerbefehle bereitstellen. Geben Sie an, ob der Bildschirm **Gehört** angezeigt wird.

![Lesen Sie die erkannte Zeichenfolge auf dem Bildschirm **Gehört**.](images/speech/speech-listening-initial.png)

![Hier sehen Sie ein Beispiel für den Ablauf der integrierten Erkennungsfunktion für eine Spracherkennung mit einer SRGS-definierten Einschränkung.](images/speech/speech-listening-intermediate.png)

![In diesem Beispiel ist die Spracherkennung erfolgreich.](images/speech/speech-listening-complete.png)

## initial Erkennung screen for a constraint based on a sgrs grammar file


intermediate Erkennung screen for a constraint based on a sgrs grammar file

final Erkennung screen for a constraint based on a sgrs grammar file <span id="Always_listening"></span><span id="always_listening"></span><span id="ALWAYS_LISTENING"></span>Immer im Spracherkennungsmodus

## Sobald sie gestartet wurde, kann Ihre App ohne Eingreifen des Benutzers auf Spracheingaben lauschen und diese erkennen.


Die Grammatikeinschränkungen sollten basierend auf dem App-Kontext angepasst werden.

Dies sorgt für eine zielgerichtete und für die aktuelle Aufgabe relevante Spracherkennung und minimiert Fehler.

<span id="What_can_I_say_"></span><span id="what_can_i_say_"></span><span id="WHAT_CAN_I_SAY_"></span>"Was kann ich sagen?" Wenn die Spracheingabe aktiviert ist, muss der Benutzer herausfinden können, welche Wörter und Ausdrücke verstanden und welche Aktionen ausgeführt werden können. Wenn die Spracherkennung vom Benutzer aktiviert wird, können Sie mithilfe der Befehlsleiste oder einem Menübefehl alle Wörter und Ausdrücke anzeigen, die im aktuellen Kontext unterstützt werden. Wenn die Spracherkennung immer aktiviert ist, können Sie ggf. den Ausdruck "Was kann ich sagen?"

## auf jeder Seite hinzufügen.


Sagt der Benutzer diesen Ausdruck, werden alle im aktuellen Kontext unterstützten Wörter und Ausdrücke angezeigt. Die Verwendung dieses Ausdrucks bietet dem Benutzer die Möglichkeit, die Sprachfunktionen im gesamten System auf einheitliche Art und Weise zu erkunden.

<span id="Recognition_failures"></span><span id="recognition_failures"></span><span id="RECOGNITION_FAILURES"></span>Spracherkennungsfehler

Die Spracherkennung funktioniert nicht immer.

Fehler treten auf, wenn die Audioqualität schlecht ist, nur ein Teil eines Ausdrucks erkannt wird oder überhaupt keine Eingabe erkannt wird. Die App muss Szenarien, in denen die Spracherkennung fehlschlägt, problemlos behandeln, dem Benutzer verständlich machen, weshalb der Fehler aufgetreten ist, und den Fehler beheben.

Ihre App muss den Benutzer darüber informieren, dass er nicht verstanden wurde und es noch einmal versuchen muss. Zeigen Sie ggf. Beispiele für unterstützte Ausdrücke an.

Da der Benutzer einen vorgeschlagenen Begriff wahrscheinlich wiederholt, erhöht sich dadurch die Wahrscheinlichkeit, dass die Erkennung erfolgreich ist. Zeigen Sie eine Liste potenzieller Übereinstimmungen an, in der der Benutzer eine Auswahl treffen kann.

Dies kann wesentlich effizienter sein, als den Erkennungsprozess zu wiederholen.

Unterstützen Sie immer alternative Eingabetypen. Dies ist insbesondere dann hilfreich, wenn wiederholte Erkennungsfehler auftreten. Sie können dem Benutzer z. B. vorschlagen, über die Tastatur, per Toucheingabe oder mit der Maus ein Wort in einer Liste potenzieller Übereinstimmungen auszuwählen. Verwenden Sie die integrierte Sprechererkennungsfunktion. Sie enthält Bildschirme, auf denen der Benutzer über Fehler bei der Spracherkennung informiert wird und die Möglichkeit erhält, es noch einmal zu versuchen. Achten Sie auf Probleme mit der Audioeingabe, und versuchen Sie, diese zu beheben.

## Die Spracherkennung kann Probleme mit der Audioqualität erkennen, die die Genauigkeit der Spracherkennung beeinträchtigen.


Sie können die von der Spracherkennung bereitgestellten Informationen verwenden, um den Benutzer über das Problem zu informieren und ihm die Möglichkeit zu bieten, Korrekturen vorzunehmen. Ist die Lautstärkeeinstellung des Mikrofons z. B. zu niedrig, können Sie den Benutzer auffordern, lauter zu sprechen oder die Lautstärke zu erhöhen.

### <span id="Constraints"></span><span id="constraints"></span><span id="CONSTRAINTS"></span>Einschränkungen

Einschränkungen oder Grammatiken definieren die gesprochenen Wörter und Ausdrücke, die von der Spracherkennung abgeglichen werden können. Sie können eine der vordefinierten Webdienstgrammatiken festlegen oder eine benutzerdefinierte Grammatik erstellen, die mit Ihrer App installiert wird.

-   <span id="Predefined_grammars"></span><span id="predefined_grammars"></span><span id="PREDEFINED_GRAMMARS"></span>Vordefinierte Grammatiken Mit vordefinierten Diktier- und Websuchgrammatiken können Sie eine Spracherkennung für Ihre App bereitstellen, ohne eine Grammatik erstellen zu müssen. Bei Verwendung dieser Grammatiken wird die Spracherkennung von einem Remotewebdienst durchgeführt, und die Ergebnisse werden an das Gerät zurückgegeben.
-   Die Standardgrammatik der Freitext-Diktierfunktion erkennt die meisten Wörter und Ausdrücke, die Benutzer in einer bestimmten Sprache sagen können, und ist für die Erkennung kurzer Ausdrücke optimiert. Die Freitext-Diktierfunktion ist nützlich, wenn Sie nicht einschränken möchten, was Benutzer sagen können.

Typische Verwendungsmöglichkeiten sind das Erstellen von Notizen oder das Diktieren eines Nachrichtentexts.

 

Die Grammatik für die Websuche enthält wie die Diktiergrammatik eine große Anzahl von Wörtern und Ausdrücken, die Benutzer sagen können. Sie ist allerdings für die Erkennung von Begriffen optimiert, die beim Suchen im Web häufig verwendet werden.

### **Hinweis**  Da vordefinierte Diktier- und Websuchgrammatiken sehr umfangreich sein können und online bereitgestellt werden (nicht auf dem Gerät), ist die Leistung u. U. nicht so gut wie bei einer lokal auf dem Gerät installierten benutzerdefinierten Grammatik.

Diese vordefinierten Grammatiken können zum Erkennen von bis zu zehn Sekunden Spracheingabe verwendet werden. Sie müssen dazu keinen Code selbst erstellen. Sie erfordern jedoch eine Netzwerkverbindung.

-   <span id="Custom_grammars"></span><span id="custom_grammars"></span><span id="CUSTOM_GRAMMARS"></span>Benutzerdefinierte Grammatiken Eine benutzerdefinierte Grammatik ist eine von Ihnen entworfene und erstellte Grammatik, die mit Ihrer App installiert wird. Die Spracherkennung anhand einer benutzerdefinierten Einschränkung wird auf dem Gerät ausgeführt. Einschränkungen per programmgesteuerter Liste sind eine unkomplizierte Methode für die Erstellung einfacher Grammatiken in Form einer Liste von Wörtern und Ausdrücken.
-   Eine Einschränkungsliste eignet sich gut für die Erkennung kurzer, einzelner Ausdrücke. Das explizite Angeben aller Wörter in einer Grammatik verbessert auch die Erkennungsgenauigkeit, da das Spracherkennungsmodul nur eine Übereinstimmung bestätigen muss.

    Die Liste kann auch programmgesteuert aktualisiert werden.

    -   Eine SRGS-Grammatik ist ein statisches Dokument, das im Gegensatz zu einer Einschränkung per programmgesteuerter Liste das in [SRGS Version 1.0](http://go.microsoft.com/fwlink/p/?LinkID=262302) definierte XML-Format verwendet. Eine SRGS-Grammatik bietet die höchstmögliche Kontrolle über die Spracherkennungsfunktion, da Sie mehrere semantische Bedeutungen in einem einzigen Erkennungsvorgang erfassen können. Hier einige Tipps für das Erstellen von SRGS-Grammatiken:
    -   Halten Sie jede Grammatik klein.
    -   Grammatiken, die weniger zu vergleichende Ausdrücke enthalten, bieten eine bessere Erkennungsgenauigkeit als größere Grammatiken mit vielen Ausdrücken. Es empfiehlt sich, anstelle einer einzigen Grammatik für die gesamte App mehrere kleinere Grammatiken für bestimmte Szenarien zu verwenden. Informieren Sie den Benutzer darüber, was er im jeweiligen App-Kontext sagen kann, und aktivieren bzw. deaktivieren Sie Grammatiken nach Bedarf. Entwerfen Sie jede Grammatik so, dass der Benutzer einen Befehl auf verschiedene Arten sprechen kann.
    -   Sie können z. B. die **GARBAGE**-Regel verwenden, um Spracheingaben abzugleichen, die in Ihrer Grammatik nicht definiert sind. So kann der Benutzer zusätzliche Wörter verwenden, die für Ihre App keine Bedeutung haben,
    -   beispielsweise "gib mir", "und", "äähm", "vielleicht" usw. Verwenden Sie das [sapi:subset](http://msdn.microsoft.com/library/windowsphone/design/jj572474.aspx)-Element, um den Vergleich von Spracheingaben zu erleichtern.
    -   Dies ist eine Microsoft-Erweiterung der SRGS-Spezifikation, die den Abgleich von Teilausdrücken ermöglicht. Definieren Sie in Ihrer Grammatik nach Möglichkeit keine einsilbigen Ausdrücke.

Die Erkennung funktioniert bei Ausdrücken mit zwei oder mehr Silben meist genauer. Vermeiden Sie Ausdrücke, die ähnlich klingen.

 

### Ausdrücke wie „Geld“, „Held“ und „fällt“ können das Erkennungsmodul z. B. verwirren und zu einer schlechten Erkennungsgenauigkeit führen.

**Hinweis**  Der von Ihnen verwendete Einschränkungstyp richtet sich nach der Komplexität der Erkennungsfunktion, die Sie erstellen möchten.

Für eine bestimmte Erkennungsaufgabe kann jeweils einer der Ansätze am besten geeignet sein, und vielleicht haben Sie in Ihrer App sogar für alle Einschränkungsarten Verwendung. <span id="Custom_pronunciations"></span><span id="custom_pronunciations"></span><span id="CUSTOM_PRONUNCIATIONS"></span>Benutzerdefinierte Aussprache

Wenn Ihre App Spezialvokabular mit ungewöhnlichen oder fiktiven Wörtern oder Wörter mit ungewöhnlicher Aussprache enthält, können Sie die Erkennungsleistung für diese Wörter verbessern, indem Sie eine benutzerdefinierte Aussprache definieren. Für eine kleine Liste von Wörtern und Ausdrücken oder eine Liste selten verwendeter Wörter und Ausdrücke können Sie eine benutzerdefinierte Aussprache in einer SRGS-Grammatik erstellen.

## Weitere Informationen finden Sie unter [token-Element](http://msdn.microsoft.com/library/windowsphone/design/hh361600.aspx).


Für größere Listen von Wörtern und Ausdrücken oder häufig verwendete Wörter und Ausdrücke können Sie separate Dokumente mit Aussprachewörterbüchern erstellen. Weitere Informationen dazu finden Sie unter [Info zu Lexika und phonetischen Alphabeten](http://msdn.microsoft.com/library/windowsphone/design/hh361646.aspx). <span id="Testing"></span><span id="testing"></span><span id="TESTING"></span>Testen

Testen Sie die Genauigkeit der Spracherkennung und jede UI, die die Spracherkennung unterstützt, mit der Zielgruppe Ihrer App. So können Sie am besten herausfinden, wie effektiv die Sprachinteraktionsfunktion in Ihrer App ist.

## Erhalten Benutzer z. B. schlechte Erkennungsergebnisse, weil Ihre App nicht auf einen gängigen Ausdruck lauscht?


Ändern Sie die Grammatik entweder so, dass der Ausdruck unterstützt wird, oder stellen Sie eine Liste der unterstützten Ausdrücke für den Benutzer bereit.

Falls Sie bereits eine Liste der unterstützten Begriffe verwenden, stellen Sie sicher, dass sie leicht zu finden ist.

<span id="Text-to-speech__TTS_"></span><span id="text-to-speech__tts_"></span><span id="TEXT-TO-SPEECH__TTS_"></span>Text-to-Spracherkennung (TTS) TTS generiert Sprachausgabe aus Nur-Text oder SSML.

Versuchen Sie, höfliche und positiv formulierte Eingabeaufforderungen zu erstellen.

Lesen Sie lange Textzeichenfolgen ggf. vor.

-   Eine SMS anzuhören, ist eine Sache, eine lange Liste von Suchergebnissen zu hören, die schwer zu merken sind, aber eine ganz andere.
-   Stellen Sie Medien-Steuerelemente bereit, mit denen der Benutzer TTS anhalten oder beenden kann.

Hören Sie sich alle TTS-Zeichenfolgen an, um sicherzustellen, dass sie verständlich sind und natürlich klingen. Wenn eine ungewöhnliche Abfolge von Wörtern kombiniert wird oder Teilenummern oder Satzzeichen gesprochen werden, kann ein Satz unverständlich werden.

## Sprache kann unnatürlich klingen, wenn der Sprechrhythmus von der normalen Sprechweise eines Muttersprachlers abweicht. 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Beide Probleme können durch Verwendung von SSML anstelle von Nur-Text als Synthesizereingabe behoben werden.</th>
<th align="left">Weitere Informationen zu SSML finden Sie unter [Steuerung der synthetischen Sprachausgabe mit SSML](http://msdn.microsoft.com/library/windowsphone/design/hh378454.aspx) und [Referenz für Speech Synthesis Markup Language](http://msdn.microsoft.com/library/windowsphone/design/hh378377.aspx).</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Andere Artikel in diesem Abschnitt](speech-recognition.md)</p></td>
<td align="left"><p>Thema</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Beschreibung](specify-the-speech-recognizer-language.md)</p></td>
<td align="left"><p>Nutzen Sie die Spracherkennung als Eingabemöglichkeit oder zum Ausführen einer Aktion, eines Befehls oder einer Aufgabe.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Hier erfahren Sie, wie Sie eine installierte Sprache für die Spracherkennung auswählen.](define-custom-recognition-constraints.md)</p></td>
<td align="left"><p>Erfahren Sie, wie Sie benutzerdefinierte Einschränkungen für die Spracherkennung definieren und verwenden.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Erfahren Sie, wie Sie die Erfassung und Erkennung langer Spracheingaben für kontinuierliches Diktieren ermöglichen.](enable-continuous-dictation.md)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie Probleme mit der Genauigkeit der Spracherkennung behandeln, die auf die Qualität der Audioeingabe zurückzuführen sind.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Legen Sie fest, wie lange eine Spracherkennung Stille oder nicht erkennbare Geräusche (Störgeräusche) ignoriert und weiterhin auf Spracheingabe wartet.](manage-issues-with-audio-input.md)</p></td>
<td align="left"><p><span id="related_topics"></span>Verwandte Artikel</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Sprachinteraktionen](set-speech-recognition-timeouts.md)</p></td>
<td align="left"><p>[Cortana-Interaktionen](https://msdn.microsoft.com/library/windows/apps/mt185598)</p></td>
</tr>
</tbody>
</table>

 


## **Beispiele**


* [Beispiel zu Spracherkennung und Sprachsynthese](https://msdn.microsoft.com/library/windows/apps/mt185614)
* [Cortana interactions](https://msdn.microsoft.com/library/windows/apps/mt185598)
 **Samples**
* [Speech recognition and speech synthesis sample](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 





<!--HONumber=Apr16_HO3-->


