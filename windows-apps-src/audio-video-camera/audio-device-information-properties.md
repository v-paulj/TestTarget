---
author: drewbatgit
ms.assetid: 3b75d881-bdcf-402b-a330-23cd29d68e53
description: "Dieser Artikel enthält eine Liste mit den DeviceInformation-Eigenschaften für Audiogeräte."
title: "Eigenschaften für Audiogeräteinformationen"
translationtype: Human Translation
ms.sourcegitcommit: 0745e96715ba49582ab762d4b25f1b8e681116f5
ms.openlocfilehash: 08ebb37679d1dd93458a3ffe846d8bd33574635d

---

# Eigenschaften für Audiogeräteinformationen

Dieser Artikel enthält eine Liste mit den Geräteinformationseigenschaften für Audiogeräte. Unter Windows sind jedem Hardwaregerät [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)-Eigenschaften mit ausführlichen Geräteinformationen zugeordnet, auf die Sie zurückgreifen können, wenn Sie bestimmte Geräteinformationen benötigen oder eine Geräteauswahl erstellen möchten. Allgemeine Informationen zum Aufzählen von Geräten unter Windows finden Sie unter [Auflisten von Geräten](../devices-sensors/enumerate-devices.md) und [Geräteinformationseigenschaften](../devices-sensors/device-information-properties.md).


|Name|Typ|Beschreibung|
|------------------------------------------------------------|------------|------------------------------------------------------|
|**System.Devices.AudioDevice.Microphone.SensitivityInDbfs**|Double|Gibt die Empfindlichkeit des Mikrofons in Dezibel relativ zu Full-Scale-Einheiten (dBFS) an.|
|**System.Devices.AudioDevice.Microphone.SignalToNoiseRationInDb**|Double|Gibt für das Mikrofon das Signal-Rausch-Verhältnis (SNR) in Dezibeleinheiten (dB) an.|
|**System.Devices.AudioDevice.SpeechProcessingSupported**|Boolean|Gibt an, ob das Audiogerät die Verarbeitung von Sprache unterstützt.|
|**System.Devices.AudioDevice.RawProcessingSupported**|Boolean|Gibt an, ob das Audiogerät die Verarbeitung von Rohdaten unterstützt.|
|**System.Devices.MicrophoneArray.Geometry**|unsigned char[]|Geometriedaten für ein Mikrofonarray.|

## Verwandte Themen

* [Auflisten von Geräten](../devices-sensors/enumerate-devices.md)
* [Geräteinformationseigenschaften](../devices-sensors/device-information-properties.md)
* [Erstellen einer Geräteauswahl](../devices-sensors/build-a-device-selector.md)
* [Medienwiedergabe](media-playback.md)







<!--HONumber=Aug16_HO3-->


