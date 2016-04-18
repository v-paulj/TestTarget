---
Description: Ein Statussteuerelement gibt dem Benutzer eine Rückmeldung, dass ein Vorgang mit langer Laufzeit ausgeführt wird.
title: Richtlinien für Statussteuerelemente
ms.assetid: FD53B716-C43D-408D-8B07-522BC1F3DF9D
label: Statussteuerelemente
template: detail.hbs
---
# Statussteuerelemente

Ein Statussteuerelement gibt dem Benutzer eine Rückmeldung, dass ein Vorgang mit langer Laufzeit ausgeführt wird. Eine *exakte* Statusanzeige zeigt den Prozentsatz für den Abschluss eines Vorgangs. Eine *unbestimmte* Statusanzeige (oder auch Statusring) zeigt an, dass ein Vorgang ausgeführt wird.

Ein Statussteuerelement ist schreibgeschützt und nicht interaktiv.

<span class="sidebar_heading" style="font-weight: bold;">Wichtige APIs</span>

-   [**ProgressBar-Klasse**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.aspx)
-   [**IsIndeterminate-Eigenschaft**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.isindeterminate.aspx)
-   [**ProgressRing-Klasse**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.aspx)
-   [**IsActive-Eigenschaft**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.isactive.aspx)

![Windows-App: unbestimmte Statusanzeige, Statusring und exakte Statusanzeige](images/ProgressBar.png)

Windows-App: unbestimmte Statusanzeige, Statusring und exakte Statusanzeige

![Windows Phone-App: Statusleisten-Statusanzeige und Statusanzeigen](images/wp_progress_bar.png)

Windows Phone-App: Statusleisten-Statusanzeige und Statusanzeigen

## Beispiele

Hier ist ein Beispiel eines Statusringsteuerelements auf einem Begrüßungsbildschirm.

![Screenshot mit einem standardmäßigen Statusringsteuerelement](images/ProgressBar_Standard.png)

Eine Statusleiste ist auch auch ein guter Indikator eines Status bzw. einer Position. Eine für einen Musiktitel verwendete Statusleiste entspricht der Zeitachse des Songs: Der Wert der Leiste entspricht der Position des Songs. Der angehaltene Status weist darauf hin, dass die Wiedergabe angehalten ist.

![Anzeige einer Statusleiste durch die Xbox Music-App beim Abspielen eines Songs](images/ProgressBar_MusicTimeline.png)

## Ist dies das richtige Steuerelement?

Die Verwendung eines Statussteuerelements ist nicht in jedem Fall erforderlich. Der Status einer Aufgabe kann auch so ausreichend zu erkennen oder der Vorgang so schnell abgeschlossen sein, dass eine Anzeige nur unnötig ablenken würde. Bei der Entscheidung, ob ein Statussteuerelement angezeigt werden sollte, sind einige Überlegungen zu berücksichtigen.

-   **Dauert der Vorgang mehr als zwei Sekunden?**

    Zeigen Sie in diesem Fall mit dem Start des Vorgangs ein Statussteuerelement an. Wenn ein Vorgang meistens (aber nicht immer) länger als zwei Sekunden dauert, warten Sie 500 ms, bevor Sie das Steuerelement anzeigen. Dadurch soll Flimmern vermieden werden.

-   **Wartet der Vorgang darauf, dass der Benutzer eine Aufgabe ausführt?**

    Wenn ja, zeigen Sie keine Statusleiste an. Statusleisten sind für den Status des Computers gedacht, nicht für den der Benutzer.

-   **Müssen Benutzer wissen, dass etwas geschieht?**

    Wenn die App z. B. im Hintergrund einen Download ausführt, der nicht vom Benutzer eingeleitet wurde, ist es auch nicht erforderlich, den Benutzer darüber zu informieren.

-   **Wird der Vorgang im Hintergrund ausgeführt, ohne die Aktivitäten des Benutzers zu blockieren, und ist er für Benutzer von geringem Interesse, aber nicht völlig irrelevant?**

    Verwenden Sie Text und Ellipsen, wenn die App Aufgaben ausführt, die zwar nicht immer sichtbar sein müssen, bei denen aber doch der Status angezeigt werden soll.

    ![Beispiel für Text als Statusanzeige](images/textprogress.png)

    Verwenden Sie die Ellipsen, um darauf hinzuweisen, dass eine Aufgabe ausgeführt wird. Bei mehreren Aufgaben oder Elementen können Sie die Anzahl der verbleibenden Aufgaben angeben. Wenn alle Aufgaben abgeschlossen sind, schließen Sie die Anzeige.

-   **Können Sie den Inhalt des Vorgangs verwenden, um den Status zu visualisieren?**

    Wenn ja, zeigen Sie kein Statussteuerelement an. Wenn beispielsweise vom Datenträger geladene „/src/assets“ angezeigt werden, erscheinen „/src/assets“ nacheinander auf dem Bildschirm. Das Anzeigen eines Statussteuerelements bringt hier keine Vorteile, sondern würde nur zu Lasten einer übersichtlichen Benutzeroberfläche gehen.

-   **Können Sie verhältnismäßig bestimmen, wie viel der gesamten Arbeit abgeschlossen ist, während der Vorgang verarbeitet wird?**

    Wenn dem so ist, verwenden Sie eine bestimmte Statusleiste, und zwar insbesondere für Vorgänge, die den Benutzer blockieren. Ansonsten sollten Sie eine unbestimmte Statusleiste bzw. einen Statusring verwenden. Es kann auch nützlich sein, wenn die Benutzer sehen, dass überhaupt etwas passiert.

## Erstellen von bestimmten Statussteuerelementen

Durch eine bestimmte Statusleiste wird angegeben, welchen Fortschritt die App erzielt hat. Mit Fortschreiten der Arbeit wird die Leiste aufgefüllt. Verwenden Sie eine exakte Statusanzeige, wenn Sie die verbleibende Arbeit (ausgedrückt als Dauer, Bytes, Dateien oder durch eine andere quantifizierbare Größe) abschätzen können.

Die Statusanzeige bietet diverse Eigenschaften für Einstellungen und das Bestimmen des Fortschritts:
- [**IsIndeterminate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.isindeterminate.aspx): Gibt an, ob die Statusanzeige unbestimmt ist. Durch Festlegen auf **false** erstellen Sie eine bestimmte Statusanzeige.
- [**Minimum**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.rangebase.minimum.aspx): Der Beginn des Wertebereichs. Der Standardwert ist 0.0.
- [**Maximum**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.rangebase.maximum.aspx): Das Ende des Wertebereichs. Der Standardwert ist 1.0. 
- [**Value**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.rangebase.value.aspx): Eine Zahl, die den aktuellen Fortschritt angibt. Wenn Sie den Status eines Dateidownloads anzeigen möchten, könnte dieser Wert auf die Anzahl heruntergeladener Byte festgelegt werden (legen Sie zusätzlich Maximum auf die Summe herunterzuladender Byte fest).
 
Das folgende Beispiel zeigt eine wertbasierte bestimmte Statusanzeige. 

```xaml
<ProgressBar IsIndeterminate="False" Maximum="100" Width="200"/>
```

```csharp
ProgressBar progressBar1 = new ProgressBar();
progressBar1.IsIndeterminate = false;
progressBar1.Maximum = 100;
progressBar1.Width = 200;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(progressBar1);
```

Den Wert einer Statusanzeige wird in der Regel nicht im Markup angegeben. Sie aktualisieren den Wert der Statusanzeige stattdessen im Code oder per Datenbindung anhand bestimmter Statusanzeigen. Wenn von Ihrer Statusanzeige beispielsweise angegeben wird, wie viele Dateien heruntergeladen wurden, wird der Wert jedes Mal aktualisiert, wenn eine weitere Datei heruntergeladen wurde.

## Erstellen von unbestimmten Statussteuerelementen

Wenn Sie nicht abschätzen können, wie lange das Fertigstellen einer Aufgabe dauert und die Aufgabe die Benutzerinteraktion nicht blockiert, verwenden Sie eine unbestimmte Statusleiste bzw. einen unbestimmten Statusring. Bei einer unbestimmten Statusleiste wird keine Leiste eingeblendet, die sich mit fortschreitendem Abschluss der Aufgabe füllt. Stattdessen wird eine Animation von Punkten angezeigt, die sich von links nach rechts bewegen. Ein unbestimmter Statusring besteht aus einer animierten Sequenz von Punkten, die sich im Kreis bewegen. 

Damit eine Statusleiste unbestimmt wird, legen Sie ihre [**IsIndeterminate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.isindeterminate.aspx)-Eigenschaft auf **true** fest.

```xaml
<ProgressBar IsIndeterminate="True" Width="200"/>
```

```csharp
ProgressBar progressBar1 = new ProgressBar();
progressBar1.IsIndeterminate = true;
progressBar1.Width = 200;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(progressBar1);
```

Um einen Statusring in der App anzuzeigen, legen Sie seine [**IsActive**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.isactive.aspx)-Eigenschaft auf **true** fest.

```xaml
<ProgressRing IsActive="True"/>
```

```csharp
ProgressRing progressRing1 = new ProgressRing();
progressRing1.IsActive = true;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(progressRing1);
```

## Empfehlungen

-   Verwenden Sie die exakte Statusanzeige für exakte Aufgaben, deren Dauer klar festgelegt ist oder die über ein vorhersehbares Ende verfügen. Verwenden Sie eine exakte Statusanzeige, wenn Sie beispielsweise die verbleibende Arbeit (ausgedrückt als Dauer, Bytes, Dateien oder durch eine andere quantifizierbare Größe) abschätzen können. Im Folgenden finden Sie einige Beispiele für bestimmte Aufgaben:

    -   Ein Foto mit einer Größe von 500 KB wird von der App heruntergeladen; bis jetzt wurden 100 KB empfangen.
    -   Eine Werbung mit einer Dauer von 15 Sekunden wird von der App angezeigt; bisher sind 2 Sekunden verstrichen.

    ![Beispiel für eine bestimmte Statusleiste](images/progress_determinate_bar.png)

-   Verwenden Sie den unbestimmten Statuskreis für nicht bestimmte und modale Aufgaben (blockiert Benutzerinteraktion).

    ![Beispiel für einen Statusring](images/progress_ring.png)

-   Verwenden Sie die unbestimmte Statusleiste für nicht bestimmte und nicht modale Aufgaben (blockiert Benutzerinteraktion nicht).

    ![Beispiel für eine unbestimmte Statusleiste](images/progress_indeterminate_bar.png)

-   Behandeln Sie teilweise modale Aufgaben als nicht modal, wenn der modale Status weniger als 2 Sekunden andauert. Bei einigen Aufgaben wird die Interaktion blockiert, bis ein Fortschritt erzielt wurde. Anschließend kann die Interaktion des Benutzers mit der App fortgesetzt werden. Wenn beispielsweise Benutzer eine Suchabfrage ausführen, wird die Interaktion bis zur Anzeige des ersten Ergebnisses blockiert. Behandeln Sie diese Aufgaben als nicht modal, und verwenden Sie den unbestimmten Statusleistenstil, wenn der modale Status weniger als 2 Sekunden andauert. Wenn der modale Status mehr als 2 Sekunden andauern kann, sollten Sie den unbestimmten Statusring für die modale Phase und die unbestimmte Statusleiste für die nicht modale Phase der Aufgabe verwenden.
-   Ziehen Sie in Erwägung, eine Möglichkeit zum Abbrechen oder Anhalten des ausgeführten Vorgangs bereitzustellen, insbesondere dann, wenn der Benutzer blockiert ist und auf den Abschluss des Vorgangs wartet und gut einschätzen kann, wie lange der Vorgang noch ausgeführt wird.
-   Verwenden Sie nicht den „Wartecursor“, um auf Aktivitäten hinzuweisen. Benutzer, die über Fingereingaben mit dem System interagieren, sehen diesen nicht, und Benutzer, die die Maus verwenden, benötigen keine zwei Methoden zum Visualisieren von Aktivitäten (Cursor und Statussteuerelement).
-   Zeigen Sie für mehrere verwandte aktive Aufgaben ein einziges Statussteuerelement an. Wenn auf dem Bildschirm mehrere verwandte Elemente angezeigt werden, die alle gleichzeitig eine Aktivität ausführen, zeigen Sie nicht mehrere Statussteuerelemente an. Zeigen Sie stattdessen nur ein Steuerelement an, das endet, wenn die letzte Aufgabe beendet ist. Wenn die App beispielsweise mehrere Fotos herunterlädt, sollten Sie nur ein einziges Statussteuerelement anstelle eines Steuerelements je Foto anzeigen.
-   Ändern Sie die Position oder die Größe des Statussteuerelements nicht, während die Aufgabe ausgeführt wird.

### Richtlinien für bestimmbare Aufgaben

-   Wenn es sich um einen modalen Vorgang handelt (ein Vorgang, der Benutzerinteraktionen blockiert) und dieser länger als 10 Sekunden dauert, bieten Sie dem Benutzer die Möglichkeit, den Vorgang abzubrechen. Die Option zum Abbrechen sollte zu Beginn des Vorgangs verfügbar sein.
-   Verteilen Sie Statusaktualisierungen gleichmäßig. Vermeiden Sie Situationen, in denen der Fortschritt über 80 Prozent anzeigt und sich dann für längere Zeit nicht mehr ändert. Der Fortschritt sollte sich zum Ende eines Vorgangs beschleunigen, nicht verlangsamen. Vermeiden Sie drastische Sprünge, beispielsweise von 0 auf 90 Prozent.
-   Wenn der Fortschritt 100 Prozent erreicht hat, warten Sie, bis die Animation der exakten Statusanzeige beendet ist, bevor Sie diese ausblenden
-   Wenn Ihre Aufgabe angehalten wurde (von einem Benutzer oder aufgrund einer externen Bedingung), aber vom Benutzer fortgesetzt werden kann, sollte dies dem Benutzer visuell signalisiert werden. In JavaScript-Apps wird dazu die CSS-Formatvorlage „win-paused“ verwendet. In C\#/C++/VB-Apps legen Sie dazu die „ShowPaused“-Eigenschaft auf „true“ fest. Zeigen Sie unter der Statusanzeige einen Statustext an, um den Benutzer darüber zu informieren, was gerade passiert.
-   Wenn die Aufgabe angehalten wurde und nicht fortgesetzt werden kann oder neu gestartet werden muss, sollte ein Fehler visuell signalisiert werden. In JavaScript-Apps wird dazu die CSS-Formatvorlage „win-error“ verwendet. In C\#/C++/VB-Apps legen Sie dazu die „ShowError“-Eigenschaft auf „true“ fest. Ersetzen Sie den Statustext (unter der Anzeige) durch eine Meldung, die den Benutzer darüber informiert, was passiert ist und wie er das Problem beheben kann (sofern möglich).
-   Wenn eine gewisse Zeit (oder eine Aufgabe) notwendig ist, bis mit der Angabe des exakten Status begonnen werden kann, sollten Sie zuerst die unbestimmte Leiste verwenden, um dann zur exakten Leiste zu wechseln. Wenn der erste Schritt bei einem Download beispielsweise der Aufbau der Serververbindung ist, können Sie nicht einschätzen, wie lange dieser Vorgang dauert. Nachdem die Verbindung aufgebaut wurde, wechseln Sie zur exakten Statusanzeige, um den Downloadstatus anzuzeigen. Achten Sie darauf, dass die Statusanzeige nach dem Wechsel an exakt derselben Position und in derselben Größe angezeigt wird.

    ![Wechsel von einer unbestimmten zu einer exakten Statusanzeige](images/progress_changing.png)

-   Wenn Sie über eine Liste mit Elementen verfügen, beispielsweise eine Liste mit Druckern, und bestimmte Aktionen einen Vorgang für Elemente in dieser Liste auslösen können (beispielsweise die Installation eines Treibers für einen der Drucker), zeigen Sie eine exakte Statusleiste neben dem Element an.

    Zeigen Sie den Inhalt (die Beschriftung) der Aufgabe über der Statusanzeige und den Status darunter an. Geben Sie keinen Statustext aus, wenn klar erkennbar ist, was gerade geschieht. Blenden Sie die Statusanzeige nach Beendigung des Vorgangs aus. Verwenden Sie den Statustext, um den Benutzer über den neuen Status einen Elements zu informieren.

    ![Inlineanzeige von Fortschritt und Status](images/progress_multiplebars.png)

-   Richten Sie zum Anzeigen einer Liste mit Aufgaben den Inhalt in einem Raster aus, damit die Benutzer den Status auf einen Blick erfassen können. Zeigen Sie Statusanzeigen für alle Elemente an – auch für solche, für die der Vorgang noch nicht begonnen hat.

    Da diese Liste dazu dient, laufende Vorgänge anzuzeigen, sollten Sie beendete Vorgänge aus der Liste entfernen.

    ![Anzeigen mehrerer Statusanzeigen](images/progress_bar_multiple.png)

-   Wenn ein Benutzer eine Aufgabe über die App-Leiste gestartet hat und die Benutzerinteraktion blockiert wird, zeigen Sie das Statussteuerelement in der App-Leiste an.

    Wenn klar ist, worauf sich die Statusleiste bezieht, können Sie diese oben an der App-Leiste ausrichten und auf eine Beschriftung sowie Angaben zum Status verzichten. Andernfalls sollten Sie sowohl eine Beschriftung als auch Statustext angeben.

    Deaktivieren Sie Interaktionen während der Aufgabe, indem Sie Steuerelemente auf der App-Leiste deaktivieren und Eingaben im Inhaltsbereich ignorieren.

-   Verringern Sie den Status nicht schrittweise. Steigern Sie den Statuswert immer. Wenn Sie eine Aktion umkehren müssen, zeigen Sie den Fortschritt der Umkehrung genauso an, wie Sie den Fortschritt jeder anderen Aktion anzeigen würden.
-   Starten Sie den Status (von 100 zu 0 %) nicht neu, es sei denn, es ist für den Benutzer offensichtlich, dass es sich beim aktuellen Schritt oder der Aufgabe nicht um den bzw. die letzte handelt. Nehmen wir beispielsweise an, eine Aufgabe besteht aus zwei Teilen: aus dem Herunterladen von Daten und dem Verarbeiten und Anzeigen der Daten. Nachdem der Download abgeschlossen ist, setzen Sie die Statusanzeige auf 0 Prozent zurück und beginnen Sie damit, den Fortschritt für die Verarbeitung der Daten anzuzeigen. Wenn es für die Benutzer nicht erkennbar ist, dass sich eine Aufgabe aus mehreren Schritten zusammensetzt, fassen Sie die Aufgaben in einer einzigen Skala von 0 bis 100 Prozent zusammen, und aktualisieren Sie den Statustext, sobald Sie von einer Aufgabe zur nächsten wechseln

### Richtlinien für den Statusring verwendende modale, unbestimmte Aufgaben

-   Zeigen Sie den Statusring im Kontext der Aktion an, d. h. nahe der Position, an der die Aktion vom Benutzer gestartet wurde oder an der die resultierenden Daten angezeigt werden.
-   Platzieren Sie den Statustext rechts neben dem Statusring.
-   Verwenden Sie für den Statusring die gleiche Farbe wie für den zugehörigen Statustext.
-   Deaktivieren Sie die Steuerelemente, mit denen der Benutzer während der Ausführung der Aufgabe nicht arbeiten soll.
-   Wenn die Aufgabe zu einem Fehler führt, blenden Sie die Statusanzeige und den Statustext aus, und zeigen Sie stattdessen an der gleichen Stelle eine Fehlermeldung an.
-   Wenn ein Vorgang in einem Dialogfeld abgeschlossen werden muss, bevor Sie zum nächsten Bildschirm fortfahren können, platzieren Sie den Statusring genau über dem Schaltflächenbereich, und zwar links ausgerichtet mit dem Inhalt des Dialogfelds.

    ![Status in einem Dialogfeld](images/prog_ring_dialog.png)

-   Platzieren Sie den Statusring in einem App-Fenster mit rechts ausgerichteten Steuerelementen links vom oder genau über dem Steuerelement, das die Aktion verursacht hat. Richten Sie den Statusring links am zugehörigen Inhalt aus.

    ![Anzeige des Status in einem App-Fenster mit rechts ausgerichteten Steuerelementen](images/prog_right_aligned_controls.png)

-   Platzieren Sie den Statusring in einem App-Fenster mit links ausgerichteten Steuerelementen rechts vom oder genau unter dem Steuerelement, das die Aktion verursacht hat.

    ![Ein Statusring mit links ausgerichteten Steuerelementen](images/prog_left_aligned_1.png)

    ![Ein Statusring unter links ausgerichteten Steuerelementen](images/prog_left_aligned_2.png)

-   Wenn Sie mehrere Elemente anzeigen, platzieren Sie den Statusring und den Statustext unter dem Titel des Elements. Ersetzen Sie den Statusring und den Statustext beim Auftreten eines Fehlers durch den Fehlertext.

    ![Eine Statusring in einer Liste mit mehreren Elementen](images/prog_ring_multiple.png)

### Richtlinien für die Statusleiste verwendende nicht modale, unbestimmte Aufgaben

-   Wenn Sie einen Status in einem Flyout anzeigen, platzieren Sie die unbestimmte Statusleiste am oberen Rand des Flyouts, und legen Sie die Breite so fest, dass sie sich über die gesamte Breite des Flyouts erstreckt. Durch diese Platzierung vermeiden Sie übermäßige Ablenkungen, aber der Benutzer wird weiterhin über die laufende Aktivität informiert Geben Sie dem Flyout keinen Titel. Ein Titel verhindert die Platzierung der Statusanzeige am oberen Rand des Flyouts.

    ![Unbestimmte Statusleiste in einem Flyout](images/prog_flyout_indeterminate_bar.png)

-   Wenn Sie den Status in einem App-Fenster anzeigen, platzieren Sie die unbestimmte Statusleiste oben im App-Fenster und über die gesamte Breite.

    ![Eine Statusleiste oben in einem App-Fenster](images/prog_indeterminate_bar_app_window.png)

### Richtlinien für Statustext

-   Wenn Sie die exakte Statusanzeige verwenden, geben Sie den Prozentsatz des Fortschritts nicht im Statustext an. Das Steuerelement liefert diese Information bereits.
-   Wenn Sie Text ohne Statussteuerelement zur Angabe einer Aktivität verwenden, sollten Sie durch eine Ellipse signalisieren, dass es sich um eine laufende Aktivität handelt.
-   Wenn Sie ein Statussteuerelement verwenden, ist eine Ellipse nicht erforderlich, da die laufende Aktivität in diesem Fall ja bereits durch das Statussteuerelement signalisiert wird.

### Richtlinien für Erscheinungsbild und Layout

-   Eine exakte Statusleiste wird als farbige Leiste angezeigt, die wächst, um eine graue Hintergrundleiste aufzufüllen. Die Proportion der gesamten Länge, die farbig ist, zeigt relativ gesehen an, in welchem Umfang der Vorgang abgeschlossen ist.
-   Eine unbestimmte Statusleiste oder ein -ring besteht aus sich kontinuierlich bewegenden Punkten.
-   Wählen Sie den Ort des Statussteuerelements und die Bedeutung anhand ihrer Wichtigkeit aus.

    Wichtige Statussteuerelemente können als Handlungsaufforderung fungieren. Sie weisen den Benutzer darauf hin, einen bestimmten Vorgang fortzusetzen, nachdem das System die Arbeit abgeschlossen hat. Einige integrierte Windows Phone-Apps verwenden einen Statusleisten-Statusindikator am obigen Bildschirmrand für wichtige Fälle. Sie können dies auch umsetzen und ihn als exakt oder unbestimmt konfigurieren.

    Weniger wichtige Fälle, beispielsweise beim Herunterladen, werden kleiner angezeigt und sind auf eine Anzeige begrenzt.

-   Verwenden Sie eine Bezeichnung, um den Statuswert anzuzeigen oder um den stattfindenden Prozess zu beschreiben oder um anzuzeigen, dass der Vorgang unterbrochen wurde. Eine Bezeichnung ist zwar optional, wird aber empfohlen.

    Verwenden Sie zum Beschreiben des stattfindenden Prozesses das Muster "Pflanzen von Gummibäumen".

    Verwenden Sie zum Anzeigen, dass der Status angehalten wurde oder eine Ausnahme erfahren hat, die Vergangenheitsform, beispielsweise „wurde angehalten“, „Download war fehlerhaft“ oder „wurde abgebrochen“.

-   Exakte Statusanzeige mit Beschriftung und Status

    ![Eine exakte Statusanzeige mit einer Beschriftung und Statusinformationen](images/progress_bar_determinate_redline.png)

-   Mehrere Statusanzeigen

    ![Empfohlenes Layout für mehrere Statusanzeigen](images/progress_bar_multi_redline.png)

-   Unbestimmter Statusring mit Statustext

    ![Layout für unbestimmten Statusring mit Statustext](images/progress_ring_status_text.png)

-   Unbestimmte Statusanzeige

    ![Layout für eine unbestimmte Statusanzeige](images/progress_indeterminate_bar_redline.png)

## Weitere Hinweise zur Verwendung

### Entscheidungsbaum zum Auswählen einer Statusformatvorlage

-   **Müssen Benutzer wissen, dass etwas geschieht?**

    Falls nicht, zeigen Sie kein Statussteuerelement an.

-   **Ist bekannt, wie lange es dauert, die Aufgabe abzuschließen?**
    -   **Ja:** **Dauert es länger als zwei Sekunden, bis die Aufgabe abgeschlossen ist?**
        -   **Ja:** Verwenden Sie eine bestimmte Statusanzeige. Stellen Sie für Aufgaben, die länger als 10 Sekunden dauern, eine Option zum Abbrechen der Aufgabe bereit.
        -   **Nein:** Zeigen Sie kein Statussteuerelement an.

    -   **Nein:** **Ist die Interaktion mit der UI für Benutzer blockiert, bis die Aufgabe abgeschlossen ist?**
        -   **Ja:** **Ist diese Aufgabe Teil eines Prozesses aus mehreren Schritten, bei dem der Benutzer spezifische Details zum Prozess wissen muss?**
            -   **Ja:** Verwenden Sie einen unbestimmten Statusring mit horizontalem Statustext in der Mitte des Bildschirms.
            -   **Nein:** Verwenden Sie einen unbestimmten Statusring ohne Text in der Mitte des Bildschirms.
        -   **Nein:** **Handelt es sich um eine primäre Aktivität?**
            -   **Ja:** **Hängt der Status mit einem einzelnen, spezifischen Element auf der UI zusammen?**
                -   **Ja:** Verwenden Sie einen unbestimmten Inlinestatusring mit Statustext neben dem zugehörigen UI-Element.
                -   **Nein:** **Wird eine große Datenmenge in eine Liste geladen?**
                    -   **Ja:** Verwenden Sie die unbestimmte Statusleiste oben mit Platzhaltern, um eingehende Inhalte darzustellen.
                    -   **Nein:** Verwenden Sie die unbestimmte Statusleiste oben im Bildschirm oder auf der Oberfläche.
            -   **Nein:** Verwenden Sie Statustext in einer Ecke oben im Bildschirm.

## Verwandte Artikel


- [**ProgressBar-Klasse**](https://msdn.microsoft.com/library/windows/apps/br227529)
- [**ProgressRing-Klasse**](https://msdn.microsoft.com/library/windows/apps/br227538)

**Für Entwickler (XAML)**
- [Hinzufügen von Statussteuerelementen](https://msdn.microsoft.com/library/windows/apps/xaml/hh780651)
- [So wird's gemacht: Erstellen einer benutzerdefinierten unbestimmten Statusleiste für Windows Phone](http://go.microsoft.com/fwlink/p/?LinkID=392426)


<!--HONumber=Mar16_HO1-->


