---
author: Mtoepke
title: "Systemressourcen für UWP-Apps und -Spiele auf der Xbox One"
description: UWP auf Xbox-Systemressourcen
area: Xbox
ms.sourcegitcommit: 6a34b0f657fc787eaa3be691b69a591cfdb2a669
ms.openlocfilehash: 79c47bbcf33b1493a8a961b800932ce6be021453

---

# Systemressourcen für UWP-Apps und -Spiele auf der Xbox One

Systemressourcen für UWP-Apps und -Spiele, die auf der Xbox One ausgeführt werden, teilen sich die Ressourcen mit dem System und anderen Apps. Daher haben UWP-Apps und -Spiele Zugriff auf die folgenden Ressourcen:

* In dieser Vorschau steht einer im Vordergrund ausgeführten App maximal 1 GB an Arbeitsspeicher zur Verfügung.
    * Der maximal verfügbare Arbeitsspeicher für eine im Hintergrund ausgeführte App beträgt 128 MB.
    * Bei Apps, die diese Arbeitsspeicheranforderungen überschreiten, treten Speicherzuweisungsfehler auf. Weitere Informationen zum Überwachen der Arbeitsspeicherverwendung finden Sie in der Referenz der [MemoryManager-Klasse](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.memorymanager.aspx).
    * **Hinweis**
            &nbsp;&nbsp;Wenn Sie die Anwendung oder das Spiel über den Visual Studio-Debugger ausführen, gelten diese Arbeitsspeicherbeschränkungen nicht. Diese Beschränkung gilt nur, wenn die Ausführung nicht im Debugmodus erfolgt.

* Gemeinsame Nutzung von 2 bis 4 CPU-Kernen, je nach Anzahl der auf dem System ausgeführten Apps und Spiele.

* Gemeinsame Nutzung von 45 % der GPU, je nach Anzahl der im System ausgeführten Apps und Spiele.

* UWP auf Xbox One unterstützt die DirectX 11-Featureebene 10. DirectX 12 wird derzeit nicht unterstützt. 

Für die **Anwendungsentwicklung** müssen Sie berücksichtigen, dass die verfügbaren Ressourcen im Vergleich zu einem Standard-PC möglicherweise eingeschränkt sind.

Bei der **Spieleentwicklung** müssen Sie bedenken, dass die Xbox One (wie andere Spielekonsolen auch) eine spezielle Hardware ist, für die ein spezielles hardwarebasiertes Entwicklungskit erforderlich ist, um auf alle Funktionen zugreifen zu können. Wenn Sie für ein Spiel das maximale Potenzial der Xbox One-Hardware benötigen, können Sie sich beim [ID@Xbox](http://www.xbox.com/en-us/Developers/id)-Programm registrieren, um Zugriff auf das SDK (und damit DirectX 12-Unterstützung) zu erhalten.



<!--HONumber=Jun16_HO4-->


