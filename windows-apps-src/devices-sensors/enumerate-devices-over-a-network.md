---
author: DBirtolo
ms.assetid: E0B9532F-1195-4927-99BE-F41565D891AD
title: Auflisten von Geräten über ein Netzwerk
description: Zusätzlich zum Ermitteln von lokal verbundenen Geräten können Sie mithilfe der Windows.Devices.Enumeration-APIs Geräte über Drahtlos- und Netzwerkprotokolle enumerieren.
---
# Enumerieren von Geräten über ein Netzwerk

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


** Wichtige APIs **

-   [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459)

Zusätzlich zum Ermitteln von lokal verbundenen Geräten können Sie mithilfe der [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459)-APIs Geräte über Drahtlos- und Netzwerkprotokolle enumerieren.

## Auflisten von Geräten über Netzwerk- oder Drahtlosprotokolle

Manchmal müssen Sie Geräte enumerieren, die nicht lokal verbunden und nur über Drahtlos- oder Netzwerkprotokolle auffindbar sind. Hierzu verfügen die [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459)-APIs über drei unterschiedliche Arten von Geräteobjekten: **AssociationEndpoint** (AEP), **AssociationEndpointContainer** (AEP Container) und **AssociationEndpointService** (AEP Service). Als Gruppe werden sie als AEPs oder AEP-Objekte bezeichnet.

Einige Geräte-APIs stellen eine Auswahlzeichenfolge bereit, mit der Sie die verfügbaren AEP-Objekte durchlaufen können. Dies kann zwei Arten von Geräten umfassen: mit dem System gekoppelte und nicht mit dem System gekoppelte Geräte. Einige Geräte erfordern möglicherweise keine Kopplung. Diese Geräte-APIs können versuchen, das Gerät zu koppeln, wenn dies für die Interaktion erforderlich ist. Wi-Fi Direct ist ein Beispiel für APIs, die diesem Muster folgen. Wenn das Gerät von diesen Geräte-APIs nicht automatisch gekoppelt wird, können Sie es mithilfe des [**DeviceInformationPairing**](https://msdn.microsoft.com/library/windows/apps/Mt168396)-Objekts aus [**DeviceInformation.Pairing**](https://msdn.microsoft.com/library/windows/apps/Dn705960) koppeln.

Es gibt jedoch Situationen, in denen Sie die Geräte manuell ohne vordefinierte Auswahlzeichenfolge ermitteln möchten. Beispielsweise kann es vorkommen, dass Sie nur Informationen über AEP-Geräte sammeln müssen, ohne mit ihnen zu interagieren, oder mehr AEP-Objekte als durch die vordefinierte Auswahlzeichenfolge ermittelt finden möchten. In diesem Fall erstellen Sie eine eigene Auswahlzeichenfolge und verwenden sie gemäß den Anweisungen unter [Erstellen einer Geräteauswahl](build-a-device-selector.md).

Wenn Sie eine eigene Auswahl erstellen, wird dringend empfohlen, den Umfang der Enumeration auf die für Sie relevanten Protokolle zu beschränken. Sie möchten z. B. nicht, dass das WLAN-Radio nach Wi-Fi Direct-Geräten sucht, wenn Sie ausdrücklich an UPnP-Geräten interessiert sind. Windows hat für jedes Protokoll eine Identität definiert, die Sie bei der Angabe des Enumerationsumfangs verwenden können. Die folgende Tabelle enthält die Protokolltypen und -bezeichner.

| Protokoll- oder Netzwerkgerätetyp              | ID                                         |
|----------------------------------------------|--------------------------------------------|
| UPnP (einschließlich DIAL und DLNA)               | **{0e261de4-12f0-46e6-91ba-428607ccef64}** |
| Webdienste für Geräte (Web Services on Devices, WSD)                | **{782232aa-a2f9-4993-971b-aedc551346b0}** |
| Wi-Fi Direct                                 | **{0407d24e-53de-4c9a-9ba1-9ced54641188}** |
| DNS-Dienstermittlung (DNS-SD)               | **{4526e8c1-8aac-4153-9b16-55e86ada0e54}** |
| Point of Service (POS)                             | **{d4bf61b3-442e-4ada-882d-fa7B70c832d9}** |
| Netzwerkdrucker (Active Directory-Drucker) | **{37aba761-2124-454c-8d82-c42962c2de2b}** |
| Windows-Sofortverbindung (WNC)                    | **{4c1b1ef8-2f62-4b9f-9bc5-b21ab636138f}** |
| WiGig-Docks                                  | **{a277f3a5-8764-4f88-8045-4c5e962640b1}** |
| WLAN-Bereitstellung für HP-Drucker           | **{c85ef710-f344-4792-bb6d-85a4346f1e69}** |
| Bluetooth                                    | **{e0cbf06c-cd8b-4647-bb8a-263b43f0f974}** |
| Bluetooth LE                                 | **{bb7bb05e-5972-42b5-94fc-76eaa7084d49}** |

 

## AQS-Beispiele

Jede AEP-Art verfügt über eine Eigenschaft, mit der Sie Ihre Enumeration auf ein bestimmtes Protokoll beschränken können. Beachten Sie, dass Sie den OR-Operator in einem AQS-Filter verwenden können, um mehrere Protokolle zu kombinieren. Die folgenden Beispiele für AQS-Filterzeichenfolgen zeigen die Abfrage für AEP-Geräte.

Diese AQS führt Abfragen für alle **AssociationEndpoint**-UPnP-Objekte aus, wenn [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) auf **AssociationEndpoint** festgelegt ist.

``` syntax
System.Devices.Aep.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

Diese AQS führt Abfragen für alle **AssociationEndpoint**-UPnP- und WSD-Objekte aus, wenn [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) auf **AssociationEndpoint** festgelegt ist.

``` syntax
System.Devices.Aep.ProtocolId:="{782232aa-a2f9-4993-971b-aedc551346b0}" OR 
System.Devices.Aep.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

Diese AQS führt Abfragen für alle **AssociationEndpointService**-UPnP-Objekte aus, wenn [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) auf **AssociationEndpointService** festgelegt ist.

``` syntax
System.Devices.AepService.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

Diese AQS führt Abfragen für **AssociationEndpointContainer**-Objekte aus, wenn [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) auf **AssociationEndpointContainer** festgelegt ist, findet diese jedoch nur, indem das UPnP-Protokoll enumeriert wird. Normalerweise wäre es nicht sinnvoll, Container zu enumerieren, die nur von einem Protokoll stammen. Dies kann jedoch nützlich sein, um den Filter auf Protokolle zu beschränken, von denen Ihr Gerät erkannt werden kann.

``` syntax
System.Devices.AepContainer.ProtocolIds:~~"{0e261de4-12f0-46e6-91ba-428607ccef64}"
```

 

 






<!--HONumber=May16_HO2-->


