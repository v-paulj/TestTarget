---
author: TylerMSFT
title: "Verbundene Apps und Geräte (Project &quot;Rome&quot;)"
description: "Dieser Abschnitt beschreibt, wie Sie mithilfe von Project &quot;Rome&quot; verbundene Geräte ermitteln, eine App auf einem anderen Gerät starten und mit einer App auf einem Remotegerät kommunizieren."
translationtype: Human Translation
ms.sourcegitcommit: ff8e16d0e376d502157ae42b9cdae11875008554
ms.openlocfilehash: 4f49acfd7efcb10d99f9d23884d20c0fc51e5a4a

---

# Verbundene Apps und Geräte (Project "Rome")

In diesem Abschnitt wird erläutert, wie Apps mithilfe von Project "Rome" geräte- und plattformübergreifend verbunden werden. Erfahren Sie, wie Sie verbundene Geräte erkennen, eine App auf einem anderen Gerät starten und mit einer App auf einem Remotegerät kommunizieren.

Die meisten Benutzer verfügen über mehrere Geräte, wobei sie häufig eine Aktivität auf einem Gerät beginnen und sie auf einem anderen Gerät abschließen. Dazu müssen Apps geräte- und plattformübergreifend sein.

Die mit Windows 10, Version 1607, eingeführten [Remotesysteme-APIs](https://msdn.microsoft.com/en-us/library/windows/apps/Windows.System.RemoteSystems) ermöglichen es Ihnen, Apps zu schreiben, mit denen Benutzer eine Aufgabe auf einem Gerät starten und auf einem anderen Gerät abschließen können. Die Aufgabe bleibt der zentrale Fokus, und Benutzer können an dem für sie komfortabelsten Gerät arbeiten. Zum Beispiel hören Sie im Auto vielleicht Radio auf Ihrem Mobiltelefon. Aber zu Hause angekommen möchten Sie möglicherweise die Wiedergabe auf Ihre Xbox One übertragen, die in Ihre Heim-Stereoanlage eingebunden ist.

Sie können Project "Rome" auch für Begleitgeräte oder Remotesteuerungsszenarien verwenden. Verwenden Sie die App-Messaging-APIs, um einen App-Kanal zwischen zwei Geräten zum Senden und Empfangen von benutzerdefinierten Nachrichten zu erstellen. Beispielsweise können Sie eine App für Ihr Mobiltelefon schreiben, die die Wiedergabe auf Ihrem Fernsehgerät steuert, oder eine Begleit-App, die Informationen über die Charaktere in einer Fernsehsendung bereitstellt, die Sie sich auf einer anderen App ansehen.  

Geräte können über Bluetooth und Wireless LAN proximal verbunden werden, oder remote über die Cloud, und sind über das Microsoft-Konto der Person verbunden, die sie verwendet.

Unter [Beispiel für Remotesysteme](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) finden Sie Beispiele für die Vorgehensweise zum Erkennen eines Remotesystems, Starten einer App auf einem Remotesystem und Verwenden von App-Diensten zum Senden von Nachrichten zwischen Apps, die auf zwei Systemen ausgeführt werden.

| Remote-Aktivität | Beschreibung                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Ermitteln von Remotegeräten](discover-remote-devices.md)  | Erfahren Sie, wie Sie Geräte erkennen, zu denen Sie eine Verbindung herstellen können. |
| [Starten einer App auf einem Remotegerät](launch-a-remote-app.md) | Erfahren Sie, wie Sie eine App auf einem Remotegerät starten.  |
| [Kommunikation mit einem App-Remotedienst](communicate-with-a-remote-app-service.md) | Erfahren Sie, wie Sie mit einer App auf einem Remotegerät interagieren. |



<!--HONumber=Aug16_HO5-->


