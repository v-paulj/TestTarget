---
Description: Description: Hier erfahren Sie, wie Sie die UI Ihrer App beim Ein- oder Ausblenden der Touch-Bildschirmtastatur anpassen können.
title: title: Reagieren auf das Vorhandensein der Bildschirmtastatur
ms.assetid: 70C6130E-23A2-4F9D-88E7-7060062DA988
label: Respond to the presence of the touch keyboard
template: detail.hbs
---

# ms.assetid: 70C6130E-23A2-4F9D-88E7-7060062DA988


label: Reagieren auf das Vorhandensein der Bildschirmtastatur template: detail.hbs


**Reagieren auf das Vorhandensein der Bildschirmtastatur**

-   [**\[ Aktualisiert für UWP-Apps unter Windows 10.**](https://msdn.microsoft.com/library/windows/apps/br209185)
-   [**Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]**](https://msdn.microsoft.com/library/windows/apps/br242255)

Wichtige APIs

![AutomationPeer](images/touchkeyboard-standard.png)

InputPane

Hier erfahren Sie, wie Sie die UI Ihrer App beim Ein- oder Ausblenden der Touch-Bildschirmtastatur anpassen können. Bildschirmtastatur mit Standard-Layout <sup>Bildschirmtastatur\\ mit\\ Standard-Layout</sup>

Die Bildschirmtastatur ermöglicht Texteingabe für Geräte, die Toucheingabe unterstützen. Texteingabesteuerelemente der Universellen Windows-Plattform (UWP) rufen standardmäßig die Bildschirmtastatur auf, wenn ein Benutzer auf ein bearbeitbares Eingabefeld tippt.

Die Bildschirmtastatur bleibt in der Regel sichtbar, während der Benutzer zwischen Steuerelementen in einem Formular navigiert. Dieses Verhalten kann jedoch je nach Steuerelementtypen innerhalb des Formulars variieren.

Um das entsprechende Verhalten der Bildschirmtastatur in einem benutzerdefinierten Texteingabe-Steuerelement zu unterstützen, das nicht von einem standardmäßigen Texteingabe-Steuerelement abgeleitet ist, müssen Sie die [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/br209185)-Klasse verwenden, um die Steuerelemente für die Microsoft-Benutzeroberflächenautomatisierung verfügbar zu machen und die richtigen Steuerelementmuster der Benutzeroberflächenautomatisierung implementieren.

Weitere Informationen finden Sie unter [Barrierefreiheit für den Tastaturzugriff](https://msdn.microsoft.com/library/windows/apps/mt244347) und [Benutzerdefinierte Automatisierungspeers](https://msdn.microsoft.com/library/windows/apps/mt297667).

Wenn diese Unterstützung für Ihr benutzerdefiniertes Steuerelement hinzugefügt wurde, können Sie auf das Vorhandensein der Bildschirmtastatur entsprechend reagieren.

**Voraussetzungen:**

-   [Dieses Thema baut auf [Tastaturinteraktionen](keyboard-interactions.md) auf.](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Sie sollten über ein grundlegendes Verständnis der standardmäßigen Tastaturinteraktionen, das Behandeln von Tastatureingaben und Ereignissen und der Benutzeroberflächenautomatisierung verfügen.

Wenn Sie noch keine Erfahrung mit der Entwicklung von UWP-Apps (Universelle Windows-Plattform) haben, sollten Sie sich in den folgenden Themen zunächst mit den hier besprochenen Technologien vertraut machen.

Erstellen Ihrer ersten App

## Informationen zu Ereignissen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](https://msdn.microsoft.com/library/windows/apps/mt185584).


**Richtlinien für die Benutzerfreundlichkeit:**

-   Nützliche Tipps zum Entwerfen einer zweckmäßigen und ansprechenden für die Tastatureingabe optimierten App finden Sie unter [Richtlinien für den Tastaturentwurf](https://msdn.microsoft.com/library/windows/apps/hh972345).

-   <span id="Touch_keyboard_and_a_custom_UI"></span><span id="touch_keyboard_and_a_custom_ui"></span><span id="TOUCH_KEYBOARD_AND_A_CUSTOM_UI"></span>Bildschirmtastatur und benutzerdefinierte Benutzeroberfläche Im Folgenden finden Sie einige grundlegende Empfehlungen für benutzerdefinierte Steuerelemente für die Texteingabe.

-   Zeigen Sie die Bildschirmtastatur während der gesamten Interaktion mit dem Formular an. Stellen Sie sicher, dass Ihre benutzerdefinierten Steuerelemente über den richtigen [**AutomationControlType**](https://msdn.microsoft.com/library/windows/apps/br209182) für die Benutzeroberflächenautomatisierung verfügen, um zu gewährleisten, dass die Tastatur eingeblendet bleibt, wenn der Fokus während der Texteingabe vom Texteingabefeld verschoben wird.

-   Wenn Sie zum Beispiel wünschen, dass die Tastatur eingeblendet bleibt, wenn während des Texteingabeszenarios ein Menü geöffnet wird, muss das Menü den **AutomationControlType** „Menu“ aufweisen.

    Vermeiden Sie die Manipulation der Eigenschaften der Benutzeroberflächenautomatisierung zur Kontrolle der Bildschirmtastatur.

    Andere Werkzeuge für die Barrierefreiheit sind von der Genauigkeit der Eigenschaften der Benutzeroberflächenautomatisierung abhängig.

    ![Stellen Sie sicher, dass Benutzer das Eingabefeld, mit dem sie interagieren, immer sehen können.](images/touch-keyboard-pan1.png)

    Da die Bildschirmtastatur einen großen Teil des Bildschirms verdeckt, stellt die UWP sicher, dass das Eingabefeld im Fokus eingeblendet, wie ein Benutzer durch die Steuerelemente des Formulars navigiert, einschließlich der Steuerelemente, die derzeit nicht in der Ansicht sind. Beim Anpassen der Benutzeroberfläche können Sie ein ähnliches Verhalten für die Darstellung der Bildschirmtastatur bereitstellen, indem Sie die [**Showing**](https://msdn.microsoft.com/library/windows/apps/br242262)- und [**Hiding**](https://msdn.microsoft.com/library/windows/apps/br242260)-Ereignisse behandeln, die vom [**InputPane**](https://msdn.microsoft.com/library/windows/apps/br242255)-Objekt zur Verfügung gestellt werden. Formular mit und ohne angezeigte Bildschirmtastatur

    ![Manchmal müssen bestimmte UI-Elemente dauerhaft auf dem Bildschirm zu sehen sein.](images/touch-keyboard-pan2.png)

## Gestalten Sie die UI so, dass sich die Formularsteuerelemente in einer verschiebbaren Region befinden und die wichtigen UI-Elemente statisch sind.


Beispiel:

```CSharp
public class MyApplication
{
    public MyApplication()
    {
        // Grab the input pane for the main application window and attach
        // touch keyboard event handlers.
        Windows.Foundation.Application.InputPane.GetForCurrentView().Showing  
            += new EventHandler(_OnInputPaneShowing);
        Windows.Foundation.Application.InputPane.GetForCurrentView().Hiding 
            += new EventHandler(_OnInputPaneHiding);
    }

    private void _OnInputPaneShowing(object sender, IInputPaneVisibilityEventArgs eventArgs)
    {
        // If the size of this window is going to be too small, the app uses 
        // the Showing event to begin some element removal animations.
        if (eventArgs.OccludedRect.Top < 400)
        {
            _StartElementRemovalAnimations();

            // Don&#39;t use framework scroll- or visibility-related 
            // animations that might conflict with the app&#39;s logic.
            eventArgs.EnsuredFocusedElementInView = true; 
        }
    }

    private void _OnInputPaneHiding(object sender, IInputPaneVisibilityEventArgs eventArgs)
    {
        if (_ResetToDefaultElements())
        {
            eventArgs.EnsuredFocusedElementInView = true; 
        }
    }

    private void _StartElementRemovalAnimations()
    {
        // This function starts the process of removing elements 
        // and starting the animation.
    }

    private void _ResetToDefaultElements()
    {
        // This function resets the window&#39;s elements to their default state.
    }
}
```

## Formular mit Bereichen, die immer sichtbar sein sollen



* [<span id="handling_events"></span><span id="HANDLING_EVENTS"></span>Behandeln von Showing- und Hiding-Ereignissen](keyboard-interactions.md)
* [Das folgende Beispiel veranschaulicht das Anfügen von Ereignishandlern für die [**showing**](https://msdn.microsoft.com/library/windows/apps/br242262)- und [**hiding**](https://msdn.microsoft.com/library/windows/apps/br242260)-Ereignisse der Bildschirmtastatur.](https://msdn.microsoft.com/library/windows/apps/mt244347)
* [<span id="related_topics"></span>Verwandte Artikel](https://msdn.microsoft.com/library/windows/apps/mt297667)


**Tastaturinteraktionen**
* [Barrierefreiheit der Tastaturnavigation](http://go.microsoft.com/fwlink/p/?linkid=246019)
* [Benutzerdefinierte Automatisierungs-Peers](http://go.microsoft.com/fwlink/p/?linkid=231633)
* [Archivbeispiele](http://go.microsoft.com/fwlink/p/?LinkID=251417)
* [Eingabe: Beispiel für die Bildschirmtastatur](http://go.microsoft.com/fwlink/p/?linkid=238570)
 

 






<!--HONumber=Apr16_HO3-->


