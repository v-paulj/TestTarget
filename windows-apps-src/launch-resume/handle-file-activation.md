---
author: TylerMSFT
title: Behandeln der Dateiaktivierung
description: "Eine App kann als Standardhandler für einen bestimmten Dateityp registriert werden."
ms.assetid: A0F914C5-62BC-4FF7-9236-E34C5277C363
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 9c6358bccdea55a7c3749388c35aa770f4325960

---

# Behandeln der Dateiaktivierung


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**Wichtige APIs**

-   [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224716)
-   [**Windows.UI.Xaml.Application.OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)

Eine App kann als Standardhandler für einen bestimmten Dateityp registriert werden. Sowohl CWP-Apps (Klassische Windows-Plattform) als auch UWP-Apps (Universelle Windows-Plattform) können als Standarddateihandler registriert werden. Wenn Ihre App vom Benutzer als Standardhandler für einen bestimmten Dateityp ausgewählt wird, wird sie dann aktiviert, wenn der Dateityp gestartet wird.

Wir empfehlen, die Registrierung für einen Dateityp nur durchzuführen, wenn davon auszugehen ist, dass alle entsprechenden Vorgänge für einen bestimmten Dateityp verarbeitet werden. Wenn der Dateityp von Ihrer App nur intern verwendet wird, ist die Registrierung eines Standardhandlers nicht erforderlich. Wenn Sie sich entscheiden, die Registrierung für eine bestimmten Dateityp durchzuführen, müssen Sie die entsprechenden Funktionen bereitstellen, die vom Benutzer bei der Aktivierung für diesen Dateityp erwartet werden. Beispielsweise kann eine Bildanzeige-App für das Anzeigen des Dateityps JPG registriert werden. Weitere Informationen zu Dateizuordnungen finden Sie unter [Richtlinien für Dateitypen und URIs](https://msdn.microsoft.com/library/windows/apps/hh700321).

Die folgenden Schritte zeigen, wie Sie den benutzerdefinierten Dateityp „.alsdk“ registrieren und Ihre App aktivieren, wenn der Benutzer eine ALSDK-Datei startet.

> **Hinweis**  In UWP-Apps sind bestimmte URIs und Dateierweiterungen für die Verwendung durch vorinstallierte Apps und das Betriebssystem reserviert. Versuche, die App mit einem reservierten URI oder einer reservierten Dateierweiterung zu registrieren, werden ignoriert. Weitere Informationen finden Sie unter [Reservierte Datei- und URI-Schemanamen](reserved-uri-scheme-names.md).

## Schritt 1: Angeben des Erweiterungspunkts im Paketmanifest


Die App empfängt nur für die im Paketmanifest angegebenen Dateierweiterungen Aktivierungsereignisse. Im Folgenden wird beschrieben, wie Sie festlegen, dass die App Dateien mit der Erweiterung `.alsdk` behandelt.

1.  Doppelklicken Sie im **Projektmappen-Explorer** auf „package.appxmanifest“, um den Manifest-Designer zu öffnen. Wählen Sie die Registerkarte **Deklarationen** und dann in der Dropdownliste **Verfügbare Deklarationen** die Option **Dateitypzuordnungen** aus, und klicken Sie dann auf **Hinzufügen**. Ausführliche Informationen zu Bezeichnern, die von Dateizuordnungen verwendet werden, finden Sie unter [ProgIDs](https://msdn.microsoft.com/library/windows/desktop/cc144152).

    Es folgt eine kurze Beschreibung der Felder, die Sie im Manifest-Designer ausfüllen können:

| Feld | Beschreibung |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Anzeigename** | Geben Sie den Anzeigenamen für eine Gruppe von Dateitypen an. Anhand des Anzeigenamens wird in der **Systemsteuerung** unter [Standardprogramme festlegen](https://msdn.microsoft.com/library/windows/desktop/cc144154) der Dateityp identifiziert. |
| **Logo** | Geben Sie das Logo zur Identifikation des Dateityps auf dem Desktop in der **Systemsteuerung** unter [Standardprogramme festlegen](https://msdn.microsoft.com/library/windows/desktop/cc144154) an. Wenn kein Logo angegeben wird, wird das kleine Logo der Anwendung verwendet. |
| **Infotipp** | Geben Sie den [Infotipp](https://msdn.microsoft.com/library/windows/desktop/cc144152) für eine Gruppe von Dateitypen an. Diese QuickInfo wird angezeigt, wenn Benutzer mit der Maus auf das Symbol für eine Datei dieses Typs zeigen. |
| **Name** | Wählen Sie einen Namen für eine Gruppe von Dateitypen aus, die identische Anzeigenamen, Logos, Infotipps und Bearbeitungsflags verwenden. Wählen Sie einen Gruppennamen aus, der auch nach App-Updates unverändert bleiben kann. **Hinweis**  Der Name darf nur aus Kleinbuchstaben bestehen. |
| **Inhaltstyp** | Geben Sie den MIME-Inhaltstyp, z. B. **image/jpeg**, für einen bestimmten Dateityp an. **Wichtiger Hinweis zu zulässigen Inhaltstypen:**  Dies ist eine alphabetische Liste der MIME-Inhaltstypen, die Sie nicht in das Paketmanifest eingeben können, da sie entweder reserviert oder unzulässig sind: **application/force-download**, **application/octet-stream**, **application/unknown**, **application/x-msdownload**. |
| **Dateityp** | Geben Sie den Dateityp, für den die Registrierung durchgeführt werden soll, mit einem vorangestellten Punkt an, z. B. „.jpeg“. **Reservierte und verbotene Dateitypen** Eine alphabetische Liste der Dateitypen für integrierte Apps, die Sie nicht für Ihre UWP-Apps registrieren können, da diese entweder reserviert oder verboten sind, finden Sie unter [Reservierte URI-Schemanamen und Dateitypen](reserved-uri-scheme-names.md). |

2.  Geben Sie `alsdk` in das Feld **Name** ein.
3.  Geben Sie `.alsdk` als **Dateityp** ein.
4.  Geben Sie „images\\Icon.png“ als Logo ein.
5.  Drücken Sie STRG+S, um die an „package.appxmanifest“ vorgenommenen Änderungen zu speichern.

Die obigen Schritte fügen dem Paketmanifest ein [**Extension**](https://msdn.microsoft.com/library/windows/apps/br211400)-Element wie das unten dargestellte hinzu. Die **windows.fileTypeAssociation**-Kategorie gibt an, dass die App Dateien mit der Erweiterung `.alsdk` behandelt.

```xml
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap:FileTypeAssociation Name="alsdk">
            <uap:Logo>images\icon.png</uap:Logo>
            <uap:SupportedFileTypes>
              <uap:FileType>.alsdk</uap:FileType>
            </uap:SupportedFileTypes>
          </uap:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
```

## Schritt 2: Hinzufügen der geeigneten Symbole


Die Symbole von Apps, die für einen Dateityp zum Standard werden, werden an verschiedenen Stellen innerhalb des Systems angezeigt. Diese Symbole werden z. B. an folgenden Stellen angezeigt:

-   ItemsView von Windows Explorer, Kontextmenüs, Menüband
-   Standardprogramme in der Systemsteuerung
-   Dateiauswahl
-   Suchergebnisse auf dem Startbildschirm

Stimmen Sie das Erscheinungsbild des Logos der App-Kachel ab, und verwenden Sie die Hintergrundfarbe der App, anstatt das Symbol transparent darzustellen. Erweitern Sie das Logo bis zum Rand, ohne eine Auffüllung vorzunehmen. Testen Sie Ihre Symbole auf weißem Hintergrund. Beispielsymbole finden Sie unter [Beispiel für Assoziationsstart](http://go.microsoft.com/fwlink/p/?LinkID=620490).
![Der Projektmappen-Explorer mit einer Ansicht der Dateien im Bildordner. Es gibt Versionen mit 16, 32, 48 und 256 Pixeln für „icon.targetsize“ und „smalltile-sdk“](images/seviewofimages.png)

## Schritt 3: Behandeln des activated-Ereignisses


Der [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)-Ereignishandler empfängt alle Dateiaktivierungsereignisse.

> [!div class="tabbedCodeSnippets"]
```vb
Protected Overrides Sub OnFileActivated(ByVal args As Windows.ApplicationModel.Activation.FileActivatedEventArgs)
      ' TODO: Handle file activation
      ' The number of files received is args.Files.Size
      ' The name of the first file is args.Files(0).Name
End Sub
```
```cpp
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs^ args)
{
       // TODO: Handle file activation
       // The number of files received is args->Files->Size
       // The first file is args->Files->GetAt(0)->Name
}
```
```cs
protected override void OnFileActivated(FileActivatedEventArgs args)
{
       // TODO: Handle file activation
       // The number of files received is args.Files.Size
       // The name of the first file is args.Files[0].Name
}
```

    > **Note**  When launched via File Contract, make sure that Back button takes the user back to the screen that launched the app and not to the app's previous content.

[!div class="tabbedCodeSnippets"] Apps sollten für jedes Aktivierungsereignis, durch das eine neue Seite geöffnet wird, einen neuen XAML-Frame erstellen. So enthält der Navigationsbackstack für den neuen XAML-Frame keinen vorherigen Inhalt, der beim Anhalten der App im aktuellen Fenster angezeigt wurde.

Apps, für die ein einziger XAML-Frame für Start- und Dateiverträge verwendet wird, sollten vor dem Navigieren zu einer neuen Seite die Seiten im Navigationsjournal des Frames löschen.

## Per Dateiaktivierung gestartete Apps sollten ggf. eine Benutzeroberfläche enthalten, über die der Benutzer zur ersten Seite der App zurückkehren kann.


Anmerkungen Die empfangenen Dateien stammen unter Umständen aus einer nicht vertrauenswürdigen Quelle. Wir empfehlen, den Inhalt einer Datei zu überprüfen, bevor Sie sie weiter verarbeiten.

> Weitere Informationen zur Eingabeüberprüfung finden Sie unter [Schreiben von sicherem Code](http://go.microsoft.com/fwlink/p/?LinkID=142053). **Hinweis**  Dieser Artikel ist für Windows 10-Entwickler bestimmt, die UWP-Apps (Apps für die universelle Windows-Plattform) schreiben.

 

## Wenn Sie für Windows 8.x oder Windows Phone 8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

**Verwandte Themen**

* [Vollständiges Beispiel](http://go.microsoft.com/fwlink/p/?LinkID=231484)

**Beispiel für Assoziationsstart**

* [Konzepte](https://msdn.microsoft.com/library/windows/desktop/cc144154)
* [Standardprogramme](https://msdn.microsoft.com/library/windows/desktop/hh848047)

**Dateityp- und Protokollzuordnungsmodell**

* [Aufgaben](launch-the-default-app-for-a-file.md)
* [Starten der Standard-App für eine Datei](handle-uri-activation.md)

**Behandeln der URI-Aktivierung**

* [Anleitungen](https://msdn.microsoft.com/library/windows/apps/hh700321)

**Richtlinien für Dateitypen und URIs**
* [**Referenz**](https://msdn.microsoft.com/library/windows/apps/br224716)
* [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br242331)

 

 



<!--HONumber=Jun16_HO5-->


