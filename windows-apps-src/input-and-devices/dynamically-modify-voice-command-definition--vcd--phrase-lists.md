---
author: Karl-Bridge-Microsoft
Description: Erfahren Sie, wie Sie mithilfe des Spracherkennungsergebnisses zur Laufzeit auf die Liste der unterstützten Begriffe (PhraseList-Elemente) in einer VCD-Datei (Voice Command Definition) zugreifen und diese aktualisieren
title: Dynamisches Ändern von VCD-Begriffslisten
ms.assetid: 98024EAC-EC0E-44AA-AEC5-A611BA7C5884
label: Modify VCD phrase lists
template: detail.hbs
---

# Dynamisches Ändern von VCD-Begriffslisten





**Wichtige APIs**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

Erfahren Sie, wie Sie mithilfe des Spracherkennungsergebnisses zur Laufzeit auf die Liste der unterstützten Begriffe (**PhraseList**-Elemente) in einer VCD-Datei (Voice Command Definition) zugreifen und diese aktualisieren

Das dynamische Ändern einer Begriffsliste zur Laufzeit ist nützlich, wenn der Sprachbefehl sich speziell auf eine Aufgabe bezieht, die benutzerdefinierte oder transiente App-Daten umfasst. 

Angenommen, Sie haben eine Reise-App, bei der Benutzer Reiseziele eingeben können, und Sie möchten, dass Benutzer die App starten können, indem sie den Namen der App gefolgt von „Reise nach &lt;Ziel&gt; anzeigen“ sagen. Im **ListenFor**-Element selbst geben Sie Folgendes an: `<ListenFor> Show trip to {destination}  </ListenFor>`, wobei „Ziel“ der Wert des Attributs **Label** für die **PhraseList** ist.

Durch das Aktualisieren der Begriffsliste zur Laufzeit muss kein eigenes **ListenFor**-Element für jedes mögliche Ziel mehr erstellt werden. Stattdessen können Sie **PhraseList** dynamisch mit Zielen auffüllen, die vom Benutzer beim Eingeben der Reiseroute angegeben werden. 

Weitere Informationen zu **PhraseList** und anderen VCD-Elementen finden Sie in unter [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593).

**Voraussetzungen:  **

Dieses Thema baut auf [Starten einer Vordergrund-App mit Sprachbefehlen in Cortana](launch-a-foreground-app-with-voice-commands-in-cortana.md) auf. Wir zeigen hier weitere Features anhand einer Reiseplanungs- und Verwaltungs-App mit dem Namen **Adventure Works**.

Wenn Sie noch keine Erfahrung mit der Entwicklung von UWP-Apps (Universelle Windows-Plattform) haben, sollten Sie sich in den folgenden Themen zunächst mit den hier besprochenen Technologien vertraut machen.

-   [Erstellen Ihrer ersten App](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Informationen zu Ereignissen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Richtlinien für die Benutzerfreundlichkeit:  **

Unter [Cortana-Entwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/dn974233) finden Sie Informationen zur Integration Ihrer App mit **Cortana**. Unter [Entwurfsrichtlinien für die Spracherkennung](https://msdn.microsoft.com/library/windows/apps/dn596121) finden Sie hilfreiche Tipps für den Entwurf einer nützlichen und interaktiven sprachaktivierten App.

## <span id="Identify_the_command"></span><span id="identify_the_command"></span><span id="IDENTIFY_THE_COMMAND"></span>Identifizieren des Befehls und Aktualisieren der Begriffsliste

Diese VCD-Beispieldatei definiert den **Befehl** „showTripToDestination“ und eine **PhraseList**, die drei Optionen für Ziele in der Reise-App **Adventure Works** definiert. Wenn der Benutzer Ziele in der App speichert oder löscht, aktualisiert die App die Optionen in der **PhraseList**.

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <CommandPrefix> Adventure Works, </CommandPrefix>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

  </CommandSet>

<!-- Other CommandSets for other languages -->

</VoiceCommands>

```

Wenn Sie ein **PhraseList**-Element in der VCD-Datei aktualisieren möchten, rufen Sie das **CommandSet**-Element ab, das die Begriffsliste enthält. Verwenden Sie das Attribut **Name** für dieses **CommandSet**-Element (**Name** muss in der VCD-Datei eindeutig sein) als Schlüssel für den Zugriff auf die Eigenschaft [**VoiceCommandManager.InstalledCommandSets**](https://msdn.microsoft.com/library/windows/apps/dn653257), und rufen Sie die [**VoiceCommandSet**](https://msdn.microsoft.com/library/windows/apps/dn653258)-Referenz ab.

Nachdem Sie den Befehlssatz identifiziert haben, rufen Sie eine Referenz zu der Begriffsliste ab, die Sie ändern möchten, und rufen Sie die [**SetPhraseListAsync**](https://msdn.microsoft.com/library/windows/apps/dn653261)-Methode mit dem **Label**-Attribut des **PhraseList**-Elements und einem Array von Zeichenfolgen als neuen Inhalt der Begriffsliste auf.

**Hinweis**  Wenn Sie eine Begriffsliste ändern, wird die gesamte Begriffsliste ersetzt. Wenn Sie neue Elemente in eine Begriffsliste einfügen möchten, müssen Sie im Aufruf von [**SetPhraseListAsync**](https://msdn.microsoft.com/library/windows/apps/dn653261) sowohl die vorhandenen Elemente als auch die neuen Elemente angeben.

In diesem Beispiel wird die im vorherigen Beispiel gezeigte **PhraseList** mit dem zusätzlichen Reiseziel „Phoenix“ aktualisiert.

```CSharp
Windows.ApplicationModel.VoiceCommands.VoiceCommnadDefinition.VoiceCommandSet commandSetEnUs;

if (Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
      InstalledCommandSets.TryGetValue(
        "AdventureWorksCommandSet_en-us", out commandSetEnUs))
{
  await commandSetEnUs.SetPhraseListAsync(
    "destination", new string[] {“London”, “Dallas”, “New York”, “Phoenix”});
}
```

## <span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>Hinweise


Eine **PhraseList** eignet sich zur Einschränkung der Erkennung für einen relativ kleinen Satz oder Wörter. Wenn der Wörtersatz zu groß ist (z. B. mehrere hundert Wörter umfasst) oder nicht eingeschränkt werden soll, verwenden Sie das **PhraseTopic**-Element und ein **Subject**-Element zum Einschränken der Relevanz der Spracherkennungsergebnisse, um die Skalierbarkeit zu verbessern.

Dieses Beispiel zeigt ein **PhraseTopic** mit einem **Szenario** „Suche“, das durch das **Subject** „Ort\Staat“ weiter eingeschränkt wird.

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <CommandPrefix> Adventure Works, </CommandPrefix>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

    <PhraseTopic Label="destination" Scenario="Search">
      <Subject>City/State</Subject>
    </PhraseTopic>

  </CommandSet>
```

## <span id="related_topics"></span>Verwandte Artikel


**Entwickler**
* [Cortana-Interaktionen](cortana-interactions.md)
* [Starten einer Vordergrund-App mit Sprachbefehlen in Cortana](launch-a-foreground-app-with-voice-commands-in-cortana.md)
* [Starten einer Hintergrund-App mit Sprachbefehlen in Cortana](launch-a-background-app-with-voice-commands-in-cortana.md)
* [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Designer**
* [Cortana-Entwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Entwurfsrichtlinien für die Spracherkennung](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Beispiele**
* [Cortana-Sprachbefehlbeispiel](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=May16_HO2-->


