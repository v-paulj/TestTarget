---
author: DelfCo
Description: "Bereiten Sie Ihre App für die Lokalisierung vor, um sie an andere Märkte, Sprachen oder Regionen anzupassen."
title: "Vorbereiten Ihrer App für die Lokalisierung"
ms.assetid: 06E1D4BB-59EA-4D71-99AC-7CB93D2A58A7
label: Prepare your app for localization
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: e52a5322767677859e32ccbecf4951745c49f36f

---

# Vorbereiten Ihrer App für die Lokalisierung





Bereiten Sie Ihre App für die Lokalisierung vor, um sie an andere Märkte, Sprachen oder Regionen anzupassen. Lesen Sie zuerst unbedingt die [empfohlene und nicht empfohlene Vorgehensweisen](guidelines-and-checklist-for-globalizing-your-app.md).

## <span id="use_resource_files_and_qualifiers."></span><span id="USE_RESOURCE_FILES_AND_QUALIFIERS."></span>Verwenden Sie Ressourcendateien und Qualifizierer.


Nehmen Sie die UI-Zeichenfolgen Ihrer App in Ressourcendateien auf, anstatt sie im Code zu platzieren. Ausführliche Informationen finden Sie unter [Aufnehmen von UI-Zeichenfolgen in Ressourcen](put-ui-strings-into-resources.md).

Geben Sie Bilddateien oder andere Dateiressourcen in deren Datei oder Ordner mit dem entsprechenden Sprachtag an. Berücksichtigen Sie, dass die Lokalisierung von Bildern, Audio und Video erhebliche Systemressourcen beansprucht. Verwenden Sie deshalb soweit möglich neutrale Medienobjekte. Weitere Informationen finden Sie unter [So wird's gemacht: Benennen von Ressourcen mit Qualifizierern](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324).

## <span id="add_contextual_comments."></span><span id="ADD_CONTEXTUAL_COMMENTS."></span>Fügen Sie Kontextkommentare hinzu.


Fügen Sie den Ressourcendateien Ihrer App Lokalisierungskommentare hinzu. Die Kommentare können vom Lokalisierer gelesen werden und sollten Informationen zum Kontext enthalten, die als Hilfestellung für eine präzise Übersetzung der Ressourcen dienen. Die Kommentare sollten auch ausreichend auf Beschränkungen hinweisen, sodass die Übersetzung die Funktionalität der Software nicht beeinträchtigt. Optional können die Kommentare auch mit dem Tool "Makepri.exe" protokolliert werden.

**XAML:** RESW-Dateien (in Visual Studio mithilfe von XAML erstellte App-Ressourcen) verfügen über ein Kommentarelement. Beispiel:

```XML
<data name="String1">
    <value>Hello World</value>
    <comment>A greeting (This is a comment to the localizer)</comment>
</data>
```

**HTML:** RESJSON-Dateien (in Visual Studio mithilfe von HTML erstellte App-Ressourcen) ermöglichen die Aufnahme von Metadaten in Felder, die mit einem Unterstrich beginnen, wie z. B. Kommentare:

```json
{
    "String1"  : "Hello World",
    "_String1.comment" : "A greeting (This is a comment to the localizer)"
}
```

## <span id="localize_sentences_instead_of_words."></span><span id="LOCALIZE_SENTENCES_INSTEAD_OF_WORDS."></span>Lokalisieren Sie Sätze anstelle von Wörtern.


Beispiel: „{0} konnte nicht synchronisiert werden.“

Der Platzhalter „{0}“ kann durch zahlreiche Wörter ersetzt werden, z. B. durch Termin, Aufgabe oder Dokument. Dieses Beispiel funktioniert zwar in der englischen Sprache, aber nicht in jedem Fall im entsprechenden deutschen Satz. Sie sehen, dass in den folgenden deutschen Sätzen einige der Wörter in der Vorlagenzeichenfolge („Der“, „Die“, „Das“) zum parametrisierten Wort passen müssen:

| Englisch                                    | Deutsch                                           |
|:------------------------------------------ |:------------------------------------------------ |
| The appointment could not be synchronized. | Der Termin konnte nicht synchronisiert werden.   |
| The task could not be synchronized.        | Die Aufgabe konnte nicht synchronisiert werden.  |
| The document could not be synchronized.    | Das Dokument konnte nicht synchronisiert werden. |

 

Erwägen Sie als weiteres Beispiel den Satz „Remind me in {0} minute(s).“ Während „minute(s)" im Englischen funktioniert, werden in anderen Sprachen möglicherweise unterschiedliche Begriffe verwendet. verwenden. Auf Polnisch heißt es je nach Kontext „minuta“, „minuty“ oder „minut“.

Lokalisieren Sie den gesamten Satz, statt nur ein einzelnes Wort, um das Problem zu beheben. Auf den ersten Blick wirkt diese Lösung vielleicht nicht ganz so elegant und sieht nach überflüssiger Arbeit aus, die Gründe sprechen jedoch für sich:

-   Für alle Sprachen wird eine saubere Fehlermeldung angezeigt.
-   Lokalisierer müssen nicht nachfragen, womit die Zeichenfolge ersetzt wird.
-   Sie müssen keine kostspielige Codefehlerbehebung implementieren, wenn ein solches Problem in der fertigen App auftaucht.

## <span id="ensure_the_correct_parameter_order."></span><span id="ENSURE_THE_CORRECT_PARAMETER_ORDER."></span>Stellen Sie sicher, dass die Parameterreihenfolge richtig ist.


Gehen Sie nicht davon aus, dass Parameter in allen Sprachen in der gleichen Reihenfolge auftreten. Beispiel: Bei der Zeichenfolge „Every %s %s“ wird das erste Vorkommen von „%s“ durch den Monatsnamen und das zweite „%s“ durch den Tag des Monats ersetzt. Dieses Beispiel funktioniert zwar in der englischen Sprache, aber nicht bei einer Lokalisierung ins Deutsche, da hier Tag und Monat eine umgekehrten Reihenfolge einnehmen.

Ändern Sie die Zeichenfolge in „Every %1 %2“, um das Problem zu beheben. Die Reihenfolge ist dann je nach Sprache austauschbar.

## <span id="don_t_over_localize."></span><span id="DON_T_OVER_LOCALIZE."></span>Vermeiden Sie eine zu starke Lokalisierung.


Lokalisieren Sie bestimmte Zeichenfolgen, nicht Tags. Beachten Sie folgende Beispiele:

| Zu stark lokalisierte Zeichenfolge                   | Richtig lokalisierte Zeichenfolge |
|:--------------------------------------- |:-------------------------- |
| &lt;link&gt;Nutzungsbedingungen&lt;/link&gt;   | Nutzungsbedingungen               |
| &lt;link&gt;Datenschutzrichtlinie&lt;/link&gt; | Datenschutzrichtlinie             |

 

Wenn das Tag &lt;link&gt; in die Ressourcen aufgenommen wird, wird es ebenfalls lokalisiert. Dadurch wird das Tag ungültig. Nur die Zeichenfolgen selbst dürfen lokalisiert werden. Betrachten Sie Tags generell als Code, der vom Lokalisierungsinhalt getrennt gehalten werden sollte. Lange Zeichenfolgen sollten jedoch Markup enthalten, um den Kontext zu wahren und eine Sortierung zu ermöglichen.

## <span id="do_not_use_the_same_strings_in_dissimilar_contexts."></span><span id="DO_NOT_USE_THE_SAME_STRINGS_IN_DISSIMILAR_CONTEXTS."></span>Verwenden Sie nicht die gleichen Zeichenfolgen in unterschiedlichen Kontexten.


Es mag vernünftig erscheinen, eine Zeichenfolge mehrmals zu verwenden. Allerdings können erhebliche Lokalisierungsprobleme auftreten, wenn ein Wort oder Ausdruck mehrere Bedeutungen oder Kontexte besitzt.

Sie können Zeichenfolgen im gleichen Kontext mehrmals verwenden. Sie können z. B. die Zeichenfolge „Volume“ sowohl für Soundeffekte als auch für die Lautstärke von Musik verwenden, da sich beides auf die Intensität von Tönen bezieht. Verwenden Sie dieselbe Zeichenfolge jedoch nicht für Festplattenvolumes, da sich hier Kontext und Bedeutung unterscheiden und das Wort anders übersetzt wird.

Ein weiteres Beispiel sind die Zeichenfolgen „on“ und „off“. Im Englischen können sich „on“ und „off“ auf das Ein- und Ausschalten von Flugzeugmodus, Bluetooth oder Geräten beziehen. Im Italienischen hängt die Übersetzung jedoch davon ab, was ein- und ausgeschaltet wird. Sie müssten für jeden Kontext ein Zeichenfolgenpaar erstellen.

Zudem können Zeichenfolgen wie „text“ oder „fas“ in der englischen Sprache als Verb und als Substantiv verwendet werden, was die Übersetzung erschweren kann. Erstellen Sie deshalb jeweils eine getrennte Zeichenfolge für das Verb und das Substantiv. Falls unklar ist, ob es sich um den gleichen Kontext handelt, gehen Sie auf Nummer sicher, und verwenden Sie getrennte Zeichenfolgen.

## <span id="identify_resources_with_unique_attributes."></span><span id="IDENTIFY_RESOURCES_WITH_UNIQUE_ATTRIBUTES."></span>Identifizieren Sie Ressourcen anhand eindeutiger Attribute.


Bei Ressourcenbezeichnern wird zwischen Groß-/Kleinschreibung unterschieden, und die Bezeichner müssen pro Ressourcendatei eindeutig sein. Verwenden Sie beim Zugriff auf eine Ressource den Ressourcenbezeichner und nicht den tatsächlichen Wert der Ressource. Ressourcenbezeichner ändern sich nicht. Die tatsächlichen Werte der Ressource ändern sich hingegen je nach Sprache.

Wählen Sie aussagekräftige Ressourcenbezeichner, um für die Übersetzung einen weiteren Kontext bereitzustellen.

Ändern Sie die Ressourcenbezeichner nicht, nachdem die Zeichenfolgenressourcen zur Übersetzung weitergegeben wurden. Lokalisierungsteams verfolgen Ergänzungen, Löschungen und Updates an den Ressourcen anhand der Ressourcenbezeichner nach. Bei Änderungen an Ressourcenbezeichnern – auch „Resource Identifiers Shift“ genannt – müssen die Zeichenfolgen neu übersetzt werden, da der Eindruck entsteht, das Zeichenfolgen gelöscht oder hinzugefügt wurden.

## <span id="choose_an_appropriate_translation_approach."></span><span id="CHOOSE_AN_APPROPRIATE_TRANSLATION_APPROACH."></span>Entscheiden Sie sich für den richtigen Übersetzungsansatz.


Nachdem die Zeichenfolgen in Ressourcendateien untergliedert wurden, können sie übersetzt werden. Der ideale Zeitpunkt für die Übersetzung ist nach Fertigstellung der Zeichenfolgen im Projekt, was in der Regel gegen Projektende der Fall ist. Der Übersetzungsprozess kann auf unterschiedliche Weise gehandhabt werden. Die Vorgehensweise hängt vom Umfang der zu übersetzenden Zeichenfolgen ab, von der Anzahl der zu übersetzenden Sprachen und wie die Übersetzung erfolgt (intern oder über externe Auftragnehmer).

Erwägen Sie folgende Optionen:

-   **Die Ressourcendateien können zum Übersetzen direkt im Projekt geöffnet werden.** Dieser Ansatz funktioniert gut für ein Projekt mit wenigen Zeichenfolgen, die in zwei oder drei Sprachen übersetzt werden müssen. Er könnte sich für Szenarien eignen, in denen ein Entwickler mehrere Sprachen spricht und die Übersetzung übernehmen kann. Dieser Ansatz ist schnell, erfordert keine Tools und minimiert das Risiko von falschen Übersetzungen. Er ist aber nicht skalierbar. Insbesondere kann es passieren, das die Ressourcen in verschiedenen Sprachen nicht mehr synchron sind, was eine mangelnde Benutzerfreundlichkeit und Verwaltungsprobleme zur Folge haben kann.
-   **Die Zeichenfolgenressourcendateien werden im XML- oder ResJSON-Textformat erstellt und können somit zur Übersetzung mit einem Texteditor übergeben werden. Die übersetzten Dateien würde dann wieder in das Projekt kopiert werden.** Bei diesem Ansatz besteht die Gefahr, dass Übersetzer versehentlich die XML-Tags bearbeiten. Andererseits besteht die Möglichkeit, Übersetzungen außerhalb des Microsoft Visual Studio-Projekts durchzuführen. Dieser Ansatz eignet sich gut für Projekte, die nur in wenige Sprachen übersetzt werden. Das XLIFF-Format ist ein XML-Format, das speziell für die Lokalisierung vorgesehen ist. Es sollte zudem von einigen Lokalisierungsanbietern oder -tools unterstützt werden. Sie können mit dem [Multilingual App Toolkit](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx) XLIFF-Dateien aus anderen Ressourcendateien wie „.resw“ oder „.resjson“ generieren.

Lokalisierern müssen möglicherweise noch andere Dateien übergeben werden, wie Bild- oder Audiodateien. In der Regel wird davon abgeraten, Dateien mit kulturellem Bezug zu erstellen, da diese schwer zu lokalisieren sind.

Berücksichtigen Sie auch die folgenden Vorschläge:

-   **Verwenden Sie ein Lokalisierungstool.** Es gibt eine Reihe von Lokalisierungstools, mit denen Ressourcendateien analysiert werden können und die nur die Bearbeitung von übersetzbaren Zeichenfolgen für den Übersetzer zulassen. Das Risiko, dass ein Übersetzer versehentlich XML-Tags bearbeitet, ist somit geringer. Der Nachteil ist aber, dass neue Tools und Prozesse in den Lokalisierungsprozess eingebunden werden müssen. Ein Lokalisierungstool eignet sich für Projekte mit vielen Zeichenfolgen, aber wenigen Sprachen. Weitere Informationen finden Sie unter [Verwenden des Multilingual App Toolkit](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx).
-   **Verwenden Sie einen Lokalisierungsanbieter.** Wenden Sie sich an einen Lokalisierungsanbieter, wenn des Projekt viele Zeichenfolgen enthält und in viele Sprachen übersetzt werden muss. Ein Lokalisierungsanbieter kann Sie hinsichtlich der Tools und Prozesse beraten sowie die Ressourcendateien übersetzen. Diese Lösung ist ideal, stellt allerdings auch die teuerste Option dar und kann die Bearbeitungszeit für den übersetzten Inhalt verlängern.
-   **Halten Sie die Lokalisierer informiert.** Informieren Sie Lokalisierer bezüglich Zeichenfolgen, die als Substantiv oder Verb betrachtet werden können. Erklären Sie konstruierte Wörter mithilfe von Terminologietools. Halten Sie die Zeichenfolgen grammatikalisch korrekt, eindeutig und allgemein verständlich, um Missverständnisse zu vermeiden.

## <span id="keep_access_keys_and_labels_consistent."></span><span id="KEEP_ACCESS_KEYS_AND_LABELS_CONSISTENT."></span>Verwenden Sie Zugriffstasten und Bezeichnungen konsistent.


Die „Synchronisierung“ der für die Barrierefreiheit verwendeten Zugriffstasten mit der Anzeige der lokalisierten Zugriffstasten stellt eine besondere Herausforderung dar, da beide Zeichenfolgenressourcen in getrennten Abschnitten kategorisiert sind. Stellen Sie für die Bezeichnungszeichenfolge unbedingt Kommentare bereit wie: `Make sure that the emphasized shortcut key  is synchronized with the access key.`

**HTML:**

Folgen Sie der unten dargestellten Implementierung. Auch hier gilt: Kommentieren Sie die Bezeichnungszeichenfolge ausreichend, sodass sie mit der Definition der Zugriffstaste verknüpft werden kann.

```HTML
<label id="theLabel" data-win-res="{accessKey: 'theLabelAccessKey'}" for="xPrinterRedirection" accessKey="L">The <u>L</u>abel</label>
<input type="checkbox" value="OFF" id="xPrinterRedirection" name="xPrinterRedirection" />
```

## <span id="support_furigana_for_japanese_strings_that_can_be_sorted."></span><span id="SUPPORT_FURIGANA_FOR_JAPANESE_STRINGS_THAT_CAN_BE_SORTED."></span>Unterstützung von Furigana für sortierbare japanische Zeichenfolgen.


Japanische Kanji-Zeichen weisen die Besonderheit auf, dass sie je nach Wort und Kontext ihrer Verwendung anders ausgesprochen werden. Bei der Sortierung japanisch bezeichneter Objekte (wie Anwendungsnamen, Dateien, Lieder usw.) führt das zu Problemen. In der Vergangenheit wurde japanisches Kanji normalerweise in einer maschinenlesbaren Anordnung, XJIS genannt, sortiert. Da es sich hierbei um keine phonetische Sortierung handelt, ist sie für menschliche Nutzer leider nicht hilfreich.

Furigana umgeht dieses Problem, da der Benutzer oder Ersteller die Phonetik für die verwendeten Zeichen angeben kann. Wenn Sie Furigana dem App-Namen wie folgt hinzufügen, können Sie sicherstellen, dass die App in der Liste an der richtigen Stelle aufgeführt wird. Wenn die UI-Sprache des Benutzers oder die Sortierreihenfolge auf Japanisch festgelegt ist und der App-Name Kanji-Zeichen ohne ergänzendes Furigana enthält, dann generiert Windows nach Möglichkeit die richtige Aussprache. Es ist jedoch möglich, App-Namen mit seltener oder spezieller Schreibweise stattdessen nach einer gebräuchlicheren Schreibweise zu sortieren. Deshalb empfiehlt es sich, bei japanischen Anwendungen (insbesondere wenn sie Kanji-Zeichen im Namen enthalten) eine Furigana-Version des App-Namens im Rahmen der japanischen Lokalisierung bereitzustellen.

1.  Fügen Sie „ms-resource:Appname“ als Paketanzeigenamen und Anwendungsanzeigenamen hinzu.
2.  Erstellen Sie unter „strings“ den Ordner „ja-JP“, und fügen Sie zwei Ressourcendateien wie folgt hinzu:

    ``` syntax
    strings\
        en-us\
        ja-jp\
            Resources.altform-msft-phonetic.resw
            Resources.resw
    ```

3.  Unter „Resources.resw“ für „ja-JP“ allgemein: Fügen Sie eine Zeichenfolgenressource für den App-Namen „希蒼“ hinzu.
4.  Unter „Resources.altform-msft-phonetic.resw“ für japanische Furigana-Ressourcen: Fügen Sie einen Furigana-Wert für den App-Namen „のあ“ hinzu.

Der Benutzer kann sowohl mit dem Furigana-Wert „のあ“ (noa) als auch mit dem phonetischen Wert „まれあお“ (mare-ao) (mithilfe der **GetPhonetic**-Funktion aus dem Eingabemethoden-Editor (Input Method Editor, IME)) nach dem App-Namen „希蒼“ suchen.

Die Sortierung folgt dem **regionalen Format der Systemsteuerung**:

-   Unter dem japanischen Benutzergebietsschema:
    -   Wenn Furigana aktiviert ist, wird „希蒼“ unter „の“ sortiert.
    -   Wenn Furigana fehlt, wird „希蒼“ unter „ま“ sortiert.
-   Unter nicht japanischem Benutzergebietsschema:
    -   Wenn Furigana aktiviert ist, wird „希蒼“ unter „の“ sortiert.
    -   Wenn Furigana fehlt, wird „希蒼“ unter „漢字“ sortiert.

## <span id="related_topics"></span>Verwandte Themen


* [Empfohlene und nicht empfohlene Vorgehensweisen für die Globalisierung und Lokalisierung](guidelines-and-checklist-for-globalizing-your-app.md)
* [Aufnehmen von UI-Zeichenfolgen in Ressourcen](put-ui-strings-into-resources.md)
* [Benennen von Ressourcen mithilfe von Qualifizierern](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)
 

 






<!--HONumber=Jun16_HO4-->


