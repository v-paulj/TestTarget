---
author: TylerMSFT
title: Deklarieren von Hintergrundaufgaben im Anwendungsmanifest
description: "Sie können die Verwendung von Hintergrundaufgaben aktivieren, indem Sie diese im App-Manifest als Erweiterungen deklarieren."
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: d7dbdab0e8d404e6607585045d49bb3dd1407de6

---

# Deklarieren von Hintergrundaufgaben im Anwendungsmanifest


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**BackgroundTasks-Schema**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)

Sie können die Verwendung von Hintergrundaufgaben aktivieren, indem Sie diese im App-Manifest als Erweiterungen deklarieren.

Hintergrundaufgaben müssen im App-Manifest deklariert sein, da sie ansonsten nicht von der App registriert werden können (eine Ausnahme wird ausgelöst). Zudem müssen Hintergrundaufgaben im Anwendungsmanifest deklariert werden, um zertifiziert zu werden.

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

    **Hinweis:**  Listen Sie alle verwendeten Triggertypen auf, da die Hintergrundaufgabe ansonsten die nicht deklarierten Triggertypen nicht registriert (bei der [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772)-Methode tritt ein Fehler auf, und eine Ausnahme wird ausgelöst).

    Dieses Beispiel für einen Codeausschnitt gibt die Verwendung von Systemereignistriggern und Pushbenachrichtigungen an:

    ```xml
                <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.BackgroundTaskClass">
                  <BackgroundTasks>
                    <Task Type="systemEvent" />
                    <Task Type="pushNotification" />
                  </BackgroundTasks>
                </Extension>
    ```

    > **Hinweis**  Normalerweise wird eine App in einem bestimmten Prozess mit der Bezeichnung „BackgroundTaskHost.exe“ ausgeführt. Sie können dem Erweiterungselement ein Executable-Element hinzufügen, damit Hintergrundaufgaben im Kontext der App ausgeführt werden. Verwenden Sie das Executable-Element nur bei Hintergrundaufgaben, für die es unbedingt erforderlich ist, z. B. [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).    

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

## Verwandte Themen

* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)



<!--HONumber=Jun16_HO4-->


