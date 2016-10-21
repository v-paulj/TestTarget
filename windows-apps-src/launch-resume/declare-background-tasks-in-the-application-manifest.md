---
author: TylerMSFT
title: Deklarieren von Hintergrundaufgaben im Anwendungsmanifest
description: "Sie können die Verwendung von Hintergrundaufgaben aktivieren, indem Sie diese im App-Manifest als Erweiterungen deklarieren."
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
translationtype: Human Translation
ms.sourcegitcommit: b877ec7a02082cbfeb7cdfd6c66490ec608d9a50
ms.openlocfilehash: 6ec298a956673c114d34d64b026394ece2c33506

---

# Deklarieren von Hintergrundaufgaben im Anwendungsmanifest


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**BackgroundTasks-Schema**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)

Sie können die Verwendung von Hintergrundaufgaben aktivieren, indem Sie diese im App-Manifest als Erweiterungen deklarieren.

> [!Important]
>  In diesem Artikel geht es insbesondere um Hintergrundaufgaben, die als separater Prozess ausgeführt werden. Einzelprozess-Hintergrundaufgaben werden nicht im Manifest deklariert.

In einem separaten Prozess ausgeführte Hintergrundaufgaben müssen im App-Manifest deklariert sein, da sie ansonsten nicht von der App registriert werden können (eine Ausnahme wird ausgelöst). Zudem müssen Hintergrundaufgaben im Anwendungsmanifest deklariert werden, um zertifiziert zu werden.

In diesem Thema wird davon ausgegangen, dass Sie eine oder mehrere Hintergrundaufgabenklassen erstellt haben und dass Ihre App die Hintergrundaufgabe so registriert, dass sie als Reaktion auf mindestens einen Auslöser ausgeführt wird.

## Manuelles Hinzufügen von Erweiterungen


Öffnen Sie das Anwendungsmanifest (Package.appxmanifest), und wechseln Sie zum „Application“-Element. Erstellen Sie ein "Extensions"-Element (sofern nicht bereits eines vorhanden ist).

Der folgende Ausschnitt stammt aus dem [Hintergrundaufgabenbeispiel](http://go.microsoft.com/fwlink/p/?LinkId=618666):

```xml
<Application Id="App"
   ...
   <Extensions>
     <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.SampleBackgroundTask">
       <BackgroundTasks>
         <Task Type="systemEvent" />
         <Task Type="timer" />
       </BackgroundTasks>
     </Extension>
     <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ServicingComplete">
       <BackgroundTasks>
         <Task Type="systemEvent"/>
       </BackgroundTasks>
     </Extension>
   </Extensions>
 </Application>
```

## Hinzufügen einer Erweiterung für eine Hintergrundaufgabe


Deklarieren Sie Ihre erste Hintergrundaufgabe.

Kopieren Sie diesen Code in das "Extensions"-Element (Attribute werden in den folgenden Schritten hinzugefügt).

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="">
      <BackgroundTasks>
        <Task Type="" />
      </BackgroundTasks>
    </Extension>
</Extensions>
```

1.  Ändern Sie das EntryPoint-Attribut so, dass diese Zeichenfolge von Ihrem Code bei der Registrierung Ihrer Hintergrundaufgabe als Einstiegspunkt verwendet wird (**namespace.classname**).

    In diesem Beispiel ist „ExampleBackgroundTaskNameSpace.ExampleBackgroundTaskClassName“ der Einstiegspunkt:

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ExampleBackgroundTaskClassName">
       <BackgroundTasks>
         <Task Type="" />
       </BackgroundTasks>
    </Extension>
</Extensions>
```

2.  Ändern Sie die Liste der Aufgabentypenattribute, um den für diese Hintergrundaufgabe verwendeten Typ der Aufgabenregistrierung anzugeben. Wenn die Hintergrundaufgabe mit mehreren Triggertypen registriert wird, fügen Sie für jeden Typ zusätzliche Task-Elemente und Type-Attribute hinzu.

    **Hinweis** Listen Sie alle verwendeten Triggertypen auf, da die Hintergrundaufgabe ansonsten die nicht deklarierten Triggertypen nicht registriert (bei der [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772)-Methode tritt ein Fehler auf, und eine Ausnahme wird ausgelöst).

    Dieses Beispiel für einen Codeausschnitt gibt die Verwendung von Systemereignistriggern und Pushbenachrichtigungen an:

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.BackgroundTaskClass">
    <BackgroundTasks>
        <Task Type="systemEvent" />
        <Task Type="pushNotification" />
    </BackgroundTasks>
</Extension>
```


## Hinzufügen von weiteren Hintergrundaufgabenerweiterungen

Wiederholen Sie Schritt 2 für alle weiteren, von Ihrer App registrierten Hintergrundaufgabenklassen.

Das folgende Beispiel zeigt das vollständige "Application"-Element aus dem [Hintergrundaufgabenbeispiel]( http://go.microsoft.com/fwlink/p/?linkid=227509): Es zeigt die Verwendung von zwei Hintergrundaufgabenklassen mit insgesamt drei Triggertypen. Kopieren Sie den Abschnitt „Extensions“ aus diesem Beispiel, und ändern Sie ihn nach Bedarf, um Hintergrundaufgaben im Anwendungsmanifest zu deklarieren.

```xml
<Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="BackgroundTask.App">
        <uap:VisualElements
          DisplayName="BackgroundTask"
          Square150x150Logo="Assets\StoreLogo-sdk.png"
          Square44x44Logo="Assets\SmallTile-sdk.png"
          Description="BackgroundTask"

          BackgroundColor="#00b2f0">
          <uap:LockScreen Notification="badgeAndTileText" BadgeLogo="Assets\smalltile-Windows-sdk.png" />
            <uap:SplashScreen Image="Assets\Splash-sdk.png" />
            <uap:DefaultTile DefaultSize="square150x150Logo" Wide310x150Logo="Assets\tile-sdk.png" >
                <uap:ShowNameOnTiles>
                    <uap:ShowOn Tile="square150x150Logo" />
                    <uap:ShowOn Tile="wide310x150Logo" />
                </uap:ShowNameOnTiles>
            </uap:DefaultTile>
        </uap:VisualElements>

      <Extensions>
        <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.SampleBackgroundTask">
          <BackgroundTasks>
            <Task Type="systemEvent" />
            <Task Type="timer" />
          </BackgroundTasks>
        </Extension>
        <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ServicingComplete">
          <BackgroundTasks>
            <Task Type="systemEvent"/>
          </BackgroundTasks>
        </Extension>
      </Extensions>
    </Application>
</Applications>
```

## Deklarieren der Hintergrundaufgabe für die Ausführung in einem separaten Prozess

Eine neue Funktion in Windows10 Version1507 ermöglicht das Ausführen von Hintergrundaufgaben in einem anderen Prozess als „BackgroundTaskHost.exe“ (der Prozess, in dem Hintergrundaufgaben standardmäßig ausgeführt werden).  Es gibt zwei Optionen: Ausführen im selben Prozess wie die Vordergrundanwendung; Ausführen in einer Instanz von „BackgroundTaskHost.exe“, die von den anderen Hintergrundaufgabeninstanzen derselben Anwendung getrennt ist.  

### Ausführen in der Vordergrundanwendung

Hier finden Sie ein XML-Beispiel, mit dem eine Hintergrundaufgabe deklariert wird, die im gleichen Prozess wie die Anwendung im Vordergrund ausgeführt wird. Beachten Sie das Attribut `Executable`:

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="ExecModelTestBackgroundTasks.ApplicationTriggerTask" Executable="$targetnametoken$.exe">
        <BackgroundTasks>
            <Task Type="systemEvent" />
        </BackgroundTasks>
    </Extension>
</Extensions>
```

> [!Note]
> Verwenden Sie das Executable-Element nur bei Hintergrundaufgaben, für die es unbedingt erforderlich ist, z.B. [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).  

### Ausführen in einem anderen Hintergrund-Hostprozess

Hier finden Sie ein XML-Beispiel, mit dem eine Hintergrundaufgabe deklariert wird, die zwar in einem „BackgroundTaskHost.exe“-Prozess ausgeführt wird, der jedoch von anderen Hintergrundaufgabeninstanzen derselben App getrennt ist. Beachten Sie das Attribut `ResourceGroup`, das festlegt, welche Hintergrundaufgaben gemeinsam ausgeführt werden.

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.SessionConnectedTriggerTask" ResourceGroup="foo">
      <BackgroundTasks>
        <Task Type="systemEvent" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimeZoneTriggerTask" ResourceGroup="foo">
      <BackgroundTasks>
        <Task Type="systemEvent" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimerTriggerTask" ResourceGroup="bar">
      <BackgroundTasks>
        <Task Type="timer" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.ApplicationTriggerTask" ResourceGroup="bar">
      <BackgroundTasks>
        <Task Type="general" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.MaintenanceTriggerTask" ResourceGroup="foobar">
      <BackgroundTasks>
        <Task Type="general" />
      </BackgroundTasks>
    </Extension>
</Extensions>
```


## Verwandte Themen


* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)



<!--HONumber=Aug16_HO3-->


