---
author: drewbatgit
ms.assetid: D20C8E01-4E78-4115-A2E8-07BB3E67DDDC
description: "In diesem Artikel wird beschrieben, wie Sie auf die Taschenlampe eines Geräts zugreifen und diese verwenden, sofern vorhanden. Die Taschenlampenfunktion wird unabhängig von der Kamera des Geräts und der Blitzfunktion der Kamera verwaltet."
title: "Kameraunabhängige Taschenlampe"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 022ca2848c575f545402b13e19c0854a9e3ec74a

---

# Kameraunabhängige Taschenlampe

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


In diesem Artikel wird beschrieben, wie Sie auf die Taschenlampe eines Geräts zugreifen und diese verwenden, sofern vorhanden. Die Taschenlampenfunktion wird unabhängig von der Kamera des Geräts und der Blitzfunktion der Kamera verwaltet. Neben dem Abrufen eines Verweises auf die Leuchte und dem Anpassen ihrer Einstellungen erfahren Sie in diesem Artikel auch, wie die Leuchtenressourcen korrekt freigegeben werden, wenn sie nicht verwendet wird, und wie Sie ermitteln können, wann sich die Verfügbarkeit der Leuchte ändert, falls sie von einer anderen App verwendet wird.

## Abrufen der Standardleuchte des Geräts

Um die Standardleuchte des Geräts abzurufen, rufen Sie [**Lamp.GetDefaultAsync**](https://msdn.microsoft.com/library/windows/apps/dn894327) auf. Die Leuchten-APIs sind im [**Windows.Devices.Lights**](https://msdn.microsoft.com/library/windows/apps/dn894331)-Namespace zu finden. Achten Sie darauf, dass Sie eine „using“-Direktive für diesen Namespace hinzufügen, bevor Sie versuchen, auf diese APIs zuzugreifen.

[!code-cs[LightsNamespace](./code/Lamp/cs/MainPage.xaml.cs#SnippetLightsNamespace)]


[!code-cs[DeclareLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetDeclareLamp)]


[!code-cs[GetDefaultLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetGetDefaultLamp)]

Wenn das zurückgegebene Objekt **null** ist, wird die **Lamp**-API auf dem Gerät nicht unterstützt. Einige Geräte unterstützen möglicherweise die **Lamp**-API nicht, auch wenn auf dem Gerät tatsächlich eine Leuchte vorhanden ist.

## Abrufen einer bestimmten Leuchte mithilfe der Leuchtenauswahlzeichenfolge

Einige Geräte verfügen möglicherweise über mehrere Leuchten. Um eine Liste der auf dem Gerät verfügbaren Leuchten zu erhalten, rufen Sie die Geräteauswahlzeichenfolge ab, indem Sie [**GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn894328) aufrufen. Diese Auswahlzeichenfolge kann dann an [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) übergeben werden. Diese Methode wird verwendet, um viele verschiedene Arten von Geräten aufzulisten. Mit der Auswahlzeichenfolge kann die Methode angewiesen werden, dass nur Geräte mit Leuchten zurückgegeben werden sollen. Das von **FindAllAsync** zurückgegeben [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395)-Objekt ist eine Sammlung von [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393)-Objekten, die die auf dem Gerät verfügbaren Leuchten darstellen. Wählen Sie eines der Objekte in der Liste aus, und übergeben Sie dann die [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437)-Eigenschaft an [**Lamp.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn894326), um einen Verweis auf die angeforderte Leuchte abzurufen. In diesem Beispiel wird die **GetFirstOrDefault**-Erweiterungsmethode aus dem **System.Linq**-Namespace zum Auswählen des **DeviceInformation**-Objekts verwendet, wobei die [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906)-Eigenschaft den Wert **Back** aufweist. Dadurch wird eine Leuchte auf der Rückseite des Gerätegehäuses ausgewählt, sofern vorhanden.

Beachten Sie, dass sich die [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393)-APIs im [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459)-Namespace befinden.

[!code-cs[EnumerationNamespace](./code/Lamp/cs/MainPage.xaml.cs#SnippetEnumerationNamespace)]

[!code-cs[GetLampWithSelectionString](./code/Lamp/cs/MainPage.xaml.cs#SnippetGetLampWithSelectionString)]

## Anpassen der Leuchteneinstellungen

Sobald Sie eine Instanz der [**Lamp**](https://msdn.microsoft.com/library/windows/apps/dn894310)-Klasse haben, aktivieren Sie die Leuchte, indem Sie die [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn894330)-Eigenschaft auf **true** setzen.

[!code-cs[LampSettingsOn](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsOn)]

Schalten Sie die Leuchte aus, indem Sie die [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn894330)-Eigenschaft auf **false** setzen.

[!code-cs[LampSettingsOff](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsOff)]

Einige Geräte haben Leuchten, die Farbwerte unterstützen. Überprüfen Sie, ob eine Leuchte Farben unterstützt, indem Sie die [**IsColorSettable**](https://msdn.microsoft.com/library/windows/apps/dn894329)-Eigenschaft überprüfen. Wenn dieser Wert **true** ist, können Sie die Farbe der Leuchte mit der [**Color**](https://msdn.microsoft.com/library/windows/apps/dn894322)-Eigenschaft festlegen.

[!code-cs[LampSettingsColor](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsColor)]

## Registrierung für Benachrichtigung, wenn sich die Leuchtenverfügbarkeit ändert

Zugriff auf die Leuchte wird der letzten App gewährt, die den Zugriff angefordert hat. Wenn also eine andere App gestartet wird, die eine Leuchtenressource anfordert, die Ihre App gerade verwendet, kann Ihre App die Leuchte erst wieder steuern, wenn die andere App die Ressource freigegeben hat. Um eine Benachrichtigung zu erhalten, wenn sich die Verfügbarkeit der Leuchte ändert, registrieren Sie einen Handler für das [**Lamp.AvailabilityChanged**](https://msdn.microsoft.com/library/windows/apps/dn894317)-Ereignis.

[!code-cs[AvailabilityChanged](./code/Lamp/cs/MainPage.xaml.cs#SnippetAvailabilityChanged)]

Überprüfen Sie im Handler für das Ereignis, ob es sich bei der Eigenschaftenänderung, die das Ereignis ausgelöst hat, um die [**LampAvailabilityChanged.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/dn894315)-Eigenschaft handelte. In diesem Beispiel wird ein Umschalter zum An- und Ausschalten der Leuchte je nach Verfügbarkeit der Leuchte aktiviert oder deaktiviert.

[!code-cs[AvailabilityChangedHandler](./code/Lamp/cs/MainPage.xaml.cs#SnippetAvailabilityChangedHandler)]

## Ordnungsgemäßes Freigeben der Leuchtenressourcen, wenn diese nicht verwendet wird

Wenn Sie die Leuchte nicht mehr verwenden, sollten Sie sie deaktivieren und [**Lamp.Close**](https://msdn.microsoft.com/library/windows/apps/dn894320) aufrufen, um die Ressource freizugeben und anderen Apps den Zugriff auf die Leuchte zu ermöglichen. Diese Eigenschaft ist der **Dispose**-Methode zugewiesen, wenn Sie C# verwenden. Wenn Sie sich für [**AvailabilityChanged**](https://msdn.microsoft.com/library/windows/apps/dn894317) registriert haben, sollten Sie die Registrierung des Handlers aufheben, wenn Sie die Leuchtenressource freigeben. Der richtige Stelle in Ihrem Code für das Freigeben der Leuchtenressource ist von Ihrer App abhängig. Um den Leuchtenzugriff auf eine einzige Seite zu beschränken, geben Sie die Ressource im [**OnNavigatingFrom**](https://msdn.microsoft.com/library/windows/apps/br227509)-Ereignis frei.

[!code-cs[DisposeLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetDisposeLamp)]

 

 







<!--HONumber=Jun16_HO4-->


