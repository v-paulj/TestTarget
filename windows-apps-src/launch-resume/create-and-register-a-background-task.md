---
author: TylerMSFT
title: "Erstellen und Registrieren einer in einem separaten Prozess ausgeführten Hintergrundaufgabe"
description: "Erstellen Sie eine Hintergrundaufgabenklasse, und registrieren Sie diese für die Ausführung, wenn sich die App nicht im Vordergrund befindet."
ms.assetid: 4F98F6A3-0D3D-4EFB-BA8E-30ED37AE098B
translationtype: Human Translation
ms.sourcegitcommit: 95c34f70e9610907897cfe9a2bf82aaac408e486
ms.openlocfilehash: 4eb67f8f63134ab33df79b0b98b252b2b27b2dda

---

# Erstellen und Registrieren einer in einem separaten Prozess ausgeführten Hintergrundaufgabe

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \].

**Wichtige APIs**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

Erstellen Sie eine Hintergrundaufgabenklasse, und registrieren Sie diese für die Ausführung, wenn sich die App nicht im Vordergrund befindet. In diesem Thema wird gezeigt, wie Sie eine Hintergrundaufgabe erstellen und registrieren, die in einem vom Vordergrundprozess separaten Prozess ausgeführt wird. Informationen zum Ausführen von Hintergrundarbeit direkt in der Vordergrundanwendung finden Sie unter [Erstellen und Registrieren einer Einzelprozess-Hintergrundaufgabe](create-and-register-a-singleprocess-background-task.md).

> [!Note]
> Wenn Sie eine Hintergrundaufgabe zur Medienwiedergabe im Hintergrund verwenden, finden Sie unter [Wiedergeben von Medien im Hintergrund](https://msdn.microsoft.com/en-us/windows/uwp/audio-video-camera/background-audio) Informationen zu Verbesserungen in Windows 10, Version 1607, die dies erleichtern.

## Erstellen einer Hintergrundaufgabenklasse

Sie können Code im Hintergrund ausführen, indem Sie Klassen schreiben, die die [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)-Schnittstelle implementieren. Dieser Code wird ausgeführt, wenn ein bestimmtes Ereignis beispielsweise durch [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) oder [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) ausgelöst wird.

Die folgenden Schritte zeigen, wie Sie eine neue Klasse zum Implementieren der [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)-Schnittstelle schreiben. Erstellen Sie zunächst ein neues Projekt in Ihrer Projektmappe für Hintergrundaufgaben. Fügen Sie zu Ihrer Hintergrundaufgabe eine neue leere Klasse hinzu, und importieren Sie den Namespace [Windows.ApplicationModel.Background](https://msdn.microsoft.com/library/windows/apps/br224847).

1.  Erstellen Sie ein neues Projekt für Hintergrundaufgaben, und fügen Sie es zu Ihrer Projektmappe hinzu. Klicken Sie zu diesem Zweck mit der rechten Maustaste auf Ihren Projektmappenknoten im **Projektmappen-Explorer**, und wählen Sie „Hinzufügen &gt; Neues Projekt“ aus. Wählen Sie dann den Projekttyp **Komponente für Windows-Runtime (Universal Windows)** aus, geben Sie dem Projekt einen Namen, und klicken Sie auf „OK“.
2.  Verweisen Sie auf das Hintergrundaufgabenprojekt aus dem Projekt für UWP-Apps (Universelle Windows-Plattform).

    Klicken Sie für eine C++-App mit der rechten Maustaste auf Ihr App-Projekt, und wählen Sie **Eigenschaften** aus. Gehen Sie dann zu **Allgemeine Eigenschaften**, und klicken Sie auf **Neuen Verweis hinzufügen**. Aktivieren Sie das Kontrollkästchen neben Ihrem Hintergrundaufgabenprojekt, und klicken Sie in beiden Dialogfeldern auf **OK**.

    Klicken Sie für eine C#-App in Ihrem App-Projekt mit der rechten Maustaste auf **Verweise**, und wählen Sie **Neuen Verweis hinzufügen** aus. Wählen Sie unter **Projektmappe** die Option **Projekte**. Wählen Sie dann den Namen Ihres Hintergrundaufgabenprojekts aus, und klicken Sie auf **OK**.

3.  Erstellen Sie eine neue Klasse zum Implementieren der Schnittstelle [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794). Die [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811)-Methode ist ein erforderlicher Einstiegspunkt und wird aufgerufen, wenn das angegebene Ereignis ausgelöst wird. Diese Methode ist in jeder Hintergrundaufgabe erforderlich.

    > [!NOTE]
    > Die Hintergrundaufgabenklasse selbst sowie alle anderen Klassen im Hintergrundaufgabenprojekt müssen **öffentliche** Klassen und **versiegelt** sein.

    Der folgende Beispielcode zeigt einen sehr einfachen Startpunkt für eine Hintergrundaufgabenklasse:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     //
    >     // ExampleBackgroundTask.cs
    >     //
    >
    >     using Windows.ApplicationModel.Background;
    >
    >     namespace Tasks
    >     {
    >         public sealed class ExampleBackgroundTask : IBackgroundTask
    >         {
    >             public void Run(IBackgroundTaskInstance taskInstance)
    >             {
    >                 
    >             }        
    >         }
    >     }
    > ```
    > ```cpp
    >     //
    >     // ExampleBackgroundTask.h
    >     //
    >
    >     #pragma once
    >
    >     using namespace Windows::ApplicationModel::Background;
    >
    >     namespace RuntimeComponent1
    >     {
    >         public ref class ExampleBackgroundTask sealed : public IBackgroundTask
    >         {
    >
    >         public:
    >             ExampleBackgroundTask();
    >
    >             virtual void Run(IBackgroundTaskInstance^ taskInstance);
    >             void OnCompleted(
    >                     BackgroundTaskRegistration^ task,
    >                     BackgroundTaskCompletedEventArgs^ args
    >                     );
    >         };
    >     }
    >
    >     //
    >     // ExampleBackgroundTask.cpp
    >     //
    >
    >     #include "ExampleBackgroundTask.h"
    >
    >     using namespace Tasks;
    >
    >     void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
    >     {
    >
    >     }
    >  ```

4.  Wenn asynchroner Code in der Hintergrundaufgabe ausgeführt wird, muss in der Hintergrundaufgabe eine Verzögerung verwendet werden. Wenn Sie keine Verzögerung verwenden, kann die Ausführung der Hintergrundaufgabe unerwartet beendet werden, wenn die Run-Methode vor dem Aufruf der asynchronen Methode abgeschlossen wurde.

    Implementieren Sie die Verzögerung in der Run-Methode vor Aufruf der asynchronen Methode. Speichern Sie die Verzögerung in einer globalen Variable, sodass die asynchrone Methode darauf zugreifen kann. Deklarieren Sie das Objekt so, dass die Verzögerung nach Abschluss des asynchronen Codes abgeschlossen wird.

    Der folgende Beispielcode ruft die Verzögerung auf, speichert sie und gibt sie nach Abschluss des asynchronen Codes frei:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     BackgroundTaskDeferral _deferral; // Note: defined at class scope so we can mark it complete inside the OnCancel() callback if we choose to support cancellation
    >     public async void Run(IBackgroundTaskInstance taskInstance)
    >     {
    >         _deferral = taskInstance.GetDeferral()
    >         //
    >         // TODO: Insert code to start one or more asynchronous methods using the
    >         //       await keyword, for example:
    >         //
    >         // await ExampleMethodAsync();
    >         //
    >         
    >         _deferral.Complete();
    >     }
    > ```
    > ```cpp
    >     BackgroundTaskDeferral^ deferral = taskInstance->GetDeferral(); // Note: defined at class scope so we can mark it complete inside the OnCancel() callback if we choose to support cancellation
    >     void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
    >     {
    >         //
    >         // TODO: Modify the following line of code to call a real async function.
    >         //       Note that the task<void> return type applies only to async
    >         //       actions. If you need to call an async operation instead, replace
    >         //       task<void> with the correct return type.
    >         //
    >         task<void> myTask(ExampleFunctionAsync());
    >         
    >         myTask.then([=] () {
    >             deferral->Complete();
    >         });
    >     }
    > ```

> [!NOTE]
> In C# werden asynchrone Methoden für Hintergrundaufgaben mithilfe der Schlagwörter **async/await** aufgerufen. In C++ kann mit einer Aufgabenfolge ein ähnliches Ergebnis erzielt werden.

Weitere Informationen zu asynchronen Mustern finden Sie unter [Asynchrone Programmierung](https://msdn.microsoft.com/library/windows/apps/mt187335). Weitere Beispiele zum Verhindern der vorzeitigen Beendigung einer Hintergrundaufgabe mithilfe von Verzögerungen finden Sie im [Beispiel zu Hintergrundaufgaben](http://go.microsoft.com/fwlink/p/?LinkId=618666).

Die folgenden Schritte werden in einer Ihrer App-Klassen durchgeführt (beispielsweise in "MainPage.xaml.cs").

> [!NOTE]
> Sie können auch eine Funktion erstellen, die sich nur um das Registrieren von Hintergrundaufgaben kümmert. Siehe dazu [Registrieren einer Hintergrundaufgabe](register-a-background-task.md). In einem solchen Fall können Sie die nächsten drei Schritte überspringen, einfach den Auslöser konstruieren und der Registrierungsfunktion zur Verfügung stellen, zusammen mit dem Aufgabennamen, dem entsprechenden Einstiegspunkt und, optional, einer Bedingung.

## Registrieren der auszuführenden Hintergrundaufgabe

1.  Finden Sie heraus, ob die Hintergrundaufgabe bereits registriert ist, indem Sie die [**BackgroundTaskRegistration.AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787)-Eigenschaft durchlaufen lassen. Dieser Schritt ist sehr wichtig. Sollte Ihre App nicht überprüfen, ob Hintergrundaufgaberegistrierungen vorhanden sind, könnte es passieren, dass sie die Aufgabe mehrere Male registriert, was zu Leistungseinbrüchen führt und die verfügbare CPU-Zeit der Aufgabe maximiert, bevor die Arbeit abgeschlossen werden kann.

    Das folgende Beispiel durchläuft die AllTasks-Eigenschaft und legt eine Kennzeichenvariable auf „true“ fest, wenn die Aufgabe bereits registriert ist:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     var taskRegistered = false;
    >     var exampleTaskName = "ExampleBackgroundTask";
    >
    >     foreach (var task in BackgroundTaskRegistration.AllTasks)
    >     {
    >         if (task.Value.Name == exampleTaskName)
    >         {
    >             taskRegistered = true;
    >             break;
    >         }
    >     }
    > ```
    > ```cpp
    >     boolean taskRegistered = false;
    >     Platform::String^ exampleTaskName = "ExampleBackgroundTask";
    >
    >     auto iter = BackgroundTaskRegistration::AllTasks->First();
    >     auto hascur = iter->HasCurrent;
    >
    >     while (hascur)
    >     {
    >         auto cur = iter->Current->Value;
    >
    >         if(cur->Name == exampleTaskName)
    >         {
    >             taskRegistered = true;
    >             break;
    >         }
    >
    >         hascur = iter->MoveNext();
    >     }
    > ```

2.  Wenn die Hintergrundaufgabe noch nicht registriert ist, verwenden Sie [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768), um eine Instanz Ihrer Hintergrundaufgabe zu generieren. Bei dem Einstiegspunkt der Aufgabe sollte es sich um den Namen der Hintergrundaufgabenklasse mit dem Namespace als Präfix handeln.

    Der Hintergrundaufgabenauslöser bestimmt, ob die Hintergrundaufgabe ausgeführt wird. Eine Liste mit möglichen Auslösern erhalten Sie unter [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839).

    Dieser Code generiert beispielsweise eine neue Hintergrundaufgabe, die beim Aktivieren des **TimeZoneChanged**-Auslösers ausgelöst wird:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     var builder = new BackgroundTaskBuilder();
    >
    >     builder.Name = exampleTaskName;
    >     builder.TaskEntryPoint = "RuntimeComponent1.ExampleBackgroundTask";
    >     builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));
    > ```
    > ```cpp
    >     auto builder = ref new BackgroundTaskBuilder();
    >
    >     builder->Name = exampleTaskName;
    >     builder->TaskEntryPoint = "RuntimeComponent1.ExampleBackgroundTask";
    >     builder->SetTrigger(ref new SystemTrigger(SystemTriggerType::TimeZoneChange, false));
    > ```

3.  Sie können eine Bedingung hinzufügen, die bestimmt, wann Ihre Aufgabe ausgeführt wird, nachdem das Auslöseereignis eintritt (optional). Wenn Sie zum Beispiel möchten, dass die Aufgabe erst ausgeführt wird, wenn der Benutzer anwesend ist, verwenden Sie die Bedingung **UserPresent**. Eine Liste mit möglichen Bedingungen finden Sie in [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

    Der folgende Beispielcode weist eine Bedingung zu, die die Anwesenheit des Benutzers voraussetzt:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
    > ```
    > ```cpp
    >     builder->AddCondition(ref new SystemCondition(SystemConditionType::UserPresent));
    > ```

4.  Registrieren Sie die Hintergrundaufgabe, indem Sie die Register-Methode für das [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)-Objekt aufrufen. Speichern Sie das [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)-Ergebnis, sodass es im nächsten Schritt verwendet werden kann.

    Der folgende Code registriert die Hintergrundaufgabe und speichert das Ergebnis:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     BackgroundTaskRegistration task = builder.Register();
    >     ```
    > ```cpp
    >     BackgroundTaskRegistration^ task = builder->Register();
    > ```

> [!NOTE]
> Universelle Windows-Apps müssen jedoch [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) aufrufen, bevor Hintergrundtriggertypen registriert werden.

Verwenden Sie den **ServicingComplete**-Trigger (siehe [SystemTriggerType](https://msdn.microsoft.com/library/windows/apps/br224839)) zum Ausführen von Konfigurationsänderungen nach dem Update wie Migrieren der Datenbank der App und Registrieren von Hintergrundaufgaben, um sicherzustellen, dass Ihre universelle Windows-App nach der Veröffentlichung eines Updates weiterhin ordnungsgemäß ausgeführt wird. Es empfiehlt sich, die Registrierung von Hintergrundaufgaben im Zusammenhang mit der vorherigen Version der App aufzuheben (siehe [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)) und die Hintergrundaufgaben für die neue Version der App zu registrieren (siehe [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)).

Weitere Informationen finden Sie unter [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md).

## Behandeln des Abschlusses der Hintergrundaufgabe mithilfe von Ereignishandlern

Sie sollten eine Methode mit dem [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)-Element registrieren, damit Ihre App Ergebnisse von der Hintergrundaufgabe abrufen kann. Beim Starten oder Fortsetzen der App wird die mark-Methode aufgerufen, wenn die Hintergrundaufgabe abgeschlossen wurde, seit sich die App zum letzten Mal im Vordergrund befand. (Die OnCompleted-Methode wird sofort aufgerufen, wenn die Hintergrundaufgabe abgeschlossen wird, während sich Ihre App im Vordergrund befindet.)

1.  Schreiben Sie eine OnCompleted-Methode, um den Abschluss von Hintergrundaufgaben zu behandeln. Das Ergebnis der Hintergrundaufgabe kann z.B. eine Aktualisierung der Benutzeroberfläche auslösen. Das hier gezeigte Methodenprofil ist für die OnCompleted-Ereignishandlermethode erforderlich, obwohl dieses Beispiel nicht den *args*-Parameter verwendet.

    Der folgende Beispielcode erkennt den Abschluss der Hintergrundaufgabe und ruft eine Beispielmethode zur Aktualisierung der Benutzeroberfläche auf, die eine Meldungszeichenfolge erfordert.

     > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
    >     {
    >         var settings = Windows.Storage.ApplicationData.Current.LocalSettings;
    >         var key = task.TaskId.ToString();
    >         var message = settings.Values[key].ToString();
    >         UpdateUI(message);
    >     }
    > ```
    > ```cpp
    >     void ExampleBackgroundTask::OnCompleted(BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >     {
    >         auto settings = ApplicationData::Current->LocalSettings->Values;
    >         auto key = task->TaskId.ToString();
    >         auto message = dynamic_cast<String^>(settings->Lookup(key));
    >         UpdateUI(message);
    >     }
    > ```

    > [!NOTE]
    > Aktualisierungen der Benutzeroberfläche sollten asynchron durchgeführt werden, um den Benutzeroberflächenthread nicht zu blockieren. Ein Beispiel dazu finden Sie in der UpdateUI-Methode im [Beispiel für eine Hintergrundaufgabe](http://go.microsoft.com/fwlink/p/?LinkId=618666).


2.  Gehen Sie dorthin zurück, wo Sie die Hintergrundaufgabe registriert haben. Fügen Sie nach dieser Codezeile ein neues [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)-Objekt hinzu. Stellen Sie Ihre OnCompleted-Methode als Parameter für den **BackgroundTaskCompletedEventHandler**-Konstruktor zur Verfügung.

    Der folgende Beispielcode fügt dem [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)-Element ein [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)-Element hinzu:

     > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
    > ```
    > ```cpp
    >     task->Completed += ref new BackgroundTaskCompletedEventHandler(this, &ExampleBackgroundTask::OnCompleted);
    > ```

## Deklarieren, dass die App Hintergrundaufgaben verwendet (im App-Manifest)

Bevor Ihre App Hintergrundaufgaben ausführen kann, müssen Sie alle Hintergrundaufgaben im App-Manifest deklarieren. Wenn Ihre App versucht, eine Hintergrundaufgabe mit einem Trigger zu registrieren, der nicht im Manifest aufgeführt ist, schlägt die Registrierung fehl.

1.  Öffnen Sie den Paketmanifest-Designer durch Öffnen der Datei namens „Package.appxmanifest“.
2.  Öffnen Sie die Registerkarte **Deklarationen**.
3.  Wählen Sie in der Dropdownliste **Verfügbare Deklarationen** die Option **Hintergrundaufgaben** aus, und klicken Sie auf **Hinzufügen**.
4.  Aktivieren Sie das Kontrollkästchen **Systemereignis**.
5.  Geben Sie in das Textfeld **Einstiegspunkt:** den Namespace und Namen der Hintergrundklasse ein, die in diesem Beispiel „RuntimeComponent1.ExampleBackgroundTask“ lautet.
6.  Schließen Sie den Manifest-Designer.

    Das folgende Erweiterungselement wird zur Datei „Package.appxmanifest“ hinzugefügt, um die Hintergrundaufgabe zu registrieren:

    ```xml
    <Extensions>
      <Extension Category="windows.backgroundTasks" EntryPoint="RuntimeComponent1.ExampleBackgroundTask">
        <BackgroundTasks>
          <Task Type="systemEvent" />
        </BackgroundTasks>
      </Extension>
    </Extensions>
    ```

## Zusammenfassung und nächste Schritte

Sie sollten jetzt über die Grundlagen verfügen, um eine Hintergrundaufgabenklasse zu schreiben, die Hintergrundaufgabe in Ihrer App zu registrieren und Ihre App so zu konfigurieren, dass Sie den Abschluss der Hintergrundaufgabe erkennt. Sie sollten auch mit der Aktualisierung des Anwendungsmanifests vertraut sein, damit Ihre App die Hintergrundaufgabe erfolgreich registrieren kann.

> [!NOTE]
> Laden Sie das [Beispiel für eine Hintergrundaufgabe](http://go.microsoft.com/fwlink/p/?LinkId=618666) herunter, um ähnliche Codebeispiele im Kontext einer vollständigen und stabilen UWP-App (Universelle Windows-Plattform) mit Hintergrundaufgaben zu erhalten.

Eine API-Referenz, konzeptionelle Richtlinien zu Hintergrundaufgaben und ausführlichere Anweisungen zum Schreiben von Apps, die Hintergrundaufgaben verwenden, finden Sie unter den folgenden verwandten Themen:

> [!NOTE]
> Dieser Artikel ist für Windows10-Entwickler bestimmt, die UWP-Apps (Universelle Windows-Plattform) schreiben. Wenn Sie für Windows8.x oder Windows Phone8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132) weiter.

## Verwandte Themen

**Ausführliche Themen mit Anweisungen zu Hintergrundaufgaben**

* [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
* [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)
* [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)
* [Überwachen von Status und Durchführung von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)
* [Ausführen einer Hintergrundaufgabe für einen Timer](run-a-background-task-on-a-timer-.md)
* [Erstellen und Registrieren einer Einzelprozess-Hintergrundaufgabe](create-and-register-a-singleprocess-background-task.md).
[Konvertieren einer Multiprozess-Hintergrundaufgabe in eine Einzelprozess-Hintergrundaufgabe](convert-multiple-process-background-task.md)  

**Ratschläge zu Hintergrundaufgaben**

* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [So wird's gemacht: Auslösen von Anhalte-, Fortsetzungs- und Hintergrundereignissen in Windows Store-Apps (beim Debuggen)](http://go.microsoft.com/fwlink/p/?linkid=254345)

**Hintergrundaufgabe – API-Referenz**

* [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)



<!--HONumber=Aug16_HO4-->


