---
author: awkoren
Description: Bereitstellen und Debuggen einer unter Verwendung der Desktop-Konvertierungserweiterungen von einer Windows-Desktopanwendung (Win32, WPF und Windows Forms) konvertierten UWP-App (Universelle Windows-Plattform)
Search.Product: eADQiWindows 10XVcnh
title: Bereitstellen und Debuggen einer von einer Windows-Desktopanwendung konvertierten UWP-App (Universelle Windows-Plattform)
translationtype: Human Translation
ms.sourcegitcommit: 2c1a8ea38081c947f90ea835447a617c388aec08
ms.openlocfilehash: 75e176f17845bdbd618c6ca63fbbb5765bef54fb

---

# Bereitstellen und Debuggen der konvertierten UWP-App

\[Einige Informationen beziehen sich auf die Vorabversion, die vor der kommerziellen Freigabe möglicherweise wesentlichen Änderungen unterliegt. Microsoft übernimmt keine Garantie, weder ausdrücklicher noch impliziter Art, für die hier bereitgestellten Informationen.\]

Dieses Thema enthält Informationen, die Sie beim erfolgreichen Bereitstellen und Debuggen der App nach der Konvertierung unterstützen. In diesem Thema finden Sie auch Informationen zu einigen internen Aspekten der Desktop-Konvertierungserweiterungen.

## Debuggen der konvertierten UWP-App

Sie haben mehrere Optionen zum Debuggen der konvertierten App.

### Anhängen an den Prozess

Wenn Sie Microsoft Visual Studio als Administrator ausführen, funktionieren die Befehle __Debuggen starten__ und __Starten ohne Debugging__ für das konvertierte App-Projekt, doch die gestartete App wird auf einer [Ebene mittlerer Integrität](https://msdn.microsoft.com/library/bb625963) ausgeführt. Das heißt, sie verfügt _nicht_ über erhöhte Rechte. Wenn Sie der gestarteten App Administratorrechte gewähren möchten, müssen Sie sie zunächst über eine Verknüpfung oder eine Kachel „als Administrator“ starten. Wenn die App ausgeführt wird, rufen Sie von einer Microsoft Visual Studio-Instanz, die als Administrator ausgeführt wird, __An Prozess anhängen__ auf, und wählen Sie im Dialogfeld den Prozess der App.

### Debugging mit F5

Visual Studio unterstützt jetzt ein neues Paketprojekt, mit dem Sie die beim Erstellen der Anwendung vorgenommenen Updates automatisch in das beim Ausführen des Konverters im Anwendungs-Installer erstellte AppX-Paket kopieren können. Nach der Konfiguration des Paketprojekts können Sie F5 verwenden, um das Debuggen direkt im AppX-Paket durchzuführen. 

Erste Schritte 

1. Stellen Sie zunächst sicher, dass Desktop App Converter verwendet werden kann. Eine Anleitung hierzu finden Sie unter [Vorschau für Desktop App Converter](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter). 

2. Führen Sie den Konverter und dann den Installer für die Win32-Anwendung aus. Der Konverter erfasst das Layout und alle an der Registrierung vorgenommenen Änderungen und gibt ein Appx-Paket mit einem Manifest und die Datei „Registry.dat“ aus, um die Registrierung zu virtualisieren:

![alt](images/desktop-to-uwp/debug-1.png)

3. Installieren und starten Sie [Visual Studio "15" Preview 2](https://www.visualstudio.com/downloads/visual-studio-next-downloads-vs.aspx). 

4. Installieren Sie das VSIX-Projekt „Desktop to UWP Packaging“ aus der [Visual Studio Gallery](http://go.microsoft.com/fwlink/?LinkId=797871). 

5. Öffnen Sie die entsprechende Win32-Lösung, die in Visual Studio konvertiert wurde.
 
6. Fügen Sie der Projektmappe das neue Paketprojekt hinzu, indem Sie mit der rechten Maustaste auf die Projektmappe klicken und „Neues Projekt hinzufügen“ wählen. Wählen Sie dann unter „Setup und Bereitstellung“ das Paketprojekt „Desktop to UWP Packaging Project“:

    ![alt](images/desktop-to-uwp/debug-2.png)

    Das resultierende Projekt wird der Projektmappe hinzugefügt:

    ![alt](images/desktop-to-uwp/debug-3.png)

    Im Paketprojekt stellt die AppXFileList eine Zuordnung von Dateien im AppX-Layout bereit. Verweise sind zunächst leer, sollten jedoch manuell auf die .exe-Projektdatei für die Buildreihenfolge festgelegt werden. 

7. Das DesktopToUWPPackaging-Projekt verfügt über eine Eigenschaftenseite, auf der Sie den AppX-Paketstamm konfigurieren und die auszuführende Kachel festlegen können:

    ![alt](images/desktop-to-uwp/debug-4.png)

    Legen Sie PackageLayout auf das Stammverzeichnis des AppX-Projekts fest, das vom Konverter (siehe oben) erstellt wurde. Wählen Sie die Kachel, die Sie ausführen möchten.

8.  Öffnen und bearbeiten Sie die Datei „AppXFileList.xml“. Diese Datei definiert, wie die Ausgabe des Win32-Debugbuilds in das vom Konverter erstellte AppX-Layout kopiert wird. Standardmäßig enthält die Datei einen Platzhalter mit einem Beispieltag und einem Kommentar:

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
      <ItemGroup>
    <!— Use the following syntax to copy debug output to the AppX layout
       <AppxPackagedFile Include="$(outdir)\App.exe">
          <PackagePath>App.exe</PackagePath>
        </AppxPackagedFile> 
        See http://etc...
    -->
      </ItemGroup>
    </Project>
    ```

    Im Folgenden finden Sie ein Beispiel für das Erstellen der Zuordnung. In diesem Fall werden die EXE- und DLL-Dateien vom Win32-Buildspeicherort in den Speicherort des Paketlayouts kopiert. 

    ```XML
    <?xml version="1.0" encoding=utf-8"?>
    <Project ToolsVersion=14.0" xmlns="http://scehmas.microsoft.com/developer/msbuild/2003">
        <PropertyGroup>
            <MyProjectOutputPath>C:\{path}</MyProjectOutputPath>
        </PropertyGroup>
        <ItemGroup>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.exe">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.exe</PackagePath>
            </LayoutFile>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.Models.dll">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.Models.dll</PackagePath>
            </LayoutFile>
        </ItemGroup>
    </Project>
    ```

    Die Datei wird wie folgt definiert: 

    Zunächst wird *MyProjectOutputPath* so definiert, dass es auf den Speicherort zeigt, wo das Win32-Projekt erstellt wird:

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="14.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        <PropertyGroup>
            <MyProjectOutputPath>C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\DesktopUWP</MyProjectOutputPath>
        </PropertyGroup>
    ```

    Anschließend gibt jede *LayoutFile* eine Datei an, die aus dem Win32-Buildspeicherort in das Appx-Paketlayout kopiert wird. In diesem Fall werden erst eine EXE-Datei und dann eine DLL-Datei kopiert. 

    ```XML
        <ItemGroup>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.exe">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.exe</PackagePath>
            </LayoutFile>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.Models.dll">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.Models.dll</PackagePath>
            </LayoutFile>
        </ItemGroup>
    </Project>
    ```

9. Legen Sie das Paketprojekt als Startprojekt fest. Hierdurch werden die Win32-Dateien in das AppX-Paket kopiert und der Debugger gestartet, wenn das Projekt erstellt und ausgeführt wird.  

    ![alt](images/desktop-to-uwp/debug-5.png)

10. Schließlich können Sie einen Haltepunkt im Win32-Code festlegen und F5 drücken, um den Debugger zu starten. Alle an der Win32-Anwendung vorgenommenen Änderungen im AppX-Paket werden kopiert. Sie können das Debugging direkt in Visual Studio durchführen.

11. Wenn Sie Ihre Anwendung aktualisieren, müssen Sie MakeAppX verwenden, um erneut ein App-Paket zu erstellen. Weitere Informationen finden Sie unter [App-Objekt-Manager (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx). 

Wenn Sie über mehrere Buildkonfigurationen verfügen (z. B. für Version und fürs Debuggen), können Sie der Datei „AppXFileList.xml“ Folgendes hinzufügen, um den Win32-Build aus verschiedenen Speicherorten zu kopieren:

```XML
<PropertyGroup>
    <MyProjectOutputPath Condition="$(Configuration) == 'DesktopUWP'">C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\DesktopUWP>
    </MyProjectOutputPath>
    <MyProjectOutputPath Condition="$(Configuration) == 'ReleaseDesktopUWP'"> C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\ReleaseDesktopUWP</MyProjectOutputPath>
</PropertyGroup>
```

Sie können auch die bedingte Kompilierung zum Aktivieren bestimmter Codepfade verwenden, wenn Sie Ihre Anwendung für UWP aktualisieren, diese jedoch weiterhin für Win32 erstellen möchten. 

1.  Im folgenden Beispiel werden nur der Code für DesktopUWP kompiliert und eine Kachel mit der WinRT-API dargestellt. 

    ```C#
    [Conditional("DesktopUWP")]
    private void showtile()
    {
        XmlDocument tileXml = TileUpdateManager.GetTemplateContent(TileTemplateType.TileSquare150x150Text01);
        XmlNodeList textNodes = tileXml.GetElementsByTagName("text");
        textNodes[0].InnerText = string.Format("Welcome to DesktopUWP!");
        TileNotification tileNotification = new TileNotification(tileXml);
        TileUpdateManager.CreateTileUpdaterForApplication().Update(tileNotification);
    }
    ```

2.  Sie können den Konfigurations-Manager zum Hinzufügen neuer Buildkonfiguration verwenden:

    ![alt](images/desktop-to-uwp/debug-6.png)

    ![alt](images/desktop-to-uwp/debug-7.png)

3.  Fügen Sie dann in den Projekteigenschaften Unterstützung für Symbole für die bedingte Kompilierung hinzu:

    ![alt](images/desktop-to-uwp/debug-8.png)

4.  Sie können jetzt das Buildziel zu DesktopUWP ändern, wenn Sie für die hinzugefügte UWP-API erstellen möchten.

### PLMDebug 

In Visual Studio können Sie mit der Taste F5 sowie mithilfe der Option „An den Prozess anhängen“ Ihre App debuggen, während sie ausgeführt wird. In einigen Fällen empfiehlt sich jedoch eine differenziertere Steuerung des Debugging-Vorgangs, wenn z.B. das Debuggen erfolgen soll, bevor die App gestartet wird. Verwenden Sie in diesen komplexeren Szenarien [**PLMDebug**](https://msdn.microsoft.com/library/windows/hardware/jj680085%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396). Mit diesem Tool können Sie die konvertierte App mit dem Windows-Debugger debuggen. Zudem verfügen Sie über die vollständige Steuerung des App-Lebenszyklus, einschließlich Anhalten, Fortsetzen und Beenden. 

PLMDebug ist im Windows SDK enthalten. Weitere Informationen finden Sie unter [**PLMDebug**](https://msdn.microsoft.com/library/windows/hardware/jj680085%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396). 

### Ausführen eines anderen Prozesses im vollständig vertrauenswürdigen Container 

Sie können benutzerdefinierte Prozesse im Container eines angegebenen App-Pakets aufrufen. Dies kann nützlich sein, um Szenarien zu testen (z.B., wenn Sie über eine benutzerdefinierte Testumgebung verfügen und die Ausgabe der App testen möchten). Verwenden Sie hierzu das PowerShell-Cmdlet ```Invoke-CommandInDesktopPackage```: 

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## Bereitstellen der konvertierten UWP-App

Es gibt zwei Möglichkeiten zum Bereitstellen der konvertierten App: Registrieren loser Dateien sowie Bereitstellen des APPX-Pakets. 

Das Registrieren loser Dateien eignet sich für das Debuggen, wenn sich die Dateien auf dem Datenträger an einem Ort befinden, der mühelos aktualisiert werden kann, und auf den Sie einfach sowie ohne Signatur und Zertifikat zugreifen können.  

Das Bereitstellen des APPX-Pakets ist eine einfache Möglichkeit, die Anwendung auf mehreren Computern bereitzustellen und querzuladen. Das Paket muss jedoch signiert werden und das Zertifikat auf dem Computer als vertrauenswürdig gelten.

### Registrieren loser Dateien

Führen Sie zum Bereitstellen der App während der Entwicklung das folgende PowerShell-Cmdlet aus: 

```Add-AppxPackage –Register AppxManifest.xml```

Ersetzen Sie zum Aktualisieren der EXE- oder DLL-Dateien Ihrer App die vorhandenen Dateien in Ihrem Paket einfach durch die neuen, vergrößern Sie die Versionsnummer in der Datei „AppxManifest.xml“, und führen Sie den oben genannten Befehl erneut aus.

Hinweis: 

* Das Laufwerk, auf dem Sie die konvertierte App installieren, muss das NTFS-Format aufweisen.

* Eine konvertierte App wird immer als interaktiver Benutzer ausgeführt.

### Bereitstellen des APPX-Pakets 

Bevor Sie Ihre App bereitstellen, müssen Sie sie mit einem Zertifikat signieren. Informationen zum Erstellen eines Zertifikats finden Sie unter [Signieren des Appx-Pakets](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter#deploy-your-converted-appx). 

Im Folgenden wird beschrieben, wie Sie ein Zertifikat importieren, das Sie zuvor erstellt haben. Sie können das Zertifikat mit CERTUTIL direkt importieren oder wie der Kunde von einem APPX-Projekt installieren, das Sie signiert haben. 

Um das Zertifikat über CERTUTIL zu installieren, führen Sie über eine Eingabeaufforderung mit Administratorrechten folgenden Befehl aus:

```cmd
Certutil -addStore TrustedPeople <testcert.cer>
```

So importieren Sie das Zertifikat wie ein Kunde aus dem APPX-Projekt:

1.  Klicken Sie im Datei-Explorer mit der rechten Maustaste auf ein APPX-Projekt, das Sie mit einem Testzertifikat signiert haben, und wählen Sie im Kontextmenü **Eigenschaften** aus.
2.  Klicken oder tippen Sie auf die Registerkarte **Digitale Signaturen**.
3.  Klicken oder tippen Sie auf das Zertifikat, und wählen Sie **Details**.
4.  Klicken oder tippen Sie auf **Zertifikat anzeigen**.
5.  Klicken oder tippen Sie auf **Zertifikat installieren**.
6.  Wählen Sie in der Gruppe **Speicherort** die Option **Lokaler Computer**.
7.  Klicken oder tippen Sie auf **Weiter** und **OK**, um das UAC-Dialogfeld zu bestätigen.
8.  Ändern Sie auf dem nächsten Bildschirm des Zertifikatimport-Assistenten die Optionsauswahl zu **Alle Zertifikate in folgendem Speicher speichern**.
9.  Klicken oder tippen Sie auf **Durchsuchen**. Scrollen Sie im Fenster „Zertifikatspeicher auswählen“ nach unten, wählen Sie die Option **Vertrauenswürdige Personen** aus, und tippen Sie dann auf **OK**.
10. Klicken oder tippen Sie auf **Weiter**. Ein neuer Bildschirm wird angezeigt. Klicken oder tippen Sie auf **Fertig stellen**.
11. Ein Bestätigungsdialogfeld sollte angezeigt werden. Wenn dies der Fall ist, klicken Sie auf **OK**. Gibt ein anderes Dialogfeld an, dass ein Problem mit dem Zertifikat vorliegt, müssen Sie mögliche Zertifikatfehler behandeln.

Hinweis: Damit Windows dem Zertifikat vertraut, muss sich das Zertifikat entweder im Knoten **Zertifikate (lokaler Computer) > Vertrauenswürdige Stammzertifizierungsstellen > Zertifikate** oder **Zertifikate (lokaler Computer) > Vertrauenswürdige Personen > Zertifikate** befinden. Nur Zertifikaten, die sich in diesen Speicherorten befinden, können auf dem lokalen Computer als vertrauenswürdige Zertifikate festgelegt werden. Andernfalls wird eine Fehlermeldung angezeigt, die der folgenden ähnelt:
```CMD
"Add-AppxPackage : Deployment failed with HRESULT: 0x800B0109, A certificate chain processed,
but terminated in a rootcertificate which is not trusted by the trust provider.
(Exception from HRESULT: 0x800B0109) error 0x800B0109: The root certificate of the signature
in the app package must be trusted."
```

Nachdem das Zertifikat nun als vertrauenswürdig eingestuft wird, gibt es zwei Möglichkeiten, das Paket zu installieren: über PowerShell oder einfach durch Doppelklicken auf die APPX-Paketdatei.  Führen Sie das folgende Cmdlet aus, um über PowerShell zu installieren:

```powershell
Add-AppxPackage <MyApp>.appx
```

## Im Hintergrund

Wenn Sie die konvertierte App ausführen, wird das UWP-App-Paket von \Programme\WindowsApps\\&lt;_Paketname_&gt;\\&lt;_App-Name_&gt;.exe gestartet. Wenn Sie dort nachschauen, stellen Sie fest, dass Ihre App über ein App-Paketmanifest („AppxManifest.xml“) verfügt, das auf einen speziellen XML-Namespace für konvertierte Apps verweist. In die Manifestdatei ist ein __&lt;EntryPoint&gt;__-Element enthalten, das auf eine vertrauenswürdige App verweist. Wenn die App gestartet wird, wird sie nicht im App-Container, sondern stattdessen wie gewohnt als Benutzer ausgeführt.

Die App wird jedoch in einer speziellen Umgebung ausgeführt, in der alle Zugriffe der App auf das Dateisystem und die Registrierung weitergeleitet werden. Die Datei mit dem Namen „Registry.dat“ wird für die Weiterleitung der Registrierung verwendet. Es ist eine Registrierungsstruktur, sodass Sie sie im Windows-Registrierungs-Editor (Regedit) anzeigen können. Beachten Sie, dass aufgrund dieses Mechanismus die Registrierung nicht für prozessübergreifende Kommunikation verwendet werden kann. Die Registrierung wurde nicht für diese Methode entwickelt und ist daher in jedem Fall nicht dafür geeignet. Wenn es um das Dateisystem geht, wird nur der AppData-Ordner an denselben Speicherort weitergeleitet, in dem App-Daten für alle UWP-Apps gespeichert werden. Dieser Speicherort wird lokaler App-Datenspeicher genannt. Sie können mithilfe der [ApplicationData.LocalFolder](https://msdn.microsoft.com/library/windows/apps/br241621)-Eigenschaft auf diesen zugreifen. Auf diese Weise wird der Code bereits zum Lesen und Schreiben von App-Daten an den richtigen Speicherort portiert, ohne dass sie etwas unternehmen müssen. Sie können dort auch direkt schreiben. Ein Vorteil der Dateisystemweiterleitung ist eine unkomplizierte Deinstallation.

In einem Ordner mit dem Namen „VFS“ sehen Sie Ordner, die die DLL-Dateien enthalten, von denen die App abhängig ist. Diese DLL-Dateien werden in den Systemordnern für die klassische Desktopversion Ihrer App installiert. Bei der UWP-App sind die DLL-Dateien für Ihre App lokal gespeichert. Auf diese Weise treten bei der Installation und Deinstallation von UWP-Apps keine Versionsprobleme auf.

### Gepackte VFS-Speicherorte

Der folgenden Tabelle können Sie entnehmen, wo Dateien, die zu Ihrem Paket gehören, für die App im System überlagert sind. Die App geht davon aus, dass sich diese Dateien in den aufgeführten Systemspeicherorten befinden, während sie sich tatsächlich in den umgeleiteten Speicherorten unter [Paketstammverzeichnis]\VFS\ befinden. Die FOLDERID-Speicherorte stammen von der [**KNOWNFOLDERID**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd378457.aspx)-Konstante.

Systemspeicherort | Umgeleiteter Speicherort (unter [Paketstammverzeichnis]\VFS\) | Gültig für Architekturen
 :---- | :---- | :---
FOLDERID_SystemX86 | SystemX86 | x86, amd64 
FOLDERID_System | SystemX64 | amd64 
FOLDERID_ProgramFilesX86 | ProgramFilesX86 | x86, amd6 
FOLDERID_ProgramFilesX64 | ProgramFilesX64 | amd64 
FOLDERID_ProgramFilesCommonX86 | ProgramFilesCommonX86 | x86, amd64
FOLDERID_ProgramFilesCommonX64 | ProgramFilesCommonX64 | amd64 
FOLDERID_Windows | Windows | x86, amd64 
FOLDERID_ProgramData | Gemeinsamer AppData-Ordner | x86, amd64 
FOLDERID_System\catroot | AppVSystem32Catroot | x86, amd64 
FOLDERID_System\catroot2 | AppVSystem32Catroot2 | x86, amd64 
FOLDERID_System\drivers\etc | AppVSystem32DriversEtc | x86, amd64 
FOLDERID_System\driverstore | AppVSystem32Driverstore | x86, amd64 
FOLDERID_System\logfiles | AppVSystem32Logfiles | x86, amd64 
FOLDERID_System\spool | AppVSystem32Spool | x86, amd64 

## Weitere Informationen
[Konvertieren der Desktopanwendung in eine UWP-App (Universelle Windows-Plattform)](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-root)

[Vorschau für Desktop App Converter](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter)

[Manuelles Konvertieren Ihrer Windows-Desktopanwendung in eine UWP-App (Universelle Windows-Plattform)](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-manual-conversion)

[Desktop-App-Bridge zu UWP-Codebeispielen auf GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)


<!--HONumber=Sep16_HO2-->


