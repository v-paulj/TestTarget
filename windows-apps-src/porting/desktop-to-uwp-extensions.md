---
author: awkoren
Description: "Neben den normalen, für alle UWP-Apps verfügbaren APIs gibt es auch einige Erweiterungen und APIs, die nur für konvertierte Desktop-Apps zur Verfügung stehen. In diesem Artikel werden diese Erweiterungen und deren Verwendung beschrieben."
Search.Product: eADQiWindows 10XVcnh
title: "Erweiterungen für konvertierte Desktop-Apps"
translationtype: Human Translation
ms.sourcegitcommit: 09ddc8cad403a568a43e08f32abeaf0bbd40d59a
ms.openlocfilehash: 2aa55797ed3a6588b3a27158282a02827fbd2109

---

# Erweiterungen für konvertierte Desktop-Apps

Konvertierte Desktopanwendungen können mit einer breiten Palette von UWP-APIs (APIs der universellen Windows-Plattform) erweitert werden. Neben den normalen, für alle UWP-Apps verfügbaren APIs gibt es aber auch einige Erweiterungen und APIs, die nur für konvertierte Desktop-Apps zur Verfügung stehen. Diese Features sind für Szenarien wie das Starten eines Prozesses, wenn sich der Benutzer anmeldet, oder die Integration des Explorers vorgesehen und sollen einen reibungslosen Übergang von der ursprünglichen Desktop-App zum konvertierten App-Paket ermöglichen.

In diesem Artikel werden diese Erweiterungen und deren Verwendung beschrieben. Die meisten erfordern die manuelle Änderung der Manifestdatei Ihrer konvertierten App. In dieser werden die von der App verwendeten Erweiterungen deklariert. Klicken Sie zum Bearbeiten des Manifests in der VisualStudio-Projektmappe mit der rechten Maustaste auf die Datei **Package.appxmanifest**, und wählen Sie *Code anzeigen* aus. 

## Startaufgaben

Startaufgaben ermöglichen der App das automatische Ausführen einer ausführbaren Datei, wenn sich ein Benutzer anmeldet. 

Fügen Sie dem Manifest Ihrer App Folgendes hinzu, um eine Startaufgabe zu deklarieren: 

```XML
<desktop:Extension Category="windows.startupTask" Executable="bin\MyStartupTask.exe" EntryPoint="Windows.FullTrustApplication">
    <desktop:StartupTask TaskId="MyStartupTask" Enabled="true" DisplayName="My App Service" />
</desktop:Extension>
```
- *Extension Category* muss immer den Wert „windows.startupTask“ besitzen.
- *Extension Executable* ist der relative Pfad zu der ausführbaren Datei, die gestartet werden soll.
- *Extension EntryPoint* muss immer den Wert „Windows.FullTrustApplication“ besitzen.
- *StartupTask TaskId* ist ein eindeutiger Bezeichner für die Aufgabe. Mit diesem Bezeichner kann Ihre App die APIs in der **Windows.ApplicationModel.StartupTask**-Klasse aufrufen, um eine Startaufgabe programmgesteuert zu aktivieren oder zu deaktivieren.
- *StartupTask Enabled* gibt an, ob die Aufgabe erstmals aktiviert oder deaktiviert startet. Aktivierte Aufgaben werden bei der nächsten Anmeldung des Benutzers ausgeführt (es sei denn, der Benutzer deaktiviert sie). 
- *StartupTask DisplayName* ist der im Task-Manager angezeigte Name der Aufgabe. Diese Zeichenfolge kann mithilfe von ```ms-resource``` lokalisiert werden. 

Apps können mehrere Startaufgaben deklarieren. Diese werden jeweils unabhängig ausgelöst und ausgeführt. Alle Startaufgaben werden im Task-Manager auf der Registerkarte **Autostart** mit dem Namen aus Ihrem App-Manifest und dem Symbol Ihrer App angezeigt. Der Task-Manager analysiert automatisch die Startauswirkungen Ihrer Aufgaben. Benutzer können die Startaufgaben Ihrer App über den Task-Manager manuell deaktivieren. Vom Benutzer deaktivierte Aufgaben können nicht programmgesteuert reaktiviert werden.

## App-Ausführungsalias

Mit einem App-Ausführungsalias können Sie einen Schlagwortnamen für Ihre App angeben. Anhand dieses Schlagworts kann die App von Benutzern oder anderen Prozessen ganz einfach und ohne Angabe des vollständigen Pfads (etwa über „Ausführen“ oder über eine Eingabeaufforderung) gestartet werden – ganz so als befände sie sich in der PATH-Variablen. Wenn Sie als Alias also beispielsweise „Foo“ deklarieren, kann ein Benutzer in „cmd.exe“ die Zeichenfolge „Foo Bar.txt“ eingeben, und Ihre App wird mit dem Pfad zu „Bar.txt“ als Teil der Ereignisargumente aktiviert.

Fügen Sie dem Manifest Ihrer App Folgendes hinzu, um einen App-Ausführungsalias anzugeben: 

```XML 
<uap3:Extension Category="windows.appExecutionAlias" Executable="exes\launcher.exe" EntryPoint="Windows.FullTrustApplication">
    <uap3:AppExecutionAlias>
        <desktop:ExecutionAlias Alias="Foo.exe" />
    </uap3:AppExecutionAlias>
</uap3:Extension>
```

- *Extension Category* muss immer den Wert „windows.appExecutionAlias“ besitzen.
- *Extension Executable* ist der relative Pfad zu der ausführbaren Datei, die gestartet werden soll, wenn der Alias aufgerufen wird.
- *Extension EntryPoint* muss immer den Wert „Windows.FullTrustApplication“ besitzen.
- *ExecutionAlias Alias* ist der Kurzname für Ihre App. Er muss immer mit der Erweiterung „.exe“ enden. 

Für die einzelnen Anwendungen im Paket kann immer nur einzelner App-Ausführungsalias angegeben werden. Wenn sich mehrere Apps mit dem gleichen Alias registrieren, ruft das System die zuletzt registrierte App auf. Wählen Sie daher einen eindeutigen Alias, um die Wahrscheinlichkeit einer Überschreibung durch andere Apps möglichst gering zu halten.

## Protokollzuordnungen 

Protokollzuordnungen ermöglichen Interoperabilitätsszenarien zwischen Ihrer konvertierten App und anderen Programmen oder Systemkomponenten. Wenn Ihre konvertierte App mithilfe eines Protokolls gestartet wird, können Sie bestimmte Parameter angeben, die an die Aktivierungsereignisargumente übergeben werden sollen, um ein entsprechendes Verhalten zu erreichen. Beachten Sie, dass Parameter nur für konvertierte, vertrauenswürdige Apps unterstützt werden. Für UWP-Apps können dagegen keine Parameter verwendet werden.  

Fügen Sie dem Manifest Ihrer App Folgendes hinzu, um eine Protokollzuordnung zu deklarieren:

```XML
<uap3:Extension Category="windows.protocol">
    <uap3:Protocol Name="myapp-cmd" Parameters="/p &quot;%1&quot;" />
</uap3:Extension>
```

- *Extension Category* muss immer den Wert „windows.protocol“ besitzen. 
- *Protocol Name* ist der Name des Protokolls. 
- *Protocol Parameter* ist die Liste der Parameter und Werte, die bei der Aktivierung als Ereignisargumente an Ihre App übergeben werden sollen. Hinweis: Wenn eine Variable einen Pfad enthalten kann, sollten Sie den Wert in Anführungszeichen einschließen, damit auch Pfade mit Leerzeichen ordnungsgemäß übergeben werden.

## Dateien und Datei-Explorer-Integration

Bei konvertierten Apps können die Behandlung bestimmter Dateitypen und die Datei-Explorer-Integration auf unterschiedliche Arten registriert werden. Dadurch können Benutzer im Rahmen ihres normalen Workflows komfortabel auf Ihre App zuzugreifen.

Fügen Sie dem Manifest Ihrer App zunächst Folgendes hinzu: 

```XML
<uap3:Extension Category="windows.fileTypeAssociation">
    <uap3:FileTypeAssociation Name="MyApp">
        ... additional elements here ...
    </uap3:FileTypeAssociation>
</uap3:Extension>
```

- *Extension Category* muss immer den Wert „windows.fileTypeAssociation“ besitzen. 
- *FileTypeAssociation Name* ist eine eindeutige ID. Diese ID wird intern zum Generieren einer Programm-ID mit Hash verwendet, die mit Ihrer Dateitypzuordnung verknüpft ist. Mithilfe dieser ID können Sie Änderungen in zukünftigen Versionen Ihrer App verwalten. Wenn Sie also beispielsweise das Symbol für eine Dateierweiterung ändern möchten, können Sie sie in eine neue Dateitypzuordnung mit einem anderen Namen verschieben.  

Fügen Sie dem Eintrag als Nächstes zusätzliche untergeordnete Elemente für die spezifischen Features hinzu, die Sie benötigen. Die verfügbaren Optionen werden im Anschluss beschrieben.

### Unterstützte Dateitypen

Ihre App kann angeben, dass sie das Öffnen bestimmter Dateitypen unterstützt. Wenn ein Benutzer mit der rechten Maustaste auf eine Datei klickt und „Öffnen mit“ auswählt, wird Ihre App in der Vorschlagsliste angezeigt.

Beispiel:

```XML
<uap:SupportedFileTypes>
    <uap:FileType>.txt</uap:FileType>
    <uap:FileType>.avi</uap:FileType>
</uap:SupportedFileTypes>
```

- *FileType* ist die von Ihrer App unterstützte Erweiterung.

### Kontextmenüverben 

Benutzer öffnen Dateien in der Regel per Doppelklick. Klickt ein Benutzer dagegen mit der rechten Maustaste auf eine Datei, erscheint ein Kontextmenü mit verschiedenen Optionen (üblicherweise Verben wie „Öffnen“, „Bearbeiten“ und „Drucken“, aber auch Optionen wie „Vorschau“), die eine differenziertere Interaktion mit der Datei ermöglichen. 

Wenn Sie einen unterstützten Dateityp angeben, wird automatisch das Verb „Öffnen“ hinzugefügt. Apps können dem Kontextmenü des Datei-Explorers aber auch zusätzliche benutzerdefinierte Verben hinzufügen. Dadurch kann die App abhängig von der Auswahl, die der Benutzer beim Öffnen einer Datei trifft, auf eine bestimmte Weise gestartet werden.

Beispiel: 

```XML
<uap2:SupportedVerbs>
    <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot">Edit</uap:Verb>
    <uap3:Verb Id="Print" Extended="true" Parameters="/p &quot;%1&quot">Print</uap:Verb>
</uap2:SupportedVerbs>
```

- *Verb Id* ist eine eindeutige ID des Verbs. Bei UWP-Apps wird sie im Rahmen der Aktivierungsereignisargumente übergeben, um eine entsprechende Behandlung der Benutzerauswahl zu ermöglichen. Bei vertrauenswürdigen konvertierten Apps werden dagegen Parameter übergeben (siehe nächster Aufzählungspunkt). 
- *Verb Parameters* ist die Liste mit Argumentparametern und -werten für das Verb. Bei vertrauenswürdigen konvertierten Apps werden diese bei der Aktivierung als Ereignisargumente übergeben, um eine Anpassung des Verhaltens für unterschiedliche Aktivierungsverben zu ermöglichen. Wenn eine Variable einen Pfad enthalten kann, sollten Sie den Wert in Anführungszeichen einschließen, damit auch Pfade mit Leerzeichen ordnungsgemäß übergeben werden. Beachten Sie, dass bei UWP-Apps keine Parameter übergeben werden können. Diese erhalten stattdessen die ID (siehe vorheriger Aufzählungspunkt). 
- *Verb Extended* gibt an, dass das Verb nur angezeigt werden soll, wenn der Benutzer **UMSCHALT** gedrückt hält, wenn er zum Anzeigen des Kontextmenüs mit der rechten Maustaste auf die Datei klickt. Dieses Attribut ist optional und standardmäßig auf *False* (Verb soll immer angezeigt werden) festgelegt. Dieses Verhalten muss für jedes Verb einzeln angegeben werden – mit Ausnahme von „Öffnen“: Bei diesem Verb ist der Wert immer *False*. 
- *Verb* ist der Name, der im Kontextmenü des Datei-Explorers angezeigt wird. Diese Zeichenfolge kann mithilfe von ```ms-resource``` lokalisiert werden.

### Mehrfachauswahlmodell

Mit der Mehrfachauswahl können Sie angeben, wie sich die App verhalten soll, wenn ein Benutzer mehrere Dateien gleichzeitig öffnet, indem er beispielsweise im Datei-Explorer zehn Dateien markiert und anschließend auf „Öffnen“ tippt.

Bei konvertierten Desktop-Apps stehen die gleichen drei Optionen zur Verfügung wie bei regulären Desktop-Apps. 
- *Player*: Ihre App wird einmal aktiviert, und alle markierten Dateien werden als Argumentparameter übergeben.
- *Single*: Ihre App wird einmal für die erste markierte Datei aktiviert. Andere Dateien werden ignoriert. 
- *Document*: Für die markierten Dateien wird jeweils eine neue (eigene) Instanz Ihrer App aktiviert.

Für unterschiedliche Dateitypen und Aktionen können unterschiedliche Einstellungen festgelegt werden. So können beispielsweise *Dokumente* im Modus *Document* und *Bilder* im Modus *Player* geöffnet werden.

Fügen Sie in Ihrem Manifest den Elementen, die mit Dateitypen und dem Starten von Dateien zusammenhängen, das *MultiSelectModel*-Attribut hinzu, um das Verhalten der App festzulegen. 

Festlegen eines Modells für einen unterstützten Dateityp: 

```XML
<uap:FileType MultiSelectModel="Document">.txt</uap:FileType>
```

Festlegen eines Modells für Verben:

```XML
<uap3:Verb Id="Edit" MultiSelectModel="Player">Edit</uap:Verb>
<uap3:Verb Id="Preview" MultiSelectModel="Document">Preview</uap:Verb>
```

Falls Ihre App keine Option für die Mehrfachauswahl angibt, wird standardmäßig *Player* verwendet, wenn der Benutzer bis zu 15Dateien öffnet. Andernfalls wird bei konvertierten Apps standardmäßig *Document* verwendet. UWP-Apps werden immer als *Player* gestartet. 

### Vollständiges Beispiel

Im Anschluss finden Sie ein vollständiges Beispiel mit vielen der oben beschriebenen Elemente für Dateien und den Datei-Explorer: 

```XML
<uap3:Extension Category="windows.fileTypeAssociation">
    <uap3:FileTypeAssociation Name="MyApp">
        <uap:SupportedFileTypes>
            <uap:FileType MultiSelectModel="Document">.txt</uap:FileType>
            <uap:FileType>.foo</uap:FileType>
    </uap:SupportedFileTypes>
    <uap2:SupportedVerbs>
            <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot">Edit</uap:Verb>
            <uap3:Verb Id="Print" Parameters="/p &quot;%1&quot">Print</uap:Verb>
    </uap2:SupportedVerbs>
    <uap:Logo>Assets\MyExtensionLogo.png</uap:Logo>
    </uap3:FileTypeAssociation>
</uap3:Extension>
```

## Weitere Informationen

- [App-Paketmanifest](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)


<!--HONumber=Aug16_HO3-->


