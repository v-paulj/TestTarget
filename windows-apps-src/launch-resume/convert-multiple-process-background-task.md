---
author: TylerMSFT
title: Konvertieren einer Multiprozess-Hintergrundaufgabe in eine Einzelprozess-Hintergrundaufgabe
description: "Konvertieren Sie eine Hintergrundaufgabe, die in einem separaten Prozess ausgeführt wird, in eine Hintergrundaufgabe, die im Vordergrund-App-Prozess ausgeführt wird."
translationtype: Human Translation
ms.sourcegitcommit: 2c34ca40d3c930254500477ab5a2e41e5206d823
ms.openlocfilehash: e342667347cf3b89a5aa193495cbf7195263b276

---

# Konvertieren einer Multiprozess-Hintergrundaufgabe in eine Einzelprozess-Hintergrundaufgabe

Die einfachste Möglichkeit, Ihre Multiprozess-Hintergrundaktivitäten in einen Einzelprozess zu konvertieren, besteht darin, Ihren [IBackgroundTask.Run](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396)-Methodencode in Ihre Anwendung zu verschieben und über [OnBackgroundActivated](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) zu initiieren.

Wenn Ihre App mehrere Hintergrundaufgaben aufweist, wird in [Hintergrundaktivierungsbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) beschrieben, wie Sie mithilfe von `BackgroundActivatedEventArgs.TaskInstance.Task.Name` ermitteln können, welche Aufgabe initiiert wird.

Wenn Sie derzeit zwischen Vordergrund- und Hintergrundprozessen kommunizieren, können Sie diesen Zustandverwaltungs- und Kommunikationscode entfernen.

## Hintergrundaufgaben und Triggertypen, die nicht konvertiert werden können

* Einzelprozess-Hintergrundaufgaben unterstützen nicht die Aktivierung einer VoIP-Hintergrundaufgabe.
* Einzelprozess-Hintergrundaufgaben unterstützen nicht die folgenden Trigger:  [DeviceUseTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) und **IoTStartupTask**



<!--HONumber=Aug16_HO3-->


