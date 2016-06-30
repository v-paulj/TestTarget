---
author: mcleblanc
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: Testen von Surface Hub-Apps mit Visual Studio
description: "Der Visual Studio-Simulator bietet eine Umgebung für das Entwerfen, Entwickeln, Debuggen und Testen von UWP-Apps, einschließlich Apps für Surface Hub."
translationtype: Human Translation
ms.sourcegitcommit: 0bf96b70a915d659c754816f4c115f3b3f0a5660
ms.openlocfilehash: 655dffb5f1948724c894de291e119be1322f6e77

---

# Testen von Surface Hub-Apps mit Visual Studio
Der Visual Studio-Simulator bietet eine Umgebung, in der Sie Apps für die universale Windows-Plattform (UWP) entwerfen, entwickeln, debuggen und testen können, einschließlich Apps, die Sie für Microsoft Surface Hub entwickelt haben. Der Simulator verwendet nicht dieselbe Benutzeroberfläche wie ein Surface Hub, ist jedoch hilfreich, um das Erscheinungsbild und Verhalten Ihrer App bei der Bildschirmgröße und -auflösung von Surface Hubs zu testen.

Weitere Informationen finden Sie unter [Ausführen von Windows Store-Apps im Simulator](https://msdn.microsoft.com/library/hh441475.aspx).

## Hinzufügen von Surface Hub-Auflösungen zum Simulator
So fügen Sie Surface Hub-Auflösungen zum Simulator hinzu:

1. Erstellen Sie eine Konfiguration für denSurface Hub mit 55 Zoll, indem Sie den folgenden XML-Code in einer Datei namens **HardwareConfigurations-SurfaceHub55.xml** speichern.  

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfHardwareConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <HardwareConfiguration>
            <Name>SurfaceHub55</Name>
            <DisplayName>Surface Hub 55"</DisplayName>
            <Resolution>
                <Height>1080</Height>
                <Width>1920</Width>
            </Resolution>
            <DeviceSize>55</DeviceSize>
            <DeviceScaleFactor>100</DeviceScaleFactor>
        </HardwareConfiguration>
    </ArrayOfHardwareConfiguration>
    ```

2. Erstellen Sie eine Konfiguration für denSurface Hub mit 84 Zoll, indem Sie den folgenden XML-Code in einer Datei namens **HardwareConfigurations-SurfaceHub84.xml** speichern.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfHardwareConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <HardwareConfiguration>
            <Name>SurfaceHub84</Name>
            <DisplayName>Surface Hub 84"</DisplayName>
            <Resolution>
                <Height>2160</Height>
                <Width>3840</Width>
            </Resolution>
            <DeviceSize>84</DeviceSize>
            <DeviceScaleFactor>150</DeviceScaleFactor>
        </HardwareConfiguration>
    </ArrayOfHardwareConfiguration>
    ```

3. Kopieren Sie die zwei XML-Dateien in **C:\Programme (x86)\Gemeinsame Dateien\Microsoft Shared\Windows Simulator\\&lt;Versionsnummer&gt;\HardwareConfigurations**.

   > **Hinweis**
            &nbsp;&nbsp;Zum Speichern der Dateien in diesem Ordner werden Administratorrechte benötigt.

4. Führen Sie Ihre App im Visual Studio-Simulator aus. Klicken Sie in der Palette auf die Schaltfläche **Change Resolution**, und wählen Sie in der Liste eine Surface Hub-Konfiguration aus.

    ![Auflösungen des Visual Studio-Simulators](images/vs-simulator-resolutions.png)

   > **Tipp**
            &nbsp;&nbsp;
            [Schalten Sie den Tablet-Modus ein](http://windows.microsoft.com/windows-10/getstarted-like-a-tablet), um das Verhalten auf einem Surface Hub besser zu simulieren.

## Bereitstellen von Apps aus Visual Studio auf einem Surface Hub
Das manuelle Bereitstellen einer App ist ein einfacher Vorgang.

### Aktivieren des Entwicklermodus
Standardmäßig installiert Surface Hub nur Apps aus dem Windows Store. Um Apps, die von einer anderen Quelle signiert wurden, zu installieren, müssen Sie den Entwicklermodus aktivieren.

> **Hinweis**
            &nbsp;&nbsp;Nachdem der Entwicklermodus aktiviert wurde, müssen Sie den Surface Hub zurücksetzen, um den Entwicklermodus wieder zu deaktivieren. Durch das Zurücksetzen des Geräts werden alle lokalen Benutzerdateien und die Konfiguration gelöscht, und anschließend wird Windows neu installiert.

1. Öffnen Sie im **Startmenü** des Surface Hub die Einstellungs-App.

   >  **Hinweis**
            &nbsp;&nbsp;Für den Zugriff auf die Einstellungs-App werden Administratorrechte benötigt.

2. Navigieren Sie zu **Update und Sicherheit > Für Entwickler**.

3. Wählen Sie **Entwicklermodus** aus, und akzeptieren Sie die Warnung.

### Bereitstellen Ihrer App aus Visual Studio
Weitere Informationen finden Sie unter [Bereitstellen und Debuggen von Apps für die Universelle Windows-Plattform (UWP)](https://msdn.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps).

   > **Hinweis**
            &nbsp;&nbsp;Dieses Feature erfordert mindestens **Visual Studio 2015 Update 1**.

1. Zur Auswahl eines Ziels navigieren Sie zur Dropdownliste mit Debugzielen neben der Schaltfläche **Debugging starten** und wählen **Remotecomputer** aus.

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Dropdownliste der Debugziele in Visual Studio](images/vs-debug-target.png)

2. Geben Sie die IP-Adresse des Surface Hub ein. Stellen Sie sicher, dass der Authentifizierungsmodus **Universell** ausgewählt ist.

   > **Tipp**
            &nbsp;&nbsp;Nachdem Sie den Entwicklermodus aktiviert haben, sehen Sie die IP-Adresse des Surface Hub auf dem Willkommensbildschirm.

3. Wählen Sie entweder **Debugging starten (F5)** aus, um die Bereitstellung und das Debugging Ihrer App auf dem Surface Hub auszuführen, oder drücken Sie STRG+F5, um nur die Bereitstellung Ihrer App auszuführen.

   > **Tipp**
            &nbsp;&nbsp;Sollte auf dem Surface Hub der Willkommensbildschirm angezeigt werden, können Sie diesen durch Drücken einer beliebigen Schaltfläche schließen.



<!--HONumber=Jun16_HO4-->


