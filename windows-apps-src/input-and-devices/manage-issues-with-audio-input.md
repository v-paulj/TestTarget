---
author: Karl-Bridge-Microsoft
Description: "Erfahren Sie, wie Sie Probleme mit der Genauigkeit der Spracherkennung behandeln, die auf die Qualität der Audioeingabe zurückzuführen sind."
title: Verwalten von Problemen bei der Audioeingabe
ms.assetid: 3E36C683-C96A-4FEE-AD52-FDB87E0CC299
label: Manage audio input issues
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 6dcab14a290367250e152fb8a1944a924d5aaf46

---

# Verwalten von Problemen bei der Audioeingabe

Erfahren Sie, wie Sie Probleme mit der Genauigkeit der Spracherkennung behandeln, die auf die Qualität der Audioeingabe zurückzuführen sind.

**Wichtige APIs**

-   [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226)
-   [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243)
-   [**SpeechRecognitionAudioProblem**](https://msdn.microsoft.com/library/windows/apps/dn631406)




## <span id="Assess_audio-input_quality"></span><span id="assess_audio-input_quality"></span><span id="ASSESS_AUDIO-INPUT_QUALITY"></span>Bewerten der Qualität der Audioeingabe


Wenn die Spracherkennung aktiviert ist, verwenden Sie das [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243)-Ereignis Ihrer Spracherkennung, um festzustellen, ob Audioprobleme die Spracheingabe stören. Das Ereignisargument ([**SpeechRecognitionQualityDegradingEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn631430)) enthält die [**Problem**](https://msdn.microsoft.com/library/windows/apps/dn631431)-Eigenschaft, die die Probleme mit der Audioeingabe aufzeigt.

Die Erkennung kann durch zu viele Hintergrundgeräusche, eine Stummschaltung des Mikrofons und die Lautstärke oder Geschwindigkeit des Lautsprechers beeinflusst werden.

Hier konfigurieren wir eine Spracherkennung und beginnen, auf das [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243)-Ereignis zu lauschen.

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

## <span id="Manage_the_speech-recognition_experience"></span><span id="manage_the_speech-recognition_experience"></span><span id="MANAGE_THE_SPEECH-RECOGNITION_EXPERIENCE"></span>Verwalten der Spracherkennungsfunktion


Mit der bereitgestellten Beschreibung der [**Problem**](https://msdn.microsoft.com/library/windows/apps/dn631431)-Eigenschaft können die Benutzer die Bedingungen für die Spracherkennung verbessern.

Hier erstellen wir einen Handler für das [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243)-Ereignis, der überprüft, ob die Lautstärke niedrig ist. Anschließend verwenden wir ein [**SpeechSynthesizer**](https://msdn.microsoft.com/library/windows/apps/dn298152)-Objekt, um den Benutzer aufzufordern, lauter zu sprechen.

```CSharp
private async void speechRecognizer_RecognitionQualityDegrading(
    Windows.Media.SpeechRecognition.SpeechRecognizer sender,
    Windows.Media.SpeechRecognition.SpeechRecognitionQualityDegradingEventArgs args)
{
    // Create an instance of a speech synthesis engine (voice).
    var speechSynthesizer =
        new Windows.Media.SpeechSynthesis.SpeechSynthesizer();

    // If input speech is too quiet, prompt the user to speak louder.
    if (args.Problem == Windows.Media.SpeechRecognition.SpeechRecognitionAudioProblem.TooQuiet)
    {
        // Generate the audio stream from plain text.
        Windows.Media.SpeechSynthesis.SpeechSynthesisStream stream;
        try
        {
            stream = await speechSynthesizer.SynthesizeTextToStreamAsync("Try speaking louder");
            stream.Seek(0);
        }
        catch (Exception)
        {
            stream = null;
        }

        // Send the stream to the MediaElement declared in XAML.
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.High, () =>
        {
            this.media.SetSource(stream, stream.ContentType);
        });
    }
}
```

## <span id="related_topics"></span>Verwandte Artikel


* [Sprachinteraktionen](speech-interactions.md)

**Beispiele**
* [Beispiel zu Spracherkennung und Sprachsynthese](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 







<!--HONumber=Jun16_HO3-->


