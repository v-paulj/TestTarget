---
author: DBirtolo
ms.assetid: 4311D293-94F0-4BBD-A22D-F007382B4DB8
title: "Auflisten von Geräten"
description: "Der Enumeration-Namespace ermöglicht die Suche nach Geräten, die intern mit dem System verbunden, extern verbunden oder über Drahtlos- oder Netzwerkprotokolle erkannt werden können."
translationtype: Human Translation
ms.sourcegitcommit: 23a600fdcf972fcb291653e8aac447e035c12c6d
ms.openlocfilehash: 2aa1a86a2cb0b413fae5fbcd87599a9f1a822324

---
# Auflisten von Geräten

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \].

## Beispiele

Die einfachste Möglichkeit zum Auflisten aller verfügbaren Geräte ist, mithilfe des [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.findallasync.aspx)-Befehls eine Momentaufnahme zu erstellen (hierauf wird in einem späteren Abschnitt eingegangen).

```CSharp
async void enumerateSnapshot(){
  DeviceInformationCollection collection = await DeviceInformation.FindAllAsync();
}
```

Wenn Sie ein Beispiel zur fortgeschrittenen Verwendung der [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459)-APIs herunterladen möchten, klicken Sie [hier](http://go.microsoft.com/fwlink/?LinkID=620536).

## Enumerations-APIs

Der Enumeration-Namespace ermöglicht die Suche nach Geräten, die intern mit dem System verbunden, extern verbunden oder über Drahtlos- oder Netzwerkprotokolle erkannt werden können. Die APIs, die Sie für die Enumeration möglicher Geräte verwenden, gehören zum [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459)-Namespace. Im Folgenden finden Sie einige Gründe für die Verwendung dieser APIs:

-   Die Suche nach einem Gerät, das mit Ihrer Anwendung verbunden werden kann.
-   Abrufen von Informationen zu Geräten, die mit dem System verbunden sind oder von diesem erkannt werden können.
-   Verfügbarkeit einer App, die Benachrichtigungen empfängt, wenn Geräte hinzugefügt, verbunden oder getrennt werden bzw. ihren Onlinestatus oder andere Eigenschaften ändern.
-   Verfügbarkeit einer App, die Hintergrundtrigger empfängt, wenn Geräte verbunden oder getrennt werden bzw. ihren Onlinestatus oder andere Eigenschaften ändern.

Diese APIs können Geräte über jedes der folgenden Protokolle und jeden der folgenden Busse auflisten, sofern das jeweilige Gerät und das System, unter dem die App ausgeführt wird, diese Technologie unterstützen. Dies ist keine vollständige Liste, und von bestimmten Geräten können auch andere Protokolle unterstützt werden.

-   Physisch verbundene Busse. Dies umfasst PCI und USB. Beispielsweise alle Elemente, die im **Geräte-Manager** angezeigt werden.
-   [UPnP](https://msdn.microsoft.com/library/windows/desktop/Aa382303)
-   Digital Living Network Alliance (DLNA)
-   [**Discovery and Launch (DIAL)**](https://msdn.microsoft.com/library/windows/apps/Dn946818)
-   [**DNS Service Discovery (DNS-SD)**](https://msdn.microsoft.com/library/windows/apps/Dn895183)
-   [Web Services on Devices (WSD)](https://msdn.microsoft.com/library/windows/desktop/Aa826001)
-   [Bluetooth](https://msdn.microsoft.com/library/windows/desktop/Aa362932)
-   [**Wi-Fi Direct**](https://msdn.microsoft.com/library/windows/apps/Dn297687)
-   WiGig
-   [**Point of Service**](https://msdn.microsoft.com/library/windows/apps/Dn298071)

In vielen Fällen müssen Sie sich über die Verwendung der Enumerations-APIs keine Gedanken machen. Der Grund hierfür ist, dass viele APIs, die Geräte verwenden, automatisch das entsprechende Standardgerät auswählen oder eine optimierte Enumerations-API bereitstellen. Beispielsweise verwendet [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/BR242926) automatisch das standardmäßige Audiowiedergabegerät. Solange die App das Standardgerät verwenden kann, ist es nicht erforderlich, die Enumerations-APIs in Ihrer Anwendung zu verwenden. Die Enumerations-APIs bieten eine allgemeine und flexible Möglichkeit zum Ermitteln verfügbarer Geräte und zur Verbindung mit den Geräten. Dieses Thema enthält Informationen zur Enumeration von Geräten und beschreibt die vier gängigen Methoden hierfür.

-   Verwenden der [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841)-Benutzeroberfläche
-   Enumerieren von Momentaufnahmen der zurzeit vom System entdeckbaren Geräte
-   Auflisten der zurzeit auffindbaren Geräte und Überwachung von Änderungen
-   Auflisten der zurzeit auffindbaren Geräte und Überwachen von Änderungen in einer Hintergrundaufgabe

## DeviceInformation-Objekte


Beim Arbeiten mit Enumerations-APIs werden Sie häufig [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)-Objekte verwenden müssen. Diese Objekte enthalten einen Großteil der verfügbaren Informationen zum Gerät. Die folgende Tabelle beschreibt einige der **DeviceInformation**-Eigenschaften, die für Sie von Interesse sein werden. Die vollständige Liste finden Sie auf der Referenzseite für **DeviceInformation**.

| Eigenschaft                         | Kommentare                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **DeviceInformation.Id**         | Dies ist der eindeutige Bezeichner des Geräts, der als Zeichenfolgenvariable bereitgestellt wird. In den meisten Fällen ist dies ein verdeckter Wert, den Sie einfach von einer Methode an eine andere übergeben, um das für Sie relevante Gerät anzugeben. Sie können diese Eigenschaft und die **DeviceInformation.Kind**-Eigenschaft auch nach dem Schließen und erneuten Öffnen Ihrer App verwenden. Hierdurch wird sichergestellt, dass Sie dasselbe [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)-Objekt wiederherstellen und wiederverwenden können. |
| **DeviceInformation.Kind**       | Diese gibt die Art des Geräteobjekts an, das durch das [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)-Objekt dargestellt wird. Dies entspricht nicht der Gerätekategorie oder dem Gerätetyp. Ein einzelnes Gerät kann durch mehrere verschiedene **DeviceInformation**-Objekte unterschiedlicher Art dargestellt werden. Die möglichen Werte dieser Eigenschaft sowie ihre Beziehung zueinander werden unter [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx) aufgeführt.                           |
| **DeviceInformation.Properties** | Die Eigenschaftensammlung enthält Informationen, die für das [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)-Objekt angefordert werden. Auf die gebräuchlichsten Eigenschaften wird einfach als Eigenschaften des **DeviceInformation**-Objekts verwiesen, z.B. mit [**DeviceInformation.Name**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.name). Weitere Informationen finden Sie unter [Geräteinformationseigenschaften](device-information-properties.md).                                                                |

 

## DevicePicker-UI


Der [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) ist ein von Windows bereitgestelltes Steuerelement, das eine kleine Benutzeroberfläche erstellt, über die der Benutzer ein Gerät aus einer Liste auswählen kann. Sie können das **DevicePicker**-Fenster auf verschiedene Weisen anpassen.

-   Sie können die auf der Benutzeroberfläche angezeigten Geräte steuern, indem Sie [**SupportedDeviceSelectors**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors.aspx)oder [**SupportedDeviceClasses**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses.aspx) oder beide zum [**DevicePicker.Filter**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.filter)-Objekt hinzufügen. In den meisten Fällen müssen Sie nur einen Selektor oder eine Klasse hinzufügen. Sie können bei Bedarf jedoch auch mehrere Selektoren oder Klassen hinzufügen. Wenn Sie mehrere Selektoren oder Klassen hinzufügen, werden diese über eine OR-Logikfunktion verbunden.
-   Sie können die für das Gerät abzurufenden Eigenschaften angeben. Fügen Sie hierzu Eigenschaften zu [**DevicePicker.RequestedProperties**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.requestedproperties) hinzu.
-   Sie können die Darstellung von [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) mithilfe von [**Appearance**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.appearance) ändern.
-   Sie können die Größe und Position von [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) angeben, wenn dieses angezeigt wird.

Während [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) angezeigt wird, werden die Inhalte der Benutzeroberfläche automatisch aktualisiert, wenn Geräte hinzugefügt, entfernt oder aktualisiert werden.

**Hinweis**  Sie können [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx) nicht mithilfe von [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) angeben. Wenn Sie Geräte mit einem bestimmten **DeviceInformationKind** ermitteln möchten, müssen Sie einen [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) erstellen und eine eigene Benutzeroberfläche bereitstellen.

 

Für die Übertragung von Medieninhalten und DIAL werden jeweils eigene Auswahlelemente bereitgestellt, wenn Sie diese verwenden möchten. Diese sind jeweils [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn972525) und [**DialDevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn946783).

## Enumerieren einer Momentaufnahme von Geräten


In einigen Szenarien ist [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) wahrscheinlich nicht für Ihre Zwecke geeignet, und Sie benötigen eine flexiblere Alternative. Vielleicht möchten Sie eine eigene Benutzeroberfläche erstellen oder müssen Geräte auflisten, ohne dem Benutzer eine Benutzeroberfläche anzuzeigen. In diesen Fällen können Sie eine Momentaufnahme der Geräte auflisten. Dies umfasst das Durchsuchen der Geräte, die derzeit mit dem System verbunden oder gekoppelt sind. Beachten Sie, dass diese Methode nur Momentaufnahmen der verfügbaren Geräte erfasst. Es werden also keine Geräte gefunden, die verbunden werden, nachdem die Liste durchlaufen wurde. Außerdem werden Sie nicht benachrichtigt, wenn ein Gerät aktualisiert oder entfernt wird. Ein weiterer möglicher Nachteil ist, dass diese Methode sämtliche Ergebnisse zurückhält, bis die gesamte Enumeration abgeschlossen ist. Aus diesem Grund sollten Sie diese Methode nicht verwenden, wenn Sie an den Objekten **AssociationEndpoint**, **AssociationEndpointContainer** oder **AssociationEndpointService** interessiert sind, da diese über ein Netzwerk- oder Drahtlosprotokoll gefunden werden. Dieser Vorgang kann bis zu 30Sekunden dauern. In diesem Szenario sollten Sie ein [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446)-Objekt verwenden, um die möglichen Geräte zu enumerieren.

Um Momentaufnahmen von Geräten zu enumerieren, verwenden Sie die [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.findallasync.aspx)-Methode. Diese Methode wartet, bis der gesamte Enumerationsvorgang abgeschlossen ist, und gibt alle Ergebnisse als ein [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationcollection.aspx)-Objekt zurück. Zudem handelt es sich um eine überladene Methode, die Ihnen verschiedene Optionen zum Filtern und Einschränken der Ergebnisse auf die für Sie relevanten Geräte bietet. Zu diesem Zweck können Sie [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381) bereitstellen oder eine Geräteauswahl übergeben. Die Geräteauswahl ist eine AQS-Zeichenfolge, die die zu enumerierenden Geräte angibt. Weitere Informationen finden Sie unter [Erstellen einer Geräteauswahl](build-a-device-selector.md).

Nachfolgend finden Sie ein Beispiel für die Momentaufnahme einer Geräteaufzählung:



Zusätzlich zur Einschränkung der Ergebnisse können Sie auch die Eigenschaften angeben, die Sie für die Geräte abrufen möchten. In diesem Fall sind die angegebenen Eigenschaften in der Eigenschaftensammlung jedes [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)-Objekts verfügbar, das in der Sammlung zurückgegeben wird. Es muss beachtet werden, dass nicht alle Eigenschaften für sämtliche Gerätearten verfügbar sind. Welche Eigenschaften für welche Gerätearten verfügbar sind, erfahren Sie unter [Geräteinformationseigenschaften](device-information-properties.md).



## Auflisten und Anzeigen von Geräten


Eine leistungsfähigere und flexiblere Methode für die Enumeration von Geräten ist die Erstellung von [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446). Diese Option bietet die größte Flexibilität bei der Enumeration von Geräten. Sie ermöglicht Ihnen die Enumeration der aktuell verfügbaren Geräte sowie den Empfang von Benachrichtigungen, wenn mit der Geräteauswahl übereinstimmende Geräte hinzugefügt oder entfernt bzw. deren Eigenschaften geändert werden. Beim Erstellen von **DeviceWatcher** stellen Sie eine Geräteauswahl bereit. Weitere Informationen zur Geräteauswahl finden Sie unter [Erstellen einer Geräteauswahl](build-a-device-selector.md). Nachdem Sie die Überwachung erstellt haben, erhalten Sie für jedes Gerät, das die angegebenen Kriterien erfüllt, die folgenden Benachrichtigungen.

-   Benachrichtigung über das Hinzufügen eines neuen Geräts
-   Benachrichtigung über die Änderung einer für Sie relevanten Eigenschaft
-   Benachrichtigung über das Entfernen eines Geräts, das entweder nicht mehr verfügbar ist oder die Filterkriterien nicht mehr erfüllt

In den meisten Fällen, in denen Sie [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) verwenden, verwalten Sie eine Liste der Geräte, fügen Elemente hinzu und entfernen oder aktualisieren Elemente, während das Überwachungselement Updates von den überwachten Geräten empfängt. Wenn Sie eine Aktualisierungsbenachrichtigung empfangen, sind die aktualisierten Informationen als [**DeviceInformationUpdate**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationupdate.aspx)-Objekt verfügbar. Um die Geräteliste zu aktualisieren, suchen Sie zunächst die entsprechenden geänderten [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393). Anschließend rufen Sie die [**Update**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.update)-Methode für dieses Objekt auf, indem Sie das **DeviceInformationUpdate**-Objekt angeben. Dabei handelt es sich um eine benutzerfreundliche Funktion, die Ihr **DeviceInformation**-Objekt automatisch aktualisiert.

Da ein [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) Benachrichtigungen sendet, wenn Geräte hinzugefügt und geändert werden, sollten Sie diese Methode für die Enumeration von Geräten verwenden, wenn Sie an **AssociationEndpoint**-, **AssociationEndpointContainer**- oder **AssociationEndpointService**-Objekten interessiert sind, da diese über Netzwerk- oder Drahtlosprotokolle enumeriert werden.

Zum Erstellen von [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) verwenden Sie eine der [**CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.createwatcher.aspx)-Methoden. Es handelt sich um überladene Methoden, die die Angabe der für Sie relevanten Geräte ermöglichen. Zu diesem Zweck können Sie [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381) bereitstellen oder eine Geräteauswahl übergeben. Die Geräteauswahl ist eine AQS-Zeichenfolge, die die zu enumerierenden Geräte angibt. Weitere Informationen finden Sie unter [Erstellen einer Geräteauswahl](build-a-device-selector.md). Sie können auch die Eigenschaften angeben, die Sie für die Geräte abrufen möchten, die für Sie interessant sind. In diesem Fall sind die angegebenen Eigenschaften in der Eigenschaftensammlung jedes [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)-Objekts verfügbar, das in der Sammlung zurückgegeben wird. Es muss beachtet werden, dass nicht alle Eigenschaften für sämtliche Gerätearten verfügbar sind. Welche Eigenschaften für welche Gerätearten verfügbar sind, erfahren Sie unter [Geräteinformationseigenschaften](device-information-properties.md).

## Überwachen von Geräten als Hintergrundaufgabe


Die Geräteüberwachung als Hintergrundaufgabe ist nahezu identisch mit der oben beschriebenen Erstellung von [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446). Tatsächlich müssen Sie zunächst ein normales **DeviceWatcher**-Objekt erstellen wie im vorangehenden Abschnitt beschrieben. Nach der Erstellung rufen Sie [**GetBackgroundTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicewatcher.enumerationcompleted.aspx) anstelle von [**DeviceWatcher.Start**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicewatcher.start) auf. Beim Aufruf von **GetBackgroundTrigger** müssen Sie angeben, welche der Benachrichtigungen Ihnen wichtig sind: Hinzufügen, Entfernen oder Aktualisieren. Benachrichtigungen über das Aktualisieren oder Entfernen können nur zusammen mit einer Benachrichtigung über das Hinzufügen angefordert werden. Nachdem Sie den Trigger registriert haben, wird die Ausführung von **DeviceWatcher** sofort im Hintergrund gestartet. Ab dann wird die Hintergrundaufgabe jedes Mal ausgelöst, wenn sie eine neue mit den Kriterien übereinstimmende Benachrichtigung für Ihre Anwendung empfängt. Sie teilt Ihnen die seit der letzten Auslösung der Anwendung vorgenommenen Änderungen mit.

**Wichtig**  Ihre Anwendung wird erstmalig von [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838) ausgelöst, wenn die Überwachung den Zustand **EnumerationCompleted** erreicht. Dies bedeutet, dass alle anfänglich verfügbaren Ergebnisse enthalten sind. Wenn die Anwendung in Zukunft ausgelöst wird, enthält die Hintergrundaufgabe nur die Benachrichtigungen über Hinzufügungs-, Aktualisierungs- und Entfernungsvorgänge, die seit dem letzten Trigger aufgetreten sind. Dies unterscheidet sich geringfügig von dem im Vordergrund ausgeführten [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446)-Objekt, da die anfänglichen Ergebnisse nicht nacheinander eingehen, sondern erst nach Erreichen des Zustands **EnumerationCompleted** gebündelt übermittelt werden.

 

Abhängig davon, ob der Scanvorgang im Vorder- oder Hintergrund ausgeführt wird, unterscheidet sich das Verhalten einiger Drahtlosprotokolle. Einige Protokolle unterstützen möglicherweise keine Scans im Hintergrund. Für Scans im Hintergrund gibt es drei Möglichkeiten. Die folgende Tabelle enthält die Möglichkeiten und Auswirkungen, die sich ggf. für die Anwendung ergeben. Für Bluetooth und Wi-Fi Direct werden Hintergrundscans beispielsweise nicht unterstützt, sodass auch [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838) nicht unterstützt wird.

| Verhalten                                  | Auswirkungen                                                                                                                                  |
|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| Identisches Verhalten im Hintergrund               | Keine                                                                                                                                    |
| Nur passive Hintergrundscans | Beim Warten auf einen passiven Scan dauert die Geräteermittlung möglicherweise länger.                                                           |
| Keine Unterstützung von Hintergrundscans            | Von [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838) werden keine Geräte erkannt, und es werden keine Updates gemeldet. |

 

Wenn [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838) ein Protokoll enthält, von dem das Scannen als Hintergrundaufgabe nicht unterstützt wird, ist der Trigger trotzdem funktionsfähig. Sie sind jedoch nicht in der Lage, Updates oder Ergebnisse über dieses Protokoll abzurufen. Die Updates für andere Protokolle oder Geräte werden weiterhin auf normale Weise erkannt.

## Verwenden von DeviceInformationKind


In den meisten Szenarien müssen Sie sich nicht mit [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx) eines [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)-Objekts befassen. Die von der verwendeten Geräte-API zurückgegebene Geräteauswahl sorgt in vielen Fällen dafür, dass Sie die richtigen Geräteobjektarten für die betreffende API erhalten. In einigen Szenarien möchten Sie jedoch **DeviceInformation** für Geräte abrufen, haben jedoch keine entsprechende Geräte-API zur Bereitstellung einer Geräteauswahl zur Verfügung. In diesen Fällen müssen Sie eine eigene Auswahl erstellen. Webdienste für Geräte (Web Services on Devices) verfügt beispielsweise über keine dedizierte API. Sie können jedoch die [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459)-APIs verwenden, um diese Geräte zu ermitteln und Informationen über sie zu erhalten, und sie anschließend mit Sockets-APIs verwenden.

Wenn Sie eine eigene Geräteauswahl zum Enumerieren von Geräteobjekten erstellen, kommt es auf das genaue Verständnis von [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx) an. Alle möglichen Arten sowie ihre Beziehungen zueinander werden auf der Referenzseite für **DeviceInformationKind** beschrieben. Eine der häufigsten Arten der Verwendung von **DeviceInformationKind** besteht darin, beim Übermitteln einer Abfrage zusammen mit einer Geräteauswahl die Art der gesuchten Geräte anzugeben. Auf diese Weise wird sichergestellt, dass nur Geräte aufgelistet werden, die mit der bereitgestellten **DeviceInformationKind** übereinstimmen. Beispielsweise könnten Sie ein **DeviceInterface**-Objekt finden und anschließend eine Abfrage ausführen, um die Informationen für das übergeordnete **Device**-Objekt abzurufen. Das übergeordnete Objekt enthält möglicherweise zusätzliche Informationen.

Es ist wichtig, zu beachten, dass die in der Eigenschaftensammlung für ein [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)-Objekt verfügbaren Eigenschaften je nach [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx) des Geräts variieren. Manche Eigenschaften sind nur für bestimmte Arten verfügbar. Weitere Informationen dazu, welche Eigenschaften für welche Arten verfügbar sind, finden Sie unter [Geräteinformationseigenschaften](device-information-properties.md). Daher erhalten Sie im vorangehenden Beispiel bei der Suche nach dem übergeordneten **Device** Zugriff auf weitere Informationen, die über das **DeviceInterface**-Geräteobjekt nicht zur Verfügung gestellt wurden. Beim Erstellen von AQS-Filterzeichenfolgen muss deshalb sichergestellt werden, dass die angeforderten Eigenschaften für die aufzulistenden **DeviceInformationKind**-Objekte verfügbar sind. Weitere Informationen zum Erstellen eines Filters finden Sie unter [Erstellen einer Geräteauswahl](build-a-device-selector.md).

Beim Enumerieren von **AssociationEndpoint**-, **AssociationEndpointContainer**- oder **AssociationEndpointService**-Objekten führen Sie eine Enumeration über ein Drahtlos- oder Netzwerkprotokoll durch. In diesen Situationen empfiehlt es sich, dass Sie nicht [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.findallasync.aspx), sondern [**CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.createwatcher.aspx) verwenden. Das liegt daran, dass die Suche über ein Netzwerk häufig zu Suchvorgängen führt, die erst nach mindestens 10Sekunden ablaufen, bevor [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicewatcher.enumerationcompleted.aspx) generiert wird. **FindAllAsync** schließt den Vorgang erst ab, wenn **EnumerationCompleted** ausgelöst wird. Bei Verwendung von [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) liegen die Ergebnisse näher an der Echtzeit, unabhängig davon, wann **EnumerationCompleted** aufgerufen wird.

## Speichern eines Geräts zur späteren Verwendung


Jedes [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)-Objekt wird durch eine Kombination aus zwei Informationselementen eindeutig identifiziert: [**DeviceInformation.Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id) und [**DeviceInformation.Kind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.kind.aspx). Wenn Sie diese beiden Informationselemente beibehalten, können Sie das **DeviceInformation**-Objekt erneut erstellen, nachdem es verloren gegangen ist, indem Sie diese Informationen für [**CreateFromIdAsync**](https://msdn.microsoft.com/library/windows/apps/br225425.aspx) bereitstellen. Auf diese Weise können Sie Benutzereinstellungen für ein Gerät speichern, mit dem die App verwendet wird.


 

 







<!--HONumber=Aug16_HO5-->


