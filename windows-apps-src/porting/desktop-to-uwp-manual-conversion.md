---
Description: Zeigt, wie Sie eine Windows-Desktopanwendung (z. B. Win32, WPF und Windows Forms) manuell in eine UWP-App (Universelle Windows-Plattform) konvertieren.
Search.Product: eADQiWindows 10XVcnh
title: Manuelles Konvertieren einer Windows-Desktopanwendung in eine UWP-App (Universelle Windows-Plattform)
ms.sourcegitcommit: 606d5237cb67cb4439704f81b180c3c48cc1556f
ms.openlocfilehash: be7599403e78c8db7ba91fe5a7c25b1c1a1ab644

---

# Manuelles Konvertieren Ihrer Windows-Desktopanwendung in eine UWP-App (Universelle Windows-Plattform)

\[Einige Informationen beziehen sich auf die Vorabversion, die vor der kommerziellen Freigabe möglicherweise wesentlichen Änderungen unterliegt. Microsoft übernimmt für die hier bereitgestellten Informationen keine Garantie, weder ausdrücklicher noch impliziter Art.\]

Die Verwendung des Konverters ist praktisch und automatisch, und es ist hilfreich, wenn Unsicherheiten darüber bestehen, was das Installationsprogramm tut. Wenn Ihre App aber mithilfe von Xcopy installiert wurde, oder wenn Sie mit den Änderungen vertraut sind, die das Installationsprogramm Ihrer App am System vornimmt, können Sie sich entscheiden, ein App-Paket und -Manifest manuell zu erstellen.

Im Folgenden sind die Schritte zum manuellen Erstellen eines Centennial-Pakets beschrieben:

## Erstellen Sie manuell ein Manifest.

Ihre _appxmanifest.xml_-Datei muss (mindestens) den folgenden Inhalt aufweisen. Ändern Sie Platzhalter, die \*\*\*SO\*\*\* formatiert sind, in eigentliche Werte für Ihre Anwendung.

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Package
       xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
       xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
       xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities">
      <Identity Name="***YOUR_PACKAGE_NAME_HERE***"
        ProcessorArchitecture="x64"
        Publisher="CN=***COMPANY_NAME***, O=***ORGANIZATION_NAME***, L=***CITY***, S=***STATE***, C=***COUNTRY***"
        Version="***YOUR_PACKAGE_VERSION_HERE***" />
      <Properties>
        <DisplayName>***YOUR_PACKAGE_DISPLAY_NAME_HERE***</DisplayName>
        <PublisherDisplayName>Reserved</PublisherDisplayName>
        <Description>No description entered</Description>
        <Logo>***YOUR_PACKAGE_RELATIVE_DISPLAY_LOGO_PATH_HERE***</Logo>
      </Properties>
      <Resources>
        <Resource Language="en-us" />
      </Resources>
      <Dependencies>
        <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.14316.0" />
      </Dependencies>
      <Capabilities>
        <rescap:Capability Name="runFullTrust"/>
      </Capabilities>
      <Applications>
        <Application Id="***YOUR_PRAID_HERE***" Executable="***YOUR_PACKAGE_RELATIVE_EXE_PATH_HERE***" EntryPoint="Windows.FullTrustApplication">
          <uap:VisualElements
           BackgroundColor="#464646"
           DisplayName="***YOUR_APP_DISPLAY_NAME_HERE***"
           Square150x150Logo="***YOUR_PACKAGE_RELATIVE_PNG_PATH_HERE***"
           Square44x44Logo="***YOUR_PACKAGE_RELATIVE_PNG_PATH_HERE***"
           Description="***YOUR_APP_DESCRIPTION_HERE***" />
        </Application>
      </Applications>
    </Package>
    ```

## Ausführen des MakeAppX-Tools

Verwenden Sie den [App-Packager (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx), um ein AppX-Paket für Ihr Projekt zu generieren. MakeAppx.exe ist in der Windows 10 SDK enthalten. 

Um MakeAppx auszuführen, stellen Sie zunächst sicher, dass Sie, wie oben beschrieben, eine Manifestdatei erstellt haben. 

Erstellen Sie als Nächstes eine Zuordnungsdatei. Die Datei sollte mit **[Dateien]** beginnen, dann jede der Quelldateien auf dem Datenträger auflisten, gefolgt von deren Zielpfad im Paket. Beispiel: 

```
[Files]
"C:\MyApp\StartPage.htm"     "default.html"
"C:\MyApp\readme.txt"        "doc\readme.txt"
"\\MyServer\path\icon.png"   "icon.png"
"MyCustomManifest.xml"       "AppxManifest.xml"
```

Führen Sie abschließend den folgenden Befehl aus: 

```cmd
MakeAppx.exe pack /f mapping_filepath /p filepath.appx
```

## Signieren des Appx-Pakets

Das Add-AppxPackage-Cmdlet erfordert, dass das bereitgestellte Anwendungspaket (.appx) signiert ist. Verwenden Sie zum Signieren des Appx-Pakets [SignTool.exe](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx), die im Lieferumfang des Microsoft Windows 10 SDK enthalten ist.

Beispielanwendung: 

```cmd
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
```

Wenn Sie beim Ausführen von MakeCert.exe zum Eingeben eines Kennworts aufgefordert werden, wählen Sie **Kein**. Weitere Informationen zu Zertifikaten und zum Signieren finden Sie unter: 

- [Gewusst wie: Erstellen temporärer Zertifikate für die Verwendung während der Entwicklung](https://msdn.microsoft.com/library/ms733813.aspx)

- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)

- [SignTool.exe (Signaturtool)](https://msdn.microsoft.com/library/8s9b9yaz.aspx)




<!--HONumber=Jun16_HO4-->


