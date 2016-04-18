---
Description: Description: Hier erfahren Sie, wie Sie eine installierte Sprache für die Spracherkennung auswählen.
title: title: Angeben der Sprache für die Spracherkennung
ms.assetid: 4C463A1B-AF6A-46FD-A839-5D6724955B38
label: Specify the speech recognizer language
template: detail.hbs
---

# ms.assetid: 4C463A1B-AF6A-46FD-A839-5D6724955B38


label: Angeben der Sprache für die Spracherkennung

template: detail.hbs Angeben der Sprache für die Spracherkennung


**Hier erfahren Sie, wie Sie eine installierte Sprache für die Spracherkennung auswählen.**

-   [**\[ Aktualisiert für UWP-Apps unter Windows 10.**](https://msdn.microsoft.com/library/windows/apps/dn653251)
-   [**Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]**](https://msdn.microsoft.com/library/windows/apps/dn653250)
-   [**Wichtige APIs**](https://msdn.microsoft.com/library/windows/apps/br206804)


SupportedTopicLanguages

SupportedGrammarLanguages

Sprache

Im Folgenden listen wir die auf einem System installierten Sprachen auf, ermitteln die Standardsprache und wählen eine andere Sprache für die Spracherkennung aus.

**Voraussetzungen:**

-   [Dieses Thema baut auf dem Thema [Spracherkennung](speech-recognition.md) auf.](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Sie sollten über Grundkenntnisse in der Spracherkennung und in Erkennungseinschränkungen verfügen.

Wenn Sie noch keine Erfahrung mit der Entwicklung von UWP-Apps (Universelle Windows-Plattform) haben, sollten Sie sich in den folgenden Themen zunächst mit den hier besprochenen Technologien vertraut machen.

Erstellen Ihrer ersten App

## Informationen zu Ereignissen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](https://msdn.microsoft.com/library/windows/apps/mt185584).


**Richtlinien für die Benutzerfreundlichkeit:** Nützliche Tipps zum Entwerfen einer hilfreichen und ansprechenden App mit Sprachfunktion finden Sie unter [Entwurfsrichtlinien für die Spracherkennung](https://msdn.microsoft.com/library/windows/apps/dn596121).

<span id="Identify_the_default_language"></span><span id="identify_the_default_language"></span><span id="IDENTIFY_THE_DEFAULT_LANGUAGE"></span>Ermitteln der Standardsprache

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; </code></pre></td>
</tr>
</tbody>
</table>
```

## Die Spracherkennung verwendet die Systemsprache als Standardsprache für die Erkennung.


Diese Sprache wird vom Benutzer auf dem Gerät unter „Einstellungen &gt; System &gt; Sprache &gt; Spracherkennungssprache” festgelegt. Zum Ermitteln der Standardsprache überprüfen wir die statische [**SystemSpeechLanguage**](https://msdn.microsoft.com/library/windows/apps/dn653252)-Eigenschaft.

<span id="Confirm_an_installed_language"></span><span id="confirm_an_installed_language"></span><span id="CONFIRM_AN_INSTALLED_LANGUAGE"></span>Bestätigen einer installierten Sprache Die installierten Sprachen können sich von Gerät zu Gerät unterscheiden.

 

Überprüfen Sie, ob eine Sprache vorhanden ist, wenn diese für eine bestimmte Einschränkung erforderlich ist.

-   **Hinweis**  Nach der Installation eines neuen Sprachpakets ist ein Neustart erforderlich.

-   Eine Ausnahme mit dem Fehlercode SPERR\_NOT\_FOUND (0x8004503a) wird ausgelöst, wenn die angegebene Sprache nicht unterstützt wird oder nicht vollständig installiert wurde.

## Zum Ermitteln der auf einem Gerät unterstützten Sprachen überprüfen Sie eine von zwei statischen Eigenschaften der [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226)-Klasse:


[**SupportedTopicLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653251) – Die Sammlung von [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804)-Objekten, die mit vordefinierten Diktier- und Websuchgrammatiken verwendet werden

[**SupportedGrammarLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653250) – Die Sammlung von [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804)-Objekten, die mit einer Einschränkungsliste oder einer Speech Recognition Grammar Specification (SRGS)-Datei verwendet werden

<span codelanguage="CSharp"></span>
```CSharp
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
var language = new Windows.Globalization.Language(“en-US”); 
var recognizer = new SpeechRecognizer(language); 
```

## <span id="Specify_a_language"></span><span id="specify_a_language"></span><span id="SPECIFY_A_LANGUAGE"></span>Angeben einer Sprache


Übergeben Sie zum Angeben einer Sprache ein [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804)-Objekt im [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226)-Konstruktor. Hier geben wir "En-US" als Sprache für die Spracherkennung an.

<span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>Anmerkungen Sie können eine Themeneinschränkung konfigurieren, indem Sie [**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446) zur [**Constraints**](https://msdn.microsoft.com/library/windows/apps/dn653241)-Sammlung der [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) hinzufügen und anschließend [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240) aufrufen. Der [**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433)-Wert **TopicLanguageNotSupported** wird zurückgegeben, wenn die Erkennung nicht mit einer unterstützten Themensprache initialisiert wird.

Sie können eine Einschränkungsliste konfigurieren, indem Sie [**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421) zur [**Constraints**](https://msdn.microsoft.com/library/windows/apps/dn653241)-Sammlung der [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) hinzufügen und anschließend [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240) aufrufen. Die Sprache einer benutzerdefinierten Liste kann nicht direkt angegeben werden. Stattdessen wird die Liste mit der Sprache der Erkennung verarbeitet.

## Bei einer SRGS-Grammatik handelt es sich um ein offenes XML-Standardformat, das durch die [**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412)-Klasse dargestellt wird.


**Anders als bei benutzerdefinierten Listen können Sie die Sprache der Grammatik im SRGS-Markup angeben.**
* [[**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240) schlägt mit dem [**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433)-Wert **TopicLanguageNotSupported** fehl, wenn die Erkennung nicht mit der gleichen Sprache initialisiert wird wie das SRGS-Markup.](speech-interactions.md)
**<span id="related_topics"></span>Verwandte Artikel**
* [Entwickler](https://msdn.microsoft.com/library/windows/apps/dn596121)
**[Sprachinteraktionen](speech-interactions.md)**
* [**Designer**](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 






<!--HONumber=Apr16_HO3-->


