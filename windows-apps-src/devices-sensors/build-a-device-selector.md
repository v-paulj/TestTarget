---
author: DBirtolo
ms.assetid: D06AA3F5-CED6-446E-94E8-713D98B13CAA
title: "Erstellen einer Geräteauswahl"
description: "Durch das Erstellen einer Geräteauswahl können Sie die Geräte begrenzen, die Sie beim Auflisten von Geräten durchsuchen."
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 67bf2795a7d555dc5cd236eeafb07009511fe5d3

---
# Erstellen einer Geräteauswahl

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


** Wichtige APIs **

-   [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459)

Durch das Erstellen einer Geräteauswahl können Sie die Geräte begrenzen, die Sie beim Auflisten von Geräten durchsuchen. Auf diese Weise erhalten Sie lediglich die relevanten Ergebnisse, und die Leistung des Systems wird optimiert. In den meisten Szenarien erhalten Sie eine Geräteauswahl von einem Gerätestapel. Beispielsweise können Sie [**GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/Dn264015) für Geräte verwenden, die über USB erkannt werden. Diese Geräteauswahl gibt eine AQS-Zeichenfolge (Advanced Query Syntax) zurück. Wenn Sie mit dem AQS-Format nicht vertraut sind, erhalten Sie weitere Informationen unter [Programmgesteuerte Verwendung der erweiterten Abfragesyntax](https://msdn.microsoft.com/library/windows/desktop/Bb266512).

## Erstellen der Filterzeichenfolge

Es gibt Situationen, in denen Sie Geräte auflisten müssen, eine angegebene Geräteauswahl für Ihr Szenario aber nicht verfügbar ist. Eine Geräteauswahl ist eine AQS-Filterzeichenfolge, die die folgenden Informationen enthält. Vor dem Erstellen einer Filterzeichenfolge müssen Sie einige wichtige Informationen zu den Geräten kennen, die Sie aufzählen möchten.

-   Die [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)-Aufzählung der betreffenden Geräte. Weitere Informationen dazu, wie sich **DeviceInformationKind** auf die Aufzählung von Geräten auswirkt, finden Sie unter [Auflisten von Geräten](enumerate-devices.md).
-   Vorgehensweise bei der Erstellung einer AQS-Filterzeichenfolge (in diesem Thema beschrieben)
-   Für Sie interessante Eigenschaften Die verfügbaren Eigenschaften richten sich nach der [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)-Aufzählung. Weitere Informationen finden Sie unter [Geräteinformationseigenschaften](device-information-properties.md).
-   Von Ihnen für die Abfrage verwendete Protokolle Dies ist nur erforderlich, wenn Sie über ein verkabeltes oder Drahtlosnetzwerk nach Geräten suchen. Weitere Informationen hierzu finden Sie unter [Auflisten von Geräten über ein Netzwerk](enumerate-devices-over-a-network.md).

Bei Verwendung der [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459)-APIs kombinieren Sie die Geräteauswahl häufig mit der für Sie relevanten Geräteart. Die Liste der verfügbaren Gerätearten wird durch die [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)-Aufzählung definiert. Diese Kombination von Faktoren hilft Ihnen, die verfügbaren Geräte auf diejenigen Geräte zu beschränken, die für Sie relevant sind. Wenn Sie die **DeviceInformationKind**-Aufzählung nicht angeben oder die verwendete Methode keinen **DeviceInformationKind**-Parameter bereitstellt, wird als Standard **DeviceInterface** verwendet.

Die [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459)-APIs enthalten eine Canonical AQS-Syntax, die jedoch nicht alle Operatoren unterstützt. Eine Liste der beim Erstellen der Filterzeichenfolge verfügbaren Eigenschaften finden Sie unter [Geräteinformationseigenschaften](device-information-properties.md).


            **Achtung**  Mithilfe des `{GUID} PID`-Formats definierte benutzerdefinierte Eigenschaften können beim Erstellen der AQS-Filterzeichenfolge nicht verwendet werden. Dies liegt daran, dass der Eigenschaftstyp vom bekannten Eigenschaftennamen abgeleitet wird.

 

Die folgende Tabelle enthält die AQS-Operatoren mit den von ihnen unterstützten Parametertypen.

| Operator                       | Unterstützte Typen                                                             |
|--------------------------------|-----------------------------------------------------------------------------|
| **COP\_EQUAL**                 | String, boolesch, GUID, UInt16, UInt32                                       |
| **COP\_NOTEQUAL**              | String, boolesch, GUID, UInt16, UInt32                                       |
| **COP\_LESSTHAN**              | UInt16, UInt32                                                              |
| **COP\_GREATERTHAN**           | UInt16, UInt32                                                              |
| **COP\_LESSTHANOREQUAL**       | UInt16, UInt32                                                              |
| **COP\_GREATERTHANOREQUAL**    | UInt16, UInt32                                                              |
| **COP\_VALUE\_CONTAINS**       | String, Zeichenfolgenarray, boolesches Array, GUID-Array, UInt16-Array, UInt32-Array |
| **COP\_VALUE\_NOTCONTAINS**    | String, Zeichenfolgenarray, boolesches Array, GUID-Array, UInt16-Array, UInt32-Array |
| **COP\_VALUE\_STARTSWITH**     | String                                                                      |
| **COP\_VALUE\_ENDSWITH**       | String                                                                      |
| **COP\_DOSWILDCARDS**          | Nicht unterstützt                                                               |
| **COP\_WORD\_EQUAL**           | Nicht unterstützt                                                               |
| **COP\_WORD\_STARTSWITH**      | Nicht unterstützt                                                               |
| **COP\_APPLICATION\_SPECIFIC** | Nicht unterstützt                                                               |


> 
            **Tipp**  Sie können für **COP\_EQUAL** oder **COP\_NOTEQUAL** den Wert **NULL** angeben. Dies führt zu einer Eigenschaft ohne Wert oder ohne vorhandenen Wert. In AQS geben Sie **NULL** mithilfe leerer Klammern \[\] an.

> 
            **Wichtig**  Bei Verwendung der Operatoren **COP\_VALUE\_CONTAINS** und **COP\_VALUE\_NOTCONTAINS** verhalten sie sich bei Zeichenfolgen und Zeichenfolgenarrays auf andere Weise. Im Falle einer Zeichenfolge führt das System eine Suche ohne Berücksichtigung der Groß-/Kleinschreibung durch, um festzustellen, ob das Gerät die angegebene Zeichenfolge als Teilzeichenfolge enthält. Im Falle eines Zeichenfolgenarrays werden Teilzeichenfolgen nicht gesucht. Mit dem Zeichenfolgenarray wird das Array durchsucht, um festzustellen, ob es die gesamte angegebene Zeichenfolge enthält. Es ist nicht möglich, ein Zeichenfolgenarray zu durchsuchen, um festzustellen, ob die Elemente im Array eine Teilzeichenfolge enthalten.

Wenn keine einzelne AQS-Filterzeichenfolge erstellt werden kann, die den richtigen Ergebnisbereich herausfiltert, können Sie Ihre Ergebnisse nach Erhalt filtern. In diesem Fall wird jedoch empfohlen, die Ergebnisse der anfänglichen AQS-Filterzeichenfolge so weit wie möglich einzuschränken, wenn Sie sie für [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459)-APIs bereitstellen. Dadurch wird die Leistung der Anwendung verbessert.

## Beispiele für AQS-Zeichenfolgen

Die folgenden Beispiele veranschaulichen, wie die AQS-Syntax verwendet werden kann, um die aufzulistenden Geräte einzuschränken. Alle diese Filterzeichenfolgen werden mit einer [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) kombiniert, um einen vollständigen Filter zu erstellen. Wenn keine Art angegeben ist, wird **DeviceInterface** als Standardart verwendet.

Wenn dieser Filter zusammen mit einer [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)-Aufzählung vom Typ **DeviceInterface** kombiniert wird, listet er alle Objekte auf, die die Schnittstellenklasse für die Audioaufzeichnung enthalten und derzeit aktiviert sind. 
            **
              =
            ** wird zu **COP\_EQUALS**.

``` syntax
System.Devices.InterfaceClassGuid:="{2eef81be-33fa-4800-9670-1cd474972c3f}" AND 
System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True
```

Wenn dieser Filter zusammen mit einer [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)-Aufzählung vom Typ **Device** kombiniert wird, listet er alle Objekte auf, die mindestens eine Hardware-ID von „GenCdRom“ aufweisen. 
            **
              ~~
            ** wird zu **COP\_VALUE\_CONTAINS**.

``` syntax
System.Devices.HardwareIds:~~"GenCdRom"
```

Wenn dieser Filter zusammen mit einer [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)-Aufzählung vom Typ **DeviceContainer** kombiniert wird, listet er alle Objekte auf, die einen Modellnamen aufweisen, der die Teilzeichenfolge „Microsoft“ enthält. 
            **
              ~~
            ** wird zu **COP\_VALUE\_CONTAINS**.

``` syntax
System.Devices.ModelName:~~"Microsoft"
```

Wenn dieser Filter zusammen mit einer [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)-Aufzählung vom Typ **DeviceInterface** kombiniert wird, listet er alle Objekte auf, deren Name mit der Teilzeichenfolge „Microsoft“ beginnt. 
            **
              ~&lt;
            ** wird zu **COP\_STARTSWITH**.

``` syntax
System.ItemNameDisplay:~<"Microsoft"
```

Wenn dieser Filter zusammen mit einer [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)-Aufzählung vom Typ **Device** kombiniert wird, listet er alle Objekte auf, für die eine **System.Devices.IpAddress**-Eigenschaft festgelegt wurde. 
            **
              &lt;&gt;\[\]** wird zu **COP\_NOTEQUALS** kombiniert mit einem **NULL**-Wert.

``` syntax
System.Devices.IpAddress:<>[]
```

Wenn dieser Filter zusammen mit einer [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991)-Aufzählung vom Typ **Device** kombiniert wird, listet er alle Objekte auf, für die keine **System.Devices.IpAddress**-Eigenschaft festgelegt wurde. 
            **=\[\]** wird zu **COP\_EQUALS**, kombiniert mit einem **NULL**-Wert.

``` syntax
System.Devices.IpAddress:=[]
```

 

 







<!--HONumber=Jun16_HO4-->


