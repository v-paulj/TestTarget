---
author: dbirtolo
title: "Zugreifen auf Sensoren und Geräte von einer Hintergrundaufgabe"
description: "Mit DeviceUseTrigger kann Ihre universelle Windows-App im Hintergrund auf Sensoren und Peripheriegeräte zugreifen. Dies ist selbst dann möglich, wenn die Vordergrund-App angehalten wird."
ms.assetid: B540200D-9FF2-49AF-A224-50877705156B
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 65471f26596f94fe550c92a10e01ca7f5cef64a1

---

# Zugreifen auf Sensoren und Geräte von einer Hintergrundaufgabe


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]



            [
              Mit **DeviceUseTrigger**
            ](https://msdn.microsoft.com/library/windows/apps/dn297337) kann Ihre universelle Windows-App im Hintergrund auf Sensoren und Peripheriegeräte zugreifen. Dies ist selbst dann möglich, wenn die Vordergrund-App angehalten wird. Je nachdem, wo Ihre App ausgeführt wird, kann sie eine Hintergrundaufgabe zum Synchronisieren von Daten mit Geräten oder zum Überwachen von Sensoren verwenden. Zur Verbesserung der Akkulaufzeit und Sicherstellung der Zustimmung durch den Benutzer unterliegt die Nutzung von [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) den Richtlinien, die in diesem Thema beschrieben werden.

Erstellen Sie zum Zugreifen auf Sensoren oder Peripheriegeräte im Hintergrund eine Hintergrundaufgabe, für die [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) verwendet wird. Ein Beispiel für die Vorgehensweise auf einem PC finden Sie unter [Beispiel für ein benutzerdefiniertes USB-Gerät](http://go.microsoft.com/fwlink/p/?LinkId=301975 ). Ein Beispiel für ein Smartphone finden Sie unter [Beispiel für Hintergrundsensoren](http://go.microsoft.com/fwlink/p/?LinkId=393307).

## Hintergrundaufgabe für Geräte – Übersicht


Wenn Ihre App für den Benutzer nicht mehr sichtbar ist, wird die App von Windows angehalten oder beendet, um Arbeitsspeicher und CPU-Ressourcen freizugeben. Dies ermöglicht anderen Apps die Ausführung im Vordergrund und reduziert den Stromverbrauch. In diesem Fall wären alle laufenden Datenereignisse ohne die Hilfe einer Hintergrundaufgabe verloren. Windows stellt den Trigger für die Hintergrundaufgabe bereit: [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337). Damit kann Ihre App lange Synchronisierungs- und Überwachungsvorgänge auf Geräten und Sensoren auch dann sicher im Hintergrund ausführen, wenn Ihre App angehalten wurde. Weitere Informationen zum App-Lebenszyklus finden Sie unter [Starten, Fortsetzen und Hintergrundaufgaben](index.md). Weitere Informationen zu Hintergrundaufgaben finden Sie unter [Unterstützen von Apps durch Hintergrundaufgaben](support-your-app-with-background-tasks.md).


            **Hinweis**  In einer universellen Windows-App erfordert das Synchronisieren eines Geräts im Hintergrund, dass der Benutzer das Synchronisieren im Hintergrund für Ihre App genehmigt hat. Das Gerät muss – mit E/A-Aktivierung – auch mit dem PC verbunden oder gekoppelt sein und kann maximal zehn Minuten lang Aktivitäten im Hintergrund ausführen. Weitere Details zur Richtlinienerzwingung sind weiter unten in diesem Thema beschrieben.

### Einschränkung: Kritische Gerätevorgänge

Einige kritische Gerätevorgänge (wie etwa zeitaufwändige Firmwareupdates) können mithilfe von [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) nicht durchgeführt werden. Diese Vorgänge können nur auf dem PC und nur von einer privilegierten App durchgeführt werden, für die [**DeviceServicingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297315) verwendet wird. Eine *privilegierte App* ist eine App, die vom Gerätehersteller dafür autorisiert wurde, diese Vorgänge auszuführen. Mithilfe von Metadaten wird angegeben, welche App, falls zutreffend, als privilegierte App für ein Gerät festgelegt wurde. Weitere Informationen finden Sie unter [Gerätesynchronisierung und -update für Windows Store-Geräte-Apps](http://go.microsoft.com/fwlink/p/?LinkId=306619).

## In einer DeviceUseTrigger-Hintergrundaufgabe unterstützte Protokolle/APIs


Hintergrundaufgaben mit [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) ermöglichen Ihrer App die Kommunikation über viele Protokolle/APIs. Die meisten davon werden von den vom System ausgelösten Hintergrundaufgaben nicht unterstützt. Folgende Protokolle werden von einer universellen Windows-App unterstützt.

| Protokoll         | DeviceUseTrigger in einer universellen Windows-App                                                                                                                                                    |
|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| USB              | ![Dieses Protokoll wird unterstützt.](images/ap-tools.png)                                                                                                                                            |
| HID              | ![Dieses Protokoll wird unterstützt.](images/ap-tools.png)                                                                                                                                            |
| Bluetooth RFCOMM | ![Dieses Protokoll wird unterstützt.](images/ap-tools.png)                                                                                                                                            |
| Bluetooth GATT   | ![Dieses Protokoll wird unterstützt.](images/ap-tools.png)                                                                                                                                            |
| MTP              | ![Dieses Protokoll wird unterstützt.](images/ap-tools.png)                                                                                                                                            |
| Netzwerk (drahtgebunden)    | ![Dieses Protokoll wird unterstützt.](images/ap-tools.png)                                                                                                                                            |
| Netzwerk (WLAN)    | ![Dieses Protokoll wird unterstützt.](images/ap-tools.png)                                                                                                                                            |
| IDeviceIOControl | ![DeviceServicingTrigger unterstützt IDeviceIOControl.](images/ap-tools.png)                                                                                                                       |
| Sensor-API      | ![](images/ap-tools.png)deviceservicingtrigger supports universal sensors apis[ (auf Sensoren der ](https://msdn.microsoft.com/library/windows/apps/dn894631)universellen Gerätefamilie beschränkt) |

 

## Registrieren von Hintergrundaufgaben im App-Paketmanifest


Ihre App führt Synchronisierungs- und Aktualisierungsvorgänge in Code durch, der als Teil einer Hintergrundaufgabe ausgeführt wird. Dieser Code ist in eine Windows-Runtime-Klasse eingebettet, mit der [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) implementiert wird (oder auf einer dedizierten JavaScript-Seite für JavaScript-Apps). Zum Verwenden einer [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)-Hintergrundaufgabe muss Ihre App diese in der App-Manifestdatei einer Vordergrund-App deklarieren, wie dies auch für vom System ausgelöste Hintergrundaufgaben erfolgt.

In diesem Beispiel für eine App-Paketmanifestdatei ist **DeviceLibrary.SyncContent** der Einstiegspunkt für eine Hintergrundaufgabe, die [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) verwendet.

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" EntryPoint="DeviceLibrary.SyncContent">
    <BackgroundTasks>
      <m2:Task Type="deviceUse" /> 
    </BackgroundTasks>
  </Extension>
</Extensions>
```

## Einführung in die Verwendung von DeviceUseTrigger


Führen Sie diese grundlegenden Schritte aus, um [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) zu verwenden. Weitere Informationen zu Hintergrundaufgaben finden Sie unter [Unterstützen Ihrer App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

1.  Ihre App registriert die Hintergrundaufgabe im App-Manifest und bettet den Code für die Hintergrundaufgabe in einer Windows-Runtime-Klasse ein, die IBackgroundTask implementiert, oder in eine dedizierte JavaScript-Seite für JavaScript-Apps.
2.  Beim Starten erstellt und konfiguriert Ihre App ein Triggerobjekt vom Typ [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) und speichert die Triggerinstanz für die künftige Verwendung.
3.  Die App prüft, ob die Hintergrundaufgabe bereits registriert wurde. Wenn nicht, führt sie dafür die Registrierung für den Trigger durch. Beachten Sie, dass es für die App nicht zulässig ist, Bedingungen für die Aufgabe festzulegen, die diesem Trigger zugeordnet ist.
4.  Wenn Ihre App die Hintergrundaufgabe auslösen muss, muss sie zuerst [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) aufrufen, um zu prüfen, ob die App eine Hintergrundaufgabe anfordern kann.
5.  Wenn die App die Hintergrundaufgabe anfordern kann, ruft sie die [**RequestAsync**](https://msdn.microsoft.com/library/windows/apps/dn297341)-Aktivierungsmethode für das Triggerobjekt des Geräts auf.
6.  Ihre Hintergrundaufgabe wird nicht wie andere Hintergrundaufgaben gedrosselt (es gilt kein CPU-Zeitkontingent), sondern wird mit reduzierter Priorität ausgeführt, um die Reaktionsfähigkeit von Vordergrund-Apps aufrechtzuerhalten.
7.  Windows überprüft dann anhand des Triggertyps, ob die erforderlichen Richtlinien erfüllt sind, einschließlich der Anforderung einer Zustimmung des Benutzers für den Vorgang, bevor die Hintergrundaufgabe gestartet wird.
8.  Windows überwacht die Systembedingungen und die Aufgabenlaufzeit und bricht die Aufgabe ggf. ab, falls die erforderlichen Bedingungen nicht mehr erfüllt sind.
9.  Wenn die Hintergrundaufgabe den aktuellen Status oder den Abschluss des Vorgangs meldet, empfängt die App diese Ereignisse über die Status- und Abschlussereignisse der registrierten Aufgabe.

**Wichtig**  
Beachten Sie bei der Verwendung von [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) diese wichtigen Punkte:

-   Die Möglichkeit zur programmgesteuerten Auslösung von Hintergrundaufgaben mithilfe von [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) wurde erstmals in Windows 8.1 und Windows Phone 8.1 eingeführt.

-   Bestimmte Richtlinien werden von Windows erzwungen, um sicherzustellen, dass die Zustimmung des Benutzers vorliegt, wenn Peripheriegeräte auf dem PC aktualisiert werden.

-   Weitere Richtlinien werden erzwungen, um für Benutzer die Akkulaufzeit zu verlängern, wenn Peripheriegeräte synchronisiert und aktualisiert werden.

-   Hintergrundaufgaben, für die [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) verwendet wird, können von Windows ggf. abgebrochen werden, wenn bestimmte Richtlinienanforderungen nicht mehr erfüllt sind, einschließlich einer Maximaldauer für die Hintergrundzeit (Gesamtbetrachtungszeit). Es ist wichtig, diese Richtlinienanforderungen zu berücksichtigen, wenn Sie diese Hintergrundaufgaben zum Interagieren mit Ihrem Peripheriegerät verwenden.

 


            **Tipp**  Laden Sie ein Beispiel herunter, um zu sehen, wie diese Hintergrundaufgaben funktionieren. Ein Beispiel für die Vorgehensweise auf einem PC finden Sie unter [Beispiel für ein benutzerdefiniertes USB-Gerät](http://go.microsoft.com/fwlink/p/?LinkId=301975 ). Ein Beispiel für ein Smartphone finden Sie unter [Beispiel für Hintergrundsensoren](http://go.microsoft.com/fwlink/p/?LinkId=393307).

 

## Einschränkungen in Bezug auf die Häufigkeit und den Vordergrund


Es besteht keine Einschränkung hinsichtlich der Häufigkeit, mit der Ihre App Vorgänge initiieren kann, aber sie kann nur jeweils einen Hintergrundaufgabenvorgang mit [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) ausführen (dies wirkt sich nicht auf andere Arten von Hintergrundaufgaben aus) und eine Hintergrundaufgabe nur initiieren, während sich Ihre App im Vordergrund befindet. Wenn sich Ihre App nicht im Vordergrund befindet, kann sie eine Hintergrundaufgabe nicht mit **DeviceUseTrigger** initiieren. Ihre App kann keine zweite **DeviceUseTrigger**-Hintergrundaufgabe initiieren, bevor die erste Hintergrundaufgabe abgeschlossen wurde.

## Einschränkungen in Bezug auf Geräte


Jede App ist zwar auf die Registrierung und Ausführung von nur einer [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)-Hintergrundaufgabe beschränkt, aber das Gerät (auf dem Ihre App ausgeführt wird) kann mehreren Apps das Registrieren und Ausführen von **DeviceUseTrigger**-Hintergrundaufgaben erlauben. Je nach Gerät kann ggf. eine Obergrenze für die Gesamtzahl an **DeviceUseTrigger**-Hintergrundaufgaben aller Apps gelten. Dies verringert den Stromverbrauch auf Geräten, auf denen die Ressourcen knapp sind. Weitere Informationen hierzu finden Sie in der folgenden Tabelle.

Über eine einzelne [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)-Hintergrundaufgabe kann Ihre App auf eine unbegrenzte Zahl von Peripheriegeräten oder Sensoren zugreifen, die nur von den unterstützten APIs und Protokollen beschränkt wird, die in der obigen Tabelle aufgeführt sind.

## Richtlinien für Hintergrundaufgaben


Windows erzwingt Richtlinien, wenn von Ihrer App eine [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)-Hintergrundaufgabe verwendet wird. Wenn diese Richtlinien nicht erfüllt sind, wird die Hintergrundaufgabe ggf. abgebrochen. Es ist wichtig, diese Richtlinienanforderungen zu berücksichtigen, wenn Sie diese Art von Hintergrundaufgaben zum Interagieren mit Geräten oder Sensoren verwenden.

### Richtlinien für die Aufgabeninitiierung

In dieser Tabelle ist aufgeführt, welche Richtlinien für die Aufgabeninitiierung für universelle Windows-Apps gelten.

| Richtlinie | DeviceUseTrigger in einer universellen Windows-App |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| Ihre App befindet sich beim Auslösen der Hintergrundaufgabe im Vordergrund. | ![Richtlinie gilt](images/ap-tools.png) |
| Das Gerät ist an das System angeschlossen (bzw. befindet sich bei einem Drahtlosgerät in Reichweite). | ![Richtlinie gilt](images/ap-tools.png) |
| Die App kann mithilfe der unterstützten Geräteperipherie-APIs (Windows-Runtime-APIs für USB, HID, Bluetooth, Sensoren usw.) auf das Gerät zugreifen. Falls Ihre App nicht auf das Gerät oder den Sensor zugreifen kann, wird der Zugriff auf die Hintergrundaufgabe verweigert. | ![Richtlinie gilt](images/ap-tools.png) |
| Der von der App bereitgestellte Einstiegspunkt für die Hintergrundaufgabe wird im App-Paketmanifest registriert. | ![Richtlinie gilt](images/ap-tools.png) |
| Es wird nur eine [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/dn297337)-Hintergrundaufgabe pro App ausgeführt. | ![Richtlinie gilt](images/ap-tools.png) |
| Die maximale Anzahl von [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/dn297337)-Hintergrundaufgaben wurde auf dem Gerät (auf dem die App ausgeführt wird) noch nicht erreicht. | Familie der Desktopgeräte: Eine unbegrenzte Anzahl von Aufgaben kann registriert und parallel ausgeführt werden. |
|  |  |
|  | Familie der Mobilgeräte: 1 Aufgabe auf einem 512 MB-Gerät. Andernfalls können 2 Aufgaben registriert und parallel ausgeführt werden. |
| Maximale Anzahl von Peripheriegeräten oder Sensoren, auf die Ihre App über eine einzelne [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/dn297337)-Hintergrundaufgabe zugreifen kann, wenn sie die unterstützten APIs/Protokolle verwendet. | Unbegrenzt |
| Die Hintergrundaufgabe verbraucht pro Minute 400ms an CPU-Zeit (bei einer CPU mit 1GHz), wenn der Bildschirm gesperrt ist. Bei nicht gesperrtem Bildschirm wird diese Zeit innerhalb von 5Minuten verbraucht. Wenn diese Richtlinie nicht erfüllt wird, kann dies zum Abbruch der Aufgabe führen. | ![Richtlinie gilt](images/ap-tools.png) |
 

### Richtlinienprüfung zur Laufzeit

Windows erzwingt die folgenden Richtlinienanforderungen zur Laufzeit, während Ihre Aufgabe im Hintergrund ausgeführt wird. Wenn eine der Laufzeitanforderungen nicht mehr erfüllt (true) ist, bricht Windows die Hintergrundaufgabe für das Gerät ab.

In dieser Tabelle ist aufgeführt, welche Laufzeitrichtlinien für universelle Windows-Apps gelten.

| Richtlinienprüfung | DeviceUseTrigger in einer universellen Windows-App |
|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| Das Gerät ist an das System angeschlossen (bzw. befindet sich bei einem Drahtlosgerät in Reichweite). | ![Richtlinienprüfung gilt](images/ap-tools.png) |
| Aufgabe führt regelmäßig E/A-Vorgänge für das Gerät durch (1 E/A alle 5 Sekunden). | ![Richtlinienprüfung gilt](images/ap-tools.png) |
| Die App hat die Aufgabe nicht abgebrochen. | ![Richtlinienprüfung gilt](images/ap-tools.png) |
| Gesamtbetrachtungszeit-Limit – Gesamtzeit, die die Aufgabe der App im Hintergrund ausgeführt werden kann. | Familie der Desktopgeräte: 10 Minuten. |
|  |  |
|  | Familie der Mobilgeräte: Kein Zeitlimit. Um Ressourcen zu schonen, können maximal eine oder zwei Aufgaben gleichzeitig ausgeführt werden. |
| Die App wurde nicht beendet. | ![Richtlinienprüfung gilt](images/ap-tools.png) |

 

## Bewährte Methoden


Unten sind bewährte Methoden für Apps angegeben, von denen die [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)-Hintergrundaufgaben verwendet werden.

### Programmieren einer Hintergrundaufgabe

Mit dem Verwenden der [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)-Hintergrundaufgabe über Ihre App wird sichergestellt, dass alle von der Vordergrund-App gestarteten Synchronisierungs- oder Überwachungsvorgänge im Hintergrund weiterhin ausgeführt werden, wenn Benutzer Apps wechseln und Ihre Vordergrund-App von Windows angehalten wird. Wir empfehlen Ihnen, dieses allgemeine Modell zum Registrieren, Auslösen und Aufheben der Registrierung von Hintergrundaufgaben zu verwenden:

1.  Rufen Sie [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) auf, um zu überprüfen, ob die App eine Hintergrundaufgabe anfordern kann. Dies muss erfolgen, bevor eine Hintergrundaufgabe registriert wird.

2.  Registrieren Sie die Hintergrundaufgabe, bevor Sie den Trigger anfordern.

3.  Verbinden Sie Ereignishandler für den aktuellen Status und den Abschluss mit dem Trigger. Wenn Ihre App aus dem angehaltenen Zustand zurückkehrt, stellt Windows für die App alle in die Warteschlange eingereihten Ereignisse zum Status und Abschluss bereit, mit denen der Status der Hintergrundaufgaben ermittelt werden kann.

4.  Schließen Sie alle geöffneten Geräte- oder Sensorobjekte, wenn Sie Ihre [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)-Hintergrundaufgabe auslösen, damit diese Geräte bzw. Sensoren geöffnet und von der Hintergrundaufgabe verwendet werden können.

5.  Registrieren Sie den Trigger.

6.  Berücksichtigen Sie die Auswirkung auf den Stromverbrauch, die mit dem Zugreifen auf ein Gerät oder einen Sensor über eine Hintergrundaufgabe verbunden ist. Wenn Sie das Berichtsintervall eines Sensors zu häufig ausführen, wird die Aufgabe unter Umständen so oft ausgeführt, dass der Akku eines Smartphones schnell leer ist.

7.  Heben Sie die Registrierung der Hintergrundaufgabe auf, wenn die Hintergrundaufgabe abgeschlossen ist.

8.  Führen Sie die Registrierung für Abbruchereignisse über die Klasse der Hintergrundaufgabe durch. Die Registrierung für Abbruchereignisse ermöglicht dem Code der Hintergrundaufgabe das saubere Beenden der ausgeführten Hintergrundaufgabe, wenn sie von Windows oder Ihrer Vordergrund-App abgebrochen wird.

9.  Heben Sie die Registrierung beim Beenden der App auf (nicht beim Anhalten), und brechen Sie alle laufenden Aufgaben ab, wenn diese für die App nicht mehr benötigt werden. Auf Systemen mit knappen Ressourcen, z. B. Smartphones mit wenig Arbeitsspeicher, ermöglicht diese Maßnahme anderen Apps die Verwendung einer [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)-Hintergrundaufgabe.

    -   Heben Sie die Registrierung auf, und brechen Sie alle laufenden Aufgaben ab, wenn Ihre App beendet wird.

    -   Beim Beenden der App werden die Hintergrundaufgaben abgebrochen, und alle vorhandenen Ereignishandler werden von den vorhandenen Hintergrundaufgaben getrennt. Sie können den Zustand Ihrer Hintergrundaufgaben dann nicht mehr ermitteln. Wenn Sie die Registrierung der Hintergrundaufgabe aufheben und sie abbrechen, können die Hintergrundaufgaben mit Ihrem Abbruchcode sauber beendet werden.

### Abbrechen einer Hintergrundaufgabe

Zum Abbrechen einer Aufgabe, die im Hintergrund Ihrer Vordergrund-App ausgeführt wird, verwenden Sie die Unregister-Methode des [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)-Objekts, das Sie in der App zum Registrieren der [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337)-Hintergrundaufgabe verwenden. Wenn Sie die Registrierung der Hintergrundaufgabe mithilfe der [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229869)-Methode unter **BackgroundTaskRegistration** aufheben, bricht die Infrastruktur der Hintergrundaufgaben Ihre Hintergrundaufgabe ab.

Für die [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229869)-Methode wird zusätzlich der boolesche Wert "true" oder „false“ verwendet, um anzugeben, ob derzeit ausgeführte Instanzen der Hintergrundaufgabe abgebrochen werden sollen, ohne deren Beendigung abzuwarten. Weitere Informationen finden Sie im API-Referenzthema für **Unregister**.

Zusätzlich zu [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229869) muss Ihre App auch [**BackgroundTaskDeferral.Complete**](https://msdn.microsoft.com/library/windows/apps/hh700504) aufrufen. So wird dem System mitgeteilt, dass der asynchrone Vorgang, der einer Hintergrundaufgabe zugeordnet ist, abgeschlossen ist.

 

 






<!--HONumber=Jun16_HO5-->


