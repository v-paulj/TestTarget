---
author: TylerMSFT
title: "Starten einer App auf einem Remotegerät"
description: "Erfahren Sie, wie Sie mithilfe von Projekt „Rome“ eine App auf einem Remotegerät starten können."
translationtype: Human Translation
ms.sourcegitcommit: ff8e16d0e376d502157ae42b9cdae11875008554
ms.openlocfilehash: d8c3783d68a1b3b216058790d84255a7fb4b612c

---

# Starten einer App auf einem Remotegerät

In diesem Artikel wird erläutert, wie eine App für die universelle Windows-Plattform (UWP) oder eine Windows-Desktop-App auf einem Remotegerät gestartet wird.

Ab Windows 10, Version 1607, kann eine UWP-App eine UWP-App oder eine Windows-Desktopanwendung auf einem Remotegerät starten, das ebenfalls unter Windows 10, Version 1607 oder höher, ausgeführt wird.

Ein Einsatzszenario für das Starten einer App auf einem Remotegerät ist, dass Benutzer einen Task auf einem Gerät starten und auf einem anderen Gerät beenden können. Wenn Sie beispielsweise auf Ihrem Heimweg im Auto Musik über Ihr Smartphone hören, können Sie die Remoteausführung verwenden, um die Wiedergabe an Ihre Xbox zu übergeben, wenn Sie zuhause angekommen sind. Sie können Daten an die Remote-App übergeben, um einen Kontext für die Remote-App bereitzustellen, damit diese den Task fortsetzen kann.

## Hinzufügen der Funktionen „RemoteSystem“

Damit Ihre App eine App auf einem Remotegerät starten kann, müssen Sie Ihrem App-Paketmanifest die Funktion <uap3:Capability Name="remoteSystem"/> hinzufügen. Sie können den Paketmanifest-Designer verwenden, um die Funktion hinzuzufügen, indem Sie auf der Registerkarte **Funktionen** die Option **Remotesystem** auswählen, oder Sie können den Code, den der Paketmanifest-Designer über diese Option in die Datei „Paket.appxmanifest“ einfügt, manuell selbst einfügen.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
 </Capabilities>
```
## Suchen eines Remotegeräts

Sie müssen zunächst das Gerät suchen, zu dem Sie eine Verbindung herstellen möchten. Eine detaillierte Anleitung dazu finden Sie unter [Ermitteln von Remotegeräten](discover-remote-devices.md). Wir verwenden hier eine einfache Methode, bei der die Funktionen zum Filtern nach Gerät oder nach Verbindungsart nicht verwendet werden. Wir erstellen ein Überwachungselement, das nach Remotesystemen sucht, und erstellen Ereignishandler, die benachrichtigt werden, wenn Geräte mit demselben Microsoft-Konto erkannt oder entfernt werden. Auf diese Weise erhalten wir eine Sammlung von Remotegeräten.

Der Code in diesen Beispielen setzt voraus, dass Sie über eine `using Windows.System.RemoteSystems`-Anweisung in Ihrer Datei verfügen.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetBuildDeviceList)]

Als Erstes müssen Sie vor einem Remotestart die Funktion `RemoteSystem.RequestAccessAsync()` aufrufen. Überprüfen Sie den Rückgabewert, um sicherzustellen, dass Ihre App auf Remotegeräte zugreifen darf. Diese Überprüfung ist beispielsweise nicht erfolgreich, wenn Sie Ihrer App nicht die Funktion `remoteSystem` hinzugefügt haben.

Die Ereignishandler der Systemüberwachung werden aufgerufen, wenn ein Gerät, mit dem wir eine Verbindung herstellen können, erkannt wird oder nicht mehr verfügbar ist. Wir verwenden diese Ereignishandler, um eine fortlaufend aktualisierte Liste der Geräte zu führen, mit denen wir eine Verbindung herstellen können.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetEventHandlers)]

Wir verfolgen die Geräte unter Verwendung der Remotesystem-IDs nach, die in einem Wörterbuch hinterlegt werden. Die Liste der aufzählbaren Geräte wird in einer ObservableCollection geschrieben. Die Verwendung einer ObservableCollection erleichtert auch, die Liste der Geräte an die Benutzeroberfläche zu binden. Dies wird in diesem Beispiel jedoch nicht getan.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetMembers)]

Fügen Sie Ihrem App-Startcode einen Aufruf an `BuildDeviceList()` hinzu, bevor Sie versuchen, eine Remote-App zu starten.

## Starten einer App auf einem Remotegerät

Sie starten eine App remote, indem Sie das Gerät, mit dem Sie sich verbinden möchten, an das [RemoteLauncher.LaunchUri](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.remotelauncher.launchuriasync.aspx)-API übergeben. Für diese Funktion gibt es drei Überladungen. Die einfachste, die in diesem Beispiel veranschaulicht wird, gibt den URI an, der die App auf dem Remotegerät aktiviert. In diesem Beispiel öffnet der URI die Karten-App auf dem Remotecomputer mit einer 3D-Ansicht der Space Needle.

Andere **RemoteLauncher.LaunchUriAsync**-Überladungen ermöglichen die Angabe von Optionen, z. B. den URI der Website, die angezeigt werden soll, wenn die App, die für den URI zuständig ist, auf dem Remotegerät nicht gestartet werden kann, sowie die Angabe einer optionalen Liste der Paketfamiliennamen, die zum Starten des URIs auf dem Remotegerät verwendet werden können. Sie können auch Daten in Form von Schlüssel-Wert-Paaren bereitstellen. Sie können beispielsweise Daten an die App übergeben, die Sie auf dem Remotegerät aktivieren, um einen Kontext für die Remote-App bereitzustellen, etwa den Namen des wiederzugebenden Titels oder die aktuelle Position der Wiedergabe bei der Übergabe von einem Gerät an ein anderes.

In der Praxis würden Sie in einem solchen Anwendungsszenario eine Benutzeroberfläche bereitstellen, um das zu verwendende Gerät zu aktivieren. In diesem Beispiel verwenden wir zur Vereinfachung für den Remoteaufruf nur das erste Gerät in der Liste.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetRemoteUriLaunch)]

Der [RemoteLaunchUriStatus](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.remotelaunchuristatus.aspx)-Wert, der von **RemoteLauncher.LaunchUriAsync()** zurückgegeben wird, enthält Informationen dazu, ob der Remotestart erfolgreich war. Falls bei dem Remotestart ein Fehler aufgetreten ist, wird ein Grund angegeben.

## Verwandte Themen

[API-Referenz für Remotesysteme](https://msdn.microsoft.com/en-us/library/windows/apps/Windows.System.RemoteSystems)  
[Übersicht über verbundene Apps und Geräte (Projekt "Rome")](connected-apps-and-devices.md)  
[Beispiel für Remotesysteme](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) veranschaulicht die Vorgehensweise zum Erkennen eines Remotesystems, zum Starten einer App auf einem Remotesystem und zum Verwenden von App-Diensten zum Senden von Nachrichten zwischen Apps, die auf zwei Systemen ausgeführt werden.



<!--HONumber=Aug16_HO5-->


