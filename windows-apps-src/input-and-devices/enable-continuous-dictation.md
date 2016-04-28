---
Description: description: Erfahren Sie, wie Sie die Erfassung und Erkennung langer Spracheingaben für kontinuierliches Diktieren ermöglichen.
title: title: Aktivieren des kontinuierlichen Diktierens
ms.assetid: 383B3E23-1678-4FBB-B36E-6DE2DA9CA9DC
label: Continuous dictation
template: detail.hbs
---

# ms.assetid: 383B3E23-1678-4FBB-B36E-6DE2DA9CA9DC


label: Kontinuierliches Diktieren Vorlage: detail.hbs

Kontinuierliches Diktieren

**\[ Aktualisiert für UWP-Apps unter Windows 10.**

-   [**Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]**](https://msdn.microsoft.com/library/windows/apps/dn913896)
-   [**Erfahren Sie, wie Sie die Erfassung und Erkennung langer Spracheingaben für kontinuierliches Diktieren ermöglichen.**](https://msdn.microsoft.com/library/windows/apps/dn913913)


Wichtige APIs

SpeechContinuousRecognitionSession



## ContinuousRecognitionSession


In [Spracherkennung](speech-recognition.md) haben Sie gelernt, wie Sie mithilfe der Methoden [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653244) oder [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245) eines [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226)-Objekts verhältnismäßig kurze Spracheingaben erfassen und erkennen. Beispiele hierfür sind das Verfassen einer SMS oder das Stellen einer Frage.

-   Bei längeren, kontinuierlichen Spracherkennungssitzungen (z. B. für Diktate oder E-Mails) wird hingegen die [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913)-Eigenschaft eines [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226)-Objekts verwendet, um ein [**SpeechContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913896)-Objekt zu erhalten.
-   <span id="Set_up"></span><span id="set_up"></span><span id="SET_UP"></span>Einrichtung
-   Zum Verwalten einer kontinuierlichen Diktiersitzung benötigt Ihre App einige Objekte:

Eine Instanz eines [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226)-Objekts. Einen Verweis auf einen UI-Verteiler zum Aktualisieren der Benutzeroberfläche während des Diktierens.

```CSharp
private SpeechRecognizer speechRecognizer;</code></pre></td>
</tr>
</tbody>
</table>
```

Eine Möglichkeit zum Nachverfolgen der kumulierten, vom Benutzer gesprochenen Wörter. Hier wird eine [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226)-Instanz als privates Feld der CodeBehind-Klasse deklariert.

Ihre App muss an einem anderen Ort einen Verweis speichern, wenn das fortlaufende Diktat nicht nur auf einer einzelnen XAML-Seite (Extensible Application Markup Language) Bestand haben soll.

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
// Speech events may originate from a thread other than the UI thread.
// Keep track of the UI thread dispatcher so that we can update the
// UI in a thread-safe manner.
private CoreDispatcher dispatcher;
```

Während des Diktats löst die Erkennung über einen Hintergrundthread Ereignisse aus. Da die Benutzeroberfläche in XAML nicht direkt von einem Hintergrundthread aktualisiert werden kann, muss Ihre App die Benutzeroberfläche mithilfe eines Verteilers auf der Grundlage von Erkennungsereignissen aktualisieren.

Hier deklarieren wir ein privates Feld, das später mit dem UI-Verteiler initialisiert wird. Zur Nachverfolgung der Spracheingaben des Benutzers müssen die von der Spracherkennung ausgelösten Erkennungsereignisse behandelt werden.

```CSharp
private StringBuilder dictatedTextBuilder;</code></pre></td>
</tr>
</tbody>
</table>
```

## Diese Ereignisse liefern die Erkennungsergebnisse für die einzelnen Äußerungen des Benutzers.


Hier wird ein [**StringBuilder**](https://msdn.microsoft.com/library/system.text.stringbuilder.aspx)-Objekt zum Speichern aller Erkennungsergebnisse der Sitzung verwendet.

-   Neue Ergebnisse werden während der Verarbeitung an das **StringBuilder**-Objekt angehängt.
-   <span id="Initialization"></span><span id="initialization"></span><span id="INITIALIZATION"></span>Initialisierung
-   Während der Initialisierung der kontinuierlichen Spracherkennung müssen folgende Schritte ausgeführt werden:
    Abrufen des Verteilers für den UI-Thread, wenn Sie die Benutzeroberfläche Ihrer App in den Ereignishandlern für die kontinuierliche Erkennung aktualisieren. Initialisieren der Spracherkennung. Kompilieren der integrierten Diktiergrammatik.

     

-   **Hinweis**   Die Spracherkennung benötigt mindestens eine Einschränkung, um das erkennbare Vokabular zu definieren.

Wenn Sie keine Einschränkung angeben, wird eine vordefinierte Diktiergrammatik verwendet.

1.  Siehe [Spracherkennung](speech-recognition.md). Einrichten der Listener für Erkennungsereignisse.

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
this.dispatcher = CoreWindow.GetForCurrentThread().Dispatcher;</code></pre></td>
    </tr>
    </tbody>
    </table>
```

2.  Die Spracherkennung wird im [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508)-Seitenereignis initialisiert.

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
this.speechRecognizer = new SpeechRecognizer();</code></pre></td>
    </tr>
    </tbody>
    </table>
```

3.  Da die von der Spracherkennung ausgelösten Ereignisse in einem Hintergrundthread auftreten, muss für die Aktualisierung des UI-Threads ein Verweis auf den Verteiler erstellt werden.

    [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) wird stets im UI-Thread aufgerufen. Anschließend wird die [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226)-Instanz initialisiert.

    Anschließend wird die Grammatik hinzugefügt, die alle Wörter und Ausdrücke definiert, die von [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) erkannt werden können, und kompiliert.

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
SpeechRecognitionCompilationResult result =
      await speechRecognizer.CompileConstraintsAsync();
```

## Wenn Sie nicht explizit eine Grammatik angeben, wird standardmäßig eine vordefinierte Grammatik verwendet.


Die Standardgrammatik eignet sich in der Regel am besten für allgemeines Diktieren. Hier wird [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240) direkt aufgerufen, ohne eine Grammatik hinzuzufügen.

<span id="Handle_recognition_events"></span><span id="handle_recognition_events"></span><span id="HANDLE_RECOGNITION_EVENTS"></span>Behandeln von Erkennungsereignissen

Hier können Sie durch Aufrufen von [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653244) oder [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245) eine einzelne kurze Äußerung oder Phrase erfassen.

Es soll jedoch eine längere, kontinuierliche Erkennungssitzung erfasst werden.

-   Hierzu werden Ereignislistener angegeben, die im Hintergrund ausgeführt werden, während der Benutzer spricht, und Handler für die Erstellung der Diktatzeichenfolge definiert.
-   Anschließend wird die [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913)-Eigenschaft unserer Erkennung verwendet, um ein [**SpeechContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913896)-Objekt zu erhalten, das Methoden und Ereignisse für die Verwaltung einer kontinuierlichen Erkennungssitzung bereitstellt.

Zwei Ereignisse sind besonders wichtig: [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900), das eintritt, wenn die Erkennung Ergebnisse generiert hat. [**Completed**](https://msdn.microsoft.com/library/windows/apps/dn913899), das eintritt, wenn die kontinuierliche Erkennungssitzung beendet ist.

Das [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900)-Ereignis wird ausgelöst, während der Benutzer spricht.

-   Die Erkennung hört kontinuierlich auf den Benutzer und löst in regelmäßigen Abständen ein Ereignis aus, das einen Teil der Spracheingabe übergibt. Die Spracheingabe muss mit der [**Result**](https://msdn.microsoft.com/library/windows/apps/dn913895)-Eigenschaft des Ereignisarguments untersucht werden. Außerdem muss im Ereignishandler eine geeignete Aktion ausgeführt werden, um den Text beispielsweise einem StringBuilder-Objekt anzufügen.
-   Als Instanz von [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) ist die Eigenschaft [**Result**](https://msdn.microsoft.com/library/windows/apps/dn913895) nützlich, um zu ermitteln, ob die Spracheingabe akzeptiert werden soll:

1.  [**Status**](https://msdn.microsoft.com/library/windows/apps/dn631440) gibt an, ob die Erkennung erfolgreich war.

```    CSharp
speechRecognizer.ContinuousRecognitionSession.ResultGenerated +=
        ContinuousRecognitionSession_ResultGenerated;
```

2.  Die Erkennung kann aus unterschiedlichen Gründen scheitern. [**Confidence**](https://msdn.microsoft.com/library/windows/apps/dn631434) gibt die relative Wahrscheinlichkeit dafür an, dass die Wörter von der Erkennung korrekt verstanden wurden. Hier wird der Handler für das kontinuierliche [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900)-Erkennungsereignis im [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508)-Seitenereignis registriert.

    Anschließend wird die Eigenschaft [**Confidence**](https://msdn.microsoft.com/library/windows/apps/dn631434) überprüft. Wenn der Wert von Confidence [**Medium**](https://msdn.microsoft.com/library/windows/apps/dn631409) oder besser ist, wird der Text an das StringBuilder-Objekt angefügt.

     

```    CSharp
private async void ContinuousRecognitionSession_ResultGenerated(
      SpeechContinuousRecognitionSession sender,
      SpeechContinuousRecognitionResultGeneratedEventArgs args)
      {

        if (args.Result.Confidence == SpeechRecognitionConfidence.Medium ||
          args.Result.Confidence == SpeechRecognitionConfidence.High)
          {
            dictatedTextBuilder.Append(args.Result.Text + " ");

            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              dictationTextBox.Text = dictatedTextBuilder.ToString();
              btnClearText.IsEnabled = true;
            });
          }
        else
        {
          await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              dictationTextBox.Text = dictatedTextBuilder.ToString();
            });
        }
      }
```

3.  Während der Eingabeerfassung wird auch die Benutzeroberfläche aktualisiert.

    **Hinweis**  Das [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900)-Ereignis wird in einem Hintergrundthread ausgelöst, der die Benutzeroberfläche nicht direkt aktualisieren kann. Wenn ein Handler die UI aktualisieren muss (wie dies beim \[Sprach- und TTS-Beispiel\] der Fall ist), müssen die Aktualisierungen über die [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317)-Methode des Verteilers an den UI-Thread übergeben werden. Anschließend wird das [**Completed**](https://msdn.microsoft.com/library/windows/apps/dn913899)-Ereignis behandelt, das das Ende des kontinuierlichen Diktierens angibt.

    Die Sitzung endet durch Aufrufen der Methoden [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/dn913908) oder [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898) (wie im nächsten Abschnitt beschrieben).

```    CSharp
speechRecognizer.ContinuousRecognitionSession.Completed +=
      ContinuousRecognitionSession_Completed;
```

4.  Die Sitzung kann auch beendet werden, wenn ein Fehler auftritt oder der Benutzer nicht mehr spricht. Überprüfen Sie die [**Status**](https://msdn.microsoft.com/library/windows/apps/dn631440)-Eigenschaft des Ereignisarguments, um den Grund für die Sitzungsbeendigung zu ermitteln ([**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433)). Hier wird der Handler für das kontinuierliche [**Completed**](https://msdn.microsoft.com/library/windows/apps/dn913899)-Erkennungsereignis im [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508)-Seitenereignis registriert. Der Ereignishandler überprüft die Status-Eigenschaft, um zu ermitteln, ob die Erkennung erfolgreich war.

    Er behandelt auch den Fall, dass ein Benutzer nicht mehr spricht. Häufig wird [**TimeoutExceeded**](https://msdn.microsoft.com/library/windows/apps/dn631433) als erfolgreiche Erkennung betrachtet, da der Benutzer aufgehört hat, zu sprechen.

     

```    CSharp
private async void ContinuousRecognitionSession_Completed(
      SpeechContinuousRecognitionSession sender,
      SpeechContinuousRecognitionCompletedEventArgs args)
      {
        if (args.Status != SpeechRecognitionResultStatus.Success)
        {
          if (args.Status == SpeechRecognitionResultStatus.TimeoutExceeded)
          {
            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              rootPage.NotifyUser(
                "Automatic Time Out of Dictation",
                NotifyType.StatusMessage);

              DictationButtonText.Text = " Continuous Recognition";
              dictationTextBox.Text = dictatedTextBuilder.ToString();
            });
          }
          else
          {
            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              rootPage.NotifyUser(
                "Continuous Recognition Completed: " + args.Status.ToString(),
                NotifyType.StatusMessage);

              DictationButtonText.Text = " Continuous Recognition";
            });
          }
        }
      }
```

## Behandeln Sie diesen Fall in Ihrem Code, um die Benutzerfreundlichkeit zu verbessern.


**Hinweis**  Das [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900)-Ereignis wird in einem Hintergrundthread ausgelöst, der die Benutzeroberfläche nicht direkt aktualisieren kann. Wenn ein Handler die UI aktualisieren muss (wie dies beim \[Sprach- und TTS-Beispiel\] der Fall ist), müssen die Aktualisierungen über die [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317)-Methode des Verteilers an den UI-Thread übergeben werden. <span id="Provide_ongoing_recognition_feedback"></span><span id="provide_ongoing_recognition_feedback"></span><span id="PROVIDE_ONGOING_RECOGNITION_FEEDBACK"></span>Bereitstellen von Erkennungsfeedback Beim Sprechen wird häufig Kontext benötigt, um das Gesagte zu verstehen.

Auch die Spracherkennung benötigt häufig Kontext, um Erkennungsergebnisse mit hoher Trefferwahrscheinlichkeit bereitstellen zu können.

So sind beispielsweise die Wörter „seid“ und „seit“ ohne den Kontext anderer Wörter nicht voneinander zu unterscheiden. Das [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900)-Ereignis wird erst ausgelöst, wenn die Erkennung zu einem gewissen Grad davon überzeugt ist, ein Wort oder eine Formulierung korrekt erkannt zu haben. Dies kann zu einer nichtoptimalen Benutzererfahrung führen, wenn der Benutzer weiterspricht und von der Erkennung keine Ergebnisse bereitgestellt werden, bis die Erkennungswahrscheinlichkeit hoch genug ist, um das [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900)-Ereignis auszulösen. Behandeln Sie das [**HypothesisGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913914)-Ereignis, um dieses scheinbare Ausbleiben einer Reaktion zu verbessern. Dieses Ereignis wird ausgelöst, wenn die Erkennung einen neuen Satz potenzieller Übereinstimmungen für das verarbeitete Wort generiert.

Das Ereignisargument stellt eine [**Hypothesis**](https://msdn.microsoft.com/library/windows/apps/dn913911)-Eigenschaft mit den aktuellen Übereinstimmungen bereit. Zeigen Sie diese dem Benutzer an, während dieser weiterspricht, um deutlich zu machen, dass die Verarbeitung weiterhin aktiv ist.

```CSharp
private async void SpeechRecognizer_HypothesisGenerated(
  SpeechRecognizer sender,
  SpeechRecognitionHypothesisGeneratedEventArgs args)
  {

    string hypothesis = args.Hypothesis.Text;
    string textboxContent = dictatedTextBuilder.ToString() + " " + hypothesis + " ...";

    await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
      dictationTextBox.Text = textboxContent;
      btnClearText.IsEnabled = true;
    });
  }
```

## Sobald eine hohe Trefferwahrscheinlichkeit erreicht und ein Erkennungsergebnis ermittelt wurde, ersetzen Sie die vorläufigen **Hypothesis**-Ergebnisse durch das endgültige [**Result**](https://msdn.microsoft.com/library/windows/apps/dn913895), das im Ereignis [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) bereitgestellt wird.


Hier werden der hypothetische Text sowie Auslassungspunkte („...“) an den aktuellen Wert der [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)-Ausgabe angefügt. Der Inhalt des Textfelds wird so lange mit neu generierten Hypothesen aktualisiert, bis die endgültigen Ergebnisse aus dem [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900)-Ereignis abgerufen werden.

<span id="Start_and_stop_recognition"></span><span id="start_and_stop_recognition"></span><span id="START_AND_STOP_RECOGNITION"></span>Starten und Beenden der Erkennung

```CSharp
if (speechRecognizer.State == SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.StartAsync();
}
```

Überprüfen Sie vor dem Starten einer Erkennungssitzung den Wert der [**State**](https://msdn.microsoft.com/library/windows/apps/dn913915)-Eigenschaft der Spracherkennung.

-   Die Spracherkennung muss sich im Zustand [**Idle**](https://msdn.microsoft.com/library/windows/apps/dn653227) befinden.
-   Nach der Überprüfung des Zustands der Spracherkennung wird die Sitzung durch Aufrufen der [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/dn913901)-Methode der [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913)-Spracherkennungseigenschaft gestartet.

Die Erkennung kann auf zwei Arten beendet werden:

```CSharp
if (speechRecognizer.State != SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.CancelAsync();
}
```

**[**StopAsync**](https://msdn.microsoft.com/library/windows/apps/dn913908) wartet, bis alle ausstehenden Erkennungsereignisse abgeschlossen sind. ([**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) wird weiter ausgelöst, bis alle ausstehenden Erkennungsvorgänge abgeschlossen wurden).**  
[**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898) beendet die Erkennungssitzung umgehend und verwirft alle ausstehenden Ergebnisse.

Nach der Überprüfung des Zustands der Spracherkennung wird die Sitzung durch Aufrufen der [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898)-Methode der [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913)-Spracherkennungseigenschaft beendet. **Hinweis**

Ein [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900)-Ereignis kann nach dem Aufrufen von [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898) auftreten. Aufgrund des Multithreadings befindet sich unter Umständen noch ein [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900)-Ereignis im Stapel, wenn [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898) aufgerufen wird.

 

## In diesem Fall wird dennoch das **ResultGenerated**-Ereignis ausgelöst.


* [Wenn Sie beim Abbrechen der Erkennungssitzung private Felder festlegen, müssen deren Werte immer im [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900)-Handler bestätigt werden.](speech-interactions.md)

**Gehen Sie beispielsweise nicht davon aus, dass ein Feld in Ihrem Handler initialisiert wird, wenn Sie es beim Abbrechen der Sitzung auf Null festlegen.**
* [<span id="related_topics"></span>Verwandte Artikel](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 






<!--HONumber=Apr16_HO3-->


