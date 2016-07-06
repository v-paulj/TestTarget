---
author: msatranjr
title: Windows-Runtime-Komponenten
description: "In diesem Dokument wird ein Unternehmensfeature beschrieben, das von Windows 10 unterstützt wird und .NET-Apps mit Toucheingabemöglichkeit die Verwendung von vorhandenem Code ermöglicht, der für wichtige unternehmenskritische Vorgänge verantwortlich ist."
ms.sourcegitcommit: 4e9f3de68c44cf545ceee2efd99d9db8cab08676
ms.openlocfilehash: fa7e7404a9a3ab5d75d47b3a5271bd1fd84b5569

---

#Vermittelte Windows-Runtime-Komponenten für quergeladene Windows Store-Apps

In diesem Artikel wird ein Unternehmensfeature beschrieben, das von Windows 10 unterstützt wird und .NET-Apps mit Toucheingabemöglichkeit die Verwendung von vorhandenem Code ermöglicht, der für wichtige unternehmenskritische Vorgänge verantwortlich ist.

##Einführung

>**Hinweis**  Der Beispielcode in diesem Dokument [kann hier heruntergeladen werden](http://go.microsoft.com/fwlink/p/?LinkID=393655). Die Microsoft Visual Studio-Vorlage für das Erstellen vermittelter Windows-Runtime-Komponenten kann hier heruntergeladen werden: [Visual Studio 2015-Vorlage für universelle Windows-Apps für Windows 10](https://visualstudiogallery.msdn.microsoft.com/10be07b3-67ef-4e02-9243-01b78cd27935).

Windows enthält ein neues Feature namens *Vermittelte Windows-Runtime-Komponenten für quergeladene Anwendungen*. Wir verwenden den Begriff „prozessübergreifende Kommunikation“ (Inter-Process Communication, IPC), um die Fähigkeit zu beschreiben, vorhandene Desktopsoftwareressourcen in einem einzigen Prozess (Desktopkomponente) auszuführen und gleichzeitig mit diesem Code in einer UWP-App zu interagieren. Dieses Modell ist Unternehmensentwicklern vertraut, da Datenbankanwendungen und Anwendungen, die NT-Dienste unter Windows verwenden, eine ähnliche, aus mehreren Prozessen bestehende Architektur verwenden.

Das Querladen der App ist eine kritische Komponente dieses Features.
Unternehmensspezifische Anwendungen gehören nicht in den allgemeinen Windows Store für Verbraucher, und Unternehmen besitzen sehr spezielle Anforderungen hinsichtlich Sicherheit, Datenschutz, Verteilung, Einrichtung und Wartung. Das Querlademodell ist daher als solches eine Anforderung derer, die dieses Feature verwenden, und ein wichtiges Implementierungsdetail.

Auf Daten ausgerichtete Anwendungen sind ein Hauptziel für diese Anwendungsarchitektur. Es ist vorgesehen, dass vorhandene Unternehmensregeln, die beispielsweise in SQL Server integriert sind, ein häufig verwendeter Teil der Desktopkomponente sind. Dies ist sicher nicht die einzige Art von Funktionalität, die die Desktopkomponente bietet, aber ein großer Teil der Notwendigkeit dieses Features beruht auf vorhandenen Daten und Unternehmenslogiken.

Da in der Unternehmensentwicklung die .NET-Laufzeit und die Sprache C\# deutlich vorherrschen, wurde dieses Feature unter Betonung auf der Verwendung von .NET für die Windows Store-App und die Desktopkomponente entwickelt. Es können zwar auch andere Sprachen und Laufzeiten für die Windows Store-App verwendet werden, aber das begleitende Beispiel zeigt lediglich C\# und ist ausschließlich auf die .NET-Laufzeit beschränkt.

##Anwendungskomponenten

>**Hinweis**  Dieses Feature ist ausschließlich für die Verwendung von.NET bestimmt. Sowohl die Client-App als auch die Desktopkomponente müssen mit .NET erstellt worden sein.

**Anwendungsmodell**

Dieses Feature basiert auf der allgemeinen Anwendungsarchitektur, die als MVVM (Model View View-Model) bekannt ist. Daher wird angenommen, dass sich das „Modell“ vollständig in der Desktopkomponente befindet. Daher sollte sofort offensichtlich sein, dass die Desktopkomponente „kopflos“ ist (d. h., sie enthält keine Benutzeroberfläche). Die Ansicht ist komplett in der quergeladenen Unternehmensanwendung enthalten. Auch wenn es keine Anforderung gibt, dass diese Anwendung auf dem „Ansichtsmodell“-Konstrukt (View-Model-Konstrukt) basiert, gehen wir davon aus, dass dieses Muster allgemein verwendet wird.

**Desktopkomponente**

Bei der Desktopkomponente in diesem Feature handelt es sich um einen neuen Anwendungstyp, der als Teil dieses Features eingeführt wird. Diese Desktopkomponente kann nur in C\# geschrieben werden und muss auf .NET 4.6 oder höher für Windows 10 ausgerichtet sein. Der Projekttyp ist ein Hybrid für die Common Language Runtime (CLR) für UWP, da das Format für die prozessübergreifende Kommunikation UWP-Typen und -Klassen enthält, während die Desktopkomponente alle Teile der .NET-Laufzeit-Klassenbibliothek aufrufen darf. Die Auswirkungen auf das Visual Studio-Projekt werden später ausführlich beschrieben. Diese Hybridkonfiguration ermöglicht den Aufruf von UWP-Typen für die auf den Desktopkomponenten basierenden Anwendung, während gleichzeitig der Desktop-CLR-Code innerhalb der Desktopkomponentenimplementierung aufgerufen werden kann.

**Vertrag**

Der Vertrag zwischen der quergeladenen Anwendung und der Desktopkomponente wird mithilfe des UWP-Typsystems beschrieben. Dies umfasst die Deklarierung mindestens einer C\#-Klasse, die eine UWP darstellen kann. Weitere Informationen zu bestimmten Anforderungen im Zusammenhang mit dem Erstellen von Windows-Runtime-Klassen mit C\# finden Sie im MSDN-Thema [Erstellen von Komponenten für Windows-Runtime in C# und Visual Basic](https://msdn.microsoft.com/en-us/library/br230301.aspx).

>**Hinweis**  Enumerationen werden im Vertrag für Windows-Runtime-Komponenten zwischen Desktopkomponente und quergeladener Anwendung zu diesem Zeitpunkt nicht unterstützt.

**Quergeladene Anwendung**

Bei der quergeladenen Anwendung handelt es sich in jeder Hinsicht um eine normale UWP-App, mit einer Ausnahme: Sie wird quergeladen und nicht über den Windows Store installiert. Viele der Installationsmechanismen sind identisch: Das Manifest und das Anwendungspaket ähneln sich (ein Zusatz zum Manifest wird später ausführlich erläutert). Nach der Aktivierung des Querladens kann ein einfaches PowerShell-Skript die erforderlichen Zertifikate und die Anwendung selbst installieren. Normalerweise besteht die bewährte Methode darin, dass die quergeladene Anwendung den WACK-Zertifizierungstest durchläuft, der in Visual Studio im Menü „Projekt/Store“ enthalten ist.

>**Hinweis** Das Querladen kann in „Einstellungen &gt; Update & Sicherheit &gt;
 Für Entwickler“ aktiviert werden.

Es muss unbedingt angemerkt werden, dass der im Lieferumfang von Windows 10 enthaltene App-Broker nur als 32-Bit-Version vorliegt. Die Desktopkomponente muss eine 32-Bit-Version sein.
Quergeladene Anwendungen können als 64-Bit-Version vorliegen (vorausgesetzt, dass 64-Bit- und 32-Bit-Proxys registriert sind), aber dies wäre untypisch. Beim Erstellen von quergeladenen Anwendungen in C\# mithilfe der normalen „neutralen“ Konfiguration und dem bevorzugten 32-Bit-Standard werden natürlich quergeladene 32-Bit-Anwendungen erstellt.

**Serverinstanzerstellung und AppDomains**

Jede quergeladene Anwendung erhält eine eigene Instanz eines App-Broker-Servers (so genannte „Multiinstanzerstellung“). Der Servercode wird innerhalb einer einzelnen AppDomain ausgeführt. Dadurch können mehrere Bibliotheksversionen in separaten Instanzen ausgeführt werden. Beispielsweise benötigt Anwendung A die Version V1.1 einer Komponente, und Anwendung B benötigt Version V2. Diese werden sauber voneinander getrennt, indem V1.1- und V2-Komponenten in separaten Serververzeichnissen gespeichert werden und die Anwendung auf den Server verweist, der die gewünschte Version unterstützt.

Die Servercodeimplementierung kann auf mehrere App-Broker-Serverinstanzen verteilt werden, indem mehrere Anwendungen auf dasselbe Serververzeichnis verweisen. Es sind nach wie vor mehrere Instanzen des App-Broker-Servers vorhanden, auf ihnen wird jedoch identischer Code ausgeführt. Alle in einer einzelnen Anwendung verwendeten Implementierungskomponenten sollten sich unter demselben Pfad befinden.

##Definieren des Vertrags

Der erste Schritt beim Erstellen einer Anwendung mithilfe dieses Features besteht darin, einen Vertrag zwischen der quergeladenen Anwendung und der Desktopkomponente zu erstellen. Dies darf nur mit Windows-Runtime-Typen erfolgen.
Glücklicherweise können diese mithilfe von C\#-Klassen einfach deklariert werden. Es gibt wichtige jedoch wichtige Überlegungen hinsichtlich der Leistung, wenn diese Konversationen definiert werden, die in einem späteren Abschnitt behandelt werden.

Die Sequenz zum Definieren des Vertrags wird wie folgt eingeführt:

**Schritt 1:** Erstellen Sie in Visual Studio eine neue Klassenbibliothek. Stellen Sie sicher, dass Sie das Projekt mit der Klassenbibliotheksvorlage und nicht mit der Windows-Runtime-Komponentenvorlage erstellen.

In der Regel folgt an dieser Stelle eine Implementierung, in diesem Abschnitt wird jedoch lediglich die Definition des prozessübergreifenden Vertrags erläutert. Das Begleitbeispiel enthält die folgende Klasse (EnterpriseServer.cs), deren Anfangsform wie folgt aussieht:

```csharp
namespace Fabrikam
{
    public sealed class EnterpriseServer
    {

        public ILis<String> TestMethod(String input)
        {
            throw new NotImplementedException();
        }
        
        public IAsyncOperation<int> FindElementAsync(int input)
        {
            throw new NotImplementedException();
        }
        
        public string[] RetrieveData()
        {
            throw new NotImplementedException();
        }
        
        public event EventHandler&lt;string&gt; PeriodicEvent;
    }
}
```

Hierdurch wird die Klasse „EnterpriseServer“ definiert, die von der quergeladenen Anwendung instanziiert werden kann. Diese Klasse stellt die in der RuntimeClass zugesagte Funktionalität bereit. Die RuntimeClass kann verwendet werden, um die WINMD-Verweisdatei zu generieren, die Bestandteil der quergeladenen Anwendung ist.

**Schritt 2:** Bearbeiten Sie die Projektdatei manuell, um den Ausgabetyp des Projekts in Windows-Runtime-Komponente zu ändern.

Klicken Sie dazu in Visual Studio mit der rechten Maustaste auf das neu erstellte Projekt, und wählen Sie „Projekt entladen“ aus. Klicken Sie anschließend erneut mit der rechten Maustaste, und wählen Sie „EnterpriseServer.csproj bearbeiten“ aus, um die Projektdatei, eine XML-Datei, für die Bearbeitung zu öffnen.

Suchen Sie in der geöffneten Datei das Tag <OutputType>, und ändern Sie den Wert in „Winmdobj“.

**Schritt 4:** Erstellen Sie eine Buildregel, die eine Windows-Metadatendatei (WINMD-Datei) als „Verweis“ erstellt. D. h., es gibt keine Implementierung.

**Schritt 5:** Erstellen Sie eine Buildregel, die eine Windows-Metadatendatei für die „Implementierung“ erstellt, d. h., sie enthält dieselben Metadateninformationen und dazu die Implementierung.

Dies erfolgt durch die folgenden Skripts. Fügen Sie die Skripts der Befehlszeile nach dem Build-Ereignis im Projekt **Eigenschaften** > **Buildereignisse** hinzu.

> **Hinweis** Das Skript unterscheidet sich abhängig von der Version von Windows, auf die Sie zielen (Windows 10) und der verwendeten Version von Visual Studio.

```cmd
call "$(DevEnvDir)..\..\vc\vcvarsall.bat" x86 10.0.10240.0

md "$(TargetDir)"\impl
md "$(TargetDir)"\reference

erase "$(TargetDir)\impl\*.winmd"
erase "$(TargetDir)\impl\*.pdb"
erase "$(TargetDir)\reference\*.winmd"

xcopy /y "$(TargetPath)" "$(TargetDir)impl"
xcopy /y "$(TargetDir)*.pdb" "$(TargetDir)impl"

cd "$(TargetDir)impl"

winmdidl /nosystemdeclares /metadata_dir:"%WindowsSdkDir%UnionMetadata" "$(TargetName).winmd"

midl /metadata_dir "%WindowsSdkDir%UnionMetadata" /iid "$(SolutionDir)SampleProxy\$(TargetName)_i.c" /env win32 /h "$(SolutionDir)SampleProxy\$(TargetName).h" /winmd "$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(SolutionDir)SampleProxy\dlldata.c" /proxy "$(SolutionDir)SampleProxy\$(TargetName)_p.c"  "$(TargetName).idl"

mdmerge -n 1 -i "$(TargetDir)\impl" -o "$(TargetDir)reference" -metadata_dir "%WindowsSdkDir%UnionMetadata" -partial
```

Nachdem der Verweis **winmd** erstellt wurde (im Ordner „Verweis“ unterhalb des Zielordners für das Projekt), wird er manuell in jedes verarbeitende quergeladene Anwendungsprojekt kopiert und referenziert. Dies wird im folgenden Abschnitt näher erläutert. Die in den oben erwähnten Buildregeln verkörperte Projektstruktur stellt sicher, dass sich die Implementierung und die **winmd**-Verweisdatei in klar getrennten Verzeichnissen in der Buildhierarchie befinden, um Verwirrung zu vermeiden.

##Quergeladene Anwendungen im Detail
Wie bereits erwähnt, wird die quergeladene Anwendung genau wie jede andere UWP-App erstellt, aber es gibt ein zusätzliches Detail: das Deklarieren der Verfügbarkeit der RuntimeClass(es) im Manifest der quergeladenen Anwendung. Dies ermöglicht der Anwendung das einfache Neuschreiben, um auf die Funktionalität in der Desktopkomponente zuzugreifen. Ein Manifesteintrag im Abschnitt <Extension> beschreibt die in der Desktopkomponente implementierte RuntimeClass und enthält Informationen darüber, wo sie sich befindet. Diese Deklarationsinhalte im Manifest der Anwendung sind mit denen von Apps für Windows 10 identisch. Beispiel:

```XML
<Extension Category="windows.activatableClass.inProcessServer">
    <InProcessServer>
        <Path>clrhost.dll</Path>
        <ActivatableClass ActivatableClassId="Fabrikam.EnterpriseServer" ThreadingModel="both">
            <ActivatableClassAttribute Name="DesktopApplicationPath" Type="string" Value="c:\test" />
        </ActivatableClass>
    </InProcessServer>
</Extension>
```

Die Kategorie lautet „inProcessServer“, da die outOfProcessServer-Kategorie mehrere Einträge enthält, die für diese Anwendungskonfiguration nicht anwendbar sind. Beachten Sie, dass die <Path>-Komponente stets „clrhost.dll“ enthalten muss (dies wird jedoch **nicht** erzwungen, und das Angeben eines anderen Werts führt zu einem undefinierten Fehler).

Der <ActivatableClass>-Abschnitt entspricht einer echten prozessinternen RuntimeClass, die von einer Windows-Runtime-Komponente im App-Paket bevorzugt wird. <ActivatableClassAttribute> ist ein neues Element, und die Attribute Name="DesktopApplicationPath"" und Type="string" sind obligatorisch und unveränderlich. Das Value-Attribut verweist auf den Ort, an dem sich die winmd-Implementierungsdatei der Desktopkomponente befindet (weitere Einzelheiten hierzu finden Sie im folgenden Abschnitt). Jede von der Desktopkomponente bevorzugte RuntimeClass sollte eine eigene <ActivatableClass>-Elementstruktur besitzen. Die ActivatableClassId muss dem vollständig qualifizierten Namespacenamen der RuntimeClass entsprechen.

Wie im Abschnitt „Definieren des Vertrags“ erwähnt wurde, muss ein Projektverweis auf die winmd-Verweisdatei der Desktopkomponente vorgenommen werden. Das Visual Studio-Projektsystem erstellt normalerweise eine aus zwei Ebenen bestehende Verzeichnisstruktur mit demselben Namen. Im Beispiel lautet dieser „EnterpriseIPCApplication\\EnterpriseIPCApplication“. Die **winmd**-Verweisdatei wird manuell in dieses Verzeichnis der zweiten Ebene kopiert. Anschließend wird das Dialogfeld „Projektverweise“ verwendet (klicken Sie auf die Schaltfläche **Durchsuchen**...), um diese **winmd**-Datei zu suchen und zu referenzieren. Danach sollte der Namespace der obersten Ebene der Desktopkomponente (z. B. Fabrikam) als Knoten der obersten Ebene im Teil „Verweise“ des Projekts angezeigt werden.

>**Hinweis** Es ist sehr wichtig, die **WINMD-Verweisdatei** in der quergeladenen Anwendung zu verwenden. Wenn Sie versehentlich die **WINMD-Implementierungsdatei** in das Verzeichnis mit der quergeladenen App übernehmen und auf sie verweisen, wird wahrscheinlich ein Fehler wie „IStringable wurde nicht gefunden“ angezeigt. Dies ist ein sicheres Zeichen dafür, dass auf die falsche **WINMD-Datei** verwiesen wurde. Die Postbuildregeln in der IPC-Server-App (im folgenden Abschnitt ausführlich erläutert) trennen diese beiden **WINMD**-Dateien sorgfältig in zwei separaten Verzeichnissen.

In <ActivatableClassAttribute Value="path"> können Umgebungsvariablen verwendet werden (insbesondere „%ProgramFiles%“). Wie bereits erwähnt, werden vom App-Broker nur 32-Bit-Versionen unterstützt. Daher wird „%ProgramFiles%“ zu „C:\Programme (x86)“ aufgelöst, wenn die Anwendung auf einem 64-Bit-Betriebssystem ausgeführt wird.

##Desktop-IPC-Serverdetail

In den vorherigen beiden Abschnitten wurden die Deklaration der Klasse und die Mechanismen zur Übertragung der **WINMD**-Verweisdatei in das quergeladene Anwendungsprojekt erläutert. Der Großteil der restlichen Arbeit in der Desktopkomponente betrifft die Implementierung. Da die gesamte Desktopkomponente Desktopcode aufrufen kann (in der Regel zum Wiederverwenden vorhandener Coderessourcen), muss das Projekt auf spezielle Weise konfiguriert werden.
In der Regel wird bei einem Visual Studio-Projekt mit .NET eines von zwei „Profilen“ verwendet.
Eines ist für den Desktop („.NetFramework“) und eines ist auf den UWP-App-Teil der CLR ausgerichtet („.NetCore“). Bei einer Desktopkomponente in diesem Feature handelt es sich um einen Hybrid aus diesen beiden. Daher wird der Verweisabschnitt sehr sorgfältig konstruiert, um diese beiden Profile zu mischen.

Ein normales UWP-App-Projekt enthält keine expliziten Projektverweise, da die gesamte Windows-Runtime-API-Oberfläche implizit enthalten ist.
In der Regel werden nur andere projektübergreifenden Verweise vorgenommen. Ein Desktopkomponentenprojekt verfügt jedoch über einen sehr speziellen Satz an Verweisen. Sein Lebenszyklus beginnt als Projekt vom Typ „Classic Desktop\\Class Library“, und daher handelt es sich um ein Desktopprojekt. Deshalb müssen explizite Verweise auf die Windows-Runtime-API (mithilfe von Verweisen auf **WINMD**-Dateien) vorgenommen werden. Fügen Sie die korrekten Verweise wie unten gezeigt hinzu.

```XML
<ItemGroup>
    <!-- These reference are added by VS automatically when you create a Class Library project-->
    <Reference Include="System" />
    <Reference Include="System.Core" />
    <Reference Include="System.Xml.Linq" />
    <Reference Include="System.Data.DataSetExtensions" />
    <Reference Include="Microsoft.CSharp" />
    <Reference Include="System.Data" />
    <Reference Include="System.Net.Http" />
<Reference Include="System.Xml" />
    <!-- These reference should be added manually by editing .csproj file-->

    <Reference Include="System.Runtime.WindowsRuntime, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089, processorArchitecture=MSIL">
      <HintPath>$(MSBuildProgramFiles32)\Microsoft SDKs\NETCoreSDK\System.Runtime.WindowsRuntime\4.0.10\lib\netcore50\System.Runtime.WindowsRuntime.dll</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\UnionMetadata\Facade\Windows.WinMD</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Foundation.FoundationContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Foundation.FoundationContract\1.0.0.0\Windows.Foundation.FoundationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Foundation.UniversalApiContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Foundation.UniversalApiContract\1.0.0.0\Windows.Foundation.UniversalApiContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.Connectivity.WwanContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.Connectivity.WwanContract\1.0.0.0\Windows.Networking.Connectivity.WwanContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.ActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ActivationCameraSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ActivationCameraSettingsContract\1.0.0.0\Windows.ApplicationModel.Activation.ActivationCameraSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ContactActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ContactActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.ContactActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract\1.0.0.0\Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Calls.LockScreenCallContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Calls.LockScreenCallContract\1.0.0.0\Windows.ApplicationModel.Calls.LockScreenCallContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Resources.Management.ResourceIndexerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Resources.Management.ResourceIndexerContract\1.0.0.0\Windows.ApplicationModel.Resources.Management.ResourceIndexerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Search.Core.SearchCoreContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Search.Core.SearchCoreContract\1.0.0.0\Windows.ApplicationModel.Search.Core.SearchCoreContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Search.SearchContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Search.SearchContract\1.0.0.0\Windows.ApplicationModel.Search.SearchContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Wallet.WalletContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Wallet.WalletContract\1.0.0.0\Windows.ApplicationModel.Wallet.WalletContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Custom.CustomDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Custom.CustomDeviceContract\1.0.0.0\Windows.Devices.Custom.CustomDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Portable.PortableDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Portable.PortableDeviceContract\1.0.0.0\Windows.Devices.Portable.PortableDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Printers.Extensions.ExtensionsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Printers.Extensions.ExtensionsContract\1.0.0.0\Windows.Devices.Printers.Extensions.ExtensionsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Printers.PrintersContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Printers.PrintersContract\1.0.0.0\Windows.Devices.Printers.PrintersContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Scanners.ScannerDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Scanners.ScannerDeviceContract\1.0.0.0\Windows.Devices.Scanners.ScannerDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Sms.LegacySmsApiContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Sms.LegacySmsApiContract\1.0.0.0\Windows.Devices.Sms.LegacySmsApiContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Gaming.Preview.GamesEnumerationContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Gaming.Preview.GamesEnumerationContract\1.0.0.0\Windows.Gaming.Preview.GamesEnumerationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract\1.0.0.0\Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Graphics.Printing3D.Printing3DContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Graphics.Printing3D.Printing3DContract\1.0.0.0\Windows.Graphics.Printing3D.Printing3DContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Management.Deployment.Preview.DeploymentPreviewContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Management.Deployment.Preview.DeploymentPreviewContract\1.0.0.0\Windows.Management.Deployment.Preview.DeploymentPreviewContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Management.Workplace.WorkplaceSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Management.Workplace.WorkplaceSettingsContract\1.0.0.0\Windows.Management.Workplace.WorkplaceSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Capture.AppCaptureContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Capture.AppCaptureContract\1.0.0.0\Windows.Media.Capture.AppCaptureContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Capture.CameraCaptureUIContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Capture.CameraCaptureUIContract\1.0.0.0\Windows.Media.Capture.CameraCaptureUIContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Devices.CallControlContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Devices.CallControlContract\1.0.0.0\Windows.Media.Devices.CallControlContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.MediaControlContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.MediaControlContract\1.0.0.0\Windows.Media.MediaControlContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Playlists.PlaylistsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Playlists.PlaylistsContract\1.0.0.0\Windows.Media.Playlists.PlaylistsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Protection.ProtectionRenewalContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Protection.ProtectionRenewalContract\1.0.0.0\Windows.Media.Protection.ProtectionRenewalContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract\1.0.0.0\Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.Sockets.ControlChannelTriggerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.Sockets.ControlChannelTriggerContract\1.0.0.0\Windows.Networking.Sockets.ControlChannelTriggerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Security.EnterpriseData.EnterpriseDataContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Security.EnterpriseData.EnterpriseDataContract\1.0.0.0\Windows.Security.EnterpriseData.EnterpriseDataContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Security.ExchangeActiveSyncProvisioning.EasContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Security.ExchangeActiveSyncProvisioning.EasContract\1.0.0.0\Windows.Security.ExchangeActiveSyncProvisioning.EasContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Services.Maps.GuidanceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Services.Maps.GuidanceContract\1.0.0.0\Windows.Services.Maps.GuidanceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Services.Maps.LocalSearchContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Services.Maps.LocalSearchContract\1.0.0.0\Windows.Services.Maps.LocalSearchContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.SystemManufacturers.SystemManufacturersContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.SystemManufacturers.SystemManufacturersContract\1.0.0.0\Windows.System.Profile.SystemManufacturers.SystemManufacturersContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.ProfileHardwareTokenContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.ProfileHardwareTokenContract\1.0.0.0\Windows.System.Profile.ProfileHardwareTokenContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.ProfileRetailInfoContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.ProfileRetailInfoContract\1.0.0.0\Windows.System.Profile.ProfileRetailInfoContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.UserProfile.UserProfileContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.UserProfile.UserProfileContract\1.0.0.0\Windows.System.UserProfile.UserProfileContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.UserProfile.UserProfileLockScreenContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.UserProfile.UserProfileLockScreenContract\1.0.0.0\Windows.System.UserProfile.UserProfileLockScreenContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.ApplicationSettings.ApplicationsSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.ApplicationSettings.ApplicationsSettingsContract\1.0.0.0\Windows.UI.ApplicationSettings.ApplicationsSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Core.AnimationMetrics.AnimationMetricsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Core.AnimationMetrics.AnimationMetricsContract\1.0.0.0\Windows.UI.Core.AnimationMetrics.AnimationMetricsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Core.CoreWindowDialogsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Core.CoreWindowDialogsContract\1.0.0.0\Windows.UI.Core.CoreWindowDialogsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Xaml.Hosting.HostingContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Xaml.Hosting.HostingContract\1.0.0.0\Windows.UI.Xaml.Hosting.HostingContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Web.Http.Diagnostics.HttpDiagnosticsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Web.Http.Diagnostics.HttpDiagnosticsContract\1.0.0.0\Windows.Web.Http.Diagnostics.HttpDiagnosticsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>

```

Die oben erwähnten Verweise sind eine sorgfältige Mischung aus Verweisen, die für die korrekte Ausführung dieses Hybridservers entscheidend sind. Das normale Verfahren besteht darin, die CSPROJ-Datei (wie für die Bearbeitung des Projekts OutputType beschrieben) zu öffnen und diese Verweise wie erforderlich hinzuzufügen.

Sobald die Verweise korrekt konfiguriert wurden, besteht die nächste Aufgabe darin, die Funktionalität des Servers zu implementieren. Weitere Informationen finden Sie im MSDN-Thema [Bewährte Methoden für die Interoperabilität mit Windows-Runtime-Komponenten (Windows Store-Apps mit C\#/VB/C++ und XAML)](https://msdn.microsoft.com/en-us/library/windows/apps/hh750311.aspx).
Die Aufgabe besteht darin, eine Windows-Runtime-Komponenten-DLL zu erstellen, die Desktopcode als Teil der Implementierung aufrufen kann. Das Begleitbeispiel enthält die in der Windows-Runtime verwendeten Hauptmuster:

-   Methodenaufrufe

-   Windows-Runtime-Ereignisquellen der Desktopkomponente

-   Asynchrone Windows-Runtime-Vorgänge

-   Zurückgegebene Arrays grundlegender Typen

**Installieren**

Zum Installieren der App kopieren Sie die **WINMD**-Implementierung in das richtige Verzeichnis, das im Manifest der zugehörigen quergeladenen Anwendung angegeben ist: der Wert Value="path" von <ActivatableClassAttribute>. Kopieren Sie auch alle zugehörigen Unterstützungsdateien und die Proxy-/Stub-DLL (letztere wird weiter unten erläutert). Wenn die **WINMD**-Implementierung nicht an den Speicherort im Serververzeichnis kopiert wird, führt dies dazu, dass alle zu erneuernden Aufrufe der quergeladenen Anwendung für die RuntimeClass einen Fehler des Typs „Klasse nicht registriert“ zurückgeben. Wenn der Proxy/Stub nicht installiert (oder nicht registriert) wird, tritt bei allen Aufrufen ein Fehler auf, und es werden keine Werte zurückgegeben. Dieser letzte Fehler ist häufig **nicht** mit sichtbaren Ausnahmen verbunden.
Wenn aufgrund dieses Fehlers Ausnahmen beobachtet werden, beziehen sie sich unter Umständen auf eine „ungültige Umwandlung“.

**Überlegungen zur Serverimplementierung**

Sie können sich den Desktop-Windows-Runtime-Server als arbeits- oder aufgabenbasierten Server vorstellen. Jeder Aufruf an den Server fungiert als Nicht-UI-Thread, und der gesamte Code muss Multithread-fähig und sicher sein. Es ist auch wichtig, welcher Teil der quergeladenen Anwendung den Server aufruft. Es sollte auch immer vermieden werden, Code mit langer Ausführungsdauer über einen Benutzeroberflächen-Thread in der quergeladenen Anwendung auszuführen. Dazu gibt es zwei wesentlichen Möglichkeiten:

1.  Verwenden Sie beim Aufrufen einer Serverfunktionalität über einen Benutzeroberflächen-Thread immer ein asynchrones Muster in der öffentlichen Oberfläche und der Implementierung des Servers.

2.  Rufen Sie die Funktionalität des Servers über einen Hintergrundthread in der quergeladenen Anwendung auf.

**Asynchrone Windows-Runtime im Server**

Aufgrund der prozessübergreifenden Natur des Anwendungsmodells erzeugen Aufrufe an den Server mehr Mehraufwand als Code, der ausschließlich innerhalb eines Prozesses ausgeführt wird. In der Regel ist es sicher, eine einfache Eigenschaft aufzurufen, die einen speicherinternen Wert zurückgibt, da dies schnell genug ausgeführt wird, um den UI-Thread nicht zu blockieren. Allerdings können alle Aufrufe, die E/A-Vorgänge beinhalten (dazu zählen sämtliche Dateiverarbeitungen und Datenbankabrufe), den aufrufenden UI-Thread potenziell blockieren und dazu führen, dass die Anwendung beendet wird, weil sie nicht reagiert. Außerdem wird bei dieser Anwendungsarchitektur aus Leistungsgründen von Eigenschaftsaufrufen für Objekte abgeraten.
Eine ausführlichere Erläuterung finden Sie im folgenden Abschnitt.

Ein korrekt implementierter Server implementiert über UI-Threads ausgeführte Aufrufe normalerweise direkt über das asynchrone Windows-Runtime-Muster. Dies kann durch das folgende Muster implementiert werden. Zunächst erfolgt die Deklaration (erneut aus dem Begleitbeispiel):

```csharp
public IAsyncOperation<int> FindElementAsync(int input)
```

Hiermit wird ein asynchroner Widows-Runtime-Vorgang deklariert, der eine ganze Zahl zurückgibt.
Die Implementierung des asynchronen Vorgangs sieht in der Regel wie folgt aus:

```csharp
return Task<int>.Run( () =>
{
    int retval = ...
    // execute some potentially long-running code here 
}).AsAsyncOperation<int>();

```

>**Hinweis** Beachten Sie, dass es beim Schreiben der Implementierung normal ist, auf andere Vorgänge mit potenziell langer Ausführungsdauer zu warten. In diesem Fall muss der **Task.Run**-Code deklariert werden:

```csharp
return Task<int>.Run(async () =>
{
    int retval = ...
    // execute some potentially long-running code here 
    await ... // some other WinRT async operation or Task
}).AsAsyncOperation<int>();
```

Clients dieser asynchronen Methode können wie bei jedem anderen asynchronen Windows-Runtime-Vorgang auf den Vorgang warten.

**Aufrufen der Serverfunktionalität aus dem Hintergrundthread einer Anwendung**

Da Client und Server in der Regel von derselben Organisation geschrieben werden, kann eine Programmiermethode angewendet werden, bei der alle Aufrufe an den Server durch einen Hintergrundthread in der quergeladenen Anwendung erfolgen. Ein direkter Aufruf, bei dem ein oder mehrere Datenstapel vom Server abgerufen werden, kann über einen Hintergrundthread erfolgen. Wenn die Ergebnisse vollständig abgerufen wurden, kann der Datenstapel, der sich im Speicher des Anwendungsprozesses befindet, in der Regel direkt über den Benutzeroberflächen-Thread abgerufen werden. C\#-Objekte sind von sich aus zwischen Hintergrundthreads und Benutzeroberflächen-Threads agil und somit für diese Art des Aufrufmusters besonders nützlich.

##Erstellen und Bereitstellen des Windows-Runtime-Proxys

Da die IPC-Methode das Marshalling von Windows-Runtime-Schnittstellen zwischen zwei Prozessen beinhaltet, muss ein global registrierter Windows-Runtime-Proxy und -Stub verwendet werden.

**Erstellen des Proxys in Visual Studio**

Der Prozess zum Erstellen und Registrieren von Proxys und Stubs für die Verwendung innerhalb eines standardmäßigen Windows Store-App-Pakets ist im Thema [Auslösen von Ereignissen in Windows-Runtime-Komponenten](https://msdn.microsoft.com/en-us/library/windows/apps/dn169426.aspx) erläutert.
Die in diesem Artikel beschriebenen Schritte sind komplizierter als der nachfolgend beschriebene Prozess, da sie das Registrieren des Proxys/Stubs innerhalb des Anwendungspakets enthalten (im Gegensatz zur globalen Registrierung).

**Schritt 1:** Erstellen Sie mithilfe der Projektmappe für das Desktopkomponentenprojekt ein Proxy-/Stub-Projekt in Visual Studio:

**Projektmappe > Hinzufügen > Projekt > Visual C++ > Win32-Konsolenoption „DLL auswählen“.**

Bei den nachfolgenden Schritten wird davon ausgegangen, dass der Name der Serverkomponente **MyWinRTComponent** lautet.

**Schritt 3:** Löschen Sie alle CPP/H-Dateien im Projekt.

**Schritt 4:** Der vorherige Abschnitt „Definieren des Vertrags“ enthält einen Postbuildbefehl, der **winmdidl.exe**, **midl.exe**, **mdmerge.exe** usw. ausführt. Eine der Ausgaben des midl-Schritts dieses Postbuildbefehls generiert vier wichtige Ausgaben:

a) Dlldata.c

b) Eine Headerdatei (z. B. MyWinRTComponent.h)

c) Eine \*\_i.c-Datei (z. B. MyWinRTComponent\_i.c)

d) Eine \*\_p.c-Datei (z. B. MyWinRTComponent\_p.c)

**Schritt 5:** Fügen Sie diese vier generieren Dateien dem Projekt „MyWinRTProxy“ hinzu.

**Schritt 6:** Fügen Sie dem Projekt „MyWinRTProxy“ eine Definitionsdatei hinzu **(Projekt/Neues Element hinzufügen/Code/Moduldefinitionsdatei)**, und aktualisieren Sie den Inhalt wie folgt:

LIBRARY MyWinRTComponent.Proxies.dll

EXPORTS

DllCanUnloadNow PRIVATE

DllGetClassObject PRIVATE

DllRegisterServer PRIVATE

DllUnregisterServer PRIVATE

**Schritt 7:** Öffnen Sie Eigenschaften für das Projekt „MyWinRTProxy“:

**Konfigurationseigenschaften > Allgemein > Zielname:**

MyWinRTComponent.Proxies

**C/C++ > Präprozessordefinitionen > Hinzufügen**

"WIN32;\_WINDOWS;REGISTER\_PROXY\_DLL"

**C/C++ > Vorkompilierte Headerdatei: „Keine vorkompilierte Headerdatei verwenden“ auswählen**

**Linker > Allgemein > Importbibliothek ignorieren: „Ja“ auswählen**

**Linker > Eingabe > Zusätzliche Abhängigkeiten: „rpcrt4.lib;runtimeobject.lib“ hinzufügen**

**Linker > Windows-Metadaten > Windows-Metadaten generieren: „Nein“ auswählen**

**Schritt 8:** Erstellen Sie das Projekt „MyWinRTProxy“.

**Bereitstellen des Proxys**

Der Proxy muss global registriert werden. Die einfachste Möglichkeit hierzu besteht darin, dass beim Installationsprozess „DllRegisterServer“ in der Proxy-DLL aufgerufen wird. Da das Feature nur x86-Server unterstützt (d. h. keine 64-Bit-Unterstützung), besteht die einfachste Konfiguration in der Verwendung eines 32-Bit-Servers, eines 32-Bit-Proxys und einer quergeladenen 32-Bit-Anwendung. Der Proxy befindet sich normalerweise in der **WINMD**-Implementierung für die Desktopkomponente.

Es muss ein weiterer Konfigurationsschritt vorgenommen werden. Damit der Proxy vom quergeladenen Prozess geladen und ausgeführt wird, muss das Verzeichnis mit „lesen/ausführen“ für ALL_APPLICATION_PACKAGES gekennzeichnet sein. Dies erfolgt über das Befehlszeilentool **icacls.exe**. Dieser Befehl muss in dem Verzeichnis ausgeführt werden, in dem sich die **WINMD**-Implementierung und die Proxy-/Stub-DLL befinden:

*icacls. /T /grant \*S-1-15-2-1:RX*

##Muster und Leistung

Es ist sehr wichtig, die Leistung bei prozessübergreifenden Übertragungen genau zu überwachen. Ein prozessübergreifender Aufruf ist mindestens zweimal so teuer wie ein prozessinterner Aufruf. Das Erstellen prozessübergreifender „geschwätziger“ Konversationen oder das Ausführen wiederholter Übertragungen von großen Objekten wie Bitmapbildern kann zu einer unerwarteten und unerwünschten Leistung der Anwendung führen.

Es folgt eine unvollständige Liste mit zu berücksichtigenden Punkten:

-   Synchrone Methodenaufrufe über den Benutzeroberflächen-Thread einer Anwendung an den Server sollten immer vermieden werden. Rufen Sie die Methode über einen Hintergrundthread in der Anwendung auf, und verwenden Sie dann bei Bedarf „CoreWindowDispatcher“, um die Ergebnisse in den Benutzeroberflächen-Thread zu übertragen.

-   Das Aufrufen asynchroner Vorgänge aus dem Benutzeroberflächen-Thread einer Anwendung ist zwar sicher, aber Sie sollten dabei die nachfolgend erläuterten Leistungsprobleme berücksichtigen.

-   Eine Massenübertragung von Ergebnissen reduziert die prozessübergreifende "Geschwätzigkeit". Dazu wird in der Regel das Windows-Runtime-Array-Konstrukt verwendet.

-   Beim Zurückgeben von *List<T>*, wobei *T* ein Objekt aus einem asynchronen Vorgang oder einem Eigenschaftsabruf ist, wird viel „Geschwätzigkeit“ erzeugt. Angenommen, Sie geben ein *List&lt;People&gt;*-Objekt zurück. Bei jedem Iterationsdurchlauf handelt es sich um einen prozessübergreifenden Aufruf. Jedes zurückgegebene *People*-Objekt wird durch einen Proxy dargestellt, und jeder Methoden- oder Eigenschaftsaufruf für dieses einzelne Objekt führt zu einem prozessübergreifenden Aufruf. Daher führt ein „harmloses“ *List&lt;People&gt;*-Objekt, wobei *Count* hoch ist, zu einer Vielzahl von langsamen Aufrufen. Durch Massenübertragung von Inhaltsstrukturen in einem Array wird eine bessere Leistung erzielt. Beispiel:

```csharp
struct PersonStruct
{
    String LastName;
    String FirstName;
    int Age;
   // etc.
}
```

Geben Sie dann * PersonStruct\[\]* anstelle von *List&lt;PersonObject&gt;* zurück.
Dadurch werden alle Daten in einem prozessübergreifenden Hop verteilt.

Wie bei allen Überlegungen in Bezug auf die Leistung sind Messungen und Tests erforderlich. Idealerweise sollte eine Telemetrie in die zahlreichen Vorgänge integriert werden, um deren Dauer zu ermitteln. Dabei ist es wichtig, einen Bereich zu messen: Wie lange dauert es beispielsweise tatsächlich, bis alle *People*-Objekte von einer bestimmten Warteschlange in der quergeladenen Anwendung verarbeitet wurden?

Eine weitere Technik ist das Testen variabler Lasten. Dazu können Leistungstest-Hooks in die Anwendung eingefügt werden, die Lasten mit variabler Verzögerung in die Serververarbeitung integrieren. So können verschiedene Lastenarten und die Reaktion der Anwendung auf die variierende Serverleistung simuliert werden.
Im Beispiel ist dargestellt, wie mithilfe entsprechender asynchroner Techniken Zeitverzögerungen in den Code eingefügt werden können. Die genaue Dauer der zu integrierenden Verzögerung und die Zufallshäufigkeit für diese künstliche Last variieren je nach Anwendungsdesign und der voraussichtlichen Umgebung, in der die Anwendung ausgeführt wird.

##Entwicklungsprozess

Wenn Sie Änderungen am Server vornehmen, müssen Sie sicherstellen, dass zuvor ausgeführte Instanzen nicht mehr ausgeführt werden. COM sorgt schließlich für das Bereinigen des Prozesses, die vom Rundown-Timer benötigte Zeit ist jedoch zu lang für eine iterative Entwicklung. Das Beenden einer zuvor ausgeführten Instanz ist somit ein regulärer Schritt im Rahmen der Entwicklung. Hierfür muss der Entwickler laufend verfolgen, welche dllhost-Instanz den Server hostet.

Der Serverprozess kann im Task-Manager oder in anderen Drittanbieter-Apps aufgesucht und beendet werden. Das Befehlszeilentool **TaskList.exe ** ist ebenfalls enthalten; es weist eine flexible Syntax wie die Folgende auf:

  
 | **Befehl** | **Aktion** |
 | ------------| ---------- |
 | tasklist | Listet alle ausgeführten Prozesse in annähernder Reihenfolge des Erstellungszeitpunkts auf, wobei die zuletzt erstellten Prozesse unten aufgeführt werden. |
 | tasklist /FI "IMAGENAME eq dllhost.exe" /M | Listet Informationen zu allen Instanzen von „dllhost.exe“ auf. Vom /M-Schalter werden die von ihnen geladenen Module aufgelistet. |
 | tasklist /FI "PID eq 12564" /M | Sie können mit dieser Option die „dllhost.exe“ abfragen, wenn Ihnen die zugehörige PID bekannt ist. |

In der Liste der Module für einen Broker-Server muss *clrhost.dll* als geladenes Modul aufgeführt werden.

##Ressourcen

-   [Projektvorlagen für vermittelte WinRT-Komponenten für Windows 10 und Visual Studio 2015](https://visualstudiogallery.msdn.microsoft.com/10be07b3-67ef-4e02-9243-01b78cd27935)

-   [NorthwindRT-Beispiel zu Projektvorlagen für vermittelte WinRT-Komponenten](http://go.microsoft.com/fwlink/p/?LinkID=397349)

-   [Bereitstellen von zuverlässigen und vertrauenswürdigen Windows Store-Apps](http://go.microsoft.com/fwlink/p/?LinkID=393644)

-   [App-Verträge und -Erweiterungen (Windows Store-Apps)](https://msdn.microsoft.com/en-us/library/windows/apps/hh464906.aspx)

-   [Querladen von Apps unter Windows 10](https://msdn.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#GroupPolicy)

-   [Bereitstellen von Windows Store-Apps für Unternehmen](http://go.microsoft.com/fwlink/p/?LinkID=264770)





<!--HONumber=Jun16_HO4-->


