---
author: PatrickFarley
title: Kommunikation mit einem App-Remotedienst
description: "Tauschen Sie Nachrichten mit einem App-Dienst aus, der auf einem Remotegerät mit Project &quot;Rome&quot; ausgeführt wird."
translationtype: Human Translation
ms.sourcegitcommit: c90304b7ca3f7185fca9146aa2303b09cba5ab9a
ms.openlocfilehash: bff77a63d0f88907410c74d4dce19fb422c1bd3f

---

# Kommunikation mit einem App-Remotedienst

Sie können nicht nur eine App auf einem Remotegerät mithilfe eines URI starten, sondern auch *App-Dienste* auf Remotegeräten ausführen und mit ihnen kommunizieren. Jedes Windows-basierte Gerät kann als Ausgangs- oder als Zielgerät oder als beides verwendet werden. Dies bietet Ihnen eine nahezu unbegrenzte Anzahl von Möglichkeiten zur Interaktion mit verbundenen Geräten, ohne eine App in den Vordergrund bringen zu müssen.

## Einrichten des App-Dienstes auf dem Zielgerät
Um einen App-Dienst auf einem Remotegerät ausführen zu können, muss bereits ein Anbieter dieses App-Dienstes auf dem Zielgerät installiert sein. In dieser Anleitung wird der Zufallsgenerator-App-Dienst verwendet, der im [Repository Beispiele für Universelle Windows-Plattform](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices) verfügbar ist. Anleitungen zum Schreiben eines eigenen App-Diensts finden Sie unter [Erstellen und Verwenden eines App-Diensts](how-to-create-and-consume-an-app-service.md).

Gleichgültig, ob Sie einen bereits erstellten App-Dienst verwenden oder einen eigenen schreiben, Sie müssen einige Änderungen vornehmen, um den Dienst mit Remotesystemen kompatibel zu machen. Wechseln Sie in Visual Studio zum Projekt des App-Dienstanbieters, und wählen Sie die Datei "Package.appxmanifest" aus. Klicken Sie mit der rechten Maustaste, und wählen Sie **Code anzeigen** aus, um den gesamten Inhalt der Datei anzuzeigen. Suchen Sie das Element **Erweiterung**, das das Projekt als App-Dienst definiert und das übergeordnete Projekt benennt.

``` xml
...
<Extensions>
    <uap:Extension Category="windows.appService" EntryPoint="RandomNumberService.RandomNumberGeneratorTask">
        <uap:AppService Name="com.microsoft.randomnumbergenerator"/>
    </uap:Extension>
</Extensions>
...
```

Ändern Sie den Namespace des Elements **AppService** in **uap3** und fügen Sie das Attribut **SupportsRemoteSystems** hinzu:

``` xml
...
<uap3:AppService Name="com.microsoft.randomnumbergenerator" SupportsRemoteSystems="true"/>
...
```

Um Elemente in diesem neuen Namespace zu verwenden, müssen Sie die Namespacedefinition am Anfang der Manifest-Datei hinzufügen.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3">
  ...
</Package>
```

Erstellen Sie Ihr App-Dienstanbieterprojekt, und stellen Sie es auf dem bzw. den Zielgeräten bereit.

## Festlegen des App-Dienstes als Ziel auf dem Ausgangsgerät
Das Gerät *, über das* der App-Remotedienst aufgerufen werden soll, muss über eine App mit Remotesystemfunktionen verfügen. Diese können der App hinzugefügt werden, die den App-Dienst auf dem Zielgerät bereitstellt (in diesem Fall installieren Sie diese App auf beiden Geräten), oder in eine völlig andere App eingefügt werden.

Die folgenden **using**-Anweisungen werden für den Code in diesem Abschnitt benötigt, damit er in der vorliegenden Form ausgeführt wird:

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetUsings)]


Instanziieren Sie zunächst ein [**AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.AppService.AppServiceConnection)-Objekt, als ob Sie einen App-Dienst lokal aufrufen würden. Dieser Vorgang wird in [Erstellen und Verwenden eines App-Diensts](how-to-create-and-consume-an-app-service.md) ausführlicher behandelt. In diesem Beispiel ist der als Ziel festzulegende App-Dienst der Zufallszahlen-Generator-Dienst.

> [!NOTE]
> Es wird davon ausgegangen, dass ein [RemoteSystem](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem)-Objekt bereits auf irgendeine Weise innerhalb des Codes zum Aufrufen der folgenden Methode geladen wurde. Anleitungen dazu, wie Sie dies einrichten können, finden Sie unter [Starten einer Remote-App](launch-a-remote-app.md).

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetAppService)]

Als Nächstes wird ein [**RemoteSystemConnectionRequest**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemConnectionRequest)-Objekt für das vorgesehene Remotegerät erstellt. Es wird dann zum Öffnen der **AppServiceConnection** zu diesem Gerät verwendet. Beachten Sie, dass in dem Beispiel unten Fehlerbehandlung und Berichte aus Platzgründen erheblich vereinfacht werden.

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetRemoteConnection)]

An diesem Punkt sollten Sie über eine offene Verbindung zu einem App-Dienst auf einem Remotecomputer verfügen.

## Exchange-Dienst-spezifische Nachrichten über die Remoteverbindung

Hier können Sie Nachrichten in Form von [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset)-Objekten an den Dienst senden und von dem Dienst empfangen (weitere Informationen finden Sie unter [Erstellen und Verwenden eines App-Diensts](how-to-create-and-consume-an-app-service.md)). Der Zufallszahlen-Generator-Dienst verwendet zwei Ganzzahlen mit den Schlüsseln `"minvalue"` und `"maxvalue"` als Eingaben, wählt nach dem Zufallsprinzip eine Ganzzahl in dem Bereich aus und gibt sie an den aufrufenden Prozess mit dem Schlüssel `"Result"` zurück.

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetSendMessage)]

Sie haben jetzt eine Verbindung zu einem App-Dienst auf einem als Ziel festgelegten Remotegerät hergestellt, einen Vorgang auf diesem Gerät ausgeführt und Daten auf Ihrem Ausgangsgerät als Antwort empfangen.

## Verwandte Themen

[Übersicht über verbundene Apps und Geräte (Projekt "Rome")](connected-apps-and-devices.md)  
[Starten einer Remote-App](launch-a-remote-app.md)  
[Erstellen und Verwenden eines App-Diensts](how-to-create-and-consume-an-app-service.md)  
[API-Referenz für Remotesysteme](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)  
[Beispiel für Remotesysteme](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) veranschaulicht die Vorgehensweise zum Erkennen eines Remotesystems, Starten einer App auf einem Remotesystem und Verwenden von App-Diensten zum Senden von Nachrichten zwischen Apps, die auf zwei Systemen ausgeführt werden.



<!--HONumber=Aug16_HO3-->


