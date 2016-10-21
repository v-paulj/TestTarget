---
author: mcleblanc
ms.assetid: B48E21AB-0EA5-444B-8333-393DD8D1B76D
title: Im Unternehmen freigegebener Speicher
description: "Im Unternehmen freigegebener Speicher definiert lokale Datenspeicherorte für Branchenanwendungen zum Freigeben von Daten."
translationtype: Human Translation
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: bd4663b25c351551cd2f4e1e780a76431d1c3a19

---
# Im Unternehmen freigegebener Speicher

Der freigegebene Speicher besteht aus zwei Speicherorten, auf die Apps mit der eingeschränkten Funktionalität **enterpriseDeviceLockdown** und einem Unternehmenszertifikat vollständigen Lese- und Schreibzugriff haben. Die **enterpriseDeviceLockdown**-Funktion ermöglicht Apps die Verwendung der API zur Gerätesperrung und den Zugriff auf im Unternehmen freigegebene Speicherordner. Weitere Informationen zur API finden Sie unter dem [**Windows.Embedded.DeviceLockdown**](http://go.microsoft.com/fwlink/?LinkId=699331)-Namespace.  

Diese Speicherorte werden auf dem lokalen Laufwerk festgelegt:
- \Data\SharedData\Enterprise\Persistent
- \Data\SharedData\Enterprise\Non-Persistent

## Szenarien

Im Unternehmen freigegebener Speicher unterstützt die folgenden Szenarien.

- Sie können Daten in einer Instanz einer App, zwischen Instanzen derselben App oder zwischen verschiedenen Apps freigeben, wenn beide über die entsprechenden Funktionen und Zertifikate verfügen.
- Sie können Daten auf der lokalen Festplatte im Ordner „\Data\SharedData\Enterprise\Persistent“ speichern, und die Daten bleiben auch nach dem Zurücksetzen des Geräts erhalten.
- Sie können Dateien über die mobile Geräteverwaltung (Mobile Device Management, MDM) auf einem Gerät bearbeiten (u. a. lesen, schreiben und löschen). Weitere Informationen zur Verwendung von im Unternehmen freigegebenem Speichern mit dem MDM-Dienst finden Sie unter [EnterpriseExtFileSystem-CSP](http://go.microsoft.com/fwlink/?LinkId=699333).

## Zugriff auf im Unternehmen freigegebenen Speicher

Das folgende Beispiel zeigt, wie Sie die Funktion für den Zugriff auf im Unternehmen freigegebenen Speicher im Paketmanifest deklarieren und mit der Windows.Storage.StorageFolder-Klasse auf die Ordner für den freigegebenen Speicher zugreifen.

Schließen Sie in das App-Paketmanifest die folgende Funktion ein:

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp rescap">

…

<Capabilities>
    <rescap:Capability Name="enterpriseDeviceLockdown"/>
</Capabilities>
```

Für den Zugriff auf den freigegebenen Datenspeicherort verwendet die App den folgenden Code.

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;
using Windows.Storage;

…

// Get the Enterprise Shared Storage folder.
var enterprisePersistentFolderRoot = @"C:\Data\SharedData\Enterprise\Persistent";

StorageFolder folder =
    await StorageFolder.GetFolderFromPathAsync(enterprisePersistentFolderRoot);

// Get the files in the folder.
IReadOnlyList<StorageFile> sortedItems =
    await folder.GetFilesAsync();

// Iterate over the results and print the list of files
// to the Visual Studio Output window.
foreach (StorageFile file in sortedItems)
    Debug.WriteLine(file.Name + ", " + file.DateCreated);
```




<!--HONumber=Aug16_HO3-->


