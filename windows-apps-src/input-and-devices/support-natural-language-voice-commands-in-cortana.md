---
author: Karl-Bridge-Microsoft
Description: Erfahren Sie, wie Sie Cortana mit flexibleren Befehlen in natürlicher Sprache erweitern, bei denen der Benutzer den App-Namen an beliebiger Stelle im Befehl verwenden kann.
title: Unterstützen von Sprachbefehlen in natürlicher Sprache in Cortana
ms.assetid: 281E068A-336A-4A8D-879A-D8715C817911
label: Support natural language voice commands
template: detail.hbs
---

# Unterstützen von Sprachbefehlen in natürlicher Sprache in Cortana

Erweitern Sie **Cortana** mit flexibleren Befehlen in natürlicher Sprache, bei denen der Benutzer den App-Namen an beliebiger Stelle im Befehl verwenden kann.

**Wichtige APIs**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**Voice Command Definition (VCD) elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)


Zur Verwendung von Sprachbefehlen, um Cortana mit Funktionen aus Ihrer App zu erweitern, muss der Benutzer sowohl die App als auch den auszuführenden Befehl bzw. die auszuführende Funktion angeben. Hierzu wird zu Beginn oder zu Ende des Sprachbefehls üblicherweise zunächst der App-Name genannt. Also beispielsweise: "Adventure Works, neue Reise nach Las Vegas hinzufügen".

Manchmal klingt der Befehl aber dadurch vielleicht merkwürdig, unnatürlich oder ergibt womöglich überhaupt keinen Sinn, wenn zuerst der Anwendungsname und dann der Befehl angegeben wird. Häufig ist es praktischer und natürlicher, den App-Namen an einer anderen Stelle innerhalb des Befehls zu nennen, was die Interaktion für den Benutzer deutlich intuitiver und angenehmer macht. Das vorherige Beispiel "Adventure Works, neue Reise nach Las Vegas hinzufügen" ließe sich beispielsweise wie folgt umformulieren: "Neue Adventure Works-Reise nach Las Vegas hinzufügen" oder "Neue Reise nach Las Vegas hinzufügen mit Adventure Works".

Der App-Name kann bei Sprachbefehlen wie folgt verwendet werden:

-   Als Präfix (vor dem Befehl)
-   Als Infix (innerhalb des Befehls)
-   Als Suffix (nach dem Befehl)

**Voraussetzungen:  **

Dieses Thema baut auf [Starten einer Hintergrund-App mit Sprachbefehlen in Cortana](launch-a-background-app-with-voice-commands-in-cortana.md) auf. Wir zeigen hier weitere Features anhand einer Reiseplanungs- und Verwaltungs-App mit dem Namen **Adventure Works**.

Wenn Sie noch keine Erfahrung mit der Entwicklung von UWP-Apps (Universelle Windows-Plattform) haben, sollten Sie sich in den folgenden Themen zunächst mit den hier besprochenen Technologien vertraut machen.

-   [Erstellen Ihrer ersten App](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Informationen zu Ereignissen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Richtlinien für die Benutzerfreundlichkeit:  **

Unter [Cortana-Entwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/dn974233) finden Sie Informationen zur Integration Ihrer App mit **Cortana**. Unter [Entwurfsrichtlinien für die Spracherkennung](https://msdn.microsoft.com/library/windows/apps/dn596121) finden Sie hilfreiche Tipps für den Entwurf einer nützlichen und interaktiven sprachaktivierten App.

## <span id="Specify_an_AppName_element_in_the_VCD"></span><span id="specify_an_appname_element_in_the_vcd"></span><span id="SPECIFY_AN_APPNAME_ELEMENT_IN_THE_VCD"></span>Angeben eines **AppName**-Elements in der VCD-Datei


Mithilfe des **AppName**-Elements wird in einem Sprachbefehl ein benutzerfreundlicher Name für eine App angegeben.
```XML
<AppName>Adventure Works</AppName>
```

## <span id="Specify_where_the_app_name_can_be_spoken_in_the_voice_command"></span><span id="specify_where_the_app_name_can_be_spoken_in_the_voice_command"></span><span id="SPECIFY_WHERE_THE_APP_NAME_CAN_BE_SPOKEN_IN_THE_VOICE_COMMAND"></span>Angeben der Position, an welcher der App-Name innerhalb des Sprachbefehls gesagt werden kann


Das **ListenFor**-Element verfügt über ein **RequireAppName**-Attribut, das angibt, wo der App-Name innerhalb des Sprachbefehls verwendet werden kann. Dieses Attribut unterstützt vier Werte:

1.  **BeforePhrase**

    Standard.

    Gibt an, dass der Benutzer den Namen Ihrer App vor dem Befehl sagen muss.

    Hier hört Cortana auf "Adventure Works, wann ist meine Reise nach Las Vegas".
```xml
<ListenFor RequireAppName="BeforePhrase"> show [my] trip to {destination} </ListenFor>
```

2.  **AfterPhrase**

    Gibt an, dass der Benutzer den Namen Ihrer App nach dem Befehl sagen muss.

    Das System stellt eine lokalisierte Begriffsliste mit präpositionalen Konjunktionen bereit. Diese enthält Ausdrücke wie "mithilfe von", "mit" und "in".

    Hier hört Cortana auf Befehle wie "Zeige meine nächste Reise nach Las Vegas in Adventure Works" oder "Zeige meine nächste Reise nach Las Vegas mit Adventure Works".
```xml
<ListenFor RequireAppName="AfterPhrase">show [my] next trip to {destination} </ListenFor>
```

3.  **BeforeOrAfterPhrase**

    Gibt an, dass der Benutzer den Namen Ihrer App vor oder nach dem Befehl sagen muss.

    Für die Suffix-Variante stellt das System eine lokalisierte Begriffsliste mit präpositionalen Konjunktionen bereit. Diese enthält Ausdrücke wie "mithilfe von", "mit" und "in".

    Hier hört Cortana auf Befehle wie "Adventure Works, zeige meine nächste Reise nach Las Vegas" oder "Zeige meine nächste Reise nach Las Vegas in Adventure Works".
``` xml
<ListenFor RequireAppName="BeforeOrAfterPhrase">show [my] next trip to {destination}</ListenFor>
```

4.  **ExplicitlySpecified**

    Gibt an, dass der Benutzer den Namen Ihrer App genau an der vorgegebenen Stelle des Befehls sagen muss. Der Benutzer muss den Namen der App nicht vor oder nach dem Befehl sagen.

    Auf den App-Namen muss explizit mit dem **{builtin:AppName}**-Tag verwiesen werden.

    Hier hört Cortana auf Befehle wie "Adventure Works, zeige meine nächste Reise nach Las Vegas" oder "Zeige meine nächste Adventure Works-Reise nach Las Vegas".
```xml
<ListenFor RequireAppName="ExplicitlySpecified">show [my] next {builtin:AppName} trip to {destination} </ListenFor>
```

## <span id="Special_cases"></span><span id="special_cases"></span><span id="SPECIAL_CASES"></span>Sonderfälle

Wenn Sie ein **ListenFor**-Element deklarieren, bei dem **RequireAppName** entweder auf "AfterPhrase" oder auf "ExplicitlySpecified" festgelegt ist, müssen bestimmte Anforderungen erfüllt werden:

1.  **{builtin:AppName}** muss genau einmal vorkommen, wenn **RequireAppName** auf „ExplicitlySpecified“ festgelegt ist.

    Bei diesem Wert kann das System nicht ableiten, an welcher Position der App-Name innerhalb des Sprachbefehls verwendet werden kann. Die Position muss explizit angegeben werden.

2.  Ein Sprachbefehl darf nicht mit einem **PhraseTopic**-Element beginnen. Dieses Element wird in der Regel für die Spracherkennung mit umfangreichem Vokabular verwendet. Es muss mindestens ein Wort vorangestellt werden.

    Dies macht es unwahrscheinlicher, dass **Cortana** Ihre App startet, wenn der Name Ihrer App (oder ein Teil des Namens) an beliebiger Stelle in einem Sprachbefehl auftaucht.

    Hier sehen Sie eine ungültige Deklaration, bei der die Gefahr besteht, dass **Cortana** die App **Adventure Works** startet, wenn der Benutzer etwas sagt wie "Show me reviews for Kinect adventure works (Rezensionen für Kinect Adventure-Inhalte anzeigen)".
```xml
<ListenFor RequireAppName="ExplicitlySpecified">{searchPhrase} {builtin:AppName}</ListenFor>
```

3.  Neben dem Namen Ihrer App und Verweisen auf **PhraseTopic**-Elemente muss die **ListenFor**-Zeichenfolge mindestens zwei Wörter enthalten.

    Ähnlich wie im zweiten Fall müssen Sie sicherstellen, dass Ihre Befehle über genügend phonetischen Inhalt verfügen, um ein unabsichtliches Starten Ihrer App zu vermeiden.

    Dadurch können Sie Ihre Anwendung optimal einrichten und dafür sorgen, dass sie nicht ungewollt gestartet wird, wenn ein Benutzer beispielsweise „Find Kinect Adventure works“ (Kinect Adventure-Inhalte suchen) sagt.

    Hier sehen Sie ungültige Deklarationen, bei denen die Gefahr besteht, dass **Cortana** die App **Adventure Works** startet, wenn der Benutzer etwas sagt wie "Hey Adventure Works" oder "Find Kinect adventure works (Kinect Adventure-Inhalte suchen)".
```xml
<ListenFor RequireAppName="ExplicitlySpecified">Hey {builtin:AppName}</ListenFor>
<ListenFor RequireAppName="ExplicitlySpecified">Find {searchPhrase} {builtin:AppName}</ListenFor>
```

## <span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>Hinweise

Durch die Unterstützung zusätzlicher Sprachbefehlsvariationen für **Cortana** erhöht sich auch die allgemeine Benutzerfreundlichkeit Ihrer App.

Vermeiden Sie die Verwendung von "Hey \[App-Name\]" als **AppName** oder **CommandPrefix**. Benutzer sagen eher "Hey Cortana", um Cortana per Sprachbefehl aufzurufen, und durch "Hey \[App-Name\]" wird der Sprachbefehl unnatürlich. Beispiel: "Hey Cortana, zeige meine nächste Reise nach Las Vegas in Hey Adventure Works".

Erweitern Sie Ihre vorhandenen Sprachbefehle ggf. um Infix-/Suffix-Varianten. Wie Sie gesehen haben, können Sie Ihren vorhandenen **ListenFor**-Elementen ohne großen Aufwand ein zusätzliches Attribut hinzufügen und so Suffix-Varianten unterstützen. "Hey Cortana, zeige meine nächste Reise nach Las Vegas in Adventure Works" ist einfach deutlich natürlicher als "Hey Cortana, Adventure Works, zeige meine nächste Reise nach Las Vegas".

Verwenden Sie in Situationen, in denen sich ein Konflikt mit vorhandenen Funktionen von **Cortana** ergibt (also beispielsweise bei Anrufen, Nachrichten oder Ähnlichem), den Namen Ihrer App ggf. als Präfix. Beispiel: "Adventure Works, sende eine Nachricht an \[Reisebüro\] wegen Las Vegas-Reise".

## <span id="Complete_example"></span><span id="complete_example"></span><span id="COMPLETE_EXAMPLE"></span>Vollständiges Beispiel


Hier sehen Sie eine VCD-Datei, die verschiedene Möglichkeiten für natürlichere Sprachbefehle veranschaulicht:

**Hinweis**  Sie können mehrere **ListenFor**-Elemente mit jeweils unterschiedlichen **RequireAppName**-Attributwerten verwenden. 

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="commandSet_en-us">
    <AppName>Adventure Works</AppName>
    <Example> When is my trip to Las Vegas? </Example>
    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforePhrase">
        when is my] trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands like 
           “Show my next trip to Las Vegas on Adventure Works”; “Show my next 
           trip to Las Vegas using Adventure Works” -->
      <ListenFor RequireAppName="AfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands when 
           the user specifies your app name either before or after the command. 
           “Adventure Works, show my next trip to Las Vegas”; 
           “Show my next trip to Last Vegas on Adventure works” -->
      <ListenFor RequireAppName="BeforeOrAfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands 
           when the user specifies your app name inline. 
           “Show my next Adventure Works trip to Las Vegas” -->
      <ListenFor RequireAppName="ExplicitlySpecified">
        show [my] next {builtin:AppName} trip to {destination} </ListenFor>

      <Feedback> Looking for trip to {destination} </Feedback>
      <Navigate />
    </Command>
    <PhraseList Label="destination">
      <Item> Las Vegas </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>
  </CommandSet>
  <!-- Other CommandSets for other languages -->
</VoiceCommands>
```

## <span id="related_topics"></span>Verwandte Artikel


**Entwickler**
* [Cortana-Interaktionen](cortana-interactions.md)
* [Definieren von benutzerdefinierten Erkennungseinschränkungen](define-custom-recognition-constraints.md)
* [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Designer**
* [Cortana-Entwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Entwurfsrichtlinien für die Spracherkennung](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Beispiele**
* [Cortana-Sprachbefehlbeispiel](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=May16_HO2-->


