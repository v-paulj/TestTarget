---
author: Karl-Bridge-Microsoft
Description: "Nutzen Sie die Spracherkennung als Eingabemöglichkeit oder zum Ausführen einer Aktion, eines Befehls oder einer Aufgabe."
title: Spracherkennung
ms.assetid: 553C0FB7-35BC-4894-9EF1-906139E17552
label: Speech recognition
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 6d1449ede6912add8b8f7e60760d4547c035ed60

---

# Spracherkennung


Nutzen Sie die Spracherkennung als Eingabemöglichkeit oder zum Ausführen einer Aktion, eines Befehls oder einer Aufgabe.

**Wichtige APIs**

-   [**Windows.Media.SpeechRecognition**](https://msdn.microsoft.com/library/windows/apps/dn653262)



Die Spracherkennung besteht aus einer Sprachlaufzeit, Erkennungs-APIs zum Programmieren der Laufzeit, einsatzfähiger Grammatik für das Diktieren und die Websuche und einer Standard-UI, die Benutzern das Auffinden und Verwenden der Spracherkennungsfeatures erleichtert.


## <span id="Set_up_the_audio_feed"></span><span id="set_up_the_audio_feed"></span><span id="SET_UP_THE_AUDIO_FEED"></span>Einrichten des Audiofeeds


Stellen Sie sicher, dass Ihr Gerät über ein Mikrofon oder Ähnliches verfügt.

Legen Sie die Gerätefunktion **Mikrofon** ([**DeviceCapability**](https://msdn.microsoft.com/library/windows/apps/br211430)) im [App-Paket](https://msdn.microsoft.com/library/windows/apps/br211474)manifest (**package.appxmanifest**-Datei) fest, um Zugriff auf den Audiofeed des Mikrofons zu erhalten. Mit dieser Funktion kann die App Audio von angeschlossenen Mikrofonen aufzeichnen.

Informationen finden Sie unter [Deklaration der App-Funktionen](https://msdn.microsoft.com/library/windows/apps/mt270968).

## <span id="Recognize_speech_input"></span><span id="recognize_speech_input"></span><span id="RECOGNIZE_SPEECH_INPUT"></span>Erkennen von Spracheingabe


Mit einer *Einschränkung* werden die Wörter und Wortgruppen (Vokabular) definiert, die eine App bei der Spracheingabe erkennt. Einschränkungen sind der Kern der Spracherkennung und verleihen Ihrer App Kontrolle über die Genauigkeit der Spracherkennung.

Sie können verschiedene Arten von Einschränkungen bei der Spracherkennung nutzen:

1.  **Vordefinierte Grammatiken** ([**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446)).

    Mit vordefinierten Diktier- und Websuchgrammatiken können Sie eine Spracherkennung für Ihre App bereitstellen, ohne eine Grammatik erstellen zu müssen. Bei Verwendung dieser Grammatiken wird die Spracherkennung von einem Remotewebdienst durchgeführt, und die Ergebnisse werden an das Gerät zurückgegeben.

    Die Standardgrammatik der Freitext-Diktierfunktion erkennt die meisten Wörter und Ausdrücke, die Benutzer in einer bestimmten Sprache sagen können, und ist für die Erkennung kurzer Ausdrücke optimiert. Die vordefinierte Grammatik für das Diktieren kommt dann zum Einsatz, wenn Sie keine Einschränkungen für Ihr [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226)-Objekt festlegen. Die Freitext-Diktierfunktion ist nützlich, wenn Sie nicht einschränken möchten, was Benutzer sagen können. Typische Verwendungsmöglichkeiten sind das Erstellen von Notizen oder das Diktieren eines Nachrichtentexts.

    Die Grammatik für die Websuche enthält wie die Diktiergrammatik eine große Anzahl von Wörtern und Ausdrücken, die Benutzer sagen können. Sie ist allerdings für die Erkennung von Begriffen optimiert, die beim Suchen im Web häufig verwendet werden.

    **Hinweis**  Da vordefinierte Diktier- und Websuchgrammatiken sehr umfangreich sein können und online bereitgestellt werden (nicht auf dem Gerät), kann die Leistung schlechter sein als bei einer lokal auf dem Gerät installierten benutzerdefinierten Grammatik.

     

    Diese vordefinierten Grammatiken können zum Erkennen von bis zu zehn Sekunden Spracheingabe verwendet werden. Sie müssen dazu keinen Code selbst erstellen. Sie erfordern jedoch eine Netzwerkverbindung.

    Zum Verwenden der Webdiensteinschränkungen muss die Unterstützung für die Spracheingabe und das Diktieren unter **Einstellungen** &gt;Datenschutz &gt; „Datenschutzeinstellungen für Sprache, Freihand und Eingabe“ aktiviert sein.

    Hier wird erläutert, wie Sie testen, ob die Spracheingabe aktiviert ist, und wie Sie die Seite „Einstellungen &gt; Datenschutz &gt; Datenschutzeinstellungen für Sprache, Freihand und Eingabe“ öffnen.

    Zuerst initialisieren wir eine globale Variable (HResultPrivacyStatementDeclined) für den HResult-Wert 0x80045509. Weitere Informationen finden Sie unter [Ausnahmebehandlung in C# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/dn532194).

```    CSharp
private static uint HResultPrivacyStatementDeclined = 0x80045509;</code></pre></td>
    </tr>
    </tbody>
    </table>
```

    We then catch any standard exceptions during recogntion and test if the [**HResult**](https://msdn.microsoft.com/library/windows/apps/br206579) value is equal to the value of the HResultPrivacyStatementDeclined variable. If so, we display a warning and call `await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts"));` to open the Settings page.

    <span codelanguage="CSharp"></span>
```    CSharp
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
catch (Exception exception)
    {
      // Handle the speech privacy policy error.
      if ((uint)exception.HResult == HResultPrivacyStatementDeclined)
      {
        resultTextBlock.Visibility = Visibility.Visible;
        resultTextBlock.Text = "The privacy statement was declined. 
          Go to Settings -> Privacy -> Speech, inking and typing, and ensure you 
          have viewed the privacy policy, and &#39;Get To Know You&#39; is enabled.";
        // Open the privacy/speech, inking, and typing settings page.
        await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts")); 
      }
      else
      {
        var messageDialog = new Windows.UI.Popups.MessageDialog(exception.Message, "Exception");
        await messageDialog.ShowAsync();
      }
    }
```

2.  **Einschränkungen per programmgesteuerter Liste** ([**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421))

    Einschränkungen per programmgesteuerter Liste sind eine unkomplizierte Methode für die Erstellung einfacher Grammatiken in Form einer Liste von Wörtern und Ausdrücken. Eine Einschränkungsliste eignet sich gut für die Erkennung kurzer, einzelner Ausdrücke. Das explizite Angeben aller Wörter in einer Grammatik verbessert auch die Erkennungsgenauigkeit, da das Spracherkennungsmodul nur eine Übereinstimmung bestätigen muss. Die Liste kann auch programmgesteuert aktualisiert werden.

    Eine Einschränkungsliste besteht aus einem Array von Zeichenfolgen, die die Spracheingaben darstellen, die von Ihrer App für einen Erkennungsvorgang akzeptiert werden. Sie können in Ihrer App eine Einschränkungsliste einrichten, indem Sie ein Einschränkungslistenobjekt für die Spracherkennung erstellen und ein Array mit Zeichenfolgen übergeben. Fügen Sie dieses Objekt dann der Einschränkungsauflistung der Erkennung hinzu. Die Erkennung ist erfolgreich, wenn die Spracherkennung die Zeichenfolgen des Arrays erkennt.

3.  **SRGS-Grammatiken** ([**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412))

    Eine Speech Recognition Grammar Specification (SRGS)-Grammatik ist ein statisches Dokument, das im Gegensatz zu einer Einschränkung per programmgesteuerter Liste das in [SRGS Version 1.0](http://go.microsoft.com/fwlink/p/?LinkID=262302) definierte XML-Format verwendet. Eine SRGS-Grammatik bietet die höchstmögliche Kontrolle über die Spracherkennungsfunktion, da Sie mehrere semantische Bedeutungen in einem einzigen Erkennungsvorgang erfassen können.

4.  **Einschränkungen für Sprachbefehle** ([**SpeechRecognitionVoiceCommandDefinitionConstraint**](https://msdn.microsoft.com/library/windows/apps/dn653220))

    Verwenden Sie eine Voice Command Definition-(VCD-)XML-Datei, um die Befehle zu definieren, mit denen der Benutzer Aktionen initiieren kann, wenn er Ihre App aktiviert. Weitere Details finden Sie unter [Starten einer Vordergrund-App mit Sprachbefehlen in Cortana](launch-a-foreground-app-with-voice-commands-in-cortana.md).

**Hinweis**  Der von Ihnen verwendete Einschränkungstyp richtet sich nach der Komplexität der Erkennungsfunktion, die Sie erstellen möchten. Für eine bestimmte Erkennungsaufgabe kann jeweils einer der Ansätze am besten geeignet sein, und vielleicht haben Sie in Ihrer App sogar für alle Einschränkungsarten Verwendung.
Informationen zu den ersten Schritten mit Einschränkungen finden Sie unter [Definieren von benutzerdefinierten Erkennungseinschränkungen](define-custom-recognition-constraints.md).

 

Die vordefinierte Diktiergrammatik von universellen Windows-Apps erkennt die meisten Wörter und kurzen Wortgruppen einer Sprache. Sie wird standardmäßig aktiviert, wenn ein Spracherkennungsobjekt ohne benutzerdefinierte Einschränkungen instanziiert wird.

In diesem Beispiel zeigen wir Ihnen, wie Sie:

-   eine Spracherkennung erstellen,
-   die standardmäßigen Einschränkungen von universellen Windows-Apps kompilieren (es wurde keine Grammatik zum Grammatiksatz der Spracherkennung hinzugefügt) und
-   die Spracherkennung starten, indem Sie die einfache Erkennungs-UI und das TTS-Feedback der [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245)-Methode verwenden. Verwenden Sie die [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653244)-Methode, wenn die Standard-UI nicht benötigt wird.

```CSharp
private async void StartRecognizing_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Compile the dictation grammar by default.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <span id="Customize_the_recognition_UI"></span><span id="customize_the_recognition_ui"></span><span id="CUSTOMIZE_THE_RECOGNITION_UI"></span>Anpassen der Erkennungs-UI


Wenn Ihre App die Spracherkennung durch Aufruf von [**SpeechRecognizer.RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245) zu leisten versucht, werden mehrere Bildschirme in der folgenden Reihenfolge angezeigt.

Wenn Sie eine Einschränkung auf der Grundlage einer vordefinierten Grammatik (Diktat oder Websuche) verwenden:

-   Der Bildschirm **Spracherkennung**
-   Der Bildschirm **Verarbeitung**
-   Der Bildschirm **Gehört** oder der Fehlerbildschirm

Wenn Sie eine Einschränkung auf der Grundlage von Wörtern oder Ausdrücken oder eine Einschränkung auf der Grundlage einer SRGS-Grammatikdatei verwenden:

-   Der Bildschirm **Spracherkennung**
-   Der Bildschirm **Sagten Sie** , falls die vom Benutzer ausgesprochenen Wörter als mehrere potenzielle Ergebnisse interpretiert werden können.
-   Der Bildschirm **Gehört** oder der Fehlerbildschirm

Die folgende Abbildung zeigt ein Beispiel für den Fluss zwischen Bildschirmen für die Spracherkennung mit einer Einschränkung auf der Grundlage einer SRGS-Grammatikdatei. In diesem Beispiel war die Spracherkennung erfolgreich.

![initial Erkennung screen for a constraint based on a sgrs grammar file](images/speech-listening-initial.png)

![intermediate Erkennung screen for a constraint based on a sgrs grammar file](images/speech-listening-intermediate.png)

![final Erkennung screen for a constraint based on a sgrs grammar file](images/speech-listening-complete.png)

Der **Spracherkennung**-Bildschirm kann Beispiele für Wörter oder Ausdrücke zur Erkennung durch die App bereitstellen. Hier zeigen wir, wie Sie die Eigenschaften der [**SpeechRecognizerUIOptions**](https://msdn.microsoft.com/library/windows/apps/dn653234)-Klasse (abgerufen durch Aufrufen der [**SpeechRecognizer.UIOptions**](https://msdn.microsoft.com/library/windows/apps/dn653254)-Eigenschaft) zur Anpassung von Inhalten auf dem **Spracherkennung**-Bildschirm verwenden.

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

## <span id="related_topics"></span>Verwandte Artikel


**Entwickler**
* [Interaktionen mit der Spracherkennung](speech-interactions.md)
            
          
            **Designer**
* [Entwicklungsrichtlinien für die Spracherkennung](https://msdn.microsoft.com/library/windows/apps/dn596121)
            
          
            **Beispiele**
* [Beispiel zu Spracherkennung und Sprachsynthese](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 







<!--HONumber=Jun16_HO3-->


