---
author: drewbatgit
ms.assetid: B5E3A66D-0453-4D95-A3DB-8E650540A300
description: In diesem Artikel wird beschrieben, wie Sie den MediaProcessingTrigger und eine Hintergrundaufgabe verwenden, um Mediendateien im Hintergrund zu verarbeiten.
title: Verarbeiten von Mediendateien im Hintergrund
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: dcf655ff80c4463a567ade0b6d1cc784b60c18be

---

# Verarbeiten von Mediendateien im Hintergrund

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


In diesem Artikel wird beschrieben, wie Sie [**MediaProcessingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806005) und eine Hintergrundaufgabe verwenden, um Mediendateien im Hintergrund zu verarbeiten.

Mit der in diesem Artikel beschriebenen Beispiel-App kann der Benutzer eine zu transcodierende Eingabemediendatei auswählen und eine Ausgabedatei für das Transcodierungsergebnis angeben. Anschließend wird eine Hintergrundaufgabe gestartet, um den Transcodierungsvorgang auszuführen. [
            **MediaProcessingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806005) unterstützt außer der Transcodierung noch viele andere Medienverarbeitungsszenarien, einschließlich Rendern von Medienkompositionen auf Datenträger und Hochladen von verarbeiteten Mediendateien nach Abschluss der Verarbeitung.

Ausführlichere Informationen zu den verschiedenen universellen Windows-App-Features in diesem Beispiel finden Sie unter:

-   [Transcodieren von Mediendateien](transcode-media-files.md)
-   [Starten, Fortsetzen und Hintergrundaufgaben](https://msdn.microsoft.com/library/windows/apps/mt227652)
-   [Kacheln, Signale und Benachrichtigungen](https://msdn.microsoft.com/library/windows/apps/mt185606)

## Erstellen einer Hintergrundaufgabe für die Medienverarbeitung

Um Ihrer vorhandenen Microsoft Visual Studio-Projektmappe eine Hintergrundaufgabe hinzuzufügen, geben Sie einen Namen für die Komposition ein.

1.  Wählen Sie im Menü **Datei** die Option **Hinzufügen** und dann **Neues Projekt** aus.
2.  Wählen Sie den Projekttyp **Komponente für Windows-Runtime (Universal Windows)** aus.
3.  Geben Sie einen Namen für das neue Komponentenprojekt ein. In diesem Beispiel wird der Projektname **MediaProcessingBackgroundTask** verwendet.
4.  Klicken Sie auf „OK“.

Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Symbol für die Datei „Class1.cs“, die standardmäßig erstellt wird, und wählen Sie **Umbenennen**aus. Benennen Sie die Datei in „MediaProcessingTask.cs“ um. Wenn Sie von Visual Studio gefragt werden, ob Sie alle Verweise auf diese Klasse umbenennen möchten, klicken Sie auf **Ja**.

Fügen Sie in der umbenannten Klassendatei die folgenden **using**-Direktiven hinzu, um diese Namespaces in Ihr Projekt einzuschließen.
                                  
[!code-cs[BackgroundUsing](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundUsing)]

Aktualisieren Sie die Klassendeklaration so, dass die Klasse von [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) erbt.

[!code-cs[BackgroundClass](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundClass)]

Fügen Sie der Klasse die folgenden Membervariablen hinzu:

-   Eine [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797)-Schnittstelle, die verwendet wird, um die Vordergrund-App mit dem Status der Hintergrundaufgabe zu aktualisieren.
-   Eine [**BackgroundTaskDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700499)-Klasse, die verhindert, dass das System die Hintergrundaufgabe herunterfährt, während die Medientranscodierung asynchron ausgeführt wird.
-   Ein **CancellationTokenSource**-Objekt, mit dem der asynchrone Transcodierungsvorgang abgebrochen werden kann.
-   Das [**MediaTranscoder**](https://msdn.microsoft.com/library/windows/apps/br207080)-Objekt, das zum Transcodieren von Mediendateien verwendet wird.

[!code-cs[BackgroundMembers](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundMembers)]

Das System ruft die [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811)-Methode einer Hintergrundaufgabe auf, wenn die Aufgabe gestartet wird. Legen Sie das an die Methode übergebene [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)-Objekt auf die entsprechende Membervariable fest. Registrieren Sie einen Handler für das [**Canceled**](https://msdn.microsoft.com/library/windows/apps/br224798)-Ereignis, das ausgelöst wird, wenn das System die Hintergrundaufgabe herunterfahren muss. Legen Sie anschließend die [**Progress**](https://msdn.microsoft.com/library/windows/apps/br224800)-Eigenschaft auf 0 (null) fest.

Rufen Sie als Nächstes die [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700507)-Methode des Hintergrundaufgabenobjekts auf, um eine Verzögerung abzurufen. Dies weist das System an, die Aufgabe nicht herunterzufahren, da Sie asynchrone Vorgänge ausführen.

Rufen Sie als Nächstes die **TranscodeFileAsync**-Hilfsmethode auf, die im nächsten Abschnitt definiert wird. Wenn diese Methode erfolgreich abgeschlossen wird, wird eine Hilfsmethode aufgerufen, um eine Popupbenachrichtigung zu öffnen, die den Benutzer darauf hinweist, dass die Transcodierung abgeschlossen ist.

Rufen Sie am Ende der **Run**-Methode die [**Complete**](https://msdn.microsoft.com/library/windows/apps/hh700504)-Methode für das Verzögerungsobjekt auf, um dem System mitzuteilen, dass die Hintergrundaufgabe abgeschlossen ist und beendet werden kann.

[!code-cs[Run](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetRun)]

In der **TranscodeFileAsync**-Hilfsmethode werden die Dateinamen für die Eingabe- und Ausgabedateien für die Transcodierungsvorgänge aus der [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622)-Eigenschaft für Ihre App abgerufen. Diese Werte werden von der Vordergrund-App festgelegt. Erstellen Sie ein [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Objekt für die Eingabe- und Ausgabedateien, und erstellen Sie dann ein Codierungsprofil für die Transcodierung.

Rufen Sie [**PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936) auf, und übergeben Sie die Eingabedatei, die Ausgabedatei und das Codierungsprofil. Das von diesem Aufruf zurückgegebene [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941)-Objekt teilt Ihnen mit, ob die Transcodierung durchgeführt werden kann. Wenn die [**CanTranscode**](https://msdn.microsoft.com/library/windows/apps/hh700942)-Eigenschaft „true“ ist, rufen Sie [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) auf, um den Transcodierungsvorgang auszuführen.

Mit der **AsTask**-Methode können Sie den Status des asynchronen Vorgangs nachverfolgen oder den Vorgang abbrechen. Erstellen Sie ein neues **Progress**-Objekt, wobei Sie die gewünschten Statuseinheiten und den Namen der Methode angeben, die aufgerufen wird, um Sie über den aktuellen Status der Aufgabe zu informieren. Übergeben Sie das **Progress**-Objekt zusammen mit dem Abbruchtoken, das Ihnen das Abbrechen der Aufgabe ermöglicht, an die **AsTask**-Methode.

[!code-cs[TranscodeFileAsync](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetTranscodeFileAsync)]

Legen Sie in der Methode, die Sie zum Erstellen des Statusobjekts im vorherigen Schritt verwendet haben (**Progress**), den Status der Hintergrundaufgabeninstanz fest. Hierdurch wird der Status an die Vordergrund-App übergeben, sofern diese ausgeführt wird.

[!code-cs[Progress](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetProgress)]

Die **SendToastNotification**-Hilfsmethode erstellt eine neue Popupbenachrichtigung, indem ein XML-Vorlagendokument für ein Popup abgerufen wird, das nur Text enthält. Das Textelement des Popup-XML-Codes wird festgelegt. Anschließend wird ein neues [**ToastNotification**](https://msdn.microsoft.com/library/windows/apps/br208641)-Objekt aus dem XML-Dokument erstellt. Schließlich wird dem Benutzer das Popup durch Aufrufen von [**ToastNotifier.Show**](https://msdn.microsoft.com/library/windows/apps/br208659) angezeigt.

[!code-cs[SendToastNotification](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetSendToastNotification)]

## Registrieren und Starten der Hintergrundaufgabe

Bevor die Hintergrundaufgabe aus der Vordergrund-App gestartet werden kann, müssen Sie die Datei „Package.appmanifest“ der Vordergrund-App aktualisieren, um dem System mitzuteilen, dass Ihre App eine Hintergrundaufgabe verwendet.

1.  Doppelklicken Sie im **Projektmappen-Explorer** auf das Symbol der Datei „Package.appmanifest“, um den Manifest-Editor zu öffnen.
2.  Klicken Sie auf die Registerkarte **Deklarationen**.
3.  Wählen Sie unter **Verfügbare Deklarationen** die Option **Hintergrundaufgaben** aus, und klicken Sie auf **Hinzufügen**.
4.  Vergewissern Sie sich, dass unter **Unterstützte Deklarationen** das Element **Hintergrundaufgaben** ausgewählt ist. Aktivieren Sie unter **Eigenschaften** das Kontrollkästchen für **Medienverarbeitung**.
5.  Geben Sie im Textfeld **Einstiegspunkt** den Namespace und den Klassennamen für den Hintergrundtest durch einen Punkt getrennt an. Für dieses Beispiel lautet die Eingabe:
   ```csharp
   MediaProcessingBackgroundTask.MediaProcessingTask
   ```
Als Nächstes müssen Sie der Vordergrund-App einen Verweis auf die Hintergrundaufgabe hinzufügen.
1.  Klicken Sie im **Projektmappen-Explorer** unter dem Vordergrund-App-Projekt mit der rechten Maustaste auf den Ordner **Verweise**, und wählen Sie **Verweis hinzufügen** aus.
2.  Erweitern Sie den Knoten **Projekte**, und wählen Sie **Projektmappe** aus.
3.  Aktivieren Sie das Kontrollkästchen neben dem Hintergrundaufgabenprojekt, und klicken Sie auf **OK**.

Fügen Sie den restlichen Code in diesem Beispiel der Vordergrund-App hinzu. Zunächst müssen Sie dem Projekt die folgenden Namespaces hinzufügen.

[!code-cs[ForegroundUsing](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetForegroundUsing)]

Als Nächstes fügen Sie die folgenden Membervariablen hinzu, die zum Registrieren der Hintergrundaufgabe benötigt werden.

[!code-cs[ForegroundMembers](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetForegroundMembers)]

Die **PickFilesToTranscode**-Hilfsmethode verwendet eine [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)-Klasse und eine [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)-Klasse, um die Eingabe- und Ausgabedateien für die Transcodierung zu öffnen. Der Benutzer kann Dateien an einem Speicherort auswählen, auf den Ihre App keinen Zugriff auf. Um sicherzustellen, dass die Hintergrundaufgabe die Dateien öffnen kann, fügen Sie sie der [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457)-Eigenschaft für die App hinzu.

Legen Sie abschließend Einträge für die Eingabe- und Ausgabedateinamen in der [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622)-Eigenschaft für Ihre App fest. Die Hintergrundaufgabe ruft die Dateinamen von diesem Speicherort ab.

[!code-cs[PickFilesToTranscode](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetPickFilesToTranscode)]

Um die Hintergrundaufgabe registrieren, erstellen Sie eine neue [**MediaProcessingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806005)-Klasse und eine neue [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)-Klasse. Legen Sie den Namen des Hintergrundaufgaben-Generators fest, damit Sie ihn später identifizieren können. Legen Sie die [**TaskEntryPoint**](https://msdn.microsoft.com/library/windows/apps/br224774)-Eigenschaft auf die gleiche Namespace- und Klassennamenszeichenfolge fest, die Sie in der Manifestdatei verwendet haben. Legen Sie die [**Trigger**](https://msdn.microsoft.com/library/windows/apps/dn641725)-Eigenschaft auf die **MediaProcessingTrigger** Instanz fest.

Stellen Sie vor dem Registrieren der Aufgabe sicher, dass Sie die Registrierung aller zuvor registrierten Aufgaben aufheben, indem Sie eine Schleife durch die [**AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787)-Sammlung durchführen. Rufen Sie zudem [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229870) für alle Aufgaben auf, die den in der [**BackgroundTaskBuilder.Name**](https://msdn.microsoft.com/library/windows/apps/br224771)-Eigenschaft angegebenen Namen aufweisen.

Registrieren Sie die Hintergrundaufgabe durch Aufrufen von [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772). Registrieren Sie Handler für das [**Completed**](https://msdn.microsoft.com/library/windows/apps/br224788)-Ereignis und das [**Progress**](https://msdn.microsoft.com/library/windows/apps/br224808)-Ereignis.

[!code-cs[RegisterBackgroundTask](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetRegisterBackgroundTask)]

Starten Sie die Hintergrundaufgabe durch Aufrufen der [**RequestAsync**](https://msdn.microsoft.com/library/windows/apps/dn765071)-Methode des **MediaProcessingTrigger**-Objekts. Das von dieser Methode zurückgegebene [**MediaProcessingTriggerResult**](https://msdn.microsoft.com/library/windows/apps/dn806007)-Objekt informiert Sie darüber, ob die Hintergrundaufgabe erfolgreich gestartet wurde. Zudem teilt es Ihnen bei einem Fehler mit, warum die Hintergrundaufgabe nicht gestartet wurde.

[!code-cs[LaunchBackgroundTask](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetLaunchBackgroundTask)]

Der **OnProgress**-Ereignishandler wird aufgerufen, wenn die Hintergrundaufgabe den Vorgangsstatus aktualisiert. Sie können diese Möglichkeit nutzen, um die Benutzeroberfläche mit Statusinformationen zu aktualisieren.

[!code-cs[OnProgress](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetOnProgress)]

Der **OnCompleted**-Ereignishandler wird aufgerufen, wenn die Ausführung der Hintergrundaufgabe abgeschlossen ist. Dies ist eine weitere Möglichkeit zum Aktualisieren der Benutzeroberfläche, um Statusinformationen für den Benutzer bereitzustellen.

[!code-cs[OnCompleted](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetOnCompleted)]


 

 







<!--HONumber=Jun16_HO4-->


