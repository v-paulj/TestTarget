---
title: Systemressourcen für UWP-Apps und -Spiele auf der Xbox One
description: UWP auf Xbox-Systemressourcen
area: Xbox
---

# Systemressourcen für UWP-Apps und -Spiele auf der Xbox One

Systemressourcen für UWP-Apps und -Spiele, die auf der Xbox One ausgeführt werden, teilen sich die Ressourcen mit dem System und anderen Apps. 
Daher haben UWP-Apps und -Spiele Zugriff auf die folgenden Ressourcen:

* In dieser Vorschau beträgt der maximal verfügbare Arbeitsspeicher 448 MB.
    * In zukünftigen Versionen wird maximal 1 GB Arbeitsspeicher verfügbar sein.
    * Wenn Sie die App oder das Spiel über den Visual Studio-Debugger ausführen, gelten diese Arbeitsspeicherbeschränkungen nicht. Diese Beschränkung gilt nur, wenn die Ausführung nicht im Debugmodus erfolgt.

* Gemeinsame Nutzung von 2 bis 4 CPU-Kernen, je nach Anzahl der auf dem System ausgeführten Apps und Spiele.

* Gemeinsame Nutzung von 45 % der GPU, je nach Anzahl der im System ausgeführten Apps und Spiele.

* UWP auf Xbox One unterstützt die DirectX 11-Featureebene 10. DirectX 12 wird derzeit nicht unterstützt. 

Für die **Anwendungsentwicklung** müssen Sie berücksichtigen, dass die verfügbaren Ressourcen im Vergleich zu einem Standard-PC möglicherweise eingeschränkt sind.

Für die **Spieleentwicklung** müssen Sie berücksichtigen, dass Xbox One, wie andere Spielekonsolen, 
eine spezialisierte Hardwarekomponente ist, die ein spezifisches hardwarebasiertes Entwicklungskit erfordert, um ihr volles Potenzial abzurufen. 
Wenn Sie an der Entwicklung eines Spiels arbeiten, das auf das maximale Potenzial der Xbox One-Hardware zugreifen muss, 
können Sie sich beim [ID@Xbox](http://www.xbox.com/en-us/Developers/id)-Programm registrieren, um Zugang zu Xbox One-Entwicklungskits zu erhalten, die DirectX 12 unterstützen.


<!--HONumber=Mar16_HO5-->


