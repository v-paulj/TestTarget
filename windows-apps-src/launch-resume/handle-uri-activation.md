---
title: Behandeln der URI-Aktivierung
description: Erfahren Sie, wie Sie eine App registrieren müssen, damit sie der Standardhandler eines URI-Schemanamens (Uniform Resource Identifier) wird.
ms.assetid: 92D06F3E-C8F3-42E0-A476-7E94FD14B2BE
---

# Behandeln der URI-Aktivierung


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**Wichtige APIs**

-   [**Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224742)
-   [**Windows.UI.Xaml.Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330)

Erfahren Sie, wie Sie eine App registrieren müssen, damit sie der Standardhandler eines URI-Schemanamens (Uniform Resource Identifier) wird. Sowohl CWP-Apps (Klassische Windows-Plattform) als auch UWP-Apps (Universelle Windows-Plattform) können als Standardhandler für ein URI-Schemanamen registriert werden. Wenn Ihre App vom Benutzer als Standardhandler für einen URI-Schemanamen ausgewählt wird, wird sie immer dann aktiviert, wenn dieser URI-Typ gestartet wird.

Wir empfehlen, die Registrierung für einen URI-Schemanamen nur durchzuführen, wenn davon auszugehen ist, dass Sie alle entsprechenden Vorgänge für diesen URI-Schematyp behandeln werden. Wenn Sie sich entscheiden, die Registrierung für einen bestimmten URI-Schemanamen durchzuführen, müssen Sie die entsprechenden Funktionen bereitstellen, die vom Benutzer bei der Aktivierung für dieses URI-Schemas erwartet werden. Beispielsweise sollte eine App, die für den URI-Schemanamen "mailto:" registriert wird, eine neue E-Mail-Nachricht öffnen, damit Benutzer eine neue E-Mail erstellen können. Weitere Informationen zu URI-Zuordnungen finden Sie unter [Richtlinien und Prüflisten für Dateitypen und URIs](https://msdn.microsoft.com/library/windows/apps/hh700321).

Die folgenden Schritte zeigen, wie Sie den benutzerdefinierten Schemanamen „.alsdk://“ registrieren und Ihre App aktivieren, wenn der Benutzer einen URI vom Typ „.alsdk://“ startet.

> **Hinweis**  In UWP-Apps sind bestimmte URIs und Dateierweiterungen für die Verwendung durch vorinstallierte Apps und das Betriebssystem reserviert. Versuche, die App mit einem reservierten URI oder einer reservierten Dateierweiterung zu registrieren, werden ignoriert. Eine alphabetische Liste der URI-Schemas, die Sie nicht für Ihre UWP-Apps registrieren können, da diese entweder reserviert oder verboten sind, finden Sie unter [Reservierte URI-Schemanamen und Dateitypen](reserved-uri-scheme-names.md).

## Schritt 1: Angeben des Erweiterungspunkts im Paketmanifest


Die App empfängt nur für die im Paketmanifest angegebenen URI-Schemanamen Aktivierungsereignisse. Im Folgenden wird beschrieben, wie Sie festlegen, dass die App den URI-Schemanamen `alsdk` behandelt.

1.  Doppelklicken Sie im **Projektmappen-Explorer** auf „package.appxmanifest“, um den Manifest-Designer zu öffnen. Wählen Sie die Registerkarte **Deklarationen** und in der Dropdownliste **Verfügbare Deklarationen** die Option **Protokoll** aus, und klicken Sie dann auf **Hinzufügen**.

    Es folgt eine kurze Beschreibung der einzelnen Felder, die Sie im Manifest-Designer für das Protokoll ausfüllen können (Einzelheiten finden Sie unter [**AppX Package Manifest**](https://msdn.microsoft.com/library/windows/apps/dn934791)):
    
| Feld | Beschreibung |
|-------|-------------|
| **Logo** | Geben Sie das Logo zur Identifikation des URI-Schemanamens in der **Systemsteuerung** unter [Standardprogramme festlegen](https://msdn.microsoft.com/library/windows/desktop/cc144154) an. Wenn kein Logo angegeben wird, wird das kleine Logo für die App verwendet. |
| **Anzeigename** | Geben Sie den Anzeigenamen zur Identifikation des URI-Schemanamens in der **Systemsteuerung** unter [Standardprogramme festlegen](https://msdn.microsoft.com/library/windows/desktop/cc144154) an. |
| **Name** | Wählen Sie einen Namen für das URI-Schema aus. |
|  | **Hinweis**  Der Name darf nur aus Kleinbuchstaben bestehen. |
|  | **Reservierte und verbotene Dateitypen** Eine alphabetische Liste der URI-Schemas, die Sie nicht für Ihre UWP-Apps registrieren können, da diese entweder reserviert oder verboten sind, finden Sie unter [Reservierte URI-Schemanamen und Dateitypen](reserved-uri-scheme-names.md). |
| **Ausführbare Datei** | Gibt die standardmäßige ausführbare Datei für den Start des Protokolls an. Wenn keine Datei angegeben wird, wird die ausführbare Datei der App verwendet. Wenn eine Datei angegeben wird, muss die Zeichenfolge zwischen 1 und 256 Zeichen lang sein, mit „.exe“ enden und darf die folgenden Zeichen nicht enthalten: &gt;, &lt;, :, ", &#124;, ? oder \*. Bei Angabe einer Datei wird auch der **Einstiegspunkt** verwendet. Wenn der **Einstiegspunkt** nicht angegeben wird, wird der für die App definierte Einstiegspunkt verwendet. |
       
| Benennung | Beschreibung |
|------|-------------|
| **Einstiegspunkt** | Gibt die Aufgabe an, die die Protokollerweiterung behandelt. Dies ist normalerweise der vollständig qualifizierte Namespacename eines Windows-Runtime-Typs. Wenn keine Angabe erfolgt, wird der Einstiegspunkt für die App verwendet. |
| **Startseite** | Die Webseite, die den Erweiterungspunkt behandelt. |
| **Ressourcengruppe** | Eine Markierung, die Sie zum Gruppieren von Erweiterungsaktivierungen zu Zwecken der Ressourcenverwaltung verwenden können. |
| **Gewünschte Ansicht** (nur Windows) | Verwenden Sie das Feld **Gewünschte Ansicht**, um anzugeben, wie viel Platz für das Fenster der App benötigt wird, wenn sie für den URI-Schemanamen gestartet wird. Die möglichen Werte für **Gewünschte Ansicht** sind **Default**, **UseLess**, **UseHalf**, **UseMore** oder **UseMinimum**. <br/>**Hinweis**  Windows bestimmt die endgültige Fenstergröße einer Ziel-App anhand zahlreicher Faktoren (z. B. die Einstellung der Quell-App, die Anzahl der Apps auf dem Bildschirm, die Bildschirmausrichtung usw.). Das Festlegen von **Gewünschte Ansicht** ist keine Garantie, dass das Fenster für die Ziel-App auch wirklich so angezeigt wird.<br/> **Mobilgerätfamilie: Gewünschte Ansicht** wird für die Mobilgerätefamilie nicht unterstützt. |
2.  Geben Sie `images\Icon.png` als **Logo** ein.
3.  Geben Sie `SDK Sample URI Scheme` als **Anzeigenamen** ein.
4.  Geben Sie `alsdk` in das Feld **Name** ein.
5.  Drücken Sie STRG+S, um die an „package.appxmanifest“ vorgenommenen Änderungen zu speichern.

    Dadurch wird dem Paketmanifest ein [**Extension**](https://msdn.microsoft.com/library/windows/apps/br211400)-Element wie das unten dargestellte hinzugefügt. Die **windows.protocol**-Kategorie gibt an, dass die App den URI-Schemanamen `alsdk` behandelt.

    ```xml
          <Extensions>
            <uap:Extension Category="windows.protocol">
              <uap:Protocol Name="alsdk">
                <uap:Logo>images\icon.png</uap:Logo>
                <uap:DisplayName>SDK Sample URI Scheme</uap:DisplayName>
              </uap:Protocol>
            </uap:Extension>
          </Extensions>
    ```

## Schritt 2: Hinzufügen der geeigneten Symbole


Die Symbole von Apps, die für einen URI-Schemanamen zum Standard werden, werden an verschiedenen Stellen innerhalb des Systems angezeigt, z. B. in der Systemsteuerung unter "Standardprogramme".

Wir empfehlen, die richtigen Symbole in das Projekt aufzunehmen, damit Ihr Logo an allen diesen Stellen gut aussieht. Stimmen Sie das Erscheinungsbild des Logos der App-Kachel ab, und verwenden Sie die Hintergrundfarbe der App, anstatt das Symbol transparent darzustellen. Erweitern Sie das Logo bis zum Rand, ohne eine Auffüllung vorzunehmen. Testen Sie Ihre Symbole auf weißem Hintergrund. Beispielsymbole finden Sie unter [Beispiel für Assoziationsstart](http://go.microsoft.com/fwlink/p/?LinkID=620490).

![Der Projektmappen-Explorer mit einer Ansicht der Dateien im Bildordner. Es gibt Versionen mit 16, 32, 48 und 256 Pixeln für „icon.targetsize“ und „smalltile-sdk“](images/seviewofimages.png)

## Schritt 3: Behandeln des activated-Ereignisses


Der [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330)-Ereignishandler empfängt alle Aktivierungsereignisse. Die **Kind**-Eigenschaft gibt den Typ des Aktivierungsereignisses an. In diesem Beispiel werden [**Protocol**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.activation.activationkind.aspx#Protocol)-Aktivierungsereignisse behandelt.

> [!div class="tabbedCodeSnippets"]
```cs
public partial class App
{
   protected override void OnActivated(IActivatedEventArgs args)
   {
      if (args.Kind == ActivationKind.Protocol)
      {
         ProtocolActivatedEventArgs eventArgs = args as ProtocolActivatedEventArgs;
         // TODO: Handle URI activation
         // The received URI is eventArgs.Uri.AbsoluteUri
      }
   }
}
```
```vb
Protected Overrides Sub OnActivated(ByVal args As Windows.ApplicationModel.Activation.IActivatedEventArgs)
   If args.Kind = ActivationKind.Protocol Then
      ProtocolActivatedEventArgs eventArgs = args As ProtocolActivatedEventArgs
      
      ' TODO: Handle URI activation
      ' The received URI is eventArgs.Uri.AbsoluteUri
   End If
End Sub
```
```cpp
void App::OnActivated(Windows::ApplicationModel::Activation::IActivatedEventArgs^ args)
{
   if (args->Kind == Windows::ApplicationModel::Activation::ActivationKind::Protocol)
   {
      Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs^ eventArgs = 
          dynamic_cast<Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs^>(args);
      
      // TODO: Handle URI activation  
      // The received URI is eventArgs->Uri->RawUri
   } 
}
```

> **Hinweis**  Achten Sie darauf, dass beim Start über einen Protokollvertrag der Benutzer über die Zurück-Schaltfläche zu dem Bildschirm zurückkehren muss, von dem aus die App gestartet wurde, und nicht zum vorherigen Inhalt der App.

Apps sollten für jedes Aktivierungsereignis, durch das eine neue Seite geöffnet wird, einen neuen XAML-[**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) erstellen. So enthält der Navigationsbackstack für den neuen XAML-**Frame** keinen vorherigen Inhalt, der beim Anhalten der App im aktuellen Fenster angezeigt wurde. Apps, für die ein einziger XAML-**Frame** für Start- und Dateiverträge verwendet wird, sollten vor dem Navigieren zu einer neuen Seite die Seiten im **Frame**-Navigationsjournal löschen.

Per Protokollaktivierung gestartete Apps sollten ggf. eine Benutzeroberfläche enthalten, über die der Benutzer zur ersten Seite der App zurückkehren kann.

## Anmerkungen


Ihr URI-Schemaname kann von jeder App oder Website verwendet werden, auch von schädlichen. Alle im URI empfangenen Daten könnten daher von einer nicht vertrauenswürdigen Quelle stammen. Wir empfehlen, niemals eine endgültige Aktion auf Grundlage der Parameter auszuführen, die Sie im URI erhalten. URI-Parameter können z. B. zum Starten der App mit der Kontoseite eines Benutzers, aber nicht zum direkten Ändern des Kontos des Benutzers verwendet werden.

> **Hinweis**  Wenn Sie für Ihre App einen neuen URI-Schemanamen erstellen, ist es wichtig, dass Sie die Ratschläge in [RFC 4395](http://go.microsoft.com/fwlink/p/?LinkID=266550) befolgen. Damit wird sichergestellt, dass der Name die Standards für URI-Schemas erfüllt.

> **Hinweis**  Achten Sie darauf, dass beim Start über einen Protokollvertrag der Benutzer über die Zurück-Schaltfläche zu dem Bildschirm zurückkehren muss, von dem aus die App gestartet wurde, und nicht zum vorherigen Inhalt der App.

Apps sollten für jedes Aktivierungsereignis, durch das ein neues URI-Ziel geöffnet wird, einen neuen XAML-[**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) erstellen. So enthält der Navigationsbackstack für den neuen XAML-**Frame** keinen vorherigen Inhalt, der beim Anhalten der App im aktuellen Fenster angezeigt wurde.

Falls Ihre Apps für Start- und Protokollverträge einen einzelnen XAML-[**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) verwenden sollen, müssen vor dem Navigieren zu einer neuen Seite die Seiten im **Frame**-Navigationsjournal gelöscht werden. Über den Protokollvertrag gestartete Apps sollten ggf. eine Benutzeroberfläche enthalten, über die der Benutzer zum Anfang der App zurückkehren kann.

> **Hinweis**  Dieser Artikel ist für Windows 10-Entwickler bestimmt, die UWP-Apps (Universelle Windows-Plattform) schreiben. Wenn Sie für Windows 8.x oder Windows Phone 8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Verwandte Themen


**Vollständiges Beispiel**

* [Beispiel für Assoziationsstart](http://go.microsoft.com/fwlink/p/?LinkID=231484)

**Konzepte**

* [Standardprogramme](https://msdn.microsoft.com/library/windows/desktop/cc144154)
* [Dateityp- und URI-Zuordnungsmodell](https://msdn.microsoft.com/library/windows/desktop/hh848047)

**Aufgaben**

* [Starten der Standard-App für einen URI](launch-default-app.md)
* [Behandeln der Dateiaktivierung](handle-file-activation.md)

**Richtlinien**

* [Richtlinien für Dateitypen und URIs](https://msdn.microsoft.com/library/windows/apps/hh700321)

**Referenz**

* [**AppX Package Manifest**](https://msdn.microsoft.com/library/windows/apps/dn934791)
* [**Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224742)
* [**Windows.UI.Xaml.Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330)

 

 





<!--HONumber=Mar16_HO1-->


