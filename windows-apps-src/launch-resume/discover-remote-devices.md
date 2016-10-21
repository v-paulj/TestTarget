---
author: PatrickFarley
title: "Ermitteln von Remotegeräten"
description: "Erfahren Sie, wie Sie Remotegeräte über Ihre App mit Project &quot;Rome&quot; ermitteln können."
translationtype: Human Translation
ms.sourcegitcommit: ff8e16d0e376d502157ae42b9cdae11875008554
ms.openlocfilehash: cb1f9cf6915378203919fdf63bcebc935af74a30

---

# Ermitteln von Remotegeräten
Ihre App kann WLAN, Bluetooth und eine Cloud-Verbindung verwenden, um Windows-basierte Geräte zu ermitteln, die mit demselben Microsoft-Konto wie das ermittelnde Gerät angemeldet sind. Kommunale Geräte, die anonyme Verbindungen akzeptieren können, wie z. B. Surface Hub und Xbox One, können ebenfalls ermittelt werden. Auf den Remotegeräten muss keine spezielle Software installiert sein, damit sie erkennbar sind.

> [!NOTE]
> In diesem Handbuch wird davon ausgegangen, dass Ihnen bereits Zugriff auf das Remotesysteme-Feature gewährt wurde, indem Sie die Schritte in [Starten einer Remote-App](launch-a-remote-app.md) befolgt haben.

## Filtern der Gruppe von erkennbaren Geräten
Die Gruppe erkennbarer Geräte kann mithilfe von [**RemoteSystemWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher) mit Filtern eingegrenzt werden. Filter können den Ermittlungstyp (lokales Netzwerk gegenüber Cloud-Verbindung), Gerätetyp (Desktop, mobiles Gerät, Xbox, Hub und Hologramm) und den Verfügbarkeitsstatus (den Status der Verfügbarkeit eines Geräts für die Verwendung von Remotesystem-Features) erkennen.

Filter-Objekte müssen erstellt werden, bevor das **RemoteSystemWatcher**-Objekt initialisiert wird, da sie als Parameter an den Konstruktor übergeben werden. Der folgende Code erstellt einen Filter von jedem verfügbaren Typ und fügt sie anschließend einer Liste hinzu.

> [!NOTE]
> Der Code in diesen Beispielen setzt voraus, dass Sie über eine `using Windows.System.RemoteSystems`-Anweisung in Ihrer Datei verfügen.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetMakeFilterList)]

Sobald eine Liste mit [**IRemoteSystemFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.IRemoteSystemFilter)-Objekten erstellt wurde, kann sie an den Konstruktor eines **RemoteSystemWatcher**übergeben werden.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetCreateWatcher)]

Wenn die [**Start**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.Start)-Methode dieses Überwachungselements aufgerufen wird, wird das [**RemoteSystemAdded**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.RemoteSystemAdded)-Ereignis nur dann ausgelöst, wenn ein Gerät erkannt wird, das alle der folgenden Kriterien erfüllt:
* Es kann von einer proximalen Verbindung erkannt werden
* Es ist ein Desktop oder ein Telefon
* Es wird als verfügbar klassifiziert

Ab diesem Punkt ist die Vorgehensweise zum Behandeln von Ereignissen, Abrufen von [**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem)-Objekten und Herstellen einer Verbindung mit Remotegeräten die gleiche wie in [Starten einer Remote-App](launch-a-remote-app.md). Kurz: Die **RemoteSystem**-Objekte werden als Eigenschaften von [**RemoteSystemAddedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemAddedEventArgs)-Objekten gespeichert, die Parameter von jedem **RemoteSystemAdded**-Ereignis sind.

## Ermitteln von Geräten durch Adresseingabe
Einige Geräte sind möglicherweise nicht mit einem Benutzer verknüpft oder durch eine Überprüfung erkennbar. Sie können jedoch trotzdem erreicht werden, wenn die ermittelnde App eine direkte Adresse verwendet. Die [**HostName**](https://msdn.microsoft.com/library/windows/apps/windows.networking.hostname.aspx)-Klasse wird verwendet, um die Adresse eines Remotegeräts darzustellen. Dies wird häufig in Form einer IP-Adresse gespeichert, jedoch sind auch verschiedene andere Formate zulässig (weitere Informationen finden Sie unter [**HostName-Konstruktor**](https://msdn.microsoft.com/library/windows/apps/br207118.aspx).

Ein **RemoteSystem**-Objekt wird abgerufen, wenn ein gültiges **HostName**-Objekt bereitgestellt wird. Wenn die Adressdaten ungültig sind, wird ein `null`-Objektverweis zurückgegeben.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetFindByHostName)]

## Verwandte Themen
[Verbundene Apps und Geräte (Project "Rome")](connected-apps-and-devices.md)  
[Starten einer Remote-App](launch-a-remote-app.md)  
[API-Referenz für Remotesysteme](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)  
Das [Beispiel für Remotesysteme](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) veranschaulicht die Vorgehensweise zum Erkennen eines Remotesystems, Starten einer App auf einem Remotesystem und Verwenden von App-Diensten zum Senden von Nachrichten zwischen Apps, die auf zwei Systemen ausgeführt werden.



<!--HONumber=Aug16_HO5-->


