---
author: mijacobs
Description: Sound vervollständigt die Benutzererfahrung einer Anwendung und trägt zur Vereinheitlichung des Erscheinungsbilds von Windows auf allen Plattformen bei.
label: Sound
title: Sound
template: detail.hbs
ms.assetid: 9fa77494-2525-4491-8f26-dc733b6a18f6
extraBodyClass: style-sound
brief: Sound helps complete an application's user experience, and gives them that extra audio edge they need to match the feel of Windows across all platforms.
---
[Einige Informationen beziehen sich auf die Vorabversion, die vor der kommerziellen Freigabe möglicherweise wesentlichen Änderungen unterliegt. Microsoft übernimmt keine Garantie, weder ausdrücklicher noch impliziter Art, für die hier bereitgestellten Informationen.]*Dieser Artikel enthält eine Vorschau von Features, die noch nicht zur Verfügung stehen.*

# Sound in UWP-Apps

Sound vervollständigt die Benutzererfahrung einer Anwendung und trägt zur Vereinheitlichung des Erscheinungsbilds von Windows auf allen Plattformen bei.

## Globale Sound-API
UWP bietet ein einfach zugängliches Soundsystem, bei dem Sie einfach "einen Schalter umlegen" können und ein beeindruckendes Audioerlebnis für Ihre gesamte App erhalten.

Der **ElementSoundPlayer** ist ein integriertes Soundsystem innerhalb von XAML, und wenn es aktiviert ist, spielen alle Standardsteuerelemente automatisch Sounds ab.
```C#
ElementSoundPlayer.State = ElementSoundPlayerState.On;
```
Der **ElementSoundPlayer** verfügt über drei Zustände: **Ein****Aus** und **Auto**.

In der Einstellung **Aus** wird unabhängig davon, wo Ihre App ausgeführt wird, niemals Sound wiedergegeben. In der Einstellung **Ein** werden für Sounds für Ihre App auf jeder Plattform wiedergegeben.

### Sound für TV und Xbox

Sound ist ein wesentlicher Bestandteil der 10-Fuß-Schnittstelle. Standardmäßig verwendet der **ElementSoundPlayer** den Zustand **Auto**, was bedeutet, dass Sound nur dann wiedergegeben wird, wenn Ihre App auf Xbox ausgeführt wird.
Weitere Informationen zur Funktionsweise von Sound für TV und Xbox finden Sie im Artikel [Entwerfen für Xbox und Fernsehgeräte](http://go.microsoft.com/fwlink/?LinkId=760736).

## Lautstärkeüberschreibung
Mit dem **Volume**-Steuerelement kann die Lautstärke aller Sounds innerhalb der App verringert werden. Allerdings können Sounds innerhalb der App nicht *lauter als die Systemlautstärke* werden.

Rufen Sie zum Festlegen der Lautstärke der App auf:
```C#
ElementSoundPlayer.Volume = 0.5f;
```
Wobei die maximale Lautstärke (relativ zur Systemlautstärke) 1,0 ist und die minimale 0,0 ist (im Wesentlichen stumm).

## Zustand des Steuerungsgrads
Wenn der Standardsound eines Steuerelements nicht erwünscht ist, kann er deaktiviert werden. Dies erfolgt über die **ElementSoundMode**-Option für das Steuerelement.

Die **ElementSoundMode**-Option verfügt über zwei Zustände: **Aus** und **Standard**. Wenn nichts festgelegt wird, hat sie den Zustand **Standard**. In der Einstellung **Aus** wird jeder Sound, der durch das Steuerelement wiedergegeben werden soll, stumm geschaltet, *außer der Fokus*.

```XAML
<Button Name="ButtonName" Content="More Info" ElementSoundMode="Off"/>
```

```C#
ButtonName.ElementSoundState = ElementSoundMode.Off;
```

## Ist das der richtige Sound?
Beim Erstellen eines benutzerdefinierten Steuerelements oder Ändern des Sounds eines vorhandenen Steuerelements ist es wichtig zu verstehen, wie alle Sounds, die das System bietet, verwendet werden.

Jeder Sound bezieht sich auf eine bestimmte grundlegende Benutzerinteraktion, und obwohl Sounds angepasst werden können, um bei einer Interaktion wiedergegeben zu werden, soll dieser Abschnitt die Szenarien erläutern, in denen Sounds verwendet werden sollten, um die Einheitlichkeit aller UWP-Apps zu gewährleisten.

### Aufrufen eines Elements
Der in unserem System am häufigsten verwendete durch ein Steuerelement ausgelöste Sound ist der **Invoke**-Sound. Dieser Sound wird wiedergegeben, wenn ein Benutzer mittels Tippen/Klicken/Eingabe/Leertaste oder Drücken der Taste "A" auf einem Gamepad ein Steuerelement aufruft.

In der Regel wird dieser Sound nur wiedergegeben, wenn ein Benutzer explizit ein einfaches Steuerelement oder ein Steuerelementteil über ein [Eingabegerät](/input-and-devices/guidelines-for-interactions/) anzielt.

<SelectButtonClick.mp3 Audioclip hier>

Um diesen Sound von einem Steuerelementereignis aus wiederzugeben, rufen Sie einfach die Play-Methode aus dem **ElementSoundPlayer** auf und übergeben sie an **ElementSound.Invoke**:
```C#
ElementSoundPlayer.Play(ElementSoundKind.Invoke);
```

### Anzeigen und Ausblenden von Inhalten
In XAML gibt es viele Flyouts, Dialogfelder und leicht ausblendbare Benutzeroberflächen, und jede Aktion, die eine dieser Überlagerungen auslöst, sollte einen **Show**- oder **Hide**-Sound aufrufen.

Wenn ein Überlagerungs-Inhaltsfenster eingeblendet wird, sollte der **Show**-Sound aufgerufen werden:

<OverlayIn.mp3 Audioclip hier>

```C#
ElementSoundPlayer.Play(ElementSoundKind.Show);
```
Wenn dagegen ein Überlagerungs-Inhaltsfenster geschlossen wird (oder einfach ausgeblendet wird), sollte der **Hide**-Sound aufgerufen werden:

<OverlayOut.mp3 Audioclip hier>

```C#
ElementSoundPlayer.Play(ElementSoundKind.Hide);
```
### Navigation innerhalb einer Seite
Beim Navigieren zwischen Bereichen oder Ansichten innerhalb einer App-Seite (siehe [Hub](/controls-and-patterns/hub/) oder [Registerkarten und Pivots](/controls-and-patterns/tabs-pivot/)) gibt es in der Regel eine bidirektionale Bewegung. Das bedeutet, dass Sie zur nächsten Ansicht bzw. zum nächsten Bereich (oder zur/zum vorherigen) wechseln können, ohne die aktuelle App-Seite zu verlassen, auf der Sie sich befinden.

Das Audioerlebnis für dieses Navigationskonzept wird durch die **MovePrevious**- und **MoveNext**-Sounds umgesetzt.

Beim Wechsel zu einer Ansicht bzw. zu einem Bereich, die/der als das *nächste Element* in einer Liste gilt, rufen Sie auf:

<PageTransitionRight.mp3 Audioclip hier>

```C#
ElementSoundPlayer.Play(ElementSoundKind.MoveNext);
```
Und beim Wechsel zu einer vorherigen Ansicht bzw. zu einem vorherigen Bereich in einer Liste, die/der als das *vorherige Element* gilt, rufen Sie auf:

<PageTransitionLeft.mp3 Audioclip hier>

```C#
ElementSoundPlayer.Play(ElementSoundKind.MovePrevious);
```
### Rückwärtsnavigation
Beim Navigieren von der aktuellen Seite zur vorherigen Seite innerhalb einer App sollte der **GoBack**-Sound aufgerufen werden:

<BackButtonClick.mp3 Audioclip hier>

```C#
ElementSoundPlayer.Play(ElementSoundKind.GoBack);
```
### Fokussieren auf ein Element
Der **Focus**-Sound ist der einzige implizite Sound in unserem System. Das heißt, dass ein Benutzer nicht mit irgendetwas direkt interagiert, jedoch trotzdem einen Sound hört.

Das Fokussieren geschieht, wenn ein Benutzer durch eine App navigiert – dies kann mit dem Gamepad, der Tastatur, der Fernbedienung oder mit Kinect geschehen. In der Regel wird der **Focus**-Sound *bei PointerEntered- oder Mauszeigeereignissen nicht wiedergegeben*.

Um ein Steuerelement zur Wiedergabe des **Focus**-Sounds einzurichten, wenn Ihr Steuerelement den Fokus erhält, rufen Sie auf:

<ElementFocus1.mp3 Audioclip hier>

```C#
ElementSoundPlayer.Play(ElementSoundKind.Focus);
```
### Wechseln zwischen Focus-Sounds
Als zusätzliche Funktion zum Aufrufen von **ElementSound.Focus** wechselt das Soundsystem standardmäßig bei jedem Navigationsaufruf zwischen 4 unterschiedlichen Sounds. Das bedeutet, dass keine zwei genau gleichen Focus-Sounds direkt nacheinander wiedergegeben werden.

Diese Wechselfunktion soll verhindern, dass die Focus-Sounds monoton werden und durch den Benutzer nicht mehr wahrgenommen werden. Denn Focus-Sounds werden am häufigsten wiedergegeben und sollten daher möglichst unaufdringlich sein.

## Verwandte Artikel
* [Entwerfen für Xbox und Fernsehgeräte](http://go.microsoft.com/fwlink/?LinkId=760736)


<!--HONumber=May16_HO2-->


