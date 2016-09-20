---
author: Jwmsft
Description: "Ein Statussteuerelement gibt dem Benutzer eine Rückmeldung, dass ein Vorgang mit langer Laufzeit ausgeführt wird."
title: "Richtlinien für Statussteuerelemente"
ms.assetid: FD53B716-C43D-408D-8B07-522BC1F3DF9D
label: Progress controls
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: de18267de982ef50913ed605e7a9272f7f8c476b

---
# Statussteuerelemente

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Ein Statussteuerelement gibt dem Benutzer eine Rückmeldung, dass ein Vorgang mit langer Laufzeit ausgeführt wird. Dies kann bedeuten, dass der Benutzer bei Anzeigen der Statusanzeige nicht mit der App interagieren kann. Je nach verwendetem Indikator wird auch die Länge der Wartezeit angegeben.

<div class="important-apis" >
<b>Wichtige APIs</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.aspx"><strong>ProgressBar-Klasse</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.isindeterminate.aspx"><strong>IsIndeterminate-Eigenschaft</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.aspx"><strong>ProgressRing-Klasse</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.isactive.aspx"><strong>IsActive-Eigenschaft</strong></a></li>
</ul>

</div>
</div>






## Typen von Statussteuerelementen

Es gibt zwei Steuerelemente, die dem Benutzer anzeigen, dass ein Vorgang ausgeführt wird: ein ProgressBar-Element oder ein ProgressRing-Element.

-   Der ProgressBar-Status *bestimmt* gibt den Prozentsatz an, zu dem eine Aufgabe abgeschlossen ist. Dieses Element für Vorgänge verwendet werden, deren Dauer bekannt ist, deren Fortschritt jedoch nicht die Interaktion des Benutzers mit der App blockieren sollte.
-   Der ProgressBar-Status *unbestimmt* gibt an, dass ein Vorgang ausgeführt wird, der die Interaktion des Benutzers mit der App nicht blockiert und dessen Abschlusszeit nicht bekannt ist.
-   Für das ProgressRing-Element gibt es nur den Status *unbestimmt*. Es sollte verwendet werden, wenn vor dem Abschluss eines Vorgangs keine Benutzerinteraktion möglich ist.

Ein Statussteuerelement ist zudem schreibgeschützt und nicht interaktiv. Dies bedeutet, dass der Benutzer diese Steuerelemente nicht direkt aufrufen oder verwenden kann.

![ProgressBar-Status](images/ProgressBar_TwoStates.png)

*Von oben nach unten – unbestimmtes ProgressBar-Element und bestimmtes ProgressBar-Element*

![ProgressRing-Status](images/ProgressRing_SingleState.png)

*Ein unbestimmtes ProgressRing-Element*

## Verwendung der einzelnen Steuerelemente

Es ist nicht immer klar erkennbar, welches Steuerelement oder welcher Status (bestimmt oder unbestimmt) zu verwenden ist, um einen Vorgang anzuzeigen. Manchmal ist eine Aufgabe so deutlich zu erkennen, dass kein Statussteuerelement erforderlich ist. Manchmal ist jedoch auch bei Verwendung eines Statussteuerelements eine Textzeile erforderlich, die den Benutzer darüber informiert, welcher Vorgang gerade ausgeführt wird.

### ProgressBar
-   **Verfügt das Steuerelement über eine festgelegte Dauer oder ein vorhersehbares Ende?**

    Verwenden Sie in diesem Fall eine bestimmte Statusanzeige, und aktualisieren Sie deren Prozentsatz oder Wert entsprechend.

-   **Kann der Benutzer fortfahren, ohne den Status des Vorgang zu überwachen?**

    Bei Verwendung eines ProgressBar-Elements ist die Interaktion nicht modal. Das bedeutet in der Regel, dass der Benutzer durch den Abschluss des Vorgangs nicht blockiert wird und die aktive App bis zum Abschluss der Aktion weiterhin verwenden kann.

-   **Schlüsselwörter**

    Wenn der anzuzeigende Vorgang sich mit den folgenden Schlüsselwörtern in Verbindung bringen lässt oder wenn Sie während des Vorgangs Text anzeigen möchten, in dem diese Schlüsselwörter vorkommen, sollten Sie ein ProgressBar-Element verwenden:

    - *Wird geladen...*
    - *Wird abgerufen...*
    - *In Bearbeitung...*

### ProgressRing

-   **Muss der Benutzer den Abschluss des Vorgangs abwarten, bevor er seine Aktivität fortsetzen kann?**

    Wenn ein Vorgang bis zu seinem Abschluss eine umfassende Interaktion mit der App erfordert, empfiehlt sich die Verwendung eines ProgressRing-Elements. Das ProgressRing-Steuerelement ist für modale Interaktionen vorgesehen, in denen die Aktivitäten des Benutzers blockiert werden, bis das ProgressRing-Element nicht mehr angezeigt wird.

-   **Wartet die App darauf, dass der Benutzer eine Aufgabe ausführt?**

    In diesem Fall verwenden Sie ein ProgressRing-Steuerelement, um den Benutzer auf eine unbestimmte Wartezeit hinzuweisen.

-   **Schlüsselwörter**

    Wenn der anzuzeigende Vorgang sich mit den folgenden Schlüsselwörtern in Verbindung bringen lässt oder wenn Sie während des Vorgangs Text anzeigen möchten, in dem diese Schlüsselwörter vorkommen, sollten Sie ein ProgressRing-Element verwenden:

    - *Wird aktualisiert*
    - *Anmelden...*
    - *Verbinden...*

### Keine Fortschrittsanzeige erforderlich
-   **Müssen Benutzer wissen, dass Vorgänge ausgeführt werden?**

    Wenn die App z.B. im Hintergrund einen Download ausführt, der nicht vom Benutzer eingeleitet wurde, ist es auch nicht unbedingt erforderlich, den Benutzer darüber zu informieren.

-   **Wird der Vorgang im Hintergrund ausgeführt, ohne die Aktivitäten des Benutzers zu blockieren, und ist er für Benutzer von geringem Interesse, aber nicht völlig irrelevant?**

    Verwenden Sie Text, wenn die App Aufgaben ausführt, die zwar nicht immer sichtbar sein müssen, bei denen aber der Status angezeigt werden soll.

-   **Möchte der Benutzer nur über den Abschluss des Vorgangs informiert werden?**

    Manchmal ist es am besten, nur auf den Abschluss eines Vorgangs hinzuweisen oder den unmittelbaren Abschluss des Vorgangs durch ein visuelles Element anzukündigen, und den restlichen Vorgang im Hintergrund auszuführen.

## Bewährte Methoden für Statussteuerelemente

Manchmal ist eine visuelle Darstellung hilfreich, um zu ermitteln, zu welchem Zeitpunkt welches Statussteuerelement verwendet werden sollte:

**ProgressBar – bestimmt**

![Beispiel für ein bestimmtes ProgressBar-Element](images/PB_DeterminateExample.png)

Beim ersten Beispiel handelt es sich um ein bestimmtes ProgressBar-Steuerelement. Wenn die Dauer des Vorgangs bekannt ist, etwa beim Installieren, Herunterladen oder Einrichten, eignet sich ein bestimmtes ProgressBar-Steuerelement.

**ProgressBar – unbestimmt**

![Beispiel für ein unbestimmtes ProgressBar-Element](images/PB_IndeterminateExample.png)

Wenn die Dauer des Vorgangs nicht bekannt ist, verwenden Sie ein unbestimmtes ProgressBar-Steuerelement. Unbestimmte ProgressBar-Elemente können auch beim Ausfüllen virtualisierter Listen verwendet werden oder um einen glatten visuellen Übergang von einem unbestimmten zu einem bestimmten ProgressBar-Element zu erstellen.

-   **Befindet sich der Vorgang in einer virtualisierten Sammlung?**

    In diesem Fall legen Sie nicht für jedes angezeigte Listenelement eine Statusanzeige fest. Positionieren Sie stattdessen ein ProgressBar-Element am Anfang der Auflistung der zu ladenden Elemente, um anzuzeigen, dass die Elemente geladen werden.

**ProgressRing – unbestimmt**

![Beispiel für ein unbestimmtes ProgressRing-Steuerelement](images/PR_IndeterminateExample.png)

Unbestimmte ProgressRing-Elemente werden verwendet, wenn jegliche Benutzerinteraktion mit der App ausgesetzt ist oder die App auf eine Benutzereingabe wartet, um den Vorgang fortzusetzen. Das „Anmelden...“- Beispiel oben ist ein optimales Szenario für das ProgressRing-Steuerelement, da der Benutzer die App erst weiterverwenden kann, nachdem der Anmeldevorgang abgeschlossen ist.

## Anpassen eines Statussteuerelements

Die beiden Statussteuerelemente sind recht einfach gehalten. Bei verschiedenen visuellen Features der Steuerelemente sind jedoch deren Anpassungsoptionen nicht offensichtlich.

**Ändern der Größe des ProgressRing-Elements**

Sie können das ProgressRing-Element so groß gestalten, wie Sie möchten. Die Mindestgröße beträgt jedoch 20x20Epx. Um die Größe eines ProgressRing-Elements zu ändern, müssen Sie dessen Höhe und Breite festlegen. Wenn nur die Höhe oder nur die Breite festgelegt wird, erhält das Steuerelement die Mindestgröße (20x20Epx). Wenn die Höhe und die Breite auf zwei verschiedene Größen festgelegt sind, wird die kleinere Größen verwendet.
Um sicherzustellen, dass Ihr ProgressRing-Steuerelement seinen Zweck erfüllt, legen Sie für die Höhe und die Breite denselben Wert fest:

```XAML
<ProgressRing Height="100" Width="100"/>
```

Wenn das ProgressRing-Element sichtbar und animiert sein soll, legen Sie die IsActive-Eigenschaft auf „true“ fest:

```XAML
<ProgressRing IsActive="True" Height="100" Width="100"/>
```

```C#
progressRing.IsActive = true;
```

**Farben der Statussteuerelemente**

Standardmäßig ist die Grundfarbe der Statussteuerelemente auf die Akzentfarbe des Systems festgelegt. Dies können Sie anpassen, indem Sie einfach die „Foreground“-Eigenschaft der Steuerelemente ändern.

```XAML
<ProgressRing IsActive="True" Height="100" Width="100" Foreground="Blue"/>
<ProgressBar Width="100" Foreground="Green"/>
```

Durch das Ändern die Vordergrundfarbe des ProgressRing-Elements ändern sich die Farben der Punkte. Die „Foreground“-Eigenschaft des ProgressBar-Elements ändert dann die Füllfarbe der Leiste. Um den ungefüllten Teil der Leiste zu ändern, überschreiben Sie einfach die „Background“-Eigenschaft.

**Anzeigen eines Wartecursors**

Manchmal ist es am besten, nur kurz einen Wartecursor anzuzeigen, wenn eine App oder ein Vorgang etwas Zeit erfordert und Sie dem Benutzer anzeigen müssen, dass mit der App oder dem Bereich, in dem sich der Wartecursor befindet, bis zu dessen Ausblenden nicht interagiert werden sollte.

```C#
Window.Current.CoreWindow.PointerCursor = new Windows.UI.Core.CoreCursor(Windows.UI.Core.CoreCursorType.Wait, 10);
```

## Verwandte Artikel


- [**ProgressBar-Klasse**](https://msdn.microsoft.com/library/windows/apps/br227529)
- [**ProgressRing-Klasse**](https://msdn.microsoft.com/library/windows/apps/br227538)

**Für Entwickler (XAML)**
- [Hinzufügen von Statussteuerelementen](https://msdn.microsoft.com/library/windows/apps/xaml/hh780651)
- [So wird's gemacht: Erstellen einer benutzerdefinierten unbestimmten Statusleiste für Windows Phone](http://go.microsoft.com/fwlink/p/?LinkID=392426)



<!--HONumber=Aug16_HO3-->


