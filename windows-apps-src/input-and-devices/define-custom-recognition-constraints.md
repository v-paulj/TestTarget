---
author: Karl-Bridge-Microsoft
Description: "Erfahren Sie, wie Sie benutzerdefinierte Einschränkungen für die Spracherkennung definieren und verwenden können."
title: "Definieren von benutzerdefinierten Erkennungseinschränkungen"
ms.assetid: 26289DE5-6AC9-42C3-A160-E522AE62D2FC
label: Define custom recognition constraints
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: d619cd6b3c9f5d62ca7d68f555e4c051abaf9bad

---

# Definieren von benutzerdefinierten Erkennungseinschränkungen

Erfahren Sie, wie Sie benutzerdefinierte Einschränkungen für die Spracherkennung definieren und verwenden können.

**Wichtige APIs**

-   [**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446)
-   [**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421)
-   [**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412)


Die Spracherkennung benötigt mindestens eine Einschränkung, um erkennbares Vokabular zu definieren. Wenn Sie keine Einschränkung angeben, wird die vordefinierte Diktiergrammatik der universellen Windows-Apps verwendet. Siehe [Spracherkennung](speech-recognition.md).


## Hinzufügen von Einschränkungen


Verwenden Sie die [**SpeechRecognizer.Constraints**](https://msdn.microsoft.com/library/windows/apps/dn653241)-Eigenschaft, um Einschränkungen für die Spracherkennung hinzuzufügen.

Im Folgenden behandeln wir die drei Arten der Spracherkennungseinschränkungen, die in einer App verwendet werden. (Informationen zu Einschränkungen bei Sprachbefehlen finden Sie unter [Starten einer Vordergrund-App mit Sprachbefehlen in Cortana](launch-a-foreground-app-with-voice-commands-in-cortana.md).)

-   [**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446) – Eine Einschränkung auf der Grundlage einer vordefinierten Grammatik (Diktat oder Websuche).
-   [**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421) – Eine Einschränkung auf der Grundlage einer Liste von Wörtern oder Ausdrücken.
-   [**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412) – Eine Einschränkung, die in einer SRGS (Speech Recognition Grammar Specification)-Datei definiert ist.

Jedes Spracherkennungsmodul kann über eine Einschränkungssammlung verfügen. Nur die folgenden Einschränkungskombinationen sind gültig:

-   Eine Einschränkung zu einem Thema oder vordefinierte Grammatik (Diktat oder Websuche). Es sind keine anderen Einschränkungen zulässig.
-   Eine Kombination aus Listeneinschränkungen und/oder Grammatikdatei-Einschränkungen.

**Denken Sie daran:  **Rufen Sie die [**SpeechRecognizer.CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240)-Methode auf, um die Einschränkungen zu kompilieren, bevor Sie den Erkennungsprozess starten.

## Angeben einer Grammatik für die Websuche (SpeechRecognitionTopicConstraint)


Themeneinschränkungen (Diktier- oder Websuchengrammatik) müssen der Einschränkungssammlung einer Spracherkennung hinzugefügt werden.

Hier fügen wir der Einschränkungssammlung eine Grammatik für die Websuche hinzu.

```CSharp
private async void WeatherSearch_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Listen for audio input issues.
    speechRecognizer.RecognitionQualityDegrading += speechRecognizer_RecognitionQualityDegrading;

    // Add a web search grammar to the recognizer.
    var webSearchGrammar = new Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint(Windows.Media.SpeechRecognition.SpeechRecognitionScenario.WebSearch, "webSearch");


    speechRecognizer.UIOptions.AudiblePrompt = "Say what you want to search for...";
    speechRecognizer.UIOptions.ExampleText = @"Ex. &#39;weather for London&#39;";
    speechRecognizer.Constraints.Add(webSearchGrammar);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();
    //await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## Angeben einer programmgesteuerten Listeneinschränkung (SpeechRecognitionListConstraint)


Listeneinschränkungen müssen der Einschränkungssammlung einer Spracherkennung hinzugefügt werden.

Beachten Sie folgende Punkte:

-   Sie können der Einschränkungssammlung mehrere Listeneinschränkungen hinzufügen.
-   Sie können eine beliebige Sammlung verwenden, die **IIterable&lt;String&gt;** für die Zeichenfolgenwerte implementiert.

Hier geben wir programmgesteuert ein Array von Wörtern als Listeneinschränkung an und fügen es der Einschränkungssammlung einer Spracherkennung hinzu.

```CSharp
private async void YesOrNo_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // You could create this array dynamically.
    string[] responses = { "Yes", "No" };


    // Add a list constraint to the recognizer.
    var listConstraint = new Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint(responses, "yesOrNo");

    speechRecognizer.UIOptions.ExampleText = @"Ex. &#39;yes&#39;, &#39;no&#39;";
    speechRecognizer.Constraints.Add(listConstraint);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## Angeben einer SRGS-(SpeechRecognitionGrammarFileConstraint-)Grammatikeinschränkung


SRGS-Grammatikdateien müssen der Einschränkungssammlung eines Spracherkennungsmoduls hinzugefügt werden.

SRGS, Version1.0, ist die branchenübliche Markupsprache zum Erstellen von Grammatik für die Spracherkennung im XML-Format. Universelle Windows-Apps bieten über SRGS hinaus auch Alternativen zur Erstellung von Grammatik für die Spracherkennung. Sie stellen aber möglicherweise fest, dass Sie beim Erstellen von Grammatik mit SRGS die besten Ergebnisse erzielen. Dies gilt besonders für komplexere Spracherkennungsszenarien.

SRGS-Grammatik bietet einen umfassenden Featuresatz, den Sie zum Erstellen komplexer Sprachinteraktionen für Ihre Apps nutzen können. Mit SRGS haben Sie beispielsweise folgende Möglichkeiten:

-   Geben Sie die Reihenfolge an, in der Wörter und Wortgruppen gesprochen werden müssen, um erkannt zu werden.
-   Kombinieren Sie Wörter mehrerer Listen und Wortgruppen für die Erkennung.
-   Verlinken Sie zu anderen Grammatiken.
-   Weisen Sie einem alternativen Wort oder einer Wortgruppe eine Gewichtung zu, um die Wahrscheinlichkeit der Verwendung zu erhöhen oder zu verringern und für die Spracheingabe so bessere Übereinstimmungen zu erzielen.
-   Binden Sie optionale Wörter oder Wortgruppen ein.
-   Verwenden Sie spezielle Regeln zum Herausfiltern nicht angegebener oder unerwarteter Eingaben, z.B. ungewollte Spracheingaben, die keine Übereinstimmung mit der Grammatik ergeben, oder Hintergrundgeräusche.
-   Verwenden Sie Semantik, um zu definieren, was Spracherkennung für Ihre App bedeutet.
-   Geben Sie verschiedene Aussprachen an, entweder direkt in einer Grammatik oder über einen Link zu einem Lexikon.

Weitere Informationen zu SRGS-Elementen und -Attributen finden Sie unter [SRGS-Grammatik – XML-Referenz](http://go.microsoft.com/fwlink/p/?LinkID=269886). Informationen zu den ersten Schritten zur Erstellung einer SRGS-Grammatik finden Sie unter [So wird's gemacht: Erstellen einer einfachen XML-Grammatik](http://go.microsoft.com/fwlink/p/?LinkID=269887).

Beachten Sie folgende Punkte:

-   Sie können der Einschränkungssammlung mehrere Grammatikdatei-Einschränkungen hinzufügen.
-   Sie können die GRXML-Dateierweiterung für XML-basierte Grammatikdokumente verwenden, die den SRGS-Regeln entsprechen.

In diesem Beispiel wird eine SRGS-Grammatik verwendet, die in einer Datei namens „srgs.grxml“ (später beschrieben) definiert ist. In den Dateieigenschaften ist die **Paketaktion** auf **Inhalt** und **In Ausgabeverzeichnis kopieren** auf **Immer kopieren** gesetzt:

```CSharp
private async void Colors_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Add a grammar file constraint to the recognizer.
    var storageFile = await Windows.Storage.StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///Colors.grxml"));
    var grammarFileConstraint = new Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint(storageFile, "colors");

    speechRecognizer.UIOptions.ExampleText = @"Ex. &#39;blue background&#39;, &#39;green text&#39;";
    speechRecognizer.Constraints.Add(grammarFileConstraint);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

Diese SRGS-Datei (srgs.grxml) enthält Tags für die semantische Interpretation. Diese Tags liefern einen Mechanismus, mit dem übereinstimmende Grammatikdaten an Ihre App zurückgegeben werden. Die Grammatik muss der [Semantic Interpretation for Speech Recognition (SISR)1.0](http://go.microsoft.com/fwlink/p/?LinkID=201765)-Spezifikation des World Wide Web Consortium (W3C) entsprechen.

Hier horchen wir auf Varianten von „Ja“ und „Nein“.

```CSharp
<grammar xml:lang="en-US" 
         root="yesOrNo"
         version="1.0" 
         tag-format="semantics/1.0"
         xmlns="http://www.w3.org/2001/06/grammar">

    <!-- The following rules recognize variants of yes and no. -->
      <rule id="yesOrNo">
         <one-of>
            <item>
              <one-of>
                 <item>yes</item>
                 <item>yeah</item>
                 <item>yep</item>
                 <item>yup</item>
                 <item>un huh</item>
                 <item>yay yus</item>
              </one-of>
              <tag>out="yes";</tag>
            </item>
            <item>
              <one-of>
                 <item>no</item>
                 <item>nope</item>
                 <item>nah</item>
                 <item>uh uh</item>
               </one-of>
               <tag>out="no";</tag>
            </item>
         </one-of>
      </rule>
</grammar>
```

## Verwalten von Einschränkungen


Nachdem eine Einschränkungssammlung für die Erkennung geladen wurde, kann Ihre App verwalten, welche Einschränkungen für Erkennungsvorgänge aktiviert sind. Dazu wird für die [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn631402)-Eigenschaft einer Einschränkung **true** oder **false** festgelegt. Die Standardeinstellung lautet **true**.

Meist ist es effizienter, Einschränkungen einmal zu laden und dann bei Bedarf zu aktivieren bzw. zu deaktivieren, als Einschränkungen für jeden Erkennungsvorgang zu laden, zu entladen und zu kompilieren. Verwenden Sie die [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn631402)-Eigenschaft nach Bedarf.

Durch Begrenzung der Anzahl der Einschränkungen wird die Datenmenge beschränkt, die die Spracherkennung zum Suchen und Abgleichen mit der Spracheingabe benötigt. So können die Leistung und die Genauigkeit der Spracherkennung verbessert werden.

Entscheiden Sie, welche Einschränkungen aktiviert sind. Dies richtet sich nach den Wörtern, die für die App im Kontext des jeweiligen Erkennungsvorgangs zu erwarten sind. Wenn es in der App beispielsweise gerade darum geht, eine Farbe anzuzeigen, müssen Sie wahrscheinlich keine Einschränkung aktivieren, bei der die Bezeichnungen von Tieren erkannt werden.

Mit den Eigenschaften [**SpeechRecognizerUIOptions.AudiblePrompt**](https://msdn.microsoft.com/library/windows/apps/dn653235) und [**SpeechRecognizerUIOptions.ExampleText**](https://msdn.microsoft.com/library/windows/apps/dn653236) können Sie Benutzern mitteilen, was gesagt werden kann. Diese Eigenschaften werden mithilfe der [**SpeechRecognizer.UIOptions**](https://msdn.microsoft.com/library/windows/apps/dn653254)-Eigenschaft festgelegt. Wenn Sie Benutzer im Voraus darüber informieren, was sie bei einem Erkennungsvorgang sagen können, erhöht sich die Wahrscheinlichkeit, dass die Benutzer etwas sagen, was einer aktiven Einschränkung zugeordnet werden kann.

## Verwandte Artikel


* [Sprachinteraktionen](speech-interactions.md)

**Beispiele**
* [Beispiel zu Spracherkennung und Sprachsynthese](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 







<!--HONumber=Aug16_HO3-->


