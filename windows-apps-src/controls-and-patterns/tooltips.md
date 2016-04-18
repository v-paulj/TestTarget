---
Description: Verwenden Sie eine QuickInfo, damit der Benutzer weitere Informationen über ein Steuerelement erhält, bevor er zum Ausführen einer Aktion aufgefordert wird.
title: QuickInfos
ms.assetid: A21BB12B-301E-40C9-B84B-C055FD43D307
label: Tooltips
template: detail.hbs

---

# QuickInfos

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Eine QuickInfo ist eine kurze Beschreibung, die mit einem anderen Steuerelement oder Objekt verknüpft ist. QuickInfos erläutern Benutzern unbekannte Objekte, die in der Benutzeroberfläche nicht direkt beschrieben werden. Sie wird automatisch angezeigt, wenn der Benutzer den Fokus auf ein Steuerelement verschiebt, es gedrückt hält oder mit dem Mauszeiger darauf zeigt. Sie wird nach einigen Sekunden wieder ausgeblendet, wenn der Benutzer den Finger, den Mauszeiger oder den Tastatur-/Gamepad-Fokus bewegt.

![Eine QuickInfo](images/controls/tool-tip.png)

<span class="sidebar_heading" style="font-weight: bold;">Wichtige APIs</span>

-   [**QuickInfo-Klasse**](https://msdn.microsoft.com/library/windows/apps/br227608)
-   [**ToolTipService-Klasse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.tooltipservice)

## Ist dies das richtige Steuerelement?

Verwenden Sie eine QuickInfo, damit der Benutzer weitere Informationen über ein Steuerelement erhält, bevor er zum Ausführen einer Aktion aufgefordert wird. QuickInfo-Steuerelemente sollten sparsam verwendet werden, vor allem nur dann, wenn sie für Benutzer, die eine bestimmte Aufgabe ausführen möchten, wirklich nützlich sind. Es gilt folgende Faustregel: Für Informationen, die bereits an einer anderen Stelle derselben Benutzeroberfläche vermittelt werden, benötigen Sie keine QuickInfo. Eine nützliche QuickInfo bietet klärende Informationen zu einer unklaren Aktion.

Wann sollten Sie eine QuickInfo verwenden? Orientieren Sie sich an folgenden Fragen:

-   **Sollen die Informationen beim Daraufzeigen sichtbar werden?**
    Wenn dies nicht erwünscht ist, verwenden Sie ein anderes Steuerelement. QuickInfos sollte niemals ohne vorhergehende Aktion angezeigt werden, sondern immer als Ergebnis einer Benutzerinteraktion.

-   **Ist das Steuerelement mit Text beschriftet?**
    Wenn dies nicht der Fall ist, fügen Sie die Beschriftung als QuickInfo hinzu. Beim UX-Entwurf empfiehlt es sich, die meisten Steuerelemente inline zu beschriften, sodass QuickInfos dafür nicht erforderlich sind. Steuerelemente in Symbolleisten sowie nur Symbole zeignde Befehlsschaltflächen müssen dagegen mit QuickInfos versehen werden.

-   **Wären eine Beschreibung oder zusätzliche Informationen zu einem Objekt hilfreich?**
    Verwenden Sie in diesem Fall eine QuickInfo. Der Text ist aber nur als Ergänzung gedacht – wichtige Informationen für die Hauptaufgaben dürfen nicht nur als QuickInfo vorhanden sein. Fügen Sie wichtige Informationen direkt in die Benutzeroberfläche ein, damit Benutzer nicht erst danach suchen müssen.

-   **Handelt es sich bei den ergänzenden Informationen um Fehler-, Warn- oder Statusmeldungen?**
    Verwenden Sie in diesem Fall ein anderes Benutzeroberflächenelement, beispielsweise ein Flyout.

-   **Müssen die Benutzer mit den Informationen interagieren?**
    Verwenden Sie in diesem Fall ein anderes Steuerelement. Eine Benutzerinteraktion mit QuickInfo ist nicht möglich, da die Informationen beim Bewegen der Maus ausgeblendet werden.

-   **Müssen die Benutzer die ergänzenden Informationen drucken?**
    Verwenden Sie in diesem Fall ein anderes Steuerelement.

-   **Ist anzunehmen, dass sich die Benutzer durch QuickInfo gestört oder abgelenkt fühlen?**
    Ziehen Sie in diesem Fall eine andere Lösung in Betracht. Möglicherweise können Sie ganz auf die zusätzlichen Informationen verzichten. Wenn Sie QuickInfos verwenden, obwohl dies die Benutzer möglicherweise irritiert, geben Sie den Benutzern die Möglichkeit, die QuickInfos zu deaktivieren.

## Beispiel

Eine QuickInfo in der Bing-Karten-App.

![Eine QuickInfo in der Bing-Karten-App](images/control-examples/tool-tip-maps.png)

## Empfehlungen

-   Verwenden Sie QuickInfos sparsam (oder gar nicht). QuickInfos stellen eine Unterbrechung dar. Eine QuickInfo kann genauso ablenkend wirken wie ein Popup. Verwenden Sie sie daher nur, wenn sie wirklich von Bedeutung sind.
-   Halten Sie den QuickInfo-Text kurz. QuickInfos eignen sich gut für kurze Sätze und Satzfragmente. Längere Textblöcke können überfrachtet wirken, sodass ein Timeout für die QuickInfo auftritt, bevor der Benutzer sie zu Ende gelesen hat.
-   Erstellen Sie hilfreiche QuickInfo-Texte mit ergänzenden Informationen. Der QuickInfo-Text muss informativ sein. Verwenden Sie keine selbstverständlichen oder bereits auf dem Bildschirm vorhandenen Informationen als QuickInfo-Text. Da der QuickInfo-Text nicht immer angezeigt wird, sollte er nur ergänzende Informationen enthalten, die nicht unbedingt gelesen werden müssen. Teilen Sie wichtige Informationen in Form von selbsterklärenden Beschriftungen für Steuerelemente oder direkt eingefügtem ergänzendem Text mit.
-   Verwenden Sie ggf. Bilder als QuickInfo. Manchmal ist ein Bild aussagekräftiger als Text. Wenn der Benutzer z. B. auf einen Hyperlink zeigt, können Sie als QuickInfo eine Vorschau der verknüpften Seite anzeigen.
-   Verwenden Sie die QuickInfo nicht, um bereits in der UI vorhandene Informationen anzuzeigen. Versehen Sie z. B. keine Schaltfläche mit QuickInfo-Text, der bereits auf der Schaltfläche selbst angezeigt wird.
-   Fügen Sie in QuickInfos keine interaktiven Steuerelemente ein.
-   Fügen Sie in QuickInfos keine Bilder ein, die wie interaktive Steuerelemente aussehen.

<span id="related_topics"></span>Verwandte Themen
-----------------------------------------------

* [**QuickInfo-Klasse**](https://msdn.microsoft.com/library/windows/apps/br227608)


<!--HONumber=Mar16_HO4-->


